
# 1 multiple access link

Two types of “links”:
- point-to-point
    - point-to-point link between Ethernet switch, host
    - PPP for dial-up access
- Broadcast (shared wire or medium)
    - old-fashioned Ethernet
    - upstream HFC in cable-based access network
    - 802.11 wireless LAN, 4G/4G. satellite


# 2 Multiple access protocols

- single shared broadcast channel  ==不同的 transmission 公用一个 channel ==
- two or more simultaneous transmissions by nodes: interference
    - collision if node receives two or more signals at the same time
- distributed algorithm that determines how nodes share channel, i.e., determine when node can transmit
- communication about channel sharing must use channel itself!
    - no out-of-band channel for coordination

# 3 multiple access channel (MAC) 

given: multiple access channel (MAC) of rate R bps

desiderata:
1. when one node wants to transmit, it can send at rate R.
2. when M nodes want to transmit, each can send at average rate R/M
3. fully decentralized:
    1. no special node to coordinate transmissions
    2. no synchronization of clocks, slots
4. simple


## 3.1 three broad classes

==不同的 transmission 公用一个 channel  使用的策略==


- channel partitioning, by time, frequency or code
    - Time Division, Frequency Division
- random access (dynamic),
    - ALOHA, S-ALOHA, CSMA, CSMA/CD
    - carrier sensing: easy in some technologies (wire), hard in others (wireless)
    - CSMA/CD used in Ethernet
    - CSMA/CA used in 802.11
- taking turns
    - polling from central site, token passing
    - Bluetooth, FDDI,  token ring

--- 


taxonomy 分类学 
three broad classes:
- channel partitioning
    - divide channel into smaller “pieces” (time slots, frequency, code)
    - allocate piece to node for exclusive use
- random access
    - channel not divided, allow collisions
    - “recover” from collisions
- “taking turns” 随时切换nodes
    - nodes take turns, but nodes with more to send can take longer turns
    - **"Taking turns"** means alternating actions or roles between two or more people in an organized or fair way, where each person gets a chance to participate or perform a task one after the other.


## 3.2 Channel partitioning MAC protocols

- channel partitioning
    - divide channel into smaller “pieces” (time slots, frequency, code)
    - allocate piece to node for exclusive use

--- 

TDMA: time division multiple access
- access to channel in “rounds”
- each station gets fixed length slot (length = packet transmission time) in each round
- unused slots go idle
- example: 6-station LAN, 1,3,4 have packets to send, slots 2,5,6 idle

![](image/Pasted%20image%2020241127213839.png)

--- 

FDMA: frequency division multiple access
 - channel spectrum divided into frequency bands
 - each station assigned fixed frequency band
 - unused transmission time in frequency bands go idle
 - example: 6-station LAN, 1,3,4 have packet to send, frequency bands 2,5,6 idle

![](image/Pasted%20image%2020241127214003.png)

## 3.3 Random access protocols

- random access
    - channel not divided, allow collisions
    - “recover” from collisions


- when node has packet to send
    - transmit at full channel data rate R.
    - no a priori coordination among nodes
- two or more transmitting nodes: “collision”
- random access MAC protocol specifies:
    - how to detect collisions
    - how to recover from collisions (e.g., via delayed retransmissions)
- examples of random access MAC protocols:
    - ALOHA, slotted ALOHA
    - CSMA, CSMA/CD, CSMA/CA


### 3.3.1 Slotted ALOHA

![](image/Pasted%20image%2020241127214206.png)



if collision: node retransmits frame in each subsequent slot with probability p until success
![](image/Pasted%20image%2020241127214215.png)


---

Slotted ALOHA: efficiency
efficiency: long-run fraction of successful slots  (many nodes, all with many frames to send)

suppose: N nodes with many frames to send, each transmits in slot with probability p
•prob that given node has success in a slot  = p(1-p)N-1
•prob that any node has a success = Np(1-p)N-1
•max efficiency: find p* that maximizes  Np(1-p)N-1
•for many nodes, take limit of Np*(1-p*)N-1 as N goes to infinity, gives:
    max efficiency = 1/e = .37

§at best: channel used for useful  transmissions 37% of time!


### 3.3.2 Pure ALOHA

![](image/Pasted%20image%2020241127214654.png)

### 3.3.3 CSMA (carrier sense multiple access)


carrier 搬运工 

![](image/Pasted%20image%2020241127214740.png)

![](image/Pasted%20image%2020241127214804.png)

![](image/Pasted%20image%2020241127214810.png)

![](image/Pasted%20image%2020241127214915.png)



## 3.4 “Taking turns” MAC protocols


“taking turns” protocols 是融合了两者 

channel partitioning MAC protocols:
- share channel efficiently and fairly at high load
- inefficient at low load: delay in channel access, 1/N bandwidth allocated even if only 1 active node!

random access MAC protocols
- efficient at low load: single node can fully utilize channel
- high load: collision overhead

“taking turns” protocols
- look for best of both worlds!

> 通过一个 master 控制不同的slaver 何时去 transmit data

polling
- master node “invites” other nodes to transmit in turn
- typically used with “dumb” devices
- concerns:
    - polling overhead
    - latency
    - single point of failure (master)

![](image/Pasted%20image%2020241127215246.png)


- token passing:
    - control token passed from one node to next sequentially.
    - token message
    - concerns:
- token overhead
- latency
- single point of failure (token)

![](image/Pasted%20image%2020241127215355.png)

### 3.4.1 Cable access network

Cable access network: FDM, TDM and random access!
- multiple downstream (broadcast) FDM channels: up to 1.6 Gbps/channel
    - single CMTS transmits into channels
- multiple upstream channels (up to 1 Gbps/channel)
    - multiple access: all users contend (random access) for certain upstream channel time slots; others assigned TDM

![](image/Pasted%20image%2020241127215620.png)


![](image/Pasted%20image%2020241127215637.png)


DOCSIS: data over cable service interface specificaiton
- FDM over upstream, downstream frequency channels
- TDM upstream: some slots assigned, some have contention\
    - downstream MAP frame: assigns upstream slots
    - request for upstream slots (and data) transmitted random access (binary backoff) in selected slots