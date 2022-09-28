# 组件之间的数据共享

## 概述

Vue 中的组件是被封装出去相互独立的模块，在使用组件时，组件与组件之间不能互相读取数据，这时就需要我们对指定数据进行数据共享操作。

组件之间关系类似于树状结构，最常见的关系莫过于如下：

1. 父子关系：并非严格的父子关系，其中可以跨代。
2. 兄弟关系：并非严格的兄弟关系，可以跨节点。

[![vYyOMj.png](https://s1.ax1x.com/2022/08/12/vYyOMj.png)](https://imgtu.com/i/vYyOMj)

## 父 -> 子共享数据

父向子共享数据需要用到**自定义属性**。

父组件：

```js
<Son :msg="message" :user="userinfo"></Son>

data() {
    return {
        message: 'hello vue.js',
        userinfo: { name: 'brokyz', age: '21' }
    }
}
```

子组件：

```js
<template>
    <div>
    	<p>父组件传递的 msg 是 {{msg}}</p>
		<p>父组件传递的 user 是 {{user}</p>
    </div>
</template>

props: ['msg', 'user']
```

注意：使用自定义属性时，建议做只读操作，尽量不要做修改。如需修改应该使用 data 接收。

## 子 -> 父共享数据

子组件向父组件传递数据时使用**自定义事件**。

子组件：

```js
export default {
    data () {
        return { count: 0 }
    },
    methods: {
        add () {
            this.count++
            
            this.$emit('numChange', this.count)
        }
    }
}
```

父组件：

```js
<Son @numChange="getNewCount"></Son>

export default {
    data () {
        return { countFromSon: 0 }
    },
    methods: {
        getNewCount(val) {
            this.countFromSon = val
        }
    }
}
```

## 兄弟之间共享数据

在 **vue2** 中，兄弟组件之间共享数据的方案是 **EventBus**。

eventBus.js：

```js
import Vue from 'vue'
// 向外共享 Vue 实例
export defaulte new Vue()
```



兄弟组件 A：将数据发送给 B

```js
import bus from './eventBus.js'

export default {
    data () {
        return {
            msg: 'hello vue.js'
        }
    },
    methods: {
        sendMsg () {
            bus.$emit('share', this.msg)
        }
    }
}
```

兄弟组件 B：接受组件 A 发来的数据

```js
import bus from './eventBus.js'

export default {
    data () {
        return {
            msgFromA: ''
        }
    },
    created () {
        bus.$on('share', val => {
            this.msgFromA = val
        })
    }
}
```