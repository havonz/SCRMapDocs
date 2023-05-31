# What are Triggers

Triggers in "Starcraft 1" refer to the configurations set in the map to achieve specific game events during gameplay.  
Classical triggers can be added to maps using map editors like ScmDraft2.  
Triggers set conditions and actions for specific events during gameplay.  
For example, a trigger can be set to "Player 1 victory when they accumulate 500 ore minerals." This means that the condition "Player 1 accumulate 500 ore minerals" will trigger the action "victory".  


## Design Structure
The design structure of StarCraft 1 triggers consists of three parts: target Players, Conditions, and Actions.
  
### Target Players
Indicates which players this trigger is effective for. A trigger can specify multiple effective players at the same time.  
If all the designated players are not in the game (computer players also count as players), the trigger will not be activated (even if the conditions are met).   
The target player can only be one or more of P1~P8.  
The effective players specified here will limit who the CurrentPlayer is in the conditions and actions.  
    
### Conditions
A trigger can specify up to 16 trigger conditions, and when all conditions are met, all actions in the trigger action will be executed.  If it is necessary to execute the action when only some conditions are met, multiple triggers can be used to trigger a switch, and then a trigger can be specified to judge the status of the switch and execute the action.  Common trigger conditions include resource collection conditions, unit death count conditions, countdown timer conditions, switch status conditions, and so on.

### Actions
A trigger can specify up to 64 actions, which will be executed in sequence when all conditions of the trigger are met. Common trigger actions include resource setting, unit death count setting, countdown timer status adjustment, switch state adjustment, and so on.

  





