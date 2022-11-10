## React Router 5

React Router 有三个不同的包，分别为：

1. react-router：路由的核心库，提供了很多的：组件、钩子。
2. **react-router-dom：包含 react-router 所有内容，并添加了一些专门用于 DOM 的组件，例如`<BrowserRouter>`等。**
3. react-router-native：包括 react-router 所有内容，并添加了一些专门用于 ReactNative 的 API，例如`<NativeRouter>`等。

在此我们只介绍 react-router-dom 的使用。

### 快速使用 React Router

**安装**

```shell
# npm
npm install react-router-dom@5
# yarn
yarn add react-router-dom@5
```

**基本使用**

在 react-router-dom 中，最最基础的两个功能就是 Link 和 Route。

**使用 Link 组件来编写路由链接**，类似于 a 标签。

**使用 Route 组件来注册路由**，也就是链接所需要对应展示的组件。

```jsx
import React, { Component } from 'react'
// 按需导入需要的 react-router-dom 功能
import { BrowserRouter, Link, Route } from 'react-router-dom'
import About from './pages/About.jsx'
import Home from './pages/Home/index.jsx'

export default class App extends Component {
  render() {
    return (
      <div>
        <BrowserRouter>
          {/* Link用于链接跳转，类似于a标签 */}
          <Link to='/Home'>Home</Link>
          <Link to='/About'>About</Link>
            
          {/* Route用于将对应链接路由到对应组件 */}
          <Route path='/Home' component={Home}></Route>
          <Route path='/About' component={About}></Route>
        </BrowserRouter>
      </div>
    )
  }
}
```

我们在使用 react-router 的时候，需要**按需引入**我们所用到的功能。

**路由链接和注册路由必须写在同一个`<BrowserRouter>`中**，因此我们可以将这个标签向上提到入口文件中，使其包裹整个`<App/>`组件。这样一来我们写的所有链接和路由就全都包裹在同一个`<BrowserRouter>`中了。

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
)
```

这样写了之后，我们就不需要在其他组件中引入`<BrowserRouter>`了。

至此，一个最简单的路由 demo 也就完成了。

### 路由组件与一般组件

一般组件就是我们在 react 中引入组件后，直接以标签的形式使用的组件，如`<Demo />`。

路由组件则是由 Route 组件进行注册路由的形式使用的组件，如`<Route path="/demo" component={Demo}/>`。

**在使用时我们一般将路由组件和一般组件分开书写，将路由组件写在 pages 文件夹中，将一般组件写在 components 文件夹中。**

路由组件的 props 身上会被 react-router 添加一些额外的路由属性，如 history、location、match。

其大概格式如下。

```js
props: 
	history: {length: 2, action: 'PUSH', location: {…}, createHref: ƒ, push: ƒ, …}
	location: {pathname: '/Home', search: '', hash: '', state: undefined, key: 'vpjxmx'}
	match: {path: '/Home', url: '/Home', isExact: true, params: {…}}
```

**其中在 history 身上也有 location 属性，因为 props 中的 location 就是来自于 history 中的 location，二者等价。**

### NavLink

NavLink 是增强版的 Link，默认为激活的链接添加 active 类，我们也可以通过 activeClassName 去指定其它激活时的类名。

```jsx
import React, { Component } from 'react'
// 按需导入需要的 react-router-dom 功能
import { NavLink, Route } from 'react-router-dom'
import About from './pages/About.jsx'
import Home from './pages/Home/index.jsx'

export default class App extends Component {
  render() {
    return (
      <div>
        {/* 声明链接跳转 */}
        <NavLink to='/Home'>Home</NavLink>
        <NavLink activeClassName="show" to='/About'>About</NavLink>
        {/* 注册路由 */}
        <Route path='/Home' component={Home}></Route>
        <Route path='/About' component={About}></Route>
      </div>
    )
  }
}
```

### 封装 NavLink

我们封装一个我们自己的 NavLink 组件，让其使用的时候默认激活类名为 show，简化我们的使用。

```jsx
import React, { Component } from 'react'
import { NavLink } from 'react-router-dom'

export default class MyNavLink extends Component {
  render() {
    return <NavLink activeClassName='show' {...this.props}></NavLink>
  }
}
```

其中使用的时候传入的标签内容是 children 属性，如`<MyNavLink>title</MyNavLink>`，就会给组件 props 中传入的 children 属性就是 title。

所以我们为了简化写法，将传入的 props 直接解构即可。

### Switch

我们路由链接查找时，会一一对我们注册的路由地址进行匹配，直至所有匹配完成，这样注册路由特别多时就会存在很大的效率问题。

我们使用 Switch 组件包裹 Route，这样我们在路由链接查找时，有一个匹配成功时就不会继续向下进行匹配了，提高了效率。

```jsx
import { Switch, Route } from 'react-router-dom'

<Switch>
  <Route path='/Home' component={Home}></Route>
  <Route path='/About' component={About}></Route>
</Switch>
```

### 路由的模糊匹配和严格匹配

路由默认开启模糊匹配，如果需要严格匹配需要手动加 exact 属性。

模糊匹配就是当链接匹配不上时，会向前查找上一级 params 参数，直至匹配成功，则展示匹配成功的。如果我们访问链接`/Home/a/b`其中，/Home/a/b 和 /Home/a 都无法匹配成功，但是 /Home 匹配成功，就会显示 /Home 的路由结果。

严格匹配就是，只匹配当前链接，不向上查找。如果我们访问链接`/Home/a/b`，就只去匹配 /Home/a/b，匹配不成功就没有显示结果。

当使用多级路由时，无法使用严格匹配。**只有在多级路由的最后一级中，我们可以使用严格匹配。**

```jsx
<Route path='/Home' component={Home}></Route>
<Route exact path='/About' component={About}></Route>
```

### Redirect

Redirect 组件会将我们的自动跳转到我们指定的链接。

```jsx
import { Route, Redirect } from 'react-router-dom'

<Route path='/Home' component={Home}></Route>
<Route path='/About' component={About}></Route>
<Redirect to='/Home' />
```

### 嵌套路由

多级路由声明跳转链接和注册路由时都要把路径写全。

```jsx
import React, { Component } from 'react'
import { NavLink, Route } from 'react-router-dom'
import Messages from './Messages'
import News from './News'

export default class Home extends Component {
  render() {
    console.log(this)
    return (
      <div>
        <h2>home</h2>
        {/* 在路由组件中声明二级路由 */}
        <NavLink to='/Home/News'>News</NavLink>
        <NavLink to='/Home/Messages'>Messages</NavLink>
        <Route path='/Home/News' component={News} />
        <Route path='/Home/Messages' component={Messages} />
      </div>
    )
  }
}
```

### 向路由组件传递参数

#### 传递 params 参数

使用 params 向路由组件传递参数时，需要在注册路由时声明要传入的 params 的 key。

在路由跳转时，在对应的位置的参数就是我们传入的 key 所对应为 value。

```jsx
<NavLink to='/Home/News/001/title001'>News</NavLink>
<Route path='/Home/News/:id/:title' component={News} />
```

在路由组件的 `this.props.match.params` 为接收到的对象形式的 params。`{id: 001, title: title001}`

#### 传递 search 参数

 向路由组件传递 search 参数时，不需要再注册路由时进行对应 key 的声明。

```jsx
<NavLink to='/Home/News/?id=001&title=title001'>News</NavLink>
<Route path='/Home/News' component={News} />
```

在路由组件的 `this.props.location.search` 为接收到的字符串形式的 search。`‘?id=001&title=title001’`。

我们需要使用 querystringify 库来解析 search 字符串为对象的形式。

```shell
# 使用 npm 安装
npm i querystringify
# 使用 yarn 安装
yarn add querystringify
```

```jsx
import React, { Component } from 'react'
import qs from 'querystringify'

export default class News extends Component {
  render() {
    console.log(this.props)
    // 解析 search 为对象
    const search = qs.parse(this.props.location.search)
    return (
      <div>
        <h3>News</h3>
        <p>
          id: {search.id}---title: {search.title}
        </p>
      </div>
    )
  }
}
```

#### 传递 state 参数

向路由组件传递 state 参数时，不需要再注册路由时进行对应 key 的声明。

```jsx
<NavLink
  to={{
    pathname: '/Home/News',
    state: { id: '001', title: 'title001' },
  }}
>
  News
</NavLink>
<Route path='/Home/News' component={News} />
```

在路由组件的 `this.props.location.state` 为接收到的对象形式的 state。`{id: 001, title: title001}`

### 路由跳转的两种模式

路由有两种跳转模式 push 和 replace。

push 为默认的压栈模式，跳转时保留历史记录。

replace 为替换模式，跳转时替换当前的记录。开启 replace 只需要在声明链接时加上 replace 属性即可。

```jsx
<NavLink replace to='/Home/News/?id=001&title=title001'>News</NavLink>
```

### 编程式路由导航

编程式路由导航就是通过 JS 代码的形式来完成路由跳转。

编程式路由导航的 api 存在于 history 对象身上。

**push 与 replace 跳转**

```jsx
this.props.history.push('/Home/News', { id: '001', title: 'title001' })

this.props.history.replace('/Home/News', { id: '001', title: 'title001' })
```

**页面前进和后退**

```jsx
// 后退
this.props.history.goBack()
// 前进
this.props.history.goForward()
// 指定前进后退数值，以下为页面后退两步
this.props.history.go(-2)
```

### withRouter

有时我们需要在一般组件中使用路由的 api，这时我们就需要 withRouter 来对我们的一般组件进行包装。暴露包装后的结果就是添加了路由对应 api 的一般组件。

```jsx
import React, { Component } from 'react'
import { withRouter } from 'react-router-dom'

class Header extends Component {
  render() {
    console.log(this.props)
    return (
      <div>
 		 Header
      </div>
    )
  }
}
// 一般组件经过 withRouter 加工后，就有相应路由组件 api 的功能了
// 返回一个加上路由 api 的新组件
export default withRouter(Header)
```

### BrowserRouter 和 HashRouter

1. 底层原理的不同
   - BrowserRouter 使用的式 H5 的 History API 来实现的路由，不兼容 ie9 及以下版本。
   - HashRouter 使用的是 URL 的哈希值来实现的路由。
2. URL 的表现形式不用
   - BrowserRouter 的路径中没有 #，如：localhost:3000/demo/test
   - HashRouter 的路径包含 #，如：localhost:3000/#/demo/test
3. 刷新后对路由 state 参数的影响
   - BrowserRouter 没有任何影响，因为 state 保存在 history 对象中。
   - HashRouter 刷新后会导致 state 参数丢失。
   - 如果刷新后都会丢失页面，可能是 Redirect 的问题，因为模糊匹配匹配成功后，会继续匹配这时可能触发 Redirect 到别的页面，需要用 Switch 去包裹 Redirect。
4. HashRouter 可以用于解决一些路径错误相关的问题。

## React Router 6

React Router 6 是目前的最先版本，默认安装时安装的就是这个版本。

在此我们在 React Router 5 的基础上来介绍 react-router-dom 的变化与使用。

### 一级路由

在 React Router 6 中，Switch 被换成了 Routes，而且注册路由必须被 Routes 包裹。

在使用 Route 时，component 参数被换成了 element，而且必须以标签的形式赋值。

```jsx
import { NavLink, Routes, Route } from 'react-router-dom'

render() {
  return (
    <div>
      {/* 声明链接跳转 */}
      <NavLink to='/Home'>Home</NavLink>
      <NavLink to='/About'>About</NavLink>
      {/* 注册路由 */}
      <Routes>
        <Route path='/Home' element={<Home />} />
        <Route path='/About' element={<About />} />
      </Routes>
    </div>
  )
}
```

### 重定向

