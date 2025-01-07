
How to Design a Datacenter?

![](image/Pasted%20image%2020250106165119.png)

# 1 Recall

Layer 2 networks are very flexible : location independent addresses, plug&play , self learning, etc.: devices (and virtual machines!) can move (migrate)
But: Layer 2 networks do not scale : despite caching, LAN wide broadcasts needed once in a while (ARP, MAC learning, DHCP, etc.)!

How large should a LAN be
Flexibility vs Scalability tradeoff

# 2 Proposal 1: Large LAN

A large LAN: High mobility … but high overhead due to learning and broadcasting
![](image/Pasted%20image%2020250106165735.png)



# 3 Proposal 2: small LAN 


![](image/Pasted%20image%2020250106165759.png)
