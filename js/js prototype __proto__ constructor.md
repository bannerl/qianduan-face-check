
![图片](https://img-blog.csdnimg.cn/20190311194017886.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NjMTg4Njg4NzY4Mzc=,size_16,color_FFFFFF,t_70#pic_center)

1. 我们需要牢记两点：

    ①```__proto__```和```constructor```属性是对象所独有的；

    ② ```prototype```属性是函数所独有的，因为函数也是一种对象，所以函数也拥有```__proto__```和```constructor```属性。

2. ```__proto__```属性的作用就是当访问一个对象的属性时，如果该对象内部不存在这个属性，那么就会去它的```__proto__```属性所指向的那个对象（父对象）里找，一直找，直到```__proto__```属性的终点null，再往上找就相当于在null上取值，会报错。通过```__proto__```属性将对象连接起来的这条链路即我们所谓的原型链。

 3. prototype属性的作用就是让该函数所实例化的对象们都可以找到公用的属性和方法，即```f1.__proto__ === Foo.prototype```。

 4. ```constructor```属性的含义就是指向该对象的构造函数，所有函数（此时看成对象了）最终的构造函数都指向```Function```。

参考链接：
https://blog.csdn.net/cc18868876837/article/details/81211729

https://juejin.im/post/5cc99fdfe51d453b440236c3