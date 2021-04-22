- [Solidity简介](#solidity简介)
  - [Solidity是什么？](#solidity是什么)
  - [Solidity的历史](#solidity的历史)
- [Solidity基本语法](#solidity基本语法)
  - [属性类型](#属性类型)
    - [address](#address)
      - [问题1. 合约中的address(this) 和 msg.sender的区别](#问题1-合约中的addressthis-和-msgsender的区别)
    - [mapping](#mapping)
    - [event](#event)
  - [特殊的全局变量](#特殊的全局变量)
    - [二进制接口`abi`](#二进制接口abi)
    - [区块`block`](#区块block)
    - [消息`msg`](#消息msg)
    - [交易`tx`](#交易tx)
    - [加密`ecc`](#加密ecc)
    - [地址类范型属性`<address>`](#地址类范型属性address)
    - [类型`type`](#类型type)
  - [方法](#方法)
    - [function](#function)
    - [construct](#construct)
  - [关键字](#关键字)
    - [可见度 `visibility`](#可见度-visibility)
    - [修饰符](#修饰符)
    - [emit](#emit)
- [EVM层](#evm层)
  - [变量存储位置](#变量存储位置)
    - [Stroage](#stroage)
    - [Memory](#memory)
    - [Stack](#stack)
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
## Solidity的历史
- [ ] 调查其开发历史
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
### address
#### 问题1. 合约中的address(this) 和 msg.sender的区别
address(this)值得是一个合约部署之后的地址， msg.sender的地址是调用者的所在地址。
### mapping
### event

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
  
- `msg.value (uint)` 这个字段主要指发送者在消息中发送多少的以太币，1以`Wei`为单位
  
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
### function
### construct

## 关键字
### 可见度 `visibility`
- `public` 定义：内部和外部均可见。
  
- `private` 定义：当前的**合约**内可见。
  
- `external` 定义：近对外部可见，也就是说定义好的函数，需要被调用时，使用`this.funcxxx()`的方式。
  
- `internal` 定义：仅内部可见
### 修饰符
### emit

# EVM层
## 变量存储位置
### Stroage
- `Storage`存储在每个账户均开辟的一个存储区域，这个区域中的数据能够一直存在于函数调用和交易之间，也就意味着在这个存储区域中存储的数据进行读写操作的成本比较高。<br>因此需要注意在使用创建这些变量时需要注意，越少使用这些storage的变量越好。
  
- 每一个合约`Contract`只能操作定义在合约内部的`Storage`， 任何非该合约内部的`Storage`变量都无法进行操作。
  
- [ ] `State Variable`和`Storage`的关系

### Memory
### Stack

## solidity汇编



# 合约审计安全问题
## 交易重铸
## 不正确验签
## 签名重放
## 重入攻击
## 不经意存储