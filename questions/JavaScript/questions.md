---
title: JavaScript 问题
---


## 目录
- [说一下JavaScripts的变量类型。](#说一下JavaScripts的变量类型)
- [聊一聊symbol类型。](#聊一聊symbol类型)
- [`null`、`undefined`和未声明变量之间有什么区别？如何检查判断这些状态值？](#nullundefined和未声明变量之间有什么区别如何检查判断这些状态值)
- [`==`和`===`的区别是什么？](#和的区别是什么)
- [宿主对象（host objects）和原生对象（native objects）的区别是什么？](#宿主对象host-objects和原生对象native-objects的区别是什么)
- [匿名函数的典型应用场景是什么？](#匿名函数的典型应用场景是什么)
- [高阶函数（higher-order）的定义是什么？](#高阶函数higher-order的定义是什么)
- [聊一聊柯里化函数（curry function）。](#聊一聊柯里化函数curry-function)
- [什么是闭包（closure），为什么使用闭包？](#什么是闭包closure为什么使用闭包)
- [请解释变量提升（hoisting）。](#请解释变量提升hoisting)

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


### `null`、`undefined`和未声明变量之间有什么区别？如何检查判断这些状态值？

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


### `==`和`===`的区别是什么?

===完全相等，指内存地址相同，不会进行强制类型转换，==会进行强制类型转换，最终toNumber进行数字比较，规则如下：
- 数字和字符串：将字符串进行toNumber操作
- 其他类型和boolean，将boolean值进行toNumber
- null与undefined 返回true
- 对象与非对象，对对象执行toPrimitive操作
- 对象与对象，比较相同地址

如[] == 0 // true => Number([].toString()) //0 => 0 == 0。toPrimitive操作，执行x.valueOf()（基本类型）或者x.toString()（基本类型），报错。

###### 参考

- 《你不知道的JavaScript中卷》 第4章 强制类型转换


[[↑] 回到顶部](#目录)


### 宿主对象（host objects）和原生对象（native objects）的区别是什么？

- 原生对象是由ECMAScript规范定义的JavaScript内置对象，包括String，Number，Boolean，RegExp，Object，Math，Symbol等。
- 宿主对象是由运行时环境（浏览器或Node）提供，如window，XMLHTTPRequest等 

###### 参考

- https://stackoverflow.com/questions/7614317/what-is-the-difference-between-native-objects-and-host-objects


[[↑] 回到顶部](#目录)


### 匿名函数的典型应用场景是什么？

匿名函数提供更好的封闭性和可读性，但是报错时无法在堆栈中找到报错的函数，增加调试难度，一般场景如下：
- IIFE(立即调用函数)
- 回调函数


[[↑] 回到顶部](#目录)


### 高阶函数（higher-order）的定义是什么？

接受函数作为参数的函数或输出一个函数，提取不同的方法作为传参。将代码进行简化，提前确认，避免每次都重复判断。
```js
function add(x, y, f) {
   return f(x) + f(y);
}
```

###### 参考

- https://www.liaoxuefeng.com/wiki/1022910821149312/1023021271742944


[[↑] 回到顶部](#目录)


### 聊一聊柯里化函数（curry function）。

函数柯里化，是将使用多个参数的一个函数转成一系列使用一个参数或多个参数的函数的技术。
场景：
- 参数复用
- 延迟运行

因为使用了arguments，闭包等技术，会存在性能问题的风险。

```js
function add(a, b) {
    return a + b;
}
// 执行 add 函数，一次传入两个参数即可
add(1, 2); // 3
// 假设有一个 curry 函数可以做到柯里化
var addCurry = curry(add);
addCurry(1)(2) // 3
```
使用闭包技术实现：
```js
// 闭包实现
function curry(fn){
    let length = fn.length
    let args = [].slice.call(arguments, 1);
    return function(){
        let _args = [...args, ...arguments];
        if (_args.length < length){
            return curry.apply(null, [fn, ..._args])
        }else{ 
            return fn.apply(null, _args)
        }
    } 
}
var fn = curry(function(a, b, c) {
    console.log([a, b, c]);
});
fn("a", "b", "c"); // ["a", "b", "c"]
fn("a", "b")("c"); // ["a", "b", "c"]
fn("a")("b")("c"); // ["a", "b", "c"]
fn("a")("b", "c"); // ["a", "b", "c"]
```
###### 参考

- https://github.com/mqyqingfeng/Blog/issues/42


[[↑] 回到顶部](#目录)


### 什么是闭包（closure），为什么使用闭包？

当一个函数可以记住并访问当前所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行的。

优点：
- 私有化
- 隐藏内部实现
- 规避冲突

缺点：
- 内存泄露
###### 参考

- 《你不知道的JavaScript上卷》 第5章 作用域闭包


[[↑] 回到顶部](#目录)


### 请解释变量提升（hoisting）。

使用var或者函数声明或初始化变量时，会将声明语句提升到当前函数作用域的顶部。但只有声明会触发提升。
```js
// 用 var 声明得到提升
console.log(foo); // undefined
var foo = 1;
console.log(foo); // 1
// 用 let/const 声明不会提升
console.log(bar); // ReferenceError: bar is not defined
let bar = 2;
console.log(bar); // 2
```
当同事使用var和函数声明时，var声明会被忽略，但是后面的函数声明会覆盖前面的。
```js
foo(); // 3 
function foo() { console.log( 1 ); } 
foo(); // 3 
var foo = function() { console.log( 2 ); }; 
foo(); // 2 
function foo() { console.log( 3 ); }
foo(); // 2 
```

###### 参考

- 《你不知道的JavaScript上卷》 第4章 提升


[[↑] 回到顶部](#目录)

