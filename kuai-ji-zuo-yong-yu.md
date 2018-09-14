这篇笔记针对JS的作用域展开讨论，包含两个方面的知识：

* 声明提升
* 变量解析顺序
* 作用域

### 一、变量提升（hoisting）

变量提升其实是“变量声明提升”，你可以引用稍后声明的变量而不会让代码报错。

变量提升的实质是：将声明的变量“提升”或移到函数体或语句的顶部，该变量返回undefined或函数体。

注意以下几点：

**1、必须使用var关键字定义的变量才能提升，无var关键字或let、const定义的变量不会提升；**

```js
console.log(a);  // undefined
var a = 1;

等价于
var a; // a=undefined
console.log(a);
a;

---------------------------------------

console.log(a);  // a is not defined
a = 1;

---------------------------------------

console.log(a,b);  // a,b is not defined
let a = 1;
const b = 1;
```

**2、针对函数，表达式赋值式不会被提升；只有声明式定义才会被提升，而且，函数体一并被提升；**

```js
console.log(foo);  // f foo(){...}
function foo(){
    var a = 1;
    return a; 
}

---------------------------------------

console.log(foo);  // foo is not defined
var func = function foo(){
    var a = 1;
    return a; 
}

---------------------------------------

foo();  // foo is not defined
var foo = function() {
    var a = 1;
    return a;
};
```

### 二、变量解析顺序（Name Resolution Order）

变量提升影响了JS变量的解析顺序，除此之外，还有几点需要注意：

**1、同一作用域内，声明相同名称的变量会被覆盖；**

```js
var a = 1;
var a = 2;
console.log(a);  // a = 1;
```

**2、变量和函数如果声明相同的名称，函数声明比变量声明优先级高，且不受顺序影响；**

```js
function a(){
    var b = 1;
    return 1;
}
var a;
console.log(a);  // f foo(){...}
```

**3、如果变量声明且被赋值，函数声明不会覆盖该变量的值，且不受顺序影响；**

```js
var a = 1;
function a(){
    var b = 1;
    return 1; 
}
console.log(a);  // a = 1
console.log(a());  // a is not a function
```

### 三、作用域（scope）

1、在所有函数之外声明的变量，叫做全局变量，可在代码任意位置被访问；

在函数内声明的变量，叫做局部变量，无论是否带var关键字，只能在函数内部被访问；

```
var a = 1;
function foo(){
    console.log(a);
}
foo(); // a = 1

-------------------------------------------------

function foo(){
    var a = 1;
    b = 2;
}

foo();
console.log(a,b)  // a,b is not defined
```

2、ES6之前仅有函数作用域和全局作用域，在代码块中声明的变量会成为其所在函数的局部变量；

ES6中引入let关键字定义变量后，变量仅在其所在的代码块内有效，即形成了代码块作用域。



