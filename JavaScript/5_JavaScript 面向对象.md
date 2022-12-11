# 构造函数和原型

## ES5创建对象的方法

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

## new在执行时会做四件事

1. 在内存中创建一个新的空对象。
2. 让`this`指向这个新的对象。
3. 执行构造函数里面的代码，给这个新的对象添加属性和方法。
4. 返回这个新的对象（所以构造函数里面不需要`return`。

## 实例成员和静态成员

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

## 构造函数的资源浪费问题

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

## 构造函数原型对象prototype

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

## constructor构造函数

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

## 原型链

![](https://pic.imgdb.cn/item/62db608bf54cd3f937e13c68.jpg)

## JS成员查找机制

1. 当访问一个对象的属性（包括方法）时，首先查找这个**对象自身**有没有该属性。
2. 如果没有就查找它的原型（也就是`__proto__`指向的`prototype`**原型对象**）。
3. 如果还有没就查找原型对象的原型（Object的原型对象）。
4. 依此类推一直找到Object为止（null）。

## 原型对象中this的指向

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

## 原型对象扩展内置对象

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

# 继承

ES6之前并没有给我们提供extends继承。我们可以通过**构造函数+原型对象**模拟实现继承，被称为**组合继承**。

## call()

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

## 借用父构造函数继承属性

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

## 借用原型对象继承方法

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

## 原理

![](https://pic.imgdb.cn/item/62de59f9f54cd3f9370034f9.jpg)

# 类和对象（ES6新增）

## 创建类

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

## 构造函数

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

## 方法

- 共有的方法可以直接写道类中。类中的方法默认都是写在原型对象上的。

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

## 继承

- 子类可以继承父类的方法。

语法：

```js
class Father {
}
class Son extends Father {
}
```

### super

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

### 就近原则重写父类方法

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

## 类的注意点

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

## 类的本质

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

# ES5中的新增方法

## 数组方法

迭代（遍历）方法：forEach()、map()、filter()、some()、every()

### forEach()

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

### filter()

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

### some()

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

### some和forEach的区别

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

## trim()去除字符串空格

- trim()可以去除两侧空格

```js
var str = '  an  dy  ';
console.log(str);
var str1 = str.trim();
console.log(str1);
```

 ## 对象方法

### Object.keys()

遍历对象中的属性和方法

```js
Object.keys(obj)
```

### Object.defineProperty()

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

# 函数进阶

## 函数的定义与调用

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

## 函数的调用方式

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

## 改变函数内部this指向

JS为我们专门提供了一些函数方法来帮助我们更优雅的处理函数内部this的指向问题，常用的有bind()、call()、apply()三种方法

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

### apply()

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

### bind()

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

### 三者的总结

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

# 严格模式

JS除了提供正常模式外，还提供了**严格模式（strict mode）**。ES5的严格模式是采用具有限制性JS变体的一种方式，即在严格的条件下运行JS代码。

严格模式在IE10以上的版本的浏览器中才会被支持，旧版本浏览器中会被忽略。

严格模式对正常的JavaScript语义做了一些更改：

1. 消除了JS语法的一些不合理、不严谨之处，减少了一些怪异的行为。
2. 消除代码运行的一些不安全之处，保证代码运行的安全。
3. 提高编译器效率，增加运行速度。
4. 禁用了在EMCAScript的未来版本中可能会定义的一些语法，为未来新版本的JS做好铺垫。比如一些保留字如：class,enum,export,extends,super不能作为变量名。

## 开启严格模式

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

## 严格模式的变化

### 变量规范

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

### 严格模式下的this指向

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

### 函数的变化

1. 函数不允许有重名的参数。

```js
function fn(a,a){
    console.log(a+a);
}
fn(1,2); // 输出为4，以为传入完成时a为2，计算的是2+2=4。严格模式不允许这样。
```

2. 函数必须声明在顶层。新版本的JS会引入"块级作用域"（ES6已经引入）。为了与新版本接轨。不允许在非函数的代码块内声明函数。比如不允许在if和for中声明函数。但是依然可以在函数中声明函数来完成函数的嵌套。

# 高阶函数

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

# 闭包

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

# 递归函数

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

## 浅拷贝 Object.assign()

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

## 深拷贝

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

# 正则表达式

**正则表达式（Regular Expression**是用于匹配字符串中字符组合的模式。在JavaScript中，正则表达式也是对象。

正则表达式通常被用来检索、替换那些符合某个模式（规则）的文本，例如表单验证：用户名表单只能输入英文字符、数字或者下划线，昵称输入框中可以输入中文（**匹配**）。此外，正则表达式还常用于过滤页面内容的一些敏感词（**替换**），或从字符串中获取我们想要的特定部分（**提取**）等。

正则表达式的特点：

1. 灵活性、逻辑性和功能性非常强。
2. 可以迅速的用极简单的方式达到字符串的复杂控制。
3. 对于刚接触的人来说，比较晦涩难懂。
4. 实际开发中，一般都是直接复制写好的正则表达式，但是要求会使用正则表达式并且根据实际情况修改正则表达式。

## 创建正则表达式

在JS中可以通过两种方式来创建一个正则表达式。

1. 通过调用RegExp对象的构造函数创建

```js
var 变量名 = new RegExp(/表达式/);
```

2. 利用字面量来创建

```js
var 变量名 = /正则表达式/;
```

## 测试正则表达式 test

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

## 边界符

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

## 字符类

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

## 量词符

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

## 预定义类

| 预定类 | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| \d     | 匹配0-9之间的任一数字，相当于[0-9]                           |
| \D     | 匹配所有0-9之外的字符，相当于`[^0-9]`                        |
| \w     | 匹配任意的字母、数字和下划线，相当于[A-Za-z0-9_]             |
| \W     | 除了所有字母、数字和下划线的字符、相当于`[^A-Za-z0-9_]`      |
| \s     | 匹配空格（包括换行符、制表符、空格符等），相当于[\t\r\n\v\f] |
| \S     | 匹配非空格字符，相当于`[^\t\r\n\v\f]`                        |

## 正则表达式参数

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

## 正则表达式的替换

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

## 实例

### 用户名验证

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
