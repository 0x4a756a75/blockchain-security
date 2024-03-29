# Solidity Cheatsheet and Best practices


## Table of contents
 * [SPDX License Identifier](#SPDX-License-Identifier)
 * [Pragma](#Pragma)
 * [Import](#Import)
 * [Ether and Wei](#Ether-and-Wei)
 * [Data Types](#Data-Types)
    + [Boolean](#Boolean)
    + [Integer](#Integer)
    + [Address](#Address)
    + [Array](#Array)
    + [Enum](#Enum)
    + [Struct](#Struct)
    + [Mapping](#Mapping)
 * [Variables](#Variables)
    + [Local](#Local)
    + [State](#State)
    + [Global](#Global)
 * [Constants](#Constants)
 * [Immutable](#Immutable)
 * [Data Locations](#Data-Locations-Storage-Memory-and-Calldata)
    + [Storage](#Storage)
    + [Memory](#Memory)
    + [Calldata](#Calldata)
 * [Functions](#Functions)
    + [Access Modifiers](#Access-Modifiers)
    + [Parameters](#Parameters)
      - [Input Paramters](#Input-Parameters)
      - [Output Parameters](#Output-Parameters)
 * [Constructors](#Constructors)
 * [Contracts](#Contracts)
 * [View](#View)
 * [Constant](#Constant) 
 * [Pure Functions](Pure-Functions)
 * [Payable-Functions](#Payable-Functions)
 * [Modifiers](#Modifiers) 
 * [Control Structures](#Control-Structures)
 * [Error Handling](#error-hadling)
 * [Try-Catch](#Try-Catch)
 * [require](#require)
 * [Events](#Events)
 * [Inheritance](#Inheritance)
 * [Virtual](#Virtual)
 * [Override](#Override)


## SPDX-License-Identifier
  the Solidity compiler encourages the use of machine-readable SPDX license identifiers.Every source file should start with a comment indicating its license:

`// SPDX-License-Identifier: MIT`

If you do not want to specify a license or if the source code is not open-source, please use the special value `UNLICENSED`.
The comment is recognized by the compiler anywhere in the file at the file level, but it is recommended to put it at the top of the file.
More information about how to use SPDX license identifiers can be found at the [SPDX website](https://spdx.dev/ids/#how).
## Pragma

 The `pragma` keyword is used to enable certain compiler features or checks. 
 The version pragma is used as follows: `pragma solidity >=0.4.22 <0.9.0;`
  A source file with the line above does not compile with a compiler earlier than version 0.4.22, and it also does not work on a compiler starting from version 0.9.0 (). Because there will be no breaking changes until version 0.9.0, you can be sure that your code compiles the way you intended. The exact version of the compiler is not fixed, so that bugfix releases are still possible.

## Import
  You can import local and external files in Solidity.
  ### **Local**
  The import directive is used to import a local file.

  ```sh
  ├── Import.sol
└── Foo.sol
  ```
  Fool.sol
  ```sh
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.10;

contract Foo {
    string public name = "Foo";
}
  ```
import.sol
```sh
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.10;
  // import Foo.sol from current directory
import "./Foo.sol";

contract Import {
    // Initialize Foo.sol
    Foo public foo = new Foo();

    // Test Foo.sol by getting it's name.
    function getFooName() public view returns (string memory) {
        return foo.name();
    }
}
```

### **External**
You can also import from **GitHub** by simply copying the url
```sh
// https://github.com/owner/repo/blob/branch/path/to/Contract.sol
import "https://github.com/owner/repo/blob/branch/path/to/Contract.sol";

// Example import ECDSA.sol from openzeppelin-contract repo, release-v3.3 branch
// https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v3.3/contracts/cryptography/ECDSA.sol
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v3.3/contracts/cryptography/ECDSA.sol";

```
```sh
import "filename";

import * as symbolName from "filename"; or import "filename" as symbolName;

import {symbol1 as alias, symbol2} from "filename";
```

### Ether and Wei
Transactions are paid with `ether`.

Similar to how one dollar is equal to 100 cent, one `ether` is equal to 10**18 wei.
```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract EtherUnits {
    uint public oneWei = 1 wei;
    // 1 wei is equal to 1
    bool public isOneWei = 1 wei == 1;

    uint public oneEther = 1 ether;
    // 1 ether is equal to 10^18 wei
    bool public isOneEther = 1 ether == 1e18;
}

```
## Data Types
### Boolean
  
  `bool`: The possible values are constants `true` and `false`.

`bool public boo = true;`

### Integer
`int` / `uint`: Signed and unsigned integers of various sizes. Keywords `uint8` to `uint256` in steps of 8 (unsigned of 8 up to 256 bits) and `int8` to `int256`. 
 
 **Default**: 
 
`uint` and `int` are aliases for `uint256` and `int256`, respectively.

**Unsigned** : uint8 | uint16 | uint32 | uint64 | uint128 | uint256(uint)

**Signed** : int8 | int16 | int32 | int64 | int128 | int256(int)

uint stands for unsigned integer, meaning non negative integers, different sizes are available
  - uint8   ranges from 0 to 2 ** 8 - 1
  - uint16  ranges from 0 to 2 ** 16 - 1
  - uint32  ranges from 0 to 2 ** 32 - 1
  - uint64  ranges from 0 to 2 ** 64 - 1
  - uint128  ranges from 0 to 2 ** 128 - 1
  - uint256 ranges from 0 to 2 ** 256 - 1

   **example**

  uint8 public u8 = 1;

  uint public u256 = 456;

  uint public u = 123; // uint is an alias for uint256

Negative numbers are allowed for int types. Like uint, different ranges are available from int8 to int256
    
  - int256 ranges from -2 ** 255 to 2 ** 255 - 1
  - int128 ranges from -2 ** 127 to 2 ** 127 - 1 

  **example**:

  int8 public i8 = -1;

  int public i256 = 456;

  int public i = -123;     // int is same as int256

  **minimum and maximum of int**

    int public minInt = type(int).min;
    int public maxInt = type(int).max;

  **Fixed Point Numbers**

  Fixed point numbers are not fully supported by Solidity yet. They can be declared, but cannot be assigned to or from.
  `fixed` / `ufixed`: Signed and unsigned fixed point number of various sizes.

  ### Address

  The address type comes in two flavours, which are largely identical:
   - `address`: Holds a **20** byte value (size of an Ethereum address).

  ```sh
    address public addr = 0xCA35b7d915458EF540aDe6068dFe2F44E8fa733c;
  ```

   - `address` payable: Same as address, but with the additional members **transfer** and **send**.

  #### **Methods**

  **balance**
  
  `<address>.balance (uint256)`: balance of the Address in Wei

  **Transfer and Send**
  - `<address>.transfer(uint256 amount)`: send given amount of Wei to Address, throws on failure
  - `<address>.send(uint256 amount) returns (bool)`: send given amount of Wei to Address, returns false on failure

   The idea behind this distinction is that `address payable` is an address you can send Ether to, while a plain address cannot be sent Ether.
  ### Array

  Arrays can be dynamic or have a fixed size.

  `uint[7]`: fixed-sized byte arrays.

  `uint[]`: dynamically-sized byte arrays.

  ### Enum

  Solidity supports enumerables and they are useful to model choice and keep track of state.
  Enums can be declared outside of a contract.

  **Enum representing shipping status**

  ```sh 
  enum Status {
        Pending,
        Shipped,
        Accepted,
        Rejected,
        Canceled
  }
```
  ### Struct

You can define your own type by creating a `struct`.

They are useful for grouping together related data.

Structs can be declared outside of a contract and imported in another contract.

```sh
struct Todo {
        string text;
        bool completed;
    }
Todo todos;
```
### Mapping

Mapping in Solidity acts like a hash table or dictionary in any other language
Maps are created with the syntax 

`mapping(key => value) <access specifier> <name>;`

`key` can be value types such as `uint, address or bytes` except for a `mapping`, a `dynamically sized array`, a `contract`, an `enum`, or a `struct`.

`value` can be any type including another mapping or an array.

 // Mapping from address to uint

`mapping(address => uint) public myMap;`

### Variables

There are 3 types of variables in Solidity
 - **Local**
    + declared inside a function
    + not stored on the blockchain 
 - **State**
    + declared outside a function
    + stored on the blockchain

 - **Global**(provides information about the blockchain)

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Variables {
    // State variables are stored on the blockchain.
    string public text = "Hello";
    uint public num = 123;

    function doSomething() public {
        // Local variables are not saved to the blockchain.
        uint i = 456;

        // Here are some global variables
        uint timestamp = block.timestamp; // Current block timestamp
        address sender = msg.sender; // address of the caller
    }
}
```

**Reading and Writing to a State Variable**

To write or update a state variable you need to send a transaction.

On the other hand, you can read state variables, for free, without any transaction fee.
```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract SimpleStorage {
    // State variable to store a number
    uint public num;

    // You need to send a transaction to write to a state variable.
    function set(uint _num) public {
        num = _num;
    }

    // You can read from a state variable without sending a transaction.
    function get() public view returns (uint) {
        return num;
    }
}

```
### Constants

Constants are variables that cannot be modified.

Their value is hard coded and using constants can save gas cost.

***coding convention to uppercase constant variables***

```sh
address public constant MY_ADDRESS = 0x777788889999AaAAbBbbCcccddDdeeeEfFFfCcCc;
uint public constant MY_UINT = 123;
```

### Immutable

Immutable variables are like constants. Values of immutable variables can be set inside the constructor but cannot be modified afterwards.

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Immutable {
    // coding convention to uppercase constant variables
    address public immutable MY_ADDRESS;
    uint public immutable MY_UINT;

    constructor(uint _myUint) {
        MY_ADDRESS = msg.sender;
        MY_UINT = _myUint;
    }
}

```
**Immutable vs Constant**
Both `immutable` and `constant` are keywords that can be used on state variables to restrict modifications to their state. The difference is that `constant` variables can never be changed after compilation, while `immutable` variables can be set within the constructor.
State variables can be declared as `constant` or `immutable`. In both cases, the variables cannot be modified after the contract has been constructed. For `constant` variables, the value has to be fixed at compile-time, while for `immutable`, it can still be assigned at construction time.

```sh
pragma solidity >0.6.4 <0.7.0;

contract C {
    uint constant X = 32**22 + 8;
    string constant TEXT = "abc";
    bytes32 constant MY_HASH = keccak256("abc");
    uint immutable decimals;
    uint immutable maxBalance;
    address immutable owner = msg.sender;

    constructor(uint _decimals, address _reference) public {
        decimals = _decimals;
        // Assignments to immutables can even access the environment.
        maxBalance = _reference.balance;
    }

    function isBalanceTooHigh(address _other) public view returns (bool) {
        return _other.balance > maxBalance;
    }
}
```

### Data Locations - Storage, Memory and Calldata

Variables are declared as either `storage`, `memory` or `calldata` to explicitly specify the location of the data.

`storage` - variable is a state variable (store on blockchain)
`memory` - variable is in memory and it exists while a function is being called
`calldata` - special data location that contains function arguments, only available for external functions

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract DataLocations {
    uint[] public arr;
    mapping(uint => address) map;
    struct MyStruct {
        uint foo;
    }
    mapping(uint => MyStruct) myStructs;

    function f() public {
        // call _f with state variables
        _f(arr, map, myStructs[1]);

        // get a struct from a mapping
        MyStruct storage myStruct = myStructs[1];
        // create a struct in memory
        MyStruct memory myMemStruct = MyStruct(0);
    }

    function _f(
        uint[] storage _arr,
        mapping(uint => address) storage _map,
        MyStruct storage _myStruct
    ) internal {
        // do something with storage variables
    }

    // You can return memory variables
    function g(uint[] memory _arr) public returns (uint[] memory) {
        // do something with memory array
    }

    function h(uint[] calldata _arr) external {
        // do something with calldata array
    }
}

```
### Functions
There are several ways to return outputs from a function.

Public functions cannot accept certain data types as inputs or outputs

**Structure**
```sh
function function_name(<parameter types>) {internal|external|public|private} [pure|constant|view|payable] [returns (<return types>)]
```
### Access-Modifiers

- `public` - Accessible from this contract, inherited contracts and externally
- `private` - Accessible only from this contract
- `internal` - Accessible only from this contract and contracts inheriting from it
- `external` - Cannot be accessed internally, only externally. Recommended to reduce gas. Access internally with this.f

The visibility specifier is given after the type for state variables and between parameter list and return parameter list for functions.
```sh
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract C {
    uint private data;

    function f(uint a) private pure returns(uint b) { return a + 1; }
    function setData(uint a) public { data = a; }
    function getData() public view returns(uint) { return data; }
    function compute(uint a, uint b) internal pure returns (uint) { return a + b; }
}

// This will not compile
contract D {
    function readData() public {
        C c = new C();
        uint local = c.f(7); // error: member `f` is not visible
        c.setData(3);
        local = c.getData();
        local = c.compute(3, 5); // error: member `compute` is not visible
    }
}

contract E is C {
    function g() public {
        C c = new C();
        uint val = compute(3, 5); // access to internal member (from derived to parent contract)
    }
}
```

### Parameters
### Input-Parameters

Parameters are declared just like variables and are memory variables.

`function f(uint _a, uint _b) {}`

### Output-Parameters

Output parameters are declared after the returns keyword.

`function f(uint _a, uint _b) returns (uint _sum) {
   _sum = _a + _b;
}`


Output can also be specified using return statement. In that case, we can omit parameter name `returns (uint)`.

Multiple return types are possible with `return (v0, v1, ..., vn)`.

### Constructors
A constructor is an optional function that is executed upon contract creation.

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

// Base contract X
contract X {
    string public name;

    constructor(string memory _name) {
        name = _name;
    }
}

// Base contract Y
contract Y {
    string public text;

    constructor(string memory _text) {
        text = _text;
    }
}

// There are 2 ways to initialize parent contract with parameters.

// Pass the parameters here in the inheritance list.
contract B is X("Input to X"), Y("Input to Y") {

}

contract C is X, Y {
    // Pass the parameters here in the constructor,
    // similar to function modifiers.
    constructor(string memory _name, string memory _text) X(_name) Y(_text) {}
}

// Parent constructors are always called in the order of inheritance
// regardless of the order of parent contracts listed in the
// constructor of the child contract.

// Order of constructors called:
// 1. Y
// 2. X
// 3. D
contract D is X, Y {
    constructor() X("X was called") Y("Y was called") {}
}

// Order of constructors called:
// 1. Y
// 2. X
// 3. E
contract E is X, Y {
    constructor() Y("Y was called") X("X was called") {}
}

```
If there is no constructor, the contract will assume the default constructor, which is equivalent to `constructor() {}`.

### View and Pure
Getter functions can be declared view or pure.

`View` function declares that no **state** variable will be **changed**.

`Pure` function declares that no **state** variable will be **changed** or **read**.
```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract ViewAndPure {
    uint public x = 1;

    // Promise not to modify the state.
    function addToX(uint y) public view returns (uint) {
        return x + y;
    }

    // Promise not to modify or read from the state.
    function add(uint i, uint j) public pure returns (uint) {
        return i + j;
    }
}
s
```
### Constant
### Payable

Functions and addresses declared `payable` can receive `ether` into the contract.
```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Payable {
    // Payable address can receive Ether
    address payable public owner;

    // Payable constructor can receive Ether
    constructor() payable {
        owner = payable(msg.sender);
    }

    // Function to deposit Ether into this contract.
    // Call this function along with some Ether.
    // The balance of this contract will be automatically updated.
    function deposit() public payable {}

    // Call this function along with some Ether.
    // The function will throw an error since this function is not payable.
    function notPayable() public {}

    // Function to withdraw all Ether from this contract.
    function withdraw() public {
        // get the amount of Ether stored in this contract
        uint amount = address(this).balance;

        // send all Ether to owner
        // Owner can receive Ether since the address of owner is payable
        (bool success, ) = owner.call{value: amount}("");
        require(success, "Failed to send Ether");
    }

    // Function to transfer Ether from this contract to address from input
    function transfer(address payable _to, uint _amount) public {
        // Note that "to" is declared as payable
        (bool success, ) = _to.call{value: _amount}("");
        require(success, "Failed to send Ether");
    }
}

```

### Fallback-function

### Modifiers 

Modifiers can be used to change the behaviour of functions in a declarative way. For example, you can use a modifier to automatically check a condition prior to executing the function.
they can be used for:
 - Restrict  access
 - Validate inputs
 - Guard against reentrancy hack

 ```sh

  // Modifier to check that the caller is the owner of
  // the contract

 modifier onlyOwner {
    require(msg.sender == owner);
      // Underscore is a special character only used inside
      // a function modifier and it tells Solidity to
      // execute the rest of the code.
    _;
  }

  // Modifiers can take inputs. This modifier checks that the
  // address passed in is not the zero address.

  modifier validAddress(address _addr) {
        require(_addr != address(0), "Not valid address");
        _;
    }

  function changeOwner(address _newOwner) public onlyOwner validAddress(_newOwner) {
        owner = _newOwner;
    }

  // Modifiers can be called before and / or after a function.
  // This modifier prevents a function from being called while
  // it is still executing.
  modifier noReentrancy() {
    require(!locked, "No reentrancy");
      locked = true;
      _;
      locked = false;
  }

  function decrement(uint i) public noReentrancy {
    x -= i;
    if (i > 1) {
      decrement(i - 1);
    }
  }
 ```
### Control-Structures
  Most of the control structures known from curly-braces languages are available in Solidity except for switch and goto.

+ if else
+ while
+ do
+ for
+ break
+ continue
+ return
+ ? :

### Error Handling

Solidity uses state-reverting exceptions to handle errors. Such an exception undoes all changes made to the state in the current call (and all its sub-calls) and flags an error to the caller. Exceptions can contain `error` data that is passed back to the caller in the form of error instances. The built-in errors **Error**(string) and **Panic**(uint256) are used by special functions. 


**Error** is used for *regular* error conditions while **Panic** is used for errors that should not be present in bug-free code.

An `error` will undo all changes made to the state during a transaction.

You can throw an `error` by calling `require`, `revert` or `assert`.

`require` is used to validate inputs and conditions before execution.
`revert` is similar to `require`. See the code below for details.
`assert` is used to check for code that should never be false. Failing `assertion` probably means that there is a bug.

The `require` function either creates an error without any data or an error of type `Error`(string). It should be used to ensure valid conditions that cannot be detected until execution time. This includes conditions on inputs or return values from calls to external contracts.

The `assert` function creates an error of type `Panic`(uint256). The same error is created by the compiler in certain situations as listed below.

Assert should only be used to test for internal errors, and to check invariants. Properly functioning code should never create a `Panic`, not even on invalid external input. If this happens, then there is a bug in your contract which you should fix. Language analysis tools can evaluate your contract to identify the conditions and function calls which will cause a Panic.


`assert(bool condition)`
causes a Panic error and thus state change reversion if the condition is not met - to be used for internal errors.

`require(bool condition)`
reverts if the condition is not met - to be used for errors in inputs or external components.

`require(bool condition, string memory message)`
reverts if the condition is not met - to be used for errors in inputs or external components. Also provides an error message.

`revert()`
abort execution and revert state changes

`revert(string memory reason)`
abort execution and revert state changes, providing an explanatory string


```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Error {
    function testRequire(uint _i) public pure {
        // Require should be used to validate conditions such as:
        // - inputs
        // - conditions before execution
        // - return values from calls to other functions
        require(_i > 10, "Input must be greater than 10");
    }

    function testRevert(uint _i) public pure {
        // Revert is useful when the condition to check is complex.
        // This code does the exact same thing as the example above
        if (_i <= 10) {
            revert("Input must be greater than 10");
        }
    }

    uint public num;

    function testAssert() public view {
        // Assert should only be used to test for internal errors,
        // and to check invariants.

        // Here we assert that num is always equal to 0
        // since it is impossible to update the value of num
        assert(num == 0);
    }

    // custom error
    error InsufficientBalance(uint balance, uint withdrawAmount);

    function testCustomError(uint _withdrawAmount) public view {
        uint bal = address(this).balance;
        if (bal < _withdrawAmount) {
            revert InsufficientBalance({balance: bal, withdrawAmount: _withdrawAmount});
        }
    }
}

```
### Contracts

Contracts can be created from another contract using new keyword. The source of the contract has to be known in advance.

```sh
contract A {
    function add(uint _a, uint _b) returns (uint) {
        return _a + _b;
    }
}

contract C {
    address a;
    function f(uint _a) {
        a = new A();
    }
}
```

 #### Calling Parent Contracts

 Parent contracts can be called directly, or by using the keyword `super`.

By using the keyword `super`, all of the immediate parent contracts will be called.


### **Panic** via `assert` and **Error** via `require`

### Try-Catch

 `try / catch` can only catch errors from external function calls and contract creation.

 ```sh
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

// External contract used for try / catch examples
contract Foo {
    address public owner;

    constructor(address _owner) {
        require(_owner != address(0), "invalid address");
        assert(_owner != 0x0000000000000000000000000000000000000001);
        owner = _owner;
    }

    function myFunc(uint x) public pure returns (string memory) {
        require(x != 0, "require failed");
        return "my func was called";
    }
}

contract Bar {
    event Log(string message);
    event LogBytes(bytes data);

    Foo public foo;

    constructor() {
        // This Foo contract is used for example of try catch with external call
        foo = new Foo(msg.sender);
    }

    // Example of try / catch with external call
    // tryCatchExternalCall(0) => Log("external call failed")
    // tryCatchExternalCall(1) => Log("my func was called")
    function tryCatchExternalCall(uint _i) public {
        try foo.myFunc(_i) returns (string memory result) {
            emit Log(result);
        } catch {
            emit Log("external call failed");
        }
    }

    // Example of try / catch with contract creation
    // tryCatchNewContract(0x0000000000000000000000000000000000000000) => Log("invalid address")
    // tryCatchNewContract(0x0000000000000000000000000000000000000001) => LogBytes("")
    // tryCatchNewContract(0x0000000000000000000000000000000000000002) => Log("Foo created")
    function tryCatchNewContract(address _owner) public {
        try new Foo(_owner) returns (Foo foo) {
            // you can use variable foo here
            emit Log("Foo created");
        } catch Error(string memory reason) {
            // catch failing revert() and require()
            emit Log(reason);
        } catch (bytes memory reason) {
            // catch failing assert()
            emit LogBytes(reason);
        }
    }
}

 ```
### require
`require(bool condition)`: throws if the condition is not met - to be used for errors in inputs or external components
```sh
 require(msg.value % 2 == 0); // Only allow even numbers
```

### Events

`events` allow the contract to notify other entities (contracts, web3 applications, etc) that something has happened. When you declare an event you can specify at max 3 indexed parameters. When a parameter is declared as indexed it allows 3rd-party apps to filter events for that specific parameter

`Events` allow logging to the Ethereum blockchain. Some use cases for events are:

Listening for `events` and updating user interface
A cheap form of storage


```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Event {
    // Event declaration
    // Up to 3 parameters can be indexed.
    // Indexed parameters helps you filter the logs by the indexed parameter
    event Log(address indexed sender, string message);
    event AnotherLog();

    function test() public {
        emit Log(msg.sender, "Hello World!");
        emit Log(msg.sender, "Hello EVM!");
        emit AnotherLog();
    }
}

```
### Inheritance

Solidity supports multiple inheritance. Contracts can inherit other contract by using the is keyword.

Function that is going to be overridden by a child contract must be declared as `virtual`.

Function that is going to override a parent function must use the keyword `override`.

Order of inheritance is important.

You have to list the parent contracts in the order from “most base-like” to “most derived”.

```sh

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

/* Graph of inheritance
    A
   / \
  B   C
 / \ /
F  D,E

*/

contract A {
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

// Contracts inherit other contracts by using the keyword 'is'.
contract B is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "B";
    }
}

contract C is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "C";
    }
}

// Contracts can inherit from multiple parent contracts.
// When a function is called that is defined multiple times in
// different contracts, parent contracts are searched from
// right to left, and in depth-first manner.

contract D is B, C {
    // D.foo() returns "C"
    // since C is the right most parent contract with function foo()
    function foo() public pure override(B, C) returns (string memory) {
        return super.foo();
    }
}

contract E is C, B {
    // E.foo() returns "B"
    // since B is the right most parent contract with function foo()
    function foo() public pure override(C, B) returns (string memory) {
        return super.foo();
    }
}

// Inheritance must be ordered from “most base-like” to “most derived”.
// Swapping the order of A and B will throw a compilation error.
contract F is A, B {
    function foo() public pure override(A, B) returns (string memory) {
        return super.foo();
    }
}
```

### Virtual
### Override
