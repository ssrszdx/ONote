是的，你说得对。在JavaScript中，`this`关键字的值确实取决于函数如何被调用。这是一些常见的情况：

1. 在全局作用域或函数外部调用函数，`this`通常指向全局对象（在浏览器中是`window`）。

```javascript
function test() {
  console.log(this);
}

test();  // 输出：Window {...} (或者全局对象)
```

2. 当函数作为对象的方法被调用时，`this`指向调用该方法的对象。

```javascript
const obj = {
  prop: 'Hello',
  method: function() {
    console.log(this.prop);
  }
};

obj.method();  // 输出："Hello"
```

3. 当函数作为构造函数（使用`new`关键字）被调用时，`this`指向新创建的对象。

```javascript
function Person(name) {
  this.name = name;
}

const john = new Person('John');
console.log(john.name);  // 输出："John"
```

4. 当使用`call`、`apply`或`bind`方法调用函数时，`this`被显式设置为你传递的对象。

```javascript
function greet() {
  console.log(`Hello, ${this.name}`);
}

const alice = { name: 'Alice' };
greet.call(alice);  // 输出："Hello, Alice"
```

在React组件的方法中，如果你想让`this`指向组件的实例，你需要在构造函数中绑定`this`，或者使用箭头函数（箭头函数会捕获定义时的`this`值）。例如，你的`handleLoginChange`方法可能需要这样绑定：

```javascript
constructor(props) {
  super(props);
  this.handleLoginChange = this.handleLoginChange.bind(this);
}

handleLoginChange(event) {
  // 在这里，`this`指向组件的实例
}
```

// @todo