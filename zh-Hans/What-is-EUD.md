# EUD 的概念

EUD 几乎是一切玩法的开端

## EUD 或 EPD

### EUD 起源
EUD 是 Extended Unit Death （单位死亡数触发器扩展使用技术）的缩写，这项技术源自于《星际争霸》 1.08 及以前版本地图编辑器的触发编辑器中的 Deaths 条件及 SetDeaths 动作存在的缓冲区溢出漏洞。  

- Deaths 条件和 SetDeaths 动作的描述性原型如下  
    ```CSS
    Trigger {
        Conditions:
            Deaths(玩家编号, 不少于/不多余/完全是, 数值, 单位编号);
        Actions:
            SetDeaths(玩家编号, 增加/减少/设为, 数值, 单位编号);
    }
    ```
    - 玩家编号 合法范围是：0~26，实际为一个 dword 值，取值范围是 `-2147483648~2147483647`  
    - 单位编号 合法范围是：0~232，实际为一个 word 值，取值范围是 `0 ~ 65535`  

在使用非法的 玩家编号 和 单位编号 的情况下，Deaths 和 SetDeaths 动作依然可以在游戏中生效，这样一来就实现了任意位置存的读写。大家通过尝试发现，Deaths 和 SetDeaths 溢出的内存位置是固定的，会访问到的内存位置是`0x58A364 + 4 * 玩家编号 + 48 * 单位编号`，当然也可以简单的将单位编号设置为 0，仅使用玩家编号做溢出锚点可以访问`0x58A364 + 4 * 玩家编号`的内存位置的 32 位数值。在访问小于 4 个字节单元的时候，就需要先读取 4 个字节，然后再用算法将 4 个字节分开（当然重制版中不需要考虑这个详见[下文](#重制版中的-eud)）。  

EPD 是指在利用该项技术的时候，使用玩家编号作为锚点偏移进行缓冲区溢出时，这个用于访问特定内存位置的 “特殊的玩家编号” 叫 EPD  
含有至少一条 Deaths 或者 SetDeaths 中的玩家编号或者单位编号溢出访问的地图被称为 EUD 地图。  


### 重制版中的 EUD
星际争霸重制版（SC:Remastered）修复了上文提到的 Deaths/SetDeaths 漏洞，因此在重制版刚刚发布时，已经不再有任何 EUD 功能。但是暴雪的软件工程师 [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) 在重制版发布不久后便开发出了“重制版 EUD 模拟器”，该功能伴随《星际争霸》[1.21.0 版](https://news.blizzard.com/en-gb/starcraft/21313396/patch-1-21-0-the-return-of-eud-maps) 于 2017 年 12 月一并发布。自此之后，星际争霸重制版在遇到了含有 EUD 触发的地图时，会自动启用 EUD 模拟器来执行地图中的触发，这使得作者依然可以像以前一样通过 Deaths/SetDeaths 触发来在星际争霸重制版实现 EUD 功能。  
不过在星际争霸重制版中，暴雪限制了地图触发对于内存的读写：有的内存只可读取、不可写入，有的内存不可读取或写入，只有少部分内存既可读取又可写入，详见[内存表](https://ldconval.github.io/eudtools/Include/EUDDB.html)。如果EUD地图在游戏中运行某条触发时读取或写入了非法内存，则游戏立即被终止（弹窗报错：抱歉，这张 EUD 地图现在不被支持......错误码是一个十六进制数，用 0xFFFFFFFF 减去这个十六进制数得到的结果就是当前触发正在尝试读取或写入的非法内存地址）。这就导致重制版 EUD 技术在功能上有了诸多限制，比如重制版无法修改任何与模型、图像相关的东西，比如无法拓展单位上限，比如无法将 1.08 的 EUD 综合插件照搬到重制版等等。   
在星际争霸重制版中，EUD 地图拥有以下特点：  
- 单位上限只能为原版的 1700，而不是拓展的单位上限 3400。（建立主机时，“单位上限” 选项整个一行均为灰色，并被强制选择为 “原版”，而无法选择 “拓展”）  
- 无法在游戏中保存游戏  
- 游戏结束后无法保存录像  
- 游戏以胜利结束后在得分界面仍然会显示战败  

在重制版中暴雪的软件工程师 [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) 给 Deaths 条件 和 SetDeaths 动作 增加了 bitmask 参数  
使用 bitmask 的条件和动作能够更加高效地判断和写入不是 4 的倍数的内存地址上的任意字节内容  
ScmDraft2 或是 euddraft 将这种用法命名成 DeathsX 和 SetDeathsX  
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

    

[RECON-BRX-2018-Starcraft-Emulating-a-buffer-overflow-for-fun-and-profit.pdf](res/RECON-BRX-2018-Starcraft-Emulating-a-buffer-overflow-for-fun-and-profit.pdf)

  



