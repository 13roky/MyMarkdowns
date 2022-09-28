## 概述

**组件化开发**就是根据**封装**的思想，把页面上**可重用的 UI 结构封装为组件**，从而方便项目的开发和维护

在 vue 框架中对组件化开发的支持度非常高。

vue 中规定：组件的后缀名是 .vue。之前接触到的 App.vue 文件本质上就是一个 vue 的组件。

在 vue 组件中有三个组成部分：

1. `template`组件的**模板结构**

1. `script`组件的 **JavaScript 行为**
2. `style`组件的**样式**

在工程化的项目中，vue 要做的事情很单纯：通过 main.js 把 App.vue 渲染到 index.html 的指定区域中。

在 App.vue 中我们就可以使用我们封装好的自定义组件。

**template**

```html
<template>
	<!-- 组件的 DOM 结构，需要被定义到这里 -->
</template>
```

注意：

- `template` 是 vue 提供的容器标签，只起到包裹性质的作用，它不会被渲染为真正的 DOM 元素
- `template` 中**只能包含唯一的根节点**

**script**

```html
<script>
	// 组件相关的 data 数据、methods 方法等
    // 都需要定义到 export default 所导出的对象中
    export default {}
</script>
```

**style**

```html
<style lang="less">
	<!-- 组件的样式，加了`lang="less"`之后就可以支持 less 语法 -->
</style>
```

## 组件的一个完整模板

show.vue

```vue
<template>
	<div>
        <p>{{ hello }}</p>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                hello: 'vue 组件模板'
            }
        }
    }
</script>

<style lang="less">
    div {
        p {
            color: skyblue;
        }
    }
</style>
```

## 使用组件

useShow.vue

```vue
<template>
	<div class="box">
        <!-- 3. 以标签形式使用刚才注册的组件 -->
        <show></show>
    </div>
</template>

<script>
    // 1. 使用 import 导入要使用的组件
    import show from '@/components/show.vue'
    export default {
        // 2. 使用 components 节点注册组件
        components: {
            'show': show
            // 如果没有自定义名字，可以直接简写 show
        }
    }
</script>

<style lang="less">

</style>
```

**注册全局组件**

```js
import show from '@/components/show.vue'

Vue.component('myShow', show)
```

## 组件的 props

props 是组件的**自定义属性**，在**封装通用组件**的时候，合理地使用 props 可以极大的**提高组件的复用性**！

vue 规定：组件中封装的自定义属性是只读的，程序员不能直接修改 props 的值。否则会直接报错。

要想修改 props 的值，可以把 props 的值转存到 data 中，因为 data 中的数据都是可读可写的！

```js
export default {
    props: ['init','init2'],
    data () {
        return {
            count: this.init,
            count2: this.init2
        }
    }
}
```

自定义属性，存在三个可选值：`default`、`type`、`required`

```js
export default {
    prop: {
        init: {
            // 定义属性的默认值
            default: 0,
            // 定义属性值的类型，如果不符合则会报错
            type: Number,
            // 使用组件时，必须传递此属性值
            required: true
        }
    }
}
```

## 组件之间的样式冲突问题

默认情况下，写在 .vue 中的样式会全局生效，这样就会造成多个组件之间的样式冲突问题。

原因：

1. 单页面应用程序中，所有组件的 DOM 结构，都是基于唯一的 index.html 页面进行呈现的
2. 每个组件中的样式，经过 webpack 构建后会整合到一起，都会影响整个 index.html 页面中的 DOM 元素

解决原理：

为每个组件分配一个唯一的自定义属性，在编写组件样式时，通过属性选择器来控制具有指定属性的标签

```vue
<template>
	<div class="container" data-v-0001>
       <h3>解决样式冲突</h3>
    </div>
</template>

<style>
    .container[data-v-0001] {
        color: skyblue
    }
</style>
```

**style 的 scoped 属性**

为了提高开发效率和开发体验，vue 为 style 节点提供了 scoped 属性，从而防止组件之间的样式冲突问题。

scoped 时以上解决冲突原理的一个实践，其会自动为组件分配自定义属性，并自动为当前组件的 DOM 和 style 样式应用。

```vue
<template>
	<div class="container">
       <h3>解决样式冲突</h3>
    </div>
</template>

<style scoped>
    .container {
        color: skyblue
    }
</style>
```

## /deep/ 样式穿透

由于我们使用了 scoped 属性，所以当前组件的样式对子组件是不生效的。如果我们想让样式对子组件生效。那么可以使用 /deep/。

```vue
<style scoped>
    .containe {
        color: skyblue;	/* 不加deep时，生成的样式为 .title[data-v-00001] 只选择当前组件 */
    }
    
    /deep/ .title {
        color: skyblue; /* 加deep时，生成的样式为 [data-v-00001] .title 选择当前组件和子组件 */
    }
</style>
```

