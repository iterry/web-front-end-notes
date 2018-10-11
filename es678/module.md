模块\(Module\)体系是ES6新加入的模块加载方案。ES6之前，社区流行的模块加载规范主要是CommonJS、CMD、AMD三种，ES6 Module规范能以更简单的实现取代它们。

### 一、CommonJS、CMD、AMD模块规范

这三个模块规范都是为JS模块化加载而生，三者有关联也存在差异。

#### CommonJS

模块规范的初衷一方面是为了合理的切分管理代码文件、依赖管理以及按需引用和加载，另一方面也为了增强JS程序的可以执行和可交换性，不局限于浏览器端。

CommonJS作为最早流行的模块规范，设计初衷也在于此：它希望JS能运行在任何地方，Nodejs就采用了这一规范。

它有如下特点：

* 一个文件就是一个模块；
* 通过exports 或 module.exports 导出模块接口，require方法来加载模块文件；
* 一次加载就执行整个脚本，重复加载从缓存中取值；
* 加载模块时同步的；

由于CommonJS是同步加载模块，这对于服务器端不是一个问题，因为所有的模块都放在本地硬盘。等待模块时间就是硬盘读取文件时间，很小。但是，对于浏览器而言，它需要从服务器加载模块，涉及到网速，代理等原因，一旦等待时间过长，浏览器处于”假死”状态。决定了不适用于浏览器环境。

```js
// 定义模块math.js
exports.add = function(a, b){
    return a + b;
}
// 调用模块
let math = require('./math')
math.add(1, 2);
```

实现：服务器端的Node.js/NPM模块、Browserify、modules-webmake。

#### AMD

AMD及异步模块定义\(Asynchronous Module Definition\)规范的出现，主要是为了实现浏览器端异步加载模块。**这里异步指的是不堵塞浏览器其他任务（dom构建，css渲染等），而加载内部是同步的（加载完模块后立即执行回调）。**

它有如下特点：

* 需要在声明模块时指定所有的依赖dependencies，并作为形参传到factory；
* 依赖的模块提前执行，**依赖前置**；
* 通过define\(\[\], fn\) 和 require\(\[\], fn\) 定义和声明；

```
// 定义模块
define('math',[], function(){
    var add = function(x, y){
        return x + y;
    }
    return {
        add: add
    }
})
//加载模块
require(['math'], function(math){
    var count = math.add(1,2);
    return count++;
})
```

AMD模块采用异步加载，并支持并行加载多个模块的方式，RequireJS、curl等浏览器端模块加载器都是遵照AMD规范来实现。

相较于CommonJS，AMD开发成本比较高，代码的管理和书写方式都相对不够简洁，不符合常规的模块化思维方式。

#### CMD

全称Common Module Definition，同AMD类型，也是适用于客户端的模块规范。

本质上，CMD的出现是为推广Sea.js而制定的。

它有如下特点：

* 依赖就近，延迟执行；
* 依赖SPM打包；
* Sea.js、coolie依此实现；

依赖就近的意思是：可以把依赖写在代码的任意位置，规范会搞定预加载。

```
define(function(require, exports, module) {
  var a = require('./a')
  a.dosomething();
  var b = require('./b')
  b.dosomething();
})
```

### 二、ES6 Modules

这是JS第一次原生支持模块的定义和调用，它定义了模块的定义和调用方式，直接使用import和export导入和导出模块。

类似CommonJS，语法简洁，支持循环依赖；

类似AMD，支持异步加载和有条件的模块加载。

#### 特点和用法

ES6模块化有如下特点：

* 一个文件一个模块、一个模块只加载和执行一次，如果需要重复加载，直接从内存读取；
* 模块内的变量都是局部变量，不会污染全局环境；
* **import入的值遵循“动态只读引用”，即无论是基础类型还是引用类型，变量是只读不可写的，但引用类型的属性和方法可以添加和修改；原始值发生变化，import加载的值也会变化，**。因此，对引入变量值的读取是在被引用模块去读取值。
  这点与CommonJS不同，CommonJS对外暴露的值，基础类型属于复制，可缓存；引用类型属于浅拷贝，两个模块引用对象指向相同内存空间。

#### 1、引入ES6 module的JS文件，需要使用属性 type=“module” 标记。

```
<script type="module" src="a.js"></script>
```

#### 2、使用export关键字导出对外内容，如：变量、函数、对象等；

导出方案支持：声明式导出、导出变量名、导出别名、匿名导出、通配符导出等等。

注意，只能有一个默认导出。

```
// 声明式导出
export var a = 1;
export var a = 1, b = function(){}, c = {};
export function funcName(){}
export class className{}

// 导出变量名
let name1, name2, name3 = ...;
export {name1, name2, name3, ...}

// 导出默认值
export default 3;
export default expression;
export default function(){};
export {name1 as default, ...};

// 导出别名
export {a as name1, b as name2,...}

// 导出通配符
export * form ...;
```

#### 3、使用import导入由另一个模块导出的接口或变量；

无论是否声明语法严格模式 strict mode，导入的模块都运行在严格模式下；

导入方案支持如下写法：

```
// 同时导入单个、多个、多个带别名
import {a} from './a';
import {a, b, c} from './a';
import {a as aaa, b as bbb} from './a';
// 导入默认值
import defaultExport form './a';
// 同时导入默认值、多个
import defaultExport, {a,b,c} from './a';
// 通配导入并使用别名
import * as newname from './a';
// 导入模块，将运行模块中的全局代码，实际不导入任何值
import './a'
```

### 三、处理循环加载

循环加载值得是模块间相互引用，彼此互为依赖。CommonJS和ES Module对循环依赖的处理有差别。

#### CommonJS模块的循环加载

**一旦模块被循环加载，就只输出已经执行的部分，未执行部分不会输出，待依赖的模块执行完毕，继续执行未执行部分。**

再次调用已经执行的依赖模块，直接取缓存的结果输出。

```
// a.js
exports.done = false;
var b = require('./b.js');
console.log('在 a.js 之中，b.done = %j', b.done);
exports.done = true;
console.log('a.js 执行完毕');

// b.js
exports.done = false;
var a = require('./a.js');
console.log('在 b.js 之中，a.done = %j', a.done);
exports.done = true;
console.log('b.js 执行完毕');

// 入口文件
var a = require('./a.js');
var b = require('./b.js');
console.log('在 main.js 之中, a.done=%j, b.done=%j', a.done, b.done);

// 输出
// 在 b.js 之中，a.done = false
// b.js 执行完毕
// 在 a.js 之中，b.done = true
// a.js 执行完毕
// 在 main.js 之中, a.done=true, b.done=true
```

#### ES6 Module规范的循环加载

ES6载入模块，不会立即去执行模块，而是生成一个引用，待需要调用这个引用再去原模块去取值。

因为，ES6不关心是否发生了循环加载，只是生成一个指向被加载模块的引用。

只要引用存在，代码就能执行；只要在需要取值的时候能取到值，代码就不会报错。

```
// a.js
import {bar} from './b.js';
export function foo() {
  console.log('foo');
  bar();
  console.log('执行完毕');
}
foo();

// b.js
import {foo} from './a.js';
export function bar() {
  console.log('bar');
  if (Math.random() > 0.5) {
    foo();
  }
}

// 执行a.js，输出如下
// foo
// bar
// 执行完毕
或
// foo
// bar
// foo
// bar
// 执行完毕
// 执行完毕
或
依据random结果继续循环调用
```



