---
weight: 15
bookFlatSection: true
title: "Lua API"
BookToC: 3
---

# Lua API

There are a number of places in your mod where you can write Lua scripts that the game will hook into; this page documents where these places are (in the form of the different types of scripts), as well as the Lua API available for you to use to interface with the game.

LSD: Revamped uses [MoonSharp](https://www.moonsharp.org/about.html) as its Lua interpreter, be sure to check the documentation to familiarise yourself with any features it may have that are specific to its environment. For example, LSD: Revamped makes extensive use of its lambda-style anonymous functions (i.e. `|x, y| x + y`).

It may be useful to refer to LSD: Revamped's existing usage of the Lua API in the Original Dreams content. For example, you can find [the Lua code for all of the interactive objects](https://github.com/figglewatts/LSDRevamped/blob/master/LSDR/Assets/Resources/Original/STG00/Scripts/doublefaceRun.lua) right there in the repo.

There is also [the Lua include scripts](https://github.com/figglewatts/LSDRevamped/tree/master/LSDR/Assets/Resources/Original/LuaInclude), and [the built-in Lua song library](https://github.com/figglewatts/LSDRevamped/blob/master/LSDR/Assets/Resources/Original/revampedSongLibraryScript.lua).

## Scripts

The different types of scripts are used in different places in your mod, and must have different functions defined depending on the script type - they are documented here.

### Dream

A dream script can be added to a [Dream](../assets/#dream).

The script must have the following format:

```lua
function start()
    -- this is called when the dream is loaded
end

function update()
    -- this is called every frame while the dream is loaded
end
```

#### Functions

- `start()`: This is called when the [Dream](../assets/#dream) is loaded.
- `update()`: This is called every frame while the [Dream](../assets/#dream) is loaded.

#### Predefined

- `this`: The [Dream](#dream-1) this Lua script is defined on.

### Journal

A journal script can be added to a [DreamJournal](../assets/#dreamjournal).

The script must have the following format:

```lua
function start()
    -- this is called when a day begins for the current dream journal
end

function update()
    -- this is called every frame while dreaming with this dream journal
end
```

#### Functions

- `start()`: This is called when a day begins for the current [DreamJournal](../assets/#dreamjournal).
- `update()`: This is called every frame while dreaming with this [DreamJournal](../assets/#dreamjournal).

#### Predefined

- `this`: The [DreamJournal](#dreamjournal) this Lua script is defined on.

### InteractiveObject

An interactive object script can be added to an [InteractiveObject](../entities/#interactiveobject) entity.

The script must have the following format:

```lua
function start()
    -- called when this entity is created
end

function update()
    -- called every frame while this entity is active
end

function intervalUpdate()
    -- called on a configurable interval while this object is active
end

function interact()
    -- called depending on the interaction type of the object
end
```

#### Functions

- `start()`: This is called when this [InteractiveObject](#interactiveobject-1) is created.
- `update()`: This is called every frame while this [InteractiveObject](#interactiveobject-1) is active.
- `intervalUpdate()`: This is called on a configurable interval while this [InteractiveObject](#interactiveobject-1) is active.
- `interact()`: This is called depending on the interaction type of the [InteractiveObject](#interactiveobject-1).

#### Predefined

- `this`: The [InteractiveObject](#interactiveobject-1) this Lua script is defined on.

### TriggerLua

A script can be added to a [TriggerLua](../entities/#triggerlua) entity in order to run Lua code when the player enters and exits a given trigger collider.

The script must have the following format:

```lua
function start()
    -- called when this entity is created
end

function update()
    -- called every frame while this entity is active
end

function onTrigger()
    -- called when the player enters the trigger
end

function onTriggerExit()
    -- called when the player exits the trigger
end
```

#### Functions

- `start()`: This is called when the [TriggerLua](#triggerlua) is created.
- `update()`: This is called every frame while the [TriggerLua](#triggerlua) is active.
- `onTrigger()`: This is called when the player enters the [TriggerLua](#triggerlua) collider.
- `onTriggerExit()`" This is called when the player exits the [TriggerLua](#triggerlua) collider.

#### Predefined

- `this`: The [TriggerLua](#triggerlua) this script it attached to.

### SongLibrary

This is a script responsible for selecting the music for a [Dream](#dream-1). It has a [SongList](#songlistasset), and the [Song](#songasset) can be selected with a [SongStyle](#songstyle-enum) and day number.

The [SongStyle](#songstyle-enum) is selected based on the day number in the following order:

1. Standard
2. Ambient
3. Lovely
4. Human
5. Electro
6. Ethnova
7. Cartoon
8. Then it loops, so Day 8 would be Standard again

In the original game, the [SongStyle](#songstyle-enum) is used to select the soundfont to play the dream's music in. You can replicate this in LSD: Revamped by having multiple audio files and mapping the [SongStyle](#songstyle-enum) to a [Song](#songasset) from the [SongList](#songlistasset), retrieving it when the script is called.

The script must have the following format:

```lua
function getSong(style, dayNumber)
    -- called when a new dream is loaded, should return a song from the given song list
end
```

#### Functions

- `getSong(style, dayNumber)`: Called when a new dream is loaded (through linking or otherwise).
  - Arguments:
    - `style` ([SongStyle](#songstyle-enum)): The current song style. This changes every day, see above for more info.
    - `dayNumber` (integer): The current day number.
  - Return:
    - It should return a [Song](#songasset) from the [SongList](#songlistasset) of this song library.
    - You can also return `nil` in order to not play any music.

#### Predefined

- `songList`: The [SongList](#songlistasset) of this song library.

## Globals

The following globals are available in any Lua script in the SDK from any scope.

### Objects

- `DreamSystem`: The [DreamSystem](#dreamsystem) API, used for controlling the current day's dreams.
- `Input`: The [Input](#input) API, used for getting input.
- `PauseSystem`: The [PauseSystem](#pausesystem) API, used for controlling the pause state of the game.
- `Persistence`: The [Persistence](#persistence) API, used for storing and retrieving persistent data over different lifetimes.
- `Unity`: The [Unity](#unity) API, used for interacting with the underlying engine.
- `Debug`: The [Debug](#debug) API, used to help debug scripts.
- `Condition`: The [Condition](#condition) API, used to provide [LuaAsyncActionRunner](#luaasyncactionrunner) action sequences with conditions for how long actions will execute for.
- `Colors`: The [Colors](#colors) API provides some [Color](#color)s for you to use in scripts.
- `Random`: The [Random](#random) API provides functions for introducing randomness in your scripts.
- `Screenshotter`: The [Screenshotter](#screenshotter) API lets you take screenshots from scripts.

### Enums

- `TextureSet`: The [TextureSet](#textureset-enum) enum is used for the different kinds of texture sets, i.e. 'Normal', or 'Kanji'.
- `Lifetime`: The [Lifetime](#lifetime-enum) enum is used to control how long data is persisted for in the [Persistence](#persistence) API.
- `SongStyle`: The [SongStyle](#songstyle-enum) enum is used for the song styles of the music in-game. For more information see [SongLibrary](#songlibrary).

### Functions

- `GetEntity(id)`: Gets the [GameObject](#gameobject) of the [entity](../entities/) with the given ID.
  - Arguments:
    - `id` (string): The ID of the entity we want to get.
  - Returns:
    - The [GameObject](#gameobject) of the entity with the given ID, or `nil` if there is no entity with that ID.
- `IsDayEven()`: Returns a boolean that is `true` if the current day number is even, and `false` otherwise.
- `IsWeekDay(number)`: Returns a boolean if the day number modulo 7 matches the given number. This allows you to check if we are on a given day in a week - for example, `IsWeekDay(1)` returns `true` for days 1, 8, 15, 22, etc.
  - Arguments:
    - `number` (integer): The day number (of the week) we are checking for. If you use a number greater than 7 this will always return `false`.
  - Returns:
    - `true` if the current day is on the given week day number, `false` otherwise.
- `IsDayLinear(slope, constant)`: Returns the result of:

  `(current_day - constant) % slope == 0`

  You can use this to check for patterns of days that are more complex than simply a given week day or odd/even.

- `SetCanControlPlayer(state)`: Sets whether or not the player can be controlled.
  - Arguments:
    - `state` (boolean): `true` if the player should be able to be controlled, `false` otherwise.
- `GetDreamByName(name)`: Get a [Dream](#dream-1) with a given name from the current [DreamJournal](#dreamjournal).
  - Arguments:
    - `name` (string): The name of the [Dream](#dream-1) to get.
  - Returns:
    - The [Dream](#dream-1) with this name, or `nil` if it was not found.

## Types

The following is all of the types available in the LSD: Revamped Lua API.

### Actions

Some objects are able to run sequences of 'actions'. A sequence of actions is essentially a coroutine - it can do a thing for a certain amount of time, or until some external condition is met, then do something else.

In LSD: Revamped this is used pretty widely as a primitive for controlling [InteractiveObject](#interactiveobject-1) behaviour.

#### LuaAsyncActionRunner

The `LuaAsyncActionRunner` is the interface to creating and running actions. You create a sequence of actions with `Do()` or `WaitUntil()`, which returns a [LuaAsyncAction](#luaasyncaction) - you can then chain additional actions onto the sequence of actions from the [LuaAsyncAction](#luaasyncaction).

##### Example

The following sequence of actions will:

1. Rotate to look at a target position.
2. Move towards the target position until we get there.
3. Look back at the point we started at.
4. Move towards the point we started at until we get there.
5. Loop, running the first action in the sequence as well as all following actions.

```lua
this.Action.Do(|| this.LookAt(target.WorldPosition))
           .Then(|| this.MoveTowards(target.WorldPosition, moveSpeed))
             .Until(Condition.WaitForLinearMove(this.GameObject, target.WorldPosition))
           .Then(|| this.LookAt(startPoint))
           .Then(|| this.MoveTowards(startPoint, moveSpeed))
             .Until(Condition.WaitForLinearMove(this.GameObject, startPoint))
           .ThenLoop()
```

##### Functions

- `Do(function)`: Start a sequence of actions by running a function. Returns a [LuaAsyncAction](#luaasyncaction).
- `WaitUntil(condition)`: Start a sequence of actions by waiting until a given [Condition](#condition) is met. Returns a [LuaAsyncAction](#luaasyncaction) so that actions can be chained.
- `Stop()`: Stop running the sequence of actions.

#### LuaAsyncAction

The `LuaAsyncAction` is a single action in the chain of actions run by [LuaAsyncActionRunner](#luaasyncactionrunner).

It can be run once, then wait until a [Condition](#condition) is met, or it can be run continuously until a [Condition](#condition) is met.

##### Functions

- `Until(condition)`: Run this action until a given [Condition](#condition) is met. Returns a [LuaAsyncAction](#luaasyncaction) so that actions can be chained.
- `Then(function)`: Chain another action after this action. Returns a [LuaAsyncAction](#luaasyncaction) so that actions can be chained.
- `ThenWaitUntil(condition)`: Run this action once, then wait until a given [Condition](#condition) is met.
- `ThenFinish()`: End the action chain here and do nothing more. This also begins execution of the actions.
- `ThenLoop()`: End the action chain here and start running the first action in the chain again. This also begins execution of the actions.

### APIs

The following are static APIs that are available to Lua scripts, most of these are accessed as [globals](#globals).

#### DreamSystem

The DreamSystem API is used to access and control various aspects of the day's dream while the player is dreaming. It can be accessed via the `DreamSystem` [global](#globals) in every script.

##### Properties

- `CurrentDream`: The current [Dream](#dream-1) the player is in. Readonly.
- `CurrentDreamIndex`: The integer index of the current [Dream](#dream-1) in the dream list of the current [DreamJournal](#dreamjournal). Note that this will be zero-indexed. Readonly.
- `DayNumber`: The current day number as an integer. Readonly.
- `YearNumber`: The current year number as an integer. This starts at 0 and is incremented by 1 every time the player completes day 365. Readonly.
- `CurrentEnvironment`: The [DreamEnvironment](#dreamenvironment) that is currently active. Readonly.

##### Functions

- `OnDreamTimeout(function)`: Set up a function to be called if the current dream ends via a timeout (the player running out of time instead of ending it via other means). When another dream is linked to, this callback will be removed.
- `SetNextTransitionColor(color)`: Set the fade [Color](#color) of the next transition between dreams. If `nil` then the color will not be overridden.
- `SetTransitionSounds(sounds)`: Set whether or not transitions between dreams will have sounds. `sounds` should be a boolean.
- `SetNextTransitionSpawnID(spawnId)`: Set the entity ID of the [PlayerSpawn](../entities/#playerspawn) the player should spawn at when the next dream is transitioned to. If the ID doesn't exist, a random spawn is used instead.
- `SetNextTransitionDream(dream)`: Set the next [Dream](#dream-1) we should transition to.
- `SetNextTransitionLockControls(lockControls)`: Set whether or not controls are locked for the next transition between dreams. When controls are locked it's like a regular link (the player cannot move or look), when they aren't locked it's like a tunnel link, so they carry on moving (i.e. to run through the tunnel). `lockControls` should be a boolean.
- `TransitionToDream()`: Transition to the dream as configured by `SetNextTransition...()` functions. If the transition [Color](#color) is not set, a color will be chosen according to the current graph score.
- `LogGraphContributionFromEntity(dynamicness, upperness, sourceEntity)`: Log a contribution to the graph for a given entity. This will allow flashbacks to spawn the player here. The scores for `dynamicness` and `upperness` are in range -9 to 9. Scores get averaged for the final graph position in the dream.
  - Arguments:
    - `dynamicness` (integer): The dynamic axis graph score. Meaningful in range -9 to 9.
    - `upperness` (integer): The upper axis graph score. Meaningful in range -9 to 9.
    - `sourceEntity` ([entity](../entities/)): The entity this graph score comes from. This allows the player to spawn in the same place the entity was originally interacted with in a flashback.
- `EndDream()`: Ends the dream/day and returns the player to the graph screen.
- `SetTextureSet(textureSet)`: Sets the current [TextureSet](#textureset-enum), modifying any materials using the texture set shaders.
- `StretchDream(amount, durationSeconds)`: Stretches the geometry of the current dream by `amount` over `durationSeconds`.
  - Arguments:
    - `amount` (number): How much we should stretch by as a ratio, for example 1 would do nothing, 2 would double the size, 0.5 would halve it, etc.
    - `durationSeconds` (number): How long it should take to stretch the dream geometry to the scale given by `amount`.
- `ApplyEnvironment(environment)`: Applies the given [DreamEnvironment](#dreamenvironment), changing the sky and fog.
- `ForceSave()`: Normally the game is saved when the day ends. Calling this will save the game midway through the dream instead. This is basically only useful for saving data stored with the [Persistence](#persistence) API with [Lifetime](#lifetime-enum) `SaveFile`.

#### Input

The Input API is used for getting input. It can be accessed via the `Input` [global](#globals).

##### Properties

- `Current`: The current [ControlScheme](#controlscheme). Readonly.
- `Interact`: The [InputAction](#inputaction) currently bound to the interact action. Readonly.
- `Run`: The [InputAction](#inputaction) currently bound to the run action. Readonly.
- `LookUp`: The [InputAction](#inputaction) currently bound to the look up action. Readonly.
- `LookDown`: The [InputAction](#inputaction) currently bound to the look down action. Readonly.

##### Functions

- `GetMove()`: Returns the current [Vector2](#vector2) movement input.
- `GetLook()`: Returns the current [Vector2](#vector2) look input.
- `GetStrafe()` Returns the current number strafe input.

#### PauseSystem

##### Properties

- `IsPaused`: Boolean, `true` if the game is paused, `false` otherwise. Readonly.

#### Persistence

The Persistence API is a way to store key/value data over given [Lifetime](#lifetime-enum)s. It can be accessed via the `Persistence` [global](#globals).

If you want to store complex data like a table, you can use the [json](https://www.moonsharp.org/additions.html#the-json-module) module of MoonSharp:

```lua
data = {
    a=1,
    b=2,
    c=3
}

-- store the table
Persistence.StoreString("table_value", Lifetime.SaveFile, json.serialize(data))

-- ...

-- retrieve the table
data = json.parse(Persistence.RetrieveString("table_value"))
```

##### Functions

- `Clear(lifetime)`: Clear all data with the given [Lifetime](#lifetime-enum).
- `StoreNumber(key, lifetime, number)`: Store a number with a given key and [Lifetime](#lifetime-enum).
  - Arguments:
    - `key` (string): The key to store the number under.
    - `lifetime` ([Lifetime](#lifetime-enum)): The lifetime of the persisted data.
    - `number` (number): The number to store.
- `StoreString(key, lifetime, string)`: Store a string with a given key and [Lifetime](#lifetime-enum).
  - Arguments:
    - `key` (string): The key to store the number under.
    - `lifetime` ([Lifetime](#lifetime-enum)): The lifetime of the persisted data.
    - `string` (string): The string to store.
- `StoreBoolean(key, lifetime, boolean)`: Store a boolean with a given key and [Lifetime](#lifetime-enum).
  - Arguments:
    - `key` (string): The key to store the number under.
    - `lifetime` ([Lifetime](#lifetime-enum)): The lifetime of the persisted data.
    - `boolean` (boolean): The boolean to store.
- `RetrieveNumber(key)`: Retrieves a number from the store.
  - Arguments:
    - `key` (string): The key of the number to retrieve.
  - Returns:
    - The number stored.
- `RetrieveString(key)`: Retrieves a string from the store.
  - Arguments:
    - `key` (string): The key of the string to retrieve.
  - Returns:
    - The string stored.
- `RetrieveBoolean(key)`: Retrieves a boolean from the store.
  - Arguments:
    - `key` (string): The key of the boolean to retrieve.
  - Returns:
    - The boolean stored.
- `HasValue(key)`: Returns `true` if there is a value for the given `key`, and `false` otherwise.
- `RemoveValue(key)`: Removes the value for the given `key`, if it existed.

#### Unity

The Unity API is used for interfacing with the Unity game engine. It can be accessed via the `Unity` [global](#globals).

##### Functions

- `Vector3(x, y, z)`: Returns a [Vector3](#vector3) consisting of the given number components.
- `ColorRGB(r, g, b)`: Returns a [Color](#color) consisting of the given RGB number components (in range 0 to 1).
- `ColorRGBA(r, g, b, a)`: Returns a [Color](#color) consisting of the given RGBA number components (in range 0 to 1).
- `DeltaTime()`: Return the current delta time in seconds as a number.
- `CurrentTime()`: Return the current time (since LSD:R started) in seconds as a number.

#### Debug

The Debug API can be used to help debug your Lua scripts. It can be accessed via the `Debug` [global](#globals).

##### Functions

- `Log(msg)`: Log the string `msg` to the console.
- `LogWarning(msg)`: Log the string `msg` to the console as a warning.
- `LogError(msg)`: Log the string `msg` to the console as an error.

#### Condition

The Condition API provides a number of conditions that can be used in 'Until' functions of [LuaAsyncActionRunner](#luaasyncactionrunner) and [LuaAsyncAction](#luaasyncaction).
It can be accessed via the `Condition` [global](#globals).

##### Functions

- `Default()`: This returns a condition that is always true.
- `WaitForSeconds(seconds)`: This returns a condition that will be true after `seconds` (number) seconds.
- `WaitForLinearMove(obj, end)`: This returns a condition that will be true when the [GameObject](#gameobject) `obj` reaches the world position [Vector3](#vector3) `end`. This should be used when the object is being moved to the end position in the action.
- `PointingAt(obj, worldPos)`: This returns a condition that will be true when the [GameObject](#gameobject) `obj`'s forward axis is pointing towards [Vector3](#vector3) `worldPos`. This should be used when the object is being rotated to point at the world position in the action.
- `Custom(function)`: This returns a condition that will be true based on the boolean result of the given function. This allows you to implement your own conditions for your actions, providing a high degree of flexibility.

#### Colors

The Colors API provides a bunch of predefined [Color](#color)s for you to access in scripts, without having to create them yourself with the [Unity](#unity) API. It can be accessed via the `Color` [global](#globals).

##### Properties

- `Red`: The [Color](#color) red. Readonly.
- `Black`: The [Color](#color) black. Readonly.
- `Blue`: The [Color](#color) blue. Readonly.
- `Clear`: A [Color](#color) with zero for every component (including alpha). Readonly.
- `Cyan`: The [Color](#color) cyan. Readonly.
- `Gray`: The [Color](#color) gray. Same as `Grey`. Readonly.
- `Grey`: The [Color](#color) grey. Same as `Gray`. Readonly.
- `Magenta`: The [Color](#color) magenta. Readonly.
- `White`: The [Color](#color) white. Readonly.
- `Yellow`: The [Color](#color) yellow. Readonly.
- `Green`: The [Color](#color) green. Readonly.

#### Random

The Random API provides functions for introducing random elements to your scripts. It can be accessed via the `Random` [global](#globals).

##### Functions

- `IntMax(max)`: Return a random integer between 0 and `max` (exclusive, so `max=4` returns integers from 0 to 3).
- `IntMinMax(min, max)`: Return a random integer between `min` and `max` (`max` exclusive, so `min=1, max=4` returns integers from 1 to 3).
- `Float()`: Return a random number between 0 and 1.
- `FloatMax(max)`: Return a random number between 0 and `max` (exclusive, so `max=4.0` returns numbers from 0 to 3.999...).
- `FloatMinMax(min, max)`: Return a random number between `min` and `max` (`max` exclusive, so `min=1.0, max=4.0` returns numbers from 1.0 to 3.999...).
- `OneIn(chance)`: Given a chance for something to occur (i.e. `chance=4` represents a 'one in 4' chance), roll for the chance and return `true` if we succeeded, `false` otherwise. Internally, this runs `Float() < (1.0 / chance)`.
- `Color()`: Return a random [Color](#color) from White, Red, Yellow, Blue, Cyan, Green, Magenta.

#### Screenshotter

The Screenshotter API is used to take screenshots of the game and save them in the screenshots folder. It's the same thing that happens when you press `F9` in-game. The screenshots are saved to the LSD:R's [persistent data path](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html), which is:

- On Windows: `C:\Users\<user>\AppData\Figglewatts\LSDR\screenshots`
- On Mac: `~/Library/Application Support/Figglewatts/LSDR/screenshots`
- On Linux: `~/.config/unity3d/Figglewatts/LSDR/screenshots`

##### Functions

- `TakeScreenshot()`: Take a screenshot of the current view and save it in the screenshot folder.

### Player

The following types are on the player entity (which can be accessed with `GetEntity("__player")`).

#### PlayerMovement

PlayerMovement controls the player's movement.

##### Properties

- `Camera` ([GameObject](#gameobject), readonly): The player camera.

##### Functions

- `SetCanControl(canControl)`: Set whether or not the player controller accepts player input as a boolean.
- `OverrideMovement()`: Call this if you want to override player movement with `StartMovingForward()`/`RotateTowards(position)`. Be sure to call `StopMovementAndRotation()` when finished to return control to the player. For an example see the [BMC staircases](https://github.com/figglewatts/LSDRevamped/blob/master/LSDR/Assets/Resources/Original/STG00/Scripts/bmcStairs.lua).
- `StopMovementAndRotation()`: Stop overriding player movement overridden with `OverrideMovement()`.
- `StartMovingForward()`: When movement is overridden with `OverrideMovement()` this will simulate the player walking straight forwards.
- `RotateTowards(position)` When movement is overridden with `OverrideMovement()` this will simulate the player rotating to face a given `position` [Vector3](#vector3) in worldspace.

#### PlayerCameraRotation

Used to control the player's camera rotation (which is different in classic and revamped control schemes).

##### Properties

- `Enabled` (boolean): Whether or not player camera rotation is enabled.

### Entity

Entities are objects in a [Dream](#dream-1) scene which have a unique ID that can be referenced in other scripts. An entity is a `MonoBehaviour`, so it is a component that can be added to a Unity GameObject. The [GameObject](#gameobject) of an entity can be retrieved in a script using `GetEntity(id)`.

#### Prefab

A [Prefab](../entities/#prefab) is a component that stores a reference to a [GameObject](#gameobject) that can be instantiated through a Lua script.

##### Functions

- `CreateInstance()`: Creates and returns an instance of the [GameObject](#gameobject) associated with this Prefab. The [GameObject](#gameobject) will be set to active when the instance is created.

#### MovieClip

A [MovieClip](../entities/#movieclip) is used to store a reference to a Unity `VideoClip`. It provides a function to play the clip as an in-dream cutscene (after which the dream will end).

##### Functions

- `Play(fadeInColor)`: Calling this will play the associated `VideoClip`, fading in with the given `fadeInColor` [Color](#color).

#### AnimatedObject

An [AnimatedObject](../entities/#animatedobject) is an object with a Unity `Animator` and animations.

##### Functions

- `Play(index)`: Will play the animation clip with the given integer index.
- `Stop()`: Will stop playing the current animation. This effectively pauses the animation.
- `Resume()`: If `Stop()` was previously called, this will resume the animation.

#### InteractiveObject

An [InteractiveObject](../entities/#interactiveobject) is a dynamic, interactable object you can encounter in a dream in LSD: Revamped. It can run a script, animating it, moving it, and whatever else you'd like.

When the player interacts with the object (depending on its interaction mode) a graph contribution can be logged, and the contribution will be saved to the save file so that flashbacks will place the player back at the object.

##### Properties

- `Forward` ([Vector3](#vector3), readonly): The normalised forward vector of this object.
- `Up` ([Vector3](#vector3), readonly): The normalised up vector of this object.
- `Right` ([Vector3](#vector3), readonly): The normalised right vector of this object.
- `GameObject` ([GameObject](#gameobject), readonly): This [InteractiveObject](../entities/#interactiveobject)'s Unity [GameObject](#gameobject).
- `Action` ([LuaAsyncActionRunner](#luaasyncactionrunner), readonly): A [LuaAsyncActionRunner](#luaasyncactionrunner) that can be used to run asynchronous actions for this object. This is used extensively in LSD: Revamped for the behaviour of the objects from the Original Dreams.
- `InteractionDistance` (number): The distance at which the player can interact with this object.

##### Functions

- `Init()`: Manually run the initialisation code for this object. This only needs to be done if an InteractiveObject instance is created via a [Prefab](#prefab) entity in a script.
- `WaitForAnimation(index, offsetSeconds)`: A [condition](#condition) that will return `true` when an animation (on the [AnimatedObject](#animatedobject) of this object) has finished playing.
  - Arguments:
    - `index` (integer): The index of the animation we're waiting for.
    - `offsetSeconds` (number): This number is added to the length of time the animation lasts for, for fine adjustment. Can be negative.
- `GetAnimationLengthSeconds(index)`: Return the length in seconds of the animation (on the [AnimatedObject](#animatedobject) of this object) at integer index `index` as a number.
- `PlayAnimation(index)`: Play the animation (on the [AnimatedObject](#animatedobject) of this object) at integer index `index`.
- `StopAnimation()`: Stop (pause) the currently playing animation (on the [AnimatedObject](#animatedobject) of this object).
- `ResumeAnimation()`: Resume the animation (on the [AnimatedObject](#animatedobject) of this object) of this object.
- `MoveTowards(worldPosition, speed)`: Move towards the given worldspace position at a given speed (scaled by the current delta time). Great to use in an [action](#luaasyncaction).
  - Arguments:
    - `worldPosition` ([Vector3](#vector3)): The world position we are moving towards.
    - `speed` (number): The speed at which we should be moving towards the position at, in units per second. This will be scaled by the current delta time, creating framerate-independent movement.
- `MoveInDirection(worldDirection, speed)`: Move in the given normalised direction in worldspace, at a given speed (scaled by the current delta time). Great to use in an [action](#luaasyncaction).
  - Arguments:
    - `worldDirection` ([Vector3](#vector3)): The normalised world direction vector we should move in.
    - `speed` (number): The speed at which we should be moving towards the position at, in units per second. This will be scaled by the current delta time, creating framerate-independent movement.
- `LookAt(worldPosition)`: Immediately rotate the object to face a given worldPosition. This essentially aligns the forward vector to point towards [Vector3](#vector3) `worldPosition`.
- `LookAtPlane(worldPosition)`: This does the same as `LookAt(worldPosition)`, except it only rotates the yaw of the object, keeping it on the same plane.
- `LookInDirection(worldDirection)`: This does the same as `LookAt(worldPosition)`, except this takes a normalised direction [Vector3](#vector3).
- `LookTowards(worldPosition, speed)`: Rotate to look towards a given world position at a given speed (scaled by the current delta time). This will only change the object's yaw. Great to use in an [action](#luaasyncaction).
  - Arguments:
    - `worldPosition` ([Vector3](#vector3)): The worldspace position to rotate to look towards.
    - `speed` (number): The speed (in degrees per second) to rotate towards the target at. This is scaled by the current delta time, creating framerate-independent rotation.
- `SnapToFloor(checkHeight)`: Snap this object to the floor, giving a configurable `checkHeight` number of how many units down to check for a floor.
- `StretchShrink(factor)`: Stretches the height of this object to a given factor immediately. For example, 1 would do nothing, 2 would double the height, 0.5 would halve the height, etc.
- `SetChildVisible(visible)`: Sets the child [GameObject](#gameobject) of this object visible or not depending on the boolean value `visible`.
- `SetRenderersActive(active)`: Sets the child renderers of this object to be active or not depending on the boolean value `active`. Used to set child meshes etc to nonvisible in an easy way.
- `SetUpdateIntervalSeconds(seconds)`: Set the rate (in seconds, as a number) at which the Lua script function `intervalUpdate()` is called on the script of this object.
- `LogGraphContribution(dynamicness, upperness)`: Log a contribution to the graph for this object. This will allow flashbacks to spawn the player here. The scores for `dynamicness` and `upperness` are in range -9 to 9. Scores get averaged for the final graph position in the dream.
  - Arguments:
    - `dynamicness` (integer): The dynamic axis graph score. Meaningful in range -9 to 9.
    - `upperness` (integer): The upper axis graph score. Meaningful in range -9 to 9.

#### DreamAudio

A [DreamAudio](../entities/#dreamaudio) stores a Unity AudioClip to be played or stopped from a Lua script.

##### Properties

- `Pitch` (number): The pitch of the audio clip. 2 doubles the pitch, 0.5 halves the pitch, etc.
- `Volume` (number): The volume of the audio clip. 0 to 1 range.

##### Functions

- `Play()`: Play the audio clip.
- `Stop()`: Stop playing the audio clip.

### Assets

The following are the Lua APIs available for the various [assets](../assets/) in the SDK.

#### Dream

A [Dream](../assets/#dream) asset.

##### Properties

- `Name` (string, readonly): The name of the dream.
- `Author` (string, readonly): The author of the dream.
- `GreyMan` (boolean, readonly): Whether or not the grey man spawns in this dream.
- `Linkable` (boolean, readonly): Whether or not this dream can be reached from random links.
- `Environments` (array of [DreamEnvironment](#dreamenvironment)s): The environments this dream can have.

#### DreamJournal

A [DreamJournal](../assets/#dreamjournal) asset.

##### Properties

- `Name` (string, readonly): The name of the dream journal.
- `Author` (string, readonly): The author of the dream journal.
- `Dreams` (array of [Dream](#dream-1)s): The dreams in this journal.

##### Functions

- `GetLinkableDream(currentDream)`: Get a random dream from this journal to link to. Pass `currentDream` ([Dream](#dream-1)) in to ensure you don't link to the same dream again.
- `GetDreamFromGraph(x, y)`: Get the dream we should start on from the [GraphSpawnMap](../assets/#graphspawnmap) given integer dynamicness and upperness scores.

#### DreamEnvironment

A [DreamEnvironment](../assets/#dreamenvironment) asset. This can be applied using the [DreamSystem](#dreamsystem) API.

##### Properties

- `FogColor` ([Color](#color)): The color of the fog in this environment.
- `SubtractiveFog` (boolean): Whether to render light or dark fog. Subtractive is dark.
- `FogStartDistance` (number): The distance at which fog starts being applied. Fog here will be at its weakest.
- `FogEndDistance` (number): The distance at which fog finishes being applied. Fog here will be at its strongest.
- `FogHeight` (number): The height of the fog in the sky. Between 0 and 1. 0.25 is a good value.
- `SkyColor` ([Color](#color)): The color of the sky.
- `SkyFogColor` ([Color](#color)): The color of the fog gradient in the sky.
- `SunColor` ([Color](#color)): The color of the sun/moon in the sky.
- `SunChance` (number): The chance the sun has to be enabled. 1 is always, 0 is never, 0.5 is half the time.
- `SunHeight` (number): The height of the sun. 2 works well.
- `SunSize` (number): The scale of the sun. 1 is normal.
- `SecondSunColor` ([Color](#color)): The color of the 2nd sun/moon.
- `SecondSunSize` (number): The size of the 2nd sun.
- `SecondSunChance` (number): The chance the 2nd sun has to be enabled. 1 is always, 0 is never, 0.5 is half the time. This chance stacks on top of `SunChance`.
- `SecondSunOffset` (number): The offset of the 2nd sun (in terms of distance away from the first sun). -0.15 gives a nice eclipse with a black 2nd sun.
- `SecondSunHeight` (number): The height of the 2nd sun. 2 works well.
- `SunBurstColor` ([Color](#color)): The color of the sunburst effect.
- `SunBurstSize`: (number): The scale of the sunburst effect.
- `SunBurstChance`: (number): The chance the sunburst has to appear. 1 is always, 0 is never, 0.5 is half the time. This chance stacks on top of `SunChance`.
- `NumberOfClouds` (integer): The number of clouds to spawn.
- `CloudsChance` (number): The chance clouds have to appear. 1 is always, 0 is never, 0.5 is half the time.
- `CloudsColor` ([Color](#color)): The color of the clouds.
- `NumberOfStars` (integer): The number of stars to spawn.
- `StarsChance` (number): The chance stars have to appear. 1 is always, 0 is never, 0.5 is half the time.
- `StarsColor` ([Color](#color)): The color of the stars.

#### SongAsset

The [SongAsset](../assets/#song) contains data about a song to play (as part of the revamped soundtrack).

##### Properties

- `Name` (string): The name of the song.
- `Author` (string): The author of the song.
- `Clip` (AudioClip): The Unity AudioClip containing the song.
- `IsSilent` (boolean, readonly): True if this song has no audio clip.

#### SongListAsset

The [SongListAsset](../assets/#songlist) contains a list of songs to be provided to a [SongLibrary](#songlibrary).

##### Properties

- `Songs` (array of [SongAsset](#songasset)): The songs in this list.

### Enums

The following are enum types available in the Lua API. They are each available in global scope.

#### TextureSet enum

Used for selecting the currently rendered texture set with the [DreamSystem](#dreamsystem) API. Available in global scope as `TextureSet`.

##### Values

- `Normal`
- `Kanji`
- `Downer`
- `Upper`

#### SongStyle enum

Used for selecting the current song style in the [DreamSystem](#dreamsystem) API. Available in global scope as `SongStyle`.

##### Values

- `Standard`
- `Ambient`
- `Lovely`
- `Human`
- `Electro`
- `Ethnova`
- `Cartoon`

#### Lifetime enum

Used for controlling data lifetimes in the [Persistence](#persistence) API. Available in global scope as `Lifetime`.

##### Values

- `Dream`: This data exists for the current dream's session only - data will be cleared when the next dream is loaded.
- `Mod`: This data exists for the lifetime of the mod. It will be cleared when a new mod is selected, or the game is exited.
- `Session`: This data exists for the lifetime of a game session. It will be cleared when the game exits.
- `SaveFile`: This data gets persisted to the save file, so it will survive game restarts.

### Misc

These are miscellaneous types available in the Lua API.

#### GameObject

This is a Unity GameObject. This is what `GetEntity(id)` will return, and has properties that get components from this GameObject (if they exist). This is how a lot of working with objects in the Lua API works.

##### Properties

- `Name` (string, readonly): The name of the GameObject.
- `WorldPosition` ([Vector3](#vector3)): The worldspace position of this GameObject.
- `LocalPosition` ([Vector3](#vector3)): The local space position of this GameObject.
- `WorldRotation` ([Vector3](#vector3)): The worldspace rotation of this GameObject in Euler angles.
- `LocalRotation` ([Vector3](#vector3)): The local space rotation of this GameObject in Euler angles.
- `Scale` ([Vector3](#vector3)): The local scape of this GameObject.
- `ForwardDirection` ([Vector3](#vector3), readonly): The normalised forward direction of this GameObject.
- `RightDirection` ([Vector3](#vector3), readonly): The normalised right direction of this GameObject.
- `UpDirection` ([Vector3](#vector3), readonly): The normalised up direction of this GameObject.
- `Active` (boolean, readonly): Whether or not this GameObject is active in its hierarchy.

###### Conversion

- `InteractiveObject` ([InteractiveObject](#interactiveobject-1), readonly): Get the [InteractiveObject](#interactiveobject-1) component from this GameObject, if it exists.
- `PlayerMovement` ([PlayerMovement](#playermovement), readonly): Get the [PlayerMovement](#playermovement) component from this GameObject, if it exists.
- `PlayerCamera` ([PlayerCameraRotation](#playercamerarotation), readonly): Get the [PlayerCameraRotation](#playercamerarotation) component from this GameObject, if it exists.
- `Action` ([LuaAsyncActionRunner](#luaasyncactionrunner), readonly): Get the [LuaAsyncActionRunner](#luaasyncactionrunner) component from this GameObject, if it exists.
- `DreamAudio` ([DreamAudio](#dreamaudio), readonly): Get the [DreamAudio](#dreamaudio) component from this GameObject, if it exists.
- `AnimatedObject` ([AnimatedObject](#animatedobject), readonly): Get the [AnimatedObject](#animatedobject) component from this GameObject, if it exists.
- `VideoClip` ([MovieClip](#movieclip), readonly): Get the [MovieClip](#movieclip) component from this GameObject, if it exists.
- `Prefab` ([Prefab](#prefab), readonly): Get the [Prefab](#prefab) component from this GameObject, if it exists.

##### Functions

- `SetActive(active)`: Set this GameObject to be active depending on the value of boolean `active`.
- `GetChildByName(name)`: Get the child GameObject of this gameObject by its string `name`. If no child is found, `nil` will be returned.
- `PositionToWorld(position)`: Convert a [Vector3](#vector3) `position` from the GameObject's local space to world space and return it.
- `DirectionToWorld(direction)`: Convert a [Vector3](#vector3) `direction` from the GameObject's local space to world space and return it.
- `PositionFromWorld(worldPosition)`: Convert a [Vector3](#vector3) worldspace position to the GameObject's local space and return it.
- `DirectionFromWorld(worldDirection)`: Convert a [Vector3](#vector3) worldspace direction to the GameObject's local space and return it.
- `LookAt(worldPosition)`: Immediately rotate the GameObject's forward vector to point towards the given [Vector3](#vector3) `worldPosition`.
- `LookAtPlane(worldPosition)`: The same as `LookAt(worldPosition)`, except it only rotates the GameObject's yaw.
- `DestroySelf()`: Destroys this GameObject.

#### ControlScheme

ControlScheme defines settings around the current control scheme.

##### Properties

- `Name` (string, readonly): The name of this control scheme.
- `FpsControls` (boolean, readonly): Whether or not FPS controls are enabled.
- `MouseSensitivity` (number, readonly): The current mouse sensitivity.
- `InvertLookY` (boolean, readonly): Whether or not the look Y axis is inverted.

#### InputAction

An InputAction is an interface to a binding of a certain input. It contains properties whose boolean values can be used to determine if a given input has been pressed or not.

##### Properties

- `IsPressed` (boolean, readonly): `true` if this input is pressed currently, `false` otherwise.
- `WasPressed` (boolean, readonly): `true` if this input was pressed this frame, `false` otherwise.
- `WasReleased` (boolean, readonly): `true` if this input was released this frame, `false` otherwise.

#### Vector2

A Vector2 is a 2-dimensional vector with number components.

##### Properties

- `x` (number): The X component.
- `y` (number): The Y component.

##### Operators

- `+`: Add 2 Vector2s together.
- `-`: Subtract 2 Vector2s.
- `*`: Multiply a Vector2 by a number.
- `/`: Divide a Vector2 by a number.

##### Functions

- `normalise()`: Return a normalised version of this Vector2.
- `length()`: Return the length of this Vector2 as a number.

#### Vector3

A Vector2 is a 3-dimensional vector with number components.

##### Properties

- `x` (number): The X component.
- `y` (number): The Y component.
- `z` (number): The Z component.

##### Operators

- `+`: Add 2 Vector3s together.
- `-`: Subtract 2 Vector3s.
- `*`: Multiply a Vector3 by a number.
- `/`: Divide a Vector3 by a number.

##### Functions

- `normalise()`: Return a normalised version of this Vector3.
- `length()`: Return the length of this Vector3 as a number.
- `negated()`: Return a negated version of this Vector3.
- `dot(vector)`: Return the dot product (as a number) between this vector and the Vector3 given in `vector`.
- `project(vector)` Return the Vector3 result of `vector` projected on this vector.

#### Color

A Color represents an RGBA color with number components (in range 0 to 1).

##### Properties

- `r` (number): The red component.
- `g` (number): The green component.
- `b` (number): The blue component.
- `a` (number): The alpha component.
