


## 0.1 TCP+TLS handshake

![](image/Pasted%20image%2020241028105328.png)


![](image/Pasted%20image%2020241028105838.png)

1 From local machine to google.comm, tlsv1.3 , send to destination Client Hallo 
目的是告诉 destination server.  我 client , 支持那些 Cipher Suite .   这些 都记录在Cipher Suites 里面了  

![](image/Pasted%20image%2020241028110110.png)

![](image/Pasted%20image%2020241028110202.png)

---


2 
server 回寄给 client 一个 message  内容是 Server Hello, Change Cipher Spec 

![](image/Pasted%20image%2020241028110324.png)

server 告诉 client 我准备用哪个 cipher suite 给你交流 

![](image/Pasted%20image%2020241028110530.png)



这之后的 application data 都是用这个 Cipher suite 加密的 (看他配上了 TLSv1.3 这个 protocol )

![](image/Pasted%20image%2020241028110649.png)


经过 application data 加密过后的内容 , 想要去解密 用 session key 
![](image/Pasted%20image%2020241028111902.png)

