# mixin

## 概述

> 在 Vue 中我们的很多组件的配置都会有共用的数据，方法，钩子等等，我们可以把这些共用的部分提取到一个 js 文件中，并导入到需要使用的组件中，通过 mixins 配置项进行使用。

我们需要创建一个 js 文件，名字随意，这里我们叫做 mixin.js

```js
export const mixin {
    
}
```

之后在需要这个混入的组件中导入。

然后通过 mixins 配置项进行配置。

## 使用

minxi.js

```js
export const mixin = {
	data() {
		return {
			name: "mixin",
			age: "now"
		}
	},
	methods: {
		showName() {
			alert(this.name)
		}
	}
}

export const mixin2 = {
	mounted(){
		console.log("this is mixin2");
	}
}
```

TestMixins.vue

```html
<template>
	<div>
		<p>{{name}}</p>
		<p>{{age}}</p>
		<button @click="showName">mixin</button>
	</div>
</template>

<script>
	import {
		mixin,
		mixin2
	} from '../mixin.js'

	export default {
		data() {
			return {
				name: "brokyz"
			}
		},
		mixins: [mixin, mixin2]
	}
</script>
```

- 如果 mixin.js 中的配置与需要配置 mixins 的组件重合时，将以组件里面的配置为主。也就是说组件和 mixin 相同的未知，组件会将其覆盖。
- 在使用声明周期函数时，minxi 和 组件都设置了相同的钩子时， 两个都会调用，而且 mixin 中的会优先执行。