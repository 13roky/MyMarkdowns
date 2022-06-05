# 选择器

[toc]

## 基础选择器

### 标签选择器

标签选择器可以把一类标签全部选择出来，能快速为同类标签统一设置样式，但是无法做出差异化设置。

```css
<style>
    div {
        color: green;
    }
</style>
<body>
    <div>
        标签选择器
    </div>
</body>
```



### 类选择器

可以给需要修改样式的标签指定class值，通过类选择器将其选择出来，进行样式修改

```html
<style>
    .header {
        color: green;
    }
</style>
<body>
    <div class="header">
        类选择器
    </div>
</body>
```

**类选择器可以进行多类名使用，多类名在标签以空格隔开**

```html
<style>
    .header {
        color: green;
    }
    .box {
        width:400;
    }
</style>
<body>
    <div class="header box">
        类选择器
    </div>
</body>
```



### id选择器

通过给标签指定id来，进行选择，与类选择器有相似处，但是**id选择器只能选择一次，不能被选择第二次，是一次性的**。

```html
<style>
    #box {
        width:400;
    }
</style>
<body>
    <div id="box">
        id选择器
    </div>
</body>
```



### 通配符选择器

选取页面中的所有元素标签。

```html
<style>
    * {
        color: red;
    }
</style>
```



## 复合选择器

复合选择器是建立在基础选择器之上的，对基本选择器进行组合形成的。



### 后代选择器

选择外层标签里面的所有内层标签。外层标签写在前面，内层标签写在后面，**以空格隔开**。

```html
<style>
    .header p {
        color: green;
    }
</style>
<body>
    <div class="header">
        <p>
            后代选择器
        </p>
    </div>
</body>
```



### 子选择器

只选择作为某元素下一级的元素，不可跨级选择, **使用`>`连接**。

```html
<style>
    .header > p {
        color: green;
    }
</style>
<body>
    <div class="header">
        <p>
            子选择器
        </p>
    </div>
</body>
```



### 并集选择器

同时选择不同的标签，**使用`,`隔开**。

```html
<style>
    .header,p,h1 {
        color: green;
    }
</style>
<body>
    <div class="header">
        <p>
            并集选择器
        </p>
        <h1>
            并集选择器
        </h1>
    </div>
</body>
```



### 伪类选择器

向某些标签添加特殊效果，比如给链接添加特殊效果。

伪类选择器具有使用顺序否为无效，link>visited>hover>active

```html
<style>
    /* 未点击的链接 */
    a:link {
        color: green;
    }
     /* 访问过的链接 */
    a:visited {
        
    }
     /* 鼠标经过的链接 */
    a:hover {
        
    }
     /* 鼠标正在按下还没有抬起的链接 */
    a:active {
        
    }
     /* 获取到焦点的表单元素 */
    input:focus {
		background-color:yellow;
    }
</style>
<body>
    <a>伪类选择器</a>
    <input type="text">
    <input type="text">
</body>
```

# 
