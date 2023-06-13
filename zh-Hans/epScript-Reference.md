# epScript 参考

<br />

- 语言文档
    - [基本语法](epScript-Reference/Syntax.md)  
    - [使用变量](epScript-Reference/Use-of-Variables.md)  
    - [使用函数](epScript-Reference/Use-of-Functions.md)  
    - [使用对象](epScript-Reference/Use-of-Objects.md)  
    - [字符串说明](epScript-Reference/Understanding-Strings.md)  
    - [内置基本对象类型](epScript-Reference/Built-in-Object-Types.md)  
    - [内置扩展对象类型](epScript-Reference/Built-in-Object-Types-Ext.md)  
    - [常量对照表](epScript-Reference/Constants-Reference.md)  
    - [内置函数库](epScript-Reference/Built-in-Functions.md) 
- 相关说明  
    - [如何开始](#%E5%A6%82%E4%BD%95%E5%BC%80%E5%A7%8B)
        - [环境准备](#%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)
        - [地图准备](#%E5%9C%B0%E5%9B%BE%E5%87%86%E5%A4%87)
        - [建立工程](#%E5%BB%BA%E7%AB%8B%E5%B7%A5%E7%A8%8B)
        - [示例工程](#%E7%A4%BA%E4%BE%8B%E5%B7%A5%E7%A8%8B)
    - [运行模式](#%E8%BF%90%E8%A1%8C%E6%A8%A1%E5%BC%8F)
        - [脚本文件扩展名区别](#%E8%84%9A%E6%9C%AC%E6%96%87%E4%BB%B6%E6%89%A9%E5%B1%95%E5%90%8D%E5%8C%BA%E5%88%AB)
        - [加载顺序](#%E5%8A%A0%E8%BD%BD%E9%A1%BA%E5%BA%8F)
    - [edd 和 eds 的区别](#edd-%E5%92%8C-eds-%E7%9A%84%E5%8C%BA%E5%88%AB)
        - [对 edd 格式](#%E5%AF%B9-edd-%E6%A0%BC%E5%BC%8F)
        - [对 eds 格式](#%E5%AF%B9-eds-%E6%A0%BC%E5%BC%8F)
    - [数据同步说明](#%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%E8%AF%B4%E6%98%8E)
    - [游戏时间说明](#%E6%B8%B8%E6%88%8F%E6%97%B6%E9%97%B4%E8%AF%B4%E6%98%8E)
        - [游戏帧（Frame）](#%E6%B8%B8%E6%88%8F%E5%B8%A7frame)
        - [游戏秒](#%E6%B8%B8%E6%88%8F%E7%A7%92)
        - [游戏速度](#%E6%B8%B8%E6%88%8F%E9%80%9F%E5%BA%A6)
        - [触发器扫描间隔](#%E8%A7%A6%E5%8F%91%E5%99%A8%E6%89%AB%E6%8F%8F%E9%97%B4%E9%9A%94)
    - [当前玩家与本机玩家](#%E5%BD%93%E5%89%8D%E7%8E%A9%E5%AE%B6%E4%B8%8E%E6%9C%AC%E6%9C%BA%E7%8E%A9%E5%AE%B6)
        - [当前玩家](#%E5%BD%93%E5%89%8D%E7%8E%A9%E5%AE%B6)
        - [本机玩家](#%E6%9C%AC%E6%9C%BA%E7%8E%A9%E5%AE%B6)

<br />

## 如何开始

以下操作，假如有遇到看不懂的地方，可以尝试搜索引擎解决  
这里假设你已经会使用 ScmDraft2 进行基本的地形设计  
- 如果不会可参考：[SCMD](https://www.scrmea.com/index.php?title=SCMD)  

这里假设你对 EUD 的基本概念已经有一定了解  
- 如果不了解可参考：[EUD 的概念](What-is-EUD.md)  

### 环境准备

准备一台 Windows 10 以上的 PC，或者虚拟机  
准备好 ScmDraft2，如果还没准备，往回看几行  

- 下载 [euddraft0.9.9.9.zip](https://github.com/armoha/euddraft/releases/download/v0.9.9.9/euddraft0.9.9.9.zip)  
    将 euddraft 解压缩到一个纯英文没有空格的路径中，例如 D:\SCRMapDevTools\euddraft0.9.9.9

- 下载 [VSCode](https://code.visualstudio.com/Download)  
    安装它，随便怎么安装都行  
    从 VSCode 插件商店中安装 eps-server 这款插件  

- 打开系统的文件扩展名显示  
    Windows 10 可以参考 [https://www.baidu.com/s?wd=win10显示文件扩展名](https://www.baidu.com/s?wd=win10%E6%98%BE%E7%A4%BA%E6%96%87%E4%BB%B6%E6%89%A9%E5%B1%95%E5%90%8D)  
    Windows 11 可以参考 [https://www.baidu.com/s?wd=win11显示文件扩展名](https://www.baidu.com/s?wd=win11显示文件扩展名)  


### 地图准备

准备一个普通的地图文件，可以用 ScmDraft2 新建，然后保存成 Starcraft: Remastered Broodwar Map (*.scx) 格式的文件  
依然保存到一个纯英文没有空格的路径中，例如 D:\Projects\test\basemap.scx


### 建立工程
1. 新建一个文本文档，并将它的扩展名改为 edd ，例如 D:\Projects\test\test.edd  
使用 VSCode 打开这个 edd 文件（直接将文件往打开的 VSCode 界面中一拖就行）  
    然后把它的内容改为  
    ```MakeFile
    [main]
    input: basemap.scx
    output: test.scx

    [main.eps]
    ```
    以上代码用相对路径做例子，实际它是支持绝对路径的  

2. 新建一个文本文档，并将它的扩展名改为 eps，例如 D:\Projects\test\main.eps  
    使用 VSCode 打开这个 eps 文件  
    然后把它的内容改为  
    ```JavaScript
    function onPluginStart() { // 游戏开始将会执行一次这个函数
        DisplayTextAll("Hello World");
    }

    function beforeTriggerExec() { // 游戏每一帧会先执行一次这个，然后执行传统触发器

    }

    function afterTriggerExec() { // 游戏每一帧在执行完传统触发器后，会执行一次这个函数

    }
    ```

3. 新建一个文本文档，并将它的扩展名改为 bat ，例如 D:\Projects\test\build.bat  
    使用 VSCode 打开这个 bat 文件，然后把它的内容改为  
    ```PowerShell
    D:\SCRMapDevTools\euddraft0.9.9.9\euddraft.exe test.edd
    ```
    以上代码假设你将 euddraft 解压到了 D:\SCRMapDevTools\euddraft0.9.9.9 这个位置，如果它不在这儿，你应该替换它  

    至此，工程就已经建立好了，直接双击运行 build.bat 就可以生成 test.scx，将这个地图放入星际重制版的地图目录中，然后进游戏就能看到它在屏幕上输出的 `Hello World`


### 示例工程

- 实在是看不懂上面的配置过程，随意选择一个简易示例工程查看  
    - [Trigger和RawTrigger的运用](res/Trigger和RawTrigger的运用/makefile.edd)
    - [修改单位消耗人口限制](res/修改单位消耗人口限制/makefile.edd)
    - [位置函数的使用](res/位置函数的使用/makefile.edd)
    - [[MSQC演示]游戏中文字菜单](res/[MSQC演示]游戏中文字菜单/makefile.edd)  <br /><br />



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
    <br />

## edd 和 eds 的区别

euddraft 对这两种扩展名的处理方式不一样

- ### 对 edd 格式

    它会在顺利编译生成地图完成后保持等待，如果项目目录中文件发生了变化，则它会自动再次编译生成地图  
    如果不顺利，则输出错误信息，你可以在修改后按 R 键让它再次编译生成  

- ### 对 eds 格式

    它会在顺利编译生成地图完成后退出  
    如果不顺利，会输出错误信息并等待你按回车键退出  <br /><br />

## 数据同步说明

如果将`非同步数据`（例如玩家当前鼠标位置）赋值给变量，该变量对于每个玩家的机器上都不一样，若以该变量的状态作为判断条件执行了`需要同步的数据`的动作（例如创建单位），则可能会导致多人游戏中玩家`需要同步的数据`不同步（例如 A 机器上创建了单位，B 机器上却没创建单位）而引发掉线。

该类任务通常可以使用 MSQC 插件来辅助解决    

[示例：游戏中文字菜单/地图主脚本.eps](res/%5BMSQC%E6%BC%94%E7%A4%BA%5D%E6%B8%B8%E6%88%8F%E4%B8%AD%E6%96%87%E5%AD%97%E8%8F%9C%E5%8D%95/%E5%9C%B0%E5%9B%BE%E4%B8%BB%E8%84%9A%E6%9C%AC.eps)  
<br /><br />



## 游戏时间说明

星际争霸1游戏中的时间概念不同于现实时间

- ### 游戏帧（Frame）

    星际争霸1的最小游戏时间单位为游戏帧（Frame），下文简写为 fr  
    `1 fr` == `1/16 游戏秒`

- ### 游戏秒

    星际争霸1中游戏秒与游戏帧换算公式  
    `1 游戏秒` == `16 fr`

- ### 游戏速度

    《星际争霸1》中常规游戏速度有七档。  
    不同的游戏速度下，每游戏帧（Frame）所表示的操作系统时间是不同的。  
    操作系统时间通常等同于现实世界的时间。  
    <details><summary>在无网络延迟的情况下，每游戏帧（Frame）对应的操作系统时间（精确值，非近似）</summary>

    ```JavaScript
    极慢（Slowest）： 1 fr == 0.167 操作系统秒
    更慢（Slower） ： 1 fr == 0.111 操作系统秒
    慢速（Slow）   ： 1 fr == 0.083 操作系统秒
    普通（Normal） ： 1 fr == 0.067 操作系统秒
    快速（Fast）   ： 1 fr == 0.056 操作系统秒
    更快（Faster） ： 1 fr == 0.048 操作系统秒
    极快（Fastest）： 1 fr == 0.042 操作系统秒
    ```
    </details>

    由此可算得，在极快（Fastest）游戏速度下，1 游戏秒为`0.042 × 16 = 0.672`现实秒，1 操作系统秒为`1 ÷ 0.042 ÷ 16 ≈ 1.488`游戏秒。

- ### 触发器扫描间隔

    《星际争霸1》的触发器是单线程轮询式的。  
    在不使用 eudTurbo 和 Wait 动作的情况下触发器的轮询间隔为`31 fr`，即`1.9375 游戏秒`。  
    并且，游戏开始后的第一次轮询是在第`2 fr`，即第`0.125 游戏秒`。

    <details><summary>游戏开始后触发器轮询时间点</summary>

    ```JavaScript
    第一次轮询于第   2 游戏帧，即第  0.1250 游戏秒
    第二次轮询于第  33 游戏帧，即第  2.0625 游戏秒
    第三次轮询于第  64 游戏帧，即第  4.0000 游戏秒
    第四次轮询于第  95 游戏帧，即第  5.9375 游戏秒
    第五次轮询于第 126 游戏帧，即第  7.8750 游戏秒
    第六次轮询于第 157 游戏帧，即第  9.8125 游戏秒
    第七次轮询于第 188 游戏帧，即第 11.7500 游戏秒
    第八次轮询于第 219 游戏帧，即第 13.6875 游戏秒
    第九次轮询于第 250 游戏帧，即第 15.6250 游戏秒
                ……以此类推……
    ```
    </details>
    
    ElapsedTime 条件的参数中的游戏秒判断是取整数部分
    ```JavaScript
    function beforeTriggerExec() {
        if (ElapsedTime(Exactly, 6)) {
            DisplayTextAll("这条消息将不会输出");
        }
    }
    ```
    所以，这个条件不会达成，因为第四次轮询在第 5.9375 游戏秒整数部分为 5，第五次轮询在第 7.8750 游戏秒整数部分为 7，所以 ElapsedTime(Exactly, 6) 永远不成立。  
    如果希望在第 6 游戏秒后一次性执行某个动作，可以这么写
    ```JavaScript
    function beforeTriggerExec() {
        once (ElapsedTime(AtLeast, 6)) {
            DisplayTextAll("这条消息会在游戏开始第 6 游戏秒后输出一次");
        }
    }
    ```
    同理 CountdownTimer 条件也是取屏幕上方倒计时的整数部分  
    因此在写有关时间判定的条件时，不应该使用 Exactly（等于），而应该用 AtLeast（大于或等于） 或者 AtMost（小于或等于）  <br /><br />

## 当前玩家与本机玩家

`当前玩家` 与 `本机玩家` 是两个不同的概念

### 当前玩家
<details><summary>只在 `当前玩家 == 本机玩家` 的机器上生效的动作（允许非同步使用，可单独在部分玩家机器上使用）</summary>

- DisplayText  
- CenterView  
- PlayWAV  
- MinimapPing  
- TalkingPortrait  
- Transmission  
- SetMissionObjectives  
</details>

<details><summary>会使用 `当前玩家` 作为参数的动作（必须同步在所有玩家机器上使用，否则掉线）</summary>

- SetAllianceStatus  
- RunAIScript  
- RunAIScriptAt  
- Draw  
- Defeat  
- Victory  
</details>

setcurpl 函数可以设置`当前玩家`这个全局变量值  
getcurpl 函数可以获取`当前玩家`这个全局变量现在是什么值  
无论你将`当前玩家`设置为什么值，代码都会在所有玩家的机器上执行  
```JavaScript
setcurpl(P1);
DisplayText("给玩家 1 打印的内容");
setcurpl(P2);
DisplayText("给玩家 2 打印的内容");
setcurpl(P3);
DisplayText("给玩家 3 打印的内容");

// CurrentPlayer 是常量数字 13，它可以使一些与玩家相关的条件或动作去访问 当前玩家 的值
// CurrentPlayer != getcurpl()
if ($CurrentPlayer == 13) {
    DisplayTextAll("嗯，对");
}

// 将 Fastest 游戏速度 x2
setcurpl(-122787 + 6);
SetDeaths(CurrentPlayer, SetTo, 21, 0);
```

### 本机玩家

getuserplayerid() 可以获取 本机玩家编号，它对每个机器都返回不同的值，与 setcurpl 函数设置的值毫无关联  
可以使用 getuserplayerid() 来获取本机玩家的编号意味着你可以在运行时决定是否在本机执行或者不执行某些代码  
它有助于提升性能，当玩家很多的情况下，并不是所有代码都需要对每个玩家执行，例如不需要为每个玩家都生成所有玩家都文字提示信息  
当然如果你因不熟悉同步规则使用 getuserplayerid() 直接或者间接污染了`需要同步的数据`，它也可能导致数据不同步而引发掉线  
```JavaScript
setcurpl(P1);
println("当前玩家编号：{}", getuserplayerid());
setcurpl(P2);
println("当前玩家编号：{}", getuserplayerid());
setcurpl(P3);
println("当前玩家编号：{}", getuserplayerid());
```

    

  

