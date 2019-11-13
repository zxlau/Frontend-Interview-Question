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












