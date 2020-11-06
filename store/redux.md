随着 JavaScript 单页应用开发日趋复杂，JavaScript 需要管理比任何时候都要多的 state （状态）。

state 在什么时候，由于什么原因，如何变化已然不受控制。

这里的复杂性很大程度上来自于：我们总是将两个难以理清的概念混淆在一起：变化和异步。

Redux 试图让 state 的变化变得可预测。

Redux要解决的问题是统一数据流，数据流完全可控并可追踪。

### Redux 三大原则

- 单一数据源
- State 是只读的
- 使用纯函数来执行修改

Redux 规定，将模型的更新逻辑全部集中于一个特定的层（Flux 里的 store，Redux 里的 reducer）。Flux 和 Redux 都不允许程序直接修改数据，而是用一个叫作 “action” 的普通对象来对更改进行描述。

### react-redux语法
```
const FilterLink = connect(
  mapStateToProps,
  mapDispatchToProps
)(Link)
```

### redux如何实现单一数据源
redux会在createStore创建一个名为currentState的属性，作为闭包保存起来，外部无法对此进行修改，只能用action进行修改。

### 和mobx对比

1. Redux的编程范式是函数式的而Mobx是面向对象的。
3. Redux 数据流流动很自然，可以充分利用时间回溯的特征，增强业务的可预测性；MobX 没有那么自然的数据流动，也没有时间回溯的能力，但是 View 更新很精确，粒度控制很细，这一点得益于Mobx的observable；对应的，Redux是用dispath进行广播，通过Provider和connect来比对前后差别控制更新粒度，有时需要自己写SCU（父组件更新导致子树更新时, 子组件树中的组件会经历一个优化阶段 称之为 SCU）；Mobx更加精细一点。
4. Redux 通过引入一些中间件来处理副作用；MobX  没有中间件，副作用的处理比较自由，比如依靠 autorunAsync 之类的方法。
6. 往往是多个 Store。


### redux-thunk
thunk 中间件使我们能够编写异步 action creator，它返回的是函数，而不是对象。我们的新 action creator 现在可以如下所示：
```
import * as TodoAPIUtil from '../util/todo_api_util';

export const RECEIVE_TODOS = "RECEIVE_TODOS";

export const requestPosts = subreddit => ({
  type: REQUEST_POSTS,
  subreddit
})

export const receiveTodos = todos => ({
  type: RECEIVE_TODOS,
  todos
});

export const fetchTodos = () => dispatch => (
  TodoAPIUtil
      .fetchTodos()
      .then(todos => dispatch(receiveTodos(todos)))
);
```

整体流程为：```Action Creator``` => ```action``` => ```store.dispatch(action)``` => ```reducer(state, action)``` => ```state``` = ```nextState```。流程图如下：
![图片](https://user-gold-cdn.xitu.io/2019/12/4/16ed0337d289e409?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

异步请求部分放在了  action  creator  中，理解起来比较简单。

我们有个单独的文件写函数返回action，但是在异步请求的时候，外部dispatch不能拿到action，所以需要在action文件中，让返回action的方法，返回一个函数，而不是一个action对象，如果是函数react-thunk则会将dispatch传进去，就可以做到异步dispatch。

### redux-saga

使用generate来完成异步的行为
```
// saga.js
import { take, put } from 'redux-saga/effects'

function* mySaga(){ 
    // 阻塞: take方法就是等待 USER_INTERACTED_WITH_UI_ACTION 这个 action 执行
    yield take(USER_INTERACTED_WITH_UI_ACTION);
    // 阻塞: put方法将同步发起一个 action
    yield put(SHOW_LOADING_ACTION, {isLoading: true});
    // 阻塞: 将等待 FetchFn 结束，等待返回的 Promise
    const data = yield call(FetchFn, 'https://my.server.com/getdata');
    // 阻塞: 将同步发起 action (使用刚才返回的 Promise.then)
    yield put(SHOW_DATA_ACTION, {data: data});
}
```

### redux和vuex区别
vuex三大原则

a 应用层级的状态应该集中到单个 store 对象中。

b. 提交 mutation
是更改状态的唯一方法，并且这个过程是同步的。

c. 异步逻辑都应该封装到 action 里面。

处理异步操作，用action，redux用中间件thunk和saga

参考链接：https://blog.csdn.net/weixin_44369568/article/details/90713881