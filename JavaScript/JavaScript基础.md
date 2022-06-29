[toc]

## 注释

---

在 JavaScript 中有2种注释：

1. 单行注释：`// 默认快捷键 ctrl + /`

2. 多行注释：`/* 默认快捷键 shift + alt + a */`

---

## 输入输出语句

---

| 方法             | 说明                           | 归属   |
| ---------------- | ------------------------------ | ------ |
| alert(msg)       | 浏览器弹出警示框               | 浏览器 |
| console.log(msg) | 浏览器控制台打印输出信息       | 浏览器 |
| prompt(info)     | 浏览器弹出输入框，用户可以输入 | 浏览器 |

---

## 变量

---

变量是程序在内存中开辟的一块用于存放数据的空间。

### 声明变量

在 JavaScript 中变量可以使用`var`进行声明，也可以省略`var`直接进行使用。

```javascript
// 单行
var test;
var test01 = 1;
test02 = 1;
var test03 = 'i am str'；
test04 = 'i am str'；
// 多行
var test = 18，
	test02 = 'i am str';
```

**注意：**

- 仅声明变量，但是未给变量赋值，控制台中输出为`undefined`.

- 未声明变量，在控制台中调用输出将报错。


### 变量的命名规范


- 由字母(A-Za-z)、数字(0-9)、下划线()、美元符号($)组成，如：usrAge，num01，_name

- 严格区分大小写。var app;和var App;是两个变量

- 不能以数字开头。18age是错误的

- 不能是关键字、保留字。例如：var、for、while

- 变量名必须有意义。MMD BBD nl → age

- 遵守驼峰命名法。首字母小写，后面单词的首字母需要大写。myFirstName

- 推荐翻译网站：有道 爱词霸

---

##   数据类型

---

JavaScript 中的数据类型类似于 Python，都是一种弱类型。他们在声明变量时，不用声明数据类型。而是在程序运行的过程中，数据类型会被自动确认。

在运行代码时，变量的数据类型是由 JS引擎根据 = 右边变量值的数据类型来判断的，运行完毕后，变量就确定了数据类型。

数据类型的分类：

1. 简单数据类型：Number，String，Boolean，Undefined，Null
2. 复杂数据类型：Object

**简单数据类型**

| 简单数据类型 | 说明                                        | 默认值    |
| ------------ | ------------------------------------------- | --------- |
| Number       | 数字型，包含整型和浮点型                    | 0         |
| Boolean      | 布尔值型，true / false                      | false     |
| String       | 字符串型 字符串需要带引号                   | “”        |
| Undefined    | 声明了变量但是没有负值此时变量就为Undefined | undefined |
| Null         | 声明变量为空值                              | null      |

### 关于数字型

1. 数字型直接赋值是使用的十进制
2. 在需要的数字前加 `0` 是使用八进制 `var a = 010`
3. 在需要的数字前加 `0x` 是表示十六进制 `var a = 0x10`

在控制台中输出的都是十进制

4. 数字型中的最大值 `Number.MAX_VALUE`
5. 数字型中的最小值 `Number.MIN_VALUE`
6. 数字型的无穷大 `Number.MAX_VALUE * 2` 比任何值都大
7. 数字型的无穷小 `Number.MIN_VALUE * 2` 比任何值都小
8. NaN，Not a Number，代表一个非数值，当的不出我们想要的结果并且不是个数字时用其表示，如`‘test’ - 100`

**isNaN()**

这个方法用来判断非数字，如果是数字返回 `false` 否则返回 `true`

### 字符串型

字符串类型使用单引号和双引号都可以，但是**推荐用单引号**

当使用到引号嵌套时，要使用不同的引号，外部是单引号内部就用双引号，外部是双引号内部就用单引号

**转义字符**

| 转义符 | 说明                    |
| ------ | ----------------------- |
| \n     | 换行                    |
| \\\    | 斜杠\                   |
| \\'    | 单引号                  |
| \\”    | 双引号                  |
| \t     | tab 缩进                |
| \b     | 空格，b 是 blank 的意思 |

**字符段长度 length**

```javascript
var a = 'i am str';
console.log(a.length);
```

### 字符串拼接

```javascript
console.log('沙漠'+'骆驼');
// out: 沙漠骆驼
console.log('永远'+18);
// out: 永远18
console.log('永远'+18+'岁');
// out: 永远18岁
```

**总结：数值相加，字符相连**

### 布尔型

```javascript
var flag = true,
    flag1 = false;
console.log(flag + 1); // true 参与运算当 1 来看
console.log(flag1 + 1); // false 参与运算当 0 来看
```

### undefined 和 null

```javascript
var str;
console.log(str); // 输出 undefined
var v1 = undefined;
console.log(v1 + 'test'); // 输出 undefinedtest
console.log(v1 + 1); // 输出 NaN

var space = null;
console.log(space + 'test'); // 输出 nulltest
console.log(space + 1); // 输出 1 ,因为什么都没有+1还是1
```

### 数据类型检测 typeof

```javascript
var num = 10;
var str = 'brokyz';
console.log(typeof num);
console.log(typeof str)
```

### 数据类型的转换

**转换为字符串**

| 方式               | 说明                         | 案例                                |
| ------------------ | ---------------------------- | ----------------------------------- |
| toString()         | 转字符串                     | var num = 1; alert(num.toString()); |
| String() 强制转换  | 转字符串                     | var num = 1; alter(String(num))     |
| **加号拼接字符串** | 和字符串拼接的结果都是字符串 | var num = 1; alter(num + ‘’)        |

**转换为数字型**

| 方式                    | 说明                               | 案例               |
| ----------------------- | ---------------------------------- | ------------------ |
| parseInt(string) 函数   | 将string类型转换为整数数值型       | parseInt(‘78’)     |
| parseFloat(string) 函数 | 将string类型转换为浮点数数值型     | parseFloat(‘3,14’) |
| Number() 强制转换函数   | 将string类型转换为数值型           | Number(‘12’)       |
| js 隐式转换 （- * /）   | 利用算数运算隐式转换 (注意不能用+) | ‘12’ - 0           |

**转换为布尔**

```javascript
console.log(Boolean('')); // false
console.log(Boolean(0)); // false
console.log(Boolean(NaN)); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false

// 只有上面五种情况为 false 其余情况都为 true

console.log(Boolean('123')); // true
console.log(Boolean('你好')); //true
```

---

## 运算符

---

### 算数运算符

| 运算符 | 描述 | 实例                  |
| ------ | ---- | --------------------- |
| +      | 加   |                       |
| -      | 减   |                       |
| *      | 乘   |                       |
| /      | 除   |                       |
| %      | 取余 | 5 % 3 = 2; 3 % 5 = 3; |

**浮点数运算时会有问题**

```javascript
console.log(0.1 + 0.2); // 0.30000000000000000004
```

由于浮点数运算时会转换成二进制，用二进制进行算术运算，这时就会存在一些误差

所以要尽量避免小数参与运算

### 前置递增运算符

`++num` 类似于 `num = num + 1` 但是更加简单

**前置的运算顺序是，先自加后返回值**

### 后置递增运算符

`num++` 类似于 `num = num + 1` 但是更加简单

前置自增和后置自增如果单独使用，效果一样

**但是后置自增的运算顺序是。先返回原值，后自加**

```javascript
var num = 10;
console.log(++num + 10) // 这里返回21，运算后num变为11

var num = 10；
console.log(num++ + 10) // 这里返回20，运算后num变为11
```

### 比较运算符

比较运算返回的是布尔类型，一般的符号一般默认会转换数据类型进行比较

| 运算符  | 说明                        | 案例 | 结果 |
| ------- | --------------------------- | ---- | ---- |
| <       | 小于                        |      |      |
| >       | 大于                        |      |      |
| >=      | 大于等于                    |      |      |
| <=      | 小于等于                    |      |      |
| ==      | 等于                        |      |      |
| !=      | 不等于                      |      |      |
| === !== | 全等 要求值和数据类型都一致 |      |      |

### 逻辑运算符

| 逻辑运算符 | 说明                   | 案例 |
| ---------- | ---------------------- | ---- |
| &&         | “逻辑与”，简称“与” and |      |
| \|\|       | ”逻辑或“，简称”或“ or  |      |
| !          | ”逻辑非“，简称“非” not |      |

**短路运算（逻辑中断）**

原理：当有多个表达式（值）时，左边的表达式值可以确定结果时，就不再继续运算右边的表达式的值；

- 逻辑与

​	如果第一个表达式的值为真，则返回表达式2

​	如果第一个表达式的值为假，则返回表达式1

```javascript
console.log(123 && 456); // 结果456
console.log(0 && 456); // 结果0
console.log(0 && 1 + 2 && 456 * 7) //结果0 ，在0之后的停止执行
```

- 逻辑或

  如果第一个表达式的值为真，则返回表达式1

  如果第一个表达式的值为假，则返回表达式2

```javascript
console.log(123 && 456); // 结果123
console.log(123 && 1 + 2 && 456 * 7); // 结果123, 后面的不运行，已经中断了
console.log(0 && 1 + 2 && 456 * 7) //结果3 ，在1+2之后的停止执行
```

### 赋值运算符

`num += 2` 等同于 `num = num +2`

| 赋值运算符 | 说明 | 案例 |
| ---------- | ---- | ---- |
| =          |      |      |
| +=、-=     |      |      |
| *=、/=、%= |      |      |

### 运算优先级

| 优先级 | 运算符     | 顺序             |
| ------ | ---------- | ---------------- |
| 1      | 小括号     | ()               |
| 2      | 一元运算符 | ++ -- !          |
| 3      | 算数运算符 | 先乘除后加减     |
| 4      | 关系运算符 | > >= < <=        |
| 5      | 相等运算符 | == ！= === ！=== |
| 6      | 逻辑运算符 | 先&&后\|\|       |
| 7      | 赋值运算符 | =                |
| 8      | 逗号运算符 | ，               |

---

## 流程控制

---

控制代码按照一定的结构顺序来执行

有三种结构：顺序就够、分支结构、循环结构

### if 语句

```javascript
if (条件表达式){
    
}
```

### if else 语句

```javascript
if(条件){
    
}else{

}
```

### else if 语句

```javascript
if(条件){
    
}else if(条件2){
    
}else if(条件3){
    
}else{
    
}
```

### 三元表达式

由三元运算符组成的式子

```javascript
条件表达式 ? 表达式1 : 表达式2
// 如果条件表达式为真，则返回表达式1的值，否则返回表达式2的值
```

### switch 语句

```javascript
switch(表达式){
	case value1:
        语句1；
     	break;
	case value2:
       	语句2;
        break;
    default:
        最后的语句;
}
```

当表达式必须全等case时才可以匹配成功

### switch 和 else if 区别

- 一般情况两个语句可以相互替换
- switch 通常处理 case 比较确定值的情况；而 else if 更加灵活，可以用于范围判断；
- switch 语句进行条件判断后直接执行到程序的条件语句，效率高；而 else if 得依次判断前面的条件。  
- 当分支比较少时，else if 语句执行效率更高。
- 分支比较多时，switch 执行效率更高。

---

## 循环

---

### for

```javascript
for(i=0;i<10;i++){
    
}
```

### while

```javascript
while(条件){
    
}
```

### do while

```javascript
do{
    
}while(条件)
```

### continue

跳过本次循环，并执行下一次循环

### break

跳过所有循环

---

## 数组

---

数组是**一组数据的集合**，其中每个数据被称为**元素**，在数组中可以**存放任意类型的元素**。数组是一种将一组数据存储在单个变量名下的优雅方式。

### 数组的创建方式

- 使用 new 创建数组

```javascript
var arr = new Array(); // 创建一个空数组
```

- 利用数组字面量来创建数组

```javascript
var arr = [];
var arr = ['brokyz',1,2,true]; // 数组里面的数据称为数组元素
// 数组声明了并赋值，称为数组的初始化
```

### 获取数组中的元素

索引（下标）：用来访问数组元素的符号（数组下标从 0 开始）

```javascript
var arr = ['brokyz',1,2,true];
console.log(arr[0]) // 输出 brokyz
console.log(arr[4]) // 输出 undefined 因为数组中没有第五个元素
```

### 遍历数组

把数组中元素从头到尾访问一遍

```javascript
// arr.length 获取数组长度
for (i = 0; i < arr.length; i++){
    console.log(arr[i]);
}
```

### 新增数组元素

- 通过修改 length 长度来修改

```javascript
        // 通过改变 length 新增数组元素
        var arr = [1, 2, 3];
        console.log(arr.length);
        arr.length = 4;
        console.log(arr.length);
        console.log(arr);
```

- 通过修改索引号追加数组

```javascript
        // 通过修改索引号新增数组元素
        var arr = [1, 2, 3];
        arr[3] = 4;
        console.log(arr);
```

- 不要直接给数组赋值 `arr = 'brokyz'` 这样数组就直接被替换了

---

## 函数

---

当一段代码需要重复被调用的时候，那么就需要用函数对代码进行封装

函数不调用是，自己不执行

函数中可以调用另一个函数，允许互相调用

### 声明函数

- 利用 function 关键字进行声明

```javascript
function sayHi(name){
    console.log('Hi, ' + name);
}
```

- 通过 var 声明函数

```javascript
var sayHi = function(name){
    console.log('Hi, ' + name);
}
```

- 声明函数中的参数为形参

### 调用函数

```javascript
sayHi(brokyz);
```

- 调用函数时的参数为实参

### 形参和实参的匹配问题

在其他语言，如 Java 中，形参和实参要求必须匹配；

但是在 JS 中，实参并不需要匹配实参的数量；调用时，实参仅仅根据自身个数对形参的前面几个进行匹配；如果调用时，实参的个数小于形参的个数，那么少的那个形参被定义为 undefined 结果为 NaN

### 函数返回值

函数使用 return 返回值

```javascript
function sayHi(name){
    return 'Hi, ' + name;
}
```

- 在函数中，执行完 return 函数就停止执行

```javascript
function sayHi(name){
    return  'Hi', name; // 返回结果时最后一个值
}
```

- return 只能返回一个值
- 但需要返回多个值时，可以用数组的形式返回
- 函数中如果存在 return 那么返回的就是指定的值，如果不存在 return 那么返回的是 undefined

### arguments 的使用

当函数中没有设置形参时，但是调用时传入了实参，这时可以在函数中通过 arguments 接受；

也就是说 arguments 中存取了所有传入的实参；

arguments 是一种伪数组，不是真正意义上的数组，它具有数组的 length 属性，按照索引的方式进行存储，但是他没有数组的一些方法，比如 pop()、push()

---

## 作用域

---

作用域就是代码变量在哪个范围内起作用

在 es6 之前有：全局作用域 局部作用域

**全局作用域**在整个 script 标签或者单独的 js 文件中起作用

**注意**：在函数内部，没有声明直接赋值的变量属于全局作用域，如`num = 10`

**局部作用域**（函数作用域）在函数内部起作用

### 全局变量和局部变量的区别

1. 全局变量只有在浏览器关闭的时候才会销毁，比较占用内存资源
2. 局部变量当我们程序执行完毕就会销毁，比较节约内存资源

### 块级作用域

在 es6 之前没有块级作用域，在 es6 新增了块级作用域

用 {} 包含的就是块级作用域

由于 es6 之前没有块级作用域，所以在{}中写的变量，在{}外面也可以使用

### 作用域链

当函数中又声明了一个函数时，就出现了作用域链的问题

作用域链：内部函数访问非自身变量，采取的是链式查找的方式来决定选取哪个值，这种结构叫作用域链

```javascript
var num = 10;

function fn(){ // 外部函数
	var num = 20;
    function fun(){ // 内部函数
        console.log(num)
    }
}
```

当出现函数嵌套时，内部函数优先去外部函数查找需要使用的变量，如果存在，就使用外部函数中的变量，如果不存在就去外部函数之外寻找所使用的变量，如果存在就使用外部函数之外的变量

---

## 预解析

js 引擎运行 js 代码分为两步：

1. 预解析：js 引擎会把 js 里面的 var 和 function 提升到当前作用域的最前面
2. 执行代码：代码按照预解析之后的顺序，从上往下执行

预解析分为：

- 变量预解析（变量提升）：把所有的变量声明提升到当前作用域最前面，但是不提升赋值操作
- 函数预解析（函数提升）：把所有的函数声明提升到当前作用域最前面，不调用函数

例子：

```javascript
// 源代码
var num = 10;
fun();
function fun() {
	console.log(num);
    var num = 20;
}

// js引擎预解析之后

var num;
function fun() {
    var num;
	console.log(num); // 由于作用域链，输出的是上面那个只定义的num
    num = 20;
}
num = 10;
fun();
// 输出为 undefined
```

---

## 对象

对象就是一个**具体事物**，在 JS 中对象是一组无序的属性和方法的集合，所有的事物都是对象，例如字符串、数值、数组、函数等

对象是由**属性**和**方法**组成的。

- 属性：事物的特性，在对象中用属性来表示
- 方法：事物的行为，在对象中用方法来表示

### 为什么需要对象

当我们保存一个值的时候，可以使用变量保存多个值，也可以使用数组保存多个值。但是如果我们要保存一个事物的完整信息时候，用对象存储就非常的方便

### 对象的创建

1. 利用字面量创建的对象

对象的字面量为 {}

```javascript
var obj = {
    uname: 'brokyz',
    agr: 18,
    sex: 'male',
    sayHi: function(){
        console.log('hi');
    }
} 
```

2. 利用 new Object 创建对象

```javascript
var obj = new Object();
obj.uname = 'brokyz';
obj.age = 18;
obj.sex = 'male';
obj.sayHi = function(){
    console.log('hi');
}
```

3. 使用构造函数创建对象

前两个方法只能创建一个对象，我们可以利用构造函数重复相同的构造

构造函数的名字首字母要大写

构造函数不需要 return 就可以返回结果

```javascript
function Apple(name){
    this.name = name;
    this.sayHi = function(){
        console.log('hi');
    }
}
var iphone = new Apple('iphone12');
console.log(iphone.name);
console.log(iphone['name']);
```

### 调用对象

```javascript
console.log(obj.uname); // 调用对象的属性
obj.sayHi() // 调用对象的方法
```

### 构造函数和对象的区别

- 构造函数：抽取了对象的公共部分，封装到了函数里它泛指了一大类
- 创建对象：特指某一个，通过 new 关键字创建对象的过程我们也称为对象实例化

### new 关键字的执行逻辑

1. new 构造函数可以在内存中创建一个空的对象
2. this 就会指向刚才创建的空对象
3. 执行构造函数里的代码，给空对象添加属性和方法
4. 返回这个对象

### 遍历对象

使用 for…in 遍历对象

```javascript
for(var k in obj){
    console.log(k); // 得到的是属性名和方法名
    console.log(obj[k]) // 得到的是属性值
}
```

