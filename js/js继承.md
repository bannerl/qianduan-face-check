实例终究是一个对象，最终会继承对象上的属性和方法，而我们如果继承一个构造函数的话，就必须要求这个构造函数的原型是继承对象的，这样实例才会有对象上的属性和方法。

所以，我们实例化的时候，才是创建一个空的对象，让空对象的原型指向构造函数的prototype（也就是 ```obj.__proto__ = o.prototype```），因为prototype是一个对象，原型指向对象，然后在绑定私有属性和方法即可



### new 原理
new 的作用主要是创建一个新对象，新对象的原型指向父类的prototype，然后调用父类构造函数方法
```
// new的原理
function dd (o) {
  var obj = {}
  obj.__proto__ = o.prototype
  o.call(obj, [...arguments].slice(1))
  return obj
}
```
### Object.create 原理
这个方法主要是继承对象上的方法和属性，不继承构造函数

```
function createObject (o) {
  var f = function () {}
  f.prototype = o
  return new f()
}
```
也可以（不过下面这种方式实现的继承性能较低，并且再继承这个返回的值也会造成性能损耗，另外```__proto__```已经是处于废弃的属性值，所以还是最好不要用下面这种方式）
```
function createObject (o) {
  var obj = {}
  obj.__proto__ = o
  return obj
}
```
### 类式继承

继承父类的公共方法和属性，同时将父类的私有属性和方法以原型链的形式继承，造成不同实例共享父类私有属性，并且没有办法向父类传动态参数

```
function Child () {
  this.name = 222
}
Child.prototype = new Parent()
Child.prototype.constructor = Child
let instance = new Child()
```

### 原型继承
继承一个对象的属性，同 Object.create

```
function createObject (o) {
  function f () {}
  f.prototype = o
  return new f()
}
```
使用如下
```
Child.prototype = createObject(Parent)
Child.prototype.constructor = Child
```
### 构造函数继承
构造函数继承是只继承构造函数方法上的属性和方法，
并且可以动态获取参数，可以用call或者apply来实现

但是无法继承原型上的方法和属性。
```
function Child (name) {
  Parent.call(this, ...arguments)
  this.name = name
}
```
### 组合继承
同样Parent被实例化两次，并且如果父类内部调用prototype上的方法或属性，子类中Parent在执行的时候是拿不到的，会报错。

### 寄生组合继承
寄生的概念是借用一个对象，继承父类原型上的属性和方法。

- 子类继承了父类的属性和方法，同时，属性没有被创建在原型链上，因此多个子类不会共享同一个属性。
- 子类可以传递动态参数给父类！
- 父类的构造函数只执行了一次！
```
function inherit (child, parent) {
    let o = Object.create(parent.prototype)
    child.prototype = Object.assign(o, child.prototype)
    child.constructor = child
}
```
子实例的__proto__为构造函数Child.prototype，
Child构造函数的prototype实际上是一个对象，继承自Object构造函数，而寄生式组合继承是在继承Object构造函数之上，让构造函数Child的prototype继承一个对象，这个对象继承了Parent的公共属性和方法

[参考链接](https://juejin.im/post/5a96d78ef265da4e9311b4d8)