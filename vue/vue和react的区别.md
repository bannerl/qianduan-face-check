### 监听数据变化的实现原理不同
- Vue 通过 getter/setter 以及一些函数的劫持，能精确知道数据变化，不需要特别的优化就能达到很好的性能
- React 默认是通过比较引用的方式进行的，如果不优化（PureComponent/shouldComponentUpdate）可能导致大量不必要的VDOM的重新渲染

### 数据流的不同
React 从诞生之初就不支持双向绑定，React一直提倡的是单向数据流，他称之为 onChange/setState()模式
### Hoc和 mixins
在 Vue 中我们组合不同功能的方式是通过 mixin，而在React中我们通过 HoC (高阶组件）
### 组件通信的区别
#### vue中通信
- 父组件通过 props 向子组件传递数据或者回调，虽然可以传递回调，但是我们一般只传数据，而通过 事件的机制来处理子组件向父组件的通信
- 子组件通过 事件 向父组件发送消息
- 通过 V2.2.0 中新增的 provide/inject 来实现父组件向子组件注入数据，可以跨越多个层级

#### react中通信
父组件通过 props 可以向子组件传递数据或者回调

可以通过 context 进行跨层级的通信，这其实和 provide/inject 起到的作用差不多。
### Vuex和Redux区别

https://juejin.im/post/6844903668446134286