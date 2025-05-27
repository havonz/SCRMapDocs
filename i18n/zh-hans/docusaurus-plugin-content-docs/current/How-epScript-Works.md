---
sidebar_position: 6
---

# epScript 实现解析

<br />

- [万物皆触发器](#%E4%B8%87%E7%89%A9%E7%9A%86%E8%A7%A6%E5%8F%91%E5%99%A8)
- [虚拟触发器（Virtual Triggers）](#%E8%99%9A%E6%8B%9F%E8%A7%A6%E5%8F%91%E5%99%A8virtual-triggers)
- [数学运算](#%E6%95%B0%E5%AD%A6%E8%BF%90%E7%AE%97)
    - [数字修改方法说明](#%E6%95%B0%E5%AD%97%E4%BF%AE%E6%94%B9%E6%96%B9%E6%B3%95%E8%AF%B4%E6%98%8E)
- [变量（EUDVariable）实现](#%E5%8F%98%E9%87%8Feudvariable%E5%AE%9E%E7%8E%B0)
    - [变量（EUDVariable）存在的形式](#%E5%8F%98%E9%87%8Feudvariable%E5%AD%98%E5%9C%A8%E7%9A%84%E5%BD%A2%E5%BC%8F)
    - [将变量的值传入到其它条件或动作](#%E5%B0%86%E5%8F%98%E9%87%8F%E7%9A%84%E5%80%BC%E4%BC%A0%E5%85%A5%E5%88%B0%E5%85%B6%E5%AE%83%E6%9D%A1%E4%BB%B6%E6%88%96%E5%8A%A8%E4%BD%9C)
    - [为什么一个 EUDVariable 在内存中占用 72 字节？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%80%E4%B8%AA-eudvariable-%E5%9C%A8%E5%86%85%E5%AD%98%E4%B8%AD%E5%8D%A0%E7%94%A8-72-%E5%AD%97%E8%8A%82)
    - [变量操作优化](#%E5%8F%98%E9%87%8F%E6%93%8D%E4%BD%9C%E4%BC%98%E5%8C%96)
- [字符串（Db 或 StringBuffer）、轻数组（EUDArray）及轻变量（EUDLightVariable）](#%E5%AD%97%E7%AC%A6%E4%B8%B2db-%E6%88%96-stringbuffer%E8%BD%BB%E6%95%B0%E7%BB%84eudarray%E5%8F%8A%E8%BD%BB%E5%8F%98%E9%87%8Feudlightvariable)
    - [存在的形式](#%E5%AD%98%E5%9C%A8%E7%9A%84%E5%BD%A2%E5%BC%8F)
    - [内存读取或拷贝](#%E5%86%85%E5%AD%98%E8%AF%BB%E5%8F%96%E6%88%96%E6%8B%B7%E8%B4%9D)

<br />

- ## 万物皆触发器

    星际争霸重制版的地图并不支持任何运行时脚本语言，epScript 脚本（`*.eps`） 之所以可以在运行时生效  
    是因为 epScript 代码最终都将以触发器的形式插入地图中，真正能在运行时生效的是触发器  

    > 触发器字节码结构参考：  
    [http://www.staredit.net/wiki/index.php/Scenario.chk#.22TRIG.22_-_Triggers](http://www.staredit.net/wiki/index.php/Scenario.chk#.22TRIG.22_-_Triggers)  
    [https://github.com/phu54321/TrigEditPlus/blob/master/TrigEditPlus/Editor/TriggerEditor.h#L71](https://github.com/phu54321/TrigEditPlus/blob/master/TrigEditPlus/Editor/TriggerEditor.h#L71)  

    <br />

- ## 虚拟触发器（Virtual Triggers）

    因为 [Scenario.chk](http://www.staredit.net/wiki/index.php/Scenario.chk) 中的 [TRIG 节（Section）](http://www.staredit.net/wiki/index.php/Scenario.chk#.22TRIG.22_-_Triggers)中的触发器不会作为一个整体加载到内存，而是以链表形式加载，运行时需要通过链表上的节点遍历定位。  
    [jjf28](http://www.staredit.net/topic/17546/#1) 发帖称只需要将触发器的字节码写入到内存中任何能访问的位置，然后将其添加到[触发器链表](https://armoha.github.io/eud-book/offsets/Player1TriggerList.html)中，它们就会正常工作。  
    这些不在 TRIG 节（Section）中的触发器可以在运行期确定其在内存中的相对位置，这意味着在这样的触发器之间实现定位跳转是相对容易的事情。[jjf28](http://www.staredit.net/topic/17546/#1) 将这样的触发器称之为虚拟触发器（Virtual Triggers）。  
    [trgk](http://www.staredit.net/topic/17546/#11) 提出 [STR 节（Section）](http://www.staredit.net/wiki/index.php/Scenario.chk#.22STR_.22_-_String_Data)在运行时会作为一个整体加载到内存中，因此若将虚拟触发器写入到 STR 节（Section）则可轻易在编译期固定其运行时内存相对位置，从而能更容易地实现运行时对触发器进行动态的修改以实现条件判断和流程控制。  
    在此基础之上，[trgk](http://www.staredit.net/topic/17546/#11) 设计出条件控制流程的 Python 伪语法库 [eudplib](https://github.com/armoha/eudplib)。  

    > 参考：[http://www.staredit.net/topic/17546/](http://www.staredit.net/topic/17546/)
    
    <br />

- ## 数学运算

    常规触发器是不具备完整的数学运算功能的。  
    可以用于模拟数学运算的功能是触发器动作（[Actions](http://www.staredit.net/wiki/index.php/Scenario.chk#Trigger_Actions_List)）中设置数字修改方法（[Number Modifiers](http://www.staredit.net/wiki/index.php/Scenario.chk#Number_Modifiers)）的设为（SetTo）/增加（Add）/减少（Substract）方法 —— 它们通常用于设置/增加/减少玩家资源或者死亡数等等。  
    再加上在重制版中暴雪的软件工程师 [Elias Bachaalany](https://starcraft.fandom.com/wiki/Elias_Bachaalany) 给 Deaths 条件 和 SetDeaths 动作 增加了 bitmask 参数 —— DeathsX 和 SetDeathsX。  
    基于这些和上一节[虚拟触发器（Virtual Triggers）](#虚拟触发器virtual-triggers)实现的自由的触发器流程控制，eudplib 的作者在 epScript 实现了基本的整数运算方法。  
    - ### 数字修改方法说明

        增加（Add）方法如果超过 4 字节范围（0xFFFFFFFF）则会回归到 0 再开始  
        ```JavaScript
        var a = 0xFFFFFFFF;
        println("a == {}", a); // a == 4294967295
        DoActions(a.AddNumber(5));
        println("a == {}", a); // a == 4
        ```

        减少（Substract）方法的限制是最多会把数字减少到 0，即使减数大于被减数  
        ```JavaScript
        var a = 10;
        DoActions(a.SubtractNumber(200000));
        println("a == {}", a); // a == 0
        ```

    <details>

    <summary>epScript 中 减法、乘法、除法、乘方、平方根 的实现模拟代码</summary>

    ```JavaScript
    // 此代码仅用于原理演示，没有处理任何边界问题，实际项目请使用已由 epScript 实现的运算符和函数

    // 相反数原理 参考源码：https://github.com/armoha/eudplib/blob/master/eudplib/core/variable/vbase.py#L125
    function my_neg(x) {
        RawTrigger(actions = list(
            // 反码 + 1 = 补码
            x.AddNumberX(0xFFFFFFFF, 0x55555555),
            x.AddNumberX(0xFFFFFFFF, 0xAAAAAAAA),
            x.AddNumber(1),
        ));
        return x;
    }

    // 绝对值原理 参考源码：https://github.com/armoha/eudplib/blob/master/eudplib/core/variable/vbase.py#L138
    function my_abs(x) {
        if (x >= 0x80000000) { // 符号位为 1
            return my_neg(x);
        }
        return x;
    }

    // Substract 不能用于计算得数小于 0 的减法，减法是使用 Add 方法加上减数的补码（即相反数）实现的
    // 减法原理 参考源码：https://github.com/armoha/eudplib/blob/master/eudplib/core/variable/eudv.py#L336
    function my_minus(a, b) {
        b = my_neg(b);
        return a + b;
    }

    // 乘法原理 参考源码：https://github.com/armoha/eudplib/blob/master/eudplib/core/calcf/muldiv.py#L384
    function my_mul(a, b) {
        var ret = 0;
        foreach (i : py_range(32)) {
            if (a & (1 << i)) {
                ret += b << i;
            }
        }
        return ret;
    }

    // 除法原理 参考源码：https://github.com/armoha/eudplib/blob/master/eudplib/core/calcf/muldiv.py#L421
    function my_div(a, b) {
        var ret = 0;
        foreach (i : py_range(31, -1, -1)) {
            if (a >= (b << i)) {
                ret += (1 << i);
                a += my_neg(b << i);
            }
        }
        return ret, a;
    }

    // 乘方原理 参考源码：https://github.com/armoha/eudplib/blob/master/eudplib/eudlib/mathf/pow.py#L14
    function my_pow(a, b) {
        var ret, _2n = 1, 1;
        while (_2n <= b) {
            if (b.AtLeastX(1, _2n)) {
                ret = my_mul(ret, a);
            }
            _2n += _2n;
            a = my_mul(a, a);
        }
        return ret;
    }

    // 平方根原理 参考源码：https://github.com/armoha/eudplib/blob/master/eudplib/eudlib/mathf/sqrt.py
    function my_sqrt(x) {
        var y = 0;
        foreach (i : py_range(15, -1, -1)) {
            y += py_pow(2, i);
            if (my_pow(y, 2) > x) {
                y += my_neg(py_pow(2, i));
            }
        }
        return y;
    }
    ```
    </details>

    <br />

- ## 变量（EUDVariable）实现

    参考来源：[https://cafe.naver.com/edac/74507](https://cafe.naver.com/edac/74507)


    - ### 变量（EUDVariable）存在的形式

        EUD 触发器本没有变量一说，eudplib 中的运行时变量（EUDVariable）的实质是一个无条件且只有一个 SetDeathsX 动作的虚拟触发器（Virtual Triggers）  
        它使用 TrigEdit++ 的语法描述大概是这样子  
        ```Lua
        一个变量 = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD(目标地址), 数字修改方法, 值, 0, 0xFFFFFFFF);
                --                                   ^
                --                            变量的值保存在这里
            };
        }
        ```

        例如有如下 epScript 代码

        ```JavaScript
        function onPluginStart() {
            var a, b = 4, 7;
            a += b;
        }
        ```

        它大概会被编译成这样的几个触发器（描述性 TrigEdit++ 伪代码，不是可用代码）

        ```Lua
        -- 声明一个变量 a 的触发器块
        a = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD( &(a.值) ),    SetTo,          4 , 0, 0xFFFFFFFF); -- &(a.值) 表示取 a 变量的[值]的地址
                --                  ^           ^             ^
                --               目标地址    数字修改方法        值
            };
            下一个触发器 = b;
        }

        -- 声明一个变量 b 的触发器块
        b = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD( &(b.值) ),    SetTo,         7 , 0, 0xFFFFFFFF); -- &(b.值) 表示取 b 变量的[值]的地址
                --                  ^           ^            ^
                --               目标地址    数字修改方法       值
            };
            下一个触发器 = 赋值的操作;
        }

        赋值的操作 = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetMemory(b.目标地址, SetTo, EPD( &(a.值) )); -- &(a.值) 表示取 a 变量的[值]的地址
                SetMemory(&(b.数字修改方法), SetTo, Add);
                SetMemory(b.下一个触发器, SetTo, 接下来的下一个触发器);
            };
            下一个触发器 = b;
        }

        --[[
        -- 经过 赋值的操作 之后的 b 大概是这样
        b = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetMemory(EPD( &(a.值) ), Add, 7);
            };
            下一个触发器 = 接下来的下一个触发器;
        }
        --]]

        接下来的下一个触发器...
        ```

        可以看到赋值操作也是一个触发器，这个触发器执行时把 b 的目标地址，设置成 a 的值地址，把 b 的方法设置为 Add，然后执行 b 触发器，这样就实现了把 b 的值加到 a 的值上了

        下面用 epScript 实际代码演示上文 epScript 代码变量赋值的过程（原理演示）
        ```JavaScript
        function onPluginStart() {
            // var a, b = 4, 7;
            const a = RawTrigger(actions = SetDeathsX(0, 0, 0, 0, 0)); // var a
            const b = RawTrigger(actions = SetDeathsX(0, 0, 0, 0, 0)); // var b
            const a_动作地址 = a + 8 + 320; const a_下一个触发器地址 = a + 4; const a_Mask地址 = a_动作地址; const a_目标地址 = a_动作地址 + 16; const a_值地址 = a_动作地址 + 20; const a_数字修改方法地址 = a_动作地址 + 24; // 用中文命名对应偏移地址方便理解
            const b_动作地址 = b + 8 + 320; const b_下一个触发器地址 = b + 4; const b_Mask地址 = b_动作地址; const b_目标地址 = b_动作地址 + 16; const b_值地址 = b_动作地址 + 20; const b_数字修改方法地址 = b_动作地址 + 24; // 用中文命名对应偏移地址方便理解
            RawTrigger(actions = list(
                SetMemory(a_值地址, SetTo, 4), // a = 4
                SetMemory(b_值地址, SetTo, 7), // b = 7
            ));

            // a += b;
            const 接下来的下一个触发器 = Forward();
            RawTrigger( // 赋值的操作
                actions = list(
                    SetMemory(b_Mask地址, SetTo, 0xFFFFFFFF),
                    SetMemory(b_目标地址, SetTo, EPD(a_值地址)),
                    SetMemoryX(b_数字修改方法地址, SetTo, ($Add << 24), 0xFF000000),
                    SetMemory(b_下一个触发器地址, SetTo, 接下来的下一个触发器), // 设置 b 的下一个触发器为 “接下来的下一个触发器”
                ),
                nextptr = b, // 本触发器的下一个触发器是 b
            );
            接下来的下一个触发器.__lshift__(NextTrigger()); // “接下来的下一个触发器” 在这

            // 这里只是输出结果，并不是赋值过程的一部分
            println("a:{} b:{}", dwread(a_值地址), dwread(b_值地址)); // a:11 b:7
        }
        ```

    - ### 将变量的值传入到其它条件或动作

        将变量的值传入到其它动作中的实现过程  

        例如有如下 epScript 代码  

        ```JavaScript
        function onPluginStart() {
            var a = 1234;
            Trigger(actions = SetResources(P1, Add, a, Ore)); // 将变量的值传入 SetResources 动作中
        }
        ```

        它大概会被编译成这样的几个触发器（描述性 TrigEdit++ 伪代码，不是可用代码）

        ```Lua
        -- 声明一个变量 a 的触发器块
        a = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD( &(a.值) ),    SetTo,        1234, 0, 0xFFFFFFFF); -- &(a.值) 表示取 a 变量的[值]的地址
                --                  ^           ^            ^
                --               目标地址    数字修改方法       值
            };
            下一个触发器 = 将变量值传递到动作参数操作;
        }

        将变量值传递到动作参数操作 = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetMemory(a.目标地址, SetTo, EPD( &(加矿操作.值) )); -- & 是取地址符 &(加矿操作.值) 表示取 加矿操作 触发器中的[值]的地址
                SetMemory(&(a.数字修改方法), SetTo, SetTo);
                SetMemory(a.下一个触发器, SetTo, 加矿操作);
            };
            下一个触发器 = a;
        }

        --[[
        -- 经过 将变量值传递到动作参数操作 之后的 a 大概是这样
        a = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeathsX(EPD( &(加矿操作.值) ), SetTo, 1234, 0, 0xFFFFFFFF);
            };
            下一个触发器 = 加矿操作;
        }
        --]]

        加矿操作 = Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetResources(P1,    Add,           这里将被更改,  Ore);
                --                   ^                  ^
                --               数字修改方法             值
            };
            下一个触发器 = 接下来的下一个触发器;
        }

        接下来的下一个触发器...
        ```

        下面用 epScript 实际代码解释变量是什么以及它的值是如何传进触发器动作的（原理演示）

        ```JavaScript
        function onPluginStart() {
            // var a = 1234;
            const a = RawTrigger(actions = SetDeathsX(0, 0, 0, 0, 0)); // var a
            const a_动作地址 = a + 8 + 320; const a_下一个触发器地址 = a + 4; const a_Mask地址 = a_动作地址; const a_目标地址 = a_动作地址 + 16; const a_值地址 = a_动作地址 + 20; const a_数字修改方法地址 = a_动作地址 + 24; // 用中文命名对应偏移地址方便理解
            RawTrigger(actions = SetMemory(a_值地址, SetTo, 1234));     // a = 1234
            
            // Trigger(actions = SetResources(P1, Add, a, Ore));
            const 加矿动作 = SetResources(P1, Add, 0, Ore); const 加矿动作_值地址 = 加矿动作 + 20;
            //                                    ^
            //                           变量 a 的值需要传递到这
            const 加矿触发器 = Forward();
            RawTrigger(
                actions = list(
                    SetMemory(a_Mask地址, SetTo, 0xFFFFFFFF),
                    SetMemory(a_目标地址, SetTo, EPD(加矿动作_值地址)),
                    SetMemoryX(a_数字修改方法地址, SetTo, ($SetTo << 24), 0xFF000000),
                    SetMemory(a_下一个触发器地址, SetTo, 加矿触发器), // 设置 a 的下一个触发器为 “加矿触发器”
                ),
                nextptr = a, // 本触发器的下一个触发器是 a
            );
            加矿触发器.__lshift__(RawTrigger(actions = 加矿动作)); // “加矿触发器” 在这
        }
        ```

    - ### 为什么一个 EUDVariable 在内存中占用 72 字节？

        根据前文内容可知，一个 EUDVariable 是一个只含有一条 SetDeathsX 动作的触发器。  
        然而在地图 Scenario.chk 结构中一条触发器需要占用 2400 字节，加之内存里触发器链表节点信息 prevTriggerPtr 和 nextTriggerPtr 占用的 8 字节，在内存中一条触发器应该是占用 2408 字节。  
        为什么又是 72 字节呢？
        <details>

        <summary>可以展开查看触发器节点（TriggerNode）的结构</summary>

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
            uint32_t executionFlags;         /* 0x1 - Ignore Conditions Once, 0x4 - Preserve Trigger, 0x8 - Disabled */
            uint8_t effPlayer[27];
            uint8_t currentAction;
        } Trigger;

        typedef struct { // Trigger node (2408 bytes)
            uint32_t prevTriggerPtr;
            uint32_t nextTriggerPtr;
            Trigger trigger;
        } TriggerNode;
        ```
        </details>

        从结构上看，在内存中，单个触发器节点（TriggerNode）确实占用了 2408 字节。  
        其中前 8 个字节是链表节点结构，之后的 320 字节是 16 个条件，每个条件占用 20 字节，而从第 328 字节开始，就是动作列表，其中 64 个动作每个占用 32 字节。  
        这个结构是固定的，因此，即使只含有一个动作，一个触发器节点也会占用 2408 字节空间。  
        然而，星际争霸1游戏运行时对触发器条件/动作的遍历是遵循短路策略的 —— 遍历条件/动作时遇到第一个空条件/动作便会忽略之后所有的条件/动作。  
        加之一个 EUDVariable 只需要用到一个动作，这意味着用于实现 EUDVariable 触发器中有许多字节是会被忽略闲置的。  
        那么，如何利用这些闲置的空间呢？答案是将多个 EUDVariable 触发器节点叠放在一起，就像扑克牌叠在一起只露出关键部分，人们就能识别这些牌上的内容。  
        现在假设我们有超过 2408 字节的内存空间，我们可以尝试在此之上构建一个假的触发器节点结构。  
        触发器节点的前 4 个字节存储上一个触发器节点（prevTriggerPtr）信息，游戏过程中似乎也没啥用，咱们可以不理它；  
        接下来第 5 到第 8 字节是下一个触发器节点（nextTriggerPtr）信息，这个是有用的，因此这几字节在叠放时不能被覆盖；  
        接着就是第 8 + 4 + 4 + 4 + 2 + 1 + 1 = 24 字节的位置（trigger.conditions\[0\].conditionType）需要设置为 0（第一个条件需要设置为空），叠放时它也不能被其它内容覆盖；  
        然后是触发器第一个动作，在第 8 + 328 + 1 = 329 字节到第 8 + 320 + 32 = 360 字节这个区间（trigger.actions\[0\]），所以这些字节也要做上标记，叠放时也不能被覆盖；  
        接下来我们还要处理触发器的第二个动作，因为它第一个动作不为空，游戏还会接着检测第二个动作，得把第二个动作设为空阻止游戏检测第三个动作，也就是将第 360 + 27 = 387 字节的位置（trigger.actions\[1\].actionType）固定为 0 并且确保其不被其它内容覆盖；  
        最后就是这个触发器节点的第 8 + 320 + 2048 + 1 = 2377 字节（trigger.executionFlags）第三 bit（Preserverd）的内容得是 1 才行，这样一个变量如果不叠放最少也要占用 2377 个字节；  
        而我们需要在 0 到 2376 之间找到一个闲置的偏移位置定义另外一个触发器节点，这个偏移位置需要使得多个重叠的触发器节点的上述关键位置不发生交叉覆盖
        <details>

        <summary>触发器节点识别的关键位置列表</summary>

        ```C
        node[i].nextTriggerPtr                    // 5 ~ 8
        node[i].trigger.condtions[0].condtionType // 24
        node[i].trigger.condtions[0].prop         // 26
        node[i].trigger.actions[0]                // 329 ~ 360
        node[i].trigger.actions[1].actionType     // 387
        node[i].trigger.actions[1].prop           // 389
        node[i].trigger.executionFlags            // 2377
        ```
        </details>

        设偏移位置为 x 那么 x 需要满足的条件是`(5~8) + i * x`、`24 + i * x`、`26 + i * x`、`(329~360) + i * x`、`387 + i * x`、`389 + i * x`、`2377 + i * x`这些位置上的关键内容不会因重叠而被覆盖（i 是叠放的触发器编号）

        <details>

        <summary>解 x 最小值的脚本</summary>

        ```Python
        #!/usr/bin/env python3

        _nextptr = [c for c in range(4 + 1, 8 + 1)]
        _trigger_conditions_0_type_prop = [24, 26]
        _trigger_actions_0 = [c for c in range(328 + 1, 360 + 1)]
        _trigger_actions_1_type_prop = [387, 389]
        _trigger_execution_flags = [2376 + 1]

        _node_plist = _nextptr + _trigger_conditions_0_type_prop + _trigger_actions_0 + _trigger_actions_1_type_prop + _trigger_execution_flags

        def detect(x):
            offsets = set()
            for i in range(500): # 测试叠放触发器个数为 500
                for b in _node_plist:
                    o = b + i * x
                    if o in offsets:
                        return False
                    else:
                        offsets.add(o)
            return True

        for x in range(0, 2376):
            if detect(x):
                print(x)
                break
        ```

        ```Python
        72
        ```

        </details>

        eudplib 里的 x 值是 72，这应该是 eudplib 作者算出来的符合上述条件的最小的整数  
        假设第一个变量触发器的节点的起始位置在 0 这个位置，第二个变量触发器节点就在 72 这个位置，以此类推  

        <details>

        <summary>写个脚本生成一下表格</summary>

        ```Python
        #!/usr/bin/env python3
        x = 72
        print("|{:^12}|{:^17}|{:^19}|{:^17}|{:^19}|{:^12}|".format("VarTrigger", "node", "cond[0].type/prop", "act[0]", "act[1].type/prop", "execflags"))
        for i in range(0, 40):
            ntp1 = 5 + i * x
            ntp2 = 8 + i * x
            ct = 24 + i * x
            a11 = 8 + 320 + 1 + i * x
            a12 = 8 + 320 + 32 + i * x
            a2t = 8 + 320 + 32 + 27 + i * x
            eflags = 2376 + 1 + i * x
            print("|{:^12}| {:^7}~{:^7} |  {:^7}/{:^7}  | {:^7}~{:^7} |  {:^7}/{:^7}  |{:^12}|".format(i, ntp1, ntp2, ct, ct + 2, a11, a12, a2t, a2t + 2, eflags))
        ```

        ```Plain Text
        | VarTrigger |      node       | cond[0].type/prop |     act[0]      | act[1].type/prop  | execflags  |
        |     0      |    5   ~   8    |    24   /  26     |   329  ~  360   |    387  /  389    |    2377    |
        |     1      |   77   ~  80    |    96   /  98     |   401  ~  432   |    459  /  461    |    2449    |
        |     2      |   149  ~  152   |    168  /  170    |   473  ~  504   |    531  /  533    |    2521    |
        |     3      |   221  ~  224   |    240  /  242    |   545  ~  576   |    603  /  605    |    2593    |
        |     4      |   293  ~  296   |    312  /  314    |   617  ~  648   |    675  /  677    |    2665    |
        |     5      |   365  ~  368   |    384  /  386    |   689  ~  720   |    747  /  749    |    2737    |
        |     6      |   437  ~  440   |    456  /  458    |   761  ~  792   |    819  /  821    |    2809    |
        |     7      |   509  ~  512   |    528  /  530    |   833  ~  864   |    891  /  893    |    2881    |
        |     8      |   581  ~  584   |    600  /  602    |   905  ~  936   |    963  /  965    |    2953    |
        |     9      |   653  ~  656   |    672  /  674    |   977  ~ 1008   |   1035  / 1037    |    3025    |
        |     10     |   725  ~  728   |    744  /  746    |  1049  ~ 1080   |   1107  / 1109    |    3097    |
        |     11     |   797  ~  800   |    816  /  818    |  1121  ~ 1152   |   1179  / 1181    |    3169    |
        |     12     |   869  ~  872   |    888  /  890    |  1193  ~ 1224   |   1251  / 1253    |    3241    |
        |     13     |   941  ~  944   |    960  /  962    |  1265  ~ 1296   |   1323  / 1325    |    3313    |
        |     14     |  1013  ~ 1016   |   1032  / 1034    |  1337  ~ 1368   |   1395  / 1397    |    3385    |
        |     15     |  1085  ~ 1088   |   1104  / 1106    |  1409  ~ 1440   |   1467  / 1469    |    3457    |
        |     16     |  1157  ~ 1160   |   1176  / 1178    |  1481  ~ 1512   |   1539  / 1541    |    3529    |
        |     17     |  1229  ~ 1232   |   1248  / 1250    |  1553  ~ 1584   |   1611  / 1613    |    3601    |
        |     18     |  1301  ~ 1304   |   1320  / 1322    |  1625  ~ 1656   |   1683  / 1685    |    3673    |
        |     19     |  1373  ~ 1376   |   1392  / 1394    |  1697  ~ 1728   |   1755  / 1757    |    3745    |
        |     20     |  1445  ~ 1448   |   1464  / 1466    |  1769  ~ 1800   |   1827  / 1829    |    3817    |
        |     21     |  1517  ~ 1520   |   1536  / 1538    |  1841  ~ 1872   |   1899  / 1901    |    3889    |
        |     22     |  1589  ~ 1592   |   1608  / 1610    |  1913  ~ 1944   |   1971  / 1973    |    3961    |
        |     23     |  1661  ~ 1664   |   1680  / 1682    |  1985  ~ 2016   |   2043  / 2045    |    4033    |
        |     24     |  1733  ~ 1736   |   1752  / 1754    |  2057  ~ 2088   |   2115  / 2117    |    4105    |
        |     25     |  1805  ~ 1808   |   1824  / 1826    |  2129  ~ 2160   |   2187  / 2189    |    4177    |
        |     26     |  1877  ~ 1880   |   1896  / 1898    |  2201  ~ 2232   |   2259  / 2261    |    4249    |
        |     27     |  1949  ~ 1952   |   1968  / 1970    |  2273  ~ 2304   |   2331  / 2333    |    4321    |
        |     28     |  2021  ~ 2024   |   2040  / 2042    |  2345  ~ 2376   |   2403  / 2405    |    4393    |
        |     29     |  2093  ~ 2096   |   2112  / 2114    |  2417  ~ 2448   |   2475  / 2477    |    4465    |
        |     30     |  2165  ~ 2168   |   2184  / 2186    |  2489  ~ 2520   |   2547  / 2549    |    4537    |
        |     31     |  2237  ~ 2240   |   2256  / 2258    |  2561  ~ 2592   |   2619  / 2621    |    4609    |
        |     32     |  2309  ~ 2312   |   2328  / 2330    |  2633  ~ 2664   |   2691  / 2693    |    4681    |
        |     33     |  2381  ~ 2384   |   2400  / 2402    |  2705  ~ 2736   |   2763  / 2765    |    4753    |
        |     34     |  2453  ~ 2456   |   2472  / 2474    |  2777  ~ 2808   |   2835  / 2837    |    4825    |
        |     35     |  2525  ~ 2528   |   2544  / 2546    |  2849  ~ 2880   |   2907  / 2909    |    4897    |
        |     36     |  2597  ~ 2600   |   2616  / 2618    |  2921  ~ 2952   |   2979  / 2981    |    4969    |
        |     37     |  2669  ~ 2672   |   2688  / 2690    |  2993  ~ 3024   |   3051  / 3053    |    5041    |
        |     38     |  2741  ~ 2744   |   2760  / 2762    |  3065  ~ 3096   |   3123  / 3125    |    5113    |
        |     39     |  2813  ~ 2816   |   2832  / 2834    |  3137  ~ 3168   |   3195  / 3197    |    5185    |
        ```
        </details>

        从结果上看，它们遵循着一个刚好不会重叠覆盖的周期，VarTrigger\[5+n\].node 的起始位置刚好是 VarTrigger\[n\].act\[0\] 的结束位置，VarTrigger\[5+n\].cond\[0\].prop 的结束后正好是 VarTrigger\[n\].act\[0\].type 的起始位置。  
        
        eudplib 的做法则是，定义一个触发器头，平移 72 字节再定义一个触发器头，第二个触发器头在第一个触发器的条件区块中，以此类推，这些交错在一起的触发器不会发生碰撞。  

        每个变量 72 字节占用就是这么来的，最后一个变量 + 2376 的位置还得有个 Preserved 标志，也就是如果有 100 个变量，占用的字节数是 (100 - 1) * 72 + 2376 字节。  


    - ### 变量操作优化

        变量是使用触发器模拟实现的，对变量的赋值、运算等操作都会生成操作触发器

        例如如下代码

        ```JavaScript
        function afterTriggerExec() {
            var a, b = 3, 5;
            b += 1;
            a += b + 1;
            // 结果是 a:10 b:6
        }
        ```

        其中的 `b += 1; a += b + 1;` 这两行代码可能会生成 6 个额外的触发器  
        这种代码生成规模是 eudplib 实现方法所导致的，它是可以被优化的  
        它显然用不着使用这么多个触发器，依照我们对变量的实现原理了解，它这么写也是可以的  

        ```JavaScript
        // 变量的 .getDestAddr() 方法能在编译期获得变量触发器中的 目标地址
        // 变量的 .getValueAddr() 方法能在编译期获得变量触发器中的 值地址
        // 变量的 .GetVTable() 方法能在编译期获得变量的虚拟触发器地址
        // 变量的 .SetModifier(method) 方法设置变量触发器中的数字修改方法为 method
        // SetNextPtr(trg, ptr) 函数用于设置触发器 trg 的下一个触发器为 ptr
        function afterTriggerExec() {
            var a, b = 3, 5;
            const next = Forward();
            RawTrigger(
                actions = list(
                    SetMemory(b.getValueAddr(), Add, 1),
                    SetMemory(a.getValueAddr(), Add, 1),
                    SetMemory(b.getDestAddr(), SetTo, EPD(a.getValueAddr())),
                    b.SetModifier(Add), // 它内部实现大概是 SetMemoryX(b.getValueAddr() + 4, SetTo, (Add << 24), 0xFF000000)
                    SetNextPtr(b.GetVTable(), next), // 设置 b 的下一个触发器为 next，它的内部实现可能是这样的 SetMemory(b.GetVTable() + 4, SetTo, next)
                ),
                nextptr = b.GetVTable(), // 本触发器的下一个触发器为 b
            );
            next.__lshift__(NextTrigger()); // 这个的实质是将 next 这个 Forward 指向下一个 Trigger
            // 结果是 a:10 b:6
        }
        ```
        以上代码仅使用 1 个额外的触发器就对 b 完成一次自增运算以及对 a 完成了在 b 自增后的基础上两次自增赋值运算  
        而 eudplib 专门为此种场景提供了 VProc 函数，它会包含一个 RawTrigger 并且在该 RawTrigger 执行后执行指定变量的虚拟触发器（变量也是一个触发器）以确保在当前 RawTrigger 对变量的虚拟触发器进行更改之后还能将每个变量更改后的的虚拟触发器逐个执行，而不再需要写回跳转的代码，以上代码就可以精简为  
        ```JavaScript
        function afterTriggerExec() {
            var a, b = 3, 5;
            VProc(
                list(b), // 当下面那个 list 中的动作执行完毕之后，就会自动跳转到 b.GetVTable() 这个触发器，执行完了之后再跳转到 NextTrigger()
                list(
                    SetMemory(b.getValueAddr(), Add, 1),
                    SetMemory(a.getValueAddr(), Add, 1),
                    SetMemory(b.getDestAddr(), SetTo, EPD(a.getValueAddr())),
                    b.SetModifier(Add), // 它内部实现大概是 SetMemoryX(b.getValueAddr() + 4, SetTo, (Add << 24), 0xFF000000)
                ),
            );
            // 结果是 a:10 b:6
        }
        ```

        上面的代码仍然可以继续精简，因为 eudplib 还提供几个整合操作的方法，直接上代码

        ```JavaScript
        function afterTriggerExec() {
            var a, b = 3, 5;
            VProc(
                list(b), // 当下面那个 list 中的动作执行完毕之后，就会自动跳转到 b.GetVTable() 这个触发器，执行完了之后再跳转到 NextTrigger()
                list(
                    b.AddNumber(1),
                    a.AddNumber(1),
                    b.QueueAddTo(a),
                ),
            );
            // 结果是 a:10 b:6
        }
        ```
    <br />

- ## 字符串（Db 或 StringBuffer）、轻数组（EUDArray）及轻变量（EUDLightVariable）

    地图中的字符串都将会被存入到 STR 节中，通常这些字符串是不可变的，但是有 EUD 就不一样了  
    咱们先不考虑 STR 节的数据结构，它大概可以使用很大很大的内存空间，通常是够用的  

    - ### 存在的形式

        相较于普通变量（EUDVariable）而言，字符串的结构就非常简单粗暴。  
        字符串就是 ASCII 字符的数组，其所占据的字节数即为包含的 ASCII 字符数量。  
        与字符串类似，轻变量也没有复杂的结构，它只占据连续的`4`个字节，表示 32 位整数。  
        轻数组是多个轻变量组成的数组，它占用连续的`数组尺寸 * 4`字节。  

    - ### 内存读取或拷贝

        使用 SetDeathsX 动作可以轻松地改变字符串的每个字节的值，类似于普通变量的操作。  
        然而，读取传递字符串的值则比较麻烦，因为没有一个可以从内存位置读取或者拷贝值的条件或动作（EUDVariable 的值传递并不依赖于内存读取）。  
        唯一可用的是判断内存位置的值是否大于/小于/等于某个值的条件 DeathsX。  
        我们可以思考一下，在传统触发器中如何将机枪兵的死亡数赋值给小狗的死亡数。  
        
        用传统触发器将机枪兵的死亡数赋值给小狗的死亡数（TrigEdit++ 代码）  
        ```Lua
        -- 先用一个触发器将小狗和彩蛇鸟的死亡数量设置为 0
        Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeaths(P1, SetTo, 0, "Zerg Zergling");
                SetDeaths(P1, SetTo, 0, "Kakaru");
            };
        }

        -- 使用 32 个触发器把机枪兵的死亡数转移到小狗和彩蛇鸟的死亡数上（机枪兵减多少，小狗和彩蛇鸟就加多少）
        for i = 31, 0, -1 do
            Trigger {
                conditions = {Deaths(P1, AtLeast, 2^i, "Terran Marine");}; -- 分别判断机枪兵的数量是否大于等于 2 的 31 到 0 次方
                actions = {
                    SetDeaths(P1, Add, 2^i, "Zerg Zergling");      -- 如果条件满足，给小狗的数量也加上 2 的 31 到 0 次方
                    SetDeaths(P1, Add, 2^i, "Kakaru");             -- 彩蛇鸟的数量和小狗同步
                    SetDeaths(P1, Subtract, 2^i, "Terran Marine"); -- 同时把机枪兵数量减去这个数
                };
            }
        end

        -- 使用 32 个触发器把彩蛇鸟的数量递减给机枪兵
        for i = 31, 0, -1 do
            Trigger {
                conditions = {Deaths(P1, AtLeast, 2^i, "Kakaru");};
                actions = {
                    SetDeaths(P1, Add, 2^i, "Terran Marine");
                    SetDeaths(P1, Add, 2^i, "Kakaru");
                };
            }
        end
        ```

        这个赋值操作用了 65 个触发器和一个中间单位彩蛇鸟来完成  
        1. 用一个触发器将小狗的死亡数和辅助的彩蛇鸟的死亡数归零  
        2. 使用 32 个触发器将机枪兵的死亡数用二进制位递减的方式转移到小狗和彩蛇鸟的死亡数上  
        3. 将彩蛇鸟的死亡数用二进制递减的方转移回机枪兵的死亡数上  
        

        在星际争霸重制版中，Deaths 是支持 bitmask 判断的（通常管它叫 DeathsX）  
        使用 DeathsX 条件和 SetDeaths 动作将机枪兵的死亡数赋值给小狗的死亡数（TrigEdit++ 代码）  
        ```Lua
        -- 先将小狗的死亡数量设置为 0
        Trigger {
            players = {P1};
            conditions = {Always();};
            actions = {
                SetDeaths(P1, SetTo, 0, "Zerg Zergling");
            };
        }

        -- 将机枪兵的的数量的每一位都附加到小狗的死亡数对应的位上
        for i = 0, 31 do
            Trigger {
                conditions = {DeathsX(P1, AtLeast, 1, "Terran Marine", 2^i);};
                actions = {
                    SetDeaths(P1, Add, 2^i, "Zerg Zergling");
                };
            }
        end
        ```

        这个赋值操作用了 33 个触发器来完成  
        1. 用一个触发器将小狗的死亡数归零  
        2. 使用 32 个触发器将机枪兵的死亡数用二进制位判断法将对应的二进制位附加到小狗的死亡数上  

        以上我们用经典触发器实现了单位死亡数值的传递。  

        因为单位死亡数的本质是一个 32 位整数，而 EUD 技术使得我们可以用 Deaths 或 SetDeaths 访问到单位死亡数之外的数据，这使得我们也可以使用该方式读取或拷贝内存中的其它地方的值。  

        用 epScript 模拟实现一个 dwread_epd（[ dwread_epd 源码 ](https://github.com/armoha/eudplib/blob/master/eudplib/eudlib/memiof/dwepdio.py#L47)）

        ```JavaScript
        // 这个函数在 epScript 已经存在，此代码仅用于 32 位正整数除法原理演示，没有处理任何边界问题
        function my_dwread_epd(playerid) {
            var ret = 0;
            foreach(i : py_range(32)) {
                Trigger(
                    conditions = DeathsX(playerid, AtLeast, 1, 0, py_pow(2, i)),
                    actions = ret.AddNumber(py_pow(2, i)),
                );
            }
            return ret;
        }
        ```

        这样我们就用了 32 个触发器模拟了一个读取指定 epd 位置的 dword 的值  
        它的细节实现还涉及内存地址是否为 4 的倍数等问题  
        例如我们知道玩家编号为 5004 对应的内存地址为 0x6557E0，玩家编号为 5005 对应的内存地址为 0x6557E4  

        |玩家编号|...|5003|5004|5005|5006|...|
        |:-:|:-:|:-:|:-:|:-:|:-:|:-:|
        |内存地址|...|0x6557DC|0x6557E0|0x6557E4|0x6557E8|...|
        |内存值|...|11223344|5566`7788`|`99AA`BBCC|DDEEFF00|...|

        要读取内存地址 0x6557E2 的 dword，Deaths 能接受的参数只能是玩家编号，这里假设它的值是 0xAA998877（为啥是反的？参考[字节序#小端序](https://zh.wikipedia.org/wiki/%E5%AD%97%E8%8A%82%E5%BA%8F#%E5%B0%8F%E7%AB%AF%E5%BA%8F)）  
        这种情况就需要读取 5004 的后半部分和 5005 的前半部分  
        其次对小于 4 字节的内存数据的读取拷贝还有更多的实现细节  
        当然这里我们只是阐述原理，需要更多的细节可以直接阅读 eudplib 的源代码  
