[toc]

## 什么是DOM？

文档对象模型（Document Object Model），是 W3C 组织推荐的处理可拓展标记语言（HTML或者XML）的编程接口。

W3C 已经定义了一系列的 DOM 接口，通过这些接口可以改变网页的内容、结构和样式。

## DOM 树

![image-20220615154553565](https://s2.loli.net/2022/06/15/pXTyoQPidwj8649.png)

- 文档：一个页面就是一个文档，DOM 中使用 document 表示
- 元素：页面中的所有标签都是元素，DOM中使用 element 表示
- 节点：网页中的所有内容都是节点（标签、属性、文本、注释等），DOM 中使用 node 表示

**DOM 把以上内容都看成是对象**



## 获取元素



| 代码                                  | 归属             | 描述                                                         |
| ------------------------------------- | ---------------- | ------------------------------------------------------------ |
| document.getElementById(‘nav’)        | DOM 老方法不推荐 | 通过元素的 id 属性值获取元素，只返回第一个元素               |
| document.getElementByTagName(‘div’)   | DOM 老方法不推荐 | 通过元素的标签名获取元素，将选择到的所有对象存入伪数组并返回 |
| document.querySelector(‘.nav li’)     | H5 新增方法推荐  | 通过选择器的方法获取元素，只返回第一个对象                   |
| document.querySelectorAll(‘.nav li’)  | H5 新增方法推荐  | 通过选择器的方法获取元素，将选择到的所有对象存入伪数组并返回 |
| document.getElementByClassName(‘nav’) | H5 新增方法      | 通过元素的类名获取元素，将选择到的所有对象存入伪数组并返回，不兼容ie |
| document.body                         |                  | 获取整个文档的body部分                                       |
| document.documentElement              |                  | 获取整个文档，即html部分                                     |



### 根据元素 ID 获取 getElementById()

- 此方法为 DOM 老 API，兼容性好，但是不推荐开发时使用。

- 此方法只返回获取到的第一个元素对象。

- 语法：

```javascript
// 获取 id 为 time 的第一个元素
var timer = document.getElementById('time');
// 返回获取到的整个元素标签
console.log(timer);
// 返回获取到的元素类型
console.log(typeof timer);
// 在控制台中显示指定 JavaScript 对象的属性，并通过类似文件树样式的交互列表显示, 更好的查看属性和方法
console.dir(timer);
```



###  通过标签名获取 getElementByTagName()

- 此方法为 DOM 老 API，兼容性好，但是不推荐开发时使用。

- 此方法将获取到的对象存放于伪数组，以伪数组的形式返回所有获取的元素。
- 如果获取到的元素只有一个，依然返回伪数组；如果没有获取到元素，返回的是一个空的伪数组、
- 语法：

```javascript
// 获取页面中所有 li
var lis = document.getElementsByTagName('li');
// 输出伪数组
console.log(lis);
// 输出伪数组中的第2个对象
console.log(lis[1]);
// 遍历
for (i = 0; i < lis.length; i++){
	console.log(lis[i]);
}
```



### 根据类名来获取 getElementByClassName()

- 此方法为 HTML5 新增的方法，仅仅兼容ie9+，平时不常用。
- 此方法将获取到的对象存放于伪数组，以伪数组的形式返回所有获取的元素。
- 如果获取到的元素只有一个，依然返回伪数组；如果没有获取到元素，返回的是一个空的伪数组。
- 语法：

```javascript
var box = document.getElementsByClassName('box');
console.log(box);
```



### 根据选择器获取 querySelector()

- 此方法为 HTML5 新增的方法，仅仅兼容ie9+，推荐使用。
- 此方法只返回获取到的第一个元素对象。

```javascript
var box = document.querySelector('.box');
console.log(box);
```



### 根据选择器获取 querySelectorAll()

- 此方法为 HTML5 新增的方法，仅仅兼容ie9+，推荐使用。
- 此方法将获取到的对象存放于伪数组，以伪数组的形式返回所有获取的元素。
- 如果获取到的元素只有一个，依然返回伪数组；如果没有获取到元素，返回的是一个空的伪数组

```javascript
var box =  document.querySelectorAll('.box');
console.log(box);
```



### 获取 body 和 html

- 在文档中元素的获取中，有专门获取 body 和 html 的方法
- 语法：

```javascript
// 获取body
var body = document.body;
console.log(body);
console.dir(body);
// 获取整个html
var htmlEle = document.documentElement;
console.log(htmlEle);
```



## 事件基础

网页中的每个元素都可以产生某些可以触发 JS 的事件，例如，我们可以在用户点击某按钮时产生一个事件，然后去执行某些操作

事件有三部分组成：事件源、事件类型、事件处理程序，我们将其称为事件三要素

1. 事件源：事件的触发对象（谁触发了事件？）
2. 事件类型：事件如何被触发 什么事件 比如鼠标点击（onclick）还是鼠标经过 还是键盘按下
3. 事件处理程序：事件被触发要执行的操作

| 事件        | 描述         |
| ----------- | ------------ |
| onclick     | 点击事件     |
| onfocus     | 获得焦点事件 |
| onblur      | 失去焦点事件 |
| onmouseover | 鼠标经过     |
| onmouseout  | 鼠标离开     |
| onmousemove | 鼠标移动     |
| onmouseup   | 鼠标弹起     |
| onmousedown | 鼠标按下     |

语法：

```javascript
// 获取按钮元素
var btn = document.querySelector('button');
// 给按钮元素添加点击事件
btn.onclick = function(){
    // 事件处理程序，但被点击时执行
    console.log('按钮被点击');
}
```



## 操作元素

JavaScript 的 DOM 操作可以改变网页内容、结构和样式，我们可以利用 DOM 操作元素来改变元素里面的内容、属性等。



### 改变元素内容

#### **innerText**

- innerText 只获取元素中的文本内容
- innerText 会删除空格和换行，只保留文本

​	语法：

```javascript
var div = document.querySelector('div');
div.innerText = 'brokyz'
```



#### **innerHTML**

- innerHTML 会保留空格和换行
- innerHTML 修改时支持 HTML 标签，在内容中添加标签将会被识别
- 推荐使用 innerHTML

```javascript
var div = document.querySelector('div');
div.innerHTML = '<strong>brokyz</strong>'
```



### 修改或添加属性内容

- 当获取到元素时，我们对元素中的属性进行添加和更改。
- **本方法对 html 中已经有定义的属性进行操作**。
- 修改属性时会直接覆盖掉原来的属性值。

语法：

```javascript
// 获取a标签
var a = document.querySelector('a');
// 更改或添加a的属性
a.href = 'https://bilibili.com';
a.id = 'bilibili';
// 修改类名属性值
a.className = 'iama';
```

#### 表单元素的属性操作

利用 DOM 可以操作如下表单元素属性：

```javascript
type、value、checked、selected、disabled
```



### 修改或添加CSS样式属性

- 我们可以通过 JS 修改元素的大小、颜色、位置等 CSS 样式。
- JS 里面的样式采用驼峰命名法，比如 fontSize、backgroundColor
- JS 修改 style 样式操作，产生的是行内样式，修改后的 CSS 权重更高

语法：

```javascript
var div = document.querySelector('div');
this.style.backgroundColor = 'antiquewhite';
this.style.width = '400px';
```



#### 使用 className 修改元素类名的方式修改CSS样式属性

- 使用 className 修改样式属性会直接覆盖掉原来的 class 值

```html
<!DOCTYPE html>
<head>
    <style>
        .text {
            width: 400px;
            height: 400px;
            background-color: antiquewhite;
        }

        .change {
            background-color: aliceblue;
        }
    </style>
</head>

<body>
    <div class=”text“>文本</div>
    
    <script>
        var div = document.querySelector('div');
        div.onclick = function(){
            // 此方法会直接覆盖掉原来class属性中的text
            this.className = 'change';
            // 如果不覆盖原来的class属性，只是追加的话可以这样使用
            this.className += ' change';
        }
    </script>
</body>
</html>
```



### 对自定义属性值的操作

- 在标签中，有些属性是内置的，比如 a标签的`href`、`target`等，这些都可以直接获取元素a，然后使用`a.href`对属性进行操作。
- 在标签中，也有些属性是我们自定义的，比如我们自己给a标签定义一个`index = '1'`，这时我们就不能使用`a.index`对自定义属性进行操作。
- 以下我们将讲解对自定义属性的操作方法：

#### 获取自定义属性值 getAttribute()

- `getAttribute()`既可以获取自定义的属性值，也可以获取自带的属性值。

语法：

```javascript
var div = document.querySelector('div');
// 获取内置属性值
console.log(div.getAttribute('id'));
// 获取自定义属性值
console.log(div.getAttribute('index'));
```



#### 添加和修改自定义属性值

- `div.setAttribute('自定义属性','要修改的值');`既可以修改自定义的属性值，也可以修改自带的属性值。
- 但是用`div.setAttribute('自定义属性','要修改的值')`修改元素的类名时，只需要指定`class`即可，不需要自带方法那样使用`className`

语法：

```javascript
var div = document.querySelector('div');
div.setAttribute('index','2');
div.setAttribute('class','box');
```

- 当使用`setAttribute`设置 class 时就是使用 class 而不是使用 className



#### 移除自定义属性值

- `div.removeAttribute()`既可以删除自定义的属性值，也可以删除自带的属性值。

语法：

```javascript
div.removeAttribute('index');
```



### H5自定义属性新规范

由于自定义属性，无法使用自带的方法`div.index`调用，所以html5新增了对自定义属性的调用支持

- h5标准要求在自定义属性前加`data-`,当调用的时候使用`dataset`调用
- 当自定义属性中有多个 - 连接时，调用时需要使用驼峰命名法调用
- 此方法存在兼容性问题，使用 dataset 获取时，仅对ie11+支持

语法：

```html
<body> 
	<div data-listname="123" data-list-name="456"></div>
	<script>
		var div = document.querySelector('div');
		console.log(div.dataset.listname);
		console.log(div.dataset.listName);
	</script>
</body>
```





## 节点操作

节点操作可以**利用层级关系获取元素**，为我们开发提供了一种更加方便的获取元素的方法。



### 节点概述

网页中的所有内容都是节点（标签、属性、文本、注释等），在 DOM 中，节点使用 node 来表示。

HTML DOM树中的所有节点均可以通过 JavaScript 进行访问，所有 HTML 元素（节点）均可被修改，也可以创建或删除。

一般地，节点至少拥有node Type（节点类型）、nodeName（节点名称）和 nodeValue（节点值）这三个基本属性。

- 元素节点 nodeType 为 1
- 属性节点 nodeType 为 2
- 文本节点 nodeType 为 3 （文本节点包含文字、空格、换行等）

**实际开发中，节点操作主要操作的是元素节点**



### 节点的使用

| 代码                      | 使用频率 | 描述                                                     |
| ------------------------- | -------- | -------------------------------------------------------- |
| ul.parentNode             | 常用     | 获取父亲节点，如果找不到返回空                           |
| ul.childNodes             | 不常用   | 得到所有子节点，包含 元素节点 文本节点等等               |
| ul.children               | 常用     | 获取所有的子**元素**节点 实际开发常用                    |
| ul.firstChild             | 不常用   | 获取第一个子节点，不管是文本节点和元素节点都可以被拿到   |
| ul.lastChild              | 不常用   | 获取最后一个子节点，不管是文本节点和元素节点都可以被拿到 |
| ul.firstElementChild      | 常用     | 获取第一个元素节点，仅支持ie9+                           |
| ul.lastElementChild       | 常用     | 获取最后一个元素节点，仅支持ie9+                         |
| li.nextSibling            | 不常用   | 返回下一个兄弟节点 包含 元素节点 文本节点等等            |
| ul.nextElementSibling     | 常用     | 返回下一个元素兄弟节点，仅支持ie9+                       |
| ul.previousSibling        | 不常用   | 返回上一个兄弟节点 包含 元素节点 文本节点等等            |
| ul.previousElementSibling | 常用     | 返回上一个元素兄弟节点，仅支持ie9+                       |



```javascript
        // 1.父节点 parentNode
        var erweima = document.querySelector('.erweima');
        console.log(erweima.parentNode); // 如果找不到父节点就返回为空

        // 2.子节点 childNodes （集合） 得到所有子节点，包含 元素节点 文本节点等等
        var ul = document.querySelector('ul');
        console.log(ul.childNodes);
        // 注意：返回值里面包含了所有的子节点
        // 如果只想获得里面的元素节点，则需要专门处理。所以我们一般不提倡使用childNodes
        for (let i = 0; i < ul.childNodes.length; i++) {
            const element = ul.childNodes[i];
            if (element.nodeType == 1){
                console.log(element);
            }
        }

        // children 获取所有的子元素节点 实际开发常用
        console.log(ul.children);

        // firstchild 获取第一个子节点，不管是文本节点和元素节点都可以被拿到
        console.log(ul.firstChild);
        // lastchild 获取最后一个子节点，不管是文本节点和元素节点都可以被拿到
        console.log(ul.lastChild);
        
        // firstElementChild 获取第一个元素节点
        console.log(ul.firstElementChild);
        // lastElementChild 获取最后一个元素节点
        console.log(ul.lastElementChild);
        // 注意：这几个返回第一个和最后一个存在兼容性问题，不支持ie9
        // 实际开发中，兼顾兼容性
        console.log(ul.children[0]);

        // 3.兄弟节点
        // nextSibling 返回下一个兄弟节点 包含 元素节点 文本节点等等
        console.log(ul.nextSibling);
        // nextElement 返回下一个元素兄弟节点
        console.log(ul.nextElementSibling);
        // prevousSibling 返回上一个兄弟节点 包含 元素节点 文本节点等等
        console.log(ul.previousSibling);
        // prevousElementSibling 返回上一个兄弟节点 包含 元素节点 文本节点等等
        console.log(ul.previousElementSibling);
```



### 创建节点并添加节点

| 代码                                         | 描述                                 |
| -------------------------------------------- | ------------------------------------ |
| ul.appendChild()                             | 在孩子的最后面添加元素               |
| ul.insertBefore(要添加的元素,指定的孩子元素) | 在指定孩子元素的前面添加要添加的元素 |

语法：

```javascript
var li = document.createElement('li');
var ul = document.querySelector('ul');
// 在ul的孩子后面追加元素
ul.appendChild(li);
// 在ul的指定孩子前面插入元素
ul.insertBefore(li,ul.children[0])
```



### 删除节点

```javascript
var ul = document.querySelector('ul');
// 删除父节点里面的孩子
ul.removeChild(ul.children[0]); // 删除ul中的第一个孩子
```



### 复制节点

```javascript
var ul = document.querySelector('ul');
//复制节点
var cloned = ul.children[0].cloneNode(true); //括号为空或者false,是浅拷贝， 则只复制标签不复制内容，如果为true，复制内容，深拷贝
ul.appendChild(cloned);
```



## 三种创建元素方式的区别

- document.write()
  - `document.write('<div>123<div>')`==文档执行完毕，他会导致页面全部重绘==


- innerHTML = ‘ ’
  - `div.innerHTML += '<div>123<div>'`使用拼接的方式创建多个元素时，耗时非常大
  - 当将要创建的元素插入数组中，再通过innerHTML插入时，效率大大提成，为最优


- document.createElement()

  - 使用这种方法创建，并使用节点操作添加到页面中时，方法效率高，但是没有innerHTML结合数组的形式效率高

  **实验：** <a href="https://brokyz.github.io/MyJavaScript/%E6%A1%88%E4%BE%8B/%E4%B8%89%E7%A7%8D%E5%88%9B%E5%BB%BA%E5%85%83%E7%B4%A0%E7%9A%84%E5%8C%BA%E5%88%AB.html" target="_blank">测试地址</a>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>三种创建元素的区别</title>
</head>

<body>
    <button>文档流执行完毕，测试document.write</button>
    <button>测试innerHTML执行效率</button>
    <button>测试innerHTML结合数组执行效率</button>
    <button>测试creatElement执行效率</button>
    <div class="test"></div>
    <script>
        // document.write 当所有文档流执行完毕时，执行这段代码会导致页面重绘
        var btn = document.querySelectorAll('button');
        btn[0].onclick = function () {
            document.write('<div>当所有文档流执行完毕时，执行这段代码会导致页面重绘 document.write</div>');
        }


        // 测试innerHTML执行效率
        var test = document.querySelector('.test');
        btn[1].onclick = function () {
            if (test.children[0]) {
                test.removeChild(test.children[0]);
            }
            var div = document.createElement('div');
            var d1 = +new Date();
            for (var i = 0; i < 2000; i++) {
                div.innerHTML += "<div style='width:100px;border:1px solid blue;'></div>"
            }
            var d2 = +new Date();
            console.log(d2 - d1);
            test.appendChild(div);

        }

        // 测试innerHTML加数组效率
        btn[2].onclick = function () {
            if (test.children[0]) {
                test.removeChild(test.children[0]);
            }

            var div = document.createElement('div');
            var d1 = +new Date();
            var arr = [];
            for (var i = 0; i < 2000; i++) {
                arr.push("<div style='width:100px;border:1px solid blue;'></div>");
            }
            div.innerHTML = arr.join('');
            test.appendChild(div);
            var d2 = +new Date();
            console.log(d2 - d1);


        }

        // creatElement效率测试
        btn[3].onclick = function () {
            if (test.children[0]) {
                test.removeChild(test.children[0]);
            }
            var div = document.createElement('div');
            var d1 = +new Date();
            // 注意创建要添加的元素一定要写在循环内，因为如果写在外面，只创建一次，append添加之后就没有了。
            for (var i = 0; i < 2000; i++) {
                var divs = document.createElement('div');
                divs.style.border = '1px solid blue';
                divs.style.width = '100px';
                div.appendChild(divs);
            }
            var d2 = +new Date();
            test.appendChild(div)
            console.log(d2 - d1);
        }
    </script>
</body>
</html>
```



## DOM 重点核心（复习）

DOM 就是文档对象模型（Document Object Model），是w3c组织推荐的处理可拓展标记语言（HTML或者XML）的标准编程接口。

W3C已经定义了一系列的 DOM 接口，通过这些接口 DOM 接口可以改变网页的内容、结构和样式。

1. 对于 JavaScript，为了能够使 JavaScript 操作 HTML，JavaScript就有了一套自己的 DOM 编程接口。
2. 对于 HTML，DOM 使得 HTML 形成了一棵 DOM 树。其中包含了文档（整个页面）、元素（页面中所有的标签）、节点（页面中所有的内容，文档是节点，元素是节点，属性也是节点）。
3. 我们获取来的 DOM 元素是一个对象（object），所以称为文档对象模型。

**关于 DOM 操作，我们主要针对于元素的操作。主要有创建、增、删、改、查、属性操作、事件操作。**



### 创建

1. document.write
2. innerHTML
3. createElement



### 增

1. appendChild
2. insertBefore



### 删

1. removeChild



### 改

主要是修改 DOM 元素的属性、内容、表单值等

1. 修改元素属性：src、href、title等
2. 修改普通元素内容：innerHTML、innerText
3. 修改表单元素：value、type、disable等
4. 修改元素样式：style、className



### 查

主要获取查询 DOM 元素

1. DOM 提供的 API 方法：getElementById、getElementByTagName 古老用法，不推荐
2. H5 提供的新方法：querySelector、querySelectorAll 推荐
3. 利用节点操作获取元素：父（parentNode）、子（children）、兄（previousElementSibling、nextElementSibling）推荐



### 属性操作

主要针对于自定义属性

1. setAttribute：设置dom的属性值
2. getAttribute：得到dom的属性值
3. removeAttribute：移除属性



### 事件操作

给元素注册事件、采取`事件源.事件类型 = 事件处理程序`

1. onclick：鼠标点击左键触发
2. onmouseover：鼠标经过触发
3. onmouseout：鼠标离开触发
4. onfocus：获得鼠标焦点触发
5. onblur：失去鼠标焦点触发
6. onmousemove：鼠标移动触发
7. onmouseup：鼠标弹起触发
8. onmousedown：鼠标按下触发
