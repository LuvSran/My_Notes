# var, let, const 区别
| 特性 | var | let | const |
| ---- | ---- | ---- | ---- |
| 作用域 | 函数作用域 | 块级作用域 | 块级作用域 |
| 变量提升 | 是(undeifne) | 是，但是不会被初始化（暂时性死区,ReferenceError） | 是，但是不会被初始化（暂时性死区,ReferenceError） |
| 重复声明 | 允许 | 不允许 | 不允许 |
| 初始值 | 可不初始化 | 可不初始化 | 必须初始化 |
| 重新赋值 | 可以 | 可以 | 不可以 |
# 原型与原型链
解释：
函数有 prototype 属性，叫做原型或原型对象，存放一些实例共用的属性或方法
对象都有\_proto\_ 属性，指向原型对象，原型对象也有原型对象，也是他的\_proto\_属性指向的，这样一层一层链式查找原型的结构叫原型链。顶层的原型是Object.prototype, 他的\_proto\_指向null
作用：
* *属性和方法共享* 可以把共用的方法定义在构造函数的prototype上，那么实例对象就可以通过原型链访问这些方法，从而节省内存，比如数组的push,map方法，都是在Array.prototype上，而不是存在于每个数组实例里
* *实现对象继承*  原型链可以模拟类的继承，让一个构造函数的原型指向另一个构造函数的实例，我们就能实现子类继承父类的属性和方法。就可以实现代码复用
* *原型链提供了一套统一的属性查找机制*，流程是自身=>原型=>原型的原型=>...=>null。所以数组可以直接使用map，push或者对象的toString方法。
* *动态扩展*  原型链是动态的。这意味着如果我们修改了原型对象（例如给 `Array.prototype` 添加了一个自定义方法），所有基于该原型创建的实例都能立即“感知”并使用这个新方法，而不需要重新创建对象。
# 有哪些数据类型
基础数据类型：number, string, boolean, symbol, bigint, null, undefined
引用数据类型: object( {}, 数组，函数，日期(Date)，正则(RegExp) ) 
# 判断数据类型的方法
typeof 适用于基础数据类型, typeof null 被判断为object
instanceof 判断引用数据类型，原理是看右侧构造函数的prototype是否在实例的原型链上，不能判断基本数据类型
Object.prototype.toString.call是最准确的
# 数组方法
**会改变原数组的方法**：
- `push()` - 末尾添加元素
- `pop()` - 删除末尾元素
- `shift()` - 删除首元素
- `unshift()` - 开头添加元素
- `splice()` - 添加/删除元素
- `sort()` - 排序
- `reverse()` - 反转数组
- `forEach()`-接受回调函数,遍历的时候在回调函数内可以通过下标进行操作修改元素
**不改变原数组的方法**：
- `concat()` - 合并数组
- `slice()` - 截取数组
- `join()` - 连接为字符串
- `map()` - 映射新数组
- `filter()` - 过滤数组
- `reduce()` - 累计计算
- `some()` - 测试某些元素
- `every()` - 测试所有元素
- `find()` - 查找元素
- `findIndex()` - 查找索引
- `includes()` - 是否包含
# 闭包
定义：函数及其词法作用域，简单来说就是函数和其外部变量环境的组合，如果一个函数访问了外部函数的变量，那么即使外部函数结束了，他所引用的变量也不会被销毁而是存在于内存中
原理：函数创建时会记下所处环境，只要内部函数还被使用，他所引用的外部环境变量就不会被GC清理
应用：数据私有化(隐藏细节，只暴露接口),模块化，函数柯里化(让函数记住之前的参数)，性能优化(防抖，节流依赖闭包记录内部timer状态)
可能引发的问题：可能会引发内存泄漏，比如说`setInterval`/`setTimeout` 的回调中，如果闭包持有了 DOM 元素引用，即使这些元素已从页面移除，只要监听器或定时器未被清理，内存泄漏就会发生。因此在闭包不再需要时，应该将引用的变量设为null, 或者直接将闭包函数设为null, 并且即时清理定时器，监听器(`clearInterval`、`clearTimeout`).
# this指向
this指向取决于函数调用方式
* new绑定，this指向新创建的实例
* 显式绑定,call, apply, bind, 指向传入的对象
* 隐式绑定，作为对象方法被调用, this 指向调用对象
* 默认绑定，直接调用，global/ window,严格模式undefined
* 箭头函数，没有自己的this, 继承外层this
# 什么是事件循环
js 是单线程的，为了防止代码阻塞，任务分为同步和异步。
同步任务直接进入执行栈执行；异步任务则交给宿主环境（Web/Node API）处理，等异步操作完成，回调函数会被推入任务队列。
任务队列分为宏任务（如 `setTimeout`）和微任务（如 `Promise.then`）。事件循环的逻辑是：
- 首先，执行栈执行当前的同步代码；
- 执行过程中，产生的微任务进入微任务队列，宏任务进入宏任务队列；
- 当同步代码执行完（执行栈清空），立即去清空所有的微任务；
- 微任务清空后，如果需要则进行 UI 渲染；
- 然后再取一个宏任务执行，执行完后再清空微任务。
* 然后再去取下一个宏任务执行，不断重复这个过程。
宏任务： script，setTimeout， setInterval， UI 渲染
微任务:   Promise.then， process.nextTick
# Promise
解释：
	Promise是ES6新引入的异步编程解决方案，*语法*上来说是一个构造函数，*逻辑*上来说是一个容器，保存着某个未来结束的事件，通常是异步操作。它有*三种互斥的状态*，Pending, Fulfilled, Rejected。状态只能从Pending变成Fulfilled或Rejected，一旦确定就不会再变。支持链式调用，也就是上一个.then的结果传给下一个.then,解决传统的回调地狱问题。
.then返回值：
	 普通值：等价于Promise.resolve()这个普通值
	 没有返回值: 等价于Promise.resolve(undefined)
	 返回Promise：新promise会等待内部promise的结果并跟随他的状态
	 抛异常：等价于Promise.reject
方法：
1. `Promise.resolve()` - 返回一个 resolved 状态的 Promise
2. `Promise.reject()` - 返回一个 rejected 状态的 Promise
3. `Promise.all()` - 所有 Promise 都成功时返回的新 Promise 才resolve, 否则 reject
4. `Promise.race()` - 第一个完成的 Promise 的结果
5. `Promise.allSettled()` - 所有 Promise 完成后返回结果
6. `Promise.prototype.then()` - 添加成功回调
7. `Promise.prototype.catch()` - 添加失败回调
8. `Promise.prototype.finally()` - 无论成功失败都执行
# async / await
是一个异步编程语法糖，async表示当前函数是一个异步函数，会将返回值包装成一个promise。await关键字在 async 函数内部使用。await 表示等待promise的完成，等价于promise.then()。async函数是`Generator` 的封装，await 相当于 `yield` 。async/await 内置了自动执行器，利用 Promise 的 .then()：当 `yield` 抛出一个 Promise 时，执行器会在这个 Promise 的 .then() 回调里调用迭代器的 .next()。
只要迭代器没有结束（done: false），就不断重复这个过程。相比于promise, async / await的可读性更好，错误处理也更直观，可以用try..catch捕获错误，写代码时候更符合同步代码的习惯
进阶：既然 await 那么好用，如果我有两个互相不依赖的接口请求），我直接写两个 await 会有什么问题？该怎么优化？
答：直接写两个await会串行阻塞，导致不必要的等待。那么我们可以await Promise.all()，将这两个接口请求放进promise.all()中包装成一个promise，同时发出请求
# 继承方式
* 原型链继承: 将子类的原型（`prototype`）直接指向父类的一个实例。
* 构造函数继承: 在子类构造函数中，通过 `Parent.call(this)` 来调用父类构造函数。
* 组合继承:  通过原型链继承父类原型上的方法，再通过借用构造函数继承父类的属性。
* 寄生组合式继承:  解决组合继承调用两次父类构造函数的问题。它通过 `Object.create(Parent.prototype)` 来创建一个父类原型的副本，并将其赋值给子类原型，从而避免了直接调用父类构造函数。
* `class` 语法:  `lass` 语法本质上是寄生组合式继承的语法糖。使用 `class` 关键字定义类，通过 `extends` 关键字实现继承，并在子类构造函数中使用 `super()` 来调用父类构造函数。
# Proxy(直接背)
Proxy 是 ES6 引入的一个内置构造函数，它用于创建对象的代理，能够拦截并自定义对这个对象的各种基本操作，比如属性的读取、赋值、函数的调用等。相当于一个“中间人”，它包裹着原始对象，所有外部访问这个对象的请求，都会先经过 Proxy 的处理。
# ES6 新特性
const、let， 模板字符串， 箭头函数，函数参数默认值，解构赋值，for...of 用于数组，for...in用于对象，Promise， 展开运算符(...)，对象字面量、class(原型链的语法糖)
# 匿名函数
匿名函数就是*没有名字*的函数。是一个函数表达式，*没有函数提升*。通常被赋值给一个变量、作为参数传递给另一个函数，或者在定义后立即执行。它可以*创建独立作用域*，避免*变量污染*全局命名空间。
# 箭头函数和普通函数的区别
*this指向*：
普通函数的this是动态的，可以用`call(), apply(), bind()`改变。而箭头函数是静态的，他没有自己的this，而是捕获定义时的外层作用域的this并沿用这个this。

*构造函数*：
普通函数可以作为构造函数，用new 关键字，可以创建一个新对象，并将 this 绑定到该对象上。而箭头函数不能作为构造函数，如果new 一个箭头函数会报错，因为箭头函数没有construct方法也没有自己的this, 所以无法实例化

*arguments对象*：
普通函数有自己的 `arguments` 对象，是一个类数组对象，包含函数被调用时传入的所有参数。箭头函数：没有自己的 `arguments` 对象。如果在箭头函数中使用 `arguments`，它会去外层作用域查找。也可以使用 ES6 的[[剩余参数 ...args]]来代替。

*prototype*: 
普通函数有prototype,而箭头函数没有。所以箭头函数没法通过原型链扩展方法

*适合场景*
适合写回调函数的时候用，因为它捕获定义时上下文的this，并且语法简洁
# call、apply、bind 的区别是什么？
call: 参数逐个传递，立即执行，返回值是函数执行结果
apply: 参数为数组传递，立即执行，返回值是函数执行结果
bind: 参数逐个传递，不立即执行，而是返回绑定this的新函数
[[call, apply, bind|具体讲解]]
# undefined 和 null 有什么区别
undefined 表示*未定义或缺失值*，一个变量声明但未赋值会被js引擎初始化为undefined。null表示*无对象引用*，一般是有意赋值为null的，表示这里应该是一个对象 ，但是现在为空。*使用typeof 时*，undefined是undefined，null是object。*转为数值时*，undefiend是NaN, null是0
# GC
# JS为什么是单线程的？
js 的主要是操作 DOM 和处理交互。DOM 树是共享资源。如果 JS 是多线程的，就会出现两个线程同时修改同一个 DOM 节点的情况。为了保证 DOM 操作的安全和确定性，js 必须保证同一时间只有一个线程在执行
*进阶：单线程处理耗时任务（如网络请求）不会卡死页面吗？*
不会。因为 js 是单线程 + 异步非阻塞模型。当遇到耗时操作（如 `setTimeout`、`fetch`）时，JS 会将这些任务委托给宿主环境的后台线程（Web APIs）去处理，主线程继续执行后续代码，不会被阻塞。当后台任务完成后，会将回调函数放入任务队列（微任务或宏任务队列）。一旦主线程的调用栈清空，事件循环机制就会去队列中取出回调函数压入栈中执行。
# Map 和 WeakMap
**Map(强引用)**
当你把一个对象作为键存入 `Map` 时，`Map` 会牢牢“抓住”这个对象。即使你在代码的其他地方把这个对象设为 `null`，只要 `Map` 实例本身还存在，这个对象就会一直驻留在内存中，不会被 GC 清理。这可能导致意外的内存泄漏。
**WeakMap (弱引用)**：
`WeakMap` 对键的引用是“弱”的。它不会阻止垃圾回收。一旦一个对象作为键，在外部没有其他任何强引用指向它时，js 引擎就会自动回收这个对象，同时 `WeakMap` 中对应的键值对也会被自动移除。
* 无法遍历：没有 `forEach()`、`keys()`、`values()` 等方法。
- 无法获取大小：没有 `size` 属性。
- 无法清空：没有 `clear()` 方法。
# 深浅拷贝
# == 和 === 区别
# for...of... 和 for...in...区别
# Promise.all 和 Promise.allSettled区别
# Symbol
# fetch / axios