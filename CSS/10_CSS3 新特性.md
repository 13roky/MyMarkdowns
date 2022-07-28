[toc]

## CSS3 新增选择器

### 属性选择器

属性选择器可以根据元素特定的属性来选择元素。这样就可以不借助于类或者id选择器。

1. 利用属性选择器可以不用借助类和id选择器。

```css
input[value] {
    
}
```

以上选择器只选择具有 value 属性的 input。

2. 属性选择器还可以选择 属性=值 的某些元素

```css
input[type=text] {

}
```

3. 属性选择器可以选择属性值开头的某些元素

```css
div[class^=icon] {

}

<div class="icon1">小图标1</div>
<div class="icon2">小图标2</div>
<div class="icon3">小图标3</div>
<div class="icon4">小图标4</div>
```

4. 属性选择器可以选择属性值结尾的某些元素

```css
div[class$=data]{
    
}

<div class="icon1-data">小图标1</div>
<div class="icon2-data">小图标2</div>
<div class="icon3-ico">小图标3</div>
```

5. 属性选择器可以选择属性值存在某些值的元素

```css
div[class*=con]{
    
}

<div class="icon1-data">小图标1</div>
<div class="icon2-data">小图标2</div>
<div class="icon3-ico">小图标3</div>
```

==注意：类选择器，伪类选择器，属性选择器的权重都是10==



### 结构伪类选择器

结构伪类选择器主要根据==文档结构==来选择元素，常用于根据父级选择里面的子元素。

| 选择符           | 简介                          |
| ---------------- | ----------------------------- |
| E:first-child    | 匹配父元素中的第一个子元素E   |
| E:last-child     | 匹配父元素中的最后一个子元素E |
| E:nth-child(n)   | 匹配父元素中的第 n 个子元素E  |
| E:first-of-type  | 指定类型 E 的第一个           |
| E:last-of-type   | 指定类型 E 的最后一个         |
| E:nth-of-type(n) | 指定类型 E 的第 n 个          |

#### nth-child（n）

选择某个父元素中的一个或者多个特定的子元素。

- ==其中 n 可以是数字，关键字和公式==

- 如果n是数字，那么就是选择第n个子元素，里面的数字从1开始
- n 可以是关键字：even偶数，odd奇数
- n 可以是公式：常见的公式如下（如果n是公式，则从0开始计算，但是第0个元素或者超出元素的个数会被忽略）

| 公式 | 取值                             |
| ---- | -------------------------------- |
| 2n   | 偶数                             |
| 2n+1 | 奇数                             |
| 5n   | 5，10，15…                       |
| n+5  | 从第五个开始（包含第五个）到最后 |
| -n+5 | 前5个（包含第五个）…             |

#### nth-child 和 nth-of-type 的区别

E:nth-child 给所有的孩子标号

E:nth-of-type 仅仅给要选择的孩子E标号

详情看如下

```html
<style>
    /* 由于 nth-child(n) 给所有孩子都标号，所以这里的第一个孩子选择的是p 光头强，但是光头强不是指定的div标签，所以不做更改 */
    section div:nth-child(1) {
		color: pink;
    }
    /* 由于 nth-of-type(n) 仅仅给指定孩子标号，所以这里只给div标号，选取第1个，就是div中标号的第一个，因此将熊大选出 */
    section div:nth-of-type(1) {
        color: pink;
    }
</style>
<body>
    <section>
		<p>光头强</p>
    	<div>熊大</div>
    	<div>熊二</div>    
    </section>
</body>
```



### 伪元素选择器（重点）

 为元素选择器可以利用CSS创建新标签元素，而不需要HTML标签，从而简化HTML结构。

| 选择符   | 简介                     |
| -------- | ------------------------ |
| ::before | 在元素内部的前面插入内容 |
| ::after  | 在元素内部的后面插入内容 |

**注意：**

- before 和 after 创建一个元素，但是属于行==内元素==
- 新创建的这个元素在文档树中是找不到的，所以我们称为==伪元素==
- ==语法：element::before{}==
- before 和 after 必须有 ==content 属性==
- before 在父元素内容的前面创建元素，after 在父元素内容的后面插入元素
- ==伪元素选择器==和==标签选择器==一样，==权重为1==

#### 伪元素清除浮动

```css
.clearfix::after {
            content: "";
            display: block;
            height: 0;
            clear: both;
            visibility: hidden;
        }
```

```css
.clearfix::before,
.clearfix::after {
	content:"";
    /* 转换为块级元素并且在一行内显示 */
    display:table;
}

.clearfix::after {
    clear:both;
}
```



## CSS3 盒子模型

CSS3中可以通过`box-sizing`来指定盒子模型，有2个值：可以指定为 ==content-box、border-box==，这样一来我们计算盒子大小的方法也发生了改变。

可以分为两种情况：

1. box-sizing: content-box; 盒子大小为 width + padding + border （以前默认的）
2. box-sizing: border-box; 盒子大小为 width

==如果盒子模型设置为第二种，那么 padding 和 border 就不会撑大盒子了 （前提是 padding 和 border 不会超过 width 宽度==



## CSS3 图片模糊处理 filter

filter 属性将模糊或颜色偏移等图形效果应用于元素。

```css
filter: 函数（）; 例如：filter:blur(5px); blur模糊处理数值越大越模糊
```



## CSS3 计算盒子宽度 calc 函数

可以使用 calc 函数在声明属性值时执行一些计算。

```
width: calc(100%-80px);
```

括号里可以使用 + - * / 来计算



## CSS3 过渡

过渡（transition）是 CSS3 中具有颠覆性的特征之一， 我们可以在不使用 Flash 动画或者 JavaScript 的情况下，当元素从一种样式变换为另一种样式时为元素添加效果。

过渡动画：是一个状态 渐可以渐的过度到另一个状态

可以让我们页面更加好看，更动感十足，虽然低版本浏览器不支持（ie9以下版本）但是不会影响页面布局。

==我们现在经常和 :hover 一起搭配使用==

```
transition: 要过渡的属性 花费时间 运动曲线 何时开始：
```

1. 属性：想要变化的 css 属性，宽度高度 背景颜色 内外边距都可以。如果想要所有的属性都变化过度，写一个 all 就可以。
2. 花费时间：单位时 秒（必须写单位）比如 0.5s
3. 运动曲线：默认是 ease（可以省略）
4. 何时开始：单位时 秒（必须写单位）可以设置延迟触发时间 默认是 0s（可以省略）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>过渡</title>
    <style>
        .box {
            width: 100px;
            height: 100px;
            margin: auto;
            background-color: pink;
            /* transition: width 1s ease-in-out 1s, height 1s ease-in-out 1s; */
            transition: all 0.5s;
        }

        .box:hover {
            width: 200px;
            height: 200px;
        }
    </style>
</head>
<body>
    <div class="box">

    </div>
</body>
</html>
```





