---
sidebar_position: 2
---

# What is EUD

EUD is the origin of virtually all StarCraft map gameplay techniques.

## EUD or EPD

### Origins of EUD
EUD stands for Extended Unit Death — a technique that extends the use of unit death count triggers. It originated from buffer overflow vulnerabilities in the Deaths condition and SetDeaths action found in the trigger editor of StarCraft's map editor (version 1.08 and earlier).  

- The descriptive prototypes of the Deaths condition and SetDeaths action are as follows  
    ```CSS
    Trigger {
        Conditions:
            Deaths(PlayerID, AtLeast/AtMost/Exactly, Number, UnitTypeID);
        Actions:
            SetDeaths(PlayerID, Add/Subtract/SetTo, Number, UnitTypeID);
    }
    ```
    - PlayerID valid range: 0–26, but actually a dword value with range: `-2147483648 ~ 2147483647` 
    - UnitTypeID valid range: 0–232, but actually a word value with range: `0 ~ 65535`  

Even with out-of-range PlayerID and UnitTypeID values, Deaths and SetDeaths still take effect in-game, enabling arbitrary memory reads and writes. Through experimentation, it was found that the overflowed addresses accessed by Deaths and SetDeaths are deterministic: `0x58A364 + 4 * PlayerID + 48 * UnitTypeID`. You can also set UnitTypeID to 0 and use only PlayerID as the offset anchor, accessing the 32-bit value at `0x58A364 + 4 * PlayerID`. When accessing values smaller than 4 bytes, you must first read 4 bytes and then extract the target bytes algorithmically (this is not a concern in [Remastered](#eud-in-remastered)).  

EPD refers to the special PlayerID value used as an overflow offset anchor to access a specific memory address with this technique.  
A map with at least one Deaths or SetDeaths trigger using an out-of-range PlayerID or UnitTypeID is called an EUD map.  


### EUD in Remastered
StarCraft: Remastered patched the Deaths/SetDeaths vulnerabilities described above, so EUD functionality was completely unavailable at Remastered's initial release. Blizzard software engineer [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) developed the "Remastered EUD Simulator" shortly after Remastered's release, which shipped with [StarCraft 1.21.0](https://news.blizzard.com/en-gb/starcraft/21313396/patch-1-21-0-the-return-of-eud-maps) in December 2017. From that point on, when StarCraft: Remastered encounters a map with EUD triggers, it automatically activates the EUD Simulator to execute them, allowing map authors to continue implementing EUD functionality through Deaths/SetDeaths triggers just as before.  

In StarCraft: Remastered, however, Blizzard restricted memory access from map triggers: some addresses are read-only, some are completely inaccessible, and only a small subset supports both reading and writing. See [EUDDB](https://ldconval.github.io/eudtools/Include/EUDDB.html) for details. If an EUD map trigger attempts to access a restricted address during gameplay, the game terminates immediately with a pop-up error: "Sorry, this EUD map is not currently supported..." The error code is a hexadecimal value; subtracting it from 0xFFFFFFFF gives the restricted address the trigger attempted to access. This results in significant functional limitations for Remastered EUD — for example, it cannot modify model or image data, extend the unit limit, or directly port EUD plug-ins from version 1.08.  

In StarCraft: Remastered, EUD maps have the following characteristics:  
- The unit limit can only be the original 1700, not the extended 3400 unit limit. (When hosting, the entire "Unit Limit" option row is grayed out and forced to "Original" and cannot be selected as "Extended")
- Cannot save during gameplay  
- Cannot save replays after the game ends  
- The game will still show defeat in the score screen after winning  

In Remastered, Blizzard software engineer [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) added bitmask parameters to the Deaths condition and SetDeaths action.  
Conditions and actions using bitmasks can more efficiently read and write arbitrary byte values at memory addresses not aligned to 4-byte boundaries.  
ScmDraft2 and euddraft refer to this usage as DeathsX and SetDeathsX.  
- For DeathsX and SetDeathsX principles, refer to:  
    ```C
    typedef struct { /* 20 bytes */
        uint32_t locationID;
        uint32_t playerID;
        uint32_t num;         // Qualified number (how many/resource amount)
        uint16_t unitID;
        uint8_t comparison;   // Numeric comparison, switch state
        uint8_t condtionType; // http://www.staredit.net/wiki/index.php/Scenario.chk#Trigger_Conditions_List
        uint8_t resType;      // Resource type, score type, Switch number (0-based)
        uint8_t prop;
        uint8_t maskFlag[2];
    } TriggerCondition;

    typedef struct { /* 32 bytes */
        uint32_t locationID;
        uint32_t stringID;
        uint32_t wavNameID;
        uint32_t time;
        uint32_t playerID;
        uint32_t target;    // Second group affected, secondary location (1-based), CUWP #, number, AI script (4-byte string), switch (0-based #)
        uint16_t resType;   // Unit type, score type, resource type, alliance status
        uint8_t actionType; // http://www.staredit.net/wiki/index.php/Scenario.chk#Trigger_Actions_List
        uint8_t num;        // Number of units (0 means All Units), action state, unit order, number modifier
        uint8_t prop;
        uint8_t padding;
        uint8_t maskFlag[2];
    } TriggerAction;

    // http://www.staredit.net/wiki/index.php/Scenario.chk#.22TRIG.22_-_Triggers
    typedef struct {
        TriggerCondition conditions[16]; /* 320 bytes */
        TriggerAction actions[64];       /* 2048 bytes */
        uint32_t flag;
        uint8_t effPlayer[27];
        uint8_t currentAction;
    } Trigger;

    typedef struct { // Trigger node (2408 bytes)
        uint32_t prevTriggerPtr;
        uint32_t nextTriggerPtr;
        Trigger trigger;
    } TriggerNode;

    TriggerNode *tnode = calloc(sizeof(TriggerNode));

    // DeathsX Condition
    tnode->trigger.conditions[0].condtionType = 15; // 15 = Deaths
    tnode->trigger.conditions[0].maskFlag = {'S', 'C'}; // enable bitmask
    tnode->trigger.conditions[0].locationID = Set the bitmask;

    // SetDeathsX Action
    tnode->trigger.actions[0].actionType = 45; // 45 = Set Deaths
    tnode->trigger.actions[0].maskFlag = {'S', 'C'}; // enable bitmask
    tnode->trigger.actions[0].locationID = Set the bitmask;
    ```

    

[RECON-BRX-2018-Starcraft-Emulating-a-buffer-overflow-for-fun-and-profit.pdf](/pdf/RECON-BRX-2018-Starcraft-Emulating-a-buffer-overflow-for-fun-and-profit.pdf)

  



