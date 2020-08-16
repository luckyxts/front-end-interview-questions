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
- [使用let、var和const创建变量有什么区别？](#使用letvar和const创建变量有什么区别)
- [请简述JavaScript中的this。](#请简述JavaScript中的this)
- [call、apply和bind有什么区别？](#callapply和bind有什么区别)
- [聊一聊箭头函数。](#聊一聊箭头函数)
- [请解释原型继承（prototypal inheritance）的工作原理。](#请解释原型继承prototypal-inheritance的工作原理)
- [new关键字具体执行了什么？](#new关键字具体执行了什么)
- [ES6的类和ES5的构造函数有什么区别？](#ES6的类和ES5的构造函数有什么区别)
- [为什么扩展JavaScript内置对象是不好的做法？](#为什么扩展JavaScript内置对象是不好的做法)
- [请尽可能详细地解释Ajax。](#请尽可能详细地解释Ajax)
- [聊一聊JS的同步和异步。](#聊一聊JS的同步和异步)
- [什么是回调地狱？](#什么是回调地狱)
- [聊一聊前端对于异步的解决方案](#聊一聊前端对于异步的解决方案)
- [Promise.all，Promise.race，报错问题](#PromiseallPromiserace报错问题)
- [描述DOM事件流。](#描述DOM事件流)
- [attribute和property之间有什么区别？](#attribute和property之间有什么区别)


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
- 延迟运行·

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
当同时使用var和函数声明时，var声明会被忽略，但是后面的函数声明会覆盖前面的。
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


### 使用let、var和const创建变量有什么区别？

- 从作用域上，var是函数作用域，let和const是块级作用域
- 从声明上，var可以重复声明，let，const重复声明会报错（const只能赋值一次）。并且var有变量提升，let和const没有。
- 使用var会挂在到全局的window上，let和const则不会。var a = 2; window.a; // 2

###### 参考

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const


[[↑] 回到顶部](#目录)


### 请简述JavaScript中的this

this调用的值，取决于函数调用的方式，一般是去寻找调用函数时，“.”前面的对象。具体绑定规则如下：
- 默认绑定，绑定为window，严格模式是undefined
- 隐式绑定
- 显示绑定 call, apply, bind
- new关键字绑定

```js
var a = "global";
let objBind = {a: "bind"};
let obj = {
    a: "obj",
    test: function(){
        console.log(this.a);
    }
}
let tmp = obj.test;
tmp();                      // "global" 默认绑定
obj.test();                 // “obj”隐式绑定
obj.test.bind(objBind)();   // "objBind" 显示绑定
function newThis(a){ 
    this.a = a;
} 
let newThisObj = new newThis("new");
console.log(newThisObj .a)  // "new" new关键字绑定
```

***绑定优先级***
- 最高优先级为new
- 显示绑定
- 隐式绑定
```js
// 显示绑定和隐式绑定
function foo() { console.log( this.a ); } 
var obj1 = { a: 2, foo: foo }; 
var obj2 = { a: 3, foo: foo }; 
obj1.foo();                     // 2 
obj2.foo();                     // 3 
obj1.foo.call( obj2 );          // 3 
obj2.foo.call( obj1 );          // 2
 
 
// 显示绑定和new关键字
function foo(something) { this.a = something; } 
var obj1 = { foo: foo }; 
var obj2 = {}; 
obj1.foo(2); console.log( obj1.a );     // 2 
obj1.foo.call( obj2, 3); 
console.log(obj2.a);                    // 3 
var bar = new obj1.foo( 4); 
console.log(obj1.a);                    // 2
console.log(bar.a);                   // 4
```


###### 参考

- 《你不知道的JavaScript上卷》 第2章 this全面解析


[[↑] 回到顶部](#目录)


### call、apply和bind有什么区别？

上述都为显示绑定，.call和.apply直接执行，bind会直接一个函数，并不直接执行。.call将参数一个一个传入，.apply将参数已数组的方式传入。
个人认为bind是函数柯里化的一种，通过,bind函数将参数保留，在之后延迟执行，将参数传入。如:
```js
var obj = {a: 1, b: 2, c: 3}
function test(a, b, c){
    obj.a = a;
    obj.b = b;
    obj.c = c;
}
test.bind(obj, ...[553333, 4])(22222);
obj.a // 55333
obj.b // 4
obj.c // 22222
```


[[↑] 回到顶部](#目录)


### 聊一聊箭头函数。

特点:
- 无法使用new关键字
- 无法使用显示绑定
- 会记住当前箭头函数创建时，所在词法作用域的this。
```js
let obj = {
    test: function(){
        setTimeout( () => { console.log(this); } , 1000)
    }
}
obj.test(); // obj
```
但是如果改写一下，通过声明的方式去使用箭头函数，结果就大不相同。
```js
let obj = {
    test: function(){
        setTimeout( m , 1000)
    }
}
let m = () => { console.log(this); }
obj.test(); // window
```
甚至还可以改写成
```js
var obj3 = {}
var obj2 = {
    test2: function(){
        return () => {
            console.log(this)
        }
    }
}
var obj1 = {
    test: function(){
        setTimeout( obj2.test2() , 1000)
    }
}
obj1.test(); //obj2
var obj1 = {
    test: function(){
        setTimeout( obj2.test2.call(obj3) , 1000)
    }
}
obj1.test(); //obj3
```
可以看到，箭头函数内部指向的this，其实跟创建箭头函数时，所在的词法作用域有关。


[[↑] 回到顶部](#目录)


### 请解释原型继承（prototypal inheritance）的工作原理。

一般意义上的继承，以类为蓝图，根据类创造出来的实例，会继承类的行为，但是蓝图的改变并不会影响已经创造的实例，已经创造的实例也无法改变类。原型继承本质是对象的关联或行为的委托。对象在获取属性时，在自身无法取得时，根据原型链__proto__不停查找。使用new关键字返回的对象，会将自身的__proto__指向构造函数的prototype属性。
```js
let obj = { 
    __proto__: {
        a: 1
    }
};
obj.a;                              // 1
function Animal(name){
    this.name = name;
}
Animal.prototype.sayName = function(){ return this.name; };
let dog = new Animal('dog');
dog.sayName();                      // dog
dog.__proto__ === Animal.prototype; // true
```
可以看到，dog.__proto__和Animal.prototype指向的同一快内存地址，所以此时更改Animal.prototype或者dog.__proto__会产生影响。ES6的类，本质也是原型继承。

###### 参考

- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/proto


[[↑] 回到顶部](#目录)


### new关键字具体执行了什么？

- 创建一个新对象，
- 绑定this，执行函数。
- 新对象的__proto__连接构造函数的prototype
- 如果构造函数没有返回值，则返回这个对象 

一个简单的封装new关键字:
```js
function myNew(){
    let obj = {};
    let fn = arguments[0];
    let returnVal = fn.apply(obj, [].slice.call(arguments, 1));
    obj.__proto__ = fn.prototype;
    if(returnVal){
        return returnVal;
    }
    return obj;
}
let animal = myNew(Animal, "name");
```


[[↑] 回到顶部](#目录)


### ES6的类和ES5的构造函数有什么区别？

ES6的类其实就是在构造函数上做了一层封装，添加了更加友好的语法糖，但最终生成的对象还是和用构造函数返回的一样。
```js
class Student{
    constructor(name) {
        this.name = 'name';
    }
    sayName(){
        return this.name;
    }
}
// 等同于
function Student(name){
    this.name = name;
}
Student.prototype.sayName = function(){
    return this.name;
}
```
同时ES6的类还提供了更加友好的继承关键字extends
```js
class BoyStudent extends Student{
    constructor(name, sex){
        super(name);
        this.sex = sex;
    }
}
```
extends为了实现原型继承，最基本的功能是将BoyStudent.prototype.__proto__指向了Student.prototype;

###### 参考

- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/class


[[↑] 回到顶部](#目录)


### 为什么扩展JavaScript内置对象是不好的做法？

- 如果都允许扩展或修改prototype的方法，一个项目往往引入多个库，这时候就会产生互相冲突的风险。
- 如果不用defineProperty的方式定义对象方法，会被枚举出。

[[↑] 回到顶部](#目录)


### 请尽可能详细地解释Ajax。

Ajax(asynchronous JavaScript and XML)。使用JavaScript向服务器提出请求并处理响应，不阻塞当前页面。使用XMLHttpRequest这个对象，可以在不重载页面的情况下与Web服务交换数据，即在不需要刷新页面的情况下，产生局部刷新效果。
 
**原生XMLHttpRequest使用方式**
- 创建XMLHttpRequest对象
- 调用open函数，填写http请求类型和url
- 填写请求头等数据
- 编写回调函数
- 发出请求
```js
let xhr = new XMLHttpRequest();
xhr.open(`get`, `/gkit/get_user_status?name=1&sex=2`);
xhr.setRequestHeader(`Content-Type`, `text/plain`);
xhr.onload = function (){
    if (this.status === 500){
        throw new Error(this.responseText);
    }
    if (this.status === 200 || this.status === 300){
        console.log(this.responseText);
    }
};
xhr.send();
```

###### 参考


- https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest
- https://baike.baidu.com/item/AJAX/8425


[[↑] 回到顶部](#目录)


### 聊一聊JS的同步和异步。

**概述**

JavaScript是一门单线程语言，所以一般情况下，代码是按照语句出现的顺序执行的。但是有时候却不是按照顺序执行。如：
```js
setTimeout(function(){
    console.log('定时器开始啦')
});
new Promise(function(resolve){
console.log('马上执行for循环啦');
for(var i = 0; i < 10000; i++){
    i == 99 && resolve();
}
}).then(function(){
    console.log('执行then函数啦')
});
console.log('代码执行结束');
// 马上执行for循环啦
// 代码执行结束
// 执行then函数啦
// 定时器开始啦
```
**事件循环**

js是单线程，无法启用多线程任务。那么假如浏览新闻时，一张图片记载很慢，难道我们要一直卡着直到图片完全显示出来吗？因为js将任务分为两类：
- 同步任务
- 异步任务
事件循环：
- 同步和异步分别进入不同的场所，同步任务进入主线程，异步任务进入Event Table并注册函数。
- 当指定的事情完成时（如ajax请求完成，setimeout执行时间满足），Event Table会将这个函数移入Event Quene。
- 主线程内的任务执行完成后，会去Event Quene会读取相应的函数，进入主线程执行。
- 不断重复上述过程。

**宏任务和微任务**

除了同步任务和异步任务，分为更加细致的宏任务和微任务。
- 宏任务 sciprt（整体代码）、setTimeout、setInterval、I/O、UI交互、postMessage、setImmediate.
- 微任务 promise.then、Object.observe、MutaionObserver、process.nextTick
```js
setTimeout(function() {
    console.log('setTimeout');
})

new Promise(function(resolve) {
    console.log('promise');
}).then(function() {
    console.log('then');
})
    console.log('console');
```
- 这段代码作为宏任务进入主线程。
- 遇到setTimeout，那么将其回调函数注册后分发到宏任务Event Quene。
- 遇到Promise，new Promise立即执行，then函数分发到微任务Event Quene.
- 执行console.log('console')。
- 此时整段代码的宏任务执行完毕，执行Event Quene中的微任务，执行then。
- 完成，第一轮循环结束。开始执行下一轮宏任务，执行setTimeout对应回调。
- 宏任务和微任务的Event Quene为空，结束。

###### 参考


- https://juejin.im/post/6844903512845860872
- https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html


[[↑] 回到顶部](#目录)


### 什么是回调地狱？

回调地狱首先存在的问题是函数嵌套造成的极差的代码可读性，但是最重要的问题在于信任问题。回调函数是指将自己的函数，发送其他函数或者第三方函数，让他人决定在何时，何地，调用几次。试想一下，如果你有一个付款的回调函数。你把这个函数交给了第三方如$.get(`/pay`, yourFn)，第三方库在拿到yourFn方法后，可以自行决定调用几次，这时候$.get可能会执行多次，也有可能一次都不行，而你希望你的函数可以执行一次。这就会产生信任问题。
- 函数嵌套造成的可读性问题
- 信任问题


[[↑] 回到顶部](#目录)


### 聊一聊前端对于异步的解决方案。

**callback回调函数**

通过传递函数的方式，来决定调用函数的时机，会有回调地狱问题。

**Promise**

通过.then的方式，解决函数嵌套造成的可读性问题，通过resolve和reject来解决信任问题，.then函数一定并且只会调用在异步结束后调用一次。

**generator**

Generator是一个生成器，生成出一个迭代器，主要是用来控制异步流程的，目前在现有的库中还是比较少看到generator的，目前主要使用generator的是redux-saga这个库，koa1.0也是用generator，但是现在都改为async/await。

```js
function request(data) {
    setTimeout( () =>{
       it.next(data)
    }, 5000)
}
 
function* gen() {
    console.log("generator start");
    let data1 = yield request("data1");
    console.log(data1);
    let data2 = yield request("data2");
    console.log(data2);
}
let it = gen();  // 获得迭代器
it.next()        // 启动
// generator start
// 5秒后，data1
// 5秒后，data2
```
可以看到，使用genertor可以将异步逻辑用同步的方式实现，从而封装一个异步操作或异步容器，这种方式需要一个全局变量，并generator要写到每一个异步回调中执行next。所以有了下面利用Promise的进一步改进。
```js
function request(data) {
    return new Promise((resolve) => {
        setTimeout( () =>{
            resolve(data)
        }, 5000)
    })
}
 
function* gen() {
    console.log("generator start");
    let data1 = yield request("data1");
    console.log(data1);
    let data2 = yield request("data2");
    console.log(data2);
}
let it = gen();  // 获得迭代器
let { value,done } = it.next()
value.then((data)=>{ // data值为'2.txt'
    let { value,done } = it.next(data);
    return value
}).then((data)=>{ // data值为'3.txt'
    let { value,done } = it.next(data);
    return value
});
// generator start
// 5秒后，data1
// 5秒后，data2
```
要注意的是data1的值是通过it.next(data)中的data传入的，但是it.next(data)函数本省返回的是request的promise对象。但是这样写并没有摆脱promise繁琐的then调用，并且需要手动执行then函数，所以才有了co库。
```js
function request(data) {
    return new Promise((resolve) => {
        setTimeout( () =>{
       		resolve(data)
    	}, 5000)
    })
}
 
function* gen() {
    console.log("generator start");
    let data1 = yield request("data1");
    console.log(data1);
    let data2 = yield request("data2");
    console.log(data2);
}
function co(it){
    return new Promise((resolve,reject)=>{
        function next(data){
            let { value,done } = it.next(data)
            if(!done){
                value.then((data)=>{
                    next(data)
                },reject)
            }else{
                resolve(value)
            }
        }
        next()
    })
}
co(gen()).then( () =>{
    console.log("generator end");
});
// generator start
// data1
// data2
// generator end
```
co函数封装了.then执行的方法，让其自动执行。generator+promise尽管在函数内部实现了同步书写，但是在不借助co库的情况下，在调用generator的时候，依旧无法摆脱繁琐的then函数，所以才有了co库进行了封装。

**asyncawait**

async await是ES7提供的新特性，是目前最佳的异步解决方案，个人认为这个其实就是generator + co库。它有如下特性：
- await只能放在async函数里
- await后面会是个promise对象，它会等待resolve，reject执行完成，并返回resolve或者reject中的值。
```js
async function test(){
    let data1 = await ajax("data1")
    console.log(data1);
    let data2 = await ajax("data2")
    console.log(data2)
}

function ajax(data){
   return new Promise((resolve) =>{
       setTimeout( () => {
           resolve(data);
       }, 5000)
   })
}
// data1
// data2
```
###### 参考

- https://www.liaoxuefeng.com/wiki/1022910821149312/1023024381818112
- https://www.jianshu.com/p/063f7e490e9a
- https://www.jianshu.com/p/4aae6d1948dd
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function

[[↑] 回到顶部](#目录)


### Promise.all，Promise.race，报错问题

当使用Promise.all，当其中一个请求发生错误，会造成后面不在执行
```js
function ajax(data){
   return new Promise( (resolve, reject) => {
      setTimeout( () => {
          if(typeof data === "string"){
            resolve(data)
          }
          else{
            reject("type err");
          }
      }, 2000)
   }) 
}
Promise.all([ajax(1),ajax("2"),ajax(3),ajax("4")]).then( (data) => {
    console.log(data)
}).catch( (err) => {console.log(err)})
// 并不会执行then方法，而是会抱出第一个错误
// type err
```
这就需要每个promise进行异常捕获，可以改写为:
```js
function ajax(data){
   return new Promise( (resolve, reject) => {
      setTimeout( () => {
          if(typeof data === "string"){
            resolve(data)
          }
          else{
            reject("type err");
          }
      }, Number(data)*1000)
   }).catch( (err) => { console.log(err)}) 
}

Promise.all([ajax(1),ajax("2"),ajax(3),ajax("4")]).then( (data) => {
    console.log(data);
}).catch( (err) => {
    console.log(err);   // 不会走到这里
})
// type err
// type err
// [undefined, "2", undefined, "4"]
```


[[↑] 回到顶部](#目录)


### 描述DOM事件流。

事件执行顺序：
- 捕获阶段
- 目标阶段
- 冒泡阶段
可以通过event.stopPropagation()阻止事件的继续传播。

###### 参考

- https://blog.csdn.net/moguzhale/article/details/53503044


[[↑] 回到顶部](#目录)


### attribute和property之间有什么区别？

- attribute（特性）是由HTML定义的，所有出现在HTML标签内的描述节点都是attribute特性，可以通过getAttribute获取。
- property（属性）是DOM对象，可以通过js操作普通对象一样获取。
```js
<input id="search" value="foo">
let input = document.getElementById("search");
input.value = "foo2";
input.getAttribute("value");  // "foo"
// 当对应的property改变的时候，attribute特性value的值为默认值，并不会随之改变
```


###### 参考

- https://segmentfault.com/a/1190000008781121


[[↑] 回到顶部](#目录)




