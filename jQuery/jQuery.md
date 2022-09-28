# 概述

jQuery 是一个快速、简洁的 JavaScript 库，其设计宗旨为”write less, do more“，提倡我们使用更少的代码去做更多的事情。

jQuery 对我们常见的 JavaScript 原生代码进行了封装，优化了 DOM 操作、事件处理、动画设计和 Ajax交互。

jQuery 可以帮助前端人员加开开发速度，提高开发效率。

# 顶级对象 $

在 jQuery 中 `$` 就代表开始一段 jQuery 代码，等价于 `jQuery`。

```jsx
// 下面二者等价
$('div').hide();
jQuery('div').hide();
```

# jQuery 和 DOM 对象

- jQuery 和 DOM 都是对象，但是二者不是同一种对象。
- DOM 是通过原生 JS 获取来的 DOM 对象。
- jQuery 是通过 jQuery 方式获取来的 jQuery 对象。其本质是对 DOM 元素进行了包装。
- jQuery 对象只能使用 jQuery 方法，DOM 对象只能使用原生的 JS 属性和方法。

## jQuery 和 DOM 相互转换

原生 JS 比 jQuery 更大，jQuery 并没有完全将原生的属性和方法都进行封装。所以当我们要使用原生的方法时，需要对对象进行转换。

1. DOM 对象转换为 jQuery 对象。

```jsx
// $(DOM对象)
$(document.querySelector('div')).hide();
```

1. jQuery 对象转换为 DOM 对象。

```jsx
// 1.第一种方式：$('div')[index]
$('div')[0].style.display = 'none';

// 2.第二种方式：$('div').get(index)
$('div').get(0).style.display = 'none';
```

- jQuery 将选择出来的其实是 DOM 对象，但是  jQuery 对其进行了封装，所以会以伪数组的形式存储选择出来的元素。
- 所以将伪数组中存储的元素拿出来就是 DOM 对象。

```jsx
console.log($('div'));
/*
S.fn.init(2) [div, div, prevObject: S.fn.init(1)]
	0: div
	1: div
	length: 2
	prevObject: S.fn.init [document]
	[[Prototype]]: Object(0)
*/
```

# 入口函数

jQuery 的入口函数允许我们等待页面加载完毕后再去执行 JS 代码。

```jsx
// 1.第一种
$(document).ready(function() {

})
// 2.第二种
$(function () {
  
})
```

# 链式编程

链式编程是为了节省代码量，看起更优雅，允许我们连写代码。

```jsx
$('div').css('color', 'red').sibling().css('color', '')
```

# jQuery 常用 API

## 基础选择器

原生的 JS 获取元素的方式非常多，很复杂，而且有些方法需要考虑兼容性。因此 jQuery 给我们做了封装，统一了获取元素的标准，我们只需要直接使用 CSS 选择器的方法使用即可。

```jsx
// 我们在里面直接使用 CSS 选择器即可
$("选择器")
```

## 筛选选择器

### 基础筛选

当我们获取一系列元素时，jQuery 允许我们对其进行筛选。筛选规则如下。

| 语法       | 用法          | 描述                                             |
| ---------- | ------------- | ------------------------------------------------ |
| :first     | $(’li:first’) | 获取第一个 li 元素                               |
| :last      | $(’li:last’)  | 获取最后一个 li 元素                             |
| :eq(index) | $(’li:eq(’2’) | 获取到的 li 元素中，选择第3个元素，索引号从0开始 |
| :odd       | $(’li:odd’)   | 获取到的 li 元素中，选择索引为奇数的元素         |
| :even      | $(’li:even’)  | 获取到的 li 元素中，选择索引为偶数的元素         |

```jsx
$('li:eq(0)').hide();
```

- 如果使用 eq 选取元素，还可以这样写：`$('li').eq(0).hide();`

### 高级筛选

| 语法               | 用法                           | 说明                                               |
| ------------------ | ------------------------------ | -------------------------------------------------- |
| parent()           | $(”li”).parent()               | 查找最近一级的父级                                 |
| children(selector) | $(”ul”).children(”li”)         | 查找最近一级的孩子，相当于$(”ul>li”)               |
| find(selector)     | $(”ul”).find(”li”)             | 查找后代，相当于$(”ul li”)                         |
| siblings(selector) | $(”.first”).siblings(”li”)     | 查找兄弟，不包括本身                               |
| nextAll([expr])    | $(”first”).nextAll()           | 查找当前元素之后的所有同辈元素                     |
| prevtAll([expr])   | $(”.last”).prevAll()           | 查找当前元素之前的所有同辈元素                     |
| hasClass(class)    | $(”div”).hasClass(”protected”) | 检查当前的元素是否含有某个特定的类，如果有返回true |
| eq(index)          | $(”li”).eq(2)                  | 相当于$('li:eq(2)')                                |

## 隐式迭代

当我们获取多个元素时，在 JS 原生方法中需要使用 for 循环遍历，来对获取到的元素进行操作。

在 jQuery 中具有隐式迭代的特性，当我们对获取到的一系列元素同时添加操作时，jQuery 会自动遍历内部 DOM 元素，这个过程就叫做隐式迭代。

简单理解就是，jQuery 会将获取到的元素在内部进行循环遍历，执行相应的操作，就不用我们在进行遍历操作。由于我们看不见其内部的遍历，所以称为隐式迭代。

```jsx
// jQuery 会将所有获取到的div元素进行隐藏操作
$('div').hide()

// JS 原生方法
let divs = document.querySeletorAll('div');
for (let i; i < divs.length, i++) {
		divs[i].style.display = 'none';
}
```

## 排他思想

通过`siblings()`来控制除了本身的其它兄弟元素。

```jsx
$("li").click(function () {
  // 1. 当前元素改变颜色
  $(this).css("color", "red");
  // 2. 其余兄弟恢复颜色
  $(this).siblings().css("color", "");
});
```

链式写法：

```jsx
$("li").click(function () {
  $(this).css("color", "red").siblings().css("color", "");
});
```

## 操作 CSS

jQuery 可以使用 `css()` 方法来修改简单的元素样式。也可以操作类，修改多个样式。

1. 参数只写属性名，则返回属性值。

```jsx
console.log($('ul').css('color'));
// rgb(0, 0, 0)
```

1. 参数属性名，属性值是设置一组样式，必须用引号。如果是数字可以不用单位和引号。

```jsx
$("li").css("color", "red");
```

1. 参数可以是对象的形式，方便设置多组样式。属性可以不加引号。

```jsx
$("li").css({
  color: "red",
  "font-size": "20px",
});
```

## 操作类名

jQuery 允许我们操作类名的方式来改变 CSS。

1. 添加类 `addClass()` 在现有类后面**追加**新的类

```jsx
$(this).addClass("center");
```

1. 删除类 `removeClass()` 删除指定类，如果没有指定则删除所有类。

```jsx
$(this).removeClass("pink");
```

1. 切换类 `toggleClass()` 如果存在指定的类名则删除，如果不存在指定类名则添加。

```jsx
$(this).toggleClass("move");
```

注意：

- 原生的 JS 中的 `className` 会覆盖元素原来的类名。
- jQuery 里面的类操作只对指定类进行操作，不影响原来的类名。

## jQuery 效果

### 显示与隐藏效果

```jsx
show([speed, [easing], [fn]]);
hide([speed, [easing], [fn]]);
toggle([speed, [easing], [fn]]);
```

- 参数都可以省略，无动画直接显示。
- speed：三种预定速度之一（slow、normal、fast）或表示动画时长的毫秒数值（如：1000）。
- easing：（Optional）用来指定切换效果，默认是 swing，可用参数 linear。
- fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

### 滑动效果

```jsx
slideUp([speed, [easing], [fn]]);
slideDown([speed, [easing], [fn]]);
slideToggle([speed, [easing], [fn]]);
```

- 参数都可以省略，无动画直接显示。
- speed：三种预定速度之一（slow、normal、fast）或表示动画时长的毫秒数值（如：1000）。
- easing：（Optional）用来指定切换效果，默认是 swing，可用参数 linear。
- fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

### 事件切换

```jsx
hover(over,out);
```

- over：鼠标移动到元素上要触发的**函数**（相当于mouseenter）。
- out：鼠标移出元素所触发的**函数**（相当于mouseleave）。

### 动画队列及停止方法

动画或者效果一旦触发就会执行，如果多次触发，就会造成多个动画或者效果排队执行。

所以我们需要停止动画排队。

```jsx
$('div').children('ul').stop().slideDown(2000);
```

- 用于停止动画或效果。
- `stop()`需要**写到动画或者效果的前面**，相当于停止结束**上一次**的动画。

### 淡入淡出效果

```jsx
fadeTo([[speed], opacity, [easing], [fn]])
```

- opacity：透明度必须填写，取值 0-1 之间。
- speed：三种预定速度之一（slow、normal、fast）或表示动画时长的毫秒数值（如：1000）。必须写。
- easing：（Optional）用来指定切换效果，默认是 swing，可用参数 linear。
- fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

### 自定义动画

```jsx
animate(params, [speed], [easing], [fn])
```

- params：想要改变的样式属性，以对象的形式传递，必须写。属性名可以不用带引号，如果是复合属性则需要采取驼峰命名法borderLeft。其余参数都可以省略。
- speed：三种预定速度之一（slow、normal、fast）或表示动画时长的毫秒数值（如：1000）。必须写。
- easing：（Optional）用来指定切换效果，默认是 swing，可用参数 linear。
- fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

## jQuery 属性操作

### 操作固有属性值 prop()

1. 获取

```jsx
console.log($("div").prop("class"));
```

1. 修改

```jsx
$("div").prop("class", "modefied");
```

### 操作自定义属性值 attr()

如果使用 prop() 获取自定义属性，获取到的是 undefined。应该使用 attr() 获取。

attr() 也可以操作固有属性。

attr() 也可以获取 H5 属性/

1. 获取

```jsx
console.log($("div").attr("index"));
```

1. 修改

```jsx
$("div").attr("index", "2");
```

### 数据缓存 data()

data() 方法可以在指定的元素上存取数据，并不会修改 DOM 元素结构。一旦页面刷新，之前存放的数据都将被移除。

使用 data() 存储的数据在元素的内存中。

```jsx
$('.storage').data("name", "brokyz");
console.log($('.storage').data("name")); // brokyz
```

这种方法还可以获取 H5 的数据。

```jsx
// 假如有个元素 <div class="div1" data-index="1"></div>
console.log($('.div1').data("index")); // 1
```

## jQuery 文本属性值

1. `html()` 获取元素内容，赋值修改

```jsx
console.log($("div").html());
$("div").html("modefied");
```

1. `text()` 获取元素文本内容，赋值修改

```jsx
console.log($("div").text());
$("div").text("modefied");
```

1. `val()` 获取表单内容

```jsx
console.log($("input").val());
$("input").val("modefied");
```

## jQuery 元素操作

### 遍历

jQuery 隐式迭代是对同一类元素做了同样的操作。如果想要给同一类元素做不同操作，就需要用到遍历。

```jsx
$("div").each(function (index, domEle) { xxx; })
```

1. each() 方法遍历匹配的每一个元素。主要用 DOM 处理。each 每一个。
2. 里面的回调函数有两个参数：index 是每个元素的索引号；demEle 是每个 DOM元素对象，不是 jQuery 对象。

**第一种遍历方法**

```jsx
$("div").each(function (index, domEle) {
  console.log(index);
  console.log(domEle);
});
```

**第二种遍历方法**

```jsx
$.each($("div"), function (index, domEle) {
  console.log(index);
  console.log(domEle);
});

arr = ["red", "blue", "pink"];
$.each(arr, function (index, ele) {
  console.log(index);
  console.log(ele);
});
```

- 这种遍历方法可以遍历任何对象。
- 主要用于处理数据。因为可以对任何对象进行遍历。

### 创建

动态创建一个元素。

```jsx
$("<li></li>");
```

### 添加元素

1. 内部添加。添加到元素内部的最后面。

```jsx
$("ul").append($("<li></li>"));
```

1. 内部添加。添加到元素内部的最前面。

```jsx
$("ul").prepend($("<li></li>"));
```

1. 添加到同级目标元素的前面。

```jsx
$("ul").before($("<div></div>"));
```

1. 添加到同级目标元素的后面。

```jsx
$("ul").after($("<div></div>"));
```

### 删除元素

1. 删除匹配的元素。

```jsx
$("ul").remove()
```

1. 删除匹配的元素集合中的所有子节点。

```jsx
$("ul").empty()
```

1. 清空匹配元素的内容。和 empty 等价，清除孩子。

```jsx
$("ul").html("")
```

## jQuery 尺寸、位置操作

## 尺寸

| 语法                                 | 用法                                                  |
| ------------------------------------ | ----------------------------------------------------- |
| width() / height()                   | 取得匹配元素宽度和高度值 只算 width / height          |
| innerWidth() / innerHeight()         | 取得匹配元素宽度和高度值 包含 padding                 |
| outerWidth() / outerHeight()         | 取得匹配元素宽度和高度值 包含 padding、border         |
| outerWidth(true) / outerHeight(true) | 取得匹配元素宽度和高度值 包含 padding、border、margin |

- 如果参数为空，则是获取相应值，返回的是数字型。
- 如果参数为数字，则是修改相应值。
- 参数可以不必写单位。

### 位置

1. offset() 设置或获取元素对于整个页面的坐标。

- offset() 方法设置或返回被选元素相对于文档的偏移坐标，跟父级没有关系，不受父级约束。
- 返回的是一个对象，里面有 left 和 top 属性。

```jsx
console.log($(".son").offset().top);
console.log($(".son").offset().left);
$(".son").offset({
		top: 200,
		left: 200
});
```

1. position() 获取距离带定位父级的位置。

- 只能获取不能设置。
- 如果上级都没有定位，那么将返回对于整个页面的位置。

```jsx
console.log($(".son").position().top);
console.log($(".son").position().left);
```

1. scrollTop() / scrollLeft() 设置或获取元素被卷去的头部和左侧。

```jsx
$(window).click(function () {
  $(this).scrollTop(333);
});
$(window).scroll(function () {
  console.log($(this).scrollTop());
})
```

## jQuery 事件

### 单个事件注册

```jsx
element.事件(function() {})
$("div").click(function() { 事件处理程序 })
```

其它事件与原生保持一致

比如 mouseover、mouseout、blur、focus、change、keydown、keyup、resize、scroll等

### **事件处理 on() 绑定多个事件**

on() 方法在匹配元素上绑定一个或**多个**事件的事件处理函数。

```jsx
element.on(events, [selector], fn)
```

1. events：一个或多个用空格分隔的事件类型，比如 click 或 keydown。
2. selector：元素的子元素选择器。

如果事件处理程序不一样：

```jsx
$("div").on({
  mouseenter: function () {
    $(this).css("background-color", "aliceblue");
  },
  click: function () {
    $(this).css("background-color", "skyblue");
  }
})
```

如果事件处理程序相同：

```jsx
$("div").on("mouseenter mouseleave", function () {
  $(this).toggleClass("color");
})
```

**on()的优势：可以事件委托**

给父元素绑定事件，将触发对象委派给子元素，由于事件冒泡，会冒泡给父亲，这样就触发了事件处理函数。

```jsx
$("ul").on("click", "li", function () {
  alert(11);
})
```

- click 事件是绑定在 ul 身上，但是触发对象是 ul 里面的小 li

**on()的优势：on可以给动态创建的元素绑定事件**

```jsx
$("ul").on("click", "li", function () {
  alert(11);
})
$("ul").append($("<li>后来创建的li</li>"))；
```

### 使用 one() 绑定事件

one() 绑定的事件只允许使用一次，使用一次之后就会失效。

```jsx
$("div").one("click", function () {
  alert(11);
})
```

### off() 解绑事件

解除所有事件：

```jsx
$("div").off()
```

解除普通事件：

```jsx
$("div").off("click")
```

解除事件委托：

```jsx
$("ul").off("click", "li");
```

### 自动触发事件 trigger()

有些事件希望自动触发，比如轮播图自动播放功能跟点击右侧按钮一致。可以利用定时器自动触发右侧按钮点击事件，不必鼠标点击触发。

1. JS 原生方法

```jsx
element.click()
```

1. trigger() 方法，参数为触发类型。

```jsx
$("div").trigger("click");
```

1. triggerHandler() 方法，参数为触发类型。不会触发元素的默认行为。

```jsx
$("div").triggerHandler("click");
```

### jQuery 事件对象

事件被触发，就会有事件对象产生。

```jsx
element.on(events, [selector], function(event) {})
```

阻止默认行为：`event.preventDefault()` 或者 `return false`

阻止冒泡：`event.stopPropagation()`

## jQuery 拷贝对象

如果想把某个对象拷贝（合并）给另一个对象使用，此时可以使用`$.extend()`方法。

语法：

```jsx
$.extend([deep], target, object1, [objectN])
```

1. deep：如果为 true 为深拷贝，默认为 false 浅拷贝。与原生 JS 特性相同。
   1. 浅拷贝只把原来对象中的复杂数据类型的地址拷贝给目标对象，进行更改时，二者都会更改。
2. target：要拷贝的目标对象。
3. object：待拷贝到第一个对象的对象。可以拷贝很多对象，出现相同属性时将会覆盖。

# jQuery 多库共存

jQuery 使用$作为标识符，便随着jQuery的流行，这可能会和其它库中的 $ 冲突。导致无法使用。

解决方法：

1. 把 $ 统一改成 jQuery。
2. 为 jQuery 变量指定新的名称：`$.noConflict()`

```jsx
var myQuery = $.noConflict();
```