## hooks 中函数
函数在每次渲染时是独立的。这就是 Capture Value 特性，后面遇到这种情况就不会一一展开，只描述为 “此处拥有 Capture Value 特性”。



### useEffect
会在didMount的时候调用，获取接口数据。

返回一个函数，会在销毁的时候调用，可以删除定时器和事件绑定。

会在didUpdate的时候使用，例如传入props的更改，需要出发重新获取数据，如果参数更改，可以多个地方字段都要修改，而使用useCallback就可以将参数的变更内聚到方法内。

### 回收机制
在组件被销毁时，通过 useEffect 注册的监听需要被销毁，这一点可以通过 useEffect 的返回值做到

### useCallback

useCallback 封装的取数函数，可以直接作为依赖传入 useEffect，useEffect 只要关心取数函数是否变化，而取数参数的变化在 useCallback 时关心，再配合 eslint 插件的扫描，能做到 依赖不丢、逻辑内聚，从而容易维护。



参考链接：https://juejin.im/post/6844903806090608647