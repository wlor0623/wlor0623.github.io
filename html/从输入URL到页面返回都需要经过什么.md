## 过程

- DNS 域名解析
- TCP 三次握手
- 发送 HTTP 请求
- 服务器处理请求并返回 HTTP 响应
- 浏览器解析渲染页面
- 断开连接

## DNS 域名解析

### 浏览器如何通过域名查询 URL 对应的 IP 地址

- 浏览器缓存：浏览器搜索自己的 DNS 缓存
- 操作系统缓存：搜索操作系统中的 DNS 缓存（内存中）
- hosts：搜索操作系统的 hosts 文件
- DNS 服务器：操作系统将域名发送至 DNS 服务器

  - LDNS：默认情况下，操作系统会将域名发送至 LDMS（本地域名服务器），LDNS 会查询自己的 DNS 缓存，查找成功则返回结果，失败则发起一个迭代 DNS 解析请求
  - LDNS 向 Root Name Server （根域名服务器，其虽然没有每个域名的的具体信息，但存储了负责每个域，如 com、net、org 等的解析的顶级域名服务器的地址）发起请求，此处，Root Name Server 返回 com 域的顶级域名服务器的地址
  - LDNS 向 com 域的顶级域名服务器发起请求，返回 google.com 域名服务器地址
  - LDNS 向 google.com 域名服务器发起请求，得到 www.google.com 的 IP 地址

- LDNS 将得到的 IP 地址返回给操作系统，同时自己也将 IP 地址缓存起来
- 操作系统将 IP 地址返回给浏览器，同时自己也将 IP 地址缓存起来
- 浏览器已经得到了域名对应的 IP 地址

## TCP 三次握手

### 第一次握手

建立连接时，客户端选择一个序号发送 SYN 包（SEQ=x）到服务器，并进入 SYN_SEND 状态，等待服务器确认

### 第二次握手

服务器收到 SYN 包，回应一个 ACK 段作为对 x（ACK=x）的回应，同时宣告它自己的序号为 y（SEQ=y），也就是 SYN+ACK 包，即 ACK（SEQ=y,ACK=x），这个时候服务器进入 SYN_RECV 状态

### 第三次握手

客户端收到服务器 ACK 包，并在它发送的第一个数据段中（SEQ=x），对服务器选择的序号（ACK=y）进行确认，即 DATA（SEQ=x,ACK=y）。之后客户端和服务器进入 ESTABLISHED（TCP 连接成功状态），完成三次握手。

**SEQ:** Sequence number 序号

**ACK:** Acknowledgement number 确认号

**SYN:** Synchronize Sequence Numbers 同步序列编号

## 发送 HTTP 请求

### 客户端发送一个 HTTP 请求到服务器的请求消息包括以下格式

- 请求行：请求方法 URL 协议/版本 回车符和换行符（例：POST /admin/auth/login HTTP/1.1）

- 请求头部：头部字段名:值 回车符和换行符（例：Accept-Encoding:gzip,deflate）

- 空行： 回车符和换行符

- 请求数据: （例：{“account”:”admin”,”password”:”admin”}）

<pre>
POST /admin/auth/login HTTP/1.1
Host: www.test.com
Connection: keep-alive
Content-Length: 38
Accept: application/json, text/plain, */*
DNT: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36
Content-Type: application/json;charset=UTF-8
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8
"account":"admin","password":"admin"}
</pre>

## 服务器处理请求并返回 HTTP 响应

### 正向代理
![image.png](https://i.loli.net/2020/03/24/UG9vFO23sfhWdca.png)

如上图，因为 google 被墙，我们需要 vpn 翻墙才能访问 google.com。

> vpn 对于“我们”来说，是可以感知到的（我们连接 vpn）
> vpn 对于”google 服务器”来说，是不可感知的(google 只知道有 http 请求过来)。

**对于人来说可以感知到，但服务器感知不到的服务器，我们叫他正向代理服务器。**

### 反向代理
![image.png](https://i.loli.net/2020/03/24/xi8ONMXjVnQyG21.png)

如上图，我们访问 baidu.com 的时候，baidu 有一个代理服务器，通过这个代理服务器，可以做负载均衡，路由到不同的 server。

> 此代理服务器,对于“我们”来说是不可感知的(我们只能感知到访问的是百度的服务器，不知道中间还有代理服务器来做负载均衡)。

此代理服务器，对于”server1 server2 server3″是可感知的(代理服务器负载均衡路由到不同的 server)

**对于人来说不可感知，但对于服务器来说是可以感知的，我们叫他反向代理服务器**

### Nginx

#### Nginx 是什么

> Nginx (“engine x”) 是一个高性能的 HTTP 和反向代理服务器，也是一个 IMAP/POP3/SMTP 服务器。

#### Php-fpm 是什么

##### cgi、fast-cgi 协议

cgi 的历史

早期的 webserver 只处理 html 等静态文件，但是随着技术的发展，出现了像 php 等动态语言。

webserver 处理不了了，怎么办呢？那就交给 php 解释器来处理吧！

交给 php 解释器处理很好，但是，php 解释器如何与 webserver 进行通信呢？

> 为了解决不同的语言解释器(如 php、python 解释器)与 webserver 的通信，于是出现了 cgi 协议。只要你按照 cgi 协议去编写程序，就能实现语言解释器与 webwerver 的通信。如 php-cgi 程序。

###### fast-cgi 的改进

有了 cgi 协议，解决了 php 解释器与 webserver 通信的问题，webserver 终于可以处理动态语言了。

但是，webserver 每收到一个请求，都会去 fork 一个 cgi 进程，请求结束再 kill 掉这个进程。这样有 10000 个请求，就需要 fork、kill php-cgi 进程 10000 次。

有没有发现很浪费资源？

> 于是，出现了 cgi 的改良版本，fast-cgi。fast-cgi 每次处理完请求后，不会 kill 掉这个进程，而是保留这个进程，使这个进程可以一次处理多个请求。这样每次就不用重新 fork 一个进程了，大大提高了效率。

##### 2、php-fpm 是什么

php-fpm 即 php-Fastcgi Process Manager.

hp-fpm 是 FastCGI 的实现，并提供了进程管理的功能。

进程包含 master 进程和 worker 进程两种进程。

master 进程只有一个，负责监听端口，接收来自 Web Server 的请求，而 worker 进程则一般有多个(具体数量根据实际需要配置)，每个进程内部都嵌入了一个 PHP 解释器，是 PHP 代码真正执行的地方。

#### Nginx 如何与 Php-fpm 结合

当我们访问 www.example.com 的时候，处理流程是这样的：

www.example.com

-&gt; Nginx

-&gt; 路由到 www.example.com/index.php

-&gt; 加载 nginx 的 fast-cgi 模块

-&gt; fast-cgi 监听 127.0.0.1:9000 地址

-&gt; www.example.com/index.php 请求到达 127.0.0.1:9000

-&gt; php-fpm 监听 127.0.0.1:9000

-&gt; php-fpm 接收到请求，启用 worker 进程处理请求

-&gt; php-fpm 处理完请求，返回给 nginx

-&gt; nginx 将结果通过 http 返回给浏览器

#### 服务器返回一个 HTTP 响应  

经过前面的几个步骤，服务器收到了我们的请求，也处理我们的请求，到这一步，它会把它的处理结果返回，也就是返回一个 HTPP 响应。 HTTP 响应与 HTTP 请求相似，HTTP 响应也由 3 个部分构成，分别是：

- 状态行
- 响应头(Response Header)
- 空行
- 响应正文

### 浏览器解析渲染页面

浏览器是一个边解析边渲染的过程。首先浏览器解析 HTML 文件构建 DOM 树，然后解析 CSS 文件构建渲染树，等到渲染树构建完成后，浏览器开始布局渲染树并将其绘制到屏幕上。

当文档加载过程中遇到 js 文件，html 文档会挂起渲染（加载解析渲染同步）的线程，不仅要等待文档中 js 文件加载完毕，还要等待解析执行完毕，才可以恢复 html 文档的渲染线程。因为 JS 有可能会修改 DOM，最为经典的 document.write，这意味着，在 JS 执行完成前，后续所有资源的下载可能是没有必要的，这是 js 阻塞后续资源下载的根本原因。所以我明平时的代码中，js 是放在 html 文档末尾的。

### 断开连接

现在的页面为了优化请求的耗时，默认都会开启持久连接（keep-alive），那么一个 TCP 连接确切关闭的时机，是这个 tab 标签页关闭的时候。这个关闭的过程就是著名的四次挥手。关闭是一个全双工的过程，发包的顺序的不一定的。一般来说是客户端主动发起的关闭，过程如下。

对于一个已经建立的连接，TCP 使用改进的三次握手来释放连接（使用一个带有 FIN 附加标记的报文段）。TCP 关闭连接的步骤如下：

第一步，当主机 A 的应用程序通知 TCP 数据已经发送完毕时，TCP 向主机 B 发送一个带有 FIN 附加标记的报文段（FIN 表示英文 finish）。

第二步，主机 B 收到这个 FIN 报文段之后，并不立即用 FIN 报文段回复主机 A，而是先向主机 A 发送一个确认序号 ACK，同时通知自己相应的应用程序：对方要求关闭连接（先发送 ACK 的目的是为了防止在这段时间内，对方重传 FIN 报文段）。

第三步，主机 B 的应用程序告诉 TCP：我要彻底的关闭连接，TCP 向主机 A 送一个 FIN 报文段。

第四步，主机 A 收到这个 FIN 报文段后，向主机 B 发送一个 ACK 表示连接彻底释放。
