---
title: "Serpent"
date: 2020-05-30T16:03:27+01:00
draft: false
---

### The Serpent Engine

Serpent is our game and content framework solution. It has been in heavy development for almost
a year and is shaping up nicely. Our initial focus is on 2D content, however we have now built
a sufficient base to allow extending our goals to 3D and XR applications.

Serpent is written entirely in the [D Language](https://dlang.org) - allowing us to take advantage
of fantastic language features such as CTFE, thread pools, C++ interoptability, etc.

We utilise [SDL](https://libsdl.org) for native windowing and event integration, and in turn we
use [BGFX](https://github.com/bkaradzic/bgfx) to actually render our content, with [OpenGL](https://www.opengl.org/) and [Vulkan](https://www.khronos.org/vulkan/) currently supported.

Currently we support modern Linux distributions, as we develop **exclusively on Linux**. The next
supported platform will be Windows, and eventually we will begin to support consoles and other
platforms too.

{{<serpent_roadmap>}}
 

### Current Features

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

### Planned Features

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
