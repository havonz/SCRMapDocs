---
sidebar_position: 2
---

# What is EUD

For the development of Starcraft maps, EUD is almost the origin of all map development techniques.

## EUD or EPD

### The original EUD
EUD is the abbreviation of Extended Unit Death (a technique to extend the use of unit death count triggers). This technique originates from the buffer overflow vulnerabilities that exist in the Deaths conditions and SetDeaths actions of the trigger editor in the map editor of StarCraft 1.08 and previous versions.  

- The descriptive prototypes of the Deaths condition and SetDeaths action are as follows  
    ```CSS
    Trigger {
        Conditions:
            Deaths(PlayerID, AtLeast/AtMost/Exactly, Number, UnitTypeID);
        Actions:
            SetDeaths(PlayerID, Add/Subtract/SetTo, Number, UnitTypeID);
    }
    ```
    - PlayerID legal range: 0 ~ 26, actually a dword value, value range: `-2147483648 ~ 2147483647` 
    - UnitTypeID legal range: 0 ~ 232, actually a word value, value range: `0 ~ 65535`  

When using illegal PlayerID and UnitTypeID, the Deaths and SetDeaths actions can still take effect in the game. In this way, arbitrary memory read and write is achieved. By trying, it is found that the overflow memory location of Deaths and SetDeaths is fixed, and the memory location that can be accessed is `0x58A364 + 4 * PlayerID + 48 * UnitTypeID`. Of course, you can also simply set the UnitTypeID to 0 and only use the PlayerID for overflow anchor point to access the 32-bit value at the memory location `0x58A364 + 4 * PlayerID`. When accessing less than 4 bytes, you need to first read 4 bytes and then separate the 4 bytes with an algorithm (of course, there is no need to consider this in detail in the [remastered](#eud-in-remastered)).  

EPD refers to the "Special PlayerID" used to access a specific memory location when using PlayerID as an offset anchor point to overflow the buffer using this technique.  
A map containing at least one overflow access of PlayerID or UnitTypeID in Deaths or SetDeaths is called an EUD map.  


### EUD in Remastered
In StarCraft: Remastered, the Deaths/SetDeaths vulnerabilities mentioned above were fixed, so there was no EUD functionality at all when Remastered was first released. However, Blizzard's software engineer [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) developed the "Remastered EUD Simulator" shortly after the release of Remastered. This feature was released with [StarCraft 1.21.0](https://news.blizzard.com/en-gb/starcraft/21313396/patch-1-21-0-the-return-of-eud-maps) in December 2017. Since then, when StarCraft: Remastered encounters a map containing EUD triggers, it will automatically enable the EUD Simulator to execute the triggers in the map, allowing authors to continue to implement EUD functionality in StarCraft: Remastered through Deaths/SetDeaths triggers as before.  

However, in StarCraft: Remastered, Blizzard has restricted map triggers to read and write memory: some memory can only be read but not written, some memory cannot be read or written, and only a small amount of memory can be both read and written. Details in [EUDDB](https://ldconval.github.io/eudtools/Include/EUDDB.html). If an EUD map attempts to read or write illegal memory when running a trigger during gameplay, the game is immediately terminated (pop-up error: Sorry, this EUD map is not currently supported... The error code is a hexadecimal number. Subtracting this hexadecimal number from 0xFFFFFFFF yields the illegal memory address the current trigger is attempting to read or write). This has led to many limitations in the functionality of Remastered EUD technology. For example, Remastered cannot modify anything related to models or images, cannot extend the unit limit, and cannot directly port 1.08's EUD plug-ins to Remastered, etc.  

In StarCraft: Remastered, EUD maps have the following characteristics:  
- The unit limit can only be the original 1700, not the extended 3400 unit limit. (When hosting, the entire "Unit Limit" option row is grayed out and forced to "Original" and cannot be selected as "Extended")
- Unable to save games during gameplay  
- Unable to save replays after the game ends  
- The game will still show defeat in the score screen after winning  

In Remastered, Blizzard's software engineer [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) added bitmask parameters to the Deaths condition and SetDeaths action.  
Conditions and actions using bitmasks can more efficiently determine and write arbitrary byte contents at memory addresses that are not multiples of 4.  
ScmDraft2 or euddraft name this usage DeathsX and SetDeathsX.  
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

  



