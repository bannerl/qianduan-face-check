### dll

### happaypack
### UglifyJsPlugin 
用来缩小（压缩优化）js文件，至少需要Node v6.9.0和Webpack v4.0.0版本。
```
 new UglifyJsPlugin({
    test: /\.js(\?.*)?$/i,  //测试匹配文件,
    include: /\/includes/, //包含哪些文件
    excluce: /\/excludes/, //不包含哪些文件
    
    //允许过滤哪些块应该被uglified（默认情况下，所有块都是uglified）。 
    //返回true以uglify块，否则返回false。
    chunkFilter: (chunk) => {
        // `vendor` 模块不压缩
        if (chunk.name === 'vendor') {
          return false;
        }
        return true;
      }
    }),
    
    cache: false,   //是否启用文件缓存，默认缓存在node_modules/.cache/uglifyjs-webpack-plugin.目录
    parallel: true,  //使用多进程并行运行来提高构建速度
```
### 提取项目中的公共代码
```
splitChunks: {
    cacheGroups: {
      venders: {
        test: /node_modules/,
        name: 'vendors',
        chunks: 'all'
      }
    }
  }
```


### 提取业务组件中的公共代码
```
default: {
    minSize: 0,
    minChunks: 2,
    reuseExistingChunk: true,
    name: 'utils'
  }
```
### 第三方插件使用少可以单独打包，不打包到verders中
```
// venders: {
//     test: node_modules/,
//     name: 'vendors',
//     chunks: 'all'
// },
lodash: {
  test: /node_modules\/lodash\//, // lodash 库单独打包，并命名为 vender-lodash
  name: 'vender-lodash'
}
```
### 缓存
修改某个文件，只有这个文件和 manifest.js 文件的 hash 会发生变化，其他文件的 hash 不变。
```
optimization: {
  runtimeChunk: {
    name: 'manifest'
  },
  splitChunks: {...}
}
```
#### HashedModuleIdsPlugin
增加、删除一些模块，可能会导致不相关文件的 hash 发生变化，这是因为 webpack 打包时，按照导入模块的顺序，module.id 自增，会导致某些模块的 module.id 发生变化，进而导致文件的 hash 变化。

解决方式： 使用 webpack 内置的 HashedModuleIdsPlugin，该插件基于导入模块的相对路径生成相应的 module.id，这样如果内容没有变化加上 module.id 也没变化，则生成的 hash 也就不会变化了。
