# JavaScript Promise异步编程

## 概述

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了`Promise`对象。

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

`Promise`对象有以下两个特点。

（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

注意，为了行文方便，本章后面的`resolved`统一只指`fulfilled`状态，不包含`rejected`状态。

有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口，使得控制异步操作更加容易。

`Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用 [Stream](https://nodejs.org/api/stream.html) 模式是比部署`Promise`更好的选择。

## Promise相关概念

### Promise状态

Promise的状态是Promise实例对象的一个属性[PromiseState]。

- pending 未决定的，promise的初始状态
- resolved / fullfilled 成功
- rejected 失败

promise的状态只能改变一次。

- 由pending改变为成功，成功结果一般称为value
- 由pending改变为失败，失败结果一般称为reason

### Promise对象的值

promise对象的值是promise实例对象的一个属性[PromiseResult]。

保存了异步任务成功或失败的结果。

只有 resolve/reject 函数可以对该属性进行修改。

## Promise的工作流程

![](https://pic.imgdb.cn/item/6329178e16f2c2beb155477a.jpg)

## Promise相关api

### Promise 构造函数

```js
new Promise(excutor){}
```

- excutor 函数：执行器 `(resolve, reject) => {}`

- resolve 函数：内部定义成功时我们调用的函数 `value => {}`
- reject 函数：内部定义失败时我们调用的函数 `reason => {}`

说明：executor 会在 Promise 内部立即同步调用， 异步操作在执行器中执行。

### Promise.prototype.then

```js
p.then(onResolved, onRejected)
```

- onResolved 函数：成功的回调函数 `(value) => {}`
- onRejected 函数：失败的回调函数`(reason) => {}`

说明：不管成功还是失败，都会返回一个新的 Promise 对象。

### Promise.prototype.catch

```js
p.catch(onRejected)
```

- onRejected 函数：失败的回调函数`(reason) => {}`

### Promise.resolve

接收一个参数，并返回一个成功或失败的Promise对象。快速得到一个Promise对象。

```js
let p = Promise.resolve(521)
// 如果传入的参数为 非Promise类型的对象，则返回结果为成功的Promise对象
console.log(p)
// 如果传入的参数为 Promise对象，则参数的结果决定了resolve的结果
// 也就是说传入的Promise为成功，那么resolve的状态和结果与传入一样
// 传入的Promise为失败，那么resolve的状态和结果也与传入一样
```

### Promise.reject

返回一个失败的Promise

```js
let p = Promise.reject(521)
let p2 = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))
console.log(p2)
```

### Promise.all

传入几个Promise对象数组，返回一个新的Promise，只有所有传入的都成功返回的才成功，有一个失败返回的就失败。

```js
let p1 = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))
let p2 = Promise.resolve(521)
let p3 = Promise.reject(123)

const res = Promise.all([p1,p2,p3])
// 如果都成功，那么返回的Promise也成功，三个成功的值以数组的形式存于返回的Promise中
// 如果有一个失败，那么返回的Promise就是失败的，结果值就为失败的结果值
```

### Promise.race

传入一个n个Promise的数组，其返回结果由第一个改变状态的Promise来决定。

```js
let p1 = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))
let p2 = Promise.resolve(521)
let p3 = Promise.reject(123)

const race = Promise.race([p1,p2,p3])
// 返回结果为第一个改变状态的Promise相同的Promise
```

### util.promise

这是nodejs中的方法，将错误优先的回调风格的函数（err，value）传入，将会返回一个promise的版本。

```js
const util = require('util')
const fs = require('fs')
const newReadFile = util.promisify(fs.readFile)
newReadFile('./promise封装fs.txt')
  .then(value => console.log(value.toString()))
```

## Promise关键问题

### Promise状态的改变

1. resolve(value)：将Promise的状态由pending改变为resolved
2. reject(reason)：将Promise的状态由pending改变为rejected
3. 抛出异常：将Promise的状态由pending改变为rejected

```js
const p = new Promise((resolve, reject) => {
    // resolve('ok')
    // reject('error')
    throw '出问题了'
})
```

### Promise执行多个回调

只要状态发生改变，回调才会执行，多个回调也会执行。

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))

// 指定回调1
p.then(value => {
	alert(value)
})
// 指定回调2
p.then(value => {
	console.log(value)
})
```

### 改变Promise状态和回调函数的先后问题

1. 当Promise中执行同步任务时，先改变状态，然后执行回调函数。

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    resolve('ok')
}))

p.then(value => {
	console.log(value)
})
```

2. 当Promise中执行异步任务时，回调函数先执行，改变状态后执行。

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok')
    })
}), 3000)

p.then(value => {
	console.log(value)
})
```

### Promise中then的返回结果

1. 返回结果非Promise对象，会将返回的值包装成成功的Promise的结果。

```js
let p = Promise.resolve(321)
let p2 = p.then((value) => {
  return 'noPromise'
})
console.log(p2)
```

2. 如果未返回结果，那么默认会返回成功的状态结果值为undefined的Promise对象。

```js
let p = Promise.resolve(321)
let p2 = p.then((value) => {

})
console.log(p2)
```

3. 如果返回Promise则与返回与Promise保持一致状态和结果的Promise对象。

```js
let p = Promise.resolve(321)
let p2 = p.then((value) => {
  return Promise.resolve('test')
  // return Promise.reject('test')
})
console.log(p2)
```

### 串联多个任务

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok')
    })
}), 3000)

p.then(value => {
	return new Promise((resolve, reject) => {
        resolve("success")
    })
}).then(value => {
    console.log(value)
})
// 最后输出success
```

### 异常穿透

只需要在链式的最后捕获异常即可，这就是异常穿透，之中的环节出现异常都会被最后的catch捕获

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok')
    })
}), 3000)

p.then(value => {
    console.log(111)
}).then(value => {
    console.log(222)
}).then(value => {
    console.log(333)
}).catch(reason => {
    console.warn(reason)
})
```

### 中断Promise链

当有一堆链式Promise链时，如果想中途停止，需要在停止的地方返回一个pending状态的Promise才可

```js
let p = Promise.reject(new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok')
    })
}), 3000)

p.then(value => {
    console.log(111)
    // 中断后续的Promise链
    return new Promise(() => {})
}).then(value => {
    console.log(222)
}).then(value => {
    console.log(333)
}).catch(reason => {
    console.warn(reason)
})
```

## async函数

async修饰的函数的返回值为promise对象，其返回逻辑与then相同。

```js
async function test() {
  return Promise.resolve('test')
}
let res = test()
console.log(res)
```

## await

1. await修饰Promise对象时，会直接返回Promise的成功值。
2. await修饰其他值时，会直接将这个值返回。

注意：await必须写在async函数中，但是async中可以没有await。

```js
async function test() {
  let res = await Promise.resolve('test')
  console.log(res)
}
test()
```

## Promise常见封装

### Promise封装定时器

```js
// 封装生成1-100随机数的函数
function rand(m, n) {
  return Math.ceil(Math.random() * (n - m + 1)) + m - 1
}
// 获取button对象
const btn = document.querySelector('button')
// 设置点击事件
btn.addEventListener('click', () => {
  // 使用promise封装一个异步操作
  // resolve为成功函数
  // reject为失败函数
  const p = new Promise((resolve, reject) => {
    setTimeout(() => {
      let n = rand(1, 100);
      if (n <= 30) {
        resolve(n)	// 将promise对象的状态设置为 成功
      } else {
        reject(n)	// 将promise对象的状态设置为 失败
      }
    },1000)
  })
  // 调用then方法，其中有两个回调函数，第一个为promise为成功时的回调函数，第二个为promise为失败时的回调函数。
  // 只写第一个时，就是成功时的回调函数 
  p.then((value) => {
    alert('中奖，抽取号码为 ' + value)
  }, (value) => {
    alert('未中奖, 抽取号码为 ' + value)
  })
})
```

### Promise封装fs

```js
const fs = require('fs')

const p = new Promise((resolve, reject) => {
  fs.readFile('./promise封装fs.txt', (err, data) => {
    if (err) reject(err)
    resolve(data)
  })
})

p.then(value => {
  console.log('成功：' + value.toString())
}, res => {
  console.log('失败：' + res)
})
```

```js
function mineReadFile(path) {
  return new Promise((resolve, reject) => {
    require('fs').readFile(path, (err, data) => {
      if (err) reject(err)
      resolve(data)
    })
  })
}

mineReadFile('./promise封装fs.txt')
.then(
  value => console.log(value.toString()),
  res => console.warn(res)
)
```

### Promise封装Ajax

```js
const btn = document.querySelector('button')
btn.addEventListener('click', function () {
  const p = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', 'https://api.ooomn.com/api/xwzc')
    xhr.send()
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status >= 200 && xhr.status < 300) {
          resolve(xhr.response)
        } else {
          reject(xhr.status);
        }
      }
    }
  })

  p.then(value => {
    console.log('成功：' + value);
  }, res => {
    console.log('失败：' + res);
  })
})
```

```js
function sendAjax(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url)
    xhr.send()
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status >= 200 && xhr.status < 300) {
          resolve(xhr.response)
        } else {
          reject(xhr.status);
        }
      }
    }
  })
}

const btn = document.querySelector('button')
btn.addEventListener('click', () => {
  sendAjax('https://api.ooomn.com/api/xwzc')
    .then(value => console.log(value))
})
```

## Promise解决回调地狱

**回调地狱形式**

```js
const fs = require('fs')

fs.readFile('./1.txt', (err, data1) => {
  if(err) throw err
  fs.readFile('./2.txt', (err, data2) => {
    if(err) throw err
    fs.readFile('./3.txt', (err, data3) => {
      if(err) throw err
      let res = data1 + data2 + data3
      console.log(res)
    })
  })
})
// 结果123
```

**使用Promise解决回调地狱1**

```js
const fs = require('fs')
const util = require('util')

const mineReadFile = util.promisify(fs.readFile)

async function main() {
  const data1 = await mineReadFile('./1.txt')
  const data2 = await mineReadFile('./2.txt')
  const data3 = await mineReadFile('./3.txt')
  let res = data1 + data2 + data3
  console.log('结果：' + res)
}
```

**使用Promise解决回调地狱2**

```js
const fs = require('fs')

let p = new Promise((resolve, reject) => {
  fs.readFile('./1.txt',(err, data) => {
    if(err) reject(err)
    resolve(data)
  })
})
p.then(value => {
  return new Promise((resolve, reject) => {
    fs.readFile('./2.txt',(err, data) => {
      if(err) reject(err)
      resolve(value + data)
    })
  })
}).then(value => {
  return new Promise((resolve, reject) => {
    fs.readFile('./3.txt',(err, data) => {
      if(err) reject(err)
      resolve(value + data)
    })
  })
}).then(value => {
  console.log(value)
})
```

