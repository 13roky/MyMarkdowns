# vue3 工程架构

## 使用 vite 搭建 vue3 工程

在 Vue3 中，官方推荐使用 vite 进行工程搭建。

vite 由 Vue 团队进行维护，并且相比 webpack 由于采用了冷启动技术，拥有更快的启动速度。

创建工程

```
npm init vite-app <project-name>
```

工程创建完成后，vite 需要手动安装依赖：

```
npm i
```

## Vue3 工程架构的变化

相比于 vue2，vue3 最大的工程架构变化就是 main.js 入口文件。其样式如下：

```js
// 引入的不是 Vue 构造函数了，引入的是一个名为 createApp 的工厂函数
import { createApp } from "vue"
import App from "./App.vue"

// 创建应用实例对象--app(类似于之前 vue2 中的 vm, 但是 app 比 vm 更"轻")
createApp(App).mount("#app")

// const app = createApp(App)
// app.mount("#app")
```

**Vue3 中组件模板结构可以没有根标签。**