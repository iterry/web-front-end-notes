在介绍Stylus和Sass等CSS预处理器/语言之前，先理清楚两个主要的问题：

1. 什么是预处理器？它的作用是什么？
2. CSS原生语言当前的写法和书写习惯存在什么问题，才出现了各种预处理语言的诞生？

### **为什么要先弄清楚这两个问题呢？**

首先，也是最重要的，带着问题去探究知识和技术，学习的好奇心和目的性更强烈，学习效果也会更好。

其次，用逻辑学的思维去分析和拆解问题，将一个问题扩展成多个问题，更加弄清问题的本质。

比如，对于前端开发特别是页面重构的同学来说，写CSS语言基本是“所见即所得”，所以，很大可能性并不清楚“预处理器”这个编程语言的概念的含义和作用。反过来，如果不清楚“预处理器”的含义，又怎么可能真正理解CSS预处理器的作用呢。所以，我建议，对于新知识的学习，最基础也最重要的就是拆解句子中的概念和词汇，看看自己是否真正的理解这些概念的内涵。

### **扯远了。先来回答这两个问题吧！**

预处理器（Preprocessor）是计算机科学术语，指的是代码编译之前由编译器调用的独立程序，用于解决源语言在语法层面、模块管理、代码复用等方面的一系列问题。回到CSS预处理器，它们基于CSS扩展了一套DSL（领域特定语言），用来解决CSS语法难以解决的问题：无法嵌套抒写、变量管理、代码块复用、逻辑流程控制以及函数、继承等高级语言的语法特性。简言之：预处理器提供高级语言的语法思路写CSS，再直接调用编译文件（less需要引入编译库js文件）或者编译出.css文件调用。

弄清楚了CSS预处理器的作用，也就大致搞清楚了CSS原生语法存在的弊端了：CSS语法就是简单的由属性值和声明组成声明块，声明块挂载在选择器上构成规则集。如下：

```js
// 一个规则集
.rule{
    width: 100px;  //一句声明，多个声明用{}包裹构成声明块
    color: red;
}
```

是不是很简单？

是不是太简单？！

当然，CSS预处理器的出现也是前端项目日渐复杂化、模块化、多人协作化的产物，如果只是单人处理的小的项目，CSS预处理器的优势可能就不那么明显了。

### **接下来，先介绍常见的三类CSS预处理语言：LESS、Sass和Stylus。**

#### 1、Sass简介

官网：[http://sass-lang.com/](http://sass-lang.com/) （英）  [https://www.sasscss.com/ ](https://www.sasscss.com/（中）) （中）

Sass诞生于2007年，是历史最悠久的CSS扩展语言，也是最成熟、稳定、使用面最广的CSS扩展语言。

它能完全兼容不同版本的CSS语法，同时提供变量（Variables）、运算（Operations）、规则嵌套（Nexted rules）、混合（Mixins）、导入（import）、表达式（Expressions）等一系列高级语法。Sass文件最后会编译出CSS文件供页面调用。

#### 2、Less简介

官网：[http://lesscss.org/（英）](http://lesscss.org/（英）) [http://lesscss.cn/（中）](http://lesscss.cn/（中）)

Less开源于2009年，创建思想上受Sass很大影响，它扩展了变量（Variables）、规则嵌套（Nexted rules）、混合（Mixins）、函数（Function）等特性，可以运行于浏览器和Node环境。

### 3、Stylus简介

官网：[http://stylus-lang.com/](http://stylus-lang.com/)

Stylus被称为一种革命性的新语言，它提供一种富有表现力、动态、简装的语法来生成CSS。

它来自Node.js社区，产生于2010年，主要用来给Node项目提供CSS预处理支持。

在语法上，它同时支持缩进和常规样式写法编写规则。

想了解这三种语言的安装、语法API和书写规范的可以去官网了解。

### 接下来，将以Stylus为例，从扩展的功能角度，做介绍。

#### 文件模块管理

模块化的基础思想就是：拆分和引入。原生CSS其实也有模块化管理的思想，比如按照功能将样式组织成多个文件（rest.css、animate.css）然后分别导入；或者使用原生的@import指令嵌入css文件。但这些方案，要么增加了请求，要么容易造成冗余，总之不能满足性能要求。

CSS预处理器扩展了@import指令功能，同时推荐开发者按照项目cli目录的方式管理编译文件，既解决了代码冗余，也契合了高级的文件模块管理方案。示例：

```
// 按照功能组织模块目录，入口文件entry和子模块在编译时会引入所需的模块。
// 全部文件最终打包成一个entry.css文件供页面调用。

entry.styl
 ├─ base.styl
 │   ├─ normalize.styl
 │   └─ reset.styl
 ├─ layout.styl
 │   ├─ header.styl
 │   │   └─ nav.styl
 │   └─ footer.styl
 ├─ section-foo.styl
 ├─ section-bar.styl
 └─ ...
```

#### 规则嵌套

使用缩进（Stylus）和大括号（Less & Sass）来呈现规则的层级和包含关系，简介且清晰。

```
.nav
    margin: auto  // 水平居中
    width: 1000px
    color: #333
    li
        float: left  // 水平排列
        width: 100px
        a
            display: block
            text-decoration: none
```

#### 编译生成

```
.nav {margin: auto /* 水平居中 */; width: 1000px; color: #333;}
.nav li {float: left /* 水平排列 */; width: 100px;}
.nav li a {display: block; text-decoration: none;}
```

#### 变量

变量除了用于解决冗余，方便管理，也可用于表达式计算。

注意：

1、变量也有其块级作用域，在声明块内定义的变量只能在声明块及嵌套块内使用，在最外部定义的变量则可以在文件任意位置使用。

2、变量声明完就要赋值，否则会报错。

```
// 将颜色设置为变量
$color-primary = #ff4466

strong
    color: $color-primary  //使用变量
    font-weight: bold

.notice
    color: $color-primary  //使用变量
```

#### 运算和表达式

变量是运算的基础，也为样式规则关系提供了清晰的关联依据。

```
    $max-lines = 3
    $line-height = 1.5

    overflow-y: hidden
    line-height: $line-height
    max-height: unit($line-height * $max-lines, 'em')
```

以上代码针对行高设置了两个变量，设置最大高度时，通过表达式明晰了高度与行高之间的关系。最终编译的代码如下：

```
.wrapper {
    overflow-y: hidden;
    line-height: 1.5;
    max-height: 4.5em;  /* = 1.5 x 3 */
}
```

#### 织入Mixin

Mixin在各种语言和框架设计中都会出现，指的是带有全部实现或部分实现的模块，提供对外接口供调用，已达到更好的代码复用。

以常见的清除浮动为例：

```
// 定义一个Mixin
cl()
    z-index:1;
    &::after
        content:''
        clear:both
        display:table

// 在需要的元素上调用
.news
    cl()
.info
    cl()

// Dom中使用
<div class="news cl"></div>
<div class="info cl"></div>
```



