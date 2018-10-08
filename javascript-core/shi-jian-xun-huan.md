这篇笔记介绍JS的执行机制——事件循环，会涉及到如下概念：单线程、异步回调、轮询、宏任务、微任务、事件队列等。

### 一、JS是单线程的语言，它的执行机制是事件循环

JS是一门单线程的语言，也就是说一个时间点只会执行一个任务。即使HTML5新增了Web Worker新标准，允许JS创建多个子线程，但Web Worker受限于主线程的控制管理，且不能操作DOM，常用的用途在大批量数据的计算处理上，本质上没有改变JS单线程的语言本质。

既然是单线程，所有的代码就需要从上到下按照顺序依次执行。如果前面的任务耗时长，后面的任务也必须排队等候。从业务场景看：图片加载、页面数据请求、音视频加载、大数据计算等耗时较长，如果一直等候这些任务加载或执行，页面的体验会有多糟糕！

### 二、事件循环的执行流程

为了解决这个问题，JS将代码任务分为两类：同步任务 和 异步任务。异步任务也成为异步回调。

如何创建异步任务呢？JS使用的是回调方法，或者称为回调函数。常见的回调包括：定时器、点击事件的执行事件、Ajax请求的success方法等等。

如何管理这两类任务的执行顺序呢？JS通过事件循环的执行机制来构建执行的并发模型。

##### 说明：

* 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数
* 当指定的事情完成时，Event Table会将这个函数移入Event Queue。
* 主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
* 上述过程会不断重复，也就是常说的Event Loop\(事件循环\)。

如图所示：![](/assets/event_loop3.jpg)**下面结合实例具体讲解这套基于“事件循环”的并发模型。**

主线程运行的时候，形成执行栈（Stack），执行栈遵循“后进先出”的原则。

普通的函数调用会被加入到栈顶，执行结束再从栈顶移除。

示例如下：

```js
// 调用函数a，a进入执行栈栈顶，由于a调用函数b，b被加入到栈顶，以此类推，c随后被加入栈顶。
// 执行栈从栈底到栈顶顺序是：a——>b——>c，执行后出栈的顺序是：c——>b——>a
function a(){b();}
function b(){c();}
function c();
a();
```

异步调用的事件或者回调函数不会立即执行，而是被添加到“任务队列”或者“消息队列”的队结构中。

当主线程执行栈内执行完毕所有的同步任务，周知消息队列，消息队列遵循“先进先出”原则，将先进入队列的任务加入执行栈执行。

这就是基本的“事件循环”模型，如下如所示：

![](/assets/event_loop2.jpg)

### 三、微任务（Microtasks）和宏任务（task）

js将执行任务分为微任务和宏任务，执行时，遇到宏任务，先执行宏任务，执行完毕再执行微任务。

* 宏任务：包括整体代码script，setTimeout，setInterval、setImmediate。
* 微任务：原生Promise、process.nextTick、Object.observe\(已废弃\)、 MutationObserver 等。

执行图示如下：

![](/assets/micro-macro-task.jpg)

定时器需要设置time参数，即设置多长时间后执行回调函数，所以，定时器的回调先会放入Event Table，等待time时长之后才会把加入消息队列Event Queue，而不是time时长之后加入到执行栈执行。

另外，将定时器的time设置为0，表示：回调任务在主线程空闲下来的时候最早执行，无需额外等待。

但官方文档规定，定时器延时间隔最小为4ms，也就是说，即便将time设置为0，其实也需要延迟4ms才会执行。

Node.js提供的setImmediate方法和setTimeout\(func,0\)原理类似，它会在下一次执行主线程读取消息队列时立即执行。

process.nextTick语句在Node中也使用的很广泛，同一般的回调不同的是，它的回调函数会直接放在当前执行栈，在执行栈之前的代码执行完后立即执行。

因此，以上非常规的回调执行顺序是：nextTick先执行，setImmediate 和 setTimeout 后执行但两者执行先后不定。

**参考：**
1、[理解Node.js实践循环](https://blog.hainest.com/understanding-the-nodeJS-event-loop/%29)
2、[并发和事件模型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop%29)
3、[Javascript中的异步](http://blog.51cto.com/cnn237111/1556987%29)
4、[详解JavaScript中的Event Loop（事件循环）机制](https://zhuanlan.zhihu.com/p/33058983%29)
5、[JS事件循环机制（event loop）之宏任务/微任务](https://juejin.im/post/5b498d245188251b193d4059%29)

