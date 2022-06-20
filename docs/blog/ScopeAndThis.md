## 作用域
### 作用域概述

作用域是代码在运行时某些特定部分中的变量与函数的可访问性。

作用域决定了变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期

![作用域](/MyBlog/blog/scope.jpg)

ES5中只有全局作用域和函数作用域，我们都知道他没有块级作用域。

ES6中多了一个let，他可以保证外层块不受内层块的影响。即内层块形成了一个块级作用域，这是let的一个特点。

函数的内部环境可以通过作用域链访问到所有的外部环境，但是外部环境却不可以访问外部环境，这就是作用域的关键。但是我们要知道，作用域是在一个函数创建时就已经形成的，而不是调用时。

### 作用域链

一般情况，变量取值是到创建这个变量的函数的作用域中取值。那在函数作用域中取不到怎么办？

如果在当前作用域中没有查到值，就会向上级作用域去查，直到查到全局作用域，这么一个查找过程形成的链条就叫做作用域链。

### var、let、const的区别

var定义的变量，没有块的概念，可以跨块访问, 不能跨函数访问。

let定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问。

const用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改

那什么是块？

```js
function fn() {

};

{

}
```

这都是块

三者还有其他区别吗？

### 声明提升

大部分编程语言都是先声明变量再使用，否则会报错。但在js中，则会变的不一样。

```js
console.log(a);
var a = 1;
```

结果是undefined，不会报错

变量声明，不管在哪里发生（声明），都会在任意代码执行前处理.

以var声明的变量的作用域就是当前执行上下文，即某个函数，或者全局作用域（声明在函数外）。

### 函数定义

定义一个函数有两种方式：函数声明和函数表达式

区别？

![函数定义](/MyBlog/blog/fn.jpg)

函数声明会有声明提升
函数表达式不会有声明提示

既然变量和函数声明都有提升，先提升谁呢？我们来看一个例子

![例子](/MyBlog/blog/example.jpg)

静静的思考两分钟。。。

然后看结果
![答案](/MyBlog/blog/answer.jpg)

然而，why？

首先说当定义的变量名和函数的参数名重复的时候，变量定义会失效，不会覆盖函数的参数。

再者，函数的定义优先级高于变量的定义，所以后面同名的变量定义会失效。
但是赋值是有效的

## this

我们先来看一道题

![this](/MyBlog/blog/this.jpg)

有答案了吗？结果我们先不说答案，继续看下去

this关键字是JavaScript中最复杂的机制之一，This其实 就是一个指针，指向调用函数的对象

为了能够准确的知道this指向的是什么，我们需要了解this的绑定规则有哪些

* 默认绑定
* 隐式绑定
* 硬(显示)绑定
* new绑定

### 默认绑定

默认绑定，在不能应用其它绑定规则时使用的默认规则，通常是独立函数调用。

![默认](/MyBlog/blog/bangding1.jpg)

如果在浏览器环境中运行，那么结果就是 Hello,check
但是如果在node环境中运行，结果就是 Hello,undefined

### 隐式绑定

函数的调用是在某个对象上触发的，即调用位置上存在上下文对象。典型的形式为 XXX.fun()

![默认](/MyBlog/blog/bangding2.jpg)

现在我们来思考一个问题，多个对象多层调用会是什么样的？

```js
function sayHi() {
  console.log('Hello', this.name)
}
var person2 = {
  name: 'check',
  sayHi: sayHi
}
var person1 = {
  name: 'future',
  friend: person2
}
person1.friend.sayHi()
```

结果是： Hello, check

为什么呢？
只有最后一层调用才会确定this指向的是什么，所以我们只需要关注最后一层，即此处的friend。

`隐式绑定有一个大坑，让我们以为的正确绑定是错的.`

再来看一段代码

```js
function sayHi() {
  console.log('Hello', this.name)
}
var person = {
  name: 'check',
  sayHi: sayHi
}
var name = 'future'
var Hi = person.sayHi;
Hi()
```
结果是啥呢？

Hello, future

Why?

Hi直接指向了sayHi的引用，在调用的时候，跟person没有半毛钱的关系，针对此类问题，我建议大家只需牢牢记住这个格式:XXX.fn();

fn()前如果什么都没有，那么肯定不是隐式绑定

再来看一段代码

```js
function sayHi(){
  console.log('Hello,', this.name);
}
var person1 = {
  name: 'quyong',
  sayHi: function(){
      setTimeout(function(){
          console.log('Hello,',this.name);
      })
  }
}
var person2 = {
  name: 'future',
  sayHi: sayHi
}
var name='check';
person1.sayHi();
setTimeout(person2.sayHi,100);
setTimeout(function(){
  person2.sayHi();
},200);
```

这个的结果是啥呢？


Hello, check

Hello, check

Hello, future

Why?

第一条输出很容易理解，setTimeout的回调函数中，this使用的是默认绑定，非严格模式下，执行的是全局对象

第二条输出，说好的XXX.fun()的时候，fun中的this指向的是XXX呢，为什么这次却不是这样了！Why?

其实这里我们可以这样理解: setTimeout(fn,delay){ fn(); },相当于是将person2.sayHi赋值给了一个变量，最后执行了变量，这个时候，sayHi中的this显然和person2就没有关系了。

第三条虽然也是在setTimeout的回调中，但是我们可以看出，这是执行的是person2.sayHi()使用的是隐式绑定，因此这是this指向的是person2，跟当前的作用域没有任何关系。

### 硬绑定(显示绑定)

显式绑定比较好理解，就是通过call,apply,bind的方式，显式的指定this所指向的对象

```js
function sayHi(){
  console.log('Hello,', this.name);
}
var person = {
  name: 'check',
  sayHi: sayHi
}
var name = 'future';
var Hi = person.sayHi;
Hi.call(person);
```

结果大家应该都知道是 Hello, check

那么，显示绑定会有隐式绑定的坑吗？

我们来看代码

```js
function sayHi(){
  console.log('Hello,', this.name);
}
var person = {
  name: 'check',
  sayHi: sayHi
}
var name = 'future';
var Hi = function(fn) {
  fn()
}
Hi.call(person, sayHi);
```

结果是啥，大家思考一下

Hello, future

why?明明通过call改变了this指向啊

再看下面这段代码

```js
function sayHi(){
  console.log('Hello,', this.name);
}
var person = {
  name: 'check',
  sayHi: sayHi
}
var name = 'future';
var Hi = function(fn) {
  fn.call(this)
}
Hi.call(person, sayHi);
```

这样就是你想的答案了

### new绑定

Js中并没有类，构造函数只是使用new操作符时被调用的函数，和普通函数并没有什么不同。任何一个函数都可以使用new来调用，此时会执行以下操作：

1、创建一个空对象，构造函数中的this指向这个空对象

2、将对象与构造函数进行原型链接

3、执行构造函数方法，属性和方法被添加到this引用的对象中

4、如果构造函数中没有返回其它对象，那么返回this，即创建的这个的新对象，否则，返回构造函数中返回的对象。

我们来看new如何改变this的指向的

```js
function _new() {
  let target = {}; 
  let [constructor, ...args] = [...arguments];
  target.__proto__ = constructor.prototype;
  let result = constructor.apply(target, args);
  if (result && (typeof (result) == "object")) {
      return result;
  }
  return target;
}

function sayHi(name){
  this.name = name;

}
var Hi = new sayHi('check');
console.log(Hi)
console.log('Hello,', Hi.name);
```
这样大家应该都理解了

四种绑定规则有一个优先级需要记住

new绑定 > 显式绑定 > 隐式绑定 > 默认绑定

到这里this的绑定我们就讲完了，回顾一开始的题，大家来自己做一下吧

答案给到大家

10 9 3 27 20

你做对了吗？








