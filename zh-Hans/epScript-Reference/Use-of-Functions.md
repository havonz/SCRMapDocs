# epScript 函数的使用

- [函数](#%E5%87%BD%E6%95%B0)
    - [函数声明](#%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
    - [函数实现](#%E5%87%BD%E6%95%B0%E5%AE%9E%E7%8E%B0)
    - [函数参数](#%E5%87%BD%E6%95%B0%E5%8F%82%E6%95%B0)
    - [函数返回值](#%E5%87%BD%E6%95%B0%E8%BF%94%E5%9B%9E%E5%80%BC)
    - [参数和返回值类型](#%E5%8F%82%E6%95%B0%E5%92%8C%E8%BF%94%E5%9B%9E%E5%80%BC%E7%B1%BB%E5%9E%8B)
    - [函数调用](#%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8)
    - [多个返回值的说明](#%E5%A4%9A%E4%B8%AA%E8%BF%94%E5%9B%9E%E5%80%BC%E7%9A%84%E8%AF%B4%E6%98%8E)


## 函数

- ### 函数声明

    ```JavaScript
    function 一个函数();
    ```

    函数如果不声明，那么它会在它实现的位置声明


- ### 函数实现

    ```JavaScript
    function 一个函数() {
        // 代码块
    }
    ```


- ### 函数参数

    函数可以有一个或者多个参数，参数之间用逗号隔开，函数的参数和返回值都是运行时值类型变量（EUDVariable）

    ```JavaScript
    function 打印两个变量值(变量1, 变量2) {
        println("{}, {}", 变量1, 变量2);
    }
    ```


- ### 函数返回值

    一个函数可以在调用后返回一个或者多个值，多个返回值用逗号隔开

    ```JavaScript
    function 交换(值1, 值2) {
        return 值2, 值1;
    }
    ```


- ### 参数和返回值类型

    函数的参数和返回值可以指定类型  
    在参数名后加上冒号并写上类型名称即可指定参数类型，在运行时便会将参数变量的值（作为编号或指针）包装成指定类型  
    在函数声明的参数列表的括号后加上冒号并写上类型名称即可指定返回值类型，在运行时便会将返回的变量的值（作为编号或指针）包装成指定类型  

    ```JavaScript
    function 创建一个新单位(player : TrgPlayer, ut : TrgUnit, loc : TrgLocation) : CUnit, TrgString {
        const cu = CUnit.from_read(EPD(0x628438));
        if (cu == 0) {
            return 0, $T("无法再创建单位");
        }
        CreateUnit(1, ut, loc, player);
        if ( Memory(0x628438, Exactly, cu.ptr) ) {
            return 0, $T("CreateUnit 没能成功创建单位，可能是参数给错或是出口被堵住了");
        }
        return cu, $T("成功");
    }

    function onPluginStart() {
        const cu, err = 创建一个新单位(P1, $U("Terran Marine"), $L("Location 1"));
        if (cu != 0) {
            cu.cgive(P8);
            cu.set_color(P8);
        } else {
            DisplayTextAll(err);
        }
    }
    ```


- ### 函数调用

    ```JavaScript
    一个函数(); // 直接调用一个没有参数的函数

    var a = 2;
    var b = 3;

    a, b = 交换(a, b); // 传入参数调用一个函数并获取其返回值

    打印两个变量值(a, b); // 传入参数调用一个函数
    ```

- ### 多个返回值的说明

    函数返回多个返回值实际上是返回一个编译期 tuple，当不需要获取所有返回值的情况下，可以用选择`[[]]`获取返回的 tuple 中的一个或多个（下标从 0 开始）  
    tuple 是一个编译期的类型，不是运行时的数据结构  

    ```JavaScript
    function 有多个返回值的函数() {
        return 1, 2, 3, 4, 5;
    }

    const r1 = 有多个返回值的函数()[[0]];

    const r4, r3 = 有多个返回值的函数()[[4, 3]];

    const r1, r2, r3, r4, r5 = 有多个返回值的函数(); // 当有足够的左值接收多个返回值时，tuple 会自动解包
    ```

