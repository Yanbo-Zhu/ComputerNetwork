# 1 快速抓包初体验

先介绍一个使用wireshark工具抓取ping命令操作的示例，让读者可以先上手操作感受一下抓包的具体过程。

  1、打开wireshark 2.6.5，主界面如下：

**![](https://img2018.cnblogs.com/blog/774327/201812/774327-20181216081023389-2002447427.png)**

  2、选择菜单栏上Capture -> Option，勾选WLAN网卡（这里需要根据各自电脑网卡使用情况选择，简单的办法可以看使用的IP对应的网卡）。点击Start。启动抓包。

![](https://img2018.cnblogs.com/blog/774327/201812/774327-20181216081719715-1763002076.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/1fba3b884c724e7e9ab1c981b9c9d156.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA546E54G16aOO,size_20,color_FFFFFF,t_70,g_se,x_16)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/5ff77aeb6adf4e3481cc721668c9abc9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA546E54G16aOO,size_20,color_FFFFFF,t_70,g_se,x_16)

  3、wireshark启动后，wireshark处于抓包状态中。

![](https://img2018.cnblogs.com/blog/774327/201812/774327-20181216082050941-1775919525.png)

  4、执行需要抓包的操作，如ping www.baidu.com。

  5、操作完成后相关数据包就抓取到了。为避免其他无用的数据包影响分析，可以通过在过滤栏设置过滤条件进行数据包列表过滤，获取结果如下。说明：ip.addr == 119.75.217.26 and icmp 表示只显示ICPM协议且源主机IP或者目的主机IP为119.75.217.26的数据包。

![](https://img2018.cnblogs.com/blog/774327/201812/774327-20181216082650625-902953031.png)

  5、wireshark抓包完成，就这么简单。关于wireshark过滤条件和如何查看数据包中的详细内容在后面介绍。


# 2 Wireshakr抓包界面

- 标签栏
- 菜单栏
- 工具栏
- 数据包过滤栏
- 数据包列表区
- 数据包详细区
- 数据包字节区
- 数据包统计区

![](https://img2018.cnblogs.com/blog/774327/201812/774327-20181216083953866-9796212.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/5aa42c01a7e14c59a1f4c3bb61a8968d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA546E54G16aOO,size_20,color_FFFFFF,t_70,g_se,x_16)

## 2.1 Display Filter(显示过滤器)

用于设置过滤条件进行数据包列表过滤。菜单路径：Analyze --> Display Filters。  

![](https://img2018.cnblogs.com/blog/774327/201812/774327-20181216161033619-451281242.png)

## 2.2 Packet List Pane(数据包列表)，
 显示捕获到的数据包，每个数据包包含编号，时间戳，源地址，目标地址，协议，长度，以及数据包信息。 不同协议的数据包使用了不同的颜色区分显示。

![](https://img2018.cnblogs.com/blog/774327/201812/774327-20181216161154412-766346180.png)

### 2.2.1 时间戳
  调整数据包列表中时间戳显示格式。调整方法为View -->Time Display Format --> Date and Time of Day。调整后格式如下：

![](https://img2018.cnblogs.com/blog/774327/201812/774327-20181216113851595-75851722.png)



### 2.2.2 域名解析和端口解析

Statustucs -> show addreas resolution 

## 2.3 Packet Details Pane(数据包详细信息),
在数据包列表中选择指定数据包，在数据包详细信息中会显示数据包的所有详细信息内容。数据包详细信息面板是最重要的，用来查看协议中的每一个字段。各行信息分别为

  （1）Frame:   物理层的数据帧概况
  （2）Ethernet II: 数据链路层以太网帧头部信息
  （3）Internet Protocol Version 4: 互联网层IP包头部信息
  （4）Transmission Control Protocol:  传输层T的数据段头部信息，此处是TCP
  （5）Hypertext Transfer Protocol:  应用层的信息，此处是HTTP协议

![](http://www.cr173.com/up/2013-5/2013050217125736394.png)

### 2.3.1 http协议是一个应用协议  
![在这里插入图片描述](https://img-blog.csdnimg.cn/e3af09fbe26842c4861175e6efd1ab06.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA546E54G16aOO,size_20,color_FFFFFF,t_70,g_se,x_16)  

### 2.3.2 IP层数据报文：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/f9af12fea0fe4aca927eeadca299e068.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA546E54G16aOO,size_20,color_FFFFFF,t_70,g_se,x_16)  

### 2.3.3 TCP数据报文
 从下图可以看到wireshark捕获到的TCP包中的每个字段。
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/737722e7d68b45ffa3b22b7414dcf7c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA546E54G16aOO,size_20,color_FFFFFF,t_70,g_se,x_16)

![](http://www.cr173.com/up/2013-5/2013050217125787134.png)

## 2.4 Dissector Pane(数据包字节区)。




# 3 Name Resolution 

Edit -> Perferemces 
激活 Resolve Network addresses 
![](image/Pasted%20image%2020241104091056.png)


这样显示出来的都是 dns name, 不再是 ip addresse 

![](image/Pasted%20image%2020241104091157.png)


## 3.1 显示 geographical 信息 

> location lookup 

注册个账户 , 去下载  geolite city 

![](image/Pasted%20image%2020241104091649.png)

使用下载的包 
MaxMind database directions 

![](image/Pasted%20image%2020241104091726.png)


![](image/Pasted%20image%2020241104091750.png)


----
查看地址 

![](image/Pasted%20image%2020241104091406.png)

![](image/Pasted%20image%2020241104091355.png)
