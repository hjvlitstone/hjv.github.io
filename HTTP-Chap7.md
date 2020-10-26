## HTTP-Chap7 确认访问用户身份的认证

为确认正在访问服务器的对方是否真的具有访问系统的权限，需要核对“密码”、“动态令牌”、“数字证书”、“生物认证”、“IC卡”等。认证方式如下：

### BASIC认证

![BASIC认证步骤](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/BASIC%E8%AE%A4%E8%AF%81%E6%AD%A5%E9%AA%A4.jpg)

Base64编码不是加密处理，不需要任何附加信息就可以对其解码。如果在想进行一次BASIC认证，一般的浏览器无法进行认证注销。所以这个认证方式并不常用。

### DIGEST认证

同样使用质询/相应的方式，但是不直接发送明文密码。一开始一方先发送认证要求给另一方，接着使用从另一方那接收到的质询码计算生成响应码，最后将响应码回给对方。就降低了泄露密码的风险。![img](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/DIGEST%E8%AE%A4%E8%AF%81.jpg)

DIGEST认证提供了高于BASIC认证的安全等级，但是和HTTPS的客户端认证相比仍然很弱。DIGEST认证提供防止密码被窃听的保护机制，但并不存在防止用户伪装的保护机制。适用范围也有所受限。

### SSL客户端认证

SSL客户端认证是借由HTTPS的客户端证书完成认证的方式，凭借客户端证书认证，服务器可确认访问是否来自己登录的客户端。一般会和基于表单认证组合形成一种双因素认证来使用。第一个认证因素的SSL客户端证书用来认证客户段计算机，另一个认证因素的密码用来确定这是用户本人的行为。

### 基于表单认证

基于表单认证本身是通过服务器端的 Web应用，将客户端发送过来的用户ID和密码与之前登陆过的信息做匹配来进行认证的。但鉴于HTTP是无状态协议，之前已认证成功的用户状态无法通过协议层面保存下来。于是使用了Cookie管理Session。![img](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/cookie%E7%AE%A1%E7%90%86session.jpg)
