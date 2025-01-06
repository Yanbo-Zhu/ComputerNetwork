
scenario: student attaches laptop to campus network, requests/receives www.google.com 
arriving mobile client attaches to network …
requests web page: www.google.com


![](image/Pasted%20image%2020241205105253.png)


# 1 get IP address through DHCP 


![](image/Pasted%20image%2020241205105412.png)

![](image/Pasted%20image%2020241205105924.png)


Client get  it's own IP address, knows name & Adresse of DNS server, IP address of its first-hop router
- connecting laptop needs to get its own IP address, Adresse of first-hop router, Adresse of DNS server: use DHCP
    - first-hop router client 是第一个遇到的 router 
- DHCP request encapsulated in UDP, encapsulated in IP, encapsulated in 802.3 Ethernet
- Ethernet frame broadcast (destination: FFFFFFFFFFFF) on LAN, received at router running DHCP server
- Ethernet demuxed  解复用 to IP demuxed, UDP demuxed to DHCP
- DHCP server formulates DHCP ACK containing client’s IP address, IP address of first-hop router for client, name ,  IP address of DNS server
- encapsulation at DHCP server, frame forwarded (switch learning) through LAN, demultiplexing at client
- DHCP client receives DHCP ACK reply

Client now has IP address, knows name & addr of DNS  server, IP address of its first-hop router

# 2 using ARP to obtain the MAC address of first hop router (before DNS, before HTTP)


ARP:  address resolution protocol , 用来 找到 mac of router interface (first hop router,)

![](image/Pasted%20image%2020241205110807.png)


- before sending HTTP request, need IP address of www.google.com:  DNS
- DNS query created, encapsulated in UDP, encapsulated in IP, encapsulated in Eth.  To send frame to router, need MAC address of router interface: ARP
- ARP query broadcast, received by router, which replies with ARP reply giving MAC address of router interface
- client now knows MAC address of first hop router, so can now send frame containing DNS query

# 3 using DNS to get the IP address of www.google.com 

![](image/Pasted%20image%2020241205113037.png)

- IP datagram containing DNS query forwarded via LAN switch from client to 1st hop router
- IP datagram forwarded from campus network into Comcast network, routed (tables created by RIP, OSPF, IS-IS and/or BGP routing protocols) to DNS server
- frame is demuxed 解复用 to DNS message 
- DNS replies to client with IP address of www.google.com 

# 4 TCP connection carrying HTTP


![](image/Pasted%20image%2020241205113323.png)

- to send HTTP request, client first opens TCP socket to web server
- TCP SYN segment (step 1 in TCP 3-way handshake) inter-domain routed to web server
- web server responds with TCP SYNACK (step 2 in TCP 3-way handshake)
- TCP connection established!


# 5 HTTP request/reply 

![](image/Pasted%20image%2020241205113345.png)



- HTTP request sent into TCP socket
- IP datagram containing HTTP request routed to www.google.com
- web server responds with HTTP reply (containing web page)
- IP datagram containing HTTP reply routed back to client
