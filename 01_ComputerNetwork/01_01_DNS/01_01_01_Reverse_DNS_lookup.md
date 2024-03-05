
https://www.mdnice.com/writing/ca0aa5180e9d495e816a4f8f65a31f54


# 1 什么是主机名

主机名是IP地址后面的域
192.100.1.0.ivu-cloud.local 

# 2 什么是反向 DNS

DNS 的用途通常是将域名解析为 IP 地址。这被称为正向解析，我们每次访问互联网上的站点时都会执行此操作。
顾名思义，反向 DNS（或rDNS）是一种将 IP 地址解析为域名的方法。


Normal forward 
正向DNS 将主机名映射到IP 地址
查询 DNS 系统来返回 IP 地址。
DNS解析将“Hostname”解析为“IP Address”

Reverse forward
主机名就是是IP地址后面的域
服务器IP 地址映射回主机名
对给定 IP 地址所关联的域名的 DNS 查询。
找到给定IP所解析的主机名。


# 3 反向 DNS 的 use case 

1
rDNS主要用于确保邮件交换记录的有效性，用于拦截邮件服务系统中的垃圾邮件。

电子邮件服务器通常使用反向查找。电子邮件服务器会检查并查看电子邮件消息是否来自有效服务器，然后再将其带到网络中。许多电子邮件服务器会拒绝来自任何不支持反向查找的服务器或来自不太可能是合法的服务器的邮件。垃圾邮件发送者经常使用来自被劫持机器的 IP 地址，这意味着不会有 PTR 记录。或者，他们可能使用动态分配的 IP 地址，这些地址会导致具有高度通用名称的服务器域。

一般情况下，垃圾邮件发送者使用动态分配的 IP 地址或者没有注册域名的 IP 地址来发送垃圾邮件，通过反向解析可以判断邮件的合法性。当邮件服务器收到邮件时，邮件服务器会查看邮件由哪个 IP 地址发出，然后根据这个 IP 地址进行反向解析，如果反向解析得到的域名与发送方邮件的域名不一致则认为邮件发送者不是从真正的邮件服务器发出，则可以拒绝接收此邮件。
比如当 me@qq.com 收到一份来自 fake@163.com 的邮件时，qq邮件服务器会查看邮件来源的 IP，根据 IP 进行反向解析，如果解析到的域名和 163.com 一致，则接收邮件，否则认为邮件来源 IP 伪造成了163服务器 IP ，则拒绝这封邮件。


2
尽管 rDNS 记录可以阻止垃圾邮件，但主要用作额外保护，它不是一种可靠的方法。需要注意的是，仅启用 rDNS 仍可能由于各种原因导致消息被拒绝。此外，rDNS 还用于分析和日志记录，帮助提供人们可读的数据，而不是完全由 IP 地址组成的日志。

日志软件还使用反向查找，以便在用户的日志数据中为用户提供人类可读的域，而不是一堆数字 IP 地址。

----


**Anti-spam:** Some email anti-spam filters use reverse DNS to check the domain names of email addresses and see if the associated IP addresses are likely to be used by legitimate email servers.

**Troubleshooting email delivery issues:** Because anti-spam filters perform these checks, email delivery problems can result from a misconfigured or missing PTR record. If a domain has no PTR record, or if the PTR record contains the wrong domain, email services may block all emails from that domain.

**Logging:** System logs typically record only IP addresses; a reverse DNS lookup can convert these into domain names for logs that are more human-readable.



# 4 反向 DNS 查找过程

由于正向 DNS 将主机名映射到 IP 地址，因此 rDNS（或反向 DNS）表明将服务器 IP 地址映射回主机名。
使用 rDNS，将 IP 地址反转，然后将in-addr.arpa添加到末尾。
例如，如果我们使用 IPv4 地址 142.250.192.196，使用 rDNS，它将变为 196.192.250.142.in-addr.arpa
这种 IP 地址进行反向 DNS 解析的方法用到的是 PTR 记录。如果域有 PTR 记录，我们可以使用下面提到的方法进行 rDNS 查找。

反向 DNS 查询向 DNS 服务器查询 PTR（指针）记录； 如果服务器没有 PTR 记录，则无法解析反向查找。PTR 记录存储 IP 地址，它们的段颠倒过来，并在其上附加“.in-addr.arpa”。例如，如果域的 IP 地址为 192.0.2.1，则 PTR 记录将在 1.2.0.192.in-addr.arpa 下存储域的信息。 长成这样 ： 1.2.0.192.in-addr.arpa  -> ivu-cloud.local 

在最新版本的互联网协议 IPv6 中，PTR 记录存储在“.ip6.arpa”域中，而不是“.in-addr.arpa”域中。


![](image/Pasted%20image%2020240305110222.png)

# 5 执行 rDNS 查找

许多在线工具可用于执行 rDNS 查找，例如：

    MXToolbox.com
    Whatismyip.com
    IPLocation.net

还可以从命令行手动执行 rDNS 查找。在 Linux 中，使用dig命令加-x参数。Windows中使用nslookup命令，也可以使用ping -a。


## 5.1 dig 

Linux中示例如下：

dig -x 114.114.114.114

输出：
```
; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el8 <<>> -x 114.114.114.114
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 22576
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 13, ADDITIONAL: 17

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 33d2a6a18768f09113da3aff61e97842fc1359ae9077046b (good)
;; QUESTION SECTION:
;114.114.114.114.in-addr.arpa.  IN      PTR

;; ANSWER SECTION:
114.114.114.114.in-addr.arpa. 388 IN    PTR     public1.114dns.com.

;; AUTHORITY SECTION:
.                       59433   IN      NS      k.root-servers.net.
.                       59433   IN      NS      d.root-servers.net.
.                       59433   IN      NS      f.root-servers.net.
.                       59433   IN      NS      b.root-servers.net.
.                       59433   IN      NS      g.root-servers.net.
.                       59433   IN      NS      i.root-servers.net.
.                       59433   IN      NS      m.root-servers.net.
.                       59433   IN      NS      c.root-servers.net.
.                       59433   IN      NS      a.root-servers.net.
.                       59433   IN      NS      j.root-servers.net.
.                       59433   IN      NS      l.root-servers.net.
.                       59433   IN      NS      e.root-servers.net.
.                       59433   IN      NS      h.root-servers.net.

;; ADDITIONAL SECTION:
b.root-servers.net.     602786  IN      AAAA    2001:500:200::b
f.root-servers.net.     602843  IN      AAAA    2001:500:2f::f
g.root-servers.net.     76912   IN      AAAA    2001:500:12::d0d
i.root-servers.net.     256494  IN      AAAA    2001:7fe::53
k.root-servers.net.     257105  IN      AAAA    2001:7fd::1
l.root-servers.net.     440012  IN      AAAA    2001:500:9f::42
m.root-servers.net.     77161   IN      AAAA    2001:dc3::35
b.root-servers.net.     256829  IN      A       199.9.14.201
d.root-servers.net.     87824   IN      A       199.7.91.13
f.root-servers.net.     602843  IN      A       192.5.5.241
g.root-servers.net.     76966   IN      A       192.112.36.4
h.root-servers.net.     406981  IN      A       198.97.190.53
i.root-servers.net.     602628  IN      A       192.36.148.17
k.root-servers.net.     67413   IN      A       193.0.14.129
l.root-servers.net.     440012  IN      A       199.7.83.42
m.root-servers.net.     602928  IN      A       202.12.27.33

;; Query time: 83 msec
;; SERVER: 10.8.255.1#53(10.8.255.1)
;; WHEN: Thu Jan 20 22:57:06 HKT 2022
;; MSG SIZE  rcvd: 668

```


可以在ANSWER SECTION看到完整的rDNS PTR记录的IP，返回子域名public1.114dns.com.:

114.114.114.114.in-addr.arpa. 388 IN    PTR     public1.114dns.com.


# 6 如何配置反向DNS记录？

如果是自己搭建的DNS服务器，可以参考其他文章做此配置。

这里用ucloud域名提供商举例：
假如需要使用反向DNS解析Zone的VPC为10.9.0.0/16，那么需要创建的Zone名为： 9.10.in-addr.arpa
假如需要添加10.9.2.1，对应的域名为www.kubeinfo.cn ，那么需要添加PTR主机记录： 1.2.9.10.in-addr.arpa,  对应值为 www.kubeinfo.cn 
即可配置生效。


