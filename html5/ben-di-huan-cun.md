缓存是Web开发中的重要话题，也是提升用户体验的关键环节，从页面全局的生命周期看，缓存可分为浏览器缓存、DNS缓存、数据库缓存、CDN缓存等等。这篇笔记记录的是HTML4/5提供的本地存储功能，包括：

* Cookie
* LocalStorage 
* SessionStorage
* Application Cache

**其中，前三者是最常用的前端数据本地缓存方案，先来做个比较：**

| **方案** | **版本** | **大小限制** | **主动禁用** | **持久性** |
| :--- | :--- | :--- | :--- | :--- |
| **Cookie** | HTML4 | 4K | 能 | 任意过期时间 |
| **LocalStorage** | HTML5 | 5M+ | 不能 | 永久；手动删除 |
| **SessionStorage** | HTML5 | 5M+ | 不能 | 关闭窗口 |

**Cookie：**

* Cookie不是HTML5的新标准，在HTML4就出现了。
* Cookie作为Http规范的一部分存在，可以通过Http响应头与服务器通信。
* Cookie不可跨域调用。
* 当Cookie记录被设置为Httponly（目的是防止信息被簒写和防止XSS攻击），该条Cookie记录无法被客户端读写。
* Cookie可以设置任意长的过期时间，若设置为负值，缓存过期；若不设置，Cookie的生命周期为浏览器会话期间（类似SessionStorage）。

**WebStorage：**

* WebStorage是HTML5新纳入的本地存储方案，包括LocalStorage\(本地缓存\)和SessionStorage\(会话缓存\)。
* L和S有诸多的共同点：存储大小5M左右（chrome为2.5M）；提供记录的操作方法（setItem、getItem、removeItem、clear）。
* L和S也存在不同点：WebStorage记录可以在同一个浏览器的不同Tab窗体间通信，且可以永久存储至手动清除记录；SessionStorage记录只能在一个Tab窗体内使用，且记录在这个窗体关闭时就清除。

值得注意的是：大部分浏览器能对Cookie禁用，但不能对WebStorage禁用；但在移动浏览器设置了“无痕浏览”条件下，这三类缓存都不生效。

---

#### **Application Cache：**

* 全称：应用程序缓存，是HTML5引入的Web应用离线存储技术，**但被Web标准弃用**，存在一定兼容性问题。
* 通过创建cache manifest文件，设置缓存资源等信息，实现离线浏览，并能加快页面载入速度，缓解服务器压力。

**作用机制：**

* 浏览器发出请求，查看页面有无缓存。
* 若没有，从服务器下载并缓存。
* 若有，浏览器将缓存文件加载，然后请求manifest文件，检查是否有更新：若没更新怎不重新拉取；若有更新，重新下载缓存文件并更新本地缓存。
* 下次在请求或者刷新页面，显示的就是最新资源。

**使用方法：**

首先，将页面设置appcache属性。

```
// manifest属性引用缓存清单文件的相对或绝对路径，这个文件设置被用来缓存的文件
// 每个需要换成的页面都需要设置manifest，否则该页面不会被缓存
<html manifest="example.appcache">
  ...
</html>
```

然后，在服务器添加对应的缓存清单文件并设置缓存信息。

缓存清单文件是一个简单的文本文件，列出了供缓存的资源，资源以URI标识。

注意：缓存清单文件的MIME类型必须设置为 text/cache-manifest。

```
CACHE MANIFEST
# v1 2011-08-14

CACHE:    
index.html
cache.html
style.css
image1.png

NETWORK:
network.html

FALLBACK:
*.html /fallback.html
```

如上所示，一个标准的cache文件包括三部分：

* CACHE——默认部分，缓存的文件列表，第一次下载后显式缓存。
* NETWORK——白名单资源，即列出的资源都是绕过缓存直接向服务器拉取。可用通配符标示。
* FALLBACK——可选，用作设置无法访问资源的后备网页地址。第一个URI代表待访问资源，第二个代表后备网页或替代资源，两者必须相关和同源。可用通配符标示。

每个缓存都有一个status属性，标示缓存的当前状态。使用最多的是 updateready，标示缓存已重新下载更新。

```
// 缓存更新后重新加载页面
applicationCache.addEventListener("updateready", function(){
    location.reload();
})
```

总之，Application Cache可作为少量静态资源（5M以内）的离线缓存解决方案，而且标准已废弃，不推荐使用了。

