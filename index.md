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
