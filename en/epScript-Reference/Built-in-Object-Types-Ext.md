# Built-in Extended Object Types

Object types related to game content  

Reference: [https://cafe.naver.com/edac/120138](https://cafe.naver.com/edac/120138)


- [Extended Object Types](#extended-object-types)
    - [CUnit](#cunit)
    - [UnitGroup](#unitgroup)
    - [CSprite](#csprite)


## Extended Object Types

- ### CUnit

    EPDCUnitMap is another way to write CUnit  
    Unit instance operation object, which can operate a specific unit on the map. In the editor, Unit actually refers to the unit type instead of the unit instance.  
    CUnit is a reference type, and the unit instance it operates on belongs to `data that needs to be synchronized`.  

    ```JavaScript
    object CUnit {
    function constructor(epd) {}
    static function from_read(epd) {}
    static function from_ptr(ptr) {}
    
        function set_color(Player : TrgPlayer){}
        function reset_buildq(){}
        function setloc(Location : TrgLocation){}
        function is_burrowed(){}
        function is_in_transport(){}
        function is_in_building(){}
        function is_air(){}
        function is_hallucination(){}
        function is_completed(){}
        function is_dying(){}
        function die(){}
        function set_speed_upgrade(){}
        function clear_speed_upgrade(){}
        function set_air(){}
        function set_ground(){}
        function set_invincible(){}
        function clear_invincible(){}
        function remove_collision(){}
        function set_hallucination(){}
        function clear_hallucination(){}
        function power(){}
        function unpower(){}
        function set_noclip(){}
        function clear_noclip(){}
        function set_gathering(){}
        function clear_gathering(){}
        function check_status_flag(Value){}
        function check_status_flag(Value, Mask){}
        function set_status_flag(Value){}
        function set_status_flag(Value, Mask){}
        function clear_status_flag(Value){}

        var prev;// CUnitMember(0x000)
        var next;// CUnitMember(0x004)  //link
        var hp;// Member(0x008, MemberKind.DWORD)  //displayed value is ceil(healthPoints/256)
        var sprite;// CSpriteMember(0x00C)
        var moveTargetPos;// Member(0x010, MemberKind.POSITION)
        var moveTargetX;// Member(0x010, MemberKind.POSITION_X)
        var moveTargetY;// Member(0x012, MemberKind.POSITION_Y)
        var moveTarget;// CUnitMember(0x014)
        var moveTargetUnit;// CUnitMember(0x014)
        var nextMovementWaypoint;// Member(0x018, MemberKind.POSITION)  //The next way point in the path the unit is following to get to its destination. Equal to moveToPos for air units since they don't need to navigate around buildings.
        var nextTargetWaypoint;// Member(0x01C, MemberKind.POSITION)  //The desired position
        var movementFlags;// MovementFlags(0x020, MemberKind.BYTE)
        var currentDirection1;// Member(0x021, MemberKind.BYTE)  //current direction the unit is facing
        var turnRadius;// Member(0x022, MemberKind.BYTE)  //flingy
        var velocityDirection1;// Member(0x023, MemberKind.BYTE)  //usually only differs from the currentDirection field for units that can accelerate and travel in a different direction than they are facing. For example Mutalisks can change the direction they are facing faster than then can change the direction they are moving.
        var flingyID;// Member(0x024, MemberKind.FLINGY)
        var unknown0x26;// Member(0x026, MemberKind.BYTE)
        var flingyMovementType;// Member(0x027, MemberKind.BYTE)
        var pos;// Member(0x028, MemberKind.POSITION)  //Current position of the unit
        var posX;// Member(0x028, MemberKind.POSITION_X)
        var posY;// Member(0x02A, MemberKind.POSITION_Y)
        var haltX;// Member(0x02C, MemberKind.DWORD)
        var haltY;// Member(0x030, MemberKind.DWORD)
        var topSpeed;// Member(0x034, MemberKind.DWORD)
        var currentSpeed1;// Member(0x038, MemberKind.DWORD)
        var currentSpeed2;// Member(0x03C, MemberKind.DWORD)
        var currentVelocityX;// Member(0x040, MemberKind.DWORD)
        var currentVelocityY;// Member(0x044, MemberKind.DWORD)
        var acceleration;// Member(0x048, MemberKind.WORD)
        var currentDirection2;// Member(0x04A, MemberKind.BYTE)
        var velocityDirection2;// Member(0x04B, MemberKind.BYTE)  //pathing related
        var playerID;// Member(0x04C, MemberKind.TRG_PLAYER)
        var owner;// Member(0x04C, MemberKind.TRG_PLAYER)
        var orderID;// Member(0x04D, MemberKind.UNIT_ORDER)
        var order;// Member(0x04D, MemberKind.UNIT_ORDER)
        var orderState;// Member(0x04E, MemberKind.BYTE)
        var orderSignal;// Member(0x04F, MemberKind.BYTE)
        var orderUnitType;// Member(0x050, MemberKind.TRG_UNIT)
        var unknown0x52;// Member(0x052, MemberKind.WORD)  //2-byte padding
        var cooldown;// Member(0x054, MemberKind.DWORD)
        var orderTimer;// Member(0x054, MemberKind.BYTE)
        var gCooldown;// Member(0x055, MemberKind.BYTE)
        var aCooldown;// Member(0x056, MemberKind.BYTE)
        var spellCooldown;// Member(0x057, MemberKind.BYTE)
        var groundWeaponCooldown;// Member(0x055, MemberKind.BYTE)
        var airWeaponCooldown;// Member(0x056, MemberKind.BYTE)
        var orderTargetPos;// Member(0x058, MemberKind.POSITION)  //ActionFocus
        var orderTargetXY;// Member(0x058, MemberKind.POSITION)
        var orderTargetX;// Member(0x058, MemberKind.POSITION_X)
        var orderTargetY;// Member(0x05A, MemberKind.POSITION_Y)
        var orderTarget;// CUnitMember(0x05C)
        var orderTargetUnit;// CUnitMember(0x05C)
        var shield;// Member(0x060, MemberKind.DWORD)
        var unitID;// Member(0x064, MemberKind.TRG_UNIT)
        var unitType;// Member(0x064, MemberKind.TRG_UNIT)
        var unknown0x66;// Member(0x066, MemberKind.WORD)  //2-byte padding
        var prevPlayerUnit;// CUnitMember(0x068)
        var nextPlayerUnit;// CUnitMember(0x06C)
        var subUnit;// CUnitMember(0x070)
        var orderQueueHead;// UnsupportedMember(0x074, MemberKind.DWORD)  //COrder
        var orderQueueTail;// UnsupportedMember(0x078, MemberKind.DWORD)
        var autoTargetUnit;// CUnitMember(0x07C)
        var connectedUnit;// CUnitMember(0x080)  //larva, in-transit, addons
        var orderQueueCount;// Member(0x084, MemberKind.BYTE)  //may be count in addition to first since can be 2 when 3 orders are queued
        var orderQueueTimer;// Member(0x085, MemberKind.BYTE)  //Cycles down from from 8 to 0 (inclusive). See also 0x122.
        var unknown0x86;// Member(0x086, MemberKind.BYTE)
        var attackNotifyTimer;// Member(0x087, MemberKind.BYTE)  //Prevent "Your forces are under attack." on every attack
        var prevUnitType;// UnsupportedMember(0x088, MemberKind.TRG_UNIT)  //zerg buildings while morphing
        var lastEventTimer;// UnsupportedMember(0x08A, MemberKind.BYTE)
        var lastEventColor;// UnsupportedMember(0x08B, MemberKind.BYTE)  //17 : was completed (train, morph), 174 : was attacked
        var unknown0x8C;// Member(0x08C, MemberKind.WORD)  // might have originally been RGB from lastEventColor
        var rankIncrease;// Member(0x08E, MemberKind.BYTE)
        var killCount;// Member(0x08F, MemberKind.BYTE)
        var lastAttackingPlayer;// Member(0x090, MemberKind.TRG_PLAYER)
        var secondaryOrderTimer;// Member(0x091, MemberKind.BYTE)
        var AIActionFlag;// Member(0x092, MemberKind.BYTE)
        var userActionFlags;// Member(0x093, MemberKind.BYTE)  //2 : issued an order, 3 : interrupted an order, 4 : hide self before death (self-destruct?)
        var currentButtonSet;// Member(0x094, MemberKind.WORD)
        var isCloaked;// Member(0x096, MemberKind.BOOL)
        var movementState;// Member(0x097, MemberKind.BYTE)
    };
    ```

    ```JavaScript
    const unit = CUnit.cast(v)        // Convert function argument or return value to CUnit object  
    const unit = CUnit(EPD)           // Create CUnit object from structure offset EPD value   
    const unit = CUnit(EPD, ptr=ptr)  // Create CUnit object from structure offset EPD value and ptr value
    const unit = CUnit.from_read(EPD) // Read from the memory address storing EPD value and create CUnit object. If the address is empty, unit is 0   
    const unit = CUnit.from_ptr(ptr)  // Calculate EPD from ptr value and create CUnit type. Cache ptr value at call location to avoid recalculating EPD
    const unit = CUnit(EPD).subUnit   // CUnit type member of CUnit instance
    ```

    ```JavaScript
    // Example: Increase resource collection over 256
    const bonusMineral = PVariable(list(492, 0, 0, 0, 0, 0, 0, 0));  // P1 is 492 + 8 = collect up to 500 mineral  
    const bonusGas = PVariable(list(992, 0, 0, 0, 0, 0, 0, 0));  // P1 is 992 + 8 = collect up to 1000 gas
    function loopUnit() {
        foreach(unit : EUDLoopCUnit()) {
            epdswitch(unit + 0x64/4, 255) {  // Unit type  
            case $U("Mineral Field (Type 1)"), $U("Mineral Field (Type 2)"), $U("Mineral Field (Type 3)"):
                // Several workers collect a mineral patch at the same time 
                unit.gatherQueueCount = 0;
                unit.nextGatherer = 0;
                break;
            case $U("Terran SCV"), $U("Zerg Drone"), $U("Protoss Probe"):
                const worker = unit;
                // If the worker is not carrying anything and has extra resources, provide extra resources
                // worker.unknown0x66 = Extra collection amount (mineral or gas)
                // worker.resourceType = Resource type (1 = mineral, 2 = gas) 
                // worker.connectedUnit = Resource (mineral patch/gas building) address
                if(worker.resourceCarryAmount == 0 && worker.unknown0x66 >= 1) {
                    if(worker.resourceType == 1) {
                        SetResources(worker.owner, Add, worker.unknown0x66, Ore);
                    } else if(worker.resourceType == 2) {
                        SetResources(worker.owner, Add, worker.unknown0x66, Gas);
                    }
                    worker.resourceType = 0;
                    worker.unknown0x66 = 0;
                }
                epdswitch(worker + 0x4D/4, 0xFF00) {  // Order
                case EncodeUnitOrder("Harvesting Minerals") * 256, EncodeUnitOrder("Enter/Exit Gas Mine") * 256: {
                    worker.connectedUnit = worker.orderTarget;  // Store the mineral address in unused space
                    // Indicates mineral/gas 
                    worker.resourceType = 1 + l2v(worker.order == EncodeUnitOrder("Enter/Exit Gas Mine"));
                    break;
                } case EncodeUnitOrder("Reset Collision (Harvester&amp;Mine)") * 256: {
                    // Operates after mining minerals or gas
                    if(worker.connectedUnit >= 1 && worker.resourceType >= 1 && worker.resourceType <= 2) {
                        const player = worker.owner;
                        const bonusAmount = (worker.resourceType == 1) ? bonusMineral[player] : bonusGas[player];
                        const targetResource = worker.connectedUnit;  // Mineral patch/gas building CUnit!
                        const resourceAmount = targetResource.resourceAmount;
                        // If the remaining amount of mineral/gas building is less than the extra collection amount
                        if (resourceAmount < bonusAmount) {
                            // Collect all remaining amounts
                            worker.unknown0x66 = resourceAmount;
                            targetResource.resourceAmount = 0;
                        } else {
                            // If the mineral patch/gas building has enough extra collection amounts, collect more
                            worker.unknown0x66 = bonusAmount;
                            targetResource.resourceAmount -= bonusAmount;
                        }
                    }
                }
                case EncodeUnitOrder("Can Harvest Minerals") * 256:
                    if(worker.orderState == 2) {
                        worker.order = py_str("Move to Harvest Minerals");
                        worker.orderState = 1;
                    }
                    break;
                }
                break;
            }
        }
    }
    ```


- ### UnitGroup

    UnitGroup is an optimized unit instance container after applying CPTricks.  

    ```JavaScript
    object GUnit {
        function remove(){}           // Remove itself from the UnitGroup you belong to
        const dying : EUDGUnitIter;   // It is not actually an iterator. If the unit is alive, the foreach code block will not execute. If the unit dies, after executing the code block in foreach, the dead unit will remove itself from the UnitGroup it belongs to.
    }

    object UnitGroup {
        function add(epd) {}
        const cp_loop : EUDGUnitIter; // cp_loop returns an iterator that iterates over all unit instances in the container. For now, let's call this unit instance type GUnit. GUnit has a remove method to remove itself from the UnitGroup it belongs to
    };
    ```

    ```JavaScript
    // epScript example

    // UnitGroup Declaration
    const zerglings = UnitGroup(1000);
    // max capacity = 1000, will use CPTrick

    // Register Unit
    zerglings.add(epd);

    // Loop UnitGroup
    foreach(unit : zerglings.cploop) {
        // Run Triggers on **any** zerglings (alive or dead)
        foreach(dead : unit.dying) {
            // Run Triggers on dead zerglings
        }  // <- dead zergling will be removed at end of *dying* block
        // Run Triggers on alive zerglings
    }

    // example usage
    function afterTriggerExec() {
        const zerglings = UnitGroup(1000);
        foreach(cunit : EUDLoopNewCUnit()) {
            epdswitch(cunit + 0x64/4, 255) {
            case $U("Zerg Zergling"):
                zerglings.add(epd);
                break;
            }
        }
        foreach(unit : zerglings.cploop) {
            foreach(dead : unit.dying) {
                // spawn Infested Terran when zergling dies
                dead.move_cp(0x4C / 4);  // Owner
                const owner = bread_cp(0, 0);
                dead.move_cp(0x28 / 4);  // Unit Position
                const x, y = posread_cp(0);

                setloc("loc", x, y);
                CreateUnit(1, "Infested Terran", "loc", owner);
            }
        }
    }
    ```

- ### CSprite

    Sprite instance operation object type

    ```Python
    class CSpriteFlags(EnumMember):
        DrawSelCircle = Flag(0x01)  # Draw selection circle
        AllySel1 = Flag(0x02)
        AllySel2 = Flag(0x04)
        Selected = Flag(0x08)  # Draw HP bar, Selected
        Flag4 = Flag(0x10)
        Hidden = Flag(0x20)  # Hidden
        Burrowed = Flag(0x40)  # Burrowed
        IscriptCode = Flag(0x80)  # Iscript unbreakable code section

    class CSprite(EPDOffsetMap):
        __slots__ = "_ptr"
        prev = CSpriteMember(0x00)
        next = CSpriteMember(0x04)
        sprite = Member(0x08, MemberKind.SPRITE)
        playerID = Member(0x0A, MemberKind.TRG_PLAYER)  # officially "creator"
        # 0 <= selectionIndex <= 11. Index in the selection area at bottom of screen.
        selectionIndex = Member(0x0B, MemberKind.BYTE)
        # Player bits indicating the visibility for a player (not hidden by the fog-of-war)
        visibilityFlags = Member(0x0C, MemberKind.BYTE)
        elevationLevel = Member(0x0D, MemberKind.BYTE)
        flags = CSpriteFlags(0x0E, MemberKind.BYTE)
        selectionTimer = Member(0x0F, MemberKind.BYTE)
        index = Member(0x10, MemberKind.WORD)
        unknown0x12 = Member(0x12, MemberKind.BYTE)
        unknown0x13 = Member(0x13, MemberKind.BYTE)
        pos = Member(0x14, MemberKind.POSITION)
        posX = Member(0x14, MemberKind.POSITION_X)
        posY = Member(0x16, MemberKind.POSITION_Y)
        mainGraphic = Member(0x18, MemberKind.DWORD)  # officially "pImagePrimary"
        imageHead = Member(0x1C, MemberKind.DWORD)
        imageTail = Member(0x20, MemberKind.DWORD)

        def __init__(self, epd: int_or_var, *, ptr: int_or_var | None = None) -> None:

        @classmethod
        def cast(cls: type[T], _from: int_or_var) -> T:

        @classmethod
        def from_ptr(cls: type[T], ptr: int_or_var) -> T:

        @classmethod
        def from_read(cls: type[T], epd) -> T:

        @property
        def ptr(self) -> int | c.EUDVariable:
    ```
