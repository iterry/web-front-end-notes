JSON（Javascript Object Notation），是一种轻量级的数据交换格式。XML也是一种数据交换格式，但其语法、规范和处理麻烦，雅虎的工程师道格拉斯·克罗克福特发明了JSON。虽然名字中有Javascript，也基于JS语法，但JSON不是JS的子集，而是独立的数据交换格式。

JSON是键值对的序列化集合，值的取值可以是数值、字符串、布尔、null、数值及对象等，但其格式有以下几点需要注意：

* 属性名称为字符串，最后一个属性值后不能有逗号。
* 无论是key还是value，只要是字符串必须用双引号括起来，否则会报错。
* 只支持如下空白字符：制表符（U+0009），回车（U+000D），换行（U+0020）以及空格（U+0020）。

**JSON有两个方法：**

* JSON.parse\(\) —— 解析JSON字符串转化为Object格式，可以额外传入一个转换函数，用来将生成的值和其属性, 在返回之前进行某些修改。
* JSON.stringify\(\) —— 将Object转化为JSON字符串，可以通过额外的参数, 控制仅包含某些属性, 或者以自定义方法来替换某些key对应的属性值。
  ## [ ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON#Polyfill) {#Polyfill}



