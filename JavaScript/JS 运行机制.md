# JS 机制解析

## 预解析

> 在我们运行 JS 代码时，JS 引擎会对我们的代码进行预解析。一般来说预解析阶段会将我们使用 var 声明的变量和我们的 function 函数声明**提升到当前作用域的最前面**。
>
> 正因如此我们也将其称为**变量提升**和**函数提升**。

**变量预解析**

​	把所有的变量**声明**提升到**当前作用域**最前面，但是**不提升赋值**操作。

**函数预解析**

​	对于使用 **function** 声明的函数`function fun(){}`，会将整个声明与函数体进行函数提升，但是不会调用。

​	对徐使用 **var** 声明的函数`var fun = function(){}`，会将声明提升，但是不会对函数体提升。

**Demo JS源代码**

```js
// JS源代码
var num = 10
fun()
function fun() {
  console.log(num)
  var num = 20
}
fun2()
var fun2 = function () {
  console.log(num)
  var num = 30
}
fun2()
console.log(num)
console.log(test)
```

**Demo JS源代码被JS引擎预解析之后**

```js
// JS引擎预解析之后
var num
var fun2
function fun() {
  var num
  console.log(num) // 由于作用域链，输出的是上面那个只定义的num
  num = 20
}
num = 10
fun()
fun2()
fun2 = function () {
  var num
  console.log(num)
  num = 30
}
fun2()
console.log(num)

/* out:
    > undefined
    > Uncaught TypeError: fun2 is not a function
    如果忽略报错，继续运行将输出以下
    > undefined
    > 10
    > Uncaught ReferenceError: test is not defined
*/
```

## 函数声明问题

> 在 JS 中，有两种函数的声明方式。
>
> 我们可以通过 var 来进行函数声明，也可以直接使用 function 关键字来进行函数声明。
>
> 二者声明并不是完全相同，而是有一些略微的差别。

**函数的两种声明方式**

```js
function fc(){
	console.log("fc")
}

var fc2 = function(){
	console.log("fc2")
}
```

**区别**

`function fc(){}`

1. 如果在使用function fc(){}进行声明时，如果在声明之前已经存在**被赋值的变量** `fc`，JS 并不会使用函数覆盖掉 `fc`变量。这将会导致声明无效。

```js
var fc = 1
function fc() {
  console.log("test")
}
console.log(fc)
fc()
/* out:
    > 1
    > Uncaught TypeError: fc is not a function
*/
```

2. 如果在 `function fc(){}`进行声明时，如果仅仅存在**被声明而没有被赋值的变量**`fc`，那么 JS 会使用函数赋值给`fc`变量。

```js
var fc
function fc() {
  console.log("test")
}
console.log(fc)
fc()
/* out:
    > ƒ fc() {
          console.log("test")
        }
    > test
*/
```

3. 使用`function fc(){}`进行声明的函数整体都会被 JS 预解析进行函数提升。

```js
// 预解析前
console.log(fc)
fc()
function fc() {
  console.log("test")
}
// 预解析后
function fc() {
  console.log("test")
}
console.log(fc)
fc()
/* out:
    > ƒ fc() {
      console.log("test")
    }
	> test
*/
```

`var fc2 = function(){}`

	1. 使用`var fc2 = function(){}`进行声明的函数，不管之前是否存在`fc2`变量，声明完成后`fc2`都是刚刚声明的函数。
	1. 使用`var fc2 = function(){}`进行声明的函数，变量提升时仅仅对声明进行提升，不对函数体进行提升。

```js
// 预解析之前
console.log(fc2)
fc2()
var fc2 = function () {
  console.log("test")
}
// 预解析之后
var fc2
console.log(fc2)
fc2()
fc2 = function () {
  console.log("test")
}
/* out:
	> undefined
	> Uncaught TypeError: fc2 is not a function
*/
```

## 同步和异步的执行机制

> 目前 JS 支持同步操作和异步操作。
>
> 但是 JS 是单线程的，所以他的异步操作本质上也是单线程的，只是 JS 让其看起来更像是多线程。

为了让 JS 能够实现同步和异步的执行，JS 中设计了**运行栈（执行栈）、异步进程处理模块、任务队列（消息队列）、事件循环**这几个机制。

**运行机制**

1. 在 JS 中，**同步任务将依次在运行栈中执行，异步任务将会提交给异步进程处理模块**。
2. 在**异步进程处理模块**中，将**判断每个异步任务的运行条件是否满足**。如果满足执行条件那么**异步进程处理模块将会将完成条件的异步任务按照完成条件的先后顺序，提交给任务队列**。
3. 任务队列会等待运行栈执行同步任务，当运行栈中的任务运行完毕，这时任务队列会依次向运行栈提交任务队列里面的异步任务。
4. 运行栈具有事件循环机制，其完成任务的时候运行栈并不会停止，而是会等待获取任务，并执行任务，然后继续等待获取任务······，所以当任务队列将其中的任务提交给运行栈时，运行栈会继续运行任务。这时就完成了异步任务的执行。

代码验证：任务队列会等待执行栈运行完毕

```js
setTimeout(()=>{
  console.log("我是异步任务: 计时器")
}, 1000)
for(let i = 0; i<5000; i++){
  console.log("我是同步任务: for循环")
}
```

- 此时定时器并不是 1s 后才执行器任务，而是等待运行栈同步任务 for 循环执行完毕后，才开始执行定时器中的异步任务。
- 注意这里的定时器本身其实是同步任务，里面定时器的异步函数才是异步任务。运行栈同步执行会顺序执行`setTimeout()`函数，然后里面的异步任务`()=>{...}`会提交给异步进程处理模块。

## 存储机制

> 在 JS 中的数据存在两种形式，分别是简单数据类型和复杂数据类型。
>
> 这两种数据类型在 JS 中有着不同的存储机制。

**简单数据类型**

​	在存储变量时存储的是值本身，因此也叫做**值类型**。分别包括：string、number、boolean、undefined、null

​	其中 null 返回类型`typeof null`的是一个空对象类型 object。这其实应该返回成 null 类型，这是 JS 设计之初的 bug。

**复杂数据类型**

​	在存储变量时存储的是内存地址（引用），因此也叫做**引用数据类型**。用 new 关键字创建的对象都是引用数据类型，如 Object、Array、Data 等。

**存储位置**

​	在内存中存在者堆和栈两个空间。

​	简单数据类型存放在栈里，复杂数据类型存放在堆里

![](https://pic1.imgdb.cn/item/633abc0d16f2c2beb1117d58.jpg)

**简单数据类型传递值（值传递）**

```js
function fun(value){
    consolr.log(value)
}
var a = 1
var b = a
fun(a)
```

- 简单数据类型传入函数的参数时，对形参的修改不会影响实参值。

![](https://pic1.imgdb.cn/item/633abd2416f2c2beb1137766.jpg)

**复杂数据类型传递值（引用传递）**

```js
function fun(value){
	console.log(value)
}
let person = {
    name: "brokyz",
    age: 21
}
let person2 = person
fun(person)
```

- 复杂数据类型传入函数参数时，对形参身上属性的修改会影响实参身上的属性。因为它们都是**引用传递**，指向同一个对象。

![](https://pic1.imgdb.cn/item/633ac80616f2c2beb126876a.jpg)

## 作用域

> 作用域控制着变量起作用的范围。
>
> 在 **ES6 之前**只存在**全局作用域**和**局部作用域**。
>
> 在 **ES6** 中添加了**块级作用域**。

**全局作用域**：在整个 script 标签或者单独的 js 文件中起作用。

**局部作用域**（函数作用域）：在函数内部起作用。

**块级作用域**：`{ }`内部叫做块级作用域，只在内部起作用。声明对象的`{ }`不是块级作用域。

**作用域链**

内部函数访问非自身变量，采取的是链式查找的方式来决定选取哪个值，这种结构叫作用域链。

当出现函数嵌套时，内部函数优先去外部函数查找需要使用的变量，如果存在，就使用外部函数中的变量，如果不存在就去外部函数之外寻找所使用的变量，如果存在就使用外部函数之外的变量。

```js
let a = "a1"
let b = "b1"
let c = "c1"
function fn1() {
  let b = "b2"
  let c = "c2"
  function fn2(){
    let c = "c3"
    console.log(a)
    console.log(b)
    console.log(c)
  }
  fn2()
}
fn1()
// out:
// > a1
// > b2
// > c3
```

**var 不存在块级作用域，let 和 const 存在块级作用域：**

```js
for (var i = 0; i < 5; i++) {
  var num1 = 10
}
while (true) {
  var num2 = 7
  break
}
console.log(`i:${i},num1:${num1},num2:${num2}`)
// > i:5,num1:10,num2:7
```

**var 和 let 的区别：**

```js
for (var i = 0; i < 10; i++) {
  setTimeout(function(){
    console.log(i);
  })
}
// 输出十个 10
```

- 每次循环时，定时器都会被提交到异步进程处理模块，然后条件成立后被提交到队列，等待运行栈里面的 for 循环运行完毕后才开始提交到运行栈执行异步操作。但是 for 循环，由于 var 没有块级作用域， i = 10，所以输出十个 10。

```js
for (let j = 0; j < 10; j++) {
  setTimeout(function(){
    console.log(j);
  })
}
// out: 0123456789
```

- 每次循环时，定时器都会被提交到异步进程处理模块，然后条件成立后被提交到队列，等待运行栈里面的 for 循环运行完毕后才开始提交到运行栈执行异步操作。由于 let 具有块级作用域，所以十次输出将会分别输出其对应的块级作用域中的 j，因此输出结果为 0123456789。

**let 和 const 都会存在暂时性死区的问题**

```js
let a = 0
{
  console.log(a)
}
let b = 1
{
  console.log(b)
  let b = 2
}
// out: 
// > 0
// > Uncaught ReferenceError: Cannot access 'b' before initialization
```

- 块级作用域内，必须先定义一个变量才能去使用。
- 一旦块级作用域中定义了变量，当使用这个变量时就会从块级作用域查找，不回去全局作用域查找。

## this 指向问题

1. 在**普通函数**中 this 指向当前函数的调用者。
2. 在箭头函数中没有绑定 this, 所以在其中使用 this 并不是指向调用者，如果使用 this，使用的是声明时所在作用域的 this，这个 this不是自己的 this，而是通过作用域链向上查找到的。
3. 在**构造函数**与**原型对象**中 this 指向当前构造函数 new 出来的实例对象。
4. 在**对象**中没有绑定 this，所以其中的 this 应指向声明对象时所在作用域中的 this。
5. 全局作用域中的 this 指向 window。如果使用严格模式则指向 undefined。

注意：定时器和 console 都是 window 的方法，所以严格模式中输出 this 都是 window。

**普通函数与构造函数中的 this**

```js
// 1.普通函数的this
function fn() {
  console.log(this)
}
fn()
// out: > window

// 构造函数的 this
function Person() {
  this.fun = function () {
    console.log(this)
  }
}
const p = new Person()
p.fun()
// out: > Person {fun: ƒ}
```

**对象与箭头函数中的this**

```js
var obj = {
  age: 20,
  say: () => {
    console.log(this)
    console.log(this.age)
  },
  say2: function () {
    console.log(this)
    console.log(this.age)
  }
}
obj.say()
obj.say2()
/* out:
      > window
      > undefined
      > {age: 20, say: ƒ, say2: ƒ}
      > 20
  由于对象不会产生作用域, 所以箭头函数通过作用域链向上查找 this, 最后使用的时 window 中的 this.
  但是 window 中没有 age, 所以输出 undefined.
  而普通函数指向自己的调用者.
*/
```

**手动改变 this 指向**

可以通过 call()、apply()、bind()三个函数手动改变 this 的指向。

`fn.call(thisArg, arg1, arg2...)`：调用 fn 函数，并改变此次调用 fn 函数中的 this 指向为 thisArg，并向函数中传递参数 arg1，agr2…

`fn.apply(thisArg, [arg1, arg2...])`: 调用 fn 函数，并改变此次调用 fn 函数中的 this 指向为 thisArg，并向函数中传递参数 arg1，agr2…

`const newFn = fn.bind(thisArg, arg1, arg2)`：不调用 fn 函数，返回一个改变fn 函数中的 this 指向为 thisArg，并向函数中传递参数 arg1，agr2…的新函数

```js
var obj = {
  name: "brokyz"
}
function fn(arr) {
  console.log(arr)
  console.log(this)
}
fn.call(obj, "red")
fn.apply(obj, ["pink"])
const newFn = fn.bind(obj)
newFn("green")
const newFn2 = fn.bind(obj, "yellow")
newFn2()
/* out:
	> red
	> {name: 'brokyz'}
	> pink
	> {name: 'brokyz'}
	> green
	> {name: 'brokyz'}
	> yellow
	> {name: 'brokyz'}
*/
```

## 构造函数与原型对象

> 当我们使用构造函数时，我们永远离不开另外两个概念，那就是实例对象和原型对象。
>
> 这是 JS 中这三者的一个结构关系。 
>
> 由这三者又可以引出原型链的概念。

**其中的关系我们可以用一张图来进行说明**

![](https://pic.imgdb.cn/item/62db608bf54cd3f937e13c68.jpg)

- 其中`Apple.prototype`叫做显式原型，`apple.__proto__`叫做隐式原型。**显式原型和隐式原型都指向同一个原型对象，二者逻辑判断为 true**。
- 原型对象会通过原型链以`.__proto__`隐式原型的形式指向父级的原型对象，**所以可以使用定义在父级的原型对象中的属性和方法**。原型链最终将会指向 null。
- 原型链可以算作式半个继承，因为这个只继承了上一级原型链中的方法，并不会继承上一级构造函数中的属性与方法。

## 模块化

