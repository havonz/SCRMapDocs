# Trigger-and-RawTrigger

[Download Demo](Trigger-and-RawTrigger.zip)

## makefile.edd
```ini
[main]
input: Trigger-and-RawTrigger-Terrain.scx
output: Trigger-and-RawTrigger.scx

[eudTurbo]
[main.eps]
```

## main.eps
```Javascript
var NextWaveTime = 3;

function doTriggerList() {
    // Set up an unconditional trigger that only triggers once 
    DoActions(CreateUnit(1, "Terran SCV", "Location 2", P1), preserved = false,);

    // Set up an unconditional trigger that only triggers once 
    DoActions(CreateUnit(1, "Zerg Zergling", "Location 1", P1), preserved = false,);

    // Set up a trigger that executes every 3 game seconds 
    Trigger(
        conditions = list(
            ElapsedTime(AtLeast, NextWaveTime), // This condition uses the variable NextWaveTime so only Trigger can be used instead of RawTrigger 
        ),
        actions = list(
            GiveUnits(1, "Zerg Zergling", P1, $L("Location 1"), P12),
            Order("Zerg Zergling", P12, $L("Location 1"), Move, $L("Location 3")),
            // Give NextWaveTime + 3 to trigger again 3 seconds after this trigger 
            NextWaveTime.AddNumber(3),
            CreateUnit(1, "Zerg Zergling", "Location 1", P1),
        ),
    );

     // Set up a trigger that kills player 12's zerglings when entering Location 3 
    RawTrigger(
        conditions = list(
            Bring(P12, AtLeast, 1, "Zerg Zergling", $L("Location 3")),
        ),
        actions = list(
            KillUnitAt(10, "Zerg Zergling", "Location 3", P12),
        ),
    );

    // Set up a trigger that turns 1 of player 1's zerglings into 10 when entering Location 3, only triggers once 
    RawTrigger(
        conditions = list(
            Bring(P1, AtLeast, 1, "Zerg Zergling", $L("Location 3")),
        ),
        actions = list(
            CreateUnit(9, "Zerg Zergling", "Location 3", P1),
        ),
        preserved = false,
    );
}


function onPluginStart() {

}

function beforeTriggerExec() {
    const cp = getcurpl();
    
    // Other code written here 

    setcurpl(cp);

    doTriggerList(); // Execute triggers
}

function afterTriggerExec() {

}
```

## build.bat
```PowerShell
@copy makefile.edd makefile.eds
@C:\Users\havonz\Applications\euddraft0.9.9.9\euddraft.exe makefile.eds
@del /f /q makefile.eds
@pause
```

## readme.txt
```
Right-click to edit the "build.bat" file and change the path of euddraft.exe in it to the path of euddraft.exe on your own computer.
Then double-click "build.bat" to compile the code and synthesize it with "Trigger-and-RawTrigger-Terrain.scx" into a new map file "Trigger-and-RawTrigger.scx".

makefile.edd
    Is the project configuration file

main.eps
    Is the code file 

Trigger-and-RawTrigger-Terrain.scx
    Is the original terrain file, this file can be opened and edited with SCMD 

Trigger-and-RawTrigger.scx
    This is the final output map file, which can be placed in the game's map file directory ([StarCraft installation or document path]\Maps\) to see the actual effect of the code in the game. It can no longer be directly opened and edited with SCMD.

Demo from: https://github.com/havonz/SCRMapDocs
```