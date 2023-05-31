# epScript 常量对照表

## TrgCount 个数类型

```JavaScript
All: 0
其它正整数
```

## TrgModifier 数值修改方法

```JavaScript
SetTo: 7    // 设置为 =
Add: 8      // 加上  +=
Subtract: 9 // 减去  -=（最多可以减为零，即使减去大于当前值的数值）
```

## TrgComparison 数值比较方法

```JavaScript
AtLeast: 0  // 不少于 >=
AtMost: 1   // 不多于 <=
Exactly: 10 // 完全是 ==
```

## TrgSwitchState 开关状态

```JavaScript
Set: 2     // 已设置
Cleared: 3 // 已清除
```

## TrgSwitchAction 开关动作

```JavaScript
Set: 4     // 设置状态
Clear: 5   // 清除状态
Toggle: 6  // 切换状态
Random: 11 // 使用随机状态
```

## TrgResource 资源类型

```JavaScript
Ore: 0       // 水晶矿
Gas: 1       // 气矿
OreAndGas: 2 // 水晶矿和气矿
```

## TrgAllyStatus 联盟状态

```JavaScript
Enemy: 0         // 敌人
Ally: 1          // 联盟
AlliedVictory: 2 // 联盟一起胜利
```

## TrgOrder 单位命令

```JavaScript
Move: 0   // 移动
Patrol: 1 // 巡逻
Attack: 2 // 攻击
```

## TrgPropState 触发器属性状态

```JavaScript
Enable: 4  // 启用状态
Disable: 5 // 禁用状态
Toggle: 6  // 切换状态
```

## TrgScore 玩家得分类型

```JavaScript
Total: 0             // 总分
Units: 1             // 单位分
Buildings: 2         // 建筑分
UnitsAndBuildings: 3 // 单位和建筑物分
Kills: 4             // 击杀分
Razings: 5           // 评级分
KillsAndRazings: 6   // 击杀和评级分
Custom: 7            // 自定义分
```

## TrgLocation 位置/区域
```JavaScript
// 位置/区域 一共 255 个，编号从 0~63,65~255
// 编号 64 表示任意位置/区域
"Location 1~64": 0~63
"Anywhere": 64
"Location 66~256": 65~255
// euddraft 编译时会将所有的位置/区域的名称从地图字符串表（Map String Table）中删除，也就是说，位置/区域名称在运行时是不存在的，仅用于地图开发者辨别
```

## TrgSwitch 开关

```JavaScript
// 开关一共 256 个，编号为 0~255
"Switch 1～256": 0~255
// euddraft 编译时会将所有的开关的名称从地图字符串表（Map String Table）中删除，也就是说，开关名称在运行时是不存在的，仅用于地图开发者辨别
```

## [TrgPlayer 玩家编号](Constants/TrgPlayer.md)  
## [TrgUnit 单位类型](Constants/TrgUnit.md)  
## [TrgAIScript AI脚本类型](Constants/TrgAIScript.md)  
## [Weapon 单位武器类型](Constants/Weapon.md)  
## [Tech 科技类型](Constants/Tech.md)  
## [Upgrade 升级类型](Constants/Upgrade.md)  
## [UnitOrder 单位命令](Constants/UnitOrder.md)  
## [Flingy 飞行类型](Constants/Flingy.md)  
## [Image 图像类型](Constants/Image.md)  
## [Icon 图标类型](Constants/Icon.md)  
## [Iscript 动画脚本类型](Constants/Iscript.md)  
## [Portrait 单位头像类型](Constants/Portrait.md)  
## [Sprites 精灵类型](Constants/Sprites.md)  
## [StatText 字符串列表](Constants/StatText.md)

