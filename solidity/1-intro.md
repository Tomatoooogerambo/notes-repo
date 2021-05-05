- [Solidity简介](#solidity简介)
  - [Solidity是什么？](#solidity是什么)
- [Solidity基本语法](#solidity基本语法)
  - [属性类型](#属性类型)
    - [`address`](#address)
      - [问题1. 合约中的`address(this)` 和 `msg.sender`的区别](#问题1-合约中的addressthis-和-msgsender的区别)
    - [`mapping`](#mapping)
    - [`event`](#event)
  - [特殊的全局变量](#特殊的全局变量)
    - [二进制接口`abi`](#二进制接口abi)
    - [区块`block`](#区块block)
    - [消息`msg`](#消息msg)
    - [交易`tx`](#交易tx)
    - [加密`ecc`](#加密ecc)
    - [地址类范型属性`<address>`](#地址类范型属性address)
    - [类型`type`](#类型type)
  - [方法](#方法)
    - [`function`](#function)
    - [`construct`](#construct)
    - [`modifier`](#modifier)
  - [关键特性](#关键特性)
    - [可见度 `visibility`](#可见度-visibility)
    - [修饰符](#修饰符)
    - [事件](#事件)
    - [多继承](#多继承)
- [EVM层](#evm层)
  - [变量存储位置](#变量存储位置)
    - [`Stroage`](#stroage)
    - [`Memory`](#memory)
    - [`Stack/Calldata`](#stackcalldata)
  - [solidity汇编](#solidity汇编)
- [合约审计安全问题](#合约审计安全问题)
  - [交易重铸](#交易重铸)
  - [不正确验签](#不正确验签)
  - [签名重放](#签名重放)
  - [重入攻击](#重入攻击)
  - [不经意存储](#不经意存储)

# Solidity简介
## Solidity是什么？
solidity其实是一个用于以太坊中编写智能合约的语言，其语法有点类似于javascript

# Solidity基本语法
```
pragma solidity >=0.7.0 <0.9.0;
```
使用`pragma`来标记语言的版本

语言面向合约编程，其实有面向对象编程的意思，只不过这个对象在solidity中特指Contract

在一个标准的合约中，其语法类似于

```
contract Coin {
    // do something here
}
```

和所有的编程语言一样，在一个结构体中，总有这个结构体中带有的方法以及方法

而在以太坊中进行开发，有些基本的属性和方法以及关键字是必须要掌握的。这一块的内容会持续更新。

作为我个人的学习笔记，避免耗费重复的时间去塑造solidity的开发体系，在这里就直接进行知识的系统归纳和细节学习

## 属性类型
### `address`
#### 问题1. 合约中的`address(this)` 和 `msg.sender`的区别
address(this)值得是一个合约部署之后的地址， msg.sender的地址是调用者的所在地址。
### `mapping`
`map`**类型的变量是不可以作为函数的参数或者返回值的**，这一点需要特别注意
### `event`

## 特殊的全局变量
### 二进制接口`abi`
- `abi`全称为 `Application Binary Interface`应用二进制接口，是合约和与以太坊生态系统交互的一套标准库.

- `abi`中的主要方法是对传递的数据做编解码，所以`abi`中的数据大多数是:

- `abi.encodeXXXX()` `abi.decodeXXX()`
### 区块`block`
- `block`全局变量主要是链上区块的一些属性
  
- [ ] 后续完善block字段
### 消息`msg`
消息和交易在以太坊中之前是一个比较容易混淆的概念。但实际上消息主要是合约与以太坊交互过程中的一个数据结构体。

`msg`中常用的字段是：
- `msg.sender (address payable)`这个是最常用的消息的发送者,或者说一个合约中函数的调用者。
  
- `msg.data (bytes)` 这个字段中包含完整的调用数据`call data`。
  
- `msg.value (uint)` 这个字段主要指发送者在消息中发送多少的以太币，1以`Wei`为单位。通常向合约支付以太币是被允许的，因为合约也有一个地址。
- 一般的写法`call{value: msg.value}("")`这种方式就将货币进行发送
  
### 交易`tx`
- `tx.gasprice (uint)`是指一笔交易中包含的gas

- `tx.origin (address payable)` 这笔交易的发送者地址
### 加密`ecc`
ecc中主要包含运算hash和运算签名方法，这些方法在dapp的开发中是经常被使用到的，所以应该尽可能去熟悉这些方法。
- `blockhash(uint blockNumber) returns (bytes32)` <br>对区块给出对应的hash值。但是需要注意的是，**目前的以太坊只支持最近出的256个区块的检索**。

- `keccak256(bytes memory) returns (bytes32)` <br>这个是**最常见**的生成交易哈希txid的函数。
  
- `sha256(bytes memory) returns (bytes32)` <br>基本的生成hash的函数。
  
- `ripemd160(bytes memory) returns (bytes20)` <br>生成RIPEMD加密哈希的函数。
  
- `ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address)` <br>该函数能够从椭圆曲线签名中获取公钥相联系的地址。
  
### 地址类范型属性`<address>`
- `balance`: 地址的余额。

- `<address payable>.send(uint256 amount) returns (bool)` <br>发送指定数量的账户余额，失败返回`false`。

- `<address payable>.transfer(uint256 amount` <br>发送指定数量的余额，失败抛出异常。
### 类型`type`
`type`主要是对合约的访问和操作中会使用到的全局变量。
- `type(C).name (string)` 合约的名称。
  
- `type(C).creationCode (bytes memory)` 对合约创建字节码。
  
- `type(C).runtimeCode (bytes memory)` 创建合约运行时字节码。
  
- `type(I).interfaceId (bytes4)` 
  
- `type(T).min( T )`: 给定类型的最小值。
  
- `type(T).max( T )`: 给定类型的最大值。

## 方法
### `function`
### `construct`
### `modifier`
修饰器主要是用来定义一个代码逻辑，来约束某个函数的执行。
```javascript
modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        // Underscore is a special character only used inside
        // a function modifier and it tells Solidity to
        // execute the rest of the code.
        _;
}
```
当你定义了一个修饰器之后，你就可以将该修饰器放在某个函数之心前执行
```javascript
function changeOwner(address _newOwner)
    public
    onlyOwner <------
    validAddress(_newOwner)
{
    owner = _newOwner;
}
```
## 关键特性
### 可见度 `visibility`
- `public` 定义：内部和外部均可见。
  
- `private` 定义：当前的**合约**内可见。
  
- `external` 定义：仅对外部可见，也就是说定义好的函数，需要被调用时，使用`this.funcxxx()`的方式。
  
- `internal` 定义：仅内部可见
### 修饰符
修饰符主要是对solidity中函数的访问变量的权限进行限制，或者是对状态变量本身的访问进行控制。

- `pure` 不允许对状态变量的**修改** 或者**访问**
  
- `view` 不允许对状态变量的**修改**但是允许访问

- `constant` 被`constant`修饰过的状态变量， 除了在初始化中赋值，其他任何操作都不能对该变量进行修改/重新分配内存/修改值。初始化完成直接存在编码中
  
- `immutable` 被该修饰符修饰过的状态变量，除了在`constructor`中被修改过后不会再允许被修改。之后存在字节编码中

- `payable` 允许函数调用接受以太币

- `virtual` 允许继承父累的合约中的函数和修饰器被重写,如果不显式表示出来，则派生合约不能够进行重写

- `override` 在继承合约中声明所实现的方法是重写了某一父类中的某个方法，需要显式表示


### 事件
`Solidity`中的事件是以太坊虚拟机的**日志功能**的顶层抽象。应用想要获得相关日志信息，都需要事件通过以太坊客户端`RPC`调用来进行注册和监听。
- `emit` 
  - 事件就是通过`emit`关键字进行发送的
  
- `indexed` 
  - `topics`不是日志`log`数据类型下的一种数据结构，它是一种特殊的数据结构，你可以使用`indexed`关键字将随事件的一些参数放在`topics`结构体中。
  - 如果你不使用`indexed`这来进行修饰，那么这个数据结构将被“`abi-encoded`”到`log`数据结构体中。
  - 需要注意，如果向`indexed`中的参数中传入数组一类的数据，那么就`topics`这一结构体中只会存储它的哈希值，因为它最大只能存32字节的数据。
  - 例如签名的哈希值，签名的哈希值也是`topics`的一种
  - 一个常见的输出例子如下，注意留意输出结果中的`topics`
```json
{
   "returnValues": {
       "_from": "0x1111…FFFFCCCC",
       "_id": "0x50…sd5adb20",
       "_value": "0x420042"
   },
   "raw": {
       "data": "0x7f…91385",
       "topics": ["0xfd4…b4ead7", "0x7f…1a91385"]
   }
}
```

- `anonymous`
  - 如果你将某个事件使用`anonymous`，那么这个事件在之后将不能直接通过指定事件的名称来获取返回结果，只能根据合约的地址进行查询信息。

### 多继承
> 首先需要注意，子类多继承父类时，构造函数是自右向左实现的。比如一下代码
```javascript
// Order of constructors called:
// 1. Y
// 2. X
// 3. E
contract E is X, Y {
    constructor() Y("Y was called") X("X was called") {
    }
}
```
> 然后记住继承的原则，这也是继承的第二知识点。当一个合约多继承了多个父类的时候。重写函数调用父累方法`super()`的原则是: **就近，就右原则**
> 例如下面的代码:
```javascript
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


# EVM层
## 变量存储位置
### `Stroage`
- `Storage`存储在每个账户均开辟的一个存储区域，这个区域中的数据能够一直存在于函数调用和交易之间，这些数据最终是要上链的。也就意味着在这个存储区域中存储的数据进行读写操作的成本比较高。<br>因此需要注意在使用创建这些变量时需要注意，越少使用这些storage的变量越好。
  
- 每一个合约`Contract`只能操作定义在合约内部的`Storage`， 任何非该合约内部的`Storage`变量都无法进行操作。
  
- `State Variable`和`Storage`的关系： `State Variables`默认存储在`Storage`中。

### `Memory`
`Memory`中的数据主要存储在合约函数中的调用中。
### `Stack/Calldata`
`CallData`中的数据主要是调用外部函数时，存放函数参数的存储区域

## solidity汇编



# 合约审计安全问题
## 交易重铸
## 不正确验签
## 签名重放
## 重入攻击
## 不经意存储