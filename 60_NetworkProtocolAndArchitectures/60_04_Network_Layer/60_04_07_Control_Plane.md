


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


# 3 Routing protocols

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


## 3.1 link state

![](image/Pasted%20image%2020241112200118.png)


## 3.2 distance vector

![](image/Pasted%20image%2020241112200336.png)





## 3.3 Comparison of LS and DV algorithms

![](image/Pasted%20image%2020241112200427.png)
