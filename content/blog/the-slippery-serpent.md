---
title: "The Slippery Serpent"
date: 2020-02-02T00:11:30Z
draft: false
authors: [ikey]
categories: [engine]
callout: "The Slippery Serpent"
featuredImage: "/img/blog/the-slippery-serpent/Intro.png"
---

Hot damn. It's been a real busy few months. We expected to be back sooner, but
as I indicated on Twitter there has been everything from hospitals to funerals
to deal with, along with the Christmas period. For the keen eyed, we've been
very, very busy of late, per the non-stop Twitter feed.

Note that after this post we'll be accompanying all updates to Serpent with
a binary tech demo. Sweet huh?

Let's discuss some seriously meaty updates to [{{<fontawesome fab fa-github>}} serpent](https://github.com/lispysnake/serpent).

{{<figure_screenshot_one image="/the-slippery-serpent/Intro">}}

### Recap: What The Fudge is Serpent?

OK so Serpent is our miniature game framework written in the D programming language,
designed to make it as easy as possible to create games as an indie game dev. It
leverages best-of-class open source technologies, such as bgfx, SDL, etc, to bring
you an all-in-one game *kit*. It's not an engine though. Plumbing, not content
creation.

As a result we've focused on 2D games through a 3D pipeline, compatible with
OpenGL, Vulkan, DirectX, Metal, etc, thanks to the underlying technologies that
Serpent is using (chiefly bgfx + SDL).

Check the [{{<fontawesome fab fa-github>}} GitHub page](https://github.com/lispysnake/serpent). page for more details and activity.

### The Rebasening (TM)

Despite all the madness in the prior 2 months, I have been working away in a private
fork of Serpent, our game framework. Over the last couple of weeks I've been gradually
cleaning that up and rebasing it into git master of [{{<fontawesome fab fa-github>}} serpent](https://github.com/lispysnake/serpent).

The internal architecture recieved a much needed lift, as has been outlined for a
long time in the TODO and README files. The most prominently visible codebase change
is the inclusion of our first Entity-Component-System architecture. It's still
pending optimisation, but already working nicely.

### Entity Component System

We've replaced the older Entity notion in favour of Entity IDs, template-driven
components and parallel-friendly entity processors (systems). One of the most
interesting features is **compiler-level mutability promises**. In short, any
processor can be `ReadOnly` or `ReadWrite`, and the entity/component views
accessible are affected by this. This allows us to group similar mutability
processors and control parallelism and data integrity, ensuring immutable
data access (read-only) for concurrent, read-only jobs.

{{<figure_screenshot_one image="/the-slippery-serpent/ECS">}}

The screenshot below is from an earlier iteration of Serpent (prior to quad batching)
but shows off some of the mutability API.

{{<figure_screenshot_one image="/the-slippery-serpent/Mutability">}}

### Tiled Support

We have completed our first-level requirements for Tiled integration. In a nutshell
that means we can now render all data layers (in the correct order) for orthographic
tilemaps. Additionally we can load all TSX tilesheets (collection or sheet format)
either inline or from an external resource.

We support basic TMX features, with support for the following encodings:

 - Base64/deflate compression
 - Base64/gzip compression
 - Base64/no compression
 - CSV

At this point we don't support the plain XML `<tile>` layer format as it is
excessively inefficient. We also plan to support tile animations and object
layers soon.

### Sprite Batching

One of the major improvements since our last posts.. we can now actually render
sprites consistently. However, after the ECS and tiled support landed it was
very apparent that drawing textured quads (sprites) one by one is hella expensive.

See the screenshot below, where rendering the 1920x1080 display of 16x16px tileset
in one layer takes > 34k draw calls.

{{<figure_screenshot_one image="/the-slippery-serpent/PreBatch">}}

We invested a whole lot of effort into building a batch system for the sprite
renderered, so-called `serpent.graphics.batch.QuadBatch`. It sorts draw calls
based on the underlying texture to avoid excessive texture switches and builds
Index Buffer/Vertex Buffer pairs of a great many quads to drastically reduce
the number of OpenGL or Vulkan draw calls.

See the screenshot below, where the same scene is now rendered in 37 draw calls
per frame. vsync is also switched off and our framerate raises to 1.5KFPS.

{{<figure_screenshot_one image="/the-slippery-serpent/PostBatch">}}


### Camera APIs

Our orthographic camera implementation has been corrected (but not perfect) to
work with orthographic tilemaps. The in-tree demo now supports basic mouse
navigation and logical scaling by an aspect ratio. Currently we rely on
point (not linear) sampling to reduce artefacts with scaled tiles. Hit 'F'
on the demo to go fullscreen and scale automatically.


{{<figure_screenshot_one image="/the-slippery-serpent/Autoscale">}}

### Next On The List

Well, we're flying ahead now with our framework. It's actually at the point
where we *can* start fleshing out early level designs and make something useful
for people to test. As always, if you want to make this happen quicker, you
can always support us in our **open source game development** efforts!.

Here's a very quick todo list for our upcoming bits:

 - Optimise the Entity Component System storage for faster iteration
 - Rework parallel policy for chunked processors (rendering..)
 - Use processor groups to implement our deferred rendering pipeline (already in progress!)
 - Land Animation Processor (+AnimationComponent)
 - Land animated tiles support
 - Flesh out demo for playability (TM)
 - Fix Vulkan fullscreen switch (due to resolution change it cranks out.)
 - Add better (async) resource loading
 - Add sound (OpenAL)
 - Add explicit Z-buffer for tiled layers so sprites can go behind tops of buildings. :O
 - Enable Wayland integration (fairly simpleish.)

As you've noticed with this blogpost, we've gone from talking about things upfront to
actually getting them done and telling you later. With our upcoming blogposts we'll
be putting out playable demos (no matter how simple) for you to try so you can be
an active part of the development process of Serpent.

### Call For Support

We appreciate any and all support we get, whether its for our upcoming games, or our
open source work. As of today, you can buy us a coffee via PayPal or bitcoin to help
with development, or buy a lifetime license to all of our games. Thank you all for
your awesomeness, and until next time, peace out. <3

Additionally, resharing our posts and links to our project on [{{<fontawesome fab fa-github>}} GitHub](https://github.com/lispysnake/serpent). would
be most appreciated! Any pull requests are welcome too.

As part of keeping goals realistic, we'll now only be selling a total of 500
lifetime licenses. We've sold 77 at the time of writing, and look forward to
you joining our small club of lifetime license holders for all of our game
titles!

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=VYHL9CEFSNCVA)

[![Donate with Bitcoin](https://en.cryptobadges.io/badge/small/168AkAQszA7mZSv2epzYoPq4qnefiyhAKG)](https://en.cryptobadges.io/donate/168AkAQszA7mZSv2epzYoPq4qnefiyhAKG)

{{<paypal_2d_game>}}
