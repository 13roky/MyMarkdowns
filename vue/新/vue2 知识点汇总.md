# vue 基础概念

## MVVM

- M：模型（Model）：对应 data 中的数据。
- V：视图（View）：模板（页面）。
- VM：试图模型（ViewModel）：Vue实例对象。

![](https://pic.imgdb.cn/item/632c1a2416f2c2beb12ef6b2.jpg)

## 一个简单 vue demo

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello, Vue!</title>
  </head>
  <body>
    <div id="app">
      <p>{{start}}</p>
    </div>
  </body>
  <script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
  <script>
    const vm = new Vue({
      el: "#app",
      data: {
        start: "Hello, Vue!",
      },
    });
  </script>
</html>
```

以上 vue demo 大致有如下四个操作：

1. 导入vue。

2. 创建Vue实例。

3. 将Vue实例与指定容器进行绑定。

4. 渲染数据。

## el 和 $mount 绑定容器

在 vue 中，将 vue 实例与指定容器进行绑定有两种方式，分别是 el 和 $mount。

```js
const vm = new Vue({
  el: "#app",	// 使用 el 与 #app 进行绑定
  data: {
    start: "Hello, Vue!",
  },
});

const vm = new Vue({
  data: {
    start: "Hello, Vue!",
  },
}).$mount('#app');	// 通过 $mount 与 #app 进行绑定
```

**注意：使用 $mount 绑定容器比在实例内部使用 el 绑定容器更加灵活。**

## data 的两种写法

对象的形式：

```js
const vm = new Vue({
  el: "#app",
  data: {
    start: "Hello, Vue!",
  },
});
```

函数的形式：

```js
const vm = new Vue({
  el: "#app",
  data() {
    return {
      start: "Hello, Vue!",
    };
  },
});
```

注意：

- **进行前端工程化开发组件时，必须使用函数的形式写data。**
- 对于函数的形式：
  - 不能使用箭头函数，因为箭头函数中this指向window，并不是指向vue实例

## vue数据代理

### Object.defineProperty

在 JS 中用于给对象添加属性

```js
let person = {
    name: "brokyz",
    sex: "男",
}

Object.defineProperty(person, 'age', {
	value: 18,
    enumerable: true,	// 控制属性是否可以枚举，默认值是false
    writable: true,		// 控制属性是否可以修改，默认值是false
    configurable: true,		// 控制属性是否可以被删除，默认值是false
})
```
设置get()和set()

```JS
let person = {
    name: "brokyz",
    sex: "男",
}

let number = 18
Object.defineProperty(person, 'age', {
    enumerable: true,	// 控制属性是否可以枚举，默认值是false
    writable: true,		// 控制属性是否可以修改，默认值是false
    configurable: true,		// 控制属性是否可以被删除，默认值是false
    // 当有人读取person对象的age属性时，get函数（getter）就会被调用，并且返回值就是指定属性的值。
    get(){
        console.log("有人读取了age属性")
        return number
    },
    // 当有人修改person对象的age属性时，set函数（setter）就会被调用，并且会收到修改的具体值。
    set(value){
        console.log("有人修改了age属性")
        number = value	// 由于我们的age属性实际上是number，所以修改age时在setter修改number才会成功修改。
    }
})
```

### JS中简单的数据代理

通过一个对象代理对另一个对象中属性的操作。

```js
let obj = {x:100}
let obj2 = {y:200}

Object.defineProperty(obj2, 'x', {
    get(){
        return obj.x
    },
    set(value){
        obj.x = value
    }
})
```

### vue中的数据代理

```js
const vm = new Vue({
  el: "#app",
  data: {
      start: "Hello, Vue!",
      end: "bye~",
  },
});
```

```
console.log(vm)

end: (...)
start: (...)
get end: ƒ proxyGetter()
set end: ƒ proxySetter(val)
get start: ƒ proxyGetter()
set start: ƒ proxySetter(val)
```

以更改start来举例：

1.  vm.start ---------------getter-----------------vm._data.start
2. vm.start-----------------setter-----------------vm._data.start

![](https://pic.imgdb.cn/item/632c2c9716f2c2beb145727c.jpg)

**注意：_data使用过数据劫持来获取创建vue实例时配置对象中的data值的，并且_data是vm实例中的真实数据，在vm实例上的start属性是通过数据代理（getter）的_data中的start得来的，当修改vm.start时，也会数据代理（setter）去修改_data中的数据start**

### 数据劫持

数据劫持就是将Vue实例对象中的标准data对象，改成Vue实例中具有getter和setter的_data的形式，这种操作就叫做数据劫持。

## vue 监测数据更新的原理

### 使用JS模仿vue监听原理

```js
let data = {
  name: "brokyz",
  age: 21,
};

const obs = new Observer(data);
console.log(obs);

// 准备一个vm实例对象
let vm = {}
vm._data = data = obs

function Observer(obj) {
  const keys = Object.keys(obj);
  keys.forEach((k) => {
    Object.defineProperty(this, k, {
      get() {
        return obj[k];
      },
      set(val) {
        console.log(`${k} 被修改了，我要去解析模板，生成虚拟dom，新旧比较...`)
        obj[k] = val;
      },
    });
  });
}
```

注意：

- 以上示例只能够监听对象中的一层数据，如果监听对象中的对象这种多层数据无效。
- 但是在vue中会有递归的方式去实现多层数据的监听。

### vue监听相关知识

基于以上的代码，我们知道vue中的属性是通过getter来获取数据值的。对属性进行了更改后，会调用setter，里面会执行相关的操作去更新页面信息。

vue属性又是对`_data`的数据代理，真实数据是`_data`，对与数据的获取和修改操作也都是在`_data`上的。

`_data`里面的真实数据，是在Vue实例化的时候通过`_data`数据劫持实例化的配置对象中的`data{}`的数据得到的，并且在实例化时就已经为所有数据配置了getter和setter。

这是，如果我们手动添加新属性时，并不会带有getter和setter，我们又知道setter触发才会处理页面更新操作。因此我们使用`vm.newdata = 10`或者`vm._data.newdara = 10`时，确实是添加上新的属性了，但是由于没有getter和setter，不被vue承认因此在vue开发者工具中看不到对应值，只有在控制台使用`console.log(vm.newdata)`是才会输出，同时这种方法由于没有setter，所以页面上的数据也并不会进行更新。

**如果我们要新添加属性，则需要用到Vue为我们提供的set()方法**

这个方法会为我们添加setter和getter

```js
const vm = new Vue({
  el: "#app",
  data: {
      persons: {
          name: "brokyz",
          age: "21",
      }
  },
});

Vue.set(vm._data.persons, "sex", "男")
// 因为有数据代理，可以直接修改属性
vm.$set(this.persons, "sex2", "男")
```

注意：这个方法只能添加到属性中对象的属性。不能直接对vue实例添加对象，也不能直接在`_data`添加对象。

**同样我们要删除属性可以使用delete()方法**

```js
const vm = new Vue({
  el: "#app",
  data: {
      persons: {
          name: "brokyz",
          age: "21",
      }
  },
});

Vue.set(vm._data.persons, "sex", "男")
// 因为有数据代理，可以直接修改属性
vm.$set(this.persons, "sex2", "男")

Vue.delete(vm._data.persons, "sex")
vm.$delete(this.persons, "sex")
```

**修改数组数据**

在vue中，数组数据中的索引是没有getter和setter的，所以通过索引直接修改数组的方法`vm._data.arr[0] = "test"`并不会被vue识别并更新页面数据。

如果想让vue监听到数组的变化，则需要用改变数组的一些方法才可以。如：

- push：尾插入
- pop：尾删除
- shift：删除首个
- unshift：在首添加
- splice：在指定位置添加替换删除
- sort：数组排序
- reverse：反转数组

以上这些方法都是影响原数组的能被vue监听，但是像filter这些返回新数组不影响原数组的方法不能被vue监听。

注意：数组中如果存储的是对象，那么这个对象中的属性是有getter和setter的。

**也可以使用set()去求改数组**

```js
const vm = new Vue({
  el: "#app",
  data: {
      persons: ["brokyz","simple"]
  },
});

Vue.set(vm._data.persons, 0, "13roky")
```

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

   	若虚拟DOM中内容没变，直接使用之前的真实DOM！
		
   	若虚拟DOM中内容发生改变，则生成新的真实DOM，随后替换掉页面中之前的真实DOM。

   b. 旧的虚拟DOM中未找到与新虚拟DOM相同的key:

   	创建新的真实DOM，随后渲染到页面。

3. 用index作为key可能会引发的问题：

		a. 若对数据进行：逆序添加、逆序删除等破坏顺序的操作会产生没必要的真实DOM更新 ==》界面效果没有问题，但是效率低。
		
		b. 如果结构中包括输入类的DOM：会产生错误DOM更新 ==》界面效果会出现问题。

4. 开发中如何选择key

		a. 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证、学号等。
		
		b. 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的。

# vue 计算属性

## 概述

计算属性指的是通过一系列运算之后，最终**得到一个属性值**。

间的的理解，当我们拥有一些数据时，我们需要将这些数据整合到一起，这时候计算属性就会完成这个操作，整合到一起的数据会变成实例的一个新属性值。当用户改变某个数据时，计算属性也会动态的调成整合后的数据。

这个动态计算出来的**属性值**可以**被**模板结构或 methods 方法**使用**。

## 使用

```html
<body>
  <div id="app">
    <p>greeting1:{{greeting1}}</p>
    <p>greeting2:{{greeting2}}</p>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    el: "#app",
    data: {
      start: "Hello, Vue!",
      end: "bye~",
    },
    computed: {
        // 简写
      greeting1() {
        return this.start + this.end;
      },
        // 完整写法
      greeting2: {
        get() {
          return this.start + this.end;
        },
        // 如果有修改计算属性的需求可以设置set(),根据值去修改依赖的属性。
        // set(value){}
      },
    },
  });
</script>
```

此时虽说我们没有手动创建 greeting 属性，但是计算属性的函数会给我们自动创建一个关联对应属性的属性。我们可以直接调用。**当对应的属性发生改变时，r计算属性也会跟着动态改变**。

特点：

- 虽然计算属性在声明时被定义为方法，但计算属性的本质是一个属性。
- 计算属性会缓存计算的结果，只有计算属性依赖的数据发生变化时，才会重新进行计算。

注意：

- 当计算属性第一次被渲染调用时，getter被调用，计算属性会存于缓存，之后只要计算属性所依赖的数据不发生改变，都会从缓存读取这个计算属性不会再次调用getter。
- 当计算属性所依赖的数据发生改变时，getter再次被调用，计算属性改变，并会重新存于缓存。

- 如果我们在vue实例外声明`let a = 1`，在计算属性中将其作为依赖，那么a发生变化时，页面不会发生变化。因为声明在vue实例外面，脱离了vue的控制。所以计算属性要使用vue实例已有的属性。
- 只有考虑读取不考虑修改时才使用简写形式。

# vue props

## 概述

> 我们的组件进行展示数据时，里面并不是都是写死的。我们需要在使用组件时，向组件内部传值，并展现我们需要的值。

## 数组形式接收 props

TestProps.vue

```html
<template>
	<div>
		<h2>name: {{name}}</h2>
		<h2>sex: {{sex}}</h2>
		<h2>age: {{myAge}}</h2>
		<button @click="myAge++">myAge++</button>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				myAge: this.age
			}
		},
		props: ['name', 'age', 'sex']
	}
</script>
```

App.vue

```html
<template>
	<div id="app">
		<testProps name="brokyz" sex="male" :age="21"></testProps>
	</div>
</template>

<script>
	import TestProps from './components/TestProps.vue'
	export default {
		name: 'App',
		components: {
			TestProps
		}
	}
</script>
```

**注意：**

1. 使用 props 传入值时，默认传入都时字符串。如果需要传入数字型，则需要在传入属性前加冒号。如`:age="21"`传入的时数字 21，`age="21"`传入的是字符串 21。
2. 在传入 props 后，如果组件需要对传入值进行修改操作，需要在 data 函数中定义，否则报错。在创建的时候是先准备好 props 之后才配置数据 data。

## 对象形式接收 props

TestProps.vue

```html
<template>
	<div>
		<h2>name: {{name}}</h2>
		<h2>sex: {{sex}}</h2>
		<h2>age: {{myAge}}</h2>
		<button @click="myAge++">myAge++</button>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				myAge: this.age
			}
		},
		props: {
			name: String,
			age: {
				type: Number,
				required: true
			},
			sex: {
				type: String,
				default: "none"
			}
		}
	}
</script>
```

App.vue

```html
<template>
	<div id="app">
		<testProps name="brokyz" :age="21"></testProps>
	</div>
</template>

<script>
	import TestProps from './components/TestProps.vue'
	export default {
		name: 'App',
		components: {
			TestProps
		}
	}
</script>
```

- 对象的形式来声明 props 可以让 props 的接收更加具体，可以指定类型，默认值，和是否必传。
- 一般来说 default 和 required 不同时出现，因为不传才会有默认值，而必传又不需要默认值。

# vue 监听器

## 概述

**watch 监听器**允许开发者动态监视数据的变化，从而**针对数据的变化做出特定的操作**。

通俗的来理解可以理解为设定了一个函数，当指定的数据发生变化之后，vue 就会帮我们自动去触发这个函数，这就是 watch 监听器。

在 watch 监听器中，函数可以接收两个参数 `newVal`、`oldVal`。这两个参数分别**记录**了变化的**新值**和变化之前的**旧值**。

```js
const vm = new Vue({
    el: '#app',
    data: { username:'' },
    watch: {
        username(newVal, oldVal) {
            console.log(newVal, oldVal)
        }
    }
})
```

```js
vm.$watch('username', {
    handler(newValue, oldValue) {
        console.log(newVal, oldVal)
    }
})
```



## 立即调用 immediate 选项

默认情况下，**组件在初次加载完毕后不会调用 watch 侦听器**。如果想让 watch 侦听器立即被调用，则需要使用 immediate 选项。示例代码如下：

```js
const vm = new Vue({
    el: '#app',
    data: { username:'' },
    watch: {
        username: {
            // handler 固定写法，表示 username 发生变化时，自动调用 handler 处理函数
            handler: function(newVal) {
                console.log(newVal)
            },
            // 表示页面初次渲染好之后，就立即触发当前的 watch 监听器
            immediate: true
        }
    }
})
```

## 监听对象 deep 选项

如果 watch 监听的是一个**对象**，那么对象的属性值发生了变化，则**无法被监听到**。此时**需要使用 deep 选项**。

```js
const vm = new Vue({
    el: '#app',
    data: {
        info: { username: 'brokyz' }
    },
    watch: {
        info: {
            // handler 固定写法，表示 username 发生变化时，自动调用 handler 处理函数
            handler: function(newVal) {
                console.log(newVal.username)
            },
            // 监听一个对象时，如果要监视内部属性变化需要加deep参数为true
            deep: true
        }
    }
})
```

## 监听对象的单个属性变化

如果只想要监听对象中单个属性的变化，可以按照如下形式：

```js
const vm = new Vue({
    el: '#app',
    data: {
        info: { username: 'brokyz' }
    },
    watch: {
        'info.username': {
            handler(newVal) {
                console.log(newVal)
            }
        }
    }
})
```

## 针对与watch与computed的延时返回问题

### computed

```html
<body>
  <div id="app">
    <p>firstname: {{firstname}}, lastname: {{lastname}}.</p>
    <p>fullname: {{fullname}}</p>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    data: {
      firstname: "zhang",
      lastname: "boqiang",
    },
    computed: {
      fullname() {
        setTimeout(() => {
          console.log("计时器被调用");
          return this.firstname + "-" + this.lastname;
        }, 1000);
      },
    },
  }).$mount("#app");
</script>
```

- 在计算属性中，由于return在定时器中，所以这个return是定时器的return。因此计算属性并没有进行return。所以fullname并没有值。
- 如果想要定时器延迟输出则需要使用watch。

### watch

```html
<body>
  <div id="app">
    <p>firstname: {{firstname}}, lastname: {{lastname}}.</p>
    <p>fullname: {{fullname}}</p>
  </div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
  const vm = new Vue({
    data: {
      firstname: "zhang",
      lastname: "boqiang",
      fullname: "zhang-booqiang",
    },
    watch: {
      firstname(newV) {
        setTimeout(function () {
          this.fullname = newV + "-" + this.lastname;
        }, 2000);
      },
      lastname(newV) {
        setTimeout(() => {
          this.fullname = this.firstname + "-" + newV;
        }, 2000);
      },
    },
  }).$mount("#app");
```

- 如果延时输出那么定时器一定要使用箭头函数，因为箭头函数中的this，指向的是外层的vue实例，可以通过this访问到vue实例的属性。如果不使用箭头函数，那么this指向的就是windows对象。

注意：computed能完成的操作，watch都能够完成。但是watch能完成的工作，computed不一定能完成。比如异步操作。

# vue 过滤器

## 概述

**过滤器（Filters）**是 vue 为开发者提供的功能，常用于**文本的格式化**。过滤器可以用在两个地方：**插值表达式**和 **v-bind 属性绑定**。

过滤器应该被添加在 JavaScript 表达式的尾部，由“管道符”进行调用，示例代码如下：

```js
<p>{{ message | formatA | format }}</p>

<div :id="message | formatA | formatB"></div>

const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    },
    filters: {
        formatA (str) {
            return str + " A"
        },
        formatB (str) {
            return str + " B"
        }
    }
})
```

## 过滤器传参

过滤器类似于函数、里面可以进行传参。

```js
<p>{{ message | formatA(arg1) }}</p>

const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    },
    filters: {
        formatA (str, arg1) {
            return str + " A" + arg1
        }
    } 
})
```

## 全局过滤器

写在对应 vue 实例里面的是私有过滤器，只允许当前实例使用。

如果需要所有实例都能使用，可以设置全局过滤器。在创建实例前，对 vue 添加全局过滤器。

```js
<p>{{ message | formatA(arg1) }}</p>

Vue.filter('formatA', (str,arg1)=>{
    return str + " A" + arg1
})

const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    }
})
```

## 过滤器的兼容性

过滤器仅在 vue2.x 和 1.x 中受支持，在 vue3.x 的版本中剔除了过滤器相关功能。

在企业级项目开发中：

- 如果使用的是 2.x 版本的 vue，则一眼可以使用过滤器相关的功能。
- 如果项目已经升级到了 3.x 版本的 vue，官方建议使用计算属性或方法代替被剔除的过滤器功能。

## 使用方法替代过滤器

```js
<p>{{ formatA(message) }}</p>

const vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue.js'
    },
    methods: {
        formatA(str){
            return str + " A"
        }
    }
})
```

## demo

```html
<body>
	<div id="app">
		<h2>显示格式化后的时间</h2>
		<h3>时间戳：{{time}}</h3>
		<!-- 计算属性实现 -->
		<h3>时间：{{fmtTime}}</h3>
		<!-- 方法实现 -->
		<h3>时间：{{getTime()}}</h3>
		<!-- 过滤器实现 -->
		<h3>时间：{{time | filterTime("YYYY-MM-DD HH:mm") | mySlice}}</h3>
		<h3 :x="info | mySlice">过滤器也可以用在属性上</h3>

	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/dayjs/1.11.5/dayjs.min.js"></script>
<script>
	const vm = new Vue({
		data: {
			time: 1621561377603, // 时间戳
			info: "123456"
		},
		computed: {
			fmtTime() {
				return dayjs(this.time).format("YYYY-MM-DD HH:mm:ss")
			}
		},
		methods: {
			getTime() {
				return dayjs(this.time).format("YYYY-MM-DD HH:mm:ss")
			}
		},

		filters: {
			filterTime(v, str = "YYYY-MM-DD HH:mm:ss") {
				return dayjs(v).format(str)
			},
			mySlice(v) {
				return v.slice(0, 4)
			}
		}
	}).$mount("#app")
</script>
```

# vue ref 引用

## 概述

>  我们在使用 Vue 时，有时会需要拿到 DOM 元素进行操作。这时 Vue 不推荐我们进行使用原生 JS 获取 DOM 元素。而是为我们提供了 ref 方便我们去获取组件中的 DOM 元素。



## 使用 ref

在使用时，我们只要给标签加上 ref 属性并指定属性名，就可以在 Vue 中通过 vm 实例的 $refs 属性获取到我们添加 ref 属性值的标签。

### demo

```html
<template>
	<div id="app">
		<h1 ref="myh1">test ref1</h1>
		<button ref="myButton" @click="showRef">print ref</button>
		<HelloWorld ref="hello"></HelloWorld>
	</div>
</template>

<script>
	import HelloWorld from './components/HelloWorld.vue'

	export default {
		name: 'App',
		components: {
			HelloWorld
		},
		methods: {
			showRef() {
				console.log(this);
				console.log(this.$refs.myh1);
				console.log(this.$refs.myButton);
				console.log(this.$refs.hello);
			}
		}
	}
</script>
```

### 在 HTML 标签中添加 ref

- 在 HTML 标签中添加 ref 时，通过 $refs 拿到的是真实 DOM 元素。
- 可以通过 JS 原生对 DOM 的操作方法进行操作。

### 在组件标签中添加 ref

- 在组件标签中添加 ref 时，通过 $refs 拿到的是该组件的实例对象 vc。

- 如果想拿到组件的所有 HTML 标签，可以给组件追加 id，然后通过原生的 JS 获取到的就是 组件内部所有的 HTML 标签。

### 关于 $refs

- Vue 会将我们添加 ref 的标签，以上面的形式，添加到当前实例的 $refs 属性中，以便我们的调用。

# vue 自定义指令

## 概述

vue 中不不仅仅有官方提供的指令，用户还可以根据自己的需要进行自定义指令。

比如当我们需要一个常用的操作将文字改为蓝色，如果我们需要修改大量标签时，就可以使用自定义指令。免去修改每一个标签的麻烦。

## 私有自定义指令

我们可以在 `directives` 节点下声明私有自定义属性。

**在自定义指令的函数中，this指向的时window。**



### 函数式：指令与元素成功绑定时和指令所在模板重新解析时都会调用。

```html
<body>
	<div id="app">
		<h2 class="myh2" v-color="'pink'">test directives</h2>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
	const vm = new Vue({
		directives: {
			color(element, binding) {
				// element是当前绑定的DOM元素，binding是一个存于着当前自定义属性相关参数的对象
				console.log(element, binding);
				element.style.color = binding.value
			}
		}
	}).$mount("#app")
</script>
```

### 对象式：可以分别指定不同阶段所调用的函数。

```vue
<body>
	<div id="app">
		<input type="text" v-focus :value="n"><br>
		<button @click="n++">更新页面模板</button>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
	const vm = new Vue({
		data: {
			n: 1,
		},
		directives: {
			focus: {
				// 指令与元素成功绑定时调用
				bind(element) {
					element.focus()
				},
				// 指令所在元素被插入页面时调用
				inserted(element) {
					element.focus()
				},
				// 指令所在的模板被重新解析时调用
				update(element) {
					element.blur()
				},
			}
		}
	}).$mount("#app")
</script>
```

## 多个自定义指令单词用 - 隔开

```html
<body>
	<div id="app">
		<input type="text" v-focus-element :value="n"><br>
		<button @click="n++">更新页面模板</button>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/cdn/vue/vue-2.7.10.js"></script>
<script>
	const vm = new Vue({
		data: {
			n: 1,
		},
		directives: {
			'focus-element': {
				// 指令与元素成功绑定时调用
				bind(element) {
					element.focus()
				},
				// 指令所在元素被插入页面时调用
				inserted(element) {
					element.focus()
				},
				// 指令所在的模板被重新解析时调用
				update(element) {
					element.blur()
				},
			}
		}
	}).$mount("#app")
</script>
```

## 全局自定义指令

```js
Vue.directive('color', function(el, binding) { 
  el.style.color = binding.value
})
```

# vue 生命周期

## 概述

>  在使用 Vue 时，我们需要执行一些 JS 代码。比如我们需要在页面中添加一个定时器来固定间隔更新时间。这时我们可能会想到直接在，Vue 实例外书写 JS 代码。这种方法能完成操作，但是 Vue 并不建议这样写。Vue 建议尽量在 Vue 实例中完成所有的操作。这时我们就需要理解 Vue 的生命周期并使用 Vue 为我们提供的生命周期函数来完成。

**Vue 生命周期**指的就是从 Vue 实例创建，到 Vue 实例销毁这一整套流成。我们可以将其理解成 **Vue 的一生**。

对应 Vue 生命周期的不同阶段，Vue 为我们提供了一些函数，可供我们在对应的生命周期阶段调用我们需要的代码，这些函数我们惩治为 **Vue 生命周期函数**，我们也可以将其称为**钩子**（hook）。

**Vue 生命周期函数会随着 Vue 运行到不同阶段被 Vue 自动执行。**

## Vue 生命周期

下面的图片完成的介绍了 Vue 的一生。其中红色框的为 Vue 生命周期函数。蓝色框为解释说明。其余为 Vue 的一生。

![](https://pic.imgdb.cn/item/632e6b3216f2c2beb18294f5.jpg)

## Vue 生命周期函数

```html
<body>
	<div id="app">
		<button @click="number++">update data +1</button><br>
		<p>data: {{number}}</p>
		<button @click="destroy">destroy</button>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/vue/2.7.10/vue.js"></script>
<script>
	Vue.config.productionTip = false
	const vm = new Vue({
		data: {
			number: 0
		},
		methods: {
			destroy() {
				this.$destroy()
			}
		},
		beforeCreate() {
			console.log("beforeCreate");
		},

		created() {
			console.log("created");
		},

		beforeMount() {
			console.log("beforeMount");
		},

		mounted() {
			console.log("mounted");
		},

		beforeUpdate() {
			console.log("beforeUpdate");
		},

		updated() {
			console.log("updated");
		},

		beforeDestroy() {
			console.log("beforeDestroy");
		},

		destroyed() {
			console.log("destroyed");
		}

	}).$mount("#app")
</script>
```

钩子解读：

1. `beforeCreate`：这里指的数据检测、数据代理创建之前。所以这里并**不能访问到 vm 实例中的 data 数据和methods方法**。注意，这并不是 Vue 实例 vm 创建之前，如果 Vue 实例都还没有创建，又哪里会有 Vue 生命周期函数呢？
2. `created`：此时已经创建了数据检测和数据代理。这时**可以访问到 vm 实例的数据、methods 方法**。
3. `beforeMount`：此时虚拟 DOM 已经解析生成并存于内存中，但是还没有转换成页面中的真实 DOM 并插入页面，所以现在页面上展现的 DOM 还都是未经 Vue 编译的 DOM 结构。因此这里**对 DOM 的操作最终都不会生效**。因为不管怎么操作，都会一瞬间改变，之后 Vue 将虚拟 DOM 转换为真实 DOM 时插入页面后，之前的 DOM 操作会被覆盖掉。所以只是一瞬间生效，最终还会被覆盖掉失效。
4. `mounted`（常用）：此时内存中的虚拟 DOM 已经转换为真实 DOM 并插入页面。所以这时页面中呈现的时经过 Vue 编译的 DOM 结构。所以这时**对 DOM 的操作都有效（尽量避免）。一般都会在此处设置定时器、发送网络请求、订阅消息、绑定自定义事件等初始化操作。**
5. `beforeUpdate`：此时数据已经发生变化，但是页面还没有进行更新。也就是这里的**数据已经是新的了，但是页面还是旧页面。注意，如果只涉及数据变化，不涉及页面更新则不会触发页面更新流程。**
6. `updated`：此时页面已经完成更新。也就是说这时候**数据是新数据，页面也是新页面。注意，如果只涉及数据变化，不涉及页面更新则不会触发页面更新流程。**
7. `beforeDestroy`（常用）：**此时 vm 中的 data、methods、指令等都处于可用状态，马上执行销毁过程。此时不会数据进行操作，因为即便操作了也不会更新页面了。一般在此阶段关闭定时器、取消订阅消息、解绑自定义事件等收尾操作**。
8. `destroyed`：此时 vm 实例已经完全销毁，无法在借助 vm 实例对页面进行管理。

**还有三个钩子在这里并没有展现**

1. `nextTick(()=>{})`，这个钩子在 vm 和 vc 身上，有时我们对组件进行操作时，真实dom还没有生成被渲染到页面上，但是我们又需要操作这个dom。这样我们就无法获取我们要操作的元素。这时我们可以写在 nextTick 身上。其中的代码会在页面渲染之后执行。
2. `activated(){}`，这个钩子在路由中存在，如果一页面进行缓存而非销毁时。那么再次进入这个页面就会触发 activated。
3. `deactivated(){}`，这个钩子在路由中存在，如果一页面进行缓存而非销毁时。那么离开这个页面就会触发 deactivated。

# vue 组件

## 概述



## 非单文件组件（过渡）

非单文件组件就是指并非一个组件占用一个文件，而是一个人间中有多个组件。我们只将其作为学习的过度，实际开发中不适用这种方式。

demo：

```html
<body>
	<div id="app">
		<h1>大哥</h1>
		<hr>
		<!-- 3. 使用组件 -->
		<xuexiao></xuexiao>
		<hr>
		<student></student>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/vue/2.7.10/vue.js"></script>
<script>
	// 1. 创建组件
	// 组件中不用指定el，因为组件不用绑定死为谁服务，追随vm实例。
	// 组件中的数据必须以函数的形式赋值。
	const school = Vue.extend({
		// 书写组件的html
		template: `
		<div>
			<h2>school：{{name}}</h2>
		</div>
	`,
		data() {
			return {
				name: "首经贸"
			}
		}
	})
	// 创建组件的简写形式，直接用对象。
	const student = {
		template: `
		<div>
			<h2>student：{{name}}</h2>
		</div>
	`,
		data() {
			return {
				name: "brokyz"
			}
		}
	}

	const vm = new Vue({
		// 2. 注册组件
		components: {
			xuexiao: school, // 可以注册名和组件名相同直接简写成 school
			student,
		},
	}).$mount("#app")
</script>

```

**使用组件一般分为三步：**

1. 创建组件：此时组件已经创建，但是还没有被 vm 实例注册使用。
2. 注册组件：此时组件被 vm 实例注册，但是并没有被 vm 实例使用。
3. 编写组件标签（使用组件）：此时组件被 vm 实例使用。

**组件中不能指定 el 或者 mount：**

​	由于组件是灵活的可复用的，所以并不需要绑定一个元素只为其服务。而是由注册该组件的 vm 实例来决定一起为谁服务。

​	可以将 vm 实例理解成大哥，组件是加入大哥麾下的小弟。大哥说打哪小弟也跟着打哪。

​	所有的组件都得有大哥才可以展现出来。

**组件中必须以函数的形式来指定数据：**

​	因为在同一个文件下，如果以对象的形式来指定数据的话，那么多个组件指向的是同一个对象。这样组件之间的数据就比较混乱。

​	demo：

```js
let data = {
	name: "brokyz"
}
let a = data
let b = data
// 此时 a 和 b 指向了同一个 data，a 和 b 将操作同一个对象。

data(){
	return {
		name: "brokyz"
    }
}
let a = data()
let b = data()
// 此时 a 和 b 之前指向的是不同的对象。
```

**组件的结构将以 template 的形式赋予组件。**

## 组件的命名

在组件中指定 name 配置项，那么不管注册的时候是什么名字，在 Vue 开发者工具中都会显示组件指定的 name。

```js
const school = Vue.extend({
	// 指定组件的名字
	name: "myschool",
    // 书写组件的html
	template: `
		<div>
			<h2>school：{{name}}</h2>
		</div>
	`,
	data() {
		return {
			name: "首经贸"
		}
	}
})
```

**组件注册时的命名规范：**

1. 单个单词全小写`school: school`：在 Vue 开发者工具中首字母会自动大写显示。`School`
2. 单个单词首字母大写`School: school`：与 Vue 开发者工具风格一致。`School`
3. 多个单词用 - 隔开`'my-school': school`：注册时要用引号包裹，在开发者工具中以大驼峰的形式显示。`MySchool`
4. 多个单词首字母大写`MySchool: school`：只有在使用脚手架的时候才能这样命名，否则组件无效。`MySchool`

## 组件嵌套

可以在组件上使用另一个组件，但是在非单个文件时注意书写顺序，需要用的组件要先创建。

```HTML
<body>
	<div id="app">
		<h1>大哥</h1>
		<hr>
		<!-- 3. 使用组件 -->
		<xuexiao></xuexiao>
		<hr>
		<student></student>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/vue/2.7.10/vue.js"></script>
<script>
	// 1. 创建组件
	// 组件中不用指定el，因为组件不用绑定死为谁服务，追随vm实例。
	// 组件中的数据必须以函数的形式赋值。

	const student = Vue.extend({
		template: `
		<div>
			<h2>student：{{name}}</h2>
		</div>
	`,
		data() {
			return {
				name: "brokyz"
			}
		}
	})

	const school = Vue.extend({
		// 书写组件的html
		name: "myschool",
		template: `
		<div>
			<h2>school：{{name}}</h2>
			<student></student>
		</div>
	`,
		data() {
			return {
				name: "首经贸"
			}
		},
		components: {
			student
		}
	})

	const vm = new Vue({
		// 2. 注册组件
		components: {
			xuexiao: school, // 可以注册名和组件名相同直接简写成 school
			student,
		},
	}).$mount("#app")
</script>
```

### app 组件

在开发中，会定义一个 app 组件，用于管理应用中的所有组件。可以将 app 理解为一人之下（root），万人之上（其他组件）。

```html
<body>
	<div id="root">
		<app></app>
	</div>
</body>
<script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/vue/2.7.10/vue.js"></script>
<script>
	// 1. 创建组件
	// 组件中不用指定el，因为组件不用绑定死为谁服务，追随vm实例。
	// 组件中的数据必须以函数的形式赋值。

	const student = Vue.extend({
		template: `
	<div>
		<h2>student：{{name}}</h2>
	</div>
`,
		data() {
			return {
				name: "brokyz"
			}
		}
	})

	const school = Vue.extend({
		// 书写组件的html
		name: "myschool",
		template: `
	<div>
		<h2>school：{{name}}</h2>
		<student></student>
	</div>
`,
		data() {
			return {
				name: "首经贸"
			}
		},
		components: {
			student
		}
	})

	const app = Vue.extend({
		template: `
			<div>
				<school></school>
			</div>
		`,
		components: {
			school
		}
	})

	const vm = new Vue({
		// 2. 注册组件
		components: {
			app
		},
	}).$mount("#root")
</script>
```

## VueComponent 构造函数

1. 创建组件的本质就是创建一个名为 VueComponent 的构造函数，这不是程序员自己定义的，而是由 `Vue.extend`帮我们生成的。

2. 我们只需要写`<school/>`或`<school><school/>`，Vue 解析时会帮我们创建 school 组件的实例对象，即 Vue 帮我们执行的：`new VueComponent(options)`。

3. 特别注意：每次调用 `Vue.extend`，返回的都是一个全新的 VueComponent！！！

4. 关于 this 的指向：

   1. 组件配置中：

      data函数、methos中的函数、watch中的函数、computed中的函数，它们的 this 全指向【VueComponent 实例对象】。

   2. new Vue(options) 中：

      data函数、methos中的函数、watch中的函数、computed中的函数，它们的 this 全指向【Vue 实例对象】。

5. VueComponent 的实例对象，也称为 vc 或 组件实例对象，Vue 实例对象，简称 vm。

6. `vm.$children`或`vc.$children`都存储了它们里面的组件。

## vm 与 vc 的内置关系

首先从表面上说，vm 需要 el，而且 data 可以是对象可以是函数。而 vc 没有 el，必须是函数 data。

然后观察其二者的原型关系：

![](https://pic.imgdb.cn/item/632f9a3f16f2c2beb1cc9181.jpg)

注意：实例对象的`__proto__`是隐式原型属性，构造函数的`prototype`式显示原型属性，二者都同时指向原型对象。一般我们用显示进行操作，隐式进行使用。

## 单文件组件

推荐 vscode 插件：vetur

### 组件 school.vue

```html
<template>
	<div class="demo">
		<h2>学校名称：{{ name }}</h2>
		<h2>学校地址：{{ address }}</h2>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				name: '首经贸',
				address: '北京市丰台区'
			};
		}
	};
</script>

<style>
	.demo {
		background-color: orange;
	}
</style>
```

**向外暴露组件：**

我们这里的单文件的组件是需要被其他人导入并且注册的，所以我们这里要向外暴露我们的 vc。

```js
// Vue 推荐的简写写法
export default {
	data() {
		return {
			name: '首经贸',
			address: '北京市丰台区'
		};
	}
};
// 上一种写完整
const school = Vue.extend({
    data() {
		return {
			name: '首经贸',
			address: '北京市丰台区'
		};
	}
})
export default school
```

### app.vue

```html
<template>
	<div>
		<School></School>
		<student></student>
	</div>
</template>

<script>
    // 引入组件
	import School from './school'
	export default {
		name: 'App',
		components: {
			School
		}
	};
</script>

<style></style>
```

### main.js

```js
import App from './App.vue'

new Vue({
	el: '#root',
	template: '<app></app>',
	components: {
		App
	},
})
```

### index.html

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="root"></div>
		<script src="https://cdn.jsdelivr.net/gh/brokyz/brokyzCDN/vue/2.7.10/vue.js"></script>
		<script src="./main.js"></script>
	</body>
</html>
```

- 引入 main.js 要在容器的下面才可以。
- 需要在这里引入 vue.js。

注意：这里都配置完成后，页面中并不能显示出来，需要在脚手架中才可以运行。

## VueCli 脚手架

脚手架可以为我们自动配置我们的前端工程化的开发环境，省去了我们使用 webpack 手动配置前端工程化开发环境的麻烦。

### 使用

**安装 VueCli**

```
npm install -g @vue/cli
```

**切换到要创建项目的目录，然后使用命令创建项目**

```
vue create projectNmae
```

**如果下载慢需要配置淘宝镜像**

```
npm config set registry https://registry.npm.taobao.org
```

**开发时编译代码**

```
npm run serve
```

### 分析脚手架结构

**配置文件：**

1. .gitignore：git 的忽略文件。
2. babel.config.js：babel 的配置文件，用于 es6 转 es5。
3. package.json：nodejs的配置文件。
4. package-lock.json：nodejs中的包的版本控制文件，里面存着报的版本信息。

**主题文件：**

1. src/main.js：vue 项目中一切的开端，整个项目的入口文件。文件中导入了 vue 和 app，创建了 vm 实例。
2. src/App.vue：项目中的一个汇总组件，所有的组件展示都汇总到这里，它是所有组件的父组件。然后再由 vm 实例调用 App 总组件进行页面展现。
3. src/components：用于存放组件。
4. src/assets：用于存放静态文件。
5. public/index.html：vm 实例最终渲染的页面，其中有元素被 vm 实例绑定。

### render 函数

我们认识脚手架之前的 vm 实例：

```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
	template: '<App></App>',
    components: {App}
}).$mount('#app')
```

上述这种 vm 实例的写法在脚手架中运行时会报错。因为在脚手架中导入的 vue 并不是完整路径，而是 vue 文件夹，其中包含了许多版本的 vue 文件，但是默认为我们导入的是 dist/vue.runtime.esm.js, 这是精简掉了模板解析器的 vue，并不是完成版的 vue.js。这个由于没有 模板解析器，所以无法解析 template 中的语法。

这时我们就可以使用 render 渲染函数：

```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
	render(createElement){
        return createElement('h1', '你好')
    }
}).$mount('#app')


// 简写
new Vue({
	render: h => h(App),
}).$mount('#app')
```

为什么 vue 会有精简版本？

​	vue 中有些东西是我们开发时用到的，这些东西上线时被 webpack 打包编译会生成 js。但是里面有些只有开发时会用的代码在上线时并不会被使用。所以 vue 为我们提供了一些精简版本的 vue。类似于 nodejs 中的 dependencies 和 devDependencies。

注意：

1. 我们所说的模板解析器是服务于的是在 vue.js 中用于创建 vm 实例时配置项里面的 template。
2. 而组件中的 tamplate 标签是我们 nodejs 中安装的 vue-template-compiler 包来进行解析的。

关于不同版本 vue 的区别：

1. vue.js 于 vue.runtime.xxx.js 的区别：
   1. vue.js 是完整版，包含了：核心功能+模板解析器。
   2. vue.runtime.xxx.js 是运行版的 Vue，只包含：核心功能，不包含模板解析器。

2. 因为 vue.runtime.xxx.js 没有模板解析器，所以不能使用 template 配置项，需要使用 render 函数接收到的 createElement 函数去指定具体内容。

# vue 插槽

## 概述

插槽就是组件内的一个预留区域，它允许使用者向组件内的预留区域来添加自己想要的内容。

比如我们已经写好了一个组件，然后以后可能会在指定区域插入广告，这时就可以先用插槽进行占位。如果使用者向插槽中传递广告时，这时广告才会显示出来。

插槽大概分为三种：默认插槽、具名插槽、作用域插槽。

插槽通过 `<slot></slot>`标签，在组件中占位。当标签内没有默认值时，如果使用者使用组件时，不插入自定义内容，那么插槽不会被渲染；如果使用者使用组件插入自定义内容时，插槽才进行渲染。如果插槽中有默认值时，如果插槽不被使用，则会输出默认值。

## 默认插槽

Skills.vue

```html
<template>
  <div id="skills">
    <h2 id="title">skills</h2>
    <hr />
    <ul>
      <li v-for="(skill, index) in skills" :key="index">{{ skill }}</li>
    </ul>
  	<!-- 默认插槽 -->
    <slot></slot>
  </div>
</template>

<script>
  export default {
    name: "Skills",
    props: ["skills"]
  }
</script>
```

App.vue

```html
<template>
  <div id="app">
    <div class="header">
      <Skills :skills="skills">
          // 组件标签内的所有内容将会被写到该组件中默认插槽的预留位置
        <h2>默认插槽</h2>
      </Skills>
    </div>
  </div>
</template>

<script>
  import Skills from "./components/Skills.vue"
  export default {
    name: "App",
    components: { Skills },
    data() {
      return {
        skills: ["唱", "跳", "rap", "篮球"]
      }
    }
  }
</script>
```

- 注意在书写样式时，结构在哪个文件，样式就书写到哪个文件中。
- 即使组件标签中的内容是要替换到组件插槽位置的，样式也要在组件标签所在的当前文件中书写。
- 因为组件标签中的内容是现在本文件渲染，然后才去替换组件插槽处的位置的。

## 具名插槽

当我们指定多个插槽时，就需要为插槽取名字了。因为默认插槽只能存在一个。

我们在使用插槽传入内容时，必须要使用 template 标签包裹内容，并指定要服务的具名插槽的名字。

Skills.vue

```html
<template>
  <div id="skills">
    <h2 id="title">skills</h2>
    <hr />
    <ul>
      <li v-for="(skill, index) in skills" :key="index">{{ skill }}</li>
    </ul>
    <!-- 默认插槽 -->
    <slot></slot>
    <!-- 具名插槽 -->
    <slot name="slot01"></slot>
  </div>
</template>

<script>
  export default {
    name: "Skills",
    props: ["skills"]
  }
</script>
```

App.vue

```html
<template>
  <div id="app">
    <div class="header">
      <!-- 默认插槽 -->
      <Skills :skills="skills">
        <h2>默认插槽</h2>
      </Skills>
      <!-- 具名插槽 -->
      <Skills :skills="skills">
        <template v-slot:slot01>
          <div>
            <h2>具名插槽</h2>
          </div>
        </template>
      </Skills>
    </div>
  </div>
</template>

<script>
  import Skills from "./components/Skills.vue"
  export default {
    name: "App",
    components: { Skills },
    data() {
      return {
        skills: ["唱", "跳", "rap", "篮球"]
      }
    }
  }
</script>
```

- 具名插槽在插槽标签中使用 name 来给插槽起名字，以达到区分的作用。
- 具名插槽在组件标签使用时其中的结构必须使用 `template` 标签包裹，并且在该标签中使用 `v-slot:xxxx`指定所使用的具名插槽的名字。可以简写成`#slot:xxx`。注意是冒号`:`不是等号`=`

## 作用域插槽

作用域插槽并不是一种单独的插槽，而是实现插槽向使用者传递数据的一种插槽的称呼。

所以默认插槽可以是作用域插槽，具名插槽也可以是作用域插槽。

Skills.vue

```html
<template>
  <div id="skills">
    <h2 id="title">skills</h2>
    <hr />
    <ul>
      <li v-for="(skill, index) in skills" :key="index">{{ skill }}</li>
    </ul>
    <!-- 默认插槽的作用域插槽 -->
    <slot :data="slotData"></slot>
    <!-- 具名插槽的作用域插槽 -->
    <slot name="slot01" :slotData="slotData"></slot>
  </div>
</template>

<script>
  export default {
    name: "Skills",
    props: ["skills"],
    data() {
      return {
        slotData: {
          content:"此数据由插槽传递给使用者"
        }
      }
    },
  }
</script>

```

App.vue

```html
<template>
  <div id="app">
    <div class="header">
      <!-- 默认插槽 -->
      <Skills :skills="skills">
        <h2>默认插槽</h2>
      </Skills>
      <!-- 具名插槽 -->
      <Skills :skills="skills">
        <template v-slot:slot01>
          <div>
            <h2>具名插槽</h2>
          </div>
        </template>
      </Skills>
      <!-- 具名插槽作用域插槽：接收插槽组件传给使用者的数据 -->
      <Skills :skills="skills">
        <template #slot01="scope">
          <div>
            <h2>作用域插槽</h2>
            <p>{{ scope }}</p>
            <p>{{ scope.slotData.content }}</p>
          </div>
        </template>
      </Skills>
      <!-- 默认插槽作用域插槽解构赋值 -->
      <Skills :skills="skills">
        <template scope="{data}">
          <p>{{ data }}</p>
          <p>{{ data.content }}</p>
        </template>
      </Skills>
    </div>
  </div>
</template>

<script>
  import Skills from "./components/Skills.vue"
  export default {
    name: "App",
    components: { Skills },
    data() {
      return {
        skills: ["唱", "跳", "rap", "篮球"]
      }
    }
  }
</script>
```

- 在插槽标签中，指定属性来向使用者传递值。
- 使用者可以在 template 标签中接收插槽传递的值。如果是具名插槽使用`#slot01="scope"`接收，scope 可自定义名字，为接收的数据对象。如果是默认插槽使用`scope="scope"`接收。其中可以参照上面代码使用 es6 的解构赋值。

# vue mixins 混入

## 概述

> 在 Vue 中我们的很多组件的配置都会有共用的数据，方法，钩子等等，我们可以把这些共用的部分提取到一个 js 文件中，并导入到需要使用的组件中，通过 mixins 配置项进行使用。

我们需要创建一个 js 文件，名字随意，这里我们叫做 mixin.js

```js
export const mixin {
    
}
```

之后在需要这个混入的组件中导入。

然后通过 mixins 配置项进行配置。



## 使用

minxi.js

```js
export const mixin = {
	data() {
		return {
			name: "mixin",
			age: "now"
		}
	},
	methods: {
		showName() {
			alert(this.name)
		}
	}
}

export const mixin2 = {
	mounted(){
		console.log("this is mixin2");
	}
}
```

TestMixins.vue

```html
<template>
	<div>
		<p>{{name}}</p>
		<p>{{age}}</p>
		<button @click="showName">mixin</button>
	</div>
</template>

<script>
	import {
		mixin,
		mixin2
	} from '../mixin.js'

	export default {
		data() {
			return {
				name: "brokyz"
			}
		},
		mixins: [mixin, mixin2]
	}
</script>
```

- 如果 mixin.js 中的配置与需要配置 mixins 的组件重合时，将以组件里面的配置为主。也就是说组件和 mixin 相同的未知，组件会将其覆盖。
- 在使用声明周期函数时，minxi 和 组件都设置了相同的钩子时， 两个都会调用，而且 mixin 中的会优先执行。

# Vue 插件

> Vue 的插件就是一个 js 文件，里面允许我们完成一些特定的功能。



## 使用

创建一个插件 plugins.js

```js
export default {
  install(Vue) {
    // 全局过滤器
    Vue.filter("mySlice", function (value) {
      return value.slice(0, 4)
    })

    // 全局指令
    Vue.directive("color", {
      // 指令与元素成功绑定时调用
      bind(element) {
        element.style.color = "red"
      },
      // 指令所在元素被插入页面时调用
      inserted(element) {
        element.style.color = "pink"
      },
      // 指令所在的模板被重新解析时调用
      update(element) {
        element.style.color = "skyblue"
      }
    })

    // 给 Vue 原型添加方法
    Vue.prototype.hello = () => {
      alert("你好啊")
    }
  }
}
```

- Vue 插件文件中需要有 install 方法，install 方法中的参数就是，Vue 构造函数，我们可以对构造函数进行操作。
- 我们可以 Vue 构造函数上添加一些全局的功能，然后一次性导入 Vue。

使用插件

在 main.js 中导入插件，并通过 Vue.use() 使用插件。

```js
// import Vue from "vue"
import Vue from "vue"
import App from "./App.vue"
// 引入插件
import plugins from './plugins'

// 关闭生产提示
Vue.config.productionTip = false

// 使用插件
Vue.use(plugins)

new Vue({
  render: (h) => h(App)
}).$mount("#app")
```

# Vue 动画与过渡

> Vue 为我们优化了 CSS 动画在 Vue 中的使用，帮助我们在使用动画的时候更加的得心应手。



## 使用 CSS 动画完成 Vue 动画

```html
<template>
  <div id="app">
    <button @click="isShow = !isShow">进入/离开</button>
    <transition name="hello" appear>
      <h1 v-show="isShow">你好</h1>
    </transition>
  </div>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        isShow: true
      }
    }
  }
</script>

<style scoped>
  h1 {
    background-color: pink;
  }

  .hello-enter-active {
    animation: brokyz 0.3s ease;
  }

  .hello-leave-active {
    animation: brokyz 0.3s ease reverse;
  }

  @keyframes brokyz {
    from {
      transform: translateY(10px);
			opacity: 0;
    }
    to {
      transform: translateY(0px);
			opacity: 1;
    }
  }
</style>
```

### transition 标签

在 Vue 中，我们需要将需要添加动画效果的元素使用`transition`标签进行包裹。

name 属性：用于控制标签中元素所使用动画的名字。比如如果指定 `name="hello"`，那么就会使用`.hello-xxxx`这个定义过的动画。如果什么都不指定，那默认使用`.v-xxxxx`。

appear 属性：用于控制页面加载时是否使用动画。如果为 true，则页面加载时使用动画。

transition 标签只能渲染一个元素，如果有其它元素需要写道一个 div 中，但是这样操作后就无法通过 v-if 等操作互斥显示。

transition-group 标签可以渲染多个标签，但是需要给指定 key 属性才可，否则报错。

### Vue 的动画 CSS 名

1. `.v-enter-active`：元素进入时的动画。
2. `.v-leave-active`：元素离开时的动画。

## 使用 CSS 过渡完成 Vue 动画

```html
<template>
  <div id="app">
    <button @click="isShow = !isShow">进入/离开</button>
    <transition name="hello" appear>
      <h1 v-show="isShow">你好</h1>
    </transition>
  </div>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        isShow: true
      }
    }
  }
</script>

<style scoped>
  h1 {
    background-color: pink;
    border-radius: 5px;
		/* 添加过渡效果 */
    transition: 0.5s ease;
  }
  /* 进入的起点  */
  .hello-enter,
  .hello-leave-to {
    transform: translateY(20px);
    opacity: 0;
  }
  /* 进入的终点 */
  .hello-enter-to,
  .hello-leave {
    transform: translateY(0);
    opacity: 1;
  }

  /* 由于进入的起点和离开的终点相同，进入的终点和离开的起点相同所以合并到一起 */
  /* .hello-leave {
    transform: translateY(0);
		opacity: 1;
  }
  .hello-leave-to {
    transform: translateY(20px);
		opacity: 0;
  } */
</style>
```

## 使用第三方动画

推荐使用 [animate.css](https://animate.style/)

安装

```
npm i animate.css
```

在需要使用的组件中导入动画库

```js
import 'animate.css'
```

之后根据，官网进行使用。如：

```html
<transition-group
      appear
      name="animate__animated animate__bounce"
      enter-active-class="animate__fadeInRight"
      leave-active-class="animate__fadeOutLeft"
    >
</transition-group>
```

# vue 组件间通讯

> 在 Vue 中一个项目中往往需要被拆分成多个组件，但是每个组件之间都会有相互访问数据的需求。这时就涉及到组件之间的通讯了。

## 使用 props 进行组件间的通讯

### 父向子通讯

这里让 App 组件向 School 组件传入一组数据，使用 props 进行数据接收。

App.vue

```html
<template>
	<div id="app">
		<h1>APP:</h1>
		<School :school="school" :detail="detail"></School>
	</div>
</template>

<script>
	import School from './components/School.vue'

	export default {
		name: 'App',
		components: {
			School
		},
		data() {
			return {
				school: {
					name: "首经贸",
					address: "北京丰台"
				}
			}
		},
	}
</script>
```

School.vue

```html
<template>
	<div id="school">
		<h2>学校： {{school.name}}</h2>
		<h2>地址： {{school.address}}</h2>
	</div>
</template>

<script>
	export default {
		props: ["school", "detail"]
	}
</script>
```

注意：

- 传入的数据由于使用了 props 接收，所以 Vue 中不允许对数据进行修改，否则将会报错。
- 但是由于我们传入的是对象类型，并不是字符串类型，我们对对象中的数据进行修改，Vue 将无法检测报错，但是并不推荐这么做。

### 子向父通讯

字向父通过 props 进行通讯的原理和父向子类似，只不过我们这里利用了一个小技巧。

我们在父组件中定义一个函数，在函数中完成对自己数据的修改。然后我们通过 props 将这个函数传递给子组件。

子组件接收到函数后，可以对函数进行调用，这样就完成了子组件向父组件的通讯。

App.vue

```html
<template>
	<div id="app">
		<h1>APP:</h1>
		<School :school="school" :changeName="changeName"></School>
	</div>
</template>

<script>
	import School from './components/School.vue'

	export default {
		name: 'App',
		components: {
			School
		},
		data() {
			return {
				school: {
					name: "首经贸",
					address: "北京丰台"
				}
			}
		},
		methods: {
			// 定义修改数据的函数
			changeName(name) {
				this.school.name = name;
			}
		}
	}
</script>
```

## 使用组件自定义事件进行子向父通讯

我们可以在父组件使用子组件的时候，在子组件标签上在子组件的 vc 上绑定组件自定义事件。

指定子组件触发事件时，调用父组件的方法。通过方法来达成通讯内容。

而在子组件这里，可以使用 vc.$emit 手动触发。

### 使用组件标签绑定组件自定义事件

App.vue

```html
<template>
	<div id="app">
		<h1>APP:</h1>
		<Student @getName="demo"></Student>
	</div>
</template>

<script>
	import Student from './components/Student.vue'

	export default {
		name: 'App',
		components: {
			Student
		},
		methods: {
			// 定义修改数据的函数
			changeName(name) {
				this.school.name = name;
			}
		},
	}
</script>
```

Student.vue

```html
<template>
	<div id="student">
		<h2>学生： {{name}}</h2>
		<h2>年龄： {{age}}</h2>
		<button @click="sendF">通讯</button>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				name: "brokyz",
				age: 21
			}
		},
		methods: {
			sendF() {
				this.$emit('getName', this.name)
			}
		}
	}
</script>
```

### 使用 $on 绑定组件自定义事件

这种方法更加灵活。

App.vue

```html
<template>
	<div id="app">
		<h1>APP:</h1>
		<!-- <Student @getName="demo"></Student> -->
		<Student ref="student"></Student>
	</div>
</template>

<script>
	import School from './components/School.vue'
	import Student from './components/Student.vue'

	export default {
		name: 'App',
		components: {
			Student
		}
		methods: {
        	// es6 以数组的形式接收剩余参数
			demo(name, ...params) {
				console.log('App得到了子组件的name：', name);
			}
		},
		mounted() {
			this.$refs.student.$on('getName', this.demo);
			// 指定只触发一次
			// this.$refs.student.$once('getName', this.demo);
		}
	}
</script> 
```

### 解绑组件自定义事件

当我们进行完通讯时，可以选择使用 vc 的 $off 解绑 vc 上面的自定义事件。

```html
<template>
	<div id="student">
		<h2>学生： {{name}}</h2>
		<h2>年龄： {{age}}</h2>
		<button @click="sendF">通讯</button>
		<button @click="unbind">解绑事件</button>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				name: "brokyz",
				age: 21
			}
		},
		methods: {
			sendF() {
				this.$emit('getName', this.name)
			},
			unbind() {
				// 解绑多个事件以数组的形式传入多个需要解绑的事件
				this.$off('getName')
				console.log("事件已解绑")
			}
		}
	}
</script>
```

### 关于组件自定义事件的注意点

- 如果使用 ref 和 $on 的方法进行自定义事件的时候，$on 里面的回调函数需要是箭头函数和钩子。如果使用了正常的回调函数声明，那么里面的 this 指向的是子组件 vc，并不是当前组件。因为这个是由 this.$refs.xxx 调用的。
- 如果在组件标签上添加 dom 原生的事件，那么组件都将其看成是自定义事件。需要添加修饰才能将其识别为原生 dom 事件。 `@click.native`
- 给谁绑定事件，就需要谁触发事件。

## 全局事件总线

全局事件总线可以完成**任意间组件通讯**。

我们通过一个组件外并且所有组件都可以访问到的拥有事件绑定 $on 和事件触发功能 $emit 的对象，通过在这个对象上进行事件的绑定与触发来达到组件间通讯的目的。我们管这个操作叫做**全局事件总线**。

![](https://pic1.imgdb.cn/item/63339cdf16f2c2beb1ae4c1b.jpg)

由于我们需要的对象，要有 $on 和 $emit 方法，所以可供我们选择的对象就只有 vm 和 vc 了。事实上这二者都可以完成。但是我们使用 vm 作为全局事件总线，将会更加方便。

我们确定了全局事件总线由 vm 担任后，我们需要让所有的组件都可以访问到全局事件总线才行。这时由于 vm 和 vc 之间的原型关系我们知道，在 vm 原型上绑定的属性可供全体组件读取。因此我们需要在 vm 的原型身上绑定一个全局事件总线，这个全局事件总线由 vm 担任，我们将其赋值为 $bus。

**下面是 test1 组件向 test2 组件通讯的实例**

main.js

```js
import Vue from "vue"
import App from "./App.vue"

Vue.config.productionTip = false

new Vue({
  render: (h) => h(App),
  beforeCreate() {
    Vue.prototype.$bus = this
  }
}).$mount("#app")
```

test1.vue

```html
<template>
  <div>
    <button @click="sendF">点击触发事件</button>
  </div>
</template>

<script>
  export default {
    methods: {
      sendF() {
          // 触发事件 demo，向回调函数传值 666
        this.$bus.$emit("demo", 666)
      }
    }
  }
</script>
```

test2.vue

```html
<script>
  export default {
    mounted() {
        // 绑定事件 demo 触发时执行回调函数
      this.$bus.$on("demo", (msg) => {
        console.log(msg)
      })
    },
    beforeDestroy() {
        // 销毁前解绑事件 demo
      this.$bus.$off("demo")
    }
  }
</script>
```

注意：

- 相对于自定义事件，全局事件总线需要在组件销毁前解绑。因为自定义事件中是绑定在当前组件的，组件销毁时自定义事件自然销毁。而全局事件总线式绑定在 vm 的原型对象上的，如果组件销毁，事件依然存在。因此需要我们手动去解绑。

## 消息订阅与发布

消息订阅与发布需要借助第三方库，我们在这里推荐 pubSub-js。

```
npm i pubsub-js
```

我们使用时可以只在需要的组件中导入，也可以直接导入到 vm.prototype 中全局使用。

下面同时使用了全局事件总线和消息订阅与发布，可以对比二者。

main.js

```js
import Vue from "vue"
import App from "./App.vue"
import pubsub from "pubsub-js"

// 将 pubsub 添加到全局
Vue.prototype.ps = pubsub

Vue.config.productionTip = false

new Vue({
  render: (h) => h(App),
  beforeCreate() {
      // 为全局添加全局事件总线
    Vue.prototype.$bus = this
  }
}).$mount("#app")
```

test1.vue （发布者）

```html
<template>
  <div>
    <button @click="sendF">点击触发事件</button>
  </div>
</template>

<script>
  export default {
    methods: {
      sendF() {
          // 全局事件总线触发 demo 事件，并向回调函数传入 666
        this.$bus.$emit("demo", 666)
          // 发布消息，并向订阅了 hello 的人传递 777
        this.ps.publish("hello", 777)
      }
    }
  }
</script>
```

test2.vue (订阅者)

```html
<script>
  export default {
    mounted() {
        // 绑定事件 demo 与 事件被触发时的回调函数
      this.$bus.$on("demo", (msg) => {
        console.log(msg)
      })
        // 订阅消息 hello，并指定接收到发布消息的回调函数
      this.psId = this.ps.subscribe("hello", (msgName, msg) => {
        console.log(msgName, msg)
      })
    },
    beforeDestroy() {
        // 解绑消息
      this.$bus.$off("demo")
        // 取消订阅
      this.ps.unsubscribe(this.psId)
    }
  }
</script>
```

注意：

- 订阅消息时，传入回调函数的形参有两个，第一个是消息名，第二个才是传递过来的消息。如果不需要第一个参数，可以使用下划线占位。`(_, msg)`
- 订阅消息时，会返回一个唯一的参数，需要接收这个参数。以便于在取消订阅的时候使用。
- 对比来看，消息订阅与发布和全局事件总线十分相似。

## Vuex

一个用于管理 Vue 中所有组件数据的插件，可以很轻松的实现组件间的通讯。

安装 Vuex：

如果在 Vue2 上需要安装 vuex3，因为最新的 vuex4 只支持 vue3.

```
npm i vuex@3
npm i vuex
```

### vuex 配置

我们需要在 src 目录下，新建 store 文件夹，在文件夹中新建 index.js。

index.js 中就是 vuex 的相关配置。

demo：index.js

```js
// 该文件用于创建Vuex中最为核心的store
import Vue from "vue"
// 引入Vuex
import Vuex from "vuex"

// 准备actions——用于响应组件中的动作
const actions = {
}

// 准备mutations——用于操作数据（state）
const mutations = {
}

// 准备state——用于存储数据
const state = {
}

// 令 Vue 实例使用插件 Vuex，注意，此步骤必须在创建 store 实例之前。
Vue.use(Vuex)

// 创建并暴露 store
export default new Vuex.Store({
  actions,
  mutations,
  state
})
```

注意：

我们在使用插件时，在 index.js 中，这样才可以保证使用 Vuex 插件的顺序在创建 store 实例之前。

如果我们在 main.js 中引用时，所有的 import 都会存在变量提升，所以不可能使得 use(Vuex) 在创建 store 实例之前。

main.js

```js
// 引入 Vue
import Vue from 'vue'
// 引入 App
import App from './App.vue'
// 引入 store
import store from './store'

// 关闭 Vue 的生产提示
Vue.config.productionTip = false

// 创建 vm
new Vue({
    el: "#app",
    // 将配置文件引入 vm 与 vc
    store
})
```

### vuex 的使用

![](https://pic1.imgdb.cn/item/633464f916f2c2beb18150dc.jpg)

**原理：**

经过以上配置之后，vc 上将会存在 $store 属性，我们可以通过 `this.$store`来使用 vuex。

在组件上，我们可以通过`this.$store.dispatch(name, value)`来将调用 actions 中的函数，并向内传递 value。

在 actions 中，函数接收两个参数，如`jia(context, value){}`其中 context 为上下文，是一个对象，里面有 dispatch、commit 方法；value 是接收到的值。一般来说，此方法多用于，对传入的数据进行一些处理或者逻辑判断。调用 context 中的 commit 方法将数据发送给 mutations，如`context.commit(name, value)`

在 mutations 中，函数也会接收两个参数，`jia(state, value){}`，其中 state 为存储的数据的数据代理对象。可以从过它，对数据进行操作。数据变更时，vue 也会进行渲染。

在 getters 中，可以让我们返回经过我们加工的数据。有点类似于计算属性，但是计算属性只能在当前组件使用，getters 可以配置在 vuex 中，所有组件调用时都是 getters 中加工后的数据。

当我们使用到 vuex 中的数据时，可以使用`this.$store.state.xxx`来读取数据。



至此 vuex 的通信流程结束。

**demo：**

index.js

```js
// 该文件用于创建Vuex中最为核心的store
import Vue from "vue"
// 引入Vuex
import Vuex from "vuex"
// 准备actions——用于响应组件中的动作
const actions = {
  jia(context, value) {
    console.log(context)
    context.commit("jia", value)
  },
  jian(c, value) {
    c.commit("jian", value)
  }
}
// 准备mutations——用于操作数据（state）
const mutations = {
  jia(state, value) {
    state.sum += value
  },
  jian(s, value) {
    console.log(s)
    s.sum -= value
  }
}
// getters 返回数据加工
const getters = {
  bigSum(state) {
    return state.sum * 10
  }
}
// 准备state——用于存储数据
const state = {
  sum: 0
}
Vue.use(Vuex)
// 创建并暴露store
export default new Vuex.Store({
  actions,
  mutations,
  state,
  getters
})

```

Count.vue

```html
<template>
  <div>
    <h2>当前求和为: {{ $store.state.sum }}</h2>
    <h2>当前求和×10: {{ $store.getters.bigSum }}</h2>
    <select v-model.number="n">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrease">-</button>
    <button @click="oddPlus">当前求和为奇数再加</button>
    <button @click="waitJia">等一等再加</button>
  </div>
</template>

<script>
  export default {
    name: "Count",
    data() {
      return {
        n: 1
      }
    },
    methods: {
      increment() {
        this.$store.dispatch("jia", this.n)
      },
      decrease() {
        this.$store.commit("jian", this.n)
      },
      oddPlus() {
          // 这些逻辑判断应该写在 actions 中
        if (this.$store.state.sum % 2 != 0) {
          this.increment()
          console.log(this.n % 2);
        }
      },
      waitJia() {
        setTimeout(() => {
          this.increment()
        }, 1000)
      }
    }
  }
</script>
```

### vuex 代码优化

#### mapState & mapGetters

在使用 vuex 时，我们如果要访问 vuex 中的数据，我们会使用标准写法`this.$store.state.xxx`。

但是如果我们调用的数据过多，就需要书写很多一大长串的前缀去调用。

vuex 考虑到了这种不便，为我们 mapSate 和 mapGetters 方法，让我们去优化代码。

```html
<template>
  <div>
    <h2>{{ bigSum }}</h2>
    <p>{{ sum }},{{ test }}</p>
  </div>
</template>

<script>
    // 引入 mapState 和 mapGetters
  import { mapState, mapGetters } from "vuex"
  export default {
    name: "Count",
    computed: {
      // ...mapState({sum1:"sum",sum2:"sum"})
      // 同名简写
      ...mapState(["sum", "test"]),
      ...mapGetters(["bigSum"])
    }
  }
</script>
```

#### mapActions & mapMutations

帮助我们简写方法

```html
<template>
  <div>
    <h2>当前求和为: {{ sum }}</h2>
    <h2>当前求和×10: {{ bigSum }}</h2>
    <select v-model.number="n">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="jia(n)">+</button>
    <button @click="jian(n)">-</button>
    <button @click="oddPlus">当前求和为奇数再加</button>
    <button @click="waitJia">等一等再加</button>
    <p>{{ sum }},{{ test }}</p>
  </div>
</template>

<script>
  import { mapState, mapGetters, mapActions, mapMutations } from "vuex"
  export default {
    name: "Count",
    data() {
      return {
        n: 1
      }
    },
    computed: {
      // ...mapState({sum1:"sum",sum2:"sum"})
      // 同名简写
      ...mapState(["sum", "test"]),
      ...mapGetters(["bigSum"])
    },
    methods: {
      // ...mapActions({increment:"jia"}),
      // ...mapMutations({decrease:"jian"}),
      // 同名简写
      ...mapActions(["jia"]),
      ...mapMutations(["jian"]),
      // increment() {
      //   this.$store.dispatch("jia", this.n)
      //   // console.log(this);
      // },
      // decrease() {
      //   this.$store.commit("jian", this.n)
      // },
      oddPlus() {
        if (this.$store.state.sum % 2 != 0) {
          this.jia(this.n)
          console.log(this.n % 2)
        }
      },
      waitJia() {
        setTimeout(() => {
          this.jia(this.n)
        }, 1000)
      }
    }
  }
</script>
```

#### vuex 模块化

在 vuex 中不同的功能之间可以分类，将他们模块化。

index.js

```js
// 该文件用于创建Vuex中最为核心的store
import Vue from "vue"
// 引入Vuex
import Vuex from "vuex"

// 求和相关的配置
const countOptions = {
  namespaced: true,
  actions: {},
  mutations: {},
  getters: {},
  state: {}
}

// 人员相关的配置
const personOptions = {
  namespaced: true,
  actions: {},
  mutations: {},
  getters: {},
  state: {}
}

Vue.use(Vuex)

export default new Vuex.Store({
  modules: {
    countAbout: countOptions,
    personAbout: personOptions
  }
})
```

Count.vue

```html
<template>
  <div>
    <h2>{{ bigSum }}</h2>
    <p>{{ sum }},{{ test }}</p>
  </div>
</template>

<script>
    // 引入 mapState 和 mapGetters
  import { mapState, mapGetters } from "vuex"
  export default {
    name: "Count",
    computed: {
        // 使用时通过 countAbout.xxx
      // ...mapState(["countAbout","personAbout"]),
        // 需要开启命名空间, 这样可以直接使用 xxx 不用通过 countAbout.xxx
        ...mapState("countAbout", ["xxx","xxx"]),
    },
    methods: {
        add(){
            // 使用模块化时，需要指定模块用/分割
            this.$store.commit('personAbout/addPerson', 666)
        }
    }
  }
</script>
```

## 数据包装

> 我们要进行传值时，有时需要传递的是对象，那么就需要将我们的数据包装成对象的形式传入。

对于唯一 id 的处理，我们可以使用 nanoid 包

```
npm i nanoid
```

数据包装:

```js
import {nanoid} from 'nanodid'

const name = "brokyz"
const age = 21
const obj = {id:nanoid(), name:name, age:age}
```

# Vue 路由

## 基本使用

### 安装

```
npm i vue-router@3
```

### 配置

1. 在 src 目录下生成 router/index.js 用于存放 router 的配置。

router/index.js

```js
// 引入 vue-router
import VueRouter from "vue-router"
// 引入相关组件
import About from "../components/About.vue"
import Home from "../components/Home.vue"

export default new VueRouter({
  routes: [
    { path: "/", redirect: "/about" },
    { path: "/about", component: About },
    { path: "/home", component: Home }
  ]
})
```

2. 在 main.js 中引入 router。

```js
// import Vue from "vue"
import Vue from "vue"
import App from "./App.vue"
// 引入 vue-router
import VueRouter from "vue-router"
// 引入配置好的 router
import router from "./router"

// 关闭生产提示
Vue.config.productionTip = false

// 使用插件
Vue.use(VueRouter)

new Vue({
  render: (h) => h(App),
  // 添加 router
  router
}).$mount("#app")
```

### 使用

在 router 的配置文件中设置组件的路由配置。

设置好后，在组件中将需要跳转的 a 链接使用 vue-router 的 router-link 代替。然后再需要显示组件视图的地方使用 router-veiw 标签。

以下实例使用了 bootstrap 3.3.5

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.5/dist/css/bootstrap.min.css" rel="stylesheet" />
```

App.vue

```html
<template>
  <div id="app">
    <div class="row">
      <div class="col-xs-offset-2 col-xs-8">
        <div class="page-header"><h2>Vue Router Demo</h2></div>
      </div>
    </div>
    <div class="row">
      <div class="col-xs-2 col-xs-offset-2">
        <div class="list-group">
          <!-- <a class="list-group-item active" href="./about.html">About</a>
          <a class="list-group-item" href="./home.html">Home</a> -->
          <router-link class="list-group-item" active-class="active" to="/about">About</router-link>
          <router-link class="list-group-item" active-class="active" to="/home">Home</router-link>
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "App"
  }
</script>
```

- `<router-link class="list-group-item" active-class="active" to="/about">About</router-link>`用于代替 a 链接。
  - 其中 `active-class="active"`表示，处于激活选中的链接使用`.active`样式。
  - `to="/about"`表示点击后需要路由到的地址。其中的`/`要注意携带。
- `<router-view></router-view>`用于展示路由之后的组件。组件将显示在此处。
- 使用 router 来调用组件时不需要手动引入组件，并再 components 中配置。因为再路由的配置文件 index.js 中已经引入过相关组件了。

## 深入解析

**路由组件与一般组件**

由路由调用显示的组件我们叫做**路由组件**，一般放置于 pages 文件夹。

正常引入由标签手动调用的组件叫做**一般组件**，一般放置于 components 文件夹。

**组件切换**

组件进行切换时，其他组件就被 router 销毁了。之后我们可以使用 keep-alive 来让其不销毁。

我们通过操作钩子就可以轻松验证。

Home.vue

```html
<template>
  <h2>我是Home的内容</h2>
</template>

<script>
export default {
  name:"Home",
  mounted() {
    console.log("home 组件被挂载");
  },
  destroyed() {
    console.log("home 组件被销毁");
  },
}
</script>
```

**vc 实例对象分析**

我们使用了 vue-router 之后，我们的 vc 实例对象自身就多了 $route 和 $router 两个属性。

$route 里面存储着路由规则。里面存储了当前组件的一些路由信息，是当前组件独有的。

$router 是整个应用的路由器，整个应用只有一个，所以不同 vc 身上的 $router 都是同一个路由器。

## 嵌套路由

嵌套路由需要在路由的配置中，在需要嵌套的对象身上继续套娃添加 children 属性，以数组形式嵌套。

router/index.js

```js
// 引入 vue-router
import VueRouter from "vue-router"
// 引入相关组件
import About from "../pages/About.vue"
import Home from "../pages/Home.vue"
import News from "../pages/News.vue"
import Message from "../pages/Message.vue"

export default new VueRouter({
  routes: [
    { path: "/", redirect: "/about" },
    { path: "/about", component: About },
    {
      path: "/home",
      component: Home,
      children: [
        { path: "", redirect: "news" },
        { path: "news", component: News },
        { path: "message", component: Message }
      ]
    }
  ]
})
```

- 在嵌套路由 children 中配置 path 时，router 会自动为我们加上`/`所以不需要我们去手动添加，如果添加后，如`path="/news"`，那么进行路由到组件的时候识别的是`//news`，就不会将 news 组件显示出来。
- 一般来说可以多层嵌套，但是实际中嵌套到五六层就已经很少见了。

home.vue

```html
<template>
  <div>
    <h2>我是Home的内容</h2>
    <ul class="nav nav-tabs">
      <li>
        <!-- <a class="list-group-item" href="./home-news.html">News</a> -->
        <router-link
          class="list-group-item"
          active-class="active"
          to="./news"
          >News</router-link
        >
      </li>
      <li>
        <router-link
          class="list-group-item"
          active-class="active"
          to="/home/message"
          >Message</router-link
        >
      </li>
    </ul>
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: "Home"
  }
</script>

```

- 嵌套路由在组件中使用方法和一层的时候类似。
- 对于 to 属性，我们在这里需要将完整地址写上如`to="/home/message"`，因为再配置文件中是嵌套配置的，需要先去找外层的 home，然后再去找内层 message，所以这里的 to 要写全。也可以使用`./`简写，如`to="./message"`，这里的`./`就已经代表了`/home`，所以也是写全了的。

## 命名路由

为了规避过长的 path 产生的不便，router 允许我们为路由进行命名。

这里仅部分举例 index.js

```js
export default new VueRouter({
  routes: [
    { path: "/", redirect: "/about" },
    { name: "guanyu", path: "/about", component: About },
    {
      name: "jia",
      path: "/home",
      component: Home,
      children: [
        { path: "", redirect: "news" },
        { name: "xinwen", path: "news", component: News },
        { name: "xinxi", path: "message", component: Message }
      ]
    }
  ]
})
```

在 Home.vue 中使用

```html
<router-link
          class="list-group-item"
          active-class="active"
          :to="{
             name:'xinwen'
          }"
>News</router-link>
```

- 如果使用 name 进行跳转时，需要给 to 传递一个包含 name 属性的对象。注意需要使用`:to`，因为这样传递的才是表达式，否则全部视为字符串。
- 这里的 `:to="{name:'xinwen'}"`与`to="/home/news"`和`:to="{path:'/home/news'}"`等价。

## 路由间传递值

在使用 vue-router 之后，vc 上会存在 $route 属性，来携带自身的路由信息。其中就有专门用于传递值的 query 和 params。

### 使用 query 传值

query 传值可以直接写在 router-link 标签的 to 上，也可以以对象的形式赋值给 to。

Message.vue

```html
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- <a href="/message1">{{ m.title }}</a> -->
        <!-- <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{
        m.title
      }}</router-link> -->
        <router-link
          :to="{
            path: '/home/message/detail',
            query: { id: m.id, title: m.title }
          }"
          >{{ m.title }}</router-link
        >
      </li>
    </ul>
    <hr />
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: "Message",
    data() {
      return {
        messageList: [
          { id: "001", title: "消息001" },
          { id: "002", title: "消息002" },
          { id: "003", title: "消息003" }
        ]
      }
    }
  }
</script>
```

- 如果需要在字符串中传递变量可以使用模板字符串，在需要的位置使用`${变量}`直接进行拼接。

- ```
  :to="`/home/message/detail?id=${m.id}&title=${m.title}`"
  :to="{
              path: '/home/message/detail',
              query: { id: m.id, title: m.title }
  }"
  上面二者等价
  ```

- 在路由组件中使用`this.$route.query.xxx`进行接收。

### 使用 params 传值

使用 params 传值，需要在路由配置中配置需要传递的值。

index.js

```js
export default new VueRouter({
  routes: [
    { path: "/", redirect: "/about" },
    { name: "guanyu", path: "/about", component: About },
    {
      name: "jia",
      path: "/home",
      component: Home,
      children: [
        { path: "", redirect: "news" },
        { name: "xinwen", path: "news", component: News },
        {
          name: "xinxi",
          path: "message",
          component: Message,
          children: [
              // 需要在此配置 params 需要传递的值
            { name: "xiangqing", path: "detail/:id/:title", component: Detail }
          ]
        }
      ]
    }
  ]
})
```

Message.vue

```html
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- <a href="/message1">{{ m.title }}</a> -->
        <!-- <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{
        m.title
      }}</router-link> -->
        <router-link
          :to="{
            name: 'xiangqing',
            params: { id: m.id, title: m.title }
          }"
          >{{ m.title }}</router-link
        >
      </li>
    </ul>
    <hr />
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: "Message",
    data() {
      return {
        messageList: [
          { id: "001", title: "消息001" },
          { id: "002", title: "消息002" },
          { id: "003", title: "消息003" }
        ]
      }
    }
  }
</script>
```

- 如果不适用对象形式给 to 传递，直接以 /xxx/xxx 在地址后拼接需要传递的值。
- 如果使用 to 的对象形式，那么**使用 params 传递值时，不能使用 path 指定路由地址，必须使用 name 来指定路由地址**。
- 路由组件接收值时需要使用`this.$route.params.xxx`进行接收。

### 路由的 props 配置

在 router 中配置 props 选项，允许路由组件由 props 配置项直接读取到路由传递的 params 值，这种写法就避免了调用一长串`this.$route.params.xxx`来获取到传递值。

**只能用来传递 params 值。**

index.js 部分

```js
export default new VueRouter({
  routes: [
    { path: "/", redirect: "/about" },
    { name: "guanyu", path: "/about", component: About },
    {
      path: "/home",
      component: Home,
      children: [
        { path: "", redirect: "news" },
        { name: "xinwen", path: "news", component: News },
        {
          name: "xinxi",
          path: "message",
          component: Message,
          children: [
            {
              name: "xiangqing",
              path: "detail/:id/:title",
              component: Detail,
              // 第一种 props 写法，在配置中直接传递确定值，不需要在 path 配置中占位
              // props: { a: 1, b: 2 }
              // 第二中 props 写法，默认将组件中 to 的 params 以 props 形式传递给子组件, 不会理会 query, 需要在 path 配置中占位
              // props: true
              // 第三种 既可以传递 query 也可以传递 params, 如果是 params 需要在 path 配置中占位
              // props($route) {
              //   return {
              //     id: $route.params.id,
              //     title: $route.params.title
              //   }
              // }
              // 解构赋值
              props({ params: { id, title } }) {
                return {
                  id,
                  title
                }
              }
              // props({ query: { id, title } }) {
              //   return {
              //     id,
              //     title
              //   }
              // }
            }
          ]
        }
      ]
    }
  ]
})
```

Message.vue

```html
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- <a href="/message1">{{ m.title }}</a> -->
        <!-- <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{
        m.title
      }}</router-link> -->
        <router-link
          :to="{
            name: 'xiangqing',
            params: { id: m.id, title: m.title }
          }"
          >{{ m.title }}</router-link
        >
      </li>
    </ul>
    <hr />
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: "Message",
    data() {
      return {
        messageList: [
          { id: "001", title: "消息001" },
          { id: "002", title: "消息002" },
          { id: "003", title: "消息003" }
        ]
      }
    }
  }
</script>
```

Detail.vue

```html
<template>
  <ul>
    <li>消息编号:{{ id }}</li>
    <li>消息标题:{{ title }}</li>
    <!-- <p>a: {{ a }}, b: {{ b }}</p> -->
  </ul>
</template>

<script>
  export default {
    name: "Detail",
    props: ["id", "title"]
  }
</script>
```

- 使用 props 传值，不管是哪种，都需要在子组件中使用 props 接收。

## 编程式路由导航

### 路由历史记录

**push**

在浏览器进行路由跳转时，浏览器保留每一条历史记录，返回时按跳转顺序返回。

使用 router-link 进行跳转时，默认使用的就是使用的 push，可以在标签属性中添加 replace 来指定使用 replace。如`<router-link replace></router-link>`

**replace**

在浏览器进行路由跳转时，替换当前的历史记录。比如从 `/home`，跳转到`/home/news`时，会将`/home`历史记录替换掉。

### 编程式路由跳转

编程式路由跳转就是用 js 代码进行跳转。

跳转时我们用的是路由器 $router。

Message.vue

```html
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
        <!-- <a href="/message1">{{ m.title }}</a> -->
        <!-- <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{
        m.title
      }}</router-link> -->
        <router-link
          :to="{
            name: 'xiangqing',
            params: { id: m.id, title: m.title }
          }"
          >{{ m.title }}</router-link
        >
        <button @click="pushShow(m)">push</button>
        <button @click="replaceShow(m)">replace</button>
      </li>
    </ul>
    <hr />
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: "Message",
    data() {
      return {
        messageList: [
          { id: "001", title: "消息001" },
          { id: "002", title: "消息002" },
          { id: "003", title: "消息003" }
        ]
      }
    },
    methods: {
      pushShow(m) {
        this.$router.push({
          name: "xiangqing",
          params: { id: m.id, title: m.title }
        })
      },
      replaceShow(m) {
        this.$router.replace({
          name: "xiangqing",
          params: { id: m.id, title: m.title }
        })
      },
      back() {
        this.$router.back()
      },
      forward() {
        this.$router.forward()
      }
    }
  }
</script>
```

- `$router.push()`保留历史路由跳转。
- `$router.replace()`替换上一个历史路由跳转。
- `$router.back()`后退到上一个页面历史。
- `$router.forward()`前进到下一个页面历史。

## 缓存路由组件

我们进行路由跳转时，有时需要保留数据。但是路由跳转后默认会销毁组件。这时就需要用到我们的缓存路由组件了。

缓存路由组件只需要在 router-view 外层包裹 keep-alive 标签即可

```html
<template>
  <div>
    <h2>我是Home的内容</h2>
    <ul class="nav nav-tabs">
      <li>
        <!-- <a class="list-group-item" href="./home-news.html">News</a> -->
        <router-link class="list-group-item" active-class="active" to="./news"
          >News</router-link
        >
      </li>
      <li>
        <router-link
          class="list-group-item"
          active-class="active"
          to="/home/message"
          >Message</router-link
        >
      </li>
    </ul>
      // 缓存路由组件，include指定被缓存的组件名。
    <keep-alive include="News">
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
  export default {
    name: "Home",
    destroyed() {
      console.log("home 组件被销毁")
    }
  }
</script>
```

- 如果 keep-alive 不指定 include 组件，那么所有路由组件都将进行缓存。如果指定了只有指定组件进行缓存。
- 在 include 中指定的是**组件的名字**。
- 指定多个组件需要用数组的形式如：

```html
<keep-alive :include="['News','Message']">
      <router-view></router-view>
</keep-alive>
```

## 新增钩子

- `activated(){}`：组件被激活时调用。
- `deactivated(){}`：组件失活时调用。
- 另一个声明周期图外的钩子是`$nextTick(() => {})`，作用是等当前组件渲染完毕再调用里面的代码。

## 路由守卫

再路由进行跳转时，守卫可以为我们再路由跳转前或路由跳转后做相应的逻辑判断。

### 全局前置守卫

全局前置守卫声明再路由的配置文件中，当初始化的时候被调用，每次路由切换之前也被调用。

```js
const router = new VueRouter({
  routes: [...]
})
// 全局前置守卫
router.beforeEach((to, from, next) => {
  if (to.path === "/about") {
    alert("不允许访问about页面")
  } else {
    next()
  }
})

export default router
```

- 需要再暴露之前设置全局前置守卫。

### 全局后置守卫

```js
// 引入 vue-router
import VueRouter from "vue-router"
// 引入相关组件
import About from "../pages/About.vue"
import Home from "../pages/Home.vue"
import News from "../pages/News.vue"
import Message from "../pages/Message.vue"
import Detail from "../pages/Detail.vue"

const router = new VueRouter({
  routes: [
    { path: "/", redirect: "/about" },
    {
      name: "guanyu",
      path: "/about",
      component: About,
      meta: { title: "About" }
    },
    {
      path: "/home",
      component: Home,
      meta: { title: "Home" },
      children: [
        { path: "", redirect: "news" },
        {
          name: "xinwen",
          path: "news",
          component: News,
          meta: { title: "News" }
        },
        {
          name: "xinxi",
          path: "message",
          component: Message,
          meta: { title: "Message" },
          children: [
            {
              name: "xiangqing",
              path: "detail/:id/:title",
              component: Detail,
              // 解构赋值
              props({ params: { id, title } }) {
                return {
                  id,
                  title
                }
              }

            }
          ]
        }
      ]
    }
  ]
})
// 全局前置守卫
router.beforeEach((to, from, next) => {
  if (to.path === "/about") {
    alert("不允许访问about页面")
  } else {
    next()
  }
})
// 全局后置守卫
router.afterEach((to, from) => {
  // 更改标题
  document.title = to.meta.title || "路由demo"
})

export default router
```

### 独享路由守卫

独享前置守卫需要写在路由配置中

```js
{
          name: "xinwen",
          path: "news",
          component: News,
          meta: { title: "News" },
          // 独享前置守卫
          beforeEnter: (to, from, next) => {
            let pass = prompt("请输入2查看")
            if (pass == 2) next()
          }
}
```

### 组件内路由守卫

- beforeRouteEnter(to, from, next){}：通过路由规则，进入该组件前被调用。有人来到这个组件时调用。next 允许进入。
- beforeRouteLeave(to, from, next){}：通过路由规则，离开该组件时被调用。有人离开这个组件时调用。next 允许离开

```html
<template>
  <h2>我是About的内容</h2>
</template>

<script>
  export default {
    name: "About",
    beforeRouteEnter(to, from, next) {
      if (prompt("输入2进入about") == 2) next()
    },
    beforeRouteLeave(to, from, next) {
      if (prompt("输入3离开about") == 3) next()
    }
  }
</script>
```

- 由于都是进入这个组件，所以 to.path 永远都是当前组件的路由地址。

## history 模式与 hash 模式

**hash 模式**（哈希）

在 http 地址中 # 后面的值全都是哈希值，请求时并不会作为路径的一部传递给服务器。

**history 模式**（工厂）

在 http 中没有 #。

**区别**

1. hash 模式的兼容性更好，history 模式兼容性略差。
2. history 模式当项目部署到服务器时，刷新页面时会出错。

**history 模式的问题**

当我们打包后，将项目部署到服务器。如果我们在页面内实现跳转时，是 vue 的路由器帮我们跳转查找面，并不是服务器帮我们查找。路由器查找的同时，将我们的 url 替换成查找地址。比如`localhost:8888/home/message`。

但是这时我们进行页面刷新时，并不是 vue 的路由器帮我们查找页面，我们会直接把 url 中的内容发送给服务器，进行请求。但是由于我们时单页面应用，并没有相应的文件。所以就会出现报错。

**解决 history 模式问题**

1. 我们使用 hash 模式时可以解决此问题，因为 # 后面的东西不会发送给服务器。
2. 我们在后端中使用响应的中间件来解决此问题。在 node.js 中我们可以使用 connect-history-api-fallback 中间件来解决此问题。

**在 router 修改路由模式**

```js
export default new VueRouter({
  mode:"history",
  routes: [
    { path: "/", redirect: "/about" },
    { name: "guanyu", path: "/about", component: About },
    {
      name: "jia",
      path: "/home",
      component: Home,
      children: [
        { path: "", redirect: "news" },
        { name: "xinwen", path: "news", component: News },
        {
          name: "xinxi",
          path: "message",
          component: Message,
          children: [
              // 需要在此配置 params 需要传递的值
            { name: "xiangqing", path: "detail/:id/:title", component: Detail }
          ]
        }
      ]
    }
  ]
})
```

- 路由模式默认为 hash 模式。
