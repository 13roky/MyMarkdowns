## diff 算法

> 当我们对 state 中的数据进行更新时，这时就涉及到了页面更新，在 react 中页面更新采用了 diff 算法。

### diff 算法的逻辑

- 当页面发生数据更新时，react 会根据新生成的数据生成新的虚拟 DOM，之后 react 会使用 diff 算法对新旧虚拟 DOM 进行比较。
  - 旧虚拟 DOM 中找到了与新虚拟 DOM 相同的 key。
    - 若新虚拟 DOM 中内容不变，则直接使用之前的真实 DOM。
    - 若新虚拟 DOM 中的内容发生改变，则生成新的真实 DOM 并替换掉页面中旧的真实 DOM，完成页面局部更新。
  - 旧虚拟 DOM 中未找到与新虚拟 DOM 相同的 key。
    - 根据数据创建新的真实 DOM，随后渲染到页面。
  - 没有指定 key 时，将会对新旧虚拟 DOM 对应的标签进行比较。
    - 若新虚拟 DOM 中内容不变，则直接使用之前的真实 DOM。
    - 若新虚拟 DOM 中的内容发生改变，则生成新的真实 DOM 并替换掉页面中旧的真实 DOM，完成页面局部更新。

### diff 算法的最小粒度是标签或节点

以以下代码进行说明。

```jsx
class Time extends React.Component {
  state = { date: new Date() }
  componentDidMount() {
    setInterval(() => {
      this.setState({ date: new Date() })
    }, 1000)
  }
  render() {
    return (
      <div>
        <h1>hello</h1>
        <input id="input1" type="text" />
        <span id="showTime">
          现在是: {this.state.date.toTimeString()}
          <input id="input2" type="text" />
        </span>
      </div>
    )
  }
}

ReactDOM.render(<Time />, document.getElementById("test"))
```

在以上代码中，使用定时器，每秒修改状态中的 `data`，进行时间的实时展示。

`input1` 生成的虚拟 DOM 为 `<input id="input1" type="text" />` 我们在生成的页面中输入值 `test` 后发现，页面的时间在不断更新的时候，`input1` 中我们输入的 `test` 值并没有被更新。这是因为我们的 `test` 是写在 真实 DOM 上的，其虚拟 DOM 依然为 `<input id="input1" type="text" />`，所以 diff 算法将其视为没有变化，不会对其进行更新。

`showTime` 中的时间每秒都会进行更新，所以其新生成的虚拟 DOM 和 旧虚拟 DOM不同，这时将会生成 `showTime` 的新真实 DOM，使用新生成的真实 DOM 替换 旧真实 DOM。这样页面就完成了针对 `showTime` 的局部更新。

这时我们可能会有疑问，如果我们在 `input2` 的页面中输入值 `test`，由于 `input2` 是写在 `showTime` 内部的，当 `showTime` 发生页面更新时，其中的 `input2` 是否也会被更新掉？

答案是否定的，这就是我们要讲的 **diff 算法对新旧 DOM 进行比较时的最小粒度（单位）是标签或节点**。`input2` 作为一个单独的标签，是单独进行新旧 DOM 比较的，不会和 `showTime` 绑定在一起进行新旧 DOM 比较。

### key 的一些问题

以以下代码进行说明。

```js
class Person extends React.Component {
  state = {
    persons: [
      { id: 1, name: "小张", age: 18 },
      { id: 2, name: "小李", age: 19 },
    ],
  }

  add = () => {
    const { persons } = this.state
    const p = { id: persons.length + 1, name: "小王", age: 20 }
    this.setState({ persons: [p, ...persons] })
  }
  render() {
    return (
      <div>
        <h2>展示人员信息</h2>
        <h3>使用index索引值作为key</h3>
        <button onClick={this.add}>添加一个小王</button>
        <ul>
          {this.state.persons.map((v, index) => {
            return (
              <li key={index}>
                {v.name}-----{v.age}
                <input type="text" />
              </li>
            )
          })}
        </ul>
        <h3>使用id作为key</h3>
        <ul>
          {this.state.persons.map((v, index) => {
            return (
              <li key={v.id}>
                {v.name}-----{v.age}
                <input type="text" />
              </li>
            )
          })}
        </ul>
      </div>
    )
  }
}
ReactDOM.render(<Person />, document.getElementById("test"))
```

当使用 index 作为 key 时，我们在 persons 数组头添加一个数据 `{ id: persons.length + 1, name: "小王", age: 20 }`。这时数据发生了更新，我们来进行慢动做分析：

```js
/*
	初始数据:
		{ id: 1, name: "小张", age: 18 },
		{ id: 2, name: "小李", age: 19 },	
		
	初始虚拟 DOM:
		<li key=0>小张-----18</li> <input key=0 type="text"/>
		<li key=1>小李-----19</li> <input key=1 type="text"/>
		
	初始真实 DOM:（在页面中手动为 input 赋值 小张、小李）
		<li key=0>小张-----18</li> <input key=0 type="text" value="小张"/>
		<li key=1>小李-----19</li> <input key=1 type="text" value="小李"/>
		
	更新后的数据:
		{ id: 3, name: "小王", age: 20 },
		{ id: 1, name: "小张", age: 18 },
		{ id: 2, name: "小李", age: 19 },
		
	更新后的虚拟DOM:
		<li key=0>小王-----20</li> <input key=0 type="text"/>
        <li key=1>小张-----18</li> <input key=1 type="text"/>
        <li key=2>小李-----19</li> <input key=2 type="text"/> 
        
    更新后真实 DOM:
    	<li key=0>小王-----20</li> <input key=0 type="text" value="小张"/>
		<li key=1>小张-----18</li> <input key=1 type="text" value="小李"/>
		<li key=2>小李-----19</li> <input key=2 type="text"/>	
*/
```

**问题1：当对数据进行逆序添加逆序删除等操作时，会存在严重的效率问题**

我们发现，由于我们使用的是 `index`，所以在 `persons` 首部插入一条数据时，`index` 会重新排列，这样更新前和更新后的数据就不能通过 `index` 而一一对应。也就是说，新旧虚拟 DOM 中的 `key` 也发生了重新排列，无法与数据一一对应。

这时 diff 算法通过 `key` 进行比对的新旧虚拟 DOM 由于没有一一对应，就会被判定为不相同，然后对每一个不相同的标签都生成新的真实 DOM 然后再页面中更新替换。这样做虽然不会有显示问题，但是其存在非常严重的效率问题。

**问题2：如果结构中包括输入类DOM时，不仅会出现效率问题，还会出现页面显示问题**

我们在其中加入了 `input` 等输入操作后，这样一来使用 `index` 将会引发页面的显示问题。

比如我们在数据对应页面中的 `input` 中输入数据各自的名字，然后继续首添加一个数据后，会发现更新后的页面中的 `input` 中的值不能对应各自的数据。

由于 `input` 的值是写在真实 DOM 中的，所以其新旧虚拟 DOM 都是相同的，不会生成新的真实 DOM。但是由于 `index` 重排，新的数据中的 `key` 发生了改变，但是 `input` 中绑定的 key 还是没有改变，这时 `input` 就会以原来绑定的 `key` 进行显示，从而造成错位的显示问题。

**使用 `id` 等唯一标识作为 `key`，将避免以上问题。**

![](https://pic1.imgdb.cn/item/635509ca16f2c2beb13a819d.jpg)

**开发时如何选择key**

1. 最好使用数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一标识。
2. 如果只是简单的展示数据，使用 `index` 作为 `key` 也是可以的。
