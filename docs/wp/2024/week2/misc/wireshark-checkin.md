---
titleTemplate: ':title | WriteUp - NewStar CTF 2024'
---
# wireshark_checkin

这么多数据包，先快速找到主要部分。

![快速找主要部分](/assets/images/wp/2024/week2/wireshark_checkin_1.png)

标绿色的是http协议相关的，鼠标左键点击`GET /reverse.c` 这个条目，看wireshark界面左下角，有一个port叫做7070，这个是http服务器的开放端口。

![找到http服务器的端口](/assets/images/wp/2024/week2/wireshark_checkin_2.png)

接下来点击最上面的搜索框，输入tcp.port == 7070，这样就可以快速过滤出所有有关http协议的报文

![过滤出http协议报文](/assets/images/wp/2024/week2/wireshark_checkin_3.png)

这样变得好看多了，但是还不够清晰，因为这里包含了所有http请求和响应，假如只想看其中一个http请求的流程：看GET /flag.txt 这个条目

![找到flag.txt](/assets/images/wp/2024/week2/wireshark_checkin_4.png)

有一个src port 33751。所以过滤器这样写：tcp.port == 33751，因为同一个http请求和响应的客户端用的是同一个端口。这样就能只看有关flag.txt这次请求和响应的所有过程。

![只看flag.txt](/assets/images/wp/2024/week2/wireshark_checkin_5.png)

这就是3次握手和四次挥手。

3次握手

![3次握手](/assets/images/wp/2024/week2/wireshark_checkin_6.png)

4次挥手

![4次挥手](/assets/images/wp/2024/week2/wireshark_checkin_7.png)

但是下面这个，只有一个FIN，怎么回事？

![只有一个FIN](/assets/images/wp/2024/week2/wireshark_checkin_8.png)

因为还有一个FIN在HTTP/1.1这里面，发送完响应后，就立刻第一次挥手了。

![HTTP/1.1](/assets/images/wp/2024/week2/wireshark_checkin_9.png)

flag在右下角。