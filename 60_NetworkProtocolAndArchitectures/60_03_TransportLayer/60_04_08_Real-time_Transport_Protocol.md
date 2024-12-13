

是针对 radio, video 的传输 

Applications like Internet radio, Internet telephony, music-on-demand, videoconferencing, video-on-demand, and other multimedia applications became more commonplace
- Problem: each application was reinventing more or less the same realtime
- transport protocol!
- Idea: having a generic real-time transport protocol for multiple applications


RTP (Real-time Transport Protocol) provides support for sending realtime media over UDP
- Often implemented as part of the application & running in user space over UDP

# 1 RTP operation


RTP operates  as follows:
- ==A multimedia application consists of multiple audio, video, text, and other streams==
- Streams are fed into RTP library
- Library multiplexes streams, encodes them in RTP packets & stuffs them into a socket
- On operating system side of socket: UDP packets are generated to wrap RTP packets and handed to IP for transmission
- Reverse process happens at receiver side
- Multimedia application receives multimedia data from RTP library & is responsible for playing out the media


RTP provides support for sending real-time media over UDP
- Often implemented as part of the application
- (a) the position of RTP in the protocol stack, (b) packet nesting

![](image/Pasted%20image%2020241213073508.png)

# 2 Basic function of RTP

Basic function of RTP: ==multiplexing of several real-time data streams onto a single stream of UDP packets==
- UDP stream can be sent to a single destination (unicasting) or to multiple destinations (multicasting)
- Note:
    - RTP just uses normal UDP => packets are not treated specially by routers (except IP quality-of-service features is enabled)
    - No special guarantees about delivery => packets may be lost, delayed, corrupted, etc. 

# 3 RTP format

RTP format contains several features to help receivers work with multimedia information: e.g., **packet numbering**
- Allows destination to determine if any packets are missing 
- If packet is missing => application needs to decide what to do: e.g.,
    - Packet carrying video data: skip video frame
    - Packet carrying audio data: approximate missing value by interpolation
- Note: ==retransmission is not a practical option== since the retransmitted packet would probably arrive too late to be useful!
    - Hence RTP has no ACKs & no mechanism to request retransmissions

RTP format contains several features to help receivers work with multimedia information: e.g., **timestamping**
- 每个 segment of packet 配有 timestamp. destination 不管什么时候收到的这些 segment都可以 根据他的timestamp 去 播放整个 stream 
- Allow source to associate a timestamp with first sample in each packet => timestamps are relative to start of stream
- Mechanism allows destination to do buffering and play each sample at the right time after start of stream, independently of when the packet containing the sample arrived:
    - Helps to reduce the effects of variation in network delay & allows multiple streams to be synchronized with each other


# 4 RTP header 

RTP header contains fields to describe the type of media and synchronize it across multiple streams

![](image/Pasted%20image%2020241213104906.png)


# 5 Real-time Transport (RTP)  Control Protocol (RTCP)

 adapt data rate according the network quality 

- RTCP is a control protocol:
    - Handles feedback, synchronization, and the user interface
    - It does not transport any media samples!
- Can be used to provide feedback on delay, variation in delay or jitter, bandwidth, congestion, and other network properties to sources:
    - Information used by encoding process to adapt data rate (i.e., quality) to network quality => best quality possible under current network circumstances


# 6 Real-Time Protocol – Buffering

Buffer at receiver is used to delay packets and absorb jitter so that streaming media is played out smoothly



![](image/Pasted%20image%2020241213105549.png)



High jitter, or more variation in delay, requires a larger playout buffer to avoid playout misses
Propagation delay which does not affect buffer size

![](image/Pasted%20image%2020241213105736.png)


