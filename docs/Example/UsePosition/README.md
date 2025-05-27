# UsePosition

[Download Demo](UsePosition.zip)

## makefile.edd
```ini
[main]
input: UsePosition-Terrain.scx
output: UsePosition.scx

[eudTurbo]

[main.eps]
```

## main.eps
```Javascript
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

function distanceBetween(x1, y1, x2, y2) {
    const x = x2 - x1;
    const y = y2 - y1;
    return sqrt(x*x + y*y);
}

function polarProjection_256(x0, y0, length, angle) {
    var dx, dy;
    if (_0998_above()) {
        dx, dy = lengthdir_256(length, angle);
        return x0 + dx, y0 + dy;
    } else {
        dx, dy = lengthdir_256(length, 320 - angle);
        return x0 + dx, y0 - dy;
    }
}

var marine_epd, ghost_epd = 0, 0;

function onPluginStart() {

}

function beforeTriggerExec() {
    const cp = getcurpl();

    once (ElapsedTime(AtLeast, 0)) {
        const cp2 = getcurpl();
        setcurpl(EPD(0x628438));
        {
            const ptr, epd = cunitepdread_cp(0);
            CreateUnit(1, "Terran Marine", "Location 1", P1);
            if (ptr != 0) {
                marine_epd = epd;
            }
        }
        {
            const ptr, epd = cunitepdread_cp(0);
            CreateUnit(1, "Terran Ghost", "Location 2", P1);
            if (ptr != 0) {
                ghost_epd = epd;
            }
        }
        setcurpl(cp2);
    }

    if (marine_epd != 0 && ghost_epd != 0) {
        const marine_cu = CUnit(marine_epd);
        const ghost_cu = CUnit(ghost_epd);
        once {
            ghost_cu.set_invincible();
            marine_cu.set_invincible();
        }
        const x0, y0 = marine_cu.getpos("pos");
        const x1, y1 = ghost_cu.getpos("pos");
        const ang = angleBetween_256(x0, y0, x1, y1);
        const dist = distanceBetween(x0, y0, x1, y1);
        const x, y = polarProjection_256(x0, y0, dist, ang);
        setcurpl(P1);
        printAt(0, "The distance from the Machine({},{})(Face:{}) to the Ghost({},{})(Face:{}) is {} , the angle is {}", x0, y0, marine_cu.currentDirection2, x1, y1, ghost_cu.currentDirection2, dist, ang);
        printAt(1, "Walking {} distance {} degrees from the Machine position will reach the Ghost's position at ({}, {})", ang, dist, x, y);
    }

    setcurpl(cp);
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
Then double-click "build.bat" to compile the code and synthesize it with "UsePosition-Terrain.scx" into a new map file "UsePosition.scx".

makefile.edd
    Is the project configuration file

main.eps
    Is the code file 

UsePosition-Terrain.scx
    Is the original terrain file, this file can be opened and edited with SCMD 

UsePosition.scx
    This is the final output map file, which can be placed in the game's map file directory ([StarCraft installation or document path]\Maps\) to see the actual effect of the code in the game. It can no longer be directly opened and edited with SCMD.

Demo from: https://github.com/havonz/SCRMapDocs
```