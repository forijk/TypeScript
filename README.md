# TypeScript 总结

- 实例属性
- ES6 中实例的属性只能通过构造函数中的 this.xxx 来定义，ES7 提案中可以直接在类里面定义：

```javascript
class Animal {
    name = 'Jack';
​
    constructor() {
        // ...
    }
}
​
let a = new Animal();
console.log(a.name); // Jack
```

- 静态属性

- ES7 提案中，可以使用 static 定义一个静态属性：

```javascript
class Animal {
    static num = 42;
​
    constructor() {
        // ...
    }
}
​
console.log(Animal.num); // 42
```

- TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 public、private 和 protected。

  - public 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
  - private 修饰的属性或方法是私有的，不能在声明它的类的外部访问
  - protected 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的

- 修饰器，这是 ES7 的一个提案 ​ http://es6.ruanyifeng.com/#docs/decorator
- ​Mixins：一种编程模式，与 TypeScript 没有直接关系，可以参考 ES6 中 Mixin 模式的实现 ​http://es6.ruanyifeng.com/#docs/class#Mixin模式的实现
