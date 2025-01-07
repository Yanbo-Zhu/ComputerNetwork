
# 1 Congestion Control: Desirable Bandwidth Allocation

## 1.1 Question: What are we trying to achieve by running a congestion control algorithm?

Answer:
- We need to specify the state in which a good congestion control algorithm will operate the network
- More than simply avoiding congestion => ==need to find a good allocation of bandwidth to transport entities that are using the network ==
    -  congestion control 的本质是 find a good allocation of bandwidth
- A good allocation:
    1. Delivers good **performance** because it uses all available bandwidth but avoids congestion
    2. It will be **fair** across competing transport entities
    3. It will quickly track changes in traffic demands


- Efficient use of bandwidth gives high goodput, low delay
- Goodput 实际吞吐 (or rate of useful packets arriving at receiver) and delay as a function of the offered load.


Congestion collapse: 拥塞发生的那一刻 , buffer 满了, reveicer 端 不再收到有用的 packet 了 都堵了 
![](image/Pasted%20image%2020241212225749.png)

Onset of Congestion : 拥塞发生的那一刻 , buffer 满了, reveicer 端 不再收到有用的 packet 了 都堵了 , 自然 收到 有效的 packet 的时间就电厂了 
![](image/Pasted%20image%2020241212225903.png)


- Performance begins to degrade at the onset of congestion
- Best performance is obtained if we allocate bandwidth up until the delay starts to climb rapidly
- This point is below the capacity
    - To identify it, Kleinrock (1979) proposed the metric of power:  power = load/delay 
- Note: load with highest power represents an efficient load for transport entity to place on the network


## 1.2 Next question: How to divide bandwidth between different transport senders?

- Possible answer: give all senders an equal fraction of bandwidth
- … but what has this to do with congestion control?
    - Networks often have no strict bandwidth reservation for each flow or connection => e.g., in IP routers all connections are competing for the same bandwidth => here the congestion control mechanism is responsible for allocating bandwidth to the competing connections


## 1.3 Question: What does a fair portion means for flows in a network? 

- Simple case:
    - N flows sharing a single link => each gets 1/N of bandwidth
- More complicated:
    - Flows having different, but overlapping, network paths
    - Example:
        - One flow may cross three links, and the other flows may cross one link
        - Three-link flow consumes more network resources => might be fairer to give it less bandwidth than the one-link flows

=> Tension between fairness and efficiency

### 1.3.1 Max-min fairness

Max-Min Fairness is a resource allocation principle used to fairly distribute bandwidth or other resources among multiple competing users or flows while ensuring that ==the most disadvantaged flows are prioritized.==

- We define notion of fairness that does not depend on length of network path
    - … looks simple but is complicated as different connections will take different paths through network and these paths will may have different capacities
- Max-min fairness
    - Often desired for network usage
    - Gives bandwidth to all flows (no starvation)
    - Gives equal shares of bottleneck
- Fair use gives bandwidth to all flows (no starvation)
- Max-min fairness gives equal shares of bottleneck

- Max-min allocations can be computed given a global knowledge of the network
- Intuitive way to think about them:
    - Rate for all flows starts at zero and is slowly increased
    - When rate reaches a bottleneck for any flow => that flow stops increasing
    - Other flows continue to increase, sharing equally in the available capacity, until they too reach their respective bottlenecks
- Another issue: level over which to consider fairness
    - fairness per host vs. fairness per connection

Advantages of Max-Min Fairness
- **Fair Allocation**: Ensures small demands are met first.
- **No Starvation**: Guarantees every flow gets some resource.
- **Simple Implementation**: Iterative algorithms are straightforward.


4 flows A-D

![](image/Pasted%20image%2020241212232002.png)



---



Key Concepts of Max-Min Fairness

- **Fair Distribution**:
    - Bandwidth is allocated in such a way that increasing the allocation for one flow reduces the allocation for another flow that has an equal or smaller share.
    - 
- **Prioritization**:
    - Smaller flows (those needing less bandwidth) are satisfied first, and leftover bandwidth is distributed to larger flows.
- **No Starvation**:
    - All flows receive some bandwidth, ensuring no flow is completely denied access.
- **Pareto Efficiency**:
    - No flow's allocation can be increased without decreasing the allocation of a flow with a smaller or equal allocation.
    - You cannot increase the allocation of any flow unless you decrease the allocation of another flow with an equal or smaller allocation.
    - This ensures that fairness is preserved, and smaller allocations are not reduced to increase larger ones.
    - 我们必须减少 原来大的 allocation of flow , 来 增加 原本小的 allocation of flow 
    - 我门不可以 减少 小的 allocation of lows, 来增加 原本大的 allocation of flow 

![](image/Pasted%20image%2020241212235717.png)



---

Conclusion
The principle ensures that Max-Min Fairness is:

- **Prioritized**: Smaller or underprivileged flows are protected.
- **Stable**: Changes in allocation require a proportional consideration of fairness across all flows.

Algorithm for Max-Min Fairness in Bandwidth Allocation

- **Sort Flows**:
    - Sort flows in ascending order of their requested bandwidth.
- **Iterative Allocation**:
    - Allocate bandwidth incrementally:
        - Initially, allocate the same amount of bandwidth to all flows.
        - If a flow reaches its requested bandwidth, freeze its allocation, and redistribute the remaining bandwidth among the unfrozen flows.
- **Terminate**:
    - Stop when all bandwidth is allocated or all flows are satisfied.

----


Example1: Max-Min Fairness in Bandwidth

Scenario:
- Total bandwidth: 20 Mbps
- Flows and their requested bandwidth:
    - Flow A: 5 Mbps
    - Flow B: 10 Mbps
    - Flow C: 15 Mbps

Steps:
1. **Initial Allocation**:
    - Divide bandwidth equally among all flows: 203≈6.67\frac{20}{3} \approx 6.67320​≈6.67 Mbps.
    - Flow A is satisfied (it only needs 5 Mbps).
    - Remaining bandwidth: 20−5=1520 - 5 = 1520−5=15 Mbps.
2. **Reallocate Remaining Bandwidth**:
    - Redistribute among the unfrozen flows (B and C): 152=7.5\frac{15}{2} = 7.5215​=7.5 Mbps each.
Result:
- Flow A: 5 Mbps (fully satisfied)
- Flow B: 7.5 Mbps (fully satisfied)
- Flow C: 7.5  Mbps (partially satisfied)


## 1.4 convergence

- Congestion control algorithm should converge quickly to a fair and efficient allocation of bandwidth
- Challenging as network environment is dynamic: connections are always coming and going in a network, and the bandwidth needed by a given connection will vary over time too (bursty traffic like Web browsing) 
- Goal: rapidly converge to ideal operating point + track that point as it changes over time
- Convergence: We want bandwidth levels to converge quickly when traffic change

![](image/Pasted%20image%2020241213000324.png)





# 2 Congestion Control: Regulating the Sending Rate

Sender may need to slow down for different reasons:
- Flow control, when the receiver is not fast enough
- Congestion control, when the network is not fast enough

(a) a fast network feeding a low-capacity receiver => flow control is needed
(b) a slow network feeding a high-capacity receiver => congestion control is needed 
- Our focus is congestion

![](image/Pasted%20image%2020241213000606.png)


Congestion window controls the sending rate
- Rate is cwnd / RTT; window can stop sender quickly
- ACK clock (regular receipt of ACKs) paces traffic and smoothes out sender bursts
- ack 回到 sender 的速度变慢了, sender 就根据这个情况发送数据了, 调整 sendungsrate 

![](image/Pasted%20image%2020250107235231.png)

---

Different congestion signals the network may use to tell the transport endpoint to slow down (or speed up)
- In explicit, precise design the routers tell sources the rate at which they may send => e.g., XCP (Explicit Congestion Control)
- Explicit, imprecise design: e.g., use of ECN (Explicit Congestion Notification) with TCP => routers set bits on packets that experience congestion to warn the senders to slow down, but they do not tell them how much to slow down
- Other designs have no explicit signal: e.g., FAST TCP measures roundtrip delay and uses that metric as a signal to avoid congestion
- TCP with drop-tail or RED routers: packet loss is inferred and used to signal that the network has become congested => most prevalent in Internet today


Signals of some congestion control protocols
![](image/Pasted%20image%2020241213000841.png)

## 2.1 The AIMD (Additive Increase Multiplicative Decrease) control law

- We consider case of binary congestion feedback
- If two flows increase/decrease their bandwidth in the same way when the network signals free/busy they will not converge to a fair allocation
- ==最终目的是 通过不断地 Additive Increase Multiplicative Decrease, 使得bandwith 的分配到达 optimale point ==
- The AIMD (Additive Increase Multiplicative Decrease) control law does converge to a fair and efficient point!
    - Multiplicative  Increase Additive Decrease 的方式 是到达不了 Optimale point 的, 因为 斜度的问题 
    - start point 随意给出 

![](image/Pasted%20image%2020241213001054.png)


![](image/Pasted%20image%2020241213001321.png)


- TCP uses AIMD with loss signal to control congestion
    - Implemented as a congestion window (cwnd) for the number of segments that may be in the network
    - Uses several mechanisms that work together
    - ![](image/Pasted%20image%2020250107235433.png)

---


## 2.2 AIMD 的具体实施过程:

Problem: AIMD rule will take a very long time to reach a good operating point on fast networks if the congestion window is started from a small size
Solution: slow start grows congestion window exponentially: Doubles every RTT while keeping ACK clock going
![](image/Pasted%20image%2020250107235543.png)


Whenever the slow start threshold is crossed, TCP switches from slow start to additive increase.
Additive increase grows cwnd slowly (congestion window)
- Adds 1 every RTT
- Keeps ACK clock
![](image/Pasted%20image%2020250107235608.png)

Slow start followed by additive increase (TCP Tahoe)
Threshold is half of previous loss cwnd
![](image/Pasted%20image%2020250107235835.png)


With fast recovery, we get the classic sawtooth (TCP Reno)
- Retransmit lost packet after 3 duplicate ACKs
- New packet for each dup. ACK until loss is repaired

![](image/Pasted%20image%2020250107235908.png)


SACK (Selective ACKs) extend ACKs with a vector to describe received segments and hence losses
- Allows for more accurate retransmissions / recovery

![](image/Pasted%20image%2020250107235937.png)




## 2.3 TCP 实际中 是怎么控制 sending rate 的 

- TCP implements an AIMD control law to adjust the sending rate and provide congestion control
- But not so easy as rates are measured over some interval and traffic is bursty
- Instead of adjusting the rate directly, TCP adjusts the size of a sliding window
- If window size is W and the round-trip time is RTT, the equivalent rate is W/RTT
- This strategy is easy to combine with flow control, which already uses a window
- Sender paces 为……定速度 packets using ACKs and hence slows down in one RTT if it stops receiving reports that packets are leaving the network





# 3 Congestion_Control


Congestion:
- informally: “too many sources sending too much data too fast for network to handle”
- manifestations:
    - long delays (queueing in router buffers)
    - packet loss (buffer overflow at routers)
- different from flow control!
- a top-10 problem!



## 3.1 Causes/costs of congestion: 


- Objective: mechanism that limits transmissions from sender based on network’s carrying capacity (rather than on receiver’s buffering capacity) 
- Solution: usage of sliding window flow-control scheme in which sender dynamically adjusts window size to match the network’s carrying capacity
- Note: dynamic sliding window can implement both flow and congestion control!  
    - 使用 动态的大小的 silding winodws也可以起到 congestion control 的作用 
- Optimal window size:
    - Network can handle c segments/sec,  
    - Round-trip time (including transmission, propagation, queueing, processing at the receiver, and return of the acknowledgement) is r
    - Sender’s window should be cr 
    - Sender 会一次发射 windows size 大小的 segment, 就是说, 然后 . 距离下次一次sender 发射 segment 到 Receiver 的时间间隔是r,  . 我们预设 receiver 在下次收到 segenent 前, 能处理 掉 cr 个 segement. 所以 Sender’s window should be cr 
- As network capacity available to any given flow varies over time, the window size should be adjusted frequently … to track changes in the carrying capacity

---

- Issue: If transport entities on many machines send too many packets into the network => network will become congested => performance degraded as packets are delayed and lost
- Controlling congestion is combined responsibility of network and transport layers:
    - Congestion occurs at routers => detected at network layer
    - But congestion is ultimately caused by traffic sent into network by transport layer!
- Approach: ==The only effective way to control congestion is for transport protocols to send packets into network more slowly==

Focus:
- Goals of congestion control
- Algorithms used by hosts to ==regulate rate at which they send packets into network==
- Note: Internet relies heavily on transport layer for congestion control => specific algorithms are built into TCP





![](image/Pasted%20image%2020241208181923.png)

### 3.1.1 scenario 1

![](image/Pasted%20image%2020241208181622.png)

### 3.1.2 scenario 2

![](image/Pasted%20image%2020241208181630.png)

![](image/Pasted%20image%2020241208181706.png)

![](image/Pasted%20image%2020241208181713.png)

![](image/Pasted%20image%2020241208181723.png)

![](image/Pasted%20image%2020241208181743.png)

“costs” of congestion:
- more work (retransmission) for given receiver throughput
- unneeded retransmissions: link carries multiple copies of a packet
    - decreasing maximum achievable throughput

### 3.1.3 scenario 3

![](image/Pasted%20image%2020241208181837.png)


![](image/Pasted%20image%2020241208181848.png)



## 3.2 Approaches towards congestion control


![](image/Pasted%20image%2020241208181957.png)

![](image/Pasted%20image%2020241208182223.png)




## 3.3 AIMD Additive Increase Multiplicative Decrease 

approach: senders can increase sending rate until packet loss (congestion) occurs, then decrease sending rate on loss event


![](image/Pasted%20image%2020241101201639.png)


Multiplicative decrease detail: sending rate is
- Cut in half on loss detected by triple duplicate ACK (TCP Reno)
- Cut to 1 MSS (maximum segment size) when loss detected by timeout (TCP Tahoe)

## 3.4 TCP CUBIC

![](image/Pasted%20image%2020241101201911.png)

increase W as a function of the cube of the distance between current time and K

![](image/Pasted%20image%2020241101202104.png)


