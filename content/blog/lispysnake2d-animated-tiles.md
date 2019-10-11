---
title: "lispysnake2d: Animated Tiles"
date: 2019-10-11T20:28:40+01:00
draft: false
authors: [ikey]
categories: [engine]
callout: "Animate All The Things"
---

Well, we've been unusually quiet the last few days. Truth be known, we've been real busy. Architecting here, energy-drink-drinking there..
The end result? We got us some full [Tiled Map Editor](https://MapEditor.org) support in our engine, along with massive tiling and
animation improvements.

{{<figure_screenshot image="/lispysnake2d-animated-tiles/screenshot_tiling" caption="Lookie here.. Full TiledMapEditor support">}}

### Don't Bore Me - What Changed?

Let's keep this short and sweet. We implemented full `TMX` and `TSX` support in the engine, allowing us to automatically splice a
texture according to the column/tilecount definitions (respecting padding, margin, etc). We then suplement the information from the
`TSX` animation definitions. `TL;DR` - Tiles are implicitly animated by using the `Tile GID` in a tile map.


### Animations, hmm?

Yeah, there's not much point in us saying we have animations if we don't then show them off. Note - we're culling anything offscreen,
drawing 3 layers (the door isn't part of that building, y'know), upscaling, blending, and animating. This uses a `TMX` file, which
in turn loads a `TSX` file, which has the definition and animations for `Overworld.png` tilesheet.

{{<youtube "k-86KUbTqvY">}}

The above video uses assets from [OpenGameArt.org](https://opengameart.org/content/zelda-like-tilesets-and-sprites)

### What's Next?

Currently we're discussing an **engine tech demo** before our major **game tech demo**. We're looking to get this out Pretty Soon (TM)
so stay tuned. We'll want to show off what we've done with the engine so far, as well as give a very small taste of what we've got
planned for our game. So yeah, technically? 2 tech demos planned.

As always - you can be one of the awesome people buying [Teh Game Raiser](/the-game-raiser/) licenses to help us get to market even
quicker! Otherwise, we have to work-work. **Shudder**.

{{<paypal_2d_game>}}
