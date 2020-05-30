---
title: "Website and Milestones"
date: 2020-05-30T19:47:27+01:00
draft: false
categories: [engine]
callout: "Website And Milestones"
featuredImage: "/img/blog/website-and-milestones/bricks.png"
---

Well, far be it from us to be idle. We've been internally getting quite
organised ready for our next stages of development. Oh, and we have some
public milestones. Woot.

{{<figure_screenshot_one image="/website-and-milestones/github">}}

### Website Refresh

Okay, so we've refreshed the website a few times.. However we've focused
more on navigation and content this time. We've rebased onto mdbootstrap
as unfortunately our web guy has had to refocus priorities due to the impact
of the COVID-19 pandemic. We completely understand having to refocus and
wish all the best!

So, in the interest of making **my life** a bit easier, I'm just reusing
the mdbootstrap project in conjunction with hugo to hopefully simplify
maintainence going forward.

We've also added "featured images" to the most recent blog posts, making
the blog feel somewhat more __alive__. Additionally we added the most
recent posts to the home page, along with some Serpent spoilers.

You'll also notice there is now a dedicated [Serpent](/serpent) section,
complete with some milestones and a roadmap.

### Lifetime `2D` License changes

Very simple change, initiated while redoing the [fundraising](/the-game-raiser) page.
We're opening the scope for the license and all license copies (past and present) will
be for **all** game types, 2D, 3D, whichever. We're really grateful for the community
support and it's the least we could do to give a little back.


### Bricks Demo

Alright in fairness, if you've been following us on Twitter, you already knew about this.
We're working on a quick demo to help further improve Serpent, a Bricks demo. Y'know the
drill, hit a ball at the bricks and hope to make them all go away. With this demo we'll
be extending upon the paddle demo to add badly needed features like particle effects,
texture recolouring, Windows support, etc.

{{<figure_screenshot_one image="/website-and-milestones/bricks">}}

### Milestones

Right now this is just a verbatim copy paste of the [Serpent](/serpent) milestones page.
Keep that page bookmarked to stay up to date with milestones and development progression.


{{<serpent_roadmap>}}
 

####  Current Features

It is wise to keep in mind our current development philosophy. We take specific milestone goals
and apply them to a working demo concept. Each demo is used to flesh out features, and to validate
that new code and functionality do not break our existing use cases.

 - Vulkan and OpenGL support
 - Incredibly powerful and fast Entity Component System
 - Almost complete support for [Tiled Map Editor](https://mapeditor.org) maps
 - Fast 2D sprite batching
 - Physics integration via [Chipmunk 2D Physics](https://chipmunk-physics.net/)
 - Audio integration via SDL_Mixer (which we will replace with OpenAL)
 - Powerful, simple APIs
 - 2D Rendering over fully 3D pipeline (shader based)

#### Planned Features

Please note this list is not complete, it serves only to illustrate the scope
and ambition of the first full Serpent release.

 - Full property based animations (including uniforms)
 - Integration of first-class scripting
 - Full animation support (procedural, keyframe, cross-interpolation, etc)
 - Character animation (2d/skeletal/etc)
 - Full font support
 - GUI rendering system (overlay)
 - Object / asset management system
 - 3D Cameras
 - 3D Audio
 - 3D Physics
 - Full input management and abstraction (gamepads, trackers, etc)
 - Visual scripting
 - Animation editor
 - Full shader pipeline support (post processing, etc)
 - Shader caching/compilation/storage
 - Offscreen render support
 - Physics debug rendering (2d/3d)
 - Asset management tool
 - Multiplayer-friendly APIs and systems
 - (Eventually) XR/VR support

### Ahem

I know, this is always awkward, but we're an open source project so it's kinda
the done thing. We know that the financial situation of the world is completely
in upheavel right now, and like you, we're trying our best to do our best.

We've cleaned up the [Fundraising](/the-game-raiser) page to make it clear
how you can support Serpent, and in addition, our tooling, practices, future
games and libraries.

You don't have to buy a license, a tip is always appreciated. Our ambition is
clear, to bring an industry-standard game and content engine to the open source
world, without the price tag. We love indie gaming, and plan to support it the best
way we can.

{{<paypal_2d_game>}}
