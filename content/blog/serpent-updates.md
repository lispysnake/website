---
title: "Serpent Updates"
date: 2019-11-01T12:13:46Z
draft: false
authors: [ikey]
categories: [engine]
callout: "Serpent: Updates"
---

It's been a real busy time for us here at Lispy Snake, Ltd. Today we'll
update you with where we're at, and where we're going. Literally. Long story
short we're only about one week away from restoring feature parity with
`lispysnake2d` and going far beyond that. We're pretty darn proud of how
the project is shaping up, with all the development happening [{{<fontawesome fab fa-github>}} in public](https://github.com/lispysnake/serpent).

{{<figure_screenshot_one image="/serpent-updates/screenshot_serpent">}}

### Quick Support Call Out

Normally we stick this fella down the bottom of the page, but that's in
contradiction of the top-third rule. So far, we've seen 75 licenses purchased
from [The Game Raiser](/the-game-raiser/) campaign. Honestly, that's really
awesome, and we're super grateful for it.

With the dramatic progress we've been making lately on `serpent`,
now's a great time for **you** to invest in a great open source framework
and top-quality games for you in future.

As a show of our dedication, we're fixing the Lispy Snake website messaging
to reflect what we're good at: We're an open source, indie game studio.
On top of that, we're going to empower others to create new content for
the enjoyment of everyone.

{{<paypal_2d_game>}}

### Rendering

Right now we've got Serpent to a point it can render a basic quad on
screen. While this may sound thoroughly unimpressive, given the short
time we've worked on porting `lispysnake2d` to D, this is huge in terms
of progress. Oh, and all that rendering is platform-independent thanks
to `bgfx`. Our screenshots show us using Vulkan, but we can quite happily
support Metal, OpenGL, and more.

At the time of writing the Serpent codebase has seen 122 commits, and
a distinct architecture is emerging. It follows a data-driven design,
making no distinction between 2D and 3D entities.

Internally, we create a class of `serpent.Entity` (such as a `Player` or
even `EnemyShip`) which contains an array of vector positions at minimum.
As the codebase expands, we'll add tagging of Entity types so that
the corresponding `Renderer` types will be responsible for drawing
certain kinds of data.

Right now, all of our work is focused on the initial `SpriteRenderer`, which
uses `bgfx` behind the scenes to draw textured quads using OpenGL. We've
hit some roadbumps along the way, such as `bimg` having no C API (thus unusable
from `dlang`). To correct this, we're now using `SDL_Image` to support loading
of `PNG` assets at runtime. In future, Serpent will also support `KTX` and `DDS`
(direct draw surface) files to load hardware optimised textures instead.

### Input

We're building various thin layers on top of our support libraries to make
`serpent` more usable to ourselves, and end users. We're also trying to do
so in a way that leverages the language-level features. Part of our exploratory
work has seen us using `std.signals` module of D Lang for input handling:

{{<highlight d>}}
        context.input.mousePressed.connect(&onMousePressed);
        context.input.mouseMoved.connect(&onMouseMoved);
        context.input.keyReleased.connect(&onKeyReleased);
{{</highlight>}}


### Wrapping It All Up

The (still developing) architecture is making it very easy to create games,
with a focus on cross-platform functionality and ease of use. Here is the
`main` routine from our in-tree demo that we're working on:

{{<highlight d>}}

import serpent;

int main()
{
    auto context = new Context();
    context.display.title("Serpent").size(1366, 768);
    context.display.pipeline.addRenderer(new SpriteRenderer());

    return context.run(new DemoGame());
}
{{</highlight>}}

Note that the API is subject to constant change and improvement, so this
should not be considered indicative of final release quality.

### SDK

Our eventual aim is that Serpent be deployed in an SDK fashion. We're
not going to ram editors down your throat. What we will do, is create
packages with the various requirements for creating apps and games using
the `serpent` game framework. This'll include the version of `bgfx` that
we utilise, along with the runtime tools like `shaderc`, `texturec`, etc.

As part of our scripting and tooling, we'll make it very trivial to
build Serpent games for multiple targets, so you can get them deployed
easier. Triple-A quality processes and software for indie developers is our
dream.


### Halloween Tech Demo

We're a little bit delayed on the Halloween tech demo, but that was to
be expected given our shift from the original C engine, to a D-lang
cross-platform framework. Despite this, we'll still be putting out the
Halloween-inspired tech demo as soon as feasible, purely to demonstrate
progress on the Serpent framework.

## Relocation

Lispy Snake, Ltd. will be relocating to the Republic of Ireland. This is
because I myself am moving. No changes will be made to the company outside
of this, and all licenses will (obviously) be retained. A formal switch
to a ROI-incorporated company will be announced some time in the future.

Long story short: Everybody wants stability and a great quality of life
for their family. Speaking from a business point of view we want certain
guarantees that cannot be provided in the current Brexit-infused climate.
**Note**: This is the one and only time we'll mention anything vaguely
political here. We don't do politics.
