---
sidebar_position: 2
---

# EUD 的概念

EUD 几乎是一切玩法的开端

## EUD 或 EPD

### EUD 起源
EUD 是 Extended Unit Death（单位死亡数触发器扩展使用技术）的缩写。这项技术源自《星际争霸》1.08 及以前版本地图编辑器的触发编辑器中 Deaths 条件和 SetDeaths 动作存在的缓冲区溢出漏洞。  

- Deaths 条件和 SetDeaths 动作的描述性原型如下  
    ```CSS
    Trigger {
        Conditions:
            Deaths(玩家编号, 不少于/不多余/完全是, 数值, 单位编号);
        Actions:
            SetDeaths(玩家编号, 增加/减少/设为, 数值, 单位编号);
    }
    ```
    - 玩家编号的合法范围是：0 ~ 26，实际为一个 dword 值，取值范围是 `-2147483648 ~ 2147483647`  
    - 单位编号的合法范围是：0 ~ 232，实际为一个 word 值，取值范围是 `0 ~ 65535`  

在使用非法玩家编号和单位编号的情况下，Deaths 和 SetDeaths 动作依然可以在游戏中生效，从而实现任意内存位置的读写。经过尝试可以发现，Deaths 和 SetDeaths 溢出后访问的内存位置是固定的，即 `0x58A364 + 4 * 玩家编号 + 48 * 单位编号`。当然，也可以简单地将单位编号设置为 0，仅使用玩家编号作为溢出锚点，访问 `0x58A364 + 4 * 玩家编号` 位置上的 32 位数值。访问小于 4 字节的数据时，需要先读取 4 个字节，再用算法拆出目标字节（重制版中不需要考虑这一点，详见[下文](#重制版中的-eud)）。  

EPD 是指在利用这项技术时，用玩家编号作为锚点偏移进行缓冲区溢出；这个用于访问特定内存位置的 “特殊玩家编号” 就叫 EPD。  
含有至少一条通过 Deaths 或 SetDeaths 的玩家编号或单位编号进行溢出访问的地图，称为 EUD 地图。  


### 重制版中的 EUD
星际争霸重制版（SC:Remastered）修复了上文提到的 Deaths/SetDeaths 漏洞，因此在重制版刚发布时，已经不再有任何 EUD 功能。后来，暴雪软件工程师 [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) 在重制版发布不久后开发出了“重制版 EUD 模拟器”，该功能随《星际争霸》[1.21.0 版](https://news.blizzard.com/en-gb/starcraft/21313396/patch-1-21-0-the-return-of-eud-maps) 于 2017 年 12 月一并发布。自此之后，星际争霸重制版在遇到含有 EUD 触发的地图时，会自动启用 EUD 模拟器来执行地图中的触发，使作者依然可以像以前一样，通过 Deaths/SetDeaths 触发在星际争霸重制版中实现 EUD 功能。  
不过在星际争霸重制版中，暴雪限制了地图触发对内存的读写：有的内存只可读取、不可写入，有的内存不可读取也不可写入，只有少部分内存既可读取又可写入，详见[内存表](https://ldconval.github.io/eudtools/Include/EUDDB.html)。如果 EUD 地图在游戏中运行某条触发时读取或写入了非法内存，游戏会立即终止（弹窗报错：抱歉，这张 EUD 地图现在不被支持......错误码是一个十六进制数，用 0xFFFFFFFF 减去这个十六进制数，得到的结果就是当前触发正在尝试读取或写入的非法内存地址）。这使重制版 EUD 技术在功能上受到不少限制，例如无法修改与模型、图像相关的内容，无法拓展单位上限，也无法将 1.08 的 EUD 综合插件直接照搬到重制版。   
在星际争霸重制版中，EUD 地图拥有以下特点：  
- 单位上限只能为原版的 1700，而不是拓展单位上限 3400。（建立主机时，“单位上限” 选项整行均为灰色，并被强制选择为 “原版”，无法选择 “拓展”）  
- 无法在游戏中保存游戏  
- 游戏结束后无法保存录像  
- 游戏以胜利结束后在得分界面仍然会显示战败  

在重制版中，暴雪软件工程师 [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) 为 Deaths 条件和 SetDeaths 动作增加了 bitmask 参数。  
使用 bitmask 的条件和动作，可以更高效地判断和写入非 4 字节对齐内存地址上的任意字节内容。  
ScmDraft2 和 euddraft 将这种用法命名为 DeathsX 和 SetDeathsX。  
- DeathsX 和 SetDeathsX 原理参考  
    ```C
    typedef struct { /* 20 bytes */
        uint32_t locationID;
        uint32_t playerID;
        uint32_t num;         // Qualified number (how many/resource amount)
        uint16_t unitID;
        uint8_t comparison;   // Numeric comparison, switch state
        uint8_t condtionType; // http://www.staredit.net/wiki/index.php/Scenario.chk#Trigger_Conditions_List
        uint8_t resType;      // Resource type, score type, Switch number (0-based)
        uint8_t prop;
        uint8_t maskFlag[2];
    } TriggerCondition;

    typedef struct { /* 32 bytes */
        uint32_t locationID;
        uint32_t stringID;
        uint32_t wavNameID;
        uint32_t time;
        uint32_t playerID;
        uint32_t target;    // Second group affected, secondary location (1-based), CUWP #, number, AI script (4-byte string), switch (0-based #)
        uint16_t resType;   // Unit type, score type, resource type, alliance status
        uint8_t actionType; // http://www.staredit.net/wiki/index.php/Scenario.chk#Trigger_Actions_List
        uint8_t num;        // Number of units (0 means All Units), action state, unit order, number modifier
        uint8_t prop;
        uint8_t padding;
        uint8_t maskFlag[2];
    } TriggerAction;

    // http://www.staredit.net/wiki/index.php/Scenario.chk#.22TRIG.22_-_Triggers
    typedef struct {
        TriggerCondition conditions[16]; /* 320 bytes */
        TriggerAction actions[64];       /* 2048 bytes */
        uint32_t flag;
        uint8_t effPlayer[27];
        uint8_t currentAction;
    } Trigger;

    typedef struct { // Trigger node (2408 bytes)
        uint32_t prevTriggerPtr;
        uint32_t nextTriggerPtr;
        Trigger trigger;
    } TriggerNode;

    TriggerNode *tnode = calloc(sizeof(TriggerNode));

    // DeathsX 条件
    tnode->trigger.conditions[0].condtionType = 15; // 15 = Deaths
    tnode->trigger.conditions[0].maskFlag = {'S', 'C'}; // 启用 bitmask
    tnode->trigger.conditions[0].locationID = 这里就是bitmask;

    // SetDeathsX 动作
    tnode->trigger.actions[0].actionType = 45; // 45 = Set Deaths
    tnode->trigger.actions[0].maskFlag = {'S', 'C'}; // 启用 bitmask
    tnode->trigger.actions[0].locationID = 这里就是bitmask;
    ```

    

[RECON-BRX-2018-Starcraft-Emulating-a-buffer-overflow-for-fun-and-profit.pdf](/pdf/RECON-BRX-2018-Starcraft-Emulating-a-buffer-overflow-for-fun-and-profit.pdf)

  


