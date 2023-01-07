  Wireshark是非常流行的网络封包分析软件，可以截取各种网络数据包，并显示数据包详细信息。常用于开发测试过程各种问题定位。本文主要内容包括：

  1、Wireshark软件下载和安装以及Wireshark主界面介绍。
  2、WireShark简单抓包示例。通过该例子学会怎么抓包以及如何简单查看分析数据包内容。
  3、Wireshark过滤器使用。通过过滤器可以筛选出想要分析的内容。包括按照协议过滤、端口和主机名过滤、数据包内容过滤。


# 1 Wireshark 和 Fiddler 比较
为了安全考虑，wireshark只能查看封包，而不能修改封包的内容，或者发送封包。
wireshark能获取HTTP，也能获取HTTPS，但是不能解密HTTPS，所以wireshark看不懂HTTPS中的内容.
总结，如果是处理HTTP,HTTPS 还是用Fiddler, 其他协议比如TCP,UDP 就用wireshark.

## 1.1 Fiddler是一个http协议调试代理工具
Fiddler是一个http协议调试代理工具，它能够记录并检查所有你的电脑和互联网之间的http通讯，设置断点，查看所有的“进出”Fiddler的数据（指cookie,html,js,css等文件）。
Fiddler 要比其他的网络调试器要更加简单，因为它不仅仅暴露http通讯还提供了一个用户友好的格式。它是用C#写出来的,包含一个简单却功能强大的基于JScript .NET 事件脚本子系统，它的灵活性非常棒，可以支持众多的http调试任务，并且能够使用.net框架语言进行扩展


# 2 TCPDUMP
那么我们在Linux系统下一般用什么抓包工具呢？
那就是TCPDUMP啦！

简单介绍一下：TCPDump可以将网络中传送的数据包完全截获下来提供分析。它支持针对网络层、协议、主机、网络或端口的过滤，并提供and、or、not等逻辑语句来帮助你去掉无用的信息。
Linux作为网络服务器，特别是作为路由器和网关时，数据的采集和分析是不可少的。TcpDump是Linux中强大的网络数据采集分析工具之一。
用简单的话来定义tcpdump，就是：dump the traffic on a network，根据使用者的定义对网络上的数据包进行截获的包分析工具。

作为互联网上经典的的系统管理员必备工具，tcpdump以其强大的功能，灵活的截取策略，成为每个高级的系统管理员分析网络，排查问题等所必备的工具之一。
