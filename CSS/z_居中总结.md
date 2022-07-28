

# 行内元素水平居中

## text-align: center;

```html
<style>
    .parent {
        background-color: aliceblue;
		text-align: center;
    }
    .child {
		background-color: orange;
    }
</style>
<body>
    <div class="parent">
        <span class="child">content</span>
    </div>
</body>
```

<div class="parent" style="background-color: aliceblue;text-align: center;">
        <span class="child" style="background-color: orange;">content</span>
</div>



## fit-content

```html
<style>
    .parent {
        background-color: aliceblue;
        width: fit-content;
        margin: auto;
    }
    .child {
		background-color: orange;       
    }
</style>
<body>
    <div class="parent">
        <span class="child">content</span>
    </div>
</body>
```

<div class="parent" style="background-color: aliceblue;width: fit-content;margin: auto;">
        <span class="child" style="background-color: orange;">content</span>
</div>

# 行内元素和行内块元素垂直居中

```html
<style>
    .parent {
        height: 200px;
        line-height: 200px;
        background-color: aliceblue;
    }
    .child {
		background-color: orange;
    }
</style>
<body>
    <div class="parent">
        <span class="child">content</span>
    </div>
</body>
```

<div class="parent" style="background-color: aliceblue;height: 100px;line-height: 100px;">
        <span class="child" style="background-color: orange;">content</span>
</div>



# 块级元素水平居中

```html
<style>
    .parent {
        height: 200px;
        width: 200px;
        background-color: aliceblue;
    }
    .child {
        height: 100px;
        width: 200px;
        
		background-color: orange;
    }
</style>
<body>
    <div class="parent">
        <div class="child">content</div>
    </div>
</body>
```

<div class="parent" style="background-color:aliceblue;">
        <div class="child" style="height: 50px;width: 100px;background-color: orange;margin: auto;">content</div>
</div>



# 块级元素水平垂直居中

## 定位方法

### 方法一：必须知道宽高

```html
<style>
    .parent {
        position: relative;
        height: 100px;
        background-color: aliceblue;
    }
    .child {
        width: 100px;
        height: 50px;
        position: absolute;
		background-color: orange;
        left: 50%;
        top: 50%;
        margin-top: -25px;
        margin-left: -50px;
    }
</style>
<body>
    <div class="parent">
        <div class="child">content</div>
    </div>
</body>
```

<div class="parent" style="position: relative;height: 100px;background-color: aliceblue;">
        <div class="child" style="width: 100px;height: 50px;position: absolute;background-color: orange;top: 50%;left: 50%;margin-top: -25px;margin-left: -50px;">content</div>
    </div>

### 方法二：不必知道宽高

```html
<style>
    .parent {
        position: relative;
        height: 100px;
        background-color: aliceblue;
    }
    .child {
        position: absolute;
		background-color: orange;
        left: 50%;
        top: 50%;
		transform: translate(-50%, -50%);
    }
</style>
<body>
    <div class="parent">
        <div class="child">content</div>
    </div>
</body>
```

<div class="parent" style="position: relative;height: 100px;background-color: aliceblue;">
        <div class="child" style="position: absolute;background-color: orange;top: 50%;left: 50%;transform: translate(-50%, -50%);">content</div>
    </div>

### 方法三：margin配合定位

当我们给子元素绝对定位后，当我们给各边界距离都为0后，子元素就会填充父元素的所有可用空间，这样一来，在水平和垂直方向上就有了可以分配的空间，这时设置`margin: auto;`就可以自动居中。

```html
<style>
    .parent {
        position: relative;
        height: 100px;
        background-color: aliceblue;
    }
    .child {
        position: absolute;
		background-color: orange;
        left: 0;
        top: 0;
        right: 0;
        bottom: 0;
		margin: auto;
    }
</style>
<body>
    <div class="parent">
        <div class="child">content</div>
    </div>
</body>
```

<div class="parent" style="position: relative;height: 100px;background-color: aliceblue;">
        <div class="child" style="width:100px;height:50px;position: absolute;background-color: orange;top: 0;left: 0;right: 0;bottom: 0;margin: auto;overflow: hidden;">content</div>
    </div>

### padding 方法

#### 方法一：给父元素加 padding

```html
<style>
    .parent {
        background-color: aliceblue;
        padding: 20px;
    }
    .child {
        height: 100px;
		background-color: orange;
    }
</style>
<body>
    <div class="parent">
        <div class="child">content</div>
    </div>
</body>
```

<div class="parent" style="background-color: aliceblue;padding: 40px;">
        <div class="child" style="height: 100px;background-color: orange;">content</div>
    </div>

#### 方法二：给子元素加 padding

```html
<style>
    .parent {
        background-color: aliceblue;
        width: 200px;
        height: 100px;
    }
    .child {
        width: 100px;
        height: 50px;
		background-color: orange;
        padding: 25px 50px;
        background-clip: content-box;
    }
</style>
<body>
    <div class="parent">
        <div class="child">content</div>
    </div>
</body>
```

<div class="parent" style="background-color: aliceblue;height: 100px;width: 200px;">
        <div class="child" style="background-color: orange;width: 100px;height: 50px;padding: 25px 50px;background-clip: content-box;">content</div>
    </div>

### flex 布局方法

`justify-content: center;`控制水平居中

`align-items: center;`控制垂直居中

```html
<style>
    .parent {
        height: 200px;
        background-color: aliceblue;
		display: flex;
        align-items: center;
        justify-content: center;
    }
    .child {
        height: 50px;
        width: 100px;
		background-color: orange;
    }
</style>
<body>
    <div class="parent">
        <div class="child">content</div>
    </div>
</body>
```

<div class="parent" style="background-color: aliceblue;height: 100px;display: flex;align-items: center;justify-content: center;">
        <div class="child" style="background-color: orange;height: 50px;width: 100px">content</div>
    </div>

### 伪元素方法

```html
<style>
    .parent {
        height: 100px;
        text-align: center;
        background-color: aliceblue;
    }
    .child {
        height: 50px;
        width: 100px;
        display: inline-block;
		background-color: orange;
        vertical-align: center;
    }
    .parent::before {
        content: "";
        height: 100px;
        display: inline-block;
        vertical-align: middle;
        background-color: yellow;
    }
</style>
<body>
    <div class="parent">
        <div class="child">content</div>
    </div>
</body>
```

<div class="parent" style="background-color: aliceblue;height: 100px;text-align:center;">
    	<div class="before" style="width:5px;height: 100px;display: inline-block; vertical-align: middle;background-color: yellow;"></div>
        <div class="child" style="background-color: orange;height: 50px;width: 100px;display: inline-block;vertical-align: middle;">content</div>
    </div>

为了方便观察，这里效果中给模拟的伪元素增加了5px的宽度，实际不用给伪元素设置宽度。

