# ref 引用

## 概述

ref 是组件的属性，用来辅助开发者在**不依赖 jQuery 的情况**下，**获取 DOM 元素或组件**的引用。

每个 vue 的组件实例上，都包含一个 **$refs 对象**，里面存储着对应的 DOM 元素或组件的引用。**默认情况下，组件的 $refs 指向一个空对象。**

## 使用 ref 引用 DOM 元素

如果想要使用 ref 引用页面上的 DOM 元素，则可以按照如下方式操作：

```js
<h3 ref="myh3">MyRef</h3>
<button @click="getRef">获取 $refs 引用</button>

methods: {
    getRef () {
        // 通过 this.$refs 引用的名称 可以获取到 DOM 元素的引用
        console.log(this.$refs.myh3)
        // 操作 DOM 元素，把文本颜色改为红色
        this.$refs.myh3.style.color = 'red'
    }
}
```

## 使用 ref 引用组件实例

如果想使用 ref 引用页面上的组件实例，则可以按照如下的方式进行操作：

```js
<counter ref="counterRef"></counter>
<button @click="getRef">获取 $refs 引用</button>

methods: {
    getRef () {
        // 通过 this.$refs 引用的名称 可以引用组件实例
        console.log(this.$refs.counterRef)
        // 引用到组件的实例之后，就可以调用组件上的 methods 方法
        this.$refs.counterRef.add()
    }
}
```

注意：使用 $refs 获得的是原生的 DOM 对象时，可以对其进行原生对象的操作。

```js
<input type="text" v-if="flag" ref="ipt" placeholder="v-if">
<button @click="showinput" v-else>输入框</button>

showinput() {
  this.flag = true;
  this.$refs.ipt.focus();
}
```

以上代码并不能使得文本框获取焦点，会报错。因为当 v-if 改为 true 时，更新的 DOM 元素还没有更新完成，所以不能立马获取到该元素。得等下一个周期才可以获取到，也就是说点击第二次的时候，文本框才会获取焦点。

这时我们可以使用 `this.$nextTick(cb)` 方法，**此方法会其中的回调函数 cb 推迟到下一个 DOM 更新周期之后执行。**这样一来回调函数中就可以操作到最新的 DOM 元素。

```js
<input type="text" v-if="flag" ref="ipt" placeholder="v-if">
<button @click="showinput" v-else>输入框</button>

showinput() {
  this.flag = !this.flag;
  this.$nextTick(() => {
    if (this.$refs.ipt) {
    	this.$refs.ipt.focus()
    }
  });
}
```

