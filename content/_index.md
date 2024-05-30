---
title: Home
type: docs
---

# LSD: Revamped SDK

Welcome to the documentation for the LSD: Revamped SDK.

[LSD: Revamped](https://lsdrevamped.net) is a fan-made modern recreation of cult-classic PS1 game ['LSD: Dream Emulator'](https://en.wikipedia.org/wiki/LSD:_Dream_Emulator).

The SDK (Software Development Kit) gives you the ability to make mods for LSD:R using the Unity editor. The SDK itself comes as a Unity package, allowing you to create your mod as a Unity project, using the tools to export it to be loaded into the game.

It has been built around Unity 2021.3, it may work in future versions of Unity too but I haven't tested that.

## Features

- Create your own dreams.
- Add anything to them that Unity supports (except custom C# scripts).
- Customise behaviour with Lua scripting.
- Control the dream graph.
- Create flashback events.
- Create video and text dreams.
- Add your own music and sound effects.

## Getting started

To get started with the SDK I would first recommend reading through/following [the tutorial](/docs/tutorials) - it will take you on a general tour of most areas of the SDK, with lots of links for you to use as jumping-off points to explore the SDK a bit deeper.

There is also [an example project](/docs/example-project) available, in the form of a mod that adds some additional dreams to the base game. Opening and exploring this Unity project may help you understand how parts of the SDK come together in practice.

The content in LSD: Revamped that has been remade from the original game is also created with the SDK, so you may find it useful to [look through that as well](https://github.com/figglewatts/LSDRevamped/tree/master/LSDR/Assets/Resources/Original) (LSD: Revamped is fully open source).

The rest of the pages on this documentation site serve as a reference manual for various parts of the SDK:

- [Assets](/docs/assets), for the Unity assets you can create for various parts of SDK functionality.
- [Entities/Components](/docs/entities), for entities and components you can use in dreams to add functionality.
- [Shaders](/docs/shaders), for the shaders provided in the SDK, to emulate effects from the original game.
- [Lua API](/docs/lua), for documentation of the Lua scripting API available in the LSD:R SDK.
- [Developer Console](/docs/developer-console), for documentation of the developer console available in-game, i.e. commands, syntax etc.

## Finally

I have done my best to document everything I expect people will need to use in the SDK, however it's very possible that either my documentation is confusing, or I have missed certain important elements. If you're confused by anything here, please reach out to me at [@Figglewatts on Twitter](https://twitter.com/Figglewatts), Figglewatts on Discord, or email me at `me (at) figglewatts.co.uk`.

It's also possible the SDK is missing support for a feature you'd like to use in your mods! If this is the case, please also get in contact with me through the same methods listed above, and I'll see what can be done about that. I'm not looking to implement new functionality in the game, more exposing any existing functionality to mods that I may have forgotten to fully expose.

It's also very possible that some parts of the SDK are broken in ways I haven't anticipated! If that's the case, again, get in contact and I can fix stuff! I'm really excited to see cool things that people make with this and I'm happy to help in any way with any issues.

Have fun!

--Figglewatts (LSD:R & SDK developer)
