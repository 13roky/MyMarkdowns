# 元素偏移量 offset 系列

offset 翻译过来就是偏移量，我们使用 offset 系列相关属性可以**动态的**得到该原告诉的位置（偏移）、大小等。

- 获得元素距离带有定位父元素的位置
- 获得元素自身的大小（宽度高度）
- 注意：返回的数值都不带单位

offset 系列常用属性：

| offset系列属性           | 作用                               |
| -------------------- | -------------------------------- |
| element.offsetParent | 返回作为该元素带有定位的父级元素 如果父级没有定位则返回body |
| element.offsetTop    | 返回元素相对带有定位父元素上方的偏移               |
| emement.offsetLeft   | 返回元素相对带有定位父元素左边框的偏移              |
| element.offsetWidth  | 返回自身包括padding、边框、内容区的宽度，返回数值不带单位 |
| element.offsetHeight | 返回自身包括padding、边框、内容区的高度、返回数值不带单位 |

## offset 和 style 的区别

| offset                              | style                               |
| ----------------------------------- | ----------------------------------- |
| offset 可以得到任意样式表中的样式值               | style 只能获得行内样式表中的样式值                |
| offset 系列获得的数值是没有单位的                | style.width 获得的是带有单位的字符串            |
| offsetWidth 包含 padding、border、width | style.width 获得不包含 padding、border 的值 |
| offsetWidth 等属性是只读属性，只能获取不能赋值       | style.width 是可读写属性，可以获取也可以赋值        |
| **所以，我们想要获取元素大小位置，用offset更合适**      | **所以，我们想要给元素赋值，用style更合适**          |

## 获取鼠标在盒子内的距离

我们可以通过事件对象的属性`e.pageX`、`e.pageY`来获取我们鼠标在浏览器中的坐标。

然后，我们通过得到的鼠标距离浏览器的距离，减去，盒子距离浏览器的距离，就是我们鼠标在盒子内的距离。

# 元素可视区 client 系列

client 翻译是客户端的意思，我们可以使用 client 系列的相关属性来获取元素可视区的相关信息。通过 client 系列的相关属性可以动态的得到该元素的边框大小、元素大小等。

| client 系列属性          | 作用                                 |
| -------------------- | ---------------------------------- |
| element.clientTop    | 返回元素上边框的大小                         |
| element.clientLeft   | 返回元素左边框的大小                         |
| element.Width        | 返回自身包括padding、内容区的宽度，不含边框，返回数值不带单位 |
| element.clientHeight | 返回自身包括padding、内容区的高度，不含边框，返回数值不带单位 |

# 立即执行函数

立即执行函数不需要调用，立马能够自己执行。

主要作用：创建一个独立的作用域，里面所有的变量都是局部变量，避免了命名冲突的问题。

```javascript
(function() {})()
// 或者
(function(){}())
```

## 案例：淘宝flexible.js

# 元素滚动 scroll 系列

使用 scroll 系列的相关属性可以动态的得到该元素的大小、滚动距离等。

| scroll 属性            | 作用                                 |
| -------------------- | ---------------------------------- |
| element.scrollTop    | 返回被卷上去的上侧距离，返回数值不带单位               |
| element.scrollLeft   | 返回被卷上去的左侧距离，返回数值不带单位               |
| element.scrollWidth  | 返回自身实际宽度，不含边框，但是包含padding，返回数值不带单位 |
| element.scrollHeight | 返回自身实际高度，不含边框，但是包含padding，返回数值不带单位 |
| window.pageYOffset   | 获得页面被卷去的头部距离，不支持ie9以下，修改此值不会影响页面   |
| window.pageXOffset   | 获得页面被卷去的左侧距离，不支持ie9以下，修改此值不会影响页面   |

当盒子内内容超出后，scroll返回的是超出之后的大小，而之前的client返回的则是固定指定的大小。

## 页面被卷去头部兼容性解决方案

被卷去的头部，存在兼容性问题：

1. 声明了 DTD，使用 `document.documentElement.scrollTop`

2. 未声明 DTD，使用`document.body.scrollTop`

3. 新方法 `window.pageYOffset`和`window.pageXOffset`，IE9 才开始支持，修改这个值，并不会影响页面位置。

兼容性函数：

```javascript
function getScroll() {
    return {
        left: window.pageXOffset || document.documentElement.scrollLeft || document.body.scrollLeft || 0,
        top: window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0,
    };
}
getScroll().left;
getScroll().top;
```

# 三大系列总结

| 三大系列对比              | 作用                                  |
| ------------------- | ----------------------------------- |
| element.offsetWidth | 返回自身包括padding、边框、内容区的宽度，返回值不带px     |
| element.clientWidth | 返回自身包括padding、内容区域的宽度，不含边框，返回值不带px  |
| element.scrollWidth | 返回自身的实际宽度（比如字太多被滚动撑开），不含边框，返回数值不带单位 |

主要用法：

1. offset系列经常用于获得元素位置：offsetTop、offsetLeft
2. client系列经常用于获得元素大小：clientWidth、clientHeight
3. scroll系列经常用于获取滚动距离：scrollTop、scrollLeft

# mouseover 和 mouseenter 区别

## mouseenter 鼠标事件

- 当鼠标移动到元素上时就会触发 mouseenter 事件
- 类似 mouseover，他们两者之间的差别是
- mouseover 鼠标经过自身盒子会触发，经过子盒子还会触发。mouseenter 只会经过自身盒子触发
- 出现这个现象的原因是因为，mouseover会冒泡，mouseenter不会冒泡

# 动画函数封装

**核心原理：通过定时器 setInterval() 不断移动盒子的位置**

## 实现步骤

1. 获得盒子当前的位置
2. 让盒子在当前位置加上1个距离
3. 利用定时器不断重复这个操作
4. 添加结束定时器的条件
5. 注意此元素需要添加定位，才能使用`element.style.left`

```javascript
        // 1. 获得盒子当前位置
        var div = document.querySelector('div');
        // 2. 让盒子在当前位置上加上1个距离
        div.style.left = div.offsetLeft + 1 + 'px';
        // 3. 用定时器不断的重复这个操作
        var timer = setInterval(function () {
            if (div.offsetLeft >= 400) {
                // 4. 停止动画的本质是停止定时器
                clearInterval(timer);
            }
            div.style.left = div.offsetLeft + 1 + 'px';
        }, 1)
```

## 动画函数的封装

```javascript
        function animate(obj, target) {
            var timer = setInterval(function () {
                if (obj.offsetLeft >= target) {
                    clearInterval(timer);
                }
                obj.style.left = obj.offsetLeft + 1 + 'px';
            }, 1)
        }

        animate(div,300);
```

## 动画封装函数优化

问题：

1. 每一个对象调用函数都会声明`var timer`开辟一个内存空间，这样调用对象一多影响性能
2. 每一个对象调用函数时，函数里面的定时器都叫`timer`，可能会引起歧义
3. 当同一个对象多次调用定时器时，动画会加快，因为定时器同时运行出现叠加

解决办法：

- 将定时器看成当前对象的属性`obj.timer`，这样就避免声明新对象，也避免了定时器名字相同引起的歧义。
- 在调用函数的时候，首先清除一下定时器，这时候在运行代码，就保证了一直是一个定时器运行。

```javascript
        function animate(obj, target) {
            clearInterval(obj.timer);
            obj.timer = setInterval(function () {
                if (obj.offsetLeft >= target) {
                    clearInterval(obj.timer);
                }
                obj.style.left = obj.offsetLeft + 1 + 'px';
            }, 1)
        }
        animate(div,300);
```

## 缓动动画

 缓动动画就是让元素运动速度有所变化，最常见的就是让速度慢慢停下来。

原理：

1. 让盒子每次移动的距离慢慢变小，速度就会慢慢落下来。
2. 核心算法：`（目标值 - 现在的位置）/ 10 `作为每次的移动距离。
3. 停止的条件是：让当前的盒子位置等于目标的位置就停止计时器。

```javascript
function animateAvg(obj, target, callback) {
            clearInterval(obj.timer);
            obj.timer = setInterval(function () {
                // 把步长值改为整数，来防止小数计算，使得目标到不了指定位置而无法停止
                // 当step是正数向上取整（取大），当是负数向下取整（取小）   
                var step = (target - obj.offsetLeft) / 10;
                step = step > 0 ? Math.ceil(step) : Math.floor(step);
                if (obj.offsetLeft == target) {
                    clearInterval(obj.timer);
                    // 回调函数写到定时器结束的时候调用
                    if (callback) {
                        callback();
                    }
                }
                obj.style.left = obj.offsetLeft + step + 'px';
                // console.log('stop');
            }, 20)
        }
```
