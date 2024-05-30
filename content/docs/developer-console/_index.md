---
weight: 16
bookFlatSection: true
title: "Developer Console"
---

# Developer Console

LSD: Revamped comes with a developer console that can be accessed in-game at any point by pressing the tilde (`) key. You can press the same key again to close it. This can be very useful for developing, testing, and debugging your mods.

In the console, messages (information, warning, and error) will be logged from various systems of the game.

You can also type commands into the console (and press Return/Enter to submit them) to control various systems.

The format of a command is:

```
Object.Specifier Arguments
```

Where:

- `Object` is the name of an object registered to the console.
- `Specifier` is a function or property of the object.
- `Arguments` is a comma-separated list of arguments.

If `Specifier` is a function, `Arguments` will be the arguments to the function.

If `Specifier` is a property, `Arguments` will be the value to set the property to.

When typing an `Object` or a `Specifier` you can press Tab to view a list of matches for what is available in the given context.

## GreymanSpawner

Used for spawning the grey man in a dream.

This is only available in the console when in a dream.

### Functions

- `GreymanSpawner.Spawn`: Spawn the grey man.

## DreamSystem

Used for controlling dreams.

### Functions

- `DreamSystem.LoadDream dreamName`: Load the [Dream](../assets/#dream) from the current [DreamJournal](../assets/#dreamjournal) with name `dreamName`.
- `DreamSystem.LoadDreamSpawn dreamName, spawnId`: Load the [Dream](../assets/#dream) from the current [DreamJournal](../assets/#dreamjournal) with name `dreamName`, spawning the player at the [PlayerSpawn](../entities/#playerspawn) with ID `spawnId`.
- `DreamSystem.ListDreams`: List the names of the [Dream](../assets/#dream)s from the current journal.
- `DreamSystem.SwitchTextures set`: Switch the current texture set. `set` is an integer.
  - 0: Normal
  - 1: Kanji
  - 2: Downer
  - 3: Upper
- `DreamSystem.ApplyDreamEnvironment index`: Apply the [DreamEnvironment](../assets/#dreamenvironment) at the given `index` from the current [Dream](../assets/#dream).
- `DreamSystem.ListEnvironments`: List the number of [DreamEnvironment](../assets/#dreamenvironment)s the current dream has.
- `DreamSystem.SetSongStyle style`: Set the current song style to the given style.
  - 0: Standard
  - 1: Ambient
  - 2: Lovely
  - 3: Human
  - 4: Electro
  - 5: Ethnova
  - 6: Cartoon
- `DreamSystem.SkipSong`: Skip the current song (equivalent to the button on the pause menu).
- `DreamSystem.TelePlayer id`: Teleport the player to the position of the [entity](../entities/) with given `id`.
- `DreamSystem.ListEntities`: List the entities currently registered.
- `DreamSystem.SetDay dayNumber`: Set the current day number to the given number.

## PlayerMovement

PlayerMovement has a few properties that control how the player moves.

### Properties

- `PlayerMovement.GravityMultiplier` (number): The multiplier for how affected by gravity the player is.
- `PlayerMovement.LinkDelay` (number): The delay in seconds when colliding with something before we link.
- `PlayerMovement.WalkMoveSpeed` (number): The player's move speed when walking.
- `PlayerMovement.RunMoveSpeed` (number): The player's move speed when running.
- `PlayerMovement.StepTimeSeconds` (number): The number of seconds it takes for the player to visually take a single step.
- `PlayerMovement.HeadBobAmount` (number): The amount that the player's camera is affected by head bobbing.
- `PlayerMovement.DebugLog` (boolean): Whether or not debug logging is enabled on the movement controller.

## Screenshotter

Can be used to take screenshots of the game.

### Functions

- `Screenshotter.TakeScreenshot`: Take a screenshot. The same as pressing F9.
