## HTTP-Chap6 确保Web安全的HTTPS
在HTTP协议中有可能存在信息窃听或身份伪装等安全问题，使用HTTPS通信机制可以有效地防止这些问题。

### HTTP的缺点

* 通信使用明文可能会被窃听：

  * HTTP本身不具备加密的功能，所以HTTP报文使用明文(指未经加密的报文)方式发送。即使已经加密处理过的通信，也会被窥视到通信内容，加密后的报文信息本身还是会被看到的。
  * 加密处理防止被窃听：
    * 通信加密：HTTP协议中没有加密机制，但可以通过和SSL或TLS的组合使用加密HTTP的通信内容。与SSL组合使用的HTTP称为HTTPS或HTTP over SSL
    * 内容加密：把HTTP报文里所含的内容进行加密处理，需客户端和服务器同时具备加密和解密机制，但是内容仍有被篡改的风险。

* 不验证通信方的身份就可能遭遇伪装：

  * HTTP协议中的请求和响应不会和通信方确认，也就是说存在“服务器是否就是发送请求URI真正指定的主机，返回的响应是否真的返回到实际提出请求的客户端”等类似问题
    * 在HTTP协议通信时，由于不存在确认通信方的处理步骤，任何人都可以发起请求。服务器只要接收到请求，不管是谁都会返回一个响应。所以可能会有“服务器伪装”、“客户端伪装”、“无访问权限的也可访问”、“无法判断请求来自何方”、“无意义的请求也会照单全收，无法阻止海量请求下的DoS攻击”等问题
    * SSL不仅可以提供加密处理，还是用了一种被称为证书的手段，可用于确认通信方。

* 无法证明报文完整性，可能被篡改

  完整性指信息的准确度

  * 请求或响应在传输途中，遭攻击者拦截并篡改内容的攻击成为中间人攻击。但是通信双方察觉不到。
  * HTTP协议确定报文完整性的方法并不便捷、可靠。PGP和MD5本身被改写的话，用户是意识不到的。

### HTTP+加密+认证+完整性保护=HTTPS

**HTTP是身披SSL外壳的HTTP**，进行了通信加密，确认通信双方的认证和报文的完整性保护。通常HTTP直接和TCP通信，当使用SSL时，则演变成先和SSL通信，再由SSL和HTTP通信。SSL是独立于HTTP的协议，其他协议也可以配合他使用，SSL是当今应用最为广泛的网络安全技术。

SSL采用一种叫做**公开密钥加密**的加密处理方式。近代的加密方法中加密算法是公开的，密钥却是保密的，加密和保密都会用到密钥，如果密钥被攻击者获得，那加密就失去了意义。公开密钥加密使用一对非对称的密钥，一把叫做私有密钥，另一把叫做公开密钥，公开密钥可以随意发布，任何人都可以获得。发送密文的乙方使用对方的公开密钥进行加密处理，对方收到被加密的信息后，再使用自己的私有密钥进行解密。

HTTPS采用**混合加密机制**，在交换密钥环节使用公开密钥加密方式，之后的建立通信交换报文阶段则使用共享密钥加密方式。

公开密钥加密无法保证公开密钥本身是没被篡改过的，可以使用数字证书认证机构颁发的公开密钥证书。服务器把证书发送给客户端，客户端对密钥进行验证，验证通过即可通信。公开密钥安全转交也是困难的，所以多数浏览器开发商发布版本时，会实现在内部植入常用认证机关的公开密钥。

https通信流程：

![https通信流程](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/https%E9%80%9A%E4%BF%A1%E6%B5%81%E7%A8%8B.jpg)

<img src="https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/https%E9%80%9A%E4%BF%A1%E6%B5%81%E7%A8%8B1.jpg" alt="https通信流程1" style="zoom:50%;" />

<img src="https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/https%E9%80%9A%E4%BF%A1%E6%B5%81%E7%A8%8B%E6%8F%8F%E8%BF%B0.jpg" alt="https通信流程描述" style="zoom:50%;" />

在以上流程中，应用层发送数据时回附加一种叫MAC的报文摘要，MAC能够查知报文是否遭到篡改，从而保护报文的完整性。

不一直使用HTTPS的原因：

* 通信慢，大量消耗CPU及内存等资源。加密处理，更多的消耗硬件资源。
* 只有在包含个人信息等敏感数据时才利用HTTPS。
* 节约购买证书的开销
