# websocket

websocket是HTML5提供的一种新的网络协议，以实现客户端与服务端的双向通信。在此之前，HTTP协议只能与服务端进行单向通信，在遇到客户端需要持续接收服务端的信息的场景下，只能使用轮询的方式进行响应，这种方式相比之下会有较大的小号。其**本质是一个协议**，主要功能是**实现双向通信**，建立在 **TCP 协议**之上。

## 使用

### 一个对象

```python

//要创建WebSocket，先实例花一个websocket对象，并传入要链接的URL

new socket ＝ WebSocket("ws://www.example.com/server.php")；

//注：
//1.ws是协议名, 必写；
//2.构造函数需要传入绝对的URL，不适用同源策略；

```

### 两个方法

```python
//关闭连接
socket.close();

//发送和接收数据
socket.send("hello world");

var message = {
    type:"test",
    text:"Hello Word!"
};

socket.send(JSON.stringify(message));

//注：
//1.Web Socket 只能通过连接发送纯文本数据，复杂的数据结构需要在发送之前进行序列化，服务端接收到的数据也要解析之后使用；
//2.服务端返回数据时，message事件会接收到一个事件，返回的数据会保存在event.data属性中；
```

### 四个事件

```python
//事件
//1.open:在成功建立连接时触发；

socket.onopen = function(){
    console.log("connection successed");
}

//2.message:服务器发来消息时触发；

socket.onmessage = function(event){
    console.log("got message",event.data);
}

//3.error:在发生错误时触发，连接不能持续；

socket.onerror = function(){
    console.log("connection error");
}

//4.close:在连接关闭时触发；

socket.onclose = function(){
    console.log("connection closed");
}

```

## 与HTTP协议对比

|  对比项   |            http            |          ws          |
|:---------:|:--------------------------:|:--------------------:|
| 未加密URL |          http://           |        ws://         |
|  加密URL  |          https://          |        wss://        |
| 字节开销  | 较大，多次请求不适合移动端 | 较小，较为适合移动端 |
| 跨域问题  |            存在            |     允许跨域访问     |
|  安全性   |        相比之下安全        |     存在安全隐患     |

## 浏览器支持

| 浏览器  |    版本     |
|:-------:|:-----------:|
| Firefox |     V6+     |
| Safari  | V5+ / IOS4+ |
| Chrome  |     ＊      |
