# Contents

- [Summary](#summary)
- [Global Events](#global-events)
- [Actions](#actions)
- [Conditionals](#conditionals)
- [Map Scripts](#map-scripts)
- [Npc Scripts](#npc-scripts)

# Summary
This is a personal documentation that I made while learning Spellforce scripting.

A lot of the code and explanations provided here requires the Source Lua files found in: https://github.com/THQNordic/SpellForceLUASources

# Global Events
To be added.

# Actions
To be added.

# Conditionals
## Functions
**OR(condition1, condition2):** This is an OR function that checks if either of the two conditions is true. It returns True if either condition1 or condition2 is true, and False otherwise. 

**ODER(condition1, condition2):** This function is similar to OR. It also checks if either of the two conditions is true and returns True if either condition1 or condition2 is true, and False otherwise.

## Tables
**IsGlobalFlagTrue{Name=“FlagName”}:** This conditional checks if a global flag (identified by “FlagName”) is set to true.

**IsGlobalFlagFalse{Name=“FlagName”}:** This conditional checks if a global flag (identified by “FlagName”) is set to false.

**Negated(condition):** This function takes a condition as an argument and returns the opposite of that condition's truth value. If the condition is true, Negated(condition) will return false, and vice versa.

**IsMonumentInUse(Name=“MonumentName”):** This conditional checks if a monument (identified by “MonumentName”) is currently in use.

**TimeOfDay{Hour=, Minute=, TimeFrame=, UpdateInterval=}**: This conditional checks whether the current time corresponds to the specified time, with a tolerance of + TimeFrame minutes. For example, at 12:30, it must be at least 12:30, and the maximum can be 12:30 + TimeFrame minutes for the condition to be true.
* **Hour =**: Specifies the hour of the day (in 24-hour format) for the condition.
* **Minute =**: Specifies the minute of the hour for the condition. This parameter is optional and defaults to 0 if not specified.
* **TimeFrame =**: Specifies the tolerance in minutes for the condition. This parameter is optional and defaults to 15 minutes if not specified.
* **UpdateInterval =**: Specifies how often the condition should be checked, in GdSteps (10 GdSteps = 1 second). This parameter is optional and defaults to 60 GdSteps if not specified.

**PlayerHasGood{Good=, Amount=, Sides=, Player=, UpdateInterval=}:** This checks whether the player has a certain amount of a certain resource.
* **Good =**: Specifies the type of Good. Goods can be GoodBoard(Wood), GoodStone, GoodMithril, GoodFood, GoodIron, GoodManaElixir(Aria), GoodManaHerb(Lenya).
* **Amount =**: Specifies the minimum amount of the specified resource. This parameter is optional and defaults to 1 if not specified.
* **Sides =**: Specifies which side to check for the resources. Sides can be SideLight (Human, Elves and Dwarves), SideDark (Dark Elves, Trolls and Orcs) or SideAll for both. This parameter is optional and defaults to SideGood if not specified.
* **Player =**: Specifies the Player to be checked (For Multiplayer). This parameter is optional and defaults to 1 if not specified.
* **UpdateInterval =**: Specifies how often the condition should be checked, in GdSteps (10 GdSteps = 1 second). This parameter is optional and defaults to 60 GdSteps if not specified.
<details>
  <summary><b>PlayerHasGood</b> Syntax:</summary>
  <pre><code>
PlayerHasGood
{
    Good = ,
    Amount = 1,
    Side = SideAll,
    Player = 1,
    UpdateInterval = 60,
}
  </code></pre>
</details>


# Map Scripts
In the context of Spellforce scripting, Map Scripts are scripts that control various aspects of the game map. They can manipulate entities, control spawn points, and manage other map-related features. Two key tables available for entity management are `RtsSpawn` and `RtsSpawnNT`.

Map Scripts have access to these tables for spawning and respawning entities. These tables are particularly useful for controlling the behavior of entities in the game, such as their spawn locations, respawn rates, and conditions for spawning.
## Spawn/Respawn Scripts
### RtsSpawn
`RtsSpawn` table is used to spawn groups of entities, typically led by one or more Chiefs. Each group can normally only spawn up to `SpawnLimit` units. If one or more `Chief` is assigned, then the condition `FigureAlive` will automatically be added to the Group, and shall any of the Chiefs die then the spawn will stop prematurely. The spawnrate of units spawned by RtsSpawn seems to be around 11~12 seconds per unit, which is not something you can change.

The `RtsSpawn` table has the following fields:
* **Clan =**: Clan number identifier.
* **MaxClanSize =**: Maximum number of living units throughout all the `Groups`.
* **MaxClanLevel =**: Untested.
* **Groups = {}**: A Table containing different `Group` tables.
* **Conditions = {}**: Conditions for enabling this Clan and subsequently the spawn of all `Groups` within.
* **Effect =**: Untested.
* **Length =**: Untested.

Each `Group` table in the `Groups` table has the following fields:
* **X =**: X-coordinate of the spawn location.
* **Y =**: Y-coordinate of the spawn location.
* **Range =**: Spawn radius of spawned units. This field is optional and defaults to 0 if not specified.
* **WaitTime =**: Time in seconds the group waits before spawning (GlobalTimeElapsed condition).
* **AvatarMinLevel =**: Minimum level of the Avatar for this Group to spawn. This field is optional and defaults to 0 if not specified.
* **AvatarMaxLevel =**: Maximum level of the Avatar for this Group to spawn. This field is optional and defaults to 0 if not specified.
* **SpawnLimit =**: Maximum number of units that can be spawned by this Group. -1 = Spawn Units{} table only once, 0 = No-limit. This field is optional and defaults to 0 if not specified.
* **Chief = {}**: NpcId of the Group Chief (the Group Boss Unit). If he dies, the Group stops spawning.
* **BeginConditions = {}**: Untested. This field is optional and defaults to a empty table if not specified.
* **Conditions = {}**: Conditions for spawn to work. If this fails, only this Group will stop spawning. This field is optional and defaults to a empty table if not specified.
* **ShuffleUnits =**: If TRUE Discards the order set in Units{} table and spawns randomly. This field is optional and defaults to FALSE if not specified.
* **Units = {}**:  Units to spawn in the order specified until it reaches the end, then it starts again from the beginning, except if SpawnLimit is equal to -1, in that case once it reaches the end the Group stop spawning units.
<details>
  <summary><b>RtsSpawn</b> Syntax:</summary>
  <pre><code>
GroupToSpawn_1 =
{
    X = 128,               -- X-coordinate of the spawn location.
    Y = 155,               -- Y-coordinate of the spawn location.
    Range = 3,             -- Units Spawn Radius.
    WaitTime = 1,          -- Time in seconds the group waits before spawning (GlobalTimeElapsed condition).
    AvatarMinLevel = 0,    -- Minimum level of the Avatar for this Group to spawn.
    AvatarMaxLevel = 0,    -- Maximum level of the Avatar for this Group to spawn.
    SpawnLimit = 10,       -- Maximum number of units that can be spawned by this Group. -1 = Spawn Units{} tabl only once, 0 = No-limit.
    Chief = NpcId or {NpcId1, NpcId2, ...}, -- NpcId of the Group Chief (the Group Boss Unit). If any of them die, the Group stops spawning.
    Conditions = {},       -- Conditions for spawn to work. If this fails, only this Group will stop spawning.
    ShuffleUnits = FALSE,  -- If TRUE Discards the order set in Units{} table and spawns randomly.
    Units = {777, 779, },  -- Units to spawn in the order specified until it reaches the end, then it starts again from the beginning, except if SpawnLimit is equal to -1, in that case once it reaches the end the Group stop spawning units.
}
<br>
RtsSpawn
{
    Clan = 3,
    MaxClanSize = 20,
    Groups = {GroupToSpawn_1},
    Conditions = {},
}
  </code></pre>
</details>

## Npc Scripts
To be added.

# Code Examples
<details>
  <summary><b>Placeholder</b></summary>
  
  <pre><code>
  placeholder code
  </code></pre>
</details>

### Player Character Identifier in different parameters (placeholder)
* NpcId = Avatar,
* FigureType = FigureAvatar,

# Units

### Rune Army
Spells and Weapon Spell effects given to Workers, rune army units, titans and swarms do not care about the level and instead the game adjusts the spell level according to the Worker Level. So a level 28 Worker would cause Rune Army spells to scale to lvl 12.


# Items and Objects (barebones for now, just so I don't forget)

### Adding new Items
It's important that after creating your new item, to go into the editor SQL Modifier and sql_item.lua and add the item model, otherwise it'll be invisible ingame.

### Adding new Map Objects
Same as the items section, it's necessary to go into SQL Modifier and sql_object.lua and add the model for the object.
