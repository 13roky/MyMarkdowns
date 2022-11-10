## redux

**在此我们只介绍 redux 核心包的使用。**

目前 `redux` 官方已将建议使用 `Redux Toolkit` 进行项目状态管理。`Redux Toolkit` 中包括了 `redux` 核心包，其经过了大量的 `redux` 使用经验汇聚而成，是我们使用 `redux` 的最佳使用方式，其简化了许多流程，并且防止了许多常见错误，是我们更简单地创建一个 redux 应用。

在此我们只介绍使用 redux 核心包创建传统的 redux 应用，并不介绍使用最新 Redux Toolkit 来创建 redux 应用。

### 为什么使用 redux

在 `react` 中，我们将每个组件需要的状态存储在当前组件中。如果组件中存在共用状态的情况，我们将状态存储在它们共同的父级组件中，然后以 `props` 的形式将状态和操作状态的方法传递给需要的子组件。

当组件中结构过于复杂，我们需要进行多次 `props` 传递，这种方式不利于我们进行开发与维护。因此我们需要一个可以集中式管理状态的工具，让我们每个需要的组件都能和我们集中管理的状态进行交互。于是 `redux` 也就被我们广泛使用了。

`redux` 是 `JavaScript` 应用的状态容器，提供可预测的状态管理。它可以和任何应用一起使用，但是我们经常将 `redux` 和 `react` 一起使用。我们将状态存储在 `redux` 中，方便我们不同组件间与状态的交互。

### redux 状态管理的运作原理

![](https://s1.ax1x.com/2022/10/29/x5uY1U.png)



`redux` 包含三个部分：`action creators`、`store`、`reducers`。**其中 Store 为核心。**

**总体流程为：**

1. 页面初始化时，`Store` 第一次调用 `Reducers`，`Reducers` 将初始化的状态返回给  `Store`。
2. 组件修改状态时，由组件触发 `Action Creators`。
3. `Action Creators` 被触发，创建事件对象 `action` `{type: 'actionType', data: 'actionData'}`，并将其分发（`dispatch`）给 `Store`。
4. `Store` 将接收到的 `action` 传递给 `Reducers`，由 `Reducers` 处理 `action`，并将处理后的新状态返回给 `Store`。
5. `Store` 接收到新的 `State`，`Store` 中存储的状态发生更新。
6. 组件使用数据时需要通过，`getState()` 获取数据。

### redux 基本使用

#### redux demo

此例子中仅仅使用了 store 和 reducer，精简了 action，由组件直接创建 action 对象，发送给 store。

**项目结构**

- src
  - components
    - Count
      - index.jsx
  - redux
    - count_actions.js
    - count_reducer.js
    - constant.js
    - store.js
  - App.jsx
  - main.jsx

**store.js**

```js
// 引入createStore，专门用于创建redux中核心store对象
import { legacy_createStore as createStore } from 'redux'
// 引入为Count组件服务的reducer
import countReducer from './count_reducer'
// 向外暴露创建好的store
export default createStore(countReducer)
```

**constant.js**

```js
/* 
  该模块是用于定义action对象中type类型的常量值, 也就是将都用到的action名进行分离成单独文件
  这样方便维护，更改时只需要在共用的文件里更改，不用到每个文件中一一更改
*/
export const INCREMENT =  'increment'
export const DECREMENT =  'decrement'
```

**count_reducer.js**

```js
/* 
  1.该文件是用于创建一个为Count组件服务的reducer，reducer的本质是一个函数
  2.reducer函数会接到两个参数，分别为：之前的状态（preState），动作对象（action）
  3.reducer第一次调用时，是store自动触发的，这时进行初始化状态
*/

// 导入分离的 action type 类型 
import { INCREMENT, DECREMENT } from './constant'

const initState = 0
export default function countReducer(preState = initState, action) {
  const { type, data } = action
  switch (type) {
    case INCREMENT:
      return preState + data
    case DECREMENT:
      return preState - data
    default:
      return preState
  }
}
```

**count_actions.js**

```js
/* 
  该文件用于为Count组件生成action对象
*/

// 导入分离的 action type 类型 
import { INCREMENT, DECREMENT } from './constant'

// action creators 完整写法
export const createIncrementAction = function (data) {
  return { type: INCREMENT, data }
}
// action  creators 简写
export const createDecrementAction = data => ({ type: DECREMENT, data })

```

**Count/index.jsx**

```jsx
import React, { Component } from 'react'
// 引入store，通过store读取store中的方法与状态
import store from '../../redux/store'
// 引入action，通过action向store发送action对象
import { createIncrementAction, createDecrementAction } from '../../redux/count_action'

export default class Count extends Component {
  // 加
  increase = () => {
    const { value } = this.selectNumber
    // 将自己写的action对象传递给store，然后由store分发给count_reducer
    store.dispatch(createIncrementAction(value * 1))
  }
  // 减
  decrease = () => {
    const { value } = this.selectNumber
    store.dispatch(createDecrementAction(value * 1))
  }
  // 如果加后是偶数则执行
  increaseIfOdd = () => {
    const { value } = this.selectNumber
    // 通过store.getState方法取到redux管理的状态
    if ((store.getState() - value * 1) % 2 === 0) {
      this.increase()
    }
  }
  // 异步加
  decreaseAsyn = () => {
    const { value } = this.selectNumber
    setTimeout(() => {
      this.increase()
    }, 1000)
  }
  render() {
    return (
      <div id='count'>
        {/* 通过store中的getState()拿到Store中存储的状态 */}
        <h1>Count: {store.getState()}</h1>
        <select ref={c => (this.selectNumber = c)}>
          <option value='1'>1</option>
          <option value='2'>2</option>
          <option value='3'>3</option>
        </select>
        <button onClick={this.increase}>+</button>
        <button onClick={this.decrease}>-</button>
        <button onClick={this.increaseIfOdd}>如果加后是偶数则加</button>
        <button onClick={this.decreaseAsyn}>异步+</button>
      </div>
    )
  }
}
```

#### 对于异步 action 的处理

1. 可以在组件层面，异步调用 action creators，由 action creators 立即发送 action 对象给 store。
2. 可以在 action creators 层面，由组件立即调用 action creators，由 action creators 异步发送 action 对象给 store。

使用第二种异步处理方法时，需要在 redux 中使用异步 action 处理中间件 `redux-thunk`。**使用了这个中间件后，允许 dispatch 一个函数，这个函数将会被异步调用。**

在 redux 中使用中间件需要用到 `applyMiddleware`。

**安装**

```shell
yarn add redux-thunk
```

**使用**

**store.js**

```js
// 引入createStore，专门用于创建redux中核心store对象
// 引入applyMiddleware引出使用中间件的方法
import { legacy_createStore as createStore, applyMiddleware } from 'redux'
// 引入为Count组件服务的reducer
import countReducer from './count_reducer'
// 引入异步action中间件
import thunk from 'redux-thunk'

export default createStore(countReducer, applyMiddleware(thunk))
```

**count_action.js**

```js
/* 
  该文件用于为Count组件生成action对象
*/
import { INCREMENT, DECREMENT } from './constant'
// 由于异步action高级函数中可以有dispatch，所以可以省略引入
// import store from './store'

export const createIncrementAction = data => ({ type: INCREMENT, data })
export const createDecrementAction = data => ({ type: DECREMENT, data })

export const createIncrementAsyncAction = data => {
  return (dispatch) => {
    setTimeout(() => {
      dispatch(createIncrementAction(data))
    }, 1000)
  }
}
```

- 异步 action 需要返回一个函数，在该函数中进行相应的异步操作，并在其中将 action 对象通过 dispatch 方法发送给 store。
- 我们在其中不需要引入 store 来使用其中的 dispatch，redux 会为我们自动传入 dispatch，我们只需要在返回的函数参数中接收即可。
- 使用了 redux-thunk 之后，在 dispatch() 中可以加入一个函数，如果传入的是一个函数，这个函数会被异步执行。由于 class 中局部开启严格模式，所以函数的 this 为 undefined，**因此不能以返回值的形式返回 action 对象**。
- 需要在函数中继续 dispatch 一个 action 对象。

#### redux 中状态的处理与获取

在组件中通过 `store.getState()` 来获取 redux 中的状态。

**单个 reducer 数据的获取**

当仅仅有一个 reducer 时，`store.getState()` 就直接时获取这个 reducer 初始化给 store 的状态。

如果存储的是简单数据，比如存储一个数字 0 可以直接通过 store.getState() 获取。

```js
store.getState()
```

如果存储的是复杂数据，比如对象`{name:'brokyz'}`，需要这样获取。

```js
store.getState().name
```

**多个 reducer 数据的获取**

多个 reducer 数据就需要用到 `redux` 中的 `combineReducers` 方法。这个方法用于处理多个 reducer，可以将多个 reducer 的状态命名后合并到一起。

**store.js**

```js
// 引入createStore，专门用于创建redux中核心store对象
// 引入applyMiddleware引出使用中间件的方法
import {
  legacy_createStore as createStore,
  applyMiddleware,
  combineReducers,
} from 'redux'
// 引入为Count组件服务的reducer
import countReducer from './count_reducer'
import personReducer from './person_reducer'
// 引入异步action中间件
import thunk from 'redux-thunk'

const allReducer = combineReducers({
  count: countReducer,
  person: personReducer,
})

export default createStore(allReducer, applyMiddleware(thunk))
```

**读取多个合并 reducer 的状态**

以上述合并的 reducer 为例：

```js
// 读取 count_reducer 的状态
store.getState().count
// 读取 person_reducer 的状态
store.getState().person
```

**subscribe 触发页面更新**

redux 中状态发生改变时，并不会自动触发页面的更新。

这时我们就需要 `subscribe` 方法，此方法会监测 redux 的状态，状态一旦更新其中的异步函数就会被调用。

所以我们在 main.jsx 入口文件中使用，一旦状态改变就调用 render 渲染页面。

```js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
import store from './redux/store'

ReactDOM.render(<App></App>, document.getElementById('root'))
// subscribe为检测redux状态改变时触发，这里触发页面自动更新
// 为了避免在每个组件的钩子中触发页面更新，所以写在最上层结构中
// 存在diff算法，并不会有效率问题，只会更新改变的内容
store.subscribe(() => {
  ReactDOM.render(<App></App>, document.getElementById('root'))
})
```

### redux 开发者工具

**下载**

redux 开发者工具是浏览器的一个插件，其允许我们对 redux 中存储的状态进行一些查看和操作。

如果能够科学上网可以直接在 google 应用上单添加插件 Redux DevTools。

如果无法访问 google 应用商店可以前往 [Extfans](https://www.extfans.com/) 进行下载 Redux DevTools。

**使用**

在我们项目中安装 redux-devtools-extension。

```shell
yarn add redux-devtools-extension
```

在 store 中引入并使用 redux-devtools-extension。

```js
import {
  legacy_createStore as createStore,
  applyMiddleware,
  combineReducers,
} from 'redux'
import countReducer from './reducers/count'
import personReducer from './reducers/person'
import thunk from 'redux-thunk'
// 引入 redux-devtools-extension
import { composeWithDevTools } from 'redux-devtools-extension'

const allReducer = combineReducers({
  he: countReducer,
  rens: personReducer,
})
// 如果 createStore 中有第二个参数，那么将其写在 composeWithDevTools() 中
// 如果没有第二个参数，那么直接在第二个参数写 composeWithDevTools()
export default createStore(
  allReducer,
  composeWithDevTools(applyMiddleware(thunk))
)
```

### reducer 更新页面的机制

**reducer 方法必须是一个纯函数**：

- 只要是同样的输入，必须得到同样的输出。

  - 不得改写参数数据。

  - 不会产生任何副作用，如网络请求、输入和输出设备。

  - 不能调用 Date.now() 或者 Math.random() 等不纯的方法。

**更新机制**

- **在 reducer 方法中，如果传入的之前的状态（preState）和返回的状态内存地址相同，则不会触发页面更新。**

```js
// 以下方法不会触发页面更新
const initState = [{ id: '001', name: 'tom' }]
export default function personReducer(preState = initState, action) {
  const { type, data } = action
  switch (type) {
    case 'add_person':
      preState.pop({ id: '002', name: 'brokyz' })
      return preState
  }
}
```

**注意：改变数据时尽量使用`[...arr]、{...obj, id:'001'}`的形式**

## react-redux

### 为什么使用 react-redux

对于 `redux` 本身，其并不是 `react` 专属的库，它是一个单独的用于管理状态的库，它可以使用在任何地方，如 `react`、`angular`、`vue` 等，只不过它通常会被我们与 `react` 一起进行使用。

为了让 `redux` 更好的为 `react` 进行服务，`react` 官方为我们提供了 `react-redux`，它是一个 UI 绑定库，可以将 `redux` 与 `react` 进行绑定。而不是单独的在组件中直接调用 `store` 与 UI 进行存储交互。

 `react-redux` 必须要配合 `redux` 来进行使用，`react-redux` 无法单独进行使用。可以理解为 `react-redux` 是 `react` 中使用 `redux` 的一个辅助工具。它能帮助我们更好的使用 `redux` 为 `react` 进行服务。

### react-redux 原理

`react-redux` 为我们提供了一个**容器组件**和 **UI 组件**的概念，我们将使用 `react-redux` 之前进行页面渲染与交互逻辑的组件叫做 **UI 组件**；在 UI 组件外，我们又包裹了一层组件，我们将其看作**容器组件**。容器组件用于与 `redux` 进行交互可以使用 `redux` 中的 api，其交互的方法将会与 `props` 的形式与 UI 组件进行交互。

![](https://s1.ax1x.com/2022/10/29/x5z9YR.png)

### react-redux 快速使用

在配合 `redux` 使用时，`react-redux` 的两个核心功能就是 `connect`	、`Provider`。

#### 安装 redux 和 react-redux

将 redux 和 react-redux 安装到项目中：

```shell
npm install redux react-redux
```

#### 为 react 组件提供 store

为了避免我们为需要 store 的组件一一引入，我们可以使用 react-redux 为我们提供的 Provider 为需要的组件提供 store，一劳永逸。Provider 需要在入口文件处，以 `<Provider>` 的形式包裹在 `<App/>` 的外侧。

**src/main.jsx**

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import store from './redux/store'
import { Provider } from 'react-redux'

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <Provider store={store}>
    <App />
  </Provider>
)
```

#### 为 UI 组件创建容器组件

为了完成 `react-redux` 为我们提供的结构，我们需要使用 `react-redux` 为我们提供的 `connect` 创建一个容器组件并与对应的 UI 组件进行绑定。

在容器组件中我们需要定义 `mapStateToProps(state)`、`mapDispatchToProps(dispatch)` 两个函数（名字随意）。

在 `mapStateToProps(state)` 中，我们接收 `state` 参数就是 `redux` 中存储的状态。我们将需要的状态以对象的形式返回，`redux` 会将我们返回的 `key-value` 以 `props` 的形式传递给 UI 组件。

在 `mapDispatchToProps(dispatch)` 中，我们接收的 `dispatch` 参数就是 `store` 的 `dispatch` 方法。我们返回一个对象，在对象中将我们需要的操作封装成一个对象中操作状态的函数，在函数中将 `action` 对象 `dispatch` 给 `store`。而对象中我们封装的方法的 key-value 将以 props 的形式传递给 UI 组件。

所以在 UI 组件中，要想完成与 `redux` 的交互，我们只需要使用 `props` 中容器组件为我们二次封装的方法和状态即可。因此我们了解到，UI 组件并没有真正和 `redux` 进行交互，是容器组件与 `rerux` 直接进行交互。

**容器组件与 UI 组件分离的写法**

分离的写法需要我们在容器组件内引入需要绑定的 UI 组件

**src/container/Count.jsx**

```jsx
// 引入 Count 的 UI 组件
import CountUI from '../../components/Count'
// 引入 connect 用于连接 UI 组件与 redux
import { connect } from 'react-redux'
import {
  createIncrementAction,
  createDecrementAction,
  createIncrementAsyncAction,
} from '../../redux/count_action'

function mapStateToProps(state) {
  return { count: state }
}

function mapDispatchToProps(dispatch) {
  return {
    increament: number => dispatch(createIncrementAction(number)),
    decrement: number => dispatch(createDecrementAction(number)),
    increamentAsync: (number, time) => dispatch(createIncrementAsyncAction(number, time)),
  }
}
// 创建并暴露一个 Count 的容器组件
export default connect(mapStateToProps, mapDispatchToProps)(CountUI)
```

**当我们使用组件时，导入和使用的是容器组件，而不是 UI 组件。**

**src/App.jsx**

```jsx
import React, { Component } from 'react'
// 引入的是容器组件
import Count from './containers/Count'

export default class App extends Component {
  render() {
    return (
      <div>
        <Count></Count>
      </div>
    )
  }
}
```

**容器组件与 UI 组件合并的写法**

这种写法将容器组件与 UI 组件写在同一个文件中，这样也就不需要我们去引入 UI 组件进行绑定了。

**src/containers/Count.jsx**

```jsx
import React, { Component } from 'react'
import './index.css'
import { connect } from 'react-redux'
import {
  createIncrementAction,
  createDecrementAction,
  createIncrementAsyncAction,
} from '../../redux/count_action'

// 定义UI组件
class Count extends Component {...}

function mapStateToProps(state) {
  return { count: state }
}

function mapDispatchToProps(dispatch) {
  return {
    increament: number => dispatch(createIncrementAction(number)),
    decrement: number => dispatch(createDecrementAction(number)),
    increamentAsync: (number, time) => dispatch(createIncrementAsyncAction(number, time)),
  }
}
// 创建并暴露一个 Count 的容器组件
export default connect(mapStateToProps, mapDispatchToProps)(Count)
```

合并时，只暴露容器组件，不暴露 UI 组件。

**定义容器组件的简写方法**

**src/containers/Count.jsx**

```jsx
import React, { Component } from 'react'
import './index.css'
import { connect } from 'react-redux'
import {
  createIncrementAction,
  createDecrementAction,
  createIncrementAsyncAction,
} from '../../redux/count_action'

// 定义 UI 组件
class Count extends Component {...}

// 定义容器组件 
export default connect(
  // mapStateToProps
  state => ({
    count: state,
  }),
  // mapDispatchToProps
  {
    increament: createIncrementAction,
    decreament: createDecrementAction,
    increamentAsync: createIncrementAsyncAction,
  }
)(Count)
```

#### 推荐的项目结构

```
|--src
|	|--containers
|	|	|--Count
|	|		|--index.jsx
|	|		|--index.css
|	|--redux
|		|--actions
|		|	|--count.js
|		|--reducers
|		|	|--count.js
|		|--constant.js
|		|--store.js
|--App.jsx
|--main.jsx
```

