# Vue 动态组件

## 概述

动态组件就是指**动态的切换组件的显示与隐藏**。

动态组件允许用户在一个框架内，切换框架内已经写好的一些组件。

## 使用

vue 为我们提供了一个 `component` 标签，它允许我们来在其中动态的渲染我们指定的组件。

在 `component` 标签中，`is` 属性中指定了需要动态渲染的组件，如果我们需要组件时，只需要改变此属性即可。

```vue
<componet :is="comName"></componet>
<button @click="conName = 'Left'">展示 Left 组件</button>
<button @click="conName = 'Right'">展示 Right 组件</button>

data(){
	return { comName: 'Left' }
},
components: {
	Left,
	Right
}
```

## 状态保持

在默认情况下，`component` 标签动态渲染的组件在进行组件切换时，被切换的组件会被销毁，这样再次切换回来时之前的组件会被重新创建，这样就导致了之前的组件状态不能被保存。

我们可以使用 `keep-alive` 标签，来指定动态组件被切换时，不被销毁，而是缓存在内存中，这样再次切换回来时，就会保留之前的状态。

```vue
<keep-alive>
	<componet :is="comName"></componet>
</keep-alive>
```

**include 属性**

在 `keep-alive` 标签未绑定 `include` 属性时，默认会将所有组件进行状态保持。

当指定了 `include` 属性是，只会将指定的组件进行状态保存。

```vue
<keep-alive include="MyLeft, MyRight">
	<componet :is="comName"></componet>
</keep-alive>
```

