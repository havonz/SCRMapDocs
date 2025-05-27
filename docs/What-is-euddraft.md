---
sidebar_position: 3
---

# Relationship between epScript, eudplib, and euddraft

- [epScript](https://raw.githubusercontent.com/armoha/eudplib/master/docs/funclist.txt) is a scripting language created by [TriggerKing](https://github.com/phu54321) , designed for the runtime development of EUD maps in StarCraft: Remastered.  
- [eudplib](https://github.com/armoha/eudplib) is a Python library, which is an extension library for generating trigger bytecodes of the StarCraft: Remastered runtime, and is the functional core of epScript.  
- [euddraft](https://github.com/armoha/euddraft) is a program integrated with Python, it will compile the written epScript script (`*.eps`) into a corresponding Python script (`*.py`) that calls eudplib to generate trigger bytecode;  
  then euddraft will execute these Python scripts(`*.py`) to generate The actual trigger bytecodes that are logically equivalent to the written epScript script (`*.eps`);  
  finally euddraft inserts these trigger bytecodes into the map to take effect when the map is loaded in the game.


  



