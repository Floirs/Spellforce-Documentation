# Spellforce-Documentation
Personal Documentation for Spellforce scripting that I made as I was learning.


# DEFINITIONS
**ODER:** This is an OR function that checks if either of the two conditions is true. It returns True if either condition1 or condition2 is true, and False otherwise.For example: ODER(condition1, condition2)

**OR:** This function is similar to ODER. It also checks if either of the two conditions is true and returns True if either condition1 or condition2 is true, and False otherwise.
For example: OR(condition1, condition2)



# CONDITIONALS
**IsGlobalFlagTrue{ Name=“FlagName” }:** This conditional checks if a global flag (identified by “FlagName”) is set to true.

**IsGlobalFlagFalse{ Name=“FlagName” }:** This conditional checks if a global flag (identified by “FlagName”) is set to false.

**Negated():** This function takes a condition as an argument and returns the opposite of that condition's truth value. If the condition is true, Negated(condition) will return false, and vice versa. For example:
`Negated( IsGlobalFlagTrue{ Name=“FlagName” } )`

**IsMonumentInUse( Name=“MonumentName” ):** This conditional checks if a monument (identified by “MonumentName”) is currently in use.

**PlayerHasGood{}:** This conditional checks if the player has a specified good. The properties of the good (such as its type, amount, and side) are defined within the brackets. For example:
```
PlayerHasGood
{
   -– Types of Goods: GoodBoard(wood), GoodStone. 
   -– Sides: SideLight for Human, Elves and Dwarves and "SideDark" for Dark Elves, Trolls and Orcs. 
   Good = <goodType>,
   Amount = <amount>, 
   Side = <side>
}
```
