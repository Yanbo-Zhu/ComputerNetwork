
https://laona.dev/post/glue-record/
https://blog.csdn.net/dranker/article/details/109754755
https://www.cloudns.net/wiki/article/295/
https://serverfault.com/questions/309622/what-is-a-glue-record

胶水记录就是域管理者向上级域管理者提供的一组主机名和IP的映射表
==也就是向 上级域的权威 DNS 服务器服务者  提供的 下级域的DNS 服务器对应的 IP 地址数据。==
# 1 What are Glue Records?

A glue record associates ("glue together") hostname (nameserver, or DNS ) with an IP address at the registry. These DNS records are created at the domain's registrar and provides a complete answer when the TLD nameserver returns a reference for an authoritative nameserver for a domain.

----


A glue record is a term for a record that's served by a DNS server that's not authoritative for the zone, to avoid a condition of impossible dependencies for a DNS zone.

Say I own a DNS zone for `example.com`. I want to have DNS servers that're hosting the authoritative zone for this domain so that I can actually use it - adding records for the root of the domain, `www`, `mail`, etc. So, I put the name servers in the registration to delegate to them - those are always names, so we'll put in `ns1.example.com` and `ns2.example.com`.

There's the trick. The TLD's servers will delegate to the DNS servers in the whois record - but they're within `example.com`. They try to find `ns1.example.com`, ask the `.com` servers, and get referred back to... `ns1.example.com`.

What glue records do is to allow the TLD's servers to send extra information in their response to the query for the `example.com` zone - to send the IP address that's configured for the name servers, too. It's not authoritative, but it's a pointer to the authoritative servers, allowing for the loop to be resolved.

# 2 **How does it work?**

When a [DNS resolver](https://www.cloudns.net/blog/recursive-dns-server/) queries a nameserver for a domain, it needs the IP address of that nameserver to complete the request. However, if the nameserver's information is stored within the same domain, a circular dependency occurs, leading to an infinite loop. Glue records resolve this issue by associating the hostname of the nameserver directly with its IP address at the registry level. This preempts the need for additional DNS lookups, streamlining the resolution process.


# 3 什么是胶水记录

https://blog.csdn.net/dranker/article/details/109754755

在讲这个概念之前，我们先理清一下域名的注册。

1. 1、确定好公司域名，如example.com.cn，并向域名注册商注册
2. 2、自建DNS服务器，可提供example.com.cn域的DNS解析服务，并对公网发布DNS服务，公网地址为111.1.1.1
3. 3、在域名注册商平台上配置 权威DNS (  administrative dns server/ administrative name server,    在注册商那配置 权威dns为ns.example.com.cn )，也就是向外界通告，example.com.cn的域名，应由谁来做解析。应配置为111.1.1.1。
4. 4、在自建的DNS服务器上，配置公司业务域名如www、mail等解析

通过以上4步，公司的域名解析服务即可生效。但第3点有个问题：在配置权威DNS指向时，即向外界通告权威DNS服务器时，只能配置域名，而不能配置111.1.1.1。
如果是使用第三方的解析服务，那这个问题就不存在，域名解析服务商会告诉你，这个地方应该填哪几个域名。

对于自建DNS的公司来讲，这就存在一个鸡生蛋还是蛋生鸡的问题。加入我们在注册商那配置权威为ns.example.com.cn，并且在我们自己DNS服务器上也加一个A记录： ns.example.com.cn 111.1.1.1
貌似就可以了，但仔细一看不然。因为此时此刻我们的DNS服务器还没有正式对外通告提供服务，在公网上也就解析不出来ns.example.com.cn的地址，DNS服务器上有配置A记录也无法生效。但是注册商那又必须配置域名，而域名DNS服务器又无法工作，陷入死循环。

这个时候胶水记录就发挥了作用：<mark> 在注册商那，创建一条胶水记录： ns.example.com.cn 111.1.1.1 </mark>

这样，即使我们自有DNS服务器还未生效，外界也知道ns.example.com.cn指向的地址是111.1.1.1。胶水记录的名字也由此而来：相当于是用胶水把这个关联关系粘起来。



# 4 胶水记录 对比 普通的 DNS 记录 

胶水记录是一种特殊的记录类型，来对比下它和普通的 DNS 记录有什么差别：

普通的 DNS 记录是保存在权威服务器上的
    权威服务器一般由域名注册商提供，当然也有由第三方提供的独立解析服务。
    用户通过域名解析后台设置解析记录，值是保存在权威服务器上的。

胶水记录保存在注册局的DNS服务器上
    注册局（Domain Registry）是管理某个后缀所有域名的机构。
    用户通过域名注册商后台设置胶水记录时，值是保存到注册局的服务器上的。


# 5 通过例子说明 (非常好)

https://laona.dev/post/glue-record/

## 5.1 DNS 查询过程

以查询 jd.com 这个域名的 A 记录为例，使用 dig 命令来分析整个查询过程。
我们会以递归服务器的工作方式来逐级查询 DNS 记录。

### 5.1.1 获取根服务器列表
为什么要根服务器列表？因为所有域名的 DNS 记录查询入口都在这里，就好像你要进入一个多级的目录，你得一级一级地进入，而根服务器就是入口。

全球共有 13 组公认的 DNS 服务器，可在 IANA 网站上查询到：
https://www.iana.org/domains/root/servers

所有递归服务器上都会内置这张表。

### 5.1.2 向根服务器发送查询

老衲随机选取一个根服务器进行查询，这里老衲选了 m.root-servers.net，它的IP地址是 202.12.27.33。

下面是和这台根服务器的对话过程：

“嗨，请问你这有 jd.com 吗？”
“没有，但是你可以找管理 .com 域名的这些服务器问问。哦，对了，他们的 IP 地址我这刚好有，直接给你吧！”

```

dig @202.12.27.33 jd.com

; <<>> DiG 9.9.4-RedHat-9.9.4-61.el7 <<>> @202.12.27.33 jd.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 29264
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 13, ADDITIONAL: 27
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;jd.com.                                IN      A

;; AUTHORITY SECTION:
com.                    172800  IN      NS      d.gtld-servers.net.
com.                    172800  IN      NS      f.gtld-servers.net.
com.                    172800  IN      NS      e.gtld-servers.net.
com.                    172800  IN      NS      g.gtld-servers.net.
com.                    172800  IN      NS      k.gtld-servers.net.
com.                    172800  IN      NS      a.gtld-servers.net.
com.                    172800  IN      NS      c.gtld-servers.net.
com.                    172800  IN      NS      j.gtld-servers.net.
com.                    172800  IN      NS      l.gtld-servers.net.
com.                    172800  IN      NS      b.gtld-servers.net.
com.                    172800  IN      NS      m.gtld-servers.net.
com.                    172800  IN      NS      i.gtld-servers.net.
com.                    172800  IN      NS      h.gtld-servers.net.

;; ADDITIONAL SECTION:
a.gtld-servers.net.     172800  IN      A       192.5.6.30
b.gtld-servers.net.     172800  IN      A       192.33.14.30
c.gtld-servers.net.     172800  IN      A       192.26.92.30
d.gtld-servers.net.     172800  IN      A       192.31.80.30
e.gtld-servers.net.     172800  IN      A       192.12.94.30
f.gtld-servers.net.     172800  IN      A       192.35.51.30
g.gtld-servers.net.     172800  IN      A       192.42.93.30
h.gtld-servers.net.     172800  IN      A       192.54.112.30
i.gtld-servers.net.     172800  IN      A       192.43.172.30
j.gtld-servers.net.     172800  IN      A       192.48.79.30
k.gtld-servers.net.     172800  IN      A       192.52.178.30
l.gtld-servers.net.     172800  IN      A       192.41.162.30
m.gtld-servers.net.     172800  IN      A       192.55.83.30
a.gtld-servers.net.     172800  IN      AAAA    2001:503:a83e::2:30
b.gtld-servers.net.     172800  IN      AAAA    2001:503:231d::2:30
c.gtld-servers.net.     172800  IN      AAAA    2001:503:83eb::30
d.gtld-servers.net.     172800  IN      AAAA    2001:500:856e::30
e.gtld-servers.net.     172800  IN      AAAA    2001:502:1ca1::30
f.gtld-servers.net.     172800  IN      AAAA    2001:503:d414::30
g.gtld-servers.net.     172800  IN      AAAA    2001:503:eea3::30
h.gtld-servers.net.     172800  IN      AAAA    2001:502:8cc::30
i.gtld-servers.net.     172800  IN      AAAA    2001:503:39c1::30
j.gtld-servers.net.     172800  IN      AAAA    2001:502:7094::30
k.gtld-servers.net.     172800  IN      AAAA    2001:503:d2d::30
l.gtld-servers.net.     172800  IN      AAAA    2001:500:d937::30
m.gtld-servers.net.     172800  IN      AAAA    2001:501:b1f9::30

;; Query time: 211 msec
;; SERVER: 202.12.27.33#53(202.12.27.33)
;; WHEN: Thu Aug 01 18:13:03 CST 2019
;; MSG SIZE  rcvd: 831

```


根服务器告诉老衲，.com 由 *.gtld-servers.net 这组服务器管理，这组服务器实际上就是 .com 域名的权威服务器，并且还顺便告诉了老衲他们的 IP 地址。

### 5.1.3 向 .com 权威服务器发送查询

老衲随机选取一个 .com 权威服务器进行查询，这里老衲选了 a.gtld-servers.net，它的IP地址是 192.5.6.30。

下面是和这台 .com 权威服务器的对话过程：
“嗨，请问你这有 jd.com 吗？”
“没有，但是你可以找管理 jd.com 域名的这些服务器问问。哦，对了，他们的 IP 地址我这刚好有，直接给你吧！”

```

 dig @192.5.6.30 jd.com  

; <<>> DiG 9.9.4-RedHat-9.9.4-61.el7 <<>> @192.5.6.30 jd.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16955
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 4, ADDITIONAL: 5
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;jd.com.                                IN      A

;; AUTHORITY SECTION:
jd.com.                 172800  IN      NS      ns1.jdcache.com.
jd.com.                 172800  IN      NS      ns2.jdcache.com.
jd.com.                 172800  IN      NS      ns3.jdcache.com.
jd.com.                 172800  IN      NS      ns4.jdcache.com.

;; ADDITIONAL SECTION:
ns1.jdcache.com.        172800  IN      A       111.13.28.10
ns2.jdcache.com.        172800  IN      A       111.206.226.10
ns3.jdcache.com.        172800  IN      A       120.52.149.254
ns4.jdcache.com.        172800  IN      A       106.39.177.32

;; Query time: 3 msec
;; SERVER: 192.5.6.30#53(192.5.6.30)
;; WHEN: Thu Aug 01 18:20:21 CST 2019
;; MSG SIZE  rcvd: 179

```


.com 权威服务器告诉老衲，jd.com 由 ns*.jdcache.com 这组服务器管理，并且还顺便告诉了老衲他们的 IP 地址。


### 5.1.4 第四步：向 jd.com 域名权威服务器发送查询

老衲随机选取一个 jd.com 权威服务器进行查询，这里老衲选了 ns1.jdcache.com，它的IP地址是 111.13.28.10。

下面是和这台 jd.com 权威服务器的对话过程：

“嗨，请问你这有 jd.com 吗？”
“有的，就在我这里，给你吧！”

```

dig @111.13.28.10 jd.com

; <<>> DiG 9.9.4-RedHat-9.9.4-61.el7 <<>> @111.13.28.10 jd.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 53607
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 8, ADDITIONAL: 9
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;jd.com.                                IN      A

;; ANSWER SECTION:
jd.com.                 120     IN      A       120.52.148.118

;; AUTHORITY SECTION:
jd.com.                 120     IN      NS      ns3.jd.com.
jd.com.                 120     IN      NS      ns3.jdcache.com.
jd.com.                 120     IN      NS      ns1.jdcache.com.
jd.com.                 120     IN      NS      ns1.jd.com.
jd.com.                 120     IN      NS      ns2.jdcache.com.
jd.com.                 120     IN      NS      ns2.jd.com.
jd.com.                 120     IN      NS      ns4.jdcache.com.
jd.com.                 120     IN      NS      ns4.jd.com.

;; ADDITIONAL SECTION:
ns1.jd.com.             120     IN      A       111.13.28.10
ns1.jdcache.com.        720     IN      A       111.13.28.10
ns2.jd.com.             120     IN      A       111.206.226.10
ns2.jdcache.com.        720     IN      A       111.206.226.10
ns3.jd.com.             120     IN      A       120.52.149.254
ns3.jdcache.com.        720     IN      A       120.52.149.254
ns4.jd.com.             120     IN      A       106.39.177.32
ns4.jdcache.com.        720     IN      A       106.39.177.32

;; Query time: 201 msec
;; SERVER: 111.13.28.10#53(111.13.28.10)
;; WHEN: Thu Aug 01 18:23:53 CST 2019
;; MSG SIZE  rcvd: 331

```


最终老衲得到了 jd.com 的 IP 地址为 120.52.148.118。


## 5.2 胶水记录在哪

在上面这部分查询过程中，其实有多台服务器返回了胶水记录：
1. 在第二步向根服务器查询返回的结果里，根服务器顺便告诉老衲的包含 `*.gtld-servers.net` 这组服务器的 IP 地址的记录，就是胶水记录。
2. 在第三步向 `.com` 权威服务器查询返回的结果里，`.com` 权威服务器顺便告诉老衲的包含 `ns*.jdcache.com` 这组服务器的 IP 地址的记录，就是胶水记录。

==所以，胶水记录就是域管理者向上级域管理者提供的一组主机名和IP的映射表：==
1. `.com` 域管理者（也就是 `.com` 域名注册局，VeriSign）向根域管理者（也就是 IANA）提供了：`.com` 的权威 DNS 服务器 `*.gtld-servers.net` 对应的 IP 地址
2. `jdcache.com` 域管理者向`.com` 域管理者（VeriSign）提供了：`jd.com` 的权威 DNS 服务器 `ns*.jdcache.com` 对应的 IP 地址

==也就是向 上级域的权威 DNS 服务器服务者  提供的 下级域的DNS 服务器对应的 IP 地址数据。==

## 5.3 胶水记录的主要作用

试想下：
    如果：没有设置这些胶水记录
    那么：这些服务器就无法顺便返回给老衲这些DNS服务器IP
    结果：老衲要想得到这些DNS服务器的IP，就得额外查询

所以胶水记录的作用就是：减少递归查询次数，加快 DNS 递归查询。

老衲认为，正是由于它是和 DNS 查询结果一起顺便返回的，所以才叫做胶水记录。


## 5.4 胶水记录的其它作用

https://laona.dev/post/glue-record/

 <mark> 所以胶水记录的另一个作用是：域名作为权威 DNS 服务器时，使解析自身域名成为可能 </mark>

如果将域名将给自己搭建的权威 DNS 服务器，会如何呢？

以下以 ming.app 使用权威 DNS 服务器 ns1.ming.app、ns2.ming.app为示例做个实验：
![](image/Pasted%20image%2020240305163209.png)

```
dig +trace +all www.ming.app

; <<>> DiG 9.11.5-P1-1ubuntu2.5-Ubuntu <<>> +trace +all www.ming.app
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 44062
;; flags: qr ra; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;.                              IN      NS

;; ANSWER SECTION:
.                       491071  IN      NS      d.root-servers.net.
.                       491071  IN      NS      c.root-servers.net.
.                       491071  IN      NS      e.root-servers.net.
.                       491071  IN      NS      h.root-servers.net.
.                       491071  IN      NS      b.root-servers.net.
.                       491071  IN      NS      m.root-servers.net.
.                       491071  IN      NS      j.root-servers.net.
.                       491071  IN      NS      a.root-servers.net.
.                       491071  IN      NS      i.root-servers.net.
.                       491071  IN      NS      f.root-servers.net.
.                       491071  IN      NS      l.root-servers.net.
.                       491071  IN      NS      g.root-servers.net.
.                       491071  IN      NS      k.root-servers.net.

;; Query time: 30 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Thu Aug 01 20:30:55 CST 2019
;; MSG SIZE  rcvd: 239

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 42488
;; flags: qr; QUERY: 1, ANSWER: 0, AUTHORITY: 7, ADDITIONAL: 11

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 1472
;; QUESTION SECTION:
;www.ming.app.                  IN      A

;; AUTHORITY SECTION:
app.                    172800  IN      NS      ns-tld1.charlestonroadregistry.com.
app.                    172800  IN      NS      ns-tld2.charlestonroadregistry.com.
app.                    172800  IN      NS      ns-tld3.charlestonroadregistry.com.
app.                    172800  IN      NS      ns-tld4.charlestonroadregistry.com.
app.                    172800  IN      NS      ns-tld5.charlestonroadregistry.com.
app.                    86400   IN      DS      23684 8 2 3A5CC8A31E02C94ABA6461912FABB7E9F5E34957BB6114A55A864D96 AEC31836
app.                    86400   IN      RRSIG   DS 8 1 86400 20190814050000 20190801040000 59944 . AiczCFnzwYcTmvZwd7xs3w81JtrT0pNSiHQw78RAABTYD12r07jicZBT AubL/XJ5fbWQseCf6KNkvzqN2JI5a5JCmFy+2pr470HBiMlBisKE6/3B 37sP3A7m1bQzwqJzEc5gv6CvBxkJEVcbLLhwgyyJxz268+yCykF/2viN vUyy1Y0sjM+Q98uuJ/UbL0Hsi+Ie4HYRwA4/P21tmYfTav399Xuv8eD4 YpI4r13/PQO/EoVXti+Sj9Sv+ze+nlhxDDUaw8lcyiLH8ztmxICS+MJr k398s2KHHQ8lHmhYPEqQQEOIen2zpRJWkK8s4iKzHTiwUykxzpLuCIXy 6gQBOw==

;; ADDITIONAL SECTION:
ns-tld1.charlestonroadregistry.com. 172800 IN A 216.239.32.105
ns-tld1.charlestonroadregistry.com. 172800 IN AAAA 2001:4860:4802:32::69
ns-tld2.charlestonroadregistry.com. 172800 IN A 216.239.34.105
ns-tld2.charlestonroadregistry.com. 172800 IN AAAA 2001:4860:4802:34::69
ns-tld3.charlestonroadregistry.com. 172800 IN A 216.239.36.105
ns-tld3.charlestonroadregistry.com. 172800 IN AAAA 2001:4860:4802:36::69
ns-tld4.charlestonroadregistry.com. 172800 IN A 216.239.38.105
ns-tld4.charlestonroadregistry.com. 172800 IN AAAA 2001:4860:4802:38::69
ns-tld5.charlestonroadregistry.com. 172800 IN A 216.239.60.105
ns-tld5.charlestonroadregistry.com. 172800 IN AAAA 2001:4860:4805::69

;; Query time: 174 msec
;; SERVER: 192.203.230.10#53(192.203.230.10)
;; WHEN: Thu Aug 01 20:30:56 CST 2019
;; MSG SIZE  rcvd: 732

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 29297
;; flags: qr; QUERY: 1, ANSWER: 0, AUTHORITY: 4, ADDITIONAL: 3

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 512
;; QUESTION SECTION:
;www.ming.app.                  IN      A

;; AUTHORITY SECTION:
ming.app.               10800   IN      NS      ns1.ming.app.
ming.app.               10800   IN      NS      ns2.ming.app.
rs5isqbli69sbr4fcjatu0sn1tfvka81.app. 900 IN NSEC3 1 0 1 139FCA6C02909936 RS5NLJ9CB29IIS9NLO4KRT2RUNI3NJ4M NS
rs5isqbli69sbr4fcjatu0sn1tfvka81.app. 900 IN RRSIG NSEC3 8 2 900 20190819163840 20190728163840 7866 app. oW/X7K5bBZflQD1wSsSsaG3CQS75PJN2Gg5k/fyKKRlfhh9PbXFAFKCE PU82m9bRENrYRMTw9CYaM3pwdcWFRBtTruQsz7pRsGeCBukr5bOJU8aJ YoMUH6n0v5oAjhMv4Y2UUA81NbCWPJjMvFOVvI+MKFdQrcdpZt0uf1aj Q1U=

;; ADDITIONAL SECTION:
ns1.ming.app.           3600    IN      A       103.102.--.--
ns2.ming.app.           3600    IN      A       103.102.--.--

;; Query time: 62 msec
;; SERVER: 216.239.32.105#53(216.239.32.105)
;; WHEN: Thu Aug 01 20:30:57 CST 2019
;; MSG SIZE  rcvd: 354

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26285
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.ming.app.                  IN      A

;; ANSWER SECTION:
www.ming.app.           300     IN      A       127.0.0.1

;; Query time: 153 msec
;; SERVER: 103.102.--.--#53(103.102.--.--)
;; WHEN: Thu Aug 01 20:30:57 CST 2019
;; MSG SIZE  rcvd: 58

```


如果没有返回下面这些胶水记录，要去哪里查 ns1.ming.app 和 ns2.ming.app 对应的 IP 地址呢？

```
;; ADDITIONAL SECTION:
ns1.ming.app.           3600    IN      A       103.102.--.--
ns2.ming.app.           3600    IN      A       103.102.--.--
```

