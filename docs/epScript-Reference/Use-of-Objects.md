---
sidebar_position: 4
---

# Use of Objects

<br />

- [Object types](#object-types)
    - [Declarations](#declarations)
    - [Creating Instances](#creating-instances)

<br />

## Object types
Object types are reference types.

- ### Declarations

    You can only declare object in module scope, and must put semicolon at end of definition.

    You can declare an object type as follows:  

    ```JavaScript
    object ObjectTypeName { 
        var fieldName1; 
        var fieldName2;
        var fieldName3;
        function methodName1_assignValueToField1(value){
            this.fieldName1 = value;
        }
        function getTheValueOfField1() { 
            return this.fieldName1;
        }
    };
    ```

    You can define constructor and destructor:

    ```js
    const objList = EUDArray(100);
    var objCount = 0;
    object Obj {
        var a, b, c;
        var index;
        function constructor(a, b, c) {
            this.a = a;
            this.b = b;
            this.c = c;

            this.index = objCount;
            objList[objCount] = this;
            objCount++;
        }
        function destructor() {  // runs on Obj.free(instance)
            objCount--;
            const lastObj = objList[objCount];
            objList[this.index] = lastObj;
        }
    };

    const staticObj = Obj(1, 2, 3);
    const dynObj = Obj.alloc(1, 2, 3);
    ```

    `(there's constructor_static but defining it in epScript has limitation.)`

    The following declares a Date object type

    ```JavaScript
    object Date {
        var year, month, day, hour, minute, second;
        /***
         * weekday: {0 = Sunday, 1 = Monday, 2 = Tuesday, 3 = Wednesday, 4 = Thursday, 5 = Friday, 6 = Saturday}
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


- ### Creating Instances
    - There are two ways to create an object instance:  
        - Static initialization: `const object1 = ObjectTypeName()`;  
        - Dynamic initialization: `const object1 = ObjectTypeName.alloc();` You can pass it to any scope for use. Remember to use `ObjectTypeName.free(object1);` to free the memory it occupies when done.  

    The following is an example of using the Date object instance:  
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
                case 0: return EPD(Db("Sun"));
                case 1: return EPD(Db("Mon")); 
                case 2: return EPD(Db("Tue"));
                case 3: return EPD(Db("Wed"));
                case 4: return EPD(Db("Thu"));
                case 5: return EPD(Db("Fri"));  
                case 6: return EPD(Db("Sat"));
            }
        };

        printAllAt(10, "\x13\x04CST : {}-{}-{}({:t} ) {}:{}:{}",
            date.year, date.month, date.day, weekdayToName(date.weekday), date.hour, date.minute, date.second);

    }
    ```



