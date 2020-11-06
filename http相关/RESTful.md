## RESTful
RESTful架构是指符合REST的约束条件和原则的Web 数据接口的设计。

### HTTP动词

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
- PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
- DELETE（DELETE）：从服务器删除资源。

##### 其他http动词
- HEAD：获取资源的元数据（和get请求表现一致，就是没有返回数据）。
- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。（获取当前URL所支持的方法）
- 

### options 预检

>w3c规范要求，对复杂请求，浏览器必须先使用options发起一个预检请求，从而获知服务器是否允许该跨域请求，服务器确认以后才能发起实际的HTTP请求，否则停止第二次正式请求。

##### 什么情况下需要预检？
1. 请求方法不是get head post
2. post 的content-type不是
    **application/x-www-form-urlencode,**
  **multipart/form-data,** **text/plain**
3. 请求设置了自定义的header字段: 比如业务需求，传一个字段，方面后端获取，不需要每个接口都传
4. http头信息不超出一下字段：（Accept、Accept-Language 、 Content-Language、 Last-Event-ID）

##### withCredentials 属性 
CORS请求默认不发送Cookie和HTTP认证信息。如果要把Cookie发到服务器，一方面要服务器同意，指定Access-Control-Allow-Credentials字段。