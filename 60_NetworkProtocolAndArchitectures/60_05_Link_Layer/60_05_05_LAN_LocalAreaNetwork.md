
# 1 MAC addresses

Media Access Control) address

32-bit IP address:
- network-layer address for interface
- used for layer 3 (network layer) forwarding
- e.g.: 128.119.40.136


MAC (or LAN or physical or Ethernet) address:
- function: used “locally” to get frame from one interface to another physically-connected interface (same subnet, in IP-addressing sense)
    - 对于每个 physically-connected interface , mac ist unique 
- 48-bit MAC address (for most LANs) burned in NIC ROM, also sometimes software settable
- e.g.: 1A-2F-BB-76-09-AD
    - hexadecimal (base 16) notation (each “numeral” represents 4 bits)


- each interface on LAN
    - has unique 48-bit MAC address
    - has a locally unique 32-bit IP address (as we’ve seen) 
    - MAC address 和  IP address  合在一起使用 , 双重锁定 某个地址 


![](image/Pasted%20image%2020241127220600.png)


- MAC address allocation administered by IEEE
- manufacturer buys portion of MAC address space (to assure uniqueness)
- analogy:
    - MAC address: like Social Security Number
    - IP address: like postal address
- MAC flat address: portability
    - can move interface from one LAN to another
    - recall IP address not portable: depends on IP subnet to which node is attached


# 2 ARP_address resolution protocol

how to determine interface’s MAC address, knowing its IP address?

ARP table: each IP node (host, router) on LAN has ==table==
- IP/MAC address mappings for some LAN nodes:          < IP address; MAC address; TTL>
- TTL (Time To Live): time after which address mapping will be forgotten (typically 20 min)

![](image/Pasted%20image%2020241127221100.png)


## 2.1 ARP protocol in action

根据 IP address 寻找 LAN 中 这个 IP address 对应的 mac 地址 

example: A wants to send datagram to B
- B’s MAC address not in A’s ARP table, so A uses ARP to find B’s MAC address



![](image/Pasted%20image%2020241127221350.png)

![](image/Pasted%20image%2020241127221722.png)


![](image/Pasted%20image%2020241127221731.png)


## 2.2 Routing to another subnet: addressing


![](image/Pasted%20image%2020241127221801.png)


![](image/Pasted%20image%2020241127221854.png)



![](image/Pasted%20image%2020241127221925.png)



mac src 和 mac dest 变化 , ip src 和 ip dest 不变 

![](image/Pasted%20image%2020241127222338.png)



![](image/Pasted%20image%2020241127222455.png)



![](image/Pasted%20image%2020241127222555.png)



# 3 Ethernet

![](image/Pasted%20image%2020241127222717.png)

physical topology
![](image/Pasted%20image%2020241127222806.png)

## 3.1 Ethernet frame structure
![](image/Pasted%20image%2020241127222820.png)


- addresses: 6 byte source, destination MAC addresses
    - if adapter receives frame with matching destination address, or with broadcast address (e.g., ARP packet), it passes data in frame to network layer protocol
    - otherwise, adapter discards frame  , 找不到地址就放弃 
- type: indicates higher layer protocol
    - mostly IP but others possible, e.g., Novell IPX, AppleTalk
    - used to demultiplex up at receiver
- CRC: cyclic redundancy check at receiver
    - error detected: frame is dropped

## 3.2 Ethernet: unreliable, connectionless

connectionless: no handshaking between sending and receiving NICs
unreliable: receiving NIC (network interface card) doesn’t send ACKs or NAKs to sending NIC
- data in dropped frames recovered only if initial sender uses higher layer rdt (e.g., TCP), otherwise dropped data lost
Ethernet’s MAC protocol: unslotted CSMA/CD with binary backoff



## 3.3 Ethernet standards: link & physical layers


![](image/Pasted%20image%2020241127224053.png)



# 4 switches
-  Switch is a link-layer device: takes an active role
    - ==store, forward Ethernet frames==
    - ==examine incoming frame’s MAC address, selectively forward  frame== to one-or-more outgoing links when frame is to be forwarded on segment, uses CSMA/CD to access segment
- transparent: hosts unaware of presence of switches
- plug-and-play, self-learning
    - switches do not need to be configured

## 4.1 multiple simultaneous transmissions

- hosts have dedicated, direct connection to switch
- switches buffer packets
- Ethernet protocol used on each incoming link, so:
    - no collisions; full duplex
    - each link is its own collision domain
- switching: A-to-A’ and B-to-B’ can transmit simultaneously, without collisions
    - ==but A-to-A’ and C to A’ can not happen simultaneously==

![](image/Pasted%20image%2020241127224435.png)

![](image/Pasted%20image%2020241127224556.png)


## 4.2 Switch forwarding table

![](image/Pasted%20image%2020241127224629.png)

---

Switch forwarding table 中 慢慢 填入 entry 的过程 

![](image/Pasted%20image%2020241127224707.png)

frame filtering/forwarding
![](image/Pasted%20image%2020241127224753.png)

如果 forwarding table 没有某个 desination 需要的 entry, 
用 flood 去 测试 这个 switch 中 所有的  interface 看能不能找到 想要的 destation 的 interface. 能找到的话 就将这个 填入到 forwarding table  作为一个新的 entry 

![](image/Pasted%20image%2020241127224935.png)


## 4.3 Interconnecting switches

![](image/Pasted%20image%2020241127225313.png)



![](image/Pasted%20image%2020241127225338.png)

## 4.4 Switches vs. routers

both are store-and-forward:
- routers: network-layer devices (examine network-layer headers)
- switches: link-layer devices (examine link-layer headers)

both have forwarding tables:
- routers: compute tables using routing algorithms, IP addresses
- switches: learn forwarding table using flooding, learning, MAC addresses


![](image/Pasted%20image%2020241127225431.png)



# 5 VLANs_virtualLAN

> A **Virtual LAN (VLAN)** is a logical grouping of devices within a physical network that are segmented into separate broadcast domains, regardless of their physical locations. VLANs are used to improve network efficiency, enhance security, and simplify network management.




Q: what happens as LAN sizes scale, users change point of attachment?

![](image/Pasted%20image%2020241127225757.png)


single broadcast domain:
- scaling: all layer-2 broadcast traffic (ARP, DHCP, unknown MAC) must cross entire LAN
- efficiency, security, privacy issues

administrative issues:
- CS user moves office to EE - physically attached to EE switch, but wants to remain logically attached to CS switch


## 5.1 **Key Characteristics of VLANs**

1. **Logical Segmentation**:
    - Devices in the same VLAN act as if they are on the same physical network, even if they are on different switches or physical locations.
    - Each VLAN creates its own broadcast domain, reducing unnecessary traffic.
2. **Uses VLAN Tags**:
    - VLANs are identified using VLAN IDs (0–4095).
    - Switches use protocols like **IEEE 802.1Q** to add VLAN tags to Ethernet frames for identification.
3. **Isolation**:
    - Devices in one VLAN cannot directly communicate with devices in another VLAN unless a router or Layer 3 switch facilitates it.

---

## 5.2 **Benefits of VLANs**

1. **Improved Network Performance**:
    - By reducing broadcast traffic, VLANs minimize congestion in large networks.
2. **Enhanced Security**:
    - Sensitive systems can be isolated in a separate VLAN, preventing unauthorized access from other parts of the network.
3. **Simplified Network Management**:
    - VLANs make it easier to manage devices based on function, department, or project instead of physical location.
4. **Scalability**:
    - VLANs allow easy expansion and reorganization of the network without changing physical connections.

## 5.3 **Example**
Imagine an office with employees from the **HR** and **IT** departments connected to the same physical switch. Without VLANs:
- All devices would belong to the same broadcast domain, sharing the same traffic.

With VLANs:
1. The **HR department** can be assigned to VLAN 10.
2. The **IT department** can be assigned to VLAN 20.

- Devices in VLAN 10 cannot communicate directly with VLAN 20 unless routing is configured.


## 5.4 Port-based VLANs

> 什么是 VLAN:  switch(es) supporting VLAN capabilities can be configured to define multiple virtual LANS over single physical LAN infrastructure.

> port-based VLAN: switch ports grouped (by switch management software) so that single physical switch or operates as multiple virtual switches

![](image/Pasted%20image%2020241127225921.png)


- traffic isolation: frames to/from ports 1-8 can only reach ports 1-8
    - can also define VLAN based on MAC addresses of endpoints, rather than switch port
- dynamic membership: ports can be dynamically assigned among VLANs
- forwarding between VLANS: done via routing (just as with separate switches)
    - in practice vendors sell combined switches plus routers


## 5.5 VLANS spanning multiple switches

![](image/Pasted%20image%2020241127230134.png)

trunk port: carries frames between VLANS defined over multiple physical switches
- frames forwarded within VLAN between switches can’t be vanilla 802.1 frames (must carry VLAN ID info)
- 802.1q protocol adds/removed additional header fields for frames forwarded between trunk ports

## 5.6 802.1Q VLAN frame format


![](image/Pasted%20image%2020241127230352.png)



