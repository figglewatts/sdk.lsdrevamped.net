---
weight: 13
bookFlatSection: true
title: "Entities/Components"
---

# Entities/Components

In LSD:R, an Entity is a GameObject in a Dream scene with a `BaseEntity`-derived MonoBehaviour component added to it. These entities are provided as part of the LSD:R SDK, and are documented on this page.

The GameObject of any entity can be retrieved from a Lua script at any time using `GetEntity(entityId)`.

## Entity List

### DreamArea

A DreamArea is a box-shaped area that logs a graph contribution when the player enters it. The graph contribution affects the player's final position on the graph when the dream ends.

![DreamArea](/img/entities/DreamArea.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.
- Only Once (boolean): If `true`, this area will only trigger once. Otherwise, it will trigger every time the player enters it.
- Graph Contribution: How much this area affects the different axes of the dream graph.

### DreamAudio

DreamAudio stores a Unity AudioClip, data about playing it, and options to determine how to play it. You can use it to add audio to your dream scenes. It can also be controlled in [lua scripts](../lua/#dreamaudio).

![DreamAudio](/img/entities/DreamAudio.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.
- Clip (AudioClip): The audio clip to play.
- Spatial (boolean): If `true`, this audio clip is played in 3D space, otherwise it will be a 2D sound.
- Full Volume Range Units (number): The range in units at which the volume of the sound is at maximum.
- Range Units (number): The range in units at which the sound is inaudible.
- Pitch (number): The pitch ratio of the audio source. 1 is normal pitch.
- Volume (number): The volume of the audio, from 0 to 1.
- Delay Before Playing Seconds (number): How long in seconds to wait before playing the clip. If 'Wait Until Audible Before Playing' is true, it will wait this long after it first becomes audible.
- Loop (boolean): If `true`, this sound is looped automatically (no customisation between loops). Set this and 'One Shot' to `false` to keep playing the sound, waiting for 'Delay Between Plays Seconds' for each play.
- One Shot (boolean): If `true`, this sound is played once and then it stops (so there is no customisation between plays). Set this and 'Loop' to `false` to keep playing the sound, waiting for 'Delay Between Plays Seconds' for each play.
- Delay Between Plays Seconds (number): If 'Loop' and 'One Shot' are `false`, the sound will keep playing at an interval defined by this number.
- Play Clip For Seconds (number): If 'Loop' and 'One Shot' are `false`, the sound will play for this amount of seconds before being stopped (cut off).
- Wait Until Audible Before Playing (boolean): If `true`, this sound will not start playing until the player is within the audible range defined by 'Range Units'.
- Controlled With Script (boolean): If `true`, this sound will not play by itself, you will have to use the `Play()` function of the [DreamAudio type in the Lua API](../lua/#dreamaudio).

### InteractiveObject

An InteractiveObject is a dynamic, interactable object you can encounter in a dream in LSD: Revamped. It can run a script, animating it, moving it, and whatever else you'd like.

When the player interacts with the object (depending on its interaction kind) a graph contribution can be logged, and the contribution will be saved to the save file so that flashbacks will place the player back at the object.

Refer to the [Lua API](../lua/#interactiveobject) for how to write scripts for this object, as well as the [`InteractiveObject` type definition](../lua/#interactiveobject-1) in the API.

![InteractiveObject](/img/entities/InteractiveObject.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.
- Script ([InteractiveObject](../lua/#interactiveobject) Lua script): The Lua script to run for this object.
- Interaction Kind: Controls when `interact()` is called on the Lua script.
  - Proximity: When the player is within 'Interaction Distance', `interact()` will be called on the Lua script.
  - View: When the player is within 'Interaction Distance' and the player camera view frustum contains the object position (i.e. the object is in view).
  - None: This entity doesn't have an interaction, `interact()` won't be detected or called. Use this if you don't need it - it will save a bit of performance.
- Interaction Distance: The distance in units at which the player can interact with this object.

### LBDChunk

LBDChunk is used as part of importing the original assets, and hooking footstep sounds up to the level data. You shouldn't need to use it manually in your mods.

![LBDChunk](/img/entities/LBDChunk.png)

### MovieClip

MovieClip is used to store a reference to a Unity Video Clip that can then be played in dreams via a Lua script. Playing a MovieClip will end the dream afterwards. For more detail, [see the Lua API](../lua/#movieclip).

![MovieClip](/img/entities/MovieClip.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.
- Clip (Unity Video Clip): The movie clip to play.

### PlayerSpawn

A PlayerSpawn entity is a spawn point for the player within a dream. Usually, the player will spawn at a random spawn point out of all of the spawn points placed in a level. Sometimes you can spawn the player at a spawn point using its ID (for example, in a [TriggerLink](#triggerlink) to make a tunnel between dreams).

![PlayerSpawn](/img/entities/PlayerSpawn.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.
- Day One Spawn (boolean): If `true`, this spawn point will be used when it is Day 1. This is only valid if the [Dream](../assets/#dream) this PlayerSpawn is in has 'FirstDay' set to `true`.
- Tunnel Entrance (boolean): If `true`, this PlayerSpawn will not be chosen for the player to spawn on at random. This is used with tunnel entrance spawn points in Original Dreams, as the player does not usually spawn on them except via traversing the tunnels.

### Prefab

A prefab is an entity containing a reference to a [GameObject](../lua/#gameobject) that can be instantiated via a Lua script.

![Prefab](/img/entities/Prefab.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.
- Object (Unity GameObject): The GameObject that should be able to be instantiated from this node.

### Target

A Target is used as a marker for a location in a script, so you can get its world position or rotation and use that for something else.

![Target](/img/entities/Target.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.

### TriggerLink

TriggerLink is a box-shaped trigger that can either cause the player to link to a different dream, or affect all links the player takes when within the trigger volume.

![TriggerLink](/img/entities/TriggerLink.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.
- Only Once (boolean): Should this trigger work only once? It will no longer function when the player has entered it once.
- Linked ([Dream](../assets/#dream)): The Dream that this should link to. If no Dream is set, then this will be a random link.
- Force Fade Color (boolean): If enabled, the link fade color will be set to 'Forced Link Color'. If disabled, the color will be based on the graph position instead.
- Forced Link Color (Color): The color to use if 'Force Fade Color' is enabled.
- Spawn Point Entity ID (string): The entity ID of a [PlayerSpawn](#playerspawn) in the Dream we are linking to that the player should spawn on when we link. If not set, a random [PlayerSpawn](#playerspawn) will be chosen.
- Play Link Sound (boolean): Whether or not the link sound should play. Tunnels don't cause a link sound.
- Lock Input (boolean): If `true`, whatever move input the player had when the link occurred will be locked and only unlocked when the next dream is loaded - this is to support continuous walking through tunnels.
- Affects Links Within (boolean): If `true`, this TriggerLink won't link immediately when the player enters it, instead it will affect any link that occurs when the player is inside the trigger volume. This can be used to make certain parts of a level consistently link to certain [Dream](../assets/#dream)s.

### TriggerLua

A TriggerLua can run Lua code when the player enters or exits a given volume.

Please see [the Lua API](../lua/#triggerlua) for more information.

![TriggerLua](/img/entities/TriggerLua.png)

#### Properties

- ID (string): The ID of the entity.
- Unregistered (boolean): If `true`, then this entity won't be registed for retrieval with the Lua function `GetEntity()` - this can be useful if your entity is a child of a different object you already have, and you will be retrieving it with `GetChildByName()` instead.
- Only Once (boolean): Should this trigger work only once? It will no longer function when the player has entered it once and exited it once.
- Script ([TriggerLua](../lua/#triggerlua) Lua script): The Lua script to run for this trigger.
- Trigger Function Name (string): The function name in the Lua script that is called when the trigger is entered by the player. Usually just leave this as the default `onTrigger`.
- Trigger Exit Function Name (string): The function name in the Lua script that is called when the trigger is exited by the player. Usually just leave this as the default `onTriggerExit`.

## Component List

These are not entities as such---they are components provided by the SDK that you can add to the GameObjects in your dream scenes that may be of some use to you.

### AnimatedObject

AnimatedObject is an interface to a GameObject's Animator (and Animation Clips) that can be controlled from [a Lua script](../lua/#animatedobject).

![AnimatedObject](/img/entities/AnimatedObject.png)

#### Properties

- Clips (list of Unity Animation Clips): The clips this AnimatedObject knows about.
- Auto Play (integer): The index of the clip (from Clips) this object should start automatically playing. If set to -1, no animation will be played.

### ShaderSetter

If you want to support your objects changing between Classic and Revamped [shaders](../shaders/) at runtime (in-game), you need to add the ShaderSetter component to any renderables that use those shaders. It will handle swapping out the materials at runtime.

![ShaderSetter](/img/entities/ShaderSetter.png)

### ChanceToAppear

ChanceToAppear will hide the object it's attached to if the 'one in' chance given does not roll successfully. For example '1 in 4' represents a 25% chance of the object being active.

![ChanceToAppear](/img/entities/ChanceToAppear.png)

#### Properties

- One In (number): The chance to roll for. Generates a random number between 0 and 1, and if it's less than `1 / chance` then the roll has succeeded and the object will not be set to inactive.

### AppearOnDay

AppearOnDay will only show the object if a given day condition is met.

![AppearOnDay](/img/entities/AppearOnDay.png)

#### Properties

- Day (day kind): The kind of day this object should appear on. This is a set of flags, so multiple can be selected for the object to appear on multiple kinds of days.
  - None: This object should never appear.
  - Odd: This object should appear on days with an odd day number.
  - Even: This object should appear on days with an even day number.
  - One: This object should appear on the 1st day of the week (days 1, 8, 15, 22 etc).
  - Two: This object should appear on the 2nd day of the week (days 2, 9, 16, 23 etc).
  - Three: This object should appear on the 3rd day of the week (days 3, 10, 17, 24 etc).
  - Four: This object should appear on the 4th day of the week (days 4, 11, 18, 25 etc).
  - Five: This object should appear on the 5th day of the week (days 5, 12, 19, 26 etc).
  - Six: This object should appear on the 6th day of the week (days 6, 13, 20, 27 etc).
  - Seven: This object should appear on the 7th day of the week (days 7, 14, 21, 28 etc).

### GraphContributor

GraphContributor allows you to log a graph contribution from an entity at a given radius without using Lua scripting.

![GraphContributor](/img/entities/GraphContributor.png)

#### Properties

- Dynamic (number): The dynamic contribution to the graph.
- Upper (number): The upper contribution to the graph.
- Contribution Radius (number): Radius in units at which this contribution will be logged when the player gets close.
- Source Entity: The entity that this contribution is from - graph contributions logged using GraphContributor require an entity source.

### LBDGraphContributor

You shouldn't need to use this in your mod, it's used when loading the level data from the original game in order to affect the graph values when the player walks around the level.

![LBDGraphContributor](/img/entities/LBDGraphContributor.png)

#### Properties

- Layout Type: The layout type of the original LBD level.
- Tile Width: The tile width of the original LBD level chunks.
- LBD Graph Data: The graph data from the original LBD level chunks.
