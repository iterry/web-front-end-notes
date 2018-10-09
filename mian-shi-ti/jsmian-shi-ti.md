#### 1、三种及以上方法合并两个一位数值。

```js
var arr1 = [1,2,3],
    arr2 = [4,5,6];

// arr1 = [1,2,3,4,5,6]
arr1.concat(arr2);

// arr1 = [1,2,3,4,5,6]
Array.prototype.push(arr1, arr2);

// arr1 = [1,2,3,4,5,6]
arr1.push(...arr2);

// arr1 = [1,2,3,4,5,6]
arr1.splice(arr1.length, 0, ...arr2)

// arr3 = [1,2,3,4,5,6]
arr3 = [...a1, ...a2]

// arr3 = [1,2,3,4,5,6]
arr3 = Array.of(...arr1, ...arr2)
```

#### 2、实现一个函数，满足如下结果：

```
add(1) //1
add(1)(2) //3
add(1)(2)(3)(4) //10
```

```
// 考察valueof 或者 toString 相关知识
function add(){
    var args = Array.prototype.slice.call(arguments);
    var fn = function(){
        var args1 = Array.prototype.slice.call(arguments);
        return add.apply(null, args.concat(args1));
    }
    // 关键是理解valueOf()会在函数调用的时候自行调用
    fn.valueOf = function(){
        return args.reduce((prev, curr) => {
            return prev+curr;
        })
    }
    return fn;
}
```

#### 3、计算以下代码的值并解释原因：

```
// 考察运算优先级、解析顺序、对象类型、原型等知识
var a = {n:1}; 
var b = a;  
a.x = a = {n:2}; 
console.log(a.x);// undefined 
console.log(b.x);// [object Object]
```

参考地址：[http://www.cnblogs.com/vajoy/p/3703859.html](http://www.cnblogs.com/vajoy/p/3703859.html "从一个简单例子来理解js引用类型指针的工作方式")

#### 4、将整数值转化为字符串：

将整数1转为为字符串有多种方法：

```js
// 方法：使用表达式计算
// 原因：+运算符后面的变量会转换成 前面的变量的类型再进行计算处理
'' + 1 = '1';

// 方法：使用自带的toString()方法
// 注意：对于整数值不能直接调用toString()。因为JS引擎无法辨别 . 指代浮点小数点 还是 点操作符
1.toString();  // 错误，会报错
1..toString();  // 正确，前者解释为小数点，后者解释为 点操作符
1.0.toString(); // 正确

// 方法：使用()操作符
// 原因：()返回表达式的值，该值能当做一个Number对象，.被解释为点操作符，从而能调用toString()方法
(1).toString();
```

#### 5、分析01+0.2 != 0.3的原因及解决方法：

原因：

ECMAScript规范定义的Number类型遵循IEEE754-2008中的64位浮点数的规则，即：小数后的有效位数至多为52位。而遵循十进制转二进制转换规则“整数，除2取余；小数，乘2取整”进行计算，0.1和0.2都会得到无穷循环的字节：

```
0.1  =  0.0001100110011001100110011 (0011)无限循环
0.2  =  0.001100110011001100110011 (0011)无限循环
```

遵从52位的取值规则，会出现精读丢失的问题，从而导致以上问题。

解决方法：

```
// 方法一：将小数转换为整数再计算
（0.1+0.2）*10  === 0.3 * 10

// 方法二：使用toFixed方法返回指定长度浮点数字
parseFloat((0.1+0.2).toFixed(10))
```

#### 6、简述encodeURI和encodeURIComponent区别

两个方法都是对URL进行编码，唯一的区别是编码的作用范围，其中：

* encodeURL不会对以下字符编码：ASCII字母、数字、~!@\#$&\*\(\):/,;?+'。
* encodeURIComponent不会对以下字符编码：ASCII字母、数字、~!\*\(\)'

由此看出，后者比前者的编码范围更大。所以，需要对URL中包含query参数进行字符编码必须使用encodeURLComponent，如微信签名；对URL编码且还需要访问，直接使用encodeURL即可。

