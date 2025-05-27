---
sidebar_position: 5
---

# 字符串说明


<br />

- 字符串说明
    - [编译期字符串（py_str）](#%E7%BC%96%E8%AF%91%E6%9C%9F%E5%AD%97%E7%AC%A6%E4%B8%B2py_str)
    - [地图字符串（TrgString）](#%E5%9C%B0%E5%9B%BE%E5%AD%97%E7%AC%A6%E4%B8%B2trgstring)
    - [字符串数据（Db）](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%95%B0%E6%8D%AEdb)
    - [TBL 字符串（stat_txt.tbl）](#tbl-%E5%AD%97%E7%AC%A6%E4%B8%B2stat_txttbl)

<br />

- ## 编译期字符串（py_str）

    编译期字符串是指在编译期产生的最终不会插入地图的字符串，它包含所有的字面量字符串（literal string）以及编译期的字符串常量  
    因为 epScript 的编译期是用 Python 实现的，所以将它称为 py_str，编译期字符串不能直接用于地图运行时  

    <details>

    <summary>当函数的参数为下列类型时，epScript 会将传入的编译期字符串参数转换成对应的常量编号</summary>

    - [TrgUnit](Constants-Reference/TrgUnit.md)  
    - [TrgLocation](Constants-Reference/Constants-Reference.md#trglocation-位置区域)  
    - [TrgSwitch](Constants-Reference/Constants-Reference.md#trgswitch-开关)  
    - [TrgAIScript](Constants-Reference/TrgAIScript.md)  
    - [Weapon](Constants-Reference/Weapon.md)  
    - [Tech](Constants-Reference/Tech.md)  
    - [Upgrade](Constants-Reference/Upgrade.md)  
    - [UnitOrder](Constants-Reference/UnitOrder.md)  
    - [Flingy](Constants-Reference/Flingy.md)  
    - [Image](Constants-Reference/Image.md)  
    - [Icon](Constants-Reference/Icon.md)  
    - [Iscript](Constants-Reference/Iscript.md)  
    - [Portrait](Constants-Reference/Portrait.md)  
    - [Sprites](Constants-Reference/Sprites.md)  
    - [StatText](Constants-Reference/StatText.md)  
    </details>

    举例子说明

    ```JavaScript
    SetDeaths(P1, Add, 10, "Terran Marine"); // SetDeaths 动作的对应的参数的类型是 TrgUnit，所以它会在编译期被替换成 TrgUnit 表中对应的整数编号传入
    ```

    上述代码中的 "Terran Marine" 这个字符串是不会插入到地图中的

    "Terran Marine" 只是用于编译期查询该单位类型的编号（机枪兵的编号为 0）

    如果不考虑人类可读性，上述代码完全等价于如下代码

    ```JavaScript
    SetDeaths(P1, Add, 10, 0);
    ```

    在 epScript 中也可以使用 py_str 声明一个编译期字符串常量

    ```JavaScript
    const csTerran_  = py_str("Terran ");
    const csMarine   = py_str("Marine");
    const csGoliath  = py_str("Goliath");
    py_print(csMarine, " ", csGoliath); // 在编译时的黑框中输出 Marine Goliath
    SetDeaths(P1, Add, 10, csTerran_ + csGoliath);
    ```

    在编译期可以通过编号索引宏获取编译期字符串在不同的常量表中的编号

    ```JavaScript
    const utTerranMarine = $U("Terran Marine");
    const stTerranMarine = $B("Terran Marine");
    py_print(utTerranMarine, stTerranMarine); // 在编译时的黑框中输出 0 1
    if (utTerranMarine == stTerranMarine) {   // 这里等价于 if (0 == 1) {
        simpleprint("所以这条信息永远不会输出");
    }
    ```

    可以使用 EncodeString 宏将编译期字符串插入到地图中成为一个地图字符串（TrgString）并返回其在地图字符串表中的编号

    ```JavaScript
    const csTerran_  = py_str("Terran ");
    const csMarine   = py_str("Marine");
    const csTerran_Marine  = csTerran_ + csMarine;
    const str_idx = EncodeString(csTerran_Marine * 3);
    DisplayText(str_idx); // 输出：Terran MarineTerran MarineTerran Marine
    ```



- ## 地图字符串（TrgString）

    <details>
    
    <summary>相关类型及函数</summary>

    - TrgString  
    - StringBuffer  
    - $T(cstr : [literal](Syntax.md#字面量字符串literal-string))
    - EncodeString(cstr: py_str)
    - GetStringIndex(cstr: py_str)
    - GetMapStringAddr(str: TrgString)
    </details>
    
    地图字符串（TrgString）是最常用的运行时字符串  
    当我们在地图编辑器中使用了任何自定义字符串时，地图编辑器就会将这个字符串插入到地图文件中的地图字符串表（Map String Table）中并赋予一个编号  
    例如我们给地图标题（Map Title）设为 “一张好玩的星际争霸地图”，这个地图标题就是一个地图字符串（TrgString），它通常会被赋予编号 1  
    触发器动作函数的字符串参数就是这个编号

    ```JavaScript
    DisplayText(1);          // 输出：一张好玩的星际争霸地图
    SetMissionObjectives(1); // 任务目标：一张好玩的星际争霸地图
    ```

    在 epScript 代码中，函数的参数类型为 TrgString 的情况下，假如传入的参数是一个编译期字符串，则该字符串会先被插入到地图的地图字符串表中，然后将它的编号传入该参数位置

    ```JavaScript
    DisplayText("你好星际争霸");
    ```

    以上代码实际相当于

    ```JavaScript
    const str_idx = EncodeString("你好星际争霸");
    DisplayText(str_idx);
    ```

    也可以用 $T 语法写成

    ```JavaScript
    const str_idx = $T("你好星际争霸");
    DisplayText(str_idx);
    ```

    再来个自定义函数接受地图字符串参数的例子

    ```JavaScript
    function test(strId : TrgString) {
        println("字符串编号：{}", strId); // 字符串编号：3
        DisplayText(strId);             // 你好星际争霸
    }

    function onPluginStart() {
        test("你好星际争霸");
    }
    ```

    也可以构造一个很长的编译期空字符串使用 EncodeString 宏转换成地图字符串（TrgString）当作字符串缓冲区充当字符串变量

    ```JavaScript
    const buf_idx = EncodeString(py_str(" ") * 128);
    const buf_addr = GetMapStringAddr(buf_idx);
    for (var i = 1; i < 10; i++) {
        dbstr_print(buf_addr, "第 ", i, " 行");
        DisplayText(buf_idx);
    }
    ```

    当然咱们没有必要自己构建维护地图字符串（TrgString）做字符串缓冲区，使用 StringBuffer 对象可以更加便捷动态管理一个特定长度的地图字符串（TrgString）

    ```JavaScript
    const buf = StringBuffer(127);
    for (var i = 1; i < 10; i++) {
        buf.insert(0, "第 ", i, " 行");
        buf.Display(); // DisplayText(buf.StringIndex);
    }
    ```

    更方便的是，epScript 提供了一系列的 print 函数使得往屏幕上输出文字不用自己维护 StringBuffer

    epScript 中维护着一个全局的容量为 1023 字节的 StringBuffer 用于实现 print 系列函数，也就是说，小于这个长度的字符串输出可以直接使用 print 系列函数

    ```JavaScript
    for (var i = 1; i < 10; i++) {
        simpleprint("第 ", i, " 行");
    }
    ```



- ## 字符串数据（Db）
    <details>
    
    <summary>相关类型及函数</summary>

    - Db  
    - dbstr_addstr(dst, src)  
    - dbstr_addstr_epd(dst, srcepd)  
    - dbstr_adddw(dst, number)  
    - dbstr_addptr(dst, ptr)  
    - dbstr_print(dst, *args)  
    - sprintf(dst, format_string, *args)  
    </details>

    Db 字符串也是一种运行时自定义字符串  
    但是它们不像地图字符串（TrgString）那样有一个编号，所以直接不能应用于传统的触发器动作  
    通常是需要将一些运行时组成字符串的部件使用 Db 存储，它们没有必要每一个都拥有一个编号  
    可以将这些数据拷贝到 StringBuffer 或者其它 TrgString 以传递到传统触发器动作中  

    ```JavaScript
    const buf = Db(128);
    const str = StringBuffer(1024);
    str.insert(0);
    for (var i = 1; i < 10; i++) {
        dbstr_print(buf, "任务目标：第 ", i, " 行");
        if (i < 9)
            str.append(buf, "\n");
        else
            str.append(buf);
    }
    str.Display();
    SetMissionObjectives(str.StringIndex);
    ```



- ## TBL 字符串（stat_txt.tbl）
    <details>
    
    <summary>相关类型及函数</summary>

    - $B([TBLKey](Constants-Reference/StatText.md) : [literal](Syntax.md#literal-strings))
    - EncodeTBL([TBLKey](Constants-Reference/StatText.md) : py_str)
    - GetTBLAddr([TBLKey或TBL编号](Constants-Reference/StatText.md))
    - settbl([TBLKey或TBL编号](Constants-Reference/StatText.md), 偏移地址, *args)
    - settbl2([TBLKey或TBL编号](Constants-Reference/StatText.md), 偏移地址, *args)
    </details>

    TBL 字符串是指星际争霸1内部的字符串表中的字符串，它包含单位名称、科技名称、技能名称等等  
    不能往 TBL 字符串表中添加字符串，可以从 [StatText](Constants-Reference/StatText.md) 找到所有的 TBLKey 对应的编号  
    TBLKey 所对应的内存中的字符串并不等于 TBLKey 本身  

    ```JavaScript
    println("{:s}", GetTBLAddr("Terran Siege Tank (Tank Mode)")); // 输出：Terran Siege Tank
    ```

    可以通过修改 TBL 表中的字符串来达到修改单位技能名称的功能

    ```JavaScript
    dbstr_print(GetTBLAddr("Terran Marine"), "机枪兵\x00"); // 修改 tbl 会导致本地化失效，所有没有修改的单位技能名称变为英文，强烈不建议修改 tbl
    ```

    

  



