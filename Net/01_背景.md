**计算机网络**（computer net)，简称网络，是指容许节点分享资源的数字电信网络。网络可按照传输介质、传输协议、网络大小、拓扑、流量控制机制、创建目的等因素区分（如按传输介质可分为有线及无线两类）。世界上最大的网络为互联网。

**OSI模型**（Open System Interconnection Model），由国际标准化组织提出，分为5层（也有7层一说，本人更倾向5层）：

* 应用层

  应用层（Application Layer）提供为应用软件而设计的接口，以设置与另一应用软件之间的通信。例如：HTTP、HTTPS、FTP、Telnet、SSH、SMTP、POP3等。

* 传输层

  传输层（Transport Layer）把传输表头（TH）加至数据以形成数据包。传输表头包含了所使用的协议等发送信息。例如:传输控制协议（TCP）等。

* 网络层

  网络层（Network Layer）决定数据的路径选择和转寄，将网络表头（NH）加至数据包，以形成分组。网络表头包含了网络资料。例如:互联网协议（IP）等。

* 链路层

  数据链路层（Data Link Layer）负责网络寻址、错误侦测和改错。当表头和表尾被加至数据包时，会形成[信息框](https://zh.wikipedia.org/wiki/資訊框)（Data Frame）。数据链表头（DLH）是包含了物理地址和错误侦测及改错的方法。数据链表尾（DLT）是一串指示数据包末端的字符串。例如以太网、无线局域网（Wi-Fi）和通用分组无线服务（GPRS）等。

  分为两个子层：逻辑链路控制（logical link control，LLC）子层和介质访问控制（Media access control，MAC）子层。

* 物理层

  物理层（Physical Layer）在局部局域网上传送[数据帧](https://zh.wikipedia.org/wiki/数据帧)（Data Frame），它负责管理电脑通信设备和网络媒体之间的互通。包括了针脚、电压、线缆规范、集线器、中继器、网卡、主机接口卡等。

接下来自底向上介绍每一层的作用。