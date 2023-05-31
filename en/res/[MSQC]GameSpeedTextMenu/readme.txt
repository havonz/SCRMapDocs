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