# epScript 基本语法

<br />

- [基本语法的说明](#%E5%9F%BA%E6%9C%AC%E8%AF%AD%E6%B3%95%E7%9A%84%E8%AF%B4%E6%98%8E)
    - [编译期（compile-time）和运行时（run-time）](#%E7%BC%96%E8%AF%91%E6%9C%9Fcompile-time%E5%92%8C%E8%BF%90%E8%A1%8C%E6%97%B6run-time)
    - [大小写敏感](#%E5%A4%A7%E5%B0%8F%E5%86%99%E6%95%8F%E6%84%9F)
    - [值类型](#%E5%80%BC%E7%B1%BB%E5%9E%8B)
    - [逻辑规则](#%E9%80%BB%E8%BE%91%E8%A7%84%E5%88%99)
    - [字面量数字（literal number）](#%E5%AD%97%E9%9D%A2%E9%87%8F%E6%95%B0%E5%AD%97literal-number)
    - [字面量字符串（literal string）](#%E5%AD%97%E9%9D%A2%E9%87%8F%E5%AD%97%E7%AC%A6%E4%B8%B2literal-string)
    - [字面量字节串（literal bytes）](#%E5%AD%97%E9%9D%A2%E9%87%8F%E5%AD%97%E8%8A%82%E4%B8%B2literal-bytes)
    - [命名规则](#%E5%91%BD%E5%90%8D%E8%A7%84%E5%88%99)
    - [引入其它模块](#%E5%BC%95%E5%85%A5%E5%85%B6%E5%AE%83%E6%A8%A1%E5%9D%97)
    - [符号](#%E7%AC%A6%E5%8F%B7)
        - [代码块 {}](#%E4%BB%A3%E7%A0%81%E5%9D%97)
        - [语法层换行符 \;](#%E8%AF%AD%E6%B3%95%E5%B1%82%E6%8D%A2%E8%A1%8C%E7%AC%A6)
        - [索引运算符 \[\]](#%E7%B4%A2%E5%BC%95%E8%BF%90%E7%AE%97%E7%AC%A6)
        - [赋值符 \=](#%E8%B5%8B%E5%80%BC%E7%AC%A6)
        - [行注释符 //](#%E8%A1%8C%E6%B3%A8%E9%87%8A%E7%AC%A6)
        - [块注释符 /\* \*/](#%E5%9D%97%E6%B3%A8%E9%87%8A%E7%AC%A6)
        - [条件判断运算符 > <](#%E6%9D%A1%E4%BB%B6%E5%88%A4%E6%96%AD%E8%BF%90%E7%AE%97%E7%AC%A6)
        - [数学运算符 + - * /](#%E6%95%B0%E5%AD%A6%E8%BF%90%E7%AE%97%E7%AC%A6)
        - [自增/自减/自乘/自除运算符](#%E8%87%AA%E5%A2%9E%E8%87%AA%E5%87%8F%E8%87%AA%E4%B9%98%E8%87%AA%E9%99%A4%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [条件判断语法](#%E6%9D%A1%E4%BB%B6%E5%88%A4%E6%96%AD%E8%AF%AD%E6%B3%95)
        - [if](#if)
        - [if else](#if-else)
        - [条件串联](#%E6%9D%A1%E4%BB%B6%E4%B8%B2%E8%81%94)
        - [条件嵌套](#%E6%9D%A1%E4%BB%B6%E5%B5%8C%E5%A5%97)
        - [单次执行](#%E5%8D%95%E6%AC%A1%E6%89%A7%E8%A1%8C)
    - [流程控制](#%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6)
        - [for 循环](#for-%E5%BE%AA%E7%8E%AF)
        - [while 循环](#while-%E5%BE%AA%E7%8E%AF)
        - [break 跳出循环](#break-%E8%B7%B3%E5%87%BA%E5%BE%AA%E7%8E%AF)
        - [foreach 迭代器循环](#foreach-%E8%BF%AD%E4%BB%A3%E5%99%A8%E5%BE%AA%E7%8E%AF)
        - [switch 变量值多重选择分支](#switch-%E5%8F%98%E9%87%8F%E5%80%BC%E5%A4%9A%E9%87%8D%E9%80%89%E6%8B%A9%E5%88%86%E6%94%AF)
        - [epdswitch 内存值多重选择分支](#epdswitch-%E5%86%85%E5%AD%98%E5%80%BC%E5%A4%9A%E9%87%8D%E9%80%89%E6%8B%A9%E5%88%86%E6%94%AF)

<br />

## 基本语法的说明

- ### 编译期（compile-time）和运行时（run-time）

    除声明变量外的任何非编译期代码都不可暴露在函数外  
    包含了任何运行时操作的函数为非编译期函数  
    所有的对变量的声明/初始化/读取/运算/赋值操作都是运行时操作  
    编译期的代码 if 条件不成立也会执行


- ### 大小写敏感

    epScript 是大小写敏感的编程语言，A 和 a 意思不同 


- ### 值类型

    epScript 基本的`值类型`只有一种，就是 32 位无符号整数 


- ### 逻辑规则

    整数`0`逻辑为假（false）  
    整数`非 0 值`逻辑为真（true）  


- ### 字面量数字（literal number）

    字面量数字包含 10 进制数字、16 进制数字、二进制数字  
    ```JavaScript
    // 以下代码中的 15、0xf、0b1111 均表示同一个数 15，只是写法不同，所以 a、b、c 是相等的
    var a = 15;
    var b = 0xf; // 16 进制数以 0x 打头
    var c = 0b1111; // 二进制数以 0b 打头
    ```


- ### 字面量字符串（literal string）

    字面量字符串用成对单引号`'`或者成对双引号`"`包裹字面值即可  
    ```JavaScript
    DisplayText("这是字面量字符串");
    DisplayText('这是字面量字符串');
    DisplayText("这是字面量\
    字符串"); // 当字符串太长时，可以用反斜杠 \ 换一行继续这个字面量字符串，这并不表示在字符串中插入换行符，以上两个写法完全等价
    ```
    字面量字符串支持使用反斜杠转义符
    |描述|说明|示例|示例结果|
    |-|-|-|-|
    |\\\\ |表示 \\ 本身|`DisplayText("你好\\星际");`|你好\星际|
    |\八进制数|表示 ASCII 编码对应的那个字符|`DisplayText("星际\101\102\103");`|星际ABC|
    |\x十六进制数|表示 ASCII 编码对应的那个字符|`DisplayText("星际\x41\x42\x43");`|星际ABC|
    |\\`换行`|表示续行，不换行|`DisplayText("你好\`<br />`星际");`|你好星际|
    |\\n|换行符，等同于 \x0A|`DisplayText("你好\n星际");`|你好<br />星际|
    |\\t|横向制表符，等同于 \\x09|`DisplayText("你好\t星际");`|你好&emsp;星际|
    |\\r|回车符，等同于 \\x0D，在游戏中没啥效果|`DisplayText("你好\r星际");`|你好星际|
    |\\"|在双引号字符串中表示双引号本身|`DisplayText("你好\"星际\"");`|你好"星际"|
    |\\'|在单引号字符串中表示单引号本身|`DisplayText('你好\'星际\'');`|你好'星际'|


- ### 字面量字节串（literal bytes）

    字面量字节串用`b"`和`"`包裹或者`b'`和`'`包裹字面值即可，字节串不以 \0 结尾，字面量字节串同样也支持字面量字符串中的转义符
    ```JavaScript
    println("{}", b"ASCII\nliteral");
    println("{}", b'ASCII\nliteral');
    ```


- ### 命名规则

    变量名、常量名及函数名只能包含`非 ASCII 部分的 UTF-8 字符`（中日韩文都可以）、[ASCII](https://zh.wikipedia.org/zh-cn/ASCII) 部分的字母/数字以及下划线`_`，并且不能以 ASCII 数字打头

    ```JavaScript
    // 合法的变量名
    var a;
    var a_b;
    var a_1;
    var _a1;
    var 这也行;

    // 不合法的变量名
    var 3y = 1;
    var abc# = 2;
    ```

    变量名、常量名及函数名不能用关键字命名，哪些是关键字我也不清楚，但这些是

    ```C#
    static var const object this function 
    return for foreach while switch epdswitch 
    break continue if else once import as 
    true false 
    ```

    变量名、常量名不能以 Python 3 的关键字命名

    ```Python
    False None True 
    and as assert break class continue 
    def del elif else except finally 
    for from global if import in 
    is lambda nonlocal not or pass 
    raise return try while with yield 
    ```

    另外一个特例是函数名用 py_ 开头的情况下，调用它要用 py_py_ 开头才行

    ```JavaScript
    function py_函数名() {
    }
    function onPluginStart() {
        py_py_函数名();
    }
    ```


- ### 引入其它模块

    可以使用`import`关键词引入其它模块，`as`给引入的模块一个别名，以下代码说明用法  

    `模块1.eps`：
    ```JavaScript
    const 模块1的一个常量 = 0;
    static var 模块1的一个变量 = 0;

    function 模块1的一个函数() {
        模块1的一个变量++;
        return 模块1的一个变量;
    }
    ```

    `模块2.eps`：
    ```JavaScript
    import 模块1;

    function 模块2的一个函数() {
        return 模块1.模块1的一个函数();
    }
    ```

    `模块3.eps`：
    ```JavaScript
    import 模块1 as m1;
    import 模块2 as m2;

    function onPluginStart() {
        println("{}", m1.模块1的一个常量);
        m2.模块2的一个函数();
    }
    ```


- ### 符号

    所有涉及语法的符号都是纯英文状态下的半角符号，可以在 [ASCII](https://zh.wikipedia.org/zh-cn/ASCII) 表中找到

    - #### 代码块

        使用成对的大括号`{}`把单句或多句代码包围起来成为一个代码块

        ```JavaScript
        {
            // 代码
        }
        ```

        若希望单句代码为一个代码块时，也可省略掉大括号，以下示例是合法的

        ```JavaScript
        function 无聊的示例函数()
            for (var i = 1; i < 10; i++)
                if (i < 5)
                    println("{} 小于 5", i);
                else if (i == 5)
                    println("{} 等于 5", i);
                else
                    println("{} 大于 5", i);
        ```

    - #### 语法层换行符

        语法层的换行符是分号`;`，而不是换行符

        ```JavaScript
        var a;var b;
        ```

    - #### 索引运算符

        `[]`
        取索引访问或修改数组中的元素

        ```JavaScript
        const a = EUDArray(10);
        a[0] = 11;
        var b = a[0];
        ```

    - #### 赋值符

        赋值符号是单个等号`=`

        ```JavaScript
        var a; // 声明变量 a
        var b = 3; // 声明变量 b 并赋值为 3
        a = 2; // 将变量 a 赋值为 2
        ```

    - #### 行注释符

        两个斜杠`//`开始行注释

        ```JavaScript
        var a = 1; // 注释是代码中不会执行的部分，从 // 开始到当前行的结尾的内容不会被认为是代码
        ```

    - #### 块注释符

        在`/*`到`*/`之间的内容叫块注释

        ```JavaScript
        var /* 注释是代码中不会执行的部分，块注释可以加在代码中间 */ a = 1;
        ```

    - #### 条件运算符

        - 大于 `>`
        - 小于 `<`
        - 大于等于 `>=`
        - 小于等于 `<=`
        - 等于 `==`
        - 逻辑与 `&&`
        - 逻辑或 `||`

        值得一提的是，对变量使用条件比较运算时，返回的是`条件表达式`常量，而非`条件表达式的结果`  
        if 的参数是`条件表达式`列表，如果将变量直接传入 if，则 if 会将其转换成一个运行时的取值是否不等于 0 的表达式  

        ```JavaScript
        if (a == 2) // 逻辑相等比较是两个等号
            单句; // 逻辑比较的是非代码块是单句可以省略掉大括号

        if (a == 2) {
            // a 等于 2
            单句1;
            单句2;
        }

        if (a != 2) {
            // a 不等于 2
        }

        if (a > 2) {
            // a 大于 2
        }

        if (a < 2) {
            // a 小于 2
        }

        if (a >= 2) {
            // a 大于或等于 2
        }

        if (a <= 2) {
            // a 小于或等于 2
        }

        if (a > 2 && a < 10) {
            // a 大于 2 并且小于 10
        }

        if (a > 10 || a <= 5) {
            // a 大于 10 或小于等于 5
        }

        if ( !(a < 2) ) {
            // a 不小于 2
        }

        var a = 3;
        var b = a > 0;       // 错误！它返回的并非 true，而是 a > 0 这个条件表达式本身
        var c = l2v(a > 0);  // 这样就对了，运行时 c 就会等于 true 或 false，取决于运行时 a 的状态
        const d = a > 0;     // 这里的 d 就代表 a > 0 这个条件表达式本身，不是 a > 0 的结果
        var e = l2v(d);      // 在运行时使用 l2v 将 d 表达式的结果赋值给 e
        ```
        
        - 逻辑非 `!`
        
        对变量`a`使用`!`会返回一个变量，它的值是`l2v((a != 0) == 0)`的结果  
        对变量`a`使用两倍连续的`!`例如`!!a`或`!(!a)`或`!!!(!a)`都是直接等于`a`本身  
        对常量或`条件表达式`使用`!`则仍然会返回`条件表达式`  

        ```js
        var four = 4;
        var b = !four; // b == 0
        if (!b   != !!four) println("b is not !!four");
        if (four == !!four) println("four is !!four");
        ```

    - #### 数学运算符

        `+` `-` `*` `/`

        ```JavaScript
        a = a + 1;
        a = a - 1;
        a = a * 2;
        a = a / 2;  // 整数除法运算符，向下取整
        a = a % 2;  // 取余运算符
        a = a ** 3; // 这个是幂运算符，返回 a 的 3 次幂，杨幂的幂～
        a = a << 1; // 左位移 1 位
        a = a >> 1; // 右位移 1 位
        ```

    - #### 自增/自减/自乘/自除运算符

        ```JavaScript
        a += 10; // 它相当于 a = a + 10;
        a -= 10; // 它相当于 a = a - 10;
        a++;     // 它相当于 a = a + 1;
        a--;     // 它相当于 a = a - 1;
        a *= 10; // 它相当于 a = a * 10;
        a /= 10; // 它相当于 a = a / 10;
        ```


- ### 条件判断语法

    - #### if

        if 语法的形式为

        ```JavaScript
        if (条件表达式1) {
            条件表达式1 满足后执行;
        }
        ```

    - #### if else

        if 的否则分支 else 语法

        ```JavaScript
        if (条件表达式1) {
            条件表达式1 满足时执行;
        } else {
            条件表达式1 不满足时执行;
        }
        ```

    - #### 条件串联

        可以将 if 串联到另外一个 else 上

        ```JavaScript
        if (条件表达式1) {
            条件表达式1 满足时执行;
        } else if (条件表达式2) {
            条件表达式1 不满足并且 条件表达式2 满足时执行;
        } else {
            条件表达式1 和 条件表达式2 都不满足时执行;
        }
        ```

    - #### 条件嵌套

        可以将 if 写到另外一个 if 的代码块中

        ```JavaScript
        if (条件表达式1) {
            条件表达式1 满足时执行;
        } else if (条件表达式2) {
            if (条件表达式3) {
                条件表达式1 不满足并且 条件表达式2 和 条件表达式3 都满足时执行;
            } else {
                条件表达式1 和 条件表达式3 都不满足并且 条件表达式2 满足时执行;
            }
        } else if (条件表达式4) {
            条件表达式1 和 条件表达式2 都不满足并且 条件表达式4 满足时执行;
        } else {
            条件表达式1 和 条件表达式2 都不满足时执行;
        }
        ```

    - #### 单次执行

        故名思义就是在运行时条件满足执行了一次之后就不再执行，通常用于加在 beforeTriggerExec 或者 afterTriggerExec 中每一帧重复判断，直到达成条件则执行一次

        ```JavaScript
        once (条件表达式) { // 在运行时重复运行 once 代码块的情况下，会在条件表达式满足时仅仅运行一次它里面的代码
            // 代码
        }

        once { // 无条件仅执行一次它里面的代码
            // 代码
        }

        // 以下代码执行后将只打印一次 0
        for (var i = 0; i < 100; i++) {
            once {
                println("{}", i);
            }
        }

        // 以下代码将只会在有机枪兵进入编号 1 到 10 的区域的任何一个区域的时候触发一次，不会在分别进入每个区域的时候触发，不会触发 10 次
        for (var i = $L("Location 1"); i <= $L("Location 10"); i++) {
            once ( Bring(P1, AtLeast, 1, "Terran Marine", i) ) {
                println("机枪兵进入区域 {}", i);
            }
        }
        ```


- ### 流程控制

    - #### for 循环

        for 循环可设定循环初始化动作表达式、循环执行条件表达式以及每循环附加动作表达式

        ```JavaScript
        for (初始化动作表达式; 循环执行条件表达式; 每循环附加动作表达式) {
            循环中的代码;
        }

        for (var i = 0; i < 10; i++) {
            println("{}", i);
        }
        // 简单描述一下，上面的代码声明了一个计数变量 i，初始值为 0，当 i < 10 的情况下就一直循环执行，并且每执行一次都将 i 自增 1，i 的作用域就是后面那个大括号里，大括号是每次循环需要执行的内容

        var i1, i8;
        for (i1, i8 = 0, 0 ; i1 < 10 && i8 < 80 ; i1++, i8 += 8) {
            printAll("{} x 8 = {}", i1, i8);
        }
        ```

    - #### while 循环

        while 循环可以设定一个循环条件，假如条件满足就一直循环执行，直到条件不满足则不再继续

        ```JavaScript
        var i = 0;
        while (i < 10) {
            println("{}", i);
            i++;
        }
        ```

    - #### break 跳出循环

        可以使用 break 跳出一个运行时循环或 switch

        ```JavaScript
        var i = 0;
        while (true) { // 设定一个永远成立的条件，也就是一直返回 true
            println("{}", i);
            if (i >= 10) {
                break;
            }
        }
        ```

        它和上面的 while 循环等效

    - #### foreach 迭代器循环

        **编译期迭代器**  
        py_range 和 py_enumerator 是编译期迭代器  
        使用编译期迭代器的情况下，foreach 是编译期循环，会在编译期静态展开，不能使用 break 和 continue  
        ```C#
        foreach (i : py_range(5)) {
            simpleprint(i + 1);
        }
        // 完全等价于以下代码
        simpleprint(0 + 1);
        simpleprint(1 + 1);
        simpleprint(2 + 1);
        simpleprint(3 + 1);
        simpleprint(4 + 1);
        ```

        ```C#
        foreach (i : py_range(3)) {
            once (ElapsedTime(AtLeast, i)) {
                println("第 {} 秒", i);
            }
        }
        // 完全等价于以下代码
        once (ElapsedTime(AtLeast, 0)) {
            println("第 {} 秒", 0);
        }
        once (ElapsedTime(AtLeast, 1)) {
            println("第 {} 秒", 1);
        }
        once (ElapsedTime(AtLeast, 2)) {
            println("第 {} 秒", 2);
        }
        ```

        **运行时迭代器**  
        名字以 EUDLoop 开头的迭代器函数通常是返回的是运行时迭代器  

        > EUDLoopPlayer、EUDLoopRange、EUDLoopUnit、EUDLoopUnit2、EUDLoopCUnit、EUDLoopNewUnit、EUDLoopNewCUnit、EUDLoopPlayerUnit、EUDLoopPlayerCUnit   
        
        其次是 EUDQueue、EUDDeque 容器也属于运行时迭代器，UnitGroup.cploop 也返回一个运行时迭代器  
        
        EUDDeque 演示  
        ```C#
        // dq3 是一个尺寸为 3 的 EUDDeque
        const dq3 = EUDDeque(3)();
        const ret = EUDCreateVariables(6);

        // 如果 dq3 为空则运行时不会有任何代码被执行
        foreach(v : dq3) {
            ret[0] += v;
        }

        // 从右侧添加 1 和 2 两个值到 dq3 这个 EUDDeque 中
        dq3.append(1);  // dq3 : (1)
        dq3.append(2);  // dq3 : (1, 2)
        foreach(v : dq3) {
            ret[1] += v; // 3 = 1 + 2
        }

        // 从右侧添加 3 和 4 两个值到 dq3 这个 EUDDeque 中
        dq3.append(3);  // dq3 : (1, 2, 3)
        dq3.append(4);  // dq3 : (2, 3, 4)
        foreach(v : dq3) {
            ret[2] += v; // 9 = 2 + 3 + 4
        }

        // 从右侧添加 5 这个值到 dq3 这个 EUDDeque 中
        dq3.append(5);  // dq3 : (3, 4, 5)
        foreach(v : dq3) {
            ret[3] += v; // 12 = 3 + 4 + 5
        }

        // 从左侧弹出一个值，这里 3 被弹出
        const three = dq3.popleft();  // dq3 : (4, 5)
        foreach(v : dq3) {
            ret[4] += v; // 9 = 4 + 5
        }

        // 从右侧添加 6 和 7 两个值到 dq3 这个 EUDDeque 中
        dq3.append(6);  // dq3 : (4, 5, 6)
        dq3.append(7);  // dq3 : (5, 6, 7)
        foreach(v : dq3) {
            ret[5] += v; // 18 = 5 + 6 + 7
        }
        ```

    - #### switch 变量值多重选择分支

        对单个值的多种状态判断的条件分支

        **普通 switch**
        ```JavaScript
        switch (day) {
            case 1:
                DisplayText("苦逼上班日子开始了");
                break;
            case 4:
            case 5:
                DisplayText("马上周末了");
                break;
            case 0, 6:
                DisplayText("好嗨啊！");
                break;
            default:
                DisplayText("期待周末");
        }

        // 上述 switch 代码可以看作以下 if 条件分支代码

        if (day == 1) {
            DisplayText("苦逼上班日子开始了");
        } else if (day == 4 || day == 5) {
            DisplayText("马上周末了");
        } else if (day == 0 || day == 6) {
            DisplayText("好嗨啊！");
        } else {
            DisplayText("期待周末");
        }
        ```

        **带 bitmask 的 switch**
        ```JavaScript
        var x = 0x101;
        switch (x, 0xff) {
            case 0:
                // (x & 0xff) == 0
                break;
            default:
                // (x & 0xff) != 0
                break;
        }
        ```

    - #### epdswitch 内存值多重选择分支

        对单个运行时内存位置的值的多种状态判断的条件分支

        ```JavaScript
        const unitId = epd + 0x64/4;
        epdswitch (unitId, 255) {  // you can put constant epd in epdswitch too
            // switch branching by unit kind
            case $U("Terran Marine"):
                // Run when unitType is marine
                break;
            case $U("Terran Ghost"):
                // Run when unitType is ghost
                break;
        }
        ```

      



