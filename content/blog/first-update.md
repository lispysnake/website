---
title: "First Update"
date: 2019-09-12T22:02:12+01:00
draft: false
authors: [ikey]
categories: [engine, release]
callout: "First Update"
---

We've been super busy here at Lispy Snake HQ, so it's time to give you a quick update as to what's been going on behind the scenes.
From the website to the core libraries and engine, a whole bunch of work has been underway to get us ready for launch. Also included
in this post, a tentative demo date! Once you've read this post, make sure to reshare it! <!--more-->

{{<figure src="/img/github_progress_update_1.png" width="85%" height="85%" link="/img/github_progress_update_1.png">}}

As you can see, we're currently relying a bit on private repositories, but we're planning to open source everything in time.


### Website Updates

A whole bunch of work has happened to the website, including, but not limited to:

 - HiDPI support
 - PNG logo switch
 - Improved mobile device support
 - Blog support (multiple posts now too.)
 - Refactor for params everywhere possible.
 - New contact page
 - Prettier landing page
 - Prettier footer widget

### Library Updates

We've been working on improving core libraries to help with our engine. This required tracking down some old convenience libraries
we've written and bringing them under the Lispy Snake umbrella, with new namespaces and goals. One library that's now public on
our [{{<fontawesome fab fa-github>}} GitHub]({{<param SocialGithub>}}/libls) is `libls`, an ongoing effort to provide standard library functions to our engine and consumers.

 - Relicensed to `ZLib` license for compatibility with titles at other companies
 - Rename from `libuf` to `libls` for namespace consistency with the 2D engine
 - Unjank include header filename
 - Add single-file-include for consuming library (`libls.h`)
 - Add `DEF_AUTOFREE` / `autofree` macros for modern C11 style programming
 - Add a new singly-linked-list type (`LsList`)

Note the library exists primarily to support our developing engine, which we require to be very lightweight, memory-efficient & clean, as well as trivially portable
to all systems (without additional runtime requirements). We're already starting to see some [pull requests](https://github.com/lispysnake/libls/pulls?q=is%3Apr+is%3Aclosed) for which we're grateful!

### Engine Updates

In between all of these happenings, we're working daily on the 2D engine. For now we've decided that using `SDL_Renderer` will offer the path of least resistance to help flesh out the API, which
in turn will reduce time-to-market for our demo significantly. In future, however, we're planning on a 3D pipeline, making Vulkan and OpenGL much easier to work with. Our plans revolve around
a 2D engine that looks and feels 2D, to help the developer work and think in 2D, but rendered using 3D technologies.

While this might not sound staggeringly different to what you've used, let's just say we're not entering the "all-in-one-game-editor-window" market. What we want is to facilitate **rapid time to market**
for fellow 2D indie game devs. When we're ready, we'll also open source the engine, but this will tie in with our demo release.

Recently, we've added:

 - `Ls2DEngine` object for core lifecycle & scene ownership
 - New `Ls2DScene` object graph
 - New root level object, `Ls2DObject`
 - Instance refcounting (atomic) on all `LsObject`s (required for upcoming texture-cache work)
 - Rebase onto `libls` for consistent API & namespace
 - Automatic output support (targeting console/mobile spawns)
 - Automatic frame rate management (tracking/capping/vsync)
 - Primitive input handling (required for upcoming GUI work)
 - Fix compilation with strict security requirements

We're developing `libls` in parallel to support the engine requirements, not all of which we'll reveal yet, but obviously will include basic data types, atomic operations and async subsystems.

### Upcoming

We're gonna keep keyboard-smashing away for a while, improving our customer facing web experience, engine, and libraries. We're hopeful to have a demo out in around 2 months
of our first title, but more on that to come. Please also keep your eyes peeled for an intro post soon from a new team member.
