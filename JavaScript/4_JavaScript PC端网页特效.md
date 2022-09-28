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

# 轮播图

[demo](https://brokyz.github.io/MyJavaScript/%E6%A1%88%E4%BE%8B/%E8%BD%AE%E6%92%AD%E5%9B%BE/%E8%BD%AE%E6%92%AD%E5%9B%BE.html)

## HTML部分

- `slideshow`: 整个轮播图的框架

- `prev`: 上一页按钮

- `next`: 下一页按钮

- `promo`: 轮播图促销图片

- `circle`: 促销图片对应的按钮

- `promo_now`: 当前促销图片对应的按钮

```html
<body>
    <div class="slideshow">
        <a src="" alt="" class="prev iconfont"><</a>
        <a src="" alt="" class="next iconfont">></a>
        <ul class="promo">
            <li>
                <img src="../../res/taobao-promo.png" alt="" class="promo_img" />
            </li>
            <li>
                <img src="../../res/taobao-promo1.png" alt="" class="promo_img" />
            </li>
            <li>
                <img src="../../res/taobao-promo2.png" alt="" class="promo_img" />
            </li>
            <li>
                <img src="../../res/taobao-promo3.png" alt="" class="promo_img" />
            </li>
        </ul>
        <ol class="circle">
        </ol>
    </div>
</body>
```

## CSS部分

- 注意: 后续需要添加动画的元素, 需要加上绝对定位.

```css
<style>
    * {
        margin: 0;
        padding: 0;
    }

    li {
        list-style: none;
    }

    a {
        text-decoration: none;
    }

    .slideshow {
        position: relative;
        width: 600px;
        height: 300px;
        margin: 200px auto;
        background-color: aliceblue;
        border-radius: 10px;
        overflow: hidden;
    }

    .prev,
    .next {
        display: none;
        position: absolute;
        top: 50%;
        width: 20px;
        height: 30px;
        margin-top: -15px;
        display: inline-block;
        color: white;
        background-color: rgba(0, 0, 0, .3);
        text-align: center;
        line-height: 30px;
        z-index: 2;
    }

    .prev:hover,
    .next:hover {
        cursor: pointer;
    }

    .prev {
        left: 0;
        border-radius: 0 15px 15px 0;
    }

    .next {
        right: 0;
        border-radius: 15px 0 0 15px;
    }

    .promo {
        position: absolute;
    }

    .promo li {
        float: left;
    }

    .promo li .promo_img {
        width: 600px;
        height: 300px;
        cursor: pointer;
    }

    .circle {
        position: absolute;
        right: 5%;
        bottom: 5px;
        height: 15px;
        /* width: 50px; */
        /* background-color: rgba(255, 255, 255, .7); */
        border-radius: 10px;
    }

    .circle li {
        float: left;
        margin-top: 2px;
        margin-left: 5px;
        width: 10px;
        height: 10px;
        background-color: rgba(255, 255, 255, 1);
        border-radius: 10px;
    }

    .circle li:hover {
        background-color: coral;
        cursor: pointer;
    }

    .circle .promo_now {
        background-color: coral;
    }
</style>
```

## JS部分

1. 获取所需要操作的元素.

   - `slideshow`: 轮播图整体框架.

   - `prev`: 轮播图上一页按钮.

   - `next`: 轮播图下一页按钮.

   - `promo`: 轮播图促销海报主体.

   - `circle`: 轮播图促销海报对应的按钮.

   - `imgWidth`: 轮播图一张促销海报的宽度.

2. 鼠标经过轮播图显示上一页和下一页的图标, 鼠标离开轮播图隐藏上一页和下一页的图标.

   - 给轮播图整体框架添加鼠标进入和离开事件.

3. 动态设定轮播图促销海报主体的宽度.

   - 由于后面为了优化第一张和最后一张的过渡动画, 所以我们这里动态调整的时候应该比`length`多一张. 

   - 促销海报的总数 * 促销海报单张的宽度 = 轮播图促销海报主体的宽度

4. 动态生成轮播图对应的显示按钮.

   - 通过循环遍历, 动态生成按钮. 这样按钮数和促销海报数就可一一对应.

   - 创建按钮的时候, 给每个按钮添加上点击事件, 让点击哪个按钮哪个按钮就以类名标记为当前按钮`promo_now`. 在添加前要使用排他思想, 清除所有按钮的`promo_now`标记.

5. 复制在轮播图中复制并尾插入轮播图的第一张图片，为了优化第一页和最后一页之间的切换动画.

   - 克隆促销海报的第一张海报, 将其复制到所有海报的最后.

   - 这样一来, 最后一张切换到第一张时, 我们可以先让其滚动到我们克隆的最后一张图片上. 等动画结束, 我们让轮播图迅速切换到第一张图片位置. 这样人眼无法发觉, 利用了一个小障眼法, 优化了动画效果.

   - 第一张切换到最后一张同理, 先让其无动画迅速切换到最后一个克隆的轮播图位置. 之后再向前翻页, 切换到实际的最后一张海报. 优化动画效果.

6. 实现轮播图下一页的逻辑.

   - 获取当前按钮`promo_now`, 以及其编号`index`, 以此来判断当前位于第几个轮播图.

   - 计算下一张轮播图所需要的移动坐标`target`.

   - 需要获取的信息完毕后, 清除当前轮播图的标记`promo_now`.

   - 使用封装好的动画函数移动轮播图, 当轮播图移动到最后一张时候, 让其施展障眼法瞬间移动到第一张.

   - 给移动到的下一张轮播图添加当前轮播图标记`promo_now`.

7. 实现轮播图上一页的逻辑.

   - 获取当前按钮`promo_now`, 以及其编号`index`, 以此来判断当前位于第几个轮播图.

   - 点击后判断, 当为第一张轮播图时, 施展障眼法瞬间移动到最后一张轮播图.

   - 计算上一张轮播图所需要的移动坐标`target`.

   - 需要获取的信息完毕后, 清除当前轮播图的标记`promo_now`.

   - 使用封装好的动画函数移动轮播图.

   - 给移动到的上一张轮播图添加当前轮播图标记`promo_now`.

8. 播图点击按钮移动到相对应的轮播图。

   - 获取当前点击按钮的`index`.

   - 计算移动到当前点击按钮位置轮播图所需要移动到的位置`target`.

   - 通过动画函数, 移动到计算好的位置.

9. 实现轮播图定时自动切换下一页.

   - 设置定时函数, 指定时间间隔调用以此, 下一页按钮.

   - 当鼠标进入轮播图时, 定时器.

   - 当鼠标离开时重新加载定时器.

```html
<script>
    // 1. 获取所需要操作的元素。
    var slideshow = document.querySelector(".slideshow");
    var prev = document.querySelector(".prev");
    var next = document.querySelector(".next");
    var ul = slideshow.querySelector(".promo");
    var ol = slideshow.querySelector(".circle");
    var imgWidth = slideshow.offsetWidth;
    // 2. 鼠标经过轮播图显示上一页下一页
    slideshow.addEventListener("mouseenter", function () {
        prev.style.display = "block";
        next.style.display = "block";
        // 9. 实现轮播图定时自动切换下一页。
        clearInterval(promo_auto);
    })
    // 2. 鼠标离开轮播图隐藏上一页下一页
    slideshow.addEventListener("mouseleave", function () {
        prev.style.display = "none";
        next.style.display = "none";
        // 9. 实现轮播图定时自动切换下一页。
        promo_auto = setInterval(function () {
            next.click();
        }, 3000);
    })
    // 3. 动态设定轮播图本质主体的宽度。
    ul.style.width = (ul.children.length + 1) * imgWidth + "px";

    // 4. 动态生成轮播图对应的显示按钮。
    for (var i = 0; i < ul.children.length; i++) {
        var li = document.createElement("li");
        li.setAttribute("index", i);
        ol.appendChild(li);
        li.addEventListener("click", function () {
            for (var i = 0; i < ol.children.length; i++) {
                ol.children[i].className = "";
            }
            this.className = "promo_now";
            // 8. 实现轮播图点击按钮移动到相对应的轮播图。
            var promo_index = this.getAttribute("index");
            target = promo_index * imgWidth;
            animate(ul, -target);
        })
    }
    // 设置第一个按钮为当前按钮
    ol.children[0].className = "promo_now";
    // 5. 复制在轮播图中复制并尾插入轮播图的第一张图片，为了优化第一页和最后一页之间的切换动画。
    var ul0 = ul.children[0].cloneNode(true);
    ul.appendChild(ul0);
    // 6. 实现轮播图下一页的逻辑。
    next.addEventListener("click", function () {
        var promo_now = document.querySelector(".promo_now");
        var promo_index = promo_now.getAttribute("index");
        target = (promo_index - 0 + 1) * imgWidth;
        promo_now.className = "";
        animate(ul, -target, function () {
            if ((promo_index - 0 + 1) == ol.children.length) {
                ul.style.left = "0px";
            }
        });
        if ((promo_index - 0 + 1) == ol.children.length) {
            ol.children[0].className = "promo_now";
        } else {
            ol.children[promo_index - 0 + 1].className = "promo_now";
        }
    })
    // 7. 实现轮播图上一页的逻辑。
    prev.addEventListener("click", function () {
        var promo_now = document.querySelector(".promo_now");
        var promo_index = promo_now.getAttribute("index");
        if (promo_index == 0) {
            ul.style.left = -(ul.children.length - 1) * imgWidth + "px";
            promo_index = (ul.children.length - 1);
        }
        target = (promo_index - 1) * imgWidth;
        promo_now.className = "";
        animate(ul, -target);
        console.log("target:" + target);
        console.log("promo_index:" + (promo_index - 1));
        ol.children[promo_index - 1].className = "promo_now";
    })

    // 9. 实现轮播图定时自动切换下一页。
    var promo_auto = setInterval(function () {
        next.click();
    }, 3000);

</script>
```

```js
function animate(obj, target, callback) {
    clearInterval(obj.timer);
    obj.timer = setInterval(function () {
        // 把步长值改为整数，来防止小数计算，使得目标到不了指定位置而无法停止
        // 当step是正数向上取整（取大），当是负数向下取整（取小）   
        var step = (target - obj.offsetLeft) / 10;
        step = step > 0 ? Math.ceil(step) : Math.floor(step);
        if (obj.offsetLeft == target) {
            clearInterval(obj.timer);
            // 回调函数写到定时器结束的时候调用
            // if (callback) {
            //     callback();
            // }
            // 高级写法
            callback && callback();
        }
        obj.style.left = obj.offsetLeft + step + 'px';
        // console.log('stop');
    }, 10)
}
```

