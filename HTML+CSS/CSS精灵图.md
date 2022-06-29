[toc]

## 什么是精灵图

在网页中，往往会用到许多图像来修饰网页元素，将这些所需要的元素都放到一张图中，这就是精灵图。



## 为什么需要精灵图

如果网页中所使用的图修饰较多，那么就会导致网页频繁请求服务器，造成服务器压力过大，倒是页面的加载速度变慢。

==为了有效降低服务器接收和发送请求的次数，提高页面加载速度==，就出现了CSS的精灵图技术（也成为CSS Sprites、CSS雪碧）。

<span style="color:red">核心原理：将网页中的一些小背景图像整合到一张大图中，这样服务器只需要一次请求就可以了</span>



## 精灵图的使用

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