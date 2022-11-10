# Vue 自定义指令

## 概述

vue 中不不仅仅有官方提供的指令，用户还可以根据自己的需要进行自定义指令。

比如当我们需要一个常用的操作将文字改为蓝色，如果我们需要修改大量标签时，就可以使用自定义指令。免去修改每一个标签的麻烦。

## 私有自定义指令

我们可以在 `directives` 节点下声明私有自定义属性。

**在自定义指令的函数中，this指向的时window。**

### 函数式：指令与元素成功绑定时和指令所在模板重新解析时都会调用。

```html
<body>
	<div id="app">
		<h2 class="myh2" v-color="'pink'">test directives</h2>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
	const vm = new Vue({
		directives: {
			color(element, binding) {
				// element是当前绑定的DOM元素，binding是一个存于着当前自定义属性相关参数的对象
				console.log(element, binding);
				element.style.color = binding.value
			}
		}
	}).$mount("#app")
</script>
```

### 对象式：可以分别指定不同阶段所调用的函数。

```vue
<body>
	<div id="app">
		<input type="text" v-focus :value="n"><br>
		<button @click="n++">更新页面模板</button>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
	const vm = new Vue({
		data: {
			n: 1,
		},
		directives: {
			focus: {
				// 指令与元素成功绑定时调用
				bind(element) {
					element.focus()
				},
				// 指令所在元素被插入页面时调用
				inserted(element) {
					element.focus()
				},
				// 指令所在的模板被重新解析时调用
				update(element) {
					element.blur()
				},
			}
		}
	}).$mount("#app")
</script>
```

## 多个自定义指令单词用 - 隔开

```html
<body>
	<div id="app">
		<input type="text" v-focus-element :value="n"><br>
		<button @click="n++">更新页面模板</button>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
	const vm = new Vue({
		data: {
			n: 1,
		},
		directives: {
			'focus-element': {
				// 指令与元素成功绑定时调用
				bind(element) {
					element.focus()
				},
				// 指令所在元素被插入页面时调用
				inserted(element) {
					element.focus()
				},
				// 指令所在的模板被重新解析时调用
				update(element) {
					element.blur()
				},
			}
		}
	}).$mount("#app")
</script>
```

## 全局自定义指令

```js
Vue.directive('color', function(el, binding) { 
  el.style.color = binding.value
})
```
