---
sidebar_position: 5
---

# epScript Reference

<br />

- Language Reference
    - [Syntax](Syntax.md)  
    - [Use Of Variables](Use-of-Variables.md)  
    - [Use Of Functions](Use-of-Functions.md)  
    - [Use Of Objects](Use-of-Objects.md)  
    - [Understanding Strings](Understanding-Strings.md)  
    - [Built-in Object Types](Built-in-Object-Types.md)  
    - [Built-in Object Types Ext](Built-in-Object-Types-Ext.md)  
    - [Constants Reference](Constants-Reference/Constants-Reference.md)  
    - [Built-in Functions](Built-in-Functions.md) 
- Description
    - [Getting Started](#getting-started)
        - [Environment Preparation](#environment-preparation)
        - [Map Preparation](#map-preparation)
        - [New Project](#new-project)
        - [Example Projects](#example-projects)
    - [Running Mode](#running-mode)
        - [Script File Extension Differences](#script-file-extension-differences)
        - [Load Order](#load-order)
    - [The Difference Between .edd And .eds](#the-difference-between-edd-and-eds)
        - [For .edd Format](#for-edd-format)
        - [For .eds Format](#for-eds-format)
    - [Data Synchronization](#data-synchronization)
    - [Game Time](#game-time)
        - [Game Frame](#game-frame-fr)
        - [Game Seconds](#game-seconds)
        - [Game Speed](#game-speed)
        - [Triggers Poll Interval](#triggers-poll-interval)
    - [Current Player And Local Player](#current-player-and-local-player)
        - [Current Player](#current-player)
        - [Local Player](#local-player)

<br />

## Getting Started 
If there are any parts you don't understand, you can try searching the Internet to solve them.  

Here we assume you already know how to use ScmDraft2 for basic terrain design.  
- If not, refer to: [SCMD](http://www.stormcoast-fortress.net/Irregularies/#downloads)   

Here we assume you already have a basic understanding of EUD.   
- If not, refer to: [What is EUD](What-is-EUD.md)  

### Environment Preparation

Prepare a Windows 10 or higher PC, or a virtual machine.  
Prepare ScmDraft2. If not already prepared, look back a few lines.  

- Download [euddraft0.9.9.9.zip](https://github.com/armoha/euddraft/releases/download/v0.9.9.9/euddraft0.9.9.9.zip)   
    Unpack euddraft to a path with only English letters and no spaces, e.g. D:\SCRMapDevTools\euddraft0.9.9.9 
- Download [VSCode](https://code.visualstudio.com/Download)  
    Install it, install it however you like.   
    Install the eps-server plugin from the VSCode plugin store.  

- Open file extension display in your operating system.   
    For Windows 10, refer to [https://www.google.com/search?q=Open-file-extension-display-in-windows-10](https://www.google.com/search?q=Open+file+extension+display+in+windows+10)   
    For Windows 11, refer to [https://www.google.com/search?q=Open-file-extension-display-in-windows-11](https://www.google.com/search?q=Open+file+extension+display+in+windows+11)  


### Map Preparation  

Prepare a normal map file, you can create a new one with ScmDraft2 and then save it as Starcraft: Remastered Broodwar Map (*.scx) format.   
Also save it to a path with only English letters and no spaces, e.g. D:\Projects\test\basemap.scx.  


### New Project
1. Create a new text document and change its extension to edd, e.g. D:\Projects\test\test.edd  
    Open this edd file with VSCode (just drag the file into the open VSCode window).  
Then change its content to:  
    ```MakeFile
    [main]
    input: basemap.scx
    output: test.scx

    [main.eps]
    ```
    The above code uses relative paths as an example, it actually supports absolute paths.   

2. Create a new text document and change its extension to eps, e.g. D:\Projects\test\main.eps  
    Open this eps file with VSCode.  
    Then change its content to:  
    ```JavaScript
    function onPluginStart() { // This function will be executed once when the game starts
        DisplayTextAll("Hello World");
    }

    function beforeTriggerExec() { // This function will be executed once each frame before classical triggers

    }

    function afterTriggerExec() { // This function will be executed once per frame after classical triggers

    }
    ```

3. Create a new text document and change its extension to bat, e.g. D:\Projects\test\build.bat  
    Open this bat file with VSCode and change its content to:  
    ```PowerShell
    D:\SCRMapDevTools\euddraft0.9.9.9\euddraft.exe test.edd
    ```
    The above code assumes you unpacked euddraft to D:\SCRMapDevTools\euddraft0.9.9.9. If not, you should replace it.  

    Now the project is ready. Just double click to run build.bat to generate test.scx. Put this map in the map directory of StarCraft: Remastered. Then when you enter the game, you will see `Hello World` output on the screen.


### Example Projects

- If you really don't understand the configuration process above, you can choose a simple example project to view:  
    - [Trigger-and-RawTrigger](../Example/Trigger-and-RawTrigger/README.md)
    - [ChangeSupplyLimit](../Example/ChangeSupplyLimit/README.md)
    - [UsePosition](../Example/UsePosition/README.md)
    - [\[MSQC\]GameSpeedTextMenu](../Example/%5BMSQC%5DGameSpeedTextMenu/README.md)  

<br /><br />



## Running Mode

### Script File Extension Differences
- If it is a `.py` format script, the extension name can be omitted in the .eds/.edd file. `.eps` format scripts need to add the extension name.  

    ```ini
    [main]
    input: basemap.scx
    output: outputmap.scx

    [eudTurbo]
    :: It actually loads a script named eudTurbo.py

    [main.eps]
    :: Load it like this if it is .eps format
    ```

### Load Order

- The order of plugin names in the configuration file is associated with their loading order after the game starts. After the game starts, onPluginStart() in the script will be executed once, and beforeTriggerExec(), triggers, and afterTriggerExec() will be executed cyclically on all players' machines.  

    For example, with the following main.edd configuration:  

    ```ini
    [main]
    input: in.scx
    output: out.scx

    [eudTurbo]
    [a.eps]
    [b.eps]

    ```

    The execution order after the game starts is:  

    ```PowerShell
    eudTurbo.onPluginStart()
    a.onPluginStart()
    b.onPluginStart()
    Executed cyclically every frame:
        eudTurbo.beforeTriggerExec()
        a.beforeTriggerExec()
        b.beforeTriggerExec()
        SCMD triggers
        b.afterTriggerExec()
        a.afterTriggerExec()
        eudTurbo.afterTriggerExec()
    ```
    <br />

## The Difference Between .edd And .eds

euddraft handles these two extensions differently.

- ### For .edd Format  

    After successfully compiling and generating the map, it will keep waiting. If the files in the project directory change, it will automatically recompile and generate the map again.  
    If unsuccessful, it will output error messages. You can press R to recompile and generate after modifying.  

- ### For .eds Format  

    After successfully compiling and generating the map, it will exit.   
    If unsuccessful, it will output error messages and wait for you to press Enter to exit.  <br /><br />


## Data Synchronization

If you assign `desync-data` (such as the player's current mouse position) to a variable, the value of the variable will be different for each player's machine. If you execute an action that requires `sync-data` (such as creating a unit) based on the state of that variable, it may cause the `sync-data` to be out of sync for players in multiplayer games (e.g. a unit is created on player A's machine but not on player B's machine), leading to a drop.  

Such tasks can usually be assisted by the MSQC plugin.  

<!-- [Example: GameSpeedTextMenu](res/%5BMSQC%5DGameSpeedTextMenu/)   -->
<br /><br />



## Game Time 

The game time in StarCraft 1 is different from real time.  

- ### Game Frame (fr) 

    The minimum unit of game time in StarCraft is the game frame (fr):  
    `1 fr` == `1/16 game seconds`

- ### Game Seconds 

    The formula for converting game seconds and game frames in StarCraft:  
    `1 game seconds` == `16 fr`

- ### Game Speed 
    StarCraft has seven standard game speeds.  
    At different game speeds, the system time represented by each game frame is different.  
    System time is usually equal to real world time.  
    <details>
    
    <summary>For each game frame, the corresponding system time (accurate value, not approximate) with no network latency</summary>

    ```JavaScript
    Slowest: 1 fr == 0.167 system seconds  
    Slower : 1 fr == 0.111 system seconds
    Slow   : 1 fr == 0.083 system seconds 
    Normal : 1 fr == 0.067 system seconds
    Fast   : 1 fr == 0.056 system seconds   
    Faster : 1 fr == 0.048 system seconds
    Fastest: 1 fr == 0.042 system seconds
    ```
    </details>

    So at the Fastest game speed, 1 game second is `0.042 × 16 = 0.672` real seconds, and 1 system second is `1 ÷ 0.042 ÷ 16 ≈ 1.488` game seconds.  

- ### Triggers Poll Interval

    Triggers in StarCraft are single-threaded polling.  
    Without using eudTurbo and Wait actions, the trigger polling interval is `31 fr`, which is `1.9375 game seconds`.  
    The first poll after the game starts is at `2 fr`, which is `0.125` game seconds.  

    <details>
    
    <summary>Trigger polling times after the game starts</summary>

    ```JavaScript
    First   poll at   2 game frames,  0.1250 game seconds
    Second  poll at  33 game frames,  2.0625 game seconds
    Third   poll at  64 game frames,  4.0000 game seconds
    Fourth  poll at  95 game frames,  5.9375 game seconds
    Fifth   poll at 126 game frames,  7.8750 game seconds
    Sixth   poll at 157 game frames,  9.8125 game seconds
    Seventh poll at 188 game frames, 11.7500 game seconds
    Eighth  poll at 219 game frames, 13.6875 game seconds
    Ninth   poll at 250 game frames, 15.6250 game seconds
                    ...And so on...
    ```
    </details>
    
    The game seconds comparsion in the ElapsedTime condition parameter takes the integer part.
    ```JavaScript
    function beforeTriggerExec() {
        if (ElapsedTime(Exactly, 6)) {
            DisplayTextAll("This message will not output");
        }
    }
    ```
    So this condition will not be met, because the fourth poll is at 5.9375 game seconds with an integer part of 5, and the fifth poll is at 7.8750 game seconds with an integer part of 7, so ElapsedTime(Exactly, 6) will never be true.  
    If you want to execute an action once after 6 game seconds, you can write like this:  
    ```JavaScript
    function beforeTriggerExec() {
        once (ElapsedTime(AtLeast, 6)) {
            DisplayTextAll("This message will output once after 6 game seconds");
        }
    }
    ```
    Similarly, the CountdownTimer condition also takes the integer part of the countdown at the top of the screen.     
    Therefore, when writing conditions related to time comparsion, Exactly (`==`) should not be used, but AtLeast (`>=`) or AtMost (`<=`) should be used.  <br /><br />


## Current Player And Local Player

`Current Player` and `Local Player` are two different concepts.

### Current Player 

The `Current Player` is a global variable. In some trigger actions, `Current Player` is used as an execution parameter.  
Some trigger conditions and actions support passing a `Player` parameter, then, you can set the `Player` parameter to `13` to use the `Current Player` global variable as its parameter.  
The value of the `Current Player` global variable does not necessarily have to be any player's ID, it can store any integer value.  

<details>
    
    <summary>Actions that only take effect on machines where Current Player == Local Player (allow desync use, can be used individually on some player machines)</summary>

- DisplayText  
- CenterView  
- PlayWAV  
- MinimapPing  
- TalkingPortrait  
- Transmission  
- SetMissionObjectives  
</details>

<details>
    
    <summary>Actions that only take effect on Current Player (must be used synchronously on all player machines, otherwise disconnected)</summary>

- SetAllianceStatus  
- RunAIScript  
- RunAIScriptAt  
- Draw  
- Defeat  
- Victory  
</details>

The `setcurpl` function can be used to set the value of the `Current Player` global variable.  
The `getcurpl` function can be used to get the current value of the `Current Player` global variable.  
No matter what value you set for the `Current Player`, the code will execute on all players' machines.  

```PHP
setcurpl(P1);
DisplayText("Printed content for player 1");
setcurpl(P2);
DisplayText("Printed content for player 2");
setcurpl(P3);
DisplayText("Printed content for player 3");

// $CurrentPlayer is the constant number 13. It can cause some player-related conditions or actions to access the current player value  
// $CurrentPlayer != getcurpl()  
if ($CurrentPlayer == 13) {
    DisplayTextAll("Well, right");
}

// Set Fastest game speed x2
setcurpl(-122787 + 6);
SetDeaths($CurrentPlayer, SetTo, 21, 0);
```

### Local Player 

getuserplayerid() can be used to get the local player ID. It returns a different value for each machine and is unrelated to the value set by setcurpl.   
The ability to use getuserplayerid() to get the local player ID means you can decide at runtime whether or not to execute certain code on the local machine.   
It helps improve performance. When there are many players, not all code needs to execute for each player, e.g. no need to generate text prompts for all players for each player.  
Of course, if you pollute sync-data directly or indirectly due to unfamiliarity with synchronization rules using getuserplayerid(), it can also lead to data synchronization causing dropped

```JavaScript
setcurpl(P1);
println("Current player ID: {}", getuserplayerid());
setcurpl(P2);
println("Current player ID: {}", getuserplayerid());
setcurpl(P3);
println("Current player ID: {}", getuserplayerid());
```

    

  

