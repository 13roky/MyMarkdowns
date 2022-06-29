[toc]

## 精灵图

### 什么是精灵图

在网页中，往往会用到许多图像来修饰网页元素，将这些所需要的元素都放到一张图中，这就是精灵图。





### 为什么需要精灵图

如果网页中所使用的图修饰较多，那么就会导致网页频繁请求服务器，造成服务器压力过大，倒是页面的加载速度变慢。

==为了有效降低服务器接收和发送请求的次数，提高页面加载速度==，就出现了CSS的精灵图技术（也成为CSS Sprites、CSS雪碧）。

<span style="color:red">核心原理：将网页中的一些小背景图像整合到一张大图中，这样服务器只需要一次请求就可以了</span>



### 精灵图的使用

1. 给盒子设定一个大小。
2. 通过 `background-position` 参数一定精灵图的位置，以便于让需要显示的内容在盒子中显示。

```css
.box {
    width: 30px;
    height: 30px;
    background: url(../img/img.png);
    background-position: -155px -105px;
}
```

- 复合写法：`background: url(../img/img.png) -155px -105px;`
- 分开写法：通过``background`指定精灵图，通过`background-position`指定位置
- 指定位置也可以拆分成`background-position-x`和`background-position-y`





## 字体图标的产生

> 在网页布局中有许多的小图标，这些图标都是由字体图标来进行实现的。



在早期这些字体图标也使用精灵图进行实现，但是精灵图也有其自身缺点：

1. 图片文件还是比较大的。
2. 图片本身放大和缩小会失真。
3. 一旦图片制作完毕想要变更非常复杂。

此时，==字体图标==的技术就完美的解决了以上的问题

==字体图标==可以为前端工程师提供同一种方便高效的图标使用方式，==展示的是图标，本质属于字体==。



## 字体图标

### 字体图标的优点

- 轻量级：一个图标字体要比一系列的图像要小 。一旦字体加载了，图标马上就会渲染出来，减少了服务器的请求。
- 灵活性：本质其实是文字，可以很随意的改变颜色、产生阴影、透明效果、旋转等。
- 兼容性：几乎所有浏览器都支持。

注意：字体图标并不能代替精灵技术，只是对工作中图标部分技术的提升和优化。



### 字体图标的使用

#### 字体图标的下载

字体图标可以去相应的图标网站进行下载，这里推荐两个网站：

1. [icomoon](https://icomoon.io)
2. [iconfont](https://www.iconfont.cn/)

这里以icomoon为例子，详情看pink老师的视频。

<a href="https://www.bilibili.com/video/BV14J4114768?p=256" target="_blank" style="display:block;background-color:pink;border-radius:5px;color:white;text-decoration:none;text-align:center;">pink老师：字体图标下载视频</a>

#### 字体图标的使用

根据以上视频下载好字体图标后，会得到一个.zip压缩文件。

将其中的fonts目录放到项目中。

然后将其中的style.css中的字体图标声明，复制到css的开头。

这里仍然以pink老师的视频为例子。

<a href="https://www.bilibili.com/video/BV14J4114768?p=257" target="_blank" style="display:block;background-color:pink;border-radius:5px;color:white;text-decoration:none;text-align:center;">pink老师：字体图标使用</a>



## 三角形的制作

三角形的制作使用 border 来进行制作。

当我们给一个盒子指定了大小为0，四个边框有大小且不同颜色时，这四个边框就会以三角的形式展现。

```css
.box {
            width: 0;
            height: 0;
            border-top: 10px solid pink;
            border-right: 10px solid red;
            border-bottom: 10px solid blue;
            border-left: 10px solid green;
            margin: auto;
        }
```

<div style="width: 0;
            height: 0;
            border-top: 50px solid pink;
            border-right: 50px solid red;
            border-bottom: 50px solid blue;
            border-left: 50px solid green;
            margin: auto;"></div>


这时我们要使用三角时，只需要将其他四个边框的颜色变为透明（transparent）即可。

根据此知识我们可以写出类似对话框效果。

```html
<head>
    <style>
        .box {
            position: relative;
            width: 150px;
            height: 200px;
            background-color: antiquewhite;
            margin: 100px auto;
        }

        .box .angle {
            position: absolute;
            top: -20px;
            right: 10px;
            width: 0;
            height: 0;
            border-top: 10px solid transparent;
            border-right: 10px solid transparent;
            border-bottom: 10px solid antiquewhite;
            border-left: 10px solid transparent;
            margin: auto;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="box2"></div>
    </div>
</body>
```

<div style="position: relative;width: 150px;height: 200px;background-color: antiquewhite;margin: auto;">
    <div style="position: absolute;
            top: -20px;
            right: 10px;
            width: 0;
            height: 0;
            border-top: 10px solid transparent;
            border-right: 10px solid transparent;
            border-bottom: 10px solid antiquewhite;
            border-left: 10px solid transparent;
            margin: auto;">
    </div></div>





## 鼠标样式 cursor

可以给元素添加`cursor`属性来控制经过其的鼠标样式。

```
{ cursor: pointer; }
```

| 属性        | 描述       |
| ----------- | ---------- |
| default     | 小白 默认  |
| pointer     | 小手       |
| move        | 移动十字架 |
| text        | 文本       |
| not-allowed | 禁止       |

```
<ul>
        <li style="cursor: default;">我是默认的小白鼠标样式</li>
        <li style="cursor: pointer;">我是小手鼠标样式</li>
        <li style="cursor: move;">我是鼠标移动鼠标样式</li>
        <li style="cursor: text;">我是鼠标文本样式</li>
        <li style="cursor: not-allowed;">我是鼠标禁止样式</li>
</ul>
```

<div><ul>
        <li style="cursor: default;">我是默认的小白鼠标样式</li>
        <li style="cursor: pointer;">我是小手鼠标样式</li>
        <li style="cursor: move;">我是鼠标移动鼠标样式</li>
        <li style="cursor: text;">我是鼠标文本样式</li>
        <li style="cursor: not-allowed;">我是鼠标禁止样式</li>
</ul></div>



## 表单的轮廓线 outline

给表单添加 `outline: 0;`或者`outline: none;`样式之后就可以去掉默认的蓝色边框。

```css
input {
            outline: none;
        }
```

未去除轮廓

<div><input type="text"></div>

去除轮廓

<div><input type="text" style="outline:none;"></div>



## 文本域拖动缩放 resize

```
textarea {
            resize: none;
            outline: none;
        }
```

未去除

<textarea name="" id="" cols="30" rows="10"></textarea>

去除后

<textarea name="" id="" cols="30" rows="10" style="outline:none;resize:none;"></textarea>

注意：文本域如果`<textarea></textarea>`不再同一行，那么显示出的文本框中会有自动的类似padding效果挤开里面的文字

<div>
    <textarea name="" id="" cols="30" rows="10" style="outline:none;resize:none;">
    </textarea>
</div>



## 图片文字居中对齐

### vertical-align

CSS的`vertical-align`属性的使用场景：经常设置图片或者表单（行内块元素）和文字垂直对齐。

只针对==行内元素或行内块元素起效==

语法：

```css
vertical-align: baseline | top | middle | bottom
```

| 值       | 描述                                 |
| -------- | ------------------------------------ |
| baseline | 默认。元素放置在父元素的基线上。     |
| top      | 把元素顶端与行中最高元素的顶端对齐   |
| middle   | 把元素放置在父元素的中部             |
| bottom   | 把元素的底端与行中最低元素的底端对齐 |

<div>
    <textarea cols="30" rows="5"></textarea>默认基线对齐
</div>
<div>
    <textarea cols="30" rows="5" style="vertical-align:top;"></textarea>顶端对齐
</div>
<div>
    <textarea cols="30" rows="5" style="vertical-align:bottom;"></textarea>底端
</div>
<div>
    <textarea cols="30" rows="5" style="vertical-align:middle;"></textarea>垂直对齐
</div>



### 图片底测空白缝隙

看上面第一个文本域与第二个文本域存在空白缝隙，这是因为行内块元素和文字的基线对齐

解决：

1. 给行内块元素加上 <span style="color:red">vertical-align</span>即可
2. 将行内块（图片）转换为块级元素<span style="color:red">display: block;</span>                                                                                                                                        hai 



## 溢出文字省略号显示

### 单行文本

我们文字的显示有时候会由于盒子不够宽而显示不开，出现换行的情况。

这时我们可以使用 `white-space: nowrap;` 来强制一行显示。

然后使用 `overflow: hidden;` 将溢出的部分隐藏起来。

最后使用 `text-overflow: ellipsis;` 对溢出部分的文字进行省略。

```
{
/* 1. 强制文本在一行内显示 */
/* 默认未normal自动换行 */
white-space: nowrap; 
/* 2. 超出的部分隐藏 */
overflow: hidden;
/* 3. 文字用省略号替代超出的部分 */
text-overflow: ellipsis;
}
```

<div style="width:100px;height:40px;background-color:pink;white-space:nowrap;overflow:hidden;text-overflow:ellipsis">
    我们文字的显示有时候会由于盒子不够宽而显示不开，出现换行的情况。
</div>

### 多行文本

多行文本溢出显示省略号，有非常大的兼容性问题，适合webKit浏览器或移动端（移动端大多是webKit内核）

```css
{
    overflow: hidden;
    text-overflow: ellipsis;
    /* 弹性伸缩盒子模型显示 */
    display: -webkit-box;
    /* 限制一个块蒜素显示的文本的行数 */
    -webkit-line-clamp: 2;
    /* 设置检索伸缩盒子对象的子元素排列方式 */
    -webkit-box-orient: vertical;
}
```

<div style="display: -webkit-box;width:100px;height:50px;background-color:pink;overflow:hidden;text-overflow:ellipsis;-webkit-line-clamp: 2;-webkit-box-orient: vertical;">
    我们文字的显示有时候会由于盒子不够宽而显示不开，出现换行的情况。
</div>
==这种方法只会省略指定的行，所以指定行下面的照常显示，因此需要配合盒子高度来进行设置。==      



## margin 负值技巧

当我们使用浮动展示商品的时候，如果商品需要加边框，那么前面一个盒子的右边框和后面一个盒子的左边框就重合了，宽度为2倍，这时就需要给一个负值margin，来让一个盒子的边框压住一个盒子的边框，这样视觉效果就是一个没有重合的边框了。

```
<style>
        * {
            padding: 0;
            margin: 0;
        }
        li {
            list-style: none;
        }
        .box {
            margin: auto;
            width: 1000px;
            height: 100px;
            background-color: aquamarine;
        }
        .box ul li {
            float: left;
            width: 199px;
            height: 100px;
            background-color: beige;
            border: 1px solid #000;
            margin-left: -1px;
        }
    </style>
<body>
    <div class="box">
        <ul>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </ul>
    </div>
</body>
```

解读：这时候有些人就会有问题，为什么给`li`一个`margin-left: -1px`之后盒子没有整体向左移动溢出父盒子。

因为给了浮动，`li`首先会向左对齐，对齐后才会向左移动-1px。

<div style="height: 100px;">
    <ul>
            <li style="float: left;width: 199px;height: 100px;border: 1px solid #000;margin-left: -1px;list-style: none;"></li>
            <li style="float: left;width: 199px;height: 100px;border: 1px solid #000;margin-left: -1px;list-style: none;"></li>
        </ul>
</div>

### 边框选中高亮

由于我们的这个盒子是左右边框相互覆盖，来解决重合的问题。

所以当我们要边框高亮时，就会出现有一边被覆盖无法高亮的情况。

这时我们给高亮的盒子增加一个==相对定位==，并根据境况给一个==z-index==值便可以使得选中的盒子覆盖在最上面。

<span style="color:red">注意：相对定位显示在浮动之上</span>



## 文字围绕浮动元素

当我们给图片添加浮动时，这时候文字就会围绕在浮动边上，并不会被浮动压住。

因为浮动的出现的初衷就是为了做文字环绕的。



## 行内块元素的妙用

行内元素和行内块元素可以通过给父类添加`text-align`来实现水平居中对齐。

而且行内块元素何以设置宽度和高度。



## 三角的巧妙运用

三角的形状可以通过控制不同`border`边框的大小来进行调整。

三角可以用`i`标签制作，专门用来做三角。



## CSS初始化

不同浏览器对有些标签的默认值是不同的，为了消除不同浏览器对HTML文本呈现的差异，照顾浏览器兼容，我们需要对CSS进行初始化。

这里我们以京东代码为例

```css
/* 把所有标签的内外边距清零 */
* {
    margin: 0;
    padding: 0
}

/* em 和 i 斜体文字不倾斜 */
em,
i {
    font-style: normal
}

/* 去掉li的小圆点 */
li {
    list-style: none
}

img {
    /* 照顾低版本浏览器，如果图片外面包含了链接会有边框的问题 */
    border: 0;
    /* 取消图片有空白缝隙的问题 */
    vertical-align: middle
}
/* 鼠标经过button时变成小手 */
button {
    cursor: pointer
}
/* 改变a颜色并取消下划线 */
a {
    color: #666;
    text-decoration: none
}

/* 京东特色，鼠标经过a变红色 */
a:hover {
    color: #c81623
}

button,
input {
    font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif
}

body {
    /* css3抗锯齿性，让文字更加清晰 */
    -webkit-font-smoothing: antialiased;
    background-color: #fff;
    font: 12px/1.5 Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;
    color: #666
}

/* 专门定义了隐藏样式，方便调用 */
.hide,
.none {
    display: none
}

/* 清除浮动，利用伪元素清除 */
.clearfix:after {
    visibility: hidden;
    clear: both;
    display: block;
    content: ".";
    height: 0
}

.clearfix {
    *zoom: 1
}
```

### Unicode编码字体：

把中文字体名称用相应的Unicode编码来代替，这样就可以有效的避免浏览器解析CSS代码时候出现乱码的问题。

比如：

黑体  \9ED1\4F53

宋体 \5B8B\4F53

微软雅黑 \5FAE\8F6F\96C5\9ED1