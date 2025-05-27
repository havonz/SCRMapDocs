# GameSpeedTextMenu

[Download Demo]([MSQC]GameSpeedTextMenu.zip)

## makefile.edd
```ini
[main]
input: GameSpeedTextMenu-Terrain.scx
output: GameSpeedTextMenu.scx

[main.eps]

[eudTurbo]

[MSQC]
QCDebug : 0
QCUnit : 218
0x6CDDC4, AtLeast, 10;0x6CDDC4, AtMost, 25;0x6CDDC8, AtLeast, 129;0x6CDDC8, AtMost, 139;MouseDown(L): menuSel, 50
0x6CDDC4, AtLeast, 10;0x6CDDC4, AtMost, 25;0x6CDDC8, AtLeast, 145;0x6CDDC8, AtMost, 155;MouseDown(L): menuSel, 100
0x6CDDC4, AtLeast, 10;0x6CDDC4, AtMost, 25;0x6CDDC8, AtLeast, 161;0x6CDDC8, AtMost, 171;MouseDown(L): menuSel, 125
0x6CDDC4, AtLeast, 10;0x6CDDC4, AtMost, 25;0x6CDDC8, AtLeast, 177;0x6CDDC8, AtMost, 187;MouseDown(L): menuSel, 150
0x6CDDC4, AtLeast, 10;0x6CDDC4, AtMost, 25;0x6CDDC8, AtLeast, 193;0x6CDDC8, AtMost, 203;MouseDown(L): menuSel, 200
0x6CDDC4, AtLeast, 10;0x6CDDC4, AtMost, 25;0x6CDDC8, AtLeast, 209;0x6CDDC8, AtMost, 219;MouseDown(L): menuSel, 400
0x6CDDC4, AtLeast, 10;0x6CDDC4, AtMost, 25;0x6CDDC8, AtLeast, 225;0x6CDDC8, AtMost, 235;MouseDown(L): menuSel, 4200
```

## main.eps
```Javascript
// EUDDB: https://armoha.github.io/eud-book/

/* Set the game speed percentage function, with the normal speed of Fastest (level: 6) as 100% */ 
function SetGameSpeed(level, speed) {
    const mspf = 1000000 / (10000 / 42 * speed);
    dwwrite_epd(EPD(0x5124D8) + level, mspf);
}

const MOUSE_X_EPD, MOUSE_Y_EPD = EPD(0x6CDDC4), EPD(0x6CDDC8);
const menuSel = PVariable();
var currentSpeedSel;

/* Register the menuSel global variable to MSQC */ 
EUDRegisterObjectToNamespace("menuSel", menuSel);

function showMenu(p) {
    const speedList = 50, 100, 125, 150, 200, 400, 4200;
    const isMouseOvered = EUDLightBool(), EUDLightBool(), EUDLightBool(), EUDLightBool(), EUDLightBool(), EUDLightBool(), EUDLightBool();
    if (getcurpl() == p) {
        const buf = StringBuffer(255);
        buf.printfAt(0, "\x02Currently selected game speed: \x07{}\x02%\x02", currentSpeedSel);
        foreach(i : py_range(0, 7)) {
            buf.insert(0);
            if (speedList[i] == currentSpeedSel) {
                buf.append("\x03[\x07x\x03]\x1E ");
            } else {
                buf.append("\x03[  ]\x02");
            }
            buf.appendf(" Game Speed {}%\x02", speedList[i]);
            buf.DisplayAt(i + 1);
        }
        if (MemoryEPD(MOUSE_X_EPD, AtLeast, 10) && MemoryEPD(MOUSE_X_EPD, AtMost, 25)) { /* The menu option X coordinates triggers the range */
            foreach(i : py_range(0, 7)) {
                if (MemoryEPD(MOUSE_Y_EPD, AtLeast, 129 + i * 16) && MemoryEPD(MOUSE_Y_EPD, AtMost, 139 + i * 16)) { /* The menu option Y coordinates triggers the range */
                    if (speedList[i] != currentSpeedSel) {
                        buf.insert(0);
                        buf.append("\x07[  ]\x1E ");
                        buf.appendf(" Game Speed {}%\x02", speedList[i]);
                        buf.DisplayAt(i + 1);
                        RawTrigger(
                            conditions = isMouseOvered[i].IsCleared(),
                            actions = list(
                                PlayWAV("sound\\glue\\mouseover.wav"),
                                isMouseOvered[i].Set(),
                            ),
                        );
                    }
                } else {
                    RawTrigger(actions = isMouseOvered[i].Clear());
                }
            }
        } else {
            const clearActions = py_list();
            foreach(v : isMouseOvered) {
                clearActions.append(v.Clear());
            }
            RawTrigger(actions = clearActions);
        }
    }
}

function onPluginStart() {
    /* Set the Fastest game speed to 100% */
    currentSpeedSel = 100;
    SetGameSpeed(6, currentSpeedSel);
}

function beforeTriggerExec() {
    const cp = getcurpl();

    RawTrigger(actions = list(
        CreateUnitWithProperties(1, "Zerg Overlord", "Location 1", P1, UnitProperty(invincible = true)),
        CreateUnitWithProperties(1, "Zerg Overlord", "Location 1", P2, UnitProperty(invincible = true)),
    ), preserved = false);

    /* Show the menu to all human players */
    foreach(p : EUDLoopPlayer("Human")) {
        setcurpl(p);
        showMenu(p);
    }

    setcurpl(cp);
}

function afterTriggerExec() {
    /* Receive MSQC selection and sync to currentSpeedSel, this loop will execute on each player's machine */ 
    foreach(p : EUDLoopPlayer("Human")) {
        if (menuSel[p] != 0) { /* If player p's menuSel has a value. */ 
            currentSpeedSel = menuSel[p]; /* Receive it */
            menuSel[p] = 0;               /* Clear it and wait for next reception */

            /* The following is operation on the local machine */
            SetGameSpeed(6, currentSpeedSel);
            setcurpl(getuserplayerid());
            
            if (p == getuserplayerid()) { /* If the local player happens to be the player who operated the menu */
                PlayWAV("sound\\glue\\mousedown2.wav");
                printAt(10, "You change the game speed to \x07{}\x02%", currentSpeedSel);
            } else {
                PlayWAV("sound\\misc\\transmission.wav");
                printAt(10, "{}{} \x02changes the game speed to \x07{}\x02%", PColor(p), PName(p), currentSpeedSel);
            }
        }
    }
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
Then double-click "build.bat" to compile the code and synthesize it with "GameTextMenu-Terrain.scx" into a new map file "GameTextMenu.scx".

makefile.edd
    Is the project configuration file

main.eps
    Is the code file 

GameTextMenu-Terrain.scx
    Is the original terrain file, this file can be opened and edited with SCMD 

GameTextMenu.scx
    This is the final output map file, which can be placed in the game's map file directory ([StarCraft installation or document path]\Maps\) to see the actual effect of the code in the game. It can no longer be directly opened and edited with SCMD.

Demo from: https://github.com/havonz/SCRMapDocs
```