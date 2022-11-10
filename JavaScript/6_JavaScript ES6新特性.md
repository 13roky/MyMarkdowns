# ES6简介

- ES的全程是EMCAScript，它是由EMCA国际标准化组织，指定的一项**脚本语言标准化规范**

- ES6就是JavaScript的一个发行版本, 于2015.06发行, 全称为ECMAScript. 一般来说JS我们刚开始学习的基础版本为ES5, ES6再ES5的基础上新增了一些新特性.
- ES6在ES5的基础上对其进行了升级, 解决了ES5的一些不足. 比如新增了类的概念.
- ES6在企业中应用很普遍.

# 新增声明方法

## let

- let的新增是为了解决es5中使用var声明变量时存在的各种问题。

使用var的问题：

```js
// 1. 使用var时，会存在声明提升的问题
console.log(test);
var test = 'test';
// 这里会把声明部分进行提升，将会输出test

// 2. 变量覆盖问题
var test = 'test1';
var test = 'test2';
console.log(test);
// 此处重复声明test变量，将会替换掉test变量的值

// 3. 块级作用域的问题
for (true){
    var test = 'test';
}
for (var i = 0; i < 2; i++){
    
}
console.log(test);  // test
console.log(i);  // 2
```

let对上述问题的改正：

```js
// 1. 不会声明提升
console.log(test);
let test = 'test';
// 这里会报错，不会提前声明

// 2. 不可重复声明
let test = 'test1';
let test = 'test2';
console.log(test);
// 此处报错，不能重复声明已经存在的变量

// 3. 修复块级作用域，let声明只在所在的作用域有效
for (true){
    let test = 'test';
}
for (let i = 0; i < 2; i++){
    
}
console.log(test);
console.log(i);
// 此时输出将会输出test is not defined
// 4. 使用let声明的变量由暂时性死区的特性
//  ES6 规定，如果区块中存在 let 和 const 命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”，也就是说使用let声明的变量都是先声明再使用 ，不存在变量提升问题。
var num = 10;
if (true) {
    console.log(num);
    let num;
}
// 报错原因：在块作用域内，let声明的变量被提升，但变量只是创建被提升，初始化并没有被提升(初始化就是给变量先赋值成undefined)，在初始化之前使用变量，就会形成一个暂时性死区。
```

for循环计数器适合用let：

```js
for (var i = 0; i < 10; i++) {
  setTimeout(function(){
    console.log(i);
  })
}
// 输出十个 10
for (let j = 0; j < 10; j++) {
  setTimeout(function(){
    console.log(j);
  })
}
// 输出 0123456789
/*
变量 i 是用 var 声明的，在全局范围内有效，所以全局中只有一个变量 i, 每次循环时，setTimeout 定时器里面的 i 指的是全局变量 i ，而循环里的十个 setTimeout 是在循环结束后才执行，所以此时的 i 都是 10。

变量 j 是用 let 声明的，当前的 j 只在本轮循环中有效，每次循环的 j 其实都是一个新的变量，所以 setTimeout 定时器里面的 j 其实是不同的变量，即最后输出 12345。（若每次循环的变量 j 都是重新声明的，如何知道前一个循环的值？这是因为 JavaScript 引擎内部会记住前一个循环的值）。
*/
```

面试题：

```js
var arr = [];
for (var i = 0; i < 2; i++) {
    arr[i] = function () {
        console.log(i);
    }
}
arr[0]();	//2
arr[1]();	//2
// 由于var声明的i并没有自己的块级作用域，所以等for循环结束之后，函数中的console.log(i)实际上调用的是循环之后的i值2

let arr = [];
for (let i = 0; i < 2; i++) {
    arr[i] = function () {
        console.log(i);
    }
}
arr[0]();	//0
arr[1]();	//1
// 由于let声明的i存在自己的块级作用域，所以本质想循环中的i不会公用，会分别在自己的块级空间中存在一个i，当调用了函数时，函数会首先从自己的块级作用域中向上进行查找i，所以输出的时自己所在的块级作用域中的i的值
```



## const

- 新增const用来声明**常量**，一旦声明，常量的值就不可改变。
- 通常来讲**常量名使用大写**。
- 一旦对变量进行声明就必须初始化。
- 和let一样，也具有块级作用域

```js
const SUM = 30;
const TEST; // 没有进行初始化将会报错
const SUM = 40; // 可以重新声明来改变值
```

- const不可更改常量的本质时，被const声明之后的内存地址不允许改变

```js
const arr = [100, 200];
arr[0] = 'a';	//此操作不会改变arr的内存地址
arr[1] = 'b';	//此操作不会改变arr的内存地址
arr = ['a', 'b'];	//此操作会改变arr的内存地址，因此不被允许
```

- 所以对于基本数据类型不可更改，对于复杂数据类型数据本身不能更改，但是数据内部的值可以更改。

# 解构赋值

ES6中允许从数组中提取值，按照对应位置，对变量赋值。对象也可以实现解构。

## 数组解构

- 数组解构允许我们按照一一对应的关系从数组中提取值，然后赋值给变量
- 如果解构不成功，那么变量的值位undefined

```js
let [a, b, c] = [1, 2, 3];
console.log(a);
console.log(b);
console.log(c);
```

## 对象解构

- 对象解构允许我们使用变量的名字匹配对象的属性，匹配成功将对象属性的值赋值给变量

```js
let person = {name: 'borkyz', age: 20, sex: '男'};
let {name, sex} = person;
let {age} = person;
console.log(name);
console.log(sex);
console.log(age);
```

- 对象解构还允许我们将提取出来的值赋值给另外的变量

```js
let person = {name: 'borkyz', age: 20, sex: '男'};
let {name: myName, sex: mySex} = person;
console.log(myName);
console.log(mySex);
```

# 箭头函数

- 箭头函数是用来简化函数定义语法的。
- 如果传入参数只有一个那么小括号可以省略/

- 在箭头函数中，如果函数体中只有一句代码，并且代码执行结果就是函数的返回值，函数体大括号就可以省略。

```js
(arg1, arg2, ...) => {code}
```

```js
let sum = (a, b) => {
    console.log(a);
    console.log(b);
    console.log(a+b);
    return a+b;
}
// 传入参数是一个那么可以省略小括号
let put = a => {
    console.log(a);
}

// 一行代码并且返回执行结果就可以省略大括号
let plus = (a, b) => a + b;
```

## 箭头函数的this指向

箭头函数不绑定this，因此箭头函数没有自己的this关键字，如果在箭头函数中调用this，那么this应该指向箭头函数定义位置中的this。

```js
function fn () {
	console.log(this);
    return () => {
        console.log(this);
    }
}

const obj = {name: 'brokyz'};
const resFn = fn.call(obj);
// 输出结果
```

## 面试题

```js
var obj = {
	age: 20,
    say: () => {
        alert(this.age);
    }
}

obj.say(); // undefined
// 箭头函数定义在了obj对象中，由于obj是对象，所以obj没有产生作用域，所以这个箭头函数实际上是被定义在了全局作用域下。因此this指向window，window中没有age所以弹出了undefined
```

# 剩余参数

- 剩余参数语法允许我们将一个不定数量的参数表示为一个数组。

- `arguments`将会以伪数组的形式存储我们传入函数的所有参数。
- 在箭头函数中无法使用`arguments`。

```js
function fn(first, ...args) {
    console.log(first);	// 10
    console.log(args);	// [20, 3]
    console.log(arguments); // [10, 20, 30]伪数组
}
fn(10, 20, 30);
// 箭头函数中无法使用 arguments
const sum = (...args) => {
    let total = 0;
    args.forEach(item => total += item);
    return total;
};
console.log(sum(10, 20, 30));	// 60
```

## 剩余参数和解构配合使用

```js
let student = ['wangwu', 'zhangsan', 'lisi'];
let [st1, ...st2] = student;
console.log(st1);   // wangwu
console.log(st2);   // ['zhangsan', 'lisi']
```

# ES6内置的对象扩展

## Array 的扩展方法

### 扩展运算符（展开语法）

扩展运算符可以将数组或者对象转为用逗号分隔的参数序列。

```js
let arr = [1, 2, 3];
console.log(...arr); // 1 2 3
// 注意：...arr实际上值是1, 2, 3带有逗号，但是输出时,被当作参数了
console.log(1, 2, 3); // 1 2 3
```

**应用：合并数组**

```js
// 方法1
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let arr3 = [...arr1, ...arr2];
console.log(arr3);	// [1, 2, 3, 4, 5, 6]

// 方法2
let arr4 = arr1.push(...arr2);
```

**应用：将伪数组转换为真正的数组**

```js
let lis = document.querySelectorAll('li');
arrLis = [...lis];
console.log(lis);   // NodeList(4) [li, li, li, li]
console.log(arrLis);    // (4) [li, li, li, li]
// 测试数组的方法
arrLis.push('a');   // 未报错
console.log(arrLis);    // (5) [li, li, li, li, 'a']
```

### 构造函数方法：Array.from()

- `Array.from()`方法可以将类数组或者可遍历的对象转换为真正的数组。

- `Array.from()`可以接受第二个函数参数来对数组进行加工。

```js
// 模仿伪数组
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    'length': 3
}
let arr = Array.from(arrayLike);    // ['a', 'b', 'c']
console.log(arr);
// 可以传入函数对数组进行加工
let arrayLike2 = {
    '0': 1,
    '1': 2,
    '2': 3,
    'length': 3
}
let arr2 = Array.from(arrayLike2, item => item * 2);    // [1, 4, 6]
console.log(arr2);
```

### 实例方法：find()

- 用于查找第一个符合条件的数组成员，如果没有找到返回 undefined。

```js
let arr = [{
    id: 1,
    name: '张三'
}, {
    id: 2,
    name: '李四'
}];
let target = arr.find((item, index) => item.id == 2);
console.log(target);    // {id: 2, name: '李四'}
```

### 实例方法：findIndex()

- 用于找出第一个符合条件的数组成员的位置，如果没有找到返回-1。

```js
let arr = [1, 5, 10, 15];
let index = arr.findIndex((vlaue, index) => vlaue > 9);
console.log(index);	// 2
```

### 实例方法：includes()

- 表示某个数组是否包含给定的值，返回布尔值。

```js
[1, 2, 3].includes(2); // true
[1, 2, 3].includes(4); // false
```

## String 的扩展方法

### 模板字符串

- 模板字符串可以**解析变量**。
- 模板字符串可以**换行**。
- 模板字符串可以**调用函数**。

```js
let name = 'brokyz';
let sayHello = `hello, my name is ${name}`;	// hello, my name is brokyz
// 模板字符串可以换行
let sayHi = () => 'Hi!';
let html = `
	<div>
		<span>${sayHi()}</span>
	</div>
`;
console.log(html);
```

### 实例方法：startsWith() 和 endsWith()

- `startsWith()`表示参数字符串是否在原字符产的头部，返回布尔值。
- `endsWith()`表示参数字符串是否在原字符串的尾部，返回布尔值。

```js
let str = 'Hello world!';
str.startsWith('Hello') // true
str.startsWith('!') // true
```

### 实例方法：repeat()

- `repeat()`方法表示将原字符串重复n次，返回一个新字符串。

```js
'x'.repeat(3)	// 'xxx'
'hello'.repeat(2)	// 'hellohello'
```

## Set 数据结构

- ES6 提供了新的数据解构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
- Set本身是一个构造函数，用来生成Set数据解构。

```js
const s = new Set();
```

- Set 函数可以接受一个数组作为参数，用来初始化。

```js
const set = new Set([1, 2, 3, 4, 4]);
console.log(set);	// Set(4) {1, 2, 3, 4}
console.log(set.size)
```

### 数组去重

```js
const set = new Set([1, 2, 3, 4, 4]);
const arr = [...set];
console.log(arr);	// [1, 2, 3, 4]
```

### 实例方法

- `add(value)`：添加某个值，返回 Set 解构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除成功。
- has(value)：返回一个布尔值，表示该值是否为 Set 成员。
- `clear()`：清除所有成员，没有返回值。

```js
const set = new Set();
s.add(1).add(2).add(3); // 向set结构中添加值
s.delete(2);    // 删除set结构中的2值
s.has(1);   // 表示set结构中是否有1这个值，返回布尔值
s.clear()   // 清除所有成员，没有返回值
```

### 遍历

- Set 结构的实例与数组一样，也拥有 forEach 方法，用于对每个成员执行某种操作，没有返回值。

```js
s.forEach(value => console.log(value));
```

```js
const set = new Set(['a', 'b', 'c']);
set.forEach(value => {
    console.log(value);
})
```

