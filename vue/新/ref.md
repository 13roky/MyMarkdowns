# ref

## 概述

>  我们在使用 Vue 时，有时会需要拿到 DOM 元素进行操作。这时 Vue 不推荐我们进行使用原生 JS 获取 DOM 元素。而是为我们提供了 ref 方便我们去获取组件中的 DOM 元素。

## 使用 ref 

在使用时，我们只要给标签加上 ref 属性并指定属性名，就可以在 Vue 中通过 vm 实例的 $refs 属性获取到我们添加 ref 属性值的标签。

### demo

```html
<template>
	<div id="app">
		<h1 ref="myh1">test ref1</h1>
		<button ref="myButton" @click="showRef">print ref</button>
		<HelloWorld ref="hello"></HelloWorld>
	</div>
</template>

<script>
	import HelloWorld from './components/HelloWorld.vue'

	export default {
		name: 'App',
		components: {
			HelloWorld
		},
		methods: {
			showRef() {
				console.log(this);
				console.log(this.$refs.myh1);
				console.log(this.$refs.myButton);
				console.log(this.$refs.hello);
			}
		}
	}
</script>
```

### 在 HTML 标签中添加 ref

- 在 HTML 标签中添加 ref 时，通过 $refs 拿到的是真实 DOM 元素。
- 可以通过 JS 原生对 DOM 的操作方法进行操作。

### 在组件标签中添加 ref

- 在组件标签中添加 ref 时，通过 $refs 拿到的是该组件的实例对象 vc。

- 如果想拿到组件的所有 HTML 标签，可以给组件追加 id，然后通过原生的 JS 获取到的就是 组件内部所有的 HTML 标签。

### 关于 $refs

- Vue 会将我们添加 ref 的标签，以上面的形式，添加到当前实例的 $refs 属性中，以便我们的调用。