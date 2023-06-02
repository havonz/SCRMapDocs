# epScript 变量的使用

- [变量说明](#%E5%8F%98%E9%87%8F%E8%AF%B4%E6%98%8E)
    - [变量声明](#%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E)
    - [静态变量](#%E9%9D%99%E6%80%81%E5%8F%98%E9%87%8F)
    - [常量声明](#%E5%B8%B8%E9%87%8F%E5%A3%B0%E6%98%8E)
    - [变量赋值](#%E5%8F%98%E9%87%8F%E8%B5%8B%E5%80%BC)
    - [变量作用域](#%E5%8F%98%E9%87%8F%E4%BD%9C%E7%94%A8%E5%9F%9F)
    - [变量的数学运算](#%E5%8F%98%E9%87%8F%E7%9A%84%E6%95%B0%E5%AD%A6%E8%BF%90%E7%AE%97)
    - [对象类型](#%E5%AF%B9%E8%B1%A1%E7%B1%BB%E5%9E%8B)
    - [值类型、引用类型](#%E5%80%BC%E7%B1%BB%E5%9E%8B%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B)
    - [运行时字符串](#%E8%BF%90%E8%A1%8C%E6%97%B6%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [静态或动态说明](#%E9%9D%99%E6%80%81%E6%88%96%E5%8A%A8%E6%80%81%E8%AF%B4%E6%98%8E)
    - [const 和 var 的说明](#const-%E5%92%8C-var-%E7%9A%84%E8%AF%B4%E6%98%8E)


## 变量说明

epScript 代码中的所有变量均允许非同步修改或访问，同步修改访问它便属于同步数据，非同步修改访问它便属于非同步数据  
epScript 中`值类型`变量只有一种类型，就是 32 位无符号整数  
没有字符串值类型变量！！没有字符串值类型变量！！没有字符串值类型变量！！


- ### 变量声明

    可以使用 `var` 语法声明一个普通变量

    ```JavaScript
    var 变量名字1;
    static var 变量名字1 = 0;
    // 以上两个写法等效
    var 变量名字2 = 初始值;
    ```


- ### 静态变量

    事实上所有的变量在内部实现都是静态的，不过局部变量每次都会恢复它的初始值  
    没有设定初始值的变量就具有静态特性，使用 `static` 关键字声明可以显式让变量具有静态特性  

    ```JavaScript
    function increaseCount() {
        static var count = 5;
        return count++;
    }

    function onPluginStart() {
        println("{}", increaseCount()); // 6
        println("{}", increaseCount()); // 7
        println("{}", increaseCount()); // 8
    }
    ```


- ### 常量声明

    常量的值运行时不可变  
    所有的引用类型（对象）都必须用 const 修饰声明，引用类型的对象状态可以在运行时更改，但其引用的目标对象在运行时不可更改  

    ```JavaScript
    const 数组变量名1 = EUDArray(数组大小);
    const 对象1 = 对象类型名();
    ```


- ### 变量赋值

    变量的值运行时可以被更改，声明后可以使用等号（`=`）对变量进行赋值

    ```JavaScript
    var 变量名字1; // 声明变量

    变量名字1 = 100; // 给变量赋值 100
    变量名字1 = 变量名字1 + 2; // 给变量自增 2
    ```


- ### 变量作用域

    变量的作用域为：从声明处往下离开当前层大括号（或文件）之前，内层作用域的同名变量比外层的更高  
    虽然没有必要，但来个错误示例吧  
    ```JavaScript
    var 模块级变量 = 100;

    function 错误示例() {
        局部变量1 = 2; // 这就不行，因为下一行才声明 局部变量1
        var 局部变量1 = 2;
        {
            局部变量1 = 1;
            var 局部变量2 = 2;
            var 局部变量1 = 100;
            局部变量2 = 局部变量1; // 有两个 局部变量1 则取作用域更小更具体的那个，也就是 100
            局部变量2 = 模块级变量;
        }
        局部变量1 = 局部变量2; // 这就不行，因为 局部变量2 在里面那层大括号里声明的，它不能离开它的作用域
        局部变量1 = 模块级变量;
    }
    ```


- ### 变量的数学运算

    虽然变量只能是 32 位无符号整数，但变量是可以表示负数的，像 C 语言的无符号 32 位整数那样
    ```JavaScript
    var a = 0;
    a -= 2;
    if (a > 0) { // 这个条件是成立的，无符号数的负数是大于正数的
        println("a == 0x{:x}", a); // a == 0xFFFFFFFE
    }
    if (a >= 0x80000000) { // 判断 a 是否为负数应该这么判断
        println("a(-{}) 小于 0", -a); // a(-2) 小于 0
    }
    if (a < -1 && a > -3) { // 这个是成立的
        println("a(0x{:x}) 小于 -1 大于 -3", a); // a(0xFFFFFFFE) 小于 -1 大于 -3
    }
    a -= 1;
    if (a == -3) { // 这个是成立的
        println("a(0x{:x}) 现在等于 -3 了", a); // a(0xFFFFFFFD) 现在等于 -3 了
    }
    var b = 0;
    DoActions(b.SubtractNumber(2)); // 这是无效的，Subtract 不能将数值减至 0 以下
    println("b == 0x{:x}", b); // b == 0x00000000
    ```


- ### 对象类型

    对象类型是引用类型，它区别于值类型

    ```JavaScript
    object 对象类型名 {
        var 字段名1;
        var 字段名2;
        var 字段名3;
        function 方法名1_给字段1赋值(值) {
            this.字段名1 = 值;
        }
        function 获取字段1的值() {
            return this.字段名1;
        }
    };
    ```

    - #### 有两种方法可以创建一个对象实例

        - 静态初始化：`const 对象1 = 对象类型名();`
        - 动态初始化：`const 对象1 = 对象类型名.alloc();` 你可以将它传递到任何作用域使用，用完了记得用 `对象类型名.free(对象1);` 释放掉它占用的内存。


- ### 值类型、引用类型

    值类型

    ```JavaScript
    var a = 27;
    var b = a;
    b = 3;
    println("{}, {}", a, b); // 输出 27, 3
    ```

    引用类型

    ```Java
    const a = EUDArray(1);
    a[0] = 27;
    const b = a;
    b[0] = 3;
    println("{}, {}", a[0], b[0]); // 输出 3, 3
    ```

    引用类型传递的是值的在内存上的位置，而值类型传递的是值本身


- ### 运行时字符串

    可以使用 GetMapStringAddr 获取地图字符串在内存中的地址，使用内存函数修改它的内容（无法改变字符串尺寸）

    ```JavaScript
    const buf_index = GetStringIndex(py_str(" ") * 64); // 一个长度为 64 全是空格的字符串
    const buf_addr = GetMapStringAddr(buf_index);
    dbstr_print(buf_addr, "字符串 1");
    DisplayText(buf_index);
    dbstr_print(buf_addr, "字符串 ", 2);
    DisplayText(buf_index);
    ```

    使用引用类型操作内存缓冲区实现（无法改变字符串尺寸）

    ```JavaScript
    const buf = StringBuffer(64);
    buf.insertf(0, "字符串 {}", 1);
    buf.append("字符串 2");
    buf.DisplayAt(6); // 输出显示到屏幕文字第六行
    DisplayText(buf.StringIndex);
    ```

    使用 Db 内存数据（无法改变字符串尺寸）

    ```JavaScript
    const buf_addr = Db(64);
    dbstr_print(buf_addr, "字符串 1");
    simpleprint(buf_addr);
    dbstr_print(buf_addr, "字符串 ", 2);
    simpleprint(buf_addr);
    ```


- ### 静态或动态说明

    所有的变量（不论全局还是局部）都是静态分配到固定内存空间的，局部变量也是静态变量，没有变量栈，每个变量都是持久化的，如下代码

    ```JavaScript
    function x() {
        var y;
        y++;
        return y;
    }
    ```

    与如下代码大约等效（不同之处在于上面代码的 y 的作用域在 x 函数里面，而下面的代码中的 x__y 变量作用域是包含 x 函数的整个模块）

    ```JavaScript
    var x__y;

    function x() {
        x__y++;
        return x__y;
    }
    ```

    它们的结果都是

    ```JavaScript
    var a = x();  // returns 1
    var b = x();  // returns 2
    var c = x();  // returns 3
    ```

    > ⚠️注意：只有当你没有给局部变量初始化值的情况下，它才会有静态特性，如果你使用`var y = 0;`声明变量时给它一个初始值，则每次调用 x 时，y 的值都是 0。euddraft 对于给变量初始值和不给初始值的行为是不同的，因此它可能导致混淆。你不应该依赖于该特性写代码，最好给所有的变量都明确指定初始值。你如果希望一个变量有静态特性，可使用 static 关键字显式声明它，并明确给它指定一个初始值，哪怕它是 0。  

    该特性对于对象也是一样的。所有的对象，无论是局部还是全局，都有固定的内存空间。例如，参考以下代码

    ```JavaScript
    function main() {
        const X = EUDArray(10);
        for (var i = 0 ; i < 10 ; i++) {
            X[i] = EUDArray(10);
        })
    }
    ```

    你可能认为我们为 X 的每个单元格分配了一个单独的 `EUDArray(10)` 实例，但是这段代码并不像那样。上面的代码等同于

    ```JavaScript
    const _t0 = EUDArray(10);  // Even intermediate values are static
    const _t1 = EUDArray(10);  // Even intermediate values are static

    function main() {
        const X = _t0;
        for (var i = 0 ; i < 10 ; i++) {
            X[i] = _t1;
        })
    }
    ```

    这不是你可能预料或期待的结果，X 数组的所有值都指向同一个 `_t1` EUDArray  
    而且不幸的是，`EUDArray` 没有 `X[i] = EUDArray.alloc(10)` 这种用法  
    以上参考资料来源：[https://github.com/phu54321/euddraft/wiki/9B.-Appendix---Static-or-Dynamic-instantiation](https://github.com/phu54321/euddraft/wiki/9B.-Appendix---Static-or-Dynamic-instantiation)



- ### const 和 var 的说明

    const 的实质是声明一个 Python 层（编译期）的变量，非地图运行时的变量，声明一个对象时它存储所引用对象的运行时地址  
    var 的实质是声明一个 EUDVariable 对象的引用的语法糖，EUDVariable 是地图运行时的一种变量  
    它与 const 的不同在于，它会在编译期将对其赋值的操作`=`编译成 Python 层的左位移符号`<<`，运行时类型的左位移符号通常被重载为对运行时对象存储的值进行更改，而语法层 const 不允许在声明初始值后用赋值符号`=`  

    若用 var 来声明一个 EUDArray 数组，实质是声明一个存储着一个 EUDArray 对象的地址的 EUDVariable 类型的变量，对这个变量使用取下标运算会抛出语法错误，因为它是一个 EUDVariable 而不是一个 EUDArray  

    ```JavaScript
    var arr = EUDArray(10);
    arr[0] = 1; // 语法错误！arr 是一个 EUDVariable，不支持取下标运算符，它内部保存着一个 EUDArray 对象地址
    const arrt = EUDArray.cast(arr); // 可以这么用，将 arr 的值包装成一个 EUDArray 对象引用
    arrt[0] = 1;
    ```

    以下说明 var 和 const 的关系，两份代码等效

    ```JavaScript
    var 变量名字 = 3;
    变量名字 = 4;
    变量名字 += 5;
    println("{}", 变量名字);
    ```

    ```JavaScript
    const 变量名字 = EUDVariable(3);
    变量名字.__lshift__(4);
    变量名字.__iadd__(5);
    println("{}", 变量名字);
    ```

    在 epScript 能做的编译期交互

    ```JavaScript
    var v = 100;
    const i = 10;
    const s = py_str("Location ") + py_str(i);
    py_print(py_str("编译到了 const s = {} 的位置").format(s));
    py_print("变量 v 的实质是一个 ", v, " 对象");
    ```

      



