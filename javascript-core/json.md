### 什么是数据交换格式
不同的计算机程序或者不同的编程语言间往往存在数据往来和处理解析，不同的语言有不同的数据结构和语法，要想不同的语言“看得懂”，就出现了数据交换格式：就像是程序间的能共同理解的语言，它通过文本以特定的格式来描述数据。常见的交换语言包括：

* JSON-JavaScript Object Notation
* XML-eXtensible Markup Language
* YAML-Yet Another Markup Language

XML是可扩展标记语言，类似XHTML的标签树结构。
采用闭合标签写法，对大小写敏感，可通过DOM循环遍历来读写数据，无论书写还是解析都比较麻烦。
完整的XML文件包括：声明、根标签、子元素和属性等：

``` js
<?xml version="1.0" encoding="UTF-8" ?>
<dates>
    <date>
        <id>1</id>
        <name>name</name>
        <abb>car</abb>
    </date>
    <date>
        <id>2</id>
        <name>color</name>
        <abb>red</abb>
    </date>
</dates>
```
YAML是一种直观的能够被电脑识别的数据序列化格式。
书写简单，解析方便，适合做序列化，适用于脚本语言使用。

```js
dates: 
 date: 
  - 
   id: 1
   name: name
   abb: "car"
  - 
   id: 2
   name: color
   abb: "red"
```

### JSON简介
JSON（Javascript Object Notation），是一种轻量级的数据交换格式。
它是基于 JavaScript Programming Language , Standard ECMA-262 3rd Edition - December 1999 的一个子集。
但它完全独立于程序语言，又因其类C语言的数据表达和结构，所以运用广泛。
JSON是键值对的序列化集合，值的取值可以是数值、字符串、布尔、null、数值及对象等，但其格式有以下几点需要注意：

* 属性名称为字符串，最后一个属性值后不能有逗号。
* 无论是key还是value，只要是字符串必须用双引号括起来，否则会报错。
* 只支持如下空白字符：制表符（U+0009），回车（U+000D），换行（U+0020）以及空格（U+0020）。

### JSON属性和方法

**JSON.parse(value [,reviver])** 
用来解析JSON字符串，构造由字符串描述的JavaScript值或对象。提供可选的reviver函数用以在返回之前对所得到的对象执行变换(操作)。
```js
var str = '{"name":"car","price":1000000}';

JSON.parse(str);
// 输出：{name: "car", price: 1000000}

JSON.parse(str, function(k, v){
  if(k === "name"){
    return "new" + v;
  }
  return v;
});
// 输出：{name: "newcar", price: 1000000}
```

**JSON.stringify(value[, replacer [, space]])**
将一个JavaScript值(对象或者数组)转换为一个 JSON字符串，如果指定了replacer是一个函数，则可以替换值，或者如果指定了replacer是一个数组，可选的仅包括指定的属性,space指定缩进时的空白字符串用于美化输出。

```js
var json = {
  "name": "car",
  "price": 1000000,
  "canbuy": true,
  "month": [7,8,9,10,11],
  "other" : {
    "color" : "red",
  }
}

JSON.stringify(json)
// 输出："{"name":"car","price":1000000,"canbuy":true,"month":[7,8,9,10,11],"other":{"color":"red"}}"

JSON.stringify(json, ['name','price'])
// 输出："{"name":"car","price":1000000}"

JSON.stringify(json, function(k, v) {
     if (k === "name"){
         return v.toUpperCase();
     }
     return v;
})
// 输出："{"name":"CAR","price":1000000,"canbuy":true,"month":[7,8,9,10,11],"other":{"color":"red"}}"

JSON.stringify(json, ['name','price'], '    ');
// 输出："{
    "name": "car",
    "price": 1000000
}"

```
此外，还可以为JSON定义toJSON()方法，直接返回序列化数据：
``` js
var json = {
  "name": "car",
  "price": 1000000,
  "canbuy": true,
  "month": [7,8,9,10,11],
  "other" : {
    "color" : "red",
  },
  toJSON: function () {
     return { 
         'newname': this.name,
         'newprice': this.price
     };
  }
}
// 输出："{"newname":"car","newprice":1000000}"
```





