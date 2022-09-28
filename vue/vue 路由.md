# Vue 路由

## 概述

**路由**就是**对应关系**。在前端中的路由指的是 **Hash 地址**和**组件**之间的**对应关系**。

通俗来理解，路由就是帮助我们通过控制 Hash 地址来完成页面中的组件的切换。

**SPA 就是单页面网站**，在 web 网站中只有一个页面，所有的组件的展示与切换都在这个唯一的页面进行完成，**不会发生页面跳转**。这种不同组件之间的切换就需要**通过前端路由**来完成。

Hash 地址在前端中指的是 # 之后的地址，这个地址在转换的时候不会发生页面跳转。如 https://brokyz.tk/#/header/sub 中 /header/sub 就是 Hash 地址。

前端路由的工作方式：

1. 用户点击了页面上的路由链接。
2. 导致 URL 地址栏中的 Hash 值发生变化。
3. 前端路由监听到了 Hash 值的变化。
4. 前端路由把当前 Hash 值对应的组件渲染到页面当中。

## 使用动态组件模拟路由

```vue
<template>
  <div class="imicontainer">
    <h1>imiRouter</h1>
    <a href="#/about">about</a>
    <a href="#/home">home</a>
    <a href="#/mine">mine</a>

    <component :is="comName"></component>
  </div>
</template>

<script>
import about from "@/components/about.vue";
import home from "@/components/home.vue";
import mine from "@/components/mine.vue";
export default {
  name: "imiRouter",
  data() {
    return {
      comName: "home",
    };
  },
  components: {
    about,
    home,
    mine,
  },
  created(){
    window.onhashchange = () => {
      switch(location.hash) {
        case '#/home':
          this.comName = 'home'
          break
        case '#/about':
          this.comName = 'about'
          break
        case '#/mine':
          this.comName = 'mine'
          break
      }
    }
  }
};
</script>

<style scoped>
  a {
    padding: 0 10px;
  }
</style>
```

## vue-router

### 概述

**vue-router** 是 vue.js 官方给出的**路由解决方案**。他只能结合 vue 项目进行使用，能够轻松的管理 SPA 项目中组件的切换。

官方文档：https://router.vuejs.prg/zh/

### vue-router 安装

在 vue2 中安装需要安装 vue-router3 和其一下的版本，因为更新的版本对 vue2 不支持。需要到 vue3 才可以使用。

所以需要安装时一定要带上版本号。

```
npm i vue-router@3 -S
```

 ### 创建路由模块

在 src 源代码目录下，新建 `router/index.js` 路由模块，并初始化如下的代码：

```js
// 1. 导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

// 2. 通过 Vue.use() 函数，把 VueRouter 安装为 Vue 的组件
Vue.use(VueRouter)

// 3. 创建路由的实例对象
const router = new VueRouter()

// 4. 向外共享路由的实例对象
export default router
```

### 导入并挂载路由模块

在 `src/main.js` 入口文件中，导入并挂载路由模块。

```js
import Vue from 'vue'
import App from './App.vue'
// 导入路由模块
import router from '@/router'

// 浏览器控制台提示信息
Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  // 可以省略为 router
  router:router
}).$mount('#app')
```

### 声明路由链接和占位符

在 `src/App.vue` 组件中，使用 vue-router 提供的 `<router-link>` 和 `<router-view>` 声明路由链接和占位符。

```vue
<template>
  <div id="app">
    <h1>vue-router</h1>
    <router-link to="/home">home</router-link>
    <router-link to="/mine">mine</router-link>
    <router-link to="/about">about</router-link>
    <hr>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "app",
}
</script>

<style lang="less">
  #app {
    a {
      padding: 0 10px;
    } 
  }
</style>
```

### 声明路由的匹配规则

在 `src/router/index.js` 路由模块中，通过 routes 数组声明路由的匹配机制。

```js
// 1. 导入 Vue 和 VueRouter 的包
import Vue from 'vue'
import VueRouter from 'vue-router'

// 2. 通过 Vue.use() 函数，把 VueRouter 安装为 Vue 的组件
Vue.use(VueRouter)

// 3. 创建路由的实例对象
import about from "@/components/about.vue";
import home from "@/components/home.vue";
import mine from "@/components/mine.vue";
const router = new VueRouter({
  routes: [
    {path: '/home', component: home},
    {path: '/mine', component: mine},
    {path: '/about', component: about},
  ]
})

// 4. 向外共享路由的实例对象
export default router
```

### 路由重定向

路由重定向指的是：用户在访问地址 A 的时候，强制用户跳转到地址 C ，从而展示特定的组件页面。

通过路由规则的 redirect 属性，指定一个新的路由地址，可以很方便地设置路由的重定向。

```js
const router = new VueRouter({
  routes: [
     // 当用户访问 / 的时候，通过重定向跳转到 /home 对应的路由规则
    {path: '/', redirect: '/home'},
    {path: '/home', component: home},
    {path: '/mine', component: mine},
    {path: '/about', component: about},
  ]
})
```

### 嵌套路由

通过路由实现组件的嵌套展示。

```vue
<template>
  <div id="home">
    <h4>home</h4>
    <router-link to="/home/tab1">tab1</router-link>
    <router-link to="/home/tab2">tab2</router-link>
    <router-view></router-view>
  </div>
</template>
```

```js
import about from "@/components/about.vue";
import home from "@/components/home.vue";
import mine from "@/components/mine.vue";
import tab1 from "@/components/tab1.vue";
import tab2 from "@/components/tab2.vue";
const router = new VueRouter({
  routes: [
    { path: '/', redirect: '/home' },
    {
      path: '/home',
      component: home,
        // 默认子路由跳转到tab1
      redirect: '/home/tab1',
      children: [
        { path: 'tab1', component: tab1 },
        { path: 'tab2', component: tab2 }
      ]
    },
    { path: '/mine', component: mine },
    { path: '/about', component: about },
  ]
})
```

### 动态路由

如果不同的路由地址，同时使用同一个组件，不用一个个绑定路由，可以使用动态路由。

```vue
<template>
  <div id="about">
    <h4>about</h4>
    <router-link to="/about/id-000001">content1</router-link>
    <router-link to="/about/id-000002">content2</router-link>
    <router-link to="/about/id-000003">content3</router-link>
    <router-view></router-view>
  </div>
</template>
```

```js
const router = new VueRouter({
  routes: [
    { path: '/', redirect: '/home' },
    {
      path: '/home',
      component: home,
      redirect: '/home/tab1',
      children: [
        { path: 'tab1', component: tab1 },
        { path: 'tab2', component: tab2 }
      ]
    },
    { path: '/mine', component: mine },
    {
      path: '/about',
      component: about,
      // 可以使用如下的动态路由
      // children: [
      //   { path: 'id-000001', component: tab1 },
      //   { path: 'id-000002', component: tab1 },
      //   { path: 'id-000003', component: tab1 }
      // ]
      children: [
        { path: ':id', component: tab1}
      ]
    },
  ]
})
```

### 动态路由传递路由参数

在路由渲染出的组件中，可以使用 `this.$route.params` 来访问获取到的动态匹配值

```vue
<template>
  <div id="tab1">
    <h4>tab1</h4>
    <P>动态路由参数 this.$route.params.id ：{{ this.$route.params.id }}</P>
  </div>
</template>
```

**也可以在路由参数中，开启 `props` 传参**

```js
{
      path: '/about',
      component: about,
      children: [
        { path: ':id', component: tab1, props: true }
      ]
}
```

```vue
<template>
  <div id="tab1">
    <h4>tab1</h4>
    <P>动态路由参数 props 接收 id：{{ id }}</P>
  </div>
</template>
<script>
export default {
  name: "tab1",
  props: ['id']
};
</script>
```

**拓展 `this.$route` 路由对象**

```js
$route: Object
    fullPath: "/about/id-000003?id=4&test=1"
    hash: ""
    matched: (2) [{…}, {…}]
    meta: {}
    name: undefined
    params: {id: 'id-000003'}
    path: "/about/id-000003"
    query: {id: '4', test: '1'}
    [[Prototype]]: Object
```

- fullPath：完整的路由地址。
- path：不带 query 的路由地址。
- params：动态路由参数。
- query：路由中的 query 值。

注意：`this.$route` 是路由参数对象，`this.$router` 是路由导航对象。

### 编程式导航 API

在浏览器中，点击链接实现导航的方式，叫做**声明式导航**。例如：普通网页中点击 `<a>` 链接、vue 项目中点击 `<router-link>` 都属于声明式导航。

在浏览器中，调用 API 方法实现导航的方式，叫做**编程式导航**。例如：普通网页中调用 `location.href` 跳转到新页面的方式，属于编程式导航。

vue-router 提供了许多**编程式导航**的 API，其中最常用的导航 API 分别是：

1.  `this.$router.push('hash 地址')`： 跳转到指定 hash 地址，并**增加一条历史记录**。

2. `this.$router.replace('hash 地址')` ：跳转到指定的 hash 地址，并**替换掉当前的历史记录**。

3. `this.$router.go(数值 n)`：实现导航历史前进、后退。
   - $router.back()：在历史记录中，后退到上一个页面。
   - $router.forward()：在历史记录中，前进到下一个页面。

### 导航守卫

**导航守卫**可以**控制路由的访问权限**。

![](https://pic.imgdb.cn/item/62fcdaa816f2c2beb1b82f6a.jpg)

**全局前置守卫**

每次发生路由的导航跳转时，都会触发全局前置守卫。因此，在全局前置守卫中，程序员可以对每个路由进行访问权限的控制。

```js
const router = new VueRouter({...})
// 调用 beforeEach 方法可以声明全局前置守卫
// 每次发生路由导航跳转时，都会自动触发回调函数          
router.beforeEach((to, from, next)=>{
    // to 是要访问的路由信息对象
    // from 是要离开的路由信息对象
    // next 是一个函数，调用 next() 表示放行，允许这次路由导航。
})                              
```

next 函数的三种调用方式：

![](https://pic.imgdb.cn/item/62fcdc2716f2c2beb1b92eb6.jpg)

当前用户拥有后台主页的访问权限，直接放行：next()

当前用户没有后台主页的访问权限，强制其跳转到登录页面：next('/login')

当前用户没有后台主页的访问权限，不允许跳转到后台主页：next(false)

```js
router.beforeEach((to, from, next) => {
    if (to.path === '/main') {
        const token = localStorage.getItem('token')
        if (token) {
            next()	// 访问的是 main 并且有 token 值，放行
        } else {
            next('/login')	// 访问的是 main 但是没有 token，跳转到登陆页面
        }
    } else {
        next()	// 访问的不是 main，直接放行
    }
})
```

