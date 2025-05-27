---
sidebar_position: 1
---

# epScript Syntax

<br />

- [Basic Syntax](#basic-syntax)
    - [Compile-time And Run-time](#compile-time-and-run-time)
    - [Case Sensitivity](#case-sensitivity)
    - [Value Types](#value-types)
    - [Logical Rules](#logical-rules)
    - [Literal Numbers](#literal-numbers)
    - [Literal Strings](#literal-strings)
    - [Literal Bytes](#literal-bytes)
    - [Naming Rules](#naming-rules)
    - [Importing Other Modules](#importing-other-modules)
    - [Symbols](#symbols)
        - [Code Block {}](#code-block)
        - [Syntax Line Break \;](#syntax-line-break)
        - [Index Operator \[\]](#index-operator)
        - [Assignment Operator \=](#assignment-operator)
        - [Line Comment //](#line-comment)
        - [Block Comment /\* \*/](#block-comment)
        - [Conditional Operator \> \<](#conditional-operator)
        - [Mathematical Operators \+ \- \* \/](#mathematical-operators)
        - [Increment/Decrement/Multiplication/Division Assignment Operators](#incrementdecrementmultiplicationdivision-assignment-operators)
    - [Conditional Statement Syntax](#conditional-statement-syntax)
        - [if](#if)
        - [if else](#if-else)
        - [Conditional Chaining](#conditional-chaining)
        - [Conditional Nesting](#conditional-nesting)
        - [Once Execution](#once-execution)
    - [Control Flow](#control-flow)
        - [for Loop](#for-loop)
        - [while Loop](#while-loop)
        - [break](#break)
        - [foreach Iterator Loop](#foreach-iterator-loop)
        - [switch](#switch)
        - [epdswitch](#epdswitch)

<br />

## Basic Syntax

- ### Compile-time And Run-time

    Any non-compile-time code outside of variable declarations cannot be exposed outside of functions.  
    Functions containing any runtime operations are non-compile-time functions.  
    All variable declaration/initialization/reading/operation/assignment operations are runtime operations.   
    Compile-time code will execute even if the if condition is not met.   


- ### Case Sensitivity

    epScript is a case-sensitive programming language where A and a have different meanings.   


- ### Value Types

    epScript has only one basic `value type`, which is a 32-bit unsigned integer. 


- ### Logical Rules

    Integer `0` is logically false.  
    `Non-zero` integer values are logically true.  


- ### Literal Numbers

    Literal numbers include decimal numbers, hexadecimal numbers, and binary numbers.
    ```JavaScript
    // The following 15, 0xf, and 0b1111 represent the same number 15, just in different ways,
    // so a, b and c are equal.
    var a = 15;
    var b = 0xf; // Hexadecimal numbers start with 0x
    var c = 0b1111; // Binary numbers start with 0b
    ```


- ### Literal Strings

    Literal strings can be enclosed in single quotes `'` or double quotes `"`.  
    ```JavaScript
    DisplayText("This is a literal string");
    DisplayText('This is a literal string');
    DisplayText("This is a literal \
    string"); // // When a string is too long, use a backslash \ to continue the literal string on the next line. This does not indicate inserting a line break in the string. The above two ways of writing are completely equivalent.
    ```
    Literal strings support using backslash escape characters
    |Description|Explanation|Example|Result|
    |-|-|-|-|
    |\\\\ |Denotes \\ |`DisplayText("Hello\\SC");`|Hello\SC|
    |\octal|Denotes a octal ASCII code|`DisplayText("SC\101\102\103");`|SCABC|
    |\xhex|Denotes a hex ASCII code|`DisplayText("SC\x41\x42\x43");`|SCABC|
    |\\`newline`|Indicates continuing the line without a line break|`DisplayText("Hello\`<br />`SC");`|HelloSC|
    |\\n|Inserts a line break, equivalent to \x0A|`DisplayText("Hello\nSC");`|Hello<br />SC|
    |\\t|Inserts a tab, equivalent to \x09|`DisplayText("Hello\tSC");`|Hello&emsp;SC|
    |\\r|Inserts a return, equivalent to \x0D, no effect in the game|`DisplayText("Hello\rSC");`|HelloSC|
    |\\"|Denotes a double quote in a double quote string|`DisplayText("Hello\"SC\"");`|Hello"SC"|
    |\\'|Denotes a single quote in a single quote string|`DisplayText('Hello\'SC\'');`|Hello'SC'|


- ### Literal Bytes

    Literal bytes can be enclosed in `b"` to `"` or `b'` to `'`. Bytes will not end with `\0`. Literal bytes also support the escape characters in literal strings.  
    ```JavaScript
    println("{}", b"ASCII\nliteral");
    println("{}", b'ASCII\nliteral');
    ```


- ### Naming Rules

    Variable names, constant names and function names can only contain `non-ASCII UTF-8 characters` (Chinese, Japanese and Korean can be used),
    [ASCII](https://en.wikipedia.org/wiki/ASCII) letters/numbers and `_`, and cannot start with ASCII numbers.
    
    ```JavaScript
    // Legal variable names
    var a;
    var a_b;
    var a_1;
    var _a1;
    var 这也行;

    // Illegal variable names
    var 3y = 1;
    var abc# = 2;
    ```

    Variable names, constant names and function names cannot be named with keywords. I'm not sure which are keywords, but these are:  

    ```C#
    static var const object this function 
    return for foreach while switch epdswitch 
    break continue if else once import as 
    true false 
    ```

    Variable names and constant names cannot be named with Python 3 keywords:

    ```Python
    False None True 
    and as assert break class continue 
    def del elif else except finally 
    for from global if import in 
    is lambda nonlocal not or pass 
    raise return try while with yield 
    ```

    In addition, when a function name starts with `py_`, you have to call it with `py_py_` to call it.

    ```JavaScript
    function py_FuncName() {
    }
    function onPluginStart() {
        py_py_FuncName();
    }
    ```


- ### Importing Other Modules

    You can use the `import` keyword to import other modules. `as` gives an alias to the imported module. The following code illustrates the usage:  

    `Module1.eps`:
    ```JavaScript
    const A_CONST = 0; 
    static var A_VAR = 0;   

    function *A_FUNC*() {
        A_VAR++;
        return A_VAR;
    }
    ```

    `Module2.eps`:
    ```JavaScript
    import Module1;  

    function MODULE2_FUNC() {  
        return Module1.A_FUNC();  
    }
    ```

    `Module3.eps`:
    ```JavaScript
    import Module1 as m1; 
    import Module2 as m2;   

    function onPluginStart() {
        println("{}", m1.A_CONST);  
        m2.MODULE2_FUNC();  
    }
    ```


- ### Symbols

    All syntax-related symbols are half-width symbols in pure English state and can be found in the [ASCII](https://en.wikipedia.org/wiki/ASCII) table.  

    - #### Code Block

        Use curly braces `{}` to denote a code block in epScript.  

        ```JavaScript
        {
            // codes
        }
        ```

        If you want a single statement to be in a code block, you can omit the curly braces. The following example is valid:  

        ```JavaScript
        function ANNOYING_EXAMPLE_FUNC()
            for (var i = 1; i < 10; i++)
                if (i < 5)
                    println("{} < 5", i);
                else if (i == 5)
                    println("{} = 5", i);
                else
                    println("{} > 5", i);
        ```

    - #### Syntax Line Break

        The syntax line break is a semicolon `;` instead of a line break.

        ```JavaScript
        var a;var b;
        ```

    - #### Index Operator
        `[]`
        Used to access or modify elements in an array.  

        ```JavaScript
        const a = EUDArray(10);
        a[0] = 11;
        var b = a[0];
        ```

    - #### Assignment Operator

        The assignment operator is a single equal sign `=`  

        ```JavaScript
        var a; // Declare variable a  
        var b = 3; // Declare variable b and assign it to 3
        a = 2; // Assign variable a to 2 
        ```

    - #### Line Comment

        Two slashes `//` starts a line comment.

        ```JavaScript
        var a = 1; // Comments are parts of the code that will not be executed. // starts a comment that continues to the end of the current line.
        ```

    - #### Block Comment

        The content between `/*` to `*/` is a block comment.  

        ```JavaScript
        var /* This is a block comment, which can be placed in the middle of code */ a = 1;
        ```

    - #### Conditional Operator

        - Greater than `>`
        - Less than `<`
        - Greater than or equal to `>=`
        - Less than or equal to `<=`
        - Equal to `==`
        - Logical and `&&`
        - Logical or `||` 

        It is worth mentioning that when using conditional comparison operators on variables, what is returned is the `conditional expression` itself, not the `conditional expression result`.  
        The parameters of if are a list of `conditional expression`s. If a variable is passed directly to if, if will convert it into an expression that checks if its runtime value is not equal to 0.  

        ```JavaScript
        if (a == 2) // Double equals sign for logical equal 
            single_sentence;   

        if (a == 2) {
            // a is equal to 2
            single_sentence1; 
            single_sentence2; 
        }

        if (a != 2) { 
            // a is not equal to 2
        }

        if (a > 2) {  
            // a is greater than 2
        }

        if (a < 2) {    
            // a is less than 2 
        }

        if (a >= 2) {     
            // a is greater than or equal to 2
        }

        if (a <= 2) {      
            // a is less than or equal to 2  
        } 

        if (a > 2 && a < 10) {   
            // a is greater than 2 and less than 10
        }

        if (a > 10 || a <= 5) { 
            // a is greater than 10 or less than or equal to 5  
        }

        if ( ! (a < 2) ) {    
            // a is not less than 2
        }

        var a = 3;
        var b = a > 0;       // Error! It returns not true but the conditional expression a > 0 itself
        var c = l2v(a > 0);  // This is correct. At runtime, c will be equal to either true or false, depending on the runtime state of a.
        const d = a > 0;     // Here, d represents the conditional expression a > 0 itself, not the result of a > 0
        var e = l2v(d);      // At runtime, use l2v to assign the result of expression d to e
        ```

        - Logical not `!`  
        
        Using `!` on the variable `a` will return a variable whose value is the result of `l2v((a != 0) == 0)`.  
        Using double consecutive `!` on the variable `a`, such as `!!a` or `!(!a)` or `!!!(!a)` is directly equal to `a` itself.  
        Using `!` on a `constant` or a `conditional expression` will still return a `conditional expression`.  

        ```js
        var four = 4;
        var b = !four; // b == 0
        if (!b   != !!four) println("b is not !!four");
        if (four == !!four) println("four is !!four");
        ```

    - #### Mathematical Operators

        `+` `-` `*` `/`

        ```JavaScript
        a = a + 1;  
        a = a - 1;
        a = a * 2;
        a = a / 2;  // Integer division operator, rounding down  
        a = a % 2;  // Modulus operator   
        a = a ** 3; // This is the exponentiation operator, returns a to the power of 3
        a = a << 1; // Left bitwise shift by 1  
        a = a >> 1; // Right bitwise shift by 1
        ```

    - #### Increment/Decrement/Multiplication/Division Assignment Operators

        ```JavaScript
        a += 10; // Equivalent to a = a + 10;  
        a -= 10; // Equivalent to a = a - 10;  
        a++;     // Equivalent to a = a + 1;  
        a--;     // Equivalent to a = a - 1;
        a *= 10; // Equivalent to a = a * 10;
        a /= 10; // Equivalent to a = a / 10;
        ```


- ### Conditional Statement Syntax

    - #### if

        The syntax of if is  

        ```JavaScript
        if (conditional expression) {
            Execute when the conditional expression is true;
        }
        ```

    - #### if else

        The else branch of if  

        ```JavaScript
        if (conditional expression) {
            Execute when the conditional expression is true; 
        } else {
            Execute when the conditional expression is false;
        }
        ```

    - #### Conditional Chaining

        You can chain if to another else  

        ```JavaScript
        if (conditional expression 1) {
            Execute when conditional expression 1 is true;
        } else if (conditional expression 2) {
            Execute when conditional expression 1 is false and conditional expression 2 is true;
        } else {
            Execute when conditional expression 1 and conditional expression 2 are both false; 
        }
        ```

    - #### Conditional Nesting

        You can nest if within the code block of another if

        ```JavaScript
        if (conditional expression 1) {
            Execute when conditional expression 1 is true;
        } else if (conditional expression 2) {
            if (conditional expression 3) {
                Execute when conditional expression 1 is false, conditional expression 2 and conditional expression 3 are both true;
            } else {
                Execute when conditional expression 1 and conditional expression 3 are false, and conditional expression 2 is true; 
            }
        } else if (conditional expression 4) {
            Execute when conditional expression 1 and conditional expression 2 are false, and conditional expression 4 is true;
        } else {
            Execute when conditional expression 1 and conditional expression 2 are both false;
        } 
        ```

    - #### Once Execution

        As the name suggests, once execution will execute its internal code only once when the condition is met during runtime, and will not execute again. It is usually used to repeatedly determine in beforeTriggerExec or afterTriggerExec for each frame until the condition is met and executed once.  

        ```JavaScript
        once (condition expression) { // In a situation where the once code block is repeatedly running during runtime, it will only run the code inside it once when the conditional expression is satisfied. 
            // Codes
        }

        once { // Unconditionally execute the code inside it only once
            // Codes
        }

        // The following code will only print 0 once
        for (var i = 0; i < 100; i++) {
            once {
                println("{}", i);
            }
        }

        // The following code will only trigger once when any area/location between Location 1 to Location 10 has at least 1 Terran Marine, instead of triggering 10 times when entering each area/location separately. 
        for (var i = $L("Location 1"); i <= $L("Location 10"); i++) {
            once ( Bring(P1, AtLeast, 1, "Terran Marine", i) ) {
                println("Terran Marine has entered location {}", i);
            }
        }
        ```


- ### Control Flow

    - #### for Loop

        The for loop can set loop initialization expression, loop execution condition expression and loop additional action expression.  

        ```JavaScript
        for (initialization expression; loop execution condition expression; loop additional action expression) {
            Code in the loop;
        }

        for (var i = 0; i < 10; i++) {
            println("{}", i);
        }
        // Briefly, the above code declares a counter variable i, initially 0, and keeps looping as long as i < 10, incrementing i by 1 each time, and the scope of i is within the braces, which is the content that needs to be executed each time the loop iterates.

        var i1, i8;
        for (i1, i8 = 0, 0 ; i1 < 10 && i8 < 80 ; i1++, i8 += 8) {
            printAll("{} x 8 = {}", i1, i8);
        }
        ```

    - #### while Loop

        The while loop can set a loop condition, and will keep looping as long as the condition is satisfied, until the condition is not satisfied.

        ```JavaScript
        var i = 0;
        while (i < 10) {
            println("{}", i);
            i++;
        }
        ```

    - #### break

        You can use break to exit a running loop or switch statement.

        ```JavaScript
        var i = 0;
        while (true) { // Sets an always true condition, that is, always returns true
            println("{}", i);
            if (i >= 10) {
                break;
            }
        }
        ```

        It is equivalent to the while loop above.

    - #### foreach Iterator Loop

        **Compile-time Iterators**  
        py_range and py_enumerator are compile-time iterators.  
        Python containers (list, tuple, etc.) are compile-time iterators.  
        When using compile-time iterators, foreach is a compile-time loop that will be statically expanded at compile time and cannot use break and continue.  
        ```C#
        foreach (i : py_range(5)) {
            simpleprint(i + 1);
        }
        // Equivalent to the following code
        simpleprint(0 + 1);
        simpleprint(1 + 1);
        simpleprint(2 + 1);
        simpleprint(3 + 1);
        simpleprint(4 + 1);
        ```

        ```C#
        foreach (i : py_range(3)) {
            once (ElapsedTime(AtLeast, i)) {
                println("The {} secound(s)", i);
            }
        }
        // Equivalent to the following code
        once (ElapsedTime(AtLeast, 0)) {
            println("The {} secound(s)", 0);
        }
        once (ElapsedTime(AtLeast, 1)) {
            println("The {} secound(s)", 1);
        }
        once (ElapsedTime(AtLeast, 2)) {
            println("The {} secound(s)", 2);
        }
        ```

        ```C#
        const arrs = py_list();
        foreach (x : list(50, 100, 150)) {
            arrs.append(EUDArray(x));
        }
        // Equivalent to the following code
        const arrs = py_list();
        arrs[0] = EUDArray(50);
        arrs[1] = EUDArray(100);
        arrs[2] = EUDArray(150);
        ```

        **Runtime Iterators**  
        Iterator functions named EUDLoop* return runtime iterators.  

        > EUDLoopPlayer, EUDLoopRange, EUDLoopUnit, EUDLoopUnit2, EUDLoopCUnit, EUDLoopNewUnit, EUDLoopNewCUnit, EUDLoopPlayerUnit, EUDLoopPlayerCUnit  

        Secondly, EUDQueue, EUDDeque containers are also runtime iterators. UnitGroup.cploop also returns a runtime iterator.  

        EUDDeque example
        ```C#
        // dq3 is a size 3 EUDDeque
        const dq3 = EUDDeque(3)();
        const ret = EUDCreateVariables(6);

        // If dq3 is empty, no code will be executed at runtime
        foreach(v : dq3) {
            ret[0] += v;
        }

        // Add 1 and 2 to the right side of dq3 EUDDeque
        dq3.append(1);  // dq3 : (1)
        dq3.append(2);  // dq3 : (1, 2)
        foreach(v : dq3) {
            ret[1] += v; // 3 = 1 + 2
        }

        // Add 3 and 4 to the right side of dq3 EUDDeque
        dq3.append(3);  // dq3 : (1, 2, 3)
        dq3.append(4);  // dq3 : (2, 3, 4)
        foreach(v : dq3) {
            ret[2] += v; // 9 = 2 + 3 + 4
        }

        // Add 5 to the right side of dq3 EUDDeque
        dq3.append(5);  // dq3 : (3, 4, 5)
        foreach(v : dq3) {
            ret[3] += v; // 12 = 3 + 4 + 5
        }

        // Pop one value from the left side, here 3 is popped
        const three = dq3.popleft();  // dq3 : (4, 5)  
        foreach(v : dq3) {
            ret[4] += v; // 9 = 4 + 5
        }

        // Add 6 and 7 to the right side of dq3 EUDDeque
        dq3.append(6);  // dq3 : (4, 5, 6)
        dq3.append(7);  // dq3 : (5, 6, 7)
        foreach(v : dq3) {
            ret[5] += v; // 18 = 5 + 6 + 7
        }
        ```

    - #### switch

        Conditional branching for multiple states of a single runtime variable value.  

        **Normal Switch**
        ```JavaScript
        switch (day) {
            case 1: 
                DisplayText("The bitter working days begin");
                break;  
            case 4:
            case 5:
                DisplayText("The weekend is coming soon");
                break;   
            case 0, 6:
                DisplayText("So cool!");  
                break;
            default:
                DisplayText("Looking forward to the weekend");
        }

        // The above switch code can be viewed as the following if conditional branching code

        if (day == 1) {
            DisplayText("The bitter working days begin");
        } else if (day == 4 || day == 5) {
            DisplayText("The weekend is coming soon");  
        } else if (day == 0 || day == 6) { 
            DisplayText("So cool!");
        } else {
            DisplayText("Looking forward to the weekend");
        }
        ```

        **Switch with Bitmask**
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

    - #### epdswitch

        Conditional branching for multiple states of a single runtime memory address value.

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

      



