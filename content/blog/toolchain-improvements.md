---
title: "Toolchain Improvements"
date: 2020-05-25T16:53:36+01:00
draft: false
authors: [ikey]
categories: [engine]
callout: "Improving the toolchain"
---

What? Two blogposts in the same month? I know, we're on fire. You're welcome. So, we've
been working the last few days to bring some serious improvements to the Serpent developer
experience. A large part of that is supporting parallel targets and having toolchain consistency.
TL;DR Windows support is coming, and we have stable toolchains.

{{<figure_screenshot_one image="/toolchain-improvements/screenshot">}}

### `serpent-toolchain`

Introducing yet-another-mini-project. We love them. This particular project is simply a set of
scripts used to build our compilers, cross-compilers and whatnot. The assumption is a modern
build host with multilib capabilities, and a basic knowledge of distribution bootstrap processes.

Right now our primary **development** target is 64-bit Linux. With that in mind, we wanted to make
it easier to build our development targets in parallel. So, as of today, we're expanding our build
targets to include the Windows operating system. Note, we're reliant on **community feedback** for
testing, as the primary development testing will be done using WINE. Ironic, we know.

Anyway, the repo is **super** rough right now, but in time it'll be made a bit more streamlined
and procedural.

For now, you can check out the mini project on [{{<fontawesome fab fa-github>}} GitHub](https://github.com/lispysnake/serpent-toolchain)

Currently, we build latest `ldc` and `dub` stable for the `native` (Linux/x86_64) target, and for
the Windows target we'll build:

 - mingw-w64 6.0.0
 - GCC 9.3.0
 - binutils 2.34

The `serpent-support` repo will be fleshed out to provide builds of SDL for Windows, and in time
we'll look to add Android support.

### More Soon ....

We have more blogposts in the works, chiefly:

 - Milestones Made Public
 - Windows Release for serpent-demo-paddle
