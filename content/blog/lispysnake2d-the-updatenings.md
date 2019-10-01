---
title: "lispysnake2d - The Updatenings"
date: 2019-09-27T10:01:07+01:00
draft: false
authors: [ikey]
categories: [engine]
callout: "We Done Made Us Some Progress"
---

We've been seriously hard at work here at the (virtual) Lispy Snake Towers. This blog post will capture some of our work so far,
and show you where we're at. Importantly, we'll discuss what the next steps look like. Oh, and there's the usual smattering of
pretty metrics and videos. We're currently still focuse on the engine side of the equation, however it's almost fleshed out enough right now to allow us
to start building the first technical demo of the game. Le gasp.

{{<figure_screenshot image="/lispysnake2d-the-updatenings/screenshot_github" caption="Hard at work over on GitHub">}}

### OK, what did you do?

So far we've got to a point where we have a tight game loop and input management. On top of that, we've added a cool `Ls2DTextureCache` with
subregion support to enable very cheap bliting of tilesheets and ensure texture deduplication. The engine currently supports an [Entity Component System](https://en.wikipedia.org/wiki/Entity_component_system)
pattern in order to simplify our initial work. Thus, we have a graph structure for each scene with compositional components:

 - `Ls2DEngine`
    - `Ls2DScene`
        - `Ls2DEntity`
            - `Ls2DSpriteComponent`
            - `Ls2DPositionComponent`
        - `Ls2DEntity`
            - `Ls2DSpriteComponent`
            - `Ls2DPositionComponent`
            - `Ls2DAnimationComponent`

As one can see, entities are added to the scene. Component siblings can discover one another through entity lookup APIs, allowing the `Sprite` component to know where
the `Position` component wishes it to draw. The new texture cache takes advantage of the new dynamic array type in [{{<fontawesome fab fa-github>}} libls]({{<param SocialGithub>}}/libls), using a cache-local dynamic array of structs
with preallocated size, vs a constantly growing list or array of pointers to structs.

### See It In Action

The demo below shows what we've been working on lately. Yep, it's super simple. And yes, it **kinda** looks like a really poor
clone of Space Invaders. But it does demonstrate working ECS tree, texture caching, use of spritesheets, input management
and rendering 60fps on 4k.

{{<youtube "A3BEEup3Whw">}}


### Cool. What's next?

Next on the radar we're going to begin fleshing out the initial technical demo. We're in discussions with an artist right now, and providing
there are enough Lifetime License sales, with your help, we'll fund that work very quickly! As the technical demo begins to grow, we'll
add features and alter the API of `lispysnake2d` to suit. The API, being C, clean, and modern, will allow us to add scripting APIs
in future too (no, we're not creating a language, we like bindings.)

We're taking a "go big, or go home" approach here, so while the ECS approach that we're providing for developers of smaller games is nice, it's not going to cut
it for our technical demo. We're going more with a data driven approach, thus the emphasis will be on multithreading, cache locality, and buttloads of renderables.
We want **loads** of ships in our demo. And particles. Everyone loves particles.

## Teh Game Rayzer

So far, 7 of you amazing souls have supported Lispy Snake through [The Game Raiser](/the-game-raiser) campaign. That's helping us pay for the GitHub organisation,
G Apps for Business, and the odd coffee. Basically, keeping the lights on, and we love you for it. As we've said before, we're happy to reinvest [contract work](/about/)
into the game development as and when it comes along. Buut, with your help, we can get our technical demo and then final game out so much quicker!

You can support the development of awesome games, and a more awesomer game engine (so portable, we need to port it to Windows from Linux) that will be open
sourced for the benefit of all indie game developers. We want the tools to build AAA quality titles, without AAA prices.

If you've already bought your Lifetime License, entitling you to free lifetime access to all of our upcoming 2D titles, then please reshare our posts and the
[Game Raiser](/the-game-raiser/) page wherever you can! More blog posts will be coming in the next week as we begin landing more and more features to support
the first demos.

{{<paypal_2d_game>}}
