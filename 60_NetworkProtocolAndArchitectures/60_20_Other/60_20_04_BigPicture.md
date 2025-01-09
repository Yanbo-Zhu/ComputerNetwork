
End-to-end argument: Dumb network, smart endpoints
Internet design philosophy: Survivability, types of supported services, cost efficiency, …
How has the Internet changed: Trustworthiness, filtering, third-parties, …
Firewalls: Packet-filtering
Ossification 骨化；成骨；（思想的）僵化 : Solutions: Encryption, greasing


# 1 Designing a network architecture

- Designing a network architecture
- How to decompose the complex functionality?
- Where to place functionality?
    - End system or network?
    - On which layer(s)
- Same functionality on multiple layers?

What were the original design goals of the Internet?
Have they been met?
What has changed?
What are current challenges?


how to decompose functionality
- Protocol layers
- Functionality implemented at a layer
    - Reliability: Transport Layer (e.g., TCP)
    - Medium Access Control: Link Layer (e.g., Ethernet)

![](image/Pasted%20image%2020250109145546.png)

Where to place functionality
On the endpoints?
Or within the network?



“E2E argument”: End-to-end argument

Place functions at the higher level
- Higher level = higher in the stack
- Higher in the stack = on the endpoints

- Placing at the lower level “may be redundant or of little value”
- “…Sometimes an incomplete version […] may be useful as a performance enhancement…” (Saltzer, J. H., D. P. Reed, and D. D. Clark (1981) "End-to-End Arguments in System Design“)

- Internet philosophy opposite to the telephone world (Intelligent network vs. dumb network)

# 2 Reliable File Transfer in which layer 

Solution 1: Implement in Operating System („lower level“)
Reliability at each „step“ between devices
What happens if the Operating System fails?
Receiver has to do checks anyway!

Solution 2: Implement in Application („higher level“)
Each „step“ unreliable, but end-to-end reliability between hosts
Reliability can be entirely implemented on application layer

![](image/Pasted%20image%2020250109153058.png)


Reliability at Lower Layers and Trade-off
- Reasons to implement reliability at lower layers anyways? (according to the “End-to-End Arguments” paper)
    - Only to improve performance!
    - E.g., with high error rate in network: Faster detection/recovery of errors
- Example: WiFi
    - Link Layer
    - Implements acknowledgements and retransmission
- Trade-off: Is the performance benefit worth the cost?

# 3 Internet End-to-End Argument

- Network layer provides one simple service
    - Best effort datagram (packet) delivery
- Transport layer at network edge (TCP)
    - Provides end-end error control
    - Performance enhancement used by many applications
        - Could provide their own error control
        - But: Every application would have to “reinvent the wheel”

---

Congestion Control

Reminder:
- Avoid “too many sources sending too much data”
- Ensures network does not collapse

Implemented on Transport Layer
Why not on Application Layer?
- Sharing implementations (reduces bugs, less work, etc…)
- Knowing everyone is doing the same thing

Congestion control too important to leave up to application/user!
- But: Hard to police
- Applications could still implement their own, or “cheat”

---

End-to-end principle
- Place functionality higher in the stack
- Higher layers = on the endpoints
- Repeat on lower layers only if …
    - …used by/improves performance of many apps
    - …it does not hurt other applications

Higher layers (endpoints) are complex (“smart”)
Lower layers (networks) provide simple service (“dumb”)
➔ Success story: Internet



# 4 Internet Design Philosophy

According to Clark (1988), in order of importance
1. Connect existing networks (ARPANET, ARPA packet radio network)
2. Survivability
3. Support multiple types of services
4. Accommodate a variety of networks
5. Allow distributed management
6. Allow host attachment with a low level of effort
7. Be cost effective
8. Allow resource accountability

Different ordering of priorities may make a different architecture!
Let’s grade the internet with these requirements


---
2. Survivability

- Continue to operate even with network failures
    - E.g. link and router failures
    - As long as network is not partitioned, two endpoints should be able to communicate
    - Failure should be transparent to endpoints
- Realization of survivability
    - Maintain E2E transport state only at end-points
    - Router fails → no state inconsistency
        - ➔ Internet: Stateless network architecture
        - No notion of a session/call at network layer

Grade: A-
- Because convergence times are relatively slow
- BGP takes minutes to converge
- IS-IS OSPF take ~ 10 seconds


---

3. Types of Services

- Different transport protocols for different application requirements
    - E.g. UDP for “real-time” applications
    - Arguably main reason for separating TCP from IP
    - Possible to build other services (but difficult in practice…)
- Different “service classes”
    - When “best-effort” is not enough
    - E.g., priority queueing for real-time traffic
    - Never deployed on a large scale
        - Trust the user or another network to mark their own traffic as “high priority”?

Grade: A-


---

4. Variety of Networks

- The mantra: IP over everything
    - Then: ARPANET, X.25, DARPA satellite network, ...
    - Now: ATM, SONET, WDM, …
- IP only “best-effort” datagram delivery
    - ➔ Requires from underlying network only to deliver a packet with a “reasonable” probability of success
    - ➔ That is a pretty low bar.

Grade: A
- Can’t name a link layer technology that IP doesn’t run over
- (Even the carrier pigeon RFC)

---

5. Distributed Management

- Administrative autonomy: IP interconnects networks
- Each network can be managed by a different organization
- Different organizations need to interact only at the boundaries
- ➔ Intra-Domain VS. Inter-Domain Routing

Grade: A


---
6. New Hosts

- Cost of attaching a new host
    - Historically, not a strong point
    - Higher than in other architectures because the intelligence is within hosts
        - ➔ Telephone vs. computer
    - Now easier with DHCP and auto-configuration

- Bad implementations or malicious users can produce considerable harm
    - See also: Internet of Things
- Grade: A-

---
7. Cost Efficiency and 8. Accountability

- Cost efficiency
    - Sources of inefficiency
        - Header overhead
        - Retransmissions
        - Routing
    - But “optimal” performance never been top priority
    - Grade: A

- Accountability 审计 承担责任的程度 
    - Easy to spoof (fake) a sender address
    - Easy to use up lots of resources: Distributed Denial-of-Service (DDoS) attacks
    - Grade: F


---

Minimalist Approach: Summary
IP meets most of its design goals
All hosts and routers run IP
Advantages
- Run over different links (Ethernet, modem, satellite, wireless)
- Support diverse applications (Web, Video streaming, …)
- Decentralized network administration


# 5 How has the Internet changed

All five changes may motivate shift away from end-end!

Operation in an untrustworthy world
- Internet was designed unencrypted
- Both endpoints and network can be malicious  恶意的
- Surveillance, fraud 欺诈，骗局，诡计；骗子；伪劣品，冒牌货 , DDoS attacks…
    - ➔ Need encryption, authentication, privacy
    - ➔ More intelligence in the network? (e.g., filter out malware, block malicious traffic)

More demanding applications
- Traffic volume grows
    - ➔ P2P, CDNs
- Internet-wide “Quality of Service” guarantees failed (IntServ, DiffServ)

ISP competition
- ISP doing more (than other ISPs) in core
- E.g., sell you DDoS protection filtering
- Maybe a competitive advantage

Rise of third party involvement
- Interposed 提出（异议等）；使插入；使干涉；干预；调停 between endpoints (even against will)
- Usually via ISPs of the country in question
- E.g. Chinese gov’t, US recording industry

Less sophisticated users

---

The conventional understanding of the Internet philosophy
- Freedom of action
- User empowerment  授权，赋权
- End-user responsibility for actions taken
- Lack of control “in” the network that limit or regulate what users can do

“The end-end argument fostered 促进，培养；领养，收养 that philosophy because they enable the freedom to innovate, install new software at will, and run applications of the users choice”


---

Consolidation

Consolidation = merging or integration of many items into one
In the Internet:
- Large, centralized entities (with „global“ network)
- More and more traffic moving towards them
- E.g.: Content providers
- E.g.: Browser engines

Less choice for users
High barrier to market entry
➔ Danger to innovation?


---

Technical Response to Changes

Modify endpoints
- “Encrypt everything”
- Harden endpoints against attack
- Content filtering, virus scanners…
- Distribute servers globally (CDNs)

Add functions to the network core
- Filtering firewalls
- Application-level firewalls
- NAT (also as security feature)
- Network virtualization
    - ➔ “Middleboxes”


# 6 Firewalls and Middlebox 



Firewalls: Packet-filtering
- Isolates organization’s internal network from larger Internet
- Filters packets based on:
    - Source/Destination IP, TCP/UDP port, TCP flags, …
    - E.g.: Block port 23 (telnet)
- Common philosophy: Block everything, only allow selectively
    - ➔ Treat all unknown traffic as an attack
![](image/Pasted%20image%2020250109201547.png)

Middlebox
Device that “transforms, inspects, filters, or otherwise manipulates traffic for purposes other than packet forwarding” [RFC 3234]
- Often on higher layer than “usual”
- E.g., Firewall (filters also based on Transport Layer headers)
- E.g., NAT (transforms IP and Transport Layer headers)

![](image/Pasted%20image%2020250109201757.png)


# 7 Ossification 

骨化；成骨；（思想的）僵化 : 
Solutions: Encryption, greasing

Middleboxes make the network „smart“
➔ Break the end-to-end principle

Middleboxes filter or break traffic they “don’t know”
Middleboxes do not get updated very often
➔ Break the end-to-end principle

Example: Innovation on Transport Layer
- TCP Fast Open – available for years, but blocked by middleboxes
- SCTP – New transport protocol, more features, but not used in the Internet (also due to middleboxes)

Protocol “freezes” in its old features: Ossification

Solution: Encryption
- Middleboxes can not read packet content
    - Can’t make (unwanted) decisions based on content
- Some parts have to remain in clear though
    - Addresses
    - Flags
    - Connection ID needed for reliable protocols
        - ➔ Other side needs to find corresponding decryption key
- Example: QUIC (Quick UDP Internet Connection)
    - New transport protocol
    - Standardized by the IETF (not by Google)

Solution: Greasing 润滑；涂油；起油腻
- Change a certain header field randomly sometimes
- E.g., QUIC decoy 诱饵；诱骗 values in its version negotiation
    - ➔ Avoid implementations to assume that these values will never change