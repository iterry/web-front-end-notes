Flexbox是CSS3新引进的布局属性，同传统的布局方式（浮动、定位）相比更加灵活、方便，且能更好的适应移动端的响应式布局。

Flexbox（Flexible box）直译：灵活的盒子，说它灵活，除了能实现常规的布局，还能提供：

* 在横轴/竖轴排列子项
* 任意改变子项的排列顺序
* 任意更改子项的对齐方式
* 快速调整子项的间距和占据空间比例
* 动态增删子项到容器

简言之，虽然是一种完全不同的布局模块，但能及其快速的实现：float、position、inline-block、word-wrap、text-align、vertical-align等属性结合才能实现或很难实现的各项布局。

#### 先厘清几个基本概念：

* Flex容器——包含子元素的父容器
* Flex项目——父容器的直接子元素，也是布局要作用的成员对象
* 主轴\(main axis\)——水平的轴向，默认轴向，默认顺序从左\(main start\)到右\(main end\)，会影响布局的方向
* 交叉轴\(cross axis\)——垂直的轴向，默认顺序从上\(cross start\)到下\(cross end\)，会影响布局的方向
* 主轴尺寸\(main size\)——单个Flex项目占据的主轴空间
* 交叉轴尺寸\(cross size\)——单个Flex项目占据的交叉轴空间

作用在Flex容器上的属性：

```js
display:flex    设置容器为flexbox容器
flex-direction　　容器内项目的排列方向(默认横向排列)　　
flex-wrap　　容器内项目换行方式
flex-flow　　以上两个属性的简写方式
justify-content　　项目在主轴上的对齐方式
align-items　　项目在交叉轴上如何对齐
align-content　　多行/多列排列的flex项目在交叉项上的对齐方式
```

作用在Flex项目上的属性：

```js
order　　项目的排列顺序。数值越小，排列越靠前，默认为0，可以为负值。
flex-grow　　项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
flex-shrink　　项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
flex-basis　　在分配多余空间之前，项目占据的主轴空间（main size）。它的默认值为auto，即项目的本来大小。
flex　　是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
align-self　　设置单个项目的对齐方式，继承父元素的align-items，可覆盖align-items属性，如果没有父元素，则等同于stretch。
```



