# 游戏中的文字菜单

[下载演示地图]([MSQC]GameSpeedTextMenu.zip)

## makefile.edd
```ini
[main]
input: 游戏中的文字菜单-地形.scx
output: 游戏中的文字菜单.scx

[地图主脚本.eps]

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

## 地图主脚本.eps
```Javascript
// 偏移地址参考：https://armoha.github.io/eud-book/

/* 设置游戏速度百分比函数，以 Fastest(level:6) 的普通速度作为 100% */
function SetGameSpeed(level, speed) {
    const mspf = 1000000 / (10000 / 42 * speed);
    dwwrite_epd(EPD(0x5124D8) + level, mspf);
}

const MOUSE_X_EPD, MOUSE_Y_EPD = EPD(0x6CDDC4), EPD(0x6CDDC8);
const menuSel = PVariable();
var currentSpeedSel;

/* 注册 menuSel 全局变量给 MSQC */
EUDRegisterObjectToNamespace("menuSel", menuSel);

function showMenu(p) {
    const speedList = 50, 100, 125, 150, 200, 400, 4200;
    const isMouseOvered = EUDLightBool(), EUDLightBool(), EUDLightBool(), EUDLightBool(), EUDLightBool(), EUDLightBool(), EUDLightBool();
    if (getcurpl() == p) {
        const buf = StringBuffer(255);
        buf.printfAt(0, "\x02当前选择的游戏速度：\x07{}\x02%\x02", currentSpeedSel);
        foreach(i : py_range(0, 7)) {
            buf.insert(0);
            if (speedList[i] == currentSpeedSel) {
                buf.append("\x03[\x07x\x03]\x1E ");
            } else {
                buf.append("\x03[  ]\x02");
            }
            buf.appendf(" 游戏速度 {}%\x02", speedList[i]);
            buf.DisplayAt(i + 1);
        }
        if (MemoryEPD(MOUSE_X_EPD, AtLeast, 10) && MemoryEPD(MOUSE_X_EPD, AtMost, 25)) { /* 菜单选框 X 坐标触发范围 */
            foreach(i : py_range(0, 7)) {
                if (MemoryEPD(MOUSE_Y_EPD, AtLeast, 129 + i * 16) && MemoryEPD(MOUSE_Y_EPD, AtMost, 139 + i * 16)) { /* 菜单选框 Y 坐标触发范围 */
                    if (speedList[i] != currentSpeedSel) {
                        buf.insert(0);
                        buf.append("\x07[  ]\x1E ");
                        buf.appendf(" 游戏速度 {}%\x02", speedList[i]);
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
    /* 设置 Fastest 的游戏速度为 100% */
    currentSpeedSel = 100;
    SetGameSpeed(6, currentSpeedSel);
}

function beforeTriggerExec() {
    const cp = getcurpl();

    RawTrigger(actions = list(
        CreateUnitWithProperties(1, "Zerg Overlord", "Location 1", P1, UnitProperty(invincible = true)),
        CreateUnitWithProperties(1, "Zerg Overlord", "Location 1", P2, UnitProperty(invincible = true)),
    ), preserved = false);

    /* 给所有的人类玩家显示菜单 */
    foreach(p : EUDLoopPlayer("Human")) {
        setcurpl(p);
        showMenu(p);
    }

    setcurpl(cp);
}

function afterTriggerExec() {
    /* 接收 MSQC 选择并同步到 currentSpeedSel，这个循环会在每一个玩家的机器上执行 */
    foreach(p : EUDLoopPlayer("Human")) {
        if (menuSel[p] != 0) { /* 如果某玩家 p 的 menuSel 有值 */
            currentSpeedSel = menuSel[p]; /* 接收它 */
            menuSel[p] = 0;               /* 清空它，等待下一次接收 */

            /* 以下就是对本机的操作了 */
            SetGameSpeed(6, currentSpeedSel);
            setcurpl(getuserplayerid());
            
            if (p == getuserplayerid()) { /* 如果本机玩家恰好就是操作菜单的玩家 */
                PlayWAV("sound\\glue\\mousedown2.wav");
                printAt(10, "你将游戏速度更改为 \x07{}\x02%", currentSpeedSel);
            } else {
                PlayWAV("sound\\misc\\transmission.wav");
                printAt(10, "{}{} \x02将游戏速度更改为 \x07{}\x02%", PColor(p), PName(p), currentSpeedSel);
            }
        }
    }
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
然后双击 “编译输出.bat” 即会将代码编译并与 “游戏中的文字菜单-地形.scx” 合成输出到一个新地图文件 “游戏中的文字菜单.scx”

makefile.edd
    是工程配置文件

地图主脚本.eps
    是代码文件

游戏中的文字菜单-地形.scx
    是原始地形文件，这个文件可以用 SCMD 打开编辑地形等

游戏中的文字菜单.scx
    这是最终输出的地图文件，可以放入游戏的地图文件目录（[星际争霸安装或文档路径]\Maps\）在游戏中看到实际代码在游戏中的效果，它已经无法再直接使用 SCMD 打开编辑


演示来自 https://github.com/havonz/SCRMapDocs
```