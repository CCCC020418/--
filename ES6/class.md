## class



类的数据类型是函数,类本身就指向构造函数,

prototype对象的constructor属性,直接指向类的本身,类的内部所有定义的方法都是不可以枚举的

constructor 方法是类的默认方法,通过new命令生成对象实例时,自动调用该方法.

一个类必须有constructor方法,如果没有显式定义,一个空的constructor方法会被默认添加

```javascript
class Point {
}

// 等同于
class Point {
  constructor() {}
}
```

定义了一个空的类point,JavaScript引擎会自动为他添加一个空的constructor方法

constructor方法默认返回实例对象(即this),完全可以指定返回另外一个对象

```javascript
class Foo {
  constructor() {
    return Object.create(null);
  }
}

new Foo() instanceof Foo
// false
```

constructor   