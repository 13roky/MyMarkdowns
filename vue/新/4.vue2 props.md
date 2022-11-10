# props

## 概述

> 我们的组件进行展示数据时，里面并不是都是写死的。我们需要在使用组件时，向组件内部传值，并展现我们需要的值。

## 数组形式接收 props

TestProps.vue

```html
<template>
	<div>
		<h2>name: {{name}}</h2>
		<h2>sex: {{sex}}</h2>
		<h2>age: {{myAge}}</h2>
		<button @click="myAge++">myAge++</button>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				myAge: this.age
			}
		},
		props: ['name', 'age', 'sex']
	}
</script>
```

App.vue

```html
<template>
	<div id="app">
		<testProps name="brokyz" sex="male" :age="21"></testProps>
	</div>
</template>

<script>
	import TestProps from './components/TestProps.vue'
	export default {
		name: 'App',
		components: {
			TestProps
		}
	}
</script>
```

**注意：**

1. 使用 props 传入值时，默认传入都时字符串。如果需要传入数字型，则需要在传入属性前加冒号。如`:age="21"`传入的时数字 21，`age="21"`传入的是字符串 21。
2. 在传入 props 后，如果组件需要对传入值进行修改操作，需要在 data 函数中定义，否则报错。在创建的时候是先准备好 props 之后才配置数据 data。

## 对象形式接收 props

TestProps.vue

```html
<template>
	<div>
		<h2>name: {{name}}</h2>
		<h2>sex: {{sex}}</h2>
		<h2>age: {{myAge}}</h2>
		<button @click="myAge++">myAge++</button>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				myAge: this.age
			}
		},
		props: {
			name: String,
			age: {
				type: Number,
				required: true
			},
			sex: {
				type: String,
				default: "none"
			}
		}
	}
</script>
```

App.vue

```html
<template>
	<div id="app">
		<testProps name="brokyz" :age="21"></testProps>
	</div>
</template>

<script>
	import TestProps from './components/TestProps.vue'
	export default {
		name: 'App',
		components: {
			TestProps
		}
	}
</script>
```

- 对象的形式来声明 props 可以让 props 的接收更加具体，可以指定类型，默认值，和是否必传。
- 一般来说 default 和 required 不同时出现，因为不传才会有默认值，而必传又不需要默认值。