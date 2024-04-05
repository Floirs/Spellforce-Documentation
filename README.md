# Contents

- [Summary](#summary)
- [Definitions](#definitions)
- [Conditionals](#conditionals)
- [Map Scripts](#map-scripts)

# Summary
This is a personal documentation that I made while learning Spellforce scripting.

A lot of the code and explanations provided here requires the Source Lua files found in: https://github.com/THQNordic/SpellForceLUASources

# Definitions
**ODER:** This is an OR function that checks if either of the two conditions is true. It returns True if either condition1 or condition2 is true, and False otherwise. 
For example: `ODER(condition1, condition2)`

**OR:** This function is similar to ODER. It also checks if either of the two conditions is true and returns True if either condition1 or condition2 is true, and False otherwise.
For example: `OR(condition1, condition2)`



# Conditionals
**IsGlobalFlagTrue{ Name=“FlagName” }:** This conditional checks if a global flag (identified by “FlagName”) is set to true.

**IsGlobalFlagFalse{ Name=“FlagName” }:** This conditional checks if a global flag (identified by “FlagName”) is set to false.

**Negated():** This function takes a condition as an argument and returns the opposite of that condition's truth value. If the condition is true, Negated(condition) will return false, and vice versa. 
For example: `Negated( IsGlobalFlagTrue{ Name=“FlagName” } )`

**IsMonumentInUse( Name=“MonumentName” ):** This conditional checks if a monument (identified by “MonumentName”) is currently in use.

**PlayerHasGood{}:** This conditional checks if the player has a specified good (resource). The properties of the good (such as its type, amount, and side) are defined within the brackets.
For example:
```
PlayerHasGood
{
   -– Types of Goods: GoodBoard(Wood), GoodStone, GoodMithril, GoodFood, GoodIron, GoodManaElixir(Aria), GoodManaHerb(Lenya).
   Good = GoodStone,

   -- Amount: Minimum amount of Goods.
   Amount = 1000,

   -– Sides: SideLight (Human, Elves and Dwarves), SideDark (Dark Elves, Trolls and Orcs) or SideAll for both.
   Side = SideLight
}
```

# Map Scripts
In the context of Spellforce scripting, Map Scripts are scripts that control various aspects of the game map. They can manipulate entities, control spawn points, and manage other map-related features. Two key tables available for entity management are `RtsSpawn` and `RtsSpawnNT`.

Map Scripts have access to these tables for spawning and respawning entities. These tables are particularly useful for controlling the behavior of entities in the game, such as their spawn locations, respawn rates, and conditions for spawning.
### RtsSpawn
`RtsSpawn` is a respawn table. This table is used to spawn groups of entities, typically led by a Chief. The `FigureAlive` condition can be used to enable or disable the spawning and respawning of these entities. Notably, `RtsSpawn` does not depend on Camps and Buildings.

Here's an example of how to use `RtsSpawn`:
```
Blades2 = 
{
    X = 215,
    Y = 290,
    Range = 1,
    Chief = 0,
    WaitTime = 80,
    AvatarMinLevel = 0,
    AvatarMaxLevel = 0,
    Conditions = 
    {
        
    },
    Units = 
    {
        302,
        304,
        305,
        307,
        302 
    },
    ShuffleUnits = TRUE 
}
InitSpawn
{
    Clan = 4,
    Groups = 
    {
        Blades1,
        Blades2 
    },
    Conditions = 
    {
        IsGlobalFlagTrue
        {
            Name = "LightSpawnStart"
        },
        FigureAlive
        {
            NpcId = 302
        } 
    } 
}
RtsSpawn
{
    Clan = 4,
    MaxClanSize = 31,
    Groups = 
    {
        Blades1,
        Blades2 
    },
    Conditions = 
    {
        IsGlobalFlagTrue
        {
            Name = "LightSpawnStart"
        },
        FigureAlive
        {
            NpcId = 302
        } 
    } 
}
```


# Items and Objects (barebones for now, just so I don't forget)

### Adding new Items
It's important that after creating your new item, to go into the editor SQL Modifier and sql_item.lua and add the item model, otherwise it'll be invisible ingame.

### Adding new Map Objects
Same as the items section, it's necessary to go into SQL Modifier and sql_object.lua and add the model for the object.
