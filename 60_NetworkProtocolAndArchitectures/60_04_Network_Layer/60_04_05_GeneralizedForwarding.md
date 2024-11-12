

# 1 Generalized Forwarding, SDN


> 每个Router 都有一个 forwarding table . match column 说明了 这个matched entry. 对应的 action column 是 当收到这个matched entry的时候, 要做什么action 
> match 整个 header 中的 gefield , 去匹配 match column 已经写好的 matched entry.

Match+action
OpenFlow: match+action

- “match plus action” abstraction: match bits in arriving packet header(s) in
    - any layers, take action
    - matching over many fields (link --, network --, transport layer)
    - local actions: drop, forward, modify, or send matched packet to controller
    - “program” network wide behaviors
- simple form of “network programmability”
    - programmable, per packet “processing”
    - historical roots: active networking
    - today: more generalized programming: P4 (see p4.org).

## 1.1 match plus action
each router contains a forwarding table
- “match plus action” abstraction: match bits in arriving packet, take action
   - destination based forwarding: forward based on
  - generalized forwarding :
       - many header fields can determine action
       - many action possible: drop/copy/modify/log packet

![](image/Pasted%20image%2020241112203704.png)


--- 
什么是 Flow table abstraction

- flow: defined by header field values (in link --, network --, transport layer fields)
- generalized forwarding: simple packet handling rules
    - match: pattern values in packet header fields
    - actions: for matched packet: drop, forward, modify, matched packet or send matched packet to controller
    - priority: disambiguate overlapping patterns
    - counters: `#bytes and #packets`  : 这个参数能够 显示 当前的 Workload,  counter 的数值越大, workload 越严重 


![](image/Pasted%20image%2020241112203720.png)

![](image/Pasted%20image%2020241112203857.png)



## 1.2 OpenFlow: match+action+Stats

> Opentable is a standlone protocoll which munipulate flow table and pu the table into switcher or router 
> OpenFlow Controller 的作用是 给个Router或者Switcher 中发布 flow table, 或者更新他 


![](image/Pasted%20image%2020241112203921.png)

![](image/Pasted%20image%2020241112203934.png)

![](image/Pasted%20image%2020241112203944.png)


### 1.2.1 OpenFlow abstraction


match+action : abstraction unifies different kinds of devices

![](image/Pasted%20image%2020241112204013.png)


### 1.2.2 OpenFlow Example 

![](image/Pasted%20image%2020241112204125.png)


![](image/Pasted%20image%2020241112204156.png)




SDN (Software-Defined Networking) ist eine Netzwerkarchitektur, bei der die Steuerung und Konfiguration des Netzwerks von der eigentlichen Weiterleitung der Datenpakete (also der "Data Plane") getrennt wird. 
SDN bietet eine flexible, zentralisierte Steuerung über das Netzwerk und ermöglicht eine dynamische, anpassungsfähige Verwaltung von Netzwerkressourcen.





