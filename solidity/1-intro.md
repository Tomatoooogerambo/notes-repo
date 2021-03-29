- [Solidity简介](#solidity简介)
  - [Solidity是什么？](#solidity是什么)
  - [Solidity的历史](#solidity的历史)
- [Solidity基本语法](#solidity基本语法)
  - [属性类型](#属性类型)
    - [address](#address)
    - [mapping](#mapping)
    - [event](#event)
  - [方法](#方法)
    - [function](#function)
    - [construct](#construct)
  - [关键字](#关键字)
    - [public](#public)
    - [emit](#emit)

# Solidity简介
## Solidity是什么？
solidity其实是一个用于以太坊中编写智能合约的语言，其语法有点类似于javascript
## Solidity的历史
- [x] 调查其开发历史
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
### mapping
### event

## 方法
### function
### construct

## 关键字
### public
### emit


