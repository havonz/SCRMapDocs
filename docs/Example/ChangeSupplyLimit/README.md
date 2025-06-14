# ChangeSupplyLimit

[Download Demo](ChangeSupplyLimit.zip)

## makefile.edd
```ini
[main]
input: ChangeSupplyLimit-Terrain.scx
output: ChangeSupplyLimit.scx

[main.eps]
```

## main.eps
```Javascript
// EUDDB: https://armoha.github.io/eud-book/

const SUP_RACE_ZERG = 0;
const SUP_RACE_TERRAN = 1;
const SUP_RACE_PROTOSS = 2;
const SUP_TYPE_AVAILABLE = 0;
const SUP_TYPE_USED = 1;
const SUP_TYPE_MAX = 2;

function SetPlayerSupply(player: TrgPlayer, race, type, amount) {
    dwwrite_epd(EPD(0x582144) + (race) * 36 + (type) * 12 + (player), amount);
}

function GetPlayerSupply(player: TrgPlayer, race, type) {
    return dwread_epd(EPD(0x582144) + (race) * 36 + (type) * 12 + (player));
}

function SetUnitSupplyProvided(ut : TrgUnit, amount) {
    bwrite(0x6646C8 + ut * 1, amount);
}

function SetUnitSupplyRequired(ut : TrgUnit, amount) {
    bwrite(0x663CE8 + ut * 1, amount);
}

function SetUnitMineralCost(ut : TrgUnit, amount) {
    wwrite(0x663888 + ut * 2, amount);
}

function SetUnitGasCost(ut : TrgUnit, amount) {
    wwrite(0x65FD00 + ut * 2, amount);
}

function SetUnitBuildTime(ut : TrgUnit, frames) {
    wwrite(0x660428 + ut * 2, frames);
}

function SetWeaponCooldown(wt : Weapon, frames) {
    bwrite(0x656FB8 + wt, frames);
}

function onPluginStart() { // This function will execute once at the start of the game
    SetPlayerSupply(P1, SUP_RACE_ZERG, SUP_TYPE_MAX, 1000); // Set player 1 zerg maximum supply to 500
    SetPlayerSupply(P1, SUP_RACE_TERRAN, SUP_TYPE_MAX, 1000); // Set player 1 terran maximum supply to 500
    SetPlayerSupply(P1, SUP_RACE_PROTOSS, SUP_TYPE_MAX, 1000); // Set player 1 protoss maximum supply to 500

    SetUnitSupplyRequired($U("Terran SCV"), 0); // Set SCV's supply requirement to 0
    SetUnitMineralCost($U("Terran SCV"), 0); // Set the mineral cost to build SCV to 0
    SetUnitBuildTime($U("Terran SCV"), 10); // Set the time to build SCV to 10 frames, build time is recommended to be at least 6 frames 

    SetUnitSupplyProvided($U("Terran Command Center"), 200); // Set the supply provided by the command center to 200, i.e. 100 supply
    SetUnitSupplyProvided($U("Terran SCV"), 200); // Set the supply provided by SCV to 200, i.e. 100 supply
    SetUnitSupplyProvided($U("Terran Ghost"), 200); // Set the supply provided by Ghost to 200, i.e. 100 supply

    SetWeaponCooldown("C-10 Concussion Rifle", 0); // Set the Ghost's weapon interval to 0 frames (even if set to 0, 0~1 frames will be randomly added)
    setcurpl(P1);
    RawTrigger(
        actions = list(
            CreateUnit(1, "Terran Command Center", "Location 1", P1),
            CreateUnit(2, "Terran SCV", "Location 2", P1),
            CreateUnit(2, "Terran Ghost", "Location 2", P1),
            SetResources(P1, Add, 100000, OreAndGas),
        ),
        preserved = false,
    );
}

function beforeTriggerExec() { // This will execute once before each frame, then execute classical triggers 
    // const cp = getcurpl();
    // setcurpl(cp);
}

function afterTriggerExec() { // This function will execute once after classical triggers execute each frame 

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
Then double-click "build.bat" to compile the code and synthesize it with "ChangeSupplyLimit-Terrain.scx" into a new map file "ChangeSupplyLimit.scx".

makefile.edd
    Is the project configuration file

main.eps
    Is the code file 

ChangeSupplyLimit-Terrain.scx
    Is the original terrain file, this file can be opened and edited with SCMD 

ChangeSupplyLimit.scx
    This is the final output map file, which can be placed in the game's map file directory ([StarCraft installation or document path]\Maps\) to see the actual effect of the code in the game. It can no longer be directly opened and edited with SCMD.

Demo from: https://github.com/havonz/SCRMapDocs
```