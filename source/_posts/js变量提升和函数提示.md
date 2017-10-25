---
title: js变量提升和函数提示
date: 2017-10-16 10:46:58
tags:
    - js
---
> 参考
> 深入理解js的变量提升和函数提升 [http://www.cnblogs.com/kawask/p/6225317.html/](http://www.cnblogs.com/kawask/p/6225317.html)

早上在掘金看到一篇文章，文章中插着一到面试题，是关于js变量提升的，自己这一块有些欠缺，就在网上扒了扒这块的教程，自己也做个笔记。后来看到 ** 函数提升优先级比变量提升要高，且不会被变量声明覆盖，但是会被变量赋值覆盖 ** 恍然大悟。

** 函数提升优先级比变量提升要高，且不会被变量声明覆盖，但是会被变量赋值覆盖 **
** 函数提升优先级比变量提升要高，且不会被变量声明覆盖，但是会被变量赋值覆盖 **
** 函数提升优先级比变量提升要高，且不会被变量声明覆盖，但是会被变量赋值覆盖 **

* * *

题目如下：
```js
alert(a)
a();
var a=3;
function a(){
  alert(10)
}   
alert(a)
a = 6;
a();
```

### 变量提升

举个例子：
```js
console.log(name)       // undefined
var name = 'xlslucky';
console.log(name)       // xlslucky

function fn() {
  console.log(sex)      // undefined
  var sex = 'man';
  console.log(sex)      // man
}
fn()
```
由于js变量提升，实际是按照以下执行的：
```js
var name;             // 变量提升，全局作用域范围内，只是声明，并没有赋值
console.log(name)     // undefined
name = 'xlslucky';    // 赋值
console.log(name)     // xlslucky

function fn() {
  var sex;            // 变量提升，函数作用域范围内
  console.log(sex)    // undefined
  sex = 'man';        // 赋值
  console.log(sex)    // man
}
fn()
```

### 函数提升

js创建函数有两种方式：函数声明和函数表达式。只有函数声明才存在函数提升。
```js
console.log(f1)
console.log(f2)
function f1() {}
var f1 = function (){}
```
上面代码执行顺序：
```js
function f1(){}
var f2;
console.log(f1)    // function f1(){}
console.log(f2)    // undefined
f2 = function (){}
```



### 回顾下上面的题
```js
function a(){   // 函数提升
  alert(10)
}
var a;          //变量声明，不会覆盖变量 
alert(a)        // function a(){ alert(10) }
a();            // 10
a = 3;
alert(a)        // 3
a = 6;
a();            // 报错
```

** 函数提升优先级比变量提升要高，且不会被变量声明覆盖，但是会被变量赋值覆盖 **
** 函数提升优先级比变量提升要高，且不会被变量声明覆盖，但是会被变量赋值覆盖 **
** 函数提升优先级比变量提升要高，且不会被变量声明覆盖，但是会被变量赋值覆盖 **