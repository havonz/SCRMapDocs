# epScript 对象的使用

<br />

- [对象类型](#%E5%AF%B9%E8%B1%A1%E7%B1%BB%E5%9E%8B)
    - [声明方法](#%E5%A3%B0%E6%98%8E%E6%96%B9%E6%B3%95)
    - [创建实例](#%E5%88%9B%E5%BB%BA%E5%AE%9E%E4%BE%8B)

<br />

## 对象类型

对象类型为引用类型


- ### 声明方法

    可以用如下方式声明一个对象类型

    ```JavaScript
    object 对象类型名 {
        var 字段名1;
        var 字段名2;
        var 字段名3;
        function 方法名1_给字段1赋值(值){
            this.字段名1 = 值;
        }
        function 获取字段1的值() {
            return this.字段名1;
        }
    };
    ```

    以下声明了一个 Date 对象类型

    ```JavaScript
    object Date {
        var year, month, day, hour, minute, second;
        /***
         * weekday: {0 = 周日, 1 = 周一, 2 = 周二, 3 = 周三, 4 = 周四, 5 = 周五, 6 = 周六}
         * @type {number}
         * @public
         */
        var weekday;

        function update_timestamp(unixTimestamp) {
            const MONTH_DAYS = EUDArray(list(31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31));
            var year, month, day = 1970, 1, 1;
            var days, seconds = div(unixTimestamp, 86400);
            const weekday = (days + 4) % 7;

            while (true) {
                for (var m = 0 ; m < 12 ; m++) {
                    var daysInMonth = maskread_epd(EPD(MONTH_DAYS) + m, 31);
                    // January of leap year (year is multiple of 4)
                    if (m == 0 && year.ExactlyX(0, 3)) daysInMonth += 1;
                    if (days < daysInMonth) {
                        day = days + 1;
                        days = 0;
                        break;
                    }
                    days -= daysInMonth;
                    month += 1;
                }
                EUDSetContinuePoint();
                if (days == 0) break;
                month = 1;
                year += 1;
            }
            const hour, minuteAndSecond = div(seconds, 3600);
            const minute, second = div(minuteAndSecond, 60);

            this.year = year;
            this.month = month;
            this.day = day;
            this.hour = hour;
            this.minute = minute;
            this.second = second;
            this.weekday = weekday;
        }
    };
    ```


- ### 创建实例

    - 有两种方法可以创建一个对象实例  
        - 静态初始化：`const 对象1 = 对象类型名();`  
        - 动态初始化：`const 对象1 = 对象类型名.alloc();` 你可以将它传递到任何作用域使用，用完了记得用 `对象类型名.free(对象1);` 释放掉它占用的内存。  

    以下是 Date 对象实例使用方法
    ```JavaScript
    function afterTriggerExec() {

        var timestamp;
        var previousSysTime;
        const newSysTime = dwread(0x51CE8C);
        once {
            timestamp = dwread(0x6D0F38);  // game start timestamp
            previousSysTime = newSysTime;
        }
        static var cumulativeSysTime = 0;
        cumulativeSysTime += (previousSysTime - newSysTime);  // time difference
        previousSysTime = newSysTime;

        const date = Date();

        if (cumulativeSysTime >= 1000) {
            const second, millisecond = div(cumulativeSysTime, 1000);
            cumulativeSysTime = millisecond;
            timestamp += second;
            // date.update_timestamp(timestamp);
            date.update_timestamp(8 * 3600 + timestamp);
        }

        const weekdayToName = function (weekday) {
            switch (weekday) {
                case 0: return EPD(Db("周日"));
                case 1: return EPD(Db("周一"));
                case 2: return EPD(Db("周二"));
                case 3: return EPD(Db("周三"));
                case 4: return EPD(Db("周四"));
                case 5: return EPD(Db("周五"));
                case 6: return EPD(Db("周六"));
            }
        };

        printAllAt(10, "\x13\x04北京时间 : {}-{}-{}({:t} ) {}:{}:{}",
            date.year, date.month, date.day, weekdayToName(date.weekday), date.hour, date.minute, date.second);

    }
    ```



