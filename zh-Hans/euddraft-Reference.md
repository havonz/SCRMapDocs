# euddraft 参考

文档资料来源：  
[http://www.staredit.net/topic/17037/](http://www.staredit.net/topic/17037/)  

<br />

- [基本配置](#%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE)
- [脚本/插件编写](#%E8%84%9A%E6%9C%AC%E6%8F%92%E4%BB%B6%E7%BC%96%E5%86%99)
    - [Python 伪语法](#python-%E4%BC%AA%E8%AF%AD%E6%B3%95)
    - [epScript](#epscript)
- [运行模式](#%E8%BF%90%E8%A1%8C%E6%A8%A1%E5%BC%8F)
    - [脚本文件扩展名区别](#%E8%84%9A%E6%9C%AC%E6%96%87%E4%BB%B6%E6%89%A9%E5%B1%95%E5%90%8D%E5%8C%BA%E5%88%AB)
    - [加载顺序](#%E5%8A%A0%E8%BD%BD%E9%A1%BA%E5%BA%8F)

<br />

## 基本配置
1. 创建一个配置文件，配置文件的扩展名为 .eds/edd  
    .eds 格式表示它仅仅会编译一次  
    .edd 格式表示它会守护模式编译，监控项目状态，在当前目录有文件被更改之后自动重新编译  

    ```ini
    [main]
    input : 这里写上输入地图的文件名
    output : 这里写上输出地图的文件名

    [插件名1]
    :: 插件的输入参数
    键1 : 值1
    键2 : 键2

    [插件名2]
    :: 如果插件没有参数就啥也不用写

    [插件名3]
    ```

    

2. 使用配置文件中的配置编译  
    可能的用法如下
    ```PowerShell
    euddraft.exe 配置文件.eds
    ```
    或者这样
    ```PowerShell
    euddraft.exe 配置文件.edd
    ```

    

3. 如果顺利，那么就编译完成并且会输出地图，如果不顺利，会有错误信息提示。  
    euddraft 内置了几个插件
    ```ini
    [dataDumper]
    :: ZergAI.bin 将会被转存到 0x68C104 这个内存位置上
    ZergAI.bin : 0x68C104, copy

    [unlimiter]
    :: 解除子弹限制，不用参数

    [eudTurbo]
    :: EUD 轮询加速，大幅降低触发器轮询间隔

    [MSQC]
    :: 参数很多，一言难尽
    ```

<br />

## 脚本/插件编写
euddraft 使用脚本编写的方式编写 EUD 触发器，编写脚本的方式有两种  
一种是直接使用 Python 的一种伪语法调用 eudplib 来完成  
另一种是使用专门为此设计的 epScript （它将会编译成 Python 伪语法并且最终也是调用 eudplib 来完成工作）  

- ### Python 伪语法

    ```python
    from eudplib import *

    # Variable definition.
    a = EUDVariable()  # 创建一个星际内可以使用的变量的引用，初始值是 0
    b = EUDVariable(1)  # 创建一个星际内可以使用的变量的引用，初始值是 1

    # IMPORTANT : Every variable is static.
    c = a + b  # 以 a + b 的结果创建一个星际内可以使用的变量的引用，赋值给 c
    c << a * b  # 将 a * b 的值赋给 c 所引用的那个星际内可用的变量，不是给 c 赋值，是给 c 引用的那个变量赋值
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
    详见：[epScript 参考](epScript-Reference.md)  


<br />

## 运行模式

### 脚本文件扩展名区别
- 假如是 py 格式的脚本，那么在 eds/edd 文件中是可以不写扩展名的，eps 格式得把扩展名加上

    ```ini
    [main]
    input: basemap.scx
    output: outputmap.scx

    [eudTurbo]
    :: 它实际是加载的一个文件名为 eudTurbo.py 的脚本

    [main.eps]
    :: 如果是 eps 格式就这么加载
    ```

### 加载顺序

- 配置文件中的插件名的先后顺序与其在游戏开始后它们的加载顺序是相关联的，游戏开始后，会执行一次脚本中的 onPluginStart()，然后在所有玩家的机器上都循环执行 beforeTriggerExec()、触发器、afterTriggerExec()

    例如有如下配置 main.edd：

    ```ini
    [main]
    input: in.scx
    output: out.scx

    [eudTurbo]
    [a.eps]
    [b.eps]

    ```
    游戏开始后的执行顺序为：
    ```PowerShell
    eudTurbo.onPluginStart()
    a.onPluginStart()
    b.onPluginStart()
    每帧循环执行:
        eudTurbo.beforeTriggerExec()
        a.beforeTriggerExec()
        b.beforeTriggerExec()
        SCMD 触发器
        b.afterTriggerExec()
        a.afterTriggerExec()
        eudTurbo.afterTriggerExec()
    ```

        

      

      

    



