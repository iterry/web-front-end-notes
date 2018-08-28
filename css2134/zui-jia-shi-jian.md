CSS代码优化常常被前端工程师忽略，究其原因，很多重构同学认为CSS对页面渲染和性能的影响很小，小到甚至可以忽略不计。

这是一个很可笑且可怕的想法，先不论有何影响大小，“既然知晓何为优秀、何为糟粕，就应该将优秀当做一种习惯”。

#### **具体来说，不好的CSS有如下表现：**

* 没有统一的命名规则，命名混乱
* 大量代码冗余，不考虑层叠、简写和继承
* 加载引入过多的文件
* 未压缩，特别是大文件
* 兼容体验差
* 实现方案差

接下来，总结下推荐的实践方案：

### 一、命名和模块规范

#### 1、样式文件放在head头部

页面渲染中，CSStree 和 DOMtree 共同构成 RenderTree，假若样式文件不在头部引入加载，在视觉上容易造成无样式的Dom闪屏，在渲染上会引起页面的整体重绘和重排。

#### 2、使用BEM或者团队自行规范的命名规则

BEM命名规范如下：

* BEM 实体名称全部是小写字母或数字。名称中的不同单词用单个连字符（-）分隔。
* BEM 元素名称和块名称之间通过两个下划线（\_\_）分隔。
* 布尔修饰符和其所修饰的实体名称之间通过两个连字符（--）来分隔。不使用名值对修饰符。

当然你完全可以使用其他的命名规范，但一定需要规范，便于协作和维护。

---

### 二、模块管理

#### 1、样式文件依据功能单独管理，可以参照SMACSS方案

SMACSS指的是可扩展和模块化的CSS，它把CSS样式规则分成几个不同类别：

* 基础：该类别中包含的是默认的 CSS 样式。作为其他样式的基础。
* 布局：该类别中包含与页面布局相关的 CSS 样式，用来进行模块的排列。
* 模块：该类别中包含的是可复用的模块的 CSS 样式。
* 状态：该类别中的 CSS 样式用来描述布局和模块在不同状态下的外观。比如在不同的屏幕尺寸下，布局会发生变化。标签式模块的每个标签页可以有显示或隐藏的状态。
* 主题：该类别和状态类似，只不过是用来改变布局和模块的视觉效果。

SMACSS管理方案可以配置预处理器使用，管理CSS模块更加明了和方便。

---

### 三、样式书写

#### 1、避免使用CSS Expressions 和 Filters

这两点是针对IE6~8的，对性能的损耗很大，而且高级浏览器也不支持这种IE的私有写法。

```
div{
    width:expression(this.Width < 600 ? “600px” :”100%” ); /*依据尺寸适配宽度*/
    filter:alpha(50); /*IE6-IE7，数值是0-100的任意整数，0表示透明*/

}
```

注意：此处Filters不是CSS3 中的filter滤镜。

#### 2、通用声明合并

尽量合并公共声明，减少冗余，方便修改。

```
// 冗余声明
.Class1{position: absolute; left: 20px; top: 10px;} 
.Class2{position: absolute; left: 20px; top: 20px;} 
.Class3{position: absolute; left: 20px; top: 30px;} 
.Class4{position: absolute; left: 20px; top: 40px;} 

// 修改后
 .class1 .class2 .class3 .class4{ 
   Position: absolute; left: 20px;
 }
```

#### 3、缩写

颜色、背景、边框、边距、列表、文字、动画等属性尽量用简写。

```
// 颜色
color:#336699;

color:#369;

// 文字
font-style:normal;
font-variant:small-caps;
font-weight:bold;
font-size:14px;
line-height:1.5em;
font-family:'宋体',arial,verdana;

font:normal small-caps bold 14px/1.5em '宋体',arial,verdana;

// 背景
background-color:#FF0;
background-image:url(bg.gif);
background-repeat:no-repeat;
background-attachment:fixed;
background-position:left top;
background-size:cover;
background-origin:center;

background:#FF0 url(bg.gif) no-repeat fixed left top/cover center;

// 动画
animation-duration:3s;
animation-timing-function:ease-in;
animation-delay:1s;
animation-iteration-count:2;
animation-direction:reverse;
animation-fill-mode:both;
animation-play-state:paused;
animation-name:slidein;

animation: 3s ease-in 1s 2 reverse both paused slidein;
```

#### 4、继承不重写

css很多属性具有继承属性，比如：颜色、文本、字体、显隐等等，继承的属性如果不是需要覆盖原属性就没必要重写。

```
font、visiblity、cursor、text-indent、text-align、line-height、word-spacing、letter-spacing、text-transform、
direction、color、list-style等等
```

---

### 四、选择器

在提出建议之前，先看看一个简单的例子：

```
#list>li{
    color: red;
}
```

按照很多人的理解，解析器应该从左至右先查找id为list的节点，再遍历其子元素查找li标签。

其实不然。**解析器对样式选择器的解析顺序是从右向左进行匹配的**！也就是说，先查找页面上所有的li节点，再进一步判断，如果li的父节点的id为list，则匹配成功。

**依据这个原理，给出如下建议：**

* 尽量用id或者class定义元素
* 选择器层级不要过多，建议不超过三层
* 避免使用通用规则

```
// 业务中不要出现这种写法，低效
*{padding:0; margin:0}
```

* 不要在class 或者 id前加元素标签名

```
// 不建议这么写，因为通常情况下，.pic不可能作用到其他标签上
a.pic{
    max-width:100%;
}
button#pic{
    max-width:100%;
}
```

* 能用子选择的不要用后代选择器

```
// 不推荐
.list tbody tr td{padding:10px;}
// 推荐
.list>tbody>tr>td{padding:10px;}
```

---

### 五、其他

1. 尽量用雪碧图（toptal）、图片压缩（tinypng）；

2. 使用流程构建工具，无论是否配合使用Sass 或者 Stylus；

3. 配合caniuse，做好新特性的前缀兼容和优雅降级；



