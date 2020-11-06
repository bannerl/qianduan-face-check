
传统React使用的数据管理库为Redux。Redux要解决的问题是统一数据流，数据流完全可控并可追踪。要实现该目标，便需要进行相关的约束。Redux由此引出了dispatch action reducer等概念，对state的概念进行强约束。然而**对于一些项目来说，太过强，便失去了灵活性**。Mobx便是来填补此空缺的。

Mobx的核心原理是通过action触发state的变化，进而触发state的衍生对象（computed value & Reactions）。

#### state
在Mobx中，State就对应业务的最原始状态，通过```observable```方法，可以使这些状态变得可观察。

当某一数据被```observable```包装后，他返回的其实是被observable包装后的类型。通过Mobx原始提供的```observable.toJS()```转换成JS再判断，或者直接使用Mobx原生提供的```APIisObservableArray```进行判断。

### computed
对于依赖state的数据而衍生出的数据，可以使用computed。

如果计算属性依赖的state没改变，或者该计算值没有被其他计算值或响应（reaction）使用，computed便不会运行。在这种情况下，computed处于暂停状态，此时若该计算属性不再被observable。那么其便会被Mobx垃圾回收。

### autorun
每当依赖的值改变时，其都会改变。

### computed 和 autorun 区别
- autorun没有了computed的优化
- 依赖值未改变的情况下也不会重新运行，但不会被自动回收
- autorun通常用来执行一些有副作用的。例如打印日志，更新UI等等。

### action
Mobx并不强制所有state的改变必须通过action来改变，这主要适用于一些较小的项目。
```
const {observable} = require('mobx')
const appState = observable({ counter: 0 })
appState.counter += 1
```
对于较大型的，需要多人合作的项目来说，可以使用Mobx提供的api configure来强制。
```
Mobx.configure({enforceActions: true})
```

如果不想到处写action，可以使用Mobx提供的工具函数runInAction来简化操作。

### Mobx的一些坑
1. 无法收集新增的属性依赖
1. 回调函数若依赖外部环境，则无法进行收集


参考链接：https://blog.csdn.net/weixin_44369568/article/details/90713881



