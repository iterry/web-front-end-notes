**这篇笔记主要记录以下重要知识：**

1. 运算符的优先级。
2. 相等和全等运算符的规则及注意事项。

### 1、运算符的优先级。

运算符的优先级在代码解读中极其重要，写法不当一不小心就陷入输出和期望不符的情况。

先汇总下JS中运算符的优先级别，优先级高的先执行：

![](/assets/yunsuanfu.jpg)

#### 几个注意的事项：

1、赋值运算符（=）的顺序是从右到左，多个连等运算需要注意赋值顺序。

2、当出现连等赋值运算和高优先级的运算符同时使用，一定要注意赋值顺序，见如下例子：

```js
var a = {n:1}; 
var b = a;  
a.x = a = {n:2}; 
console.log(a.x);// undefined 
console.log(b.x);// [object Object]
```

---

### 2、== 和 === 的规则和注意事项。

== 为相等运算符，===为全等运算符。

**全等的规则如下：优先比较两者的类型，再比较值。**

1、不同类型，直接返回false；

2、类型相同的基础类型（String、Number、Null），值相同返回true，否则返回false；

3、类型相同的基础类型（undefined、null），同类型与自身严格相等；

4、类型相同的引用类型（Object、Array、Function），直接比较两者的引用指针是否指向同一内存空间/对象。值相同，引用指针不同也为false；         



**相等运算符规则如下：优先比较两者的类型，类型不同则先转换类型，再比较值。**

1、类型不同的基础类型（String、Number、Null）之间，**先转换为数值类型**再比较值；

2、类型不同的基础类型（undefined、null）**与其他任意类型比较均为false，两者之间比较则为true**；

3、基础类型和引用类型比较，**先将引用类型转换为基础类型得到值**，两者再比较；

4、类型相同的引用类型比较，**只要两者的引用指针不同，即便类型相同、值相同也为false**；

5、**NaN和包括自身在内的任意类型值比较，返回false；**

6、{} 除了和同享引用指针的同类型全等外，**和包括自身在内的其他任意类型都不相等**；

7、Symbol相互之间，以及和其他任意类型比较，返回false；

**以下为示例：**

```js
undefined === undefined // true
null === null // true
undefined === null  // false
undefined == null  // true
undefined == 0  //false
null == false  // false
!null = '1'  // true  Number(!null) == 1

a = d = [1];
b = [1];
c = 1;
a === b // false
a == b // false
a == c // true
a === d // true;

1 == !undefined // true
'' == '0'  // false
'' == 0  // true Number('')==0

'\t\r\n' == 0 // true Number('\t\r\n')==0

NaN == NaN // false
NaN == 0  // false
{} == 0 // false  Number({}) == NaN

Symbol('1') == Symbol('1')  // false
!Symbol == 1  // true
```

**一般建议：**

1、由于不同的类型变量比较会转换类型，可能会产生意料之外的结果，所以，为了代码严谨考虑，推荐使用 ===；
2、异或运算符在算法中用的很广泛，有几个特点：相同为0，不同为1.，异或同一个数两次，原数不变。

