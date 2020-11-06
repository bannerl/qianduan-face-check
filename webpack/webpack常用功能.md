### 分片打包
webpack 分片机制就是将代码进行分片打包，实现按需加载，使用分片机制前，首先需要满足两个配置条件：

1.output 选项中需要有 chunkFileName：

```
chunkFilename: "static/js/[name].chunk.js",
```

2.plugins 选项中需要有 CommonsChunkPlugin：

```
new webpack.optimize.CommonsChunkPlugin({ name:'common', filename:'static/js/common.js'})
```
几种引入文件的方式
- require
- import
- require.ensure

> 使用 require 或 import 进行打包时，webpack 在执行静态编译检查时，会将 require 或 import 后的代码打包到同一文件中，而如果使用 require.ensure 引入文件，webpack 在检查时发现这种引入方式，就知道要对这部分文件进行分片打包了，打包的文件名就是前面在 webpack 配置中定义的 chunkFilename。

使用 require.ensure 较为简单，这里以 Home 组件为例：

```
require.ensure([],(require) => {
    const User = require("./User").default
},"user");
```
下面说一下几个相关参数的含义：
require.ensure 接受两个参数：

- 第一个参数是数组，表示该模块依赖的模块，该模块会在被依赖的模块都被装载后再引入，如果没有依赖性，使用空数组即可
- 第二个参数是一个回调函数，相关的异步模块就是在这个函数中被引入的
- 第三个参数是分片（chunk）文件的文件名，也就是 chunkFileName 配置中的 [name] 属性，如果不指定该参数，将默认为一个数字作为唯一的 ID，建议总是定义 chunk 文件名，方便后期调试.
- 

#### react-router 中的使用
同样，配合 require.ensure 也可以实现路由的按需加载。在使用路由按需加载时，我们只需将前面的函数进行一下包装，再将 component 换成 getComponent 即可。举个例子：
```
{
  //账号登录页
  path: 'login' /*路由訪問路徑*/,
  getComponent: (nextState, cb) => {
    require.ensure([], require => {
      cb(null, require('./bizlogic/login/loginId'))
    })
  },
  onEnter: rootOnEnter,
  onLeave: rootOnLeave
},
```



[参考链接](https://www.jianshu.com/p/a54a66282cb9)