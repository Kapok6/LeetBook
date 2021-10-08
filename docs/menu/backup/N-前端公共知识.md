

# 1. 公共知识

## 工程化

https://lluvio.github.io/blog/front-end-architecture.html

### webpack

1. 入门指南
- 知乎：https://www.zhihu.com/question/39290543
- csdn：https://blog.csdn.net/qq_34935885/article/details/72862314
- 官方：https://webpack.js.org/guides/
2. 

## 浏览器

### Cookie、session和web Storage

cookie、sessionStorage和localStorage异同：

1. 同：存储在浏览器，同源的。
2. 异：作用、大小限制、作用域、时效。

#### Cookie

1. **作用**：与服务器交互，http规范的一部分。
2. **存储内容**：名字、值、过期时间、路径和域（路径与域一起构成cookie的作用范围），为字符串。
3. 存储位置：
   1. 若不设置“过期时间”：表示仅当前会话有效，关闭浏览器失效，被称为“会话cookie"，存储在客户端浏览器上。
   2. 若设置了失效时间：则存储在硬盘上，关闭浏览器仍然有效直到超过过期时间。
4. 作用域：在所有同源窗口中都是共享。
5. 大小限制：单个cookie一般40k以内，很多浏览器限制20个以内。
6. 劣势：（1）可以被捕获和分析，不安全。（2）大小受限。（3）始终在同源的http请求中携带，每次请求一个新的页面的时候cookie都会被发送过去，这样无形中浪费了带宽。（4）另外cookie还需要指定作用域，不可跨域调用。 

 #### Session

1. 存储内容：为对象。
2. 存储位置：服务器，然后生成一个唯一的session id，跟客户端之间交互。
3. 时效：过期时间，是从session不活动的时候开始计时。默认30分钟过期，可设置永不过期，。
4. 优势：安全，不容易被篡改。
5. 劣势：会在一定时间内保存在服务器上，当访问增多，比较占服务器的性能。

#### SessionStorage/LocalStorage

1. 作用：存储数据。
2. 存储位置：浏览器。
3. 作用域：sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localstorage在所有同源窗口中都是共享的；
4. 大小限制：5M甚至更大。
5. SessionStorage时效：仅当前窗口未关闭时，一直有效。若关闭/重新打开，则sessionStorage会被销毁。
6. LocalStorage时效：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；
7. SessionStorage优势：（1）减少网络流量：避免再向服务器请求数据，也减少客户端和浏览器间的数据传递(cookie)。（2）可快速显示数据。（3）支持事件通知机制，数据更新可通知监听者。