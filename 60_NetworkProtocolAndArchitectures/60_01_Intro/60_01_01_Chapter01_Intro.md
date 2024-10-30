

# 1 Packet-Switching


packet-switching: hosts break application-layer messages into packets
• forward packets from one router to the next, across links on path from source to destination
• each packet transmitted at full link capacity



# 2 Forward and Routing 


![](image/Pasted%20image%2020241021065302.png)

Forwarding 是个动作, 将paket 发射出去的 action
Route 是确认 将packet发射到那个destination 的 查找动作

Name: whom would you like to reach? (object identity)

Address: where is the object? (locator)

Routing: each switch has to know which of his outputs should be used for a given destination address
- Hopefully contributes to short “overall trip distance, time”
- Some understanding of the possible routes is necessary to decide

Forwarding: a packet has arrived. How to “get rid of it” in the way consistent with the routing?
- With possibly short delay and - hopefully - little delay variation
- Structuring of the information describing packet destination and the way routing information is stored matters for execution time

![](image/Pasted%20image%2020241028071854.png)

# 3 Packet Loss

![](image/Pasted%20image%2020241021065534.png)


![](image/Pasted%20image%2020241021065634.png)
# 4 Packet delay 

![](image/Pasted%20image%2020241021065557.png)


![](image/Pasted%20image%2020241021065616.png)



# 5 Throughput 

throughput: rate (bits/time unit) at which bits are being sent from sender to receiver
• instantaneous: rate at given point in time rate over longer period of time
• average: rate over longer period of time 

![](image/Pasted%20image%2020241021065744.png)

# 6 Internet protocol stack 


![](image/Pasted%20image%2020241021070030.png)




