#### 什么是`webpack`
`webpack`是一个打包模块化`javascript`工具，在`webpack`里一切文件皆模块，通过`loader`转换文件，通过`plugin`注入钩子，最后输出由多个模块组合成的文件。
#### 介绍下 `webpack` 热更新原理，是如何做到在不刷新浏览器的前提下更新页面
1.当修改了一个或多个文件；<br>
2.文件系统接收更改并通知`webpack`；<br>
3.`webpack`重新编译构建一个或多个模块，并通知`HMR`服务器进行更新；<br>
4.`HMR Server` 使用`webSocket`通知`HMR runtime` 需要更新，`HMR`运行时通过`HTTP`请求更新`jsonp`；<br>
5.`HMR`运行时替换更新中的模块，如果确定这些模块无法更新，则触发整个页面刷新。
#### `webpack` 工作过程
`webpack`起来在启动后会从`Entry`里配置的`Module`开始，递归解析`Entry`依赖的所有`Module`。每找到一个`Module`，就会根据配置的`Loader`去找出对应的转换规则，对`Module`进行转换后，再解析出当前`Module`依赖的`Module`。这些模块会以`Entry`为单位进行分组，一个`Entry`及其所有依赖的`Module`被分到了一个组也就是一个`Chunk`。最后，`webpack`会将所有`Chunk`转换成文件输出。在整个流程中，`webpack`会在恰当的时机执行`plugin`里定义的逻辑。

#### babel 配置 react 版本，兼容浏览器版本
```js
presets: [
    require.resolve('@babel/preset-react'),
    [
      require.resolve('@babel/preset-env'),
      {
        // "modules": false,
        "modules": "commonjs",
        "targets": {
          "browsers": ["last 4 versions", "ie >= 9", "safari >= 10"] // "last 2 Chrome versions",
        },
        "useBuiltIns": "entry",
        "corejs": 2,
      }
    ],
  ],
```
支持移动端：
```
chrome, opera, edge, firefox, safari, ie, ios, android, node, electron
```

### 优化相关
#### 优化`loader`配置
可以通过`test、include、exclude`三个配置项来命中`loader`要应用规则的文件。<br>
例如：
```js
moudle: {
    rules: [
        {
            test: /\.js$/,
            // babel-loader 支持缓存转换出的结果，通过 cacheDirectory选项开启
            use: ['babel-loader?cacheDirectory'],
            // 只对项目根目录下的src目录中的文件采用babe-loader
            include: path.resolve(__dirname, 'src')
        }
    ]
}
```
#### 优化 `resolve.modules`配置
`resolve.modules`用于配置`webpack`去哪些目录下寻找第三方模块。`resolve.modules`的默认值是['node_modules']，含义是先去当前目录`./node_modules`,如果没有找到，就去上一级目录`../node_modules`寻找，以此类推。
```js
resolve: {
    modules: [path.resolve(__dirname, 'node_modules')]
}
```
##### 优化 `resolve.mainFields` 配置
`resolve.mainFields`用于配置第三方模块使用哪个入口文件。
##### 优化 `resolve.alias` 配置
`resolve.alias` 配置项通过别名来将原导入路径映射成一个新的导入路径。
##### 优化 `resolve.extensions` 配置
在导入语句没带文件后缀时， `Webpack` 会在自动带上后缀后去尝试询问文件是否存在,`resolve.extensions` 用于配置在尝试过程中用到的后缀列表，默认是:
```js
    extensions: ['.js', '.json']
```
##### 使用 `HappyPack`
由于有大量文件需要解析和处理，所以构建是文件读写和计算密集型的操作， 特别是当文件数量变多后， `Webpack` 构建慢的问题会显得更为严重。运行在 `Node.js` 之上的 `Webpack`是单线程模型的，也就是说 `Webpack` 需要一个一个地处理任务，不能同时处理多个任务。`HappyPack`就能让 `Webpack` 做到这一点，它将任
务分解给多个子进程去并发执行，子进程处理完后再将结果发送给主进程。
##### 使用 `ParallelUglifyPlugin`
当 `Webpack` 有多个 `JavaScript` 文件需要输出和压缩时 ， 原本会使用 `UglifyJS` 去一个一个压缩 再输出，但是 `Paralle!UglifyPlugin`会开启多个子进程，将对多个文件的压缩工作分配给多个子进程去完成，每个子进程其实还是通过 `UglifyJS` 去压缩代码，但是变成了并行执行。所 以 `Paralle!UglifyPlugin` 能更快地完成对多个文件的压缩工作。
##### 压缩`javascript`
目前最成熟的 `JavaScript`代码压缩工具是 `UglifyJS`,它会分析`JavaScript`代码语法树，理解代码的含义，从而做到去掉无效代码、去掉日志输出代码、缩短变量名等优化 。<br/>
我们需要通过插件的形式在 `Webpack` 中接入 `UglifyJS`。目前有两个成熟的插件，如下所述。<br/>
`UglifyJsPlugin`:通过封装 `UglifyJS` 实现压缩 。<br/>
`ParallelUglifyPlugin`: 多进程并行处理压缩
##### 压缩`es6`
在用上面所讲的压缩方法去压缩 `ES6` 代码时，我们会发现 `UglifyJS` 报错退出， 原因是 `UglifyJS`只理解 `ES5`语法的代码。为了压缩 `ES6`代码，需要使用专门针对 `ES6`代码的 `UglifyES`
##### 压缩`css`
比较成熟、可靠的 `css` 压缩工具是 `cssnano`，基于 `PostCSS`。
将 `cssnano` 接入 `Webpack` 中也非常简单，因为 `css-loader` 己经将其内置了，要开启 `cssnano` 去压缩代码，则只需开启 `css-loader` 的 `minimize` 选项。
```js
    modue: {
        rules: [
            {
                test: /\.css/,
                use: ExtractTextPlugin.extract({
                    use: ['css-loader?minimize']
                })
            }
        ]
    }
```
##### 使用 `Tree Shaking`
`Tree Shaking` 可以用来剔除 `JavaScript` 中用不上的死代码。它依赖静态的 `ES6` 模块化 语 法，例如通过`import`和`export`导入、导出。<br/>
假如有一个文件 `util.js`里存放了很多 工具函数和常量，在 `main.js` 中会导入和使用 `util.js`，代码如下:
```js
// util.js
export function funcA() {}
export function funB() {}
```
```js
// main.js
import { funcA } from './util.js';
funcA()
```
`Tree Shaking` 后的`util.js`如下:
```js
export function funcA(){}
```
由于只用到了 `util.js` 中的 `funcA`，所以剩下的都被 `Tree Shaking` 当作死代码剔除了 。
##### `webpack`接入`TreeShaking`
为了将采用 `ES6` 模块化的代码提交给 `Webpack`，需要配置 `Babel` 以让其保留 `ES6` 模块化语句。修改 `.babelrc` 文件如下:
```js
{
    "presets": [
        [
            "env",
            {
                "modules": false
            }
        ]
    ]
}
```
##### 如何通过`Webpack`提取公共代码
`Webpack` 内置了专门用于提取多个 `Chunk` 中的公共部分的插件 `CommonsChunkPlugin`, `CommonsChunkPlugin` 的大致使用方法如下 :
```js
const CommonsChunkPlugin = require('webpack/lib/optimize/CommonsChunkPlugin');
new CommonsChunkPlugin({
    // 从哪些 chunk 中提取
    chunks: ['a', 'b'],
    // 能提取的公共部分形成一个新的chunk
    name: 'common'
})
```
通过以上配置就能从网页 `A` 和网页 `B` 中抽离出公共部分，放到 `common` 中 。
#### `webpack` 打包 `vue` 速度太慢怎么办？
1.使用`webpack-bundle-analyzer`对项目进行模块分析生成`report`，查看`report`后看看哪些模块体积过大，然后针对性优化，比如我项目中引用了常用的`UI`库`element-ui`和`v-charts`等<br>
2.配置`webpack`的`externals` ，官方文档的解释：防止将某些`import`的包(`package`)打包到 `bundle` 中，而是在运行时(`runtime`)再去从外部获取这些扩展依赖
#### `loader`的执行顺序为什么是后写的先执行
其实为啥是从右往左，而不从左往右，只是`Webpack`选择了`compose`方式，而不是`pipe`的方式而已，在技术上实现从左往右也不会有难度。<br>
`webpack`选择了函数式编程的方式，所以`loader`的顺序编程了从右往左。
#### 什么是`loader`
`loader`是文件加载器，能够加载资源文件，并对这些文件进行一些处理，诸如编译、压缩等，最终一起打包到指定的文件中。<br>
处理一个文件可以使用多个`loader`，`loader`的执行顺序是和本身的顺序是相反的，即最后一个`loader`最先执行，第一个`loader`最后执行。<br>
第一个执行的`loader`接收源文件内容作为参数，其他`loader`接收前一个执行的`loader`的返回值作为参数。最后执行的`loader`会返回此模块的`JavaScript`源码。
#### 有哪些常见的`Loader`？他们是解决什么问题的？
`file-loader`：把文件输出到一个文件夹中，在代码中通过相对 `URL` 去引用输出的文件<br>
`url-loader`：和 `file-loader` 类似，但是能在文件很小的情况下以 `base64` 的方式把文件内容注入到代码中去<br>
`source-map-loader`：加载额外的 `Source Map` 文件，以方便断点调试<br>
`image-loader`：加载并且压缩图片文件<br>
`babel-loader`：把 `ES6` 转换成 `ES5`<br>
`css-loader`：加载 `CSS`，支持模块化、压缩、文件导入等特性<br>
`style-loader`：把 `CSS` 代码注入到 `JavaScript` 中，通过 `DOM` 操作去加载 `CSS`。<br>
`eslint-loader`：通过 `ESLint` 检查 `JavaScript` 代码<br>
#### 常见的`plugin`
`define-plugin`：定义环境变量<br>
`commons-chunk-plugin`：提取公共代码<br>
`uglifyjs-webpack-plugin`：通过`UglifyES`压缩`ES6`代码<br>
#### `plugin`
在 `Webpack` 运行的生命周期中会广播出许多事件，`Plugin` 可以监听这些事件，在合适的时机通过 `Webpack` 提供的 `API` 改变输出结果。所以`plugin`可以扩展`webpack`的功能。
#### `plugin`和`loader`的区别是什么？
对于`loader`，它就是一个转换器，将A文件进行编译形成`B`文件，这里操作的是文件，比如将`A.scss`或`A.less`转变为`B.css`，单纯的文件转换过程。<br>
`plugin`是一个扩展器，它丰富了`wepack`本身，针对是`loader`结束后，`webpack`打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听`webpack`打包过程中的某些节点，执行广泛的任务。
#### `webpack`的构建流程是什么?从读取配置到输出文件这个过程尽量说全
1、初始化参数：从配置文件和 `Shell` 语句中读取与合并参数，得出最终的参数；<br>
2、开始编译：用上一步得到的参数初始化 `Compiler` 对象，加载所有配置的插件，执行对象的 `run` 方法开始执行编译；<br>
3、确定入口：根据配置中的 `entry` 找出所有的入口文件；<br>
4、编译模块：从入口文件出发，调用所有配置的 `Loader` 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；<br>
5、完成模块编译：在经过第4步使用 `Loader` 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；<br>
6、输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 `Chunk`，再把每个 `Chunk` 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；<br>
7、输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

#### 插件
```js
class MyPlugin {
 // 构造方法
 constructor (options) {
  console.log('MyPlugin constructor:', options)
 }
 // 应用函数
 apply (compiler) {
  // 绑定钩子事件
  compiler.plugin('compilation', compilation => {
   console.log('MyPlugin')
  ))
 }
}
module.exports = MyPlugin
```
`webpack`启动后，在读取配置的过程中会先执行 `new MyPlugin(options)` 初始化一个 `MyPlugin` 获得其实例。<br>
在初始化 `compiler` 对象后，再调用 `myPlugin.apply(compiler)` 给插件实例传入 `compiler` 对象。<br>
插件实例在获取到 `compiler` 对象后，就可以通过 `compiler.plugin`(事件名称, 回调函数) 监听到 `Webpack` 广播出来的事件。<br>
并且可以通过 `compiler` 对象去操作 `webpack`。<br>

#### `compiler`是啥，`compilation`又是啥？
`Compiler` 对象包含了 `Webpack` 环境所有的的配置信息，包含 `options，loaders，plugins` 这些信息，这个对象在 `Webpack` 启动时候被实例化，它是全局唯一的，可以简单地把它理解为 `Webpack` 实例；<br>
`Compilation` 对象包含了当前的模块资源、编译生成资源、变化的文件等。当 `Webpack` 以开发模式运行时，每当检测到一个文件变化，一次新的 `Compilation` 将被创建。`Compilation` 对象也提供了很多事件回调供插件做扩展。通过 `Compilation` 也能读取到 `Compiler` 对象。

#### `Compiler`和 `Compilation` 的区别在于
`Compiler` 代表了整个 `Webpack` 从启动到关闭的生命周期，而 `Compilation` 只是代表了一次新的编译。

#### 是否写过`Loader`和`Plugin`？描述一下编写`loader`或`plugin`的思路？
`Loader`像一个"翻译官"把读到的源文件内容转义成新的文件内容，并且每个`Loader`通过链式操作，将源文件一步步翻译成想要的样子。编写`Loader`时要遵循单一原则，每个`Loader`只做一种"转义"工作。 每个`Loader`的拿到的是源文件内容（`source`），可以通过返回值的方式将处理后的内容输出，也可以调用`this.callback()`方法，将内容返回给`webpack`。 还可以通过 `this.async()`生成一个`callback`函数，再用这个`callback`将处理后的内容输出出去。 此外`webpack`还为开发者准备了开发`loader`的工具函数集——`loader-utils`。<br>

相对于`Loader`而言，`Plugin`的编写就灵活了许多。 `webpack`在运行的生命周期中会广播出许多事件，`Plugin` 可以监听这些事件，在合适的时机通过 `Webpack` 提供的 `API` 改变输出结果。

#### 写个简单Loader
```js
// 定义
module.exports = function(src) {
 //src是原文件内容（abcde），下面对内容进行处理，这里是反转
 var result = src.split('').reverse().join('');
 //返回JavaScript源码，必须是String或者Buffer
 return `module.exports = '${result}'`;
}
//使用
{
test: /\.txt$/,
    use: [{
        './path/reverse-txt-loader'
    }]
},
```
#### tree shaking 原理

开启ScopeHoisting(作用域提升)：所有代码打包到一个作用域内，然后使用压缩工具根据变量是否被引用进行处理，删除未被引用的代码

未开启ScopeHoisting：每个模块保持自己的作用域，由webpack的treeShaking对export打标记，未被使用的导出不会被webpack链接到exports（即被引用数为0），然后使用压缩工具将被引用数为0的变量清除。（类似于垃圾回收的引用计数机制？）

