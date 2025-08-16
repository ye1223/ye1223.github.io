---
title: TCP
categories:
  - []
wordCount: 37
charCount: 454
imgCount: 3
vidCount: 0
wsCount: 0
cbCount: 0
readTime: About 48 seconds
featured_image:
  - https://s2.loli.net/2023/09/18/as3J7Y8ePvRCAjg.png
tags:
  - Computer Network
date: 2023-09-11 15:44:44
---


<strong style="color:red;">在不可靠的网络链路中，进行可靠的链接/断开</strong>

**全双工（Full Duplex）**

1. 双向传输:TCP连接的两端都可以同时发送和接收数据包。
2. 双向流量控制:发送方和接收方都可以通过滑动窗口机制控制数据流量,防止对方缓冲区溢出。
3. 双向确认:发送的数据包可以得到对方的确认应答ACK。接收方也可以通过ACK告知发送方自己已经正确接收了数据。
4. 双向重传:如果任意一方没有收到确认或数据包丢失,都可以触发重传机制,直到数据正确发送。
5. 全双工通信:TCP的全双工特性使得双方可以同时进行发言而不会相互干扰,就像电话通话一样。



> `ACK` 和`FIN` 是 TCP 协议中两个重要的控制标志(control flag)
>
> `ACK(Acknowledgement) `确认号，表示确认号,确认接收端已正确接收到前面的数据
>
> `FIN (Finish)` 结束标志，表示发送端已经发送完数据,可以关闭连接了。



## 三次握手🤝


<img src="https://i.imgur.com/bJWYomc.png" alt="三次挥手示意图" style="width:50%" />

## 传输

一包数据可能被拆为多包数据发送

**丢包问题？乱序问题？**

TCP为每个链接建立了一个==发送缓冲区==

<img src="https://s2.loli.net/2023/09/11/ACj3KLUHcNiVIMs.png" style="width:50%;" alt="传输示意图" />

> 一问一答的方式发送接收（发送端一次也可以发送多包数据，接收端只用回复一次ACK）

> 发送端可以切割发送，接收端利用序列号和长度重组出完整的数据

> 丢失了某个数据包，接收端可以要求发送端重传。
>
> 接收端向发送端发送ACK=xxx，发送端重传，接收端收到再**补齐**数据



## 四次挥手👋🏻

这里以客户端取消l为例

<img src="https://s2.loli.net/2023/09/11/nrxG86lYUf7CqMX.png" alt="四次挥手示意图" style="width:50%;" />


Client等待一段时间，目的为了确保ACK成功发送给Server，否则Server重新发送FIN给client，再重新执行




