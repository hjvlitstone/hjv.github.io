## HTTP-Chap5 HTTP首部
HTTP协议请求和响应报文中必定包含HTTP首部

### HTTP报文首部

HTTP协议的请求和响应报文中必定包含HTTP首部，首部内容位客户端和服务器分别处理请求和响应提供所需要的信息。

* http请求报文：由方法、URI、http版本、http首部字段等部分组成
* http响应报文：由http版本、状态码、http首部字段构成

### HTTP首部字段

HTTP首部字段是由首部字段名和字段值构成的，中间用：分隔，如Content-Type: text/html 表示报文主体的对象类型。也可以有多个值，

Keep-Alive: timeout=15, max=100

HTTP首部字段将定义成缓存代理和非缓存代理的行为，分成2种类型：

* 端到端首部(End-to-end Header):分在此类别中的首部会转发给请求/响应对应的最终接收目标，*且必须保存在由缓存生成的响应中*，另外规定他必须转发。
* 逐跳首部(Hop-by-hop Header):分在此类别中的首部只对单次转发有效，会因通过缓存或代理而不再转发，如果要使用逐跳首部，需提供Connection首部字段

### 通用首部字段

请求报文和响应报文都会使用到的首部

* Cache-Control:操作缓存的工作机制

* Connection:控制不再转发给代理的首部字段；管理持久连接Keep-Alive

* Date:HTTP报文的日期和时间

* Pragma：no-cache 同Cache-Control:no-cache，为了兼容http/1.0，只用在客户端发送的请求中，一般请求中两个都写。

* Trailer:事先说明在报文主体后记录了哪些首部字段

* Transfer-Encoding:规定了传输报文主体时采用的编码方式

* Upgrade: 用于检测HTTP协议及其他协议是否可使用更高的版本进行通信，其参数值可以用来指定一个完全不同的通信协议。

  ![http首部Upgrade](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/http%E9%A6%96%E9%83%A8Upgrade.jpg)

Connection的值是Upgrate，表示Upgrade字段仅本次传输有效，不再转发这个字段

![images](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/http%E9%A6%96%E9%83%A8Connection.jpg)

* Via:为了追踪客户端和服务器之间请求和响应报文的传输路径，经常会和TRACE方法一起使用，比如代理服务器接收到由TRACE方法发送过来的请求(其中Max-Forwards:0)时，代理服务器就不能再转发该请求了，他会将自己的信息附加到Via首部后，返回该请求的响应。
* Warning

##### 请求首部字段

用于补充请求的附加信息、客户端信息、对相应内容的优先级等内容

* Accept：此字段告诉服务器，用户代理能够处理的媒体类型及媒体类型的相对优先级，q代表权重，‘，’分隔每一项。如：

  ```http
  Accept: text/plain; q=0.5, text/html,
                 text/x-dvi; q=0.8, text/x-c
  ```

表示，首选text/html和text/x-c类型的，次之选text/x-dvi类型，如果还没有，则选择text/plain的

* Accept-Charset：可用来通知服务器用户代理支持的字符集及字符集的相对优先顺序

* Accept-Encoding：告知服务器用户代理智齿的内容编码及内容编码的优先级顺序

* Accept-Language：告知服务器用户代理能够处理的自然语言集(中文或英文等)及自然语言集的相对优先集

  `Accept-Language:zh-cn,zh;q=0.7,en-us,en;q=0.3`

   客户端在服务器有中文版资源的情况下，会请求其返回中文版资源对应的响应，没有中文版时返回英文版

* Authorization

![http首部Authorization](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/http%E9%A6%96%E9%83%A8Authorization.jpg)

想要通过服务器认证的用户代理会在接收到返回的401状态码响应后，把首部字段Authorization加入到请求中，值是用户代理的认证信息(证书值)

* Expect
* From：告知服务器使用用户代理的用户的电子邮箱地址
* Host：必须被包含在请求中的首部字段，告知服务器请求的资源所处的互联网主机名和端口号。如一个服务器只有一个IP地址，上面运行着多个域名的虚拟主机，就需要HOST字段帮助区分，不传具体值也行
* If-Match：形如If-xxx这种形式的请求首部字段，称为条件请求，服务器只有判断该条件为真时才会执行请求。If-Match告知服务器匹配资源所用的实体标记值(ETag),和服务器的这个值一致时，服务器才会接受请求，它的值也可以是*，服务器就会忽略判断，资源存在就处理。
* If-Modified-Since：如果在这个字段指定的日期时间后资源发生了更新，服务器会接受请求。用于确认客户端或代理拥有的本地资源的有效性。
* If-None-Match：和If-Match相反，在GET或HEAD方法中使用这个字段可以获取最新的资源。![http首部IfNoneMatch](E:\hepfiles\files\个人文件\http\img-http首部\http首部IfNoneMatch.jpg)

* If-Range：若此字段的值和请求资源的ETag值或时间一致时，则继续读取头部的Range字段，作为范围请求处理，不一致返回全部资源。
* If-Unmodified-Since：告知服务器指定的请求资源只有在字段值内指定的日期时间之后，未发生更新的情况下，才能处理请求。
* Max-Forwards：指定可经过服务器的最大数目，没经过一个，该值减一，为零的那个不往下继续转发，返回响应。
* Proxy-Authorization：认证所需要的信息，发生在客户端和代理之间。
* Range: bytes=5001-10000 处理后返回206的响应，无法处理请求时，返回200及全部资源。
* Referer：告知服务器请求的原始资源的URI
* TE: 告知服务器客户端能够处理响应的传输编码方式及相对优先级
* User-Agent：将创建请求的浏览器和用户代理名称等信息传达给服务器

### 响应首部字段

* Accept-Ranges：用来告知客户端服务器是否能处理范围请求，以指定获取服务器某个部分的资源。可处理范围请求时指定其为bytes，反之none

* Age: 告知客户端，源服务器在多久前创建了响应，单位为秒。若创建该响应的服务器时缓存服务器，Age值是指缓存后的响应再次发起认证到认证完成的时间值。代理创建响应时必须加上首部字段Age

* ETag：告知客户端实体标识，资源被缓存时，服务器会为每份资源分配对应的唯一ETag值，资源更新时，ETag值也会更新。强ETag值，无论实体发生多么细微的变化都会改变其值；弱ETag值，只有资源发生了根本改变，产生差异时才会改变ETag值

* Location：可以将响应接收方引导至某个与请求URI位置不同的资源

* Proxy-Authenticate：把由代理服务器所要求的认证信息发送给客户端，认证行为是在客户端与代理之间进行的

* Retry-After：告知客户端应该在多久之后再次发送请求

* Server：告知客户端当前服务器上安装的HTTP服务器应用程序的信息

* Vary：对缓存进行控制。从代理服务器接收到源服务器返回包含Vary指定项的响应之后，若在要进行缓存，仅对请求中含有相同Vary指定首部字段的请求返回进行缓存。即使对相同资源发起请求，但由于Vary指定的首部字段不相同，因此必须要从源服务器重新获取资源。

  ![http首部Vary](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/http%E9%A6%96%E9%83%A8Vary.jpg)

* WWW-Authenticate：用于HTTP访问认证。

### 实体首部字段

* Allow：用于通知客户端能够支持Request-URI指定资源的所有HTTP方法
* Content-Encoding：会告知客户端，服务器对实体的主体部分选用的内容编码方式。内容编码是指在不丢失实体信息的前提下所进行的压缩。
* Content-Language：告知客户端，实体主体使用的是什么语言
* Content-Length：表明了实体主体部分的大小(字节)
* Content-Location：给出与报文主体部分相对应的URI
* Content-MD5：是一串由MD5算法生成的值，其目的在于检查报文主体在传输过程中是否保持完整，以及确认传输到达。
* Content-Range：表示当前发送部分及整个实体大小
* Content-Type：说明了实体主体内对象的媒体类型
* Expires：将资源失效的日期告诉客户端。首部字段Cache-Control有指定的max-age指令时，比起首部字段Expires，会优先处理max-age指令。
* Last-Modified：指明资源最终修改的时间

### 为Cookie服务的首部字段

Cookie的工作机制时用户识别及状态管理。Web网站为了管理用户的状态，会通过Web浏览器，把一些数据临时写入用户的计算机内。

* Set-Cookie：服务器响应的首部字段。
* Cookie：请求首部字段。当客户端想获得HTTP状态管理支持时，就会在请求中包含从服务器接收到的cookie。

### 其他首部字段

HTTP首部字段是可以自行扩展的

* X-Frame-Options：http响应首部，用于控制网站内容在其他Web网站的Frame标签内的显示问题。有以下两个可指定的字段值:DENY:拒绝；SAMEORIGIN:仅同源域名下的页面匹配时许可加载该资源。
* X-XSS-Protection：响应首部，它是针对跨站脚本攻击的一种对策，用于控制浏览器XSS防护机制的开关
* DNT：请求首部，拒绝个人信息被收集，时表示拒绝被精准广告追踪的一种方法
* P3P：响应首部，可以让Web网站上的个人隐私变成一种仅供程序可理解的形式，以达到保护用户隐私的目的。
