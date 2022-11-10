# vue 过滤器

## 概述

**过滤器（Filters）**是 vue 为开发者提供的功能，常用于**文本的格式化**。过滤器可以用在两个地方：**插值表达式**和 **v-bind 属性绑定**。

过滤器应该被添加在 JavaScript 表达式的尾部，由“管道符”进行调用，示例代码如下：

```js
<p>{{ message | formatA | format }}</p>

<div :id="message | formatA | formatB"></div>

const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    },
    filters: {
        formatA (str) {
            return str + " A"
        },
        formatB (str) {
            return str + " B"
        }
    }
})
```

## 过滤器传参

过滤器类似于函数、里面可以进行传参。

```js
<p>{{ message | formatA(arg1) }}</p>

const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    },
    filters: {
        formatA (str, arg1) {
            return str + " A" + arg1
        }
    } 
})
```

## 全局过滤器

写在对应 vue 实例里面的是私有过滤器，只允许当前实例使用。

如果需要所有实例都能使用，可以设置全局过滤器。在创建实例前，对 vue 添加全局过滤器。

```js
<p>{{ message | formatA(arg1) }}</p>

Vue.filter('formatA', (str,arg1)=>{
    return str + " A" + arg1
})

const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    }
})
```

## 过滤器的兼容性

过滤器仅在 vue2.x 和 1.x 中受支持，在 vue3.x 的版本中剔除了过滤器相关功能。

在企业级项目开发中：

- 如果使用的是 2.x 版本的 vue，则一眼可以使用过滤器相关的功能。
- 如果项目已经升级到了 3.x 版本的 vue，官方建议使用计算属性或方法代替被剔除的过滤器功能。

## 使用方法替代过滤器

```js
<p>{{ formatA(message) }}</p>

const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    },
    methods: {
        formatA(str){
            return str + " A"
        }
    }
})
```

## demo

```html
<body>
	<div id="app">
		<h2>显示格式化后的时间</h2>
		<h3>时间戳：{{time}}</h3>
		<!-- 计算属性实现 -->
		<h3>时间：{{fmtTime}}</h3>
		<!-- 方法实现 -->
		<h3>时间：{{getTime()}}</h3>
		<!-- 过滤器实现 -->
		<h3>时间：{{time | filterTime("YYYY-MM-DD HH:mm") | mySlice}}</h3>
		<h3 :x="info | mySlice">过滤器也可以用在属性上</h3>

	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/dayjs/1.11.5/dayjs.min.js"></script>
<script>
	const vm = new Vue({
		data: {
			time: 1621561377603, // 时间戳
			info: "123456"
		},
		computed: {
			fmtTime() {
				return dayjs(this.time).format("YYYY-MM-DD HH:mm:ss")
			}
		},
		methods: {
			getTime() {
				return dayjs(this.time).format("YYYY-MM-DD HH:mm:ss")
			}
		},

		filters: {
			filterTime(v, str = "YYYY-MM-DD HH:mm:ss") {
				return dayjs(v).format(str)
			},
			mySlice(v) {
				return v.slice(0, 4)
			}
		}
	}).$mount("#app")
</script>
```



