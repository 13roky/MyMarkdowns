# Node.js

# 导入模块

类似于 Python 在 Node.js 中，一些功能将以模块的形式存在，需要使用时需要导入模块。

```jsx
const fs = require('fs')
```

# fs 文件系统模块

## fs.readFile() 读取文件

读取指定文件中的指定内容。

```jsx
fs.readFile(path[, options], callback)
```

- path：必选参数，以字符串的形式来表示文件的路径。
- options：可选参数，表示以什么编码形式来读取文件。
- callback：必选参数，文件读取完成后，通过回调函数拿到读取结果。
- err：回调函数中的第一个参数，表示文件读取失败返回的结果。如果读取失败，err 为错误对象；如果成功则为 null。
- dataStr：回调函数中的第二个参数，表示文件读取成功返回的结果。如果文件读取成功，返回文件内容；如果读取失败，返回 undefined。

```jsx
const fs = require('fs');

fs.readFile('fs文件系统\\\\testFile.txt', 'utf-8', function (err, dataStr) {
  console.log('err is : ' + err);
  console.log('content is : ' + dataStr);
});
```

## fs.writeFile() 写入文件

向指定文件中写出指定内容。写入成功将会覆盖原内容。

```jsx
fs.writeFile(path, data[, options], callback)
```

- path：必选参数，以字符串的形式来表示文件的路径。
- data：必选参数，表示要写入的内容。
- options：可选参数，表示以什么格式写入内容，默认是 utf8。
- callback：必选参数，文件写入完成后的回调函数。
- err：回调函数中的参数，表示文件读取失败返回的结果。如果写入成功返回 null；如果写入失败返回错误对象。

```jsx
fs.writeFile('./2.txt', '123', function (err) {
  console.log('err is : ' + err);
});
```

- 此方法只能够创建文件，不能够创建路径。
- 写入文件时会覆盖原来的文件内容。

## 文件路径

### __dirname

表示文件所在的目录的绝对路径。

```jsx
console.log(__dirname)
// d:\\Dev\\Projects\\myNodejs\\fs文件系统
```

### __filename

表示文件的绝对路径。

```jsx
console.log(__filename)
// d:\\Dev\\Projects\\myNodejs\\fs文件系统\\readFile.js
```

### 代码运行时路径的问题

在 VScode 中，代码运行默认实在工作区目录运行代码的，而不是在代码当前目录运行。所以在路径识别时，也是以工作区根目录为基准来进行文件识别的。

如果需要从代码当前目录运行代码，需要对相应的运行参数进行设置。

比如使用 Code Runner 运行插件时，需要在插件设置里面将 CWD（Code Working Directory）设置为代码当前路径而不是项目的根目录。

# Path 路径模块

path 模块是 Node.js 官方提供的、用来处理路径的模块。它里面提供了一系列的属性和方法，来满足开发者对路径的处理需求。

```jsx
const path = require('path')
```

## path.join() 路径拼接

将多个路径片段拼接为完整的路径字符串。

```jsx
path.join([...path])
```

- ../：将会抵消上一个路径。
- ./：无意义。

```jsx
console.log(path.join('/a', '/b/c', '../d', './e', '/f'))
// \\a\\b\\d\\e\\f
console.log(path.join(__dirname, './testFile.txt'))
// d:\\Dev\\Projects\\myNodejs\\fs文件系统\\testFile.txt
console.log(path.join(__dirname, '../测试/test.txt'))
// d:\\Dev\\Projects\\myNodejs\\测试\\test.txt
```

## path.basename() 文件名

可以从文件路径中，获取到文件的名称。

```jsx
path.basename(path[, ext])
```

- ext：可选。拓展名，将会删除指定拓展名。

```jsx
console.log(path.basename('/fs文件系统/路径拼接.js'))
// 路径拼接.js
console.log(path.basename('/fs文件系统/路径拼接.js', '.js'))
// 路径拼接
```

## path.extname() 获取扩展名

可以从文件路径中，获取到文件扩展名。

```jsx
path.basename(path)
console.log(path.extname('/fs文件系统/路径拼接.js'))
// .js
```

# http 模块

http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 http.createServer() 方法，就能方便的把一台普通电脑，变成一台 web 服务器，从而对外提供资源服务。

```jsx
const http = require('http')
```

## 创建基本的 web 服务器

```jsx
// 1.导入http模块
const http = require('http');
// 2.创建web服务器实例
const server = http.createServer();
// 3.为web服务器绑定监听事件，监听客户端请求
server.on('request', (req, res) => {
  console.log('someone request the server.');
  const str = 'request url is ' + req.url + ' , request method is ' + req.method;
  console.log(str);
	res.setHeader('Content-Type', 'text/html; charset=utf-8');
  // 返回内容
  res.end(str);
});
// 4.启动服务器，设置监听端口
server.listen(3000, () => {
  console.log('server running on <http://127.0.0.1:3000>');
});
```

## http.createServer()

创建 web 服务器实例。

```jsx
const server = http.createServer();
```

## server.on()

为服务器实例对象添加监听事件，用于监听客户端请求。

```jsx
server.on('request', function(req, res){
	console.log('someone request the server.');
})
```

- 第一个参数：为服务器绑定 request 的事件，用于监听客户端请求。

- 第二个参数：指定回调函数，用于指定客户端触发监听事件索要执行的代码。

- req 请求对象：回调函数的第一个参数，为 req 请求对象。里面存储了与

  客户端

  相关的

  数据

  或

  属性

  。

  - req.url：客户端请求的地址。
  - req.method：客户端请求的方法。

- res 响应对象：回调函数的第二个参数，为 res 响应对象。里面存储了与

  服务器

  相关的

  数据

  或

  属性

  。

  - res.end()：向客户端发送指定的内容，并结束这次请求的处理过程。

## 解决中文乱码的问题

当向客户端响应中文内容的时候，会出现乱码问题，此时需要使用 res 响应对象**手动设置内容的编码格式**。

```jsx
res.setHeader('Content-Type', 'text/html; charset=utf-8');
```

## web 服务器小案例

```jsx
const http = require('http');
const path = require('path');
const fs = require('fs');

// 创建 web 服务器实例
const server = http.createServer();

// 为服务器绑定监听事件，监听客户端请求
server.on("request", (req, res) => {
  let url;

  console.log('someone visit our web server.');
  const str = '请求地址是 ' + req.url + ' , 请求方法是' + req.method;
  console.log(str);

  // 设置响应头，解决中文乱码问题
  res.setHeader('Content-Type', 'text/html; charset=utf-8');

  // 路由到指定文件
  if (req.url == '/') {
    url = path.join(__dirname, './web/index.html');
  } else {
    url = path.join(__dirname, './web', req.url);
  }

  // 查找文件并输出，如果不存在则返回404
  fs.readFile(url, (err, dataStr) => {
    if (err) {
      return res.end('<h1>404<h1>');
    }
    res.end(dataStr);
  });

});

// 启动启动服务器 
server.listen(3000, () => {
  console.log('server running at <http://127.0.0.1:3000>');
});
```

# 模块化

模块化就是**遵守固定的规则**，将一个**大文件**拆分成**独立并相互依赖**的多个**小模块**。

代码模块化的优点：

1. 提高了代码的**复用性**。
2. 提高了代码的**可维护性**。
3. 可以实现**按需加载**。

## CommonJS 模块化规范

Node.js 遵循了 CommonJS 模块化规范，CommonJS 规定了**模块的特性**和**模块之间如何相互依赖**。

CommonJS 规定：

1. 每个模块内部，**module 变量**代表当前模块。
2. module 变量是一个对象，它的 exports 属性（即 **module.exports**）**是对外的接口**。
3. 加载某个模块，其实是加载该模块的 module.exports 属性。**require() 方法用于加载模块**。

模块化规范的好处：大家都遵守同样的模块化规范写代码，降低了沟通成本，极大的方便了各个模块之间的相互调用，利人利己。

## Node.js 中模块的分类

Node.js 中根据模块来源的不同，将模块分为了 3 大类，分别是：

- 内置模块（内置模块是由 Node.js **官方**提供的，例如 fs、path、http等）。
- 自定义模块（**用户创建**的每个 .js 文件，都是自定义模块）。
- 第三方模块（由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，**使用前需要提前下载**）

## 加载模块

使用 require() 方法加载需要的模块，可以加载需要的内置模块、用户自定义模块、第三方模块进行使用。

```jsx
// 1. 加载内置模块
const http = require('http');

// 2. 加载用户自定义模块, 可以省略.js扩展名
const http = require('./custom.js');

// 3. 加载第三方模块，需要提前下载
const http = require('moment');
```

## 模块作用域

和**函数作用域类似**，在自定义模块中定义的**变量、方法**等成员，**只能在当前模块内被访问**，这种**模块级别的访问限制**，叫做**模块作用域**。

```jsx
// 当我们访问我们平时普通的js文件时，不可以得到里面的普通方法和属性
const custom = require('./custom');
console.log(custom);
// 由于存在模块作用域，输出结果为空对象 { }
```

- 模块作用域有效的防止了全局变量的污染问题。

## 向外共享模块作用域中的成员

### module 对象

在每个 .js 自定义模块中都有一个 module 对象，它里面**存储了和当前模块有关的信息**。

```jsx
console.log(module);
/*
Module {
  id: '.',
  path: 'd:\\\\Dev\\\\Projects\\\\myNodejs\\\\模块化',
  exports: {},
  filename: 'd:\\\\Dev\\\\Projects\\\\myNodejs\\\\模块化\\\\module.js',
  loaded: false,
  children: [],
  paths: [
    'd:\\\\Dev\\\\Projects\\\\myNodejs\\\\模块化\\\\node_modules',
    'd:\\\\Dev\\\\Projects\\\\myNodejs\\\\node_modules',
    'd:\\\\Dev\\\\Projects\\\\node_modules',
    'd:\\\\Dev\\\\node_modules',
    'd:\\\\node_modules'
  ]
}
*/
```

### module.exports 对象

在 module 模块中，可以使用 module.exports 对象，将模块内的成员共享出去，供外界使用。

外界用 **require() 方法**导入自定义模块时，得到的就是 module.exports 所指向的对象。

```jsx
// test.js文件
const m = require('./testExports');
console.log(m);

// testExports.js文件
module.exports.username = 'brokyz';
module.exports.sayHi = function () {
	console.log('Hi');
}
```

注意：使用 require() 方法导入模块时，导入的结果，**永远以 module.exports 指向的对象为准**。

### exports 对象

由于 module.exports 单词写起来比较复杂，为了简化向外共享成员的代码，Node 提供了 exports 对象。默认情况下，**exports 和 module.exports 指向同一个对象**。

**最终的共享结果，还是以 module.exports 指向的对象为准**。

```jsx
console(module.exports === exports)
// true
```

注意：

1. 第一种情况

```jsx
exports.age = 21;
module.exports.name = 'brokyz';
// 由于二者指向的对象是同一个内存地址的对象，所以两个属性都被添加上了
```

1. 第二种情况

```jsx
exports = {
	age: 21;
}

module.exports.name = 'brokyz';
// 由于exports更改了内存地址，指向了新的对象。但是最终结果以，module.exports为主，所以返回的里面只有name
```

1. 第三种情况

```jsx
exports.age = 21;

module.exports = {
	name: 'brokyz'
};
// module.exports指向了新的对象，以module.exports为准。
```

# npm 与 包

## 概述

Node.js 中的**第三方模块**又叫做**包**。

Node.js 中的包都是免费且开源的，不需要付费即可下载使用。

**包都是基于内置模块封装出来的**，提供了更高级、更方便的 API，**极大的提高了开发效率**。

npm 是全球最大的包共享平台，我们可以通过 https://www.npmjs.com，对包进行查找。

注意：

- 从 https://www.npmjs.com/ 网站上搜索自己所需要的包。
- 从 https://registry.npmjs.org/ 服务器上下载自己需要的包。

npm 公司还为我们提供了一个包管理工具，Node Package Manager（也就是我们所说的 `npm包管理工具`），可以使用 npm -v 查看版本。

## 包管理配置文件 package.json

npm 规定，在项目的根目录，必须提供一个叫做 package.json 的包管理配置文件。用来记录与**项目有关的一些配置信息**。如：

- 项目的名称、版本号、描述等。
- 项目中都用到了哪些包。
- 哪些包只在开发期间会使用到。
- 哪些包在开发和部署时都会用到。

## 包分类

在 Node.js 中包可以分为两种，分别是项目包和全局包。项目包中又有开发依赖包和核心依赖包。

- 项目包：被安装在项目目录中的包，仅在当前项目可以使用。
  - 开发依赖包：被记录到 devDependencies 节点中的包。只在开发期间会用到。
  - 核心依赖包：被记录到 dependencies 节点中的包。在开发期间和部署上线之后都会用到的包。
- 全局包：被安装到计算机全局的包。默认会被安装到 C:\Users\用户目录\AppData\Roaming\npm\node_modules 目录下。安装后在当前计算机全局可用。

## npm i 安装包

npm i 或者 npm install 都是安装包。

包安装成功后，会在项目文件夹生成一个 `node_modules` 文件夹和 `package-lock.json` 的配置文件。

```jsx
# 简写
npm i moment
# 详细
npm install moemnt
# 指定版本号
npm i moment@2.22.2
# 将包安装为开发包
npm i webpack -D
# 将包安装到全局
npm i webpack -g
```

- `node_modules` 文件夹用于存放所有安装到项目中的包。require() 导入第三方包时，就是从这个目录查找并加载包的。
- `package-lock.json` 配置文件用来记录 node_modules 目录下的每一个包的下载信息，比如包名、版本号、下载地址等。
- 关于包的版本号：包的版本号是以“点分十进制”形式定义的，总共有三位数字，例如 2.22.2
  - 第1位数字代表大版本。
  - 第2位数字代表功能版本。
  - 第3位数字代表Bug修复版本。
  - 只要前面的数字增长，后面的数字就要清零。

## 自动生成 package.json

注意：自动生成命令**只能在英文目录**下成功运行。**不能出现中文和空格**。

```jsx
npm init
npm init -y
```

- 带有 -y 参数时，创建时采取默认选项。如不带有参数，那么会在终端进行相应地设置，如设置项目名称，版本等。
- 注意，如果没有 package.json 时，如果进行第三方包安装也会生成此文件，但是里面只记录了所依赖的第三方包，并没有其它记录，如没有项目名，版本号等。这时如果我们自动生成配置文件是，并不会覆盖我们之前的依赖。所以可以在中途生成配置文件。

示例：

```jsx
{
  "name": "testnpm", // 项目名
  "version": "1.0.0", // 项目版本号
  "description": "", // 项目描述
  "main": "test.js", // 包的入口文件
  "scripts": {
    "test": "echo \\"Error: no test specified\\" && exit 1"
  },
  "keywords": [], // 关键词，发布时用于搜索
  "author": "", // 作者
  "license": "ISC", // 所遵守的开源许可协议
  "dependencies": { // 开发和部署时用到的第三方包
    "jQuery": "^1.7.4",
    "moment": "^2.29.4"
  },
  "devDependencies": { // 开发时用到的第三方包，部署时不需要
    "webpack": "^5.74.0"
  }
}
```

## **多人协作问题**

当我们使用的第三方包比较多时，整个项目的体积就会比较大，其中大部分空间都是由第三方包占据，项目源代码占据的体积较小。

为了方便团队成员之间共享源代码，我们在共享的时候就需要剔除 node_modules 文件夹。比如 git 上传时，在 .gitignore 中忽略此文件夹。

**由于存在 package.json 配置文件，我们拉取到代码时，仅仅需要执行 `npm i` 就会自动为我们安装配置文件中所存在的包。**

这样就优化了成员之间协作的效率。

## npm uninstall 卸载包

```jsx
npm uninstall moment
```

- 当卸载响应的包之后，会自动从 package.json 的 dependencies 中移除掉。

## devDependencies 节点

如果某些包只在开发阶段用到，部署项目是不需要用到，那么建议将包记录到 devDependencies 节点下面。

如果有些包，在开发和部署上线时都需要用到，那么需要将包记录到 dependencies 节点中。

可使用如下命令记录到开发节点

```jsx
// 简写
npm i webpack -D
// 详细
npm install webpack --save-dev
```

## 下包速度慢

在使用 npm 下包时，默认从国外的 npm 官方服务器进行下载，由于服务器在国外，所以下载速度满。

我们可以使用淘宝的 npm 镜像服务器。淘宝提供了一个服务器，每隔一段时间会专门把国外 npm 官方服务器中的包同步到自己的服务器，并对外提供下载。这样就提高下载的速度。也就是说，这时一种npm 服务器的镜像站。

**切换 npm 镜像源**

```jsx
# 查看当前的下包镜像源
npm config get registry
# 将下包镜像源改为淘宝
npm config set registry=https://registry.npm.taobao.org/
```

**nrm 镜像切换工具**

为了方便切换下包的镜像源，我们可以使用 nrm 工具，它提供了命令，可以快速查看和切换下包镜像源。

由于我们需要经常使用，所以将其安装到全局 -g

```jsx
# 将nrm安装到全局
npm install nrm -g
# 查看所有可用镜像源
nrm ls
# 将下包镜像源切换为 taobao
nrm use taobao
```

## 规范的包结构

一个规范的包，组成结构必须符合以下 3 点要求：

1. 包必须以单独目录存在。
2. 包的顶级目录下必须包含 package.json 这个包管理配置文件
3. package.json 中必须包含 name, version, main 三个属性，分别代表包的名字、版本号、包的入口。

## 开发自己的包

初始化包的结构：

1. 新建项目文件夹，作为包的根目录
2. 在项目中新建如下文件
   1. package.json 包管理配置文件
   2. index.js 包的入口文件
   3. [README.md](http://README.md) 包的说明文档

注意：

- 在包管理配置文件中的 name 才是包的真正名字，而不是文件夹名。
- 包名不能和已发布的包重名，提前去 npm 官网查询。

包中的 package.json

```jsx
{
  "name": "myPackage", // 包名
  "version": "1.0.0", // 包版本号
  "description": "测试包发布", // 包描述
  "main": "index.js", // 包的入口文件
  "keywords": ["brokyz","myPackage"], // 关键词，发布时用于搜索
  "license": "ISC", // 所遵守的开源许可协议
}
```

### 模块化拆分

将不同的模块写入到不同的 js 文件，然后在包的入口文件中进行调用和暴露

例 index.js

```jsx
const data = require('./src/dataFormat');
const escape = require('./src/htmlExcape');

module.exports = {
// es6 展开运算符，将data对象中的所有属性和方法展开暴露
	...data,
	...escape
}
```

### 包的说明文档

包的说明文档中一般包含以下：

安装方式、导入方式、各种功能的用法、开源协议等

### 发布包

1. 去npm官网注册账号
2. **在终端使用 npm login 进行登录。在登陆前必须将 npm 源地址转换为 npm 官方服务器。**
3. 将终端切换到**包的根目录**后，运行 npm publish 包名，即可把包发布到 npm 上。

### 删除已发布的包

```jsx
npm unpublish 包名 --force
```

- npm unpublish 命令只能删除 72 小时以内发布的包，超过事件时就永远不能删除了
- 删除的包在 24 小时内不允许重复发布。
- 尽量不要发布没有意义的包。

## 模块的加载机制

模块在**第一次 require() 之后会被加载进缓存**。这也就意味着多次调用同一个包，不会导致模块的代码被多次执行。

注意：无论是内置模块、用户自定义模块、第三方模块，他们都会优先从缓存中加载，从而**提高模块的加载效率**。

### 内置模块的加载机制

内置模块的加载机制是优先级最高的。

### 自定义模块的加载机制

使用 require() 加载自定义模块时，必须指定以 `./` 或 `../` 开头的路径标识符。如果没有指定，node 会将其作为内置模块或第三方模块进行加载。

同时，如果 require() 导入模块时，如果省略了扩展名，Node.js 会按顺序加载如下文件：

1. 按照**确切的文件名**进行加载
2. 补全 .js 扩展名加载
3. 补全 .json 扩展名加载
4. 补全 .node 扩展名加载
5. 加载失败，终端报错

### 第三方模块加载机制

如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ./ 和 ../ 开头，那么 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块。

如果没有找到对应的第三方模块，则移动到上一层父目录中进行加载，直到文件系统的根目录。

例如，假设在 C:\Users\brokyz\project\foo.js 文件里面调用了 require(’tools’)，那么 Node.js 会做如下查找。

1. C:\Users\brokyz\project\node_modules\tools
2. C:\Users\brokyz\node_modules\tools
3. C:\Users\node_modules\tools
4. C:\node_modules\tools

### 目录作为模块

当把目录作为模块标识符，传递给 require() 加载时，有三种加载方式：

1. 在加载目录下查找 package.json 文件，并找到 main 属性，作为入口加载。
2. 如果没有 package.json 文件，或者出错，则 Node.js 会试图加载目录下的 index.js 文件。
3. 如果上面都失败了，Node.js 会打印报错信息。

# express 框架

官方给出的概念：Express 是基于 Node.js 平台，快速、开放、极简的 Web 开发框架。

通俗的理解：Express 的作用和 Node.js 内置的 http 模块类似，是专门用来创建 Web 服务器。但是 Express 为我们封装了很多方法，能够极大的提高我们的开发效率。

Express 的本质：就是一个 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法。

## 安装

```jsx
npm i express
```

## 创建基本web服务器

```jsx
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

```jsx
app.get('url', function(req, res) {
	res.send('success');
})
```

监听post请求

```jsx
app.post('url', function(req, res) {
	res.send('success');
})
```

- url：需要被请求的地址。
- res.send()：响应的内容。

## 获取 url 携带的查询参数

通过 req.query 对象，可以访问到客户端通过查询字符串的形式，发送到服务器的参数。

```jsx
app.get('/get', function (req, res) {
  res.send(req.query);
});
```

请求地址 http://127.0.0.1:3000/post?name=brokyz&age=21

返回 `{"name":"brokyz","age":"21"}`

如果不传入查询参数，那么 req.query 是空对象

## 获取 url 中的动态参数

如果我们想要获取请求 url 中的某个位置的参数，可以使用 req.params 对象。

```jsx
app.get('/get/:id/test', function (req, res) {
  res.send(req.params);
});
```

请求地址 http://127.0.0.1:3000/get/123456/test/234

返回 `{"id":"123456","id2":"234"}`

## express.static() 托管静态资源

通过 express.static() 我们可以非常方便地创建一个静态资源服务器。

例如通过以下代码，就可以将 public 目录下的图片、CSS文件、JavaScript文件对外公开访问了。

```jsx
app.use(express.static('public'))
```

此时就可以访问：

http://127.0.0.1:3000/images/bg.jpg

http://127.0.0.1:3000/images/bg.css

http://127.0.0.1:3000/images/bg.js

http://127.0.0.1:3000/images/bg.html

**注意：express在指定静态目录中查找文件，并对外提供资源访问路径。因此，存放静态文件的目录名不会出现在 URL 中。**

托管多个静态资源目录：

```jsx
app.use(express.static('public'))
app.use(express.static('files'))
```

- 当添加多个静态目录时，如果两个目录中用同名文件，**会根据添加路径的顺序查找所需要的文件**，因此会显示第一个查找到的文件。

挂载访问路径前缀：

```jsx
app.use('/test', express.static('public'))
```

此时就可以访问：

http://127.0.0.1:3000/test/images/bg.jpg

http://127.0.0.1:3000/test/images/bg.css

http://127.0.0.1:3000/test/images/bg.js

http://127.0.0.1:3000/test/images/bg.html

## 路由