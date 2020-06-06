---
title: "Isometric Tiled Support"
date: 2020-06-06T16:02:58+01:00
draft: false
categories: [engine]
callout: "Tiled Map Editor: Isometric Support"
featuredImage: "/img/blog/isometric-tiled-support/screenshot.png"
---

Despite a really, really busy week involving everything from car crashes
to an ISP failure, we've still been working. This is just a very quick
blogpost detailing our latest work on [{{<fontawesome fab fa-github>}} serpent-tiled](https://github.com/lispysnake/serpent-tiled)

{{<figure_screenshot_one image="/isometric-tiled-support/screenshot">}}

### Enter: Isometric Support

This in in the really early stages right now but we've landed initial support for
Tiled Map Editor maps (`.tmx`) in the isometric projection. To understand how this
works, it's best to look at the above screenshot rendered in the `orthographic`
projection.


{{<figure_screenshot_one image="/isometric-tiled-support/orthographic">}}


### Work In Progress

Long story short, it allows one to create psuedo-3D environments using 2D tiles.
Right now it's not optimised, and there is currently no clipping/culling support,
but it is on the way.

{{<figure_screenshot_one image="/isometric-tiled-support/stats">}}

The isometric support will be used in an upcoming demo with a **very** interesting
twist, and we'll not spoiler it.. so stay tuned!

