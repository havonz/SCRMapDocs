# Trigger和RawTrigger的运用

[下载演示地图](Trigger-and-RawTrigger.zip)

## makefile.edd
```ini
[main]
input: Trigger和RawTrigger的运用-地形.scx
output: Trigger和RawTrigger的运用.scx

[eudTurbo]
[地图主脚本.eps]
```

## 地图主脚本.eps
```Javascript
var NextWaveTime = 3;

function doTriggerList() {
    // 设定一个无条件触发器，只触发一次
    DoActions(CreateUnit(1, "Terran SCV", "Location 2", P1), preserved = false,);

    // 设定一个无条件触发器，只触发一次
    DoActions(CreateUnit(1, "Zerg Zergling", "Location 1", P1), preserved = false,);

    // 设定一个每 3 游戏秒执行的触发器
    Trigger(
        conditions = list(
            ElapsedTime(AtLeast, NextWaveTime), // 这个条件中用到了变量 NextWaveTime 所以只能用 Trigger 而不能用 RawTrigger
        ),
        actions = list(
            GiveUnits(1, "Zerg Zergling", P1, $L("Location 1"), P12),
            Order("Zerg Zergling", P12, $L("Location 1"), Move, $L("Location 3")),
            // 给 NextWaveTime + 3 就是本次触发之后 3 秒之后再触发一次
            NextWaveTime.AddNumber(3),
            CreateUnit(1, "Zerg Zergling", "Location 1", P1),
        ),
    );

    // 设定一个玩家12的小狗进入 Location 3 就死的触发器
    RawTrigger(
        conditions = list(
            Bring(P12, AtLeast, 1, "Zerg Zergling", $L("Location 3")),
        ),
        actions = list(
            KillUnitAt(10, "Zerg Zergling", "Location 3", P12),
        ),
    );

    // 设定一个 玩家1 小狗进入 Location 3 变 10 只，只触发一次
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

function beforeTriggerExec() { // 游戏每一帧会先执行一次这个，然后执行传统触发器
    const cp = getcurpl();
    
    // 其它代码写在这里

    setcurpl(cp);

    doTriggerList(); // 执行触发器列表
}

function afterTriggerExec() { // 游戏每一帧在执行完传统触发器后，会执行一次这个函数

}
```

## 编译输出.bat
```PowerShell
@copy makefile.edd makefile.eds
@C:\Users\havonz\Applications\euddraft0.9.9.9\euddraft.exe makefile.eds
@del /f /q makefile.eds
@pause
```

## 说明.txt
```
右键编辑 “编译输出.bat” 文件，将其中的 euddraft.exe 路径改成你自己电脑上的  euddraft.exe 的路径
然后双击 “编译输出.bat” 即会将代码编译并与 “Trigger和RawTrigger的运用-地形.scx” 合成输出到一个新地图文件 “Trigger和RawTrigger的运用.scx”

makefile.edd
    是工程配置文件

地图主脚本.eps
    是代码文件

Trigger和RawTrigger的运用-地形.scx
    是原始地形文件，这个文件可以用 SCMD 打开编辑地形等

Trigger和RawTrigger的运用.scx
    这是最终输出的地图文件，可以放入游戏的地图文件目录（[星际争霸安装或文档路径]\Maps\）在游戏中看到实际代码在游戏中的效果，它已经无法再直接使用 SCMD 打开编辑

演示来自 https://github.com/havonz/SCRMapDocs
```