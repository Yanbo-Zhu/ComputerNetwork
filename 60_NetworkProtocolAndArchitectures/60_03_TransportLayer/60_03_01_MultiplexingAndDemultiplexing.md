

multiplexing at sender:
handle data from multiple sockets, add transport header (later used for demultiplexing)

demultiplexing at receiver
use header info to deliver received segments to correct socket


![](image/Pasted%20image%2020241207122928.png)


# 1 How demultiplexing works

- host receives IP datagrams
    - each datagram has source IP address, destination IP address
    - each datagram carries one transport-layer segment
    - each segment has source, destination port number
- host uses IP addresses & port numbers to direct segment to appropriate socket

![](image/Pasted%20image%2020241207123223.png)


# 2 UDP 和 TCP Demultiplexing 的不同 

- Multiplexing, demultiplexing: based on segment, datagram header field values
- UDP: demultiplexing using destination port number (only)
    - ==IP/UDP datagrams with same dest. port , but different source IP addresses and/or source port numbers will be directed to same socket at receiving host==  只要他们 use same destination port number,  不管他们的 source IP 和 source port number 一不一样, 他们都会被用 同一个 socket at reviceiver's side 被处理 
- TCP: demultiplexing using 4-tuple: source and destination IP addresses, and port numbers
    - Three segments, all destined to IP address: B, dest port: 80 are demultiplexed to different sockets
        - ==segments with same destination IP address and same dest. port 会备用不同的 socker 去处理 ==
- Multiplexing/demultiplexing happen at all layers

# 3 Connectionless demultiplexing (UDP)

when receiving host receives UDP segment:
- checks destination port # in segment
- directs UDP segment to socket with that port 

IP/UDP datagrams with same dest. port #, but different source IP addresses and/or source port numbers will be directed to same socket at receiving host

![](image/Pasted%20image%2020241207123326.png)


![](image/Pasted%20image%2020241207123339.png)


# 4 Connection-oriented demultiplexing


- TCP socket identified by 4-tuple:
    - source IP address
    - source port number
    - dest IP address
    - dest port number
- demux: receiver uses all four values (4-tuple) to direct segment to appropriate socket
- server may support many simultaneous TCP sockets:
    - each socket identified by its own 4-tuple
    - each socket associated with a different connecting client

![](image/Pasted%20image%2020241207123845.png)