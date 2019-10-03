---
title: "lispysnake2d: Tiling, Cameras, Action"
date: 2019-10-03T13:01:05+01:00
draft: false
authors: [ikey]
callout: "Yo, We Heard You Like Squares..."
categories: [engine]
---

Well, as promised, we've been very hard at work. Thus, it's time for another
development update on the `lispysnake2d` engine! In a nutshell, we've now
got a proper camera API, tilemaps, animations and scaling.

{{<youtube "QAXWQmh-dno">}}

The above video uses assets from the awesome [Kenney](https://opengameart.org/users/kenney) on [OpenGameArt.org](https://opengameart.org)

### Entity Rework

As previously discussed, we have a basic Entity Component System in place for
simplistic game development. That did, however, introduce an issue that blocked
development of the Tile Map. We didn't want the complexity of graphed components
for a static, layered tilemap.

As a result, we've converted `Ls2DEntity` into an abstract type, with two
concrete implementations:

 - `Ls2DBasicEntity` - typical Entity that you add components to, for behaviour and appearance
 - `Ls2DTileMap` - tilemap implementation with fixed 0,0 position and camera awareness.

### TileMap introduction

The TileMap does exactly what it says on the tin: it's a map of tiles. You've already seen
this method in many sidescroller and RPG style games, where the overall display is constructed
primarily of tiling assets in a grid formation. We're planning to use this method for scenes
in our upcoming game when we interact with space stations and other locations.

Internally, `Ls2DTileMap` is powered by a dynamic array of layers, each of which contains an
allocated `uint32_t*` blob of tiles. These tiles simply contain a global ID, with the upper
3 bits reserved for tile flipping. This is to ensure compatibility with the [Tiled Map Editor](https://www.MapEditor.org/)

As such, looking up a tile is ridiculously easy internally:

{{<highlight c>}}
        uint32_t *tile = &layer->tiles[x + self->width * y];
        uint32_t gid = *tile & LS2D_TILE_MASK;
        ...
{{</highlight>}}

In future, using a tilemap will be as simple as adding the entity to a scene and loading a
file, such as a TMX asset.

### Camera Fixes

We've reworked the camera API, and we can now point it at a given X, Y, or entity.
Right now we've not added bounds checking, but that's very trivial. What it does
mean is we can look around our game worlds, and start adding camera-follows-player
behaviour. It also means we'll be able to navigate our 2D tilemaps.

### Next On The List

Next up is to get the TileMap rendering textures (trivial) and fully supporting the
`TMX` file format of [Tiled Map Editor](https://www.MapEditor.org/). This will allow
us to use powerful open source tools to add value to our engine, without having to
reinvent various editors.

At the time of writing this post, 45 [life-time licenses](/the-game-raiser) have been
purchased, which is allowing us to spend more time on the engine and upcoming game.
Thank you all for supporting us. To be clear, the engine and game will be developed
regardless of how many licenses are sold, but with license sales, we can greatly
reduce our time to market.

{{<paypal_2d_game>}}
