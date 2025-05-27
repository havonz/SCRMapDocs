---
sidebar_position: 4
---

# euddraft Reference

Reference:  
[http://www.staredit.net/topic/17037/](http://www.staredit.net/topic/17037/)  

<br />

- [Basic configuration](#basic-configuration)
- [Script/Plugin Writing](#scriptplugin-writing)
    - [Python Pseudo-Syntax](#python-pseudo-syntax)
    - [epScript](#epscript)
- [Running Mode](#running-mode)
    - [Script File Extension Differences](#script-file-extension-differences)
    - [Load Order](#load-order)

<br />

## Basic configuration
1. Create a configuration file with the extension .eds/.edd  
    .eds format means it will only be compiled once 
    .edd format means it will compile in daemon mode, monitor the project status, and automatically recompile after files in the current directory are modified  

    ```ini
    [main]
    input : Enter the file name of the input map here
    output : Enter the file name of the output map here

    [Plugin name 1]
    :: Plugin input parameters
    Key 1 : Value 1 
    Key 2 : Value 2 

    [Plugin name 2]
    :: If the plugin has no parameters, nothing needs to be written

    [Plugin name 3]
    ```

    

2. Compile using the configuration in the configuration file  
    Possible usages are:
    ```PowerShell
    euddraft.exe Config-file.eds
    ```
    Or like this:
    ```PowerShell
    euddraft.exe Config-file.edd
    ```

    

3. If successful, the map will be output. If unsuccessful, there will be error messages.  
    euddraft has built-in several plugins  
    ```ini
    [dataDumper]
    :: ZergAI.bin will be transferred to memory location 0x68C104
    ZergAI.bin : 0x68C104, copy

    [unlimiter]
    :: Remove bullet limit, no parameters required 

    [eudTurbo]
    :: Greatly reduce the trigger polling interval to accelerate EUD polling

    [MSQC]
    :: Many parameters, hard to explain briefly

    [freeze]
    freeze : false
    ```

<br />

## Script/Plugin Writing
euddraft uses script writing to write EUD triggers. There are two ways to write scripts:  
One is to directly use a pseudo-syntax of Python to call eudplib to complete  
The other is to use epScript specially designed for this purpose (which will be compiled into Python pseudo-syntax and ultimately also call eudplib to complete the work)  

- ### Python Pseudo-Syntax

    ```python
    from eudplib import *

    # Variable definition.
    a = EUDVariable()  # Create a reference to a variable that can be used in Starcraft, with an initial value of 0
    b = EUDVariable(1) # Create a reference to a variable that can be used in Starcraft, with an initial value of 1

    # IMPORTANT : Every variable is static.
    c = a + b  # Create a reference to a variable that can be used in Starcraft with the result of a + b, and assign it to c
    c << a * b  # Assign the value of a * b to the variable referenced by c, not to c itself, but to the variable referenced by c
    # Almost every C operator works here too, with division being // instead of /

    # -----------------------------------------------------------------------------

    # If
    if EUDIf()([cond1, cond2, cond3]):
        pass  # Code
    if EUDElseIf()(cond4):
        pass  # code
    if EUDElse()():
        pass
    EUDEndIf()


    # While / LoopN
    if EUDWhile()(conds):
        pass  # Code
    EUDEndIf()


    # EUDLoopList
    for ptr, epd in EUDLoopList(ptr):
        pass

    # EUDLoopRange
    for i in EUDLoopRange(0, 100):
        pass

    # EUDLoopUnit
    for ptr, epd in EUDLoopUnit():
        pass

    # EUDLoopUnit2
    for ptr, epd in EUDLoopUnit2():
        pass

    # EUDLoopCUnit
    for cunit in EUDLoopCUnit():
        pass

    # EUDLoopNewUnit
    for ptr, epd in EUDLoopNewUnit():
        pass

    # EUDLoopNewCUnit
    for cunit in EUDLoopNewCUnit():
        pass

    # EUDLoopPlayerUnit
    for ptr, epd in EUDLoopPlayerUnit(player):
        pass

    # EUDLoopPlayerCUnit
    for cunit in EUDLoopPlayerCUnit(player):
        pass

    # Break & Continue
    # While / LoopN
    if EUDWhile()(conds):
        EUDBreak()  # Break out
        EUDBreakIf(cond)  # Break if
        EUDContinue()  # Goto continue point
        EUDContinueIf(cond)  # Continue if
        # Some codes
        EUDSetContinuePoint()  # Here is continue point.
        # Do some i++ thing here
        pass  # Code
    EUDEndWhile()  # NOT EUDEndIf

    # there's also EUDWhileNot, EUDIfNot, EUDBreakIfNot, EUDContinueIfNot etc.

    if EUDLoopN()(100):
        pass  # code
    EUDEndLoopN()

    if EUDPlayerLoop()():
        pass  # code
    EUDEndPlayerLoop()

    EUDSwitch(variable) # OR EPDSwitch(epd_address)
    # you can add bitmask on EUDSwitch too: like EUDSwitch(variable, 0xFF)
    if EUDSwitchCase()(0):
        pass  # code
        EUDBreak()
    if EUDSwitchCase()(1, 2):
        pass  # code
        EUDBreak()
    if EUDSwitchCase()(3, 4, 5, 6):
        pass  # code
        EUDBreak()
    EUDEndSwitch()

    # Function definition. no recursion allowed
    @EUDFunc
    def funcname(arg1, arg2):
        # Use arguments to do stuff.
        # All arguments are EUDVariable type

        # EUDReturn to return value
        EUDReturn(arg1 + arg2)

        # Legacy support, but use this only on the very end of the function.
        return arg1 + arg2


    # -----------------------------------------------------------------------------

    # Resource declaration

    a = Db(4)  # Create empty space with size 4byte
    a = Db(b'\x04\x01\x06\x08')  # Create memory with initial value 04 01 06 08
    ```

- ### epScript
    [epScript Reference](epScript-Reference/epScript-Reference.md)  


<br />

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

        

      

      

    



