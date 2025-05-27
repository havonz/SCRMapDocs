# 修改单位消耗人口限制

[下载演示地图](ChangeSupplyLimit.zip)

## makefile.edd
```ini
[main]
input: 修改单位消耗人口限制-地形.scx
output: 修改单位消耗人口限制.scx

[地图主脚本.eps]
```

## 地图主脚本.eps
```Javascript
// 偏移地址参考：https://armoha.github.io/eud-book/

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

function onPluginStart() { // 游戏开始将会执行一次这个函数
    SetPlayerSupply(P1, SUP_RACE_ZERG, SUP_TYPE_MAX, 1000); // 将 玩家1 最大虫子人口 改成 500
    SetPlayerSupply(P1, SUP_RACE_TERRAN, SUP_TYPE_MAX, 1000); // 将 玩家1 最大人类人口 改成 500
    SetPlayerSupply(P1, SUP_RACE_PROTOSS, SUP_TYPE_MAX, 1000); // 将 玩家1 最大神族人口 改成 500

    SetUnitSupplyRequired($U("Terran SCV"), 0); // 设置 SCV 的人口需求为 0 也就是不用人口
    SetUnitMineralCost($U("Terran SCV"), 0); // 设置建造 SCV 的矿物消耗为 0
    SetUnitBuildTime($U("Terran SCV"), 10); // 设置建造 SCV 的时间为 10/24 秒，建造时间建议至少设为 6

    SetUnitSupplyProvided($U("Terran Command Center"), 200); // 将控制中心的人口提供量改成 200，也就是 100 人口
    SetUnitSupplyProvided($U("Terran SCV"), 200); // 将 SCV 的人口提供量改成 200，也就是 100 人口
    SetUnitSupplyProvided($U("Terran Ghost"), 200); // 将 Ghost 的人口提供量改成 200，也就是 100 人口

    SetWeaponCooldown("C-10 Concussion Rifle", 0); // 将鬼兵的武器间隔设置为 0 帧（即使设置为 0 也会随机加上 0~1 帧）
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

function beforeTriggerExec() { // 游戏每一帧会先执行一次这个，然后执行传统触发器
    // const cp = getcurpl();
    // setcurpl(cp);
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
然后双击 “编译输出.bat” 即会将代码编译并与 “修改单位消耗人口限制-地形.scx” 合成输出到一个新地图文件 “修改单位消耗人口限制.scx”

makefile.edd
    是工程配置文件

地图主脚本.eps
    是代码文件

修改单位消耗人口限制-地形.scx
    是原始地形文件，这个文件可以用 SCMD 打开编辑地形等

修改单位消耗人口限制.scx
    这是最终输出的地图文件，可以放入游戏的地图文件目录（[星际争霸安装或文档路径]\Maps\）在游戏中看到实际代码在游戏中的效果，它已经无法再直接使用 SCMD 打开编辑

演示来自 https://github.com/havonz/SCRMapDocs
```