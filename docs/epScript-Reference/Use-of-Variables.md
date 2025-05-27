---
sidebar_position: 2
---

# Use of Variables

<br />

- [Basic Description](#basic-description)
    - [Variable Declaration](#variable-declaration)
    - [Static Variables](#static-variables)
    - [Constant Declaration](#constant-declaration)
    - [Variable Assignment](#variable-assignment)
    - [Variable Scope](#variable-scope)
    - [Mathematical Operations Of Variables](#mathematical-operations-of-variables)
    - [Object Types](#object-types)
    - [Value Types, Reference Types](#value-types-reference-types)
    - [Runtime Strings](#runtime-strings)
    - [Appendix Static Or Dynamic Instantiation](#appendix-static-or-dynamic-instantiation)
    - [Explanation Of const And var](#explanation-of-const-and-var)

<br />

## Basic Description

All variables in epScript code allow desync modification or access. sync modification access belongs to synced-data, and desync modification access belongs to desync-data.  
epScript has only one value type variable, which is a 32-bit unsigned integer.  

- ### Variable Declaration

    You can use `var` syntax to declare a normal variable

    ```JavaScript
    var variableName1;
    static var variableName1 = 0;
    // The above two writings are equivalent
    var variableName2 = initialValue; 
    ```

- ### Static Variables

    In fact, all variables are implemented internally as static, but local variables are restored to their initial values each time.  
    Variables without an initial value have static properties. Using the `static` keyword to declare a variable can explicitly give it static properties.  

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


- ### Constant Declaration

    The value of a constant cannot be changed at runtime.  
    All reference types (objects) must be declared with the const modifier. The state of reference type objects can be changed at runtime, but the target object they refer to cannot be changed at runtime.  

    ```JavaScript
    const arrayVariableName1 = EUDArray(arraySize);
    const object1 = objectTypeName();
    ```


- ### Variable Assignment

    The value of a variable can be changed at runtime. After declaration, the equal sign (`=`) can be used to assign a value to the variable.

    ```JavaScript
    var variableName1; // Declare variable

    variableName1 = 100; // Assign variable to 100
    variableName1 = variableName1 + 2;  // Increment variable by 2
    ```


- ### Variable Scope

    The scope of a variable is from its declaration down to leaving the current layer of braces (or file). Variables of the same name in inner scopes have higher priority than those in outer scopes.   
    Although unnecessary, here is an incorrect example:  
    ```JavaScript
    var moduleLevelVariable = 100;

    function incorrectExample() {
        localVariable1 = 2; // This is wrong because localVariable1 is declared in the next line 
        var localVariable1 = 2; 
        {
            localVariable1 = 1;
            var localVariable2 = 2;
            var localVariable1 = 100; 
            localVariable2 = localVariable1; // There are two localVariable1, take the one with the smaller and more specific scope, which is 100
            localVariable2 = moduleLevelVariable;
        }
        localVariable1 = localVariable2; // This is wrong because localVariable2 is declared inside that layer of braces, it can not leave its scope
        localVariable1 = moduleLevelVariable;
    }
    ```


- ### Mathematical Operations Of Variables

    Although variables can only be 32-bit unsigned integers, variables can represent negative numbers, like unsigned 32-bit integers in C language.  
    ```JavaScript
    var a = 0;
    a -= 2;
    if (a > 0) { // This condition is true, the negative number of an unsigned number is greater than the positive number
        println("a == 0x{:x}", a); // a == 0xFFFFFFFE
    }
    if (a >= 0x80000000) { // To determine if a is negative, it should be judged like this
        println("a(-{}) is less than 0", -a); // a(-2) is less than 0
    }
    if (a < -1 && a > -3) { // This is true
        println("a(0x{:x}) is less than -1 and greater than -3", a); // a(0xFFFFFFFE) is less than -1 and greater than -3
    }
    a -= 1; 
    if (a == -3) { // This is true
        println("a(0x{:x}) is now equal to -3", a); // a(0xFFFFFFFD) is now equal to -3
    }
    var b = 0;
    DoActions(b.SubtractNumber(2)); // This is invalid, Subtract action cannot subtract the value below 0
    println("b == 0x{:x}", b); // b == 0x00000000
    ```


- ### Object Types

    Object types are reference types, which are different from value types.

    ```JavaScript
    object ObjectTypeName {
        var fieldName1; 
        var fieldName2;
        var fieldName3;
        function methodName1_AssignValueToField1(value) { 
            this.fieldName1 = value;
        }
        function GetTheValueOfField1() {
            return this.fieldName1;
        }
    };
    ```

    - #### There are two ways to create an object instance  

        - Static initialization: `const object1 = ObjectTypeName();`  
        - Dynamic initialization: `const object1 = ObjectTypeName.alloc();` You can pass it to any scope for use. Remember to release the memory it occupies with `ObjectTypeName.free(object1);`  


- ### Value Types, Reference Types

    Value types

    ```JavaScript
    var a = 27;
    var b = a;
    b = 3;
    println("{}, {}", a, b); // Outputs 27, 3
    ```

    Reference types

    ```Java
    const a = EUDArray(1);
    a[0] = 27;
    const b = a;
    b[0] = 3;
    println("{}, {}", a[0], b[0]); // Outputs 3, 3
    ```

    Reference types pass the memory address of the value, while value types pass the value itself.


- ### Runtime Strings

    You can use GetMapStringAddr to get the address of the map string in memory and use memory functions to modify its content (unable to change the string size).  

    ```JavaScript
    const buf_index = GetStringIndex(py_str(" ") * 64); // A string of length 64, all spaces
    const buf_addr = GetMapStringAddr(buf_index);
    dbstr_print(buf_addr, "String 1"); 
    DisplayText(buf_index);
    dbstr_print(buf_addr, "String ", 2); 
    DisplayText(buf_index);
    ```

    Implement using StringBuffer to operate memory buffers (unable to change string size).  

    ```JavaScript
    const buf = StringBuffer(64);
    buf.insertf(0, "String {}", 1);
    buf.append("String 2");
    buf.DisplayAt(6); // Output display to line 6 of the screen text
    DisplayText(buf.StringIndex);
    ```

    Use Db memory data (unable to change string size).  

    ```JavaScript
    const buf_addr = Db(64);
    dbstr_print(buf_addr, "String 1");
    simpleprint(buf_addr);
    dbstr_print(buf_addr, "String ", 2);
    simpleprint(buf_addr);
    ```


- ### Appendix Static Or Dynamic Instantiation

    Every variable in epScript has fixed memory address. So every variable is persistent. For example, consider this code.

    ```JavaScript
    function x() {
        var y;
        y++;
        return y;
    }
    ```

    It is roughly equivalent to the following code (the difference is that the scope of y in the above code is inside the x function, while the scope of x__y in the following code is the entire module containing the x function):  

    ```JavaScript
    var x__y;

    function x() {
        x__y++;
        return x__y;
    }
    ```

    The results are:

    ```JavaScript
    var a = x();  // returns 1
    var b = x();  // returns 2
    var c = x();  // returns 3
    ```

    > **warning**
    > A local variable only has static properties when you do not initialize it. If you declare a variable with var y = 0; and initialize it, the value of y will be 0 every time x is called. euddraft's behavior for initializing and not initializing variables may lead to confusion. You should not rely on this feature to write code, it is best to explicitly specify initial values for all variables. If you want a variable to have static properties, you can explicitly declare it with the static keyword and explicitly specify an initial value, even if it is 0.  

    Same applies for objects. All objects, local, global, or temporary, have fixed memory space. Consider this code for example.

    ```JavaScript
    function main() {
        const X = EUDArray(10);
        for (var i = 0 ; i < 10 ; i++) {
            X[i] = EUDArray(10);
        }
    }
    ```

    You may think that we're assigning a separate `EUDArray(10)` instance for each cell of X, but this code don't act like that.The code above is equivalent to:

    ```JavaScript
    const _t0 = EUDArray(10);  // Even intermediate values are static
    const _t1 = EUDArray(10);  // Even intermediate values are static

    function main() {
        const X = _t0;
        for (var i = 0 ; i < 10 ; i++) {
            X[i] = _t1;
        }
    }
    ```

    This is not what you might expect. All cells of X points to the same `_t1` EUDArray.  
    We need to use dynamic instantiation like `X[i] = EUDArray.alloc(10);` to assign 10 different EUDArray to each X cell. Sadly EUDArray cannot be dynamically instantiated, so it doesn't have `.alloc()`. So we cannot fix this code.  

    Although we cannot dynamically create arrays at runtime, we can still statically allocate buffers and dynamically use them at runtime. Refer to the following code:  

    ```JavaScript
    function onPluginStart() {
        const X = EUDArray(10);
        foreach (i : py_range(5)) {
            X[i] = EUDArray(10);
        }
        const X_2 = EUDArray.cast(X[2]);
        X_2[3] = 4;
        println("{}", X_2[3]);
    }
    ```

    The above code is equivalent to:  

    ```JavaScript
    function onPluginStart() {
        const X = EUDArray(10);
        X[0] = EUDArray(10);
        X[1] = EUDArray(10);
        X[2] = EUDArray(10);
        X[3] = EUDArray(10);
        X[4] = EUDArray(10);
        const X_2 = EUDArray.cast(X[2]);
        X_2[3] = 4;
        println("{}", X_2[3]);
    }
    ```

    Reference: [https://github.com/phu54321/euddraft/wiki/9B.-Appendix---Static-or-Dynamic-instantiation](https://github.com/phu54321/euddraft/wiki/9B.-Appendix---Static-or-Dynamic-instantiation)



- ### Explanation Of const And var
    The essence of const is to declare a variable at the Python (compile time), not a map runtime variable. When declaring an object, it stores the runtime address of the referenced object.  
    The essence of var is syntactic sugar for declaring a reference to an EUDVariable object, which is a map runtime variable.  
    The difference from const is that at compile time, it will compile the assignment operation `=` into the left shift operator `<<` at the Python. The left shift operator of runtime types is usually overloaded to change the value stored in the runtime object, while the const at the syntax level does not allow the use of the assignment operator `=` after declaring the initial value.  

    If you use var to declare an EUDArray, the essence is to declare an EUDVariable that stores the address of an EUDArray object. Using index operations on this variable will throw an syntax error because it is an EUDVariable rather than an EUDArray.  

    ```JavaScript
    var arr = EUDArray(10);
    arr[0] = 1; // Syntax error! arr is an EUDVariable and does not support index operators. It internally stores the address of an EUDArray object.
    const arrt = EUDArray.cast(arr); // You can do this to wrap the value of arr into an EUDArray object  
    arrt[0] = 1;
    ```

    The following illustrates the relationship between var and const. The two pieces of code are equivalent.

    ```JavaScript
    var variableName = 3;
    variableName = 4;
    variableName += 5;
    println("{}", variableName);
    ```

    ```JavaScript
    const variableName = EUDVariable(3);
    variableName.__lshift__(4);
    variableName.__iadd__(5);
    println("{}", variableName);
    ```

    Compile-time interaction that can be done in epScript

    ```JavaScript
    var v = 100;
    const i = 10;
    const s = py_str("Location ") + py_str(i);
    py_print(py_str("Compiled to const s = {}").format(s));
    py_print("v is a ", v, " object");
    ```

      



