# epScript 内置的基本对象类型

<br />

## 基本对象类型

- ### EUDVariable

    其实就是 var 声明的那个变量啦，它也是一个对象，只不过它在语法上被定义为值类型，和其它对象类型有一些区别，比如可以用等号赋值等  
    一个 EUDVariable 将使用一个只有一条 SetDeathsX 动作的虚拟触发器模拟，占用 72 字节，以下是它的类型结构，它有很多条件和动作函数  
    ```JavaScript
    object EUDVariable {
        // 常规方法
        function ineg(){}                   // euddraft 0.9.9.8 版新增，将自身值转换为相反数，相当于 x = -x; 支持转换成动作这样用 DoActions(v.ineg(action = true));
        function iabs(){}                   // euddraft 0.9.9.8 版新增，将自身值转换为绝对值，相当于 x = (x & (1 << 31) == 0) ? x : -x; 支持转换成动作这样用 DoActions(v.iabs(action = true));

        // 常规条件
        function AtLeast(v){}               // 变量值 >= v
        function AtMost(v){}                // 变量值 <= v
        function Exactly(v){}               // 变量值 == v
        function AtLeastX(v,mask){}         // (变量值 & mask) >= v
        function AtMostX(v,mask){}          // (变量值 & mask) <= v
        function ExactlyX(v,mask){}         // (变量值 & mask) == v
        function MaskAtLeast(v){}           // 变量的虚拟触发器 SetDeathsX 的动作的 Mask >= v
        function MaskAtMost(v){}            // 变量的虚拟触发器 SetDeathsX 的动作的 Mask <= v
        function MaskExactly(v){}           // 变量的虚拟触发器 SetDeathsX 的动作的 Mask == v
        function MaskAtLeastX(v,msk){}      // 变量的虚拟触发器 SetDeathsX 的动作的 (Mask & msk) >= v
        function MaskAtMostX(v,msk){}       // 变量的虚拟触发器 SetDeathsX 的动作的 (Mask & msk) <= v
        function MaskExactlyX(v,msk){}      // 变量的虚拟触发器 SetDeathsX 的动作的 (Mask & msk) == v

        // 常规动作
        function SetNumber(v){}             // 变量值 = v
        function AddNumber(v){}             // 变量值 = 变量值 + v
        function SubtractNumber(v){}        // 变量值 = 变量值 - v if 变量值 <= v else 变量值 = 0
        function SetNumberX(v,mask){}       // 变量值 = 变量值 - (变量值 & mask) + (v & mask)
        function AddNumberX(v,mask){}       // 变量值 = 变量值 - (变量值 & mask) + ( ((变量值 & mask) + (v & mask)) & mask )
        function SubtractNumberX(v,mask){}  // 变量值 = 变量值 - (变量值 & mask) + ( ((变量值 & mask) - (v & mask)) & mask )
    
        // 编译期常量函数
        function GetVTable(){}              // 编译期获取变量虚拟触发器的运行时地址
        function getMaskAddr(){}            // 编译期获取变量虚拟触发器的 SetDeathsX 动作中的掩码参数的运行时地址
        function getValueAddr(){}           // 编译期获取变量虚拟触发器的 SetDeathsX 动作中的数值参数的运行时地址
        function getDestAddr(){}            // 编译期获取变量虚拟触发器的 SetDeathsX 动作中的玩家编号参数的运行时地址

        // 下列动作实际只能配合 VProc 使用
        function SetMask(v){}               // 将变量的虚拟触发器 SetDeathsX 的动作的 Mask = v
        function AddMask(v){}               // 将变量的虚拟触发器 SetDeathsX 的动作的 Mask = Mask + v
        function SubtractMask(v){}          // 将变量的虚拟触发器 SetDeathsX 的动作的 Mask = Mask - v if Mask >= v else Mask = 0
        function SetMaskX(v,msk){}          // 将变量的虚拟触发器 SetDeathsX 的动作的 Mask = Mask - (Mask & msk) + (v & msk)
        function AddMaskX(v,msk){}          // 将变量的虚拟触发器 SetDeathsX 的动作的 Mask = Mask - (Mask & msk) + ( ((Mask & msk) + (v & msk)) & msk )
        function SubtractMaskX(v,msk){}     // 将变量的虚拟触发器 SetDeathsX 的动作的 Mask = Mask - (Mask & msk) + ( ((Mask & msk) - (v & msk)) & msk )
        function SetDest(dest){}            // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 = dest
        function AddDest(dest){}            // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 = 玩家编号 + dest
        function SubtractDest(dest){}       // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 = 玩家编号 - dest if 玩家编号 >= dest else 玩家编号 = 0
        function SetDestX(dest,mask){}      // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 = 玩家编号 - (玩家编号 & mask) + (dest & mask)
        function AddDestX(dest,mask){}      // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 = 玩家编号 - (玩家编号 & mask) + ( ((玩家编号 & mask) + (dest & mask)) & mask )
        function SubtractDestX(dest,mask){} // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 = 玩家编号 - (玩家编号 & mask) + ( ((玩家编号 & mask) - (dest & mask)) & mask )
        function SetModifier(modifier){}    // 将变量的虚拟触发器 SetDeathsX 的动作的 数值修改方法 设置为 modifier
        function QueueAssignTo(dest){}      // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 设置为 dest 并将 数值修改方法 设置为 SetTo
        function QueueAddTo(dest){}         // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 设置为 dest 并将 数值修改方法 设置为 Add
        function QueueSubtractTo(dest){}    // 将变量的虚拟触发器 SetDeathsX 的动作的 玩家编号 设置为 dest 并将 数值修改方法 设置为 Subtract，Subtract 方法无法将数值从正数减到负数
    };
    ```

<br />

- ### EUDLightVariable

    轻变量，它和 var 声明的变量不同，它仅开辟 4 字节的内存空间，它的传值操作相较于普通变量更消耗资源，判断值或者写入值和普通变量一样仅需要执行一条触发器。（普通变量实质是一个虚拟触发器，占用 72 字节）  
    要将它的值当作参数传递到其它函数中则需要使用 dwread 函数来读取它。  
    若一个普通变量（EUDVariable）的值不需要当作其它函数的参数（例如用于计数判断、自增、自减、赋值、开关等不与其它函数/条件/动作关联的场景），则可使用 EUDLightVariable 来替代该普通变量。  

    ```JavaScript
    object EUDLightVariable {
        // 编译期常量函数
        function getValueAddr(){}

        // 能够通过以下函数、条件、动作搭配达成的目标，都可选用 EUDLightVariable 实现，否则就应该用普通变量（EUDVariable）
        // 常规方法
        function ineg(){}                   // 将自身值转换为相反数，相当于 x = -x; 在 euddraft 0.9.9.8 及之后的版本中支持转换成动作这样用 DoActions(v.ineg(action = true));
        function iabs(){}                   // euddraft 0.9.9.8 新增，将自身值转换为绝对值，相当于 x = (x & (1 << 31) == 0) ? x : -x; 支持转换成动作这样用 DoActions(v.iabs(action = true));
        // 常规条件
        function AtLeast(v){}               // 轻变量值 >= v
        function AtMost(v){}                // 轻变量值 <= v
        function Exactly(v){}               // 轻变量值 == v
        function AtLeastX(v,mask){}         // (轻变量值 & mask) >= v
        function AtMostX(v,mask){}          // (轻变量值 & mask) <= v
        function ExactlyX(v,mask){}         // (轻变量值 & mask) == v
        // 常规动作
        function SetNumber(v){}             // 轻变量值 = v
        function AddNumber(v){}             // 轻变量值 = 轻变量值 + v
        function SubtractNumber(v){}        // 轻变量值 = 轻变量值 - v if 轻变量值 <= v else 轻变量值 = 0
        function SetNumberX(v,mask){}       // 轻变量值 = 轻变量值 - (轻变量值 & mask) + (v & mask)
        function AddNumberX(v,mask){}       // 轻变量值 = 轻变量值 - (轻变量值 & mask) + ( ((轻变量值 & mask) + (v & mask)) & mask )
        function SubtractNumberX(v,mask){}  // 轻变量值 = 轻变量值 - (轻变量值 & mask) + ( ((轻变量值 & mask) - (v & mask)) & mask )
    };
    ```

    示例

    ```JavaScript
    const lv = EUDLightVariable(100);
    DoActions(lv.AddNumber(564)); // lv += 564;
    if (lv != 150) {
        println("{}", dwread(lv.getValueAddr())); // 这个读取值的过程比普通变量要消耗更多的触发器资源，大概在 32 倍以上
    }
    lv.ineg(); // lv = -lv;
    if (lv > 0x80000000 && lv < -663) {
        println("小于 -663 的负数");
    }
    ```

<br />

- ### EUDLightBool

    轻布尔型（开关）变量，它使用最少 1 位（八分之一字节）存储，布尔型变量尽量用这个，而非 var  
    在 eudplib 内部实现中每 32 个 EUDLightBool 共用一个 EUDLightVariable  
    布尔型（开关）只能表示两个状态，1 表示 Set，0 表示 Cleared，默认初始值为 Cleared  

    ```JavaScript
    object EUDLightBool {
        // 编译期常量函数
        function getValueAddr() {}

        // 常规条件
        function IsSet(){}      // 当前开关为已设置状态
        function IsCleared(){}  // 当前开关为已清除状态

        // 常规动作
        function Set(){}        // 设置为开状态
        function Clear(){}      // 设置为关状态
        function Toggle(){}     // 转换开关状态
    };
    ```

    示例

    ```JavaScript
    const b = EUDLightBool(); // 默认初始值为 Cleared
    DoActions(
        b.Set(),
        b.Clear(),
        b.Toggle(),
    );
    if ( b.IsSet() ) {
        simpleprint("b is set");
    }
    if ( b.IsCleared() ) {
        simpleprint("b is cleared");
    }
    ```

<br />

- ### EUDArray

    轻静态数组容器，它可以使用`[...]`语法初始化声明，不能动态更改尺寸

    ```JavaScript
    object EUDArray {
        function constructor(size) {}
        const length;
    };
    ```

    示例

    ```JavaScript
    const a = EUDArray(10); // 声明一个尺寸为 10 的数组（下标 0~9）
    a[0] = 29; // 将 a 数组中的下标为 0 的元素赋值为 29

    println("数组尺寸:{} [0]位置的值:{}", a.length, a[0]); // 数组尺寸:10 [0]位置的值:29

    const b = [3, 2, 1]; // 声明一个尺寸为 3 的数组（下标 0~2）并初始化为 b[0] = 3; b[1] = 2; b[2] = 1;

    const c = [list(3, 2, 1), 4, list(5, 6)]; // 声明一个尺寸为 6 的数组（下标 0~5）并初始化为 b[0] = 3; b[1] = 2; b[2] = 1; b[3] = 4; b[4] = 5; b[5] = 6;
    ```

<br />

- ### EUDVArray

    使用虚拟触发器实现的静态数组容器，它可以使用`VArray(...)`语法初始化声明，不能动态更改尺寸

    它在数组下标为变量时有更快的访问速度

    ```JavaScript
    object EUDVArray {
        function constructor(size) {}
        const length;
    };
    ```

    示例

    ```C#
    const a = EUDVArray(4)(list(1, 2, 3, 4)); // 声明一个尺寸为 4 的数组并初始化为 a[0] = 1; a[1] = 2; a[2] = 3; a[3] = 4;

    const b = VArray(1, 2, 3, 4); // 和上面那个一样

    const c = VArray(list(1, 2, 3, 4)); // 和上面那个一样

    const d = VArray(list(1, 2), list(3, 4)); // 和上面那个一样

    const d = EUDVArray(4)(); // 声明一个尺寸为 4 的数组，等同 VArray(0, 0, 0, 0);
    foreach (i : py_range(4)) {
        d[i] = i + 1;
    }
    ```

<br />

- ### PVariable

    玩家变量，它实际上是 `EUDVArray(8)()` 的另一种写法，就是对每个玩家都存储不一样的值的一个数组，星际最多 8 个玩家

    ```JavaScript
    object PVariable {
        const length;
    };
    ```

    示例

    ```JavaScript
    // 它俩等价
    const pv1 = PVariable();
    const pv2 = EUDVArray(8)();
    ```

<br />

- ### EUDVArrayReader

    用于快速遍历 EUDVArray 的类

    ```JavaScript
    object EUDVArrayReader {
        function seek(varr_ptr, varr_epd, eudv, acts) {}
        function read(acts) {}
    }
    ```

<br />

- ### EUDDeque

    使用虚拟触发器实现的静态双向队列容器，EUDDeque 是运行时迭代器类型

    ```JavaScript
    object EUDDeque {
        function constructor(size) {}
        function append(arg) {}
        function appendleft(arg) {}
        function pop() {}
        function popleft() {}
        function clear() {}
        function empty() {}
        const length;
    };
    ```

    示例

    ```JavaScript
    const dq = EUDDeque(10)();

    // `.length` : 获取当前双向队列长度
    println("双向队列中元素个数 {}", dq.length);

    // `.append(x)` : 将 x 添加到双向队列的最右侧
    dq.append(10);

    // `.pop()` : 将双向队列最右侧的元素弹出来（移除并返回），你需要先判断双向队列中是否还有元素，如果里面没有元素，直接使用这个方法的行为是未定义的
    println("双向队列最右边的的值弹出 {}", dq.pop());

    // `.appendleft(x)` : 将 x 添加到双向队列的最左侧
    dq.appendleft(13);

    // `.popleft()` : 将双向队列最左侧的元素弹出来（移除并返回），你需要先判断双向队列中是否还有元素，如果里面没有元素，直接使用这个方法的行为是未定义的
    println("双向队列最左边的的值弹出 {}", dq.popleft());

    // `.clear()` : 清空双向队列
    dq.clear();

    // `.empty()` : 判断当前双向队列中元素个数是否为 0
    if (dq.empty()) {
        println("双向队列为空");
    }
    ```

<br />

- ### StringBuffer

    内存静态字符串缓冲操作类型

    ```JavaScript
    object StringBuffer {
        function constructor(content) {}
        function constructor(len) {}
        function append(*args) {}
        function appendf(format_string, *args) {}
        function insert(index, *args) {}
        function insertf(index, format_string, *args) {}
        function delete(start, length=1) {}
        function Display() {}
        function DisplayAt(line) {}
        function print(*args) {}
        function printf(format_string, *args) {}
        function printfAt(line, format_string, *args) {}
        function Play() {}
        function fadeIn(*args, line=-1, color=None, wait=1, reset=True, tag=None) {}
        function fadeOut(*args, line=-1, color=None, wait=1, reset=True, tag=None) {}
        function fadeInf(format_string, *args, line=-1, color=None, wait=1, reset=True, tag=None) {}
        function fadeOutf(format_string, *args, line=-1, color=None, wait=1, reset=True, tag=None) {}
        function length();
        const StringIndex;
        const epd;
    };
    ```

    除初始化方法外，StringBuffer 对象所有的方法都是非同步方法，都只在`当前玩家 == 本机玩家`的机器上生效

    ```JavaScript
    const buf = StringBuffer(64); // 初始化缓冲区尺寸

    setcurpl(P1); // 设置 当前玩家 为 P1
    buf.insert(0, "给 玩家1 显示的信息");  // 这一行仅仅会修改 P1 机器上的 buf，因为当前玩家是 P1

    if (getuserplayerid() == $P2) {     // 本机玩家是 P2 才会执行
        buf.insert(0, "这行代码没有卵用"); // 本机玩家是 P2 但当前玩家是 P1 所以这行代码不生效
    }

    setcurpl(P2); // 设置 当前玩家 为 P2
    buf.insert(0, "给 玩家2 显示的信息");  // 这一行仅仅会修改 P2 机器上的 buf，因为当前玩家是 P2

    setcurpl(P1);
    buf.Display(); // 给 玩家1 显示 “给 玩家1 显示的信息”

    setcurpl(P2);
    buf.Display(); // 给 玩家2 显示 “给 玩家2 显示的信息”
    ```

- #### StringBuffer

    - `StringBuffer`(content)  
        若 [content] 是字符串或者字节串，则以该字符串或字节串初始化一个 StringBuffer 对象  
        若 [content] 是一个整数，则以 [content] 作为尺寸尺寸初始化一个 StringBuffer 对象  
        [content] 是可选参数，默认为整数 218  

    ```JavaScript
    const s1 = StringBuffer();         // 尺寸为 218 的 StringBuffer 对象，初始内容为 218 个回车符
    const s2 = StringBuffer(64);       // 尺寸为  64 的 StringBuffer 对象，初始内容为  64 个回车符
    const s3 = StringBuffer("havonz"); // 尺寸为   6 的 StringBuffer 对象，初始内容为 “havonz”
    ```


- #### .insert

    - `.insert`(index, *args)  
        将可变参数 [*args] 转换成字符串按顺序插入到`当前玩家`机器上的 StringBuffer 对象缓冲区的 `[index] * 4` 位置（索引不是 4 的倍数就不能用这个了）  

    - `.insertf`(index, format_string, *args)  
        将可变参数 [*args] 使用格式 [format_string] 格式化后插入到`当前玩家`机器上的 StringBuffer 对象缓冲区的 `[index] * 4` 位置（索引不是 4 的倍数就不能用这个了）  

    ```JavaScript
    const s1 = StringBuffer();
    s1.insert(0, "havonz");
    s1.insert(1, "0");
    s1.Display(); // havo0
    ```


- #### .append

    - `.append`(*args)  
        将可变参数 [*args] 转换成字符串按顺序插入到`当前玩家`机器上的 StringBuffer 对象缓冲区中的字符串的末尾  

    - `.appendf`(format_string, *args)  
        将可变参数 [*args] 使用格式 [format_string] 格式化后插入到`当前玩家`机器上的 StringBuffer 对象缓冲区中的字符串的末尾  

    ```JavaScript
    const s1 = StringBuffer();
    setcurpl(getuserplayerid());
    s1.insert(0);
    s1.append("你好！");
    s1.appendf("{}{}", PColor(getuserplayerid()), PName(getuserplayerid()) );
    s1.Display();
    ```


- #### .delete

    - `.delete`(start, length=1)  
        从`当前玩家`机器上的 StringBuffer 对象的 `[start] * 4` 索引位置删除掉 `[length] * 4` 个字节（索引不是 4 的倍数就不能用这个了）  

        

- #### .Display

    - `.Display()`  
        将 StringBuffer 缓冲区的字符串打印到`当前玩家`屏幕滚动信息的最底下哪一行  
    - `.DisplayAt`(line)  
        将 StringBuffer 缓冲区的字符串打印到`当前玩家`屏幕滚动信息从上到下第 [line] 行  

    ```JavaScript
    const s1 = StringBuffer();
    setcurpl(getuserplayerid());
    s1.insert(0, "你好！星际争霸");
    s1.DisplayAt(0); // 输出到最上一行
    s1.Display();    // 输出到最下一行
    ```


- #### .print

    - `.print`(*args)  
        使用当前 StringBuffer 将多个参数 [*args] 按顺序打印到`当前玩家`屏幕滚动信息的下一行，并把最底下的信息往上滚动  

    - `.printf`(formatstring, *args)  
        使用当前 StringBuffer 以格式 [format_string] 格式化打印多个参数 [*args] 到`当前玩家`屏幕滚动信息的下一行，并把最底下的信息往上滚动  

    - `.printfAt`(line, formatstring, *args)  
        使用当前 StringBuffer 以格式 [format_string] 格式化打印多个参数 [*args] 到`当前玩家`屏幕滚动信息的从上往下的第 [line] 行（取值范围 0~10）  

    ```JavaScript
    const s1 = StringBuffer();
    setcurpl(getuserplayerid());
    s1.print("你好！星际争霸"); // 在最底下滚出一条信息
    s1.printf("你好！{}{}", PColor(getuserplayerid()), PName(getuserplayerid()) ); // 将上一条信息往上滚动，并将这条消息滚出来
    s1.printfAt(0, "你好！{}{}", PColor(getuserplayerid()), PName(getuserplayerid()) ); // 将这条信息从最上一行打印出来
    ```

      

- #### .Play

    - `.Play()`  
        将`当前玩家`机器上的 StringBuffer 对象的内容作为声音文件名，播放该声音文件  
        当目标声音文件包含本地化声音时，则使用 StringBuffer 动态拼接的文件名会无法播放  

    ```JavaScript
    setcurpl(P1);
    buf.insert(0, "sound\\Zerg\\Devourer\\");
    buf.append("ZDvPss00.WAV\0");
    buf.Display();    // 在 玩家1 的屏幕上的下一行输出 “sound\Zerg\Devourer\ZDvPss00.WAV”
    buf.DisplayAt(9); // 在 玩家1 的屏幕上的第十行输出 “sound\Zerg\Devourer\ZDvPss00.WAV”
    buf.Play();       // 找到文字指向的 wav 并在 玩家1 的电脑上播放它

    StringBuffer("sound\\terran\\advisor\\tadupd04.wav").Play(); // nuclear launch detected.
    ```

    

- #### .fade

    - `.fadeIn`(*args, line=0, color=None, wait=1, reset=true, tag=None)  
        使 [*args] 组合成一个文本从 [line] 行以 [clolor] 颜色渐显出现，步间隔帧数为 [wait]，是否重置 [reset]，特效文本标签为 [tag]，循环调用，返回非 0 表示特效尚未完成还需继续调用，返回 0 表示特效已完成  

    - `.fadeOut`(*args, line=0, color=None, wait=1, reset=true, tag=None)  
        使 [*args] 组合成一个文本从 [line] 行以 [clolor] 颜色渐隐消失，步间隔帧数为 [wait]，是否重置 [reset]，特效文本标签为 [tag]，循环调用，返回非 0 表示特效尚未完成还需继续调用，返回 0 表示特效已完成  

    - `.fadeInf`(format_string, *args, line=0, color=None, wait=1, reset=true, tag=None)  
        使 [*args] 使用 [format_string] 格式化成一个文本从 [line] 行以 [clolor] 颜色渐显出现，步间隔帧数为 [wait]，是否重置 [reset]，特效文本标签为 [tag]，返回非 0 表示特效尚未完成还需继续调用，返回 0 表示特效已完成  

    - `.fadeOutf`(format_string, *args, line=0, color=None, wait=1, reset=true, tag=None)  
        使 [*args] 使用 [format_string] 格式化成一个文本从 [line] 行以 [clolor] 颜色渐隐消失，步间隔帧数为 [wait]，是否重置 [reset]，特效文本标签为 [tag]，返回非 0 表示特效尚未完成还需继续调用，返回 0 表示特效已完成  

    ```JavaScript
    function 单次渐显然后渐隐文字() {
        const buf = StringBuffer(128);
        const onceWait = EUDLightVariable(0);

        if (getcurpl() != getuserplayerid()) {
            return;
        }

        if (onceWait >= 10000) {
            return;
        }

        const text = py_str("\x13\x04失去\x19人\x04性\n\x13\x04失去\x19很多\x04\n\x13\x04失去\x19兽\x04性\n\x13\x04失去\x19一切\x04");

        const tecolor = 4, 2, 0x1E, 5, 0;

        if (onceWait <= 0) {
            if ( 0 != buf.fadeIn(text, line = 3, color = tecolor, wait = 2, tag = py_str("渐显特效")) ) {
                return;
            }
        }

        if (onceWait <= 100) {
            DoActions(onceWait.AddNumber(1));
            return;
        }

        TextFX_SetTimer("渐显特效", SetTo, 0);
        TextFX_Remove("渐显特效");

        if ( 0 != buf.fadeOut(text, line = 3, color = tecolor, wait = 2, tag = py_str("渐隐特效")) ) {
            return;
        }

        TextFX_SetTimer("渐隐特效", SetTo, 0);
        TextFX_Remove("渐隐特效");
        DoActions(onceWait.SetNumber(10000)); /* 执行完了 */
    }

    function beforeTriggerExec() {
        const cp = getcurpl();
    
        setcurpl(getuserplayerid());
        单次渐显然后渐隐文字();

        setcurpl(cp);
    }
    ```

<br />

- ### Db

    静态内存字节数据类型

    ```JavaScript
    object Db {
    function constructor(content) {}
    function GetDataSize() {}
    };
    ```

    支持使用整数、字符串、字节串初始化一段内存字节数据

    使用`Db("string")`等同于`Db(b"string\0")`(UTF-8)

    ```JavaScript
    const buf1 = Db(b"string\0"); // Db(b"string\0")
    const buf2 = Db("string");    // Db(b"string\0")
    const buf3 = Db(5);           // Db(b"\0\0\0\0\0")
    ```

<br />

- ### EUDByteStream

    内存字节流操作类

    ```JavaScript
    object EUDByteStream {
        function seekepd(epd) {}
        function seekoffset(ptr) {}
        function copyto(stream : EUDByteStream) {}
        function readbyte() {}
        function writebyte(byte) {}
    }
    ```

    ```JavaScript
    const buf = Db(b"\0uck fu\0k fuck");
    sprintf(buf, "908 + 8 = {}", 908 + 8);
    StringBuffer().printAt(6, ptr2s(buf));
    const stream = EUDByteStream();
    stream.seekoffset(buf);
    StringBuffer().printAt(7, stream.readbyte());
    stream.seekoffset(buf);
    stream.writebyte(97);
    stream.writebyte(98);
    stream.writebyte(99);
    StringBuffer().printAt(8, ptr2s(buf));
    ```

<br />

- ### ~~CPString~~

    **已废弃**
    CPTrick 优化的字符串缓冲操作类

    ```JavaScript
    object CPString {
    function constructor(content) {}
    function Display() {}
    function GetVTable() {}
    };
    ```

    ```JavaScript
    const s1 = CPString("一个字符串");
    const s2 = CPString(b"stringstringstring");
    const s3 = CPString(64);
    ```

<br />

- ### ~~DBString~~

    **已废弃**
    静态内存字符串

    ```JavaScript
    object DBString {
    function constructor(content) {}
    function GetStringMemoryAddr() {}
    function Display() {}
    function Play() {}
    };
    ```

    ```JavaScript
    const s = DBString("一个很长的字符串\0一个很长的字符串");
    const buf = s.GetStringMemoryAddr();
    s.Display();
    sprintf(buf, "908 + 8 = {}", 908 + 8);
    s.Display();
    ```

    

  

  

  





