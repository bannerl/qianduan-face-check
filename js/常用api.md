
encodeURI方法不会对下列字符编码 
> ASCII字母、数字、~!@#$&*()=:/,;?+'

encodeURIComponent方法不会对下列字符编码
> ASCII字母、数字、~!*()'

所以encodeURIComponent比encodeURI编码的范围更大。

实际例子来说，encodeURIComponent会把 http://  编码成  http%3A%2F%2F 而encodeURI却不会。


### defer
文档解析时，遇到设置了defer的脚本，就会在后台进行下载，但是并不会阻止文档的渲染，当页面解析&渲染完毕后。
会等到所有的defer脚本加载完毕并按照顺序执行，执行完毕后会触发DOMContentLoaded事件。
![图片](https://user-images.githubusercontent.com/9568094/31621324-046d4a44-b25f-11e7-9d15-fe4d6a5726ae.png)
### async
async脚本会在加载完毕后执行。
async脚本的加载不计入DOMContentLoaded事件统计，也就是说下图两种情况都是有可能发生的
![图片](https://user-images.githubusercontent.com/9568094/31622216-6c37db9c-b261-11e7-8bd3-79e5d4ddd4d0.png)

![图片](https://user-images.githubusercontent.com/9568094/31621170-b4cc0ef8-b25e-11e7-9980-99feeb9f5042.png)

![图片](https://segmentfault.com/img/bVWhRl?w=801&h=814)

![图片](http://segmentfault.com/img/bVcQV0)

#### defer
如果你的脚本代码依赖于页面中的DOM元素（文档是否解析完毕），或者被其他脚本文件依赖。
例：

- 评论框

- 代码语法高亮

- polyfill.js

#### async
如果你的脚本并不关心页面中的DOM元素（文档是否解析完毕），并且也不会产生其他脚本需要的数据。
例：

百度统计

如果不太能确定的话，用defer总是会比async稳定。


### load & DOMContentLoaded
当 DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片。
(譬如如果有async加载的脚本就不一定完成)

当 onload 事件触发时，页面上所有的DOM，样式表，脚本，图片都已经加载完成了。
（渲染完毕了）


**prefetch**用来告诉浏览器在空闲时提前下载用户在接下来的浏览中可能用到的文件

**preload**则因为浏览器执行css时会阻塞DOM解析，所以通常用preload来提前下载当前页面马上要用到的文件

目前策略改为：prefetch入口相关的常用的asyncChunks，preload chunk-vendors和入口chunk

