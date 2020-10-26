## HTTP-Chap1 了解Web及网络基础

### 子网掩码
同一网段指的是IP地址和[子网掩码](https://baike.baidu.com/item/子网掩码)相与得到相同的[网络地址](https://baike.baidu.com/item/网络地址/9765459)。想在同一网段，必须做到[网络标识](https://baike.baidu.com/item/网络标识/7040658)相同。各类IP的网络标识算法都是不一样的，需要根据子网掩码的位数来判断。

### IP地址分类
ip地址分类需要复习下：一般情况下，我们将IP地址分为A/B/C三类，还有一些特殊的地址可以归为D/E类。
* 对于**A类**的地址来说，第一个字节是网络号，剩下的都是主机号的代表。这一类的IP地址首数字一般是“0”，地址的网络号在126以下，很多运用大型网络的公司会采用A类的IP地址。(01111111=>127)，数字0和 127不作为主机的IP地址，但是属于A类地址，数字127保留给内部回送函数，而数字0则表示该地址是本地宿主机，不能传送。
* 而对于**B类**的IP地址来说，它的前两个字节都代表着网络号，剩下的才是主机号，因此，地址的开头也就变成了“10”，网络号的取值在128-191(10000000-10111111)这个区间段之间。中等规模的网络一般都采用B类IP地址。
* 再来说说**C类**的IP地址，对于这类IP地址来说，它的前三个字节都被归属在网络号中，只有最后一个字节代表的才是主机号，它的数字最前列以“110”打头，网络号取值在192-223(11000000-11011111)这两个数字之间，依次类推，它被广泛地应用于小型的网络。
* D类IP地址第一个字节以“lll0”开始，它是一个专门保留的地址。它并不指向特定的网络，目前这一类地址被用在多点广播(Multicast)中。多点广播地址用来一次寻址一组计算机，它标识共享同一协议的一组计算机。
* E类IP地址以“llll0”开始，为将来使用保留

### 网络中两台主机的通信过程
* 在同一网段中
  * 应用层：TCP/IP协议上tcp的端口对应的各种应用程序,客户机要访问某个应用程序就会要求打开主机的这个固定的端口。而客户机自己会打开一个大于1024的随机端口用来跟对方的主机进行通信。
  * 传输层：对数据分段，添加**TCP报头**(包含**源端口，目的端口**，顺序号，确认序列号等)
  * 网络层：添加**IP报头**（包含**源IP地址，目的IP地址**）封装成数据包
  * 数据链路层：在数据包的前面封装数据帧头，数据包尾封装校验位。从而使数据包=>数据帧(添加**源MAC地址和目的MAC地址**，如果主机不知道目标MAC地址则想交换机发送ARP广播从而得到目标MAC地址)
  * **主机对物理层的操作：**

    将从逻辑链路层发送过来数据帧转换成能在物理线路上传输的电子信号，传递给网络上的转发设备交换机，由交换机进行处理。

    **交换机对数据帧的处理：**

    交换机接收到数据流后根据发送过来的数据帧的MAC地址查找目的主机，将数据发送给目的主机。转发过程不改变数据帧结构。

    **目的主机接收到数据帧的操作：**
    
    当目的主机接收到数据帧后**对比目的MAC(数据链路层)**，如是发送给自己的，则拆去数据帧头，发往网络层，**网络层对比目的IP**，如相同则拆包发往传输层，**传输层再对比目的端口**，确认相同则拆去数据段交给应用程进行数据组装。
    <img src="https://pic4.zhimg.com/v2-c7e1eb309118e0a5e309b33dabda9905_r.jpg" alt="v2-c7e1eb309118e0a5e309b33dabda9905_r.jpg (700×510)" style="zoom:50%;" />
* 不在同一网段中
  * 主机Ａ应用层数据到传输层被分段，打上TCP头（含源端口，目的端口），再向下发给网络层，打上IP地址（含源IP，目的IP）,再向下发送给数据链路层，打上数据帧（含源MAC，目的MAC），由于不知道目的MAC，则在MAC上打上于网关（路由器接口）的MAC地址，发往路由器A。
  * 路由器A收到消息后，重新封装数据帧（得知发送到B主机需要经过路由器B）,于是修改源MAC地址为路由器A的MAC地址，目标地址为B的MAC地址，（路由器A进行NAT地址转换）
  * 路由器B收到路由器A的信息，核对地址，检查IP，修改MAC，源MAC改为路由器B的MAC,（假如主机B在路由器B的子网中）目标MAC填写主机B的MAC，即下一步将信息发送给主机B。
  * 主机B接收到数据后对数据拆帧、包，TCP头，检查其目的地址与校验，重新整合这些数据流之后，将这个数据流传递给应用层处理。
* 总结：对于处于同一交换机的lan来说，进行通信，需要知道目标的ip和mac地址，通过发生广播数据包（一般2层目标mac地址为FFFFFFFF，3层目标ip就为目标ip），会像处于同一lan的所有ip进行比较,找到目标ip后向源发送一个单播包，这样就知道了目标ip的mac地址，但是如果ip不在一个网段或者同一个lan,这时通过arp广播，是不行的，需要路由器转发，这就需要配置源ip的网卡以及掩码，网关（网关需要配置成路由器接口的ip）,这样数据包的目的mac就填充成了路由器接口的mac,发生数据包到路由器，然后路由器拆包，检查2层mac地址，发现目标mac为自己的mac,欣喜若狂，然后拆掉3层检查ip,发现目标ip不是自己，然后拿着目标ip的网络号和路由表中的表项进行比较，找到最优的（最长匹配原则），若未找到就丢弃，并给源返回一个icmp错误，如果找到，重新封包（目标mac为下一跳ip对应的mac地址），把数据发生下一跳地址，当数据包到后，总是进行拆包，先检查2层mac地址是否为自己的，如果是，再拆掉3层ip头，检查目标ip是否为自己的，如果是，再拆4层，检查目标端口号是否在监听或者等待数据中，如果是，就把拆掉的应用数据发生给当前端口对应的进程，若未找到对应的端口，则会设置rst错误

注意：1.数据通信是双向的，比如ping ，既可以发生数据又可以接收数据

　　　2.对于路由器接口直连的网络，直接就在路由表中生成，其他的需要配置，（一般配置就是比如华为 ip route-static 源ip 掩码 下一跳地址），既要路由表配置目标ip的路由，让其可以到达目的地，也要配置回来的，就是源ip对应的路由，让其可以返回一个回应，这才是一个能够ping通的关键
* 其他知识：
  * ip地址 每次上网都不一样。由dns随机自动分配。
  * mac地址唯一。
  * <img src="C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20201012151613541.png" alt="image-20201012151613541" style="zoom:30%;" />
```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/hjvlitstone/hjv.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
