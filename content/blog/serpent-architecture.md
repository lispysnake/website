---
title: "Serpent Architecture"
date: 2020-02-26T12:16:04Z
draft: false
authors: [ikey]
categories: [engine]
callout: "Serpent Architecture Update"
---

Welcome to the last technical post prior to the release of the Serpent tech demo.
We hadn't actually planned on another post, but so much has changed in the codebase
of Serpent it deserved some mention.

It's not even been 4 weeks since our last post, and the code monkeys have been at
it around the clock. We have some large changes to discuss, and their benefits.

{{<figure_screenshot_one image="/serpent-architecture/GitStats">}}


### First up, some upstream work

Only a super minor contribution here, but it helped enable others to quickly and
easily build the brilliant `MangoHUD` project on Solus.

[{{<fontawesome fab fa-github>}} Add Solus support](https://github.com/flightlessmango/MangoHud/pull/41)

As a result we're now using MangoHUD as part of our test processes to get realistic
frametime graphs - plus it looks slick as hell. We'll continue to use it as part of
our Vulkan validation / fun.

{{<figure_screenshot_one image="/serpent-architecture/MangoHUD">}}


### Entity Component System

Wait, didn't we talk about this last time? :) From an API perspective, the ECS
was essentially complete. In terms of performance, it sucked.


#### Hold up, what is an entity component system?

This is not a new notion in game development, in fact its pretty much the defacto
standard nowadays. Long story short, it is a form of compositional programming
that favours the procedural over traditional object-oriented practices.

Every 'thing' in the game world is an Entity, which is usually an integer ID
of some sort. Instead of being an object with behaviours, an Entity is composed
through different components - each usually containing pure data. This provides
a way to decouple behaviour and state. It also allows great flexibility in
defining new 'types' in the game world, through component composition.

In an Entity Component System architecture, Systems process the data of these
components, associated with entities, to do Stuff. Like draw, or update physics,
etc.

#### And yours sucked.. why?

Performance! Or more precisely, memory layout. Initially we did the absolute
minimum level of work required to have a functioning ECS so that we could
then concentrate on rendering and game loop code. As a result, it was a
very ugly and inefficient implementation. Random access has been replaced
with sequential access where possible.

#### Introduce.. the new one. >_>

So yeah, we completely revamped it and made it a **useful system**. As
a result of this work, we can trivially access hundreds of thousands of
entities in a cache coherent fashion with minimal compromises. I'm not
saying it's a perfect solution, but in the context of the framework, it's
a pretty solid one.

{{<figure_screenshot_one image="/serpent-architecture/BlobMoarPerf">}}

As seen in the screenshot, our test could create 1 million entities consisting
of 3 components (1 implicit, 2 explicit) in just over 2 seconds. It's interesting
to note that the deferred APIs became quicker than the direct APIs. As a result
we're going to phase out the older direct API and prefer the thread-safe deferred
component mutation APIs now.

Also seen, we're able to iterate those entities (and assert some data values) in
just under 4 ms without threading and read-only access. On the test machine in question,
that got roughly 4 times quicker with threading. Unsurprising then, it is a quad core
laptop.

The nicest thing to see is that read-write access is still incredibly fast, with the
single threaded version running in shy of 4ms and threaded variant running at just over
1ms.

#### Industry standard features

Our new ECS features compiler-level mutability promises as standard. That is,
there are two primary ways of interface with entities.

#### `ReadOnly`

The Read Only View can only **read** data. It is not possible to mutate any
entity data without extreme abuse, i.e. deliberate programmer error.

A Processor is serpent-jargon for a System. It will be called as part of
a larger `Group` every frame tick, in sequence. ReadOnly processors are
provided with a `View!ReadOnly` struct and cannot modify entity data, but
can process it in bulk. Consequently, we allow `ReadOnly` groups to execute
in parallel.

#### `ReadWrite`

The read-write view provides the same API as the read-only view, but also
allows mutation of entity data, such as adding and removing components. They
may also destroy entities and create new ones. Consequently only one
`ReadWrite` processor may run at a time, to ensure data consistency.

However, a `ReadWrite` processor may use the `withComponentsChunked` APIs
to iterate entity data in parallel **internally** - ensuring each thread
only visits each entity once. This still benefits from the memory layout
changes and leads to huge performance boosts. A vital building block
for physics systems, for example.

Due to compiler level promises, we're able to define a very clear order
of execution, with explicit sync points and ensure thread safety whilst
being massively parallel.

Internally, all entities that share the same set of components are
automatically stored in Archetypes, or 'Super Types'. These archetypes
are then composed of `Page`s, which are built from `Chunk`s of memory.

Each chunk is allocated on a per component basis, as a contiguous block
of memory. We use some compiler magic to ensure valid struct alignment,
and automatically insert padding to force each `Chunk` to be exactly
16KiB in size (`2<<13`). As a result, we can easily fetch multiple chunks
into the L1 cache as part of a page.

#### `withComponents(...)`

The `withComponents` query allows searching for an entity by a matching
component signature, and iterate them with a `foreach` query. This is
very powerful, and iterations will return combined ranges across multiple
pages and archetypes.

The `withComponents` ranges will always return a tuple that can be
expanded automatically by the compiler, for very intuitive usage. The
first element is **always** a `const EntityIdentifier*` which is used
internally for bookkeeping.

Additionally, due to the archetype implementation, it is very cheap!
We don't need to test every entity for component membership, just iterate
all relevant component data in matching archetypes.

{{<highlight d>}}
        foreach (entity, transform, sprite; view.withComponents!(TransformComponent,
                SpriteComponent))
{{</highlight>}}

#### `withComponentsChunked(...)`

This is very similar to `withComponents,` but does not return a tuple range
of elements. Instead, it returns a range of ranges. Each range is over a
tuple set (SOA) range but matching the internal pages. Thus, if the maximum
size of a page is 4095 elements, each iteration of this function will return
at most 4095 elements.

This allows for excellent threading opportunities and very fast performance.

{{<highlight d>}}
    final override void run(View!ReadWrite view)
    {
        import std.parallelism : parallel;

        foreach (chunk; parallel(view.withComponentsChunked!(TransformComponent, PhysicsComponent)))
        {
            /* Find all physics entities */
            foreach (entity, transform, physics; chunk)
            {
                /* We use frameTime as opposed to deltaTime for frame-independent movement */
                auto frameTime = context.frameTime();

                transform.position.x += physics.velocityX * frameTime;
                transform.position.y += physics.velocityY * frameTime;
            }
        }
    }
{{</highlight>}}


#### Lots of templating..

A lot of the new features are possible thanks to the powerful features of DLang, chiefly
a sensible approach to template metaprogramming and builtin range support. This leads
to us having idiomatic APIs that feel natural, and not bolted on with various macros
and such. An example from the internal `archetype.d` function, `chunkRangedRW`, which
is input into the `withComponentsRW`:

{{<highlight d>}}

    /**
     * Read-write chunked range for components in this archetype
     */
    final auto chunkRangedRW(C...)() @safe nothrow
    {
        import std.algorithm;
        import std.range;

        return pages.map!((a) => a.rangedRW!C(&this)).chain;
    }
{{</highlight>}}

And further up the chain in `entitymanager.d`:

{{<highlight d>}}

        return archetypes.filter!((a) => a.satisfies(sig))
            .map!((b) => b.chunkRangedRW!C())
            .joiner();
            
{{</highlight>}}

#### SoA and AoS ...

Technically.. both? From a storage perspective we do directly work
with arrays of structs.. with a gotcha. We're enforcing struct alignment
at the compiler level with errors, and inserting missing padding to ensure
16KiB chunks. We're also using a plain ol' data approach. Note this is a
**large** array of structs, not an array of pointers to structs.

When it comes to **reading** the data, it becomes a struct of arrays..
of structs. With alignment, padding, and all the performance benefits.


### Culling

We implemented simple culling in both the tiled and sprite renderers,
also increasing performance. If it ain't onscreen, don't draw it.

Sprite Culling:

{{<youtube "z4854MZz4T8">}}

### Batch Optimizations

We were able to massively reduce the number of draw calls required for
huge scenes, such as in tilemaps. We implemented explicit Z-ordering
that respected alpha blending and minimised texture switches, reducing
draw calls (expensive) from a few hundred to less than 10.

{{<youtube "cmBojUpIkDs">}}

### Better UV normals support

We added some initial support for UV normals, which basically means we
have a standard way of manipulating textures. This support was added to
both renderers, allowing us to dynamically flip the normals, or the texture
orientation, without needing a texture swap.

### Basic input support

The demo is still very rudimentary and doesn't quite match the code quality
of the rest of the codebase, but we prepared a fun little video that shows
multiple entities with a 'player' being controlled with the keyboard, and
followed by the camera. Also known as the makings of a proper demo.

Also included: Completely useless explosion. Because it looked cool. Note this
video predated the new and improved ECS so we couldn't **actually** kill the
animation when it finished :P

{{<youtube "C7U3ukQWcMw">}}

### Next Steps

This was a huge hurdle to overcome in realising our goals as an innovative,
technically proficient game development framework. We still have a lot of
ground to cover in terms of tuning the graphical pipeline, but as more of
these systems emerge the framework grows ever more powerful.

Consequently, we're still ploughing ahead with the original plan of building
technical demos to show progress in serpent. We've got one coming Soon (TM),
just stay tuned.


### Le Milestone

We've now officially sold more than 100 of our Lifetime 2D Licenses! At the time
of writing we've sold 102 - and this in turn has helped us to power
through our development goals.

{{<paypal_2d_game>}}

With your continued support, we can build the best-in-class open source game
development framework, supporting a whole ecosystem of budding independent
content creators. Additionally, you can now opt to sponsor me [{{<fontawesome fab fa-github>}} directly](https://github.com/sponsors/ikeycode) to
work ony on open source efforts such as serpent.

In the mean time, make sure you keep an eye on our {{<fontawesome fab fa-twitter>}} [Twitter]({{<param SocialTwitter>}}) and {{<fontawesome fab fa-github>}} [GitHub]({{<param SocialGithub>}}) accounts
to see what we're up to!
