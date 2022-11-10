# vue 指令

## 概述

指令是 vue 为开发者提供的模板语法，用于辅助开发者渲染页面的基本结构。

vue 中的指令按照不同的用途可分为如下 6 大类：

1. **内容渲染**指令。
2. **属性绑定**指令。
3. **事件绑定**指令。
4. **双向绑定**指令。
5. **条件渲染**指令。
6. **列表渲染**指令。

## 内容渲染指令

内容渲染指令用来辅助开发者渲染 DOM 元素的文本内容。常用的内容渲染指令有如下三个：

- v-text
- {{ }} 插值表达式
- v-html

**v-text**

v-text 会覆盖元素内的默认值。

只能渲染纯文本内容。

```html
<div id="app">
  <p v-text="username">默认值</p>
</div>

const vm = new Vue({
  el: "#app",
  data: {
    username: "brokyz"
  }
});
```

**{{ }} 插值表达式**

vue 提供的 **{{ }}** 语法，专门用来解决 v-text 会覆盖默认文本内容的问题。这种 {{ }} 语法的专业名称是**插值表达式**。

```html
<div id="app">
  <p>{{ username }}</p>
</div>

const vm = new Vue({
  el: "#app",
  data: {
    username: "brokyz"
  }
});
```

插值表达式不会覆盖文本内容，在开发中常用。

**v-html**

v-html 可以渲染带有 html 标签的字符串。

```html
<div id="app">
  <div v-html="html">默认值</div>
</div>

const vm = new Vue({
  el: "#app",
  data: {
	html: "<h1>v-html</h1>"
  }
});
```

## 属性绑定指令

如果需要为**元素的属性**动态绑定**属性值**，则需要用到 **v-bind** 属性绑定指令。

后台数据发生改变时，页面会跟着改变。但是页面数据改变时，不会对后台数据进行改变。

v-bind 可以简写为冒号 :

```html
<body>
  <div id="app">
    <input type="text" v-bind:placeholder="inputValue" />
    <br />
    <img :src="imgSrc" />
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    el: "#app",
    data: {
      inputValue: "this is brokyz",
      imgSrc: "https://v2.cn.vuejs.org/images/logo.svg"
    },
    methods: {
      test(value, e) {
        console.log(value);
        console.log(e);
        console.log(arguments[1]);
      },
    },
  });
</script>
```

**使用 JavaScript 表达式**

在 vue 提供的模板渲染语法中，除了支持**绑定简单的数据值**之外，还支持 JavaScript 表达式的运算：

```html
{{ number + 1}}

{{ ok ? 'YES' : 'NO'}}

{{ message.split('').reverse().join('') }}

<div :id="'list-' + id"></div>
```

### 绑定class样式

可以用`:class`动态的控制元素的class值，将会在原class上追加，不会影响已经有的class。

```html
<style>
  .box {
    width: 300px;
    height: 300px;
    border: 2px solid #000;
  }

  .box1 {
    border-radius: 8px;
  }

  .box2 {
    background-color: bisque;
  }

  .box3 {
    font-size: 30px;
  }
</style>
<body>
  <div id="app">
    <div class="box" :class="arrClass">test1</div>
    <div class="box" :class="objClass">test2</div>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    el: "#app",
    data: {
      // 数组形式
      arrClass: ["box1", "box2", "box3"],
      // 对象形式
      objClass: {
        box1: true,
        box2: false,
        box3: true,
      },
    },
  });
</script>
```

## 绑定style样式

```html
<body>
  <div id="app">
    <div :style="{fontSize: fsize + 'px'}">test1 {{fsize}}</div>
    <div :style="styleObj">test2 {{styleObj.fontSize}}</div>
    <div :style="[styleObj, styleObj2]">test3 {{styleObj.fontSize}}</div>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    el: "#app",
    data: {
      fsize: 40,
      styleObj: {
        fontSize: "30px",
        backgroundColor: "pink",
      },
      styleObj2: {
        color: "red",
      },
    },
  });
</script>
```

## 事件绑定指令

vue 提供了 v-on 时间绑定指令，用来辅助程序员为 DOM 元素绑定监听事件。

v-on 可以简写成 @。

时间绑定指令中触发的为**事件处理函数**，写在 vue 实例的 methods 中。

```html
<body>
  <div id="app">
    <h3>count 的值为：{{count}}</h3>
    <button v-on:click="addCount">+1</button>
    <button @click="subCount">-1</button>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    el: "#app",
    data: {
      count: 0,
    },
    methods: {
      addCount() {
        this.count += 1;
      },
      subCount() {
        this.count -= 1;
      },
    },
  });
</script>
```

### 事件参数对象 $event

在 vue 中使用 $event 来代表事件对象。当没有任何参数时可以省略 $event，vue 默认会给事件处理函数传递一个事件参数对象，由用户来决定是否接收。

无参数时：

```js
<button @click="showEvent">事件对象</button>

showEvent(e) {
  console.log(e);
}
```

有参数时不会自动传递事件对象，需要使用 $event 手动传输：

```js
<button @click="showEvent('a', $event, 'c')">事件对象</button>

showEvent(a, e, c) {
  console.log(e);
}
```

### 事件修饰符

在事件处理函数中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。因此，vue 提供了事件修饰符的概念，来辅助程序员更方便的对事件的触发进行控制。常用的 5 个事件修饰符如下：

| 事件修饰符 | 说明                                                      |
| ---------- | --------------------------------------------------------- |
| .prevent   | 阻止默认行为（例如：阻止 a 链接的跳转、阻止表单的提交等） |
| .stop      | 阻止事件冒泡                                              |
| .capture   | 以捕获的方式触发当前的事件处理函数                        |
| .once      | 绑定的事件只触发1次                                       |
| .self      | 只有在 event.target 是当前元素自身时触发事件处理函数      |

```js
<a href="https://www.baidu.com" @click.prevent="onLinkClick">百度首页</a>
```

### 按键修饰符

在监听键盘事件时，我们经常需要判断详细的按键。此时，可以为键盘相关的事件添加按键修饰符。

```js
// 当 key 是 Enter 时调用 vm.submit()
<input @keyup.enter="submit">
// 当 key 是 Esc 时调用 vm.clearInput()
<input @keyup.esc="clearInput">
```

## 双向绑定指令

vue 提供了 **v-model 双向数据绑定**指令，用来辅助开发者在**不操作 DOM** 的前提下，**快速获取表单的数据**。

通俗理解就是，页面数据改变后台也会跟着改变，后台数据改变页面也会跟着改变。

```html
<p>用户名：{{username}}</p>
<input type="text" v-model="username" />

<p>选中的省份时：{{province}}</p>
<select v-model="province">
    <option value="">请选择</option>
    <option value="1">北京</option>
    <option value="2">深圳</option>
    <option value="3">上海</option>
</select>
```

### v-model 修饰符

| 修饰符  | 作用                                                 |
| ------- | ---------------------------------------------------- |
| .number | 自动将用户的输入值**转为数值类型**                   |
| .trim   | 自动过滤用户输入的**首尾空白字符**                   |
| .lazy   | 在改变时而非输入时更新（**失去焦点完成改变时更新**） |

```js
<input type="text" v-model.number="n1" /> +
<input type="text" v-model.number="n2" /> =
<span>{{n1 + n2}}</span>
```

### 收集表单数据

```html
<body>
	<!-- 
	收集表单数据：
		若：<input type="text"/>, 则v-model收集的是value值，且用户输入的就是value值。
		若: <input type="radio"/>, 则v-model收集的是value值，且要给标签手动配置value值。
		若：<input type="checkbox"/>
			1. 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
			2. 配置input的value属性：
				a. v-model的初始值是非数组，那么手机的就是checked（勾选 or 未勾选，是布尔值）
				b. v-model的初始值是数组，那么收集的就是value组成的数组
		备注：v-model的三个修饰符：
			1. lazy：失去焦点后再收集数据
			2. number：输入字符串转为有效数字
			3. trim：过滤输入的首尾空格
	 -->
	<div id="app">
		<form @submit.prevent="submit">
			账号：<input type="text" v-model.trim="userInfo.username"><br><br>
			密码：<input type="password" v-model,trim="userInfo.password"><br><br>
			年龄：<input type="number" v-model.number="userInfo.age"><br><br>
			性别：男<input type="radio" name="sex" v-model="userInfo.sex" value="male">女<input type="radio" name="sex"
				v-model="userInfo.sex" value="female"><br><br>
			爱好：
			学习<input type="checkbox" value="study" v-model="userInfo.hobby">
			打游戏<input type="checkbox" value="game" v-model="userInfo.hobby">
			吃饭<input type="checkbox" value="eat" v-model="userInfo.hobby">
			<br><br>
			城市
			<select v-model="userInfo.city">
				<option value="">请选择</option>
				<option value="beijing">北京</option>
				<option value="shanghai">上海</option>
				<option value="guangzhou">广州</option>
			</select>
			<br><br>
			其他信息：
			<textarea v-model.lazy="userInfo.others"></textarea><br><br>
			<input type="checkbox" v-model="userInfo.agree">同意<br><br>
			<button>提交</button>
		</form>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
	const vm = new Vue({
		data: {
			userInfo: {
				username: '',
				password: '',
				age: '',
				sex: '',
				hobby: [],
				city: '',
				others: '',
				agree: '',
			}
		},
		methods: {
			submit() {
				alert(JSON.stringify(this.userInfo))
			}
		},
	}).$mount("#app")
</script>
```



## 条件渲染指令

**条件渲染指令**用来辅助开发者**按需控制 DOM 的显示与隐藏**。条件渲染指令有如下两个，分别是：

- v-if

  通过动态的创建和移除 DOM 元素，从而控制元素在页面上的显示与隐藏。

  有更高的切换开销。适合运行时条件很少改变的情况。

- v-show

  通过动态为元素添加或移除 style="display: none;"样式，从而控制元素的显示与隐藏；

  有更高的初始渲染开销。适合运行时频繁的切换的情况。

```js
<div id="app">
    <p v-if="true"></p>
	<p v-show="false"></p>
</div>
```

- v-else & v-else-if

  必须配合 v-if 使用。

```js
<div v-if="type === 'A'">优秀</div>
<div v-else-if="type === 'B'">良好</div>
<div v-else>差</div>
```

## 列表渲染指令

vue 提供了 v-for 列表渲染指令，用来辅助开发者基于一个数组来循环渲染一个列表结构。v-for 指令需要使用 item in items 形式的特殊语法，其中：

- items 是待循环的数组
- item 是被循环的每一项
- 可以选择添加参数 index 来记录每一项的索引值

```html
<body>
  <div id="app">
    <ul>
      <li v-for="(item, index) in list">
        姓名是：{{item.name}} id是: {{item.id}}
      </li>
    </ul>
    <p v-for="(v, k) in person">{{k}}: {{v}}</p>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    el: "#app",
    data: {
      list: [
        { id: 1, name: "brokyz" },
        { id: 2, name: "simple" },
      ],
      person: {
        name: "brokyz",
        age: 21,
        address: "Beijing",
      },
    },
  });
</script>
```

**注意：谁要循环就给谁绑定 v-for。**

### 列表查询

### 列表排序

### 使用 key 维护列表状态

当**列表的数据变化**时，默认情况下，vue 会**尽可能的复用**已存在的 DOM 元素，从而**提升渲染的性能**。但这种默认的性能优化策略，会导致**有状态的列表无法被正确更新**。

为了给 vue 一个提示，以便它能跟踪每个节点的身份，从而在保证**有状态的列表被正确更新**的前提下，**提升渲染的性能**。此时，需要为每项提供一个**唯一的 key 属性**。

可以将key理解成每个循环渲染出的元素的标识。

```js
<ul>
	<li v-for="(item, index) in list" :key="item.id">
      	<input type="checkbox"/>
        姓名是：{{item.name}}
	</li>  
</ul>
```

key 的注意事项：

1. key 的值只能是**字符串**或**数字**类型
2. key 的值**必须具有唯一性**（即：key 的值不能重复）
3. 建议把**数据项 id 属性的值**作为 key 的值（因为 id 属性的值具有唯一性）
4. 使用 **index 的值**当作 key 的值**没有任何意义**（因为 index 的值不具有唯一性）
5. 建议使用 v-for 指令时**一定要指定 key 的值**（既提升性能、又防止列表状态紊乱）

**当key为index时的问题**

![](https://pic.imgdb.cn/item/632bbe1016f2c2beb1cb7378.jpg)

- 当我们使用v-for渲染数据时，vue会首先在内存里生成虚拟DOM，之后将虚拟DOM转换为真实DOM渲染到页面，也就是说页面上呈现的都是真实DOM，如果我们在input中赋值，是赋值在真实DOM中的，并没有赋值到内存中的虚拟DOM。
- 当数据发生改变时，index索引会重新排列，会在内存中生成新的虚拟DOM，新的虚拟DOM会根据key值与老虚拟DOM进行对比算法（diff）。对比相同key值中节点的不同，如图中key=0时，文本节点出现不同，那么将使用新的文本节点作为虚拟DOM；标签节点相同，那么将保持这个标签节点。但是在真实DOM中的input中的值时根据key来绑定的，所以原来处于key=0的值，会继续位于key=0处。又由于index进行了重新排列，所以就出现了数据不对应的情况。
- 但是如果将id值作为key，数据发生改变时，id不会重新排列，并且唯一，所以真实DOM中的数据也会一一对应虚拟DOM。

**总结：**

1. 虚拟DOM中key的作用：

   key是虚拟DOM对象的标识，当数据发生改变时，Vue会根据【新数据】生成【新的虚拟DOM】，随后vue将进行【新虚拟DOM】与【旧虚拟DOM】的差异比较。

2. 差异对比规则：

   a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：

   ​	若虚拟DOM中内容没变，直接使用之前的真实DOM！

   ​	若虚拟DOM中内容发生改变，则生成新的真实DOM，随后替换掉页面中之前的真实DOM。

   b. 旧的虚拟DOM中未找到与新虚拟DOM相同的key:

   ​	创建新的真实DOM，随后渲染到页面。

3. 用index作为key可能会引发的问题：

​		a. 若对数据进行：逆序添加、逆序删除等破坏顺序的操作会产生没必要的真实DOM更新 ==》界面效果没有问题，但是效率低。

​		b. 如果结构中包括输入类的DOM：会产生错误DOM更新 ==》界面效果会出现问题。

4. 开发中如何选择key

​		a. 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证、学号等。

​		b. 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的。