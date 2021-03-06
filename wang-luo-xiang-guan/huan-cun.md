缓存（Cache），指的是可以快速访问的副本资源。在学习缓存之前，先思考几个问题：

* 缓存有哪些种类，能解决什么问题？
* 在Web请求的哪些流程或节点需要用到缓存？
* 什么资源适合缓存？
* 如何更新缓存，保持同源资源一致？

### 缓存的种类和作用

Web访问的重要课题之一就是响应和访问速度，而缓存机制就是提升响应速度的重要策略。良好的缓存策略不仅能减少海量请求带来的网络带宽消耗，缓解服务器压力，更重要的是能加速客户端的资源获取速度，直接提升页面打开速度。

从请求的全部流程看，缓存分为：浏览器本地缓存、HTTP缓存、CDN缓存、数据库缓存等等。

CDN缓存和数据库缓存在对应知识的笔记会单独讲解到，这边笔记主要学习的是HTTP缓存策略。

### 什么资源适合缓存

理论上，所有的静态资源都应该缓存，只不过依据资源的大小、修改频率需要不同的缓存策略。

### HTTP缓存机制

报文指的是客户端和服务器间通信时发送和响应的数据块，依据发送源分为请求报文（request）和响应报文（response）。报文分为首部（header）和主体部分（body）：首部携带缓存相关规则信息、主体包含请求传输的内容。

假定三个主体：客户端（发起请求）——&gt; 缓存数据库（缓存资源）——&gt;服务器（源资源库）。

先看看涉及到的报头规则：

| **类型** | **规则名** | **取值** | **描述** |
| :--- | :--- | :--- | :--- |
| 响应 | **Expires** | 标准日期：Wed, 04 Sep 2019 07:05:59 GMT | 绝对日期、告诉浏览器在<br/>过期时间前可以使用缓存 |
| 响应 | **Cache-Control** | no-cache | 告诉浏览器无论有无<br/>缓存都请求源服务器 |
|  |  | no-store | 强制不创建副本，<br/>从源服务器拉资源 |
|  |  | max-age=xxx（秒） | 设置缓存的新鲜时长，<br/>首次缓存时间+max-age=过期时间 |
|  |  | public | 响应可以被任何形式缓存，<br/>比如浏览器缓存、CDN缓存、代理缓存 |
|  |  | private（默认） | 响应只能被单个用户的浏览器缓存 |
|  |  | must-revalidate | 告诉浏览器、缓存服务器，本地副本<br/>过期前，可以使用本地副本；<br/>本地副本一旦过期，必须去<br/>源服务器进行有效性校验。 |
| 响应 | **Last-Modified** | Tue, 04 Sep 2018 01:31:49 GMT| 资源最后的修改时间 |
| 请求 | **If-Modified-Since** | Tue, 04 Sep 2018 01:31:49 GMT | 如果第一次请求时响应中<br/>Last-Modified非空，之后的请求<br/>同一资源，会在它作为该项的值<br/>传给服务器做验证 |
| 响应 | **Etag** | 1d3c4e5e2788464:0 | 服务器生成的资源的唯一标识 |
| 请求 | **If-None-Match** | 1d3c4e5e2788464:0 | 如果第一次请求Etag非空，<br/>之后的请求会把它作为该项的值<br/>传给服务器做验证 |
| 响应 | **Vary** | Accept-Encoding | 辅助从多个缓存副本中筛选合适的版本 |

**当报头同时设置了以上多个规则，判断优先级如下：**

Cache-Control &gt; Expires &gt; E-tag / If-None-Match &gt; Last-Modified / If-Modified-Since

#### **有两个名词了解下：**

新鲜度：指的是缓存资源的过期状态。缓存资源有通过expires或者Cache-Control：max-age设置了缓存的过期时间，在这个时间之前，缓存是“新鲜的”，客户端请求可以直接从缓存数据库拉取资源，不需再请求源服务器；在这个时间之后，缓存资源变得“陈旧”，客户端发出请求，缓存检索到资源已过期，缓存会先将此请求附加 If-Modified-Since 规则头，发给源服务器，目的是检查过期的缓存资源是否依然“新鲜”可用。源服务器验证后，若返回304（不返回实体信息），表明缓存依然“新鲜”；若返回200（携带新的信息），表明缓存已过期，返回和更新新的资源。

校验值：资源在服务器的唯一标识符，用作过期资源在源服务器身份的匹配校验。

**我们来看看最简单的请求-响应流程是怎样的：**

首次请求：

请求服务器，回传资源和响应规则并存储至缓存数据库，见下图：

![](/assets/cache4.png)

二次请求：

如果缓存依然“新鲜”，直接从缓存拿资源；

如果缓存“不新鲜”，请求源服务器做匹配，决定是否使用缓存资源。

见下图：![](/assets/cache7.png)从请求流程上看，第一次请求：

![](/assets/cache2.png)

重复请求：

![](/assets/cache3.png)

以上内容和图示参考自：[彻底弄懂HTTP缓存机制及原理](https://www.cnblogs.com/chenqf/p/6386163.html)

### 缓存失效和加速资源

Http缓存策略很好的解决了请求效率和带宽等问题，从理论上来说，资源过期时间设置的尽量长即可，但在实际业务中，不同的资源的更新频率不一样。对于频繁更新或者固定更新频率的资源来说好处理；对于长时间不更新或者不确定时间更新的资源就比较麻烦，特别是一些css/js资源，需要更新变动并且影响全局。

一种方案是revving加速技术。它的原理：文件使用特定的命名格式——URL+版本号，添加了版本号的资源被视为独立的资源。这种方案对于不频繁更新的非大量重复使用资源管理很有用。其中版本号可以是数项增序、时间戳、md5截位等。

另一种方案需要借助自动化构建工具。它的原理：按照一定格式管理文件命名，通过自动构建自动管理生产的文件名和引用的文件名。如下：

```
// 原资源名称
static/app.a.js

// 打包后
static/app.3jjd923ccd.a.js
```

### 用户行为的缓存的影响

用户行为指的是浏览器回退、前进、常规刷新、F5刷新、Ctrl+F5刷新、特定Tab关闭又打开。

| **行为** | **Expires / Cache-Control** | **E-tag / Last-Modified** |
| :--- | :--- | :--- |
| Enter/常规刷新 | 有效 | 有效 |
| history回退 | 有效 | 有效 |
| history前进 | 有效 | 有效 |
| 特定Tab关闭再打开 | 有效 | 有效 |
| F5刷新 | 无效 | 有效 |
| Ctrl+F5刷新 | 无效 | 无效 |







