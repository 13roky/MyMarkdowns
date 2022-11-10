# Ajax 基础概念

## 客户端与服务器

服务器：上网过程中，负责**存放和对外提供资源**的电脑，叫做服务器。

客户端：上网过程中，负责**获取和消费资源**的电脑，叫做客户端。

## URL 地址

URL（全称是JniformResourceLocator）中文叫**统一资源定位符**，用于标识互联网上每个资源的唯一存放位置。浏览器只有通过URL地址，才能正确定位资源的存放位置，从而成功访问到对应的资源。

常见的 URL：

https://www.baidu.com

https://jd.com

### URL 的组成部分

URL 一般由三种部分组成：

1. 客户端与服务器之间的**通信协议**。
2. 存有该资源的**服务器名称**。
3. 资源在服务器上具体的**存放位置**。

以[`https://www.cnblogs.com/brokyz/p/16443138.html](<https://www.cnblogs.com/brokyz/p/16443138.html>)举例：`

- http：通信协议。
- [www.cnblogs.com](http://www.cnblogs.com)：服务器名称。
- [brokyz/p/16443138.html](https://www.cnblogs.com/brokyz/p/16443138.html)：资源在服务器上具体的存放位置。

## 客户端与服务器的通信过程

| 客户端                  | web服务器                                 |
| ----------------------- | ----------------------------------------- |
| 1. 打开浏览器           | 1. 服务器接收到客户端发来的资源请求       |
| 2. 输入要访问的网站地址 | 2. 服务器在内部处理这次请求，找到相关资源 |
| 3. 向服务器发起资源请求 | 3. 服务器把找到的资源，响应给客户端       |

注意：

1. 客户端和服务器之间的通信过程，分为**请求——处理——响应**三个步骤。
2. 网页中的每一个资源，都是通过**请求——处理——响应**的方式从服务器获取来的。

### 基于浏览器的开发者工具分析通信过程

1. 在 chrome 开发者工具中切换到 Network 面板。
2. 选中 Doc 页签。
3. 刷新页面，分析客户端与服务器之间的通信过程。

## 服务器提供的常见资源

文字内容、图片、音频、视频….等都是我们网页中的常见资源。

网页中的**数据也是一种资源**。

- HTML 是网页的骨架。
- CSS 是网页的颜值。
- JavaScript 是网页的行为。
- data 是网页的灵魂。

### 网页中如何请求数据

数据，也是服务器对外提供的一种资源。只要是资源，必然要通过请求一处理-响应的方式进行获取。

- 如果要在网页中请求服务器上的数据资源，则需要用到XMLHttpRequest对象。
- XMLHttpRequest（简称xhr）是浏览器提供的 JS 成员，通过它，可以请求服务器上的数据资源。
- 最简单的用法`var xhrObj=new XMLHttpRequest()`

### 资源的请求方式

客户端请求服务器时，请求的方式有很多种，最常见的两种请求方式分别为 `get` 和 `post` 请求。

- `get`请求通常用于获取服务端资源（向服务器要资源）

  例如：根据URL地址，从服务器获取HTML文件、css文件、js文件、图片文件、数据资源等

- `post`请求通常用于向服务器提交数据（往服务器发送资源）

  例如：登录时向服务器**提交的登录信息**、注册时向服务器**提交的注册信息**、添加用户时向服务器**提交的用户信息**等各种**数据提交操作**。

# 接口

## 概念

使用 Ajax 请求数据时，**被请求的 URL 地址**，就叫做**数据接口**（简称接口）。同时，每个接口必须有**请求方式**。

例如：

https://autumnfish.cn/api/joke/list   获取笑话的接口（GET请求）

https://mock.apifox.cn/m1/1301638-0-default/pet  添加宠物信息的接口（POST请求）

## 接口的请求过程

- 通过 GET 方式请求接口的过程。
  1. 网页中的 Ajax 向服务器发送 GET 请求。
  2. 服务器收到请求后，在内部处理请求。
  3. 服务器处理完成后，将结果响应给网页。
  4. 网页中的 Ajax 收到响应，呈现给网页。
- 通过 POST 方式请求接口的过程。
  1. 网页中的 Ajax 向服务器发送 POST 请求。
  2. 服务器收到请求后，在内部处理请求。
  3. 服务器处理完成后，将结果响应给网页。
  4. 网页中的 Ajax 收到响应，呈现给网页。

## 接口测试工具

为了验证接口能否被正常访问，我们常常需要使用接口测试工具，来对数据接口进行检测。

接口测试工具能让我们在**不写任何代码**的情况下，**对接口**进行**调用**和**测试**。

可以使用 **apifox** 进行测试：https://www.apifox.cn/

## 接口文档

接口文档就是**接口说明文档，是我们调用接口的依据**。好的接口文档包含了对**接口 URL**、**参数**以及**输出内容**的说明，我们**参照接口文档就能方便的知道接口的作用**，以及接口如何进行调用。

**接口文档的组成部分**

接口文档可以包含很多信息，也可以按需求进行精简，不过，一个合格的接口文档，应该包含以下6项内容，从而为接口的调用提供依据。

1. **接口名称**：用来标识各个接口的简单说明，如登录接口，获取图书列表接口等。
2. **接口 URL**：接口的调用地址。
3. **调用方式**：接口的调用方式，如 GET 或 POST。
4. **参数格式**：接口需要传递的参数，每个参数必须包含**参数名称、参数类型、是否必选、参数说明**这 4 项内容。
5. **相应格式**：接口的返回值的详细描述，一般包含**数据名称、数据类型、说明** 3 项内容。
6. 返回示例（可选）：通过对象的形式，列举服务器返回数据的结构。

# 了解 Ajax

**什么是Ajax**

Ajax 的全称是 Asynchronous Javascript And XML（异步JavaScript和XML）。

通俗的理解：在网页中利用 XMLHttpRequest 对象和服务器进行数据交互的方式，就是Ajax。

**为什么学习 Ajax**

之前所学的技术，只能把网页做的更美观漂亮，或添加一些动画效果，但是，Ajax 能让我们轻松实现网页与服务器之间的数据交互。

**Ajax 典型应用场景**

- 用户名检测：注册用户时，通过 Ajax 的形式，动态检测用户名是否被占用。
- 搜索提示：当输入搜索关键字时，通过 Ajax 的形式，动态加载搜索提示列表。
- 数据分页显示：当点击页码值的时候，通过 Ajax 的形式，根据页码值动态刷新表格的数据。
- 数据的增删改查：数据的添加、删除、修改、查询操作，都需要通过 Ajax 的形式，来实现数据的交互。

**Ajax 的优点**

Ajax 允许我们在不刷新整个网页的前提下，更新部分网页内容（局部加载）。

# JS 原生 Ajax

## get 请求

```jsx
// 1. 创建请求对象
let xhr = new XMLHttpRequest();
// 2. 设置请求方法和请求的url接口地址
xhr.open('get', '<https://autumnfish.cn/api/joke/list?num=2>', true);
// 3. 监听响应事件，当得到响应式执行
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && /^2\\d{2}$/.test(xhr.status)) {
        console.log(xhr.responseText);
    }
};
// 4. 发送请求，只有执行这步时才会发送上面设置的请求
xhr.send()
```

## post 请求

```jsx
// 1. 创建请求对象
let xhr = new XMLHttpRequest();
// 2. 设置请求方法和请求的url接口地址
xhr.open("post", "<http://www.liulongbin.top:3006/api/addbook>");
// 3. 设置请求头（在复杂参数时必须使用）
xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded", true)
// 4. 监听响应事件，当得到响应式执行
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && /^2\\d{2}$/.test(xhr.status)) {
        console.log(xhr.responseText);
    }
};
// 5. 发送请求并携带post参数，只有执行这步时才会发送上面设置的请求
xhr.send("bookname=test&author=brokyz&publisher=test");
```

# JQuery 中的 Ajax

## 简介

浏览器中提供的 **XMLHttpRequest 用法比较复杂**，所以 **jQuery** 对 XMLHttpRequest 进行了封装，提供了一系列 Ajax 相关的函数，极大地**降低了Ajax的使用难度**。

jQuery 中发起 Ajax 请求最常用的三个方法如下：

- `$.get()`
- `$.post()`
- `$.ajax()`

## $.get()

jQuery 中`$.get()`函数的功能单一，专门用来发起 get 请求，从而将服务器上的资源请求到客户端来进行使用。 **`$.get()`函数的语法如下：**

```jsx
$.get(url, [data], [callback])
```

| 参数名   | 参数类型 | 是否必选 | 说明                     |
| -------- | -------- | -------- | ------------------------ |
| url      | string   | 是       | 要请求的资源地址         |
| data     | object   | 否       | 请求资源期间要携带的参数 |
| callback | function | 否       | 请求成功执行的回调函数   |

demo：

```jsx
$(function () {
    $("button").on("click", function () {
        $.get("<https://autumnfish.cn/api/joke/list>", { num: 10 }, function (res) {
            console.log(res);
        });
    });
});
```

## $.post()

jQuery 中`$.post()`函数的功能弹一，专门用来发起 post 请求，从而向服务器提交数据。

`$.post()`**函数的语法如下：**

```jsx
$.post(url, [data], [callback])
```

| 参数名   | 参数类型 | 是否必选 | 说明                   |
| -------- | -------- | -------- | ---------------------- |
| url      | string   | 是       | 要提交数据的地址       |
| data     | object   | 否       | 要提交的数据           |
| callback | function | 否       | 请求成功执行的回调函数 |

demo：

```jsx
$(function () {
    $("button").on("click", function () {
        $.post("<https://mock.apifox.cn/m1/1301638-0-default/pet>", { name: 'brokyz', status: 'sold' }, function (res) {
            console.log(res);
        });
    });
});
```

## $.ajax()

使用`$.ajax()`发起 GET 请求：

```jsx
$(function () {
    $.ajax({
        type: 'GET',
        url: '<https://autumnfish.cn/api/joke/list>',
        data: {num: 10},
        success: function(res) {
            console.log(res);
        },
        error: function(err) {
            console.log(err);
        }
    })
});
```

使用`$.ajax()`发起 POST 请求：

```jsx
$(function () {
    $.ajax({
        type: 'POST',
        url: '<https://mock.apifox.cn/m1/1301638-0-default/pet>',
        data: { name: 'brokyz', status: 'sold' },
        success: function(res) {
            console.log(res);
        }
    })
});
```

# Axios 中的 Ajax

## 简介

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

在客户端中axios使用XMLHttpRequestlai模块，在服务器端axios使用nodejs的原生http模块。

## axios.get

```js
axios.get('https://api.ooomn.com/api/xwzc', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

## axios.post

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

## axios

```js
// 发送 POST 请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
```

## 并发请求

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```

# form 表单与 Ajax

## 简介

表单在网页中主要负责**数据采集功能**。HTML中的<form>标签，就是用于采集用户输入的信息，并通过<form>标签的提交操作，把采集到的信息提交到服务器端进行处理。

表单有三个组成部分：

- 表单标签：`<form></form>`
- 表单域：表单标签的内部
- 表单按钮：写在表单域中的`<button type=”submit”>submit</button>`

## 表单的常用属性

### action

`action`属性用来规定当提交表单时，向何处发送表单数据。

`action`属性的值应该是后端提供的一个 URL 地址，这个 URL 地址专门负责接收表单提交过来的数据。

当<form>表单在未指定 action 属性值的情况下，action 的默认值为当前页面的 URL 地址。

注意：当提交表单后，页面会**立即跳转**到 action 属性指定的 URL地址

### target

`target`属性用来规定在何处打开 action URL。

它的可选值有5个：

- `_self`：在相同的框架中打开。默认值。
- `_blank`：在新窗口打开。
- `_parent`：在父框架集中打开。（不常用）
- `_top`：在整个窗口中打开。（不常用）
- `framename`：在指定的框架中打开（不常用）

### method

`method`属性用来规定以何种方式把表单数据提交到 action URL。

它的可选值有两个，分别是 get 和 post。

默认情况下，method 的值为 get，表示通过 URL 地址的形式，把表单数据提交到 action URL。

注意：

get 方式适合提交**少量的**、**简单的**数据。

post：方式适合用来提交**大量的**、**复杂的**、或包含**文件上传**的数据。

实际开发中表单的 post 提交方式用的最多，很少用 get。例如登录、注册、添加数据等表单操作，都需要使用 post 方法来提交表单。

### enctype

`enctype`属性用来规定在发送表单数据之前如何**对数据进行编码**。

它的可选值有3个：

- `application/x-www-form-urlencoded`：在发送前编码所有字符。默认值。
- `multipart/form-data`：不对字符进行编码。在使用包含文件上传控件的表单时，必须使用该值。
- `text/plain`：空格转换为”+“加号，但不对特殊字符编码。（很少用）

## 表单同步提交

通过点击 submit 按钮，触发表单提交的操作，从而使页面跳转到 action URL 的行为，叫做表单的同步提交。

缺点：

1. 表单同步提交后，**整个页面会发生跳转**，跳转到 action URL 所指向的地址，用户体验差。
2. 表单同步提交后，**页面之前的状态和数据会发生丢失**。

解决方案：

让表单只负责采集数据，Ajax负责将数据提交到服务器。

## 通过 Ajax 提交表单数据

### jQuery 监听提交事件

```jsx
$(function () {
    // 1. 第一种
    $('#f1').submit(function (e) {
        alert('监听表单提交');
    });

    // 2. 第二种
    $('#f1').on('submit', function (e) {
        alert('监听表单提交');
    });
})
```

### preventDefault() 阻止默认行为

`preventDefault()`可以阻止表单提交后仍然会发生跳转等默认行为。

```jsx
$(function () {
    $('#f1').submit(function (e) {
        alert('监听表单提交');
		// 阻止默认行为
		e.preventDefault();
    });
})
```

### **serialize**() 获取表单数据

`serialize()`是jQuery中的方法，可以获取到表单提交时的表单内的数据。

```html
<body>
  <form id="myform" action="">
    <table>
      <tr>
        <td>username:</td>
        <td><input type="text" name="username" value="brokyz"></td>
      </tr>
      <tr>
        <td>password:</td>
        <td><input type="password" name="password" value="testpassword"></td>
      </tr>
      <tr>
        <td></td>
        <td><button type="submit" id="submit">提交</button></td>
      </tr>
    </table>
  </form>
</body>
```

```jsx
$(function () {
    $('#f1').submit(function (e) {
        alert('监听表单提交');
		// 阻止默认行为
		e.preventDefault();
		let data = $(this).serialize();
    });
})
```

注意：必须为每个表单元素添加 name 属性。

### demo

```jsx
<body>
    <form action="/login" id="f1">
        <input type="text" name="username">
        <input type="password" name="password">
        <button type="submit">提交</button>
    </form>
    <script>
        $(function () {
            // 1. 第一种
            $('#f1').submit(function (e) {
                alert('监听表单提交');
                // 阻止表单的默认行为
                e.preventDefault();
                // 获取表单的值
                let data = $(this).serialize();
                console.log(data);  // username=brokyz&password=123456
            });
        })
    </script>
</body>
```

# 跨域与jsonp

## 同源策略

如果**两个页面的协议，域名和端口都相同**，则两个页面具有相同的源，称为**同源**。

对于`http://www.brokyz.com/index.html`页面的同源检测。

| URL                                  | 是否同源 | 原因                         |
| ------------------------------------ | -------- | ---------------------------- |
| http://www.brokyz.com/other.html     | 是       | 同源（协议、域名、端口相同） |
| https://www.brokyz.com/about.html    | 否       | 协议不同（http与https）      |
| http://blog.brokyz.com/movie.html    | 否       | 域名不同                     |
| http://www.brokyz.com:7000/home.html | 否       | 端口不同                     |
| http://www.brokyz.com:80/main.html   | 是       | 同源                         |

**同源策略是浏览器提供的一个安全功能**。

MDN官方解释：同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制。

通俗理解：A网站的 JavaScript，不允许与非同源网站 C 之间进行资源交互，例如：

1. 无法读取非同源网页的 Cookie、LocalStorage 和 indexedDB。
2. 无法接触非同源网页的 DOM。
3. 无法向非同源地址发送 Ajax 请求。

## 跨域

**跨域就是非同源。**

出现跨域的根本原因：**浏览器的同源策略**不允许非同源的 URL 之间进行资源交互。

网页A：http://www.brokyz.com/index.html

接口B：http://www.api.com/userlist

网页A和接口B不同源，所以在网页A中不能使用Ajax发起请求接口B，这是由于浏览器的同源策略不允许发起请求。

注意：由于我们平时请求api时，并不会出现跨域问题，这时因为，在后端中使用cors已经解决了跨域问题。

**浏览器对跨域请求拦截的原理**

![https://pic.imgdb.cn/item/62e8b30b16f2c2beb1ae2238.jpg](https://pic.imgdb.cn/item/62e8b30b16f2c2beb1ae2238.jpg)

### 如何实现跨域数据请求

现如今，实现跨域数据请求，最主要的两种解决方案，分别是 **JSONP** 和 **CORS**。

JSONP： 出现的早，兼容性好 (兼容低版本IE)。是前端桯序员为了解决跨域问题，被迫想出来的一**临时解决方案**。缺点是**只支持GET 请求**，不支持 POST 请求。

SORS：出现的比较晚，它是 W3C 标准，属于跨域 Ajax 请求的根本解决方案。支持 GET 和 POST 请求。缺点是不兼容某些低版本的浏览器。

## JSONP

### 实现原理

由于**浏览器同源策略**的限制，网页中**无法通过 Ajax 请求非同源的接口数据**。但是 **<script> 标签不受浏览器同源策略的影响**，可以通过 src 属性，请求非同源的js 脚本。

因此，JSONP的实现原理，就是通过<script>标签的 src 属性，**请求跨域的数据接口**，然后将自己调用数据的函数名以**函数调用**的形式作为 url 的参数传递给服务器让服务器去调用，函数接收跨域接口响应回来的数据。

```jsx
// 1. 原生jsonp
<body>
  <script>
    function success(data) {
      console.log("拿到了data数据");
      console.log(data);
    }
  </script>

  <script src="<http://www.brokyz.tk/jsonp?callback=success&name=brokyz&age=21>"></script>
</body>
```

1. 在本机定义一个函数，作为服务器的回调函数，来传递需要的数据。
2. 通过<script>标签携带接口地址，来向服务器发送请求。在接口中需要加上刚刚定义好的回调函数的名字。
3. 服务器接受到请求，通过接口中携带的回调函数，知道了请求方机器上获取数据的函数。
4. 服务器调用函数，将需要发送的结果作为参数传入函数。
5. 本机的函数被服务器调用，获得了传递过来的数据。

```jsx
// 2. jQuery jsonp
$.ajax({
  url: "<http://www.brokyz.tk/jsonp?callback=success&name=brokyz&age=21>",
  dataType: "jsonp",
  jsonp: "cb",
  jsonpCallback: "abc",
  success: function (res) {
    console.log(res);
  },
});
```

- 默认情况下，使用 jQuery 发起 JSONP 请求，会自动携带一个callback=jQueryxxxx的参数，这是jQuery随机生成的一个回调函数的名字。
- jsonp：需要发送到服务器的参数名称，默认为 callback。（可选）
- jsonpCallback：自定义回调函数的名字，默认为随机生成。（可选）
- jQuery 中的 JSONP，也是通过<script>标签的 src 属性来实现跨域访问的，只不过 jQuery 采用的是动态创建和移除<script>标签的方式，来发起 JSONP 数据请求。
- 在发起 JSONP 请求的时候，动态向<header>中 append 一个 <script> 标签。