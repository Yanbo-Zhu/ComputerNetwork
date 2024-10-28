
# 1 HTTP/HTTPS协议

![](image/Pasted%20image%2020240216165526.png)

![](image/Pasted%20image%2020240216170154.png)

# 2 HTTPS协议
## 2.1 SSL/TLS

SSL: Secure Sockets Layer 安全套接字 协议
TLS: Transport Layer Security  传输层安全协议 

他们主要用于 在 internet 上数据传输的安全性和完整性 


看[[使用数字证书通讯]] 

## 2.2 加密验证方式 

![](image/Pasted%20image%2020240216172206.png)

![](image/Pasted%20image%2020240216172256.png)

## 2.3 公钥和私钥


![](image/Pasted%20image%2020240216172503.png)#

公钥和私钥都可以用于 加密解密
公钥加密，私钥解密： 称为信息加密与信息解密
私钥加密， 公钥解密： 称为数字签名与签名验证



# 3 HTTPS重要概念

## 3.1 SSL, TLS & HTTPS

certificate: 
Secure Sockets Layer (SSL) certificates, sometimes called digital certificates, are used to establish an encrypted connection between a browser or user’s computer and a server or website.


SSL: Secure Sockets Layer
[SSL](https://www.digicert.com/faq/public-trust-and-certificates/what-is-ssl) is standard technology for securing an internet connection by encrypting data sent between a website and a browser (or between two servers). It prevents hackers from seeing or stealing any information transferred, including personal or financial data.


TLS: Transport Layer Security
TLS is an updated, more secure version of SSL. We still refer to our security certificates as SSL because it’s a more common term, but when you buy SSL from DigiCert, you get the most trusted, up-to-date TLS certificates.



HTTPS: Hyper Text Protocol Secure
HTTPS appears in the URL when a website is secured by an SSL/TLS certificate. Users can view the details of the certificate, including the issuing authority and the corporate name of the website owner, by clicking the lock symbol on the browser bar.


## 3.2 数字证书 (SSL/TLS 证书)

![](image/Pasted%20image%2020240217133314.png)

某个通讯方的 数字证书:
通讯方的数字证书是 可以是 被某个权威CA 颁发的。 
里面包含了 通讯方的信息， 通讯方的公钥， 证书有效时间， 域名， CA的数字签名 

某个私人用户的 数字证书 
可以是也可是自己生成的。 用户自己搭建一个ca, 然后用 ca 生成 自己的数字证书。 将这个ca 的公钥和 用户自己的数字证书 分享给 其他的用户
里面包含了 私人用户的个人信息， 私人用户的公钥， 证书有效时间， 域名

## 3.3 根证书 (CA公钥)

CA 产生的某个用户的 数字证书 是进过 CA私钥 加密的。 
CA 会将 生成的数字证书 和 CA 公钥 发给 user 

![](image/Pasted%20image%2020240217133510.png)


## 3.4 数字摘要

利用 hash 函数产生的 值

![](image/Pasted%20image%2020240217133900.png)


摘要就是 某个 image 的 RepoDigests 项的内容 

![](image/Pasted%20image%2020240217133823.png)

## 3.5 数字签名 

![](image/Pasted%20image%2020240217134148.png)

数字签名 ( 就是用 用户x自己私钥 加密明文后 得到的密文) 
一般会将 密文 （数字签名） 附到 明文后， 形成一个整个东西， 回复给 提出联络的 user 
# 4 基础知识 
## 4.1 明文通信过程 

![](image/Pasted%20image%2020240216183921.png)


## 4.2 使用 数字签名加密

![](image/Pasted%20image%2020240216184208.png)


![](image/Pasted%20image%2020240216184304.png)

## 4.3 钓鱼问题


![](image/Pasted%20image%2020240216185223.png)


## 4.4 使用数字证书通讯 (非对称加密 TLS/SSL)

[97_01_HTTP_HTTPS协议](97_01_HTTP_HTTPS协议.md)


![](image/Pasted%20image%2020241028090818.png)


整个通信过程包含三个阶段： 通信基础构建阶段， 通信关系建立阶段与通信阶段 

要区分 数字证书 和 数字签名 

1 通信基础构建阶段
生成用户1的数字证书和 CA公钥 
这个阶段的最终结果就是 用户1 从 ca 处 得到 数字证书和 CA公钥
![](image/Pasted%20image%2020240216185802.png)

CA: Certificate Authority
数字证书中包含： 用户 张三 的个人信息 和 用户 1 的公钥 
数字证书 就是用 CA 的私钥 加密后 形成的。 只有用 CA 的公钥 才能解密。

2 通信关系建立阶段
将用户 张三  的数字证书和CA公钥 交给用户 李四
用户 李四 用 CA 的公钥 解密 用户 张三 的数字证书， 从而得到用户张三  的个人信息和用户1的公钥 

这个阶段的最终结果就是 用户李四  得到 用户张三 数字证书 (用户1 的个人信息和用户1的公钥  ) 和 CA公钥

![](image/Pasted%20image%2020240216185844.png)

3 通信阶段

因为在 阶段2 已经 李四 获得了 用户张三  的个人信息和用户1的公钥。 
所以在 阶段3 的 初始阶段， 再次获得 用户x 的个人信息和用户x的公钥。 对比之下就能  李四 发现 用户x 是不是 张三 .


![](image/Pasted%20image%2020240216190221.png)
上图中第二个4 是多余的 

在 阶段3 的 握手阶段， 
1. 用户李四 使用 阶段3 的 初始阶段 获得的 用户x 的公钥。 用户李四 用 用户x 的公钥  来加密 用户李四 想给用户x 的指令 
2. 用户x 使用自己的私钥 去解密. 
3. 用户x 把想要 回复给 用户李四 的内容:   回复的内容的明文 + 数字签名 ( 就是用 用户x自己私钥 加密明文后 得到的密文) + 用户x 的数字证书
4. 用户2 得到上面东西后 
    1. 用ca 公钥 解密用户x 的数字证书， 看看用户x 是不是争取的用户
    2. 用用户x的公钥 解密 数字证书， 得到解密后的 明文
    3. 讲解密后的明文 和  直接得到的加密后的明文 比较， 看看消息有没有被篡改



## 4.5 对称加密 (https 的工作原理)

整个通信过程包含三个阶段： 通信基础构建阶段， 通信关系建立阶段与通信阶段 
身份验证采用非对称加密方式, 通信过程采用对称加密方式 

1 通信基础构建阶段
这个和非对称加密通信(使用数字证书通讯 ) 是一样的 没有变化 

![](image/Pasted%20image%2020240217125647.png)

2 通信关系建立阶段
这个和非对称加密通信(使用数字证书通讯 ) 多了 5,6,7 过程 
==重点是用户2 生成了 秘钥R , 这个 秘钥会在以后的通信过程中用到 ==
用户 2 自己会保存 密钥R, 然后也会将 R 分享给 用户x

![](image/Pasted%20image%2020240217125821.png)

3 通信阶段
这个阶段 这个和非对称加密通信(使用数字证书通讯 ) 比较 , 4,5,6,7,8 就不在使用 CA证书了 ， 而使用 密钥R 

![](image/Pasted%20image%2020240217130514.png)

# 5 HTTPS通信过程 

和对称加密过程基本相似 

整个通信过程包含三个阶段： 通信基础构建阶段， 通信关系建立阶段与通信阶段 

1 通信基础构建阶段
![](image/Pasted%20image%2020240217132534.png)


2 通信关系建立阶段
![](image/Pasted%20image%2020240217132842.png)



3 通信阶段 
![](image/Pasted%20image%2020240217133016.png)




