## super
>子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

##### super 当做函数调用
```
class A {}
class B extends A {
  constructor() {
    super();   //实例化父类
  }
}
```
>注意，super虽然代表了父类A的构造函数，但是返回的是子类B的实例，即super内部的this指的是B，因此super()在这里相当于```A.prototype.constructor.call(this)```。
##### super 当做对象使用
在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
```
class Parent{       
    static methodA () {       
        console.log('static methodA in Parent')
    }
    parentMethod () {
        console.log('parentMethod')
    }
}
    
class Child extends Parent{
    constructor () {
        super()
        console.log(super.parentMethod())
    }
    static methodA () {   
        super.methodA()    
        console.log('static methodA in Child')
    }  
}

Child.methodA();
//输出：
//static methodA in Parent
// static methodA in Child
```
#### Object.getPrototypeOf()

>Object.getPrototypeOf方法可以用来从子类上获取父类。可以使用这个方法判断，一个类是否继承了另一个类。