# 1 1

## 1.1 提取 HTTP 的 User-Agent

从 HTTP 请求头中提取 HTTP 的 User-Agent：
`$ tcpdump -nn -A -s1500 -l | grep "User-Agent:"`

通过 `egrep` 可以同时提取User-Agent 和主机名（或其他头文件）：
`$ tcpdump -nn -A -s1500 -l | egrep -i 'User-Agent:|Host:'`

## 1.2 抓取 HTTP 有效数据包

抓取 80 端口的 HTTP 有效数据包，排除 TCP 连接建立过程的数据包（SYN / FIN / ACK）：
`$ tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'`

## 1.3 抓取 HTTP GET 和 POST 请求

抓取 HTTP GET 请求包：
`$ tcpdump -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x47455420' ` 或者 
`$ tcpdump -vvAls0 | grep 'GET'`

可以抓取 HTTP POST 请求包：
注意：该方法不能保证抓取到 HTTP POST 有效数据流量，因为一个 POST 请求会被分割为多个 TCP 数据包。
```sh
$ tcpdump -s 0 -A -vv 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504f5354' 
$ tcpdump -vvAls0 | grep 'POST'
```

上述两个表达式中的十六进制将会与 GET 和 POST 请求的 ASCII 字符串匹配。例如，`tcp[((tcp[12:1] & 0xf0) >> 2):4]` 首先会确定我们感兴趣的字节的位置（在 TCP header 之后），然后选择我们希望匹配的 4 个字节。




## 1.4 提取 HTTP POST 请求中的密码

从 HTTP POST 请求中提取密码和主机名：
`$ tcpdump -s 0 -A -n -l | egrep -i "POST /|pwd=|passwd=|password=|Host:"`

```
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp7s0, link-type EN10MB (Ethernet), capture size 262144 bytes
11:25:54.799014 IP 10.10.1.30.39224 > 10.10.1.125.80: Flags [P.], seq 1458768667:1458770008, ack 2440130792, win 704, options [nop,nop,TS val 461552632 ecr 208900561], length 1341: HTTP: POST /wp-login.php HTTP/1.1
.....s..POST /wp-login.php HTTP/1.1
Host: dev.example.com
.....s..log=admin&pwd=notmypassword&wp-submit=Log+In&redirect_to=http%3A%2F%2Fdev.example.com%2Fwp-admin%2F&testcookie=1
```

## 1.5 提取 HTTP 请求的 URL

提取 HTTP 请求的主机名和路径：
`$ tcpdump -s 0 -v -n -l | egrep -i "POST /|GET /|Host:"`

```sh
tcpdump: listening on enp7s0, link-type EN10MB (Ethernet), capture size 262144 bytes
    POST /wp-login.php HTTP/1.1
    Host: dev.example.com
    GET /wp-login.php HTTP/1.1
    Host: dev.example.com
    GET /favicon.ico HTTP/1.1
    Host: dev.example.com
    GET / HTTP/1.1
    Host: dev.example.com
```



## 1.6 抓取 ICMP 数据包
查看网络上的所有 ICMP 数据包：
$ tcpdump -n icmp

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp7s0, link-type EN10MB (Ethernet), capture size 262144 bytes
11:34:21.590380 IP 10.10.1.217 > 10.10.1.30: ICMP echo request, id 27948, seq 1, length 64
11:34:21.590434 IP 10.10.1.30 > 10.10.1.217: ICMP echo reply, id 27948, seq 1, length 64
11:34:27.680307 IP 10.10.1.159 > 10.10.1.1: ICMP 10.10.1.189 udp port 59619 unreachable, length 115

## 1.7 抓取非 ECHO/REPLY 类型的 ICMP 数据包

通过排除 echo 和 reply 类型的数据包使抓取到的数据包不包括标准的 ping 包：
$ tcpdump 'icmp[icmptype] != icmp-echo and icmp[icmptype] != icmp-echoreply'

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp7s0, link-type EN10MB (Ethernet), capture size 262144 bytes
11:37:04.041037 IP 10.10.1.189 > 10.10.1.20: ICMP 10.10.1.189 udp port 36078 unreachable, length 156


## 1.8 抓取 DHCP 报文

最后一个例子，抓取 DHCP 服务的请求和响应报文，67 为 DHCP 端口，68 为客户机端口。
$ tcpdump -v -n port 67 or 68

```sh
tcpdump: listening on enp7s0, link-type EN10MB (Ethernet), capture size 262144 bytes
14:37:50.059662 IP (tos 0x10, ttl 128, id 0, offset 0, flags [none], proto UDP (17), length 328)
    0.0.0.0.68 > 255.255.255.255.67: BOOTP/DHCP, Request from 00:0c:xx:xx:xx:d5, length 300, xid 0xc9779c2a, Flags [none]
      Client-Ethernet-Address 00:0c:xx:xx:xx:d5
      Vendor-rfc1048 Extensions
        Magic Cookie 0x63825363
        DHCP-Message Option 53, length 1: Request
        Requested-IP Option 50, length 4: 10.10.1.163
        Hostname Option 12, length 14: "test-ubuntu"
        Parameter-Request Option 55, length 16: 
          Subnet-Mask, BR, Time-Zone, Default-Gateway
          Domain-Name, Domain-Name-Server, Option 119, Hostname
          Netbios-Name-Server, Netbios-Scope, MTU, Classless-Static-Route
          NTP, Classless-Static-Route-Microsoft, Static-Route, Option 252
14:37:50.059667 IP (tos 0x10, ttl 128, id 0, offset 0, flags [none], proto UDP (17), length 328)
    0.0.0.0.68 > 255.255.255.255.67: BOOTP/DHCP, Request from 00:0c:xx:xx:xx:d5, length 300, xid 0xc9779c2a, Flags [none]
      Client-Ethernet-Address 00:0c:xx:xx:xx:d5
      Vendor-rfc1048 Extensions
        Magic Cookie 0x63825363
        DHCP-Message Option 53, length 1: Request
        Requested-IP Option 50, length 4: 10.10.1.163
        Hostname Option 12, length 14: "test-ubuntu"
        Parameter-Request Option 55, length 16: 
          Subnet-Mask, BR, Time-Zone, Default-Gateway
          Domain-Name, Domain-Name-Server, Option 119, Hostname
          Netbios-Name-Server, Netbios-Scope, MTU, Classless-Static-Route
          NTP, Classless-Static-Route-Microsoft, Static-Route, Option 252
14:37:50.060780 IP (tos 0x0, ttl 64, id 53564, offset 0, flags [none], proto UDP (17), length 339)
    10.10.1.1.67 > 10.10.1.163.68: BOOTP/DHCP, Reply, length 311, xid 0xc9779c2a, Flags [none]
      Your-IP 10.10.1.163
      Server-IP 10.10.1.1
      Client-Ethernet-Address 00:0c:xx:xx:xx:d5
      Vendor-rfc1048 Extensions
        Magic Cookie 0x63825363
        DHCP-Message Option 53, length 1: ACK
        Server-ID Option 54, length 4: 10.10.1.1
        Lease-Time Option 51, length 4: 86400
        RN Option 58, length 4: 43200
        RB Option 59, length 4: 75600
        Subnet-Mask Option 1, length 4: 255.255.255.0
        BR Option 28, length 4: 10.10.1.255
        Domain-Name-Server Option 6, length 4: 10.10.1.1
        Hostname Option 12, length 14: "test-ubuntu"
        T252 Option 252, length 1: 10
        Default-Gateway Option 3, length 4: 10.10.1.1

```

# 2 #
## 2.1 提取 Cookies

提取 Set-Cookie（服务端的 Cookie）和 Cookie（客户端的 Cookie）：
$ tcpdump -nn -A -s0 -l | egrep -i 'Set-Cookie|Host:|Cookie:'

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on wlp58s0, link-type EN10MB (Ethernet), capture size 262144 bytes
Host: dev.example.com
Cookie: wordpress_86be02xxxxxxxxxxxxxxxxxxxc43=admin%7C152xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxfb3e15c744fdd6; _ga=GA1.2.21343434343421934; _gid=GA1.2.927343434349426; wordpress_test_cookie=WP+Cookie+check; wordpress_logged_in_86be654654645645645654645653fc43=admin%7C15275102testtesttesttestab7a61e; wp-settings-time-1=1527337439复制代码

## 2.2 抓取 DNS 请求和响应

DNS 的默认端口是 53，因此可以通过端口进行过滤
`$ tcpdump -i any -s0 port 53`

向 Google 公共 DNS 发起的出站 DNS 请求和 A 记录响应可以通过 tcpdump 抓取到：
$ tcpdump -i wlp58s0 -s0 port 53

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on wlp58s0, link-type EN10MB (Ethernet), capture size 262144 bytes
14:19:06.879799 IP test.53852 > google-public-dns-a.google.com.domain: 26977+ [1au] A? play.google.com. (44)
14:19:07.022618 IP google-public-dns-a.google.com.domain > test.53852: 26977 1/0/1 A 216.58.203.110 (60)

## 2.3 抓取 SMTP/POP3 协议的邮件
可以提取电子邮件的正文和其他数据。例如，只提取电子邮件的收件人：
$ tcpdump -nn -l port 25 | grep -i 'MAIL FROM\|RCPT TO'复制代码
抓取 NTP 服务的查询和响应
$ tcpdump dst port 123

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
21:02:19.112502 IP test33.ntp > 199.30.140.74.ntp: NTPv4, Client, length 48
21:02:19.113888 IP 216.239.35.0.ntp > test33.ntp: NTPv4, Server, length 48
21:02:20.150347 IP test33.ntp > 216.239.35.0.ntp: NTPv4, Client, length 48
21:02:20.150991 IP 216.239.35.0.ntp > test33.ntp: NTPv4, Server, length 48复制代码

## 2.4 抓取 SNMP 服务的查询和响应
通过 SNMP 服务，渗透测试人员可以获取大量的设备和系统信息。在这些信息中，系统信息最为关键，如操作系统版本、内核版本等。使用 SNMP 协议快速扫描程序 onesixtyone，可以看到目标系统的信息：
$ onesixtyone 10.10.1.10 public

Scanning 1 hosts, 1 communities
10.10.1.10 [public] Linux test33 4.15.0-20-generic #21-Ubuntu SMP Tue Apr 24 06:16:15 UTC 2018 x86_64复制代码
可以通过 tcpdump 抓取 GetRequest 和 GetResponse：
$ tcpdump -n -s0  port 161 and udp
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on wlp58s0, link-type EN10MB (Ethernet), capture size 262144 bytes
23:39:13.725522 IP 10.10.1.159.36826 > 10.10.1.20.161:  GetRequest(28)  .1.3.6.1.2.1.1.1.0
23:39:13.728789 IP 10.10.1.20.161 > 10.10.1.159.36826:  GetResponse(109)  .1.3.6.1.2.1.1.1.0="Linux testmachine 4.15.0-20-generic #21-Ubuntu SMP Tue Apr 24 06:16:15 UTC 2018 x86_64"

## 2.5 抓取用户名和密码

本例将重点放在标准纯文本协议上，过滤出于用户名和密码相关的报文：
$ tcpdump port http or port ftp or port smtp or port imap or port pop3 or port telnet -l -A | egrep -i -B5 'pass=|pwd=|log=|login=|user=|username=|pw=|passw=|passwd=|password=|pass:|user:|username:|password:|login:|pass |user '复制代码



# 3 
## 3.1 找出发包数最多的 IP

找出一段时间内发包最多的 IP，或者从一堆报文中找出发包最多的 IP，可以使用下面的命令：

`$ tcpdump -nnn -t -c 200 | cut -f 1,2,3,4 -d '.' | sort | uniq -c | sort -nr | head -n 20`

```sh
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp7s0, link-type EN10MB (Ethernet), capture size 262144 bytes
200 packets captured
261 packets received by filter
0 packets dropped by kernel
    108 IP 10.10.211.181
     91 IP 10.10.1.30
      1 IP 10.10.1.50
```

-   **cut -f 1,2,3,4 -d '.'** : 以 `.` 为分隔符，打印出每行的前四列。即 IP 地址。
-   **sort | uniq -c** : 排序并计数
-   **sort -nr** : 按照数值大小逆向排序

## 3.2 抓取 IPv6 流量
可以通过过滤器 ip6 来抓取 IPv6 流量，同时可以指定协议如 TCP：
$ tcpdump -nn ip6 proto 6复制代码
从之前保存的文件中读取 IPv6 UDP 数据报文：
$ tcpdump -nr ipv6-test.pcap ip6 proto 17


# 4 
 
## 4.1 切割 pcap 文件

当抓取大量数据并写入文件时，可以自动切割为多个大小相同的文件。
例如，下面的命令表示每 3600 秒创建一个新文件 `capture-(hour).pcap`，每个文件大小不超过 `200*1000000` 字节：

`$ tcpdump  -w /tmp/capture-%H.pcap -G 3600 -C 200`

这些文件的命名为 `capture-{1-24}.pcap`，24 小时之后，之前的文件就会被覆盖。


## 4.2 结合 Wireshark 进行分析

通常 `Wireshark`（或 tshark）比 tcpdump 更容易分析应用层协议。一般的做法是在远程服务器上先使用 `tcpdump` 抓取数据并写入文件，然后再将文件拷贝到本地工作站上用 `Wireshark` 分析。

还有一种更高效的方法，可以通过 ssh 连接将抓取到的数据实时发送给 Wireshark 进行分析。

`-c` 选项用来限制抓取数据的大小。
如果不限制大小，就只能通过 `ctrl-c` 来停止抓取，这样一来不仅关闭了 tcpdump，也关闭了 wireshark。

以 MacOS 系统为例，可以通过 `brew cask install wireshark` 来安装，然后通过下面的命令来分析：
`$ ssh root@remotesystem 'tcpdump -s0 -c 1000 -nn -w - not port 22' | /Applications/Wireshark.app/Contents/MacOS/Wireshark -k -i -`

例如，如果想分析 DNS 协议，可以使用下面的命令：
`$ ssh root@remotesystem 'tcpdump -s0 -c 1000 -nn -w - port 53' | /Applications/Wireshark.app/Contents/MacOS/Wireshark -k -i -`

抓取到的数据：
![](https://hugo-picture.oss-cn-beijing.aliyuncs.com/images/20200210170101.png)

