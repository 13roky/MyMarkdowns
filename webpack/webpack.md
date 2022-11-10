# 一、概述

## 1. webpack 的 5 大核心

1. entry（入口）

提示 wenpack 从哪个文件开始进行打包。

2. output（输出）

提示 webpack 打包完的文件输出到哪里去，如何命名等…

3. loader（加载器）

webpack 本身只能够处理 js、json 等资源，其他资源，如 less 需要借助 loader 才可以进行解析。

4. plugins（插件）

扩展 webpack 的功能。

5. mode （模式）

webpack 的工作模式：

- 开发模式：development
- 生产模式：production

# 二、hello webpack

## 1. webpack 项目结构

1. 初始化项目的 package.json 文件。

```npm
npm init -y
```

2. 安装 webpack、webpack-cli。

```npm
npm i webpack webpack-cli -D
```

- webpack：我们项目中需要的 webpack 本体。
- webpack-cli：支持我们在终端运行 webpack 的相关命令。

3. 创建出如下项目结构

- project
  - dist
  - node_modules
  - public
    - index.html
  - src
    - js
    - main.js
  - package-lock.json
  - package.json

## 2. 打包 webpack

1. 将 package.json 中的 main 入口文件该文 `/src/main.js`。
2. 使用 npx 命令运行 webpack 打包命令。

```npm
// 开发模式
npx webpack ./src/main.js --mode=development
// 生产模式
npx webpack ./src/main.js --mode=production
```

- npx 可以临时将当前代码环境变量改到 `/node_modules/.bin/` 目录下。

# 三、webpack 配置文件

1. 手动在项目根目录创建 webpack 的配置文件 `webpack.config.js`。

webpack.config.js

```js
// node.js 核心模块，专门用来处理路径
const path = require("path")

module.exports = {
  // 入口：相对路径
  entry: "./src/main.js",
  // 输出
  output: {
    // 文件的输出路径：绝对路径
    path: path.resolve(__dirname, "dist"),
    // 文件名
    filename: "main.js",
    // 打包前将 path 目录清空
    clean: true
  },
  // 加载器
  module: {
    rules: [
      // loader 配置
    ]
  },
  // 插件
  plugins: [
    // plugin 配置
  ],
  // 模式
  mode: "development"
}
```

2. 再次运行 webpack 打包。

```npm
npx webpack
```

- 由于我们配置了 webpack 配置文件，所以这里我们只需要简写。
- wenpack 会先去根目录寻找配置文件，然后根据配置进行打包。

# 四、处理样式资源

## 1. 处理 css 资源

1. 下载 css-loader、style-loader。

```npm
npm i css-loader style-loader -D
```

- style-loader：将 js 中的 css 文件通过创建 style 标签的形式添加到 html 文件中生效。
- css-loader：将 css 资源编译成 commonjs 的模块到 js 中。

2. 在 `webpack.config.js` 中配置上已经安装好的 loader。

```js
module: {
  rules: [
    // loader 配置
    {
      test: /\.css$/, // 检测 .css 文件
      use: ["style-loader", "css-loader"]
    }
  ]
}
```

## 2. 处理 less 资源

1. 下载 less、less-loader。

```npm
npm i less less-loader -D
```

2. 在 `webpack.config.js` 中配置上已经安装好的 loader。

```js
module: {
  rules: [
    {
      test: /\.less$/,
      use: ["style-loader", "css-loader", "less-loader"]
    }
  ]
}
```

- less-loader：将 less 文件编译成 css 文件。
- 之后将编译好的 css 文件继续使用 css loader 处理，所以也需要 style-loader、css-loader。

## 3. 处理 sass 资源

1. 下载 sass、sass-loader。

```npm
npm i sass sass-loader -D
```

2. 在 `webpack.config.js` 中配置上已经安装好的 loader。

```js
module: {
  rules: [
    {
      test: /\.s[ac]ss$/,
      use: ["style-loader", "css-loader", "sass-loader"]
    }
  ]
}
```

- sass 文件有两种命名方式：.sass、.scss。

## 4. 处理 stylus 资源

1. 下载 stylus、stylus-loader。

```npm
npm i stylus stylus-loader -D
```

2. 在 `webpack.config.js` 中配置上已经安装好的 loader。

```js
module: {
  rules: [
    {
      test: /\.styl$/,
      use: ["style-loader", "css-loader", "stylus-loader"]
    }
  ]
}
```

# 五、处理图片资源

在 webpack4 时，我们处理图片资源通过 file-loader 和 url-loader 进行处理。

webpack5 已经将这两个 loader 功能内置到 webpack 中，我们只需要简单配置即可处理图片资源。

```js
module: {
  rules: [
    {
      test: /\.(png|jpe?g|gif|webp)$/,
      type: "asset",
      parser: {
        dataUrlCondition: {
          // 小于 10kb 的图片转 base64
          // 优点： 减少请求数量；缺点：体积变大
          maxSize: 10 * 1024
        }
      },
      generator: {
        // 设置图片资源的文件地址，文件名指定10位哈希值加扩展名
        filename: "static/images/[hash:10][ext][query]"
      }
    }
  ]
}
```

# 六、处理其他资源

当需要原封不动的处理一些资源时，使用以下配置。

```js
{
  test: /\.(tff|woff2?|mp3|mp4|avi)$/,
  type: "asset/resource",
  generator: {
    filename: "static/media/[hash:10][ext][query]"
  }
}
```

# 七、处理 js 资源

我们可以使用 babel 来将我们 js 中 es6 的语法转换成浏览器兼容的 js 语法。

**下载 babel**

```
npm i babel-loader @babel/core @babel/preset-env -D
```

其中 @babel/preset-env 为 babel 的预设，可以理解为 babel 的插件，用来拓展 babel 的功能。

- `@babel/preset-env`: 一个智能预设，允许您使用最新的 JavaScript。
- `@babel/preset-react`：一个用来编译 React jsx 语法的预设
- `@babel/preset-typescript`：一个用来编译 TypeScript 语法的预设

根据自己的需要选择对应的预设。

**在根目录创建 babel 配置文件：`./babel.config.js`**

```js
module.exports = {
  presets: ["@babel/preset-env"]
}
```

**在 webpack 配置文件中使用 babel**

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        // exclude: /node_modules/, // 排除node_modules代码不编译
        path.resolve(__dirname, "src")
        loader: "babel-loader"
      }
    ]
  }
}
```

# 八、CSS 处理

## 1. 提取 CSS 单文件

webpack 使用 loader 处理 css 文件时，会将 css 样式打包进 js 文件中，当 js 文件被加载时，会在 html 页面中动态添加一个 style 来生成样式。

但是由于 js 需要被加载后，css 才得以显示，这就导致了闪屏现象，影响用户体验。

因此我们需要使用插件 `mini-css-extract-plugin` 将 css 样式打包成单独的 css 文件，以 link 的形式引入，解决闪屏现象。

**安装**

```
npm i mini-css-extract-plugin -D
```

**配置**

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin")

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
      },
      {
        test: /\.less$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "less-loader"]
      } 
    ]
  },
  plugins: [
    // 提取css成单独文件
    new MiniCssExtractPlugin({
      // 定义输出文件名和目录
      filename: "static/css/main.css"
    })
  ]
}

```

- 使用插件提供的 loader 来替代我们之前使用的 style-loader。
- 我们所有的不同 css 文件中的样式都会被打包输出到一个我们指定的 `static/css/main.css` 文件中。

## 2. CSS 代码压缩

**安装**

```
npm i css-minimizer-webpack-plugin -D
```

**配置**

```js
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")

module.exports = {
  plugins: [
    // css压缩
    new CssMinimizerPlugin()
  ]
}
```

## 3. CSS 兼容性处理

我们可以使用 postcss 来处理 css 样式的一些兼容性问题。

**安装**

```
npm i postcss-loader postcss postcss-preset-env -D
```

**配置**

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader",
          {
            loader: "postcss-loader",
            options: {
              postcssOptions: {
                plugins: [
                  "postcss-preset-env" // 能解决大多数样式兼容性问题
                ]
              }
            }
          }
        ]
      }
  }
}
```

**兼容性设置**

我们可以在 `package.json` 中指定我们需要的兼容性。

```json
{
  // 兼容 ie8 及以上
  "browserslist": ["ie >= 8"]
}
```

实际开发中我们可以做如下设置

```json
{
  // 兼容市面上99%的浏览器的最新版的前两个版本。只兼容还在使用的浏览器。
  "browserslist": ["last 2 version", "> 1%", "not dead"]
}
```

**合并简写配置**

```js
// 获取处理样式的Loaders(通用简化)
const getStyleLoaders = (preProcessor) => {
  return [
    MiniCssExtractPlugin.loader,
    "css-loader",
    {
      loader: "postcss-loader",
      options: {
        postcssOptions: {
          plugins: [
            "postcss-preset-env" // 能解决大多数样式兼容性问题
          ]
        }
      }
    },
    preProcessor
  ].filter(Boolean)
}

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: getStyleLoaders()
      },
      {
        test: /\.less$/,
        use: getStyleLoaders("less-loader")
      }
  }
}
```

优化版

```js
// 获取处理样式的Loaders(通用简化)
function getStyleLoaders() {
  return [
    MiniCssExtractPlugin.loader,
    "css-loader",
    {
      loader: "postcss-loader",
      options: {
        postcssOptions: {
          plugins: [
            "postcss-preset-env" // 能解决大多数样式兼容性问题
          ]
        }
      }
    }
  ]
    .concat(...arguments)
    .filter(Boolean)
}

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: getStyleLoaders()
      },
      {
        test: /\.less$/,
        use: getStyleLoaders("less-loader")
      }
  }
}
```



# 九、开发服务器

开发服务器允许我们实时监控我们的变换并展现。

**安装**

```
npm i webpack-dev-server -D
```

**使用**

```js
module.exports = {
  devServer: {
    host: "localhost", // 启动服务器域名
    port: "3000", // 启动服务器端口号
    open: true, // 是否自动打开浏览器
  },
  mode: "development"
}
```

**运行指令**

```
npx webpack serve
```

# 十、源代码映射

SourceMap（源代码映射）是一个用来生成源代码与构建后代码一一映射的文件的方案。

它会生成一个 xxx.map 文件，里面包含源代码和构建后代码每一行、每一列的映射关系。当构建后代码出错了，会通过 xxx.map 文件，从构建后代码出错位置找到映射后源代码出错位置，从而让浏览器提示源代码文件出错位置，帮助我们更快的找到错误根源。

```js
module.exports = {
  devtool: "source-map"
  // devtool: "cheap-module-source-map"
}
```

- 开发模式：`cheap-module-source-map`
  - 优点：打包编译速度快，只包含行映射
  - 缺点：没有列映射
- 生产模式：`source-map`
  - 优点：包含行/列映射
  - 缺点：打包编译速度更慢

# 十一、提升打包速度

## 1. HRM 热模块替换

当我们在开发模式中修改了代码时，webpack 默认会将所有模块重新打包，影响效率。

而 HRM 就允许我们修改某个模块的代码时，就只有这个模块的代码需要重新打包编译，其它模块不做处理，无需重新加载整个页面，这样就大大提升了打包效率。

**使用**

```javascript
module.exports = {
  // 其他省略
  devServer: {
    host: "localhost", // 启动服务器域名
    port: "3000", // 启动服务器端口号
    open: true, // 是否自动打开浏览器
    hot: true, // 开启HMR功能（只能用于开发环境，生产环境不需要了）
  }
}
```

- 此时 css 样式经过 style-loader 处理，已经具备 HMR 功能了。 但是 js 还不行。

支持js

```js
// main.js
import count from "./js/count";
import sum from "./js/sum";

// 判断是否支持HMR功能
if (module.hot) {
  module.hot.accept("./js/count.js", function (count) {
    const result1 = count(2, 1);
    console.log(result1);
  });

  module.hot.accept("./js/sum.js", function (sum) {
    const result2 = sum(1, 2, 3, 4);
    console.log(result2);
  });
}
```

- 这样写比较繁琐，实际开发中我们都会使用框架，vue-loader 或 react-hot-loader 会解决此情况。

## 2. oneOf

在进行打包时，每个文件类型都会一一和`test`进行正则匹配，即使匹配上了，也会继续匹配后面的规则，这样就比较影响效率。

我们使用 oneOf 后，当匹配上一个规则时，就不会向下继续进行匹配了。

**使用**

```js
module.exports = {
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.css$/,
            use: [MiniCssExtractPlugin.loader, "css-loader"]
          },
          {
            test: /\.less$/,
            use: [MiniCssExtractPlugin.loader, "css-loader", "less-loader"]
          }
        ]
      }
    ]
  }
}
```

## 3. include / exclude

在 loader 和 plugins 对文件进行处理时，我们往往需要排除和指定文件位置。

- include：包含，只处理 xxx 文件

- exclude：排除，除了 xxx 文件以外其他文件都处理

**使用**

```js
const path = require("path");
const ESLintWebpackPlugin = require("eslint-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            // exclude: /node_modules/, // 排除node_modules代码不编译
            include: path.resolve(__dirname, "../src"), // 也可以用包含
            loader: "babel-loader",
          },
        ],
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "../src"),
      exclude: "node_modules", // 默认值
    })
  ],
};
```

## 4. cache

每次打包时 js 文件都要经过 Eslint 检查 和 Babel 编译，速度比较慢。

我们可以缓存之前的 Eslint 检查 和 Babel 编译结果，这样第二次打包时速度就会更快了。

**使用**

```js
const path = require("path");
const ESLintWebpackPlugin = require("eslint-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            // exclude: /node_modules/, // 排除node_modules代码不编译
            include: path.resolve(__dirname, "../src"), // 也可以用包含
            loader: "babel-loader",
            ///////////////////////////////////
            options: {
              cacheDirectory: true, // 开启babel编译缓存
              cacheCompression: false, // 缓存文件不要压缩
            },
          },
        ],
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "../src"),
      exclude: "node_modules", // 默认值
      //////////////////////////////////
      cache: true, // 开启缓存
      // 缓存目录
      cacheLocation: path.resolve(
        __dirname,
        "../node_modules/.cache/.eslintcache"
      ),
    }),
  ],
};
```

## 5. thread 多线程打包

当项目非常庞大时，打包的速度非常的慢，因此我们需要提升打包速度。

我们要提升项目打包速度，就需要提升 js 的打包速度，因为其他文件相对较少。

而对 js 进行处理的工具就是 eslint、babel、Terser 三个工具，所以我们要提升这三个工具的速度。

我们可以开启多进程同时处理 js 文件，这样打包速度就更快了。

**注意：需要在项目特别耗时使用，因为每个进程的启动时间就有 600 ms 左右的开销。**

**安装**

```
npm i thread-loader -D
```

**使用**

```js
const os = require("os");
const path = require("path");
const ESLintWebpackPlugin = require("eslint-webpack-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
const TerserPlugin = require("terser-webpack-plugin");

// cpu核数
const threads = os.cpus().length;

module.exports = {
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            // exclude: /node_modules/, // 排除node_modules代码不编译
            include: path.resolve(__dirname, "../src"), // 也可以用包含
            use: [
              {
                loader: "thread-loader", // 开启多进程
                options: {
                  workers: threads, // 数量
                },
              },
              {
                loader: "babel-loader",
                options: {
                  cacheDirectory: true, // 开启babel编译缓存
                },
              },
            ],
          },
        ],
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "../src"),
      exclude: "node_modules", // 默认值
      cache: true, // 开启缓存
      // 缓存目录
      cacheLocation: path.resolve(
        __dirname,
        "../node_modules/.cache/.eslintcache"
      ),
      threads, // 开启多进程
    }),
    // css压缩
    // new CssMinimizerPlugin(),
  ],
  optimization: {
    minimize: true,
    minimizer: [
      // css压缩也可以写到optimization.minimizer里面，效果一样的
      new CssMinimizerPlugin(),
      // 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了
      new TerserPlugin({
        parallel: threads // 开启多进程
      })
    ],
  },
};
```

# 十二、减少代码体积

## 1. Tree Shaking

开发时我们定义了一些工具函数库，或者引用第三方工具函数库或组件库。

如果没有特殊处理的话我们打包时会引入整个库，但是实际上可能我们可能只用上极小部分的功能。

这样将整个库都打包进来，体积就太大了，因此我们使用 Tree Shaking 进行代码移除。

`Tree Shaking` 是一个术语，通常用于描述移除 JavaScript 中的没有使用上的代码。

**Webpack 已经默认开启了这个功能，无需其他配置。**

## 2. babel

Babel 为编译的每个文件都插入了辅助代码，使代码体积过大！

Babel 对一些公共方法使用了非常小的辅助代码，比如 `_extend`。默认情况下会被添加到每一个需要它的文件中。

你可以将这些辅助代码作为一个独立模块，来避免重复引入。

`@babel/plugin-transform-runtime`: 禁用了 Babel 自动对每个文件的 runtime 注入，而是引入 `@babel/plugin-transform-runtime` 并且使所有辅助代码从这里引用。

**安装**

```
npm i @babel/plugin-transform-runtime -D
```

**使用**

```js
module.exports = {
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            // exclude: /node_modules/, // 排除node_modules代码不编译
            include: path.resolve(__dirname, "../src"), // 也可以用包含
            use: [
              {
                loader: "babel-loader",
                options: {
                  cacheDirectory: true, // 开启babel编译缓存
                  cacheCompression: false, // 缓存文件不要压缩
                  plugins: ["@babel/plugin-transform-runtime"], // 减少代码体积
                },
              },
            ],
          },
        ],
      },
    ],
  },
};
```

## 3. 图片压缩

开发如果项目中引用了较多图片，那么图片体积会比较大，将来请求速度比较慢。

我们可以对图片进行压缩，减少图片体积。

**注意：如果项目中图片都是在线链接，那么就不需要了。本地项目静态图片才需要进行压缩。**

**安装**

```
npm i image-minimizer-webpack-plugin imagemin -D
```

模式包：

1. 无损压缩

```
npm install imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemin-svgo -D
```

2. 有损压缩

```
npm install imagemin-gifsicle imagemin-mozjpeg imagemin-pngquant imagemin-svgo -D
```

**无损压缩的使用**

```js
const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

module.exports = {
  optimization: {
    minimizer: [
      // 压缩图片
      new ImageMinimizerPlugin({
        minimizer: {
          implementation: ImageMinimizerPlugin.imageminGenerate,
          options: {
            plugins: [
              ["gifsicle", { interlaced: true }],
              ["jpegtran", { progressive: true }],
              ["optipng", { optimizationLevel: 5 }],
              [
                "svgo",
                {
                  plugins: [
                    "preset-default",
                    "prefixIds",
                    {
                      name: "sortAttrs",
                      params: {
                        xmlnsOrder: "alphabetical",
                      },
                    },
                  ],
                },
              ],
            ],
          },
        },
      }),
    ],
  },
};
```

# 十三、优化代码运行性能

## 1. 代码分割

### 认识 chunk

chunk 翻译过来就是块的意思，在 webpack 中项目中使用到的每个文件都是一个模块，在打包的过程中这些**符合要求的模块会被合并成 chunk**（可以理解为小块合并为大块）。

chunk 有两种形式：

- `initial(初始化)` 是入口起点的 main chunk。此 chunk 包含为入口起点指定的所有模块及其依赖项。
- `non-initial` 是可以延迟加载的块。可能会出现在使用 `import()`语句动态导入的模块或者当模块被多次导入与使用而被`SplitChunksPlugin`进行代码分割时。

每个 chunk 都有对应的 **asset(资源)**。资源，是指输出文件（即打包结果）。

每个入口都对应着一个 chunk 组，chunk 组就是这个入口文件生成的 `initial chunk` 和 `non-initial chunk` 的集合。

**./webpack.config.js**

```js
module.exports = {
  // 多入口
  entry: {
    app: "./src/app.js",
    main: "./src/main.js",
  },
};
```

**./src/app.js**

```js
import { count } from "./js/count"
console.log(count(1, 2, 3))
```

**./src/main.js**

```js
import { count } from "./js/count"
console.log(count(1, 2, 3, 4))
// 动态导入 hello.js
import("./js/hello")
```

如将以上文件打包，会生成一个名为 app 的 initial chunk 和一个名为 main 的 initial chunk。

app 中包含： 

- count 
- 以及其他依赖

main 中包含：

- count 
- 以及除 hello 以外的所有依赖

然后会为 main 中的 hello 创建 non-initial chunk，因为 hello 是被动态引入的。

如果 count 满足条件，触发代码分割时，count 也将成为 non-initial chunk。但是此例中并未满足代码分割条件。

**output：**

- /dist/app.js：一个 initial chunk 对应 app
- /dist/main.js：一个 initial chunk 对应 main
- /dist/394.js：一个 non-initial chunk 对应 hello

默认情况下，这些 `non-initial` chunk 没有名称，因此会使用唯一 ID 来替代名称。 在使用动态导入时，我们可以通过使用 magic comment（魔术注释）来显式指定 chunk 名称：

```js
import(/* webpackChunkName: "hello" */ "./js/hello")
```

**output：**

- /dist/app.js：一个 initial chunk 对应 app
- /dist/main.js：一个 initial chunk 对应 main
- /dist/hello.js：一个 non-initial chunk 对应 hello

**输出资源名称**

输出文件的名称会受配置中的两个字段的影响：

- [`output.filename`](https://webpack.docschina.org/configuration/output/#outputfilename) - 用于 `initial` chunk 文件
- [`output.chunkFilename`](https://webpack.docschina.org/configuration/output/#outputchunkfilename) - 用于**动态导入的** `non-initial` chunk 文件（对代码分割的 chunk 无效）
- 在某些情况下，使用 `initial` 和 `non-initial` 的 chunk 时，可以使用 `output.filename`。

这些字段中会有一些 [占位符](https://webpack.docschina.org/configuration/output/#template-strings)。常用的占位符如下：

- `[id]` - chunk id（例如 `[id].js` -> `485.js`）
- `[name]` - chunk name（例如 `[name].js` -> `app.js`）。如果 chunk 没有名称，则会使用其 id 作为名称
- `[contenthash]` - 输出文件内容的 md4-hash（例如 `[contenthash].js` -> `4ea6ff1de66c537eb9b2.js`）

### SplitChunks

当模块符合一定条件时，会将模块进行代码分割，作为 non-initial chunk 打包出资源文件。

默认触发代码分割的条件：

```js
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async',
      minSize: 20000,	// 分割代码最小的大小 20kb
      minRemainingSize: 0, // 类似于minSize，最后确保提取的文件大小不能为0
      minChunks: 1,	// 至少被引用的次数，满足条件才会代码分割
      maxAsyncRequests: 30,	// 按需加载时并行加载的文件的最大数量
      maxInitialRequests: 30,	// 入口js文件最大并行请求数量
      enforceSizeThreshold: 50000,	// 超过50kb一定会单独打包（此时会忽略minRemainingSize、maxAsyncRequests、maxInitialRequests）
      cacheGroups: {
        // 组，哪些模块要打包到一个组
        defaultVendors: { // 组名
          test: /[\\/]node_modules[\\/]/,	// 需要打包到一起的模块
          priority: -10,	// 权重（越大越高）
          reuseExistingChunk: true,	// 如果当前 chunk 包含已从主 bundle 中拆分出的模块，则它将被重用，而不是生成新的模块
        },
        default: { // 会覆盖掉上面的默认配置
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true,
          // 自定义分割chunk的名字
          name: (module, chunks, cacheGroupKey) => {
 		    const moduleFileName = module
   			  .identifier()
    		  .split("\\")
   			  .reduceRight((item) => item);
 			const splitName = path.basename(moduleFileName, ".js")
            return `${cacheGroupKey}-${allChunksNames}-${splitName}.splitChunk`;
		  },
        },
      },
    },
    // 打包时生成一个体积很小runtime~xxx.js文件，用作映射其他chunk文件，目的是更新后，以较小的代价利用缓存，提升页面加载速度。
    runtimeChunk: {
      name: (entrypoint) => `runtime~${entrypoint.name}`,
    },
  },
};
```

**chunk 文件的命名**

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  // 单入口
  // entry: "./src/main.js",
  // 多入口
  entry: {
    app: "./src/app.js",
    main: "./src/main.js",
  },
  output: {
    path: path.resolve(__dirname, "dist"),
    // 根据入口文件生成的 chunk 组的名字
    filename: "static/js/[name].js", // 入口文件打包输出资源命名方式 initial chunk
    // 动态导入的文件名
    chunkFilename: "static/js/[name].importChunk.js", // 动态导入输出资源命名方式 non-initial chunk
    assetModuleFilename: "static/media/[name].[hash][ext]", // 图片、字体等资源命名方式（注意用hash）
    clean: true,
  },
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            // 小于 10kb 的图片转 base64
            maxSize: 10 * 1024,
          },
        },
        // 设置了 assetModuleFilename 就可以不用在此指定命名
        // generator: {
        //   // 设置图片资源的文件地址，文件名指定10位哈希值加扩展名
        //   filename: "static/images/[hash:10][ext][query]",
        // },
      },
      {
        test: /\.(tff|woff2?|mp3|mp4|avi)$/,
        type: "asset/resource",
        // 设置了 assetModuleFilename 就可以不用在此指定命名
        // generator: {
        //   filename: "static/media/[hash:10][ext][query]",
        // },
      },
      {
        test: /\.js$/,
        exclude: /node_modules/, // 排除node_modules代码不编译
        loader: "babel-loader",
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      // 以 public/index.html 为模板创建文件
      // 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源
      template: path.resolve(__dirname, "public/index.html"),
    }),
    new MiniCssExtractPlugin({
      // 定义输出文件名和目录
      filename: "static/css/[name].css",
      // css 也会生成 chunk
      chunkFilename: "static/css/[name].chunk.css",
    }),
  ],
  optimization: {
    // 代码分割配置
    splitChunks: {
      chunks: "all",
      cacheGroups: {
        default: {
          minSize: 0,  // 进行代码分割的最小文件大小
          minChunks: 2,	// 进行代码分割的最少引用次数
          // 指定输出代码分割的 non-initial chunk 的输出名称 xxx.splitChunk.js
          name: (module) => {
            const moduleFileName = module
              .identifier()
              .split("\\")
              .reduceRight((item) => item);
            const splitName = path.basename(moduleFileName, ".js");
            return `${splitName}.splitChunk`;
          },
        },
      },
    },
    // 打包时生成一个体积很小runtime~xxx.js文件，用作映射其他chunk文件，目的是更新后，以较小的代价利用缓存，提升页面加载速度
    runtimeChunk: {
      name: (entrypoint) => `runtime~${entrypoint.name}`,
    },
  },
  resolve: {
    extensions: [".vue", ".js", ".json"], // 自动补全文件扩展名，让vue可以使用
  },
  // devtool: "source-map",
  mode: "production",
};
```

## 2. preload / prefetch

我们前面已经做了代码分割，同时会使用 import 动态导入语法来进行代码按需加载（我们也叫懒加载，比如路由懒加载就是这样实现的）。

但是加载速度还不够好，比如：是用户点击按钮时才加载这个资源的，如果资源体积很大，那么用户会感觉到明显卡顿效果。

我们想在浏览器空闲时间，加载后续需要使用的资源。我们就需要用上 `Preload` 或 `Prefetch` 技术。

- `Preload`：告诉浏览器立即加载资源。
- `Prefetch`：告诉浏览器在空闲时才开始加载资源。
- 当前页面优先级高的资源用 `Preload` 加载。
- 下一个页面需要使用的资源用 `Prefetch` 加载。

**安装**

```
npm i @vue/preload-webpack-plugin -D
```

**使用**

```js
const PreloadWebpackPlugin = require("@vue/preload-webpack-plugin");

module.exports = {
  plugins: [
    new PreloadWebpackPlugin({
      rel: "preload", // preload兼容性更好
      as: "script",
      // rel: 'prefetch' // prefetch兼容性更差
    }),
  ],
};
```

## 3. Network Cache

将来开发时我们对静态资源会使用缓存来优化，这样浏览器第二次请求资源就能读取缓存了，速度很快。

但是这样的话就会有一个问题, 因为前后输出的文件名是一样的，都叫 main.js，一旦将来发布新版本，因为文件名没有变化导致浏览器会直接读取缓存，不会加载新资源，项目也就没法更新了。

所以我们从文件名入手，确保更新前后文件名不一样，这样就可以做缓存了。

它们都会生成一个唯一的 hash 值。

- fullhash（webpack4 是 hash）

每次修改任何一个文件，所有文件名的 hash 至都将改变。所以一旦修改了任何一个文件，整个项目的文件缓存都将失效。

- chunkhash

根据不同的入口文件(Entry)进行依赖文件解析、构建对应的 chunk，生成对应的哈希值。我们 js 和 css 是同一个引入，会共享一个 hash 值。

- contenthash

根据文件内容生成 hash 值，只有文件内容变化了，hash 值才会变化。所有文件 hash 值是独享且不同的。

**使用**

```js
module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "../dist"), // 生产模式需要输出
    // [contenthash:8]使用contenthash，取8位长度
    filename: "static/js/[name].[contenthash:8].js", // 入口文件打包输出资源命名方式
    chunkFilename: "static/js/[name].[contenthash:8].chunk.js", // 动态导入输出资源命名方式
    assetModuleFilename: "static/media/[name].[hash][ext]", // 图片、字体等资源命名方式（注意用hash）
    clean: true,
  },
  plugins: [
    // 提取css成单独文件
    new MiniCssExtractPlugin({
      // 定义输出文件名和目录
      filename: "static/css/[name].[contenthash:8].css",
      chunkFilename: "static/css/[name].[contenthash:8].chunk.css",
    }),
  ],
};
```

## 4. core.js

过去我们使用 babel 对 js 代码进行了兼容性处理，其中使用@babel/preset-env 智能预设来处理兼容性问题。

它能将 ES6 的一些语法进行编译转换，比如箭头函数、点点点运算符等。但是如果是 async 函数、promise 对象、数组的一些方法（includes）等，它没办法处理。

所以此时我们 js 代码仍然存在兼容性问题，一旦遇到低版本浏览器会直接报错。所以我们想要将 js 兼容性问题彻底解决

`core-js` 是专门用来做 ES6 以及以上 API 的 `polyfill`。

`polyfill`翻译过来叫做垫片/补丁。就是用社区上提供的一段代码，让我们在不兼容某些新特性的浏览器上，使用该新特性。

**使用**

main.js

```javascript
// 添加promise代码
const promise = Promise.resolve();
promise.then(() => {
  console.log("hello promise");
});
```

此时 Eslint 会对 Promise 报错。

修改配置文件

- 下载包

```text
npm i @babel/eslint-parser -D
```

.eslintrc.js

```javascript
module.exports = {
  // 继承 Eslint 规则
  extends: ["eslint:recommended"],
  parser: "@babel/eslint-parser", // 支持最新的最终 ECMAScript 标准
  env: {
    node: true, // 启用node中全局变量
    browser: true, // 启用浏览器中全局变量
  },
  plugins: ["import"], // 解决动态导入import语法报错问题 --> 实际使用eslint-plugin-import的规则解决的
  parserOptions: {
    ecmaVersion: 6, // es6
    sourceType: "module", // es module
  },
  rules: {
    "no-var": 2, // 不能使用 var 定义变量
  },
};
```

运行指令

```text
npm run build
```

此时观察打包输出的 js 文件，我们发现 Promise 语法并没有编译转换，所以我们需要使用 `core-js` 来进行 `polyfill`。

使用`core-js`

- 下载包

```text
npm i core-js
```

**手动全部引入**

```javascript
import "core-js";
import count from "./js/count";
import sum from "./js/sum";
// 引入资源，Webpack才会对其打包
import "./css/iconfont.css";
import "./css/index.css";
import "./less/index.less";
import "./sass/index.sass";
import "./sass/index.scss";
import "./styl/index.styl";

const result1 = count(2, 1);
console.log(result1);
const result2 = sum(1, 2, 3, 4);
console.log(result2);
// 添加promise代码
const promise = Promise.resolve();
promise.then(() => {
  console.log("hello promise");
});
```

这样引入会将所有兼容性代码全部引入，体积太大了。我们只想引入 promise 的 `polyfill`。

**手动按需引入**

```javascript
import "core-js/es/promise";
import count from "./js/count";
import sum from "./js/sum";
// 引入资源，Webpack才会对其打包
import "./css/iconfont.css";
import "./css/index.css";
import "./less/index.less";
import "./sass/index.sass";
import "./sass/index.scss";
import "./styl/index.styl";

const result1 = count(2, 1);
console.log(result1);
const result2 = sum(1, 2, 3, 4);
console.log(result2);
// 添加promise代码
const promise = Promise.resolve();
promise.then(() => {
  console.log("hello promise");
});
```

只引入打包 promise 的 `polyfill`，打包体积更小。但是将来如果还想使用其他语法，我需要手动引入库很麻烦。

**自动按需引入**

main.js

```javascript
// 添加promise代码
const promise = Promise.resolve();
promise.then(() => {
  console.log("hello promise");
});
```

babel.config.js

```javascript
module.exports = {
  // 智能预设：能够编译ES6语法
  presets: [
    [
      "@babel/preset-env",
      // 按需加载core-js的polyfill
      { useBuiltIns: "usage", corejs: { version: "3", proposals: true } },
    ],
  ],
};
```

此时就会自动根据我们代码中使用的语法，来按需加载相应的 `polyfill` 了。

## 5. PWA

开发 Web App 项目，项目一旦处于网络离线情况，就没法访问了。

我们希望给项目提供离线体验。

渐进式网络应用程序(progressive web application - PWA)：是一种可以提供类似于 native app(原生应用程序) 体验的 Web App 的技术。

其中最重要的是，在 **离线(offline)** 时应用程序能够继续运行功能。

内部通过 Service Workers 技术实现的。

**下载包**

```text
npm i workbox-webpack-plugin -D
```

**修改配置文件**

```javascript
const WorkboxPlugin = require("workbox-webpack-plugin");

module.exports = {
  plugins: [
    new WorkboxPlugin.GenerateSW({
      // 这些选项帮助快速启用 ServiceWorkers
      // 不允许遗留任何“旧的” ServiceWorkers
      clientsClaim: true,
      skipWaiting: true,
    }),
  ],
};
```

修改 main.js

```javascript
// 开启PWA
if ("serviceWorker" in navigator) {
  window.addEventListener("load", () => {
    navigator.serviceWorker
      .register("/service-worker.js")
      .then((registration) => {
        console.log("SW registered: ", registration);
      })
      .catch((registrationError) => {
        console.log("SW registration failed: ", registrationError);
      });
  });
}
```

**运行指令**

```text
npm run build
```

此时如果直接通过 VSCode 访问打包后页面，在浏览器控制台会发现 `SW registration failed`。

因为我们打开的访问路径是：`http://127.0.0.1:5500/dist/index.html`。此时页面会去请求 `service-worker.js` 文件，请求路径是：`http://127.0.0.1:5500/service-worker.js`，这样找不到会 404。

实际 `service-worker.js` 文件路径是：`http://127.0.0.1:5500/dist/service-worker.js`。

**解决路径问题**

- 下载包

```text
npm i serve -g
```

serve 也是用来启动开发服务器来部署代码查看效果的。

- 运行指令

```text
serve dist
```

此时通过 serve 启动的服务器我们 service-worker 就能注册成功了。

# 十四、Vue 脚手架

项目架构

```
webpack_vuecli
    |--dist
    |--public
    |	|--index.html
	|--src
	|	|--components
	|	|--App.vue
	|	|--main.js
	|--babel.config.js
	|--package.json (npm init -y)
	|--webpack.config.js
		
```

webpack.config.js

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
const TerserWebpackPlugin = require("terser-webpack-plugin");
const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");
const { VueLoaderPlugin } = require("vue-loader");
const { DefinePlugin } = require("webpack");
const CopyPlugin = require("copy-webpack-plugin");

// 判断当前运行模式
const isProduction = process.env.NODE_ENV === "production";

function getStyleLoaders() {
  return [
    isProduction ? MiniCssExtractPlugin.loader : "vue-style-loader",
    "css-loader",
    {
      loader: "postcss-loader",
      options: {
        postcssOptions: {
          plugins: [
            "postcss-preset-env", // 能解决大多数样式兼容性问题
          ],
        },
      },
    },
  ]
    .concat(...arguments)
    .filter(Boolean);
}

module.exports = {
  entry: "./src/main.js",
  output: {
    path: isProduction ? path.resolve(__dirname, "./dist") : undefined,
    filename: isProduction
      ? "static/js/[name].[contenthash:10].js"
      : "static/js/[name].js",
    chunkFilename: isProduction
      ? "static/js/[name].[contenthash:10].chunk.js"
      : "static/js/[name].chunk.js",
    assetModuleFilename: "static/js/[hash:10][ext][query]",
    clean: isProduction,
  },
  module: {
    rules: [
      {
        test: /\.css$/, // 用来匹配 .css 结尾的文件
        use: getStyleLoaders(), // use 数组里面 Loader 执行顺序是从右到左
      },
      {
        test: /\.less$/,
        use: getStyleLoaders("less-loader"),
      },
      {
        test: /\.js$/,
        include: path.resolve(__dirname, "./src"),
        loader: "babel-loader",
        options: {
          cacheDirectory: true,
          cacheCompression: false,
          plugins: [
            // "@babel/plugin-transform-runtime" // babel.config.js presets中包含了
          ],
        },
      },
      {
        test: /\.vue$/,
        loader: "vue-loader", // 内部会给vue文件注入HMR功能代码
        options: {
          // 开启缓存
          cacheDirectory: path.resolve(
            __dirname,
            "node_modules/.cache/vue-loader"
          ),
        },
      },
      {
        test: /\.(png|jpe?g|gif|svg)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
      },
      {
        test: /\.(ttf|woff2?)$/,
        type: "asset/resource",
      },
    ],
  },
  plugins: [
    // 将 html 模板打包到打包目录
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, "./public/index.html"),
    }),
    // 打包时将指定文件复制到打包目录
    new CopyPlugin({
      patterns: [
        {
          from: path.resolve(__dirname, "./public"),
          to: path.resolve(__dirname, "./dist"),
          toType: "dir",
          noErrorOnMissing: true,
          globOptions: {
            ignore: ["**/index.html"], // 忽略 HtmlWebpackPlugin 已经处理的 index.html
          },
          info: {
            minimized: true,
          },
        },
      ],
    }),
    // css 提取为单文件
    isProduction &&
      new MiniCssExtractPlugin({
        filename: "static/css/[name].[contenthash:10].css",
        chunkFilename: "static/css/[name].[contenthash:10].chunk.css",
      }),
    // 启用 vue-loader
    new VueLoaderPlugin(),
    // 定义环境变量，消除 vue 控制台提示
    new DefinePlugin({
      __VUE_OPTIONS_API__: "true",
      __VUE_PROD_DEVTOOLS__: "false",
    }),
  ].filter(Boolean),
  optimization: {
    // 生产模式进行压缩
    minimize: isProduction,
    // 压缩的操作
    minimizer: [
      new CssMinimizerPlugin(),	// 压缩 css
      new TerserWebpackPlugin(),	// 压缩 js
      new ImageMinimizerPlugin({	// 无损压缩图片资源
        minimizer: {
          implementation: ImageMinimizerPlugin.imageminGenerate,
          options: {
            plugins: [
              ["gifsicle", { interlaced: true }],
              ["jpegtran", { progressive: true }],
              ["optipng", { optimizationLevel: 5 }],
              [
                "svgo",
                {
                  plugins: [
                    "preset-default",
                    "prefixIds",
                    {
                      name: "sortAttrs",
                      params: {
                        xmlnsOrder: "alphabetical",
                      },
                    },
                  ],
                },
              ],
            ],
          },
        },
      }),
    ],
    // 代码分割 non-initial chunk
    splitChunks: {
      chunks: "all",
    },
    runtimeChunk: {
      name: (entrypoint) => `runtime~${entrypoint.name}`,
    },
  },
  resolve: {
    // 自动补齐文件后缀
    extensions: [".vue", ".js", ".json"],
    alias: {
      // 路径别名
      "@": path.resolve(__dirname, "./src"),
    },
  },
  devServer: {
    open: true,
    host: "localhost",
    port: 3000,
    hot: true,
    compress: true,
    historyApiFallback: true, // 解决vue-router刷新404问题
  },
  mode: isProduction ? "production" : "development",
  devtool: isProduction ? "source-map" : "cheap-module-source-map",
};
```

**安装相关依赖**

```
npm i webpack -D webpack-cli webpack-dev-server css-loader style-loader less less-loader babel-loader @babel/core @vue/cli-plugin-babel mini-css-extract-plugin css-minimizer-webpack-plugin postcss-loader postcss postcss-preset-env html-webpack-plugin copy-webpack-plugin vue-loader vue-style-loader vue-template-compiler
```

图片处理相关插件需要翻墙安装，否则报错，也可以通过 cnpm 安装

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

```
npm i image-minimizer-webpack-plugin imagemin -D
```

```
cnpm install imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemin-svgo -D
```

package.json

```json
{
  "name": "webpack-vue",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "npx webpack",
    "serve": "npx webpack serve"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.19.3",
    "@babel/preset-env": "^7.19.4",
    "@vue/cli-plugin-babel": "^5.0.8",
    "babel-loader": "^8.2.5",
    "copy-webpack-plugin": "^11.0.0",
    "css-loader": "^6.7.1",
    "css-minimizer-webpack-plugin": "^4.2.2",
    "html-webpack-plugin": "^5.5.0",
    "image-minimizer-webpack-plugin": "^3.6.1",
    "imagemin": "^8.0.1",
    "imagemin-gifsicle": "^7.0.0",
    "imagemin-jpegtran": "^7.0.0",
    "imagemin-optipng": "^8.0.0",
    "imagemin-svgo": "^10.0.1",
    "less": "^4.1.3",
    "less-loader": "^11.1.0",
    "mini-css-extract-plugin": "^2.6.1",
    "postcss": "^8.4.18",
    "postcss-loader": "^7.0.1",
    "postcss-preset-env": "^7.8.2",
    "style-loader": "^3.3.1",
    "vue-loader": "^17.0.0",
    "vue-style-loader": "^4.1.3",
    "vue-template-compiler": "^2.7.13",
    "webpack": "^5.74.0",
    "webpack-cli": "^4.10.0",
    "webpack-dev-server": "^4.11.1"
  },
  "dependencies": {
    "vue": "^3.2.41"
  },
  "browserslist": [
    "last 2 version",
    "> 1%",
    "not dead"
  ]
}
```

babel.config.js

```js
module.exports = {
  presets: ["@vue/cli-plugin-babel/preset"],
};
```

