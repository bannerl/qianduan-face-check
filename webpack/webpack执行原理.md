
### 流程图
奉上一张滴滴云博客的WebPack 编译流程图,不喜欢看文字讲解的可以看流程图理解记忆
![流程图](https://user-gold-cdn.xitu.io/2020/5/18/172256efb94eec6d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![流程图](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/71b263000fa94db792cf1e98d67a578a~tplv-k3u1fbpfcp-zoom-1.image)
### Webpack 的 Tapable
Webpack 的 Tapable 事件流机制保证了插件的有序性，将各个插件串联起来， Webpack 在运行过程中会广播事件，插件只需要监听它所关心的事件，就能加入到这条webapck机制中，去改变webapck的运作，使得整个系统扩展性良好。

webpack中最核心的负责编译的Compiler和负责创建bundles的Compilation都是Tapable的实例，可以直接在 Compiler 和 Compilation 对象上广播和监听事件，方法如下：

```
/**
* 广播事件
* event-name 为事件名称，注意不要和现有的事件重名
*/
compiler.apply('event-name',params);
compilation.apply('event-name',params);
/**
* 监听事件
*/
compiler.plugin('event-name',function(params){});
compilation.plugin('event-name', function(params){});
```
### Compiler
Compiler 对象包含了当前运行Webpack的配置，包括entry、output、loaders等配置，这个对象在启动Webpack时被实例化，而且是全局唯一的。Plugin可以通过该对象获取到Webpack的配置信息进行处理。

### 理解Compilation（负责创建bundles）

Compilation对象代表了一次资源版本构建。当运行 webpack 开发环境中间件时，每当检测到一个文件变化，就会创建一个新的 compilation，从而生成一组新的编译资源。一个 Compilation 对象表现了当前的模块资源、编译生成资源、变化的文件、以及被跟踪依赖的状态信息，简单来讲就是把本次打包编译的内容存到内存里。Compilation 对象也提供了插件需要自定义功能的回调，以供插件做自定义处理时选择使用拓展。

简单来说,Compilation的职责就是构建模块和Chunk，并利用插件优化构建过程。


### js编译原理
1. 获取主模块内容
2. 分析模块
    - 安装@babel/parser包（转AST）
1. 对模块内容进行处理
    - 安装@babel/traverse包（遍历AST收集依赖）
    - 安装@babel/core和@babel/preset-env包   （es6转ES5）
1. 递归所有模块
2. 生成最终代码
