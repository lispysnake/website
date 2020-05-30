---
title: "Serpent Paddle Demo Release"
date: 2020-05-17T21:09:52+01:00
draft: false
authors: [ikey]
categories: [release, engine]
callout: "Alpha Release: Serpent Paddle Demo"
featuredImage: "/img/blog/serpent-paddle-demo-release/screenshot.png"
---

Welcome to the first post in a **long time** from the Lispy Snake Crew! We've been super busy these past
few months and we've finally got something worth showing, an alpha demo! TL;DR Awesome progress with
our D-Lang written, OpenGL/Vulkan rendering BGFX-based SDL-weilding, cross-platforming Linux-first
work-in-progress that is our Serpent Game Framework.

{{<figure_screenshot_one image="/serpent-paddle-demo-release/screenshot">}}


### Get The Demo

You can grab the game from [{{<fontawesome fab fa-itch-io>}} itch.io](https://lispysnake.itch.io/serpent-paddle-demo) or from [{{<fontawesome fab fa-github>}} GitHub](https://github.com/lispysnake/serpent-demo-paddle/releases/tag/v0.0.1)

{{<itchio_game id="647067" name="serpent-paddle-demo" alt="Serpent Paddle Demo By Lispy Snake, Ltd.">}}

### Run The Demo

Just extract the tarball, and run the binary on a Linux/x86_64 host. Those with Vulkan support can try to
run the binary with the `-v` flag. Note that Mutter uses may experience utter failure when switching to
fullscreen with the `F` keybinding.

{{<highlight bash>}}

    tar xf serpent-demo-paddle.tar.xz
    cd paddle-game
    ./serpent-demo-paddle
{{</highlight>}}

Hit `Q` to Quit, Up/Down to move the paddle. `F` to toggle fullscreen.

Start with `-d` flag for `debug`, `-n` flag to disable `vsync`, and `-f` flag to start fullscreen.
The demo is deliberately running at a logical scaling size of `1366x768`



### What's it all about?

It's just a super simple game involving two paddles, a ball and a couple of obstacles. Use the arrow
keys on your keyboard to move the blue obstacle up and down, competing against the red paddle and
physics. First to 3 points, wins!

{{<youtube "zRkgpw2zQ_w">}}


### Special Thanks

As well as our usual thanks to the community, and contributors, we'd also like to thank [Kenney.nl](https://kenney.nl) and
[OpenGameArt](https://opengameart.org) as fantastic resources for open assets, used in this demo.

### Technical Details

This demo game is built using the Serpent Game Framework, which is under incredibly fast development.
We're using the demos (multiple!) to accelerate cadence and land features more rapidly. As such, you
can expect much more in the way of demos.

#### Physics

For this demonstration we implemented a new wrapper library, [{{<fontawesome fab fa-github>}} serpent-physics2d](https://github.com/lispysnake/serpent-physics2d).
It's a fairly lightweight wrapper around the [Chipmunk2D](https://chipmunk-physics.net/) Physics Library.
With it we get fully functioning, high quality, pixel perfect 2D physics in Serpent. Just add the
module to any Serpent-using-project to enjoy! We've got callbacks, shapes and whatnot. It'll improve, we swear.

#### Audio

The eventual plan is to actual use OpenAL for our sound system, but we wanted basic sound, quickly, in our demo.
To that end we set up [{{<fontawesome fab fa-github>}} serpent-audio](https://github.com/lispysnake/serpent-audio) as a simple wrapper around
SDL-Mixer. It's crude, but it does the job nicely. We'll replace this module with something more robust for our
next demos (We need 3D sound .. right? :))

#### And ..

Well, there was so much happening in the last few months, so many massive changes, modularising, rebasing,
cleaning, performance work, feature landing.. it's really hard to summarise.

Basically, lots happened, and we kicked ass.

### Next Up?

We're working on a 'bricks' demo. Similar to this one. But with bricks. Oh and special effects this time.
Additionally we're working on a slick public milestone list we're just **itching** to release..

Each demo will be tied to specific points in our milestone. We're going to need the bricks demo to
enhance the core serpent library (deferred callbacks, shaders, etc). We're also going to need a 3D demo.

Oh, and Windows is happening, thanks to awesome community contributions!

{{<figure_screenshot_one image="/serpent-paddle-demo-release/windows">}}


### Don't Forget

You can always support us if you like what we're doing!

{{<paypal_2d_game>}}
