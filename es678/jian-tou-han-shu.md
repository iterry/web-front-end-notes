#### 箭头函数是ES6引入的新概念，用来定义函数。

#### 一、定义和调用

基本定义和使用：

```
let func = a => ++a;
// 等价于
let func = function(a){
    return ++a;
}

let func = (a,b) => a+b;
// 等价于
let func = function(a, b){
    return a + b;
}

// 当函数体代码块多于一条，用{}括起来
let func = (a,b) => {
    let c = a+b;
    return c;
}
// 等价于
let func = function(a,b){
    return a+b;
}

//使用
func();
```

如果代码块返回值是一个对象，必须在对象外层加上小括号：

```
let func = (a, b) => ({ a: a, b:b });

//等价于
var func = function func(a, b) {
   return { a: a, b: b };
};
```

箭头函数可以大大简化回调函数：

```
[1,2,3,4].map( n => n*n);

// 等价于
[1,2,3,4].map(function(n){
    return n*n;
});
```

配合rest数组模拟arguments对象：

```
let func = (a, ...values) => {
    let total = 0;
    total += a;
    for(let val of values){
        total += val;
    }
    return total;
}
func(1, 2, 3, 4);  // 10
```

#### 二、与Fcuntion的联系和区别

箭头函数不是function函数的缩写。

typeof返回的和function函数的一致；它是Function的实例：

```
let func = n => n;
console.log(typeof func);  // function
console.log(func instanceof Function);  //true
```

**箭头函数块内的this指针指向定义时所在的上下文，在生存期内固定不变，使用call、apply、bind方法也无法更改：**

```
function func() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 1000);
}

var id = 1;

func.call({ id: 42 });  // 42
```

箭头函数不能当做构造函数和Generator函数，即不能通过 new 命令创建一个对象：

```
var Car = (title, color) =>{
  this.title = title;
  this.color = color;
}

var car = new Car('benz','black');  // Uncaught TypeError: Car is not a constructor
```

箭头函剔除了arguments类数组对象，argument在函数体内被当做普通变量看待：

```
// ES6写法
let func = () => {
  console.log(arguments[0])
}

func(1)； // Uncaught SyntaxError: Identifier 'func' has already been declared

// babel编译为
var _arguments = arguments;
var func = function func() {
  console.log(_arguments[0]);
};

func(1);
```

用扩展运算符来代替arguments：

```
// arguments变量的写法
function sortNumbers() {
  return Array.prototype.slice.call(arguments).sort();
}

// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```



