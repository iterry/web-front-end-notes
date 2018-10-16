几个非常有用的表单属性或方法：

#### 1、defaultValue——设置或返回文本框中的**初始内容**，适用 HTTMLInputElement 类型的元素。

```
 <input type="text" value="Hello world">

 var inp = document.querySelector('input');
 console.log(inp.defaultValue); // hello world

 inp.value = 'temorrow';

 console.log(inp.value); //temorrow
 console.log(inp.defaultValue); // hello world
```

用途：重置input、textarea元素的默认值；重置表单

#### 2、indeterminate——checkbox除了未选、已选外的第三种状态-不确定的，默认是false。

它有如下特点：

1. 没有对应的attribute，对它设置也不会影响到元素已有的check状态。
2. 在视觉上，不同的浏览器显示有差别。
3. 同时设置了选中和 不确定，视觉上显示为不确定。

```
<input id="checky" type="checkbox">

var inp = document.querySelector('input');
console.log(inp.indeterminate); //false
console.log(inp.checked); // false
inp.indeterminate = true;
inp.checked = true;
console.log(inp.indeterminate); // true
console.log(inp.checked); // true
```

用途：嵌套复选框的视觉运用。如下图:

![](/assets/indeterminatecheckboxes.png)

引申：CSS伪类提供 :indeterminate，用于表示状态不确定的表单元素，可以作用于checkbox、radio、progress等表单元素。

```
input:indeterminate{
    color:blue;
}
```

以上CSS属性会作用到以下元素：

1. indeterminate 属性设置为true的checkbox。
2. **未被选中的radio。**
3. **未设置vaule的progress。**

#### 3、selectionStart、selectionEnd 和 selectionDirection

这三个属性记录输入框的文本选择内容信息，前两者表示选择内容的起止索引，后者表示选择的操作方向：forward\(从左往右\)、backward\(从右往左\)，这个属性仅在使用鼠标箭头选择有效，在触摸屏选择返回none。

```
var text=document.getElementById('con');
var start=text.selectionStart;
var end = text.selectionEnd;
var ss = text.value.substring(start, end); //输出玩家选择的文本
alert(ss); 
```



