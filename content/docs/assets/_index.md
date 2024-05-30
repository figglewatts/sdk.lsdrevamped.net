---
weight: 12
bookFlatSection: true
title: "Assets"
---

# Assets

The SDK comes with a number of asset types you can create and use in your mods. This page documents them.

## Dream

A Dream is a level in LSD: Revamped. It's a single location that the player can walk around and find things within. This asset stores a path to a prefab to instantiate when the dream is loaded, and a bunch of data about the dream.

A Dream is referenced by a [DreamJournal](#dreamjournal). Your [DreamJournal](#dreamjournal) will be comprised of multiple Dreams.

You can create a Dream in the Assets menu with `Create > LSDR SDK > Dream`.

![Dream](/img/assets/dream.png)

### Properties

- Name (string)
  - The name of this dream.
- Author (string)
  - The author of this dream.
- GreyMan (boolean)
  - Whether or not the grey man can spawn in this dream.
- Linkable (boolean)
  - If true, this dream is able to be linked to from random links (i.e. walking into walls).
  - If false, this dream can only be reached with direct links (i.e. a [TriggerLink](../entities/#triggerlink)).
- FirstDay (boolean)
  - If true, this dream is added to the pool of dreams that can be spawned into on Day 1 of a [DreamJournal](#dreamjournal).
- EnvironmentOffset (integer)
  - The offset into the [DreamEnvironmentSequence](#dreamenvironmentsequence) this dream uses for its environment. This allows dreams to have environment variation on the same day.
  - Negative offsets are not supported.
- Environments ([DreamEnvironmentSequence](#dreamenvironmentsequence))
  - Controls which [DreamEnvironment](#dreamenvironment) this dream uses. The current day number added to EnvironmentOffset is used as an index into this sequence.
- DreamPrefabPath (string)
  - The path to the prefab to load when loading this dream. This prefab should be your entire dream level, as you want it loaded in LSD: Revamped.
- SongLibrary ([LuaSongLibrary](#luasonglibrary)/[OriginalSongLibrary](#originalsonglibrary))
  - Used to determine which songs play for this dream. These songs will only be played when using the Revamped soundtrack. If you don't specify a song library here, then the default Revamped songs will be used instead.
- DreamScript ([LuaScript](#luascript))
  - The Lua script to use for this dream.
  - This is a [Dream](../lua/#dream)-type Lua script.

## DreamEnvironment

A DreamEnvironment can be added to a [DreamEnvironmentSequence](#dreamenvironmentsequence) associated with a [Dream](#dream) to control the [Dream](#dream)'s fog, sky, clouds, sun, and stars.

You can create a DreamEnvironment in the Assets menu with `Create > LSDR SDK > Dream environment`.

![DreamEnvironment](/img/assets/DreamEnvironment.png)

### Properties

#### Fog

- FogColor (color)
  - The color of the fog.
- SubtractiveFog (boolean)
  - Controls whether fog is additive or subtractive. Additive fog works best with brighter colours, and subtractive fog works best with everything else.
- FogStartDistance (float)
  - The distance at which fog starts, in units.
  - Note that enabling a further draw distance in the game settings will quadruple this value.
- FogEndDistance (float)
  - The distance at which fog is at its strongest, in units.
  - Note that enabling a further draw distance in the game settings will quadruple this value.
- FogHeight (float)
  - Controls how high the fog effect in the sky is.
- FogGradient (float)
  - Controls the strength of the fog effect in the sky.

#### Sky

- SkyColor (color)
  - The color of the sky.
- SkyFogColor (color)
  - The color of the fog in the sky. Set to the same color as the sky if you want to disable this effect.
- SunChance (float)
  - Likelihood of sun being present when the dream is loaded.
  - `1` is always and `0` is never. `0.5` would mean 50% of the time.
- SunColor (color)
  - The color of the sun.
- SunHeight (float)
  - The height of the sun in the sky.
- SunSize (float)
  - The scale of the sun.
- SecondSunChance (float)
  - Likelihood of there being a second sun present when the dream is loaded.
  - `1` is always and `0` is never. `0.5` would mean 50% of the time.
  - This chance is applied on top of SunChance (i.e. a second sun can only exist with a first sun), so a value of `0.5` for SunChance and `0.5` for SecondSunChance means an effective `0.25` chance of there being a second sun.
  - The second sun will always draw on top of the first sun.
- SecondSunColor (color)
  - The color of the second sun.
  - You can make this black with a small value for its offset to reproduce the eclipse/crescent moon effect from the original game.
- SecondSunOffset (float)
  - The offset away from the first sun this sun should be. This is a horizontal offset, use height for a vertical offset.
- SecondSunHeight (float)
  - The height in the sky of the second sun.
- SecondSunSize (float)
  - The scale of the second sun.
- SunBurstChance (float)
  - The chance of a sunburst effect being present on the first sun.
  - `1` is always and `0` is never. `0.5` would mean 50% of the time.
  - This chance is applied on top of SunChance (i.e. a sunburst can only exist with the first sun), so a value of `0.5` for SunChance and `0.5` for SunBurstChance means an effective `0.25` chance of there being a sunburst.
- SunBurstColor (color)
  - The color of the sunburst effect.
- SunBurstSize (float)
  - The scale of the sunburst effect.
- CloudsChance (float)
  - The chance for clouds being present in the sky.
  - `1` is always and `0` is never. `0.5` would mean 50% of the time.
- CloudsColor (color)
  - The color of the clouds.
- NumberOfClouds (integer)
  - How many clouds should spawn.
- StarsChance (float)
  - The chance for stars to be present in the sky.
  - `1` is always and `0` is never. `0.5` would mean 50% of the time.

## DreamEnvironmentSequence

A DreamEnvironmentSequence is set on a [Dream](#dream), and controls the [DreamEnvironment](#dreamenvironment) that the dream should use on certain days.

The environment is selected from the Environments of the DreamEnvironmentSequence based on the current day number offset by the [Dream](#dream)'s EnvironmentOffset value.

For example, on Day 5 with an EnvironmentOffset of 0, the [DreamEnvironment](#dreamenvironment) at index 4 from the DreamEnvironmentSequence will be used. With an EnvironmentOffset of 1, the [DreamEnvironment](#dreamenvironment) at index 5 will be used. On Day 1 with an EnvironmentOffset of 0, the [DreamEnvironment](#dreamenvironment) with index 0 will be used.

Using EnvironmentOffset in this way allows you to use the same DreamEnvironmentSequence for multiple [Dream](#dream)s in your [DreamJournal](#dreamjournal), but still allow them to have lots of variation in terms of their [DreamEnvironment](#dreamenvironment).

![DreamEnvironmentSequence](/img/assets/DreamEnvironmentSequence.png)

### Properties

- Environments (list of [DreamEnvironment](#dreamenvironment))
  - The list of [DreamEnvironment](#dreamenvironment)s this sequence is comprised of.

## DreamJournal

A DreamJournal is what you play in LSD: Revamped. It's a named collection of [Dream](#dream)s, along with some additional data about them.

Your [LSDRevampedMod](#lsdrevampedmod) can contain multiple DreamJournals, and the journals can be selected from the gameplay settings menu of LSD: Revamped.

![DreamJournal](/img/assets/DreamJournal.png)

### Properties

- Name (string)
  - The name of the DreamJournal. This is displayed in menus.
- Author (string)
  - The author of the DreamJournal.
- Dreams (list of [Dream](#dream))
  - The list of [Dream](#dream)s this journal is comprised of.
- GraphSpawnMap ([GraphSpawnMap](#graphspawnmap))
  - The [Dream](#dream) that you begin gameplay on is determined by the graph in-game. The [GraphSpawnMap](#graphspawnmap) controls which graph squares map to which dreams.
- FootstepIndex ([MaterialFootstepIndex](#materialfootstepindex)/[UVFootstepIndex](#uvfootstepindex)/[LBDFootstepIndex](#lbdfootstepindex))
  - The FootstepIndex controls which footstep sounds play on which surfaces. There are a few different types that can be used here depending on scene setup.
- LuaScriptIncludes (list of [LuaScript](#luascript))
  - This is a list of Lua scripts to 'include' in the context of any Lua scripts run as part of your DreamJournal.
  - Anything you define in these scripts will be accessible to your other Lua scripts.
- JournalScript ([LuaScript](#luascript))
  - The Lua script to use for this journal.
  - This is a [Journal](../lua/#journal)-type Lua script.
- SpecialDays (mapping of integer to [VideoSpecialDay](#videospecialday)/[TextSpecialDay](#textspecialday)/[RandomSpecialDay](#randomspecialday))
  - A special day is a text or video dream. This controls when these occur for this DreamJournal.
  - The key is the day number that should use one of these special days, the value is a special day asset that describes what should be done.

## GraphSpawnMap

In game, the graph square landed on at the end of the previous day is used to determine which [Dream](#dream) is loaded on the start of the next day.

The GraphSpawnMap inspector allows you to add [Dream](#dream)s and paint them onto a graphic of the graph.

![GraphSpawnMap](/img/assets/GraphSpawnMap.png)

To use the inspector, add [Dream](#dream)s from your journal to the asset either manually, or by using the 'Add from journal' button to add the [Dream](#dream)s already added from your [DreamJournal](#dreamjournal).

When a [Dream](#dream) is added to the GraphSpawnMap, it will be assigned a random colour to identify it on the graph. You can change this colour yourself by clicking on the coloured square next to the [Dream](#dream).

Each square can be painted with a [Dream](#dream) by clicking on that [Dream](#dream)'s 'Paint' button, then painting the graph squares you want to associate with this [Dream](#dream) using the left mouse button. A graph square can only be associated with one [Dream](#dream) at a time.

You can use the right mouse button to erase graph squares - you can paint with the right mouse button to erase multiple squares too. If you click the 'x' button next to a [Dream](#dream) it will erase it from the graph.

Any graph squares left unassigned to a [Dream](#dream) will link to a random [Dream](#dream) from the [DreamJournal](#dreamjournal) when landed on.

## LSDRevampedMod

An LSDRevampedMod is the asset that links everything together - it contains a list of [DreamJournal](#dreamjournal)s that are contained within your mod, as well as metadata for name and author.

There is also a large 'Build' button that can be used to build an asset bundle containing your mod - this can then be loaded in-game.

![LSDRevampedMod](/img/assets/LSDRevampedMod.png)

### Properties

- Name (string)
  - The name of the mod. This will show in menus.
- Author (string)
  - The author of the mod.
- Journals (list of [DreamJournal](#dreamjournal))
  - These are the [DreamJournal](#dreamjournal)s that you'll be able to switch between in your mod.

## RandomSpecialDay

A special day is a day where text or a video is played, instead of regular gameplay.

This kind of special day lets you choose a random special day out of a list of other special days. In the original game, special days could choose from a pool of 2 video and 4 text dreams per special day, so the RandomSpecialDay can accomodate for that.

![RandomSpecialDay](/img/assets/RandomSpecialDay.png)

### Properties

- Dynamic Contribution (int)
  - The contribution to the dynamic axis of the graph this day provides.
- Upper Contribution (int)
  - The contribution to the upper axis of the graph this day provides.
- Always (bool)
  - In the settings of LSD: Revamped it's possible to disable special days, so that all special days are instead gameplay days. When Always is set to true, it means this special day will be shown regardless of whether the special days are disabled in the game settings.
  - This is used in the original dreams by the cutscene that plays when you reach day 365.
- SpecialDayChoices (list of [RandomSpecialDay](#randomspecialday)/[TextSpecialDay](#textspecialday)/[VideoSpecialDay](#videospecialday))
  - The pool of special days this special day will select from.

## TextSpecialDay

A special day is a day where text or a video is played, instead of regular gameplay.

This kind of special day will display text on screen.

![TextSpecialDay](/img/assets/TextSpecialDay.png)

### Properties

- Dynamic Contribution (int)
  - The contribution to the dynamic axis of the graph this day provides.
- Upper Contribution (int)
  - The contribution to the upper axis of the graph this day provides.
- Always (bool)
  - In the settings of LSD: Revamped it's possible to disable special days, so that all special days are instead gameplay days. When Always is set to true, it means this special day will be shown regardless of whether the special days are disabled in the game settings.
  - This is used in the original dreams by the cutscene that plays when you reach day 365.
- OriginalTexture (texture)
  - This isn't currently used.
  - In the original game, text special days were simply textures. In the original dreams, this is used to store this texture.
- TextJa (string)
  - This is the Japanese text that will be displayed. Don't use this if your text isn't in Japanese, as I might be adding a language option for English in the future - it's not currently implemented, but this will make support for it easier.
- TextEn (string)
  - This is the English text that will be displayed. If you're using English text and you have a value for TextJa, then the Japanese text will be prioritised. This is because I haven't implemented localisation yet.

## VideoSpecialDay

A special day is a day where text or a video is played, instead of regular gameplay.

This kind of special day will play a skippable video.

![VideoSpecialDay](/img/assets/VideoSpecialDay.png)

### Properties

- Dynamic Contribution (int)
  - The contribution to the dynamic axis of the graph this day provides.
- Upper Contribution (int)
  - The contribution to the upper axis of the graph this day provides.
- Always (bool)
  - In the settings of LSD: Revamped it's possible to disable special days, so that all special days are instead gameplay days. When Always is set to true, it means this special day will be shown regardless of whether the special days are disabled in the game settings.
  - This is used in the original dreams by the cutscene that plays when you reach day 365.
- VideoClip (video)
  - The Unity video clip to play for this special day.

## LBDFootstepIndex

When in a [Dream](#dream), footstep sounds play depending on the surface the player is walking on. A footstep index functions as a souped-up lookup table that maps a number of forms of data about the surface the player is walking on to footstep sounds.

The LBDFootstepIndex maps the footstep data from the original game's LBD level data files to footstep sounds.

There is an LBDFootstepIndex provided in the SDK (to match the data from the original game), so you shouldn't need to make your own LBDFootstepIndex unless you're doing something cool, in which case also let me know because it sounds awesome.

If you want to use LBD files from the original game in your mod and have accurate footsteps, you will need to use an LBDFootstepIndex.

![LBDFootstepIndex](/img/assets/LBDFootstepIndex.png)

### Properties

- Fallback Sound (audio)
  - The Unity audio clip to play when we're unable to map the incoming data to a footstep sound. So, the default footstep sound.
- Fallback Pitch (float)
  - The pitch modifier of the fallback sound. This effectively scales the frequency of the sound, so 2 is twice as fast, and 0.5 is twice as slow.
- Index (mapping of integer to footstep data)
  - The key is an integer, this corresponds to the footstep portion of the LBD tile data from the original game.
  - The value is footstep data, comprised of an audio clip and a pitch modifier, as in the fallback sound.

## MaterialFootstepIndex

When in a [Dream](#dream), footstep sounds play depending on the surface the player is walking on. A footstep index functions as a souped-up lookup table that maps a number of forms of data about the surface the player is walking on to footstep sounds.

The MaterialFootstepIndex maps Unity materials to footstep data. The material used for the lookup is the material from the first surface of the mesh used by the MeshRenderer of the collider the player collides with.

This means that the MaterialFootstepIndex only works with meshes with a single surface.

![MaterialFootstepIndex](/img/assets/MaterialFootstepIndex.png)

### Properties

- Fallback Sound (audio)
  - The Unity audio clip to play when we're unable to map the incoming data to a footstep sound. So, the default footstep sound.
- Fallback Pitch (float)
  - The pitch modifier of the fallback sound. This effectively scales the frequency of the sound, so 2 is twice as fast, and 0.5 is twice as slow.
- Index (mapping of Unity material to footstep data)
  - The key is a Unity material that should be associated with the footstep data.
  - The value is footstep data, comprised of an audio clip and a pitch modifier, as in the fallback sound.

## UVFootstepIndex

When in a dream, footstep sounds play depending on the surface the player is walking on. A footstep index functions as a souped-up lookup table that maps a number of forms of data about the surface the player is walking on to footstep sounds.

The UVFootstepIndex maps regions from texture UVs to footstep sounds. It is intended to be used with things like texture atlases. The UVs are sourced from the mesh of the MeshRenderer of the mesh collider the player is walking on.

This means that the UVFootstepIndex only works with mesh colliders.

![UVFootstepIndex](/img/assets/UVFootstepIndex.png)

### Properties

- Fallback Sound (audio)
  - The Unity audio clip to play when we're unable to map the incoming data to a footstep sound. So, the default footstep sound.
- Fallback Pitch (float)
  - The pitch modifier of the fallback sound. This effectively scales the frequency of the sound, so 2 is twice as fast, and 0.5 is twice as slow.
- Index (mapping of UV rectangle to footstep sounds)
  - The key is a UV rectangle (so all values are between 0 and 1).
  - The value is footstep data, comprised of an audio clip and a pitch modifier, as in the fallback sound.

## OriginalSongLibrary

When in a dream, music can be playing - the OriginalSongLibrary plays music for a number of [Dream](#dream)s depending on the current song style and day number.

It contains a list of groupings of [SongList](#songlist)s, one for each of the song styles. Each [SongList](#songlist) corresponds with a dream from the journal, in order.

The list is selected based on the song style, and the song from the list is selected based on the day number.

You shouldn't need to use this, it was made to support how the music worked in the original game. A [LuaSongLibrary](#luasonglibrary) is a much more flexible option.

![OriginalSongLibrary](/img/assets/OriginalSongLibrary.png)

### Properties

- OriginalDreams (list of groupings of [SongList](#songlist))

## LuaSongLibrary

When in a dream, music can be playing - the [LuaSongLibrary](#luasonglibrary) selects music based on a function provided by a Lua script.

![LuaSongLibrary](/img/assets/LuaSongLibrary.png)

### Properties

- SongList ([SongList](#songlist))
  - The list of songs the Lua script has access to.
- Script ([LuaScript](#luascript))
  - The Lua script for the song library.
  - This is a [SongLibrary](../lua/#songlibrary)-type Lua script.

## Song

A Song groups an audio clip with a song title and author, to be displayed in the pause menu in-game.

![Song](/img/assets/Song.png)

### Properties

- Name (string)
  - The name of the song. Displayed in the pause menu when the song is playing.
- Author (string)
  - The author of the song. Displayed in the pause menu when the song is playing.
- Clip (audio)
  - The Unity audio clip that the song can be played from.

## SongList

A list of [Song](#song)s to be played by a song library.

![SongList](/img/assets/SongList.png)

### Properties

- Songs (list of [Song](#song))
  - The list of songs that are available to play via this list.

## LuaScript

Any Lua scripts (`.lua`) placed in the assets of your mod project will be loaded as a LuaScript asset. These can be assigned in various places in the SDK where Lua scripts can be executed.

![LuaScript](/img/assets/LuaScript.png)
