## webpack

#### chunk
Chunk不同于entry、 output、module这样的概念，它们对应着Webpack配置对象中的一个字段，Chunk没有单独的配置字段，但是这个词出现在CommonsChunkPlugin（Webpack3以前）、optimization.splitChunks（Webpack4以后）这样的名称之中。

Chunk是Webpack打包过程中，一堆module的集合。我们知道Webpack的打包是从一个入口文件开始，也可以说是入口模块，入口模块引用这其他模块，模块再引用模块。Webpack通过引用关系逐个打包模块，这些module就形成了一个Chunk。

Chunk在Webpack里指一个代码块。

如果我们有多个入口文件，可能会产出多条打包路径，一条路径就会形成一个Chunk。

> chunkFilename就是为了配置决定非入口chunk的名称。例如动态组件，分割代码。

##### Bundle 和 Chunk

通常我们会弄混这两个概念，以为Chunk就是Bundle，Bundle就是我们最终输出的一个或多个打包文件。确实，大多数情况下，一个Chunk会生产一个Bundle。但有时候也不完全是一对一的关系，比如我们把 devtool配置成'source-map'。就会存在打包成两个Bundle文件，一个js文件，一个map文件，但是还是只有一个chunk文件。
##### 产生Chunk的三种途径
- entry入口
- 异步加载模块
- 代码分割（code spliting）

chunk 参考链接： https://juejin.im/post/6844903889393680392#heading-2

#### 代码分割
公共代码的分割，以及动态加载组件的一个分割
```
plugins: [
    //...此前的代码
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      minChunks: function(module) {
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          module.resource.indexOf(
            path.join(__dirname, './node_modules')
          ) === 0
        )
      }
    }),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest',
      chunks: ['vendor', 'index']
    })
]
```

#### externals

使用 ```externals``` 让webpack不打包某部分，然后在其他地方引入cdn上的js文件，利用缓存下载cdn文件达到减少打包时间的目的。 配置```externals```选项：
```
externals: {
    'vue': 'window.Vue',
    'vuex': 'window.Vuex',
    'vue-router': 'window.VueRouter'
    ...
}
```
#### Loaders
>loader 用于对模块的源代码进行转换。

loader 可以使你在 import 或"加载"模块时预处理文件。因此，loader 类似于其他构建工具中“任务(task)”，并提供了处理前端构建步骤的强大方法。loader 可以将文件从不同的语言（如 TypeScript）转换为 JavaScript，或将内联图像转换为 data URL。loader 甚至允许你直接在 JavaScript 模块中 import CSS文件！
#### Plugins
>Loaders和Plugins常常被弄混，但是他们其实是完全不同的东西：Loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个;插件并不直接操作单个文件，它直接对整个构建过程其作用。

插件（Plugins）是用来拓展webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
常用的插件：```HtmlWebpackPlugin```，```HotModuleReplacementPlugin```，```CopyWebpackPlugin```，```DllReferencePlugin```，```HappyPack```，```UglifyJsPlugin```

##### 常用插件
```new webpack.optimize.CommonsChunkPlugin ```代码分割

webpack4 代码分割 ```SplitChunksPlugin```
```
splitChunks: { // 该选项将用于配置 SplitChunksPlugin 插件
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendor',
          chunks: 'initial',
          minChunks: 2
        }
      }
    }
```

```CleanWebpackPlugin``` 主要用于清除 dist 目录下的文件，这样每次打包就不必手动清除了。

```HtmlWebpackPlugin``` 则是为了在 dist 目录下新建 html 模板并自动插入依赖的 js。

```BundleAnalyzerPlugin``` 主要是为了生成打包后的 js 文件包含的依赖，如此时进行打包，则生成：
#### Webpack Dev Server
>webpack-dev-server是一个小型的node.js Express服务器,它使用webpack-dev-middleware中间件来为通过webpack打包生成的资源文件提供Web服务。它还有一个通过Socket.IO连接着webpack-dev-server服务器的小型运行时程序。webpack-dev-server发送关于编译状态的消息到客户端，客户端根据消息作出响应。

简单来说，webpack-dev-server就是一个小型的静态文件服务器。使用它，可以为webpack打包生成的资源文件提供Web服务。
#### Webpack Merge
合并俩个webpack的js配置文件，不包括plugin
#### Webpack Chain
链式包装器
引入 webpack-chain 后，我们所有的 webpack 配置通过一个链式包装器便可生成了：

```
const Config = require('webpack-chain');
const config = new Config();
// 链式生成配置
...
// 导出 webpack 配置对象
export default config.toConfig();
```



