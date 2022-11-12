## React 进阶

### setState 详解

**对象式 setState**：`setState(stateChange, [callback])`

1. stateChange 为状态改变对象(该对象可以体现出状态的更改)
2. callback 是可选的回调函数, 它在状态更新完毕、界面也更新后（ render 调用后）才被调用

**函数式 setState**：`setState(updater, [callback])`

1. updater 为返回 stateChange 对象的函数。
2. updater 可以接收到 state 和 props。
3. callback 是可选的回调函数，它在状态更新、界面也更新后（ render 调用后）才被调用。

**总结**：

1. 对象式的 setState 是函数式的 setState 的简写方式（语法糖）
2. 使用原则：
   1. 如果新状态不依赖于原状态 ===> 使用对象方式
   2. 如果新状态依赖于原状态 ===> 使用函数方式
   3. 如果需要在 setState() 执行后获取最新的状态数据，要在第二个 callback 函数中读取。

**Demo**：

```jsx
import React, { Component } from 'react'

export default class index extends Component {
  state = { count: 0 }

  add = () => {
    const { count } = this.state
    this.setState({ count: count + 1 }, () =>
      console.log('add Out2: ', this.state.count)
    )
    console.log('add Out1: ', this.state.count)
  }

  add2 = () => {
    this.setState(
      (state, props) => {
        return { count: state.count + 1 }
      },
      () => console.log('add2 Out', this.state.count)
    )
  }
    
  render() {
    return (
      <div>
        <h1>当前和为：{this.state.count}</h1>
        <button onClick={this.add2}>+1</button>
      </div>
    )
  }
}
```

### lazy 懒加载

当我们有些组件因为用户网速原因导致资源没有加载进来，内容没有呈现的时候，这时我们就可以使用懒加载组件。

我们使用 lazy 函数懒加载引入组件，只有组件被渲染时，才去导入。

组件加载进来的时候需要时间，这是我们可以使用 Suspense 来指定组件未加载进来时对应位置要显示的内容。

经常和路由组件进行搭配使用。

我们可以使用这个功能制作骨架屏。

```jsx
import React, { Component, lazy, Suspense } from 'react'
import { NavLink, Routes, Route } from 'react-router-dom'
// import Home from './Home'
// import About from './About'
const Home = lazy(() => import('./Home'))
const About = lazy(() => import('./About'))

export default class index extends Component {
  render() {
    return (
      <div>
        <h2>Lazy Demo</h2>
        <NavLink to='home'>Home</NavLink>
        <NavLink to='about'>About</NavLink>
        <Suspense fallback={<h1>Loading....</h1>}>
          <Routes>
            <Route path='home' element={<Home />} />
            <Route path='About' element={<About />} />
          </Routes>
        </Suspense>
      </div>
    )
  }
}
```

### Hooks

Hook 是 React 16.8.0 版本增加的新特性/新语法。可以让你在函数组件中使用 state 以及其他的 React 特性。

**三个常用的 Hook**：

1. State Hook: `React.useState()`
2. Effect Hook: `React.useEffect()`
3. Ref Hook: `React.useRef()`

#### state hook

state hook 可以让函数式组件使用和操作 state 状态。

`const [xxx, setXxx] = React.useState(initValue) `

- useState() 返回一个包含两个元素的数组，第1个为内部当前状态值，第2个为个更新当前状态值的函数。

- initValue 为初始化的状态值。

setXxx() 的两种使用方法：

1. `setXxx(newValue)`: 参数为非函数值，直接指定新的状态值。内部用其覆盖原来的状态值。
2. `setXxx(value => newValue)`: 参数为函数，接收原本的状态值，返回新的状态值，内部用其覆盖原来的状态值。

```jsx
import React from 'react'

export default function index() {
  const [count, setCount] = React.useState(0)
  return (
    <div>
      <h1>count: {count}</h1>
      <button
        onClick={() => {
          setCount(count => count + 1)
        }}>
        +1
      </button>
    </div>
  )
}
```

#### effect hook

在函数式组件中，我们无法使用类式组件中的生命周期钩子。但是 React 为我们提供了 effect hook，这个可以完成对 state 的监听，类似于 Vue 中的监听器 watch。恰好其能帮助我们完成类式组件中生命周期的功能。

使用例子：

```jsx
React.useEffect(() => {
  const timer = setInterval(() => {
    console.log('useEffect timer')
  }, 1000)
  return () => {
    clearInterval(timer)
  }
}, [state])
```

- 第二个参数数组，为监听的状态，当这个状态发生改变时就会触发当前 useEffect。
  - 如不写第二个参数，则为监听所有状态，相当于 componentDidMount 和 componentDidUpdate 的组合。
  - 如果第二个参数为空数组，则什么都不监听，相当于 componentDidMount。
  - 如果数组中携带状态，则会监听这个状态变化，相当于 componentDidMount 和 Vue 中的 watch 监听器。
- 第一个参数为触发时需要执行的逻辑函数，一般做有副作用的操作，返回一个函数，当组件被销毁时会执行返回的函数，用来做收尾性工作。

#### ref hook

当我们需要在函数式组件中拿到打了 ref 标签的元素时，就需要用到 useRef 了，这个和类式组件中的 createRef 相似。

```jsx
import React from 'react'

export default function index() {
  const myRef = React.useRef()

  return (
    <div>
      <input type='text' ref={myRef} />
      <button onClick={() => alert(myRef.current.value)}>show</button>
    </div>
  )
}
```

### Fragment

在 react 中组件必须包裹在一个标签中，一般我们会用`<div></div>` 进行包裹，但是这个 div 也会被渲染到页面上，增加了不必要的解构。相当于 vue 中的 template。

这时我们就可以使用 Fragment 标签或空标签 `<></>`，这个标签不会被渲染到页面。

```jsx
import React, {Fragment} from 'react'

export default function index() {
  return (
    <Fragment key='001'>
      <h1>fragment</h1>
      <h2>fragment</h2>
    </Fragment>
  )
}
```

Fragment 和 空标签的区别：

- 两个标签都不会被渲染到页面上。
- 当使用遍历渲染时，Fragment 可以携带 key 属性。也可以携带其他属性，但是由于不会被渲染到页面，除了遍历的使用场景和空标签没区别。
- 空标签不能携带属性，遍历时无法携带 key 属性。