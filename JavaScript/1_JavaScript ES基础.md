# JS 基础

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
var arr = [1, 2, 3, 1, 2, 3, 1, 2, 3]
var set = new Set(arr)
// arr = new Array(...set)
arr = [...set]
console.log(arr)
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

# JS 高级

## 构造函数和原型

### ES5创建对象的方法

- 在ES6之前，如果想创建对象有三种方式：
  - 通过字面量`{}`创建对象
  - 利用`new Object`创建对象
  - 使用构造函数创建对象

```js
// 1. 通过字面量{}创建对象
var obj = {
    uname: 'brokyz',
    agr: 18,
    sex: 'male',
    sayHi: function(){
        console.log('hi');
    }
} 
// 2. 利用new Object创建对象
var obj = new Object();
obj.uname = 'brokyz';
obj.age = 18;
obj.sex = 'male';
obj.sayHi = function(){
    console.log('hi');
}
// 3. ES5使用构造函数创建对象
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

### new在执行时会做四件事

1. 在内存中创建一个新的空对象。
2. 让`this`指向这个新的对象。
3. 执行构造函数里面的代码，给这个新的对象添加属性和方法。
4. 返回这个新的对象（所以构造函数里面不需要`return`。

### 实例成员和静态成员

1. 实例成员：在构造函数内部通过`this`来添加的成员。只能通过实例化的对象来访问。
2. 静态成员：在构造函数本身上添加的成员。只能通过构造函数来访问。

```js
function Apple(name){
    // 实例成员
    this.name = name;
    this.sayHi = function(){
        console.log('hi');
    }
}
// 实例成员只能通过实例化对象来访问
console.log(new Apple('brokyz').name);
// 静态成员需要在构造函数本身上添加
Apple.age = 15;
// 只能通过构造函数来访问
console.log(Apple.age);
```

### 构造函数的资源浪费问题

```js
function Apple(name){
    this.name = name;
    this.sayHi = function(){
        console.log('hi');
    }
}
var apple1 = new Apple('iphone12');
var apple2 = new Apple('iphone13');
console.log(apple1.sayHi === apple2.sayHi);  // false
```

- 构造函数中的方法`sayHi`属于复杂数据类型，创建实例时会在内存中单独开辟空间来存放。
- 实例化对象之间并不能公用相同的方法。所以实例化对象会分别在内存中开辟空间来存放自己的方法。
- 比如`apple1`在内存中有一个自己的`sayHi`方法。当继续实例化`apple2`时，构造函数会为其在内存开辟一个新的`sayHi`方法。

### 构造函数原型对象prototype

- 解决了构造函数的资源浪费问题。
- 构造函数通过原型分配的函数时所有对象**共享的**。
- 在JS中，**每一个构造函数都有一个`prototype`属性**，指向另一个对象。注意这个`prototype`就是一个对象，这个对象的所有属性和方法，都会被构造函数所拥有。
- **我们可以把不变的方法，直接定义在`prototype`上，这样所有对象的实例就可以共享这些方法**。这样一来所有的实例化对象，都不用被独自开辟方法，可以共同去`ptototype`中寻找使用方法。

```js
function Apple(name){
    this.name = name;
}
Apple.prototype.sayHi = function(){
    console.log('hi');
}
var apple1 = new Apple('iphone12');
var apple2 = new Apple('iphone13');
console.log(apple1.sayHi === apple2.sayHi); // true
console.log(apple1._proto_ === Apple.prototype); // true
```

> 为什么方法定义在构造函数的prototype上,而实例化对象可以使用

- 在实例化对象身上还有`__proto__`属性指向构造函数的`prototype`原型对象，我们之所以可以使用构造函数`prototype`原型对象的属性和方法，就时因为对象有`_proto_`原型的存在。
- `__proto__`**对象原型**和**原型对象**`prototype`时等价的。

![](https://pic.imgdb.cn/item/62db60fef54cd3f937e37c03.jpg)

### constructor构造函数

- 对象原型(`_proto`)和构造函数(`prototype`)原型对象里面都有一个`constructor`属性，我们将其称为构造函数，因为他会指回构造函数本身。
- `constructor`主要用于记录该对象引用哪个构造函数，它可以让原型对象重新指向原来的构造函数。

```js
function Apple(name){
    this.name = name;
}
Apple.prototype.sayHi = function(){
    console.log('hi');
}
var apple1 = new Apple('iphone12');
var apple2 = new Apple('iphone13');
console.log(Apple.prototype);	// 里面存在constructor属性
console.log(apple1.__proto__);	// 里面存在constructor属性
console.log(Apple.prototype.constructor);	// 指向构造函数本身Apple(name){...}
console.log(apple1.__proto__.constructor);	// 指向构造函数本身Apple(name){...}
```

- 我们可以在原型对象中直接赋值添加方法**会覆盖掉**`constructor`，这时我们需要手动指会构造函数。

```js
function Apple(name){
    this.name = name;
}
Apple.prototype = {
    // 手动指回
    constructor: Apple,
    sayHi: function() {
        console.log('hi');
    },
    open: function() {
        console.log('open');
    }
}
var apple1 = new Apple('iphone12');
var apple2 = new Apple('iphone13');

console.log(Apple.prototype);	// 如果没有手动指回，里面的constructor被覆盖掉，所以不存在constructor
console.log(apple1.__proto__);	// 如果没有手动指回，里面的constructor被覆盖掉，所以不存在constructor
console.log(Apple.prototype.constructor);
console.log(apple1.__proto__.constructor);
```

总结：如果我们修改了原来的原型对象，修改时如果以对象的形式来进行赋值的话，必须手动利`constructor`指向回原来的构造函数。

### 原型链

![](https://pic.imgdb.cn/item/62db608bf54cd3f937e13c68.jpg)

### JS成员查找机制

1. 当访问一个对象的属性（包括方法）时，首先查找这个**对象自身**有没有该属性。
2. 如果没有就查找它的原型（也就是`__proto__`指向的`prototype`**原型对象**）。
3. 如果还有没就查找原型对象的原型（Object的原型对象）。
4. 依此类推一直找到Object为止（null）。

### 原型对象中this的指向

- 构造函数和原型对象中，`this`都指向实例对象。

```js
function Apple(name){
    this.name = name;
}
let that;
Apple.prototype.sayHi = function(){
    that = this;
    console.log('hi');
}
var apple1 = new Apple('iphone12');
apple1.sayHi();
console.log(that === apple1); // true
```

### 原型对象扩展内置对象

- 我们可以通过原型对象，对原来的内置对象进行拓展自定义的方法。比如给数组增加自定义求偶数和的功能。

```js
// 因为数组对象中并没有内置求和方法，所以我们可以通过原型对象进行添加
Array.prototype.sum = function() {
    var sum = 0;
    for (let i = 0; i < this.length; i++) {
        sum += this[i];
    }
    return sum;
    let arr = [1, 2, 3];
    console.log(arr.sum()); // 返回求和结果6
}
```

注意：

1. 通过`Array.prototype.sum = function()`方法添加的时在原型对象中进行追加。不会覆盖掉原来的原型对象。
2. 如果使用`Array.prototype = {}`方法进行添加时，这里时重新定义整个原型对象，会覆盖掉原来所有的方法和属性如排序和反转数组。

**所以对内置对象添加新的方法只能通过第一种方式添加，第二种方式会覆盖掉本来的原型对象中的方法**

## 继承

ES6之前并没有给我们提供extends继承。我们可以通过**构造函数+原型对象**模拟实现继承，被称为**组合继承**。

### call()

调用这个函数，并且修改函数运行时的this指向

```js
fun.call(thisArg, arg1, arg2, ...)
```

- thisArg：当前调用函数this的指向对象
- arg1, arg2：传递的其他参数

```js
function fn(x, y) {
    console.log('fun');
    console.log('this');
    console.log(x + y);
}
var o = {
    name: 'andy'
};
fn();
// 1. call() 可以调用函数
fn.call();
// 2. call() 可以改变这个函数的this指向 此时这个函数的this 就指向了0这个对象
fn.call(o, 1, 1);
```

### 借用父构造函数继承属性

```js
function Father(uname,age){
    this.uname = uname;
    this.age = age;
}

function Son(uname, age, score){
    // 调用Father将里面的Father中的this指向为Son中的this，本质指向了实例对象
    Father.call(this, uname, age);
    this.score = score;
}
var son = new Son('brokyz', 18);
console.log(son);
```

- 在Son中通过call()调用了Father，将Father中的this指向了Son中的this，也就是实例对象son，将相应参数传入调用的Father，这样就可以利用Father生成相应的属性，实现了函数的继承。
- 注意这种方法是继承属性，不是继承方法。方法的继承需要借用原型对象继承。

### 借用原型对象继承方法

```js
function Father(uname, age) {
    this.uname = uname;
    this.age = age;
}

function Son(uname, age, score) {
    // 调用Father将里面的Father中的this指向为Son中的this，本质指向了实例对象
    Father.call(this, uname, age);
    this.score = score;
}
// Son.prototype = Father.prototype; 
// 浅拷贝这样直接赋值会有问题，如果修改了子原型对象，父原型对象也会跟着修改
// 让Son的原型对象指向Father的一个实例对象，Father的实例可以使用其原型对象中的方法，这样就实现了方法的继承
Son.prototype = new Father();
// 如果利用对象的形式修改了原型对象，别忘了利用constructor指回原来的原型对象
// 因为这里是用Father实例对象覆盖了Son的原型对象
Son.prototype.consturctor = Son;
// 子构造函数专门的方法
Son.prototype.exam = function () {
    console.log('exam');
};
Father.prototype.money = function() {
    console.log('money');
}
var son = new Son('brokyz', 18, 100);
console.log(son);
son.money()
```

### 原理

![](https://pic.imgdb.cn/item/62de59f9f54cd3f9370034f9.jpg)

## ES5中的新增方法

### 数组方法

迭代（遍历）方法：forEach()、map()、filter()、some()、every()

#### forEach()

```js
array.forEach(function(currentValue, index, arr))
```

- currentValue：数组当前项的值
- index：数组当前项索引
- arr：数组对象本身

```js
var arr = [1, 2, 3];
arr.forEach(function(value, index, arr) {
    console.log('数组元素:' + value);
    console.log('数组索引:' + index);
    console.log('数组本身:' + arr);
})
```

#### filter()

```js
array.filter(function(currentValue, index, arr))
```

- filter()方法创建一个新的数组，新数组中的元素是同过检查指定数组中符合条件的所有元素，**主要用于筛选数组**
- **注意它直接返回一个新数组**
- currentValue：数组当前项的值
- index：数组当前项的索引
- arr：数组对象本身

```js
var arr = [12,66,4,88];
var newArr = arr.filter(function (value, index, arr) {
    return value >= 20;
})
console.log(newArr);
```

#### some()

```js
array.some(function(currentValue, index, arr))
```

- some()方法用于检测数组中的元素是否满足指定条件。就是查找数组中是否有满足条件的元素。
- **注意它的返回值是布尔值，如果查找这个元素，就返回true，如果查不到就返回false。**
- 如果找到第一个满足条件的元素，则终止循环不在继续查找。
- currentValue：数组房前项的值
- index：数组当前项的索引
- arr：数组对象本身

```js
var arr = [12, 66, 4, 88];
var flag = arr.some(function (value, index, arr) {
    return value < 2;
});
console.log(flag);
```

#### some和forEach的区别

- 在forEach()里面，**遇到了return会跳出本次循环但是继续下一次迭代（filter和forEach相同）**

```js
var arr = ['pink', 'green', 'red', 'blue'];
arr.forEach(function (value) {
    if (value == 'green') {
        console.log('找到了元素');
        return true;    // 在forEach里面，遇到了return会跳出本次循环但是继续下一次迭代
    }
    console.log(22);
})
```

- **在some()里面，遇到了return true就是终止遍历 迭代效率更高**

```js
var arr = ['pink', 'green', 'red', 'blue'];
arr.some(function (value) {
    if (value == 'green') {
        console.log('找到了元素');
        return true;    // 在some里面，遇到了return true就是终止遍历 迭代效率更高
    }
    console.log(11);
});
```

### trim()去除字符串空格

- trim()可以去除两侧空格

```js
var str = '  an  dy  ';
console.log(str);
var str1 = str.trim();
console.log(str1);
```

### 对象方法

#### Object.keys()

遍历对象中的属性和方法

```js
Object.keys(obj)
```

#### Object.defineProperty()

定义对象中新属性和修改原有的属性。

```js
Obeject.defineProperty(obj, prop, desriptor)
```

- obj：必须。目标对象
- prop：必须。需定义或修改的属性的名字
- descriptor：必须。目标属性所拥有的特性
  - value：设置属性的值 默认为undefined
  - writable：值是否可以重写。true|flase 默认为false
  - enumerable：目标属性是否可以被枚举。true|false 默认为false
  - configurable：目标属性是否可以被删除或是否可以再次修改特性。true|false 默认为false

```js
var obj = {
    id: 1,
    pname: '小米',
    price: 1999
};
// 1. 以前的对象添加和修改属性的方式
// obj.num = 1000;
// obj.price = 99;
// 2. Object.defineProperty() 定义新属性或修改原有的属性
Object.defineProperty(obj, 'num', {
    value: 1000
});
Object.defineProperty(obj, 'price', {
    value: 9.9
});
Object.defineProperty(obj, 'id', {
    writable: false
});
Object.defineProperty(obj, 'address', {
    value: 'beijing',
    writable: false,
    enumerable: false,
    configurable: false // 如果为false则不允许删除属性，不允许再修改特性。
});
delete obj.address;
obj.id = 2;
console.log(obj);
console.log(Object.keys(obj)); // ['id', 'pname', 'price']
// 通过defineProperty添加的属性才会默认带有enumerable: false
```

## 函数进阶

### 函数的定义与调用

1. 函数声明方式function关键字（命名函数）

```js
function fn(){}
```

2. 函数表达式（匿名函数）

```js
var fn = function(){}
```

3. new Function()   不常用效率低

```js
var fn = new Function('a','b', 'console.log(a+b)');
fn(1,2);
console.dir(fn);
console.log(fn intanceof Object);
```

- Function里面参数都必须是字符串格式
- 第三种方式执行效率低，也不方便书写，因此较少使用
- 所有函数都是Function实例（对象）
- 函数也属于对象

### 函数的调用方式

1. 普通函数

```js
function fn() {
    console.log('test');
}
fn();
```

2. 对象的方法

```js
var o = {
    sayHi: function() {
        console.log('test');
    }
}
o.sayHi();
```

3. 构造函数

```js
function Star() {
    
}
new Star();
```

4. 绑定事件函数

```js
btn.onclick = function() {
    // 点击了按钮可以调用这个函数
}
```

5. 定时器函数

```js
setInterval(function(){
    // 定时器自动1秒执行一次
}, 1000)
```

6. 立即执行函数

```js
(function(){
    console.log('test');
})(); // 立即执行函数会自动调用
```

## 函数内部的this指向问题

| 调用方式     | this指向                                   |
| ------------ | ------------------------------------------ |
| 普通函数调用 | window                                     |
| 构造函数调用 | 实例对象，原型对象里面的方法也指向实例对象 |
| 对象方法调用 | 该方法所属对象                             |
| 事件绑定方法 | 绑定事件对象                               |
| 定时器函数   | window                                     |
| 立即执行函数 | window                                     |

1. 普通函数 this指向调用者

```js
function fn() {
    console.log(this);
}
fn(); // 指向window，因为这是window.fn()的缩写
```

2. 对象方法 this指向调用者，一般为window

```js
var o = {
    sayHi: function() {
        console.log(this);
    }
}
o.sayHi(); //指向对象o
```

3. 构造函数和原型对象 this指向调用它的实例对象

```js
var that1;
var that2;
function Star() {
    that1 = this; 
}
Star.prototype.sing = function() {
    that2 = this;
}
var brokyz = new Star();
brokyz.sing();
console.log(that1);
console.log(that2);
```

4. 绑定事件函数 this指向调用者

```js
btn.onclick = function() {
    console.log(this);
}
```

5. 定时器中的this也是执行window

```js
setInterval(function(){
    console.log(this);
}, 1000)
// 指向window因为是 window.setInterval()的缩写
```

6. 立即执行函数 中的this指向window，类似于普通函数，只不过不需要我们手动调用

```js
(function(){
    console.log(this);
})(); // 立即执行函数会自动调用
```

### 改变函数内部this指向

JS为我们专门提供了一些函数方法来帮助我们更优雅的处理函数内部this的指向问题，常用的有bind()、call()、apply()三种方法

#### call()

调用这个函数，并且修改函数运行时的this指向

```js
fun.call(thisArg, arg1, arg2, ...)
```

- thisArg：当前调用函数this的指向对象
- arg1, arg2：传递的其他参数

```js
function fn(x, y) {
    console.log('fun');
    console.log('this');
    console.log(x + y);
}
var o = {
    name: 'andy'
};
fn();
// 1. call() 可以调用函数
fn.call();
// 2. call() 可以改变这个函数的this指向 此时这个函数的this 就指向了0这个对象
fn.call(o, 1, 1);
```

#### apply()

aplly()方法调用一个函数。简单理解为调用函数的方式，但是它可以改变函数的this指向。

```js
fun.apply(thisArg, [argsArray])
```

- thisArg：在fun函数运行时指定的this值
- argsArray：传递的值，必须包含在数组里面
- 返回值就是函数的返回值，因为它就是调用函数

```js
var o = {
    name: 'brokyz'
}
function fn(arr) {
    console.log(this);
    console.log(arr);
}
fn.apply(o, ['pink']);
// 1. 调用函数 第二个可以改变函数内部的this指向
// 2. 但是他的参数必须是数组(伪数组)
// 3. apply的主要应用 比如我们可以利用aplly借助于数学内置对象求最大值
var arr = [1,66,3,99,4];
var max = Math.max.apply(Math, arr)
```

#### bind()

bind()方法不会调用函数。但是能够改变函数内部的this指向

```js
fun.bind(thisArg, arg1, arg2, ...)
```

- thisArg：在fun函数运行时指定的this值
- arg1，arg2：传递的其他参数
- 返回由指定的this值和初始化参数改造的**原函数拷贝**。也就是说返回的时改完的新新函数

```js
var o = {
    name: 'brokyz'
}
function fn() {
    console.log(this);
}
var f = fn.bind(0);
f();
// 1. 不会调用原来的函数
// 2. 可以改变原来的函数内部的this指向
// 3. 返回的是原函数改变this之后的新函数
```

例子：当点击按钮之后就禁用这个按钮，3秒后自动开启

```js
var btn = document.querySeletor('button');
btn.onlcick = function() {
    this.disabled = true;
    setTimeout(function(){
        this.disabled = false; // this指向的是window，所以要用bind
    }.bind(this), 3000)
}
```

#### 三者的总结

相同点：

都可以改变函数内部的this指向。

区别：

1.  call和apply会调用函数，并且改变函数内部this的指向。
2. call和apply传递的参数不一样，call传递参数arg1，arg2，...的形式，apply必须数组形式[arg]
3. bind不会调用函数，可以改版函数内部this指向

使用场景：

1. call经常用作继承。
2. apply经常和数组有关系的操作。比如借助数学对象实现数组求最大值。
3. bind不调用函数，但是还想改变this的指向。比如改变定时器内部this的指向。

## 严格模式

JS除了提供正常模式外，还提供了**严格模式（strict mode）**。ES5的严格模式是采用具有限制性JS变体的一种方式，即在严格的条件下运行JS代码。

严格模式在IE10以上的版本的浏览器中才会被支持，旧版本浏览器中会被忽略。

严格模式对正常的JavaScript语义做了一些更改：

1. 消除了JS语法的一些不合理、不严谨之处，减少了一些怪异的行为。
2. 消除代码运行的一些不安全之处，保证代码运行的安全。
3. 提高编译器效率，增加运行速度。
4. 禁用了在EMCAScript的未来版本中可能会定义的一些语法，为未来新版本的JS做好铺垫。比如一些保留字如：class,enum,export,extends,super不能作为变量名。

### 开启严格模式

1. 给整个脚本添加严格模式

```html
<script>
	'use strict';
</script>
```

或：

```html
<script>
	(function(){
        'use strict';
    })();
</script>
```

2. 为某个函数开启严格模式

```html
<script>
	function fn(){
        'use strict';
    }
    function f(){
        'use strict';
    }
</script>
```

### 严格模式的变化

#### 变量规范

1. 在正常模式中，如果一个变量没有声明就赋值，默认是全局变量。严格模式禁止这种用法，变量都必须先用var命令声明，然后再使用。
2. 严禁删除已经生命好的变量。

```js
'use strict';
// 1. 变量必须先声明
// num = 10;
var num = 10;
console.log(num);
// 2. 我们不能随意删除已经声明好的变量
// delete num;
```

#### 严格模式下的this指向

1. 在正常模式下，全局作用域中的this指向window对象
2. 严格模式下，全局作用域中的this是undefined

```js
'use strict';
function fn(){
    console.log(this)
}
fn();
```

3. 正常模式下，构造函数不加new也可以调用，this指向全局对象
4. 严格模式下，构造函数如果不加new调用，this会报错
5. 如果是通过new创建构造函数，那么this依然指向实例对象

```js
'use strict';
function Star(){
    this.sex = '男'; // 在严格模式下这个this指向undefined
}
Star();
```

6. 严格模式下，定时器的this还是指向window
7. 严格模式下，事件、对象还是指向调用者

#### 函数的变化

1. 函数不允许有重名的参数。

```js
function fn(a,a){
    console.log(a+a);
}
fn(1,2); // 输出为4，以为传入完成时a为2，计算的是2+2=4。严格模式不允许这样。
```

2. 函数必须声明在顶层。新版本的JS会引入"块级作用域"（ES6已经引入）。为了与新版本接轨。不允许在非函数的代码块内声明函数。比如不允许在if和for中声明函数。但是依然可以在函数中声明函数来完成函数的嵌套。

## 高阶函数

高阶函数是对其他函数进行操作的函数，它**接受函数作为参数**或**将函数作为返回值输出**。满足其一就是高阶函数。

```js
// 将函数作为参数，可以为函数作为回调函数使用。
function fn(a,b,callback){
    console.log(a+b);
    callback&&callback();
}
fn(1,2,function(){alert('hi')})

// 将函数作为参数返回
function fn() {
    return function(){}
}
fn();
```

## 闭包

**闭包**指有权访问另一个函数作用域中变量的**函数**。------JavaScript高级程序设计

简单理解就是，有个作用域可以访问另外一个函数内部的局部变量。

**闭包的主要作用：延伸了变量的作用范围**

```js
function fn() {
    var num = 10;
    function fun() {
        console.log(num);
    }
    fun();
}
fn();
```

用fn外面的作用域访问fn内部的局部变量

```js
function fn() {
    var num = 10;
    return function() {
        console.log(num);
    }
}
var f = fn();
f();
```

- 所以闭包不及让函数内部的作用域访问局部变量，也可以使用全局作用域来访问函数的局部变量。

**闭包应用-点击li输出当前li的索引号：**

```html
<body>
    <ul class="nav">
        <li>榴莲</li>
        <li>小汉堡</li>
        <li>臭豆腐</li>
        <li>腐乳</li>
    </ul>
    <script>
        // 闭包应用-点击li输出当前li的索引号
        // 1. 我们可以利用动态添加属性的方式
        var lis = document.querySelector('.nav').querySelectorAll('li');
        // for (var i = 0; i < lis.length; i++) {
        //     lis[i].index = i;
        //     lis[i].onclick = function () {
        //         console.log(this.index);
        //     };
        // }
        // 2.利用闭包的方式得到当前小li的索引号
        for (var i = 0; i < lis.length; i++) {
            // 利用for循环创建了4个立即执行函数
            (function (i) {
                // console.log(i);
                lis[i].onclick = function () {
                    console.log(i); // 调用了立即执行函数的i，使用了闭包
                };
            })(i);
        }
    </script>
</body>
```

**闭包应用-3秒钟之后，打印所有li元素的内容：**

```html
<body>
    <ul class="nav">
        <li>榴莲</li>
        <li>小汉堡</li>
        <li>臭豆腐</li>
        <li>腐乳</li>
    </ul>
    <script>
        // 闭包应用-3秒钟之后，打印所有li元素的内容：
        var lis = document.querySelector('.nav').querySelectorAll('li');
        for (var i = 0; i < lis.length; i++) {
            // 回调函数只有触发了才会执行，这是异步任务，会先进入任务队列中等待执行
            // setTimeout(function () {
            //     console.log(lis[i]);
            // }, 2000);
            (function (i) {
                setTimeout(function () {
                    console.log(lis[i].innerHTML);
                }, 2000);
            })(i);
        }
    </script>
</body>
```

**闭包应用-计算打车价格**

```js
// 闭包应用-计算打车价格
// 打车起步价13（3公里内），之后每多一公里增加5元，用户输入公里数就可以计算打车价格
// 如果有拥堵状况，总价格多收取10块钱拥堵费
var car = (function () {
    var start = 13; // 起步价
    var total = 0;    // 总价
    return {
        price: function (n) {
            if (n <= 3) {
                total = start;
            } else {
                total = 13 + (n - 3) * 5;
            }
            return total;
        },
        yd: function (flag) {
            return flag ? total + 10 : total;
        }, // 拥堵之后的费用
    };
})();
console.log(car.price(1));
console.log(car.yd(true));
console.log(car.price(5));
console.log(car.yd(true));
```

- 由于函数price和yd使用了其他函数中的局部变量start和total，所以产生了闭包，而包含这个变量的函数就被称为闭包函数

## 递归函数

如果**一个函数在内部可以调用其本身**，那么这个函数就是**递归函数**

递归函数的作用和循环效果一样

由于递归很容易发生“栈溢出”错误（stack overflow），所以**必须要加退出条件return**

```js
// 递归函数：函数内部自己调用自己，这个函数就是递归函数
var num = 1;
function fn(){
    console.log('我要打印6句话');
    if (num == 6) {
        return; // 递归里面必须加退出条件
    }
    num++;
    fn();
}
fn();
```

**利用递归求1-n阶乘**

```js
function fn(n){
    if(n == 1) {
        return 1;
    }
    return n * fn(n-1);
}
fn(3);
// return 3 * fn(2)
// return 3 * 2 * fn(1)
// return 3 * 2 * 1
```

**利用递归函数求斐波那契数列**

```js
function fn(n) {
    if (n == 1 || n == 2){
        return 1;
    }
	return fn(n-1) + fn(n-2);
}
fn(4);
```

**根据id号返回相应的数据**

```js
var data = [{
    id: 1,
    name: '家电',
    goods: [{
        id: 11,
        gname: '冰箱',
        goods: [{
            id: 111,
            gname: '海尔'
        }]
    }, {
        id: 12,
        gname: '洗衣机'
    }]
}, {
    id: 2,
    name: '服饰'
}];
// 1. 利用 forEach去遍历里面的每一个对象
function getID(json, id) {
    var o = {};
    json.forEach(function (item) {
        if (item.id == id) {
            // console.log(item);
            o = item;
            // 2. 我们想要得到里层的数据 11 12 可以利用递归函数完成
            // 里面应该有goods数组并且数组的长度不为0
        } else if (item.goods && item.goods.length > 0) {
            o = getID(item.goods, id);
        }
    });
    return o;
}
console.log(getID(data, 11));
console.log(getID(data, 111));
```

### 浅拷贝 Object.assign()

浅拷贝只拷贝一层，更深层次对象级别的只拷贝引用

可以通过Object.assign()来实现浅拷贝

```js
Object.assign(obj2,obj1)
// 将obj1浅拷贝给obj2
```

```js
var obj = {
    id: 1,
    name: 'andy',
    msg: {
        age: 18
    }
};
var o = {};
// for(var k in obj) {
//     // k是属性名字 obj[k]是属性值
//     o[k] = obj[k];
// }
// console.log(o)
// o.msg.age = 20;
// console.log(obj)
Object.assign(o, obj);
console.log(o);
// 由于浅拷贝更深层次对象只拷贝引用，所以o修改msg.age时obj也会同时跟着修改
```

### 深拷贝

拷贝所有层

```js
var obj = {
    id: 1,
    name: 'andy',
    msg: {
        age: 18
    }
};
var o = {};
function deepCopy(newobj, oldobj) {
    // 判断我们的属性值属于哪种数据类型
    for (var k in oldobj) {
        // 1. 获取属性值
        var item = oldobj[k];
        // 2. 判断这个值是否是数组
        if (item instanceof Array) {
            newobj[k] = [];
            deepCopy(newobj[k], item);
        } else if (item instanceof Object) {
            // 3. 判断这个值是否是对象
            newobj[k] = {};
            deepCopy(newobj[k], item);
        } else {
            // 4. 属于简单数据类型
            newobj[k] = item;
        }
    }
}
deepCopy(o, obj);
console.log(o);
```

## 正则表达式

**正则表达式（Regular Expression**是用于匹配字符串中字符组合的模式。在JavaScript中，正则表达式也是对象。

正则表达式通常被用来检索、替换那些符合某个模式（规则）的文本，例如表单验证：用户名表单只能输入英文字符、数字或者下划线，昵称输入框中可以输入中文（**匹配**）。此外，正则表达式还常用于过滤页面内容的一些敏感词（**替换**），或从字符串中获取我们想要的特定部分（**提取**）等。

正则表达式的特点：

1. 灵活性、逻辑性和功能性非常强。
2. 可以迅速的用极简单的方式达到字符串的复杂控制。
3. 对于刚接触的人来说，比较晦涩难懂。
4. 实际开发中，一般都是直接复制写好的正则表达式，但是要求会使用正则表达式并且根据实际情况修改正则表达式。

### 创建正则表达式

在JS中可以通过两种方式来创建一个正则表达式。

1. 通过调用RegExp对象的构造函数创建

```js
var 变量名 = new RegExp(/表达式/);
```

2. 利用字面量来创建

```js
var 变量名 = /正则表达式/;
```

### 测试正则表达式 test

test()正则对象方法，用于检测正则表达式是否符合规范，该对象会返回true或flase，其参数是测试字符串。

```js
regexObj.test(str)
```

1. regexObj是写的正则表达式
2. str是我们要测试的文本
3. 就是检测str文本是否符合我们写的正则表达式规范

```js
var reg = /123/;
reg.test(123);  //true
reg.test('abc'); //false
```

### 边界符

`^`：表示以规定字符开头。

`$`：表示以规定字符结尾。

```js
// 边界符 ^ $
var rg = /abc/;
// 只要包含abc这个字符串返回的都是true
rg.test('123adc123');	//true
// ^开头字符，规范了以规定字符开头
var reg = /^abc/;
rg.test('123adc123');	//false
rg.test('adc123');	//true
// $结尾符号，规范了以规定字符结尾
var reg = /^abc$/;
rg.test('123adc123');	//false
rg.test('adc123');	//false
rg.test('adc');	//true
```

### 字符类

`[]`：表示有一系列字符可供选择，只要匹配其中一个就可以了。

`-`：范围符号，表示一个范围选择。如`[a-z]`。

`[^]`：中括号内部的`^`表示取反符号，表示不能包含括号里的东西。

`()`：表示里面的字符要作为整体出现，而不是只出现一个。

```js
var rg = /[abc]/;
// 只要包含a或者b或者c都返回true
console.log(re.test('andy'));	//true
console.log(re.test('baby'));	//true
console.log(re.test('color'));	//true
console.log(re.test('red'));	//false
var rg1 = /^[abc]$/;
console.log(re1.test('aa'));	//false
console.log(re1.test('a'));	//true
var rg2 = /^[a-z]$/;	//表示任意一个小写英文字符
console.log(re2.test('f'));	//true
console.log(re2.test('F'));	//false
// 可以进行字符组合
var rg3 = /^[a-zA-Z]$/;
var rg4 = /^[^a-zA-Z]$/	//不能包含括号内的字符

var rg5 = /^(abc){3}$/;
console.log(re5.test('abc')); // false
console.log(re5.test('abcabcabc')); //true
```

### 量词符

用来规定某个模式出现的次数

`*`：重复0次或者更多次。

`+`：重复1次或者更多次。

`?`：重复0次或1次。

`{n}`：重复n次。

`{n,}`：重复n次或更多次。

`{n,m}`：重复n到m次。

```js
var rg = /^[a]{3,6}$/;
console.log(re.test('aa')); //flase
console.log(re.test('aaa')); //true
console.log(re.test('aaaaaa')); //true
console.log(re.test('aaaaaaa')); //false
```

### 预定义类

| 预定类 | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| \d     | 匹配0-9之间的任一数字，相当于[0-9]                           |
| \D     | 匹配所有0-9之外的字符，相当于`[^0-9]`                        |
| \w     | 匹配任意的字母、数字和下划线，相当于[A-Za-z0-9_]             |
| \W     | 除了所有字母、数字和下划线的字符、相当于`[^A-Za-z0-9_]`      |
| \s     | 匹配空格（包括换行符、制表符、空格符等），相当于[\t\r\n\v\f] |
| \S     | 匹配非空格字符，相当于`[^\t\r\n\v\f]`                        |

### 正则表达式参数

```js
/表达式/[switch]
```

switch（也称为修饰词）规定了正则表达式按照什么模式来进行匹配。有三种模式：

1. g：全局匹配
2. i：忽略大小写
3. gi：全局匹配和忽略大小写

```js
var re = /test/g;
```

### 正则表达式的替换

replace()方法可以实现替换字符串的操作，用来替换的参数可以是一个字符串或是一个正则表达式。

可以使用正则表达式来匹配要替换的字符。

```js
stringObject.replace(regexp/substr,replacement)
```
1. 第一个参数：被替换的字符串或者正则表达式
1. 第二个字符串：替换为字符串
1. 返回值：替换完的新字符串

```js
// 替换replace
var str = 'andy和red';
var newStr = str.replace('andy','baby'); 
console.log(newStr);
var newStr1 = str.replace('/andy/','baby');
console.log(newStr);

```

替换敏感词

```html
<body>
    <textarea name="words" id="" cols="30" rows="10"></textarea>
    <button>替换敏感词</button>
    <script>
        var text = document.querySelector('textarea');
        var btn = document.querySelector('button');
        btn.addEventListener('click', function (){
            console.log(text.value);
            text.value = text.value.replace(/激情/g,'**');
        });
    </script>
</body>
```

### 实例

#### 用户名验证

```js
// 规定用户名为6-16位，由字符、数字、_和-组成
var reg = /^[a-zA-Z0-9_-]{6,16}$/;
console.log(reg.test('brokyz_0223'));  //true
console.log(reg.test('brokyz_0223.!'));  //false
```

html

```html
<body>
    <input id="user_name" type="text"><span id="user_text" style="color:#ccc">请输入用户名</span>
    <script>
        var userName = document.querySelector('#user_name');
        var userText = document.querySelector('#user_text');
        userName.addEventListener('blur', function () {
            if (/^[a-zA-Z0-9_-]{6,16}$/.test(this.value)) {
                userText.innerHTML = '√'
                userText.style.color = 'green';
            } else {
                userText.innerHTML = '×';
                userText.style.color = 'red';
            }
        });

    </script>
</body>
```

# ES6

- ES的全程是EMCAScript，它是由EMCA国际标准化组织，指定的一项**脚本语言标准化规范**

- ES6就是JavaScript的一个发行版本, 于2015.06发行, 全称为ECMAScript. 一般来说JS我们刚开始学习的基础版本为ES5, ES6再ES5的基础上新增了一些新特性.
- ES6在ES5的基础上对其进行了升级, 解决了ES5的一些不足. 比如新增了类的概念.
- ES6在企业中应用很普遍.

## 新增声明方法

### let

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

### const

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

## 解构赋值

ES6中允许从数组中提取值，按照对应位置，对变量赋值。对象也可以实现解构。

### 数组解构

- 数组解构允许我们按照一一对应的关系从数组中提取值，然后赋值给变量
- 如果解构不成功，那么变量的值位undefined

```js
let [a, b, c] = [1, 2, 3];
console.log(a);
console.log(b);
console.log(c);
```

### 对象解构

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

## 箭头函数

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

### 箭头函数的this指向

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

### 面试题

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

## 剩余参数

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

### 剩余参数和解构配合使用

```js
let student = ['wangwu', 'zhangsan', 'lisi'];
let [st1, ...st2] = student;
console.log(st1);   // wangwu
console.log(st2);   // ['zhangsan', 'lisi']
```

## ES6内置的对象扩展

### Array 的扩展方法

#### 扩展运算符（展开语法）

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

#### 构造函数方法：Array.from()

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

#### 实例方法：find()

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

#### 实例方法：findIndex()

- 用于找出第一个符合条件的数组成员的位置，如果没有找到返回-1。

```js
let arr = [1, 5, 10, 15];
let index = arr.findIndex((vlaue, index) => vlaue > 9);
console.log(index);	// 2
```

#### 实例方法：includes()

- 表示某个数组是否包含给定的值，返回布尔值。

```js
[1, 2, 3].includes(2); // true
[1, 2, 3].includes(4); // false
```

### String 的扩展方法

#### 模板字符串

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

#### 实例方法：startsWith() 和 endsWith()

- `startsWith()`表示参数字符串是否在原字符产的头部，返回布尔值。
- `endsWith()`表示参数字符串是否在原字符串的尾部，返回布尔值。

```js
let str = 'Hello world!';
str.startsWith('Hello') // true
str.startsWith('!') // true
```

#### 实例方法：repeat()

- `repeat()`方法表示将原字符串重复n次，返回一个新字符串。

```js
'x'.repeat(3)	// 'xxx'
'hello'.repeat(2)	// 'hellohello'
```

### Set 数据结构

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

#### 数组去重

```js
const set = new Set([1, 2, 3, 4, 4]);
const arr = [...set];
console.log(arr);	// [1, 2, 3, 4]
```

#### 实例方法

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

#### 遍历

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

## 类和对象（ES6新增）

### 创建类

语法：

```js
class Star {
    // class body
}
```

创建实例：

```js
var xx = new Star();
```

### 构造函数

- 当生成实例时将会自动执行构造函数。
- 如果省略不写时，会自动生成构造函数。

语法：

```js
class Star {
    constructor(name){
        this.name = name;
    }
}
```

### 方法

- 共有的方法可以直接写道类中。

语法：

```js
class Star {
    constructor(name){
        this.name = name;
    }
    
    sayHi(){
        console.log("Hi!")
    }
}
```

### 继承

- 子类可以继承父类的方法。

语法：

```js
class Father {
}
class Son extends Father {
}
```

#### super

- super允许子类调用父类的方法和构造函数

```js
class Father {
    constructor(x,y){
        this.x = x;
        this.y = y;
    }
    sum(){
		console.log(this.x + this.y);
    }
}

class Son extends Father {
    constructor(x,y){
        // 使用父类的构造方法
        super(x,y);
    }
}
new Son(1, 2).sum();
```

#### 就近原则重写父类方法

- 当子类重写了父类的方法时，调用时调用的时子类的方法。
- 如果想使用父类方法需要用到super

```js
class Father {
    say(){
        console.log("father");
    }
}
class Son extends Father {
    say(){
        // 调用父类的say()
        console.log(super.say() + "son");
    }
}
let son = new Son();
son.say();	// 调用的是子类的方法
```

> 子类可以对父类的方法进行拓展

- 子类中必须在构造函数中使用super，必须放在this前面，才可以使用父类的方法

```js
class Father {
    constructor(x,y){
        this.x = x;
        this.y = y;
    }
    sum(){
		console.log(this.x + this.y);
    }
}

class Son extends Father {
    constructor(x,y){
        // 使用父类的方法时需要将参数传递给父类，因为父类中的this是父类中的
        super(x,y);	// 必须写道this前面
        this.x = x;
        this.y = y;
    }
    
    subtract(x,y){
        console.log(this.x - this.y);
    }
}

let son = new Son(5, 3);
son.sum();
son.substact();
```

### 类的注意点

- ES6中的类没有变量提升，必须先声明再使用。
- 在类中使用共有属性和方法必须要加this，调用本类中的方法也是需要加this
- 当获取对象时，也是需要用this的
- this的指向问题，谁调用指向谁

```js
let that;
class Star {
    constructor(name){
        that = this;
        this.name = name;
        this.sayHi();
        this.btn = document.querySelector('button');
        this.btn.onclick = this.sayHi; //这里调用的是函数名所以不加括号。
        // 这里btn调用了函数，所以函数中的this为btn，因此this.name无法输出name
        // 可以给一个全局变量赋值，然后调用全局变量来解决
    }
    
    sayHi(){
        console.log("Hi!" + this.name);
        console.log("Hi!" + that.name);
    }
}
```

### 类的本质

- 在ES6之前通过**构造函数+原型对象**实现面向对象编程
- 在ES6通过**类**实现面向对象编程

**类的本质其实还是一个函数，我们也可以简单的认为类就是构造函数的另外一种写法。**

类和构造函数都有以下特点：

1. 构造函数有原型对象prototype
2. 构造函数原型对象prototype里面有constructor指向构造函数（类）本身
3. 构造函数可以通过原型对象添加方法
4. 构造函数创建的实例对象有`__proto__`原型指向构造函数的原型对象

```js
class Star {
    constructor(uname) {
        this.uname = uname;
    }
    eat() {
        console.log('eat');
    }
}
console.log(typeof Star);
// 1. 类有原型对象
console.log(Star.prototype);
// 2. 类的原型对象里面有constructor
console.log(Star.prototype.constructor);
// 3. 类可以通过原型对象添加方法
Star.prototype.sing = function() {
    console.log('sing');
}
var star = new Star('brokyz');
console.log(star);
console.log(star.__proto__ === Star.prototype); // true
```

## Promise异步编程

### 概述

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了`Promise`对象。

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

`Promise`对象有以下两个特点。

（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

注意，为了行文方便，本章后面的`resolved`统一只指`fulfilled`状态，不包含`rejected`状态。

有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口，使得控制异步操作更加容易。

`Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用 [Stream](https://nodejs.org/api/stream.html) 模式是比部署`Promise`更好的选择。

### Promise相关概念

#### Promise状态

Promise的状态是Promise实例对象的一个属性[PromiseState]。

- pending 未决定的，promise的初始状态
- resolved / fullfilled 成功
- rejected 失败

promise的状态只能改变一次。

- 由pending改变为成功，成功结果一般称为value
- 由pending改变为失败，失败结果一般称为reason

#### Promise对象的值

promise对象的值是promise实例对象的一个属性[PromiseResult]。

保存了异步任务成功或失败的结果。

只有 resolve/reject 函数可以对该属性进行修改。

### Promise的工作流程

![](https://pic.imgdb.cn/item/6329178e16f2c2beb155477a.jpg)

### Promise 运行机制

```js
const p = new Promise((res, err) => {
    setTimeout(() => {
        console.log("promise running 1")
    }, 1000)
    res("promise fulfilled")
    console.log("promise running 2")
})

console.log(p)

p.then((res)=>{
    console.log("p.then res")
},(err)=>{
    console.log("p.then err")
})
/*
	> promise running 2
	> Promise {PromiseState: "fulfilled"}
	> p.then res
	> promise running 1
*/
```

- 对于 Promise 中的代码，是在声明 p 时就会立即执行。尽管其中存在异步任务，异步任务本身也是立即执行的，异步任务的回调函数会被放到队列中等待条件满足后执行。
- res() 和 err() 函数不会停止代码运行，只会改变 p 的状态，改变状态之后下面的代码会继续执行，直至 Promise 中的代码运行完毕。

- Promse 出现报错会停止之后代码运行，并立即改变状态 Pending -》rejected。

```js
const p = new Promise((res, err) => {
    setTimeout(() => {
        console.log("promise running 1")
        res("promise fulfilled")
    }, 5000) 
    console.log("promise running 2")
})

console.log(p)

p.then((res)=>{
    console.log("p.then res")
},(err)=>{
    console.log("p.then err")
})

console.log("end")

/*
	> promise running 2
    > Promise {PromiseState: ...}
    > end
    > promise running 1
    > p.then res
*/
```

- 当使用 p.then 时，如果 p 中的 res() 或 err() 函数没有在异步回调函数中延迟改变 Promise 状态，p.then 会立即判断 Promise 状态并执行其后续逻辑。如果 p 中的 res() 或 err() 函数在异步回调函数中延迟改变 Promise 状态，那么 p.then 会等待 Promise 中的异步回调函数改变 Promise 状态后在运行，此时 p.then 也会变成异步任务进入队列等待执行。
- Promise 对象中的 PromiseState **属性类似于数据代理**，其值取决于你什么时候查看。对于以上代码，尽管 Promise 对象在改变状态之前被输出。但是在你第一次查看时，其显示的是你查看时的状态。比如 5s 前查看输出结果时 pending，但是 5s 后点开查看结果就会变成 fulfilled。一旦第一次查看之后，其中的控制台输出值就固定了。

### Promise相关api

#### Promise 构造函数

```js
new Promise(excutor){}
```

- excutor 函数：执行器 `(resolve, reject) => {}`

- resolve 函数：内部定义成功时我们调用的函数 `value => {}`
- reject 函数：内部定义失败时我们调用的函数 `reason => {}`

说明：executor 会在 Promise 内部立即同步调用， 异步操作在执行器中执行。

#### Promise.prototype.then

```js
p.then(onResolved, onRejected)
```

- onResolved 函数：成功的回调函数 `(value) => {}`
- onRejected 函数：失败的回调函数`(reason) => {}`

说明：不管成功还是失败，都会返回一个新的 Promise 对象。

#### Promise.prototype.catch

```js
p.catch(onRejected)
```

- onRejected 函数：失败的回调函数`(reason) => {}`

#### Promise.resolve

接收一个参数，并返回一个成功或失败的Promise对象。快速得到一个Promise对象。

```js
let p = Promise.resolve(521)
// 如果传入的参数为 非Promise类型的对象，则返回结果为成功的Promise对象
console.log(p)
// 如果传入的参数为 Promise对象，则参数的结果决定了resolve的结果
// 也就是说传入的Promise为成功，那么resolve的状态和结果与传入一样
// 传入的Promise为失败，那么resolve的状态和结果也与传入一样
```

#### Promise.reject

返回一个失败的Promise

```js
let p = Promise.reject(521)
let p2 = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))
console.log(p2)
```

#### Promise.all

传入几个Promise对象数组，返回一个新的Promise，只有所有传入的都成功返回的才成功，有一个失败返回的就失败。

```js
let p1 = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))
let p2 = Promise.resolve(521)
let p3 = Promise.reject(123)

const res = Promise.all([p1,p2,p3])
// 如果都成功，那么返回的Promise也成功，三个成功的值以数组的形式存于返回的Promise中
// 如果有一个失败，那么返回的Promise就是失败的，结果值就为失败的结果值
```

#### Promise.race

传入一个n个Promise的数组，其返回结果由第一个改变状态的Promise来决定。

```js
let p1 = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))
let p2 = Promise.resolve(521)
let p3 = Promise.reject(123)

const race = Promise.race([p1,p2,p3])
// 返回结果为第一个改变状态的Promise相同的Promise
```

#### util.promise

这是nodejs中的方法，将错误优先的回调风格的函数（err，value）传入，将会返回一个promise的版本。

```js
const util = require('util')
const fs = require('fs')
const newReadFile = util.promisify(fs.readFile)
newReadFile('./promise封装fs.txt')
  .then(value => console.log(value.toString()))
```

### Promise关键问题

#### Promise状态的改变

1. resolve(value)：将Promise的状态由pending改变为resolved
2. reject(reason)：将Promise的状态由pending改变为rejected
3. 抛出异常：将Promise的状态由pending改变为rejected

```js
const p = new Promise((resolve, reject) => {
    // resolve('ok')
    // reject('error')
    throw '出问题了'
})
```

#### Promise执行多个回调

只要状态发生改变，回调才会执行，多个回调也会执行。

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))

// 指定回调1
p.then(value => {
	alert(value)
})
// 指定回调2
p.then(value => {
	console.log(value)
})
```

#### 改变Promise状态和回调函数的先后问题

1. 当Promise中执行同步任务时，先改变状态，然后执行回调函数。

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))

p.then(value => {
	console.log(value)
})
```

2. 当Promise中执行异步任务时，回调函数先执行，改变状态后执行。

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok')
    })
}), 3000)

p.then(value => {
	console.log(value)
})
```

#### Promise中then的返回结果

1. 返回结果非Promise对象，会将返回的值包装成成功的Promise的结果。

```js
let p = Promise.resolve(321)
let p2 = p.then((value) => {
  return 'noPromise'
})
console.log(p2)
```

2. 如果未返回结果，那么默认会返回成功的状态结果值为undefined的Promise对象。

```js
let p = Promise.resolve(321)
let p2 = p.then((value) => {

})
console.log(p2)
```

3. 如果返回Promise则与返回与Promise保持一致状态和结果的Promise对象。

```js
let p = Promise.resolve(321)
let p2 = p.then((value) => {
  return Promise.resolve('test')
  // return Promise.reject('test')
})
console.log(p2)
```

#### 串联多个任务

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok')
    })
}), 3000)

p.then(value => {
	return new Promise((resolve, reject) => {
        resolve("success")
    })
}).then(value => {
    console.log(value)
})
// 最后输出success
```

#### 异常穿透

只需要在链式的最后捕获异常即可，这就是异常穿透，之中的环节出现异常都会被最后的catch捕获

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok')
    })
}), 3000)

p.then(value => {
    console.log(111)
}).then(value => {
    console.log(222)
}).then(value => {
    console.log(333)
}).catch(reason => {
    console.warn(reason)
})
```

#### 中断 Promise 链

当有一堆链式Promise链时，如果想中途停止，需要在停止的地方返回一个pending状态的Promise才可

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok')
    })
}), 3000)

p.then(value => {
    console.log(111)
    // 中断后续的Promise链
    return new Promise(() => {})
}).then(value => {
    console.log(222)
}).then(value => {
    console.log(333)
}).catch(reason => {
    console.warn(reason)
})
```

### async函数

async修饰的函数的返回值为promise对象，其返回逻辑与then相同。

```js
async function test() {
  return Promise.resolve('test')
}
let res = test()
console.log(res)
```

### await

1. await修饰Promise对象时，会直接返回Promise的成功值。
2. await修饰其他值时，会直接将这个值返回。

注意：await必须写在async函数中，但是async中可以没有await。

```js
async function test() {
  let res = await Promise.resolve('test')
  console.log(res)
}
test()
```

### Promise常见封装

#### Promise封装定时器

```js
// 封装生成1-100随机数的函数
function rand(m, n) {
  return Math.ceil(Math.random() * (n - m + 1)) + m - 1
}
// 获取button对象
const btn = document.querySelector('button')
// 设置点击事件
btn.addEventListener('click', () => {
  // 使用promise封装一个异步操作
  // resolve为成功函数
  // reject为失败函数
  const p = new Promise((resolve, reject) => {
    setTimeout(() => {
      let n = rand(1, 100);
      if (n <= 30) {
        resolve(n)	// 将promise对象的状态设置为 成功
      } else {
        reject(n)	// 将promise对象的状态设置为 失败
      }
    },1000)
  })
  // 调用then方法，其中有两个回调函数，第一个为promise为成功时的回调函数，第二个为promise为失败时的回调函数。
  // 只写第一个时，就是成功时的回调函数 
  p.then((value) => {
    alert('中奖，抽取号码为 ' + value)
  }, (value) => {
    alert('未中奖, 抽取号码为 ' + value)
  })
})
```

#### Promise 封装 fs

```js
const fs = require('fs')

const p = new Promise((resolve, reject) => {
  fs.readFile('./promise封装fs.txt', (err, data) => {
    if (err) reject(err)
    resolve(data)
  })
})

p.then(value => {
  console.log('成功：' + value.toString())
}, res => {
  console.log('失败：' + res)
})
```

```js
function mineReadFile(path) {
  return new Promise((resolve, reject) => {
    require('fs').readFile(path, (err, data) => {
      if (err) reject(err)
      resolve(data)
    })
  })
}

mineReadFile('./promise封装fs.txt')
.then(
  value => console.log(value.toString()),
  res => console.warn(res)
)
```

#### Promise 封装 Ajax

```js
const btn = document.querySelector('button')
btn.addEventListener('click', function () {
  const p = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', 'https://api.ooomn.com/api/xwzc')
    xhr.send()
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status >= 200 && xhr.status < 300) {
          resolve(xhr.response)
        } else {
          reject(xhr.status);
        }
      }
    }
  })

  p.then(value => {
    console.log('成功：' + value);
  }, res => {
    console.log('失败：' + res);
  })
})
```

```js
function sendAjax(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url)
    xhr.send()
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status >= 200 && xhr.status < 300) {
          resolve(xhr.response)
        } else {
          reject(xhr.status);
        }
      }
    }
  })
}

const btn = document.querySelector('button')
btn.addEventListener('click', () => {
  sendAjax('https://api.ooomn.com/api/xwzc')
    .then(value => console.log(value))
})
```

### Promise解决回调地狱

**回调地狱形式**

```js
const fs = require('fs')

fs.readFile('./1.txt', (err, data1) => {
  if(err) throw err
  fs.readFile('./2.txt', (err, data2) => {
    if(err) throw err
    fs.readFile('./3.txt', (err, data3) => {
      if(err) throw err
      let res = data1 + data2 + data3
      console.log(res)
    })
  })
})
// 结果123
```

**使用Promise解决回调地狱1**

```js
const fs = require('fs')
const util = require('util')

const mineReadFile = util.promisify(fs.readFile)

async function main() {
  const data1 = await mineReadFile('./1.txt')
  const data2 = await mineReadFile('./2.txt')
  const data3 = await mineReadFile('./3.txt')
  let res = data1 + data2 + data3
  console.log('结果：' + res)
}
```

**使用Promise解决回调地狱2**

```js
const fs = require('fs')

let p = new Promise((resolve, reject) => {
  fs.readFile('./1.txt',(err, data) => {
    if(err) reject(err)
    resolve(data)
  })
})
p.then(value => {
  return new Promise((resolve, reject) => {
    fs.readFile('./2.txt',(err, data) => {
      if(err) reject(err)
      resolve(value + data)
    })
  })
}).then(value => {
  return new Promise((resolve, reject) => {
    fs.readFile('./3.txt',(err, data) => {
      if(err) reject(err)
      resolve(value + data)
    })
  })
}).then(value => {
  console.log(value)
})
```
