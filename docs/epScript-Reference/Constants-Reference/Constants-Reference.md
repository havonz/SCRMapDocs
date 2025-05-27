---
sidebar_position: 8
---

# epScript Constants Reference

## TrgCount

```JavaScript
All: 0
Other integers
```

## TrgModifier

```JavaScript
SetTo: 7    // =
Add: 8      // +=
Subtract: 9 // -= (It can be reduced to zero at most, even if a number greater than the current value is subtracted)
```

## TrgComparison

```JavaScript
AtLeast: 0  // >=
AtMost: 1   // <=
Exactly: 10 // ==
```

## TrgSwitchState

```JavaScript
Set: 2
Cleared: 3
```

## TrgSwitchAction

```JavaScript
Set: 4
Clear: 5
Toggle: 6
Random: 11
```

## TrgResource

```JavaScript
Ore: 0
Gas: 1
OreAndGas: 2
```

## TrgAllyStatus

```JavaScript
Enemy: 0
Ally: 1
AlliedVictory: 2
```

## TrgOrder

```JavaScript
Move: 0
Patrol: 1
Attack: 2
```

## TrgPropState

```JavaScript
Enable: 4
Disable: 5
Toggle: 6
```

## TrgScore

```JavaScript
Total: 0
Units: 1
Buildings: 2
UnitsAndBuildings: 3
Kills: 4
Razings: 5
KillsAndRazings: 6
Custom: 7
```

## TrgLocation
```JavaScript
// There are a total of 255 locations/areas, numbered from 0 to 63, 65 to 255
// No. 64 means Anywhere
"Location 1~64": 0~63
"Anywhere": 64
"Location 66~256": 65~255
// When euddraft compiles, it will remove all location/area names from the map string table, that is, location/area names do not exist at runtime and are only used by map developers to distinguish 
```

## TrgSwitch

```JavaScript
// There are a total of 256 switches, numbered 0 to 255
"Switch 1~256": 0~255
// When euddraft compiles, it will remove all switch names from the map string table, that is, switch names do not exist at runtime and are only used by map developers to distinguish
```

## [TrgPlayer](TrgPlayer.md)  
## [TrgUnit](TrgUnit.md)  
## [TrgAIScript](TrgAIScript.md)  
## [Weapon](Weapon.md)  
## [Tech](Tech.md)  
## [Upgrade](Upgrade.md)  
## [UnitOrder](UnitOrder.md)  
## [Flingy](Flingy.md)  
## [Image](Image.md)  
## [Icon](Icon.md)  
## [Iscript](Iscript.md)  
## [Portrait](Portrait.md)  
## [Sprites](Sprites.md)  
## [StatText](StatText.md)

