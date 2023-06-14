# Built-in Functions

<br />

- [Conditions and Actions](#conditions-and-actions)
    - [Normal Condition Functions](#normal-condition-functions)
        - [Accumulate](#accumulate)
        - [Bring](#bring)
        - [Command](#command)
        - [CountdownTimer](#countdowntimer)
        - [Deaths](#deaths)
        - [Memory](#memory)
        - [Kills](#kills)
        - [ElapsedTime](#elapsedtime)
        - [LeastKills/MostKills](#leastkillsmostkills)
        - [LeastResources/MostResources](#leastresourcesmostresources)
        - [Opponents](#opponents)
        - [Score](#score)
        - [Switch](#switch)
        - [~~Always/Never~~](#alwaysnever)
    - [Extended Condition Functions](#extended-condition-functions)
        - [IsUserCP](#isusercp)
        - [Is64BitWireframe](#is64bitwireframe)
    - [Normal Action Functions](#normal-action-functions)
        - [CenterView](#centerview)
        - [CreateUnit](#createunit)
        - [Defeat/Victory/Draw](#defeatvictorydraw)
        - [DisplayText](#displaytext)
        - [GiveUnits](#giveunits)
        - [KillUnit](#killunit)
        - [LeaderBoard](#leaderboard)
        - [MinimapPing](#minimapping)
        - [ModifyUnit](#modifyunit)
        - [MoveLocation](#movelocation)
        - [MoveUnit](#moveunit)
        - [MuteUnitSpeech/UnMuteUnitSpeech](#muteunitspeechunmuteunitspeech)
        - [Order](#order)
        - [PauseGame/UnpauseGame](#pausegameunpausegame)
        - [PauseTimer/UnpauseTimer](#pausetimerunpausetimer)
        - [PlayWAV](#playwav)
        - [~~PreserveTrigger~~](#preservetrigger)
        - [RemoveUnit](#removeunit)
        - [RunAIScript](#runaiscript)
        - [SetAllianceStatus](#setalliancestatus)
        - [SetCountdownTimer](#setcountdowntimer)
        - [SetDeaths](#setdeaths)
        - [SetMemory](#setmemory)
        - [SetDoodadState](#setdoodadstate)
        - [SetInvincibility](#setinvincibility)
        - [SetMissionObjectives](#setmissionobjectives)
        - [SetNextScenario](#setnextscenario)
        - [SetResources](#setresources)
        - [SetScore](#setscore)
        - [SetSwitch](#setswitch)
        - [TalkingPortrait](#talkingportrait)
        - [Transmission](#transmission)
        - [~~Wait~~](#wait)
    - [Extended Action Functions](#extended-action-functions)
        - [SetKills](#setkills)
        - [SetCurrentPlayer](#setcurrentplayer)
        - [AddCurrentPlayer](#addcurrentplayer)
        - [DisplayTextAll](#displaytextall)
        - [PlayWAVAll](#playwavall)
        - [MinimapPingAll](#minimappingall)
        - [CenterViewAll](#centerviewall)
        - [SetMissionObjectivesAll](#setmissionobjectivesall)
        - [TalkingPortraitAll](#talkingportraitall)
        - [SetNextPtr](#setnextptr)
- [Extended Functions](#extended-functions)
    - [Compile Time](#compile-time)
        - [Get Index](#get-index)
        - [list](#list)
        - [EUDCreateVariables](#eudcreatevariables)
        - [SetVariables](#setvariables)
        - [SCMD2Text](#scmd2text)
        - [unProxy](#unproxy)
        - [UnitProperty](#unitproperty)
        - [GetPropertyIndex](#getpropertyindex)
        - [GetPlayerInfo](#getplayerinfo)
        - [EUDRegisterObjectToNamespace](#eudregisterobjecttonamespace)
        - [GetEUDNamespace](#geteudnamespace)
        - [MPQAddFile](#mpqaddfile)
        - [MPQAddWave](#mpqaddwave)
    - [Compile-time Python Macros](#compile-time-python-macros)
        - [py_print](#py_print)
        - [py_list](#py_list)
        - [py_open](#py_open)
        - [py_eval](#py_eval)
        - [py_str](#py_str)
        - [py_len](#py_len)
        - [py_enumerate](#py_enumerate)
        - [py_range](#py_range)
    - [Compile-time Bytes Conversion](#compile-time-bytes-conversion)
        - [b2i](#b2i)
        - [i2b](#i2b)
        - [u2b/b2u](#u2bb2u)
        - [UTF8 Encode/Decode](#utf8-encodedecode)
    - [General Functions](#general-functions)
        - [EPD](#epd)
        - [l2v](#l2v)
        - [parse](#parse)
        - [EUDFuncPtr](#eudfuncptr)
        - [getgametick](#getgametick)
    - [Trigger Construction Functions](#trigger-construction-functions)
        - [RawTrigger](#rawtrigger)
        - [Trigger](#trigger)
        - [PTrigger](#ptrigger)
        - [DoActions](#doactions)
        - [VProc](#vproc)
    - [Runtime Iterators](#runtime-iterators)
        - [EUDLoopPlayer](#eudloopplayer)
        - [EUDLoopRange](#eudlooprange)
        - [EUDLoopUnit](#eudloopunit)
    - [Display Text Functions](#display-text-functions)
        - [DisplayTextAt](#displaytextat)
        - [print](#print)
        - [GetGlobalStringBuffer](#getglobalstringbuffer)
        - [eprint](#eprint)
        - [TextFX](#textfx)
    - [Players Functions](#players-functions)
        - [getuserplayerid](#getuserplayerid)
        - [playerexist](#playerexist)
        - [Current Player](#current-player)
        - [PColor](#pcolor)
        - [PName](#pname)
        - [SetPName](#setpname)
        - [EUDPlayerLoop](#eudplayerloop)
    - [Location Functions](#location-functions)
        - [setloc](#setloc)
        - [addloc](#addloc)
        - [dilateloc](#dilateloc)
        - [getlocTL](#getloctl)
        - [setloc_epd](#setloc_epd)
    - [Memory Operation Functions](#memory-operation-functions)
        - [dwbreak](#dwbreak)
        - [read/write](#readwrite)
        - [read_epd/write_epd](#read_epdwrite_epd)
        - [add_epd/subtract_epd](#add_epdsubtract_epd)
        - [repmovsd_epd](#repmovsd_epd)
        - [dwepdread_epd](#dwepdread_epd)
        - [cunitread_epd](#cunitread_epd)
        - [posread_epd](#posread_epd)
        - [_cp Series](#_cp-series)
        - [readgen](#readgen)
        - [memcpy](#memcpy)
        - [memcmp](#memcmp)
        - [strcpy](#strcpy)
        - [strcmp](#strcmp)
        - [strlen](#strlen)
        - [strnstr](#strnstr)
        - [dbstr](#dbstr)
        - [ptr2s/epd2s](#ptr2sepd2s)
        - [hptr](#hptr)
        - [gettextptr](#gettextptr)
        - [dwpatch_epd](#dwpatch_epd)
        - [GetMapStringAddr](#getmapstringaddr)
        - [GetTBLAddr](#gettbladdr)
        - [settbl](#settbl)
    - [Math Functions](#math-functions)
        - [atan2](#atan2)
        - [sqrt](#sqrt)
        - [lengthdir](#lengthdir)
        - [pow](#pow)
        - [div](#div)
        - [rand](#rand)
        - [seed](#seed)
        - [randomize](#randomize)
    - [Bitwise Operation Functions](#bitwise-operation-functions)
        - [bitand](#bitand)
        - [bitor](#bitor)
        - [bitnot](#bitnot)
        - [bitxor](#bitxor)
        - [bitnand](#bitnand)
        - [bitnor](#bitnor)
        - [bitnxor](#bitnxor)
        - [bitlshift](#bitlshift)
        - [bitrshift](#bitrshift)
    - [QueueGameCommand Functions](#queuegamecommand-functions)
        - [QueueGameCommand](#queuegamecommand)
        - [QueueGameCommand_MinimapPing](#queuegamecommand_minimapping)
        - [QueueGameCommand_QueuedRightClick](#queuegamecommand_queuedrightclick)
        - [QueueGameCommand_Select](#queuegamecommand_select)
        - [QueueGameCommand_PauseGame](#queuegamecommand_pausegame)
        - [QueueGameCommand_ResumeGame](#queuegamecommand_resumegame)
        - [QueueGameCommand_RestartGame](#queuegamecommand_restartgame)
        - [QueueGameCommand_UseCheat](#queuegamecommand_usecheat)
        - [QueueGameCommand_TrainUnit](#queuegamecommand_trainunit)
        - [QueueGameCommand_MergeDarkArchon](#queuegamecommand_mergedarkarchon)
        - [QueueGameCommand_MergeArchon](#queuegamecommand_mergearchon)

<br />

## Conditions and Actions

- ### Normal Condition Functions

    Normal condition functions are functions encapsulated based on the conditions in classical triggers, just like the trigger conditions in ScmDraft2.  
    Any normal condition function will return a trigger condition expression constant (not a logical value). The concepts of `condition expression` and `condition expression result` need to be distinguished clearly.  
    If you need to use a variable to store the condition expression result, you should pass it into the condition list of a trigger or as an if syntax parameter. You can also use l2v to get the runtime result of the condition expression. See the following example:  
    
    ```JavaScript
    var vc0 = Accumulate(P1, AtLeast, 500, Ore);  // This is wrong!!! It does not return a logical value.
    const c1 = Accumulate(P1, AtLeast, 500, Ore); // This is ok, it returns a constant condition expression, which can be used as RawTrigger or Trigger conditions parameter   

    // Use a variable to store the logical value returned by the condition  
    var vc1 = 0;  
    Trigger(
        conditions = Accumulate(P1, AtLeast, 500, Ore),
        actions = vc1.SetNumber(1),  
    );
    var vc2 = l2v(Accumulate(P1, AtLeast, 500, Ore));
    ```

    <br />

    - #### **Accumulate**

        - `Accumulate`(player : TrgPlayer, AtLeast/AtMost/Exactly : TrgComparison, value, resourceType : TrgResource) : Condition   
            Compare whether the [resourceType] collected by [player] is [AtLeast/AtMost/Exactly] [value]

        Example

        ```JavaScript
        if ( Accumulate(P1, AtLeast, 500, Ore) ) {
            // If player 1 accumulate at least 500 ore minerals
        }
        ```


    <br />

    - #### **Bring**

        - `Bring`(player : TrgPlayer, AtLeast/AtMost/Exactly : TrgComparison, value, unitType : TrgUnit, location : TrgLocation) : Condition  
            Compare whether the number of [unitType] of [player] in [location] is [AtLeast/AtMost/Exactly] [value]  
            
            When the second parameter of Bring is AtMost, it will detect unfinished buildings, incubating creep tumors; it will not detect loaded units or units still in training; it will ignore the height setting of [location].  
            When the second parameter of Bring is AtLeast/Exactly, it will detect loaded units; it will not detect units still in training, unfinished buildings, or incubating creep tumors.  
            Bring cannot detect Scanner Sweep units or Map Revealers.  
            Units killed using KillUnit or KillUnitAt can still be detected by the Bring condition in the current frame; units removed using RemoveUnit or RemoveUnitAt can no longer be detected by the Bring condition in the current frame, and units previously killed using KillUnit or KillUnitAt will also no longer be detected by Bring.  

            [Bring Condition Bug](http://www.staredit.net/wiki/index.php?title=Bring_Condition_Bug)  

        Example

        ```JavaScript
        KillUnitAt(All, "Terran Marine", $L("Location 1"), P1); // Kill all Terran Marines of player 1 at Location 1. After this action, Bring can still detect player 1's Terran Marines at Location 1 in the current frame.   
        RemoveUnitAt(1, "Map Revealer", "Anywhere", P1);        // This action does not remove any units, but it refreshes all units previously killed using KillUnit or KillUnitAt in the current frame to ensure Bring no longer detects units killed in the current frame.    
        if (Bring(P1, AtLeast, 15, "Terran Marine", $L("Location 1"))) {
            // If the number of Terran Marines of player 1 at Location 1 is at least 15  
        }
        ```


    <br />

    - #### **Command**

        - `Command`(player : TrgPlayer, AtLeast/AtMost/Exactly : TrgComparison, value, unitType : TrgUnit) : Condition  
            Compare whether the number of [unitType] under the control of [player] on the map is [AtLeast/AtMost/Exactly] [value]  
            
            When the second parameter of Command is AtMost, it will detect loaded units, units still in training, unfinished buildings, incubating creep tumors.  
            When the second parameter of Command is AtLeast/Exactly, it will detect loaded units; it will not detect units still in training, unfinished buildings, or incubating creep tumors.  
            Command can detect Scanner Sweep units and Map Revealers.  
            Units killed or removed using KillUnit, KillUnitAt, RemoveUnit, RemoveUnitAt can still be detected by the Command condition in the current frame.  
        
        - `CommandMost`(unitType : TrgUnit) : Condition  
            Compare whether the [unitType] under the control of the current player on the map is more than any other player (including neutral players).  
        
        - `CommandLeast`(unitType : TrgUnit) : Condition  
            Compare whether the [unitType] under the control of the current player on the map is less than any other player (including neutral players).  
        
        - `CommandMostAt`(unitType : TrgUnit, location : TrgLocation) : Condition  
            Compare whether the [unitType] under the control of the current player at [location] is more than any other player (including neutral players).  
        
        - `CommandLeastAt`(unitType : TrgUnit, location : TrgLocation) : Condition  
            Compare whether the [unitType] under the control of the current player at [location] is less than any other player (including neutral players). 

        Example

        ```JavaScript
        const cp = getcurpl();  

        foreach (p: EUDLoopPlayer()) {  
            setcurpl(p);  
            if (Command(CurrentPlayer, AtMost, 0, "(buildings)")) { // When the second parameter of Command is AtMost, the statistics will include unfinished units/buildings  
                Defeat();     // If the number of buildings of the current player is at most 0, it is judged as defeat  
            }  

            if (CommandMost("Terran Marine")) {  
                println("Player {} has the most Terran Marines", p);  
            }  

            if (CommandLeast("Terran Marine")) {
                println("Player {} has the least Terran Marines", p);
            }  

            if (CommandMostAt("Terran Marine", $L("Location 1"))) {
                println("Player {} has the most Terran Marines at Location 1", p);
            }  

            if (CommandLeastAt("Terran Marine", $L("Location 1"))) {
                println("Player {} has the least Terran Marines at Location 1", p);
            }
        }  

        setcurpl(cp);
        ```


    <br />

    - #### **CountdownTimer**

        - `CountdownTimer`(AtLeast/AtMost/Exactly : TrgComparison, seconds) : Condition  
            Compare whether the remaining seconds of the countdown timer are [AtLeast/AtMost/Exactly] [seconds] game seconds  

        This condition should not use Exactly to compare because trigger polling does not occur every game second. One game second is 16 game frames, not equal to one real second.  

        Example

        ```JavaScript
        if ( CountdownTimer(AtMost, 1) ) {
            PauseTimer();
        }
        ```

    <br />

    - #### **Deaths**
        - `Deaths`(player : TrgPlayer, AtLeast/AtMost/Exactly : TrgComparison, value, unitType : TrgUnit) : Condition  
            Compare whether the death count of [unitType] of [player] is [AtLeast/AtMost/Exactly] [value]  

            <details><summary>When [player] or [unitType] is out of range</summary>
            
            It is an EUD condition that compares whether the 32-bit unsigned integer stored at `0x58A364 + ([player] * 4 + [unitType] * 48)` is [AtLeast/AtMost/Exactly] [value].  
            Its synchronization depends on the synchronization of the data stored at `0x58A364 + ([player] * 4 + [unitType] * 48)`.  
            </details>
        - `DeathsX`(player: TrgPlayer, AtLeast/AtMost/Exactly: TrgComparison, value, unitType: TrgUnit, mask) : Condition  
            <details><summary>This condition is usually not used to compare player unit deaths</summary>
        
            It is usually used to compare whether the 32-bit (unsigned integer value & [mask]) stored at `0x58A364 + ([player] * 4 + [unitType] * 48)` is [AtLeast/AtMost/Exactly] [value].  
            Its synchronization depends on the synchronization of the data stored at `0x58A364 + ([player] * 4 + [unitType] * 48)`.  
            </details>

        > **Note** 
        > Units killed or removed using trigger actions are not counted in the death count (Deaths);  
        > Suicidal units (Zerg Scourge, Infested Terran, Vulture Spider Mine) that successfully detonate are not counted in the death count (Deaths), but are counted if killed by other units (without successful detonation);  
        > Units killed by allies are also counted in the death count (Deaths).  
        
        Example

        ```JavaScript
        if ( Deaths(P1, AtLeast, 15, "Terran Marine") ) {
            // If player 1 has at least 15 Terran Marines deaths
        }
        ```

    <br />

    - #### **Memory**
        - `Memory`(memoryAddress, AtLeast/AtMost/Exactly : TrgComparison, value) : Condition  
            Compare whether the 32-bit unsigned integer stored at [memoryAddress] is [AtLeast/AtMost/Exactly] [value].  
            Its synchronization depends on the synchronization of the data stored at [memoryAddress].  

        - `MemoryX`(memoryAddress, AtLeast/AtMost/Exactly : TrgComparison, value, mask) : Condition  
            Compare whether the 32-bit (unsigned integer value & [mask]) stored at [memoryAddress] is [AtLeast/AtMost/Exactly] [value]  
            Its synchronization depends on the synchronization of the data stored at [memoryAddress].  

        - `MemoryEPD`(epd, AtLeast/AtMost/Exactly : TrgComparison, value) : Condition  
            Compare whether the 32-bit unsigned integer stored at `0x58A364 + ([epd] * 4)` is [AtLeast/AtMost/Exactly] [value]  
            Its synchronization depends on the synchronization of the data stored at `0x58A364 + ([epd] * 4)`.  

        - `MemoryXEPD`(epd, AtLeast/AtMost/Exactly : TrgComparison, value, mask) : Condition  
            Compare whether the 32-bit (unsigned integer value & [mask]) stored at `0x58A364 + ([epd] * 4)` is [AtLeast/AtMost/Exactly] [value]  
            Its synchronization depends on the synchronization of the data stored at `0x58A364 + ([epd] * 4)`.  

        Example

        ```JavaScript
        function MorphLarvaEPD(epd, newUnit: TrgUnit) {
            if (MemoryXEPD(epd + 0x64/4, Exactly, 35, 0xFFFF)) {
                SetMemoryXEPD(epd + 0x4D/4, SetTo, 42 << 8, 0xFFFF00);
                SetMemoryXEPD(epd + 0x98/4, SetTo, newUnit, 0xFFFF);
            }
        }
        ```

    <br />

    - #### **Kills**

        - `Kills`(player : TrgPlayer, AtLeast/AtMost/Exactly : TrgComparison, value, unitType : TrgUnit) : Condition  
            Compare whether the kills of [unitType] of [player] is [AtLeast/AtMost/Exactly] [value]  

            Kills is not kill score, note the difference  
            Killing own or allied units is not counted in the Kills  
        
        Example

        ```JavaScript
        if ( Kills(P1, AtLeast, 15, "Terran Marine") ) {
            // If player 1 killed at least 15 Terran Marines
        }
        ```

    <br />

    - #### **ElapsedTime**

        - `ElapsedTime`(AtLeast/AtMost/Exactly : TrgComparison, game seconds) : Condition  
            Compare whether the elapsed game time is [AtLeast/AtMost/Exactly] [value] game seconds  

        This condition should not use Exactly to compare because trigger polling does not occur every game second. One game second is 16 game frames, not equal to one real second.  

        Example

        ```JavaScript
        if ( ElapsedTime(AtLeast, 5) ) {
            // The elapsed game time exceeds 5 game seconds
        }
        ```

    <br />

    - #### **LeastKills/MostKills**

        - `LeastKills`(unitType : TrgUnit) : Condition  
            Compare whether the `current player`'s kill count of [unitType] is the least on the map  

        - `MostKills`(unitType : TrgUnit) : Condition  
            Compare whether the `current player`'s kill count of [unitType] is the most on the map  

        Example

        ```JavaScript
         if (LeastKills("Terran Marine")) {  
            // The current player killed the least Terran Marines  
         }  

         if (MostKills("Terran Marine")) {  
            // The current player killed the most Terran Marines  
         }
        ```

    <br />

    - #### **LeastResources/MostResources**

        - `LeastResources`(resourceType : TrgResource) : Condition  
            Compare whether the `current player`'s [resourceType] is the least on the map

        - `MostResources`(resourceType : TrgResource) : Condition  
            Compare whether the `current player`'s [resourceType] is the most on the map

        Example

        ```JavaScript
        if ( LeastResources(Ore) ) {
            // The current player has the least ore
        }

        if ( MostResources(Gas) ) {
            // The current player has the most gas
        }
        ```

    <br />

    - #### **Opponents**

        - `Opponents`(player : TrgPlayer, AtLeast/AtMost/Exactly : TrgComparison, value) : Condition  
            Compare whether the number of opponents of [player] in the current game is [AtLeast/AtMost/Exactly] [value]  

        Example

        ```JavaScript
        if ( Opponents(P1, AtMost, 2) ) {
            // Player 1 has at most 2 opponents
        }
        ```

    <br />

    - #### **Score**

        - `Score`(player : TrgPlayer, score type : TrgScore, AtLeast/AtMost/Exactly : TrgComparison, value) : Condition  
            Compare whether the [score type] score of [player] is [AtLeast/AtMost/Exactly] [value] points  

        - `LowestScore`(score type : TrgScore) : Condition  
            Compare whether the current player's current [score type] is the lowest score  

        - `HighestScore`(score type : TrgScore) : Condition  
            Compare whether the current player's current [score type] is the highest score  

        Example

        ```JavaScript
         if (Score(P1, Kills, AtLeast, 10000)) {  
            // Player 1's kill score is at least 10000 points. Kill score is not kills, note the difference.  
         }  

         if (LowestScore(Buildings)) {  
            // If the current player's building score is now the lowest  
         }  

         if (HighestScore(Kills)) {  
            // If the current player's kill score is now the highest  
         }
        ```

    <br />

    - #### **Switch**

        - `Switch`(switch : TrgSwitch, state : TrgSwitchState) : Condition  
            Compare whether the state of [switch] is [state]  

        Example

        ```JavaScript
        if ( Switch($S("Switch 1"), Set) ) {
            // Switch 1 is Set
        }

        if ( Switch($S("Switch 1"), Cleared) ) {
            // Switch 1 is Cleared
        }
        ```

    <br />

    - #### **~~Always/Never~~**

        - ~~Always() : Condition~~  
            Always executes unconditionally, useless in epScript

        - ~~Never() : Condition~~  
            Never executes, In most cases, this function is useless in epScript.  

    <br />
    <br />

- ### Extended Condition Functions 


    - #### **IsUserCP**

        - `IsUserCP()`: Condition  
            Desync condition used to check if the local player is the current player  

    <br />

    - #### **Is64BitWireframe**

        - `Is64BitWireframe()`: Condition  
            Desync condition used to check if the local Starcraft client is 64-bit  


    <br />
    <br />

- ### Normal Action Functions

    Normal action functions are functions encapsulated based on classical triggers in ScmDraft 2.  
    Any trigger action function (including extended trigger functions) returns an action expression constant. The concepts of `action expression` and `executing action expression` need to be clearly distinguished.  
    If the non-comment code between two semicolons is only a call to an action function, epScript will pass it to an unconditional trigger DoActions for execution. Refer to the example:  

    ```JavaScript
    const a1 = CenterView($L("Location 1")); // This declares an action expression constant and does not execute it
    CenterView($L("Location 1")); // This means executing an action, equivalent to DoActions(CenterView($L("Location 1")));
    DoActions(a1); // The a1 declared in the first line is executed at this time
    ```

    <br />

    - #### **CenterView**

        - `CenterView`(location : TrgLocation) : Action  
            Allows desync execution and sets the `current player`'s camera to the [location]  

        Example

        ```JavaScript
        setcurpl(P1);
        CenterView($L("Location 1"));
        ```

    <br />

    - #### **CreateUnit**

        - `CreateUnit`(number, unitType : TrgUnit, location : TrgLocation, player : TrgPlayer) : Action  
            Create [number] of [unitType] for [player] at [location]. The moment a unit is created, the supply used by the unit will immediately (in the current frame) increase.  

        - `CreateUnitWithProperties`(number, unitType : TrgUnit, Where : location : TrgLocation, player : TrgPlayer, properties : TrgProperty) : Action  
            Create [number] of [unitType] with [properties] for [player] at [location]. The moment a unit is created, the supply used by the unit will immediately (in the current frame) increase.  

        Example

        ```JavaScript
        CreateUnit(2, "Terran Siege Tank", $L("Location 1"), P1);
        CreateUnitWithProperties(1, "Terran Marine", $L("Location 1"), P1, UnitProperty(
             hitpoint = 100,       // Health percentage  
             shield = 100,         // Shield percentage   
             energy = 100,         // Energy percentage  
             hanger = 0,           //  
             resource = 0,         //  
             cloaked = False,      // Whether invisible  
             burrowed = False,     // Whether burrowed 
             intransit = False,    // Whether being transported  
             hallucinated = False, // Whether hallucinated
             invincible = False)   // Whether invincible  
        );
        ```

    <br />

    - #### **Defeat/Victory/Draw**

        - `Defeat()` : Action  
            The current player loses and ends the game 

        - `Victory()` : Action  
            The current player wins and ends the game

        - `Draw()` : Action  
            All players draw and end the game

    <br />

    - #### **DisplayText**

        - `DisplayText`(text : TrgString) : Action  
            Allows desync execution and displays [text] on the next line of the text area on the current player's screen  

            > The argument [text] of this action is actually the number of this text entry in the map string table. If this entry does not exist in the map string table, epScript will first insert [text] into the map string table and then use its ID as its argument.   

        Example

        ```JavaScript
        const idx = $T("Hello StarCraft!");
        dbstr_print(GetMapStringAddr(idx), "WTF StarCraft!");
        DisplayText("Hello StarCraft!"); // Outputs WTF StarCraft!
        ```

    <br />

    - #### **GiveUnits**

        - `GiveUnits`(number : TrgCount, unitType : TrgUnit, owner : TrgPlayer, area : TrgLocation, recipient : TrgPlayer) : Action  
            Give up to [number] [unitType] units of [owner] player in [area] to [recipient] player. [number] = 0 represents all units.  

            > This action will cause the rally points of the given units to be lost.  

        Example

        ```JavaScript
        // Give up to 3 Marines of Player 2 in Location 1 to Player 1
        GiveUnits(3, "Terran Marine", P2, $L("Location 1"), P1);
        ```

    <br />

    - #### **KillUnit**

        - `KillUnit`(unitType : TrgUnit, player : TrgPlayer) : Action  
            Kill all [unitType] units of [player], including units still in the build queue and nuclear missiles not yet launched in nuclear silos.  

        - `KillUnitAt`(number : TrgCount, unitType : TrgUnit, specified area : TrgLocation, player : TrgPlayer) : Action  
            Kill up to [number] [unitType] units of [player] in [specified area]. [number] = 0 represents all units. Does not include units still in the build queue or nuclear missiles not yet launched in nuclear silos.  

            > KillUnitAt(All, "Scanner Sweep", "Anywhere", P1) cannot kill Scanner Sweeps.  
            > KillUnitAt(All, "Map Revealer", "Anywhere", P1) cannot kill Map Revealers.  

            > **Warning**  
            > This action has a bug:  
            > If this action kills any unit inside a transporter (Dropship/Bunker, etc.), then all units of the same type inside that transporter (Dropship/Bunker, etc.) will be killed, and these killed units will not be counted within the [number] parameter specified.  
            > For example, if executing KillUnitAt(1, "Terran Marine", "Location 1", P1) kills a marine inside a bunker in Location 1, then all marines in that bunker will be killed, and if there are more marines outside the bunker in that area, one more will be killed.  

        Example

        ```JavaScript
        KillUnit("Terran Marine", P1); // Kill all Marines of Player 1
        KillUnitAt(3, "Terran Siege Tank", $L("Location 1"), P1) // Kill up to 3 Tanks of Player 1 in Location 1
        ```

    <br />

    - #### **LeaderBoard**

        - `LeaderBoardComputerPlayers`(state : TrgPropState) : Action  
            Set the enabled state of the LeaderBoard for computer players.  

        - `LeaderBoardControl`(unitType : TrgUnit, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' [unitType] control counts described as [label].  

        - `LeaderBoardControlAt`(unitType : TrgUnit, area : TrgLocation, label : TrgString) : Action  
           Display a LeaderBoard in descending order of all players' [unitType] control counts in [area] described as [label].  

        - `LeaderBoardGoalControl`(goal, unitType : TrgUnit, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' [unitType] control counts closest to [goal] described as [label].  

        - `LeaderBoardGoalControlAt`(goal, unitType : TrgUnit, area : TrgLocation, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' [unitType] control counts in [area] closest to [goal] described as [label].  

        - `LeaderBoardGoalKills`(goal, unitType : TrgUnit, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' kills of [unitType] closest to [goal] described as [label].  

        - `LeaderBoardGoalResources`(goal, resourceType : TrgResource, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' collections of [resourceType] closest to [goal] described as [label].  

        - `LeaderBoardGoalScore`(goal, scoreType : TrgScore, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' [scoreType] scores closest to [goal] described as [label].  

        - `LeaderBoardGreed`(goal) : Action  
            Display a LeaderBoard in descending order of all players' collections of crystal and gas mines closest to [goal].  

        - `LeaderBoardKills`(unitType : TrgUnit, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' kills of [unitType] described as [label].  

        - `LeaderBoardResources`(resourceType : TrgResource, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' collections of [resourceType] described as [label].  

        - `LeaderBoardScore`(scoreType : TrgScore, label : TrgString) : Action  
            Display a LeaderBoard in descending order of all players' [scoreType] scores described as [label].  

        Example

        ```JavaScript
        LeaderBoardGoalControlAt(10, "Terran Marine", $L("Destination"), "Marines reaching destination");
        ```

    <br />

    - #### **MinimapPing**

        - `MinimapPing`(location : TrgLocation) : Action  
            Allow desync execution. Display a Ping on the minimap at [specified location] for the current player.  

        Example

        ```JavaScript
        // Display Ping at Location 1 on Minimap for Player 1
        setcurpl(P1);
        MinimapPing($("Location 1"));
        ```

    <br />

    - #### **ModifyUnit**

        - `ModifyUnitEnergy`(number : TrgCount, unitType : TrgUnit, player : TrgPlayer, area : TrgLocation, percentage) : Action  
            Change the energy of up to [number] [unitType] units of [player] in [area] to [percentage] percent. [number] = 0 represents all (All) units.  

        - `ModifyUnitHangarCount`(added, number : TrgCount, unitType : TrgUnit, player : TrgPlayer, area : TrgLocation) : Action  
            Add up to [added] loaded units to up to [number] [unitType] units of [player] in [area]. [number] = 0 represents all (All) units.  

            For example, Interceptors in Carriers, Scarabs in Reavers. Note that this action cannot add Spider Mines to Vultures.  

        - `ModifyUnitHitPoints`(number : TrgCount, unitType : TrgUnit, player : TrgPlayer, area : TrgLocation, percentage) : Action  
            Change the hit points of up to [number] [unitType] units of [player] in [area] to [percentage] percent. [number] = 0 represents all (All) units.  

        - `ModifyUnitResourceAmount`(number : TrgCount, player : TrgPlayer, area : TrgLocation, new value) : Action  
            Change the resource value of up to [number] units of [player] in [area] to [new value]. [number] = 0 represents all (All) units.  

        - `ModifyUnitShields`(number : TrgCount, unitType : TrgUnit, player : TrgPlayer, area : TrgLocation, percentage) : Action  
            Change the shields of up to [number] [unitType] units of [player] in [area] to [percentage] percent. [number] = 0 represents all (All) units.  

        Example

        ```JavaScript
        // Change the hit points of up to 100 Marines of Player 1 in Location 1 to 100%
        ModifyUnitHitPoints(100, "Terran Marine", P1, $L("Location 1"), 100);
        ```

    <br />

    - #### **MoveLocation**

        - `MoveLocation`(location : TrgLocation, unitType : TrgUnit, player : TrgPlayer, area : TrgLocation) : Action  
            Move the center of [location] onto one of the [unitType] units of [player] in [area].  

        Example

        ```JavaScript
        // Move the center of Location 1 onto one of Player 1's Marines anywhere on the map
        MoveLocation($L("Location 1"), "Terran Marine", P1, $L("AnyWhere"));
        ```

    <br />

    - #### **MoveUnit**

        - `MoveUnit`(number : TrgCount, unitType : TrgUnit, player : TrgPlayer, startArea : TrgLocation, targetLocation : TrgLocation) : Action  
            Instantly move up to [number] [unitType] units of [player] in [startArea] to [targetLocation]. [number] = 0 represents all (All) units.  

        Example

        ```JavaScript
        // Instantly move up to 10 Marines of Player 1 in Location 1 to Location 2
        MoveUnit(10, "Terran Marine", P1, $L("Location 1"), $L("Location 2"));
        ```

    <br />

    - #### **MuteUnitSpeech/UnMuteUnitSpeech**

        - `MuteUnitSpeech()` : Action  
            Allow desynchronized execution. Mute all speeches of the current player's units (except trigger units' speeches).  

        - `UnMuteUnitSpeech()` : Action  
            Allow desynchronized execution. Unmute all speeches of the current player's units (except trigger units' speeches).  

    <br />

    - #### **Order**

        - `Order`(unitType : TrgUnit, player : TrgPlayer, startArea : TrgLocation, order : TrgOrder, targetLocation : TrgLocation) : Action  
            Issue an [order] for [player]'s [unitType] units in [startArea] towards the center of [targetLocation].  

        Example

        ```JavaScript
        // Order all Marines of Player 1 in Location 1 to attack-move to the center of Location 2
        Order("Terran Marine", P1, $L("Location 1"), Attack, $L("Location 2"))
        ```

    <br />

    - #### **PauseGame/UnpauseGame**

        - `PauseGame()` : Action  
            Pause the game for all players.  

        - `UnpauseGame()` : Action  
            Unpause the game for all players.  

    <br />

    - #### **PauseTimer/UnpauseTimer**

        - `PauseTimer()` : Action  
            Pause the countdown timer for all players.  

        - `UnpauseTimer()` : Action  
            Unpause the countdown timer for all players.  

    <br />

    - #### **PlayWAV**

        - `PlayWAV`(WAVName : TrgString) : Action  
            Allow desynchronized execution. Play a WAV file named [WAVName] for the current player.

    <br />

    - #### **~~PreserveTrigger~~**

        - ~~PreserveTrigger() : Action~~  
            Preserve the trigger. Because classical triggers become disabled after executing once, this action is needed to repeatedly trigger. Useless in epScript.

    <br />

    - #### **RemoveUnit**

        - `RemoveUnit`(unitType : TrgUnit, player : TrgPlayer) : Action  
            Remove all [unitType] units of [player] from the map, including units still in the production queue. Can remove nuclear missiles that have not been launched in Nuclear Silos. The supply used by the removed units will decrease in the next frame.  

        - `RemoveUnitAt`(number : TrgCount, unitType : TrgUnit, area : TrgLocation, player : TrgPlayer) : Action  
            Remove up to [number] [unitType] units of [player] in [area] from the map, [number] = 0 represents all (All) units. Does not include units still in the production queue. Will not remove nuclear missiles that have not been launched in Nuclear Silos. The supply used by the removed units will decrease in the next frame.  

            > RemoveUnitAt(All, "Scanner Sweep", "Anywhere", P1) cannot remove Scanner Sweeps.   
            > RemoveUnitAt(All, "Map Revealer", "Anywhere", P1) cannot remove Map Revealers.  

            > **Warning**  
            > This action has a bug:  
            > If this action removes any unit inside a transporter (Dropship/Bunker, etc.), then all units of the same type inside that transporter (Dropship/Bunker, etc.) will be removed, and these removed units will not be counted within the [number] parameter specified.  
            > For example, if executing RemoveUnitAt(1, "Terran Marine", "Location 1", P1) removes a marine inside a bunker in Location 1, then all marines in that bunker will be removed, and if there are more marines outside the bunker in that area, one more will be removed.  

    <br />

    - #### **RunAIScript**

      - `RunAIScript`(Script : TrgAIScript) : Action  
            Run AI script [Script] for the current player.  

      - `RunAIScriptAt`(Script : TrgAIScript, area : TrgLocation) : Action  
            Run AI script [Script] for the current player in [area].  
        
        Example

        ```JavaScript
        RunAIScriptAt("Terran Custom Level", $L("Location 1"));
        ```

    <br />

    - #### **SetAllianceStatus**

      - `SetAllianceStatus`(targetPlayer : TrgPlayer, status : TrgAllyStatus) : Action  
            Set the alliance status of the current player towards [targetPlayer] to [status].  

        Example

        ```JavaScript
        // Set Player 1 allied towards Player 2, Player 2 enemy towards Player 1
        setcurpl(P1);
        SetAllianceStatus(P2, Ally);
        setcurpl(P2);
        SetAllianceStatus(P1, Enemy);
        ```

    <br />

    - #### **SetCountdownTimer**

      - `SetCountdownTimer`(SetTo/Add/Subtract : TrgModifier, number) : Action  
            Set countdown timer to [SetTo/Add/Subtract] [number] game seconds. 1 game second = 16 frames.  
        
        Example

        ```JavaScript
        SetCountdownTimer(SetTo, 100); // Set countdown timer to 100 game seconds  
        SetCountdownTimer(Add, 5); // Add 5 game seconds to countdown timer       
        SetCountdownTimer(Subtract, 3); // Subtract 3 game seconds from countdown timer 
        ```

    <br />

    - #### **SetDeaths**

        - `SetDeaths`(player : TrgPlayer, SetTo/Add/Subtract : TrgModifier, value, unitType : TrgUnit) : Action  
            Set the number of deaths of [player]'s [unitType] units to [SetTo/Add/Subtract] [value].  

            <details><summary>When [player] or [unitType] is out of range</summary> 

            It will be an EUD action to set the current value stored at memory address `0x58A364 + ([player] * 4 + [unitType] * 48)` to [SetTo/Add/Subtract] [value].  
            Whether desynchronized execution is allowed depends on the synchronicity of the data stored at memory address `0x58A364 + ([player] * 4 + [unitType] * 48)`. 
            </details> 

      - `SetDeathsX`(player : TrgPlayer, SetTo/Add/Subtract: TrgModifier, value, unitType: TrgUnit, mask) : Action  
         <details><summary>This function is not usually used to set player unit death counts.</summary>

        ```Markdown
         SetTo   : Set the current value stored at memory address `0x58A364 + ([player] * 4 + [unitType] * 48)` to `current value - (current value & mask) + (value & mask)`  
         Add     : Set the current value stored at memory address `0x58A364 + ([player] * 4 + [unitType] * 48)` to `current value - (current value & mask) + ( ((current value & mask) + (value & mask)) & mask )`  
         Subtract: Set the current value stored at memory address `0x58A364 + ([player] * 4 + [unitType] * 48)` to `current value - (current value & mask) + ( ((current value & mask) - (value & mask)) & mask )` (the subtraction in the formula can subtract to a minimum of 0)  
        ```
        Whether desynchronized execution is allowed depends on the synchronicity of the data stored at memory address `0x58A364 + ([player] * 4 + [unitType] * 48)`  
        </details>


        Example

        ```JavaScript
        SetDeaths(P1, Add, 10, "Terran Marine"); // Add 10 to the number of deaths of Player 1's Marines
        ```

    <br />

    - #### **SetMemory**

        - `SetMemory`(memoryAddress, SetTo/Add/Subtract : TrgModifier, value) : Action  
            Set the 32-bit positive integer value stored at [memoryAddress] to [SetTo/Add/Subtract] [value].  
            Whether desynchronized execution is allowed depends on the synchronicity of the data stored at [memoryAddress].  

        - `SetMemoryX`(memoryAddress, SetTo/Add/Subtract : TrgModifier, value, mask) : Action  
            <details><summary>SetMemory that supports mask access, can modify any one or more bits of 32 bits.</summary>

            ```Markdown
            SetTo   : Set the current value stored at [memoryAddress] to `current value - (current value & mask) + (value & mask)`  
            Add     : Set the current value stored at [memoryAddress] to `current value - (current value & mask) + ( ((current value & mask) + (value & mask)) & mask )`  
            Subtract: Set the current value stored at [memoryAddress] to `current value - (current value & mask) + ( ((current value & mask) - (value & mask)) & mask )` (the subtraction in the formula can subtract to a minimum of 0)  
            ```
           Whether desynchronized execution is allowed depends on the synchronicity of the data stored at [memoryAddress].  
            </details>


        - `SetMemoryEPD`(epd : TrgPlayer, SetTo/Add/Subtract : TrgModifier, value) : Action  
            Set the 32-bit positive integer value stored at memory address `0x58A364 + ([epd] * 4)` to [SetTo/Add/Subtract] [value].  
            Whether desynchronized execution is allowed depends on the synchronicity of the data stored at memory address `0x58A364 + ([epd] * 4)`. 

        - `SetMemoryXEPD`(epd : TrgPlayer, SetTo/Add/Subtract : TrgModifier, value, mask) : Action  
            <details><summary>SetMemoryEPD that supports mask access, can modify any one or more bits of 32 bits.</summary>

            ```Markdown
            SetTo   : Set the current value stored at memory address `0x58A364 + ([epd] * 4)` to `current value - (current value & mask) + (value & mask)`  
            Add     : Set the current value stored at memory address `0x58A364 + ([epd] * 4)` to `current value - (current value & mask) + ( ((current value & mask) + (value & mask)) & mask )`  
            Subtract: Set the current value stored at memory address `0x58A364 + ([epd] * 4)` to `current value - (current value & mask) + ( ((current value & mask) - (value & mask)) & mask )` (the subtraction in the formula can subtract to a minimum of 0)  
            ```
            Whether desynchronized execution is allowed depends on the synchronicity of the data stored at memory address `0x58A364 + ([epd] * 4)`.  
            </details>

        Example
        ```JavaScript
        function MorphLarvaEPD(epd, newUnit: TrgUnit) {
            if (MemoryXEPD(epd + 0x64/4, Exactly, 35, 0xFFFF)) {
                SetMemoryXEPD(epd + 0x4D/4, SetTo, 42 << 8, 0xFFFF00);
                SetMemoryXEPD(epd + 0x98/4, SetTo, newUnit, 0xFFFF);
            }
        }
        ```

    <br />

    - #### **SetDoodadState**

        - `SetDoodadState`(state : TrgPropState, unitType : TrgUnit, player : TrgPlayer, area : TrgLocation) : Action  
            Set the Doodad state of [player]'s [unitType] units in [area] to [state].  

        Example

        ```JavaScript
        SetDoodadState(Enable, "Terran Marine", P1, $L("Location 1"));
        ```

    <br />

    - #### **SetInvincibility**

        - `SetInvincibility`(state : TrgPropState, unitType : TrgUnit, player : TrgPlayer, area : TrgLocation) : Action  
            Set the invincibility state of [player]'s [unitType] units in [area] to [state].  

        Example

        ```JavaScript
        SetInvincibility(Enable, "Terran Marine", P1, $L("Location 1")); // Set the invincibility state of Player 1's Marines in Location 1 to Enable
        SetInvincibility(Disable, "Terran Marine", P1, $L("Location 1")); // Set the invincibility state of Player 1's Marines in Location 1 to Disable
        ```

    <br />

    - #### **SetMissionObjectives**

        - `SetMissionObjectives`(text : TrgString) : Action  
             Allows desynchronized execution. Set the current player's mission objectives description to [text].   

            > The argument [text] of this action is actually the ID of this text entry in the Map String Table. If this entry does not exist in the Map String Table, epScript will first insert this [text] into the Map String Table and then use its ID as the argument.  

        Example

        ```JavaScript
        SetMissionObjectives("Our objectives are:\nNo cavities!");
        ```

    <br />

    - #### **~~SetNextScenario~~**

        - ~~`SetNextScenario`(text : TrgString) : Action~~  
            Set the name of the next map to load after the current game ends to [text]. Must be in the same directory.   

            > **Note**
            > `SetNextScenario` is singleplayer-only and currently does not work on EUD maps (useless at the moment).


    <br />

    - #### **SetResources**

        - `SetResources`(player : TrgPlayer, SetTo/Add/Subtract : TrgModifier, value, resourceType : TrgResource) : Action  
         Set [player]'s [resourceType] resources to [SetTo/Add/Subtract] [value].  

        Example

        ```JavaScript
        SetResources(P1, Add, 1000, Ore); // Give Player 1 1000 Ore
        SetResources(P1, Substract, 1000, Gas); // Take 1000 Gas from Player 1
        SetResources(P1, SetTo, 5000, OreAndGas); // Set Player 1's Ore and Gas resources to 5000
        ```

    <br />

    - #### **SetScore**

        - `SetScore`(player : TrgPlayer, SetTo/Add/Subtract : TrgModifier, value, scoreType : TrgScore) : Action  
            Set [player]'s [scoreType] score to [SetTo/Add/Subtract] [value].  

        Example

        ```JavaScript
        SetScore(P1, Add, 1000, Kills); // Give Player 1 1000 Kills score
        ```

    <br />

    - #### **SetSwitch**

        - `SetSwitch`(switchName : TrgSwitch, switchAction : TrgSwitchAction) : Action  
            Set the state of switch [switchName] to [switchAction].  

        Example

        ```JavaScript
        SetSwitch($S("Switch 1"), Set);    // Set Switch 1 state to Set
        SetSwitch($S("Switch 1"), Clear);  // Set Switch 1 state to Cleared
        SetSwitch($S("Switch 1"), Toggle); // Toggle Switch 1 state, if it was originally Set it will be switched to Cleared, if it was originally Cleared it will be switched to Set
        SetSwitch($S("Switch 1"), Random); // Set Switch 1 to a random state, after using it Switch 1's state may be Set or may be Cleared
        ```

    <br />

    - #### **TalkingPortrait**

        - `TalkingPortrait`(unitType : TrgUnit, milliseconds) : Action  
            Allows desynchronized execution. Display the portrait of [unitType] at the current player's unit portrait for [milliseconds] game milliseconds.  

        Example

        ```JavaScript
        // Have the Marine give instructions to Player 1
        setcurpl(P1);
        TalkingPortrait("Terran Marine", 5000);
        ```

    <br />

    - #### **Transmission**

        - ~~`Transmission`(unitType : TrgUnit, area : TrgLocation, WAVName : TrgString, SetTo/Add/Subtract : TrgModifier, time, text : TrgString) : Action~~  
            Allows desynchronized execution. Play a sound [WAVName] for the current player and display the portrait of [unitType] units in [area] at the player's unit portrait for [SetTo/Add/Subtract] [time] game milliseconds, while pinging the unit on the minimap and outputting the text information [text].  

            > **Note**
            > This function will affect trigger control flow, it is not recommended to use in epScript.

        Example

        ```JavaScript
        Transmission("Terran Marine", $L("Location 3"), "sound\\Zerg\\Advisor\\ZAdUpd00.WAV", Add, 5000, "Our objectives are:\nNo cavities！");
        ```

    <br />

    - #### **~~Wait~~**

        - ~~Wait(milliseconds) : Action~~

            > **Note**
            > This function will affect trigger control flow, it is not recommended to use in epScript.


    <br />
    <br />


- ### Extended Action Functions

    Extended action functions are functions that cannot be found in classical triggers but can still be regarded as trigger actions. They will return action constant expressions or expression lists. 
    They can be added to RawTrigger or DoActions action lists, and may no longer be a single trigger action.   

    <br />

    - #### **SetKills**

        - `SetKills`(player : TrgPlayer, SetTo/Add/Subtract : TrgModifier, value, unitType : TrgUnit) : Action | [Action]  
            Set [player]'s kills against [unitType] to [SetTo/Add/Subtract] [value].  
            Kills are not kill scores, note the difference.  
            Composed of one or three classical trigger actions, depending on whether the player number is CurrentPlayer.  

        Example

        ```JavaScript
        // https://euddb.website/?pg=entry&id=502  
        // In fact, there is no SetKills action in classical triggers, it is simulated by EUD.  

        // If the player number is not CurrentPlayer (13), its internal implementation is probably like this, returning a constant expression  
        SetDeaths(player number - 2736, SetTo/Add/Subtract, value, unitType);  

        // If the player number is CurrentPlayer (13), its internal implementation is probably like this, returning a tuple containing three constant expressions  
        DoActions(  
             SetDeaths(EPD(0x6509B0), Add, -2736, 0),  
             SetDeaths(CurrentPlayer, SetTo/Add/Subtract, value, unitType),  
             SetDeaths(EPD(0x6509B0), Add, 2736, 0),  
         ); 
        ```

    <br />

    - #### **SetCurrentPlayer**

        - `SetCurrentPlayer`(playerID : TrgPlayer) : [Action]  
            Allows desynchronized execution. Set `CurrentPlayer` and cpcache to [playerID].  
            Composed of three classical trigger actions.  

        Example

        ```JavaScript
        RawTrigger(actions = list(
            SetCurrentPlayer(P1),
            DisplayText("Content displayed to Player 1"),
            SetCurrentPlayer(P2),
            DisplayText("Content displayed to Player 2"),
            SetCurrentPlayer(P3),
            DisplayText("Content displayed to Player 3"),
        ));
        ```

    <br />

    - #### **AddCurrentPlayer**

        - `AddCurrentPlayer`(playerid : TrgPlayer) : [Action]  
            Allows desynchronized execution. Set `CurrentPlayer` and cpcache to [playerID].  
            Composed of three classical trigger actions.  

        Example

        ```JavaScript
        RawTrigger(actions = list(
            SetCurrentPlayer(P3),
            DisplayText("Content displayed to Player 3"),
            AddCurrentPlayer(-1),
            DisplayText("Content displayed to Player 2"),
            AddCurrentPlayer(-1),
            DisplayText("Content displayed to Player 1"),
        ));

        // https://euddb.website/?pg=entry&id=426
        // Set all game speeds from Slowest to Fastest to 200%
        RawTrigger(actions = list(
            SetCurrentPlayer(EPD(0x5124D8)),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
        ));
        ```

    <br />

    - #### **DisplayTextAll**

        - `DisplayTextAll`(text : TrgString) : [Action]  
            Display [text] on the next line of the text area for all players (including observers).  
            Composed of two classical trigger actions.

    <br />

    - #### **PlayWAVAll**

        - `PlayWAVAll`(WAVName) : [Action]  
            Play the sound [WAVName] for all players (including observers).  
            Composed of two classical trigger actions.  

    <br />

    - #### **MinimapPingAll**

        - `MinimapPingAll`(location) : [Action]  
            Issue a ping at [location] on the minimap for all players (including observers).  
            Composed of two classical trigger actions.  

    <br />

    - #### **CenterViewAll**

        - `CenterViewAll`(location) : [Action]  
            Center the camera on [location] for all players (including observers).  
            Composed of two classical trigger actions.  

    <br />

    - #### **SetMissionObjectivesAll**

        - `SetMissionObjectivesAll`(text : TrgString) : [Action]  
            Set the mission objectives text to [text] for all players (including observers).  
            Composed of two classical trigger actions.  

    <br />

    - #### **TalkingPortraitAll**

        - `TalkingPortraitAll`(unitType, milliseconds) : [Action]  
            Display the portrait of [unitType] at all players' (including observers) unit portraits for [milliseconds] game milliseconds.  
            Composed of two classical trigger actions.  

    <br />

    - #### **SetNextPtr**

        - `SetNextPtr`(trg, dest) : Action  
            Set the next trigger pointer of trigger [trg] to [dest].  
            It is essentially a SetDeaths action `SetDeaths(EPD(trg + 4), SetTo, dest, 0)`  

        Example

        ```javascript
        // The .getDestAddr() method of a variable can obtain the destination address in the variable trigger at compile time.
        // The .getValueAddr() method of a variable can obtain the value address in the variable trigger at compile time. 
        // The .GetVTable() method of a variable can obtain the virtual trigger address of the variable at compile time.
        // The .SetModifier(method) method of a variable sets the numeric modification method of the variable trigger to method.
        // The SetNextPtr(trg, ptr) function is used to set the next trigger of trg to ptr.
        function afterTriggerExec() {  
            var a, b = 3, 5;  
            const next = Forward();  
            RawTrigger(  
                actions = list(  
                    SetMemory(b.getValueAddr(), Add, 1),  
                    SetMemory(a.getValueAddr(), Add, 1),  
                    SetMemory(b.getDestAddr(), SetTo, EPD(a.getValueAddr())),  
                    b.SetModifier(Add), // Internally it is probably implemented as SetMemoryX(b.getValueAddr() + 4, SetTo, (Add << 24), 0xFF000000)  
                    SetNextPtr(b.GetVTable(), next), // Set the next trigger of b to next, the internal implementation may be like this: SetMemory(b.GetVTable() + 4, SetTo, next)  
                ),  
                nextptr = b.GetVTable(), // The next trigger of this trigger is b  
            );  
            next.__lshift__(NextTrigger()); // The essence of this is to point next, the Forward, to the next Trigger  
            // The result is a:10 b:6  
        }
        ```


    <br />
    <br />
    <br />

## Extended Functions  
- ### Compile Time

    - #### **Get Index**

        - `$L`(areaName : literal) : py_int  
        - `GetLocationIndex`(areaName : py_str) : py_int  
        - `EncodeLocation`(areaName : py_str) : py_int  
            Gets the [areaName] defined in the map editor (usually SCMD) converted to the corresponding area ID. All functions with TrgLocation type parameters will automatically use this macro.  

        - `$T`(text : literal) : py_int  
        - `GetStringIndex`(text : py_str) : py_int  
        - `EncodeString`(text : py_str) : py_int  
            Gets the ID of an [text] entry in the map string table (Map String Table). All functions with TrgString type parameters will automatically use this macro, that is, functions that accept TrgString type parameters actually accept the ID of the string entry in the map string table.  

            > This macro can arbitrarily provide [text] keys. If the map string table already contains the [text] entry, the ID of the entry in the map string table is returned. If the map string does not have a [text] entry, a new ID→text key-value pair is inserted into the map string table and the ID is returned.

            > For example, $T("Force 1") will return 4 because it already exists. And $T("\x03Pool farmer") may return 3 (and at the same time create a new item 3: "\x03Pool farmer" in the map string dictionary).  

        - `$S`(switchName : literal) : py_int  
        - `GetSwitchIndex`(switchName : py_str) : py_int  
        - `EncodeSwitch`(switchName : py_str) : py_int  
            Gets the [switchName] defined in the map editor (usually SCMD) converted to the corresponding switch ID. All functions with TrgSwitch type parameters will automatically use this macro.  

        - `$U`(unitType : literal) : py_int  
        - `GetUnitIndex`(unitType : py_str) : py_int  
        - `EncodeUnit`(unitType : py_str) : py_int  
            Gets the [unitType] in the map unit.dat converted to the corresponding unitTypeID. All functions with TrgUnit type parameters will automatically use this macro.  

        - `$B`(TBLKey : literal) : py_int  
        - `EncodeTBL`(TBLKey : py_str) : py_int  
            Gets the index ID corresponding to [TBLKey] in the map TBL dictionary. All functions with StatText type parameters will automatically use this macro.  

        - `EncodeWeapon`(weaponName : py_str) : py_int  
            Gets the [weaponName] in the map weapon.dat converted to the corresponding weapon name ID. All functions with Weapon type parameters will automatically use this macro.  

        - `EncodeTech`(techName : py_str) : py_int  
            Gets the [techName] in the map tech.dat converted to the corresponding technology name ID. All functions with Tech type parameters will automatically use this macro.  

        All $ syntax macros ($L, $T, $S, $U, $B) only support literal strings as parameters.  

        Example

        ```PHP
        const l1 = $L("Location 1");
        const s2 = $S("Switch 2");
        const ut = $U("Terran Marine"); // Returns 0
        const aiid = $B("AI Harass Here"); // Returns 1538

        // Change the string ID of the SCV unit name. $T("\x03Pool farmer") will insert a new string into the map during compilation and return the ID of this string here.
        // https://euddb.website/?pg=entry&id=258
        wwrite(0x660260 + 2 * $U("Terran SCV"), $T("\x03Pool farmer"));

        // You can directly use $ syntax constants 
        // EncodePlayer 
        if (EncodePlayer(P1) == $P1) { py_print($P1, $P2, $CurrentPlayer, $AllPlayers, $Force1, $NonAlliedVictoryPlayers); } 
        // EncodeModifier 
        if (EncodeModifier(SetTo) == $SetTo) { py_print($SetTo, $Add, $Subtract); }
        // EncodeComparison 
        if (EncodeComparison(AtLeast) == $AtLeast) { py_print($Exactly, $AtLeast, $AtMost); } 
        // EncodeResource
        if (EncodeResource(OreAndGas) == $OreAndGas) { py_print($Ore, $Gas, $OreAndGas); }
        // EncodeSwitchAction 
        if (EncodeSwitchAction(Set) == $Set) { py_print($Set, $Clear, $Toggle, $Random); } // $Set == EncodeSwitchAction(Set)  
        // EncodeSwitchState
        if (EncodeSwitchState(Set) != $Set) { py_print($Cleared); } // Note this!!! $Set != EncodeSwitchState(Set)    
        // EncodeAllyStatus 
        if (EncodeAllyStatus(Ally) == $Ally) { py_print($Enemy, $Ally, $AlliedVictory); }   
        // EncodeOrder 
        if (EncodeOrder(Move) == $Move) { py_print($Move, $Patrol, $Attack); }   
        // EncodePropState
        if (EncodePropState(Enable) == $Enable) { py_print($Enable, $Disable, $Toggle); }
        // EncodeScore 
        if (EncodeScore(Total) == $Total) { py_print($Total, $Units, $Buildings, $UnitsAndBuildings, $Razings, $KillsAndRazings, $Custom); } // There is no $Kills, EncodeScore(Kills) == 4 
        // EncodeCount
        if (EncodeCount(All) == 0) { py_print(EncodeCount(All)); } // EncodeCount(All) is 0, EncodeCount any positive integer is equal to that positive integer itself  

        // Some less commonly used ones, I didn't write the documentation and examples, the usage is consistent, refer to the constants reference
        // EncodeAIScript
        // EncodeFlingy
        // EncodeIcon
        // EncodeImage
        // EncodeIscript
        // EncodePortrait
        // EncodeProperty
        // EncodeSprite
        // EncodeUnitOrder
        // EncodeUpgrade
        ```

    <br />

    - #### **list**

        - `list`(*args) : py_list  
            Creates and returns a flat compile-time Python list. It requires at least one argument. This function will automatically flatten lists and cannot create multi-dimensional lists.  
            If you want to create an empty compile-time list, use the py_list function.  
            Compile-time lists can be iterated at compile time using foreach syntax, and their indices can only be constants.  

        Example

        ```JavaScript
        var a, b, c, d;  
        const list1 = list(a, b, c, d); // list is a compile-time container, it just references a/b/c/d here, not the values of a/b/c/d
        const list2 = list(15, 4, list(99, 47)); // The list will be flattened, no multi-dimensional list will be created, this is equivalent to list(15, 4, 99, 47)
        foreach(i, v : py_enumerate(list2)) {
            list1[i] = v;  
        }
        println("{}, {}, {}, {}", a, b, c, d); // 15, 4, 99, 47
        ```

    <br />

    - #### **EUDCreateVariables**

        - `EUDCreateVariables`(count) : py_list[EUDVariable]  
            Creates [count] variables at compile time and returns a compile-time list containing references to the created variables.   
            Compile-time lists can be iterated at compile time using foreach syntax, and their indices can only be constants.  

        Example

        ```JavaScript
        const vs = EUDCreateVariables(3);
        vs[0] = 1;
        vs[1] = 2;
        vs[2] = 3;
        // vs[0], vs[1], and vs[2] are three variables. vs does not exist at runtime, so the index of vs must be a compile-time constant.
        ```

    <br />

    - #### **SetVariables**

        - `SetVariables`(varList : py_list, number : py_list, opList : py_list)  
            Uses at least one trigger to set all variables in [varList] to [number] values according to the corresponding operators in [opList]. This macro is used to optimize.  
            Even if the target value is a variable, it will not take effect dynamically before the action is completed. It will only keep the target value as the state when it started executing.  

        Example

        ```JavaScript
        var a, b, c = 10, 10, 10;
        SetVariables(
        list(  a,    b,      c   ),
        list(  3,    2,      4   ),
        list(SetTo, Add, Subtract),
        );
        // The above code is equivalent to
        const op1 = list(a,    SetTo, 3),
                    list(b,      Add, 2),
                    list(c, Subtract, 4);
        SeqCompute(op1);
        ```

    <br />

    - #### **SCMD2Text**

        - `SCMD2Text`(text) : py_str  
            Compile-time converts the hexadecimal numeric values <XX> in the text [text] to the corresponding ASCII characters.  

        Example

        ```JavaScript
        // The following two lines are equivalent
        simpleprint(SCMD2Text("<03>Haha<02>")); // Print yellow Haha
        simpleprint("\x03Haha\x02"); // Print yellow Haha
        ```

    <br />

    - #### **unProxy**

        - `unProxy`(x) : duck  
            Compile-time gets the pointer value of the reference type x.  

        Example

        ```JavaScript
        const a = EUDArray(list(9, 8, 7));
        var b = unProxy(a);
        const c = EUDArray.cast(b + 4); // c is actually a reference to a[1]
        c[0] = 888888;
        println("b:{} a:{} c:{} a[1]:{} c[0]:{}", b, a, c, a[1], c[1]); // b:421156492 a:421156492 c:421156496 a[1]:888888 c[1]:7
        ```

    <br />

    - #### **UnitProperty**

        - `UnitProperty`(...) : CUWP  
            Compile-time inserts a `Create Unit with Properties` into the map and returns it. You can use GetPropertyIndex to get its number. See the example for field explanations.

        Example

        ```JavaScript
        // All fields are optional  
        const prop = UnitProperty(
            hitpoint = 100,       // HP percentage 0~100  
            shield = 100,         // Shield percentage 0~100  
            energy = 100,         // Energy percentage 0~100  
            hanger = 0,           // 0~4294967295  
            resource = 0,         // 0~65536 (Count)  
            cloaked = False,      // Is invisible True/False
            burrowed = False,     // Is burrowed True/False
            intransit = False,    // Is being transported True/False
            hallucinated = False, // Is a hallucination True/False
            invincible = False    // Is invincible True/False
        );  
        CreateUnitWithProperties(1, "Terran Marine", $L("Location 3"), P1, prop);
        ```

    <br />

    - #### **GetPropertyIndex**

        - `GetPropertyIndex`(property : CUWP) : py_int  
            Compile-time gets the number of a CUWP [property] in the map CUWP list.

        Example

        ```JavaScript
        // All fields are optional
        const prop = UnitProperty(
            hitpoint = 100,       // HP percentage 0~100
        );
        py_print(GetPropertyIndex(prop));
        ```

    <br />

    - #### **GetPlayerInfo**

        - `GetPlayerInfo`(player: TrgPlayer) : py_struct  
            Compile-time gets the information of player [player] in the map information.[player] only supports constants. The information obtained is the information set in the map, not the runtime information.  

        Example

        ```JavaScript
        const pinfo = GetPlayerInfo(0);
        setcurpl(0);
        printAt(9, "Player 1 type:{}({}) race:{}({}) force:{}", pinfo.typestr, pinfo.type, pinfo.racestr, pinfo.race, pinfo.force);
        ```

    <br />

    - #### **EUDRegisterObjectToNamespace**

        - `EUDRegisterObjectToNamespace`(funcname, obj) : duck  
            Registers an object to the global namespace, mainly used for parameter passing between modules.

        Example

        ```JavaScript
        const menuSel = PVariable();
        EUDRegisterObjectToNamespace("menuSel", menuSel);
        ```

    <br />

    - #### **GetEUDNamespace**

        - `GetEUDNamespace()` : py_dict[str, duck]  
            Gets the global namespace dictionary, which records the objects registered in EUDRegisterObjectToNamespace. 

        Example

        ```JavaScript
        function afterTriggerExec() {
            setcurpl(P1);
            const menuSel2 = GetEUDNamespace().get("menuSel");
            println(9, "{}", menuSel2[0]);
        }
        ```

    <br />

    - #### **MPQAddFile**

        - `MPQAddFile`(fname, contents, isWave = false)  
            Adds a file named [fname] with byte content [contents] to the output map set by output. If [isWave] is set to true, it will be compressed using Wave lossy compression before importing.  

        Example

        ```JavaScript
        MPQAddFile("1.txt", py_open("C:/1.txt", "rb").read());
        ```

    <br />

    - #### **MPQAddWave**

        - `MPQAddWave`(fname, content)  
            It is equivalent to MPQAddFile(fname, contents, true).  

        Example

        ```JavaScript
        MPQAddWave("1.wav", py_open("C:/1.wav", "rb").read());
        ```


    <br />
    <br />


- ### Compile-time Python Macros

    In epScript, you can call all built-in functions of Python 3 with the py_ prefix. Here are some common ones.

    <br />

    - #### **py_print**

        - `py_print`(*args)  

            Compile-time uses Python's print function to print output content to the compile log interface for debugging output.  

        Example

        ```JavaScript
        py_print("This will only output to the CLI during compilation and has nothing to do with the map.");
        ```

        <br />

    - #### **py_list**

        - `py_list`(iter) : py_list  
            Creates and returns a compile-time Python list.  
            Compile-time lists can be iterated at compile time using foreach syntax, and their indices can only be constants.  

        Example

        ```JavaScript
        const lst = py_list();
        lst.append(DisplayText("11111"));
        lst.append(DisplayText("22222"));
        lst.append(DisplayText("33333"));
        DoActions(lst);
        ```

    <br />

    - #### **py_open**

        - `py_open`(filename, mode) : py_file  
            Compile-time opens the file [filename] in [mode] mode and returns a Python file object.  

        Example

        ```JavaScript
        function onPluginStart() {
            MPQAddWave("1.wav", py_open("C:/1.wav", "rb").read()); // Import an external file into the map
        }
        ```

    <br />

    - #### **py_eval**

        - `py_eval`(str) : duck  
            Simple Python code execution at compile time, returning the result.

        Example

        ```JavaScript
        const locs = py_eval('[EncodeLocation(("Location {}").format(x)) for x in range(1, 4)]'); // Return list($L("Location 1"), $L("Location 2"), $L("Location 3")) from Python  
        const actions = py_eval('[]'); // Return an empty list from Python
        foreach(loc : py_enumerate(locs)) {
        actions.append(CreateUnit(1, "Terran Marine", loc, P1)); // Add an action to the list
        }   
        DoActions(actions); // Execute all actions in the list at once  
        ```

    <br />

    - #### **py_str**

        - `py_str`(val) : py_str  
            Compile string wrapping conversion at compile time. 

        Example

        ```JavaScript
        const actions = py_list();
        foreach(i : py_range(0, 3)) {
            actions.append(CreateUnit(1, "Terran Marine", py_str("Location ") + py_str(i + 1), P1)); // Add an action to the list
        }
        DoActions(actions); // Execute all actions in the list at once
        ```

    <br />

    - #### **py_len**

        - `py_len`(gconstant) : py_int  
            Gets the length of global constants at the compile-time Python level.  

        Example

        ```JavaScript
        const lst = py_list();
        lst.append(DisplayText("11111"));
        lst.append(DisplayText("22222"));
        lst.append(DisplayText("33333"));
        foreach(i : py_range(0, py_len(lst))) {
            DoActions(lst[i]);
        }
        ```

    <br />

    - #### **py_enumerate**

        - `py_enumerate`(vlist) : py_iter  
            Compile-time enumeration iterator to enumerate and expand items in compile-time containers.  

        Example

        ```JavaScript
        var a, b, c, d;
        const list1 = list(a, b, c, d); // list is a compile-time container. Here it just puts the references of a/b/c/d together instead of the values of a/b/c/d.
        const list2 = list(15, 4, 99, 47);
        foreach(i, v : py_enumerate(list2)) {
            list1[i] = v;
        }
        println("{}, {}, {}, {}", a, b, c, d); // 15, 4, 99, 47
        ```

    <br />

    - #### **py_range**

        - `py_range`(start, end, step) : py_iter  
            Compile-time counting iterator to iterate from [start] to [end] in steps of [step] and expand the code block. Includes [start] but excludes [end].  

        Example

        ```JavaScript
        // https://euddb.website/?pg=entry&id=542
        // https://euddb.website/?pg=entry&id=543
        // The principle of reading memory values is to use 32 triggers to compare the value of each bit in 32 bits. If the i-th bit is greater than 0, the variable is appended with 2 to the power of i-1.
        function GetMouseXY() {
            var x, y;
            RawTrigger(actions = list(
                SetDeaths(EPD(x.getValueAddr()), SetTo, 0, 0),
                SetDeaths(EPD(y.getValueAddr()), SetTo, 0, 0),
            ));
            foreach(i : py_range(32)) { /* Use 64 triggers here to read mouse X Y values */
                RawTrigger(
                    conditions = DeathsX(EPD(0x6CDDC4), AtLeast, 1, 0, py_pow(2,i)),
                    actions = SetDeaths(EPD(x.getValueAddr()), Add, py_pow(2,i), 0),
                );
                RawTrigger(
                    conditions = DeathsX(EPD(0x6CDDC8), AtLeast, 1, 0, py_pow(2,i)),
                    actions = SetDeaths(EPD(y.getValueAddr()), Add, py_pow(2,i), 0),
                );
            }
            return x, y;
        }

        // The above GetMouseXY function is actually equivalent to the following
        function GetMouseXY() {
            return dwread(0x6CDDC4), dwread(0x6CDDC8);
        }
        ```


    <br />
    <br />

- ### Compile-time Bytes Conversion

    <br />

    - #### **b2i**

        - `b2i1`(content, index) : py_int  
        - `b2i1`(content, index) : py_int  
        - `b2i4`(content, index) : py_int  
            Converts the byte, word, or dword at position [index] in the literal byte string [content] to a positive integer constant using little endian.  

        Example

        ```JavaScript
        printAt(2, "0x{:x},0x{:x},0x{:x},0x{:x}", b2i1(b"fuck"), b2i1(b"fuck",1), b2i1(b"fuck",2), b2i1(b"fuck",3)); // 0x66, 0x75, 0x63, 0x6B
        printAt(3, "0x{:x},0x{:x},0x{:x}", b2i2(b"fuck"), b2i2(b"fuck",1), b2i2(b"fuck",2)); // 0x7566, 0x6375, 0x6B63
        printAt(4, "0x{:x}", b2i4(b"fuck")); // 0x6B637566
        ```

    <br />

    - #### **i2b**

        - `i2b1`(i) : py_byte  
        - `i2b2`(i) : [py_byte]  
        - `i2b4`(i) : [py_byte]  
            Converts the integer constant [i] to one, two or four byte constants using little endian.  

        Example

        ```JavaScript
        printAt(4, "{}", i2b4(0x6B637566));
        ```

    <br />

    - #### **u2b/b2u**

        - `u2b`(s) : [py_byte]  
        - `b2u`(b) : py_str  
            Converts between byte literal and string literal.  

        Example

        ```JavaScript
        printAt(5, "{}", u2b("fuck")); // b'fuck'
        printAt(6, "{}", b2u(b"fuck")); // fuck
        ```

    <br />

    - #### **UTF8 Encode/Decode**

        - `b2utf8`(str) : [py_byte]  
            Decodes [str] using UTF-8.  

        - `u2utf8`(str) : py_str  
            Encodes [str] using UTF-8. 

        Example

        ```JavaScript
        printAt(5, "{}", b2utf8("fuck")); // b'fuck'
        printAt(6, "{}", u2utf8(b"fuck")); // fuck
        ```

    <br />
    <br />
    

- ### General Functions

    <br />

    - #### **EPD**

        - `EPD`(ptr) : py_int | EUDVariable  
        Converts the specified pointer [ptr] to an EPD offset. In memory, the player number occupies 4 bytes (32-bit integer). It actually subtracts [ptr] from 0x58A364 and then divides by 4.  
        If the argument is a constant, the result can be returned at compile time.  

        Example

        ```JavaScript
        // https://euddb.website/?pg=entry&id=240
        const epd = EPD(0x5124D8); // (0x5124D8-0x58A364)/4 = -0x1DFA3 = -122787
        ```

    <br />

    - #### **l2v**

        - `l2v`(conditionalExpression) : EUDVariable  
            Uses a trigger at runtime to get the logical value false or true of [conditionalExpression].  

        Example

        ```JavaScript
        var isP1MarineDeaths100 = l2v(Deaths(P1, AtLeast, 100, "Terran Marine"));
        ```

    <br />

    - #### **parse**

        - `parse`(address, radix=10) : py_list[EUDVariable, EUDVariable]  
            Parses a string at memory [address] into a number using [radix] notation.  
            Returns: number, digits  
            Returns 0, 0 on parse failure

        Example

        ```JavaScript
        const numstr = Db("102a\r\r\r\r\r\r\r\r\r\r\r\0");  
        var num, digits;  
        num, digits = parse(numstr, 8);  
        println("0x{:x}, {},  8 radix digits:{}", num, num, digits); // 0x00000042, 66,  8 radix digits:3
        num, digits = parse(numstr, 10);  
        println("0x{:x}, {}, 10 radix digits:{}", num, num, digits); // 0x00000066, 102, 10 radix digits:3
        num, digits = parse(numstr, 16);  
        println("0x{:x}, {}, 16 radix digits:{}", num, num, digits); // 0x0000102A, 4138, 16 radix digits:4
        ```

    <br />

    - #### **EUDFuncPtr**

        - `EUDFuncPtr`(argn, retn) : py_int  
            Declares a pointer to a closure function with [argn] arguments and [retn] return values.  

        - `EUDTypedFuncPtr`(argtypes, rettypes) : py_int  
            Declares a pointer to a closure function with an argument type list of [argtypes] and a return value type list of [rettypes].  

        Example

        ```JavaScript
        function GetMouseMapXY() {
            const screenX, screenY = dwread_epd(EPD(0x62848C)), dwread_epd(EPD(0x6284A8));
            const mouseX, mouseY = dwread_epd(EPD(0x6CDDC4)), dwread_epd(EPD(0x6CDDC8));
            const x, y = screenX + mouseX, screenY + mouseY;
            return x, y;
        }

        var funcptr = 0;

        function beforeTriggerExec() {
            static var x, y = 0, 0;
            once (funcptr == 0) {
                funcptr = EUDFuncPtr(0, 2)(function() {
                    x, y = GetMouseMapXY();
                    return x, y;
                });
            }

            setcurpl(P1);
            printAt(9, "before x = {}, y = {}", x, y);
        }

        function afterTriggerExec() {
            const x, y = EUDFuncPtr(0, 2).cast(funcptr)();
            setcurpl(P1);
            printAt(10, "after funcptr returns {}, {}", x, y);
        }
        ```

    <br />

    - #### **getgametick**

        - `getgametick()` : EUDVariable  
            Gets the number of game frames elapsed. At the `Fastest` game speed, it is 42 milliseconds per frame.  

        Example

        ```JavaScript
        var tick = getgametick();
        ```

    <br />
    <br />

- ### Trigger Construction Functions

    Trigger construction functions can be used to construct triggers with custom properties.

    <br />

    - #### **RawTrigger**

        - `RawTrigger`(conditions = list(...), actions = list(...), preserved = true/false) : RawTrigger  
            Inserts a static classical trigger and cannot pass variables. Returns a trigger pointer.  
            The conditions field passes a list of constant conditional expressions with a maximum of 16 classical trigger conditions.  
            The actions field passes a list of constant action expressions with a maximum of 64 classical trigger actions.  
            The preserved field defaults to true, indicating that it will execute each time it is called. If set to false, it will execute once after the condition is met and will never execute again.  

        Example

        ```JavaScript
        // https://euddb.website/?pg=entry&id=542
        // https://euddb.website/?pg=entry&id=543
        // The principle of reading memory values is to use 32 triggers to compare the value of each bit in 32 bits. If the i-th bit is greater than 0, the variable is appended with 2 to the power of i-1.
        function GetMouseXY() {
            var x, y;
            RawTrigger(actions = list(
                SetDeaths(EPD(x.getValueAddr()), SetTo, 0, 0),
                SetDeaths(EPD(y.getValueAddr()), SetTo, 0, 0),
            ));
            foreach(i : py_range(32)) { /* Use 64 triggers here to read mouse X Y values */
                RawTrigger(
                    conditions = DeathsX(EPD(0x6CDDC4), AtLeast, 1, 0, py_pow(2,i)),
                    actions = SetDeaths(EPD(x.getValueAddr()), Add, py_pow(2,i), 0),
                );
                RawTrigger(
                    conditions = DeathsX(EPD(0x6CDDC8), AtLeast, 1, 0, py_pow(2,i)),
                    actions = SetDeaths(EPD(y.getValueAddr()), Add, py_pow(2,i), 0),
                );
            }
            return x, y;
        }

        // The above GetMouseXY function is actually equivalent to the following
        function GetMouseXY() {
            return dwread(0x6CDDC4), dwread(0x6CDDC8);
        }
        ```

    <br />

    - #### **Trigger**

        - `Trigger`(conditions = list(...), actions = list(...), preserved = true/false)  
            Inserts an extended trigger, which may be split into many classical triggers. It is not limited to 16 conditions and 64 actions, and variables can be passed in, but no return value is returned.  
            Passing variables will not take effect dynamically before the actions are completed, and will only maintain the state of the variables when the trigger starts executing.  
            For clarity of code, it is generally recommended not to use Trigger and instead use if conditions to complete dynamic conditional comparisons.  
            The conditions field passes a list of conditional expressions.  
            The actions field passes a list of action expressions.  
            The preserved field defaults to true, indicating that it will execute each time it is called. If set to false, it will execute once after the condition is met and will never execute again.  

        Example

        ```JavaScript
        Trigger(
            conditions = list(
                Bring(P1, AtMost, 0, "Zerg Cerebrate", "Location 1")
            ),
            actions = list(
                KillUnitAt(All, "Terran Medic", "Location 1", P1)
            ),
            preserved = false,
        );
        ```

    <br />

    - #### **PTrigger**

        - `PTrigger`(players, conditions = list(...), actions = list(...))  
            Inserts a trigger that matches the current player. When the `current player` is any of the players in [players], it will execute. There is no preserved field, and the other fields are used in the same way as Trigger. 

        Example

        ```JavaScript
        setcurpl(P8);
        PTrigger(list(P3, P8),
            conditions = ElapsedTime(AtLeast, 3),
            actions = KillUnit($U("Terran Medic"), P1),
        );
        ```

    <br />

    - #### **DoActions**

        - `DoActions`(actions, preserved = true/false)  
            A Trigger function that executes actions unconditionally.  
            In epScript, any non-comment code between two semicolons is automatically wrapped in this function as a trigger.  
            actions is a list of action expressions.  
            The preserved field defaults to true, indicating that it will execute each time it is called. If set to false, it will execute once after the condition is met and will never execute again.  

        Example

        ```JavaScript
        Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"); // If a line of code is only a trigger action function call, it will be wrapped in a DoActions after compilation.  

        DoActions(Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"));  

        DoActions(list(// To be more explicit, you can add list  
            Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"),  
        ));  

        Trigger(actions = list(  
            Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"),  
        ));

        // The above four usages are completely equivalent  

        const a = Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"); // This line of code is not equivalent to the above usages. It is used to declare an action expression constant named a, which will not be automatically wrapped in DoActions.  

        DoActions(  
            Order("Zerg Guardian", P8, $L("UpArea"), Patrol, "Player1Home"),  
            Order("Zerg Guardian", P8, $L("MiddleArea"), Patrol, "Player1Home"),  
            Order("Zerg Devourer", P8, $L("UpArea"), Patrol, "Player1Home"),  
            Order("Zerg Devourer", P8, $L("MiddleArea"), Patrol, "Player1Home"),  
            preserved = false, // The preserved parameter must be a named parameter  
        );  

        // Static Feature Demo  
        // The variables passed to the actions of DoActions will be replaced with a static constant value when DoActions starts executing. No matter how many times the variable is modified during the execution of DoActions, the passed in value is determined before execution.  
        var c = 0;  
        c.AddNumber(1);  
        DoActions(  
            c.AddNumber(100),  
            CreateUnit(c, "Terran SCV", "Location 2", P1),  
        );  
        printAt(10, "You would expect to create {} SCVs, but only 1 was actually created", c); // You would expect to create 100 SCVs here, but only 1 was actually created
        ```

    <br />

    - #### **VProc**

        - `VProc`(vars, actions) : RawTrigger | [RawTrigger]  
            The action list [actions] can only pass constant action expression lists. VProc will execute all the virtual triggers of the variables in [vars] in order after executing all the actions in [actions] in order.  
            This means that the virtual triggers of some variables can first be modified in [actions], and then the modified virtual triggers of these variables will be executed by VProc after the actions are completed.  
            It is usually used to optimize the overhead of serial assignment or bitwise operations on variables. Its overhead is slightly higher than RawTrigger but lower than DoActions or Trigger.  
            A virtual trigger of a variable can only have one SetDeathsX action. If multiple actions are queued to the variable queue, the last one is taken.  

            [Variable Operation Optimization](../How-epScript-Works.md#variable-operation-optimization)

        Example

        ```JavaScript
        var unrelatedVariable = 0;  
        var c, d = 0, 0;  

        c = 1;  
        RawTrigger(actions = list(  
            c.AddNumber(100),  
            c.QueueAssignTo(d),  
        ));  
        println("After RawTrigger process c:{} d:{}", c, d); // c:101 d:0  

        c = 1;  
        VProc(c, list(  
            c.AddNumber(100),
            c.QueueAssignTo(d),  
        ));  
        println("After VProc(c) process c:{} d:{}", c, d); // c:101 d:101  

        c = 1;  
        VProc(list(c, d), list(  
            c.AddNumber(100),  
            c.QueueAssignTo(d),  
            d.QueueAddTo(c),  
        ));  
        println("After VProc(c,d) process c:{} d:{}", c, d); // c:202 d:101  

        c = 1;  
        VProc(list(d, c), list(  
            c.AddNumber(100),  
            c.QueueAssignTo(d),  
            d.QueueAddTo(c),  
        ));  
        println("After VProc(d,c) process c:{} d:{}", c, d); // c:202 d:202
        ```


    <br />
    <br />

- ### Runtime Iterators

    Runtime iterators can be used with the compile-time loop syntax foreach to construct a runtime loop. The number of times it executes is controlled by the internal flow control logic of the iterator.  
    break and continue can be used in the foreach code block belonging to the runtime iterator, and they will be compiled into EUDContinue() and EUDBreak().  
    Let's explain the principle of runtime iterators in an equivalent code way. For example, there is the following code:  

    ```JavaScript
    foreach (cu : EUDLoopNewCUnit()) {
        py_print(cu);
        println("{}", cu);
        continue;
    }
    ```

    It is roughly equivalent to:  

    ```JavaScript
    {  
        const it = EUDLoopNewCUnit();  
        const cu = py_next(it); // The first compile-time iteration gets the storage location of the iteration result and sets the start of the EUD runtime loop code block, equivalent to an EUDWhile()(the runtime it has a next item)  
            py_print(cu);       // Compile-time output, will only execute once  
            println("{}", cu);       
            EUDContinue();  
        py_next(it, 0);         // The second compile-time iteration sets the end position of the EUD runtime loop code block, equivalent to an EUDEndWhile()  
    }
    ```

    <br />

    - #### **EUDLoopPlayer**

        - `EUDLoopPlayer`(ptype, force, race) : EUDIterator  
            Iterates over active players of type [ptype], force [force] and race [race]. Internally uses playerexist to detect.  
            Active players do not include vacant or leaving players.  

        Example

        ```JavaScript
        // ptype is an optional parameter, can be "Human" or "Computer"  
        // force is an optional parameter, can be Force1 Force2 Force3 Force4  
        // race  is an optional parameter, can be "Zerg" "Terran" "Protoss"
        foreach (p: EUDLoopPlayer("Human", Force1, "Zerg")) {
            setcurpl(p);
            println("You are Zerg");
        }
        ```

    <br />

    - #### **EUDLoopRange**

        - `EUDLoopRange`(start, end=None) : EUDIterator  
            Iterates over values from [start] to [end] - 1.  

        Example

        ```C#
        // Print 1 to 4
        foreach (i : EUDLoopRange(1, 5)) {
            simpleprint(i);
        }
        // The above code is roughly equivalent to
        for (var i = 1; i < 5; i++) {
            simpleprint(i);
        }
        ```

    <br />

    - #### **EUDLoopUnit**

        - `EUDLoopUnit()` : EUDIterator  
            Iterates over the ptr and epd of all units on the main chain. 
            Does not include subunits, Scanner Sweep, Map Revealer, etc.  

        Example

        ```JavaScript
        foreach (ptr, epd : EUDLoopUnit()) {
            const u = CUnit(epd);
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }
        ```

        <br />

        - `EUDLoopUnit2()` : EUDIterator  
            Iterates over the ptr and epd of all units.  
            Does not include subunits, Scanner Sweep, Map Revealer, etc.  

        Example

        ```JavaScript
        // The Unit Node Table is a doubly linked list with a main chain and branch chains.  
        // FirstUnitPointer -> Unit1 <-> Unit2 <-> "Terran Siege Tank" <-> Unit4 -> Null  
        //                                                   |   
        //                                              "Tank Turret"  
        // The main chain and branch chains both occupy memory space. For example, in the above example, Bring will determine that there are a total of 4 units, but in fact 5 of the 1700 unit spaces are occupied.  
        // EUDLoopUnit() will only loop the units on the main chain, so it will ignore "Tank Turret" - the tank turret.  
        // EUDLoopUnit2() will loop all units occupying the Unit Node Table space, because it does not loop in the order of the linked list, but in the order of memory.  
        // EUDLoopUnit(): Loop along the main chain unit, unable to loop to sub unit and map revealer, etc.  
        // EUDLoopUnit2(): Loop along the Memory index to loop all units.  
        foreach (ptr, epd : EUDLoopUnit2()) {  
            const u = CUnit(epd);  
            if (u.unitID == $U("Terran Marine")) {  
                u.hp = 0x100 * 10;  
            }  
        }
        ```

        <br />

        - `EUDLoopCUnit()` : EUDIterator  
            It uses EUDLoopUnit2 to traverse and wraps the traversed pointers into CUnit objects.  
            Does not include subunits, Scanner Sweep, Map Revealer, etc.

        Example

        ```JavaScript
        foreach (u : EUDLoopCUnit()) {
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }
        ``` 

        <br />

        - `EUDLoopNewUnit`(allowance = 2) : EUDIterator  
            Iterates over up to [allowance] ptr and epd of new units that have appeared since the last time `current frame` called EUDLoopNewUnit or EUDLoopNewCUnit.  
            Does not include subunits, Scanner Sweep, Map Revealer, etc.  

        - `EUDLoopNewCUnit`(allowance = 2) : EUDIterator  
            Iterates over up to [allowance] new units that have appeared since the last time `current frame` called EUDLoopNewUnit or EUDLoopNewCUnit and wraps the traversed pointers into CUnit objects.  
            Does not include subunits, Scanner Sweep, Map Revealer, etc.  

        > **Warning**  
        > Calling EUDLoopNewUnit or EUDLoopNewCUnit multiple times in the same frame will traverse the same units.  
        > Zerg peculiarity: New units are only larvae, cancelled Zerg Extractor drones, the second unit of twin units (zergling, scourges, Nydus canal), and any units created out of thin air using the CreateUnit function.   

        > **Note**  
        > The hatching process of Zerg eggs will use the address of the larva. The hatched unit will continue to use this address. If it is hatching a dog, one of the dogs will continue to use the larva's address and the other will be a new unit. Scourges are similar.  
        > Buildings hatched by Zerg drones will use the drone's address. Cancelling the hatch will turn back into a drone with the same address and will not produce a new unit.  
        > For an instant when a drone hatches into a Zerg Extractor, it will inherit the address of the Vespene Geyser unit (the Vespene Geyser itself is a unit). The drone is considered dead. If the construction is cancelled, a new drone is returned. This drone can be traversed.   
        > Units queued in the barracks: they are only detected when they walk out after completion.   
        > Buildings: Can be detected as soon as construction starts (unfinished construction).   
        > Archons and Dark Archons: After Templars merge, the Archon will use the address of one of the Templars. The other Templar is considered dead and will not produce a new unit. If the merge is cancelled, one Templar will inherit the Archon's address and the other will be a new unit. Dark Archons are similar.

        Example

        ```JavaScript
        foreach (ptr, epd : EUDLoopNewUnit(1700)) {
            const u = CUnit(epd);
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }

        foreach (u : EUDLoopNewCUnit(1700)) {
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }

        // The following code is a complete traversal of new units, the code comes from: GGRush
        const NewUnits = UnitGroup(1000);
        const ChangeableUnits = EUDDeque(1000)();

        function onPluginStart() {
            GetGlobalStringBuffer();
        }

        function beforeTriggerExec() {
            foreach(cunit: EUDLoopNewCUnit()) {
                NewUnits.add(cunit);
            }

            ChangeableUnits.append(-1);
            var uid, value;
            while(True) {
                value = ChangeableUnits.popleft();
                if(value == -1) break;
                else if(value < 228) uid = value;
                else if(value >= EPD(0x59CCA8)) {
                    const epd = value;
                    if(!MemoryXEPD(epd + 0x64/4, Exactly, prev, 0xFFFF)) NewUnits.add(value); 
                    else {
                        ChangeableUnits.append(uid);
                        ChangeableUnits.append(epd);
                    }
                }
            }

            foreach(unit: NewUnits.cploop) {
                foreach(dead: unit.dying) {}	// Existence check: living units continue to execute code, dead units continue
                unit.move_cp(0x64/4);
                if(DeathsX(CurrentPlayer, Exactly, $U("Zerg Larva"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Egg"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Drone"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Hydralisk"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Lurker Egg"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Mutalisk"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Mutalisk Cocoon"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Hatchery"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Lair"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Creep Colony"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Spire"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Protoss High Templar"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Protoss Dark Templar (Unit)"), 0, 0xFFFF)
                ) {
                    const uid = wread_cp(0, 0);
                    ChangeableUnits.append(uid);
                    ChangeableUnits.append(unit.epd);
                }
                // start
                // Your codes
                // end
                unit.remove();	//This code is necessary!!!Don't skip it
            }
        }
        ```

        <br />

        - `EUDLoopPlayerUnit`(player: TrgPlayer) : EUDIterator  
            Iterates over all units of player [player] ptr and epd.

        - `EUDLoopPlayerCUnit`(player: TrgPlayer) : EUDIterator  
            Iterates over all units of player [player] and wraps the traversed pointers into CUnit objects.

        Example

        ```JavaScript
        foreach (ptr, epd : EUDLoopPlayerUnit(P1)) {
            const u = CUnit(epd);
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }

        foreach (u : EUDLoopPlayerCUnit(P1)) {
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }

        // Change all Marines of Owner player to NewOwner player
        // Using cunit.give will affect the iteration of EUDLoopPlayerCUnit, so you need to first add all cunits to a queue container
        // and then change the owner of each cunit from the container
        const givequeue = EUDQueue(100);
        foreach(cunit: EUDLoopPlayerCUnit(Owner)) {
            if(cunit.unitType != $U("Terran Marine")) continue;
            givequeue.append(cunit);
        }
        while (!givequeue.empty()) {
            const cunit = CUnit(givequeue.pop());
            cunit.cgive(NewOwner);
        }
        ```

    <br />
    <br />

- ### Display Text Functions

    <br />

    - #### **DisplayTextAt**

        - `DisplayTextAt`(行, 文本 : TrgString)  
            Displays [text] on line [line] of the `Local Player == Current Player` screen  
            It is different from DisplayText and does not return a trigger action expression  

        - `DisplayTextAllAt`(行, 文本 : TrgString)  
            Displays [text] on line [line] for all players (including observers) 
            It is different from DisplayTextAll and does not return a trigger action expression  

        Example

        ```JavaScript
        var text_10 = $T("_10");
        var text_09 = $T("_09");
        var line = 10;
        DisplayTextAllAt(line, text_10);
        line -= 1;
        DisplayTextAllAt(line, text_09);
        line -= 1;
        setcurpl(P1);
        DisplayTextAt(line, "Only displayed to P1");
        ```

    <br />

    - #### **print**

        - `simpleprint`(*args, spaced=true)  
            Prints multiple arguments [*args] in order on the next line of the `Local Player == Current Player` screen scroll information. The named argument spaced indicates whether to separate each printed argument with spaces, the default is true. 

        - `println`(format_string : py_str, *args)  
            Prints multiple arguments [*args] formatted according to [format_string] on the next line of the `Local Player == Current Player` screen scroll information.

        - `printAt`(line, format_string : py_str, *args)  
            Prints multiple arguments [*args] formatted according to [format_string] on line [line] from top to bottom (range 0~10) of the `Local Player == Current Player` screen scroll information.

        - `printAll`(format_string : py_str, *args)  
            Prints multiple arguments [*args] formatted according to [format_string] on the next line of all player screen scroll information.

        - `printAllAt`(line, format_string : py_str, *args)  
            Prints multiple arguments [*args] formatted according to [format_string] on line [line] from top to bottom (range 0~10) of all player screen scroll information.

        <details><summary>Formatting placeholders</summary>

        - `{}`: Generic placeholder used to output the value or constant pointer of a variable  
        - `{{}}`: Outputs `{}` itself  
        - `{:c}`: Outputs the value at the position as the player ID in the corresponding player color code, like PColor(the value at the position)  
        - `{:n}`: Outputs the value at the position as the player ID in the corresponding player name, like PName(the value at the position)  
        - `{:s}`: Outputs the value at the position as a string pointer to the string it points to, like ptr2s(the value at the position)  
        - `{:t}`: Outputs the value at the position as an EPD string pointer to the string it points to, like epd2s(the value at the position)  
        - `{:x}`: Outputs the numeric value at the position as an 8-digit hexadecimal number left-padded with 0's, like hptr(the numeric value at the position)  
        </details>

        Example

        ```JavaScript
        // simpleprint(*args, spaced=true)  
        simpleprint("Hello", "Starcraft");                  // Prints "Hello Starcraft" on the next line of the current player's screen scroll information.  
        simpleprint("Hello", "Starcraft", spaced = false);  // Prints "HelloStarcraft" on the next line of the current player's screen scroll information.  
        
        // println(format_string, *args)  
        println("{} {}", "Hello", "Starcraft");             // Prints "Hello Starcraft" on the next line of the current player's screen scroll information.  
        
        // printAt(line, format_string, *args)  
        printAt( 0, "{} {}", "Hello", "Starcraft");         // Prints "Hello Starcraft" on the top line of the current player's screen scroll information.  
        printAt(10, "{} {}", "Hello", "Starcraft");         // Prints "Hello Starcraft" on the bottom line of the current player's screen scroll information.  
        
        // printAll(format_string, *args)  
        printAll("{} {}", "Hello", "Starcraft");            // Prints "Hello Starcraft" on the next line of all players' screen scroll information.  
        
        // printAllAt(line, format_string, *args)  
        printAllAt( 0, "{} {}", "Hello", "Starcraft");      // Prints "Hello Starcraft" on the top line of all players' screen scroll information.  
        printAllAt(10, "{} {}", "Hello", "Starcraft");      // Prints "Hello Starcraft" on the bottom line of all players' screen scroll information.
        ```

    <br />

    - #### **GetGlobalStringBuffer**

        - `GetGlobalStringBuffer()` : StringBuffer  
            Gets the StringBuffer used internally by the print series functions of `Local Player == Current Player`. Its capacity is 1023 bytes.

        Example

        ```JavaScript
        // The following two lines of code are equivalent  
        printAt( 0, "{} {}", "Hello", "Starcraft");  
        GetGlobalStringBuffer().printfAt(0, "{} {}", "Hello", "Starcraft");
        ```

    <br />

    - #### **eprint**

        - `eprintln`(*args)  
            Prints multiple arguments [*args] in order to the error message below the center of the `current player`'s screen. Printing more than 218 bytes of content (including color code string endings, etc.) will cause an error. 

        - `eprintf`(format_string, *args)  
            Prints multiple arguments [*args] formatted according to the literal string [format_string] to the error message below the center of the `current player`'s screen. Printing more than 218 bytes of content (including color code string endings, etc.) will cause an error.

        - `eprintAll`(format_string, *args)  
            Prints multiple arguments [*args] formatted according to the literal string [format_string] to the error message below the center of all players' screens. Printing more than 218 bytes of content (including color code string endings, etc.) will cause an error. 

        - `eprintln2`(*args)  
            Prints multiple arguments [*args] in order to the error message below the center of the `current player`'s screen.   
            Replaces stat_txt.tbl[871]: "Unit's waypoint list is full." This error message, then coordinates with QueueGameCommand_QueuedRightClick(xy) can output more than 218 bytes of content in the error message.

        <details><summary>Formatting placeholders</summary>

        - `{}`: Generic placeholder used to output the value or constant pointer of a variable  
        - `{{}}`: Outputs `{}` itself  
        - `{:c}`: Outputs the value at the position as the player ID in the corresponding player color code, like PColor(the value at the position)  
        - `{:n}`: Outputs the value at the position as the player ID in the corresponding player name, like PName(the value at the position)  
        - `{:s}`: Outputs the value at the position as a string pointer to the string it points to, like ptr2s(the value at the position)  
        - `{:t}`: Outputs the value at the position as an EPD string pointer to the string it points to, like epd2s(the value at the position)  
        - `{:x}`: Outputs the numeric value at the position as an 8-digit hexadecimal number left-padded with 0's, like hptr(the numeric value at the position)  
        </details>

        Example

        ```JavaScript
        eprintln("Hello", "Starcraft");                  // Prints "HelloStarcraft" to the error message below the center of the current player's screen.  
        eprintf("{}-{}", "Hello", "Starcraft");          // Prints "Hello-Starcraft" to the error message below the center of the current player's screen.  
        eprintAll("{}-{}", "Hello", "Starcraft");        // Prints "Hello-Starcraft" to the error message below the center of all players' screens. 
        ```

    <br />

    - #### **TextFX**

        - TextFX_FadeIn(*args, color=None, wait=1, reset=True, tag=None, encoding="UTF-8")  
            Fade in text effect

        - TextFX_FadeOut(*args, color=None, wait=1, reset=True, tag=None, encoding="UTF-8")  
            Fade out text effect

        - `TextFX_Remove`(tag)  
            Removes the text effect with the local tag [tag]

        - `TextFX_SetTimer`(tag, SetTo/Add/Subtract : TrgModifier, value)  
            Sets the timer of the text effect with the local tag [tag] to [SetTo/Add/Subtract] [value]


    <br />
    <br />

- ### Players Functions

    <br />

    - #### **getuserplayerid**

        - `getuserplayerid()` : EUDVariable  
            Gets the local player ID. The local player is not the `current player`. The local player ID obtained on each player's computer is different. It returns `desync-data`

        Example

        ```JavaScript
        setcurpl(getuserplayerid());
        println("The ID of the current player: {}", getuserplayerid());
        ```

    <br />

    - #### **playerexist**

        - `playerexist`(player) : EUDVariable  
            Checks if player [player] still exists in the game. Computer players are also players.

        Example

        ```JavaScript
        if (playerexist(P1)) {
            // Player 1 exists
        }
        ```

    <br />

    - #### **Current Player**

        - `getcurpl()` : EUDVariable  
            Gets the value of cpcache. If cpcache has no value or cpcache's value is different from the `current player`, the value of the `current player` is cached in cpcache and returned. 

        - `setcurpl`(cp)  
            Sets the value of the `current player` to [cp] and caches it in cpcache.

        - `addcurpl`(n)  
            Increments the value of the `current player` by [n] and caches it in cpcache.

        - `setcurpl2cpcache()`  
            Restores the value of the `current player` to the cached value in cpcache.   

        The `current player` can be thought of as a global variable. Specifying the player ID as CurrentPlayer(13) in trigger conditions or actions that require a specific player ID will use it. Some trigger actions internally use it. The `current player` may not even refer to a player and may store any value.

        <details><summary>Actions that only take effect on the machine where current player == local player (allow desync use, can be used separately on some player machines)</summary>

        - DisplayText  
        - CenterView  
        - PlayWAV  
        - MinimapPing  
        - TalkingPortrait  
        - Transmission  
        - SetMissionObjectives  
        </details>

        <details><summary>Actions that use current player as a parameter (must be synchronized on all player machines, otherwise disconnected)</summary>

        - SetAllianceStatus  
        - RunAIScript  
        - RunAIScriptAt  
        - Draw  
        - Defeat  
        - Victory  
        </details>

        Example

        ```JavaScript
        const cp = getcurpl();  
        setcurpl(P1);  
        println("Hello Player 1");  
        setcurpl(cp);  

        // CurrentPlayer is the constant number 13, which can cause some player-related conditions or actions to access the value of the current player
        // CurrentPlayer != getcurpl()

        // https://euddb.website/?pg=entry&id=426
        setcurpl(-122787); // PlayerID offset for game speed level 1
        addcurpl(6); // Game speed level 7
        SetDeaths(CurrentPlayer, SetTo, 21, 0); // Game speed x2

        // https://euddb.website/?pg=entry&id=815
        // https://euddb.website/?pg=entry&id=920
        SetMemory(0x6509B0, SetTo, 210382); // Change the current player to 210382 (game brightness level 0~31)
        SetDeaths(CurrentPlayer, SetTo, 15, 0); // Set brightness to 15
        setcurpl2cpcache(); // Restore the current player to cpcache to prevent interference with getcurpl and other functions
        ```

    <br />

    - #### **PColor**

        - `PColor`(player: TrgPlayer) : Db*  
            Returns player [player]'s color code in the game. Using the formatting placeholder `{:c}` in formatted text is equivalent. 

    <br />

    - #### **PName**

        - `PName`(player: TrgPlayer) : Db*  
            Returns player [player]'s name in the game. Using the formatting placeholder `{:n}` in formatted text is equivalent. 

        Example

        ```JavaScript
        println("玩家1: {}{}", PColor(P1), PName(P1)); // If Player 1 is named Soze and the color is red, this output will print the red Soze
        println("玩家1: {:c}{:n}", P1, P1);            // Equivalent to the above
        ```

    <br />

    - #### **SetPName**

        - `SetPName`(player : TrgPlayer, *args)  
            Sets [player]'s name to the text composed of multiple arguments [*args].  

        - `SetPNamef`(player: TrgPlayer, format_string, *args)  
            Sets [player]'s name to the text formatted by multiple arguments [*args] using [format_string].  

        这俩函数均不能影响 PName 函数获取到的那个名字，只影响玩家聊天显示的那个名字，并且只对当前帧有效，每一帧都需要重复运行

        Example

        ```JavaScript
        // The following two usages are equivalent
        SetPName(cp, epd2s(title), " \x07level: \x04", level, " ", PColor(cp), PName(cp));
        SetPNamef(cp, "{:t} \x07level: \x04{} {:c}{:n}", title, level, cp, cp);
        ```

    <br />

    - #### **EUDPlayerLoop**

        - `EUDPlayerLoop()()`  
        - `EUDEndPlayerLoop()`  
            These two are a pair. It will sequentially set the `current player` to each active player (including computer players). After completion, the value of the current player will be the value before the Loop started.  

        Example

        ```JavaScript
        // Give all players 1000 ore minerals, including computer players
        EUDPlayerLoop()();
            SetResources(CurrentPlayer, Add, 1000, Ore);
        EUDEndPlayerLoop();
        ```

    <br />
    <br />

- ### Location Functions

    <br />

    - #### **setloc**

        - `setloc`(loc : TrgLocation, x, y)  
            Sets the upper left and lower right coordinates of location [loc] to [x], [y], [x], [y] respectively (that is, set the location to a point).  

        - `setloc`(loc : TrgLocation, left, top, right, bottom)  
            Sets the upper left and lower right coordinates of location [loc] to [left], [top], [right], [bottom] respectively.  

        Example

        ```JavaScript
        setloc($L("Location 1"), 1234, 2345);
        setloc($L("Location 1"), 1234, 1234, 2345, 2345);
        ```

    <br />

    - #### **addloc**

        - `addloc`(loc : TrgLocation, x, y)  
            Sets the left and right coordinates of location [loc] to add [x] and the up and down coordinates to add [y] (the actual size remains unchanged, the center moves to another position).  

        - `addloc`(loc : TrgLocation, left, top, right, bottom)  
            Sets the upper left, upper right, lower left and lower right of location [loc] to add [left], [top], [right], [bottom] respectively.  

        Example

        ```JavaScript
        addloc($L("Location 1"), 123, 234);
        addloc($L("Location 1"), 123, 234, 345, 456);
        // Used with lengthdir  
        addloc($L("Location 1"), lengthdir_256(888, 73)); // Move Location 1 area 888 coordinates in the 73 degree (256 degree system) direction  
        addloc($L("Location 1"), lengthdir(888, 102)); // Move Location 1 area 888 coordinates in the 102 degree direction
        ```

    <br />

    - #### **dilateloc**

        - `dilateloc`(loc : TrgLocation, x, y)  
            Sets the upper left, upper right, lower left and lower right of location [loc] to add -[x], -[y], [x], [y] respectively (the center remains unchanged, the area expands).  

        - `dilateloc`(loc : TrgLocation, left, top, right, bottom)  
            Sets the upper left, upper right, lower left and lower right of location [loc] to add -[left], -[top], [right], [bottom] respectively.  

        Example

        ```JavaScript
        dilateloc($L("Location 1"), 5, 5);
        dilateloc($L("Location 1"), 1, 2, 3, 4);
        ```

    <br />

    - #### **getlocTL**

        - `getlocTL`(loc : TrgLocation) : py_tuple[EUDVariable, EUDVariable]  
            Gets the upper left coordinate of a location.  

        Example

        ```JavaScript
        const top, left = getlocTL($L("Location 1"));
        ```

    <br />

    - #### **setloc_epd**

        - `setloc_epd`(loc : TrgLocation, epd)  
            Sets the coordinates of location [loc] to the value stored at local memory address `0x58A364 + [epd] * 4`.  

        Example

        ```JavaScript
        // It is same as the following function
        function setloc_epd(loc : TrgLocation, epd) {
            const x, y = posread_epd(epd);
            setloc(loc, x, y);
        }
        ```

    <br />
    <br />

- ### Memory Operation Functions

    <br />

    - #### **dwbreak**

        - `dwbreak`(number) : py_tuple[EUDVariable, EUDVariable, EUDVariable, EUDVariable, EUDVariable, EUDVariable]  
            Splits a dword value [number] into word and byte forms.  

        Example

        ```JavaScript
        const w1, w2, b1, b2, b3, b4 = dwbreak(1234 + 0x10000 * 5678)[[0,1,2,3,4,5]];
        println("w1:{} w2:{} b1:{} b2:{} b3:{} b4:{}", w1, w2, b1, b2, b3, b4);
        ```

    <br />

    - #### **read/write**

        - `dwread`(ptr) : EUDVariable  
        - `wread`(ptr) : EUDVariable  
        - `bread`(ptr) : EUDVariable  

            Reads the dword/word/byte value at the specified local memory address [ptr].    

        - `dwwrite`(ptr, dw)  
        - `wwrite`(ptr, w)  
        - `bwrite`(ptr, b)  

            Writes dword/word/byte values to local memory address [ptr].  

        Example

        ```JavaScript
        // 0x582144: https://euddb.website/?pg=entry&id=490
        const SUP_RACE_ZERG = 0;
        const SUP_RACE_TERRAN = 1;
        const SUP_RACE_PROTOSS = 2;
        const SUP_TYPE_AVAILABLE = 0;
        const SUP_TYPE_USED = 1;
        const SUP_TYPE_MAX = 2;

        function SetPlayerSupply(player: TrgPlayer, race, type, amount) {
            dwwrite(0x582144 + (race) * 36 * 4 + (type) * 12 * 4 + (player) * 4, amount);
        }
        function GetPlayerSupply(player: TrgPlayer, race, type) {
            return dwread(0x582144 + (race) * 36 * 4 + (type) * 12 * 4 + (player) * 4);
        }

        SetPlayerSupply(P1, SUP_RACE_ZERG, SUP_TYPE_MAX, 800); // Set player 1's Zerg supply maximum to 400
        ```

    <br />

    - #### **read_epd/write_epd**

        - `dwread_epd`(epd) : EUDVariable  
            Reads the dword value at local memory address `0x58A364 + [epd] * 4`.  

        - `dwwrite_epd`(epd, value)  
            Writes a dword value to local memory address `0x58A364 + [epd] * 4`.  

        - `wread_epd`(epd, subp) : EUDVariable  
        - `bread_epd`(epd, subp) : EUDVariable  

            Reads the word/byte value at local memory address `0x58A364 + [epd] * 4 + [subp]`, `[subp] < 4`.  

        - `wwrite_epd`(epd, subp, value)  
        - `bwrite_epd`(epd, subp, value)  

            Writes a word/byte value to local memory address `0x58A364 + [epd] * 4 + [subp]`, `[subp] < 4`.  

        - `maskread_epd`(epd, mask) : EUDVariable  
            Uses [mask] as a mask to read the dword value at local memory address `0x58A364 + [epd] * 4`.  

        Example

        ```JavaScript
        // Similar to the example of read/write, but the memory offset benchmarks of _epd series are different. EPD macros can be used for conversion.  
        function SetPlayerSupply(player: TrgPlayer, race, type, amount) {
            dwwrite_epd(EPD(0x582144) + (race) * 36 + (type) * 12 + (player), amount);
        }
        function GetPlayerSupply(player: TrgPlayer, race, type) {
            return dwread_epd(EPD(0x582144) + (race) * 36 + (type) * 12 + (player));
        }
        const oe, os = div(EncodeWeapon("C-10 Concussion Rifle"), 4);
        println("bread_epd Ghost weapon interval {}", bread_epd(EPD(0x656FB8) + oe, os));
        println("maskread_epd Ghost weapon interval {}", bitrshift(maskread_epd(EPD(0x656FB8) + 1 + oe, 0xFF000000), 24));
        bwrite_epd(EPD(0x656FB8) + oe, os, 1); // Modify Ghost weapon attack interval to 1
        ```

    <br />

    - #### **add_epd/subtract_epd**

        - `dwadd_epd`(epd, value)  
            Increments the dword value at local memory address `0x58A364 + [epd] * 4` by [value].  

        - `dwsubtract_epd`(epd, value)  
            Decrements the dword value at local memory address `0x58A364 + [epd] * 4` by [value].  

        - `wadd_epd`(epd, subp, value)  
        - `badd_epd`(epd, subp, value)  

            Increments the word/byte value at local memory address `0x58A364 + [epd] * 4 + [subp]` by [value], `[subp] < 4`.  

        - `wsubtract_epd`(epd, subp, value)  
        - `bsubtract_epd`(epd, subp, value)  

            Decrements the word/byte value at local memory address `0x58A364 + [epd] * 4 + [subp]` by [value], `[subp] < 4`.  

    <br />

    - #### **repmovsd_epd**

        - `repmovsd_epd`(dstepdp, srcepdp, copydwn)  
            Copies `[copydwn] * 4` bytes of content from local memory address `0x58A364 + [srcepdp] * 4` to memory address `0x58A364 + [dstepdp] * 4`.   

        Example

        ```JavaScript
        const src = Db(b"___1___2___3___4___5");
        const dst = Db(20);
        repmovsd_epd(EPD(src), EPD(dst), 5);
        // dst will be Db(b"___1___2___3___4___5")
        ```

    <br />

    - #### **dwepdread_epd**

        - `dwepdread_epd`(epd) : py_tuple[EUDVariable, EUDVariable]  
            Reads a pointer from local memory address `0x58A364 + [epd] * 4` and returns the pointer and its EPD value.  

        - `epdread_epd`(epd) : EUDVariable  
            Reads a pointer from local memory address `0x58A364 + [epd] * 4` and returns its EPD value.  

        Example

        ```JavaScript
        // Create a Marine and get its pointer and EPD value
        CreateUnit(1, "Terran Marine", $L("Location 1"), P1);
        const lastUnitEPD = EPD(0x628438);
        const ptr1, epd1 = dwepdread_epd(lastUnitEPD);
        var epd2 = epdread_epd(lastUnitEPD);
        ```

    <br />

    - #### **cunitread_epd**

        - `cunitread_epd`(epd) : EUDVariable  
            Reads a cunit pointer from local memory address `0x58A364 + [epd] * 4`. This function is optimized for reading cunit pointers and returns a pointer.  

        - `cunitepdread_epd`(epd) : py_tuple[EUDVariable, EUDVariable]  
            Reads a cunit pointer from local memory address `0x58A364 + [epd] * 4`. This function is optimized for reading cunit pointers and returns the pointer and its EPD value.  

        Example

        ```JavaScript
        // Create a Marine and get its pointer and EPD value
        CreateUnit(1, "Terran Marine", $L("Location 1"), P1);
        const lastUnitEPD = EPD(0x628438);
        const ptr1, epd1 = cunitepdread_epd(lastUnitEPD);
        var ptr2 = cunitread_epd(lastUnitEPD);
        ```

    <br />

    - #### **posread_epd**

        - `posread_epd`(epd) : py_tuple[EUDVariable, EUDVariable]  
            Reads a pos (location) from local memory address `0x58A364 + [epd] * 4`.  

        Example

        ```JavaScript
        const screenTilePosEPD = EPD(0x57F1D0);
        const x, y = posread_epd(screenTilePosEPD);
        println("The current screen coordinates on the map: ({}, {})", x, y);
        ```

    <br />

    - #### **_cp Series**

        - `dwread_cp`(cpoffset) : EUDVariable  
        - `dwwrite_cp`(cpoffset, value)  
        - `dwadd_cp`(cpoffset, value)  
        - `dwsubtract_cp`(cpoffset, value)  
        - `wread_cp`(cpoffset, subp) : EUDVariable  
        - `bread_cp`(cpoffset, subp) : EUDVariable  
        - `wwrite_cp`(cpoffset, subp, w)  
        - `bwrite_cp`(cpoffset, subp, b)  
        - `maskread_cp`(cpoffset, mask) : EUDVariable  
        - `maskwrite_cp`(cpoffset, mask, value)  
        - `dwepdread_cp`(cpoffset) : py_tuple[EUDVariable, EUDVariable]  
        - `epdread_cp`(cpoffset) : EUDVariable  
        - `cunitread_cp`(cpoffset) : EUDVariable  
        - `cunitepdread_cp`(cpoffset) : py_tuple[EUDVariable, EUDVariable]  
        - `posread_cp`(cpoffset) : py_tuple[EUDVariable, EUDVariable]  
        
        The usage of all functions in the _cp series can refer to the _epd series. The _cp series will use `Current Player + [cpoffset]` as epd.  
        They are usually used to improve code running efficiency and reduce the final number of triggers generated.  

        Example

        ```JavaScript
        // The following code is just to demonstrate the usage of _cp, not to improve efficiency. To improve efficiency, you may need to think for yourself.
        const screenTilePosEPD = EPD(0x57F1D0);

        setcurpl(screenTilePosEPD); // Set Current Player to screenTilePosEPD
        const x, y = posread_cp(0); // Read the value at the relative offset 0 bytes from Current Player, which actually reads the value at screenTilePosEPD

        setcurpl(P1); // Set Current Player to P1 to output information to Player 1
        println("Current screen coordinates on the map: ({}, {})", x, y);
        ```

    <br />

    - #### **readgen**

        - `readgen_epd`(mask, args) : duck  
        - `readgen_cp`(mask, args) : duck  

            Can be used to create custom local memory read functions.  

        Example

        ```JavaScript
        // 256 grids = 8192 pixels = x and y are within 0 ~ 8191 (0x1FFF)
        // Compile-time functions can only be defined using py_eval
        const posread_epd = readgen_epd(
            0x1FFF1FFF,
            list(0, py_eval('lambda x: x if x < 65536 else 0')),
            list(0, py_eval('lambda y: y // 65536 if y >= 65536 else 0')),
        );
        const x, y = posread_epd(epd_address);
        ```

    <br />

    - #### **memcpy**

        - `memcpy`(dst, src, copylen)  
            Copies [copylen] bytes of content from local memory address [src] to memory address [dst].  

    <br />

    - #### **memcmp**

        - `memcmp`(buf1, buf2, count) : EUDVariable  
            Compares [count] bytes of content between local [buf1] and [buf2] memory blocks.  
            If the two memory blocks are exactly the same, returns 0.  
            Otherwise, compares the first different byte and returns a result greater than or less than 0.  

    <br />

    - #### **strcpy**

        - `strcpy`(dst, src)  
            Copies the string (terminated by `\x00`) from local memory address [src] to memory address [dst].  

    <br />

    - #### **strcmp**

        - `strcmp`(s1, s2) : EUDVariable  
            Compares the strings (terminated by \x00) between local [s1] and [s2] memory blocks.  
            If the two memory blocks are exactly the same, returns 0.  
            Otherwise, compares the first different byte and returns a result greater than or less than 0.  

    <br />

    - #### **strlen**

        - `strlen`(ptr) : EUDVariable  
            Gets the number of ASCII characters in the string (terminated by \x00) pointed to by the local pointer [ptr].  

        - `strlen_epd`(epd) : EUDVariable  
            Gets the number of ASCII characters in the string (terminated by \x00) pointed to by the local [epd] offset pointer.  

    <br />

    - #### **strnstr**

        - `strnstr`(ptr, substr, count) : EUDVariable  
            Searches for another string [substr] within the first [count] ASCII characters of the string pointed to by the local pointer [ptr].  
            Returns the pointer if found, otherwise returns 0.  

    <br />

    - #### **dbstr**

        - `dbstr_addstr`(dst, src) : EUDVariable  
            Copies the local string [src] to memory address [dst], returns address [dst] + strlen([src]).  

        - `dbstr_addstr_epd`(dst, srcepd) : EUDVariable  
            Copies the string at local memory address `0x58A364 + [srcepd] * 4` to memory address [dst], returns address [dst] + strlen_epd([srcepd]).  

        - `dbstr_adddw`(dst, number) : EUDVariable  
            Converts a numeric value to text output at local memory address [dst], returns address [dst] + strlen(itoa([number])).  

        - `dbstr_addptr`(dst, ptr) : EUDVariable  
            Converts a numeric value to hexadecimal digit text output at local memory address [dst], returns address [dst] + strlen(itox([number])).  

        - `dbstr_print`(dst, *args, EOS=true, encoding="UTF-8")  
            Combines multiple parameters [*args] into a string output at local memory address [dst].  
            Named parameter [EOS] specifies whether to append a string termination symbol at the end of the string, default true.  
            Named parameter [encoding] specifies the encoding, default UTF-8.  

        - `sprintf`(dst, format_string : py_str, *args, EOS=true, encoding="UTF-8")  
            Formats multiple parameters [*args] according to [format_string] and outputs them to local memory address [dst].  
            Named parameter [EOS] specifies whether to append a string termination symbol at the end of the string, default true.  
            Named parameter [encoding] specifies the encoding, default UTF-8.  

        Example

        ```JavaScript
        const s = Db(100);
        var addr = unProxy(s);
        addr = dbstr_addstr(addr, Db("0123"));
        addr = dbstr_adddw(addr, 4567);
        addr = dbstr_addptr(addr, 0x89ABCDEF);
        simpleprint(s); // 0123456789ABCDEF

        dbstr_print(s, "0123", 4567, hptr(0x89ABCDEF));
        simpleprint(s); // 0123456789ABCDEF

        sprintf(s, "{}{}{:x}", "0123", 4567, 0x89ABCDEF);
        simpleprint(s); // 0123456789ABCDEF
        ```

    <br />

    - #### **ptr2s/epd2s**

        - `ptr2s`(ptr) : Db*  
            Reads the string at local memory address [ptr], equivalent to using `{:s}` placeholder in formatted text.  

        - `epd2s`(epd) : Db*  
            Reads the string at local memory address `0x58A364 + [srcepd] * 4`, equivalent to using `{:t}` placeholder in formatted text.  

    <br />

    - #### **hptr**

        - `hptr`(value) : Db*  
            Converts [value] to hexadecimal output, equivalent to using `{:x}` placeholder in formatted text.  

        Example

        ```JavaScript
        println("{}, {}", 0xAABBCC, hptr(0xAABBCC)); // 11189196, 00AABBCC
        println("{}, {:x}", 0xAABBCC, 0xAABBCC);     // 11189196, 00AABBCC
        ```

    <br />

    - #### **gettextptr**

        - `gettextptr()` : EUDVariable  
            Gets the local screen text pointer for the next line displayed on the screen.  

    <br />

    - #### **dwpatch_epd**

        - `dwpatch_epd`(dstepd, value)  
            Patches the local memory address `0x58A364 + [dstepd] * 4` by [value].  

    <br />

    - #### **GetMapStringAddr**

        - `GetMapStringAddr`(strID : TrgString) : EUDVariable  
            Gets the memory address of a local map string or string ID [strID].  

        Example

        ```JavaScript
        // It supports using strings and IDs to get, the following usages are equivalent
        const addr = GetMapStringAddr(6);
        const addr = GetMapStringAddr("Force 3");
        ```

    <br />

    - #### **GetTBLAddr**

        - `GetTBLAddr`(TBLKey : StatText) : EUDVariable  
            Gets the memory address of a TBL table Key/ID [TBLKey].  

            > It is worth mentioning that the TBLKey string itself may not actually exist in memory.  
            > For example, there is no "Terran Siege Tank (Tank Mode)" string in memory.  
            > The string corresponding to the TBLKey "Terran Siege Tank (Tank Mode)" (located in the stat_txt.tbl string section) prints out as "Terran Siege Tank"  

        Example

        ```JavaScript
        // It supports using TBLKey or TBL ID to get, the following usages are equivalent
        const addr = GetTBLAddr(4);
        const addr = GetTBLAddr("Terran Goliath");
        const addr = wread(dwread_epd(EPD(0x6D1238)) + $B("Terran Goliath"));
        ```

    <br />

    - #### **settbl**

        - `settbl`(tblID : StatText, offset, *args, encoding="cp949")  
        - `settbl2`(tblID : StatText, offset, *args, encoding="cp949")  

            Sets the local memory string value of the specified [tblID] in the TBL table at offset [offset] to *args. The difference between settbl and settbl2 is that settbl will add an EOS character at the end of the set string, while settbl2 will not.

        - `settblf`(tblID : StatText, offset, format_string, *args, encoding="cp949")  
        - `settblf2`(tblID : StatText, offset, format_string, *args, encoding="cp949")  

            Sets the local memory string value of the specified [tblID] in the TBL table at offset [offset] to a formatted string. The difference between settblf and settblf2 is that settblf will add an EOS character at the end of the set string, while settblf2 will not.

      ```JavaScript
        // The following code is equivalent
        settbl("Terran Goliath", 1, "1234");
        settbl2("Terran Goliath", 1, "1234\0");
        dbstr_print(GetTBLAddr("Terran Goliath") + 1, "1234\0", EOS = false); // Rename Terran Goliath to T1234
        ```

    <br />
    <br />

- ### Math Functions

    <br />

    - #### **atan2**

        - `atan2`(y, x) : EUDVariable  
            Arctangent function of two arguments, returns the polar angle of the point (x, y), i.e. the angle with the x-axis.  

        - `atan2_256`(x, y) : EUDVariable  
            The difference from atan2 is that in processing angles, it divides a circumference into 256 equal parts, and the angle is in 256 degrees, not 360 degrees.  

        > StarCraft units use angles stored in one byte, so they use the 256 degree system.  
        > 0 degrees faces up, 0 to 256 increments clockwise.  
        > 64 degrees faces right, 128 degrees faces down, 192 degrees faces left.  

        > **Warning**  
        > In euddraft version 0.9.9.7 and earlier, atan2_256 uses the mathematical coordinate system.  
        > In euddraft version 0.9.9.8 and above, atan2_256 is changed to use the StarCraft coordinate system.  

        Example

        ```JavaScript
        function _0998_above() {
            static var is0998above = false;
            once is0998above = l2v(atan2_256(10, 10) >= 90);
            return is0998above;
        }

        function angleBetween_256(x1, y1, x2, y2) {
            if (_0998_above()) {
                return atan2_256(y2 - y1, x2 - x1);
            }
            return atan2_256(x2 - x1, y1 - y2);
        }

        println("The angle from (131, 33) to (765, 546) is {}", angleBetween_256(131, 33, 765, 546));
        ```

    <br />

    - #### **sqrt**

        - `sqrt`(x) : py_int | EUDVariable  
            Calculates the square root of [x].  

        Example

        ```JavaScript
        function distanceBetween(x1, y1, x2, y2) {
            const x = x2 - x1;
            const y = y2 - y1;
            return sqrt(x*x + y*y);
        }

        println("The distance from (131, 33) to (765, 546) is {}", distanceBetween(131, 33, 765, 546));
        ```

    <br />

    - #### **lengthdir**

        - `lengthdir`(length, angle) : tuple[EUDVariable, EUDVariable]  
            Calculates the coordinates of another point by traveling [length] distance from (0, 0) in the direction of [angle] degrees.  

        - `lengthdir_256`(length, angle) : tuple[EUDVariable, EUDVariable]  
            The difference from lengthdir is that in processing angles, it divides a circumference into 256 equal parts, and the angle is in 256 degrees, not 360 degrees.  

        > StarCraft units use angles stored in one byte, so they use the 256 degree system.  
        > 0 degrees faces up, 0 to 256 increments clockwise.  
        > 64 degrees faces right, 128 degrees faces down, 192 degrees faces left.  

        > **Warning**  
        > In euddraft version 0.9.9.7 and earlier, lengthdir_256 uses the mathematical coordinate system.  
        > In euddraft version 0.9.9.8 and above, lengthdir_256 is changed to use the StarCraft coordinate system.  

        Example

        ```JavaScript
        function _0998_above() {
            static var is0998above = false;
            once is0998above = l2v(atan2_256(10, 10) >= 90);
            return is0998above;
        }

        function polarProjection_256(x, y, length, angle256) {
            var ox, oy;
            if (_0998_above()) {
                ox, oy = lengthdir_256(length, angle256);
            } else {
                ox, oy = lengthdir_256(length, 320 - angle256);
            }
            return x + ox, y - oy;
        }

        const x, y = polarProjection_256(1264, 880, 888, 73);

        println("Traveling 888 distance from (1264, 880) in the direction of 73 degrees (256 degrees) arrives at ({}, {})", x, y);
        ```

        [Example: UsePosition/main.eps](../res/UsePosition/main.eps)

    <br />

    - #### **pow**

        - `pow`(x, y) : py_int | EUDVariable  
            Calculates [x] to the power of [y]. If both arguments are compile-time constants, it can return a constant at compile time.  

        Example

        ```JavaScript
        println("2^10 = {}", pow(2, 10));
        ```

    <br />

    - #### **div**

        - `div`(a, b) : py_tuple[EUDVariable, EUDVariable]  
            Unsigned integer division [a] divided by [b], supports only positive integers, returns quotient and remainder.  

        - `div_towards_zero`(a, b) : py_tuple[EUDVariable, EUDVariable]  
            Added in euddraft 0.9.9.8. Signed integer division, calculates the quotient and remainder of (a ÷ b), rounding the quotient towards zero.  

        - `div_floor`(a, b) : py_tuple[EUDVariable, EUDVariable]  
            Added in euddraft 0.9.9.8. Signed integer division, calculates the quotient and remainder of (a ÷ b), rounding the quotient towards negative infinity.

        - `div_euclid`(a, b) : py_tuple[EUDVariable, EUDVariable]  
            Added in euddraft 0.9.9.8. Signed integer division, calculates the quotient and remainder of Euclidean division of a by b.  
            This computes the quotient such that `a = quotient * b + remainder`, and `0 <= remainder < abs(b)`.  
            In other words, the result is a ÷ b rounded to the quotient such that `a >= quotient * b`.  
            If `a > 0`, this is equal to round towards zero; if `a < 0`, this is equal to round towards +/- infinity (away from zero).  

        Example

        ```JavaScript
        var a, b, quotient, remainder;
        a, b = 17, 3;
        quotient, remainder = div(a, b);
        println("div(17, 3) returns {}, {}", quotient, remainder);                  // div(17, 3) returns 5, 2
        a, b = 17, -3;
        quotient, remainder = div_towards_zero(a, b);
        println("div_towards_zero(17, -3) returns -{}, {}", -quotient, remainder);  // div_towards_zero(17, -3) returns -5, 2
        quotient, remainder = div_floor(a, b);
        println("div_floor(17, -3) returns -{}, -{}", -quotient, -remainder);       // div_floor(17, -3) returns -6, -1
        quotient, remainder = div_euclid(a, b);
        println("div_euclid(17, -3) returns -{}, {}", -quotient, remainder);        // div_euclid(17, -3) returns -5, 2
        a, b = -17, -3;
        quotient, remainder = div_towards_zero(a, b);
        println("div_towards_zero(-17, -3) returns {}, -{}", quotient, -remainder); // div_towards_zero(-17, -3) returns 5, -2
        quotient, remainder = div_floor(a, b);
        println("div_floor(-17, -3) returns {}, -{}", quotient, -remainder);        // div_floor(-17, -3) returns 6, -1
        quotient, remainder = div_euclid(a, b);
        println("div_euclid(-17, -3) returns {}, {}", quotient, remainder);         // div_euclid(-17, -3) returns 6, 1
        ```

    <br />

    - #### **rand**

        - `rand()` : EUDVariable  
            Generates a random integer in the range of 0~0xFFFF.  

        - `dwrand()` : EUDVariable  
            Generates a random integer in the range of 0~0xFFFFFFFF.

        > **Note**
        > Do not use random number functions in desync conditions.  

        Example

        ```JavaScript
        const r = rand();
        ```

    <br />

    - #### **seed**

        - `srand`(seed)  
            Sets the random seed to [seed].  

        - `getseed()` : EUDVariable  
            Gets the set random seed.  

        > **Note**
        > Do not use random number functions in desync conditions.    

        Example

        ```JavaScript
        var seed = getseed();
        srand(seed + 1);
        ```

    <br />

    - #### **randomize**

        - `randomize()` : EUDVariable  
            Initializes the random seed.  

        > **Note**
        > Do not use random number functions in desync conditions.    

        Example

        ```JavaScript
        function onPluginStart() {
            randomize();
        }
        ```

    <br />
    <br />

- ### Bitwise Operation Functions

    <br />

    - #### **bitand**

        - `bitand`(a, b) : py_int | EUDVariable  
            Bitwise AND operation [a] & [b]  

        Example

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}", bitand(a, b)); // 0 (binary 0b0000）
        ```

    <br />

    - #### **bitor**

        - `bitor`(a, b) : py_int | EUDVariable  
            Bitwise OR operation [a] | [b]  

        Example

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}", bitor(a, b)); // 15 (binary 0b1111）
        ```

    <br />

    - #### **bitnot**

        - `bitnot`(a) : py_int | EUDVariable  
            Bitwise NOT operation ~[a]  

        Example

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}, {}", bitnot(a), b); // 12, 12 (binary 0b1100, 0b1100)
        ```

    <br />

    - #### **bitxor**

        - `bitxor`(a, b) : py_int | EUDVariable  
            Bitwise XOR operation [a] ^ [b]  

        Example

        ```JavaScript
        var a = 0b0111; // 7
        var b = 0b1110; // 14
        println("{}", bitxor(a, b)); // 9 (binary 0b1001)
        ```

    <br />

    - #### **bitnand**

        - `bitnand`(a, b) : py_int | EUDVariable  
            Bitwise NAND operation ~([a] & [b])  

        Example

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}", bitnand(a, b)); // 15 (binary 0b1111)
        ```

    <br />

    - #### **bitnor**

        - `bitnor`(a, b) : py_int | EUDVariable  
            Bitwise NOR operation ~([a] | [b])  

        Example

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}", bitnor(a, b)); // 0 (binary 0b0000)
        ```

    <br />

    - #### **bitnxor**

        - `bitnxor`(a, b) : py_int | EUDVariable  
            Bitwise XNOR operation ~([a] ^ [b])  

        Example

        ```JavaScript
        var a = 0b0111;
        var b = 0b1110;
        println("{}", bitnxor(a, b)); // 6 (binary 0b0110)
        ```

    <br />

    - #### **bitlshift**

        - `bitlshift`(a, b) : py_int | EUDVariable  
            Bitwise Left shift operation [a] << [b]  

        Example

        ```JavaScript
        var a = 0b0111; // 7
        var b = 1;
        println("{}", bitlshift(a, b)); // 14 (binary 0b1110)
        ```

    <br />

    - #### **bitrshift**

        - `bitrshift`(a, b) : py_int | EUDVariable  
            Bitwise Right shift operation [a] >> [b]  

        Example

        ```JavaScript
        var a = 0b0111; // 7
        var b = 1;
        println("{}", bitrshift(a, b)); // 3 (binary 0b0011)
        ```


    <br />
    <br />

- ### QueueGameCommand Functions

    Queue game command to packet queue.  

    Starcraft periodically broadcasts game packets to other player. Game packets are stored to queue, and this function add data to that queue, so that SC can broadcast it.
  
    The QueueGameCommand functions are all for the local player rather than the current player, and cannot be used for players not in the game or computer players.  

    > **Note**
    > If packet queue is full, this function fails. This behavior is silent
    > without any warning or error, since this behavior shouldn't happen in
    > common situations. So **Don't** use this function too much in a frame.

    <br />

    - #### **QueueGameCommand**

        - `QueueGameCommand`(data, size)  
            Adds a data packet of size [size] [data] to the local broadcast queue. All functions in this section are wrappers for sending specific data packets to this function.  

    <br />

    - #### **QueueGameCommand_MinimapPing**

        - `QueueGameCommand_MinimapPing`(xy)  
            Adds a data packet to the local broadcast queue to ping at coordinates [xy] on the minimap. The xy calculation method is x + y * 65536.  

        Example

        ```JavaScript
        // Ping at coordinates 1234, 2345
        QueueGameCommand_MinimapPing(1234 + 2345 * 65536);
        ```

    <br />

    - #### **QueueGameCommand_QueuedRightClick**

        - `QueueGameCommand_QueuedRightClick`(xy)  
            Adds a data packet to the local broadcast queue for a right click at coordinates [xy]. The xy calculation method is x + y * 65536.  

        Example

        ```JavaScript
        // Right click at coordinates 1234, 2345. If units are selected, they will move there.
        QueueGameCommand_QueuedRightClick(1234 + 2345 * 65536);
        ```

    <br />

    - #### **QueueGameCommand_Select**

        - `QueueGameCommand_Select`(n, ptrList: EUDArray)  
            Adds a data packet to the local broadcast queue to select some units. [n] is the number of units, [ptrList] is the cunit pointer list, not the epd list.  

            > **Note**
            > This only sends a "units selected" data packet locally, it does not actually select units on the local screen. It only tells other online players that the local player has selected these units. If RightClick data packets are sent immediately after, these units will move to the target location.  

        Example

        ```JavaScript
        const uar = EUDArray(12);
        if (playerexist(P1) && GetPlayerInfo(P1).type == 0x06) { // Check if player 1 is an online human player
            foreach(i : py_range(3)) {
                uar[i] = dwread_epd(EPD(0x628438));
                CreateUnitWithProperties(1, "Zerg Overlord", "Location 1", P1, UnitProperty(invincible = true));
            }
        }
        if (getuserplayerid() == $P1) { // Check if the local player is player 1
            QueueGameCommand_Select(3, uar);
            QueueGameCommand_QueuedRightClick(1234 + 2345 * 65536);
        }
        ```

    <br />

    - #### **QueueGameCommand_PauseGame**

        - `QueueGameCommand_PauseGame()`  
            Adds a pause game data packet to the local broadcast queue.  

    <br />

    - #### **QueueGameCommand_ResumeGame**

        - `QueueGameCommand_ResumeGame()`  
            Adds a resume game data packet to the local broadcast queue.  

    <br />

    - #### **QueueGameCommand_RestartGame**

        - `QueueGameCommand_RestartGame()`  
            Adds a restart game data packet to the local broadcast queue.  

    <br />

    - #### **QueueGameCommand_UseCheat**

        - `QueueGameCommand_UseCheat`(cheats)  
            Use [cheats] locally, invalid for multiplayer games.  

        <details><summary>Cheat code list</summary>

        ```js
        0x00000001 Black Sheep Wall
        0x00000002 Operation CWAL
        0x00000004 Power Overwhelming
        0x00000008 Something For Nothing
        0x00000010 Show me the Money
        0x00000020
        0x00000040 Game Over Man
        0x00000080 There is no Cow Level
        0x00000100 Staying Alive
        0x00000200 Ophelia
        0x00000400
        0x00000800 The Gathering
        0x00001000 Medieval Man
        0x00002000 Modify the Phase Variance
        0x00004000 War Aint What It Used To Be
        0x00008000
        0x00010000
        0x00020000 Food For Thought
        0x00040000 Whats Mine Is Mine
        0x00080000 Breathe Deep
        0x20000000 Noglues
        ```
        </details>

        Example

        ```JavaScript
        QueueGameCommand_UseCheat(0x00000001 | 0x00000002 | 0x00000010);  // Enable Black Sheep Wall + Operation CWAL + Show me the Money
        QueueGameCommand_UseCheat(0x00000002);                            // Disable Operation CWAL
        QueueGameCommand_UseCheat(0);                                     // Disable all cheats
        ```

    <br />

    - #### **QueueGameCommand_TrainUnit**

        - `QueueGameCommand_TrainUnit`(unit: TrgUnit)  
            Adds a train specified unitType data packet to the local broadcast queue. Use with QueueGameCommand_Select to select units.  

        Example

        ```JavaScript
        once {
            const uar = EUDArray(12);
            if (playerexist(P1) && GetPlayerInfo(P1).type == 0x06) { // Check if player 1 is an online human player
                SetResources(P1, Add, 10000, OreAndGas);
                uar[0] = dwread_epd(EPD(0x628438));
                CreateUnitWithProperties(1, "Terran Command Center", "Location 1", P1, UnitProperty(invincible = true));
            }
            if (getuserplayerid() == $P1) { // Check if the local player is player 1
                QueueGameCommand_Select(1, uar); // Check if the local player is player 1
                QueueGameCommand_QueuedRightClick(1234 + 2345 * 65536); /* Set the rally point to 1234, 2345 */
                QueueGameCommand_TrainUnit("Terran SCV"); /* 训练一个 SCV */
            }
        }
        ```

    <br />

    - #### **QueueGameCommand_MergeDarkArchon**

        - `QueueGameCommand_MergeDarkArchon()`  
            Adds a merge dark archon data packet to the local broadcast queue. Use with QueueGameCommand_Select to select units.  

        Example

        ```JavaScript
        once {
            const uar = EUDArray(12);
            if (playerexist(P1) && GetPlayerInfo(P1).type == 0x06) { // Check if player 1 is an online human player
                foreach(i : py_range(6)) {
                    uar[i] = dwread_epd(EPD(0x628438));
                    CreateUnitWithProperties(1, "Protoss Dark Templar", "Location 1", P1, UnitProperty(invincible = true));
                }
            }
            if (getuserplayerid() == $P1) { // Check if the local player is player 1
                QueueGameCommand_Select(6, uar);
                QueueGameCommand_MergeDarkArchon();
            }
        }
        ```

    <br />

    - #### **QueueGameCommand_MergeArchon**

        - `QueueGameCommand_MergeArchon()`  
            Adds a merge archon data packet to the local broadcast queue. Use with QueueGameCommand_Select to select units.  

        Example

        ```JavaScript
        once {
            const uar = EUDArray(12);
            if (playerexist(P1) && GetPlayerInfo(P1).type == 0x06) { // Check if player 1 is an online human player
                foreach(i : py_range(6)) {
                    uar[i] = dwread_epd(EPD(0x628438));
                    CreateUnitWithProperties(1, "Protoss High Templar", "Location 1", P1, UnitProperty(invincible = true));
                }
            }
            if (getuserplayerid() == $P1) { // Check if the local player is player 1
                QueueGameCommand_Select(6, uar);
                QueueGameCommand_MergeArchon();
            }
        }
        ```

