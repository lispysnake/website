---
title: "Enter the Miniature Dragon"
date: 2019-10-20T20:38:06+01:00
draft: false
authors: [ikey]
categories: [engine, release]
callout: "Enter The Miniature Dragon"
---

Man, have **we been busy**. Like, for real. In this post, we'll introduce
you to the name and mockups for our first title, as well as show you
some open source goodness.

### The Last Peacekeeper

This is the working title for our upcoming first game. We've done a whole lot
of storyboarding and character development internally, and feel it's time to
show the world where we're headed.

{{<page_content image="/tlp/titlescreen" caption="The Last Peacekeeper Titlescreen Mock-up.">}}

Do note that as with all development, expect constant reiteration and improvement.
As such, we've prepared a special landing page to keep you up to date with the
core plan for the title.

> In The Last Peacekeeper, the first game from Lispy Snake, you play as a bounty hunter bringing the galaxy’s most wanted to justice. That is, until a series of events forces you to confront some of the darkest forces in the universe - including your own demons. This game melds strategy, adventure, and RPG elements to create a gritty adventure that will bring you hours of captivating excitement. This game will break you, build you back up, and then break you again. The question is, do you have what it takes to be The Last Peacekeeper?

Read more on [The Last Peacekeeper](/the-last-peacekeeper)

### Serpent

Cue absurd GIF with vague reference to what we're talking about:

{{<tenor 5494199 "small-small-snake-snake-tiny-animal-gif-5494199">}}

We've been hard at work on our engine for a while now, and with that came the somewhat
obvious (yet still hard-hitting) realisation that creating engines just doesn't make
sense anymore. Considering how much custom logic would form the engine-proper within
our games, making a generic game engine that reimplements the world just didn't make
a lot of sense.

That's where [{{<fontawesome fab fa-github>}} Serpent](https://github.com/lispysnake/serpent) comes in.
Whilst it may be a young project right now, it does have some very sensible goals. In a nutshell, we're
looking exclusively to create a gaming **framework**, or "kit". The README does a good job of explaining
our [design decisions](https://github.com/lispysnake/serpent/blob/master/README.md) - but this being
a blog we'll give you the basic laydown.

 - We wanted a safe, friendly language.
 - We love C - but avoiding heavy problematic dependencies means needlessly reimplementing the basics every time (ADT, refcounts, etc)
 - We also like the whole "string safety" and "UTF" thing.
 - We're leveraging the best in class of open source libraries under one roof.
 - Reinvent only what needs reinventing.

To that extent - we're writing Serpent in the [D Programming Language](https://dlang.org/) or 'D Lang' as it is known.
Note only does this offer us safety, but powerful compiler and runtime features, such as TMP, concurrency, etc.
For our rendering, we're using [bgfx](https://github.com/bkaradzic/bgfx) - supporting all major targets and technologies,
including Vulkan. Internally this handles multithreading, drawcall ordering, etc.

We're still using [SDL](https://libsdl.org) for basic windowing, input management, etc. This affords us the advantage of
the great controller support expected in the gaming world. Also on the planned integration list is [OpenAL](https://www.openal.org/) for audio, and potentially [Newton Dynamics](http://newtondynamics.com/forum/newton.php) for physics.

The net result is going to be a gaming framework, in D, that drastically reduces time to market for 2D titles. We're also going to make
sure we use it in all the right ways, with parallel hotpaths in trusted native libraries. It's a super exciting time here at Lispy Snake,
and we can't wait to make you part of the adventure.

### FOSS All The Things

If you check out our [{{<fontawesome fab fa-github>}} Serpent](https://github.com/lispysnake) - you'll see a bunch of projects just appeared. Included is Serpent, as well as the old code from our
investigative efforts into a [{{<fontawesome fab fa-github>}} C-based game engine](https://github.com/lispysnake/lispysnake2d) It's limited and relies on some hardcoding for demo purposes, but it serves as
documentation for where we went wrong. TL;DR - pick your battles.

Most importantly, though, you'll see `serpent`, `serpent-support` as public repos now. Our framework development will happen in the
public eye - just please note we're currently only focusing on Linux x86_64 targets and will heavily change our code daily.

### Lastly

As previously indicated - we can spend more development time on these cool projects, and our game, if **you** support us.

To do that - buy a [license today](/the-game-raiser/) that will reward you with lifetime, free access, to all of our upcoming 2D titles.
That's pretty sweet, at only $20.

{{<paypal_2d_game>}}
