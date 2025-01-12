

# 1 Packet-Switching


packet-switching: hosts break application-layer messages into packets
• forward packets from one router to the next, across links on path from source to destination
• each packet transmitted at full link capacity


# 2 Forward and Routing 


![](image/Pasted%20image%2020241021065302.png)

![](image/Pasted%20image%2020250109222442.png)

Forwarding 是个动作, 将paket 发射出去的 action
Route 是确认 将packet发射到那个destination 的 查找动作

Hosts send and receive, routers forward messages
- Forwarding: router received messages on one interface and forwards it on another interface (the right one!) toward the destination
- Routing: procedure to decide which path to use for which messages

Name: whom would you like to reach? (object identity)
Address: where is the object? (locator)

Routing: each switch has to know which of his outputs should be used for a given destination address
- Hopefully contributes to short “overall trip distance, time”
- Some understanding of the possible routes is necessary to decide

Forwarding: a packet has arrived. How to “get rid of it” in the way consistent with the routing?
- With possibly short delay and - hopefully - little delay variation
- Structuring of the information describing packet destination and the way routing information is stored matters for execution time

![](image/Pasted%20image%2020241028071854.png)


# 3 Forwarding through Router 

Tasks: forwarding, execution of routing protocols
Conceptual architecture:
![](image/Pasted%20image%2020250109222601.png)

## 3.1 Router: Input

Input interface
![](image/Pasted%20image%2020250109223109.png)

- Buffering, if packets are received faster than processed internally
- Packet loss, if buffer overflows
- Efficient forwarding: each interface often has a copy of the routing table
- Most important: efficient data structures for fast search


## 3.2 Router: Switching Fabric

Memory coupled switching
- CPU copied packets from input interface to main memory, after deciding where to forward the packet, it copies the packet to the output interface
- Internal memory bus is used twice (limiting the performance)
- Architecture of old routers but sometimes used today as well

![](image/Pasted%20image%2020250109223239.png)

---

Bus coupled switching
- Single bus connects all ports, can be used for a single transfer per time -> competition
- Often used for small routers

![](image/Pasted%20image%2020250109223503.png)


---

Switching matrix / switching network
- Known from interconnects of CPUs in parallel computers
- Example: Crossbar, every port can be connected to every other (quadratic number of connections)
- Often also multi-stage
![](image/Pasted%20image%2020250109223625.png)


## 3.3 Router: Output

Output interface
- Buffering, if switching fabric can deliver packets faster than transmission over output interface
- Packet loss, if buffer overflows
- Active queue management: decision, which packet should be discarded
- Scheduling: if multiple packets are buffered, which to be processed next

![](image/Pasted%20image%2020250109223740.png)



## 3.4 Impact of Buffering and Packet Loss

If switching fabric is faster than # ports x data rate:
- Normally no input buffering needed
- If multiple inputs want to forward to the same output port, buffering is needed

![](image/Pasted%20image%2020250109222825.png)

Head-of-the-Line (HOL) Blocking
- If multiple inputs want to forward to the same output, some need to wait until switching fabric becomes available again
- All packets waiting at one such input port need to wait, despite their output would be available
![](image/Pasted%20image%2020250109222919.png)


# 4 Routing

见 60_04_07_Control_Plane 中

# 5 Packet Loss

![](image/Pasted%20image%2020241021065534.png)


![](image/Pasted%20image%2020241021065634.png)
# 6 Packet delay 

![](image/Pasted%20image%2020241021065557.png)


![](image/Pasted%20image%2020241021065616.png)



# 7 Throughput 

throughput: rate (bits/time unit) at which bits are being sent from sender to receiver
• instantaneous: rate at given point in time rate over longer period of time
• average: rate over longer period of time 

![](image/Pasted%20image%2020241021065744.png)

# 8 Internet protocol stack 


![](image/Pasted%20image%2020241021070030.png)




