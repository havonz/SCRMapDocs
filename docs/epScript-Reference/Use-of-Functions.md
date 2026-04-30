---
sidebar_position: 3
---

# Use of Functions

<br />

- [Functions](#functions)
    - [Declarations](#function-declarations)
    - [Implementation](#function-implementation)
    - [Parameters](#function-parameters)
    - [Return Values](#function-return-values)
    - [Parameter And Return Value Types](#parameter-and-return-value-types)
    - [Calls](#function-calls)
    - [Explanation Of Multiple Return Values](#explanation-of-multiple-return-values)

<br />

## Functions

- ### Function Declarations

    ```JavaScript
    function aFunction();
    ```

    If a function is not explicitly declared, it is implicitly declared at the point where it is defined.


- ### Function Implementation

    ```JavaScript
    function aFunction() {
        // Codes
    }
    ```


- ### Function Parameters

    A function can have one or more parameters, separated by commas. Both parameters and return values are runtime value-type variables (EUDVariable).

    ```JavaScript
    function printTwoVariableValues(parameter1, parameter2) {
        println("{}, {}", parameter1, parameter2);
    }
    ```


- ### Function Return Values

    A function can return one or more values after being called, with multiple return values separated by commas.

    ```JavaScript
    function exchange(value1, value2) {
        return value2, value1;
    }
    ```


- ### Parameter And Return Value Types
    Function parameters and return values can have types specified.  
    To specify a parameter's type, add a colon after the parameter name followed by the type name; at runtime, the parameter value (as a number or pointer) will be wrapped into the specified type.  
    To specify the return type, add a colon after the closing parenthesis of the parameter list followed by the type name; at runtime, the returned value (as a number or pointer) will be wrapped into the specified type.  

    ```JavaScript
    function createANewUnit(player : TrgPlayer, ut : TrgUnit, loc : TrgLocation) : CUnit, TrgString {
        const cu = CUnit.from_read(EPD(0x628438));
        if (cu == 0) {
            return 0, $T("Unable to create unit");  
        }
        CreateUnit(1, ut, loc, player);
        if ( Memory(0x628438, Exactly, cu.ptr) ) {  
            return 0, $T("CreateUnit failed to create the unit; the parameters may be incorrect, or the exit may be blocked.");
        }
        return cu, $T("Success");
    }

    function onPluginStart() {
        const cu, err = createANewUnit(P1, $U("Terran Marine"), $L("Location 1"));
        if (cu != 0) {
            cu.cgive(P8);
            cu.set_color(P8);
        } else {
            DisplayTextAll(err);
        }
    }
    ```

- ### Function Calls

    ```JavaScript
    aFunction(); // Directly call a function without arguments

    var a = 2;  
    var b = 3;

    a, b = exchange(a, b); // Pass arguments to call a function and get its return value

    printTwoVariableValues(a, b); // Pass arguments to call a function   
    ```

- ### Explanation Of Multiple Return Values
    A function returning multiple values actually returns a compile-time tuple. When you do not need all the return values, you can use `[[]]` to select one or more values (starting from index 0) from the returned tuple.  
    A tuple is a compile-time type, not a runtime data structure.  

    ```JavaScript
    function aFunctionWithMultipleReturnValues() {
        return 1, 2, 3, 4, 5;  
    }

    const r1 = aFunctionWithMultipleReturnValues()[[0]];

    const r4, r3 = aFunctionWithMultipleReturnValues()[[4, 3]];

    const r1, r2, r3, r4, r5 = aFunctionWithMultipleReturnValues(); // When there are enough lvalues to receive all return values, the tuple is automatically unpacked
    ```
