## 组件的生命周期

> 在 react 中组件从创建到销毁的过程中，有一系列的特殊阶段，react 为这些特殊阶段提供了生命周期函数，方便我们在这些阶段进行相应的操作。

生命周期函数有很多叫法，如生命周期回调函数，生命周期钩子函数，生命周期函数，生命周期钩子等，它们都是同一个意思。

在组件中，生命周期函数由组件实例对象调用，所以 `this` 就是指向组件实例对象的，不需要使用箭头函数赋值的方法声明生命周期函数。

### 生命周期函数（旧版）

以下是生命周期函数旧版的示意图，适用于 react17 之前的版本。（不适用于react17）

![](https://pic1.imgdb.cn/item/63550d6716f2c2beb13ee139.jpg)

初始化阶段：

	1. `constructor()`
	1. `componentWillMount()`
	1. `render()`
	1. `componentDidMount()`

更新阶段：

- 当前组件数据更新时 `setState()`

  1. `shouldComponentUpdate()`

  2. `componentWillUpdate()`
  3. `render()`
  4. `componentDidUpdate()`

- 当前组件强制更新时 `forceUpdate()`

  1. `componentWillUpdate()`

  2. `render()`
  3. `componentDidUpdate()`

- 父组件发生页面更新时（render调用时，除了第一次调用）

  1. `componentWillReceiveProps()`：无论父组件有没有传递 props 父组件页面发生更新一定会触发。
  2. `shouldComponentUpdate()`：返回值为 `true` 继续下一个阶段，返回值为 `false` 则在此阶段停止，不会继续下一个阶段。

  2. `componentWillUpdate()`
  3. `render()`
  4. `componentDidUpdate()`

卸载阶段：由 `ReactDOM.unmountComponentAtNode()` 触发

1. `componentWillUnmount()`

**其中两个最常用的两个钩子为**

- `componentDidMount()`：一般在这个钩子中**做一些初始化的事情**，如：开启定时器、发送网络请求、订阅消息。
- `componentWillUnmount()`：一般在这个钩子中**做一些收尾的事情**。如：关闭计时器、取消订阅消息。

测试demo：

```js
class Count extends React.Component {
  constructor(props) {
    console.log("Count-constructor")
    super(props)
    this.state = {
      count: 0,
    }
  }
  
  // 组件挂载后触发
  componentDidMount() {
    console.log("Count-componentDidMount")
  }
  // 组件被卸载前触发
  componentWillUnmount() {
    console.log("Count-componentWillUnmount")
  }
  // 组件状态更新前触发
  shouldComponentUpdate() {
    console.log("Count-shouldComponentUpdate")
    return true
  }
  // 组件状态更新前触发
  componentWillUpdate() {
    console.log("Count-componentWillUpdate")
  }
  // 无论父组件有没有传递props父组件页面发生更新一定会触发
  componentWillReceiveProps() {
    console.log("Count-componentWillReceiveProps")
  }
  render() {
    console.log("Count-render")
    return (
      <div>
        <h2>当前和为: {this.state.count}</h2>
        <button
          onClick={() => {
            this.setState({ count: ++this.state.count })
          }}
        >
          点我+1
        </button>
        <br />
        <button
          onClick={() => {
            ReactDOM.unmountComponentAtNode(document.getElementById("test"))
          }}
        >
          卸载组件
        </button>
        <br />
        <button
          onClick={() => {
            this.forceUpdate()
          }}
        >
          强制更新
        </button>
      </div>
    )
  }
}

class Father extends React.Component {
  state = {
    FatherCount: 0,
  }
  render() {
    return (
      <div>
        <h1>父组件传递给子组件的props: {this.state.FatherCount}</h1>
        <button
          onClick={() => {
            this.setState({ FatherCount: ++this.state.FatherCount })
          }}
        >
          FatherCount++
        </button>
        <Count />
      </div>
    )
  }
}
// 渲染组件
ReactDOM.render(<Father />, document.getElementById("test"))
```

### 生命周期函数（新版）

以下是生命周期函数新版的示意图，适用于 react17 之后的版本。（适用于react17）

![](https://pic1.imgdb.cn/item/635512f116f2c2beb146be2a.jpg)

**生命周期函数新旧版的区别**

由于后续版本中 React 将支持异步加载，而 `componentWillMount()`、`componentWillReceiveProps`、`componentWillUpdate()` 这三个钩子存在一些问题，所以不在建议使用。如果必须进行使用，需要在前面加上`UNSAFE_`前缀。但是无法与新增的两个钩子同时使用。

在新版本中新增了两个钩子和改变了一个钩子：

- `getDerivedStateFromProps(props, state)`：参数`props`是传入组件的`props`，参数`state`是组件的状态。返回值是将返回的值添加到组件的`state`中。此方法给类本身使用，而不是由实例对象调用，所以需要使用`static`声明
- `getSnapshotBeforeUpdate(preProps, preState)`：此钩子在触发页面更新阶段，触发 `render()` 之后，但是页面更新之前触发。参数`preProps`是页面更新之前传入组件的`props`，参数`preState`是页面更新之前的`state`。返回值将作为参数传递给`componentDidUpdate()`。

- `componentDidUpdate(preProps, preState, value)`：前两个参数和上面一样。参数`value`为接收到的`getSnapshotBeforeUpdate()`的返回的值。

React17 相关资源

```html
<script src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
<script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
```

demo:

```js
class Count extends React.Component {
  constructor(props) {
    console.log("Count-constructor")
    super(props)
    this.state = {
      count: 0,
    }
  }
  // UNSAFE_componentWillMount() {
  //   console.log("test")
  // }
  static getDerivedStateFromProps(props, state) {
    console.log("Count-getDeriveStateFromProps")
    console.log("props: ", props)
    console.log("state: ", state)
    console.log("-----------")
    return props
  }
  getSnapshotBeforeUpdate(preProps, preState) {
    console.log("Count-getSnapshotBeforeUpdate")
    console.log("preProps: ", preProps)
    console.log("preState: ", preState)
    console.log("-----------")
    return "这是从getSnapshotBeforeUpdate传递来的数据"
  }
  componentDidUpdate(preProps, preState, value) {
    console.log("Count-componentDidUpdate")
    console.log("preProps: ", preProps)
    console.log("preState: ", preState)
    console.log(value)
    console.log("-----------")
  }
  // 组件挂载后触发
  componentDidMount() {
    console.log("Count-componentDidMount")
    console.log(this)
    console.log("-----------")
  }
  // 组件状态更新前触发
  shouldComponentUpdate() {
    console.log("Count-shouldComponentUpdate")
    console.log("-----------")
    return true
  }
  render() {
    console.log("Count-render")
    console.log("-----------")
    return (
      <div>
        <h2>当前和为: {this.state.count}</h2>
        <button
          onClick={() => {
            this.setState({ count: this.state.count + 1 })
          }}
        >
          点我+1
        </button>
        <br />
        <button
          onClick={() => {
            ReactDOM.unmountComponentAtNode(document.getElementById("test"))
          }}
        >
          卸载组件
        </button>
        <br />
        <button
          onClick={() => {
            this.forceUpdate()
          }}
        >
          强制更新
        </button>
      </div>
    )
  }
}

class Father extends React.Component {
  state = {
    FatherCount: 0,
  }
  render() {
    return (
      <div>
        <h1>父组件传递给子组件的props: {this.state.FatherCount}</h1>
        <button
          onClick={() => {
            this.setState({ FatherCount: ++this.state.FatherCount })
          }}
        >
          FatherCount++
        </button>
        <Count testProps="testProps" />
      </div>
    )
  }
}
// 渲染组件
ReactDOM.render(<Father />, document.getElementById("test"))
```

