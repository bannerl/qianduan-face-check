**1、值变量**

以 @ 开头 定义变量，并且使用时 直接 键入 @名称。

在平时工作中，我们就可以把 常用的变量 封装到一个文件中，这样利于代码组织维护。
```
@lightPrimaryColor: #c5cae9;
@textPrimaryColor: #fff;
@accentColor: rgb(99, 137, 185);
@primaryTextColor: #646464;
@secondaryTextColor: #000;
@dividerColor: #b6b6b6;
@borderColor: #dadada;
```
**2、extend和方法区别**
```
/* Less */
#main{
  width: 200px;
}
#main {
  &:after {
    content:"Less is good!";
  }
}
#wrap:extend(#main) {}
// 或者
#wrap {
    &:extend(#main)
}
```
```
/* Less */
#main{
  width: 200px;
}
#main {
  &:after {
    content:"Less is good!";
  }
}
#wrap:extend(#main all) {}
```
从表面 看来，extend 与 方法 最大的差别，就是 extend 是同个选择器共用同一个声明，而 方法 是使用自己的声明，这无疑 增加了代码的重复性。

**3、导入**
1. 导入 less 文件 可省略后缀
2. @import 的位置可随意放置

**4、方法**
```
.card { // 等价于 .card()
    background: #f6f6f6;
    -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
    box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
}
#wrap{
  .card;//等价于.card();
}
/* 生成的 CSS */
#wrap{
  background: #f6f6f6;
  -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
  box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
}
```
- . 与 # 皆可作为 方法前缀。
- 方法后写不写 () 看个人习惯。

默认参数
```
.border(@a:10px,@b:50px,@c:30px,@color:#000){
    border:solid 1px @color;
    box-shadow: @arguments;//指代的是 全部参数
}
```
方法的匹配模式
```
.triangle(top,@width:20px,@color:#000){
    border-color:transparent  transparent @color transparent ;
}
.triangle(right,@width:20px,@color:#000){
    border-color:transparent @color transparent  transparent ;
}
```

方法筛选用when
如果你希望你的方法接受数量不定的参数，你可以使用... ，犹如 ES6 的扩展运算符。
```
/* Less */
.boxShadow(...){
    box-shadow: @arguments;
}
.textShadow(@a,...){
    text-shadow: @arguments;
}
```



[参考链接](https://juejin.im/post/5a2bc28f6fb9a044fe464b19)

