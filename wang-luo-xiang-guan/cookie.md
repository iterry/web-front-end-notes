在HTML4.1到HTML5很长一段时间，客户端Cookie一度用于本地数据的存储乃至唯一的存储手段。HTML5的Web Storage和离线存储技术流行开来后，Cookie本地存储功能渐渐式微。但同Web Storage存储机制不一样的地方是：Cookie每次请求都会携带数据发送服务器验证，而Web Storage则只是在本地管理增删查改。从这个层面上，Cookie易带来额外的性能开销。

#### Cookie的经典用途：

1. 注册、登陆状态管理。
2. 浏览器行为跟踪。
3. 个人信息和个性化设置信息（积分、浏览记录、购物车、喜好设置等）。

#### Cookie作用流程：

1. 客户端向服务器发起http请求，服务器会先检查响应头是否带有记录了当前用户的Cookie信息。
2. 若没有，表示用户没注册或者首次访问，会在服务器创建用户标示记录，并在响应头添加set-cookie选项。
3. 浏览器收到响应会保存Cookie记录。
4. 之后，该浏览器或已注册用户向同一服务器发送请求，请求头都会带上Cookie信息。

图示如下：

![](/assets/cookie.png)

特别注意的是：Cookie按照生存周期，可以分为会话期Cookie和持久性Cookie，前者在浏览器关闭后就自动删除，仅在会话期有效（类似session），后者需要设置指定的过期时间（Expires）或有效期（Max-Age）。

##### 服务器设置Cookie代码示例如下：

```js
// 初次设置Cookie
HTTP/1.0 200 OK
Content-type: text/html
Set-cookie: user_name= terry

// 带Cookie记录的请求
HTTP/1.0 200 OK
Get /demo.html
Host: www.test.com
cookie: user_name = terry

// 设置过期时间,设置的时间只和客户端有关，与服务器无关
Set-Cookie: user_name = terry; Expires = xxxxxxxxxxxx（格林时间）
Set-Cookie：user-name = terry; Max-Age = xxxxxxxx（秒）

// 敏感信息设置Secure，只能适用于https请求
Set-Cookie: user_name = terry; Expires = xxxxxxxxxxxx; Secure

// 设置HttpOnly，禁止客户端访问，避免XSS攻击
Set-Cookie: user_name = terry; Expires = xxxxxxxxxxxx; HttpOnly
```



