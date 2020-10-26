## HTTP-Chap8 基于HTTP的功能追加协议

### 使用浏览器进行全双工通信的WebSocket

WebSocket，即Web浏览器与Web服务器之间全双工通信标准。一旦Web服务器与金额护短之间建立起WebSocket协议的通信连接，之后所有的通信都依靠这个专用协议进行。通信过程中可互相发送JSON.XML.HTML，图片等任意格式的数据。

* 推送功能：支持由服务器向客户端推送数据的功能，不必等待客户端的请求。
* 减少通信量：只要建立起WebSocket连接，就希望一直保持连接状态。和HTTP相比，不但每次连接时的总开销减少，而且由于WebSocket的首部信息很小，通信量也相应减少了。

为了实现WebSocket通信，需要用到HTTP的Upgrade首部字段，告知服务器通信协议发生改变，以达到握手的目的。<img src="https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/WebSocket%E8%AF%B7%E6%B1%82%E5%A4%B4.jpg" alt="WebSocket请求头" style="zoom:80%;" />

响应返回101 Switching Protocols，成功握手确立WebSocket连接后，通信时不再使用HTTP的数据帧，而采用WebSocket独立的数据帧。![WebSocket通信流程](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/WebSocket%E9%80%9A%E4%BF%A1%E6%B5%81%E7%A8%8B.jpg)

JS可调用http://www.w3.org/TR/websockets/内提供的WebSocket程序接口，以实现WebSocket协议下全双工通信。以下为每50ms发送一次数据的实例：

![WebSocket通信实例](https://github.com/hjvlitstone/hjv.github.io/blob/gh-pages/images/WebSocket%E9%80%9A%E4%BF%A1%E5%AE%9E%E4%BE%8B.jpg)
