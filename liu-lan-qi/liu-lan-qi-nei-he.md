内核是浏览器底层架构最核心和基础的部分，按照功能可分为：渲染引擎和JS引擎。

渲染引擎（Rendering Engine）也称为布局引擎（Layout Engine）、排版引擎，负责对网页语法的解释和渲染显示到浏览器。一个渲染引擎通常包括HTML解释器、CSS解释器、布局layout、网络等模块：

![](/assets/8888.gif)

**通常讲的浏览器内核就是指代的浏览器的渲染引擎。**

不同的浏览器使用不同的渲染内核，对HTML/JS/CSS的标准语法的解释也存在差异，导致在显示效果、语法支持度和渲染效率上也存在差别，所以也就导致了网页程序在不同内核的浏览器下的表现和渲染差异，乃至bug都不统一。

### 简单列举下几大厂商的引擎：

| **浏览器** | **渲染内核** | **JS引擎** |
| :--- | :--- | :--- |
| IE/Edge | Trident\(&lt;=IE10\)；EdgeHTML | JScript\(&lt;IE9\)；Chakra\(IE9+及Edge\) |
| Safari | Webkit/Webkit2 | JSCore/Nitro\(4+\) |
| Chrome | Chromium\(Webkit\)；Blink | V8 |
| Firefox | Gecko | SpiderMonkey\(&lt;3.0\)；TraceMonkey\(&lt;3/6\)；JaegerMonkey\(4.0+\) |
| Opera | Presto；Blink | Futhark\(9.5-10.2\)；Carakan\(10.5+\) |

### 一、几大厂商浏览器内核简介

**IE/Edge： **微软的IE浏览器浏览器更新至IE10后，伴随着WIN10系统的上市，迁移到了全新的浏览器Edge。除了JS引擎沿用之前IE9就开始使用的查克拉\(Chakra\)，渲染引擎使用了新的内核EdgeHTML（本质上不是对Trident的完全推翻重建，而是在Trident基础上删除了过时的旧技术支持的代码，扩展和优化了对新的技术的支持，所以被看做是全新的内核）。

**Safari：**Safari自2003年面世，就一直是苹果公司的产品自带的浏览器，它使用的是苹果研发和开源的Webkit引擎。Webkit引擎包含WebCore排版引擎及JavaScriptCore解析引擎，均是从KDE的KHTML及KJS引擎衍生而来。Webkit2发布于2010年，它实现了元件的抽象画，提高了元件的重复利用效率，提供了更加干净的网页渲染和更高效的渲染效率。另外，Webkit也是苹果Mac OS X系统引擎框架版本的名称，主要用于Safari、Dashboard、Mail。

**Chrome：**提到Chrome浏览器，一般人会认为使用的Webkit内核，这种说法不完全准确。Chrome发布于2008年，使用的渲染内核是Chromium，它是fork自Webkit，但把Webkit梳理得更有条理可读性更高，效率提升明显。2013年，由于Webkit2和Chromium在沙箱设计上的冲突，谷歌联手Opera自研和发布了Blink引擎，逐步脱离了Webkit的影响。所以，可以这么认为：Chromium扩展自Webkit止于Webkit2，其后Chrome切换到了Blink引擎。另外，Chrome的JS引擎使用的V8引擎，应该算是最著名和优秀的开源JS引擎，大名鼎鼎的Node.js就是选用V8作为底层架构。

**Firefox：**火狐的内核Gecko也是开源引擎，任何程序员都能为其提供扩展和建议。火狐的JS引擎历经SpiderMonkey、TraceMonkey到现在的JaegerMonkey。其中JaegerMonkey部分技术借鉴了V8、JSCore和Webkit，算是集思广益。

**Opera：**Opera在2013年V12.16之前使用的是Opera Software公司开发的Presto引擎，之后连同谷歌研发和选择Blink作为Opera浏览器的排版内核。

### 二、国内的浏览器内核现状

从全球范围内看，Chrome浏览器的市场份额遥遥领先，IE/Edge由于微软系统的市场占有率优势，也有相当大的使用比例，这两者合计占据了市场近80%比例。

国内市场乱象横生，但Chrome和IE/Edge也占据了超过50%的市场占有率，另外，国内浏览器厂商（QQ、2345、搜狗、猎豹、UC、360）也有一定的市场占有率。总结国内厂商内核来看，一般为三类：一、使用的Trident单核，如：2345、世界之窗；二、使用Trident+Webkit/Blink双核，如：UC、猎豹、360、百度；三、使用Webkit/Blink单核，如：搜狗、遨游。

总之，国内并没有优秀的自研内核，通常都是使用的Webkit/Blink 或者 Trident 内核，除此之外的差异通常体现在体验扩展和各自的商业诉求上。

### 三、移动浏览器内核

通常指的是移动设备系统自带的浏览器内核。IOS设备内置Safari使用的Webkit，同苹果PC一致；Andriod4.4之前版本系统浏览器内核是Webkit，之后切换到Chromium/Blink；WinPhone8及以上使用的是Trident。

值得注意的是，由于系统环境条件及功能差异，不同浏览器内核在PC和移动端都有特定的优化或扩展定制，所以在具体的使用中就存在差异。

