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

## some()

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

