# epScript 内建函数

</br>

## 触发器条件和动作

- ### 普通条件函数

    普通条件函数是依据传统触发器中的条件封装的函数，就像 ScmDraft2 里面的触发器条件那样  
    所有的触发器条件函数都会返回一个触发器条件表达式常量（而非逻辑值），`条件表达式`和`条件表达式的结果`这两个概念需要区分清楚  
    你如果需要用变量储存条件表达式判断的结果，则应该将其传入一个触发器的条件列表或者当作 if 语法参数，也可用 l2v 取条件表达式的运行时结果，参考以下示例  
    ```JavaScript
    var vc0 = Accumulate(P1, AtLeast, 500, Ore);  // 这是不对的！！！它不返回一个逻辑值。
    const c1 = Accumulate(P1, AtLeast, 500, Ore); // 这样可以，它返回的就是一个常量条件表达式，它可以当作 RawTrigger 或者 Trigger 的 conditions 参数

    // 使用变量存储条件返回的逻辑值方法
    var vc1 = 0;
    Trigger(
        conditions = Accumulate(P1, AtLeast, 500, Ore),
        actions = vc1.SetNumber(1),
    );
    var vc2 = l2v(Accumulate(P1, AtLeast, 500, Ore));
    ```

    </br>

    - ####  **Accumulate**

        - `Accumulate`(玩家 : TrgPlayer, 不少于/不多于/等于 : TrgComparison, 数值, 资源类型 : TrgResource) : Condition  
            判断 [玩家] 收集的 [资源类型] 是否 [不少于/不多于/等于] [数值]

        示例

        ```JavaScript
        if ( Accumulate(P1, AtLeast, 500, Ore) ) {
            // 如果 玩家1 收集不少于 500 水晶矿
        }
        ```


    </br>

    - #### **Bring**

        - `Bring`(玩家 : TrgPlayer, 不少于/不多于/等于 : TrgComparison, 数值, 单位类型 : TrgUnit, 指定区域 : TrgLocation) : Condition  
            判断 [玩家] 在 [指定区域] 的 [单位类型] 是否 [不少于/不多余/等于] [数值] 个  
            Bring 第二个参数为 AtMost （不多于）的情况下，会检测到尚未建造完成的建筑、孵化中的虫卵；不会检测到被装载的单位、尚在训练中的单位；会忽略掉 [指定区域] 的高度设置  
            Bring 第二个参数为 AtLeast/Exactly （不少于/等于）的情况下，会检测到被装载的单位；不会检测到尚在训练中的单位、尚未建造完成的建筑、孵化中的虫卵  
            Bring 无法检测到 Scanner Sweep（扫雷达特效单位）和 Map Revealers（地图小雷达）  
            使用 KillUnit 或 KillUnitAt 杀死的单位在当前帧仍然可以被 Bring 条件检测到；使用 RemoveUnit 或 RemoveUnitAt 移除的单位在当前帧不再可以被 Bring 条件检测到，并且也会让在这之前用 KillUnit 或 KillUnitAt 杀死的单位也不再被 Bring 检测到。  

        示例
        ```JavaScript
        KillUnitAt(All, "Terran Marine", $L("Location 1"), P1); // 杀死 玩家1 在 Location 1 所有的 机枪兵，这个动作之后在当前帧 Bring 条件仍然可以检测到 玩家1 在 Location 1 的 机枪兵
        RemoveUnitAt(1, "Map Revealer", "Anywhere", P1);        // 这个动作不会移除任何单位，但是会刷新当前帧在这之前用 KillUnit、KillUnitAt 杀死的所有单位，以确保 Bring 不再会检测到当前帧被杀死的单位
        if ( Bring(P1, AtLeast, 15, "Terran Marine", $L("Location 1")) ) {
            // 如果 玩家1 在 Location 1 这个区域的机枪兵数量不少于 15 个
        }
        ```


    </br>

    - #### **Command**

        - `Command`(玩家 : TrgPlayer, 不少于/不多于/等于 : TrgComparison, 数值, 单位类型 : TrgUnit) : Condition  
            判断地图上受 [玩家] 控制的 [单位类型] 是否 [不少于/不多于/等于] [数值] 个 
            Command 条件的第二个参数为 AtMost （不多于）的情况下，会检测到被装载的单位、尚在训练中的单位、尚未建造完成的建筑、孵化中的虫卵  
            Command 条件的第二个参数为 AtLeast/Exactly （不少于/等于）的情况下，会检测到已装载的单位；不会检测到尚在训练中的单位、尚未建造完成的建筑、孵化中的虫卵  
            Command 可以检测到 Scanner Sweep（扫雷达特效单位）和 Map Revealers（地图小雷达）  
            使用 KillUnit、KillUnitAt、RemoveUnit、RemoveUnitAt 杀死移除的单位在当前帧仍然可以被 Command 条件检测到。

        - `CommandMost`(单位类型 : TrgUnit) : Condition  
            判断地图上受到当前玩家控制的 [单位类型] 是否比其它任何一个玩家都多（包括中立玩家）

        - `CommandLeast`(单位类型 : TrgUnit) : Condition  
            判断地图上受到当前玩家控制的 [单位类型] 是否比其它任何一个玩家都少（包括中立玩家）

        - `CommandMostAt`(单位类型 : TrgUnit, 指定区域 : TrgLocation) : Condition  
            判断 [指定区域] 受到当前玩家控制的 [单位类型] 是否比其它任何一个玩家都多（包括中立玩家）

        - `CommandLeastAt`(单位类型 : TrgUnit, 指定区域 : TrgLocation) : Condition  
            判断 [指定区域] 受到当前玩家控制的 [单位类型] 是否比其它任何一个玩家都少（包括中立玩家）

        示例

        ```JavaScript
        const cp = getcurpl();

        foreach (p: EUDLoopPlayer()) {
            setcurpl(p);
            if ( Command(CurrentPlayer, AtMost, 0, "(buildings)") ) { // 当 Command 条件的第二个参数为 AtMost 的时候，统计会包含尚未建造完的单位/建筑
                Defeat(); // 当前玩家的建筑物不多于 0 的情况下，判定为失败
            }

            if ( CommandMost("Terran Marine") ) {
                println("玩家 {} 的机枪兵最多", p);
            }
        
            if ( CommandLeast("Terran Marine") ) {
                println("玩家 {} 的机枪兵最少", p);
            }
        
            if ( CommandMostAt("Terran Marine", $L("Location 1")) ) {
                println("玩家 {} 在 Location 1 的机枪兵最多", p);
            }
        
            if ( CommandLeastAt("Terran Marine", $L("Location 1")) ) {
                println("玩家 {} 在 Location 1 的机枪兵最少", p);
            }
        }

        setcurpl(cp);
        ```


    </br>

    - #### **CountdownTimer**

        - `CountdownTimer`(不少于/不多于/等于 : TrgComparison, 秒数) : Condition  
            判断倒数计时器剩余的秒数是否 [不少于/不多于/等于] [秒数] 游戏秒

        该条件不应该使用 等于（Exactly）方法判断，因为并不是每游戏秒都会有一次触发器轮询，一游戏秒为 16 游戏帧（Frame），不等于一现实秒

        示例

        ```JavaScript
        if ( CountdownTimer(AtMost, 1) ) {
            PauseTimer();
        }
        ```

    </br>

    - #### **Deaths**

        - `Deaths`(玩家 : TrgPlayer, 不少于/不多于/等于 : TrgComparison, 数值, 单位类型 : TrgUnit) : Condition  
            判断 [玩家] 的 [单位类型] 死亡数量是否 [不少于/不多于/等于] [数值] 个  
            <details><summary>当 [玩家] 或者 [单位类型] 不在合理的范围时</summary>

            它就是 EUD 条件，功能为判断 `0x58A364 + ([玩家] * 4 + [单位类型] * 48)` 这个内存地址上存储的 32 位正整数值是否 \[不少于/不多于/等于\] \[数值\]  
            其同步性取决于`0x58A364 + ([玩家] * 4 + [单位类型] * 48)`这个内存地址上存储的数据的同步性
            </details>

        - `DeathsX`(玩家: TrgPlayer, 不少于/不多于/等于: TrgComparison, 数值, 单位类型: TrgUnit, 掩码) : Condition  
            <details><summary>这个条件通常不用于判断玩家单位死亡数</summary>

            通常用于判断 `0x58A364 + ([玩家] * 4 + [单位类型] * 48)` 这个内存地址上存储的 32 位 \(正整数值 & \[掩码\]\) 是否 \[不少于/不多于/等于\] \[数值\]  
            其同步性取决于`0x58A364 + ([玩家] * 4 + [单位类型] * 48)`这个内存地址上存储的数据的同步性  
            </details>

        > **Note**
        > 使用触发器动作杀死或者移除的单位不计入死亡数（Deaths）；  
        > 自爆单位（Zerg Scourge 爆蚊、Infested Terran 自爆人、Vulture Spider Mine 蜘蛛雷）成功自爆不会计入死亡数（Deaths），但被其它单位杀死（没有成功自爆）则计入死亡数；  
        > 被己方或同盟杀死的单位也会计入死亡数（Deaths）。

        示例

        ```JavaScript
        if ( Deaths(P1, AtLeast, 15, "Terran Marine") ) {
            // 如果 玩家1 有不少于 15 个机枪兵死亡
        }
        ```

    </br>

    - #### Memory

        - `Memory`(内存地址, 不少于/不多于/等于: TrgComparison, 数值) : Condition  
            判断 [内存地址] 上存储的 32 位正整数值是否 [不少于/不多于/等于] [数值]  
            其同步性取决于 [内存地址] 上存储的数据的同步性  

        - `MemoryX`(内存地址, 不少于/不多于/等于: TrgComparison, 数值, 掩码) : Condition  
            判断 [内存地址] 的上存储的 32 位 (正整数值 & [掩码]) 是否 [不少于/不多于/等于] [数值]  
            其同步性取决于 [内存地址] 上存储的数据的同步性  

        - `MemoryEPD`(epd, 不少于/不多于/等于: TrgComparison, 数值) : Condition  
            判断`0x58A364 + ([epd] * 4)`这个内存地址上存储的 32 位正整数值是否 [不少于/不多于/等于] [数值]  
            其同步性取决于`0x58A364 + ([epd] * 4)`这个内存地址上存储的数据的同步性  

        - `MemoryXEPD`(epd, 不少于/不多于/等于: TrgComparison, 数值, 掩码) : Condition  
            判断`0x58A364 + ([epd] * 4)`这个内存地址上存储的 32 位 (正整数值 & [掩码]) 是否 [不少于/不多于/等于] [数值]  
            其同步性取决于`0x58A364 + ([epd] * 4)`这个内存地址上存储的数据的同步性  

        示例
        ```JavaScript
        function MorphLarvaEPD(epd, newUnit: TrgUnit) {
            if (MemoryXEPD(epd + 0x64/4, Exactly, 35, 0xFFFF)) {
                SetMemoryXEPD(epd + 0x4D/4, SetTo, 42 << 8, 0xFFFF00);
                SetMemoryXEPD(epd + 0x98/4, SetTo, newUnit, 0xFFFF);
            }
        }
        ```

    </br>

    - #### Kills

        - `Kills`(玩家 : TrgPlayer, 不少于/不多于/等于 : TrgComparison, 数值, 单位类型 : TrgUnit) : Condition  
            判断 [玩家] 的 [单位类型] 击杀数是否 [不少于/不多于/等于] [数值] 个  
            击杀数 不是 击杀分数，注意区分  
            杀死己方或者同盟的单位不计入击杀数  

        示例

        ```JavaScript
        if ( Kills(P1, AtLeast, 15, "Terran Marine") ) {
            // 如果 玩家1 杀死了不少于 15 个机枪兵
        }
        ```

    </br>

    - #### **ElapsedTime**

        - `ElapsedTime`(不少于/不多于/等于 : TrgComparison, 游戏秒) : Condition  
            判断游戏逝去时间是否 [不少于/不多于/等于] [数值] 游戏秒

        该条件不应该使用 等于（Exactly）方法判断，因为并不是每游戏秒都会有一次触发器轮询，一游戏秒为 16 游戏帧（Frame），不等于一现实秒

        示例

        ```JavaScript
        if ( ElapsedTime(AtLeast, 5) ) {
            // 游戏逝去时间超过 5 游戏秒
        }
        ```

    </br>

    - #### **LeastKills/MostKills**

        - `LeastKills`(单位类型 : TrgUnit) : Condition  
            判断当前玩家击杀 [单位类型] 数量是否全场最少

        - `MostKills`(单位类型 : TrgUnit) : Condition  
            判断当前玩家击杀 [单位类型] 数量是否全场最多

        示例

        ```JavaScript
        if ( LeastKills("Terran Marine") ) {
            // 当前玩家杀死的机枪兵最少
        }

        if ( MostKills("Terran Marine") ) {
            // 当前玩家杀死的机枪兵最多
        }
        ```

    </br>

    - #### **LeastResources/MostResources**

        - `LeastResources`(资源类型 : TrgResource) : Condition  
            判断当前玩家的 [资源类型] 是否全场最少

        - `MostResources`(资源类型 : TrgResource) : Condition  
            判断当前玩家的 [资源类型] 是否全场最多

        示例

        ```JavaScript
        if ( LeastResources(Ore) ) {
            // 当前玩家水晶矿全场最少
        }

        if ( MostResources(Gas) ) {
            // 当前玩家气矿全场最多
        }
        ```

    </br>

    - #### **Opponents**

        - `Opponents`(玩家 : TrgPlayer, 不少于/不多于/等于 : TrgComparison, 数值) : Condition  
        判断 [玩家] 在当前游戏中的对手是否 [不少于/不多于/等于] [数值] 个

        示例

        ```JavaScript
        if ( Opponents(P1, AtMost, 2) ) {
            // 玩家1 的对手 不多于 2 个
        }
        ```

    </br>

    - #### **Score**

        - `Score`(玩家 : TrgPlayer, 得分类型 : TrgScore, 不少于/不多于/等于 : TrgComparison, 数值) : Condition  
            判断 [玩家] 的 [得分类型] 分数是否 [不少于/不多于/等于] [数值] 分

        - `LowestScore`(得分类型 : TrgScore) : Condition  
            判断当前玩家现在的 [得分类型] 是不是全场最低分

        - `HighestScore`(得分类型 : TrgScore) : Condition  
            判断当前玩家现在的 [得分类型] 是不是全场最高分

        示例

        ```JavaScript
        if ( Score(P1, Kills, AtLeast, 10000) ) {
            // 玩家1 的 击杀分数 不少于 10000 分，击杀分数 不是 击杀数，注意区别
        }

        if ( LowestScore(Buildings) ) {
            // 如果当前玩家现在的 建筑分 是最低分
        }

        if ( HighestScore(Kills) ) {
            // 如果当前玩家现在的 击杀分 是最高分
        }
        ```

    </br>

    - #### **Switch**

        - `Switch`(开关 : TrgSwitch, 开关状态 : TrgSwitchState) : Condition  
            判断 [开关] 的状态是否为 [开关状态]

        示例

        ```JavaScript
        if ( Switch($S("Switch 1"), Set) ) {
            // Switch 1 的状态是 Set
        }

        if ( Switch($S("Switch 1"), Cleared) ) {
            // Switch 1 的状态是 Cleared
        }
        ```

    </br>

    - #### **~~Always/Never~~**

        - ~~Always() : Condition~~  
            总是无条件执行，在 epScript 中似乎没啥用处

        - ~~Never**()** : Condition~~  
            总是不执行，在 epScript 中似乎没啥用处

    </br>
    </br>

- ### 扩展条件函数


    - #### IsUserCP

        - `IsUserCP()`: Condition  
            非同步条件，用于判断`本机玩家`是否为`当前玩家`

      

    - #### Is64BitWireframe

        - `Is64BitWireframe()`: Condition  
            非同步条件，判断本机星际客户端是否为 64 位的


    </br>
    </br>

- ### 普通动作函数

    普通动作函数是依据传统触发器中的动作封装的函数，就像 ScmDraft2 里面的触发器动作那样  
    所有的触发器动作函数（也包括扩展触发器函数）都返回一个动作表达式常量，`动作表达式`和`执行动作表达式`这两个概念需要区分清楚  
    若两个分号之间非注释代码仅为动作函数调用，epScript 会将它传入一个无条件触发器 DoActions 执行，参考示例  
    ```JavaScript
    const a1 = CenterView($L("Location 1")); // 这是声明一个动作常量表达式，并不会执行它
    CenterView($L("Location 1")); // 这表示执行一个动作，等同于 DoActions(CenterView($L("Location 1")));
    DoActions(a1); // 第一行声明的 a1 这个时候才执行
    ```

    </br>

    - #### **CenterView**

        - `CenterView`(指定位置 : TrgLocation) : Action  
            允许非同步执行，将当前玩家的镜头设置到指定位置

        示例

        ```JavaScript
        setcurpl(P1);
        CenterView($L("Location 1"));
        ```

    </br>

    - #### **CreateUnit**

        - `CreateUnit`(个数, 单位类型 : TrgUnit, 指定位置 : TrgLocation, 玩家 : TrgPlayer) : Action  
            给 [玩家] 在 [指定位置] 创建 [个数] 个 [单位类型]，单位创建的一瞬间，单位所占用的人口会立刻（在当前帧）上升。

        - `CreateUnitWithProperties`(个数, 单位类型 : TrgUnit, Where : 指定位置 : TrgLocation, 玩家 : TrgPlayer, 属性 : TrgProperty) : Action  
            给 [玩家] 在 [指定位置] 创建 [个数] 个 [单位类型] 并附带 [属性] 属性，单位创建的一瞬间，单位所占用的人口会立刻（在当前帧）上升。

        示例

        ```JavaScript
        CreateUnit(2, "Terran Siege Tank", $L("Location 1"), P1);
        CreateUnitWithProperties(1, "Terran Marine", $L("Location 1"), P1, UnitProperty(
        hitpoint = 100,       // 生命百分比
        shield = 100,         // 护盾百分比
        energy = 100,         // 能量百分比
        hanger = 0,           // 
        resource = 0,         //
        cloaked = False,      // 是否隐形
        burrowed = False,     // 是否已钻地
        intransit = False,    // 是否正在被运输
        hallucinated = False, // 是否是个幻影
        invincible = False)   // 是否无敌
        );
        ```

    </br>

    - #### **Defeat/Victory/Draw**

        - `Defeat()` : Action  
            当前玩家败北结束游戏

        - `Victory()` : Action  
            当前玩家获胜结束游戏

        - `Draw()` : Action  
            所有玩家平局结束游戏

    </br>

    - #### **DisplayText**

        - `DisplayText`(文本 : TrgString) : Action  
            允许非同步执行，在当前玩家屏幕上的文本区的下一行显示 [文本]  

            > 该动作的参数 [文本] 实际为该文本条目在地图字符串表（Map String Table）中的编号，假如这个条目在地图字符串表不存在，那么 epScript 会先将该 [文本] 插入到地图字符串表中然后将其编号作为它的参数。  

        示例

        ```JavaScript
        const idx = $T("Hello StarCraft!");
        dbstr_print(GetMapStringAddr(idx), "WTF StarCraft!");
        DisplayText("Hello StarCraft!"); // 输出 WTF StarCraft!
        ```

    </br>

    - #### **GiveUnits**

        - `GiveUnits`(个数 : TrgCount, 单位类型 : TrgUnit, 所有者 : TrgPlayer, 指定区域 : TrgLocation, 接手者 : TrgPlayer) : Action  
            将 [指定区域] 的最多 [个数] 个 [所有者] 玩家的 [单位类型] 送给 [接手者] 玩家，[个数] 为 0 代表所有（All）  

            > 这个动作会导致单位的集结点丢失

        示例

        ```JavaScript
        // 将 Location 1 区域的 玩家2 的最多 3个 机枪兵送给 玩家1
        GiveUnits(3, "Terran Marine", P2, $L("Location 1"), P1);
        ```

    </br>

    - #### **KillUnit**

        - `KillUnit`(单位类型 : TrgUnit, 玩家 : TrgPlayer) : Action  
            杀死 [玩家] 所有的 [单位类型]，包含尚在建造队列中的单位，可以杀死核弹井中尚未发射的核弹

        - `KillUnitAt`(个数 : TrgCount, 单位类型 : TrgUnit,  指定区域 : TrgLocation, 玩家 : TrgPlayer) : Action  
            杀死最多 [个数] 个 [玩家] 在 [指定区域] 的 [单位类型]，[个数] 为 0 代表所有（All），不包含尚在建造队列中的单位，不会杀死核弹井中尚未发射的核弹  

            > KillUnitAt(All, "Scanner Sweep", "Anywhere", P1) 无法杀死雷达特效单位  
            
            > **Warning**  
            > 该动作存在一个这样的 bug：  
            > 假如该动作执行后杀死了区域中的一个运输机/碉堡或任意装载设备中的任意一个单位，那么该运输机/碉堡或装载设备中的所有同类型单位都会被杀死，并且这些被杀死单位不会算在 [个数] 参数指定的个数以内  
            > 假如 KillUnitAt(1, "Terran Marine", "Location 1", P1) 这个动作执行后杀死了 Location 1 区域的一个碉堡中的机枪兵，那么该碉堡内的所有机枪兵都会被杀死，并且这个区域碉堡外如果还有机枪兵，还会被杀死一个  

        示例

        ```JavaScript
        KillUnit("Terran Marine", P1); // 杀死 玩家1 所有的机枪兵
        KillUnitAt(3, "Terran Siege Tank", $L("Location 1"), P1) // 杀死 玩家1 在 Location 1 区域的最多 3 个坦克
        ```

    </br>

    - #### **LeaderBoard**

        - `LeaderBoardComputerPlayers`(状态 : TrgPropState) : Action  
            设定排行榜对电脑玩家的启用状态

        - `LeaderBoardControl`(单位类型 : TrgUnit, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家 [单位类型] 控制数从大到小的排行榜

        - `LeaderBoardControlAt`(单位类型 : TrgUnit, 指定区域 : TrgLocation, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家在 [指定区域] 的 [单位类型] 控制数从大到小的排行榜

        - `LeaderBoardGoalControl`(目标个数, 单位类型 : TrgUnit, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家 [单位类型] 控制数最接近 [目标个数] 的排行榜

        - `LeaderBoardGoalControlAt`(目标个数, 单位类型 : TrgUnit, 指定区域 : TrgLocation, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家在 [指定区域] 的 [单位类型] 控制数最接近 [目标个数] 的排行榜

        - `LeaderBoardGoalKills`(目标个数, 单位类型 : TrgUnit, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家击杀 [单位类型] 数量最接近 [目标个数] 的排行榜

        - `LeaderBoardGoalResources`(目标数量, ResourceType : TrgResource, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家采集 [资源类型] 数量最接近 [目标数量] 的排行榜

        - `LeaderBoardGoalScore`(目标分数, 得分类型 : TrgScore, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家 [得分类型] 分数最接近 [目标分数] 的排行榜

        - `LeaderBoardGreed`(目标数量) : Action  
            显示所有玩家采集水晶矿和气矿最接近 [目标数量] 的排行榜

        - `LeaderBoardKills`(单位类型] : TrgUnit, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家击杀 [单位类型] 个数的排行榜

        - `LeaderBoardResources`(资源类型 : TrgResource, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家收集 [资源类型] 数量的排行榜

        - `LeaderBoardScore`(得分类型 : TrgScore, 标签 : TrgString) : Action  
            显示描述为 [标签] 所有玩家的 [得分类型] 得分的排行榜

        示例

        ```JavaScript
        LeaderBoardGoalControlAt(10, "Terran Marine", $L("目的地"), "机枪兵到达目地");
        ```

    </br>

    - #### **MinimapPing**

        - `MinimapPing`(指定位置 : TrgLocation) : Action  
            允许非同步执行，为当前玩家在小地图 [指定位置] 显示 Ping

        示例

        ```JavaScript
        // 给 玩家1 小地图的 Location 1 位置显示 Ping
        setcurpl(P1);
        MinimapPing($("Location 1"));
        ```

    </br>

    - #### **ModifyUnit**

        - `ModifyUnitEnergy`(个数 : TrgCount, 单位类型 : TrgUnit, 玩家 : TrgPlayer,  指定区域 : TrgLocation, 百分数) : Action  
            更改 [玩家] 在 [指定区域] 的最多 [个数] 个 [单位类型] 的能量为百分之 [百分数]，[个数] 为 0 代表所有（All）

        - `ModifyUnitHangarCount`(新增, 个数 : TrgCount, 单位类型 : TrgUnit, 玩家 : TrgPlayer, 指定区域 : TrgLocation) : Action  
            给 [玩家] 在 [指定区域] 的最多 [个数] 个 [单位类型] 增加最多 [新增] 装载单位，[个数] 为 0 代表所有（All）

        例如航母的小飞机、金甲的子弹；注意，该动作对无法增加雷车的地雷数量

        - `ModifyUnitHitPoints`(个数 : TrgCount, 单位类型 : TrgUnit, 玩家 : TrgPlayer, 指定区域 : TrgLocation, 百分数) : Action  
            更改 [玩家] 在 [指定区域] 的最多 [个数] 个 [单位类型] 的生命值为百分之 [百分数]，[个数] 为 0 代表所有（All）

        - `ModifyUnitResourceAmount`(个数 : TrgCount, 玩家 : TrgPlayer, 指定区域 : TrgLocation, 新值) : Action  
            将 [玩家] 在 [指定区域] 的最多 [个数] 个单位的资源值改为 [新值]，[个数] 为 0 代表所有（All）

        - `ModifyUnitShields`(个数 : TrgCount, 单位类型 : TrgUnit, 玩家 : TrgPlayer, 指定区域 : TrgLocation, 百分数) : Action  
            更改 [玩家] 在 [指定区域] 的最多 [个数] 个 [单位类型] 的护盾值为百分之 [百分数]，[个数] 为 0 代表所有（All）

        示例

        ```JavaScript
        // 将 玩家1 在 Location 1 区域的最多 100个 机枪兵 的生命值设置为 100%
        ModifyUnitHitPoints(100, "Terran Marine", P1, $L("Location 1"), 100);
        ```

    </br>

    - #### **MoveLocation**

        - `MoveLocation`(需要移动的位置 : TrgLocation, 目标单位类型 : TrgUnit, 玩家 : TrgPlayer, 目标区域 : TrgLocation) : Action  
            将 [需要移动的位置] 的中心对准到 [玩家] 在 [目标区域] 的某个 [目标单位类型] 身上

        示例

        ```JavaScript
        // 将 Location 1 区域的中心移动到 玩家1 在地图上任意位置的某个机枪兵身上
        MoveLocation($L("Location 1"), "Terran Marine", P1, $L("AnyWhere"));
        ```

    </br>

    - #### **MoveUnit**

        - `MoveUnit`(个数 : TrgCount, 单位类型 : TrgUnit, 玩家 : TrgPlayer, 起始区域 : TrgLocation, 目标区域 : TrgLocation) : Action  
            将 [玩家] 在 [起始区域] 最多 [个数] 个 [单位类型] 瞬间移动到 [目标区域]，[个数] 为 0 代表所有（All）

        示例

        ```JavaScript
        // 将 玩家1 在 Location 1 区域的最多 10个 机枪兵 瞬移到 Location 2
        MoveUnit(10, "Terran Marine", P1, $L("Location 1"), $L("Location 2"));
        ```

    </br>

    - #### **MuteUnitSpeech/UnMuteUnitSpeech**

        - `MuteUnitSpeech()` : Action  
            允许非同步执行，给当前玩家所有单位讲话静音（触发器单位讲话例外）

        - `UnMuteUnitSpeech()` : Action  
            允许非同步执行，给当前玩家所有单位讲话取消静音（触发器单位讲话例外）

    </br>

    - #### **Order**

        - `Order`(单位类型 : TrgUnit, 玩家 : TrgPlayer, 起始区域 : TrgLocation, 命令 : TrgOrder, 目标区域 : TrgLocation) : Action  
            对 [玩家] 在 [起始区域] 的 [单位类型] 发布 [命令] 指向 [目标区域] 中心

        示例

        ```JavaScript
        // 命令 玩家1 在 Location 1 的所有 机枪兵 攻击-移动到 Location 2 区域中心
        Order("Terran Marine", P1, $L("Location 1"), Attack, $L("Location 2"))
        ```

    </br>

    - #### **PauseGame/UnpauseGame**

        - `PauseGame()` : Action  
            为所有玩家暂停游戏

        - `UnpauseGame()` : Action  
            为所有玩家继续游戏

    </br>

    - #### **PauseTimer/UnpauseTimer**

        - `PauseTimer()` : Action  
            为所有玩家暂停倒数计时器

        - `UnpauseTimer()` : Action  
            为所有玩家继续倒数计时器

    </br>

    - #### **PlayWAV**

        - `PlayWAV`(WAVName : TrgString) : Action  
            允许非同步执行，为当前玩家播放一个 WAV 文件 [WAVName]

    </br>

    - #### **~~PreserveTrigger~~**

      - ~~PreserveTrigger() : Action~~  
        保留触发器，因为传统触发器执行一次后就失效，所以需要重复触发则需要加上这一条，epScript 中几乎用不着

    </br>

    - #### **RemoveUnit**

        - `RemoveUnit`(单位类型 : TrgUnit, 玩家 : TrgPlayer) : Action  
            从地图上移除 [玩家] 所有的 [单位类型]，包含尚在建造队列中的单位，可以移除核弹井中尚未发射的核弹，单位被移除后，单位所占用的人口要下一帧才会下降。

        - `RemoveUnitAt`(个数 : TrgCount, 单位类型 : TrgUnit, 指定区域 : TrgLocation, 玩家 : TrgPlayer) : Action

            从地图上移除 [玩家] 在 [指定区域] 最多 [个数] 个 [单位类型] ，[个数] 为 0 代表所有（All），不包含尚在建造队列中的单位，不会移除核弹井中尚未发射的核弹，单位被移除后，单位所占用的人口要下一帧才会下降。

            > RemoveUnitAt(All, "Scanner Sweep", "Anywhere", P1) 无法移除雷达特效单位  
            > RemoveUnitAt(All, "Map Revealer", "Anywhere", P1) 无法移除地图小雷达单位  

            > **Warning**  
            > 该动作存在一个这样的 bug：  
            > 假如该动作执行后移除了区域中的一个运输机/碉堡或任意装载设备中的任意一个单位，那么同属这个运输机/碉堡或装载设备中的所有同类型单位都会被移除，并且这些被移除单位不会算在 [个数] 参数指定的个数以内  
            > 假设 RemoveUnitAt(1, "Terran Marine", "Location 1", P1) 这个动作执行后移除了 Location 1 区域的一个碉堡中的机枪兵，那么该碉堡内的所有机枪兵都会被移除，并且这个区域碉堡外如果还有机枪兵，还会被移除一个

    </br>

    - #### **RunAIScript**

      - `RunAIScript`(Script : TrgAIScript) : Action  
            对当前玩家执行 AI 脚本 [Script] 

      - `RunAIScriptAt`(Script : TrgAIScript, 指定区域 : TrgLocation) : Action  
            对当前玩家在 [指定区域] 执行 AI 脚本 [Script]

        示例

        ```JavaScript
        RunAIScriptAt("Terran Custom Level", $L("Location 1"));
        ```

    </br>

    - #### **SetAllianceStatus**

      - `SetAllianceStatus`(目标玩家 : TrgPlayer, 状态 : TrgAllyStatus) : Action  
            设置当前玩家对 [目标玩家] 的联盟状态为 [状态]

        示例

        ```JavaScript
        // 设置 玩家1 对 玩家2 友善，玩家2 对 玩家1 敌对
        setcurpl(P1);
        SetAllianceStatus(P2, Ally);
        setcurpl(P2);
        SetAllianceStatus(P1, Enemy);
        ```

    </br>

    - #### **SetCountdownTimer**

      - `SetCountdownTimer`(设为/增加/减少 : TrgModifier, 游戏秒) : Action  
            设置倒数计时器 [设为/增加/减少] [游戏秒] 游戏秒，一游戏秒为 16 帧

        示例

        ```JavaScript
        SetCountdownTimer(SetTo, 100); // 将倒数计时器设置为 100 游戏秒
        SetCountdownTimer(Add, 5); // 给倒数计时器加 5 游戏秒
        SetCountdownTimer(Substract, 3); // 给倒数计时器减少 3 游戏秒
        ```

    </br>

    - #### **SetDeaths**

        - `SetDeaths`(玩家 : TrgPlayer, 设为/增加/减少 : TrgModifier, 数值, 单位类型 : TrgUnit) : Action  
            设置 [玩家] 的 [单位类型] 的死亡个数 [设为/增加/减少] [数值] 个  
            <details><summary>当 [玩家] 或者 [单位类型] 不在合理的范围时</summary>

            它将是一个 EUD 动作，功能为将 `0x58A364 + ([玩家] * 4 + [单位类型] * 48)` 这个内存地址上存储的当前值 [设为/增加/减少] [数值]  
            其是否允许非同步执行取决于`0x58A364 + ([玩家] * 4 + [单位类型] * 48)`这个内存地址上存储的数据的同步性
            </details>

      - `SetDeathsX`(玩家 : TrgPlayer, 设为/增加/减少: TrgModifier, 数值, 单位类型: TrgUnit, 掩码) : Action  
        <details><summary>这个函数通常不用于设置玩家单位死亡数量</summary>

        ```Markdown
        设为（SetTo）   ：将 `0x58A364 + ([玩家] * 4 + [单位类型] * 48)` 这个内存地址上存储的当前值设为 `当前值 - (当前值 & 掩码) + (数值 & 掩码)`
        增加（Add）     ：将 `0x58A364 + ([玩家] * 4 + [单位类型] * 48)` 这个内存地址上存储的当前值设为 `当前值 - (当前值 & 掩码) + ( ((当前值 & 掩码) + (数值 & 掩码)) & 掩码 )`
        减少（Subtract）：将 `0x58A364 + ([玩家] * 4 + [单位类型] * 48)` 这个内存地址上存储的当前值设为 `当前值 - (当前值 & 掩码) + ( ((当前值 & 掩码) - (数值 & 掩码)) & 掩码 )` （公式中的减法最小可以减为 0）
        ```
        其是否允许非同步执行取决于`0x58A364 + ([玩家] * 4 + [单位类型] * 48)`这个内存地址上存储的数据的同步性  
        </details>


        示例

        ```JavaScript
        SetDeaths(P1, Add, 10, "Terran Marine"); // 将 玩家1 的 机枪兵 死亡数 +10
        ```

    </br>

    - #### SetMemory

        - `SetMemory`(内存地址, 设为/增加/减少 : TrgModifier, 数值) : Action  
            将 [内存地址] 上的存储的 32 位正整数值 [设为/增加/减少] [数值]  
            其是否允许非同步执行取决于 [内存地址] 上存储的数据的同步性  

        - `SetMemoryX`(内存地址, 设为/增加/减少 : TrgModifier, 数值, 掩码) : Action  
            <details><summary>支持掩码访问的 SetMemory，可以修改 32 位中任何一位或多位的值</summary>

            ```Markdown
            设为（SetTo）   ：将 [内存地址] 上存储的当前值设为 `当前值 - (当前值 & 掩码) + (数值 & 掩码)`
            增加（Add）     ：将 [内存地址] 上存储的当前值设为 `当前值 - (当前值 & 掩码) + ( ((当前值 & 掩码) + (数值 & 掩码)) & 掩码 )`
            减少（Subtract）：将 [内存地址] 上存储的当前值设为 `当前值 - (当前值 & 掩码) + ( ((当前值 & 掩码) - (数值 & 掩码)) & 掩码 )` （公式中的减法最小可以减为 0）
            ```
            其是否允许非同步执行取决于 [内存地址] 上存储的数据的同步性  
            </details>


        - `SetMemoryEPD`(epd  : TrgPlayer, 设为/增加/减少 : TrgModifier, 数值) : Action  
            将`0x58A364 + ([epd] * 4)`这个内存地址上存储的 32 位正整数值 [设为/增加/减少] [数值]  
            其是否允许非同步执行取决于`0x58A364 + ([epd] * 4)`这个内存地址上存储的数据的同步性  

        - `SetMemoryXEPD`(epd  : TrgPlayer, 设为/增加/减少 : TrgModifier, 数值, 掩码) : Action  
            <details><summary>支持掩码访问的 SetMemoryEPD，可以修改 32 位中任何一位或多位的值</summary>

            ```Markdown
            设为（SetTo）   ：将 `0x58A364 + ([epd] * 4)` 这个内存地址上存储的当前值设为 `当前值 - (当前值 & 掩码) + (数值 & 掩码)`
            增加（Add）     ：将 `0x58A364 + ([epd] * 4)` 这个内存地址上存储的当前值设为 `当前值 - (当前值 & 掩码) + ( ((当前值 & 掩码) + (数值 & 掩码)) & 掩码 )`
            减少（Subtract）：将 `0x58A364 + ([epd] * 4)` 这个内存地址上存储的当前值设为 `当前值 - (当前值 & 掩码) + ( ((当前值 & 掩码) - (数值 & 掩码)) & 掩码 )` （公式中的减法最小可以减为 0）
            ```
            其是否允许非同步执行取决于`0x58A364 + ([epd] * 4)`这个内存地址上存储的数据的同步性  
            </details>

        示例
        ```JavaScript
        function MorphLarvaEPD(epd, newUnit: TrgUnit) {
            if (MemoryXEPD(epd + 0x64/4, Exactly, 35, 0xFFFF)) {
                SetMemoryXEPD(epd + 0x4D/4, SetTo, 42 << 8, 0xFFFF00);
                SetMemoryXEPD(epd + 0x98/4, SetTo, newUnit, 0xFFFF);
            }
        }
        ```

    </br>

    - #### **SetDoodadState**

        - `SetDoodadState`(State : TrgPropState, 单位类型 : TrgUnit, 玩家 : TrgPlayer, 指定区域 : TrgLocation) : Action  
        将 [玩家] 在 [指定区域] 中的 [单位类型] 的 Doodad 状态设置为 [State]

        示例

        ```JavaScript
        SetDoodadState(Enable, "Terran Marine", P1, $L("Location 1"));
        ```

    </br>

    - #### **SetInvincibility**

        - `SetInvincibility`(State : TrgPropState, 单位类型 : TrgUnit, 玩家 : TrgPlayer, 指定区域 : TrgLocation) : Action  
        设置 [玩家] 在 [指定区域] 的 [单位类型] 的无敌状态为 [State]

        示例

        ```JavaScript
        SetInvincibility(Enable, "Terran Marine", P1, $L("Location 1")); // 将 玩家1 在 Location 1 区域的 机枪兵 的无敌状态设置为 启用
        SetInvincibility(Disable, "Terran Marine", P1, $L("Location 1")); // 将 玩家1 在 Location 1 区域的 机枪兵 的无敌状态设置为 禁用
        ```

    </br>

    - #### **SetMissionObjectives**

        - `SetMissionObjectives`(文本 : TrgString) : Action  
            允许非同步执行，设置当前玩家任务目标描述为 [文本]  

            > 该动作的参数 [文本] 实际为该文本条目在地图字符串表（Map String Table）中的编号，假如这个条目在地图字符串表不存在，那么 epScript 会先将该 [文本] 插入到地图字符串表中然后将其编号作为它的参数。  

        示例

        ```JavaScript
        SetMissionObjectives("我们的目标是：\n没有蛀牙！");
        ```

    </br>

    - #### **SetNextScenario**

        - `SetNextScenario`(文本 : TrgString) : Action  
            设置本局游戏完成后要载入的下一张地图名 [文本]，必须要在同一目录  

            > 该动作的参数 [文本] 实际为该文本条目在地图字符串表（Map String Table）中的编号，假如这个条目在地图字符串表不存在，那么 epScript 会先将该 [文本] 插入到地图字符串表中然后将其编号作为它的参数。  

        

    </br>

    - #### **SetResources**

        - `SetResources`(玩家 : TrgPlayer, 设为/增加/减少 : TrgModifier, 数值, 资源类型 : TrgResource) : Action  
            设置 [玩家] 的 [资源类型] 资源 [设为/增加/减少] [数值]  

        示例

        ```JavaScript
        SetResources(P1, Add, 1000, Ore); // 给 玩家1 加 1000 水晶矿
        SetResources(P1, Substract, 1000, Gas); // 给 玩家1 扣掉 1000 气矿
        SetResources(P1, SetTo, 5000, OreAndGas); // 将 玩家1 的 水晶矿 和 气矿 都设置为 2000
        ```

    </br>

    - #### **SetScore**

        - `SetScore`(玩家 : TrgPlayer, 设为/增加/减少 : TrgModifier, 数值, 得分类型 : TrgScore) : Action  
            设置 [玩家] 的 [得分类型] 得分 [设为/增加/减少] [数值]

        示例

        ```JavaScript
        SetScore(P1, Add, 1000, Kills); // 给 玩家1 加 1000 分的 击杀分数
        ```

    </br>

    - #### **SetSwitch**

        - `SetSwitch`(开关名 : TrgSwitch, 开关操作 : TrgSwitchAction) : Action  
            将开关 [开关名] 的状态设置为 [开关操作]

        示例

        ```JavaScript
        SetSwitch($S("Switch 1"), Set); // 设置 Switch 1 的状态为 Set
        SetSwitch($S("Switch 1"), Clear); // 设置 Switch 1 的状态为 Cleared
        SetSwitch($S("Switch 1"), Toggle); // 转换 Switch 1 的状态，如果它本来是 Set 将会转换成 Cleared，如果本来是 Cleared 那么会转换到 Set
        SetSwitch($S("Switch 1"), Random); // 设置 Switch 1 为随机状态，使用之后 Switch 1 的状态可能是 Set 也可能是 Cleared
        ```

    </br>

    - #### **TalkingPortrait**

        - `TalkingPortrait`(单位类型 : TrgUnit, 毫秒) : Action  
            允许非同步执行，在当前玩家单位头像那里显示 [单位类型] 的头像持续 [毫秒] 游戏毫秒

        示例

        ```JavaScript
        // 让 机枪兵 为 玩家1 做出指示精神
        setcurpl(P1);
        TalkingPortrait("Terran Marine", 5000);
        ```

    </br>

    - #### **Transmission**

        - `Transmission`(单位类型 : TrgUnit, 指定区域 : TrgLocation, WAVName : TrgString, 设为/增加/减少 : TrgModifier, Time, 文本 : TrgString) : Action  
            允许非同步执行，为当前玩家播放一段声音 [WAVName] 并且让玩家单位头像那里显示 [指定区域] 内 [单位类型] 头像 [设为/增加/减少] [Time] 游戏毫秒，同时在小地图该单位上发出 Ping 同时输出文字信息 [文本]

            > 该动作的参数 [文本]/[WAVName] 实际为该文本条目在地图字符串表（Map String Table）中的编号，假如这个条目在地图字符串表不存在，那么 epScript 会先将该 [文本]/[WAVName] 插入到地图字符串表中然后将其编号作为它的参数。  

        

        示例

        ```JavaScript
        Transmission("Terran Marine", $L("Location 3"), "sound\\Zerg\\Advisor\\ZAdUpd00.WAV", Add, 5000, "我们的目标是：\n没有蛀牙！");
        ```

    </br>

    - #### **~~Wait~~**

        - ~~Wait(毫秒) : Action~~

            等待 [毫秒] （在 epScript 中建议不使用）


    </br>
    </br>


- ### 扩展动作函数

    扩展动作函数是一些无法在传统触发器中找到对应的动作的函数，但它仍然可以看作是触发器动作，会返回动作常量表达式或表达式列表  
    可以加入 RawTrigger 或 DoActions 的动作列表，它可能不再是单一的触发器动作  

    </br>

    - #### SetKills

        - `SetKills`(玩家: TrgPlayer, 设为/增加/减少 : TrgModifier, 数值, 单位类型: TrgUnit) : Action | [Action]  
            设置 [玩家] 的对 [单位类型] 击杀数 [设为/增加/减少] [数值]  
            击杀数 不是 击杀分数，注意区分  
            由一条或者三条传统触发器构成，取决于玩家编号是否为 CurrentPlayer  

        示例

        ```JavaScript
        // https://euddb.website/?pg=entry&id=502
        // 事实上传统触发器是没有 SetKills 这个动作的，它是用 EUD 模拟出来的

        // 如果 玩家编号 不是 CurrentPlayer(13) 的情况下，它的内部实现大概是这样，返回一条常量表达式
        SetDeaths(玩家编号 - 2736, 设为/增加/减少, 数值, 单位类型);

        // 如果 玩家编号 是 CurrentPlayer(13) 的情况下，它的内部实现大概是这样，返回包含三条常量表达式的 tuple
        DoActions(
            SetDeaths(EPD(0x6509B0), Add, -2736, 0),
            SetDeaths(CurrentPlayer, 设为/增加/减少, 数值, 单位类型),
            SetDeaths(EPD(0x6509B0), Add, 2736, 0),
        );
        ```

    </br>

    - #### SetCurrentPlayer

        - `SetCurrentPlayer`(playerid : TrgPlayer) : [Action]  
            允许非同步执行，将`当前玩家`和 cpcache 设置为 [playerid]  
            由三条传统触发器动作构成  

        示例

        ```JavaScript
        RawTrigger(actions = list(
            SetCurrentPlayer(P1),
            DisplayText("给玩家1显示的内容"),
            SetCurrentPlayer(P2),
            DisplayText("给玩家2显示的内容"),
            SetCurrentPlayer(P3),
            DisplayText("给玩家3显示的内容"),
        ));
        ```

    </br>

    - #### AddCurrentPlayer

        - `AddCurrentPlayer`(playerid : TrgPlayer) : [Action]  
            允许非同步执行，将`当前玩家`和 cpcache 自增 [playerid]  
            由三条传统触发器动作构成  

        示例

        ```JavaScript
        RawTrigger(actions = list(
            SetCurrentPlayer(P3),
            DisplayText("给玩家3显示的内容"),
            AddCurrentPlayer(-1),
            DisplayText("给玩家2显示的内容"),
            AddCurrentPlayer(-1),
            DisplayText("给玩家1显示的内容"),
        ));

        // https://euddb.website/?pg=entry&id=426
        // 将 Slowest 到 Fastest 游戏速度都设置为 200%
        RawTrigger(actions = list(
            SetCurrentPlayer(EPD(0x5124D8)),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
            AddCurrentPlayer(1),
            SetDeaths(CurrentPlayer, SetTo, 21, 0),
        ));
        ```

    </br>

    - #### DisplayTextAll

        - `DisplayTextAll`(文本 : TrgString) : [Action]  
            为所有的玩家（包括观察者）文本区的下一行显示 [文本]  
            由两条传统触发器动作构成

    </br>

    - #### PlayWAVAll

        - `PlayWAVAll`(声音路径) : [Action]  
            为所有的玩家（包括观察者）播放声音 [声音路径]  
            由两条传统触发器动作构成  

    </br>

    - #### MinimapPingAll

        - `MinimapPingAll`(位置) : [Action]  
            为所有的玩家（包括观察者）在小地图上的 [位置] 发一个 Ping  
            由两条传统触发器动作构成  

    </br>

    - #### CenterViewAll

        - `CenterViewAll`(位置) : [Action]  
            将所有的玩家（包括观察者）的镜头设置到 [位置]  
            由两条传统触发器动作构成  

    </br>

    - #### SetMissionObjectivesAll

        - `SetMissionObjectivesAll`(文本 : TrgString) : [Action]  
            设置所有玩家（包括观察者）的任务目标内容更改为 [文本]  
            由两条传统触发器动作构成  

    </br>

    - #### TalkingPortraitAll

        - `TalkingPortraitAll`(单位类型, 毫秒) : [Action]  
            在所有玩家（包括观察者）单位头像那里显示 [单位类型] 的头像持续 [毫秒] 游戏毫秒  
            由两条传统触发器动作构成  

    </br>

    - #### SetNextPtr

        - `SetNextPtr`(trg, dest) : Action  
            设置触发器 [trg] 的下一个触发器指针为 [dest]  
            它实质是一个 SetDeaths 动作 `SetDeaths(EPD(trg + 4), SetTo, dest, 0)`  

        示例
        ```javascript
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
        


    </br>
    </br>
    </br>

## 扩展函数

- ### 编译期

    - #### **编号索引**

        - `$L`(区域名称 : 字面量字符串) : py_int  
        - `GetLocationIndex`(区域名称 : py_str) : py_int  
        - `EncodeLocation`(区域名称 : py_str) : py_int  
            获取在地图编辑器（通常是 SCMD）中划定的区域的 [区域名称] 转换为对应的区域编号，所有函数的 TrgLocation 类型参数都会自动使用这个宏处理  

        - `$T`(文本 : 字面量字符串) : py_int  
        - `GetStringIndex`(文本 : py_str) : py_int  
        - `EncodeString`(文本 : py_str) : py_int  
            获取一个 [文本] 条目在地图字符串表（Map String Table）中的编号，所有函数的 TrgString 类型参数都会自动使用这个宏处理，也就是说接受 TrgString 类型作为参数的函数实际上接受的参数是字符串条目在地图字符串表中的编号。  

            > 该宏可以任意提供 [文本] 键，假如地图字符串表已经存在该 [文本] 条目，则返回该条目在地图字符串表中的编号，如果地图字符串没有 [文本] 条目，就会新建一个 编号→文本 键值对插入地图字符串表中，并返回这个编号。

            > 比如 $T("Force 1") 会得到 4，因为已经存在了。而 $T("\x03啥也不是的农民") 有可能会返回 3（同时在地图字符串字典中新建一项 3 : "\x03啥也不是的农民" ）

        - `$S`(开关名称 : 字面量字符串) : py_int  
        - `GetSwitchIndex`(开关名称 : py_str) : py_int  
        - `EncodeSwitch`(开关名称 : py_str) : py_int  
            获取在地图编辑器（通常是 SCMD）中定义的 [开关名称] 转换为对应的开关编号，所有函数的 TrgSwitch 类型参数都会自动使用这个宏处理  

        - `$U`(单位类型 : 字面量字符串) : py_int  
        - `GetUnitIndex`(单位类型 : py_str) : py_int  
        - `EncodeUnit`(单位类型 : py_str) : py_int  
            获取地图 unit.dat 的 [单位类型] 转换为对应的单位类型编号，所有函数的 TrgUnit 类型参数都会自动使用这个宏处理  

        - `$B`(TBLKey : 字面量字符串) : py_int  
        - `EncodeTBL`(TBLKey : py_str) : py_int  
            获取地图 TBL 字典中 [TBLKey] 对应的索引编号，所有函数的 StatText 类型参数都会自动使用这个宏处理  

        - `EncodeWeapon`(武器类型 : py_str) : py_int  
            获取地图 weapon.dat 的 [武器类型] 转换为对应的武器类型编号，所有函数的 Weapon 类型参数都会自动使用这个宏处理  

        - `EncodeTech`(科技类型 : py_str) : py_int  
            获取地图 tech.dat 的 [科技类型] 转换为对应的科技类型编号，所有函数的 Tech 类型参数都会自动使用这个宏处理  

        所有的 $ 语法宏（$L、$T、$S、$U、$B）都仅支持字面量字符串作为参数  

        示例

        ```PHP
        const l1 = $L("Location 1");
        const s2 = $S("Switch 2");
        const ut = $U("Terran Marine"); // 返回 0
        const aiid = $B("AI Harass Here"); // 返回 1538

        // 更改 SCV 单位的名称字符串编号，$T("\x03啥也不是的农民") 会在编译时插入一个新字符串到地图中，并把这个字符串的编号返回到这儿
        // https://euddb.website/?pg=entry&id=258
        wwrite(0x660260 + 2 * $U("Terran SCV"), $T("\x03啥也不是的农民"));

        // 可以直接使用 $ 语法的常量
        // EncodePlayer
        if (EncodePlayer(P1) == $P1) { py_print($P1, $P2, $CurrentPlayer, $AllPlayers, $Force1, $NonAlliedVictoryPlayers); }
        // EncodeModifier
        if (EncodeModifier(SetTo) == $SetTo) { py_print($SetTo, $Add, $Subtract); }
        // EncodeComparison
        if (EncodeComparison(AtLeast) == $AtLeast) { py_print($Exactly, $AtLeast, $AtMost); }
        // EncodeResource
        if (EncodeResource(OreAndGas) == $OreAndGas) { py_print($Ore, $Gas, $OreAndGas); }
        // EncodeSwitchAction
        if (EncodeSwitchAction(Set) == $Set) { py_print($Set, $Clear, $Toggle, $Random); } // $Set 代表 EncodeSwitchAction(Set)
        // EncodeSwitchState
        if (EncodeSwitchState(Set) != $Set) { py_print($Cleared); } // 注意这个！！！$Set 不代表 EncodeSwitchState(Set)
        // EncodeAllyStatus
        if (EncodeAllyStatus(Ally) == $Ally) { py_print($Enemy, $Ally, $AlliedVictory); }
        // EncodeOrder
        if (EncodeOrder(Move) == $Move) { py_print($Move, $Patrol, $Attack); }
        // EncodePropState
        if (EncodePropState(Enable) == $Enable) { py_print($Enable, $Disable, $Toggle); }
        // EncodeScore
        if (EncodeScore(Total) == $Total) { py_print($Total, $Units, $Buildings, $UnitsAndBuildings, $Razings, $KillsAndRazings, $Custom); } // 没有 $Kills，EncodeScore(Kills) == 4
        // EncodeCount
        if (EncodeCount(All) == 0) { py_print(EncodeCount(All)); } // EncodeCount(All) 为 0，EncodeCount 任何正整数都等于那个正整数本身

        // 还有一些不常用的我就没写文档和例子，用法一致，参照常量对照表使用
        // EncodeAIScript
        // EncodeFlingy
        // EncodeIcon
        // EncodeImage
        // EncodeIscript
        // EncodePortrait
        // EncodeProperty
        // EncodeSprite
        // EncodeUnitOrder
        // EncodeUpgrade
        ```

    </br>

    - #### list

        - `list`(*args) : py_list[]  
            编译期 Python 层平坦列表容器生成函数，至少需要一个参数，这个函数会自动拉平列表，不能生成多维列表  
            如果要生成一个空的 Python 列表，可以用 py_list  
            编译期 list 数组下标（索引）只能是常量  

        示例

        ```JavaScript
        var a, b, c, d;
        const list1 = list(a, b, c, d); // list 是编译期容器，它这里只是把 a/b/c/d 的引用装起来，而非 a/b/c/d 的值
        const list2 = list(15, 4, list(99, 47)); // 列表会被拉平，不会生成多维列表，这个等同 list(15, 4, 99, 47)
        foreach(i, v : py_enumerate(list2)) {
            list1[i] = v;
        }
        println("{}, {}, {}, {}", a, b, c, d); // 15, 4, 99, 47
        ```

    </br>

    - #### EUDCreateVariables

        - `EUDCreateVariables`(count) : py_list[EUDVariable]  
            编译期创建 [count] 个变量，返回一个编译期 list，list 内包含被创建的变量的引用  
            编译期 list 数组下标（索引）只能是常量  

        示例

        ```JavaScript
        const vs = EUDCreateVariables(3);
        vs[0] = 1;
        vs[1] = 2;
        vs[2] = 3;
        // vs[0]、vs[1]、vs[2] 是三个变量，vs 在运行时不存在，所以 vs 的下标都必须是编译期常量
        ```

    </br>

    - #### **SetVariables**

        - `SetVariables`(变量列表, 目标列表, 操作符列表)  
            使用最少一个触发器将 [变量列表] 中所有的变量按照 [操作符列表] 中对应的操作为 [目标列表] 值，该宏用于优化  
            传入目标值是变量也不会在动作完成之前动态生效，仅仅会保持目标值为它开始执行时的状态  

        示例

        ```JavaScript
        var a, b, c = 10, 10, 10;
        SetVariables(
        list(  a,    b,      c   ),
        list(  3,    2,      4   ),
        list(SetTo, Add, Subtract),
        );
        // 以上代码相当于
        const op1 = list(a,    SetTo, 3),
                    list(b,      Add, 2),
                    list(c, Subtract, 4);
        SeqCompute(op1);
        ```

    </br>

    - #### SCMD2Text

        - `SCMD2Text`(text) : py_str  
            编译期将文本 [text] 中的尖括号 16 进制数字值 <XX> 转换成对应的 ASCII 字符

        示例

        ```JavaScript
        // 以下两行效果等价
        simpleprint(SCMD2Text("<03>哈哈<02>")); // 打印出黄色的 哈哈
        simpleprint("\x03哈哈\x02"); // 打印出黄色的 哈哈
        ```

    </br>

    - #### **unProxy**

        - `unProxy`(x) : duck  
            编译期获取引用类型 x 的的指针值

        示例

        ```JavaScript
        const a = EUDArray(list(9, 8, 7));
        var b = unProxy(a);
        const c = EUDArray.cast(b + 4); // c 实际是 a[1] 的引用
        c[0] = 888888;
        println("b:{} a:{} c:{} a[1]:{} c[0]:{}", b, a, c, a[1], c[1]); // b:421156492 a:421156492 c:421156496 a[1]:888888 c[1]:7
        ```

    </br>

    - #### **UnitProperty**

        - `UnitProperty`(...) : CUWP  
            编译期在地图的中插入一个创建单位属性（Create Unit with Properties）并返回它，可以使用 GetPropertyIndex 获取它的编号，字段解释看示例

        示例

        ```JavaScript
        // 所有字段都可选
        const prop = UnitProperty(
        hitpoint = 100,       // 生命百分比 0~100
        shield = 100,         // 护盾百分比 0~100
        energy = 100,         // 能量百分比 0~100
        hanger = 0,           // 0~4294967295
        resource = 0,         // 0~65536 (Count)
        cloaked = False,      // 是否隐形 True(启用)/False(禁用)
        burrowed = False,     // 是否已钻地 True(启用)/False(禁用)
        intransit = False,    // 是否正在被运输 True(启用)/False(禁用)
        hallucinated = False, // 是否是个幻影 True(启用)/False(禁用)
        invincible = False    // 是否无敌 True(启用)/False(禁用)
        );
        CreateUnitWithProperties(1, "Terran Marine", $L("Location 3"), P1, prop);
        ```

    </br>

    - #### GetPropertyIndex

        - `GetPropertyIndex`(创建单位属性 : CUWP) : py_int  
            编译期获取一个创建单位属性在地图 CUWP 列表中的编号

        示例

        ```JavaScript
        // 所有字段都可选
        const prop = UnitProperty(
        hitpoint = 100,       // 生命百分比 0~100
        );
        py_print(GetPropertyIndex(prop));
        ```

    </br>

    - #### GetPlayerInfo

        - `GetPlayerInfo`(player: TrgPlayer) : py_struct  
            编译期获取地图信息中玩家 [player] 的信息，[player] 仅支持常量，获取的信息是地图设定的信息，并非运行时信息

        示例

        ```JavaScript
        const pinfo = GetPlayerInfo(0);
        setcurpl(0);
        printAt(9, "玩家 1 类型：{}({}) 种族：{}({}) 所属势力：{}", pinfo.typestr, pinfo.type, pinfo.racestr, pinfo.race, pinfo.force);
        ```

    </br>

    - #### EUDRegisterObjectToNamespace

        - `EUDRegisterObjectToNamespace`(funcname, obj) : duck  
            注册一个对象到全局的名字空间，主要用于模块间反射传递参数

        示例

        ```JavaScript
        const menuSel = PVariable();
        EUDRegisterObjectToNamespace("menuSel", menuSel);
        ```

    </br>

    - #### GetEUDNamespace

        - `GetEUDNamespace()` : py_dict[str, duck]  
            获取全局名字空间字典表，它记录着 EUDRegisterObjectToNamespace 里面注册的对象

        示例

        ```JavaScript
        function afterTriggerExec() {
            setcurpl(P1);
            const menuSel2 = GetEUDNamespace().get("menuSel");
            println(9, "{}", menuSel2[0]);
        }
        ```

    </br>

    - #### MPQAddFile

        - `MPQAddFile`(fname, contents, isWave = false)  
            向 output 设定的输出地图中添加一个字节组内容为 [contents] 的文件 [fname]，如果 [isWave] 标志设置为 true，则会在导入前使用 Wave 有损压缩的方式压缩它

        示例

        ```JavaScript
        MPQAddFile("1.txt", py_open("C:/1.txt", "rb").read());
        ```

    </br>

    - #### MPQAddWave

        - `MPQAddWave`(fname, content)  
            它相当于是 MPQAddFile(fname, contents, true)

        示例

        ```JavaScript
        MPQAddWave("1.wav", py_open("C:/1.wav", "rb").read());
        ```


    </br>
    </br>


- ### 编译期 Python 宏

    

    在 epScript 中可以使用 py_ 前缀调用所有的 Python 3 内建函数，这里列举一些常用的

    </br>

    - #### py_print

        - `py_print`(*args)  

            编译期使用 Python 的 print 函数打印输出内容到编译日志界面，用于调试输出

        示例

        ```JavaScript
        py_print("这个仅仅会输出到编译时的黑框中，跟地图一毛钱关系也没有");
        ```

        </br>

    - #### py_list

        - `py_list`(iter) : py_list  
            编译期 Python 列表创建

        示例

        ```JavaScript
        const lst = py_list();
        lst.append(DisplayText("11111"));
        lst.append(DisplayText("22222"));
        lst.append(DisplayText("33333"));
        DoActions(lst);
        ```

    </br>

    - #### py_open

        - `py_open`(filename, mode) : py_file  
            编译期以 [mode] 模式打开文件 [filename]，返回一个 Python 文件对象

        示例

        ```JavaScript
        function onPluginStart() {
            MPQAddWave("1.wav", py_open("C:/1.wav", "rb").read()); // 导入一个外部文件到地图中
        }
        ```

    </br>

    - #### py_eval

        - `py_eval`(str) : duck  
            编译期简单的 Python 代码执行，返回结果

        示例

        ```JavaScript
        const locs = py_eval('[EncodeLocation(("Location {}").format(x)) for x in range(1, 4)]'); // 从 Python 中返回 list($L("Location 1"), $L("Location 2"), $L("Location 3"))
        const actions = py_eval('[]'); // 从 Python 中返回一个空 list
        foreach(loc : py_enumerate(locs)) {
            actions.append(CreateUnit(1, "Terran Marine", loc, P1)); // 往列表中添加一个动作
        }
        DoActions(actions); // 一次性执行列表所有的动作
        ```

    </br>

    - #### py_str

        - `py_str`(val) : py_str  
            编译期字符串包装转换

        示例

        ```JavaScript
        const actions = py_list();
        foreach(i : py_range(0, 3)) {
            actions.append(CreateUnit(1, "Terran Marine", py_str("Location ") + py_str(i + 1), P1)); // 往列表中添加一个动作
        }
        DoActions(actions); // 一次性执行列表所有的动作
        ```

    </br>

    - #### py_len

        - `py_len`(gconstant) : py_int  
            编译期 Python 层全局常量长度判断

        示例

        ```JavaScript
        const lst = py_list();
        lst.append(DisplayText("11111"));
        lst.append(DisplayText("22222"));
        lst.append(DisplayText("33333"));
        foreach(i : py_range(0, py_len(lst))) {
            DoActions(lst[i]);
        }
        ```

    </br>

    - #### py_enumerate

        - `py_enumerate`(vlist) : py_iter  
            编译期枚举迭代器，枚举展开编译期容器中的项

        示例

        ```JavaScript
        var a, b, c, d;
        const list1 = list(a, b, c, d); // list 是编译期容器，它这里只是把 a/b/c/d 的引用装起来，而非 a/b/c/d 的值
        const list2 = list(15, 4, 99, 47);
        foreach(i, v : py_enumerate(list2)) {
            list1[i] = v;
        }
        println("{}, {}, {}, {}", a, b, c, d); // 15, 4, 99, 47
        ```

    </br>

    - #### py_range

        - `py_range`(start, end, step) : py_iter  
            编译期计数迭代器，从 [start] 步进为 [step] 迭代到 [end] 结束展开代码块，包含 [start] 不包含 [end]

        示例

        ```JavaScript
        // https://euddb.website/?pg=entry&id=542
        // https://euddb.website/?pg=entry&id=543
        // 读取内存值的原理，使用 32 个触发器判断 32 位中的每一位的值，如果第 i 位的值大于 0 则给变量附加 2 的 i-1 次方的值
        function GetMouseXY() {
            var x, y;
            RawTrigger(actions = list(
                SetDeaths(EPD(x.getValueAddr()), SetTo, 0, 0),
                SetDeaths(EPD(y.getValueAddr()), SetTo, 0, 0),
            ));
            foreach(i : py_range(32)) { /* 这里用 64 个触发器来读取鼠标的 X Y 值 */
                RawTrigger(
                    conditions = DeathsX(EPD(0x6CDDC4), AtLeast, 1, 0, py_pow(2,i)),
                    actions = SetDeaths(EPD(x.getValueAddr()), Add, py_pow(2,i), 0),
                );
                RawTrigger(
                    conditions = DeathsX(EPD(0x6CDDC8), AtLeast, 1, 0, py_pow(2,i)),
                    actions = SetDeaths(EPD(y.getValueAddr()), Add, py_pow(2,i), 0),
                );
            }
            return x, y;
        }

        // 上面的 GetMouseXY 函数实际实现等价于下面这个
        function GetMouseXY() {
            return dwread(0x6CDDC4), dwread(0x6CDDC8);
        }
        ```


    </br>
    </br>

- ### 编译期字节转换

    </br>

    - #### b2i

        - `b2i1`(content, index) : py_int  
        - `b2i1`(content, index) : py_int  
        - `b2i4`(content, index) : py_int  
            将字面量字节组 [content] 在 [index] 位置的 byte、word、dword 使用小端序（Little Endian）方式转换转换成正整数常量

        示例

        ```JavaScript
        printAt(2, "0x{:x},0x{:x},0x{:x},0x{:x}", b2i1(b"fuck"), b2i1(b"fuck",1), b2i1(b"fuck",2), b2i1(b"fuck",3)); // 0x66, 0x75, 0x63, 0x6B
        printAt(3, "0x{:x},0x{:x},0x{:x}", b2i2(b"fuck"), b2i2(b"fuck",1), b2i2(b"fuck",2)); // 0x7566, 0x6375, 0x6B63
        printAt(4, "0x{:x}", b2i4(b"fuck")); // 0x6B637566
        ```

    </br>

    - #### i2b

        - `i2b1`(i) : py_byte  
        - `i2b2`(i) : [py_byte]  
        - `i2b4`(i) : [py_byte]  
        将整数常量 [i] 使用小端序（Little Endian）方式转换成一个字节两个字节或者四个字节常量

        示例

        ```JavaScript
        printAt(4, "{}", i2b4(0x6B637566));
        ```

    </br>

    - #### u2b/b2u

        - `u2b`(s) : [py_byte]  
        - `b2u`(b) : py_str  
            字节字面量与字符串字面量转换

        示例

        ```JavaScript
        printAt(5, "{}", u2b("fuck")); // b'fuck'
        printAt(6, "{}", b2u(b"fuck")); // fuck
        ```

    </br>

    - #### utf8 编码/解码

        - `b2utf8`(str) : [py_byte]  
            使用 UTF-8 解码 [str]

        - `u2utf8`(str) : py_str  
            使用 UTF-8 编码 [str]

        示例

        ```JavaScript
        printAt(5, "{}", b2utf8("fuck")); // b'fuck'
        printAt(6, "{}", u2utf8(b"fuck")); // fuck
        ```

    </br>
    </br>
    

- ### 常规函数

    </br>

    - #### **EPD**

        - `EPD`(ptr) : py_int | EUDVariable  
        将指定 [ptr] 指针转换成 EPD 偏移值，内存中玩家编号占用 4 字节（32 位整数），它实际就是将 [ptr] 减去 0x58A364 得到的数再除以 4  
        参数是常量的情况下可以在编译期返回结果

        示例

        ```JavaScript
        // https://euddb.website/?pg=entry&id=240
        const epd = EPD(0x5124D8); // (0x5124D8-0x58A364)/4 = -0x1DFA3 = -122787
        ```

    </br>

    - #### l2v

        - `l2v`(条件表达式) : EUDVariable  
            使用一个触发器在运行时获取 [条件表达式] 的结果逻辑值 false 或者 true

        示例

        ```JavaScript
        var isP1MarineDeaths100 = l2v(Deaths(P1, AtLeast, 100, "Terran Marine"));
        ```

    </br>

    - #### EUDFuncPtr

        - `EUDFuncPtr`(argn, retn) : py_int  
            声明一个 [argn] 个参数和 [retn] 个返回值的闭包函数的指针

        - `EUDTypedFuncPtr`(argtypes, rettypes) : py_int  
            声明一个参数类型列表为 [argtypes] 返回值类型列表为 [rettypes] 的闭包函数的指针

        示例

        ```JavaScript
        function GetMouseMapXY() {
            const screenX, screenY = dwread_epd(EPD(0x62848C)), dwread_epd(EPD(0x6284A8));
            const mouseX, mouseY = dwread_epd(EPD(0x6CDDC4)), dwread_epd(EPD(0x6CDDC8));
            const x, y = screenX + mouseX, screenY + mouseY;
            return x, y;
        }

        var funcptr = 0;

        function beforeTriggerExec() {
            static var x, y = 0, 0;
            once (funcptr == 0) {
                funcptr = EUDFuncPtr(0, 2)(function() {
                    x, y = GetMouseMapXY();
                    return x, y;
                });
            }

            setcurpl(P1);
            printAt(9, "before x = {}, y = {}", x, y);
        }

        function afterTriggerExec() {
            const x, y = EUDFuncPtr(0, 2).cast(funcptr)();
            setcurpl(P1);
            printAt(10, "after funcptr returns {}, {}", x, y);
        }
        ```

    </br>
    </br>

- ### 触发器构建函数

    触发器构建函数可以使用自定义构建各种属性的触发器

    </br>

    - #### RawTrigger

        - `RawTrigger`(conditions = list(...), actions = list(...), preserved = true/false) : RawTrigger  
            插入一个静态的传统触发器，不能传入变量，返回一个触发器指针  
            conditions 字段传入条件表达式常量列表，最多支持 16 个传统触发器条件  
            actions 字段传入动作表达式常量列表，最多支持 64 个传统触发器动作  
            preserved 字段默认是 true，表示每次调用都会执行，如果设为 false，那么它将会在条件达成后执行一次，往后再也不会执行  

        示例
        ```JavaScript
        // https://euddb.website/?pg=entry&id=542
        // https://euddb.website/?pg=entry&id=543
        // 读取内存值的原理，使用 32 个触发器判断 32 位中的每一位的值，如果第 i 位的值大于 0 则给变量附加 2 的 i-1 次方的值
        function GetMouseXY() {
            var x, y;
            RawTrigger(actions = list(
                SetDeaths(EPD(x.getValueAddr()), SetTo, 0, 0),
                SetDeaths(EPD(y.getValueAddr()), SetTo, 0, 0),
            ));
            foreach(i : py_range(32)) { /* 这里用 64 个触发器来读取鼠标的 X Y 值 */
                RawTrigger(
                    conditions = DeathsX(EPD(0x6CDDC4), AtLeast, 1, 0, py_pow(2,i)),
                    actions = SetDeaths(EPD(x.getValueAddr()), Add, py_pow(2,i), 0),
                );
                RawTrigger(
                    conditions = DeathsX(EPD(0x6CDDC8), AtLeast, 1, 0, py_pow(2,i)),
                    actions = SetDeaths(EPD(y.getValueAddr()), Add, py_pow(2,i), 0),
                );
            }
            return x, y;
        }

        // 上面的 GetMouseXY 函数实际实现等价于下面这个
        function GetMouseXY() {
            return dwread(0x6CDDC4), dwread(0x6CDDC8);
        }
        ```

    </br>

    - #### Trigger

        - `Trigger`(conditions = list(...), actions = list(...), preserved = true/false)  
            插入一个扩展触发器，扩展触发器可能会被拆分成很多个传统触发器，它没有 16 个条件和 64 个动作的限制，并且可以传入变量，不返回值  
            传入变量不会在动作完成之前动态生效，仅仅会保持变量在触发器开始执行时的状态  
            为了代码明确，通常建议不使用 Trigger 而使用 if 条件来完成动态的条件判断  
            conditions 字段传入条件表达式列表  
            actions 字段传入动作表达式列表  
            preserved 字段默认是 true，表示每次调用都会执行，如果设为 false，那么它将会在条件达成后执行一次，往后再也不会执行

        示例

        ```JavaScript
        Trigger(
            conditions = list(
                Bring(P1, AtMost, 0, "Zerg Cerebrate", "Location 1")
            ),
            actions = list(
                KillUnitAt(All, "Terran Medic", "Location 1", P1)
            ),
            preserved = false,
        );
        ```

    </br>

    - #### PTrigger

        - `PTrigger`(players, conditions = list(...), actions = list(...))  
            插入一个匹配当前玩家的触发器，`当前玩家`为 [players] 中任何一个玩家时，它就会执行，没有 preserved 字段，其它字段用法和 Trigger 一样。

        示例

        ```JavaScript
        setcurpl(P8);
        PTrigger(list(P3, P8),
            conditions = ElapsedTime(AtLeast, 3),
            actions = KillUnit($U("Terran Medic"), P1),
        );
        ```

    </br>

    - #### **DoActions**

        - `DoActions`(actions, preserved = true/false)  
            一个无条件执行动作的 Trigger 函数  
            在 epScript 中，两个分号之间非注释代码仅为函数调用会自动使用这个函数包裹成一个触发器  
            actions 为动作表达式列表  
            preserved 字段默认是 true，表示每次调用都会执行，如果设为 false，那么它将会在条件达成后执行一次，往后再也不会执行  

        示例

        ```JavaScript
        Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"); // 如果一行代码仅是一个触发器动作函数调用，那么它在编译后会在外面套上一个 DoActions

        DoActions(Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"));

        DoActions(list( // 明确一点儿可以加上 list
            Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"),
        ));

        Trigger(actions = list(
            Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"),
        ));

        // 上面四个用法完全等价

        const a = Order("Zerg Hydralisk", P8, $L("UpArea"), Patrol, "Player1Home"); // 这一行代码与上面用法不等价，它用于声明一个名为 a 的动作表达式常量，它不会被自动用 DoActions 包裹

        DoActions(
            Order("Zerg Guardian", P8, $L("UpArea"), Patrol, "Player1Home"),
            Order("Zerg Guardian", P8, $L("MiddleArea"), Patrol, "Player1Home"),
            Order("Zerg Devourer", P8, $L("UpArea"), Patrol, "Player1Home"),
            Order("Zerg Devourer", P8, $L("MiddleArea"), Patrol, "Player1Home"),
            preserved = false, // preserved 参数必须是命名参数
        );

        // 静态特性演示
        // 传入 DoActions 序列的动作的变量会在 DoActions 开始执行的时候被替换成一个静态常量值传入，不管 DoActions 的运行过程中修改多少次变量，传入的值是在运行之前就已经确定好了的
        var c = 0;
        c.AddNumber(1);
        DoActions(
            c.AddNumber(100),
            CreateUnit(c, "Terran SCV", "Location 2", P1),
        );
        printAt(10, "你觉得应该创建了 {} 个 SCV，但实际只有 1 个", c); // 你觉得这里应该创建了 100 个 SCV，但实际只有 1 个
        ```

    </br>

    - #### VProc

        - `VProc`(vars, actions) : RawTrigger | [RawTrigger]  
            变量处理过程 [actions] 只能传入常量动作表达式列表，VProc 会在顺序执行完 [actions] 所有动作后，再顺序将 [vars] 中所有变量的虚拟触发器都执行一遍  
            这意味着可以先在 [actions] 对部分变量的虚拟触发器进行修改，动作执行完成后再由 VProc 执行这些被修改过的变量的虚拟触发器  
            它通常用于优化对变量进行串行赋值或位运算的开销，它的开销略高于 RawTrigger 但小于 DoActions 或 Trigger  
            一个变量的虚拟触发器中只能有一次 SetDeathsX 动作，若设定多个动作到变量队列中取最后一个  

        示例

        ```JavaScript
        var 无关变量 = 0;
        var c, d = 0, 0;

        c = 1;
        RawTrigger(actions = list(
            c.AddNumber(100),
            c.QueueAssignTo(d),
        ));
        println("RawTrigger 过程后 c:{} d:{}", c, d); // c:101 d:0

        c = 1;
        VProc(c, list(
            c.AddNumber(100),
            c.QueueAssignTo(d),
        ));
        println("VProc(c) 过程后 c:{} d:{}", c, d); // c:101 d:101

        c = 1;
        VProc(list(c, d), list(
            c.AddNumber(100),
            c.QueueAssignTo(d),
            d.QueueAddTo(c),
        ));
        println("VProc(c,d) 过程后 c:{} d:{}", c, d); // c:202 d:101

        c = 1;
        VProc(list(c, d), list(
            c.AddNumber(100),
            c.QueueAssignTo(d),
            d.QueueAddTo(c),
        ));
        println("VProc(d,c) 过程后 c:{} d:{}", c, d); // c:202 d:202
        ```


    </br>
    </br>

- ### 运行时迭代器

    运行时迭代器可以使用编译期循环 foreach 语法构建一个运行时循环代码块，运行期会执行的次数由迭代器内部的流程控制逻辑控制  
    运行时迭代器所属的 foreach 代码块中可以使用 continue 或者 break，它们会编译为 EUDContinue() 和 EUDBreak()  
    咱们使用等价代码的方式来解释一下运行时迭代器的原理，例如有如下代码  
    ```JavaScript
    foreach (cu : EUDLoopNewCUnit()) {
        py_print(cu);
        println("{}", cu);
        continue;
    }
    ```
    它大约等价于

    ```JavaScript
    {
        const it = EUDLoopNewCUnit();
        const cu = py_next(it); // 首次编译期迭代获取迭代结果存储的位置并设定 EUD 运行时循环代码块开始，相当于一个 EUDWhile()(运行时it还有下一个项)
            py_print(cu);       // 编译期输出，只会执行一次
            println("{}", cu);
            EUDContinue();
        py_next(it, 0);         // 第二次编译期迭代设置 EUD 运行循环代码块结束位置，相当于一个 EUDEndWhile()
    }
    ```

    </br>

    - #### EUDLoopPlayer

        - `EUDLoopPlayer`(ptype, force, race) : EUDIterator  
            迭代类型为 [ptype] 势力为 [force] 种族为 [race] 的活跃玩家，内部使用 playerexist 检测  
            活跃玩家不包括空缺或离开游戏的玩家

        示例

        ```JavaScript
        // ptype 是可选参数，可以是 "Human" 或 "Computer"
        // force 是可选参数，可以是 Force1 Force2 Force3 Force4
        // race  是可选参数，可以是 "Zerg" "Terran" "Protoss"
        foreach (p: EUDLoopPlayer("Human", Force1, "Zerg")) {
            setcurpl(p);
            println("你是虫族");
        }
        ```

    </br>

    - #### EUDLoopRange

        - `EUDLoopRange`(start, end=None) : EUDIterator  
            迭代从 [start] 到 [end] - 1 的值

        示例

        ```C#
        // 打印 1 到 4 屏幕
        foreach (i : EUDLoopRange(1, 5)) {
            simpleprint(i);
        }
        // 以上代码大约等价于
        for (var i = 1; i < 5; i++) {
            simpleprint(i);
        }
        ```

    </br>

    - #### EUDLoopUnit

        - `EUDLoopUnit()` : EUDIterator  
            迭代单位主链上的所有单位的 ptr, epd  
            不包含子单位、Scanner Sweep（雷达特效单位）、Map Revealer（地图雷达）等  

        示例

        ```JavaScript
        foreach (ptr, epd : EUDLoopUnit()) {
            const u = CUnit(epd);
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }
        ```

        </br>

        - `EUDLoopUnit2()` : EUDIterator  
            迭代所有单位的 ptr, epd  
            迭代的单位会包含子单位、Scanner Sweep（雷达特效单位）、Map Revealer（地图雷达）等  

        示例

        ```JavaScript
        // Unit Node Table 是一个双向链表 (doubly linked list)，有主链和支链
        // FirstUnitPointer  ->  Unit1 <-> Unit2 <-> "Terran Siege Tank" <-> Unit4 -> Null
        //                                                   |
        //                                             "Tank Turret"
        // 主链和支链都会占用内存空间，比如以上的例子中，Bring 条件会判定总共有 4 个单位，但是实际上 1700 个单位空间有 5 个都被占用了。
        // EUDLoopUnit() 只会 loop 主链上的单位，因此会无视掉 "Tank Turret" -- 坦克头上的炮台
        // 而 EUDLoopUnit2() 会 loop 所有占用 Unit Node Table 空间的单位，因为它不是按照链表的顺序 loop 的，而是按照内存顺序 loop 的。
        // EUDLoopUnit(): 顺着主链 loop unit，无法 loop 到 sub unit 和 map revealer 等
        // EUDLoopUnit2(): 顺着 Memory index 来 loop unit，可以 loop 到全部 unit
        foreach (ptr, epd : EUDLoopUnit2()) {
            const u = CUnit(epd);
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }
        ```

        </br>

        - `EUDLoopCUnit()` : EUDIterator  
            它使用 EUDLoopUnit2 遍历并将遍历出来的指针包装成 CUnit 对象  
            迭代的单位会包含子单位、Scanner Sweep（雷达特效单位）、Map Revealer（地图雷达）等

        示例

        ```JavaScript
        foreach (u : EUDLoopCUnit()) {
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }
        ``` 

        </br>

        - `EUDLoopNewUnit`(allowance = 2) : EUDIterator  
            迭代最多 [allowance] 个从`非本帧最后一次`调用 EUDLoopNewUnit 或者 EUDLoopNewCUnit 以来新出现的单位的 ptr, epd  
            不包含子单位、Scanner Sweep（雷达特效单位）、Map Revealer（地图雷达）等

        - `EUDLoopNewCUnit`(allowance = 2) : EUDIterator  
            迭代最多 [allowance] 个从`非本帧最后一次`调用 EUDLoopNewUnit 或者 EUDLoopNewCUnit 以来新出现的单位并将遍历出来的指针包装成 CUnit 对象  
            不包含子单位、Scanner Sweep（雷达特效单位）、Map Revealer（地图雷达）等

        > **Warning**  
        > 同一帧多次调用 EUDLoopNewUnit 或者 EUDLoopNewCUnit 将会遍历到相同的单位  
        > 虫族的特殊性：新单位只有幼虫、取消建造气矿的农民、双生单位的第二个单位（狗、自爆蚊、第二个传送门），以及任何使用 CreateUnit 函数凭空创造的单位  

        > **Note**  
        > 虫卵孵化的过程会使用幼虫的地址，孵化完成后的单位依然使用这个地址，如果是孵化小狗其中一只小狗继续使用幼虫的地址，另外一只是新单位，自爆蚊类似  
        > 虫族农民孵化出来的建筑会使用农民的地址，取消孵化会变回农民，地址不变，不产生新单位  
        > 农民孵化成气矿的一瞬间会继承气矿单位（气矿本身就是一个单位）的地址，农民算作死亡，如果取消建造，返回的是一个新的农民，这个农民可以被遍历到  
        > 在出兵建筑里排队造的兵种：建造完毕走出来的时候才被检测到  
        > 建筑：刚开始建造就可以检测出来（未完成建造）  
        > 红白球：电兵合体之后白球将使用其中一个电兵的地址，另外一个电兵算作死亡，不会产生新单位，如果取消合体，则其中一个电兵继承白球地址，另外一个算新单位，红球类似

        示例

        ```JavaScript
        foreach (ptr, epd : EUDLoopNewUnit(1700)) {
            const u = CUnit(epd);
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }

        foreach (u : EUDLoopNewCUnit(1700)) {
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }

        // 下面代码是完整的新单位遍历，代码来自：GGRush
        const NewUnits = UnitGroup(1000);
        const ChangeableUnits = EUDDeque(1000)();

        function onPluginStart() {
            GetGlobalStringBuffer();
        }

        function beforeTriggerExec() {
            foreach(cunit: EUDLoopNewCUnit()) {
                NewUnits.add(cunit);
            }

            ChangeableUnits.append(-1);
            var uid, value;
            while(True) {
                value = ChangeableUnits.popleft();
                if(value == -1) break;
                else if(value < 228) uid = value;
                else if(value >= EPD(0x59CCA8)) {
                    const epd = value;
                    if(!MemoryXEPD(epd + 0x64/4, Exactly, prev, 0xFFFF)) NewUnits.add(value); 
                    else {
                        ChangeableUnits.append(uid);
                        ChangeableUnits.append(epd);
                    }
                }
            }

            foreach(unit: NewUnits.cploop) {
                foreach(dead: unit.dying) {}	//存在检测：存活的继续执行代码，死亡的continue
                unit.move_cp(0x64/4);
                if(DeathsX(CurrentPlayer, Exactly, $U("Zerg Larva"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Egg"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Drone"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Hydralisk"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Lurker Egg"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Mutalisk"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Mutalisk Cocoon"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Hatchery"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Lair"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Creep Colony"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Zerg Spire"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Protoss High Templar"), 0, 0xFFFF)
                || DeathsX(CurrentPlayer, Exactly, $U("Protoss Dark Templar (Unit)"), 0, 0xFFFF)
                ) {
                    const uid = wread_cp(0, 0);
                    ChangeableUnits.append(uid);
                    ChangeableUnits.append(unit.epd);
                }
                // start
                // 你的代码
                // end
                unit.remove();	//This code is necessary!!!Don't skip it
            }
        }
        ```

        </br>

        - `EUDLoopPlayerUnit`(player: TrgPlayer) : EUDIterator  
            迭代玩家 [player] 所有的单位的 ptr, epd

        - `EUDLoopPlayerCUnit`(player: TrgPlayer) : EUDIterator  
            迭代玩家 [player] 所有的单位并将遍历出来的指针包装成 CUnit 对象

        示例

        ```JavaScript
        foreach (ptr, epd : EUDLoopPlayerUnit(P1)) {
            const u = CUnit(epd);
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }

        foreach (u : EUDLoopPlayerCUnit(P1)) {
            if (u.unitID == $U("Terran Marine")) {
                u.hp = 0x100 * 10;
            }
        }

        // 将 玩家 Owner 的机枪兵全改成 玩家 NewOwner 的
        // 使用 cunit.give 会影响 EUDLoopPlayerCUnit 迭代，所以要先将所有 cunit 加入到一个队列容器中
        // 然后再从容器中对每一个 cunit 进行更改所有者
        const givequeue = EUDQueue(100);
        foreach(cunit: EUDLoopPlayerCUnit(Owner)) {
            if(cunit.unitType != $U("Terran Marine")) continue;
            givequeue.append(cunit);
        }
        while (!givequeue.empty()) {
            const cunit = CUnit(givequeue.pop());
            cunit.cgive(NewOwner);
        }
        ```

    </br>
    </br>

- ### 屏幕输出文字函数

    </br>

    - #### DisplayTextAt

        - `DisplayTextAt`(行, 文本 : TrgString)  
            在`本机玩家 == 当前玩家`屏幕上第 [行] 行显示 [文本]  
            它和 DisplayText 不同，不返回触发器动作表达式

        - `DisplayTextAllAt`(行, 文本 : TrgString)  
            为所有的玩家（包括观察者）在第 [行] 行显示 [文本]  
            它和 DisplayTextAll 不同，不返回触发器动作表达式

        示例

        ```JavaScript
        var text_10 = $T("_10");
        var text_09 = $T("_09");
        var line = 10;
        DisplayTextAllAt(line, text_10);
        line -= 1;
        DisplayTextAllAt(line, text_09);
        line -= 1;
        setcurpl(P1);
        DisplayTextAt(line, "只显示给 P1");
        ```

    </br>

    - #### print

        - `simpleprint`(*args, spaced=true)  
            将多个参数 [*args] 按顺序打印到`本机玩家 == 当前玩家`屏幕滚动信息的下一行，命名参数 spaced 表示是否将打印出来的每个参数用空格隔开，默认 true

        - `println`(format_string : py_str, *args)  
            以格式 [format_string] 格式化打印多个参数 [*args] 到`本机玩家 == 当前玩家`屏幕滚动信息的下一行

        - `printAt`(line, format_string : py_str, *args)  
            以格式 [format_string] 格式化打印多个参数 [*args] 到`本机玩家 == 当前玩家`屏幕滚动信息的从上往下的第 [line] 行（取值范围 0~10）

        - `printAll`(format_string : py_str, *args)  
            以格式 [format_string] 格式化打印多个参数 [*args] 到所有玩家屏幕滚动信息的下一行

        - `printAllAt`(line, format_string : py_str, *args)  
            以格式 [format_string] 格式化打印多个参数 [*args] 到所有玩家屏幕滚动信息的从上往下的第 [line] 行（取值范围 0~10）

        <details><summary>格式化占位符</summary>

        - `{}`：通用占位符，用于输出一个变量的值或常量的指针
        - `{{}}`：输出`{}`本身
        - `{:c}`：将位置上的值作为玩家编号并输出为对应的玩家颜色代码，就像 PColor(位置上的值) 那样
        - `{:n}`：将位置上的值作为玩家编号并输出为对应的玩家名字，就像 PName(位置上的值) 那样
        - `{:s}`：将位置上的值作为字符串指针输出为其指向的字符串，就像 ptr2s(位置上的值) 那样
        - `{:t}`：将位置上的值作为字符串 EPD 指针输出为其指向的字符串，就像 epd2s(位置上的值) 那样
        - `{:x}`：将位置上的数值输出为左补 0 的 8 位 16 进制数字，就像 hptr(将位置上的数值) 那样
        </details>

        示例

        ```JavaScript
        // simpleprint(*args, spaced=true)
        simpleprint("你好", "星际争霸");                 // 在当前玩家屏幕滚动信息下一行输出 “你好 星际争霸”
        simpleprint("你好", "星际争霸", spaced = false); // 在当前玩家屏幕滚动信息下一行输出 “你好星际争霸”

        // println(format_string, *args)
        println("{} {}", "你好", "星际争霸");            // 在当前玩家屏幕滚动信息下一行输出 “你好 星际争霸”

        // printAt(line, format_string, *args)
        printAt( 0, "{} {}", "你好", "星际争霸");        // 在当前玩家屏幕滚动信息最上面一行输出 “你好 星际争霸”
        printAt(10, "{} {}", "你好", "星际争霸");        // 在当前玩家屏幕滚动信息最下面一行输出 “你好 星际争霸”

        // printAll(format_string, *args)
        printAll("{} {}", "你们好", "星际争霸");         // 在所有玩家的屏幕滚动信息下一行输出 “你们好 星际争霸”

        // printAllAt(line, format_string, *args)
        printAllAt( 0, "{} {}", "你们好", "星际争霸");   // 在所有玩家的屏幕最上面一行输出 “你们好 星际争霸”
        printAllAt(10, "{} {}", "你们好", "星际争霸");   // 在所有玩家的屏幕最下面一行输出 “你们好 星际争霸”
        ```

    </br>

    - #### GetGlobalStringBuffer

        - `GetGlobalStringBuffer()` : StringBuffer  
            获取`本机玩家 == 当前玩家` print 系列函数中内部使用的 StringBuffer，它的容量为 1023 字节

        示例

        ```JavaScript
        // 以下两行代码等价
        printAt( 0, "{} {}", "你好", "星际争霸");
        GetGlobalStringBuffer().printfAt(0, "{} {}", "你好", "星际争霸");
        ```

    </br>

    - #### eprint

        - `eprintln`(*args)  
            将多个参数 [*args] 按顺序打印到`当前玩家`屏幕中心偏下的错误提示信息，打印超过 218 个字节的内容（包含颜色代码字符串结尾等）会发生错误

        - `eprintf`(format_string, *args)  
            以字面字符串 [format_string] 作为格式格式化打印多个参数 [*args] 到`当前玩家`屏幕中心偏下的错误提示信息，打印超过 218 个字节的内容（包含颜色代码字符串结尾等）会发生错误

        - `eprintAll`(format_string, *args)  
            以字面字符串 [format_string] 作为格式格式化打印多个参数 [*args] 到所有玩家屏幕中心偏下的错误提示信息，打印超过 218 个字节的内容（包含颜色代码字符串结尾等）会发生错误

        - `eprintln2`(*args)  
            将多个参数 [*args] 按顺序打印到`当前玩家`屏幕中心偏下的错误提示信息  
            替换 stat_txt.tbl[871]: "Unit's waypoint list is full." 这个错误提示的信息，再自行配合 QueueGameCommand_QueuedRightClick(xy) 可在错误信息提示那里输出超过 218 字节内容

        <details><summary>格式化占位符</summary>

        - `{}`：通用占位符，用于输出一个变量的值或常量的指针
        - `{{}}`：输出`{}`本身
        - `{:c}`：将位置上的值作为玩家编号并输出为对应的玩家颜色代码，就像 PColor(位置上的值) 那样
        - `{:n}`：将位置上的值作为玩家编号并输出为对应的玩家名字，就像 PName(位置上的值) 那样
        - `{:s}`：将位置上的值作为字符串指针输出为其指向的字符串，就像 ptr2s(位置上的值) 那样
        - `{:t}`：将位置上的值作为字符串 EPD 指针输出为其指向的字符串，就像 epd2s(位置上的值) 那样
        - `{:x}`：将位置上的数值输出为左补 0 的 8 位 16 进制数字，就像 hptr(将位置上的数值) 那样
        </details>

        示例

        ```JavaScript
        eprintln("你好", "星际争霸");             // 输出 “你好星际争霸” 到当前玩家屏幕中心偏下的错误提示信息
        eprintf("{}-{}", "你好", "星际争霸");     // 输出 “你好-星际争霸” 到当前玩家屏幕中心偏下的错误提示信息
        eprintAll("{}-{}", "你们好", "星际争霸"); // 输出 “你们好-星际争霸” 到所有玩家的屏幕中心偏下的错误提示信息
        ```

    </br>

    - #### TextFX

        - TextFX_FadeIn(*args, color=None, wait=1, reset=True, tag=None, encoding="UTF-8")  
            渐显特效文字

        - TextFX_FadeOut(*args, color=None, wait=1, reset=True, tag=None, encoding="UTF-8")  
            渐隐特效文字

        - `TextFX_Remove`(标签)  
            移除本机标签为 [标签] 的特效文字

        - `TextFX_SetTimer`(标签, 设为/增加/减少 : TrgModifier, 数值)  
            将本机标签为 [标签] 的特效文字的计时器 [设为/增加/减少] [数值]


    </br>
    </br>

- ### 玩家相关函数

    </br>

    - #### **getuserplayerid**

        - `getuserplayerid()` : EUDVariable  
            获取本机玩家编号，本机玩家不是`当前玩家`，每个玩家电脑上获取到的本机玩家是不同的，它返回`非同步数据`

        示例

        ```JavaScript
        setcurpl(getuserplayerid());
        println("当前玩家的编号：{}", getuserplayerid());
        ```

    </br>

    - #### **playerexist**

        - `playerexist`(player) : EUDVariable  
            判断玩家 [player] 是否仍在游戏中，电脑玩家也是玩家

        示例

        ```JavaScript
        if (playerexist(P1)) {
            // 玩家1 存在
        }
        ```

    </br>

    - #### **当前玩家**

        - `getcurpl()` : EUDVariable  
            获取 cpcache 的值，如果 cpcache 没有值或 cpcache 的值和`当前玩家`不一样，则将`当前玩家`的值缓存到 cpcache 并返回

        - `setcurpl`(cp)  
            设置`当前玩家`的值为 [cp]，并缓存到 cpcache

        - `addcurpl`(n)  
            将`当前玩家`的值自增 [n]，并缓存到 cpcache

        - `setcurpl2cpcache()`  
            将`当前玩家`的值恢复到 cpcache 缓存的值  

        可以把`当前玩家`看作是一个全局变量，在需要指定玩家编号的触发器条件或动作中指定玩家编号为 CurrentPlayer（13）会使用它，并且一些触发器动作内部会使用它，`当前玩家`甚至可能不是指一个玩家，可能存储任何数值

        <details><summary>只在 `当前玩家 == 本机玩家` 的机器上生效的动作（允许非同步使用，可单独在部分玩家机器上使用）</summary>

        - DisplayText  
        - CenterView  
        - PlayWAV  
        - MinimapPing  
        - TalkingPortrait  
        - Transmission  
        - SetMissionObjectives  
        </details>

        <details><summary>会使用 `当前玩家` 作为参数的动作（必须同步在所有玩家机器上使用，否则掉线）</summary>

        - SetAllianceStatus  
        - RunAIScript  
        - RunAIScriptAt  
        - Draw  
        - Defeat  
        - Victory  
        </details>

        示例

        ```JavaScript
        const cp = getcurpl();
            setcurpl(P1);
            println("你好 玩家 1");
        setcurpl(cp);

        // CurrentPlayer 是常量数字 13，它可以使一些与玩家相关的条件或动作去访问 当前玩家 的值
        // CurrentPlayer != getcurpl()

        // https://euddb.website/?pg=entry&id=426
        setcurpl(-122787); // 第 1 档游戏速度 PlayerID 偏移
        addcurpl(6); // 第 7 挡游戏速度
        SetDeaths(CurrentPlayer, SetTo, 21, 0); // 游戏速度 x2

        // https://euddb.website/?pg=entry&id=815
        // https://euddb.website/?pg=entry&id=920
        SetMemory(0x6509B0, SetTo, 210382); // 将 当前玩家 改为 210382 （游戏亮度等级 0~31）
        SetDeaths(CurrentPlayer, SetTo, 15, 0); // 设置亮度为 15
        setcurpl2cpcache(); // 将 当前玩家 恢复到 cpcache，防止干扰 getcurpl 等函数的使用
        ```

    </br>

    - #### PColor

        - `PColor`(player: TrgPlayer) : Db*  
            返回玩家 [player] 在游戏中的颜色代码，在格式化文本中使用 `{:c}` 占位符等效

    </br>

    - #### PName

        - `PName`(player: TrgPlayer) : Db*  
            返回玩家 [player] 在游戏中的名字，在格式化文本中使用 `{:n}` 占位符等效

        示例

        ```JavaScript
        println("玩家1: {}{}", PColor(P1), PName(P1)); // 如果 玩家1 叫 Soze，颜色是红色，那么这个输出就是会打印出红色的 Soze
        println("玩家1: {:c}{:n}", P1, P1);            // 与上面那条等效
        ```

    </br>

    - #### SetPName

        - `SetPName`(player : TrgPlayer, *可变参数)  
            设置玩家 [player] 的名字为多个文本或其它参数组合起来的字符串

        - `SetPNamef`(player: TrgPlayer, 指定格式, *可变参数)  
            设置玩家 [player] 的名字为一个用 [指定格式] 格式化的文本

        这俩函数均不能影响 PName 函数获取到的那个名字，只影响玩家打字显示的那个名字，并且只对当前帧有效，每一帧都需要重复运行

        示例

        ```JavaScript
        // 以下两个用法等效
        SetPName(cp, epd2s(title), " \x07等级: \x04", level, " ", PColor(cp), PName(cp));
        SetPNamef(cp, "{:t} \x07等级: \x04{} {:c}{:n}", title, level, cp, cp);
        ```

    </br>

    - #### **EUDPlayerLoop**

      - `EUDPlayerLoop()()`  
      - `EUDEndPlayerLoop()`  

        这俩是一对儿，它会把当前玩家依次设为每一个活动玩家（包括电脑玩家），运行完毕后当前玩家值就是 Loop 开始前的当前玩家值

        示例

        ```JavaScript
        // 给所有玩家都加 1000 水晶，也包括电脑玩家
        EUDPlayerLoop()();
            SetResources(CurrentPlayer, Add, 1000, Ore);
        EUDEndPlayerLoop();
        ```

    </br>
    </br>

- ### 区域位置相关函数

    </br>

    - #### setloc

        - `setloc`(loc : TrgLocation, x, y)  
            设置区域 [loc] 左上右下坐标分别为 [x], [y], [x], [y] （也就是将区域设置为一个点）

        - `setloc`(loc : TrgLocation, left, top, right, bottom)  
            设置区域 [loc] 左上右下坐标分别为 [left], [top], [right], [bottom]

        示例

        ```JavaScript
        setloc($L("Location 1"), 1234, 2345);
        setloc($L("Location 1"), 1234, 1234, 2345, 2345);
        ```

    </br>

    - #### addloc

        - `addloc`(loc : TrgLocation, x, y)  
            设置区域 [loc] 的左右坐标加 [x] 上下坐标加 [y] （实际大小不变，中心平移到另外一个位置）

        - `addloc`(loc : TrgLocation, left, top, right, bottom)  
            设置区域 [loc] 的左上右下分别加上 [left], [top], [right], [bottom]

        示例

        ```JavaScript
        addloc($L("Location 1"), 123, 234);
        addloc($L("Location 1"), 123, 234, 345, 456);
        // 配合 lengthdir 的用法
        addloc($L("Location 1"), lengthdir_256(888, 73)); // 将 Location 1 区域往 73 度（256 度制）方向平移 888 个坐标的距离
        addloc($L("Location 1"), lengthdir(888, 102)); // 将 Location 1 区域往 102 度方向平移 888 个坐标的距离
        ```

    </br>

    - #### dilateloc

        - `dilateloc`(loc : TrgLocation, x, y)  
            设置区域 [loc] 的左上右下分别加上 -[x], -[y], [x], [y] （中心不变，区域扩张）

        - `dilateloc`(loc : TrgLocation, left, top, right, bottom)  
            设置区域 [loc] 的左上右下分别加上 -[left], -[top], [right], [bottom]

        示例

        ```JavaScript
        dilateloc($L("Location 1"), 5, 5);
        dilateloc($L("Location 1"), 1, 2, 3, 4);
        ```

    </br>

    - #### getlocTL

        - `getlocTL`(loc : TrgLocation) : py_tuple[EUDVariable, EUDVariable]  
            获取一个区域的左上坐标

        示例

        ```JavaScript
        const top, left = getlocTL($L("Location 1"));
        ```

    </br>

    - #### setloc_epd

        - `setloc_epd`(loc : TrgLocation, epd)  
            设置区域 [loc] 的坐标为一个 EPD 地址 [epd] 上存储的值

        示例

        ```JavaScript
        // 它的内部实现大概与下面这个函数等效
        function setloc_epd(loc : TrgLocation, epd) {
            const x, y = posread_epd(epd);
            setloc(loc, x, y);
        }
        ```

    </br>
    </br>

- ### 内存读写相关函数

    </br>

    - #### dwbreak

        - `dwbreak`(number) : py_tuple[EUDVariable, EUDVariable, EUDVariable, EUDVariable, EUDVariable, EUDVariable]  
        将一个 dword 值 [number] 切分成 word 及 byte 形式

        示例

        ```JavaScript
        const w1, w2, b1, b2, b3, b4 = dwbreak(1234 + 0x10000 * 5678)[[0,1,2,3,4,5]];
        println("w1:{} w2:{} b1:{} b2:{} b3:{} b4:{}", w1, w2, b1, b2, b3, b4);
        ```

    </br>

    - #### **read/write**

        - `dwread`(ptr) : EUDVariable  
        - `wread`(ptr) : EUDVariable  
        - `bread`(ptr) : EUDVariable  

            读取本机指定内存地址 [ptr] 上的 dword/word/byte 值

        - `dwwrite`(ptr, dw)  
        - `wwrite`(ptr, w)  
        - `bwrite`(ptr, b)  

            将 dword/word/byte 值写入到本机内存地址 [ptr] 上

        示例

        ```JavaScript
        // 0x582144 参考 https://euddb.website/?pg=entry&id=490
        const SUP_RACE_ZERG = 0;
        const SUP_RACE_TERRAN = 1;
        const SUP_RACE_PROTOSS = 2;
        const SUP_TYPE_AVAILABLE = 0;
        const SUP_TYPE_USED = 1;
        const SUP_TYPE_MAX = 2;

        function SetPlayerSupply(player: TrgPlayer, race, type, amount) {
            dwwrite(0x582144 + (race) * 36 * 4 + (type) * 12 * 4 + (player) * 4, amount);
        }
        function GetPlayerSupply(player: TrgPlayer, race, type) {
            return dwread(0x582144 + (race) * 36 * 4 + (type) * 12 * 4 + (player) * 4);
        }

        SetPlayerSupply(P1, SUP_RACE_ZERG, SUP_TYPE_MAX, 800); // 将 玩家1 的虫族人口最大值设置为 400
        ```

    </br>

    - #### **read_epd/write_epd**

        - `dwread_epd`(epd) : EUDVariable  
            读取本机指定 [epd] 偏移地址上的 dword 值

        - `dwwrite_epd`(epd, value)  
            将 dword 值写入到本机 [epd] 偏移地址上

        - `wread_epd`(epd, subp) : EUDVariable  
        - `bread_epd`(epd, subp) : EUDVariable  

            读取本机指定玩家编号 [epd] 附加 [subp] 字节的偏移地址上的 word/byte 值，[subp] 小于 4（实际读取内存地址是 0x58A364 + epd * 4 + subp）

        - `wwrite_epd`(epd, subp, value)  
        - `bwrite_epd`(epd, subp, value)  

            将 word/byte 值写入到本机指定玩家编号 [epd] 的附加 [subp] 字节的偏移地址上，[subp] 小于 4（实际写入内存地址是 0x58A364 + epd * 4 + subp）  

        - `maskread_epd`(epd, mask) : EUDVariable  
            使用 [mask] 做掩码读取本机指定 [epd] 偏移地址上的 dword 值

        示例

        ```JavaScript
        // 与上面那个例子很相似，不过 _epd 系列基准的内存偏移不同，用 EPD 宏可以转换
        function SetPlayerSupply(player: TrgPlayer, race, type, amount) {
            dwwrite_epd(EPD(0x582144) + (race) * 36 + (type) * 12 + (player), amount);
        }
        function GetPlayerSupply(player: TrgPlayer, race, type) {
            return dwread_epd(EPD(0x582144) + (race) * 36 + (type) * 12 + (player));
        }
        const oe, os = div(EncodeWeapon("C-10 Concussion Rifle"), 4);
        println("bread_epd 鬼兵的武器间隔 {}", bread_epd(EPD(0x656FB8) + oe, os));
        println("maskread_epd 鬼兵的武器间隔 {}", bitrshift(maskread_epd(EPD(0x656FB8) + 1 + oe, 0xFF000000), 24));
        bwrite_epd(EPD(0x656FB8) + oe, os, 1); // 修改鬼兵的武器攻击间隔为 1
        ```

    </br>

    - #### add_epd/subtract_epd

        - `dwadd_epd`(epd, value)  
            对本机指定 [epd] 偏移地址的 dword 值自增 [value]  

        - `dwsubtract_epd`(epd, value)  
            对本机指定 [epd] 偏移地址的 dword 值自减 [value]  

        - `wadd_epd`(epd, subp, value)  
        - `badd_epd`(epd, subp, value)  

            对本机指定玩家编号 [epd] 对应的偏移位置附加 [subp] 字节位置上的 word/byte 值自增 [value]（实际操作的内存地址是 0x58A364 + epd * 4 + subp）  

        - `wsubtract_epd`(epd, subp, value)  
        - `bsubtract_epd`(epd, subp, value)  

            对本机指定玩家编号 [epd] 对应的偏移位置附加 [subp] 字节上的 word/byte 值自减 [value]（实际操作的内存地址是 0x58A364 + epd * 4 + subp）  

    </br>

    - #### repmovsd_epd

        - `repmovsd_epd`(dstepdp, srcepdp, copydwn)  
            从本机 [srcepdp] 拷贝 [copydwn] * 4 字节内存到 [dstepdp]

        示例

        ```JavaScript
        const src = Db(b"___1___2___3___4___5");
        const dst = Db(20);
        repmovsd_epd(EPD(src), EPD(dst), 5);
        // dst 将会是 Db(b"___1___2___3___4___5")
        ```

    </br>

    - #### dwepdread_epd

      - `dwepdread_epd`(epd) : py_tuple[EUDVariable, EUDVariable]  
        从本机 [epd] 偏移位置读取一个指针，它同时返回指针和该指针的 EPD 值

      - `epdread_epd`(epd) : EUDVariable  
        从本机 [epd] 偏移位置读取一个指针，它返回该指针的 EPD 值

        示例

        ```JavaScript
        // 创建一个机枪兵并获取它的指针和 EPD 值
        CreateUnit(1, "Terran Marine", $L("Location 1"), P1);
        const lastUnitEPD = EPD(0x628438);
        const ptr1, epd1 = dwepdread_epd(lastUnitEPD);
        var epd2 = epdread_epd(lastUnitEPD);
        ```

    </br>

    - ####  cunitread_epd

        - `cunitread_epd`(epd) : EUDVariable  
            从本机 [epd] 偏移位置读取一个 cunit 指针，该函数为读取 cunit 指针优化过，它返回一个指针

        - `cunitepdread_epd`(epd) : py_tuple[EUDVariable, EUDVariable]  
            从本机 [epd] 偏移位置读取一个 cunit 指针，该函数为读取 cunit 指针优化过，它返回该指针和该指针的 EPD 值

        示例

        ```JavaScript
        // 创建一个机枪兵并获取它的指针和 EPD 值
        CreateUnit(1, "Terran Marine", $L("Location 1"), P1);
        const lastUnitEPD = EPD(0x628438);
        const ptr1, epd1 = cunitepdread_epd(lastUnitEPD);
        var ptr2 = cunitread_epd(lastUnitEPD);
        ```

    </br>

    - #### posread_epd

        - `posread_epd`(epd) : py_tuple[EUDVariable, EUDVariable]  
            从本机 [epd] 偏移位置读取一个 pos（位置）

        示例

        ```JavaScript
        const screenTilePosEPD = EPD(0x57F1D0);
        const x, y = posread_epd(screenTilePosEPD);
        println("当前屏幕在地图上的坐标: ({}, {})", x, y);
        ```

    </br>

    - #### _cp 系列

        - `dwread_cp`(cpoffset) : EUDVariable  
        - `dwwrite_cp`(cpoffset, value)  
        - `dwadd_cp`(cpoffset, value)  
        - `dwsubtract_cp`(cpoffset, value)  
        - `wread_cp`(cpoffset, subp) : EUDVariable  
        - `bread_cp`(cpoffset, subp) : EUDVariable  
        - `wwrite_cp`(cpoffset, subp, w)  
        - `bwrite_cp`(cpoffset, subp, b)  
        - `maskread_cp`(cpoffset, mask) : EUDVariable  
        - `maskwrite_cp`(cpoffset, mask, value)  
        - `dwepdread_cp`(cpoffset) : py_tuple[EUDVariable, EUDVariable]  
        - `epdread_cp`(cpoffset) : EUDVariable  
        - `cunitread_cp`(cpoffset) : EUDVariable  
        - `cunitepdread_cp`(cpoffset) : py_tuple[EUDVariable, EUDVariable]  
        - `posread_cp`(cpoffset) : py_tuple[EUDVariable, EUDVariable]  
        
        _cp 系列所有函数用法都可以参考 _epd 系列，区别在于 _epd 系列需要传递一个 epd 偏移值作为其参数，而 _cp 是以传入一个以`当前玩家`这个全局变量的偏移值做参数  
        它们通常用于提升代码运行效率，减少最终生成的触发器字节码的指令（触发器）数量

        示例

        ```JavaScript
        // 以下代码仅仅为了演示 _cp 用法，并不是提升效率的做法，要提升效率可能需要自己思考
        const screenTilePosEPD = EPD(0x57F1D0);

        setcurpl(screenTilePosEPD); // 把 当前玩家 设置为 screenTilePosEPD
        const x, y = posread_cp(0); // 读取相对 当前玩家 位置偏移 0 字节的值，实际就是读取 screenTilePosEPD 位置上的值

        setcurpl(P1); // 把 当前玩家 设置为 P1 以便输出信息给 玩家1
        println("当前屏幕在地图上的坐标: ({}, {})", x, y);
        ```

    </br>

    - #### readgen

        - `readgen_epd`(mask, args) : duck  
        - `readgen_cp`(mask, args) : duck  

        可用于创建自定义规则本机内存读取函数

        示例

        ```JavaScript
        // 256 grids = 8192 pixels = x and y are within 0 ~ 8191 (0x1FFF)
        // 编译期函数只能用 py_eval 定义
        const posread_epd = readgen_epd(
            0x1FFF1FFF,
            list(0, py_eval('lambda x: x if x < 65536 else 0')),
            list(0, py_eval('lambda y: y // 65536 if y >= 65536 else 0')),
        );
        const x, y = posread_epd(epd_address);
        ```

    </br>

    - #### memcpy

        - `memcpy`(dst, src, copylen)  
            从本机内存地址 [src] 拷贝 [copylen] 字节内容到内存地址 [dst]

    </br>

    - #### memcmp

        - `memcmp`(buf1, buf2, count) : EUDVariable  
            对比本机 [buf1] 和 [buf2] 两个内存块 [count] 字节内容  
            如果两块内存完全一致，则返回 0  
            否则，将对比第一个不同的字节并返回大于或者小于 0 的结果

    </br>

    - #### strcpy

        - `strcpy`(dst, src)  
            从本机内存地址 [src] 拷贝字符串（以 `\x00` 终止的字节块）内容到内存地址 [dst]

    </br>

    - #### strcmp

        - `strcmp`(s1, s2) : EUDVariable  
            对比本机 [s1] 和 [s2] 两个内存块上的字符串（以 `\x00` 终止的字节块）内容  
            如果两块内存完全一致，则返回 0  
            否则，将对比第一个不同的字节并返回大于或者小于 0 的结果

    </br>

    - #### strlen

      - `strlen`(ptr) : EUDVariable  
        获取本机指针 [ptr] 指向的字符串（以 `\x00` 终止的字节块）长度，单位是字节

      - `strlen_epd`(epd) : EUDVariable  
        获取本机 [epd] 偏移位置指针指向的字符串（以 `\x00` 终止的字节块）长度，单位是字节

    </br>

    - #### strnstr

        - `strnstr`(ptr, substr, count) : EUDVariable  
            在本机指针 [ptr] 指向的字符串的前 [count] 个字符中搜索另外一个字符串 [substr]  
            找到了就返回找到的位置的指针，没找到返回 0

    </br>

    - #### dbstr

        - `dbstr_addstr`(dst, src) : EUDVariable  
            将本机字符串 [src] 拷贝到 [dst] 位置，返回地址 dst + strlen(src)

        - `dbstr_addstr_epd`(dst, srcepd) : EUDVariable  
            将本机 [srcepd] 编号偏移位置内存的字符串拷贝到 [dst] 位置，它与 dbstr_addstr(dst, EPD(srcepd)) 等价，返回地址 dst + strlen_epd(srcepd)

        - `dbstr_adddw`(dst, number) : EUDVariable  
            将一个数字值转换成文本打印到本机 dst 位置上，返回地址 dst + strlen(itoa(number))

        - `dbstr_addptr`(dst, ptr) : EUDVariable  
            将一个数字值转换成 16 进制数字文本打印到本机 dst 位置上，返回地址 dst + strlen(itox(number))

        - `dbstr_print`(dst, *args, EOS=true, encoding="UTF-8")  
            将多个参数以字符串的形式打印到本机 [dst] 位置上；命名参数 [EOS] 指定是否在字符串结尾附加字符串结尾符号，默认 true；命名参数 [encoding] 指定编码，默认 UTF-8

        - `sprintf`(dst, format_string : py_str, *args, EOS=true, encoding="UTF-8")  
            以 [format_string] 作为格式将多个参数格式化后打印到本机 [dst] 位置上；命名参数 [EOS] 指定是否在字符串结尾附加字符串结尾符号，默认 true；命名参数 [encoding] 指定编码，默认 UTF-8

        示例

        ```JavaScript
        const s = Db(100);
        var addr = unProxy(s);
        addr = dbstr_addstr(addr, Db("0123"));
        addr = dbstr_adddw(addr, 4567);
        addr = dbstr_addptr(addr, 0x89ABCDEF);
        simpleprint(s); // 0123456789ABCDEF

        dbstr_print(s, "0123", 4567, hptr(0x89ABCDEF));
        simpleprint(s); // 0123456789ABCDEF

        sprintf(s, "{}{}{:x}", "0123", 4567, 0x89ABCDEF);
        simpleprint(s); // 0123456789ABCDEF
        ```

    </br>

    - #### ptr2s/epd2s

      - `ptr2s`(ptr) : Db*

        以字符串形式读取本机 [ptr] 指向的内存，在格式化文本中使用 `{:s}` 占位符等效

      - `epd2s`(epd) : Db*

        以字符串形式读取本机 [epd] 指向的内存，在格式化文本中使用 `{:t}` 占位符等效

    </br>

    - #### hptr

        - `hptr`(value) : Db*  
            将 [value] 转成 16 进制形式输出，在格式化文本中使用 `{:x}` 占位符等效

        示例

        ```JavaScript
        println("{}, {}", 0xAABBCC, hptr(0xAABBCC)); // 11189196, 00AABBCC
        println("{}, {:x}", 0xAABBCC, 0xAABBCC);     // 11189196, 00AABBCC
        ```

    </br>

    - #### gettextptr

        - `gettextptr()` : EUDVariable  
            获取本机显示到屏幕上的下一行文本的行

    </br>

    - #### dwpatch_epd

        - `dwpatch_epd`(dstepd, value)  
            给目标 EPD 位置 [dstepd] 打内存补丁

    </br>

    - #### GetMapStringAddr

        - `GetMapStringAddr`(strID : TrgString) : EUDVariable  
            获取本机一个地图字符串或字符串编号 [strID] 的内存地址  
            这个不是 $T （或称 EncodeString）获取到的那个，$T 获取的是地图字符串在地图文件中对应的编号

        示例

        ```JavaScript
        // 它支持使用字符串及 ID 获取，以下这几种用法等效
        const addr = GetMapStringAddr(6);
        const addr = GetMapStringAddr("Force 3");
        ```

    </br>

    - #### GetTBLAddr

        - `GetTBLAddr`(TBLKey : StatText) : EUDVariable  
            获取本机一个 TBL 表中的Key/编号 [TBLKey] 的内存地址  
            这个不是 $B（或称 EncodeTBL） 获取到的那个，EncodeTBL 获取 TBL 表中保存的各种信息字符串在 tbl 文件中对应的编号  

            > 值得一提的是，TBLKey 字符串本身并不一定在内存中实际存在。  
            > 比如，内存中并不存在 "Terran Siege Tank (Tank Mode)" 这个字符串。  
            > "Terran Siege Tank (Tank Mode)" 这个 TBLKey 所对应的地址内的字符串 （位于 stat_txt.tbl string section 内）打印出来是 "Terran Siege Tank"

        示例

        ```JavaScript
        // 它支持使用 TBLKey 或是 TBL 编号获取，以下这几种用法等效
        const addr = GetTBLAddr(4);
        const addr = GetTBLAddr("Terran Goliath");
        const addr = wread(dwread_epd(EPD(0x6D1238)) + $B("Terran Goliath"));
        ```

    </br>

    - #### settbl

        - `settbl`(tblID : StatText, offset, *args, encoding="cp949")  
        - `settbl2`(tblID : StatText, offset, *args, encoding="cp949")  

            设置本机指定 [tblID] 在 TBL 表中的偏移 [offset] 内存字符串的值为 *args，settbl 和 settbl2 的区别在于，settbl 会在设置的字符串最后加上 EOS 字符，而 settbl2 不会

        - `settblf`(tblID : StatText, offset, format_string, *args, encoding="cp949")  
        - `settblf2`(tblID : StatText, offset, format_string, *args, encoding="cp949")  

            设置本机指定 [tblID] 在 TBL 表中的偏移 [offset] 内存字符串的值为一个格式化字符串，settblf 和 settblf2 的区别在于，settblf 会在设置的字符串最后加上 EOS 字符，而 settblf2 不会

      ```JavaScript
        // 以下代码等价
        settbl("Terran Goliath", 1, "1234");
        settbl2("Terran Goliath", 1, "1234\0");
        dbstr_print(GetTBLAddr("Terran Goliath") + 1, "1234\0", EOS = false); // 将 Terran Goliath 改名成 T1234
        ```

    </br>
    </br>

  ### 数学函数

    </br>

    - #### atan2

        - `atan2`(y, x) : EUDVariable  
            两个参数的反正切函数，返回的是原点至点 (x,y) 的方位角，即与 x 轴的夹角

        - `atan2_256`(x, y) : EUDVariable  
            与 atan2 区别在于，在对角度对处理上，它将一个圆周分成 256 等份，角度是 256 度制，不是 360 度制

        示例

        ```JavaScript
        function angleBetween(x1, y1, x2, y2) {
            return atan2(x2 - x1, y1 - y2);
        }

        // 星际中的单位使用的角度用一个字节存储，所以是 256 度制
        // 0   度表示面朝上，0 到 256 递增向右旋转
        // 64  度表示面朝右，128 度表示面朝下，192 度表示面朝左
        // 所以它需要使用一个叫 atan2_256 的函数，它会把 360 度制角度换算成 256 度制

        function angleBetween_256(x1, y1, x2, y2) {
            return atan2_256(x2 - x1, y1 - y2);
        }

        println("从 (131, 33) 到 (765, 546) 的角度是 {}", angleBetween_256(131, 33, 765, 546));
        ```

    </br>

    - #### sqrt

        - `sqrt`(x) : py_int | EUDVariable  
            计算 [x] 的平方根（取整）

        示例

        ```JavaScript
        function distanceBetween(x1, y1, x2, y2) {
            const x = x2 - x1;
            const y = y2 - y1;
            return sqrt(x*x + y*y);
        }

        println("从 (131, 33) 到 (765, 546) 的距离是 {}", distanceBetween(131, 33, 765, 546));
        ```

    </br>

    - #### lengthdir

        - `lengthdir`(length, angle) : tuple[EUDVariable, EUDVariable]  
            计算从 0, 0 位置出发向着 [angle] 角度出发走 [length] 距离的另外一个坐标  

        - `lengthdir_256`(length, angle) : tuple[EUDVariable, EUDVariable]  
            与 lengthdir 的区别在于，在对角度对处理上，它将一个圆周分成 256 等份，角度是 256 度制，不是 360 度制  

        示例

        ```JavaScript
        function polarProjection(x, y, length, angle) {
            const ox, oy = lengthdir(length, angle);
            return x + ox, y + oy;
        }

        // 星际中的单位使用的角度用一个字节存储，所以是 256 度制
        // 0   度表示面朝上，0 到 256 递增向右旋转
        // 64  度表示面朝右，128 度表示面朝下，192 度表示面朝左
        // lengthdir_256 函数，会把 360 度制角度换算成 256 度制，但 lenghdir_256 是 0 度表示朝右，0 到 256 递增向左旋转
        // 所以实际应该把单位面向的角度转换过去，并且把返回的 y 换算成 -y
        function polarProjection_256(x, y, length, angle) {
            var angt = 320;
            VProc(
                angle,
                list(
                    angle.AddNumberX(0xFFFFFFFF, 0x55555555),
                    angle.AddNumberX(0xFFFFFFFF, 0xAAAAAAAA),
                    angle.AddNumber(1),
                    angle.QueueAddTo(angt),
                ),
            );
            var ox, oy = lengthdir_256(length, angt);
            VProc(
                list(ox, oy),
                list(
                    oy.AddNumberX(0xFFFFFFFF, 0x55555555),
                    oy.AddNumberX(0xFFFFFFFF, 0xAAAAAAAA),
                    oy.AddNumber(1),
                    ox.QueueAddTo(x), oy.QueueAddTo(y),
                ),
            );
            return x, y;
        }

        const x, y = polarProjection_256(1264, 880, 888, 73);

        println("从 (1264, 880) 出发朝 73 度（256 度制）走 888 的距离到达 ({}, {})", x, y);
        ```

    </br>

    - #### pow

        - `pow`(x, y) : py_int | EUDVariable  
            计算 [x] 的 [y] 次幂，如果两个参数都是常量，那么它的返回值也可以是常量

        示例

        ```JavaScript
        println("2^10 = {}", pow(2, 10));
        ```

    </br>

    - #### div

        - `div`(a, b) : py_tuple[EUDVariable, EUDVariable]  
            无符号整数除法 [a] 除以 [b]，仅支持正整数，返回商和余数

        - `div_towards_zero`(a, b) : py_tuple[EUDVariable, EUDVariable]  
            向下取整有符号除法 [a] 除以 [b]，返回商和余数，当商是负数时，商向上取整（向 0 的方向）

        - `div_floor`(a, b) : py_tuple[EUDVariable, EUDVariable]  
            向下取整有符号除法 [a] 除以 [b]，返回商和余数，商是负数时依然会向下取整（向远离 0 的方向）

        - `div_euclid`(a, b) : py_tuple[EUDVariable, EUDVariable]  
            计算 [a] 除以 [b] 的欧几里得商和余数。  
            这是计算有符号（负数）除法，它计算商使得 `a = 商 * b + 余数`，并且 `0 <= 余数 < 绝对值(b)`。  
            换句话说，结果是将 a ÷ b 四舍五入到商的值，使得 `a >= 商 * b`。如果 `a > 0`，则等于向零舍入；如果 `a < 0`，则等于朝着正负无穷大方向（远离零）四舍五入。  

        示例

        ```JavaScript
        // 0.9.9.7 除了 div 其它几个都报错 : Undefined function f_div_*
        ```

    </br>

    - #### rand

        - `rand()` : EUDVariable  
            生成一个随机 word（范围 0~0xFFFF）  

        - `dwrand()` : EUDVariable  
            生成一个随机 dword（范围 0~0xFFFFFFFF）  

      注意不要在非同步条件下使用随机数函数

        示例

        ```JavaScript
        const r = rand();
        ```

    </br>

    - #### seed

        - `srand`(seed)  
            设置随机因子为 [seed]

        - `getseed()` : EUDVariable  
            获取设置好的随机因子

      注意不要在非同步条件下使用随机数函数

        示例

        ```JavaScript
        var seed = getseed();
        srand(seed + 1);
        ```

    </br>

    - #### randomize

        - `randomize()` : EUDVariable  
            初始化随机因子

        注意不要在非同步条件下使用随机数函数

        示例

        ```JavaScript
        function onPluginStart() {
            randomize();
        }
        ```

    </br>

    - #### getgametick

        - `getgametick()` : EUDVariable  
            获取逝去的游戏帧数，游戏速率为极快（Fastest）的情况下是 42 毫秒每帧

        示例

        ```JavaScript
        var tick = getgametick();
        ```

      

    

  ### 位运算函数

    </br>

    - #### bitand

        - `bitand`(a, b) : py_int | EUDVariable  
            按位与运算 [a] & [b]

        示例

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}", bitand(a, b)); // 0 （二进制的 0b0000）
        ```

    </br>

    - #### bitor

        - `bitor`(a, b) : py_int | EUDVariable  
        按位或运算 [a] | [b]

        示例

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}", bitor(a, b)); // 15 （二进制的 0b1111）
        ```

    </br>

    - #### bitnot

        - `bitnot`(a) : py_int | EUDVariable  
        按位取反运算 ~[a]

        示例

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}, {}", bitnot(a), b); // 12, 12 （二进制的 0b1100, 0b1100）
        ```

    </br>

    - #### bitxor

        - `bitxor`(a, b) : py_int | EUDVariable  
        按位异或运算 [a] ^ [b]

        示例

        ```JavaScript
        var a = 0b0111; // 7
        var b = 0b1110; // 14
        println("{}", bitxor(a, b)); // 9 （二进制的 0b1001）
        ```

    </br>

    - #### bitnand

        - `bitnand`(a, b) : py_int | EUDVariable  
        按位与反运算 ~([a] & [b])

        示例

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}", bitnand(a, b)); // 15 （二进制的 0b1111）
        ```

    </br>

    - #### bitnor

        - `bitnor`(a, b) : py_int | EUDVariable  
        按位或反运算 ~([a] | [b])

        示例

        ```JavaScript
        var a = 0b0011; // 3
        var b = 0b1100; // 12
        println("{}", bitnor(a, b)); // 0 （二进制的 0b0000）
        ```

    </br>

    - #### bitnxor

        - `bitnxor`(a, b) : py_int | EUDVariable  
        按位异或反运算 ~([a] ^ [b])

        示例

        ```JavaScript
        var a = 0b0111;
        var b = 0b1110;
        println("{}", bitnxor(a, b)); // 6 （二进制的 0b0110）
        ```

    </br>

    - #### bitlshift

        - `bitlshift`(a, b) : py_int | EUDVariable  
        [a] << [b]

        左位移运算

        示例

        ```JavaScript
        var a = 0b0111; // 7
        var b = 1;
        println("{}", bitlshift(a, b)); // 14 （二进制的 0b1110）
        ```

    </br>

    - #### bitrshift

        - `bitrshift`(a, b) : py_int | EUDVariable  
        [a] >> [b]

        右位移运算

        示例

        ```JavaScript
        var a = 0b0111; // 7
        var b = 1;
        println("{}", bitrshift(a, b)); // 3 （二进制的 0b0011）
        ```


    </br>
    </br>

- ### QueueGameCommand 函数

    星际争霸定期向其它玩家广播数据包，此系列函数用于将数据包添加到本机广播队列中，等待星际争霸广播它  
    QueueGameCommand 系列的函数都是作用于`本机玩家`而非`当前玩家`，没法向不在游戏内的玩家或电脑的玩家使用，可用 getuserplayerid() 获取本机玩家的编号判断  

    > 注意：如果本机数据包队列已满，这些函数调用会失效，并且是静默的，它不会有提示也不会有错误的返回值，所以不要过于频繁的调用这些函数。

    </br>

    - #### QueueGameCommand

        - `QueueGameCommand`(data, size)  
            向本机广播队列添加尺寸为 [size] 数据包 [data]，本节所有的函数都是对这个函数发送特定的数据包的封装

    </br>

    - #### QueueGameCommand_MinimapPing

        - `QueueGameCommand_MinimapPing`(xy)  
            向本机广播队列添加小地图 [xy] 坐标上发 Ping 的数据包，xy 的计算方法是 x + y*65536

        示例

        ```JavaScript
        // 在 1234, 2345 这个坐标上发起 Ping
        QueueGameCommand_MinimapPing(1234 + 2345 * 65536);
        ```

    </br>

    - #### QueueGameCommand_QueuedRightClick

        - `QueueGameCommand_QueuedRightClick`(xy)  
            向本机广播队列添加右键点击 [xy] 的数据包，xy 的计算方法是 x + y*65536

        示例

        ```JavaScript
        // 在 1234, 2345 这个坐标上点右键，如果有选中单位，那么它就会往这跑
        QueueGameCommand_QueuedRightClick(1234 + 2345 * 65536);
        ```

    </br>

    - #### QueueGameCommand_Select

        - `QueueGameCommand_Select`(n, ptrList: EUDArray)  
            向本机广播队列添加选中一些单位的数据包，[n] 是单位个数，[ptrList] 是 cunit 指针列表，不是 epd 列表

            > 这个只是会发出本机 “已经选中单位的数据包”，并不会在本机画面上选中单位，它只是告诉其它在线玩家，本机选中了这些单位，如果紧接着发 RightClick 数据包，则这些单位就会往目标位置移动

        示例

        ```JavaScript
        const uar = EUDArray(12);
        if (playerexist(P1) && GetPlayerInfo(P1).type == 0x06) { // 判断玩家1是否为在线的人类玩家
            foreach(i : py_range(3)) {
                uar[i] = dwread_epd(EPD(0x628438));
                CreateUnitWithProperties(1, "Zerg Overlord", "Location 1", P1, UnitProperty(invincible = true));
            }
        }
        if (getuserplayerid() == $P1) { // 判断本机是否玩家1
            QueueGameCommand_Select(3, uar);
            QueueGameCommand_QueuedRightClick(1234 + 2345 * 65536);
        }
        ```

    </br>

    - #### QueueGameCommand_PauseGame

        - `QueueGameCommand_PauseGame()`  
            向本机广播队列添加暂停游戏的数据包

    </br>

    - #### QueueGameCommand_ResumeGame

        - `QueueGameCommand_ResumeGame()`  
            向本机广播队列添加继续游戏的数据包

    </br>

    - #### QueueGameCommand_RestartGame

        - `QueueGameCommand_RestartGame()`  
            向本机广播队列添加重新开始游戏的数据包

    </br>

    - #### QueueGameCommand_UseCheat

        - `QueueGameCommand_UseCheat`(启用/禁用)  
            向本机广播队列添加 [启用/禁用] true/false 作弊的数据包，它不会真启用/禁用作弊，只是会发出一个 “我作弊了” 或者 “我取消作弊了” 的数据包

        示例

        ```JavaScript
        QueueGameCommand_UseCheat(true);  // 启用作弊
        QueueGameCommand_UseCheat(false); // 关闭作弊
        ```

    </br>

    - #### QueueGameCommand_TrainUnit

        - `QueueGameCommand_TrainUnit`(unit: TrgUnit)  
            向本机广播队列添加训练指定单位类型的数据包，配合 QueueGameCommand_Select 选中单位后使用

        示例

        ```JavaScript
        once {
            const uar = EUDArray(12);
            if (playerexist(P1) && GetPlayerInfo(P1).type == 0x06) { // 判断玩家1是否为在线的人类玩家
                SetResources(P1, Add, 10000, OreAndGas);
                uar[0] = dwread_epd(EPD(0x628438));
                CreateUnitWithProperties(1, "Terran Command Center", "Location 1", P1, UnitProperty(invincible = true));
            }
            if (getuserplayerid() == $P1) { // 判断本机是否玩家1
                QueueGameCommand_Select(1, uar); /* 选中它 */
                QueueGameCommand_QueuedRightClick(1234 + 2345 * 65536); /* 设置集结点到 1234, 2345 */
                QueueGameCommand_TrainUnit("Terran SCV"); /* 训练一个 SCV */
            }
        }
        ```

    </br>

    - #### QueueGameCommand_MergeDarkArchon()

        - `QueueGameCommand_MergeDarkArchon()`  
            向本机广播队列添加合成红球（黑暗执政官）的数据包，配合 QueueGameCommand_Select 选中单位后使用

        示例

        ```JavaScript
        once {
            const uar = EUDArray(12);
            if (playerexist(P1) && GetPlayerInfo(P1).type == 0x06) { // 判断玩家1是否为在线的人类玩家
                foreach(i : py_range(6)) {
                    uar[i] = dwread_epd(EPD(0x628438));
                    CreateUnitWithProperties(1, "Protoss Dark Templar", "Location 1", P1, UnitProperty(invincible = true));
                }
            }
            if (getuserplayerid() == $P1) { // 判断本机是否玩家1
                QueueGameCommand_Select(6, uar);
                QueueGameCommand_MergeDarkArchon();
            }
        }
        ```

    </br>

    - #### QueueGameCommand_MergeArchon

        - `QueueGameCommand_MergeArchon()`  
            向本机广播队列添加合成白球（光明执政官）的数据包，配合 QueueGameCommand_Select 选中单位后使用

        示例

        ```JavaScript
        once {
            const uar = EUDArray(12);
            if (playerexist(P1) && GetPlayerInfo(P1).type == 0x06) { // 判断玩家1是否为在线的人类玩家
                foreach(i : py_range(6)) {
                    uar[i] = dwread_epd(EPD(0x628438));
                    CreateUnitWithProperties(1, "Protoss High Templar", "Location 1", P1, UnitProperty(invincible = true));
                }
            }
            if (getuserplayerid() == $P1) { // 判断本机是否玩家1
                QueueGameCommand_Select(6, uar);
                QueueGameCommand_MergeArchon();
            }
        }
        ```

      

    

  







