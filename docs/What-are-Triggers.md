---
sidebar_position: 1
---

# What are Triggers

In StarCraft 1, triggers set outcomes for certain game events. The ScmDraft2 editor adds basic triggers.  
Triggers determine what conditions trigger what actions during gameplay.  
For example, "Player 1 wins upon accumulating over 500 ores" means the condition "Player 1 accumulates at least 500 ores" triggers the victory action.  

## Design Structure
The design structure of StarCraft 1 triggers consists of three parts: target Players, Conditions, and Actions.
  
### Target Players
The target players sets which players the trigger affects. A trigger can affect multiple players.  
If no set players are in the game (including computer players), the trigger will not activate (even if conditions are met).  
Target players can only be P1~P8.  
The players set here determine the Current Player in conditions and actions.  
    
### Conditions
A trigger allows up to 16 conditions. When all are met, all actions will execute.  
To execute some actions when only part of the conditions are met, split the trigger into multiple triggers associated with a switch. Then create a trigger to compare the switch status and execute actions.  
Conditions include comparing player resources, unit deaths, countdown timer, and switch states, etc.     

### Actions
A trigger allows up to 64 actions.   
Actions include changing player resources, unit deaths, countdown timer, and switch states, etc.  

  





