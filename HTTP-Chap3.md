## HTTP-Chap3 返回结果的HTTP状态码

### 2xx 成功

* 200 OK(成功)   
* 204 No Content(请求处理成功，但没有资源返回)
* 206 Partial Content(客户端进行了范围请求，服务器成功响应)

### 3xx 重定向

浏览器需要执行某些特殊的处理以正确处理。当返回301，302，303时，几乎所有的浏览器都会把POST改成GET，并删除请求报文内的主体，之后请求会自动再次发送。

* 301 Moved Permanently(请求的资源已被分配了新的URI，以后应使用资源现在所指的URI)永久性重定向
* 302 Found(请求的资源已被分配了新的URI，希望用户本次能使用新的URI访问)临时性重定向
* 303 See Other(由于请求对应的资源存在着另一个URI，应使用GET方法定向获取请求的资源)
* 304 Not Modified(客户端发送附带条件的请求时，服务器端允许请求访问资源，但发生请求为满足条件的情况)
* 307 Temporary Redirect(临时重定向，和302相同，只是不会处理使POST变成GET)

### 4xx 客户端错误

* 400 Bad Request(请求报文中存在语法错误)
* 401 Unauthorized(用户信息需认证或用户认证失败)
* 403 Forbidden (对请求资源的访问被服务器拒绝了)
* 404 Not Found(服务器上没有请求的资源)

### 5xx 服务器错误

* 500 Internal Server Error(服务器端在执行请求时发生了错误)
* 503 Service Unavailable(服务器暂时正处于朝服在或停机维护)
