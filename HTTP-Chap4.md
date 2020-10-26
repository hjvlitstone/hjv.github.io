## HTTP-Chap5 与HTTP协作的Web服务器

一台Web服务器可搭建多个独立域名的Web网站，也可做为通信路径上的中转服务器提升传输效率

### 通信数据转发程序：代理、网关、隧道

* 代理：一种有转发功能的应用程序，扮演了位于服务器和客户端"中间人"的角色
* 网关：转发其他服务器通信数据的服务器
* 隧道：是在相隔甚远的客户端和服务器两者之间进行中转，并保持双方通信连接的应用程序。

### 代理

使用代理服务器的理由有：利用缓存技术减少网络贷款的流量，组织内部针对特定网站的访问控制，以获取访问日志为主要目的等。代理有多种使用方法，按两种基准分类。一种是是否使用缓存，另一种时是否会修改报文。

* 缓存代理：代理转发响应时，缓存代理会预先将资源的副本保存在代理服务器上，再次受到对相同资源的请求时，就可以不从源服务器那里获取资源，而是将之前缓存的资源作为响应返回。
* 透明代理：转发请求或响应时，不对报文做任何加工的代理类型被称为透明代理，反之叫非透明代理。

### 网关

网关的工作机制和代理十分相似，而网关能使通信线路上的服务器提供非HTTP协议服务。利用网关能提高通信的安全性，因为可以在网关和客户端之间的通信线路上加密以确保连接的安全。

### 隧道

隧道可按要求建立起一条与其他服务器的通信线路，届时使用SSL等加密手段进行通信。隧道的目的是确保客户段能与服务器进行安全的通信。隧道本身不会去解析HTTP请求，原样中转，隧道会在通信双方断开连接时结束。

### 保存资源的缓存

指代理服务器或客户端本地磁盘内保存的资源副本，利用缓存可减少对源服务器的访问，因此也就节省了通信流量和通信时间。缓存服务器时代理服务器的一种。