---
title: "lispysnake2d: Animations"
date: 2019-09-30T17:32:00+01:00
draft: false
authors: [ikey]
---

# Shiftin' Pixels Like a Boss

We like making things and you like watching them happen. Our most recent introduction into the
`lispysnake2d` codebase is animations. However, it took more than a few changes to implement it
the way we wanted.

{{<youtube "guSlEb2wHtE">}}

The above video uses assets from the awesome [Kenney](https://opengameart.org/users/kenney) on [OpenGameArt.org](https://opengameart.org)


### Texture Cache

We've fixed the existing bugs in our `Ls2DTextureCache` making it super reliable and quick to
work with. Additionally, we've implemented proper `subregion` support, i.e. dividing images
into regions to blit small areas.

### Tile Sheets

Building upon those `subregion` introductions, we now have simple `Ls2DTileSheet` objects.
In essence, these are a mapping of a subregion **name** to a subregion itself. To make life
even easier, we introduce support for `<TextureAtlas>` XML files to define sprites and regions
in a large tilesheet.

Large tilesheets are especially beneficial to 2D games, allowing us to optimise for the GPU
by having less texture switches, and ensuring power-of-2 image sizes. Long story short, we
whack all of our images into one big image, and describe those regions in a file.

Nothing ground breaking or new, but it does greatly easy the maintainence burden and improve
scalability considerably.

### Animations

The animations are currently implemented using an `Ls2DAnimation` object, which is a linear
array of frames. Each frame simply has a duration and texture handle, linking back into the
primary texture cache. On every update, we check if its time to progress to the next frame,
stop or loop.

To facilitate an `ECS` approach, we've also provided an `Ls2DAnimationComponent`, which will
be sourced by the sprite if set to render the correct texture.

### Wrapping it all up.

As a result, it gets infinitely easier to construct animations for our entities:

{{<highlight c>}}
        sheet = ls2d_tile_sheet_new_from_xml(cache, "demo_data/platform/spritesheet_player1.xml");
        ls2d_animation_add_frame(walking, ls2d_tile_sheet_lookup(sheet, "p1_walk01.png"), 1000 / 20);
{{</highlight>}}

**Note**: The API is subject to change, and may differ considerably by the time we open it up
publicly. Internally we're considering the requirement to prebake certain assets as CBOR streams
and adding scripting.

### Shameless Plug

If you like what you see developing before your eyes, it's not too late to purchase your
[Lifetime License for 2D titles](/the-game-raiser) to support development of the engine and
game in a timely fashion.

{{<paypal_2d_game>}}
