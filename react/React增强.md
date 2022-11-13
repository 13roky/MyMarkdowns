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

### 组件渲染优化

#### 对于类式组件

到目前位置我们使用的组件有两个弊端：

1. 当我们使用 `this.setState({})` 时，只调用但不修改数据，当前页面的 render 也会触发，影响效率。
2. 当我们父组件的 render 触发时，即使子组件是一个静态组件，子组件的 render 也会触发，尽管子组件并没有从父组件接收改变的数据，这种情况的子组件不需要 render。

我们可以采用生命周期钩子 `shouldComponentUpdate(newProps, newState)` 来解决这两个问题：

- 这个钩子会在 render 之前调用，如果返回值为 true 则会继续向下进行 render，如果返回为 false 则流程到此为止，不会 render。
- 在这个函数中，参数为新的 props 和 state，而 this.props 和 this.state 是旧的，因为此时组件还没有发生更新。
- 我们就可以通过新旧 props 和 state 是否相等，来控制是否进行 render，从而就解决了以上两个弊端。

当然我们并不需要每个组件去进行手写，React 为我们提供了 PureComponent，我们继承时只需要继承这个就行了。

```jsx
import React, { PureComponent } from 'react'

export default class Father extends PureComponent {
  state = {
    count: 0,
  }
  render() {
    console.log('father---render')
    return (
      <div>
        <button
          onClick={() => {
            this.setState({ })
          }}>
          更新空值
        </button>
      </div>
    )
  }
}
```

#### 对于函数式组件

对于函数式组件，也存在于类式组件相同的问题。

1. 当我们父组件进行 render 渲染时，不管其子组件是否依赖父组件的数据而进行呈现，子组件都会也执行 render。

上面这个问题我们可以使用 `React.memo()` 来解决。

1. 向 memo 处理后的函数式组件中传入没有改变的 props 时，子组件并不会进行 render 渲染。
2. 但是这时如果我们的 props 传入一个函数，不管函数是否改变，子组件都会进行 render 渲染。

为了解决传入 props 函数后，子组件依然会跟着 render 的问题，我们采用 useMemo() 或 useCallback() 来解决。

```jsx
import React from 'react'

export default function Count() {
  console.log('father render')

  React.useEffect(() => {
    console.log('father update')
  })
    
  const [count, setCount] = React.useState(0)
  
  const fn = React.useMemo(() => {
    return () => {
      console.log('test usememo')
    }
  }, [])
  
  return (
    <div>
      <h2>count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <hr />
      <Child count={fn} />
    </div>
  )
}

function Child(props) {
  console.log('child---render')

  React.useEffect(() => {
    console.log('child---update')
  })
  return (
    <div>
      <h3>Child</h3>
      <button onClick={props.count}>btn</button>
    </div>
  )
}

Child = React.memo(Child)
```

其中 useMemo 和 useCallback 功能类似。

```jsx
const fn = React.useMemo(() => {
  return () => {
    console.log('test usememo')
  }
}, [])

const fn2 = React.useCallback(() => {
  console.log('test useCallback')
}, [])
```

上面的两个 fn 和 fn2 是等价的，所以 useCallback 就是专门封装了 useMemo 返回值为函数的情况。

后面的空数组表示什么都不监听（类似于 useEffect），也就是说 fn 一直都不会变化。这样以 props 的形式传入，就不会引起 memo 处理的子组件触发 render 了。

### 错误边界

有时我们的子组件 render 时会发生一些错误，这时整个页面就无法运行了。我们希望当某个组件出现问题时，不影响整个页面的展示。这时我们就需要用到错误边界技术。

两个用到的核心生命周期钩子:

`static getDeivedStateFromError(error)`：

- 当组件的子组件发生错误时触发。

- 返回一个新的 state。

- 所以我们需要定义一个 state 用于标识其子组件是否发生错误。通过这个标识我们来判断是否渲染这个组件。这样错误组件就不会被渲染到页面，也就不会影响整个页面因为这个错误而无法显示了。

`componentDidCatch(error, info)`：

- 组件发生错误时调用，一般用于统计错误信息，发送给后台。

```jsx
import React, { Component } from 'react'

export default class index extends Component {
  state = {
    hasError: '',
  }

  static getDerivedStateFromError(error) {
    console.log('@@@', error)
    return { hasError: true }
  }

  componentDidCatch(error, info) {
    console.log(error, info)
  }
  
  render() {
    return (
      <div>
        <h1>father</h1>
        {this.state.hasError ? <h1>当前网络不稳定</h1> : <Child />}
      </div>
    )
  }
}

class Child extends Component {
  render() {
    return <h2>{child}</h2>
  }
}
```

### render props 插槽

render props 技术就是 vue 中的插槽技术。向组件内部动态传入。

#### children props

```jsx
import React, { Component } from 'react'

export default class index extends Component {
  render() {
    return (
      <div>
        <h1>father</h1>
        <A>
          <p>children props</p>
        </A>
      </div>
    )
  }
}

class A extends Component {
  render() {
    return (
      <>
        <h2>A</h2>
        {this.props.children}
      </>
    )
  }
}
```

**render props**

```jsx
import React, { Component } from 'react'

export default class index extends Component {
  render() {
    return (
      <div>
        <h1>father</h1>
        <A render={data => <p>render props: {data}</p>} />
      </div>
    )
  }
}

class A extends Component {
  state = {
    data: 'test',
  }
  render() {
    return (
      <>
        <h2>A</h2>
        {this.props.render(this.state.data)}
      </>
    )
  }
}
```

### context 传值

context 是一种组件间通信的方式，可以用来进行隔代组件传值。

首先要创建使用 React.createContext 创建一个 context 对象，要保证这个对象可以被使用者读取到。

然后使用 context 的 Provider 来存入值，并向后代提供数据。

```jsx
import React, { Component, useContext } from 'react'

// createContext 中可以传入一个默认值，当其 Provider 没有被使用时，其他组件读取时就是默认值。
export const msgContext = React.createContext('默认值')
// 设置在Devtool中显示的名字
msgContext.displayName = 'msgContext'

export default class father extends Component {
  state = {
    msg: '一条来自父组件的消息',
  }
  render() {
    return (
      <div>
        <h1>father</h1>
        <msgContext.Provider value={this.state.msg}>
          <Child />
        </msgContext.Provider>
      </div>
    )
  }
}
```

在类式组件中接收父辈的 context 使用 `static contextType = msgContext`。

读取 context 中的值使用 `this.context`

```jsx
import React, { Component, useContext } from 'react'
import {msgContext} from './father.jsx'

export default class Child extends Component {
  static contextType = msgContext
  render() {
    return (
      <>
        <h2>Child: {this.context}</h2>
        <Grand />
      </>
    )
  }
}
```

在函数式组件中读取 context 有两个方法。

第一种是使用 context 对象中的 Consumer 来完成。

```jsx
import React from 'react'
import {msgContext} from './father.jsx'

export default function Grand() {
  return (
    <>
      <msgContext.Consumer>
        {value => {
          return <h1>Grand: {value}</h1>
        }}
      </msgContext.Consumer>
    </>
  )
}
```

第二种是使用钩子 useContext 来读取。

```jsx
import React from 'react'
import {msgContext} from './father.jsx'

function Grand() {
  const value = React.useContext(msgContext)
  return (
    <>
      <h2>Grand: {value}</h2>
    </>
  )
}
```

### useMemo

useMemo 就是 Vue 中的计算属性。

```jsx
import React from 'react'

export default function index() {
  const [count, setCount] = React.useState(0)
  const computed = React.useMemo(() => `count 值加2 为${count + 2}`, [count])
  return (
    <div>
      <h1>count: {count}</h1>
      <button
        onClick={() => {
          setCount(count + 1)
        }}>
        +1
      </button>
      <h2>computed: {computed}</h2>
    </div>
  )
}
```

