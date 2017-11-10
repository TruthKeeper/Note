## HTTP

Hyper Text Transfer Protocol，超文本传输协议，基于**TCP/IP**的一个**应用层**协议，无状态协议（每一个请求都是独立的）

### 版本变迁

- 1.0：客户端每次与web服务器建立连接一次只能获取一个资源，每次都需要三次握手

- 1.1：允许客户端和web服务器建立连接后获取多个资源(长连接)，现在绝大多数都是用的此协议（默认Connection:Keep-Alive）

- 2.0：压缩了HTTP协议的头信息，减少数据包的大小，多路复用机制，提高响应速度，允许通过单一的连接来发起多重的请求，并且从1.x的基于文本格式的解析变成了二进制格式，提高了健壮性

### 访问过程

- 查询浏览器是否有IP缓存
- 无缓存则通过域名（也称主机名）从本机的的hosts文件中寻找（hosts文件被恶意修改就是DNS劫持）
- 上述本机操作没有寻找IP则向DNS域名解析服务器发起请求
- 获取到IP地址后，发起3次握手，建立TCP连接
- 生成HTTP请求报文（请求行、请求头、请求体），并发送
- 服务端返回给客户端HTTP响应包
- 浏览器解析HTML代码，并使用多线程下载其中的CSS、JS等素材
- 浏览器对页面进行渲染，呈现给用户

### 消息报文

#### 请求行

请求方式 / 地址 / 版本号 / 协议

GET www.baidu.com HTTP 1.1

#### 请求头

键值对的形式，常见的有Host，Cache-Control，Connection，Range，User-Agent等等

#### 请求体

POST中内容键值对用&拼接，还有文件上传数据等

### 状态码

- 100+：用于指定客户端信息的状态码（用途不明）
- 200+：成功状态码
- 300+：重定向状态码
- 400+：客户端错误状态码
- 500+：服务端错误状态码

### URL、URI

- URL：统一资源定位符，资源的具体位置

- URI:统一资源标识符，资源的唯一标识

### POST、GET

- GET默认情况下可以被浏览器缓存，而POST不可以
- GET的参数直接暴露在URL中，而POST会保存在RequestBody中
- GET设计层面上（语义）是获取数据，而POST是上传、修改数据
- 浏览器和服务器会对访问的URL长度做限制，所以相对而言，POST可以携带更多的数据
- GET只有URL编码，而POST有更多的编码格式
- GET是幂等（多次执行和一次执行的影响相同）的，而POST是不幂等的，参照获取个人资料和点赞功能的区别

### Session机制

本质上Session机制不属于HTTP协议中的一部分，用来弥补HTTP无状态的缺陷，Session机制是一种在客户端和服务端之间保持状态的一种解决方案，例如浏览网站时，服务端会校验客户端的请求报文中是否包含Session ID，如果有，则检索出服务端的Session用来使用，不然则创建一个Session，并且生成Session ID作为应答发送给客户端

### Cookie

基于Session机制的会话跟踪技术，持久化客户端访问的数据，现在绝大多数浏览器使用该规则，大小有限制，不适合复杂的数据存储

服务端分配给客户端的响应报文头中带有Set-Cookie（会有多个），里面包含了键值对，其中：
- Domain：域，表示当前Cookie所属的分类
- Path：Cookie所属的路径

客户端接收到Cookie后在每次请求进行判断，如果所属Domain、Path对应，并且未过期

- Cookie是保存在客户端的，Session是保存在服务端的
- Cookie是不安全的，可以被修改，而Session是相对安全的

### 缓存

通常指浏览器、移动端缓存


![](https://raw.githubusercontent.com/TruthKeeper/Note/master/Http/HTTP%E7%BC%93%E5%AD%98.png)


**Cache-Control**:Request，Response中的头headers：

> 请求Request

- no-cache  客户端不读取缓存，向web服务器重新请求
- no-store  请求和响应都不读取缓存
- max-age  max-age=%d  表示请求后%d秒内再次发起请求，不会再去请求服务器
- max-stale  max-stale=%d  表示如果请求超时了，客户端也可以接收到或者小于%d秒内的响应结果

> 响应Response

- public：表示该响应可以被任何缓存区缓存，通常JS，CSS等一些静态文件都是public的
- private：表示该响应的缓存是私有的，仅针对某个用户，不能共享
- no-cache  （建议）不做缓存
- no-store  （建议）请求和响应都不做缓存
- max-age=%d  （建议）本次响应的缓存时长
- max-stale=%d  允许读取时间小于%d秒的缓存








