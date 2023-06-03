# How epScript Works 

<br />

- [Everything Is A Trigger](#everything-is-a-trigger)
- [Virtual Triggers](#virtual-triggers)
- [Mathematical Operations](#mathematical-operations)
    - [Number Modifier Description](#number-modifier-description)
- [Variable Implementation](#eudvariable-implementation)
    - [What Are Variables](#what-are-variables-eudvariable)
    - [Passing The Variable To Conditions Or Actions](#passing-the-variable-to-conditions-or-actions)
    - [Why Does An Variable Occupy 72 Bytes In Memory?](#why-does-an-eudvariable-occupy-72-bytes-in-memory)
    - [Variable Operation Optimization](#variable-operation-optimization)
- [Strings And Light Variables](#strings-db-or-stringbuffer-light-arrays-eudarray-and-light-variables-eudlightvariable)
    - [Structure](#structure)
    - [Memory Reading or Copying](#memory-reading-or-copying)

<br />

- ## Everything Is A Trigger

    The maps of Starcraft: Remastered do not support any runtime scripting languages.  
    The reason why epScript scripts (`*.eps`) can take effect at runtime is that epScript code will eventually be inserted into the map in the form of triggers, and triggers can really take effect at runtime.   

    > Trigger bytecode structure reference:   
    > [http://www.staredit.net/wiki/index.php/Scenario.chk#.22TRIG.22_-_Triggers](http://www.staredit.net/wiki/index.php/Scenario.chk#.22TRIG.22_-_Triggers)  
    > [https://github.com/phu54321/TrigEditPlus/blob/master/TrigEditPlus/Editor/TriggerEditor.h#L71](https://github.com/phu54321/TrigEditPlus/blob/master/TrigEditPlus/Editor/TriggerEditor.h#L71)  

    <br />

- ## Virtual Triggers

    Because the triggers in the [TRIG section](http://www.staredit.net/wiki/index.php/Scenario.chk#.22TRIG.22_-_Triggers) in [Scenario.chk](http://www.staredit.net/wiki/index.php/Scenario.chk) are not loaded into the memory as a whole, but are loaded in the form of a [node list](https://euddb.website/?pg=entry&id=763), and the nodes on the node list need to be traversed to locate them during runtime.  
    [jjf28](http://www.staredit.net/topic/17546/#1) posted that you just need to write the bytecode of the trigger to any accessible location in memory, then add it to the trigger node list , and they will work normally.  
    These triggers that are not in the TRIG section can determine their relative positions in memory at runtime, which means that it is relatively easy to achieve positioning jumps between such triggers. jjf28 calls such triggers Virtual Triggers.  
    [trgk](http://www.staredit.net/topic/17546/#11) proposed that the STR section will be loaded into the memory as a whole at runtime, so if a virtual trigger is written to the STR section, the relative position of its runtime memory can be easily fixed at compile time, thereby enabling more It is easy to implement dynamic modification of triggers during runtime to realize conditional control flow.  
    On this basis, trgk designed a Python pseudo-syntax library [eudplib](https://github.com/armoha/eudplib) for conditional control flow.  

    > Reference: [http://www.staredit.net/topic/17546/](http://www.staredit.net/topic/17546/)

    <br />

- ## Mathematical Operations

    Ordinary triggers do not have complete mathematical operation capabilities.  
    The functions that can be used to simulate mathematical operations are the [number modifier](http://www.staredit.net/wiki/index.php/Scenario.chk#Number_Modifiers) (SetTo/Add/Subtract) in the trigger [actions](http://www.staredit.net/wiki/index.php/Scenario.chk#Trigger_Actions_List) - they are usually used to SetTo/Add/Subtract player resources or death counts, etc.  
    In addition, in the Remastered Edition, Blizzard software engineer [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) added bitmask parameters (DeathsX and SetDeathsX) to the Deaths condition and SetDeaths action.  
    Based on these and the free trigger flow control implemented in the last section [Virtual Triggers](#virtual-triggers), the author of eudplib implemented basic integer operations in epScript.  

    - ### Number Modifier Description

        The Add method will overflow to 0 and start over if it exceeds the 4-byte range (0xFFFFFFFF)  
        ```JavaScript
        var a = 0xFFFFFFFF;
        println("a == {}", a); // a == 4294967295
        DoActions(a.AddNumber(5));
        println("a == {}", a); // a == 4
        ```

        The Subtract method is limited to subtracting a number down to 0 at most, even if the subtrahend is greater than the minuend.  
        ```JavaScript
        var a = 10;
        DoActions(a.SubtractNumber(200000));
        println("a == {}", a); // a == 0
        ```

    <details><summary>Simulated code for subtraction, multiplication, division, power and square root in epScript</summary>

    ```JavaScript
    // This code is for principle demonstration only and does not handle any boundary issues. Please use the operators and functions already implemented by epScript in actual projects. 

    // Opposite number: https://github.com/armoha/eudplib/blob/master/eudplib/core/variable/vbase.py#L125
    function my_neg(x) {
        RawTrigger(actions = list(
            // // Inverse code + 1 = Complementary code
            x.AddNumberX(0xFFFFFFFF, 0x55555555),
            x.AddNumberX(0xFFFFFFFF, 0xAAAAAAAA),
            x.AddNumber(1),
        ));
        return x;
    }

    // Absolute value: https://github.com/armoha/eudplib/blob/master/eudplib/core/variable/vbase.py#L138
    function my_abs(x) {
        if (x >= 0x80000000) { // Signed bit is 1
            return my_neg(x);
        }
        return x;
    }

    // Subtract cannot be used to calculate subtraction where the result is less than 0. Subtraction is implemented by adding the complement of the subtrahend (i.e. the opposite number) to the minuend. 
    // Subtraction implementation: https://github.com/armoha/eudplib/blob/master/eudplib/core/variable/eudv.py#L336
    function my_minus(a, b) {
        b = my_neg(b);
        return a + b;
    }

    // Multiplication implementation: https://github.com/armoha/eudplib/blob/master/eudplib/core/calcf/muldiv.py#L384
    function my_mul(a, b) {
        var ret = 0;
        foreach (i : py_range(32)) {
            if (a & (1 << i)) {
                ret += b << i;
            }
        }
        return ret;
    }

    // Division implementation: https://github.com/armoha/eudplib/blob/master/eudplib/core/calcf/muldiv.py#L421
    function my_div(a, b) {
        var ret = 0;
        foreach (i : py_range(31, -1, -1)) {
            if (a >= (b << i)) {
                ret += (1 << i);
                a += my_neg(b << i);
            }
        }
        return ret, a;
    }

    // Power implementation: https://github.com/armoha/eudplib/blob/master/eudplib/eudlib/mathf/pow.py#L14
    function my_pow(a, b) {
        var ret, _2n = 1, 1;
        while (_2n <= b) {
            if (b.AtLeastX(1, _2n)) {
                ret = my_mul(ret, a);
            }
            _2n += _2n;
            a = my_mul(a, a);
        }
        return ret;
    }

    // Square root implementation: https://github.com/armoha/eudplib/blob/master/eudplib/eudlib/mathf/sqrt.py
    function my_sqrt(x) {
        var y = 0;
        foreach (i : py_range(15, -1, -1)) {
            y += py_pow(2, i);
            if (my_pow(y, 2) > x) {
                y += my_neg(py_pow(2, i));
            }
        }
        return y;
    }
    ```
    </details>

    <br />

- ## EUDVariable Implementation

    Reference: [https://cafe.naver.com/edac/74507](https://cafe.naver.com/edac/74507)

    - ### What Are Variables (EUDVariable)

        EUD triggers themselves have no concept of variables. Runtime variables (EUDVariable) in eudplib are essentially virtual triggers with only one SetDeathsX action and no conditions.   
        
        Its syntax in TrigEdit++ would be something like this:  
        ```Lua
        A_Variable = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD(destAddr), modifier, value, 0, 0xFFFFFFFF);
                --                                    ^
                --                     The variable value is stored here
            };
        }
        ```

        For example, the following epScript code:  

        ```JavaScript
        function onPluginStart() {
            var a, b = 4, 7;
            a += b;
        }
        ```

        Would roughly be compiled into several triggers like this (descriptive TrigEdit++ pseudocode, not usable code):  

        ```Lua
        -- Declaration of variable a trigger block
        a = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD( &(a.value) ),            SetTo,          4 , 0, 0xFFFFFFFF); -- &(a.value) refers to the address of a variable [value] field
                --                    ^                    ^             ^
                --           Destination Address    Number Modifier  The value
            };
            next_trigger = b;
        }

        -- Declaration of variable b trigger block
        b = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD( &(b.value) ),            SetTo,          7 , 0, 0xFFFFFFFF); -- &(b.value) refers to the address of b variable [value] field
                --                    ^                    ^             ^
                --           Destination Address    Number Modifier  The value
            };
            next_trigger = AssignmentOperation;
        }

        AssignmentOperation = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetMemory(b.destAddr, SetTo, EPD( &(a.value) )); -- &(a.value) refers to the address of a variable [value] field
                SetMemory(&(b.modifier), SetTo, Add);
                SetMemory(b.next_trigger, SetTo, NEXT_NEXT_TRIGGER);
            };
            next_trigger = b;
        }

        --[[
        -- After the AssignmentOperation, b would be something like this:
        b = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetMemory(EPD( &(a.value) ), Add, 7);
            };
            next_trigger = NEXT_NEXT_TRIGGER;
        }
        --]]

        NEXT_NEXT_TRIGGER... 
        ```

        You can see that the assignment operation is also a trigger. When this trigger executes, it sets b's target address to a's value address, sets b's method to Add, and then executes the b trigger, thus implementing adding b's value to a's value.  

        Below is an actual epScript code demonstration of the variable assignment process described above (principle demonstration):  
        ```JavaScript
        function onPluginStart() {
            // var a, b = 4, 7;
            const a = RawTrigger(actions = SetDeathsX(0, 0, 0, 0, 0)); // var a
            const b = RawTrigger(actions = SetDeathsX(0, 0, 0, 0, 0)); // var b
            const a_actionAddr = a + 8 + 320; const a_next_trigger = a + 4; const a_maskAddr = a_actionAddr; const a_destAddr = a_actionAddr + 16; const a_valueAddr = a_actionAddr + 20; const a_modifierAddr = a_actionAddr + 24;
            const b_actionAddr = b + 8 + 320; const b_next_trigger = b + 4; const b_maskAddr = b_actionAddr; const b_destAddr = b_actionAddr + 16; const b_valueAddr = b_actionAddr + 20; const b_modifierAddr = b_actionAddr + 24;
            RawTrigger(actions = list(
                SetMemory(a_valueAddr, SetTo, 4), // a = 4
                SetMemory(b_valueAddr, SetTo, 7), // b = 7
            ));

            // a += b;
            const NEXT_NEXT_TRIGGER = Forward();
            RawTrigger( // Assignment operation
                actions = list(
                    SetMemory(b_maskAddr, SetTo, 0xFFFFFFFF),
                    SetMemory(b_destAddr, SetTo, EPD(a_valueAddr)),
                    SetMemoryX(b_modifierAddr, SetTo, ($Add << 24), 0xFF000000),
                    SetMemory(b_next_trigger, SetTo, NEXT_NEXT_TRIGGER), // Set b's next trigger to "NEXT_NEXT_TRIGGER"
                ),
                nextptr = b, // The next trigger of this trigger is b
            );
            NEXT_NEXT_TRIGGER.__lshift__(NextTrigger()); // "NEXT_NEXT_TRIGGER" 在这

            // This just outputs the result and is not part of the assignment process
            println("a:{} b:{}", dwread(a_valueAddr), dwread(b_valueAddr)); // a:11 b:7
        }
        ```

    - ### Passing The Variable To Conditions Or Actions

        The process of passing the value of a variable to action parameters    

        For example, the following epScript code:

        ```JavaScript
        function onPluginStart() {
            var a = 1234;
            Trigger(actions = SetResources(P1, Add, a, Ore)); // Pass the value of the variable to the SetResources action
        }
        ```

        Would roughly be compiled into several triggers like this (descriptive TrigEdit++ pseudocode, not usable code):  

        ```Lua
        -- Declaration of variable a trigger block
        a = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD( &(a.value) ),           SetTo,         1234, 0, 0xFFFFFFFF); -- &(a.value) refers to the address of a variable [value] field
                --                    ^                   ^             ^
                --           Destination Address   Number Modifier   The value
            };
            next_trigger = PassValueToActionOperation;
        }

        PassValueToActionOperation = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetMemory(a.destAddr, SetTo, EPD( &(AddMineralOperation.value) )); -- &(AddMineralOperation.value) refers to the address of AddMineralOperation variable [value] field
                SetMemory(&(a.modifier), SetTo, SetTo);
                SetMemory(a.next_trigger, SetTo, AddMineralOperation);
            };
            next_trigger = a;
        }

        --[[
        -- After PassValueToActionOperation, a would be something like this:  
        a = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD( &(AddMineralOperation.value) ), SetTo, 1234, 0, 0xFFFFFFFF);
            };
            next_trigger = AddMineralOperation;
        }
        --]]

        AddMineralOperation = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetResources(P1,    Add,        This-will-be-changed,  Ore);
                --                   ^                   ^
                --             Number Modifier        The value
            };
            next_trigger = NEXT_NEXT_TRIGGER;
        }

        NEXT_NEXT_TRIGGER...
        ```

        The following epScript code explains what a variable is and how its value is passed into trigger actions (principle demonstration):  

        ```JavaScript
        function onPluginStart() {
            // var a = 1234;
            const a = RawTrigger(actions = SetDeathsX(0, 0, 0, 0, 0)); // var a
            const a_actionAddr = a + 8 + 320; const a_next_trigger = a + 4; const a_maskAddr = a_actionAddr; const a_destAddr = a_actionAddr + 16; const a_valueAddr = a_actionAddr + 20; const a_modifierAddr = a_actionAddr + 24;
            RawTrigger(actions = SetMemory(a_valueAddr, SetTo, 1234)); // a = 1234
            
            // Trigger(actions = SetResources(P1, Add, a, Ore));
            const AddMineralOperation = SetResources(P1, Add, 0, Ore); const AddMineralOperation_valueAddr = AddMineralOperation + 20;
            //                                                ^
            //                         The value of the variable a needs to be passed here 
            const AddMineralOperation = Forward();
            RawTrigger(
                actions = list(
                    SetMemory(a_maskAddr, SetTo, 0xFFFFFFFF),
                    SetMemory(a_destAddr, SetTo, EPD(AddMineralOperation_valueAddr)),
                    SetMemoryX(a_modifierAddr, SetTo, ($SetTo << 24), 0xFF000000),
                    SetMemory(a_next_trigger, SetTo, AddMineralOperation), // Set a's next trigger to "AddMineralOperation"
                ),
                nextptr = a, // The next trigger of this trigger is a
            );
            AddMineralOperation.__lshift__(RawTrigger(actions = AddMineralOperation)); // "AddMineralOperation" is here 
        }
        ```

    - ### Why Does An EUDVariable Occupy 72 Bytes In Memory? 

        As mentioned earlier, an EUDVariable is a trigger that contains only one SetDeathsX action.  
        However, in the Scenario.chk structure of the map, a trigger occupies 2400 bytes. In addition, the trigger node information prevTriggerPtr and nextTriggerPtr in memory occupies 8 bytes. Therefore, a trigger should occupy 2408 bytes in memory.  
        Why is it 72 bytes?  
        <details><summary>You can expand to view the TriggerNode structure</summary>

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
            uint32_t executionFlags;         /* 0x1 - Ignore Conditions Once, 0x4 - Preserve Trigger, 0x8 - Disabled */
            uint8_t effPlayer[27];
            uint8_t currentAction;
        } Trigger;

        typedef struct { // Trigger node (2408 bytes)
            uint32_t prevTriggerPtr;
            uint32_t nextTriggerPtr;
            Trigger trigger;
        } TriggerNode;
        ```
        </details>

        Structurally, a single TriggerNode occupies 2408 bytes in memory.  
        The first 8 bytes are the linked list node structure, followed by 320 bytes for 16 conditions, with each condition occupying 20 bytes. From the 328th byte onwards is the action list, with 64 actions occupying 32 bytes each.    
        This structure is fixed, so even with only one action, a trigger node will occupy 2408 bytes of space.  
        However, when StarCraft 1 is running, the traversal of trigger conditions/actions follows a short-circuit policy - when traversing conditions/actions, the first empty condition/action will ignore all subsequent conditions/actions.  
        In addition, an EUDVariable only needs to use one action, which means that many bytes used to implement the EUDVariable trigger are actually ignored and idle.  
        So how can we use this idle space? The answer is to stack  multiple EUDVariable trigger nodes, like a poker hand with only the key parts exposed, allowing people to identify the contents.  
        Now assume we have more than 2408 bytes of memory space. We can try to build a fake trigger node structure on top of this.  
        The first 4 bytes of the trigger node store the information of the previous trigger node (prevTriggerPtr), which seems to have no use during the game. We can ignore it.   
        The next 5 to 8 bytes are the information of the next trigger node (nextTriggerPtr), which is useful. So these few bytes cannot be overwritten when stacked.   
        Then there is the position of the 8 + 4 + 4 + 4 + 2 + 1 + 1 = 24th byte (trigger.conditions[0].conditionType) that needs to be set to 0 (the first condition needs to be empty). It also cannot be overwritten by other content when stacked.  
        Then there is the first action of the trigger, from the 329th byte (8 + 328 + 1) to the 360th byte (8 + 320 + 32), at the position of trigger.actions[0]. So these bytes also need to be marked and cannot be overwritten when stacked.   
        Next, we need to handle the second action of the trigger. Since its first action is not empty, the game will continue to detect the second action. We have to set the second action to empty to prevent the game from detecting the third action. That is, we fix the 387th byte (360 + 27) (trigger.actions[1].actionType) to 0 and ensure that it is not overwritten by other content.   
        Finally, the content of the 2377th byte (8 + 320 + 2048 + 1) (trigger.executionFlags) of this trigger node must be 1. If not stacked, such a variable would occupy at least 2377 bytes.   
        We need to find an idle offset between 0 and 2376 to define another trigger node. This offset needs to prevent the crossing overwrite of the above key positions of multiple overlapping trigger nodes.  

        <details><summary>List of key positions for trigger node identification</summary>

        ```C
        node[i].nextTriggerPtr                    // 5 ~ 8
        node[i].trigger.condtions[0].condtionType // 24
        node[i].trigger.condtions[0].prop         // 26
        node[i].trigger.actions[0]                // 329 ~ 360
        node[i].trigger.actions[1].actionType     // 387
        node[i].trigger.actions[1].prop           // 389
        node[i].trigger.executionFlags            // 2377
        ```
        </details>

        If the offset is x, then x needs to satisfy the conditions that the key content at `(5~8) + i * x`, `24 + i * x`, `26 + i * x`, `(329~360) + i * x`, `387 + i * x`, `389 + i * x`, `2377 + i * x` will not be overwritten due to overlap  (i is the number of stacked triggers)  

        <details><summary>Script to solve the minimum value of x</summary>

        ```Python
        #!/usr/bin/env python3

        _nextptr = [c for c in range(4 + 1, 8 + 1)]
        _trigger_conditions_0_type_prop = [24, 26]
        _trigger_actions_0 = [c for c in range(328 + 1, 360 + 1)]
        _trigger_actions_1_type_prop = [387, 389]
        _trigger_execution_flags = [2376 + 1]

        _node_plist = _nextptr + _trigger_conditions_0_type_prop + _trigger_actions_0 + _trigger_actions_1_type_prop + _trigger_execution_flags

        def detect(x):
            offsets = set()
            for i in range(500): # Test the number of stacked triggers is 500
                for b in _node_plist:
                    o = b + i * x
                    if o in offsets:
                        return False
                    else:
                        offsets.add(o)
            return True

        for x in range(0, 2376):
            if detect(x):
                print(x)
                break
        ```

        ```Python
        72
        ```

        </details>

        The x value in eudplib is 72, which should be the smallest integer calculated by the eudplib author that satisfies the above conditions.   
        Assuming the starting position of the first variable trigger node is at position 0, the second variable trigger node is at position 72, and so on.  

        <details><summary>This is the script to generate the trigger stack test output table</summary>

        ```Python
        #!/usr/bin/env python3
        x = 72
        print("|{:^12}|{:^17}|{:^19}|{:^17}|{:^19}|{:^12}|".format("VarTrigger", "node", "cond[0].type/prop", "act[0]", "act[1].type/prop", "execflags"))
        for i in range(0, 40):
            ntp1 = 5 + i * x
            ntp2 = 8 + i * x
            ct = 24 + i * x
            a11 = 8 + 320 + 1 + i * x
            a12 = 8 + 320 + 32 + i * x
            a2t = 8 + 320 + 32 + 27 + i * x
            eflags = 2376 + 1 + i * x
            print("|{:^12}| {:^7}~{:^7} |  {:^7}/{:^7}  | {:^7}~{:^7} |  {:^7}/{:^7}  |{:^12}|".format(i, ntp1, ntp2, ct, ct + 2, a11, a12, a2t, a2t + 2, eflags))
        ```

        ```Plain Text
        | VarTrigger |      node       | cond[0].type/prop |     act[0]      | act[1].type/prop  | execflags  |
        |     0      |    5   ~   8    |    24   /  26     |   329  ~  360   |    387  /  389    |    2377    |
        |     1      |   77   ~  80    |    96   /  98     |   401  ~  432   |    459  /  461    |    2449    |
        |     2      |   149  ~  152   |    168  /  170    |   473  ~  504   |    531  /  533    |    2521    |
        |     3      |   221  ~  224   |    240  /  242    |   545  ~  576   |    603  /  605    |    2593    |
        |     4      |   293  ~  296   |    312  /  314    |   617  ~  648   |    675  /  677    |    2665    |
        |     5      |   365  ~  368   |    384  /  386    |   689  ~  720   |    747  /  749    |    2737    |
        |     6      |   437  ~  440   |    456  /  458    |   761  ~  792   |    819  /  821    |    2809    |
        |     7      |   509  ~  512   |    528  /  530    |   833  ~  864   |    891  /  893    |    2881    |
        |     8      |   581  ~  584   |    600  /  602    |   905  ~  936   |    963  /  965    |    2953    |
        |     9      |   653  ~  656   |    672  /  674    |   977  ~ 1008   |   1035  / 1037    |    3025    |
        |     10     |   725  ~  728   |    744  /  746    |  1049  ~ 1080   |   1107  / 1109    |    3097    |
        |     11     |   797  ~  800   |    816  /  818    |  1121  ~ 1152   |   1179  / 1181    |    3169    |
        |     12     |   869  ~  872   |    888  /  890    |  1193  ~ 1224   |   1251  / 1253    |    3241    |
        |     13     |   941  ~  944   |    960  /  962    |  1265  ~ 1296   |   1323  / 1325    |    3313    |
        |     14     |  1013  ~ 1016   |   1032  / 1034    |  1337  ~ 1368   |   1395  / 1397    |    3385    |
        |     15     |  1085  ~ 1088   |   1104  / 1106    |  1409  ~ 1440   |   1467  / 1469    |    3457    |
        |     16     |  1157  ~ 1160   |   1176  / 1178    |  1481  ~ 1512   |   1539  / 1541    |    3529    |
        |     17     |  1229  ~ 1232   |   1248  / 1250    |  1553  ~ 1584   |   1611  / 1613    |    3601    |
        |     18     |  1301  ~ 1304   |   1320  / 1322    |  1625  ~ 1656   |   1683  / 1685    |    3673    |
        |     19     |  1373  ~ 1376   |   1392  / 1394    |  1697  ~ 1728   |   1755  / 1757    |    3745    |
        |     20     |  1445  ~ 1448   |   1464  / 1466    |  1769  ~ 1800   |   1827  / 1829    |    3817    |
        |     21     |  1517  ~ 1520   |   1536  / 1538    |  1841  ~ 1872   |   1899  / 1901    |    3889    |
        |     22     |  1589  ~ 1592   |   1608  / 1610    |  1913  ~ 1944   |   1971  / 1973    |    3961    |
        |     23     |  1661  ~ 1664   |   1680  / 1682    |  1985  ~ 2016   |   2043  / 2045    |    4033    |
        |     24     |  1733  ~ 1736   |   1752  / 1754    |  2057  ~ 2088   |   2115  / 2117    |    4105    |
        |     25     |  1805  ~ 1808   |   1824  / 1826    |  2129  ~ 2160   |   2187  / 2189    |    4177    |
        |     26     |  1877  ~ 1880   |   1896  / 1898    |  2201  ~ 2232   |   2259  / 2261    |    4249    |
        |     27     |  1949  ~ 1952   |   1968  / 1970    |  2273  ~ 2304   |   2331  / 2333    |    4321    |
        |     28     |  2021  ~ 2024   |   2040  / 2042    |  2345  ~ 2376   |   2403  / 2405    |    4393    |
        |     29     |  2093  ~ 2096   |   2112  / 2114    |  2417  ~ 2448   |   2475  / 2477    |    4465    |
        |     30     |  2165  ~ 2168   |   2184  / 2186    |  2489  ~ 2520   |   2547  / 2549    |    4537    |
        |     31     |  2237  ~ 2240   |   2256  / 2258    |  2561  ~ 2592   |   2619  / 2621    |    4609    |
        |     32     |  2309  ~ 2312   |   2328  / 2330    |  2633  ~ 2664   |   2691  / 2693    |    4681    |
        |     33     |  2381  ~ 2384   |   2400  / 2402    |  2705  ~ 2736   |   2763  / 2765    |    4753    |
        |     34     |  2453  ~ 2456   |   2472  / 2474    |  2777  ~ 2808   |   2835  / 2837    |    4825    |
        |     35     |  2525  ~ 2528   |   2544  / 2546    |  2849  ~ 2880   |   2907  / 2909    |    4897    |
        |     36     |  2597  ~ 2600   |   2616  / 2618    |  2921  ~ 2952   |   2979  / 2981    |    4969    |
        |     37     |  2669  ~ 2672   |   2688  / 2690    |  2993  ~ 3024   |   3051  / 3053    |    5041    |
        |     38     |  2741  ~ 2744   |   2760  / 2762    |  3065  ~ 3096   |   3123  / 3125    |    5113    |
        |     39     |  2813  ~ 2816   |   2832  / 2834    |  3137  ~ 3168   |   3195  / 3197    |    5185    |
        ```
        </details>

        From the results, they follow a cycle that barely does not overlap. The starting position of VarTrigger[5+n].node just happens to be the ending position of VarTrigger[n].act[0]. The end of VarTrigger[5+n].cond[0].prop just happens to be the starting position of VarTrigger[n].act[0].type.   
        
        eudplib's approach is to define a trigger header, then shift 72 bytes to define another trigger header. The second trigger header is in the condition block of the first trigger, and so on. These staggered triggers do not collide.   

        Each variable occupies 72 bytes in this way. The position of the last variable plus 2376 also needs to have a Preserved flag. That is, if there are 100 variables, the number of bytes occupied is (100 - 1) * 72 + 2376 bytes.  


    - ### Variable Operation Optimization

        Variables are implemented using triggers. Assigning values to variables and operating on variables will generate operation triggers.  

        For example:

        ```JavaScript
        function afterTriggerExec() {
            var a, b = 3, 5;
            b += 1;
            a += b + 1;
            // Result is a:10 b:6
        }
        ```

        The `b += 1; a += b + 1;` two lines of code may generate 6 additional triggers.  
        This scale of code generation is caused by the eudplib implementation method, and it can be optimized.  
        It obviously does not need to use so many triggers. According to our understanding of the implementation principle of variables, it can also be written like this:  

        ```JavaScript
        // The .getDestAddr() method of variables can get the destination address in the variable trigger at compile time.
        // The .getValueAddr() method of variables can get the value address in the variable trigger at compile time.  
        // The .GetVTable() method of variables can get the virtual trigger address of the variable at compile time.
        // The .SetModifier(method) method sets the numeric modification method in the variable trigger to method.
        // SetNextPtr(trg, ptr) function is used to set the next trigger of trigger trg to ptr.
        function afterTriggerExec() {
            var a, b = 3, 5;
            const next = Forward();
            RawTrigger(
                actions = list(
                    SetMemory(b.getValueAddr(), Add, 1),
                    SetMemory(a.getValueAddr(), Add, 1),
                    SetMemory(b.getDestAddr(), SetTo, EPD(a.getValueAddr())),
                    b.SetModifier(Add), // Internally it is probably implemented as SetMemoryX(b.getValueAddr() + 4, SetTo, (Add << 24), 0xFF000000)
                    SetNextPtr(b.GetVTable(), next), // Set the next trigger of b to next, which may be implemented internally like this: SetMemory(b.GetVTable() + 4, SetTo, next)
                ),
                nextptr = b.GetVTable(), // The next trigger of this trigger is b
            );
            next.__lshift__(NextTrigger()); // The essence of this is to point the Forward next to the next Trigger
            // Result is a: 10 b: 6
        }
        ```
        The above code uses only 1 additional trigger to complete an increment operation on b and two increment assignments on a based on the increment of b.  
        For such scenarios, eudplib specifically provides the VProc function, which contains a RawTrigger. After this RawTrigger is executed, it executes the virtual trigger (the variable is also a trigger) of the specified variable to ensure that after the current RawTrigger changes the virtual trigger of the variable, each The changed virtual trigger of the variable can be executed one by one without having to write back jump code. The above code can be simplified to:  
        ```JavaScript
        function afterTriggerExec() {
            var a, b = 3, 5;
            VProc(
                list(b), // After the actions in the following list are executed, it will automatically jump to b.GetVTable(), the trigger. After it is executed, it will jump to NextTrigger().
                list(
                    SetMemory(b.getValueAddr(), Add, 1),
                    SetMemory(a.getValueAddr(), Add, 1),
                    SetMemory(b.getDestAddr(), SetTo, EPD(a.getValueAddr())),
                    b.SetModifier(Add), // Internally it is probably implemented as SetMemoryX(b.getValueAddr() + 4, SetTo, (Add << 24), 0xFF000000)
                ),
            );
            // Result is a: 10 b: 6
        }
        ```

        The above code can be further simplified, because eudplib also provides several integrated operation methods. Directly code:

        ```JavaScript
        function afterTriggerExec() {
            var a, b = 3, 5;
            VProc(
                list(b), // After the actions in the following list are executed, it will automatically jump to b.GetVTable(), the trigger. After it is executed, it will jump to NextTrigger()
                list(
                    b.AddNumber(1),
                    a.AddNumber(1),
                    b.QueueAddTo(a),
                ),
            );
            // Result is a: 10 b: 6
        }
        ```
    <br />

- ## Strings (Db Or StringBuffer), Light Arrays (EUDArray) And Light Variables (EUDLightVariable)  

    The strings in the map will be stored in the STR section. Usually these strings are immutable, but EUD is different. Let's not consider the data structure of the STR section for now. It can probably use a very large memory space, which is usually enough.  

    - ### Structure

        Compared with ordinary variables (EUDVariable), the structure of strings is very simple and crude.  
        Strings are arrays of ASCII characters, and the number of bytes they occupy is the number of ASCII characters they contain.  
        Similar to strings, light variables also do not have a complex structure. They only occupy 4 consecutive bytes, representing a 32-bit integer.  
        Light arrays are arrays of multiple light variables, occupying consecutive `ArraySize * 4` bytes.  

    - ### Memory Reading or Copying

        The SetDeathsX action can easily change the value of each byte in a string, similar to ordinary variable operations.  
        However, reading or passing the value of a string is rather troublesome, because there is no condition or action that can read or copy a value from a memory location (the value passing of EUDVariable does not depend on memory reading).  
        The only available option is the DeathsX condition to judge whether the value at a memory location is greater than, less than or equal to a certain value.  
        We can think about how to assign the death count of Marines to Zerglings in classical triggers.  

        Assign the death count of Marines to Zerglings in classical triggers (TrigEdit++ code)  
        ```Lua
        -- First use a trigger to set the death counts of Zerglings and Kakarus to 0
        Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeaths(P1, SetTo, 0, "Zerg Zergling");
                SetDeaths(P1, SetTo, 0, "Kakaru");
            };
        }

        -- Use 32 triggers to transfer the death count of Marines to Zerglings and Kakarus (Marines decrease by how much, Zerglings and Kakarus increase by that much)
        for i = 31, 0, -1 do
            Trigger {
                conditions = {Deaths(P1, AtLeast, 2^i, "Terran Marine");}; -- Judge whether the number of Marines is greater than or equal to 2 to the power of 31 to 0
                actions = {
                    SetDeaths(P1, Add, 2^i, "Zerg Zergling");      -- If the condition is met, add 2 to the power of 31 to 0 to the number of Zerglings
                    SetDeaths(P1, Add, 2^i, "Kakaru");             -- The number of Kakarus is synchronized with the Zerglings
                    SetDeaths(P1, Subtract, 2^i, "Terran Marine"); -- At the same time, subtract this number from the number of Marines
                };
            }
        end

        -- Use 32 triggers to decrement the number of Kakarus to Marines 
        for i = 31, 0, -1 do
            Trigger {
                conditions = {Deaths(P1, AtLeast, 2^i, "Kakaru");};
                actions = {
                    SetDeaths(P1, Add, 2^i, "Terran Marine");
                    SetDeaths(P1, Add, 2^i, "Kakaru");
                };
            }
        end
        ```

        This assignment operation uses 65 triggers and an intermediate unit type Kakaru to complete:  
        1. Use a trigger to reset the death counts of Zerglings and the auxiliary Kakarus to 0  
        2. Use 32 triggers to transfer the death count of Marines to Zerglings and Kakarus in a binary decremented manner  
        3. Use 32 triggers again to transfer the death count of Kakarus back to Marines in a binary incremented manner.  
        

        In the StarCraft Remastered, Deaths supports bitmask (usually called DeathsX).  
        Use the DeathsX condition and SetDeaths action to assign the death count of Marines to Zerglings (TrigEdit++ code)  
        ```Lua
        -- First set the death count of Zerglings to 0 
        Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeaths(P1, SetTo, 0, "Zerg Zergling");
            };
        }

        -- Add each bit of the Marine's count to the corresponding bit of the Zergling's death count
        for i = 0, 31 do
            Trigger {
                conditions = {DeathsX(P1, AtLeast, 1, "Terran Marine", 2^i);};
                actions = {
                    SetDeaths(P1, Add, 2^i, "Zerg Zergling");
                };
            }
        end
        ```

        This assignment operation uses 33 triggers to complete:  
        1. Use a trigger to reset the Zergling death count to 0  
        2. Use 32 triggers to add the corresponding binary bits of the Marine's death count to the Zergling's death count based on binary bit comparison.    

        Above we used classic triggers to achieve the passing of unit death count values.   

        Because the unit death count is essentially a 32-bit integer, and EUD technology allows us to access data other than the unit death count using Deaths or SetDeaths, we can also use this method to read or copy the values of other memory locations.  

        Simulate an implementation of dwread_epd using epScript ([dwread_epd](https://github.com/armoha/eudplib/blob/master/eudplib/eudlib/memiof/dwepdio.py#L47))

        ```JavaScript
        // This function already exists in epScript, this code is only for demonstrating the principle of 32-bit integer division, and does not handle any boundary issues
        function my_dwread_epd(playerid) {
            var ret = 0;
            foreach(i : py_range(32)) {
                Trigger(
                    conditions = DeathsX(playerid, AtLeast, 1, 0, py_pow(2, i)),
                    actions = ret.AddNumber(py_pow(2, i)),
                );
            }
            return ret;
        }
        ```

        So we used 32 triggers to simulate reading the dword value at the specified epd location.  
        Its implementation details also involve issues such as whether the memory address is a multiple of 4.  
        For example, we know that the memory address corresponding to player ID 5004 is 0x6557E0, and the memory address corresponding to player ID 5005 is 0x6557E4.   

        |Player ID|...|5003|5004|5005|5006|...|
        |:-:|:-:|:-:|:-:|:-:|:-:|:-:|
        |Memory Address|...|0x6557DC|0x6557E0|0x6557E4|0x6557E8|...|
        |Memory Value|...|11223344|5566`7788`|`99AA`BBCC|DDEEFF00|...|

        To read the dword at memory address 0x6557E2, the only parameter Deaths can accept is the player ID. Here, assume its value is 0xAA998877 (why is it reversed? Refer to [Little Endian](https://en.wikipedia.org/wiki/Endianness#Little_endian))   
        In this case, you need to read the latter half of 5004 and the first half of 5005.   
        Secondly, there are more implementation details for reading and copying memory data less than 4 bytes.   
        Of course, here we only explain the principle. For more details, you can read the source code of eudplib directly.   
