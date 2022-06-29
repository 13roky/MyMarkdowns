[toc]

> 有些时候，网页中会有对话窗弹出，其中就需要引入三角元素，来标识是哪个元素弹出的对话框。

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