

# 1 总览 

https://www.sfn.cn/news/technology/detail/217.html?navId=22

DNS是互联网中一项重要的基础服务，它将简单易记的域名转换成可由计算机识别的IP地址，以便客户端对服务器的正常访问。而由DNS构建起的域名与IP地址之间的对应关系，称之为“DNS记录”（record）。通过设置不同的解析记录，可以实现对主机名不同的解析效果，从而满足不同场景下的域名解析需求。常见的域名解析记录，主要有以下几种类型。

 一、A记录
A（Address）记录是用来指定主机名（或域名）对应的IP地址记录。用户可以将该域名下的网站服务器指向到自己的web server上，同时也可以设置域名的子域名。简单来讲，A记录就是指定域名对应的IP地址。如我们添加一条A记录将www的主机指向IP192.168.1.1，那么当你访问www主机时就会解析到192.168.1.1这个IP上。

二、CNAME记录
通常称别名解析，是主机名到主机名的映射。当需要将域名指向另一个域名，再由另一个域名提供 IP 地址，就需要添加 CNAME 记录，最常用到 CNAME的场景包括做CDN、企业邮箱、全局流量管理等。与A记录不同的是，CNAME别名记录设置的值不是一个固定的IP，而是主机的别名地址。

别名解析可以提供更大的灵活性，便于统一管理。比如，当主机因某种因素的影响需要更换IP时，如果域名做了CNAME记录，就可以同时更新别名的解析指向，不需要进行新的解析操作。 

 三、NS记录
如果需要把子域名交给其他DNS服务商解析，就需要添加NS记录（Name Server）。NS记录是域名服务器记录，用来指定该域名由哪个DNS服务器来进行解析。NS记录中的IP即为该DNS服务器的IP地址。大多数域名注册商默认用自己的NS服务器来解析用户的DNS记录。DNS服务器NS记录地址一般以以下的形式出现：ns1.domain.com、ns2.domain.com等。

四、SOA记录
SOA，是起始授权机构记录，说明了在众多 NS 记录里哪一台才是主要的服务器。在任何DNS记录文件中，都是以SOA ( Startof Authority )记录开始。SOA资源记录表明此DNS名称服务器是该DNS域中数据信息的最佳来源。

SOA记录与NS记录的区别：NS记录表示域名服务器记录，用来指定该域名由哪个DNS服务器来进行解析；SOA记录设置一些数据版本和更新以及过期时间等信息。

五、AAAA记录
AAAA记录(AAAA record)是用来将域名解析到IPv6地址的DNS记录。用户可以将一个域名解析到IPv6地址上，也可以将子域名解析到IPv6地址上。国内大多数IDC不支持AAAA记录的解析，因此如果想进行AAAA记录解析，则需对域名NS记录设置一些专业的域名解析服务商，由他们提供AAAA记录的设置。国科云云解析支持IPv6环境下的AAAA记录解析。 


 六、TXT记录
TXT记录，一般指某个主机名或域名的标识和说明。如：admin IN TXT "管理员, 电话：XXXXXXXXXXX"，mail IN TXT "邮件主机，存放在xxx , 管理人：AAA"，Jim IN TXT "contact: abc@mailserver.com"，也就是说，通过设置TXT记录内容可以使别人更方便地联系到你。TXT 记录常用的方式还有做 SPF 记录（反垃圾邮件）和SSL证书的DNS验证等。

七、MX记录
MX（Mail Exchanger）记录是邮件交换记录，主要用于邮箱解析，在邮件系统发送邮件时根据收信人的地址后缀进行邮件服务器的定位。MX记录允许设置一个优先级，当多个邮件服务器可用时，会根据该值决定投递邮件的服务器。

MX记录的权重对 Mail 服务非常重要，当发送邮件时，Mail 服务器先对域名进行解析，查找 MX记录。先找权重数最小的服务器（比如说是 10），如果能连通，那么就将服务器发送过去；如果无法连通 MX 记录为 10 的服务器，才将邮件发送到权重更高的 mail 服务器上。


八、PTR记录
PTR是pointer 的简写，即“反向DNS”，domain name pointer，可以粗略的理解为DNS反向，是一个指针记录，用于将一个IP地址映射到对应的主机名，也可以看成是A记录的反向，即通过IP访问域名。


九、SRV记录
即服务定位（SRV）资源记录，用于定义提供特定服务的服务器的位置，如主机（hostname），端口（port number）等。


十、URL转发
URL转发，是指通过服务器的特殊设置，将当前访问的域名指向另一个指定的网络地址。根据目标地址的隐藏与否，URL转发可以分为显性URL和隐性URL两种。

显性URL：将域名指向一个http（s)协议地址，访问域名时，自动跳转至目标地址，地址栏显示为目标网站地址。

隐性URL：与显性URL类似，但隐性转发会隐藏真实的目标地址，地址栏中显示为仍为此前输入的地址。 


# 2 record 之间的冲突共存

 在RR值相同的情况下，同一条线路下，在几种不同类型的解析中不能共存(X为不允许)：

X：在相同的RR值情况下，同一条线路下，不同类型的解析记录不允许共存。如：已经设置了www.example.com的A记录，则不允许再设置www.example.com的CNAME记录；

无限制：在相同的RR值情况下，同一条线路下，不同类型的解析记录可以共存。如：已经设置了www.example.com的A记录，则还可以再设置www.example.com的MX记录；

可重复：指在同一类型下，同一条线路下，可设置相同的多条RR值。如：已经设置了www.example.com的A记录，还可以再设置www.example.com的A记录。 

![](image/Pasted%20image%2020240305110041.png)



# 3 DNS A Record

https://www.cloudflare.com/zh-cn/learning/dns/dns-records/dns-a-record/


## 3.1 什么是 DNS A记录？

“A”代表“地址”，这是最基础的 [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) 记录类型：它表示给定[域](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/)的 [IP 地址](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/)。比如，拉取 cloudflare.com 的 DNS 记录，A 记录当前返回的 IP 地址为：104.17.210.9。

A 记录只保存 IPv4 地址。如果一个网站拥有 IPv6 地址，它将改用[“AAAA”记录](https://www.cloudflare.com/learning/dns/dns-records/dns-aaaa-record/)。

下面是一个 A 记录示例：

|example.com|record type:|value:|TTL|
|---|---|---|---|
|@|A|192.0.2.1|14400|

该示例中的“@”符号表示这是根域的记录，“14400”值是 [TTL（生存时间）](https://www.cloudflare.com/learning/cdn/glossary/time-to-live-ttl/)，以秒为单位。A 记录的默认 TTL 是 14,400 秒。这意味着，如果更新 A 记录，需要 240 分钟（14,400 秒）后才会生效。

绝大多数网站只有一个 A 记录，但可以有多个。一些高知名度网站有数个不同的 A 记录，作为[循环负载均衡](https://www.cloudflare.com/learning/dns/glossary/round-robin-dns/)技术的一部分，该技术可以将请求流量分配到托管相同内容的多个 IP 地址中的一个。

## 3.2 什么时候使用 DNS A 记录？

A 记录最常见的用途是 IP 地址查找：将域名（如“cloudflare.com”）与 IPv4 地址进行匹配。这让用户的设备能够连接和加载网站，而无需用户记住和输入实际的 IP 地址。用户的 Web 浏览器通过向 [DNS 解析器](https://www.cloudflare.com/learning/dns/dns-server-types/)发送查询来自动完成此过程。

DNS A 记录还用于运营基于域名系统的黑洞名单 (DNSBL)。DNSBL 可以帮助邮件服务器识别和阻止来自已知垃圾邮件域的电子邮件。

如果您想了解有关 DNS A 记录的更多信息，可在[此处](https://tools.ietf.org/html/rfc1035)查看原始 1987 RFC，其中定义了 A 记录和几个其他 DNS 记录类型。要了解有关域名系统工作原理的更多信息，请参阅[什么是 DNS？](https://www.cloudflare.com/learning/dns/what-is-dns/)

# 4 DNS PTR (pointer record)

https://www.cloudflare.com/learning/dns/dns-records/dns-ptr-record/

一个 PTR 例子
例如，如果域的 IP 地址为 192.0.2.1，则 PTR 记录为 在 1.2.0.192.in-addr.arpa 下存储域的信息。
 长成这样 ： 1.2.0.192.in-addr.arpa  -> ivu-cloud.local 
 
![](image/Pasted%20image%2020240305110222.png)
## 4.1 What is a DNS PTR record

A Record:  domain name -> ip addresss
DNS pointer record:  ip addesse -> domain name 

The Domain Name System, or DNS, correlates domain names with IP addresses. 
A DNS pointer record (PTR for short) provides the domain name associated with an IP address. A DNS PTR record is exactly the opposite of the 'A' record, which provides the IP address associated with a domain name.

DNS PTR records are used in reverse DNS lookups. 
When a user attempts to reach a domain name in their browser, a DNS lookup occurs, matching the domain name to the IP address. 
A reverse DNS lookup is the opposite of this process: it is a query that starts with the IP address and looks up the domain name.


## 4.2 How are DNS PTR records stored

In IPv4:
While DNS A records are stored under the given domain name, DNS PTR records are stored under the IP address — reversed, and with ".in-addr.arpa" added. For example, the PTR record for the IP address 192.0.2.255 would be stored under "255.2.0.192.in-addr.arpa".

什么是 "in-addr.arpa" ： 
"in-addr.arpa" has to be added because PTR records are stored within the .arpa top-level domain in the DNS. .arpa is a domain used mostly for managing network infrastructure, and it was the first top-level domain name defined for the Internet. (The name "arpa" dates back to the earliest days of the Internet: it takes its name from the Advanced Research Projects Agency (ARPA), which created ARPANET, an important precursor to the Internet.)

in-addr.arpa is the namespace within .arpa for reverse DNS lookups in IPv4.

In IPv6:
IPv6 addresses are constructed differently from IPv4 addresses, and IPv6 PTR records exist in a different namespace within .arpa. IPv6 PTR records are stored under the IPv6 address, reversed and converted into four-bit sections (as opposed to 8-bit sections, as in IPv4), plus ".ip6.arpa".


# 5 Wildcard domain


A wildcard DNS record is a record in a DNS zone that will match requests for non-existent domain names. 
A wildcard DNS record is specified by using a * as the leftmost label (part) of a domain name, e.g.    ` *. example.com .`
A wildcard DNS record (catch-all DNS record) is a record in a DNS zone that can match queries for domain names that do not exist. It is created by using an asterisk (*) as a subdomain (the left part of a domain name, such as *. example.com).


In the Domain Name System (DNS), a CNAME (Canonical Name) record is used to create an alias from one domain name to another. 
A wildcard DNS record is a record that matches requests for non-existent subdomains


