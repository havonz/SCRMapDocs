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

    If a function is not declared, it will be declared at the location where it is implemented.


- ### Function Implementation

    ```JavaScript
    function aFunction() {
        // Codes
    }
    ```


- ### Function Parameters

    A function can have one or more parameters, separated by commas. Parameter passing and return value passing are done through runtime value type variables (EUDVariable).

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
    Function parameters and return values can set types.  
    To set the parameter type, add a colon after the parameter name and write the type name, indicating that the runtime parameter value (as a number or pointer) will be set to the specified type.   
    To set the return value type, add a colon after the closing parenthesis of the function declaration parameter list and write the type name, indicating that the returned runtime value (as a number or pointer) will be set to the specified type.  

    ```JavaScript
    function createANewUnit(player : TrgPlayer, ut : TrgUnit, loc : TrgLocation) : CUnit, TrgString {
        const cu = CUnit.from_read(EPD(0x628438));
        if (cu == 0) {
            return 0, $T("Unable to create unit");  
        }
        CreateUnit(1, ut, loc, player);
        if ( Memory(0x628438, Exactly, cu.ptr) ) {  
            return 0, $T("CreateUnit failed to create the unit, possibly incorrect parameters or the location can no longer place more units.");
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
    A function that returns multiple return values actually returns a compile-time tuple. When you do not need to get all the return values, you can use selection `[[]]` to get one or more (starting from index 0) from the returned tuple.  
    A tuple is a compile-time type, not a runtime data structure.  

    ```JavaScript
    function aFunctionWithMultipleReturnValues() {
        return 1, 2, 3, 4, 5;  
    }

    const r1 = aFunctionWithMultipleReturnValues()[[0]];

    const r4, r3 = aFunctionWithMultipleReturnValues()[[4, 3]];

    const r1, r2, r3, r4, r5 = aFunctionWithMultipleReturnValues(); // When there are enough left values to receive multiple return values, the tuple will automatically unpack
    ```
