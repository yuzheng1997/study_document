#### JavaScript

​	this是在函数被调用是发生的绑定，它的指向性完全取决于函数在什么地方调用。

​	`this` 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。

#### 作用域

##### 词法作用域

##### 动态作用域

javaScript采用词法作用域，也就是在编程阶段，作用域就已经明确下来。而JavaScript的this机制跟动态作用域相似，是在运行是动态绑定的。

#### this的四种绑定规则

##### 默认绑定

```js
function foo() {
	console.log(this.a)
}
foo()
// 不适用任何的修饰符
// this 指 window
```

##### 隐式绑定

```js
function foo() {
  console.log(this.a);
}

let obj1 = {
  a: 1,
  foo,
};

let obj2 = {
  a: 2,
  foo,
}

obj1.foo();   // 输出 1
obj2.foo();   // 输出 2
// 拥有上下文对象的时候，this绑定到调用的对象上
```

##### new绑定

在 JavaScript 中，`new` 操作符并不像其他面向对象的语言一样，而是一种模拟出来的机制。
在 JavaScript 中，所有的函数都可以被 `new` 调用，这时候这个函数一般会被称为「构造函数」，实际上并不存在所谓「构造函数」，更确切的理解应该是对于函数的「构造调用」。

```js
// 模拟new
function New(Constructor, ...args){
    let obj = {};   // 创建一个新对象
    Object.setPrototypeOf(obj, Constructor.prototype);  // 连接新对象与函数的原型
    return Constructor.apply(obj, args) || obj;   // 执行函数，改变 this 指向新的对象
}

function Foo(a){
    this.a = a;
}

New(Foo, 1);  // Foo { a: 1 }
```



##### 显示绑定

显示绑定就是使用bind(this,args0,...argsn),apply(this, ...args),call(this,args0,...argsn)

apply和call的区别在于参数形式不同

bind和apply的区别是，apply是立即执行函数，bind会返回一个新的函数。

```js
function foo() {
  console.log(this.a);  // 输出 1
  bar.apply({a: 2}, arguments);
}

function bar(b) {
  console.log(this.a + b);  // 输出 5
}

var a = 1;
foo(3);

```

##### 优先级

new >显示绑定>隐式绑定>默认绑定

#### 内部函数和闭包

```js
function a () {
	let a = 1;
	function b () {
		let b = 2;
		return a + b;
	}
    return b();
}
// a () 3
function out (a) {
    return function inM (b) {
        return a + b;
    }
}
// out(1)(2) 3
```

​	一个函数被定义在另一个函数的内部，内部函数可以访问外部函数的变量，唯一不同的是，out函数已经返回了，按理局部变量已经不存在。out已经不能工作了，但却变量仍然存在。

​	每次JavaScript执行一个函数时，都会创建一个作用域对象，用来保存在这个函数创建的局部变量，使用一切被传入的变量初始化。