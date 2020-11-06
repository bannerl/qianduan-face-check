### webpack Plugin的工作原理
1. 读取配置的过程中会先执行 new HelloPlugin(options) 初始化一个 HelloPlugin 获得其实例。
1. 初始化 compiler 对象后调用 HelloPlugin.apply(compiler) 给插件实例传入 compiler 对象。
1. 插件实例在获取到 compiler 对象后，就可以通过compiler.plugin(事件名称, 回调函数) 监听到 Webpack 广播出来的事件。
并且可以通过 compiler 对象去操作 Webpack。


参考链接：
https://juejin.im/post/6844904161515929614
https://juejin.im/post/6844904070868631560