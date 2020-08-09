---
title: JavaScript 问题
---


## 目录

- [说一下JavaScripts的变量类型。](#说一下JavaScripts的变量类型)
- [聊一聊symbol类型。](#聊一聊symbol类型)
- [null、undefined和未声明变量之间有什么区别？如何检查判断这些状态值？。](#null、undefined和未声明变量之间有什么区别？如何检查判断这些状态值？)


### 说一下JavaScripts的变量类型。

JavaScript有两种变量类型，一种是基本类型（原始数据类型），一种是对象类型。基本类型具体包括数字number(NaN也属于数字)，布尔类型boolean，字符串类型string，undefined，null，symbol。

###### 参考

- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types


[[↑] 回到顶部](#目录)


### 聊一聊symbol类型。

**symbol是什么？**
- ECMAScript2015中新添加的特性。
- 是JavaScript的原始数据类型，Symbol实例唯一且不可改变。
- Symbol对象是Symbol原始值的封装。

**symbol的作用？**
- symbol通常被用作一个对象的键值，当你想让它是私有的时候。
- 通过调用内置对象Symbol，会返回一个symbol值，使用new关键会报错.
- 作为键值的symbol类型无法被枚举出，同时也无法通过Object.keys, Object.getOwnPropertyNames获取。
- 可以通过Object.getOwnPropertySymbols获得

###### 参考

- https://developer.mozilla.org/zh-CN/docs/Glossary/Symbol


[[↑] 回到顶部](#目录)


### null、undefined、undeclared有什么区别？如何检查判断这些状态值？。

**null、undefined、undeclared分别是什么？**
- null常常表示声明了变量，并且主动赋值了null。
- 当一个变量只声明却不赋值时，该变量的默认为undefined。
- undeclared表示该变量并未被声明。

**null、undefined、undeclared如何判断？**
- 通过typeof null返回值为"object"，所以无法判断。通过Object.prototype.toString.call(null),返回值为"[object Null]"。
- 通过typeof undefined"undefined"，所以无法判断。通过Object.prototype.toString.call(undefined),返回值为"[object Undefined]"。
- 通过typeof一个未声明的变量，返回为"undefined"。通过Object.prototype.toString的方式会报错。通过try/catch或if(安全防范机制)
进行判断。

###### 参考

- 《你不知道的JavaScript中卷》 p6-p10


[[↑] 回到顶部](#目录)


