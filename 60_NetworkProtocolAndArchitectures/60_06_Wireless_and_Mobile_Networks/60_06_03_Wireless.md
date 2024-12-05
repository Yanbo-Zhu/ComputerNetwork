
# 1 Wireless links and network characteristics


## 1.1 Wireless link characteristics

important differences from wired link ….
- decreased signal strength: radio signal attenuates 减弱 as it propagates through matter (path loss)
- interference from other sources: wireless network frequencies (e.g., 2.4 GHz) shared by many devices (e.g., WiFi, cellular, motors): interference
- multipath propagation: radio signal reflects off objects ground, arriving at destination at slightly different times
…. make communication across (even a point to point) wireless link much more “difficult”

---

SNR: signal-to-noise ratio
- larger SNR – easier to extract signal from noise (a “good thing”)

SNR versus BER tradeoffs
- given physical layer: increase power -> increase SNR->decrease BER
- given SNR: choose physical layer that meets BER requirement, giving highest throughput
    - SNR may change with mobility: dynamically adapt physical layer (modulation technique, rate)

![](image/Pasted%20image%2020241205143352.png)

---

Multiple wireless senders, receivers create additional problems (beyond multiple access):

![](image/Pasted%20image%2020241205143609.png)



## 1.2 Code Division Multiple Access (CDMA)

CDMA的关键是 chipping sequence 

- unique “code” assigned to each user; i.e., code set partitioning
    - all users share same frequency, but each user has own ==“chipping” sequence== (i.e., code) to encode data
    - allows multiple users to “coexist” and transmit simultaneously with minimal interference (if codes are “orthogonal”)
- encoding: inner product: (original data) X (chipping sequence)
- decoding: summed inner-product: (encoded data) X (chipping sequence)

Chipping sequence 就是 下图中的 code 行 中的 一段 code chipping 


CDMA encode/decode
![](image/Pasted%20image%2020241205143909.png)




CDMA: two-sender interference
![](image/Pasted%20image%2020241205143930.png)


recevier 端 用 sender 1 的 sequence chipping 去 解码  channel sum 的 信号代码 


# 2 WiFi: 802.11 wireless LANs


![](image/Pasted%20image%2020241205150438.png)




## 2.1 LAN architecture

- wireless host communicates with base station
    - base station = access point (AP)
- Basic Service Set (BSS) (aka “cell”) in infrastructure mode contains:
    - wireless hosts 
- access point (AP): base station
- ad hoc mode: hosts only

![](image/Pasted%20image%2020241205150827.png)


## 2.2 Channels, association


AP = access point , 在上图中就是 base station 

- spectrum divided into channels at different frequencies
    - AP admin chooses frequency for AP
    - interference possible: channel can be same as that chosen by neighboring AP
        - 什么是 interference:  wireless network frequencies (e.g., 2.4 GHz) shared by many devices
- arriving host: must associate with an AP 新加入的 host 必须和一个 AP 关联 
    - scans channels, listening for beacon frames containing AP’s name (SSID) and MAC address
        - Scan the channel 看看这个 channel 中有那些 AP, 可以供使用. 得到 这些 AP 的 name 和 MAC address 
    - selects AP to associate with
        - 选择一个AP加入进去. 
    - then may perform authentication
    - then typically run DHCP to get IP address in AP’s subnet
        - get the ip address in this ap subnet 

![](image/Pasted%20image%2020241205153339.png)


## 2.3 passive/active scanning

通过 scanning 
host 找 access point 的过程, 或者 access point 找 host 的过程 

![](image/Pasted%20image%2020241205153706.png)
passive scanning: ( access point 找 host )
(1)beacon 警示灯 frames sent from APs
(2)association Request frame sent: H1 to selected AP
(3)association Response frame sent from  selected AP to H1

---

![](image/Pasted%20image%2020241205153714.png)
active scanning: ( host 找 access point )
(1)Probe Request frame broadcast from H1
(2)Probe Response frames sent from APs
(3)Association Request frame sent: H1 to selected AP
(4)Association Response frame sent from selected AP to H1


## 2.4 multiple access

- avoid collisions: 2+ nodes transmitting at same time
- 802.11: CSMA - sense before transmitting 
    - don’t collide 冲突，抵触 with detected ongoing transmission by another node
- 802.11: no collision detection!
    - difficult to sense collisions: high transmitting signal, weak received signal due to fading
    - can’t sense all collisions in any case: hidden terminal, fading
    - goal: avoid collisions: CSMA/CollisionAvoidance


![](image/Pasted%20image%2020241205154910.png)



## 2.5 MAC Protocol: CSMA/CA

![](image/Pasted%20image%2020241205154954.png)

Sender
- 1 if sense channel idle for DIFS  then transmit entire frame (no CD)
- if sense channel busy, then start random backoff time timer counts down.  while channel idle,  transmit when timer expires. 
    - if no ACK, increase random backoff interval, repeat 2


Receiver 
if frame received OK, return ACK after SIFS (ACK needed due to hidden terminal problem)

## 2.6 Avoiding collisions (more)

CTS stands for **Clear to Send**. It is part of the **RTS/CTS (Request to Send / Clear to Send)** handshake protocol used in wireless communication to reduce collisions and ensure successful transmission.

idea: sender “reserves” channel use for data frames using small reservation packets 
下面是一个过程如何 预定一个 channel for a wireless transmission
- sender first transmits small request-to-send (RTS) packet to BS (base station) using CSMA
    - Sender 首先发送一个RTS告诉BS方, 我们要 要发送 信息给你了 
    - RTSs may still collide  with 冲突，抵触 each other (but they’re short)
        - 如果 首次的TS被阻塞了, 就等一会再次发送RTS 
- BS broadcasts (clear-to-send) CTS in response to RTS ( request-to-send )
    - BS boardcasts all nodes through sending CTS
    - CTS heard by all nodes
- sender transmits data frame
    -  然后发送 再发送 data frame后, 就会再在于 collision 了
- other stations defer transmissions

![](image/Pasted%20image%2020241205155716.png)


## 2.7 frame: addressing


![](image/Pasted%20image%2020241205205122.png)

Address1: targart MAC addresse
Address2: source MAC 
Address3: AP 是attch to which router interface 
CRC Cyclic Redundancy Check 

---



![](image/Pasted%20image%2020241205205254.png)

H1: host 
R1: Router 
AP: access point (base station )




![](image/Pasted%20image%2020241205205303.png)

## 2.8 mobility within same subnet
同一个  same subnet 内 不同 host 之间的通信 
一个 host 就是一个 device 

H1: host 
R1: Router 
AP: access point (base station )


- H1 remains in same IP subnet: IP address can remain same
- switch: which AP is associated with H1?
    - self-learning (Ch. 6): switch will see frame from H1 and “remember” which switch port can be used to reach H1



![](image/Pasted%20image%2020241205210033.png)


## 2.9 advanced capabilities
1 
Rate adaptation
- base station, mobile dynamically change transmission rate (physical layer modulation technique) as mobile moves, SNR varies

1. SNR decreases, BER increase as node moves away from base station
    1. SNR: signal-to-noise ratio
    2. **BER (Bit Error Rate)** is a measure of how frequently errors occur in a communication system during data transmission. It quantifies the ratio of the number of incorrect bits (errors) received to the total number of bits transmitted over a communication channel.
2. When BER becomes too high, switch to lower transmission rate but with lower BER

![](image/Pasted%20image%2020241205210511.png)

---

2
power management
- node-to-AP: “I am going to sleep until next beacon frame”  node要休息, 等待被 beacon frame唤醒 
    - AP knows not to transmit frames to this node
    - node wakes up before next beacon frame
- beacon frame: contains list of mobiles with AP-to-mobile frames waiting to be sent
    - node will stay awake if AP-to-mobile frames to be sent; otherwise sleep again until next beacon frame


什么是 beacon frame
![](image/Pasted%20image%2020241205153706.png)
passive scanning: ( access point 找 host )
(1)beacon 警示灯 frames sent from APs
(2)association Request frame sent: H1 to selected AP
(3)association Response frame sent from  selected AP to H1



## 2.10 Personal area networks: Bluetooth

一种 ad hoc 传输方式 , host 对 host 直接链接, 不经过 access point 

- less than 10 m diameter
- replacement for cables (mouse, keyboard, headphones)
- ad hoc: no infrastructure
- 2.4-2.5 GHz ISM radio band, up to 3 Mbps
- master controller / clients devices:
    - master polls clients, grants requests for client transmissions


- TDM, 625 msec sec. slot
- FDM: sender uses 79 frequency channels in known, pseudo-random order slot-to-slot (spread spectrum)
    - other devices/equipment not in piconet only interfere in some slots
- parked mode: clients can “go to sleep” (park) and later wakeup (to preserve battery)
- bootstrapping: nodes self-assemble (plug and play) into piconet


![](image/Pasted%20image%2020241205211312.png)


# 3 Cellular networks: 4G and 5G

- the solution for wide-area mobile Internet
- widespread deployment/use:
    - more mobile-broadband-connected devices than fixed-broadband-connected devices devices (5-1 in 2019)!
    - 4G availability: 97% of time in Korea (90% in US)
- transmission rates up to 100’s Mbps
- technical standards: 3rd Generation Partnership Project (3GPP)
    - wwww.3gpp.org
    - 4G: Long-Term Evolution (LTE)standard

similarities to wired Internet
- edge/core distinction, but both below to same carrier
- global cellular network: a network of networks
- 普遍的，广泛的 widespread use of protocols we’ve studied: HTTP, DNS, TCP, UDP, IP, NAT, separation of data/control planes, SDN, Ethernet, tunneling
- interconnected to wired Internet

differences from wired Internet
- different wireless link layer
- mobility as a 1st class service
- user “identity” (via SIM card)
- business model: users subscribe to a cellular provider
    - strong notion of “home network” versus roaming on visited nets
    - global access, with authentication infrastructure, and inter-carrier settlements


## 3.1 Elements of 4G LTE architecture

1 
Mobile device:
- smartphone, tablet, laptop, IoT, ... with 4G LTE radio
- 64-bit International Mobile Subscriber Identity (IMSI), stored on SIM (Subscriber Identity Module) card
- LTE jargon: User Equipment (UE)

![](image/Pasted%20image%2020241205213043.png)



---

2 Base station:
- at “edge” of carrier’s network
- manages wireless radio resources, mobile devices in its coverage area (“cell”)
- coordinates device authentication with other elements
- similar to WiFi AP but:
    - active role in user mobility
    - coordinates with nearly base stations to optimize radio use
- LTE jargon: eNode-B


![](image/Pasted%20image%2020241205213427.png)


--- 
3 
Home Subscriber Service
- stores info about mobile devices for which (which mobile devices) the HSS’s network is their “home network” 
- works with MME in device authentication

![](image/Pasted%20image%2020241205213508.png)

---

4 Serving Gateway (S-GW), PDN Gateway (P-GW)
- lie on data path from mobile to/from Internet
- P-GW
    - gateway to mobile cellular network
    - Looks like nay other internet gateway router
    - provides NAT services
- other routers:
    - extensive use of tunneling

![](image/Pasted%20image%2020241205214009.png)



---

5 Mobility Management Entity
- device authentication (device-to-network, network-to-device) coordinated with mobile home network HSS
- mobile device management:
    - device handover between cells
    - tracking/paging device location
- path (tunneling) setup from mobile device to P-GW

![](image/Pasted%20image%2020241205214705.png)
## 3.2 LTE: data plane control plane separation


![](image/Pasted%20image%2020241205214811.png)

### 3.2.1 LTE data plane protocol stack


First hop 

![](image/Pasted%20image%2020241205214846.png)



![](image/Pasted%20image%2020241205214939.png)

---
Packet core

tunneling:
- mobile datagram encapsulated using GPRS Tunneling Protocol (GTP), sent inside UDP datagram to S-GW
- S-GW re-tunnels datagrams to P-GW
- supporting mobility: only tunneling endpoints change when mobile user moves

Serving Gateway (S-GW), PDN Gateway (P-GW)

![](image/Pasted%20image%2020241205215416.png)


---

Associating with a BS (Base station)


![](image/Pasted%20image%2020241205215438.png)



![](image/Pasted%20image%2020241205215706.png)

## 3.3 Global cellular network: a network of IP networks


![](image/Pasted%20image%2020241205215942.png)

home network HSS:
- identify & services info, while in home network and roaming
all IP:
- carriers interconnect with each other, and public internet at exchange points
- legacy 2G, 3G: not all IP, handled otherwise


# 4 5G

high peak bitrate 
low latency 
large traffic capacity 

通过 millimeter wave frequence 实现 

![](image/Pasted%20image%2020241205220122.png)











