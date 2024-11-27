
link layer has responsibility of transferring datagram from one node to physically adjacent node over a link
the data link layer provides mechanisms for converting packets to frames while the physical layer converts frames to bits which are then transmitted over the physical media


# 1 terminology:

hosts and routers: nodes

communication channels that connect adjacent nodes along communication path: links
- wired
- wireless
- LANs

layer-2 packet: frame, encapsulates datagram


# 2 context

Datagram transferred by different link protocols over different links:
• e.g., WiFi on first link, Ethernet on next link
▪ each link protocol provides different services
• e.g., may or may not provide reliable data transfer over link

Each link protocol provides different services
• e.g., may or may not provide reliable data transfer over link

transportation analogy:
▪ trip from Princeton to Lausanne
• limo: Princeton to JFK
• plane: JFK to Geneva
• train: Geneva to Lausanne
▪ tourist = datagram
▪ transport segment = communication link
▪ transportation mode = link-layer protocol
▪ travel agent = routing algorithm

# 3 services

- framing, link access:
    - encapsulate datagram into frame, adding header, trailer channel access if shared medium
    - “MAC” addresses in frame headers identify source, destination (different from IP address!)
- reliable delivery between adjacent nodes
    - we already know how to do this!
    - seldom used on low bit-error links
    - wireless links: high error rates
        - Q: why both link-level and end-end reliability?
- flow control:
    - pacing between adjacent sending and receiving nodes
- error detection:
    - errors caused by signal attenuation, noise.
    - receiver detects errors, signals retransmission, or drops frame
- error correction:
    - receiver identifies and corrects bit error(s) without retransmission
- half-duplex and full-duplex:
    - with half duplex, nodes at both ends of link can transmit, but not at same time


# 4 Where is the link layer implemented?

▪ in each-and-every host
▪ link layer implemented in network interface card (NIC) or on a chip  被网卡执行 传输 
    • Ethernet, WiFi card or chip
    • implements link, physical layer
▪ attaches into host’s system buses
▪ combination of hardware, software, firmware

![](image/Pasted%20image%2020241127210816.png)


# 5 Interfaces communicating

![](image/Pasted%20image%2020241127211203.png)

sending side:
▪ encapsulates datagram in frame
▪ adds error checking bits, reliable data transfer, flow control, etc.

receiving side:
▪ looks for errors, reliable data transfer, flow control, etc.
▪ extracts datagram, passes to upper layer at receiving side






