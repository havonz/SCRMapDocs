---
sidebar_position: 5
---

# Understanding Strings

<br />

- Strings
    - [Compile-time Strings](#compile-time-strings-py_str)
    - [Map Strings](#map-strings-trgstring)
    - [String Data](#string-data-db)
    - [TBL Strings](#tbl-strings-stat_txttbl)

<br />

- ## Compile-time Strings (py_str)

    Compile-time strings refer to strings generated at compile time that will never be inserted into the map. They contain all literal strings and compile-time string constants.   
    Because the epScript compiler is implemented in Python, it is called py_str. Compile-time strings cannot be used directly for map runtime.  

    <details>
    
    <summary>When a function parameter is one of the following types, epScript will convert the incoming compile-time string argument into the corresponding constant number</summary>

    - [TrgUnit](Constants-Reference/TrgUnit.md)  
    - [TrgLocation](Constants-Reference/Constants-Reference.md#trglocation)  
    - [TrgSwitch](Constants-Reference/Constants-Reference.md#trgswitch)  
    - [TrgAIScript](Constants-Reference/TrgAIScript.md)  
    - [Weapon](Constants-Reference/Weapon.md)  
    - [Tech](Constants-Reference/Tech.md)  
    - [Upgrade](Constants-Reference/Upgrade.md)  
    - [UnitOrder](Constants-Reference/UnitOrder.md)  
    - [Flingy](Constants-Reference/Flingy.md)  
    - [Image](Constants-Reference/Image.md)  
    - [Icon](Constants-Reference/Icon.md)  
    - [Iscript](Constants-Reference/Iscript.md)  
    - [Portrait](Constants-Reference/Portrait.md)  
    - [Sprites](Constants-Reference/Sprites.md)  
    - [StatText](Constants-Reference/StatText.md)  
    </details>

    Here is an example to illustrate:

    ```JavaScript
    SetDeaths(P1, Add, 10, "Terran Marine"); // The corresponding parameter type of the SetDeaths action is TrgUnit, so it will be replaced with the integer ID in the TrgUnit table at compile time 
    ```

    The string "Terran Marine" in the above code will not be inserted into the map. 

    "Terran Marine" is only used at compile time to query the ID of the unit type (the ID of the Marine is 0).

    If human readability is not considered, the above code is completely equivalent to the following code:

    ```JavaScript
    SetDeaths(P1, Add, 10, 0);
    ```

    In epScript, you can also use py_str to declare a compile-time string constant.

    ```JavaScript
    const csTerran_  = py_str("Terran ");
    const csMarine   = py_str("Marine");
    const csGoliath  = py_str("Goliath");
    py_print(csMarine, " ", csGoliath); // Output Marine Goliath in the compile-time CLI
    SetDeaths(P1, Add, 10, csTerran_ + csGoliath);
    ```

     At compile time, you can get the ID of the compile-time string in different constant tables through the macro index.

    ```JavaScript
    const utTerranMarine = $U("Terran Marine");
    const stTerranMarine = $B("Terran Marine");
    py_print(utTerranMarine, stTerranMarine); // Output 0 1 in the compile-time CLI
    if (utTerranMarine == stTerranMarine) {   // This is equivalent to if (0 == 1) {
        simpleprint("So this message is never printed");
    }
    ```

    You can use the EncodeString macro to insert a compile-time string into the map as a map string (TrgString) and return its ID in the map string table.

    ```JavaScript
    const csTerran_  = py_str("Terran ");
    const csMarine   = py_str("Marine");
    const csTerran_Marine  = csTerran_ + csMarine;
    const str_idx = EncodeString(csTerran_Marine * 3);
    DisplayText(str_idx); // Output: Terran MarineTerran MarineTerran Marine
    ```



- ## Map Strings (TrgString)

    <details>
    
    <summary>Related types and functions</summary>

    - TrgString
    - StringBuffer
    - $T(cstr : [literal](Syntax.md#literal-strings))
    - EncodeString(cstr: py_str)
    - GetStringIndex(cstr: py_str)
    - GetMapStringAddr(str: TrgString)
    </details>
    
    Map strings (TrgString) are the most commonly used runtime strings.  
    When we use any custom string in the map editor, the map editor will insert this string into the map string table in the map file and assign it an ID.   
    For example, if we set the Map Title to "An interesting StarCraft map", this map title is a map string (TrgString) and is usually assigned the ID 1.  
    The string parameters of trigger action functions are this ID.  

    ```JavaScript
    DisplayText(1);          // Output: An interesting StarCraft map 
    SetMissionObjectives(1); // Mission objectives: An interesting StarCraft map
    ```

    In epScript code, if a compile-time string is passed as an argument where the function parameter type is TrgString, this string will first be inserted into the map string table of the map, and then its ID will be passed to that argument position.

    ```JavaScript
    DisplayText("Hello StarCraft");
    ```

    The above code is actually equivalent to:

    ```JavaScript
    const str_idx = EncodeString("Hello StarCraft");
    DisplayText(str_idx);
    ```

    You can also write it using $T syntax:

    ```JavaScript
    const str_idx = $T("Hello StarCraft");
    DisplayText(str_idx);
    ```

    Here is an example of a custom function that accepts map string parameters:

    ```JavaScript
    function test(strId : TrgString) {
        println("String ID:{}", strId);
        DisplayText(strId);
    }

    function onPluginStart() {
        test("Hello StarCraft");
        // Outputs:
        // String ID:3 
        // Hello StarCraft
    }
    ```

    You can also construct a very long compile-time empty string using the EncodeString macro to convert it into a map string (TrgString) to act as a string buffer like a string variable.

    ```JavaScript
    const buf_idx = EncodeString(py_str(" ") * 128);
    const buf_addr = GetMapStringAddr(buf_idx);
    for (var i = 1; i < 10; i++) {
        dbstr_print(buf_addr, "Line text ", i);
        DisplayText(buf_idx);
    }
    ```

    Of course, we do not need to build and maintain map strings (TrgString) ourselves as string buffers. Using the StringBuffer object can more easily dynamically manage a map string (TrgString) of a specific length.

    ```JavaScript
    const buf = StringBuffer(127);
    for (var i = 1; i < 10; i++) {
        buf.insert(0, "Line text ", i);
        buf.Display(); // DisplayText(buf.StringIndex);
    }
    ```

    More conveniently, epScript provides a series of print functions so that outputting text to the screen does not require maintaining StringBuffer yourself. 

    epScript maintains a GlobalStringBuffer with a capacity of 1023 bytes to implement the print function series. That is to say, strings less than this length can be output directly using the print function series.

    ```JavaScript
    for (var i = 1; i < 10; i++) {
        simpleprint("Line text ", i);
    }
    ```



- ## String Data (Db)
    <details>
    
    <summary>Related types and functions</summary>

    - Db  
    - dbstr_addstr(dst, src)  
    - dbstr_addstr_epd(dst, srcepd)  
    - dbstr_adddw(dst, number)  
    - dbstr_addptr(dst, ptr)  
    - dbstr_print(dst, *args)  
    - sprintf(dst, format_string, *args)  
    </details>

    Db strings are also a kind of runtime custom string.  
    However, unlike map strings (TrgString), they do not have an ID, so they cannot be directly applied to classical trigger actions.  
    Usually, some runtime string components need to be stored using Db. They do not necessarily each have an ID.  
    They can be copied to StringBuffer or other TrgStrings to pass to classical trigger actions.  

    ```JavaScript
    const buf = Db(128);
    const str = StringBuffer(1024);
    str.insert(0);
    for (var i = 1; i < 10; i++) {
        dbstr_print(buf, "Mission objective: Line text ", i);
        if (i < 9)
            str.append(buf, "\n");
        else
            str.append(buf);
    }
    str.Display();
    SetMissionObjectives(str.StringIndex);
    ```



- ## TBL Strings (stat_txt.tbl)
    <details>
    
    <summary>Related types and functions</summary>
    
    - $B([TBLKey](Constants-Reference/StatText.md) : [literal](Syntax.md#literal-strings))
    - EncodeTBL([TBLKey](Constants-Reference/StatText.md) : py_str)
    - GetTBLAddr([TBLKey](Constants-Reference/StatText.md))
    - settbl([TBLKey](Constants-Reference/StatText.md), offset, *args)
    - settbl2([TBLKey](Constants-Reference/StatText.md), offset, *args)
    </details>

    TBL strings refer to strings in Starcraft's internal string table, containing unit names, tech names, ability names, etc.   
    You cannot add strings to the TBL string table. You can find the ID corresponding to all TBLKeys from [StatText](Constants-Reference/StatText.md).  
    The string in memory corresponding to the TBLKey is not equal to the TBLKey itself.  

    ```JavaScript
    println("{:s}", GetTBLAddr("Terran Siege Tank (Tank Mode)")); // Output: Terran Siege Tank
    ```

    You can modify the names of units and abilities by modifying strings in the TBL table.s

    ```JavaScript
    dbstr_print(GetTBLAddr("Terran Marine"), "机枪兵\x00"); // Modifying tbl will cause localization to fail. All unit and ability names that have not been modified will become English. Strongly not recommended to modify tbl.
    ```

    

  



