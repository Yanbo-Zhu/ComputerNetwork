
- approaches to network control plane
    - per router control (traditional)
    - logically centralized control (software defined networking)
- traditional routing algorithms
    - implementation in Internet: OSPF , BGP
- SDN controllers
    - implementation in practice: ODL, ONOS
- Internet Control Message Protocol
- network management


# 1 data plane 和 control plane 的区别 

data plane: 
forwarding: move packets from router’ s input to appropriate router output data plane

control plane: 
routing: determine route taken by packets from source to destination


# 2 Two approaches to structuring network control plane
- per router control (traditional)
- logically centralized control (software defined networking) (SDN)

1
Per-router control plane
Individual routing algorithm components in each and every router interact in the control plane
![](image/Pasted%20image%2020241112195428.png)

2 
Software Defined Networking (SDN) control plane
Remote controller computes, installs forwarding tables in routers

![](image/Pasted%20image%2020241112195502.png)




# 3 Routing table 

![](image/Pasted%20image%2020241220161830.png)


# 4 longest prefix match 

Longest Prefix Match (LPM) is a fundamental algorithm used in computer networking, particularly in IP routing. It determines the best route for a packet by finding the most specific (longest) matching prefix in a routing table.

How It Works
1. **Routing Table:** Contains entries with destination prefixes (e.g., `192.168.0.0/16`, `192.168.1.0/24`).
2. **IP Packet:** Has a destination IP address (e.g., `192.168.1.5`).
3. **Match:** The router checks which prefix in the routing table matches the destination IP address.
4. **Longest Prefix:** Among all matching prefixes, the one with the longest mask (most bits) is chosen because it is the most specific route.



Example
Routing Table

|Prefix|Next Hop|
|---|---|
|`192.168.0.0/16`|Router A|
|`192.168.1.0/24`|Router B|
|`10.0.0.0/8`|Router C|

Incoming Packet
Destination IP: `192.168.1.5`

Matching Process
1. Compare `192.168.1.5` with `192.168.0.0/16`.
    - Matches: First 16 bits are the same.
2. Compare `192.168.1.5` with `192.168.1.0/24`.
    - Matches: First 24 bits are the same.
3. The longest matching prefix is `192.168.1.0/24` (24 bits), so the packet is forwarded to **Router B**.


# 5 Routing: Basic

- Routing: Mechanism to decide which path to use to transmit packets through a network
- Intradomain: within a routing domain (= within one administrative domain), algorithms usually find shortest paths, but scalability is limited to smaller networks, two popular variants:
    - Link-State: every router has full topology information of the entire routing domain, every router independently calculates shortest paths to all other routers (actually to all networks) using, e.g., the Dijkstra algorithm, example: OSPF
    - Distance vector: every router only knows its direct neighbors and the destinations that can be reached via this neighbor, shortest paths are distributed using, e.g., the Bellman-Ford algorithm, example: RIP
- Interdomain: between multiple routing domains
    -  Exchanges routing information may include entire paths, rules and filters decide which paths to use, example: BGP


- Unicast routing (point to point)
    - Proactive: routing information is exchanged and updated proactively, routing tables can immediately be used for data exchange -> we concentrate on this
    - Reactive: routing information is established only when data is ready for transmission
- Multicast routing (point to multipoint)
    - Efficient forwarding to multiple receivers
    - Extension to unicast routing
- Ad hoc routing
    - Dynamic network topology
    - Routing information can get outdated quickly
- Data centric routing
    - Routing based on content rather than destination address
    - Example: named data networking (NDN)


# 6 Routing: Graphs


Graph abstraction: link costs

![](image/Pasted%20image%2020241112195851.png)

Graphs are often used as an abstraction for networks
Allows the use of search algorithms from graph theory
Basic terminology:
![](image/Pasted%20image%2020250109224814.png)



Example of an undirected graph
- N = {A, B, C, D, E, F}
- E = {(A,B), (A,C), (A,D), (B,A), (B,C), (B,D), (C,A), (C,B), (C,D), (C,E), (C,F), (D,A), (D,B), (D,C), (D,E), (E,C), (E,D), (E,F), (F,C), (F,E)}
- c(A,B) = c(B,A) = 2, c(A,C) = c(C,A) = 5, ...
- ![](image/Pasted%20image%2020250109224900.png)



Some more terminology
- v 代表 某个 node 
- A path is a sequence (v1,v2,..., vn), so that all pairs (v1,v2), (v2,v3),..., (vn-1,vn)  属于 E
- The cost of a path is the sum of all edge costs c(v1,v2)+c(v2,v3)+..+c(vn-1,vn), it is called distance D(v1,vn )
- A path between two nodes v1 and v2 is called a shortest path, if there is no other path between both nodes with smaller cost (please note, there might be multiple shortest paths with the same cost)
- A cycle is a path where start node = end node
- A graph is connected, if there is a path between every pair of nodes

- A tree is connected graph that has no cycles
- A spanning tree of a graph G = (N,E) is a tree B = (N,E´) mit E´ belongs to  E
- A minimal spanning tree of a graph G for a node v is a spanning tree of G, that contains the shortest path between v and every other node in G
![](image/Pasted%20image%2020250109225122.png)




Properties of shortest paths
- If v is part of a shortest path between x and y, then the shortest path from x to
- y can be constructed from the shortest paths between x and v and v and y
    - ![](image/Pasted%20image%2020250109225227.png)
- Recursion scheme: D(x,y) = minv{D(x,v) + D(v,y)}
- This is the fundamental basis for shortest path algorithms




# 7 Routing protocols

Routing protocol goal: determine “good” paths (equivalently, routes), from sending hosts to receiving host, through network of routers 

path: sequence of routers packets traverse from given initial source host to final destination host 

“good”: least “cost”, “fastest”, “least congested”

routing: a “top 10” networking challenge!

![](image/Pasted%20image%2020241112195826.png)


---
Routing algorithm classification

- Link-State: every router has full topology information of the entire routing domain, every router independently calculates shortest paths to all other routers (actually to all networks) using, e.g., the Dijkstra algorithm, example: OSPF
- Distance vector: every router only knows its direct neighbors and the destinations that can be reached via this neighbor, shortest paths are distributed using, e.g., the Bellman-Ford algorithm, example: RIP

![](image/Pasted%20image%2020241112195909.png)


## 7.1 link state routing 


- All nodes have full knowledge of network topology
- This is achieved by flooding link state advertisements
- Every node calculates the shortest path to all other nodes using the Dijkstra algorithm
- If the topology changes (this can be identified by the underlying data link layer), changes are flooded again followed by a re-calculation of shortest paths

---

LSA Flooding
- Flooding of Link-State-Advertisements (LSA) containing
    - ID of the node that created the LSA
    - All direct neighbors (IDs and path (=link) cost to the neighbor)
    - Sequence number
    - Time to live
- Every node creates LSA containing all direct neighbors and sends it to all neighbors
- Received LSAs are first processed to update local topology information and then forwarded to all neighbors except the one from which it was received => flooding
- To improve reliability, acknowledgements and retransmissions are used in combination with sequence numbers and time to live field


### 7.1.1 Example LSA Flooding

- initial LSA creation and distribution to neighbors
    - ![](image/Pasted%20image%2020250112174320.png)
- received LSAs are used to update link states; LSAs are forwarded to all neighbors
    - ![](image/Pasted%20image%2020250112174425.png)
- final update of link state information
    - ![](image/Pasted%20image%2020250112174446.png)




### 7.1.2 Dijkstra Algorithm


![](image/Pasted%20image%2020241112200118.png)

Calculation of the routing table
- Routing table contains an entry for every destination node v including the next hop on the shortest path
- How to derive the next hop node from vector p(v) with predecessors?
- Recursive function: nexthop(v) = if p(v)=u then v else nexthop(p(v))

- Main idea
    - Iteratively calculate the minimal spanning tree
    - The set of nodes N´ contains all nodes, to which shortest paths are already known
    - N´ is initialized with the start node u
    - In every iteration, the neighbor node v is added, that has the lowest path cost from nodes in N´ and the edge to v has the shortest path: D(u,v) = minw in N'{D(u,w) + c(w,v)}
    - this is a specialization of the general recursion scheme D(u,v) = minw{D(u,w) + D(w,v)}, where the second part is an edge rather than a path
    - The algorithm terminates if N = N´

![](image/Pasted%20image%2020250112174726.png)


- Used data structures
    - Set of nodes N, start node u
    - N´ is the set of nodes to which shortest paths from u are known already
    - Costs c(v,w) between nodes v and w
        - Positive costs, if v and w are neighbors
        - 0 if v = w
        - unlimited else
    - Distance D(v) summarizes costs of the currently known shortest path from u to v
    - p(v) denotes the predecessor of v on the currently known shortest path from u to v

- Initialization
    - N´= {u};
    - For all vÎN: D(v)=c(u,v);

- Repeat until N´= N
    - ![](image/Pasted%20image%2020250112175621.png)


![](image/Pasted%20image%2020250112180055.png)


### 7.1.3 Forward Search Algorithm

- More practical approach to the Dijkstra algorithm
    - Every node collects LSAs and directly calculates routing table
    - Most popular version: Forward-Search-Algorithmus
    - All entries are stores in the form (destination, costs, next hop) within two separate lists
        - Acknowledged list (similar to N´)
        - Tentative (temporarary) list (similar to all neighbors from nodes in N´)
    - nexthop(v) is the next node to reach a node v from the start node u
    - Values for c(w,v) can directly be read from the LSAs
    - Values for D(w) and nexthop(w) are stored in the two lists
- Initialization
    - acknowledgedList = <(u,0,-)>, tentativeList = <>;
- Repeat
    - ![](image/Pasted%20image%2020250112180641.png)
- Until tentativeList is empty

(c,4,d) , from a, to c , through d , the total cost from a to c  ist 4

![](image/Pasted%20image%2020250112180748.png)

## 7.2 distance vector

![](image/Pasted%20image%2020241112200336.png)


### 7.2.1 Bellman Ford Algorithm

- Distributed shortest path search:
    - Every node tells its direct neighbors which other nodes can be reached at which cost (distance)
    - Initialization: only information about direct neighbors, with every exchange, paths to more nodes are learned and updated to converge to shortest paths
    - Makes use of property of shortest paths: D(u,v) = minw{D(u,w) + D(w,v)}
    - Here, for the first part just a single edge is used: D(u,v) = minw{c(u,w) + D(w,v)}
    - Whenever a new shortest path to a destination has been found, this is distributed again to all neighbors
    - Algorithm terminates, if no changes are made after receiving an update (the routing algorithm converges)

![](image/Pasted%20image%2020250112182155.png)

![](image/Pasted%20image%2020250112181922.png)


![](image/Pasted%20image%2020250112181930.png)


---

Example 

![](image/Pasted%20image%2020250112182007.png)

nh 为 经过 那个node, 到达 destination node 

![](image/Pasted%20image%2020250112182016.png)

![](image/Pasted%20image%2020250112182309.png)

![](image/Pasted%20image%2020250112182339.png)

![](image/Pasted%20image%2020250112182441.png)


### 7.2.2 Distance Vector Routing

Behavior in case of topology changes
- The Bellman Ford algorithm also works in case of topology changes (就是 d to c 的 cost 从 3 变为 1, 之后 整个 Distance Vector table 数值应该怎么更新  )
- If connection costs are getting smaller, the algorithm converges quickly: good news travel fast
- If connection costs are increasing, cycles may appear temporarily, and it may take some time to convergence: bad news travel slowly

![](image/Pasted%20image%2020250112182007.png)

![](image/Pasted%20image%2020250112182735.png)

![](image/Pasted%20image%2020250112182743.png)

![](image/Pasted%20image%2020250112182753.png)


### 7.2.3 Infinity Problem


- Outdated information in distributed routing tables may contain a cyclic path
- The slow iteration only ends, when the costs of the alternate path have been reached
- Solutions
    - Limit maximum costs (e.g., 16)  就是 避免新的 edge cost 过于大, 因为 infinite iterative incresment 
    - Poisoned reverse: if the shortest path from u to v goes via next hop w, u sends to w the infinite cost for the distance from u to v
    - Poisoned reverse may avoid cycles of length 3 but not larger cycles. 

---

example 

![](image/Pasted%20image%2020250112182844.png)


- 因为这时候 c 表中 c 到 a 的 cost 为 5, 还没有更新, 所以 b 表中 b to a via c 的 new cost 为 5+1 = 6 
![](image/Pasted%20image%2020250112182849.png)

![](image/Pasted%20image%2020250112183117.png)

![](image/Pasted%20image%2020250112183126.png)

- 缓步上升到 50 
![](image/Pasted%20image%2020250112183132.png)
## 7.3 Comparison of LS and DV algorithms


Link State Routing
- Centralized
- Complexity of Dijkstra algorithm is O( $n^2$) for n nodes
- Efficient implementations achieve O(n log n)
- Scalability limited
- Message overhead: O(ne) for n nodes and e edges
- Robustness: does not tolerate faulty / malicious information 
- Dynamic metrics may lead to instability (thus not used)

Distance Vector Routing
- Distributed
- Convergence problems in case of cycles
- Scalability limited
- Robustness: error propagation if faulty / malicious information is provided
- Dynamic metrics not supported

![](image/Pasted%20image%2020241112200427.png)





# 8 Internet approach to scalable routing


aggregate router into Region / Autonomous system AS
![](image/Pasted%20image%2020241119224319.png)


forwarding table 
![](image/Pasted%20image%2020241119224450.png)

# 9 intra-AS routing: OSPF (Open Shortest Path First )


OSPF: Open Shortest Path First 
link state routing
IS IS protocol (ISO standard, not RFC standard) essentially same as OSPF

router 发射信号 给整个as 内的 router, 计算相互减的 link costs 
![](image/Pasted%20image%2020241119224750.png)

Hierarchical OSPF
![](image/Pasted%20image%2020241119225001.png)

backbone 中的 router 负责向 其他 as 中 发送数据源 

# 10 inter-AS routing: AS routing:  BGP (Border Gateway Protocol):

BGP 告知别人 你这个 as 的存在 

BGP (Border Gateway Protocol): the de facto inter domain routing protocol
• glue that holds the Internet

allows subnet to advertise its existence, and the destinations it can reach, to rest of Internet: I am here, here is who I can reach, and how”

BGP provides each AS a means to:
•eBGP: obtain subnet reachability information from neighboring ASes
•iBGP: propagate reachability information to all AS internal routers.
• determine “ good” routes to other networks based on reachability information and policy

![](image/Pasted%20image%2020241120092629.png)


![](image/Pasted%20image%2020241120092811.png)



![](image/Pasted%20image%2020241120093534.png)

![](image/Pasted%20image%2020241120093748.png)


## 10.1 BGP path advertisement

从 AS 3 发送 X 到 AS1 
![](image/Pasted%20image%2020241120093616.png)


![](image/Pasted%20image%2020241120093810.png)


1d 的 forwarding table 

![](image/Pasted%20image%2020241120093820.png)

![](image/Pasted%20image%2020241120094500.png)


## 10.2 Why different Intra--, Inter AS routing

policy:
inter AS: admin wants control over how its traffic routed, who routes through its network
intra AS: single admin, so policy less of an issue

scale:
hierarchical routing saves table size, reduced update traffic

performance:
intra AS: can focus on performance
inter AS: policy dominates over performance


## 10.3 Hot potato routing

![](image/Pasted%20image%2020241120094835.png)


201 < 262 

2d 想要到达 as3 , 明显 走 2c 更近. 但是还是选择了 2a, 因为 他短视, 到 2a 的 link-cost 更低 , 相比于2c 

## 10.4 BGP: achieving policy via advertisements

provider network 更加优先选用自己的 customer network 而不是别人的 
B choosees not to advertise BAw to C, either BxC 

![](image/Pasted%20image%2020241120095016.png)

![](image/Pasted%20image%2020241120123937.png)




## 10.5 BGP route selection
优先级最上的 最优先被使用

router may learn about more than one route to destination
AS, selects route based on:
1. local preference value attribute: policy decision
2. shortest AS PATH
3. closest NEXT HOP router: hot potato routing
4. 4.additional criteria

# 11 Software defined networking (SDN) control plane


旧的方式  Per-router control plane  
==computed versus router computer forwarding tables==
Individual routing algorithm components in each and every router interact in the control plane to computer forwarding tables
每一router都已一个自己的  control plane , 自己的 computer forwarding tables 

![](image/Pasted%20image%2020241120124821.png)

新的方式: Remote controller computes, installs forwarding tables in routers

![](image/Pasted%20image%2020241120124845.png)


## 11.1 (SDN) control plane 的优点和缺点


优点 
Why a logically centralized control plane?
- easier network management: avoid router misconfigurations, greater flexibility of traffic flows
- table based forwarding (recall OpenFlow API) allows “programming” routers
    - centralized “programming” easier: compute tables centrally and distribute
    - distributed “programming” more difficult: compute tables as result of distributed algorithm (protocol) implemented in each and every router
- open (non proprietary) implementation of control plane
    - foster innovation: let 1000 flowers bloom

sdn 的缺点 
one could imagine SDN computed congestion control: controller sets sender rates based on router reported (to
controller) congestion levels



## 11.2 Traffic engineering: difficult with traditional routing


有点时候 需要重新定义 link weights 
![](image/Pasted%20image%2020241120125517.png)

有目的性的希望 traffic 分两个路走 
![](image/Pasted%20image%2020241120125524.png)


![](image/Pasted%20image%2020241120125610.png)



## 11.3 Software defined networking (SDN) architecture 


![](image/Pasted%20image%2020241120125835.png)

快速地给每个 router 指定 forwarding table , 用 api 的形式 去更新 table 
![](image/Pasted%20image%2020241120131736.png)




![](image/Pasted%20image%2020241120131925.png)



![](image/Pasted%20image%2020241120132056.png)


![](image/Pasted%20image%2020241120132156.png)

## 11.4 OpenFlow protocol

OpenFlow Controller 和 switcher 之间的联系 

![](image/Pasted%20image%2020241120132328.png)

---

1.  OpenFlow Controller 下命令去 修改 flow entries in thr openFlow tables in the controller switcher 
2.  当switcher 之间在传输包裹的时候遇到link failure ,  switcher 将包裹的控制权交给 controller . controller通过 更上层的centric的routing 算法 updated link status info. 然后  在这个switcher 中安装新的 flow table, 告诉 switcher 该将这个 package 发给那个 switcher 的那个 port

![](image/Pasted%20image%2020241120132526.png)



![](image/Pasted%20image%2020241120132541.png)


### 11.4.1 一个例子

当switcher 之间在传输包裹的时候遇到link failure ,  switcher 将包裹的控制权交给 controller . controller通过 更上层的centric的routing 算法 updated link status info. 
然后  在这个switcher 中安装新的 flow table, 告诉 switcher 该将这个 package 发给那个 switcher 的那个 port

![](image/Pasted%20image%2020241120133243.png)



![](image/Pasted%20image%2020241120133915.png)



## 11.5 SDN implementation

### 11.5.1 OpenDaylight (ODL) controller

![](image/Pasted%20image%2020241120140756.png)

### 11.5.2 ONOS controller

![](image/Pasted%20image%2020241120140809.png)


## 11.6 SDN Mac Learning 

> Challenge: Controller may miss events


### 11.6.1 MAC learning process

Principle : for packet ( src,dst ) arriving at port p
- If dst unknown : broadcast packets to all ports
    - Otherwise forward directly to known port
- Also: if src unknown , switch learns : src is behind p

1. h1 sends to h2: flood
    1. ![](image/Pasted%20image%2020250106164008.png)
2. h1 sends to h2: flood , learn (h1,p1)
    1. dstmac =h1,fwd(1)
3. h3 sends to h1: forward to p1
    1. ![](image/Pasted%20image%2020250106164103.png)
4.  h3 sends to h1: forward to p1, learn (h3,p3)
    1. ![](image/Pasted%20image%2020250106164140.png)
5. h1 sends to h3: forward to p3
    1. ![](image/Pasted%20image%2020250106164158.png)


### 11.6.2 How to implement this behavior (MAC-Learning process) in SDN

![](image/Pasted%20image%2020250106164317.png)



1. Initially table : Send everything to controller
    1. ![](image/Pasted%20image%2020250106164401.png)
2. When h1 sends to h2
    1. Principle : only send to controller if destination unknown
    2. ![](image/Pasted%20image%2020250106164441.png)
    3. Controller learns that h1@p1, updates table , and floods
        1. ![](image/Pasted%20image%2020250106164559.png)
3. Now assume h2 sends to h1
    1. Switch knows destination : message forwarded to h1
    2. BUT : No controller interaction , does not learn about h2 : no new rule for h2
        1. ![](image/Pasted%20image%2020250106164645.png)
4. Now , when h3 sends to h2
    1. Dest unknown : goes to controller which learns about h3. And then floods
    2. ![](image/Pasted%20image%2020250106164755.png)
5. Now , if h2 sends to h3 or h1
    1. Destinations known : controller does not learn about h2
    2. ![](image/Pasted%20image%2020250106164822.png)
6. Ouch! Controller cannot learn about h2 anymore : whenever h2 is source , destination is known . (因为 h3 和 h1 都已经作为 已知的 dst 登录在表里面了)
    2. All future requests to h2 will all be flooded : inefficient




# 12 Internet Control Message Protocol


![](image/Pasted%20image%2020241120134830.png)



![](image/Pasted%20image%2020241120134850.png)


# 13 network management and configuration

Device, hardware的管理h和配置更新 

Network management includes the deployment, integration and coordination of the hardware, software, and human elements to monitor, test, poll, configure, analyze, evaluate, and control the network and element resources to meet the real time, operational performance, and Quality of Service requirements at a reasonable cost.


![](image/Pasted%20image%2020241120135126.png)

![](image/Pasted%20image%2020241120135314.png)

## 13.1 SNMP (Simple Network Management Protocol)

Operator queries/sets devices data (MIB) using Simple Network Management Protocol (SNMP)


![](image/Pasted%20image%2020241120135554.png)

![](image/Pasted%20image%2020241120135602.png)


![](image/Pasted%20image%2020241120135609.png)


![](image/Pasted%20image%2020241120135621.png)




## 13.2 NETCONF/YANG

- more abstract, network wide, holistic
- emphasis on multi device configuration management.
- YANG: data modeling language
- NETCONF: communicate YANG compatible actions/data to/from/among remote devices

### 13.2.1 NETCONF

![](image/Pasted%20image%2020241120135757.png)

![](image/Pasted%20image%2020241120135821.png)


![](image/Pasted%20image%2020241120135932.png)

![](image/Pasted%20image%2020241120140045.png)

### 13.2.2 YANG

==data modeling language used to specify structure, syntax, semantics of NETCONF network management data==

![](image/Pasted%20image%2020241120135857.png)


















