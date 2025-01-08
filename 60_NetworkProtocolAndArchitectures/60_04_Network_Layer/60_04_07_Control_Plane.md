
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



# 5 Routing protocols

Routing protocol goal: determine “good” paths (equivalently, routes), from sending hosts to receiving host, through network of routers 

path: sequence of routers packets traverse from given initial source host to final destination host 

“good”: least “cost”, “fastest”, “least congested”

routing: a “top 10” networking challenge!

![](image/Pasted%20image%2020241112195826.png)


---
Graph abstraction: link costs

![](image/Pasted%20image%2020241112195851.png)

---
Routing algorithm classification

![](image/Pasted%20image%2020241112195909.png)


## 5.1 link state

![](image/Pasted%20image%2020241112200118.png)


## 5.2 distance vector

![](image/Pasted%20image%2020241112200336.png)





## 5.3 Comparison of LS and DV algorithms

![](image/Pasted%20image%2020241112200427.png)






# 6 Internet approach to scalable routing


aggregate router into Region / Autonomous system AS
![](image/Pasted%20image%2020241119224319.png)


forwarding table 
![](image/Pasted%20image%2020241119224450.png)

# 7 intra-AS routing: OSPF (Open Shortest Path First )


OSPF: Open Shortest Path First 
link state routing
IS IS protocol (ISO standard, not RFC standard) essentially same as OSPF

router 发射信号 给整个as 内的 router, 计算相互减的 link costs 
![](image/Pasted%20image%2020241119224750.png)

Hierarchical OSPF
![](image/Pasted%20image%2020241119225001.png)

backbone 中的 router 负责向 其他 as 中 发送数据源 

# 8 inter-AS routing: AS routing:  BGP (Border Gateway Protocol):

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


## 8.1 BGP path advertisement

从 AS 3 发送 X 到 AS1 
![](image/Pasted%20image%2020241120093616.png)


![](image/Pasted%20image%2020241120093810.png)


1d 的 forwarding table 

![](image/Pasted%20image%2020241120093820.png)

![](image/Pasted%20image%2020241120094500.png)


## 8.2 Why different Intra--, Inter AS routing

policy:
inter AS: admin wants control over how its traffic routed, who routes through its network
intra AS: single admin, so policy less of an issue

scale:
hierarchical routing saves table size, reduced update traffic

performance:
intra AS: can focus on performance
inter AS: policy dominates over performance


## 8.3 Hot potato routing

![](image/Pasted%20image%2020241120094835.png)


201 < 262 

2d 想要到达 as3 , 明显 走 2c 更近. 但是还是选择了 2a, 因为 他短视, 到 2a 的 link-cost 更低 , 相比于2c 

## 8.4 BGP: achieving policy via advertisements

provider network 更加优先选用自己的 customer network 而不是别人的 
B choosees not to advertise BAw to C, either BxC 

![](image/Pasted%20image%2020241120095016.png)

![](image/Pasted%20image%2020241120123937.png)




## 8.5 BGP route selection
优先级最上的 最优先被使用

router may learn about more than one route to destination
AS, selects route based on:
1. local preference value attribute: policy decision
2. shortest AS PATH
3. closest NEXT HOP router: hot potato routing
4. 4.additional criteria

# 9 Software defined networking (SDN) control plane


旧的方式  Per-router control plane  
==computed versus router computer forwarding tables==
Individual routing algorithm components in each and every router interact in the control plane to computer forwarding tables
每一router都已一个自己的  control plane , 自己的 computer forwarding tables 

![](image/Pasted%20image%2020241120124821.png)

新的方式: Remote controller computes, installs forwarding tables in routers

![](image/Pasted%20image%2020241120124845.png)


## 9.1 (SDN) control plane 的优点和缺点


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



## 9.2 Traffic engineering: difficult with traditional routing


有点时候 需要重新定义 link weights 
![](image/Pasted%20image%2020241120125517.png)

有目的性的希望 traffic 分两个路走 
![](image/Pasted%20image%2020241120125524.png)


![](image/Pasted%20image%2020241120125610.png)



## 9.3 Software defined networking (SDN) architecture 


![](image/Pasted%20image%2020241120125835.png)

快速地给每个 router 指定 forwarding table , 用 api 的形式 去更新 table 
![](image/Pasted%20image%2020241120131736.png)




![](image/Pasted%20image%2020241120131925.png)



![](image/Pasted%20image%2020241120132056.png)


![](image/Pasted%20image%2020241120132156.png)

## 9.4 OpenFlow protocol

OpenFlow Controller 和 switcher 之间的联系 

![](image/Pasted%20image%2020241120132328.png)

---

1.  OpenFlow Controller 下命令去 修改 flow entries in thr openFlow tables in the controller switcher 
2.  当switcher 之间在传输包裹的时候遇到link failure ,  switcher 将包裹的控制权交给 controller . controller通过 更上层的centric的routing 算法 updated link status info. 然后  在这个switcher 中安装新的 flow table, 告诉 switcher 该将这个 package 发给那个 switcher 的那个 port

![](image/Pasted%20image%2020241120132526.png)



![](image/Pasted%20image%2020241120132541.png)


### 9.4.1 一个例子

当switcher 之间在传输包裹的时候遇到link failure ,  switcher 将包裹的控制权交给 controller . controller通过 更上层的centric的routing 算法 updated link status info. 
然后  在这个switcher 中安装新的 flow table, 告诉 switcher 该将这个 package 发给那个 switcher 的那个 port

![](image/Pasted%20image%2020241120133243.png)



![](image/Pasted%20image%2020241120133915.png)



## 9.5 SDN implementation

### 9.5.1 OpenDaylight (ODL) controller

![](image/Pasted%20image%2020241120140756.png)

### 9.5.2 ONOS controller

![](image/Pasted%20image%2020241120140809.png)


## 9.6 SDN Mac Learning 

> Challenge: Controller may miss events


### 9.6.1 MAC learning process

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


### 9.6.2 How to implement this behavior (MAC-Learning process) in SDN

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




# 10 Internet Control Message Protocol


![](image/Pasted%20image%2020241120134830.png)



![](image/Pasted%20image%2020241120134850.png)


# 11 network management and configuration

Device, hardware的管理h和配置更新 

Network management includes the deployment, integration and coordination of the hardware, software, and human elements to monitor, test, poll, configure, analyze, evaluate, and control the network and element resources to meet the real time, operational performance, and Quality of Service requirements at a reasonable cost.


![](image/Pasted%20image%2020241120135126.png)

![](image/Pasted%20image%2020241120135314.png)

## 11.1 SNMP (Simple Network Management Protocol)

Operator queries/sets devices data (MIB) using Simple Network Management Protocol (SNMP)


![](image/Pasted%20image%2020241120135554.png)

![](image/Pasted%20image%2020241120135602.png)


![](image/Pasted%20image%2020241120135609.png)


![](image/Pasted%20image%2020241120135621.png)




## 11.2 NETCONF/YANG

- more abstract, network wide, holistic
- emphasis on multi device configuration management.
- YANG: data modeling language
- NETCONF: communicate YANG compatible actions/data to/from/among remote devices

### 11.2.1 NETCONF

![](image/Pasted%20image%2020241120135757.png)

![](image/Pasted%20image%2020241120135821.png)


![](image/Pasted%20image%2020241120135932.png)

![](image/Pasted%20image%2020241120140045.png)

### 11.2.2 YANG

==data modeling language used to specify structure, syntax, semantics of NETCONF network management data==

![](image/Pasted%20image%2020241120135857.png)


















