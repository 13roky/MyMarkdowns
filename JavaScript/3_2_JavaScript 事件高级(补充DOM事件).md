[TOC]

## 注册事件

给元素添加事件，称为**注册事件**或者**绑定事件**。

注册事件有两种方式：**传统方式**和**方法监听注册方式**。

### 传统注册方式

- 使用 on 开头的事件 onclick

- `<button onclick="alert('hi')"></button>`

- btn.onclock = function(){}

- 特点：注册事件的**唯一性**

- 同一个元素同一个事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数。

### 方法监听注册方式

- w3c 标准 推荐方式
- addEventListener() 他是一个方法
- IE9之前的IE版本不支持此方法，可以使用attachEvent() 代替
- 特点：同一个元素同一个事件可以注册多个监听器

#### addEventListener 事件监听方式

```javascript
eventTarget.addEventListener(type, listener, useCapture)
```

eventTarget.addEventListener() 方法将指定的监听器注册到 evenTarget (目标对象) 上，当该对象触发指定事件时，就会执行事件处理函数。

该方法有三个参数：

- type：事件类型字符串，比如 click、mouseover，注意这里并没有on
- listener：事件处理函数，事件发生时，会调用该监听函数
- useCapture：事件流，默认为false。flase为冒泡阶段，true为捕获阶段。

代码实例：

```javascript
// 给元素btn添加两个监听鼠标点击事件
btn.addEvenListener('click',function(){
    alert(22);
})
btn.addEvenListener('click',function(){
    alert(33);
})
```

#### attachEvent 事件监听方式

```javascript
eventTarget.attachEvent(type, callback)
```

该方法将指定的监听器注册到指定的对象上，当对象被触发，就会执行指定的回调函数。

**该方法只支持ie9之前的ie浏览器，并不被其他浏览器支持。**

- type：事件类型字符串，比如 onclick、onmouseover，这里要带有 on
- callback：事件处理函数，当目标触发事件时回调函数被调用

代码实例：

```javascript
btn.attachEvent('onclick', function(){
    alert(11)
})
```

#### 对不同浏览器的兼容性处理

这里写一个兼容性函数，来适配不同浏览器的兼容性问题（PS：IE就是个毒瘤）

兼容性要首先照顾大多数浏览器，再处理特殊浏览器

```javascript
function addEventListener(element, eventName, fn){
    // 判断当前浏览器是否支持 addEventListener
    if(element.addEvenListener){
        element.addEventListener(eventName, fn);
    }else if(element.attachEvent){
        element.attachEvent('on' + eventName, fn);
    }else{
        // 相当于 element.onclick = fn
        element['on' + eventName] = fn;
    }
}
```

## 删除事件（解绑事件）

### 传统方式删除事件

直接给指定的对象的相应的事件赋值为null，即可删除该事件。

```javascript
eventTarget.onclick = null;
```

### 监听方式删除事件

#### 监听方式删除事件 removeEventListener

```javascript
eventTarget.removeEventListener(type, listener, useCapture)
```

- type：事件类型字符串，比如 click、mouseover，注意这里并没有on

- listener：需要解绑的事件处理函数

- useCapture：事件流，默认为false。flase为冒泡阶段，true为捕获阶段。

代码实例：

```javascript
btn.addEventListener('click', fn);
function fn() {
    alert(22);
    btn.removeEventListener('click', fn);
}
```

注意：如果要删除监听事件，那么添加监听事件时一定要用具体的函数名，不能使用匿名函数。使用匿名函数时，再删除事件时候，无法指定事件触发函数。

#### 监听方式删除事件 detachEvent

此方法对应 attackEvent

```javascript
eventTarget.detachEvent(type, callback)
```

- type：事件类型字符串，比如 onclick、onmouseover，这里要带有 on
- callback：要删除的事件处理函数

代码实例：

```javascript
btn.attachEvent('onclick', fn);
function fn() {
    alert(33);
    btn.detachEvent('onclick', fn);
}
```

#### 对不同浏览器的兼容性处理

```javascript
function removeEventListener(element, eventName, fn){
    // 判断当前浏览器是否支持 removeEventListener
    if(element.removeEvenListener){
        element.removeEventListener(eventName, fn);
    }else if(element.detachEvent){
        element.detachEvent('on' + eventName, fn);
    }else{
        // 相当于 element.onclick = fn
        element['on' + eventName] = null;
    }
}
```

## DOM 事件流

事件流描述的是**从页面接收事件的顺序**。

**事件发生**时会在元素节点之间按照特定的**顺序传播**，这个传播过程称为 DOM 事件流。

DOM 事件流分为3个阶段：

1. 捕获阶段
2. 当前目标阶段
3. 冒泡阶段

![](https://pic.imgdb.cn/item/62b2834f0947543129ba9989.jpg)

事件冒泡：IE最早提出，事件开始时由最具体的元素接收，然后逐级向上传播到 DOM 最顶层节点的过程。

事件捕获：网景最早提出，由 DOM 最顶层节点开始，然后逐级向下传播到最具体的元素接收的过程。

### 代码验证

1. JS 代码只能执行捕获或者冒泡的其中一个阶段。
2. onclick 和 attachEvent 只能得到冒泡阶段。
3. addEventListener中的第三个参数如果是 true，表示再事件捕获阶段调用事件处理程序；如果是false（不写默认时false）则表示再事件冒泡阶段调用事件处理程序。

[演示地址](https://brokyz.github.io/MyJavaScript/%E6%A1%88%E4%BE%8B/%E4%BA%8B%E4%BB%B6%E6%B5%81.html)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件流</title>
    <style>
        .father {
            width: 300px;
            height: 300px;
            margin: 10px auto;
            background-color: antiquewhite;
            overflow: hidden;
        }

        .son {
            width: 200px;
            height: 200px;
            margin: 50px auto;
            background-color: aliceblue;
        }
    </style>
</head>

<body>
    <div class="father">father
        <div class="son">
            son 冒泡阶段 触发事件处理程序
        </div>
    </div>
    <div class="father">father2
        <div class="son">
            son2 捕获阶段 触发事件处理程序
        </div>
    </div>
    <script>
        var son = document.querySelectorAll('.son');
        // 指定事件在冒泡阶段触发事件处理程序
        son[0].addEventListener('click', function () {
            alert('son');
        })
        son[0].parentNode.addEventListener('click', function () {
            alert('fatehr');
        })

        // 指定事件在捕获阶段触发事件处理程序
        son[1].addEventListener('click', function () {
            alert('son');
        }, true)
        son[1].parentNode.addEventListener('click', function () {
            alert('fatehr');
        }, true)
    </script>
</body>

</html>
```

## 事件对象

事件对象只有有了事件才会存在，他是系统给我们自动创建的，不需要我们传递参数。

事件对象是我们事件的一系列的相关数据的集合，跟事件相关的，比如鼠标点击里面就包含了鼠标的相关信息，如鼠标坐标等。如果是键盘事件，那么里面就包含的键盘事件的信息，比如判断用户按下了哪个键。

事件对象可以由我们自己命名，比如 event、evt、e。

对 ie678 有兼容性问题，ie678 不可自己对事件对象命名，只能使用`window.event`

代码：

```javascript
div.onclick = function(event) {
    console.log(event);
}
```

兼容性处理：

```javascript
div.onclick = function(e) {
    e = e || window.event;
    console.log(e);
}
```

### 常见事件对象的属性和方法

| 属性                  | 描述                                |
| ------------------- | --------------------------------- |
| e.target            | 返回触发事件的对象 不兼容ie678                |
| this                | 返回的是绑定事件的对象                       |
| e.srcElement        | 返回触发事件的对象 兼容ie678                 |
| e.currentTarget     | 和this相似 但是不兼容ie678                |
| e.type              | 返回事件的类型，比如 click、mouseover 不带on   |
| e.cancelBubbel      | 阻止冒泡 ie678不兼容                     |
| e.returnValue       | 阻止默认事件（默认行为）非标准 比如不让链接跳转 ie678不兼容 |
| e.preventDefault()  | 阻止默认事件（默认行为）标准                    |
| e.stopPropagation() | 阻止冒泡 标准                           |

### this 和 target

如果我们有一个 ul>li，给 ul 绑定一个点击事件。

那么当我们点击 li 的时候：

 e.target 返回的是触发事件的对象，也就是 li，e.target 不兼容ie678

this 返回的是绑定事件的对象，也就是 ul

对于这两个方法我们还有两个额外的相似的方法：

e.srcElement 对应 e.target，但是前者兼容了 ie678

e.currentTarget 对应 this，但是前者不兼容ie678

### 阻止默认行为

有些标签默认就带有一些行为，比如 a 链接跳转等，我们可以使用如下方法阻止其默认行为。

阻止行为的方法有两种：

- e.preventDefault()：标准的阻止默认事件方法，推荐使用，但是不兼容ie678
- e.returnValue：这是一个属性，兼容ie678。
- return false：这种方法也可以阻止默认事件，而且没有兼容性问题。需要注意的是，用这种方法后，return之后的代码将不会执行。

```javascript
a.onclick = function(e) {
    e.preventDefault();
    e.returnValue;
    return false;
}
```

### 阻止冒泡（重点）

我们已经知道 dom 事件流包括：捕获，目标，冒泡三个阶段。

事件开始冒泡时，事件将会由具体的目标元素向上逐级传递到dom最顶层节点。

阻止冒泡就是阻止事件向上传播。

阻止冒泡的方法有两种：

- e.stopPropagation()：标准阻止冒泡的方法，推荐使用，但是不兼容ie678
- e.cancelBubbel：这是一个属性，用于阻止冒泡，兼容ie678

```javascript
div.onclick = function(e) {
    e.stopPropagation();
    e.cancelBubbel = True;
}
```

## 事件委托

事件委托也称为事件代理，在jQuery里面也叫事件委派。

其思想就是，当一个父节点中的所有子节点都需要点击事件时，我们不是给每个子节点单独设置事件监听器，而是将事件监听器设置到父亲节点上，然后利用冒泡原理来影响每个子节点，这样点击子节点就可以触发父节点的监听器。

这样我们只需要操作一次DOM，提高了程序的性能。

```html
<body>
    <ul>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
    <script>
        var ul = document.querySelector('ul');
        ul.addEventListener('click', function(e) {
            e.target.style.backgroundColor = 'pink';
        })
    </script>
</body>
```

## 常用的鼠标事件

### 禁止鼠标右键菜单（contextmenu）

contextmenu 主要控制应该何时显示上下文菜单，主要用于程序员取消默认的上下文菜单。

```javascript
document.addEventListener('contextmenu', function(e) {
    e.preventDefault();
})
```

### 禁止鼠标选中（selectstart 开始选中）

```javascript
document.addEventListener('selectstart', function(e) {
    e.preventDefault();
})
```

### 鼠标事件对象

event 对象代表了事件的状态，跟事件相关的一些列信息的集合。其中就包裹键盘事件信息和鼠标事件信息。鼠标事件对象是 MouseEvent；键盘事件对象是 KeyboardEvent。

| 鼠标事件对象    | 描述                       |
| --------- | ------------------------ |
| e.clientX | 返回鼠标相对于浏览器窗口可视区的 X 坐标    |
| e.clientY | 返回鼠标相对于浏览器窗口可视区的 Y 坐标    |
| e.pageX   | 返回鼠标相对于文档页面的 X 坐标 IE9+支持 |
| e.pageY   | 返回鼠标相对于文档页面的 Y 坐标 IE9+支持 |
| e.screenX | 返回鼠标相对于电脑屏幕的 X 坐标        |
| e.screenY | 返回鼠标相对于电脑屏幕·的 Y 坐标       |

## 常用的键盘事件

| 键盘事件       | 描述                                       |
| ---------- | ---------------------------------------- |
| onkeyup    | 某个键盘按键被松开时触发                             |
| onkeydown  | 某个键盘按键被按下时触发                             |
| onkeypress | 某个键盘按键被按下时触发 但是这个方法不识别功能键，比如 ctrl shift等 |

当三个事件同时存在时，执行顺序永远为：keydown --> keypress --> keyup

### 键盘事件对象

| 事件对象    | 描述                              |
| ------- | ------------------------------- |
| key     | 记录用户按下的键盘按键，除了ie678，还有些其他浏览器不兼容 |
| keyCode | 记录用户按下键的 ASCLL 值                |
