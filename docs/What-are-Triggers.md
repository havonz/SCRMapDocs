---
sidebar_position: 1
---

# What are Triggers

In StarCraft 1 maps, triggers are configurations that set specific outcomes for certain game events. Basic editors like ScmDraft2 allow you to add triggers to maps.  
Triggers define which conditions cause which actions to execute during gameplay.  
For example, a trigger of "Player 1 wins upon accumulating over 500 minerals" means the condition "Player 1 accumulates at least 500 minerals" causes the victory action to execute.  

## Design Structure
The design structure of StarCraft 1 triggers consists of three parts: target Players, Conditions, and Actions.
  
### Target Players
Target players specifies which players the trigger applies to. A trigger can target multiple players simultaneously.  
If no set players are in the game (including computer players), the trigger will not activate (even if conditions are met).  
Target players can only be P1~P8.  
The players set here determine the Current Player in conditions and actions.  
    
### Conditions
A trigger can have up to 16 conditions. When all conditions are met, all actions in that trigger will execute.  
To execute actions when only some of the conditions are met, split the trigger into multiple triggers and link their actions to a switch, then create another trigger to check the switch state and execute the desired actions.  
Conditions include comparing player resources, unit deaths, countdown timer, and switch states, etc.     

### Actions
A trigger can have up to 64 actions.   
Actions include changing player resources, unit deaths, countdown timer, and switch states, etc.  

  





