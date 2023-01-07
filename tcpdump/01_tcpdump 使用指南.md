# 1 帖子:
https://www.cnblogs.com/wongbingming/p/13212306.html
https://juejin.cn/post/6844904084168769549

![](http://image.iswbm.com/20200630095709.png)

# 2 使用 
1. 使用命令 tcpdump
2. 执行想要测试的程序
3. 只能通过 `ctrl-c` 来停止 tcpdump抓取
    1. 或者在 执行命令的时候 设置了 -c x,  (要抓取那几个包)


# 3 基本语法
tcpdump 的各种参数：
![](http://image.iswbm.com/20200628111325.png)

1.  option 可选参数：将在后边一一解释。
2.  proto 类过滤器：根据协议进行过滤，可识别的关键词有： tcp, udp, icmp, ip, ip6, arp, rarp,ether,wlan, fddi, tr, decnet
3.  type 类过滤器：可识别的关键词有：host, net, port, portrange，这些词后边需要再接参数。
4.  direction 类过滤器：根据数据流向进行过滤，可识别的关键字有：src, dst，同时你可以使用逻辑运算符进行组合，比如 src or dst

proto、type、direction 这三类过滤器的内容比较简单，也最常用，因此我将其放在最前面，也就是 **第三节：常规过滤规则**一起介绍。
而 option 可选的参数非常多，有的甚至也不经常用到，因此我将其放到后面一点，也就是 **第四节：可选参数解析**

当你看完前面六节，你对 tcpdump 的认识会上了一个台阶，至少能够满足你 80% 的使用需求。
你一定会问了，还有 20% 呢？ 其实 tcpdump 还有一些过滤关键词，它不符合以上四种过滤规则，可能需要你单独记忆。关于这部分我会在 **第六节：特殊过滤规则** 里进行介绍。

# 4 tcpdump 的输出


## 4.1 输出内容结构

tcpdump 输出的内容虽然多，却很规律。
这里以我随便抓取的一个 tcp 包为例来看一下

`21:26:49.013621 IP 172.20.20.1.15605 > 172.20.20.2.5920: Flags [P.], seq 49:97, ack 106048, win 4723, length 48`

从上面的输出来看，可以总结出：

1.  第一列：时分秒毫秒 21:26:49.013621
2.  第二列：网络协议 IP
3.  第三列：发送方的ip地址+端口号，其中172.20.20.1是 ip，而15605 是端口号
4.  第四列：箭头 >， 表示数据流向
5.  第五列：接收方的ip地址+端口号，其中 172.20.20.2 是 ip，而5920 是端口号
6.  第六列：冒号
7.  第七列：数据包内容，包括Flags 标识符，seq 号，ack 号，win 窗口，数据长度 length，其中 [P.] 表示 PUSH 标志位为 1，更多标识符见下面

## 4.2 Flags 标识符

使用 tcpdump 抓包后，会遇到的 TCP 报文 Flags，有以下几种：

-   `[S]` : SYN（开始连接）
-   `[P]` : PSH（推送数据）
-   `[F]` : FIN （结束连接）
-   `[R]` : RST（重置连接）
-   `[.]` : 没有 Flag （意思是除上面四种类型外的其他情况，有可能是 ACK 也有可能是 URG）


# 5 常规过滤器 proto、type、direction 


## 5.1 基于IP地址过滤：host, src, dst 
1 
使用 host 就可以指定 host ip 进行过滤
$ tcpdump host 192.168.10.100
数据包的 ip 可以再细分为源ip和目标ip两种

2 根据源ip进行过滤
$ tcpdump -i eth2 src 192.168.10.100

3 根据目标ip进行过滤
$ tcpdump -i eth2 dst 192.168.10.200

4 
如果想只抓取从该主机发出的流量，可以使用下面的命令：
$ tcpdump src host 1.2.3.4

## 5.2 基于网段进行过滤：net
网段
net <net_address>/<net_mask>

源网段
src net <net_address>/<net_mask>

目的网段
dst net <net_address>/<net_mask>

Network 过滤器用来过滤某个网段的数据，使用的是 CIDR 模式。
可以使用四元组（x.x.x.x）、三元组（x.x.x）、二元组（x.x）和一元组（x）。
- 四元组就是指定某个主机，
- 三元组表示子网掩码为 255.255.255.0，
- 二元组表示子网掩码为 255.255.0.0，
- 一元组表示子网掩码为 255.0.0.0。例如，

抓取所有发往网段 192.168.1.x 或从网段 192.168.1.x 发出的流量： $ tcpdump net 192.168.1
抓取所有发往网段 10.x.x.x 或从网段 10.x.x.x 发出的流量： $ tcpdump net 10
和 Host 过滤器一样，这里也可以指定源和目的： $ tcpdump src net 10
也可以使用 CIDR 格式： $ tcpdump src net 172.16.0.0/12


若你的ip范围是一个网段，可以直接这样指定
$ tcpdump net 192.168.10.0/24

网段同样可以再细分为源网段和目标网段

1 根据 源网段进行过滤
$ tcpdump src net 192.168

2  根据 目标网段进行过滤
$ tcpdump dst net 192.168



## 5.3 基于端口进行过滤：port

```shell
端口: port <port>
源端口: src port <port>
目的端口: dst port <port>
端口范围: tcpdump -i <interface_1> portrange <port_lower>-<port_upper>
```


表达式

```sh
"<expression>" := "<expression>" && "<expression>"
"<expression>" := "<expression>" || "<expression>"
"<expression>" := !"<expression>"
```


使用 port 就可以指定特定端口进行过滤
$ tcpdump port 8088

端口同样可以再细分为源端口，目标端口
1 根据 源端口进行过滤
$ tcpdump src port 8088

2 根据目标端口进行过滤
$ tcpdump dst port 8088

3 如果你想要同时指定两个端口你可以这样写
$ tcpdump port 80 or port 8088
但也可以简写成这样
$ tcpdump port 80 or 8088

4 
如果你的想抓取的不再是一两个端口，而是一个范围，一个一个指定就非常麻烦了，此时你可以这样指定一个端口段。
$ tcpdump portrange 8000-8080
$ tcpdump src portrange 8000-8080
$ tcpdump dst portrange 8000-8080

5 
对于一些常见协议的默认端口，我们还可以直接使用协议名，而不用具体的端口号
比如 http == 80，https == 443 等
$ tcpdump tcp port http

## 5.4 基于协议进行过滤：proto

Proto 过滤器用来过滤某个协议的数据，关键字为 proto，可省略。
proto 后面可以跟上协议号或协议名称，支持 icmp, igmp, igrp, pim, ah, esp, carp, vrrp, udp和 tcp。
因为通常的协议名称是保留字段，所以在于 proto 指令一起使用时，必须根据 shell 类型使用一个或两个反斜杠（\）来转义。Linux 中的 shell 需要使用两个反斜杠来转义，MacOS 只需要一个。

常见的网络协议有：tcp, udp, icmp, http, ip,ipv6 等
protocol 可选值：ip, ip6, arp, rarp, atalk, aarp, decnet, sca, lat, mopdl, moprc, iso, stp, ipx, or netbeui

若你只想查看 icmp 的包，可以直接这样写
$ tcpdump icmp
$ tcpdump -n proto \\icmp
$ tcpdump -n icmp

以 UDP 为例，可以加上参数 udp 或 protocol 17，这两个命令意思相同。
$ tcpdump -i eth0 udp
$ tcpdump -i eth0 proto 17

TCP
$ tcpdump -i eth0  tcp 
$ tcpdump -i eth0 proto 6

### 5.4.1 基本IP协议的版本进行过滤

1
当你想查看 tcp 的包，你也许会这样子写
$ tcpdump tcp

这样子写也没问题，就是不够精准，为什么这么说呢？
ip 根据版本的不同，可以再细分为 IPv4 和 IPv6 两种，如果你只指定了 tcp，这两种其实都会包含在内。

那有什么办法，能够将 IPv4 和 IPv6 区分开来呢？

1 
很简单，如果是 IPv4 的 tcp 包 ，就这样写（友情提示：数字 6 表示的是 tcp 在ip报文中的编号。）
$ tcpdump 'ip proto tcp'
$ tcpdump ip proto 6
$ tcpdump 'ip protochain tcp'
$ tcpdump ip protochain 6

2 
而如果是 IPv6 的 tcp 包 ，就这样写
$ tcpdump 'ip6 proto tcp'
$ tcpdump ip6 proto 6
$ tcpdump 'ip6 protochain tcp'
$ tcpdump ip6 protochain 6

3 
关于上面这几个命令示例，有两点需要注意：
1. 跟在 proto 和 protochain 后面的如果是 tcp, udp, icmp ，那么过滤器需要用引号包含，这是因为 tcp,udp, icmp 是 tcpdump 的关键字。
2. 跟在ip 和 ip6 关键字后面的 proto 和 protochain 是两个新面孔，看起来用法类似，它们是否等价，又有什么区别呢？
    1. 关于第二点，网络上没有找到很具体的答案，我只能通过 man tcpdump 的提示， 给出自己的个人猜测，但不保证正确。
3. proto 后面跟的 ``<protocol>`` 的关键词是固定的，只能是 ip, ip6, arp, rarp, atalk, aarp, decnet, sca, lat, mopdl, moprc, iso, stp, ipx, or netbeui 这里面的其中一个。
4. 而 protochain 后面跟的 protocol 要求就没有那么严格，它可以是任意词，只要 tcpdump 的 IP 报文头部里的 protocol 字段为 ``<protocol>`` 就能匹配上。

理论上来讲，下面两种写法效果是一样的
$ tcpdump 'ip && tcp'
$ tcpdump 'ip proto tcp'

同样的，这两种写法也是一样的
$ tcpdump 'ip6 && tcp'
$ tcpdump 'ip6 proto tcp'


# 6 option 可选参数

## 6.1 设置不解析域名提升速度 (-n)
-   `-n`：不把ip转化成域名，直接显示 ip，避免执行 DNS lookups 的过程，速度会快很多
-   `-nn`：不把协议和端口号转化成名字，速度也会快很多。
-   `-N`：不打印出host 的域名部分.。比如,，如果设置了此选现，tcpdump 将会打印'nic' 而不是 'nic.ddn.mil'.

## 6.2 文件
### 6.2.1 过滤结果输出到文件(-w)
使用 tcpdump 工具抓到包后，往往需要再借助其他的工具进行分析，比如常见的 wireshark 。
而要使用wireshark ，我们得将 tcpdump 抓到的包数据生成到文件中，最后再使用 wireshark 打开它即可。
使用 -w 参数后接一个以 .pcap 后缀命令的文件名，就可以将 tcpdump 抓到的数据保存到文件中。
    $ tcpdump icmp -w icmp.pcap

#### 6.2.1.1 -C：file-size
-C：file-size，tcpdump 在把原始数据包直接保存到文件中之前, 检查此文件大小是否超过file-size. 如果超过了, 将关闭此文件,另创一个文件继续用于原始数据包的记录.
新创建的文件名与-w 选项指定的文件名一致, 但文件名后多了一个数字.  该数字会从1开始随着新创建文件的增多而增加. 
file-size的单位是百万字节(nt: 这里指1,000,000个字节,并非1,048,576个字节, 后者是以1024字节为1k, 1024k字节为1M计算所得, 即1M=1024 ＊ 1024 ＝ 1,048,576)

例子: 
当抓取大量数据并写入文件时，可以自动切割为多个大小相同的文件。例如，下面的命令表示每 3600 秒创建一个新文件 capture-(hour).pcap，每个文件大小不超过200x1000000 字节：
`$ tcpdump  -w /tmp/capture-%H.pcap -G 3600 -C 200`
这些文件的命名为 capture-{1-24}.pcap，24 小时之后，之前的文件就会被覆盖。


### 6.2.2 从文件中读取包数据(-r)
使用 -w 是写入数据到文件，而使用 -r 是从文件中读取数据。
读取后，我们照样可以使用上述的过滤器语法进行过滤分析。
$ tcpdump icmp -r all.pcap

### 6.2.3 外部文件作为过滤条件表达式的输入 (-F)
-F：使用file 文件作为过滤条件表达式的输入, 此时命令行上的输入将被忽略.

## 6.3 控制详细内容的输出 (-v)
-v：产生详细的输出. 比如包的TTL，id标识，数据包长度，以及IP包的一些选项。同时它还会打开一些附加的包完整性检测，比如对IP或ICMP包头部的校验和。
-vv：产生比-v更详细的输出. 比如NFS回应包中的附加域将会被打印, SMB数据包也会被完全解码。（摘自网络，目前我还未使用过）
-vvv：产生比-vv更详细的输出。比如 telent 时所使用的SB, SE 选项将会被打印, 如果telnet同时使用的是图形界面，其相应的图形选项将会以16进制的方式打印出来（摘自网络，目前我还未使用过）


## 6.4 控制时间的显示 (-t)
-t：在每行的输出中不输出时间, Don't print a timestamp on each dump line.
-tt：在每行的输出中会输出时间戳, Print an unformatted timestamp on each dump line.
-ttt：输出每两行打印的时间间隔(以毫秒为单位), Print a delta (micro-second resolution) between current and previous line on each dump line.
-tttt：在每行打印的时间戳之前添加日期的打印（此种选项，输出的时间最直观), Print a timestamp in default format proceeded by date on each dump line.
-ttttt: # Print a delta (micro-second resolution) between current and first line on each dump line.


## 6.5 不同进制显示数据包的头部 (-x, -A)
-A：以ASCII码方式显示每一个数据包. 
-X 表示同时使用十六进制和 ASCII 字符串打印报文的全部数据。这两个参数不能一起使用。

1 以16 进制显示数据
-x：以16进制的形式打印每个包的头部数据（但不包括数据链路层的头部）
-xx：以16进制的形式打印每个包的头部数据（包括数据链路层的头部）
-X：以16进制和 ASCII码形式打印出每个包的数据(但不包括连接层的头部)，这在分析一些新协议的数据包很方便。
-XX：以16进制和 ASCII码形式打印出每个包的数据(包括连接层的头部)，这在分析一些新协议的数据包很方便。

2 以ASCII码
-A：以ASCII码方式显示每一个数据包(不显示链路层头部信息). 在抓取包含网页数据的数据包时, 可方便查看数据

## 6.6 过滤指定网口的数据包 (-i)
-i：指定要过滤的网卡接口，如果要查看所有网卡接口，可以 -i any

通常是以太网卡或无线网卡，也可以是 vlan 或其他特殊接口。
如果该系统上只有一个网络接口，则无需指定

任意网口
tcpdump -i any

多个网口
tcpdump -i <interface_1> -i <interface_2>

## 6.7 过滤特定流向的数据包 (-Q)
-Q： 选择是入方向还是出方向的数据包，可选项有：in, out, inout，也可以使用 --direction=`[direction] `这种写法

## 6.8 不让网络接口进入混杂模式 (-p)
-p : 不让网络接口进入混杂模式。默认情况下使用 tcpdump 抓包时，会让网络接口进入混杂模式。
如果设备接入的交换机开启了混杂模式，使用 -p 选项可以有效地过滤噪声

一般计算机网卡都工作在非混杂模式下，此时网卡只接受来自网络端口的目的地址指向自己的数据。
当网卡工作在混杂模式下时，网卡将来自接口的所有数据都捕获并交给相应的驱动程序。


## 6.9 行缓冲模式 (-l ), 数据包缓冲模式 (-c, )
如果想实时将抓取到的数据通过管道传递给其他工具来处理，需要使用 -l 选项来开启行缓冲模式（或使用 -c 选项来开启数据包缓冲模式）。

1 
使用 -l 选项可以将输出通过立即发送给其他命令，其他命令会立即响应。
 -l : 基于行的输出，便于你保存查看，或者交给其它工具分析
$ tcpdump -i eth0 -s0 -l port 80 | grep 'Server:'

2 
-c : 捕获 count 个包 tcpdump 就退出
`-c` 选项用来限制抓取数据的大小。
如果不限制大小，就只能通过 `ctrl-c` 来停止抓取，这样一来不仅关闭了 tcpdump，也关闭了 wireshark。

## 6.10 想要截取的 报文的长度: (-s)
-s : tcpdump 默认只会截取前 96 字节的内容，要想截取所有的报文内容，可以使用 -s number， number 就是你要截取的报文字节数，如果是 0 的话，表示截取报文全部内容。

## 6.11 其他常用的一些参数

-q : 简洁地打印输出。即打印很少的协议相关信息, 从而输出行都比较简短.
-S : 使用绝对序列号，而不是相对序列号, TCP sequence numbers


## 6.12 对输出内容进行控制的参数
-D : 显示所有可用网络接口的列表
-e : 每行的打印输出中将包括数据包的数据链路层头部信息. 显示数据链路层信息。默认情况下 tcpdump 不会显示数据链路层信息，使用 -e 选项可以显示源和目的 MAC 地址，以及 VLAN tag 信息。
-E : 揭秘IPSEC数据, 显示 mac 地址。需要指定网口
-L ：列出指定网络接口所支持的数据链路层的类型后退出
-Z：后接用户名，在抓包时会受到权限的限制。如果以root用户启动tcpdump，tcpdump将会有超级用户权限。
-d：打印出易读的包匹配码
-dd：以C语言的形式打印出包匹配码.
-ddd：以十进制数的形式打印出包匹配码


# 7 过滤规则组合

## 7.1 and or not
有编程基础的同学，对于下面三个逻辑运算符应该不陌生了吧
and：所有的条件都需要满足，也可以表示为 &&
or：只要有一个条件满足就可以，也可以表示为 ||
not：取反，也可以使用 !

1 
举个例子，我想需要抓一个来自10.5.2.3，发往任意主机的3389端口的包
$ tcpdump src 10.5.2.3 and dst port 3389

2 
当你在使用多个过滤器进行组合时，有可能需要用到括号，而括号在 shell 中是特殊符号，因为你需要使用引号将其包含。
例子如下：
$ tcpdump 'src 10.0.2.4 and (dst port 3389 or 22)'

## 7.2 相等和不相等
而在单个过滤器里，常常会判断一条件是否成立，这时候，就要使用下面三个符号
=：判断二者相等
`==` 判断二者相等
!=：判断二者不相等


当你使用这三个符号时，tcpdump 还提供了一些关键字的接口来方便我们进行判断，比如
if：表示网卡接口名、
proc：表示进程名
pid：表示进程 id
svc：表示 service class
dir：表示方向，in 和 out
eproc：表示 effective process name
epid：表示 effective process ID

比如我现在要过滤来自进程名为 nc 发出的流经 en0 网卡的数据包，或者不流经 en0 的入方向数据包，可以这样子写
$ tcpdump "( if=en0 and proc =nc ) || (if != en0 and dir=in)"


# 8 特殊过滤规则

## 8.1 根据 tcpflags 进行过滤
 tcp 的首部有一个标志位。

![TCP 报文首部](http://image.iswbm.com/20200606095627.png)

![](http://image.iswbm.com/20200629085659.png)

tcpdump 支持我们根据数据包的标志位进行过滤
proto [ expr :size ]
- proto：可以是熟知的协议之一（如ip，arp，tcp，udp，icmp，ipv6）
- expr：可以是数值，也可以是一个表达式，表示与指定的协议头开始处的字节偏移量。有我们熟知的直接偏移量如tcpflags，也有取值常量如tcp-syn，tcp-ack或者tcp-fin。
- size：是可选的， 不是一定要写 表示从字节偏移量开始取的字节数量。

例子
` tcpdump -i eth0 'tcp[13] == 2'`   等效于 `tcpdump -i eth0 tcp[tcpflags] == tcp-syn`
-  proto 的值: tcp
- expr 的值: tcpflags 或者 13   

### 8.1.1 tcp协议中的别名常量 

1 
tcpflags 可以理解为是一个别名常量，相当于 13，它代表着与指定的协议头开头相关的字节偏移量，也就是标志位，所以 tcp[tcpflags] 等价于 tcp[13] ，


对应下图中的报文位置。
<mark>从 整个 tcp报文的最开始 (源端口的最左处)计算,  经过 首部长度, 再到保留位 (其实只有两位), 一共有 13byte. Tag 其实占8bits : CEUAPRSF </mark>

![](http://image.iswbm.com/20200628222034.png)

2 
tcp-fin, tcp-syn, tcp-rst, tcp-push, tcp-ack, tcp-urg 这些同样可以理解为别名常量，分别代表 1，2，4，8，16，32，64。
这些数字是如何计算出来的呢？

以 tcp-syn 为例，你可以参照下面这张图，计算出来的值 是就是 2
由于数字不好记忆，所以一般使用这样的“别名常量”表示。
![](http://image.iswbm.com/20200628222010.png)



3 
因此当下面这个表达式成立时，就代表这个包是一个 syn 包。
`tcp[tcpflags] == tcp-syn`


### 8.1.2 icmp 协议中的别名常量  
tcp 中有 类似 tcp-syn 的别名常量，其他协议也是有的，比如 icmp 协议，可以使用的别名常量有
`icmp-echoreply, icmp-unreach, icmp-sourcequench,  icmp-redirect, icmp-echo, icmp-routeradvert, icmp-routersolicit, icmp-timx-ceed, icmp-paramprob,  icmp-tstamp, icmp-tstampreply,icmp-ireq,  icmp-ireqreply, icmp-maskreq, icmp-maskreply`


### 8.1.3 抓取某个特定数据包

下面以最常见的 syn包为例，演示一下如何用 tcpdump 抓取到 syn 包


1、第一种写法：使用数字表示偏移量
$ tcpdump -i eth0 "tcp[13] & 2 != 0" 
https://www.runoob.com/w3cnote/bit-operation.html
位运算  x & y: 会将 两边都转化为 二进制吗码, 然后进行 每一个位的对比. 这一位上如果必须都为1 , 则这一位的输出结果为1. 
0&0=0  0&1=0  1&0=0  1&1=1

2、第二种写法：使用别名常量表示偏移量
$ tcpdump -i eth0 "tcp[tcpflags] & tcp-syn != 0" 

3、第三种写法：使用混合写法
$ tcpdump -i eth0 "tcp[tcpflags] & 2 != 0" 
or
$ tcpdump -i eth0 "tcp[13] & tcp-syn != 0" 


### 8.1.4 同时捕获多种类型的包

1、第一种写法
`$ tcpdump -i eth0 'tcp[13] == 2 or tcp[13] == 16'`

2、第二种写法
`$ tcpdump -i eth0 'tcp[tcpflags] == tcp-syn or tcp[tcpflags] == tcp-ack'`

3、第三种写法
`$ tcpdump -i eth0 "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"` 

4、第四种写法：注意这里是 单个等号，而不是像上面一样两个等号，18（syn+ack） = 2（syn） + 16（ack）
`$ tcpdump -i eth0 'tcp[13] = 18' # or $ tcpdump -i eth0 'tcp[tcpflags] = 18'`

### 8.1.5 其他例子
```sh
显示只有`SYN`的报文：
tcpdump -i <interface> "tcp[tcpflags]==tcp-syn)"

显示只有`SYN+ACK`的报文：
tcpdump -i <interface> "tcp[tcpflags]==(tcp-syn|tcp-ack)"

显示只有`ACK`的报文：
tcpdump -i <interface> "tcp[tcpflags]==tcp-ack)"

显示只有`FIN`的报文：
tcpdump -i <interface> "tcp[tcpflags]==tcp-fin)"

显示只有`FIN+ACK`的报文：
tcpdump -i <interface> "tcp[tcpflags]==(tcp-fin|tcp-ack)"

显示只有`RST`的报文：
tcpdump -i <interface> "tcp[tcpflags]==(tcp-rst)"

显示包含`SYN`的报文：
tcpdump -i <interface> "tcp[tcpflags] & (tcp-syn) !=0"

显示包含有`FIN`的报文：
tcpdump -i <interface> "tcp[tcpflags] & (tcp-fin) !=0"

显示包含有`PUSH`的报文：
tcpdump -i <interface> "tcp[tcpflags] & (tcp-push) !=0"

显示包含有`ACK`的报文：
tcpdump -i <interface> "tcp[tcpflags] & (tcp-ack) !=0"
```



### 8.1.6 详解 
```sh
$ tcpdump -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4]'
```

命令里的可选参数，在前面的内容里已经详细讲过了。这里不再细讲。
本节的重点是引号里的内容，看起来很复杂的样子。

将它逐一分解，我们只要先理解了下面几种用法，就能明白

-   `tcp[n]`：表示 tcp 报文里 第 n 个字节
    
-   `tcp[n:c]`：表示 tcp 报文里从第n个字节开始取 c 个字节，tcp[12:1] 表示从报文的第12个字节（因为有第0个字节，所以这里的12其实表示的是13）开始算起取一个字节，也就是 8 个bit。查看 [tcp 的报文首部结构](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_segment_structure)，可以得知这 8 个bit 其实就是下图中的红框圈起来的位置，而在这里我们只要前面 4个bit，也就是实际数据在整个报文首部中的偏移量。
    
    ![](http://image.iswbm.com/20200629085659.png)
    
-   `&`：是[位运算](https://en.wikipedia.org/wiki/Bitwise_operation)里的 and 操作符，比如 `0011 & 0010 = 0010`
    
-   `>>`：是位运算里的右移操作，比如 `0111 >> 2 = 0001`
    
-   `0xf0`：是 10 进制的 240 的 16 进制表示，但对于位操作来说，10进制和16进制都将毫无意义，我们需要的是二进制，将其转换成二进制后是：11110000，这个数有什么特点呢？前面个 4bit 全部是 1，后面4个bit全部是0，往后看你就知道这个特点有什么用了。
    

分解完后，再慢慢合并起来看

1. `tcp[12:1] & 0xf0` 其实并不直观，但是我们将它换一种写法，就好看多了，假设 tcp 报文中的 第12 个字节是这样组成的 `10110000`，那么这个表达式就可以变成 10110110 && 11110000 = 10110000，得到了 10110000 后，再进入下一步。

2. `tcp[12:1] & 0xf0) >> 2` ：如果你不理解 tcp 报文首部里的数据偏移，请先点击这个前往我的[上一篇文章](https://mp.weixin.qq.com/s?__biz=MzIzMzMzOTI3Nw==&mid=2247488180&idx=1&sn=09526224732ebfcccb52847f27298c70&chksm=e8867256dff1fb40c9f47bafd0e87a9237c5a9ebf33c8a3d0a598276b496d29cdaa3fbff8d26&token=1970357830&lang=zh_CN#rd)，搞懂数据偏移的意义，否则我保证你这里会绝对会听懵了
    1. `tcp[12:1] & 0xf0) >> 2` 这个表达式实际是 `(tcp[12:1] & 0xf0) >> 4 ) << 2` 的简写形式。所以要搞懂 `tcp[12:1] & 0xf0) >> 2` 只要理解了`(tcp[12:1] & 0xf0) >> 4 ) << 2` 就行了 。
3. 从上一步我们算出了 `tcp[12:1] & 0xf0` 的值其实是一个字节，也就是 8 个bit，但是你再回去看下上面的 tcp 报文首部结构图，表示数据偏移量的只有 4个bit，也就是说 上面得到的值 10110000，前面 4 位（1011）才是正确的偏移量，那么为了得到 1011，只需要将 10110000 右移4位即可，也就是 `tcp[12:1] & 0xf0) >> 4`
4. 至此我们是不是已经得出了实际数据的正确位置呢，很遗憾还没有，前一篇文章里我们讲到 Data Offset 的单位是 4个字节，因为要将 1011 乘以 4才可以，除以4在位运算中相当于左移2位，也就是 `<<2`，与前面的 `>>4` 结合起来一起算的话，最终的运算可以简化为 `>>2`。

至此，我们终于得出了实际数据开始的位置是 `tcp[12:1] & 0xf0) >> 2` （单位是字节）。

找到了数据的起点后，可别忘了我们的目的是从数据中拿到 HTTP 请求的方法，是 GET 呢 还是 POST ，或者是其他的？

有了上面的经验，我们自然懂得使用 `tcp[((tcp[12:1] & 0xf0) >> 2):4]` 从数据开始的位置再取出四个字节，然后将结果与 `GET` （注意 GET最后还有个空格）的 16进制写法（也就是 `0x47455420`）进行比对。

0x47   -->   71    -->  G, 
0x45   -->   69    -->  E 
0x54   -->   84    -->  T
0x20   -->   32    -->  空格
如果相等，则该表达式为True，tcpdump 认为这就是我们所需要抓的数据包，将其输出到我们的终端屏幕上。

![](http://image.iswbm.com/20200629130407.png)


## 8.2 基于包大小进行过滤
若你想查看指定大小的数据包，也是可以的

$ tcpdump less 32 
$ tcpdump greater 64 
$ tcpdump <= 128

## 8.3 根据 mac 地址进行过滤
例子如下，其中 ehost 是记录在 /etc/ethers 里的 name
$ tcpdump ether host [ehost]
$ tcpdump ether dst	[ehost]
$ tcpdump ether src	[ehost]

## 8.4 过滤通过指定网关的数据包 (gateway)
$ tcpdump gateway [host]

## 8.5 过滤广播/多播数据包
$ tcpdump ether broadcast
$ tcpdump ether multicast

$ tcpdump ip broadcast
$ tcpdump ip multicast

$ tcpdump ip6 multicast




