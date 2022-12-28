---
description: 作为一名前端开发，js是前端的中坚力量。那么 javascript 三座大山，你知道是哪些呢？
---

# 🌄 三座大山

## 1️⃣ 作用域和闭包

**`作用域`**指代码当前上下文，控制着变量和函数的可见性和生命周期。最大的作用是隔离变量，不同作用域下同名变量不会冲突。

**`作用域链`**指如果在当前作用域中没有查到值，就会向上级作用域去查，直到查到全局作用域，这样一个查找过程形成的链条就被称之为作用域链。

作用域可以堆叠成层次结构，子作用域可以访问父作用域，反过来则不行。

作用域可以细分为四种：**`全局作用域`**、**`模块作用域`**、**`函数作用域`**、**`块级作用域`**

**全局作用域：** 代码在程序的任何地方都能被访问，例如 window 对象。但全局变量会污染全局命名空间，容易引起命名冲突。

**模块作用域：**早期 js 语法中没有模块的定义，因为最初的脚本小而简单。后来随着脚本越来越复杂，就出现了模块化方案（AMD、CommonJS、UMD、ES6模块等）。通常一个模块就是一个文件或者一段脚本，而这个模块拥有自己独立的作用域。

**函数作用域：**顾名思义由函数创建的作用域。闭包就是在该作用域下产生。

**块级作用域：**由于 js 变量提升存在变量覆盖、变量污染等设计缺陷，所以 ES6 引入了块级作用域关键字来解决这些问题。典型的案例就是 let 的 for 循环和 var 的 for 循环。

```javascript
// var demo
for(var i=0; i<10; i++) {
    console.log(i);
}
console.log(i); // 10

// let demo
for(let i=0; i<10; i++) {
    console.log(i);
}
console.log(i); //ReferenceError：i is not defined
```

了解完作用域接下来说说**`闭包`：**函数A里包含了函数B，而函数B使用了函数A的变量，那么函数B被称为闭包或者闭包就是能够读取函数A内部变量的函数。

可以看出闭包是函数作用域下的产物，闭包会随着外层函数的执行而被同时创建，它是一个函数以及其捆绑的周边环境状态的引用的组合。换而言之，**闭包是内层函数对外层函数变量的不释放**。

闭包的特征：1. 函数内返回函数；2. 内部函数可以访问外层函数的作用域；3. 参数和变量不会被 GC，始终驻留在内存中；4.没有内存，就没有闭包。

所以使用闭包会消耗内存、不正当使用会造成内存溢出的问题，在退出函数之前，需要将不使用的局部变量全部删除。如果不是某些特定需求，在函数中创建函数是不明智的，闭包在处理速度和内存消耗方面对脚本性能具有负面影响。

**`闭包的应用场景：`**

<pre class="language-typescript" data-title="循环中使用"><code class="lang-typescript"><strong>// demo1 输出 3 3 3
</strong>for(var i = 0; i &#x3C; 3; i++) {
    setTimeout(function()ty
        console.log(i);
    }, 1000);
} 
// demo2 输出 0 1 2
for(let i = 0; i &#x3C; 3; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}
// demo3 输出 0 1 2
for(let i = 0; i &#x3C; 3; i++) {
    (function(i){
        setTimeout(function() {
        console.log(i);
        }, 1000);
    })(i)
}
</code></pre>

{% code title="模拟私有方法" %}
```typescript
// 模拟对象的get与set方法
var Counter = (function() {
var privateCounter = 0;
function changeBy(val) {
    privateCounter += val;
}
return {
    increment: function() {
    changeBy(1);
    },
    decrement: function() {
    changeBy(-1);
    },
    value: function() {
    return privateCounter;
    }
}
})();
console.log(Counter.value()); /* logs 0 */
Counter.increment();
Counter.increment();
console.log(Counter.value()); /* logs 2 */
Counter.decrement();
console.log(Counter.value()); /* logs 1 */
```
{% endcode %}

{% code title="setTimeout中使用" %}
```typescript
// setTimeout中使用，setTimeout(fn, number): fn 是不能带参数的。使用闭包绑定一个上下文可以在闭包中获取这个上下文的数据。
function func(param){ return function(){ alert(param) }}
const f1 = func(1);setTimeout(f1,1000);
```
{% endcode %}

{% code title="生产者/消费者模型" %}
```typescript
/*不使用闭包*/
//生产者
function producer(){
    const data = new（...）
    return data
}
//消费者
function consumer(data){
    // do consume...
}
const data = producer()

/*使用闭包*/
function process(){
    var data = new (...)
    return function consumer(){
        // do consume data ...
    }
}
const processer = process()
processer()
```
{% endcode %}

{% code title="实现继承" %}
```typescript
// 以下两种方式都可以实现继承，但是闭包方式每次构造器都会被调用且重新赋值一次所以，所以实现继承原型优于闭包
// 闭包
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
  this.getName = function() {
    return this.name;
  };

  this.getMessage = function() {
    return this.message;
  };
}
// 原型
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}
MyObject.prototype.getName = function() {
  return this.name;
};
MyObject.prototype.getMessage = function() {
  return this.message;
};
```
{% endcode %}

<figure><img src="../.gitbook/assets/closure.png" alt=""><figcaption><p>一张图看懂闭包的生命周期</p></figcaption></figure>

## 2️⃣ 原型和原型链

## 3️⃣ 异步和单线程
