# ES6简介

- ES6就是JavaScript的一个发行版本, 于2015.06发行, 全称为ECMAScript. 一般来说JS我们刚开始学习的基础版本为ES5, ES6再ES5的基础上新增了一些新特性.
- ES6在ES5的基础上对其进行了升级, 解决了ES5的一些不足. 比如新增了类的概念.
- ES6在企业中应用很普遍.



# let和const

> 1. let 

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
console.log(test);
// 此时输出将会输出test
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
console.log(test);
// 此时输出将会输出test is not defined
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



> 2. const

- 新增const用来声明**常量**，一旦声明，常量的值就不可改变。
- 通常来讲**常量名使用大写**。
- 一旦对变量进行声明就必须初始化。

```js
const SUM = 30;
const TEST; // 没有进行初始化将会报错
```



# 解构赋值

- 解构赋值是对赋值方法的一个扩展，他将简化用户对赋值操作的需要。

