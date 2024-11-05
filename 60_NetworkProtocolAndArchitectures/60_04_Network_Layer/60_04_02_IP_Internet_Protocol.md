
![](image/Pasted%20image%2020241105191436.png)


# 1 IP_Datagram_format


![](image/Pasted%20image%2020241105191502.png)


# 2 IP address and interface 

![](image/Pasted%20image%2020241105191632.png)


One Router's have multiple interfaces, because it connects with multiple subdomain 



# 3 Subnets


![](image/Pasted%20image%2020241105192127.png)

![](image/Pasted%20image%2020241105192444.png)


> Subnet 的意义是 Device interfaces that can physically reach each other without passing through an intervening router 

detach each interface from its host or router, creating “islands” of isolated networks
each isolated network is called a subnet


223.1.3.0/24
subnet mask: /24
the first 24 bit are subnet part 
25-32: tbhe last 8 bit are host part 

# 4 CIDR

![](image/Pasted%20image%2020241105192522.png)


# 5 DHCP: How does host get IP address?

Dynamic Host Configuration Protocol: dynamically get address from as server

DHCP 的意义是获取 ip address of xx from network server 

![](image/Pasted%20image%2020241105192657.png)

DHCP server 被部署在 router 


![](image/Pasted%20image%2020241105192716.png)


第一部中 Arriving client broadcast in the domain to finde DHCP server 
第二步中 DHCP server 还不知道 arrving client 的 ip adress, 所以他只能通过 broadcast 去找 arrving client, 而不是 通过 ip addresse of arriving client  直接发送给 这个 client 
之后 arrving client 将自己的 ip address 登记在 dhcp server 中 

![](image/Pasted%20image%2020241105193127.png)


![](image/Pasted%20image%2020241105193626.png)


## 5.1 example 


client 从 dhcp 中获取一些信息的过程 

![](image/Pasted%20image%2020241105193650.png)


![](image/Pasted%20image%2020241105193709.png)





