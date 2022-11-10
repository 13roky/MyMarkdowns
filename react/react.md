# React 入门

## hello react

在 html 中使用 react 需要引入以下三个文件：

- `react.development.js`：react 本尊
- `react-dom.development.js`：为 react 将虚拟 DOM 转换成真实 DOM 渲染到页面
- `babel.min.js`：解析 react 的 jsx 语法

```js
<body>
  <div id="test"></div>
  <!-- 导入 react -->
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <!-- 导入 react-dom 用于渲染 -->
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <!-- 导入 balel 用于解析 jsx 代码 -->
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>

  <!-- jsx 代码 -->
  <script type="text/babel">
    // 1.创建虚拟DOM
    const VDOM = <h1>Hello,React</h1>
    // 2.渲染虚拟DOM到页面
    ReactDOM.render(VDOM, document.getElementById("test"))
  </script>
</body>
```

## 虚拟 DOM 的创建方式

**1. 使用 jsx 创建虚拟 DOM**

参考 hello react 代码

**这种方式中 babel 会将我们 jsx 中的 VDOM 转换成 DOM 节点。**

**2. 使用 js 创建虚拟 DOM**

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script type="text/javascript">
    // 1.创建虚拟DOM
    const VDOM = React.createElement(
      "h1",
      { id: "title" },
      "<span>Hello,React</span>"
    )
    // 2.渲染虚拟DOM到页面
    ReactDOM.render(VDOM, document.getElementById("test"))
  </script>
</body>
```

使用 js 创建虚拟 DOM 时，不需要使用 babel 转译。

**但是声明的虚拟 DOM 必须是 DOM 节点，不能是字符串等形式。**

## 关于虚拟 DOM

- 本质是 Object 类型的对象（一般对象）
- **虚拟 DOM 比较“轻”，真实 DOM 比较“重”，因为虚拟 DOM 是 React 内部在用，不需要真实 DOM 上那么多属性**
- 虚拟 DOM 最终会被 React 转换为真实 DOM，呈现在页面上

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>

  <script type="text/babel">
    // 1.创建虚拟DOM
    const VDOM = (
      <h1>
        <span>Hello,React</span>
      </h1>
    )
    // 2.渲染虚拟DOM到页面
    ReactDOM.render(VDOM, document.getElementById("test"))
	
    const TDOM = document.getElementById("test") // 创建真实DOM进行对比
    console.log("虚拟DOM", VDOM)
    console.dir(TDOM)
    console.log(typeof VDOM)
    console.log(VDOM instanceof Object)
  </script>
```

## jsx 语法规则

1. 定义虚拟 DOM 时，不要写引号，因为引号会被当做字符串，并不会被 babel 解析。**单行直接写 html，多行用括号`()`包裹。**
2. 标签中混入 JS 表达式时要用花括号 `{}`。
3. 标签中样式的类名指定不要用 class，应该使用 `className`。
4. 内联样式，要用`style={{ key:value }}`的形式。第一个 {} 标识其中是表达式，第二个 {} 表示对象。
5. 只能由一个根标签，与 vue2 的 `<tamplate>` 规则类似。
6. 标签必须闭合，因为有些单标签不会闭合，这将会报错，我们应将其闭合，如`<input>`变为`<input />`。
7. 标签首字母
      (1) 若小写字母开头，则将改标签为 html 中同名元素，若 html 中无该标签对应的同名元素，则报错。
      (2) 若大写祖母开头，React 就会去渲染对应的组件，若组件没有定义，则报错。

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    const myId = "MyId"
    const myData = "helLo,react"
    // 1.创建虚拟DOM
    const VDOM = (
      <div>
        <h1 className="title" id={myId.toLocaleLowerCase()}>
          <span style={{ color: "white", fontSize: "20px" }}>{myData}</span>
        </h1>
        <h1 className="title" id={myId.toLocaleUpperCase()}>
          <span style={{ color: "white", fontSize: "20px" }}>{myData}</span>
        </h1>
        <input type="text" />
      </div>
    )
    // 2.渲染虚拟DOM到页面
    ReactDOM.render(VDOM, document.getElementById("test"))
  </script>
</body>
```

## js 语句与 js 表达式的区别

1. 表达式：一个表达式会产生一个值，可以放在任何需要值的地方。下面这些都是表达式：

​       （1）`a`

​       （2）`a+b`

​       （3）`demo(1)`

​       （4）`arr.map()`

​       （5）`function test(){}`

​    2. 语句（代码）下面这些是语句：

​       （1）`if()`

​       （2）`for(){}`

​       （3）`switch(){case:xxx}`

## jsx 多条数据渲染

- 使用 `arr.map()`将数据拼接成节点，然后逐条渲染到页面。

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
      
    const data = ["Angular", "React", "Vue"]
    const VDOM = (
      <div>
        <h1>jsx数据展示</h1>
        <ul>
          {data.map((item, index) => {
            return <li key={index}>{item}</li>
          })}
        </ul>
      </div>
    )
    ReactDOM.render(VDOM, document.getElementById("test"))
  </script>
</body>
```

# React 面向组件编程

## 定义组件

**函数式组件**

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>

  <script type="text/babel">
    // 1.创建函数式组件
    function Mycomponent() {
      return <h2>我是用函数定义的组件(适用于[简单组件]的定义)</h2>
    }
    // 2.渲染组件到页面
    ReactDOM.render(<Mycomponent />, document.getElementById("test"))
  </script>
</body>
```

- 声明函数式组件时，函数名要大写，返回值为虚拟 DOM。
- 渲染组件时，以标签的形式`<Mycomponent />`来表示组件。
- 函数式组件有很多局限性，一般很少使用，生产中类式组件被广泛运用。

注意：

- 没必要纠结于构造函数和函数的概念，这里声明的仅仅是一个 React 组件。
- 由于 js 是一个灵活的语言，并没有对构造函数和函数有内部定义，所以我们完全是通过约定俗成的规范（首字母大小写）来区分一个函数是否是构造函数。其实二者本质就是一个函数而已。

**类式组件**

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    // 1.创建类式组件
    class Mycomponent extends React.Component {
      // render是在类的原型对象上的，供实例使用
      render() {
        return <h2>我是用类定义的组件(适用于[复杂组件]的定义)</h2>;
      }
    }
    // 2.渲染组件到页面
    ReactDOM.render(<Mycomponent />, document.getElementById("test"));
    /* 
      执行了ReactDOM.render(<Mycomponent/>, document.getElementById("test"))之后发生了什么？
        1. React解析组件标签，找到了Mycomponent组件
        2. 发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法
        3. 将render方法返回的虚拟DOM转为真实DOM，随后呈现在页面中
      */
  </script>
</body>
```

- 类式组件需要继承`React.Component`才会被 React 承认为组件。
- `render() `，函数返回我们的虚拟 DOM，这将被定义在类的原型对象上，供实例使用。

**ReactDOM.render 逻辑**

1. React 解析我们提供给 ReactDOM.render 的组件标签，然后找到了 Mycomponent 组件。
2. 对组件进行判断，发现其是类式组件，之后 new 出该类的实例，并通过该实例调用其原型对象上的 render 方法。
3. 将 render 方法返回的虚拟 DOM 转换为真实 DOM，随后呈现在页面中。

## 关于类知识的复习

```js
<script>
  /* 
    总结：
      1. 类中的构造器不是必须要写的，要对实例添加一些初始化的操作，如添加指定属性时才写
      2. 如果A类继承了B类，且A类中写了构造器，那么A类构造器中的super时必须要调用的
      3. 类中要定义的方法都是放在了类的原型对象上，供实例去使用。
  */
  // 创建一个Person类
  class Person {
    // 构造器函数
    constructor(name, age) {
      this.name = name
      this.age = age
    }
    // 一般方法
    speak() {
      console.log(`我叫${this.name}, 年龄是${this.age}`)
    }
  }

  // 创建一个Student类，继承与Person类
  class Student extends Person {
    constructor(name, age, grade) {
      super(name, age)
      this.grade = grade
    }
    speak() {
      console.log(
        `我叫${this.name}, 年龄是${this.age},我读的是${this.grade}`
      )
    }
    // 赋值声明函数
    study = function() {
      console.log("我要努力学习")
    }
  }

  class Car {
    // 类中可以直接写赋值语句，给Car的实例对象添加一个属性名为a值为1
    wheel = 4
    constructor(name, price) {
      this.name = name
      this.price = price
      // this.wheel = 4 // 固定值可以直接写在类中
    }
  }
  const c1 = new Car("奔驰", 199)
  const c2 = new Car("宝马", 99)

</script>
```

- 类中的构造器并不是必要的，如果要对创建的实例对象身上追加属性等初始化操作时才需要构造器。
- 如果子类继承了父类，且父类中使用了构造器，那么子类中也必须使用构造器，并在子类构造器中使用 `super(a, b)` 调用父类类构造器并传入父类构造器需要的值。

- 类中的方法都是定义在类的原型对象上的；但是使用赋值声明的对象是声明在实例对象身上的；一般直接在类中声明常量，不能用 let、var 等进行修饰。

- 在类中只要由 `static` 声明的方法和变量，都是声明在类本身的，或者说都是声明在构造器身上的，不需要 `new` 就可以直接调用。

**类中 this 的指向**

```js
<script>
  class Person {
    constructor(name, age) {
      this.name = name
      this.age = age
    }
    speak() {
      console.log(this)
    }
    study = () => {
      console.log(this)
    }
  }

  const p1 = new Person("tom", 18)
  p1.speak()	// p1
  const x = p1.speak
  // 类中定义的方法都是局部开启严格模式的
  x()	// undefined
  const x2 = p1.study
  x2()	// p1
</script>
```

- 在类的构造函数中，this 指向类的实例对象。
- 在类的普通函数中
  - this 指向函数的调用者，一般调用者为实例对象，所以一般为实例对象。
  - 在特殊情况下，当方法被赋值出去，这样调用者就不是实例对象了，而是指向了实际调用者，但是类中的方法是开启严格模式的，所以例子中不是指向 window 而是 undefined。
- 在 React 组件中
  - 里面的函数一般都是作为回调函数立即调用的，所以其 this 为 undefined。
  - 为了避免这种情况，我们使用箭头函数的形式来声明函数。这样 this 就指向了 箭头函数所在作用域内的 this 也就是实例对象了。

## 组件中的方法

**关于方法**

组件中的方法，在调用的时候一般都是有 React 通过回调函数的形式来调用的，因此调用者不是实例对象，这时 this 的指向由于类中函数默认开启严格模式，本来该指向 window 的 this 指向了 undefined。

我们可以使用箭头函数赋值的方式来解决此问题，因为箭头函数中不存在 this，所以在其中调用 this 其实是通过作用域链查找到的箭头函数所在作用域中的 this。

```js
class Test extends React.Component {
	say = () => {
        console.log("hello")
    }
}
```

## 组件实例的三大属性

### state

> state 是 react 组件实例对象身上的一个属性，以对象的形式用于存储和管理状态（数据）

#### 初始化

在构造器中声明

```js
class Weather extends React.Component {
  constructor(props) {
    super(props)
    // 初始化状态
    this.state = {
      isHot: true,
    }
  }  
}
```

在类中声明（推荐）

```js
class Weather extends React.Component {
    // 初始化状态
	state = {
      isHot: true,
    }
}
```

#### 使用

由于 state 初始化在实例对象身上，所以使用时也从实例对象身上调用。

```js
class Weather extends React.Component {
  constructor(props) {
    super(props)
    // 初始化状态
    this.state = {
      isHot: true,
    }
    // 解决changeWeather中this指向问题
    this.changeWeather = this.changeWeather.bind(this)
  }
  // 渲染组件到页面
  // render调用了 n+1 次 1是初始化的那次，n是状态更新的次数
  render() {
    const { isHot } = this.state
    return (
      <h1 onClick={this.changeWeather}>
        今天天气很{isHot ? "炎热" : "凉爽"}
      </h1>
    )
  }
  // changeWeather在Weather的原型对象上，供实例使用
  // 由于changeWeather是作为onclick的回调，所以不是通过实例调用的，是直接调用
  // 类中的方法默认开启了局部严格模式，所以changeWeather中的this为undefined
  changeWeather() {
    const isHot = this.state.isHot
    console.log(this)
    // 这种方式值会改变，但是页面不会重新渲染
    // this.state.isHot = !this.state.isHot;
    // 不会覆盖整个对象，只会对指定key-value进行修改
    this.setState({ isHot: !isHot })
  }
}
```

简化版

```js
// 1.创建组件
class Weather extends React.Component {
  // 初始化状态
  state = {
    isHot: true,
    wind: "微风",
  }
  constructor(props) {
    super(props)
  }
  // 渲染组件到页面
  // render调用了 n+1 次 1是初始化的那次，n是状态更新的次数
  render() {
    const { isHot, wind } = this.state
    return (
      <h1 onClick={this.changeWeather}>
        今天天气很{isHot ? "炎热" : "凉爽"}, {wind}
      </h1>
    )
  }
  // 这种写法写在实例自身上而并不是原型对象上，使用箭头函数其中this为其所在的作用域中的this
  // 类中的方法默认开启了局部严格模式，所以changeWeather中的this为undefined
  changeWeather = () => {
    console.log(this)
    const isHot = this.state.isHot
    // 这种方式值会改变，但是页面不会重新渲染
    // this.state.isHot = !this.state.isHot;
    this.setState({ isHot: !isHot })
  }
}
// 2.渲染组件到页面
ReactDOM.render(<Weather />, document.getElementById("test"))
```

### props

> 用于在组件标签中向组件内传递属性值

props 属性也存在于组件实例对象身上。

#### 基本使用

```js
// 创建组件
class Person extends React.Component {
  render() {
    const { name, sex, age } = this.props
    return (
      <ul>
        <li>姓名: {name}</li>
        <li>性别: {sex}</li>
        <li>年龄: {age + 1}</li>
      </ul>
    )
  }
}
// 渲染组件到页面
ReactDOM.render(
  <Person name="tom" age={18} sex="女" />,
  document.getElementById("test1")
)
ReactDOM.render(
  <Person name="jerry" age={18} sex="女" />,
  document.getElementById("test2")
)
const p = { name: "brokyz", age: 18, sex: "男" }
// 这里{...p}中的{}是React中指定其中为表达式, 单从展开运算符来看,...p语法错误,但是babel会为我们进行处理.
ReactDOM.render(<Person {...p} />, document.getElementById("test3"))
```

- 在组件标签中传入指定属性，然后再组件内部对传入值进行处理

#### 对传入 props 进行限制

```js
<body>
  <div id="test1"></div>
  <div id="test2"></div>
  <div id="test3"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/prop-types.js"></script>
  <script type="text/babel">
    // 创建组件
    class Person extends React.Component {
      // 使用static修饰的变量是添加到类本身的属性, 不添加的化是添加到实例对象的属性
      // 对标签属性进行类型,必要性限制
      static propTypes = {
        name: PropTypes.string.isRequired, // 限制name必传,且为字符串
        sex: PropTypes.string, // 限制sex为字符串
        age: PropTypes.number, // 限制age为数值
        speak: PropTypes.func, // 限制speak为函数
      }
      
      // 指定默认标签属性
      static defaultProps = {
        sex: "不男不女",
        age: "18",
      }
      
      render() {
        const { name, sex, age } = this.props // props是只读的
        // undefined 参与运算是 Nan
        return (
          <ul>
            <li>姓名: {name}</li>
            <li>性别: {sex}</li>
            <li>年龄: {age + 1}</li>
          </ul>
        )
      }
    }

    // 渲染组件到页面
    ReactDOM.render(
      <Person name="tom" age={18} sex="男" speak={speak} />,
      document.getElementById("test1")
    )
    ReactDOM.render(
      <Person name="jerry" age={18} sex="女" />,
      document.getElementById("test2")
    )
    const p = { name: "brokyz", age: 18, sex: "男" }
    // 这里{...p}中的{}是React中指定其中为表达式, 单从展开运算符来看,...p语法错误,但是babel会为我们进行处理.
    ReactDOM.render(<Person {...p} />, document.getElementById("test3"))

    function speak() {
      console.log("speak!!")
    }
  </script>
</body>
```

- 在 react16 之后，对属性进行限制的属性 React.PropTypes 由于会使 React 对象变得臃肿上，所以被抽离出来成为一个单独的库，使用时需要引入`prop-types.js`。
- 对 props 进行限制，需要在类本身上声明`propTypes`属性，然后使用`PropTypes`对指定 props 进行限制。
- 如果要设定 props 的默认值，需要在类本身上声明`defaultProps`属性。

#### props 与构造器

```js
<body>
  <div id="test1"></div>
  <div id="test2"></div>
  <div id="test3"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/prop-types.js"></script>
  <script type="text/babel">
    // 创建组件
    class Person extends React.Component {
      // 如果不写构造器,不会产生任何影响,开发中基本不写构造器
      // 但是一旦写了构造器,一定要将super(props),否则在构造器内的this.props将会是undefined
      // 所以是否使用super(props)取决于是否希望在构造器中通过this使用props
      constructor(props) {
        super(props)
        console.log(this.props)
      }
      // 使用static修饰的变量是添加到类本身的属性, 不添加的化是添加到实例对象的属性
      // 对标签属性进行类型,必要性限制
      static propTypes = {
        name: PropTypes.string.isRequired, // 限制name必传,且为字符串
        sex: PropTypes.string, // 限制sex为字符串
        age: PropTypes.number, // 限制age为数值
        speak: PropTypes.func, // 限制speak为函数
      }

      // 指定默认标签属性
      static defaultProps = {
        sex: "不男不女",
        age: "18",
      }

      render() {
        const { name, sex, age } = this.props // props是只读的
        // undefined 参与运算是 Nan
        return (
          <ul>
            <li>姓名: {name}</li>
            <li>性别: {sex}</li>
            <li>年龄: {age + 1}</li>
          </ul>
        )
      }
    }

    // 渲染组件到页面
    ReactDOM.render(
      <Person name="tom" age={18} sex="男" />,
      document.getElementById("test1")
    )
  </script>
</body>
```

#### 函数式组件使用 props

```js
<script type="text/babel">
  // 创建函数式组件
  function Person(props) {
    const { name, age, sex } = props
    return (
      <ul>
        <li>姓名: {name}</li>
        <li>性别: {sex}</li>
        <li>年龄: {age}</li>
      </ul>
    )
  }
  Person.propTypes = {
    name: PropTypes.string.isRequired, // 限制name必传,且为字符串
    sex: PropTypes.string, // 限制sex为字符串
    age: PropTypes.number, // 限制age为数值
    speak: PropTypes.func, // 限制speak为函数
  }
  Person.defaultProps = {
    sex: "不男不女",
    age: "18",
  }
  // 渲染组件到页面
  ReactDOM.render(
    <Person name="tom" age={18} sex="男" />,
    document.getElementById("test1")
  )
</script>
```

### refs

#### 字符串形式的 refs

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    /* 
      字符串类型的refs存在效率问题
    */
    class Demo extends React.Component {
      showData = () => {
        const { input1 } = this.refs
        console.log(input1.value)
      }
      showData2 = () => {
        const { input2 } = this.refs
        console.log(input2.value)
      }

      render() {
        return (
          <div>
            <input ref="input1" type="text" placeholder="点击按钮提示数据" />
            <button onClick={this.showData}>点我</button>
            <input
              ref="input2"
              onBlur={this.showData2}
              id="input2"
              type="text"
              placeholder="失去焦点提示数据"
            />
          </div>
        )
      }
    }

    ReactDOM.render(<Demo />, document.getElementById("test"))
  </script>
</body>
```

#### 回调函数类型的 refs

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    /* 
      回调函数式ref最终被react执行了，是将节点绑定在实例对象身上的，而不是实例对象身上的refs身上
    */
    class Demo extends React.Component {
      showData = () => {
        const { input1 } = this.refs
        console.log(input1.value)
      }
      showData2 = () => {
        const { input2 } = this
        console.log(input2.value)
      }

      render() {
        return (
          <div>
            <input ref="input1" type="text" placeholder="点击按钮提示数据" />
            <button onClick={this.showData}>点我</button>
            <input
              ref={(currentNode) => {
                this.input2 = currentNode
              }}
              onBlur={this.showData2}
              id="input2"
              type="text"
              placeholder="失去焦点提示数据"
            />
          </div>
        )
      }
    }

    ReactDOM.render(<Demo />, document.getElementById("test"))
  </script>
</body>
```

#### 回调函数的执行次数

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    /*
      当页面首次渲染时：
        回调函数式ref在页面首次进行渲染调用render的时候会被执行一次
      当页面进行更新时：
        当数据发生更新后，react会调用render来更新页面，这时为了保证上一次的回调式ref被清空，这里调用了两次
        第一次传入了null，为了验证上一个是否被清空
        第二次传入节点
        注意：这里每次的回调式函数由于都是我们传进去的所以都是一个新的函数，它们有不同的内存地址
      以上是内联式的回调ref的调用次数
      如果是绑定在class中的回调函数ref，只会在首次进行页面渲染时调用一次，页面更新数据时不会进行调用
    */
    class Demo extends React.Component {
      state = { isHot: true }
      changeWeather = () => {
        this.setState({ isHot: !this.state.isHot })
      }
      showInfo = () => {
        alert(this.input1.value)
      }
      showInfo2 = () => {
        alert(this.input2.value)
      }
      refClass = (currentNode) => {
        this.input2 = currentNode
        console.log("class式函数ref", currentNode)
      }
      render() {
        return (
          <div>
            <h2 onClick={this.changeWeather}>
              今天天气很{this.state.isHot ? "炎热" : "凉爽"}
            </h2>
            <input
              ref={(currentNode) => {
                this.input1 = currentNode
                console.log("内联式函数ref:", currentNode)
              }}
              type="text"
            />
            <button onClick={this.showInfo}>点我提示输入数据</button>
            <input ref={this.refClass} type="text" />
            <button onClick={this.showInfo2}>点我提示输入数据</button>
          </div>
        )
      }
    }

    ReactDOM.render(<Demo />, document.getElementById("test"))
  </script>
</body>
```

#### createRef 形式的 ref

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    /*
      React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点
      该容器专人专用，只能存取一个节点
    */
    class Demo extends React.Component {
      myRef = React.createRef()
      myRef2 = React.createRef()

      showInfo = () => {
        alert(this.myRef.current.value)
        console.log(this.myRef.current.value)
      }
      showInfo2 = () => {
        alert(this.myRef2.current.value)
        console.log(this.myRef2.current.value)
      }
      render() {
        return (
          <div>
            <input ref={this.myRef} type="text" />
            <button onClick={this.showInfo}>点我提示输入数据</button>
            <input
              onBlur={this.showInfo2}
              ref={this.myRef2}
              type="text"
              placeholder="失去焦点"
            />
          </div>
        )
      }
    }

    ReactDOM.render(<Demo />, document.getElementById("test"))
  </script>
</body>
```

## react 事件处理

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    /*
      1.通过onXxx属性指定事件处理函数（注意大小写）
        a.React使用的是自定义（合成）事件，而不是使用原生DOM事件————为了更好的兼容
        b.React中的事件是通过事件委托方式处理的（委托给组件最外层的元素）
      2.通过event.target得到发生事件的DOM元素对象（避免滥用ref）
        a.当事件触发和事件处理的对象都是同一个对象，那么就可以使用event.target
    */
    class Demo extends React.Component {
      myRef = React.createRef()
      myRef2 = React.createRef()

      showInfo = () => {
        alert(this.myRef.current.value)
        console.log(this.myRef.current.value)
      }
      // 当操作对象始终是本身，使用event避免ref滥用
      showInfo2 = (event) => {
        alert(event.target.value)
        console.log(event.target.value)
      }
      render() {
        return (
          <div>
            <input ref={this.myRef} type="text" />
            <button onClick={this.showInfo}>点我提示输入数据</button>
            <input
              onBlur={this.showInfo2}
              type="text"
              placeholder="失去焦点"
            />
          </div>
        )
      }
    }

    ReactDOM.render(<Demo />, document.getElementById("test"))
  </script>
</body>
```

## react 收集表单数据

### 非受控组件

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    /* 
      操作DOM获取到你要的数据
      ref属性接受一个回调函数，返回一个element节点,通过节点获取到数据
      可以在构造函数里面定义一个变量，通过变量来创建组件引用，就可以获取到这个节点
    */
    // 创建非受控组件
    class Login extends React.Component {
      handleSubmit = (event) => {
        event.preventDefault()  // 阻止默认行为
        const { username, password } = this
        alert(
          `你输入的用户名是: ${username.value}, 你输入的密码是: ${password.value}`
        )
      }
      render() {
        return (
          <form action="http://www.atguigu.com" onSubmit={this.handleSubmit}>
            用户名:
            <input
              ref={(c) => (this.username = c)}
              type="text"
              name="username"
            />
            密码:
            <input
              ref={(c) => (this.password = c)}
              type="password"
              name="password"
            />
            <button>登录</button>
          </form>
        )
      }
    }

    ReactDOM.render(<Login />, document.getElementById("test"))
  </script>
</body>
```

### 受控组件

```js
<body>
  <div id="test"></div>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/react-dom.development.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/react/16.8.2/babel.min.js"></script>
  <script type="text/babel">
    /* 
      在React中默认可以完成从state到表单value的动态绑定。
      给input提供onchange事件，一旦检测到文本框内容有变化，马上执行onchange事件获取表单的内容。 
    */
    // 创建受控组件
    class Login extends React.Component {
      state = {
        username: "", // 用户名
        password: "", // 密码
      }
      // 保存用户名到状态中
      saveUsername = (event) => {
        this.setState({ username: event.target.value })
      }
      // 保存密码到状态中
      savePassword = (event) => {
        this.setState({ password: event.target.value })
      }
      demo = (event) => {
        console.log(event.target.value)
      }

      handleSubmit = (event) => {
        event.preventDefault() // 阻止默认行为
        const { username, password } = this.state
        alert(`你输入的用户名是: ${username}, 你输入的密码是: ${password}`)
      }
      render() {
        return (
          <form action="http://www.atguigu.com" onSubmit={this.handleSubmit}>
            用户名:
            <input onChange={this.saveUsername} type="text" name="username" />
            密码:
            <input
              onChange={this.savePassword}
              type="password"
              name="password"
            />
            <button>登录</button>
          </form>
        )
      }
    }

    ReactDOM.render(<Login />, document.getElementById("test"))
  </script>
</body>
```

