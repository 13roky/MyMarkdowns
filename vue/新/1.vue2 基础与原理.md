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

