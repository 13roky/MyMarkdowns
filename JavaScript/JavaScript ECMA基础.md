[TOC]

## 注释

---

在 JavaScript 中有2种注释：

1. 单行注释：`// 默认快捷键 ctrl + /`

2. 多行注释：`/* 默认快捷键 shift + alt + a */`

---

## 输入输出语句

---

| 方法               | 说明              | 归属  |
| ---------------- | --------------- | --- |
| alert(msg)       | 浏览器弹出警示框        | 浏览器 |
| console.log(msg) | 浏览器控制台打印输出信息    | 浏览器 |
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

## 数据类型

---

JavaScript 中的数据类型类似于 Python，都是一种弱类型。他们在声明变量时，不用声明数据类型。而是在程序运行的过程中，数据类型会被自动确认。

在运行代码时，变量的数据类型是由 JS引擎根据 = 右边变量值的数据类型来判断的，运行完毕后，变量就确定了数据类型。

数据类型的分类：

1. 简单数据类型：Number，String，Boolean，Undefined，Null
2. 复杂数据类型：Object

**简单数据类型**

| 简单数据类型    | 说明                         | 默认值       |
| --------- | -------------------------- | --------- |
| Number    | 数字型，包含整型和浮点型               | 0         |
| Boolean   | 布尔值型，true / false          | false     |
| String    | 字符串型 字符串需要带引号              | “”        |
| Undefined | 声明了变量但是没有负值此时变量就为Undefined | undefined |
| Null      | 声明变量为空值                    | null      |

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

| 转义符 | 说明               |
| --- | ---------------- |
| \n  | 换行               |
| \\\ | 斜杠\              |
| \\' | 单引号              |
| \\” | 双引号              |
| \t  | tab 缩进           |
| \b  | 空格，b 是 blank 的意思 |

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

| 方式            | 说明             | 案例                                  |
| ------------- | -------------- | ----------------------------------- |
| toString()    | 转字符串           | var num = 1; alert(num.toString()); |
| String() 强制转换 | 转字符串           | var num = 1; alter(String(num))     |
| **加号拼接字符串**   | 和字符串拼接的结果都是字符串 | var num = 1; alter(num + ‘’)        |

**转换为数字型**

| 方式                    | 说明                  | 案例                 |
| --------------------- | ------------------- | ------------------ |
| parseInt(string) 函数   | 将string类型转换为整数数值型   | parseInt(‘78’)     |
| parseFloat(string) 函数 | 将string类型转换为浮点数数值型  | parseFloat(‘3,14’) |
| Number() 强制转换函数       | 将string类型转换为数值型     | Number(‘12’)       |
| js 隐式转换 （- * /）       | 利用算数运算隐式转换 (注意不能用+) | ‘12’ - 0           |

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

| 运算符 | 描述  | 实例                    |
| --- | --- | --------------------- |
| +   | 加   |                       |
| -   | 减   |                       |
| *   | 乘   |                       |
| /   | 除   |                       |
| %   | 取余  | 5 % 3 = 2; 3 % 5 = 3; |

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

| 运算符     | 说明             | 案例  | 结果  |
| ------- | -------------- | --- | --- |
| <       | 小于             |     |     |
| >       | 大于             |     |     |
| >=      | 大于等于           |     |     |
| <=      | 小于等于           |     |     |
| ==      | 等于             |     |     |
| !=      | 不等于            |     |     |
| === !== | 全等 要求值和数据类型都一致 |     |     |

### 逻辑运算符

| 逻辑运算符 | 说明              | 案例  |
| ----- | --------------- | --- |
| &&    | “逻辑与”，简称“与” and |     |
| \|\|  | ”逻辑或“，简称”或“ or  |     |
| !     | ”逻辑非“，简称“非” not |     |

**短路运算（逻辑中断）**

原理：当有多个表达式（值）时，左边的表达式值可以确定结果时，就不再继续运算右边的表达式的值；

- 逻辑与

​    如果第一个表达式的值为真，则返回表达式2

​    如果第一个表达式的值为假，则返回表达式1

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

| 赋值运算符    | 说明  | 案例  |
| -------- | --- | --- |
| =        |     |     |
| +=、-=    |     |     |
| *=、/=、%= |     |     |

### 运算优先级

| 优先级 | 运算符   | 顺序             |
| --- | ----- | -------------- |
| 1   | 小括号   | ()             |
| 2   | 一元运算符 | ++ -- !        |
| 3   | 算数运算符 | 先乘除后加减         |
| 4   | 关系运算符 | > >= < <=      |
| 5   | 相等运算符 | == ！= === ！=== |
| 6   | 逻辑运算符 | 先&&后\|\|       |
| 7   | 赋值运算符 | =              |
| 8   | 逗号运算符 | ，              |

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

## 内置对象

在 JS 中除了我们自己定义创建的对象以外，还存在一些 JS 自带的对象，这为我们提供了一些常用的功能，我们直接拿来使用即可。

### Math 对象

Math 对象不是一个构造函数，不需要我们使用 new 去创建，我们直接只用里面的属性和方法即可。

这里介绍几个常用的属性和方法，具体属性和方法可以查看 [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math)

#### 圆周率 Math.PI

这个是就是我们的圆周率 Π

```javascript
console.log(Math.PI)
```

#### 最大值和最小值

求最大值和最小值。

如果给定的参数中至少有一个参数无法被转换成数字，则会返回 NaN。

如果没有参数，则结果为 -Infinity。

```javascript
console.log(Math.max(1, 3, 2));
console.log(Math.min(1, 3, 2));
```

#### 向下取整和向上取整

```javascript
Math.floor() // 向下取整
Math.ceil() // 向上取整
```

#### 四舍五入取整

```javascript
Math.round() // 四舍五入，注意 当为-3.5时是，-3
```

#### 绝对值

```javascript
Math.abs()    // 绝对值 
// 存在隐士转换，会把字符型能转换成数字的转换为数字型
```

#### 随机数（重点）

`Math.random()`函数返回一个浮点，伪随机数在范围 [0,1)，我们可以对其进行缩放，得到我们想要的范围。

```javascript
// 取到 [min,max) 范围
Math.random() * (max - min) + min;
// 取到 [min,max) 的整数
Math.floor(Math.random() * (max - min)) + min;
// 取到 [min,max] 的整数
Math.floor(Math.random() * (max - min + 1)) + min;
```

### Date 日期对象

Date 是一个构造函数，我们只能通过 Date 构造函数来实例化对象。

具体使用产看 [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)

```javascript
// 如果没有参数，返回系统的当前时间
var date = new Date();

// 如果有参数，则返回参数的格式化时间，支持一下参数写法
var birthday = new Date('December 17, 1995 03:24:00');
var birthday = new Date('1995-12-17T03:24:00');
var birthday = new Date(1995, 11, 17); // 从0开始数，这里是1995年 12月 17日
var birthday = new Date(1995, 11, 17, 3, 24, 0);

console.log(Date.now())    // 输出自Unix诞生到现在的毫秒数
console.log(Date.parse('04 Dec 1995 00:12:00 GMT')) // 输出自Unix诞生到指定时间的毫秒数
console.log(date.getFullYear()) // 获得 date 的年
console.log(date.getMonth()+1)    // 获得 date 的月。由于月从0算起，所以加一
console.log(date.getDate())    // 获得 date 的日。
console.log(date.getHours()) // 获得 date 的时。
console.log(date.getMinutes())    // 获得 date 的分。
console.log(date.getSeconds())    // 获得 date 的秒。
console.log(date.getDay()) // 获得 date 时当前周的第几天，从0其，0是第一天。
// 同理如果get换为set就是设置
date.setMonth(0) // 设置date月份为1月
```

### Array 数组对象

JavaScript 的 **`Array`** 对象是用于构造数组的全局对象，数组是类似于列表的高阶对象。

详情使用方法请查看 [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)。

#### 数组的两种创建方式

```javascript
// 通过字面量创建数组
var arr = [1,2,3];
// 通过构造函数创建
var arr = new Array(3);    // 创建了一个长度为3的空数组
var arr = new Array(1,2,3) // 创建了一个数组为[1,2,3],等价于第一种方法；这种写法必须是两个元素以上才行。
```

### 检测数组的两种方式

```javascript
// instanceof 运算符用来检测是否为数组 
console.log([1,2] instanceof Array);    // 同理可以用于检测其他类型
// Array.isArray() 方法
console.log(Array.isArray(arr))
```

#### 添加元素

```javascript
var arr = [1,2,3];
// 使用push()进行尾插入
arr.push(4，5);    // 将4，5尾插入数组arr，返回新数组的长度，原数组会发生变化
// 使用unshift()进行首插入
arr.unshift(-1.0)    // 将-1,0首插入数组arr，返回新数组的长度，原数组会发生变化
```

#### 删除数组元素

```javascript
var arr = [1,2,3,4,5,6];
// 使用pop()进行尾删除，每次只能删除一个元素，返回的是删除的元素，原数组发生变化
arr.pop();
// 使用shift()进行首删除，每次只能删除一个元素，返回的是删除的元素，原数组发生变化
arr.shift();
```

#### 数组排序

```javascript
const array1 = ['one', 'two', 'three'];
// reverse()翻转数组，不改变原数组
const reversed = array1.reverse();    //out:"reversed:" Array ["three", "two", "one"]
// sort()对数组进行排序 冒泡方法，不改变原数组
var arr = [1, 30, 'March', 'Jan', 'Feb', 'Dec',4, 21, 100000];
console.log(arr.sort()); // [1, 100000, 21, 30, 4, "Dec", "Feb", "Jan", "March"] 这种不带参数只根据第一个字符进行排序
// 升序排列
console.log(arr.sort(function(a,b){
    return a-b;
}));
// 降序排列
console.log(arr.sort(function(a,b){
    return b-a;
}));
```

#### 数组索引的获取

```javascript
var arr = [1,2,3,4,5,1,2];
// indexOf()从前往后查找
console.log(arr.indexOf(2));    // 返回数组中指定元素的第一个索引
// lastIndexOf()从后往前查找
console.log(arr.lastIndexOf(2));
```

### 数组去重（重点）

自写函数

```javascript
function unique(arr){
    var newArr = [];
    for (var i = 0; i < arr.length; i++) {
        if (newArr.indexOf(arr[i]) === -1) {
            newArr.push(arr[i]);
        }
    }
    return newArr;
}
```

利用`Set`对象

```javascript
var arr = [1,2,3,1,2,3,1,2,3];
var set = new Set(arr);
console.log(set);
```

#### 数组转字符串

```javascript
var arr = [1,2,3];
// toString()转换字符串，以逗号分隔
console.log(arr.toString());
// join()转换为字符串，默认为逗号，可以指定分隔符
console.log(arr.join('&'));
```

#### 指定返回对象

```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];
// 使用slice()得到数组，不会改变原数组
console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]
console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]
console.log(animals.slice(-2));
// expected output: Array ["duck", "elephant"]
console.log(animals.slice(2, -1));
// expected output: Array ["camel", "duck"]
// 注意是左闭右开
```

#### 在指定位置删除和添加元素

```javascript
const months = ['Jan', 'March', 'April', 'June'];
// splice(start,del,element) star:从第几个元素开始 del：向后删除几个元素，element:在这个位置前面添加element。 改变原数组
months.splice(1, 0, 'Feb'); // 从index为1的元素March开始，删除0个元素，在其前面插入Feb
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "June"]
months.splice(4, 1, 'May'); // 从index为4的元素开始，删除1个元素，在前面插入May
// replaces 1 element at index 4
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "May"]
months.splice(4, 1); // 从index为4的元素开始，删除1个元素.
// expected output: Array ["Jan", "Feb", "March", "April"]
```

#### 合并数组

```javascript
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2,array2); // 不改变原数组
console.log(array3);
// > Array ["a", "b", "c", "d", "e", "f", "d", "e", "f"]
```

### String 字符串对象

**`String`** 全局对象是一个用于字符串或一个字符序列的构造函数。

#### 基本包装类型

只有对象才会有属性和方法 这是复杂数据类型才会有的。

而字符串是简单数据类型，但是可以**通过基本包装类型。将其转换为复杂数据类型**。

**基本包装类型就是，把简单数据类型包装称为复杂数据类型，这样基本数据类型就有了属性和方法**。

```javascript
var str = 'andy';
console.log(str.length);

// 上面两个代码的内部执行逻辑为
// 1. 把简单数据类型转换为复杂数据类型
var temp = new String('andy');
// 2. 把临时变量的值给 str, 这样str就成为了一个对象
str = temp;
// 3. 销毁临时变量
temp = null;
```

#### 字符串的不可变性

```javascript
// 在内存中开辟了一个内存空间用来存储andy，让声明的str指向andy的内存地址
var str = 'andy';
// 在内存中又新开辟了一个内存空间来存储red，这是改变str指向red的内存地址，但是andy依然存在于内存中，其内存地址依然存在，并不会进行销毁
str = 'red';
// 上面过程就是字符串的不可变性，所以我们尽量避免大量的字符串拼接操作
```

#### 根据字符返回位置

字符串的所有方法，都不会改变字符串本身（字符串是不可变的），操作完成会返回一个新的字符串。

```javascript
var str = 'brokyzbrokyz';
console.log(str.indexOf('k'));    // 3,只输出第一个
console.log(str.indexOf('k',4))    // 9, 指定从3后开始查找
```

查找所有位置

```javascript
var str = 'brokyzbrokyzbrokyz';
var index = str.indexOf('b');
var num = 0;
while(index !== -1) {
    num++;
    console.log(index);
    index = str.indexOf('o',index+1)
}
console.log(num);
```

#### 根据位置返回字符

```javascript
var str = 'andy';
// charAt() 根据位置返回索引号
console.log(str.charAt(3));
// 遍历字符
for(var i = 0; i < str.length; i++) {
    console.log(str.charAt(i));
}
// charCodeAt(index) 返回相应索引号的字符的ASCLL值 常用于判断用户按下了哪个按键
console.log(str.charCodeAt(3))

// H5新增
console.log(str[3])
```

#### 拼接和截取字符串

| 方法名                       | 说明                                          |
| ------------------------- | ------------------------------------------- |
| concat(str1,str2,str3...) | concat() 方法用于连接两个或多个字符串。拼接字符串，等效于+，+更常用     |
| substr(start,length)      | 从start开始，向后去length个个数                       |
| slice(start,end)          | 从start开始，截取到end位置，end取不到                    |
| substring(start,end)      | 从start开始，截取到end位置，end取不到，基本和slice相同，但是不支持负号 |

#### 替换字符串

```javascript
// 使用replace('被替换的字符','替换的字符') 只会替换第一个字符
var str = 'brokyzbrokyz';
console.log(str.replace('z','s'));
// 替换所有
while (str.indexOf('z') !=== -1) {
    str = str.replace('z','s');
}
```

#### 字符串转换为数组

```javascript
var str = '1,2,3,4,5';
// 使用split('分隔符') 转换为数组
console.log(str.split(','));
```

#### 大小写转换

```javascript
// toLowerCase() 将字符串转换为小写
console.log('中文简体 zh-CN || zh-Hans'.toLowerCase());    // 中文简体 zh-cn || zh-hans
// toupperCase() 将字符串转换为大写
console.log('The quick brown fox jumps over the lazy dog.'.toUpperCase());
// expected output: "THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG."
```

## JS 简单数据类型和复杂数据类型

简单数据类型又叫做基本数据类型或者**值类型**，复杂数据类型又叫做**引用类型**。

- 值类型：简单数据类型，在存储时变量中存储的时值本身，因此叫做值类型。
  - string，number，boolean，undefined，null
  - 简单数据类型null，返回的时一个空对象 object。如果有个变量我们以后打算存储为对象，但是现在不需要，这个时候就可以赋值为null。
- 引用类型：复杂数据类型，在存储时变量中存储的仅仅是地址（引用），因此叫做引用类型。
  - 通过 new 关键字创建的对象（内置对象、自定义对象等），如 Object、Array、Date等。

### 堆和栈

1. 栈（操作系统）：由操作系统自动分配释放存放函数的参数值、局部变量的值等。其操作方式类似于数据结构中的栈。

**简单数据类型存在在栈里**

2. 堆（操作系统）：存储复杂类型（对象），一般由程序员分配释放，若程序员不释放，由垃圾回收机制回收。

**复杂数据类型存放到堆里面**

**注意：JS 中没有堆栈的概念，通过堆栈的方式们可以让大家更容易理解代码的一些执行方式，便于将来学习其他语言。**

### 简单数据类型的内存分配

值类型变量的数据直接存放在变量（栈空间）中

比如`var age = 18;`

会在栈中开辟一个内存空间，里面存放了18这个值，然后让 age指向18的内存地址。

### 复杂数据类型的内存分配

复杂数据类型的存储和栈堆都有关系。

比如 `var arr = [1,2,3];`

会在堆里面开辟一个空间，存放了[1,2,3]，然后在栈里面会存放堆的内存地址，之后 arr 会指向栈里面的内存地址，然后由栈中的内存地址指向堆里面存放的数据。

### 简单数据类型传参

简单类型传参同一变量不会开辟新的内存地址。

比如`var a = 1; a = 2;`

会在栈中开辟一个内存空间用来存放1这个值，然后由a指向栈中1对应的内存地址。当让a变为2时，并不会开辟新的内存空间来存放2，而是a指向栈中的内存地址不发生改变，其内存地址存放的值由1改为2。

但是如果是string，就得考虑字符串的不可变性了。

### 复杂数据类型传参

有如下代码：

```javascript
function Person(name) {
    this.name = name;
}

funtion f1(x) {
    console.log(x.name);
    x.name = "Z";
    console.log(x.name);
}

var p = new Person("brokyz");
console.log(p.name);
f1(p);
console.log(p.name);
```

![](https://pic.imgdb.cn/item/62c011705be16ec74a594175.jpg)

1. 首先通过`new`关键字给变量`p`创建了一个`Person`对象，初始化`name = 'brokyz'`。这时在堆中开辟了一个内存空间，将创建的`Person`对象存在堆中，在栈中开辟了一个内存空间，这个内存空间中存放了堆中`Person`的内存地址的16进制的值，然后由变量`p`指向栈中存放`Person`的内存地址值。这样一来变量`p`先是指向了栈中存放的堆内存地址值，然后这个值指向了堆中的`Person`对象。
2. 当我们调用函数`f1(p)`时，这时将实参`p`传递给形参`x`。这时在栈中重新开辟了一个内存地址，存放了和实参`p`一样的值，然后形参`x`指向这个栈中新开辟的值。也就是说，此时`p`和`x`指向的是栈中不同的内存地址，但是内存地址的存放的值都是指向同一个`Person`的堆内存地址。在函数中通过`x.name = 'z'`修改了`Person`的属性值，是`p`和`x`同时指向的`Person`，所以这时候输出`p.name`时，其属性也发生了改变。
