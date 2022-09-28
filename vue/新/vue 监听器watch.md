# watch 监听器

## 概述

**watch 监听器**允许开发者动态监视数据的变化，从而**针对数据的变化做出特定的操作**。

通俗的来理解可以理解为设定了一个函数，当指定的数据发生变化之后，vue 就会帮我们自动去触发这个函数，这就是 watch 监听器。

在 watch 监听器中，函数可以接收两个参数 `newVal`、`oldVal`。这两个参数分别**记录**了变化的**新值**和变化之前的**旧值**。

```js
const vm = new Vue({
    el: '#app',
    data: { username:'' },
    watch: {
        username(newVal, oldVal) {
            console.log(newVal, oldVal)
        }
    }
})
```

```js
vm.$watch('username', {
    handler(newValue, oldValue) {
        console.log(newVal, oldVal)
    }
})
```



## 立即调用 immediate 选项

默认情况下，**组件在初次加载完毕后不会调用 watch 侦听器**。如果想让 watch 侦听器立即被调用，则需要使用 immediate 选项。示例代码如下：

```js
const vm = new Vue({
    el: '#app',
    data: { username:'' },
    watch: {
        username: {
            // handler 固定写法，表示 username 发生变化时，自动调用 handler 处理函数
            handler: function(newVal) {
                console.log(newVal)
            },
            // 表示页面初次渲染好之后，就立即触发当前的 watch 监听器
            immediate: true
        }
    }
})
```

## 监听对象 deep 选项

如果 watch 监听的是一个**对象**，那么对象的属性值发生了变化，则**无法被监听到**。此时**需要使用 deep 选项**。

```js
const vm = new Vue({
    el: '#app',
    data: {
        info: { username: 'brokyz' }
    },
    watch: {
        info: {
            // handler 固定写法，表示 username 发生变化时，自动调用 handler 处理函数
            handler: function(newVal) {
                console.log(newVal.username)
            },
            // 监听一个对象时，如果要监视内部属性变化需要加deep参数为true
            deep: true
        }
    }
})
```

## 监听对象的单个属性变化

如果只想要监听对象中单个属性的变化，可以按照如下形式：

```js
const vm = new Vue({
    el: '#app',
    data: {
        info: { username: 'brokyz' }
    },
    watch: {
        'info.username': {
            handler(newVal) {
                console.log(newVal)
            }
        }
    }
})
```

## 针对与watch与computed的延时返回问题

### computed

```html
<body>
  <div id="app">
    <p>firstname: {{firstname}}, lastname: {{lastname}}.</p>
    <p>fullname: {{fullname}}</p>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    data: {
      firstname: "zhang",
      lastname: "boqiang",
    },
    computed: {
      fullname() {
        setTimeout(() => {
          console.log("计时器被调用");
          return this.firstname + "-" + this.lastname;
        }, 1000);
      },
    },
  }).$mount("#app");
</script>
```

- 在计算属性中，由于return在定时器中，所以这个return是定时器的return。因此计算属性并没有进行return。所以fullname并没有值。
- 如果想要定时器延迟输出则需要使用watch。

### watch

```html
<body>
  <div id="app">
    <p>firstname: {{firstname}}, lastname: {{lastname}}.</p>
    <p>fullname: {{fullname}}</p>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    data: {
      firstname: "zhang",
      lastname: "boqiang",
      fullname: "zhang-booqiang",
    },
    watch: {
      firstname(newV) {
        setTimeout(function () {
          this.fullname = newV + "-" + this.lastname;
        }, 2000);
      },
      lastname(newV) {
        setTimeout(() => {
          this.fullname = this.firstname + "-" + newV;
        }, 2000);
      },
    },
  }).$mount("#app");
```

- 如果延时输出那么定时器一定要使用箭头函数，因为箭头函数中的this，指向的是外层的vue实例，可以通过this访问到vue实例的属性。如果不使用箭头函数，那么this指向的就是windows对象。

注意：computed能完成的操作，watch都能够完成。但是watch能完成的工作，computed不一定能完成。比如异步操作。
