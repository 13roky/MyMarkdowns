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

React Router 6 推荐我们使用函数式组件。

### 一级路由

在 React Router 6 中，Switch 被换成了 Routes，而且注册路由必须被 Routes 包裹。

在使用 Route 时，component 参数被换成了 element，而且必须以标签的形式赋值。

```jsx
import { NavLink, Routes, Route } from 'react-router-dom'


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

```

Route 不区分大小写，如果给了 caseSensitive 参数则区分路径的大小写。

### 重定向

React Router 6 中不在使用 Redirect 进行重定向，将使用 Navigate 组件进行重定向，当组件被渲染时，将会触发页面重定向。

```jsx
<Routes>
  <Route path='/Home' element={<Home />} />
  <Route path='/' element={<Navigate to='/Home' />} />
</Routes>
```

可以指定 replace 模式 `<Navigate to='/Home' replace />`

### NavLink

在 React Router 6 中的 NavLink 功能没有改变，但是对于激活类的指定类名的设置方式发生了改变。

```jsx
computedClassName = ({ isActive }) => {
  return isActive ? 'mystyle myactive' : 'mystyle'
}

<NavLink
  className={({ isActive }) => {
    return isActive ? 'mystyle myactive' : 'mystyle'
  }}
  to='/Home'
>
  Home
</NavLink>

<NavLink className={this.computedClassName} to='/About'>
  About
</NavLink>
```

在 NavLink 中加 end 属性时，当子路由被激活时，父路由取消激活。

### 路由表

我们可以使用 useRoutes 来帮我们自动生成路由。

```jsx
import React from 'react'
// 按需导入需要的 react-router-dom 功能
import { NavLink, Navigate, useRoutes } from 'react-router-dom'
import About from './pages/About'
import Home from './pages/Home'

export default function App() {
  const element = useRoutes([
    {
      path: '/about',
      element: <About />,
    },
    {
      path: '/home',
      element: <Home />,
    },
    {
      path: '/',
      element: <Navigate to='/home' />,
    },
  ])
  return (
    <div>
      {/* 声明链接跳转 */}
      <NavLink to='/Home'>Home</NavLink>
      <NavLink to='/About'>About</NavLink>
      {/* 注册路由 */}
      {element}
    </div>
  )
}
```

在生产环境下，我们都会将路由表和组件分离，将 element 独自写在 src/routes/index.js 中，然后在需要的组件中引入即可。

### 嵌套路由

在一级路由使用路由表的情况下，使用嵌套路由只需要注册链接，和使用 Outelet 声明路由组件存在的位置。

路由表

```jsx
import { Navigate } from 'react-router-dom'
import Home from '../pages/Home'
import About from '../pages/About'
import News from '../pages/Home/News'
import Messages from '../pages/Home/Messages'

export default [
  {
    path: '/about',
    element: <About />,
  },
  {
    path: '/home',
    element: <Home />,
    children: [
      {
        path: 'news',
        element: <News />,
      },
      {
        path: 'messages',
        element: <Messages />,
      },
    ],
  },
  {
    path: '/',
    element: <Navigate to='/home' />,
  },
]
```

嵌套路由使用

```jsx
import React from 'react'
import { NavLink, Outlet } from 'react-router-dom'

export default function Home() {
  return (
    <div>
      <h2>Home</h2>
      <NavLink to='news'>News</NavLink>
      <NavLink to='/home/messages'>Messages</NavLink>
      {/* 指定路由组件呈现的位置 */}
      <Outlet />
    </div>
  )
}
```

### 向路由组件传递参数

#### 传递 params 参数

传递 params 参数需要在注册路由时声明传递的参数，如果使用路由表，则需要在路由表中声明。

```jsx
{
  path: 'detail/:id/:title/:content',
  element: <Detail />,
}
```

声明好要传递的参数之后，就需要在注册链接时在声明的对应位置携带对应的参数。

```jsx
<ul>
  {messages.map(msg => {
    return (
      <li key={msg.id}>
        <NavLink to={`detail/${msg.id}/${msg.title}/${msg.content}`}>
          {msg.title}
        </NavLink>
      </li>
    )
  })}
</ul>
<Outlet />
```

传递的参数将会在路由组件的 this.props.match.params 上以对象的形式存在。

如果使用了函数式声明组件，那么将无法使用 this 读取到路由组件本身，需要使用 react-router-dom 提供的钩子函数 useParams。

```jsx
import React from 'react'
import { useParams } from 'react-router-dom'

export default function Detail() {
  const { id, title, content } = useParams()
  return (
    <div>
      <ul>
        <li>{id}</li>
        <li>{title}</li>
        <li>{content}</li>
      </ul>
    </div>
  )
}
```

顺便一提，我们依然可以使用提供的钩子 useMatch 直接读取到整个 match 属性，但是需要传入完成的路由路径。当然也有 useLocation 钩子可以获取 location 属性。

```jsx
import React from 'react'
import { useMatch } from 'react-router-dom'

export default function Detail() {
  const match = useMatch('/home/messages/detail/:id/:title/:content')
  const { id, title, content } = match.params
  return (
    <div>
      <ul>
        <li>{id}</li>
        <li>{title}</li>
        <li>{content}</li>
      </ul>
    </div>
  )
}
```

#### 传递 search 参数

search 参数传递时不需要在注册路由时声明传递的数据，只需要在注册链接时以 query 的形式传递参数即可。

```jsx
<ul>
  {messages.map(msg => {
    return (
      <li key={msg.id}>
        <NavLink to={`detail?id=${msg.id}&title=${msg.title}&content=${msg.content}`}>
          {msg.title}
        </NavLink>
      </li>
    )
  })}
</ul>
<Outlet />
```

在路由组件读取 search 时需要使用 useSearchParams 获取。

```jsx
import React from 'react'
import { useSearchParams } from 'react-router-dom'

export default function Detail() {
  const [search, setSearch] = useSearchParams()
  console.log(search)
  const id = search.get('id')
  const title = search.get('title')
  const content = search.get('content')
  return (
    <div>
      <ul>
        <li>{id}</li>
        <button onClick={() => setSearch('id=008&title=haha&content=xixi')}>
          setSearch
        </button>
        <li>{title}</li>
        <li>{content}</li>
      </ul>
    </div>
  )
}
```

其中 setSearch 为改变当前地址栏的 query 参数。

#### 传递 state 参数

传递 state 参数不需要再注册路由时声明，只需要再注册链接时携带即可。

```jsx
<ul>
  {messages.map(msg => {
    return (
      <li key={msg.id}>
        <NavLink
          to='detail'
          state={{
            id: msg.id,
            title: msg.title,
            content: msg.content,
          }}
        >
          {msg.title}
        </NavLink>
      </li>
    )
  })}
</ul>
<Outlet />
```

在路由组件读取 state 时就需要 useLocation 来读取。

```jsx
import React from 'react'
import { useLocation } from 'react-router-dom'

export default function Detail() {
  const { id, title, content } = useLocation().state || {}

  return (
    <div>
      <ul>
        <li>{id}</li>
        <li>{title}</li>
        <li>{content}</li>
      </ul>
    </div>
  )
}
```

### 编程式路由导航

编程式路由导航在 React Router 6 中的函数式声明中都由 useNavigate 完成。

```jsx
import React, { useState } from 'react'
import { Outlet, useNavigate } from 'react-router-dom'

export default function Messages() {
  const navigate = useNavigate()
  const [messages] = useState([
    { id: '001', title: '消息1', content: '锄禾日当午' },
    { id: '002', title: '消息2', content: '汗滴禾下土' },
    { id: '003', title: '消息3', content: '谁之盘中餐' },
    { id: '004', title: '消息4', content: '粒粒皆辛苦' },
  ])
  
  function back() {
    navigate(-1)
  }
    
  function forward() {
    navigate(1)
  }
    
  function codeLink(msg) {
    navigate('detail', {
      replace: false,
      state: {
        id: msg.id,
        title: msg.title,
        content: msg.content,
      },
    })
  }

  return (
    <div>
      <h3>Messages</h3>
      <button onClick={back}>back</button>
      <button onClick={forward}>forward</button>
      <ul>
        {messages.map(msg => {
          return (
            <li key={msg.id}>
              <button onClick={() => codeLink(msg)}>useNavigate</button>
            </li>
          )
        })}
      </ul>
      <Outlet />
    </div>
  )
}
```

注意：编程式导航

### 不常用的 Hooks API

#### useInRouterContext

只要包括在 BrowserRouter 或 HashRouter 之中的组件，都是 true，不包含则为 false。

```jsx
import React from 'react'
import { useInRouterContext } from 'react-router-dom'

export default function News() {
  console.log(useInRouterContext())
  return <div>News</div>
}
```

#### useNavigationType

判断当前路由组件是 push 模式显示的还是 replace 模式显示的。

```jsx
import React from 'react'
import { useNavigationType } from 'react-router-dom'

export default function News() {
  console.log(useNavigationType())
  return <div>News</div>
}
```

#### useOutlet

用来呈现当前组件中渲染的嵌套路由。也就是 Outlet 所展示的组件。

如果组件还没有被挂载，那么则为 null，组件已经被渲染则显示组件对象本身。

```jsx
import React from 'react'
import { NavLink, Outlet, useOutlet } from 'react-router-dom'

export default function Home() {
  console.log(useOutlet())
  return (
    <div>
      <h2>Home</h2>
      <NavLink to='news'>News</NavLink>
      <NavLink to='/home/messages'>Messages</NavLink>
      {/* 指定路由组件呈现的位置 */}
      <Outlet />
    </div>
  )
}
```

#### useResolvedPath

帮助我们解析路径。

```jsx
import React from 'react'
import { useResolvedPath } from 'react-router-dom'

export default function News() {
  console.log(useResolvedPath('/user?id=001&name=tom#qwe'))
  // out: {pathname: '/user', search: '?id=001&name=tom', hash: '#qwe'}
  return <div>News</div>
}
```

