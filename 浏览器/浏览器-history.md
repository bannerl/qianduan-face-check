
## history
### pushState
使用语法:

```history.pushState(state, title, url);```

- state：Object,与要跳转到的URL对应的状态信息。
- title：页面名字，方便调试。
- url：要跳转到的URL地址，不能跨域，对于单页应用来说没用，传空即可。

如：
```
history.pushState({
     "page": "productList"
 }, "首页", "");
 ```
### replaceState

用新的state和URL替换当前。不会造成页面刷新。语法和pushState相同，该方法一般在首页使用，更改一下首页的参数名，方便监控和判断。

##### 获取历史记录
我们想从操作页面，返回目标页面，需要在操作页面1记录当前历史位置

> 公式为：history.length - 操作页面1的length + 1

上面取负值即可
![图片](https://user-gold-cdn.xitu.io/2018/10/24/166a4e138a6b4b9e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
