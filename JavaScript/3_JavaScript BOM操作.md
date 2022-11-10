# BOM 操作

## 什么是 BOM

BOM （Browser Object Model）即**浏览器对象模型**，它提供了独立于内容而与**浏览器窗口进行交互的对象**，其核心对象是 window。

BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性。

BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA，DOM 的标准化组织是 W3C，BOM 最初是 Netspace 浏览器标准的一部分。

| DOM                        | BOM                         |
| -------------------------- | --------------------------- |
| 文档对象模型                     | 浏览器对象模型                     |
| DOM 就是把 [文档] 当作一个 [对象] 来看待 | 把 [浏览器] 当做一个 [对象] 来看待       |
| DOM 的顶级对象是 document        | BOM 的顶级对象是 window           |
| DOM 主要学习的是操作页面元素           | BOM 学习的是浏览器窗口交互的一些对象        |
| DOM 是 W3C 标准规范             | DOM 是浏览器厂商在各自浏览器上定义的，兼容性比较差 |

## BOM 的构成

**window 对象是浏览器的顶级对象**，它具有双重角色。

1. 它是 JS 访问浏览器的一个接口。
2. 它是一个全局对象。定义在全局作用域中的变量、函数都会变成 window 对象的属性和方法。

在调用的时候可以省略 window，前面学习的对话框都属于 window 对象方法，如 alert()、prompt()等。

**注意：window下的一个特殊属性 window.name**

## 页面加载事件

```javascript
window.onload = function() {}
或者
window.addEventListener("load",function(){})
```

window.onload 是窗口（页面）加载事件，当文档内容完全加载完成会触发该事件（包括图像、脚本文件、CSS文件等）才调用处理函数。

注意：

1. 有了 window.onload 就可以把 JS 代码写到页面元素的上方，因为 onload 是等页面内容全部加载完毕，再去执行处理函数。
2. window.onload 传统注册事件方式只能写一次，如果有多个，会以最后一个 window.onload 为准。
3. 如果使用 addEventListener 则没有限制，写多少个都行。

```javascript
document.addEventListener("DOMcontentLoaded",function(){})
```

DOMContentLoaded 事件触发时，仅当 DOM 加载完成，不包括样式表，图片，flash等等。

ie9以上才支持。

如果页面有很多图图片，从用户访问到 onload 触发可能需要较长的时间，交互效果就不能实现，必然影响用户体验，此时用DOMContentLoaded 事件比较合适。

## 调整窗口大小事件

```javascript
window.onresize = function(){}
window.addEventListener("resize",function(){})
```

window.onresize 是调整窗口大小加载事件，当触发时就调用的处理函数。

注意：

1. 只要窗口大小发生像素变化，就会触发这个事件。
2. 我们经常利用这个事件完成响应式布局。window.innerWidth 当前屏幕的宽度。

```
// 浏览器窗口的当前宽度
window.innerWidth
```

## 定时器

window 对象给我们提供了 2 种非常好的方法 - **定时器**。

### setTimeout()

```javascript
window.setTimeout(调用函数, [延迟的毫秒数])
// 在调用的时候 window 可以省略
setTimeout(function() {
    console.log('三秒过去了');
}, 3000)
// 延迟时间单位是毫秒，如果不写默认为0
function callback(){
    console.log('三秒过去了');
}
setTimeout(callback, 3000);
setTimeout('callback()', 3000); // 不提倡这种写法
// 页面中可能有多个定时器，我们经常给定时器加标识符（名字）
var timer1 = setTimeout(callback, 3000);
var timer2 = setTimeout(callback, 5000);
```

setTimeout() 方法用于设置一个定时器，该定时器在定时器到期后执行调用函数。

注意：

1. window 可以省略。
2. 这个调用函数可以**直接写函数，或者写函数名**或者采取字符串 `’函数名()‘`三种形式。
3. 延迟的毫秒数省略默认是0，如果写，必须是毫秒。
4. 因为定时器可能有很多，所以我们经常给定时器赋值一个标识符。

setTimeout() 这个函数我们也叫做**回调函数 callback**

普通函数是按照代码顺序直接调用。

而这个函数，**需要等待**时间，时间到了才去调用这个函数，因此称为回调函数。

简单理解，回调，就是回头调用。上一件事情干完，再回头去调用这个函数。

之前我们学的 element.onclick = function() {} 或者 element.addEventListener("click",fn) 里面的函数也是回调函数。

**停止 setTimeout() 定时器**

```javascript
window.clearTimeout(timeoutID)
// 可以省略window
clearTimeout()
```

### setInterval() 定时器

```javascript
window.setInterval(回调函数, 间隔毫秒数)
```

setInterval() 方法重复调用一个函数，每隔这个时间，就去调用一次回调函数。

注意：

1. window 可以省略。
2. 这个调用函数可以**直接写函数，或者写函数名**或者采取字符串 `’函数名()‘`三种形式。
3. 延迟的毫秒数省略默认是0，如果写，必须是毫秒。
4. 因为定时器可能有很多，所以我们经常给定时器赋值一个标识符。

**停止 clearInterval() 定时器**

```javascript
window.clearInterval(timeoutID)
// 可以省略window
clearInterval()
```

### this

在我们使用停止定时器时，不能在定时器中使用 this 停止定时器。

因为定时器的时 window 对象的函数，this 指向当前对象，所以指向的是 windows。

所以 this 指向的是调用函数的**对象**。

## JS 执行机制

### JS 老版本是单线程的

JavaScript 语言的一大特点就是**单线程**，也就是说，**同一个时间只能做一件事**。这是因为 JavaScript 这门脚本语言的诞生的使命所致——JavaScript 是为处理页面中用户的交互，以及操作 DOM 而诞生的。比如我们对某个 DOM 元素进行添加和删除操作，不能同时进行。应该先进行添加，之后再删除。

比如以下代码：

```javascript
console.log(1);
setTimeout(function(){
    console.log(3);
},1000);
console.log(2);
```

这段代码需要等定时器执行完，才会向下执行代码，如果定时器设置时间较长，那么就会等待很久。

### 现在新版 Js 支持同步和异步

为了解决这个问题，利用多核 CPU 的计算能力，HTML5 提出 Web Worker 标准，允许JavaScript 脚本创建多个线程。于是，JS中出现了**同步**和**异步**。

#### 同步

前一个任务结束后再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的。比如做饭的同步做法：我们要烧水煮饭，等水开了（10分钟之后），再去切菜，炒菜。

#### 异步

在做一件事情时，因为这件事情需要花费很长时间，在做这件事情的同时，你还可以去处理其他的事情。比如做饭的异步做法，在我们烧水的同时，利用这10分钟，去切菜，炒菜。

**同步和异步的本质区别：这条流水线上各个流程的执行顺序不同**。

### JS 执行机制的本质

有如下代码：

```javascript
console.log(1);
setTimeout(function(){
    console.log(3);
},0);
console.log(2);
```

以上代码的输出结果为 1，2，3。

在 JS 中有两种任务：

1. 同步任务：所有的同步任务都在主线程上执行，形成一个**执行栈**。

2. 异步任务：异步任务是通过**回调函数**来实现的，在执行栈执行同步任务时，其中的回调函数会被放到任务队列（消息队列）里面等待主线程执行结束再执行。
   
   一般来说异步任务有如下三种类型：
   
   - 普通事件，如 click、resize等
   - 资源加载，如 load、error等
   - 定时器，包括 setInterval、setTimeout等

| 主线程执行栈             | 任务队列（消息队列）                    |
| ------------------ | ----------------------------- |
| console.log(1);    | function(){ console.log(3); } |
| setTimeout(fn, 0); |                               |
| console.log(2);    |                               |

当执行栈执行到`setTimeout(fn, 0);`时，定时器本身是**同步任务**，但是定时器里面的**回调函数**是异步任务，被放到任务队列中，等待执行栈执行完毕，再去执行任务队列里面的任务。

在执行栈和任务队列之间，又有一个**异步进程处理**的步骤，这一步骤将决定，回调函数在何时被加入任务队列。

以如下代码举例：

```javascript
console.log(1);
document.onclick = function() {
    console.log('click');
}
console.log(2);
setTimeout(function() {
    console.log(3);
},3000)
```

1. 首先执行栈会按照顺序执行同步任务。
2. 当执行到包含异步任务的同步任务时，这个同步任务就会被提交到异步进程中进行处理。
3. 在异步进程中，其中待处理的任务只有满足了条件时，才会被放到任务队列中。比如点击事件，只有被点击时，其中的异步进程才会被送到任务队列。
4. 当执行栈中的同步任务运行完毕，执行栈会将任务队列中的异步任务按顺序拿到执行栈进行执行。
5. 然后运行完成后，执行栈并没有停止，它还会不断地去获得任务，执行任务，这种机制叫做循环机制。比如这时我们再次点击页面，异步进程会再次将异步任务放到任务队列，由于执行栈会重复获取任务，执行栈又会将任务队列中的任务拿到执行栈进行执行。

重点：由于主线程不断的重复获取任务、执行任务、再获取任务、再执行，所以这种机制被称为**事件循环**

## location 对象

window 对象给我们提供了一个 location 属性，用于**获取设置窗体的 url**，并且可以用于解析 url。因为这个属性返回的是一个对象，所以我们将这个属性也成为 location 对象。

### URL

**统一资源定位符**（Uniform Resource Locator，URL）是互联网上标准资源的地址。互联网上每个文件都有一个唯一的 URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

URL 的一般格式为：

```
protocol://host[:port]/path/[?query]#fragment
http://www.brokyz.tk/index.html?name=andy&age=18#link
```

| 组成       | 说明                                     |
| -------- | -------------------------------------- |
| protocol | 通信协议 常用的 http、ftp、maito等               |
| host     | 主机（域名）                                 |
| port     | 端口号 可选，省略时使用方案默认端口，如http默认为80          |
| path     | 路径 有 零或多个/符号隔开的字符串，一般用来表示主机上的一个目录或文件地址 |
| query    | 参数 以键值对的形式，通过&符号分隔                     |
| fragment | 片段 #后面内容 常见于链接 锚点                      |

### location 对象的属性

| 属性                | 返回值                             |
| ----------------- | ------------------------------- |
| location.href     | 获取或者设置 整个 URL，如果不打印设置url会实现页面跳转 |
| location.host     | 返回主机（域名）                        |
| location.port     | 返回端口号 如果未写则返回空字符串               |
| location.pathname | 返回路径                            |
| location.search   | 返回参数                            |
| location.hash     | 返回片段 #后面内容 常用于链接 锚点             |

```javascript
// 5秒后页面跳转
var t = 5;
setTimeout(function() {
    if (t == 0){
        location.href = 'https://baidu.com';
    }else{
        t--;
    }
})
```

### 获取 URL 参数

```html
<body>
    <form action="获取URL参数.html">
        <input type="text" name="uname">
        <input type="submit" value="提交">
    </form>
    <div></div>
    <script>
        console.log(location.search)
        var params = location.search.substring(1);
        console.log(params);
        var arr = params.split('=');
        console.log(arr);
        var div = document.querySelector('div');
        if(arr[1]){
            div.innerHTML = '这里是' + arr[1];
        }
    </script>
</body>
```

- substring(1)：输入第1位之后的字符
- split('=')：通过=拆分字符，拆分返回拆分后的数组

### location 常用方法

| location方法            | 返回值                                                 |
| --------------------- | --------------------------------------------------- |
| location.assign(url)  | 跟 href 一样，可以跳转页面（也称为重定向页面）                          |
| location.replace(url) | 跳转替换当前页面，因为不记录历史，所以不能后退页面                           |
| location.reload()     | 重新加载页面，相当于刷新按钮或者 f5 如果参数为true 强制刷新 ctrl+f5（无视浏览器缓存） |

## navigator 对象

navgator 包含了有关浏览器的相关信息，他有很多属性，我们最常用的时 userAgent，该属性可以返回由客户机发送服务器的 user-agent 头部的值。

下面前端代码可以判断用户哪个终端打开页面，实现跳转。

```
//
if ((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
      window.location.href = "../H5/index.html"; //手机
}
```

## history 对象

window 对象给我们提供了一个 hostory 对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中）访问过的 URL。

| history对象方法       | 作用                                        |
| ----------------- | ----------------------------------------- |
| history.back()    | 可以后退功能                                    |
| history.forward() | 前进功能                                      |
| history.go(参数)    | 前进后退功能 参数如果是1 前进1 个页面，2是前进2，如果时-1 后退1 个页面 |
