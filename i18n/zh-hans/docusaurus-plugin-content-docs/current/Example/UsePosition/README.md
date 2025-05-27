# 位置函数的使用

[下载演示地图](UsePosition.zip)

## makefile.edd
```ini
[main]
input: 位置函数的使用-地形.scx
output: 位置函数的使用.scx

[eudTurbo]

[地图主脚本.eps]
```

## 地图主脚本.eps
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

function beforeTriggerExec() { // 游戏每一帧会先执行一次这个，然后执行传统触发器
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
        printAt(0, "机枪兵({},{})(脸朝:{}) 与 鬼兵({},{})(脸朝:{}) 的距离为 {} 角度为 {}", x0, y0, marine_cu.currentDirection2, x1, y1, ghost_cu.currentDirection2, dist, ang);
        printAt(1, "从机枪兵位置向 {} 度走 {} 的距离将到达 ({},{}) 鬼兵的位置", ang, dist, x, y);
    }

    setcurpl(cp);
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
然后双击 “编译输出.bat” 即会将代码编译并与 “位置函数的使用-地形.scx” 合成输出到一个新地图文件 “位置函数的使用.scx”

makefile.edd
    是工程配置文件

地图主脚本.eps
    是代码文件

位置函数的使用-地形.scx
    是原始地形文件，这个文件可以用 SCMD 打开编辑地形等

位置函数的使用.scx
    这是最终输出的地图文件，可以放入游戏的地图文件目录（[星际争霸安装或文档路径]\Maps\）在游戏中看到实际代码在游戏中的效果，它已经无法再直接使用 SCMD 打开编辑

演示来自 https://github.com/havonz/SCRMapDocs
```