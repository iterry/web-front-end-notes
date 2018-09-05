浏览器渲染是将页面资源绘制显示到浏览器窗口的过程，它由浏览器的渲染引擎负责处理。目前常见的浏览器四大内核（Trident、Gecko、Webkit、Blink）在绘制原理上大致相同，但实现细节和具体处理上也有各自的处理方式（这篇笔记若涉及到具体的渲染模型，会以Webkit为例）。

**这篇笔记将会涉及到如下内容：**

* 浏览器渲染基本流程
* 各个流程节点的详解
* CSS、JS、DOM结构顺序对渲染的影响
* 建议及注意事项

### 一、浏览器渲染的基本流程

从输入网址到页面显示，整个请求、解析、加载、渲染流程包括以下几点：

* DNS域名解析
* TCP三次握手
* 建立TCP连接后发Http请求
* 服务器响应请求
* 浏览器渲染

前四点网络模块笔记中整理。

**浏览器渲染流程图示如下：**

![](/assets/render.png)

1. 浏览器从服务器获取到资源，包括：HTML、CSS、JS、图片资源等等。
2. 解析HTML构建DOM Tree，其构建过程是一个深度遍历过程，即：外层节点的所有子节点构建完成才去构建兄弟节点。
3. 解析CSS生成CSSOM（CSS Rule Tree）。
4. 将DOM Tree和CSSOM合并生成Render Tree。
5. 基于Render Tree计算Dom节点类型、属性、样式、层级关系，确定详细绘制信息。
6. 在屏幕绘制Render Tree，显示页面。
7. 若JS对DOM或者CSS进行了操作，重新对Render Tree计算Layout，确定是否需要重绘（repaint）或者重排（reflow）。

整个流程相对比较清晰，接下来我们再对流程中的各个概念最深入的剖析：

#### 一、HTML Parser

负责将HTML标识解析为解析树，解析树由DOM元素及其属性节点组成，它的根是“document”对象。

不同的浏览器厂商定制了不同的解析器，解析过程也存在差异，但大体上遵从HTML5描述的算法规范。

良好的DOM书写习惯或者符合HTML5语义的DOM结构利于浏览器解析。

```js
<html>
    <head></head>
    <body>
        <p>Hello DOM</p>
        <div>
            <img src=”example.png” />
        </div>
    </body>
</html>
```

这段HTML对应的DOM Tree如下：

![](/assets/render1.png)

#### 二、CSS解析

CSS是声明式语法块集合，CSS Parser使用Flex和Bison解析生成器从CSS文件自动生成解析器。

样式来源包括外链文件、行内样式元素以及标签上的可视化属性（bgcolor）。

为每个元素查找匹配的规则都需要遍历整个规则表，所以选择器声明不当或者冗余而不优化，容易产生性能问题，特别是文件数据比较多的情况。

![](/assets/render2.png)

如果样式文件是以外链Link到页面，这个文件的阻塞文档的解析直至资源被加载完毕；

如果样式文件是以JS插入进页面，可以为外链添加defer标示，是脚本的加载解析使用另外的线程，从而不阻塞文档的加载和解析。

#### 三、渲染树Render Tree

渲染树是页面按照正确顺序的基础，它由元素显示序列中的可见元素组成，是文档的可视化表示。

渲染元素在FF中称为frames，在Webkit中用renderer或渲染对象来描述。

渲染对象用一个包含该节点元素所有css属性的矩形区域来表示，而渲染对象的显示方式（display）决定了它的创建类型：

* 块级元素按照块级元素创建
* 行内块元素按照块级元素来创建
* 行内元素按照行内元素来创建
* ......
* 隐藏元素\(display:none\)不创建

所以，Render Tree和DOM Tree并不是一一对应的关系：display:none 、document根元素、Head元素等不可见元素不会在Render Tree中生成。

处理html和body标签将构建渲染树的根，这个根渲染对象对应被css规范称为containing block的元素——包含了其他所有块元素的顶级块元素。它的大小就是viewport——浏览器窗口的显示区域，Firefox称它为viewPortFrame，webkit称为RenderView，这个就是文档所指向的渲染对象，树中其他的部分都将作为一个插入的Dom节点被创建。

![](/assets/render3.png)
