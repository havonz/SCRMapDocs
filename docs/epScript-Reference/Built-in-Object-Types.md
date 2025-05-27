---
sidebar_position: 6
---

# Built-in Basic Object Types

<br />

- [Basic Object Types](#basic-object-types)
    - [EUDVariable](#eudvariable)
    - [EUDLightVariable](#eudlightvariable)
    - [EUDLightBool](#eudlightbool)
    - [EUDArray](#eudarray)
    - [EUDVArray](#eudvarray)
    - [PVariable](#pvariable)
    - [EUDVArrayReader](#eudvarrayreader)
    - [EUDDeque](#euddeque)
    - [StringBuffer](#stringbuffer)
        - [StringBuffer](#stringbuffer)
        - [.insert](#insert)
        - [.append](#append)
        - [.delete](#delete)
        - [.Display](#display)
        - [.print](#print)
        - [.Play](#play)
        - [.fade](#fade)
    - [Db](#db)
    - [EUDByteStream](#eudbytestream)
    - [~~CPString~~](#cpstring)
    - [~~DBString~~](#dbstring)

<br />

## Basic Object Types

- ### EUDVariable

    It is actually the variable declared with var. It is also an object, but it is syntactically defined as a value type, so it has some differences from other object types, such as being assignable with =, etc.  
    An EUDVariable will use a virtual trigger with only one SetDeathsX action to simulate, occupying 72 bytes. Here is its type structure. It has many conditional and action functions.  
    ```JavaScript
    object EUDVariable {
        // Common methods
        function ineg(){}                // Added in euddraft 0.9.9.8, Negate variable in-place (same as x = -x;). Supports action alternative DoActions(v.ineg(action = true));
        function iabs(){}                // Added in euddraft 0.9.9.8, Self-assign absolute value in-place (same as x = (x & (1 << 31) == 0) ? x : -x;). Supports action alternative DoActions(v.iabs(action = true));

        // Common conditions
        function AtLeast(v){}               // Variable value >= v
        function AtMost(v){}                // Variable value <= v 
        function Exactly(v){}               // Variable value == v
        function AtLeastX(v,mask){}         // Variable (value & mask) >= v
        function AtMostX(v,mask){}          // Variable (value & mask) <= v
        function ExactlyX(v,mask){}         // Variable (value & mask) == v
        function MaskAtLeast(v){}           // The Mask of the SetDeathsX action of the variable's virtual trigger >= v
        function MaskAtMost(v){}            // The Mask of the SetDeathsX action of the variable's virtual trigger <= v
        function MaskExactly(v){}           // The Mask of the SetDeathsX action of the variable's virtual trigger == v
        function MaskAtLeastX(v,msk){}      // (The Mask of the SetDeathsX action of the variable's virtual trigger & msk) >= v
        function MaskAtMostX(v,msk){}       // (The Mask of the SetDeathsX action of the variable's virtual trigger & msk) <= v 
        function MaskExactlyX(v,msk){}      // (The Mask of the SetDeathsX action of the variable's virtual trigger & msk) == v

        // Common actions
        function SetNumber(v){}             // Variable value = v
        function AddNumber(v){}             // Variable value = value + v
        function SubtractNumber(v){}        // Variable value = value - v if value <= v else value = 0
        function SetNumberX(v,mask){}       // Variable value = value - (value & mask) + (v & mask)
        function AddNumberX(v,mask){}       // Variable value = value - (value & mask) + ( ((value & mask) + (v & mask)) & mask )  
        function SubtractNumberX(v,mask){}  // Variable value = value - (value & mask) + ( ((value & mask) - (v & mask)) & mask )
    
        // Compile-time constant functions
        function GetVTable(){}              // Get the runtime address of the variable's virtual trigger at compile time
        function getMaskAddr(){}            // Get the runtime address of the Mask parameter in the SetDeathsX action of the variable's virtual trigger at compile time
        function getValueAddr(){}           // Get the runtime address of the value parameter in the SetDeathsX action of the variable's virtual trigger at compile time 
        function getDestAddr(){}            // Get the runtime address of the PlayerID parameter in the SetDeathsX action of the variable's virtual trigger at compile time

        // These methods of EUDVariable are practically only usable in combination with VProc
        function SetMask(v){}               // Set the Mask of the SetDeathsX action of the variable's virtual trigger to v
        function AddMask(v){}               // Set the Mask of the SetDeathsX action of the variable's virtual trigger to Mask + v
        function SubtractMask(v){}          // Set the Mask of the SetDeathsX action of the variable's virtual trigger to Mask - v if Mask >= v else Mask = 0  
        function SetMaskX(v,msk){}          // Set the Mask of the SetDeathsX action of the variable's virtual trigger to Mask - (Mask & msk) + (v & msk)
        function AddMaskX(v,msk){}          // Set the Mask of the SetDeathsX action of the variable's virtual trigger to Mask - (Mask & msk) + ( ((Mask & msk) + (v & msk)) & msk ) 
        function SubtractMaskX(v,msk){}     // Set the Mask of the SetDeathsX action of the variable's virtual trigger to Mask - (Mask & msk) + ( ((Mask & msk) - (v & msk)) & msk )
        function SetDest(dest){}            // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to dest
        function AddDest(dest){}            // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to PlayerID + dest
        function SubtractDest(dest){}       // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to PlayerID - dest if PlayerID >= dest else PlayerID = 0
        function SetDestX(dest,mask){}      // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to PlayerID - (PlayerID & mask) + (dest & mask)
        function AddDestX(dest,mask){}      // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to PlayerID - (PlayerID & mask) + ( ((PlayerID & mask) + (dest & mask)) & mask )
        function SubtractDestX(dest,mask){} // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to PlayerID - (PlayerID & mask) + ( ((PlayerID & mask) - (dest & mask)) & mask )
        function SetModifier(modifier){}    // Set the value modification method of the SetDeathsX action of the variable's virtual trigger to modifier
        function QueueAssignTo(dest){}      // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to dest and set the value modification method to SetTo
        function QueueAddTo(dest){}         // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to dest and set the value modification method to Add      
        function QueueSubtractTo(dest){}    // Set the PlayerID of the SetDeathsX action of the variable's virtual trigger to dest and set the value modification method to Subtract. The Subtract method cannot subtract a value from positive to negative.
    };
    ```

<br />

- ### EUDLightVariable

    Light variables are different from variables declared with var. They only occupy 4 bytes of memory space. Their value passing operations consume more resources than ordinary variables. Comparing values or writing values is the same as ordinary variables and only requires executing one trigger.  
    To pass its value as an argument to other functions, you need to use the dwread function to read it.  
    If the value of an ordinary variable (EUDVariable) does not need to be passed as an argument to other functions (for example, used for counting comparisons, incrementing, decrementing, assigning, switching, etc. scenarios not associated with other functions/conditions/actions), EUDLightVariable can be used instead of the ordinary variable.  


    ```JavaScript
    object EUDLightVariable {
        // Compile-time constant functions
        function getValueAddr(){}        

        // The goals that can be achieved by the following functions, conditions and actions can optionally use EUDLightVariable, otherwise an ordinary variable (EUDVariable) should be used.
        // Common methods
        function ineg(){}                   // Negate variable in-place (same as x = -x;). Supports action alternative DoActions(v.ineg(action = true)); in euddraft 0.9.9.8 and later versions
        function iabs(){}                   // Added in euddraft 0.9.9.8, Self-assign absolute value in-place (same as x = (x & (1 << 31) == 0) ? x : -x;). Supports action alternative DoActions(v.iabs(action = true));
        // Common conditions
        function AtLeast(v){}               // Light variable value >= v
        function AtMost(v){}                // Light variable value <= v 
        function Exactly(v){}               // Light variable value == v
        function AtLeastX(v,mask){}         // (Light variable value & mask) >= v
        function AtMostX(v,mask){}          // (Light variable value & mask) <= v 
        function ExactlyX(v,mask){}         // (Light variable value & mask) == v
        // Common actions
        function SetNumber(v){}             // Light variable value = v
        function AddNumber(v){}             // Light variable value = value + v
        function SubtractNumber(v){}        // Light variable value = value - v if value <= v else value = 0
        function SetNumberX(v,mask){}       // Light variable value = value - (value & mask) + (v & mask)
        function AddNumberX(v,mask){}       // Light variable value = value - (value & mask) + ( ((value & mask) + (v & mask)) & mask )  
        function SubtractNumberX(v,mask){}  // Light variable value = value - (value & mask) + ( ((value & mask) - (v & mask)) & mask )
    };
    ```

    Example

    ```JavaScript
    const lv = EUDLightVariable(100);
    DoActions(lv.AddNumber(564)); // lv += 564;
    if (lv != 150) {
        println("{}", dwread(lv.getValueAddr())); // This process of reading the value consumes more triggers than ordinary variables, about 32 times or more
    } 
    lv.ineg(); // lv = -lv;
    if (lv > 0x80000000 && lv < -663) {
        println("Less than -663 negative number");
    }
    ```

<br />

- ### EUDLightBool
    Light Boolean (switch) variable, it uses at least 1 bit (one eighth byte) to store, prefer to use this instead of var for Boolean variables  
    In the internal implementation of eudplib, every 32 EUDLightBool share one EUDLightVariable  
    Boolean (switch) can only represent two states, 1 means Set, 0 means Cleared, the default initial value is Cleared  

    ```JavaScript
    object EUDLightBool {
        // Compile-time constant functions
        function getValueAddr() {}        

        // Regular conditions
        function IsSet(){}      // The current switch is in the set state
        function IsCleared(){}  // The current switch is in the cleared state

        // Regular actions
        function Set(){}        // Set to on
        function Clear(){}      // Set to off
        function Toggle(){}     // Toggle switch state
    };
    ```

    Example

    ```JavaScript
    const b = EUDLightBool(); // Default initial value is Cleared
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

    Light static array container, it can be declared using `[...]` syntax, the size cannot be dynamically changed.

    ```JavaScript
    object EUDArray {
        function constructor(size) {}
        const length;
    };
    ```

    Example

    ```JavaScript
    const a = EUDArray(10); // Declare an array of size 10 (index 0~9)
    a[0] = 29; // Set the element at index 0 of array a to 29

    println("Array size:{} [0] value:{}", a.length, a[0]); // Array size: 10 [0] value: 29

    const b = [3, 2, 1]; // Declare an array of size 3 (index 0~2) and initialize to b[0] = 3; b[1] = 2; b[2] = 1;

    const c = [list(3, 2, 1), 4, list(5, 6)]; // Declare an array of size 6 (index 0~5) and initialize to b[0] = 3; b[1] = 2; b[2] = 1; b[3] = 4; b[4] = 5; b[5] = 6; 
    ```

<br />

- ### EUDVArray
    Static array container implemented using virtual triggers, it can be declared using `VArray(...)` syntax, the size cannot be dynamically changed.  
    
    EUDVArray container supports the static setting of the reference type of the values it contains.    

    It has faster access speed when the array index is a variable.  

    ```JavaScript
    object EUDVArray {
        function constructor(size : py_int, basetype : type) : _EUDVArrayClass {}
    };

    object _EUDVArray {
        function constructor(vars : list) {}
        const length;
    }
    ```

    Example

    ```C#
    const a = EUDVArray(4)(list(1, 2, 3, 4)); // Declare an array of size 4 and initialize to a[0] = 1; a[1] = 2; a[2] = 3; a[3] = 4;

    const b = VArray(1, 2, 3, 4); // Same as above

    const c = VArray(list(1, 2, 3, 4)); // Same as above

    const d = VArray(list(1, 2), list(3, 4)); // Same as above

    const d = EUDVArray(4)(); // Declare an array of size 4, equivalent to VArray(0, 0, 0, 0);
    foreach (i : py_range(4)) {
        d[i] = i + 1;
    }

    // Declare a 4 x 2 two-dimensional array
    const e = EUDVArray(4, EUDVArray(2))(list(VArray(5, 6), VArray(7, 8), VArray(9, 10), VArray(11, 12)));
    var a = e[2][1]; // Supported in euddraft 0.9.9.8
    println("e[2][1] == {}", a); // e[2][1] == 10
    ```

<br />

- ### PVariable

    Player variable, which is actually another representation of `EUDVArray(8)()`, that is, an array that stores different values for each player, with a maximum of 8 players in StarCraft.  

    ```JavaScript
    object PVariable {
        const length;
    };
    ```

    Example

    ```JavaScript
    // They are equivalent
    const pv1 = PVariable();
    const pv2 = EUDVArray(8)();
    ```

<br />

- ### EUDVArrayReader

    For traversing EUDVArray

    ```JavaScript
    object EUDVArrayReader {
        function seek(varr_ptr, varr_epd, eudv, acts) {}
        function read(acts) {}
    }
    ```

<br />

- ### EUDDeque

    Static double-ended queue container implemented using virtual triggers, EUDDeque is a runtime iterator type.  
  
    ```JavaScript
    object EUDDeque {
        function constructor(size : py_int, basetype : type) : _EUDDequeClass {}
    };

    object _EUDDeque {
        function constructor() {}
        function append(arg) {}
        function appendleft(arg) {}
        function pop() {}
        function popleft() {}
        function clear() {}
        function empty() {}
        const length;
    };
    ```

    Example

    ```JavaScript
    const dq = EUDDeque(10)();

    // `.length` : Get the current number of elements in the deque
    println("Number of elements in deque {}", dq.length);

    // `.append(x)` : Add x to the far right of the deque
    dq.append(10);

    // `.pop()` : Pop the element at the far right of the deque (remove and return), you need to determine if there are elements in the deque first, if there are no elements inside, the behavior of using this method directly is undefined.
    println("The value at the far right of the deque pops out {}", dq.pop());

    // `.appendleft(x)` : Add x to the far left of the deque
    dq.appendleft(13);

    // `.popleft()` : Pop the element at the far left of the deque (remove and return), you need to determine if there are elements in the deque first, if there are no elements inside, the behavior of using this method directly is undefined.
    println("The value at the far left of the deque pops out {}", dq.popleft());

    // `.clear()` : Clear the deque
    dq.clear();

    // `.empty()` : Determine if the number of elements in the current deque is 0
    if (dq.empty()) {
        println("Deque is empty");
    }

    ```

<br />

- ### StringBuffer

    Static in-memory string buffer operation type

    ```JavaScript
    object StringBuffer {
        function constructor(content : py_str | py_bytes) {}
        function constructor(len : py_int) {}
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

    Except for the initialization method, all methods of the StringBuffer object are asynchronous methods and only take effect on the machine where `Current Player == Local Player`.

    ```JavaScript
    const buf = StringBuffer(64); // Initialize buffer size

    setcurpl(P1); // Set current player to P1
    buf.insert(0, "Information displayed to player 1");  // This line will only modify buf on P1's machine because the current player is P1

    if (getuserplayerid() == $P2) {     // The local player is P2
        buf.insert(0, "This line of code is useless"); // The local player is P2 but the current player is P1 so this line of code does not work
    }

    setcurpl(P2); // Set current player to P2
    buf.insert(0, "Information displayed to player 2");  // This line will only modify buf on P2's machine because the current player is P2

    setcurpl(P1);
    buf.Display(); // Display "Information displayed to player 1" to player 1

    setcurpl(P2);
    buf.Display(); // Display "Information displayed to player 2" to player 2
    ```

- #### StringBuffer

    - `StringBuffer`(content)  
        If [content] is a string or byte string, a StringBuffer object is initialized with that string or byte string.  
        If [content] is an integer, a StringBuffer object is initialized with [content] as the size.  
        [content] is an optional parameter, the default is 218.  

    ```JavaScript
    const s1 = StringBuffer();         // StringBuffer object with size 218, initial content is 218 * \r
    const s2 = StringBuffer(64);       // StringBuffer object with size 64, initial content is 64 * \r
    const s3 = StringBuffer("havonz"); // StringBuffer object with size 6, initial content is "havonz"
    ```


- #### .insert

    - `.insert`(index, *args)  
        Convert the variable arguments [*args] into strings and insert them in order into the buffer of the `current player`'s machine StringBuffer object at position `[index] * 4` (this cannot be used if the index is not a multiple of 4).
    
    - `.insertf`(index, format_string, *args)  
        Format the variable arguments [*args] using [format_string] and insert into the buffer of the `current player`'s machine StringBuffer object at position `[index] * 4` (this cannot be used if the index is not a multiple of 4).


    ```JavaScript
    const s1 = StringBuffer();
    s1.insert(0, "havonz");
    s1.insert(1, "0");
    s1.Display(); // havo0
    ```


- #### .append

    - `.append`(*args)  
        Convert the variable arguments [*args] into strings and append them in order to the end of the string in the buffer of the `current player`'s machine StringBuffer object.
    
    - `.appendf`(format_string, *args)  
        Format the variable arguments [*args] using [format_string] and append to the end of the string in the buffer of the `current player`'s machine StringBuffer object.

    ```JavaScript
    const s1 = StringBuffer();
    setcurpl(getuserplayerid());
    s1.insert(0);
    s1.append("Hello!");
    s1.appendf("{}{}", PColor(getuserplayerid()), PName(getuserplayerid()) );
    s1.Display();
    ```


- #### .delete

    - `.delete`(start, length=1)  
        Delete `[length] * 4` bytes from the `[start] * 4` index position of the StringBuffer object on the `current player`'s machine (this cannot be used if the index is not a multiple of 4).  


- #### .Display

    - `.Display()`  
        Print the string in the StringBuffer buffer to the bottom line of the scrolling message on the `current player`'s screen.  

    - `.DisplayAt`(line)  
        Print the string in the StringBuffer buffer to the [line]th line from top to bottom of the scrolling message on the `current player`'s screen.  

    ```JavaScript
    const s1 = StringBuffer();
    setcurpl(getuserplayerid());
    s1.insert(0, "Hello! StarCraft");
    s1.DisplayAt(0); // Output to the top line
    s1.Display();    // Output to the bottom line
    ```


- #### .print

    - `.print`(*args)  
        Use the current StringBuffer to print multiple arguments [*args] sequentially to the next line of the scrolling message on the `current player`'s screen, scrolling the bottom message up.  

    - `.printf`(formatstring, *args)  
        Use the current StringBuffer to format print multiple arguments [*args] to the next line of the scrolling message on the `current player`'s screen using the [format_string] format, scrolling the bottom  

    - `.printfAt`(line, formatstring, *args)  
        Use the current StringBuffer to format print multiple arguments [*args] to the [line]th line (range 0~10) from top to bottom of the scrolling message on the `current player`'s screen using the [format_string] format.  

    ```JavaScript
    const s1 = StringBuffer();
    setcurpl(getuserplayerid());
    s1.print("Hello! StarCraft"); // Scroll out a message at the bottom
    s1.printf("Hello!{}{}", PColor(getuserplayerid()), PName(getuserplayerid()) ); // Scroll up the previous message and scroll out this message
    s1.printfAt(0, "Hello!{}{}", PColor(getuserplayerid()), PName(getuserplayerid()) ); // Print this message from the top line
    ```


- #### .Play

    - `.Play()`  
        Use the content of the StringBuffer object on the `current player`'s machine as a sound file name and play that sound file.  
        When the target sound file contains localized sounds, the dynamically concatenated file name using StringBuffer will be unable to play.  

    ```JavaScript
    setcurpl(P1);
    buf.insert(0, "sound\\Zerg\\Devourer\\");
    buf.append("ZDvPss00.WAV\0");
    buf.Display();    // Output "sound\Zerg\Devourer\ZDvPss00.WAV" on the next line of the screen of player 1
    buf.DisplayAt(9); // Output "sound\Zerg\Devourer\ZDvPss00.WAV" on the tenth line of the screen of player 1 
    buf.Play();       // Find the wav pointed to by the text and play it on player 1's computer

    StringBuffer("sound\\terran\\advisor\\tadupd04.wav").Play(); // nuclear launch detected.
    ```


- #### .fade

    - `.fadeIn`(*args, line=0, color=None, wait=1, reset=true, tag=None)  
        Make [*args] combine into a text gradually appearing from [line] line in [clolor] color, with [wait] frames interval, whether to reset [reset], special effect text tag [tag], call repeatedly, return non-0 means the special effect is not completed and needs to continue calling, return 0 means the special effect is completed.  

    - `.fadeOut`(*args, line=0, color=None, wait=1, reset=true, tag=None)  
        Make [*args] combine into a text gradually disappearing from [line] line in [clolor] color, with [wait] frames interval, whether to reset [reset], special effect text tag [tag], call repeatedly, return non-0 means the special effect is not completed and needs to continue calling, return 0 means the special effect is completed.  

    - `.fadeInf`(format_string, *args, line=0, color=None, wait=1, reset=true, tag=None)  
        Make [*args] format into a text using [format_string] gradually appearing from [line] line in [clolor] color, with [wait] frames interval, whether to reset [reset], special effect text tag [tag], return non-0 means the special effect is not completed and needs to continue calling, return 0 means the special effect is completed.  

    - `.fadeOutf`(format_string, *args, line=0, color=None, wait=1, reset=true, tag=None)  
        Make [*args] format into a text using [format_string] gradually disappearing from [line] line in [clolor] color, with [wait] frames interval, whether to reset [reset], special effect text tag [tag], return non-0 means the special effect is not completed and needs to continue calling, return 0 means the special effect is completed.  

    ```JavaScript
    function fadeInAndFadeOutTextOnce() {
        const buf = StringBuffer(128);
        const onceWait = EUDLightVariable(0);

        if (getcurpl() != getuserplayerid()) {
            return;
        }

        if (onceWait >= 10000) {
            return;
        }

        const text = py_str("\x13\x04lose\x19humanity\n\x13\x04lose\x19a lot\n\x13\x04lose\x19beast\n\x13\x04lose\x19all\n"); 

        const tecolor = 4, 2, 0x1E, 5, 0;

        if (onceWait <= 0) {
            if ( 0 != buf.fadeIn(text, line = 3, color = tecolor, wait = 2, tag = py_str("fadeInEff")) ) {
                return;
            }
        }

        if (onceWait <= 100) {
            DoActions(onceWait.AddNumber(1));
            return;
        }

        TextFX_SetTimer("fadeInEff", SetTo, 0);
        TextFX_Remove("fadeInEff");

        if ( 0 != buf.fadeOut(text, line = 3, color = tecolor, wait = 2, tag = py_str("fadeOutEff")) ) {
            return;
        }

        TextFX_SetTimer("fadeOutEff", SetTo, 0);
        TextFX_Remove("fadeOutEff");
        DoActions(onceWait.SetNumber(10000)); /* Done */
    }

    function beforeTriggerExec() {
        const cp = getcurpl();

        setcurpl(getuserplayerid());
        fadeInAndFadeOutTextOnce();

        setcurpl(cp);
    }
    ```


<br />

- ### Db

    Static memory bytes object type

    ```JavaScript
    object Db {
    function constructor(content) {}
    function GetDataSize() {}
    };
    ```

    Support initializing a memory byte data using integers, strings, bytes

    `Db("string")` is equivalent to `Db(b"string\0")`(UTF-8)

    ```JavaScript
    const buf1 = Db(b"string\0"); // Db(b"string\0")
    const buf2 = Db("string");    // Db(b"string\0")
    const buf3 = Db(5);           // Db(b"\0\0\0\0\0")
    ```

<br />

- ### EUDByteStream

    Memory byte stream operation object type

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

    **Deprecated**
    CPTricks optimized string buffer operation object type

    ```JavaScript
    object CPString {
    function constructor(content) {}
    function Display() {}
    function GetVTable() {}
    };
    ```

    ```JavaScript
    const s1 = CPString("a string");
    const s2 = CPString(b"stringstringstring");
    const s3 = CPString(64);
    ```

<br />

- ### ~~DBString~~

    **Deprecated**
    Static memory string object type

    ```JavaScript
    object DBString {
    function constructor(content) {}
    function GetStringMemoryAddr() {}
    function Display() {}
    function Play() {}
    };
    ```

    ```JavaScript
    const s = DBString("a very long string\0a very long string");
    const buf = s.GetStringMemoryAddr();
    s.Display();
    sprintf(buf, "908 + 8 = {}", 908 + 8);
    s.Display();
    ```

    

  

  

  





