## Express 框架

官方给出的概念：Express 是基于 Node.js 平台，快速、开放、极简的 Web 开发框架。

通俗的理解：Express 的作用和 Node.js 内置的 http 模块类似，是专门用来创建 Web 服务器。但是 Express 为我们封装了很多方法，能够极大的提高我们的开发效率。

Express 的本质：就是一个 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法。

## 安装

```Bash
npm i express
```

## 创建基本web服务器

```React
// 导入express
const express = require('express');
// 创建express实例
const app = express();

// 监听get请求
app.get('/get', function (req, res) {
  // 响应内容
  res.send({
    name: 'brokyz',
    age: 21,
    gender: '男'
  });
});
// 监听post请求
app.get('/post', function (req, res) {
  // 响应内容
  res.send('请求成功');
});

// 启动服务器监听3000端口
app.listen(3000, () => {
  console.log('express server running at <http://127.0.0.1:3000>');
});
```

## 监听客户端请求

监听get请求

```JavaScript
app.get('url', function(req, res) {
  res.send('success');
})
```

监听post请求

```React
app.post('url', function(req, res) {
  res.send('success');
})
```

- url：需要被请求的地址。
- res.send()：响应的内容。

## 获取 url 携带的查询参数

通过 req.query 对象，可以访问到客户端通过查询字符串的形式，发送到服务器的参数。

```React
app.get('/get', function (req, res) {
  res.send(req.query);
});
```

请求地址 http://127.0.0.1:3000/post?name=brokyz&age=21

返回 `{"name":"brokyz","age":"21"}`

如果不传入查询参数，那么 req.query 是空对象

## 获取 url 中的动态参数

如果我们想要获取请求 url 中的某个位置的参数，可以使用 req.params 对象。

```React
app.get('/get/:id/test', function (req, res) {
  res.send(req.params);
});
```

请求地址 http://127.0.0.1:3000/get/123456/test/234

返回 `{"id":"123456","id2":"234"}`

## express.static() 托管静态资源

通过 express.static() 我们可以非常方便地创建一个静态资源服务器。

例如通过以下代码，就可以将 public 目录下的图片、CSS文件、JavaScript文件对外公开访问了。

```React
app.use(express.static('public'))
```

此时就可以访问：

http://127.0.0.1:3000/images/bg.jpg

http://127.0.0.1:3000/images/bg.css

http://127.0.0.1:3000/images/bg.js

http://127.0.0.1:3000/images/bg.html

**注意：express在指定静态目录中查找文件，并对外提供资源访问路径。因此，存放静态文件的目录名不会出现在 URL 中。**

托管多个静态资源目录：

```React
app.use(express.static('public'))
app.use(express.static('files'))
```

- 当添加多个静态目录时，如果两个目录中用同名文件，**会根据添加路径的顺序查找所需要的文件**，因此会显示第一个查找到的文件。

挂载访问路径前缀：

```React
app.use('/test', express.static('public'))
```

此时就可以访问：

http://127.0.0.1:3000/test/images/bg.jpg

http://127.0.0.1:3000/test/images/bg.css

http://127.0.0.1:3000/test/images/bg.js

http://127.0.0.1:3000/test/images/bg.html

## 路由

在 Express 中，路由指的是**客户端的请求与服务器处理函数之间的映射关系**。

Express 中的路由分为 3 部分，分别是**请求类型、请求 URL 地址、处理函数**，格式如下：

语法：

```React
app.METHOD(PATH, HANDLER)
```

demo：

```React
app.get('/get', function(req, res) {
  res.send('success');
})

app.post('/post', function(req, res) {
  res.send('success');
})
```

路由匹配的过程：

每当一个请求到达服务器后，**需要先经过路由匹配**，只有匹配成功后，才会调用对应的处理函数。

在匹配时，会按照路由的顺序进行匹配，如果请求类型和请求的 URL 同时匹配成功，则 Express 会将这次请求，转交给对应的 function 函数进行处理。

## 路由模块化

为了方便对路由进行模块化的管理，Express 不建议将路由直接挂载到 app 上，而是推荐将路由抽离为单独的模块。

将路由抽离为单独的模块步骤如下：

1. 创建路由模块对应的 .js 文件。
2. 调用 **express.Router()** 函数创建路由对象。
3. 向路由对象上挂载具体的路由。
4. 使用 **module.express** 向外共享路由对象。
5. 使用 app.use() 函数注册路由模块。

demo：router.js

```React
// router.js 将路由单独分为一个模块
const express = require('express');
// 直接创建路由对象，此对象专门用于记录路由
const router = express.Router();

router.get('/get', (req, res) => {
  res.send('success');
});

router.post('/post', (req, res) => {
  res.send('success');
});
// 向外暴露路由
module.exports = router;
```

demo：main.js

```React
const express = require('express');
const router = require('./router');
// 创建express实例
const app = express();
// 挂载路由模块，并添加统一请求前缀api
app.use('/api', router);
// 启动监听端口3000
app.listen(3000, () => {
  console.log('server running on http://localhost:3000');
});
```

## 中间件

概念：

当一个请求到达 Express 的服务器后，可以连续调用多个中间件，从而对这次请求进行预处理。

客户端请求 => 中间件1处理 => 中间件2处理 => 中间件n处理 => 路由 => 响应客户端数据

## 全局中间件

```JavaScript
use(function(req, res, next) {
  console('这是中间件1');
  next();
})

use(function(req, res, next) {
  console('这是中间件2');
  next();
})
```

- 多个中间件之间**共享一份 req 和 res**。 基于这样的特性，我们可以在上有中间件中，**统一为 req 和 res 对象添加自定义的属性和方法**，供**下游的中间件或路由进行使用**。
- 中间件一定要写在路由之前，因为代码执行时时从上往下执行。

## 局部中间件

局部生效的中间件可以放在指定的路由内。

```JavaScript
const midware1 = (req, res, next) {
  console.log('这是一个局部生效中间件1');
}

const midware2 = (req, res, next) {
  console.log('这是一个局部生效中间件2');
}
router.get('/get', midware1, midware2, (req, res) => {
  res.send('success');
});

router.get('/get', [midware1, midware2], (req, res) => {
  res.send('success');
});
```

## 中间件的注意事项

1. 一定要在**路由之前注册**中间件。
2. 客户端发送过来的请求，**可以连续调用多个**中间件进行处理。
3. 执行完中间件的业务代码之后，**不要忘记调用 next() 函数**。
4. 为了**防止代码逻辑混乱**，调用 next() 函数后不要再写额外的代码。
5. 连续调用多个中间件时，多个中间件之间，**共享 req 和 res 对象**。

## 中间件的类别

1. 应用级别的中间件

   通过 app.use() 或 app.get() 或 app.post()，绑定到 app 实例上的中间件，叫做应用级别的中间件。

2. 路由级别的中间件

   绑定到 express.Router() 实例上的中间件，叫做路由级别的中间件。它的用法和应用级别的中间件没有任何区别。只不过，应用级别的中间件是绑定在 app 实例上，路由级别的中间件绑定在 router 实例上。

3. 错误级别的中间件

   错误级别的中间件的作用：专门用来获取整个项目中发生的异常错误，从而防止项目异常崩溃的问题。

   格式：错误级别的中间件处理函数中，必须有4个形参，形参顺序从前到后，分别是（err，req，res，next）。

   错误级别中间件必须要写在路由之后才可以捕获错误

```React
router.get('/get', [midware1, midware2], (req, res) => {
  throw new Error('服务器内部发生错误')
  res.send('success');
});

router.use(function(err, req, res, next) {
  console.log('发生了错误：' + err.message);
  res.send('发生了错误：' + err.message);
})
```

1. Express 内置中间件

   1. express.static 快速托管静态资源的内置中间件。

   2. express.json 解析 JSON 格式的数据请求（有兼容性，只有 4.16.0之后才可以用）。

      当请求方在请求时在 body 中传入 none 格式的 JSON 数据，将会被中间件解析。并在req中添加body属性进行结果储存，以供路由使用。

      当在请求体中传入 JSON 数据后，如果不使用中间件解析，将没有 res.body 属性，为 undefined。

```JavaScript
app.use(express.json());
app.post('/user', (req, res) => {
  console.log(req.body);
  res.send(req.body);
})
3. express.urlencoded 解析 URL-encode 格式的请求体数据。（4.16.0以上版本适用）。

当请求方在请求时在 body 中传入 x-www-form-urlencoded 格式的 JSON 数据，将会被中间件解析。并在req中添加body属性进行结果储存，以供路由使用。

当在请求体中传入 JSON 数据后，如果不使用中间件解析，将没有 res.body 属性，为 undefined。
app.use(express.urlencoded());
app.post('/user', (req, res) => {
  console.log(req.body);
  res.send(req.body);
})
```

## 自定义中间件

```JavaScript
const qs = requre('querystring')
app.use((res, req, next) => {
  let str = '';
  // 监听req的data事件，来获取客户端发送到服务器的数据
  req.on('data', (chunk) => {
    str += chunk;
  })
  
  req.on('end', () => {
    const body = qs.parse(str)
    console.log(str)
    next();
  })
})
app.post('/user', (req, res) => {
  console.log(req.body);
  res.send(req.body);
})
```

**监听 req 的 data 事件**

在中间件中，需要监听 req 对象的 data 事件，来获取客户端发送到服务器的数据。

如果数据量比较大，无法一次性发送完毕，客户端就会把数据切分后，分批发送到服务器。所以 data 事件可能会触发多次，每触发一次 data 事件时，获取到的数据只是完整数据的一部分，需要手动对获取到的字符串进行拼接。

拿到的数据是字符串形式，类query类型如 name=brokyz&age=21

**监听 req 的 end 事件**

当请求体数据接受完毕后，会自动触发 req 的 end 事件。

因此在 end 事件中，我们可以拿到并处理完整的请求体数据。

**使用 querystring 模块解析请求体数据**

Node.js 内置了一个 querystring 模块，专门用来处理查询字符串。通过这个模块的 parse() 函数，可以轻松把查询字符串，解析成对象的格式。

**将自定义中间件封装为模块**

```JavaScript
const qs = requre('querystring');

function bodyParser(req, req, next) {
  let str = '';
  // 监听req的data事件，来获取客户端发送到服务器的数据
  req.on('data', (chunk) => {
    str += chunk;
  })
  
  req.on('end', () => {
    const body = qs.parse(str)
    console.log(str)
    next();
  })
}
module.exports = bodyParser
```

## 跨域资源共享

### CORS

CORS（Cross-Origin Resource Sharing，跨域资源共享）由一系列 **HTTP 响应头**组成，**这些 HTTP 响应头决定浏览器是否阻止前端 JS 代码跨域获取资源。**

浏览器的**同源安全策略**默认会阻止网页跨域获取资源。但是如果服务器**配置了CORS相关的HTTP响应头，就可以解除浏览器端的跨域访问限制。**

cors 是 Express 的一个第三方中间件。通过安装和配置 cors 中间件，可以很方便的解决跨域问题。

使用步骤如下：

1. 运行 npm i cors 安装中间件
2. 使用 const cors = require('cors')导入中间件
3. 在路由之前调用 app.use(cors())配置中间件

```JavaScript
const cors = require('cors')
// 使用cors中间件解决跨域
app.use(cors)
app.post('/user', (req, res) => {
  console.log(req.body);
  res.send(req.body);
})
```

**CORS 响应头 - Access-Control-Allow-Origin**

响应头部中可以携带一个 Access-Control-Allow-Origin 字段，其语法如下：

```JavaScript
Access-Control-Allow-Origin: <origin> | *
```

origin 参数的值指定了允许访问该资源的外域 URL。

例如下面的字段值，只允许来自 http://itcast.cn 的请求：

```JavaScript
res.setHeader('Access-Control-Allow-Origin','http://itcast.cn')
// 允许任何网站都可以访问
res.setHeader('Access-Control-Allow-Origin','*')
```

**CORS 响应头 - Access-Control-Allow-Headers**

默认情况下，CORS仅支持客户端向服务器发送如下的9个请求头：

```JavaScript
Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、
Width、Content-Type(值仅限于 text/plain、multipart/form-data、
application/x-www-form-urlencodes 三者之一)
```

如果客户端发送了额外的请求头信息，则需要在服务器端，通过 Access-Control-Allow-Headers 对外额外的请求头声明，否则这次请求会失败。

**CORS 响应头 - Access-Control-Allow-Methods**

默认情况下，CORS 仅支持客户端发起 GET、POST、HEAD 请求。

如果客户端希望通过 PUT、DELETE 等方式请求服务器的资源、则需要在服务器端，通过 Access-Control-Allow-Methods 来指明实际请求时所允许使用的 HTTP 方法。

```JavaScript
res.setHeader('Access-Control-Allow-Methods','GET, POST, DELETE')
// 允许任何网站都可以访问
res.setHeader('Access-Control-Allow-Methods','*')
```

### 简单请求

同时满足以下两大条件的请求，就属于简单请求：

1. 请求方式：GET、POST、HEAD 三者之一
2. HTTP 头部信息不超过以下几种字段：

```JavaScript
无自定义头部字段、Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、
Width、Content-Type(只有三个值 text/plain、multipart/form-data、application/x-www-form-urlencodes)
```

### 预检请求

只要符合以下任何一个条件的请求，都需要进行预检请求：

1. 请求方式为：GET、POST、HEAD 之外的 Method 类型
2. 请求头中包含自定义头部字段
3. 向服务器发送了 application/json 格式的数据

在浏览器与服务器正式通信之前，浏览器会向发送 OPTION 请求进行预检，以获知服务器是否允许该实际请求，所以这一次的 OPTION 被称为预检请求。服务器成功响应预检请求后，才会发送真正的请求，并携带真实数据。

### JSONP

浏览器通过`script`标签的 src 属性，请求服务器上的数据，同时服务器返回一个函数。这种请求数据的方式叫做 JSONP。

特点：

1. JSONP 不属于真正的 Ajax 请求，因为它没有使用 XMLHttpRequest 这个对象。

JSONP 的接口必须配置在 CORS 中间件之前

demo:

1. 获取客户端发送来的回调函数的名字
2. 得到要通过 JSONP 形式发送给客户端的数据
3. 根据前两部得出的数据，拼接处一个函数调用的字符串
4. 把上一步拼接到的字符串，动态的以`script`标签响应给客户端进行解析执行

```JavaScript
app.get('/api/jsonp', (res, req) => {
  // 获取客户端发送过来的回调函数的名字
  const funName = req.query.callback
  // 得到要通过 JSONP 发送给客户端的数据
  const data = {name: 'brokyz', age: 21}
  // 根据前两部的结果，拼接出一个函数调用的字符串, 将对象转换为json格式的数据
  const script = `${funName}(${JSON.stringfy(data)})`
  res.send(script)  
})
```

## mysql 模块
