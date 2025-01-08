
![](image/Pasted%20image%2020241208182554.png)

QUIC is a relatively new transport layer protocol. Version 1.0 of it has been standardized by the IETF in RFC9000 in 2021. It is based on UDP and offers multiplexed and secure Communications. It solves several of the issues the traditional transport stack (HTTP + TLS + TCP) has.


- Most web traffic uses same stack of HTTP(2) – TLS – TCP 
- Since the introduction of TCP internet has changed vastly
- A lot of “hacks” and issues surrounding TCP
    - Using TLS over TCP incurs two handshakes which leads to longer load times and lower QoE 
    - Using multiple connections to load a website
    - Head of line blocking 
    - Users are now mobile, e.g. changing networks, leading to broken TCP connections
- Other attempts at transport protocol design have been done over the years, e.g. SCTP


# 1 

Answer the following questions about the core concepts of QUIC and how they help alleviate these issues.
(a) What is Head-of-line (HOL) blocking ? Briefly explain how HOL blocking can cause issues when loading a web page.
(b) Which mechanism in QUIC prevents HOL blocking? Name the mechanism and briefly describe how it dealing with HOL blocking.
(c) End users are much more mobile today compared to when TCP was developed. Which consequences does this increased mobility have on the transport layer? What possible issues does this cause for connections?
(d) Which mechanism in QUIC deals with the issues caused by user mobility? Name the mechanism and briefly explain how it prevents the issues from occurring.
(e) User privacy and security are a top priority in toady’s internet. TLS is a protocol that is commonly used to secure transport layer connections. However, if you use TLS over TCP you introduce an issue. What is this issue and how


# 2 理论 

QUIC 就是给 UDP 加入 error and congestion control 和 connection establishment 


- QUIC is a UDP based reliable transport protocol, provides similar guarantees as TCP
- Specified in RFC 9000 (among others); with upgradeability in mind
- Uses variable bit-length encoding for most header fields for efficiency
- Uses UDP packets allowing easier implementations and deployment
    - Prevents compatibility issues with middleware (routers, switches etc.) that stifled SCTP
    - Can be implemented entirely in user space
- Along with QUIC, new version of HTTP (HTTP/3) was developed and introduced
    - Includes further improvements, not part of this lecture
- QUIC quickly  gained support by most big vendors and developers
    - Browsers (Chromium, Safari, Firefox etc.)
    - Languages (Rust, C, Python, Go etc.)




# 3 application-layer protocol, on top of UDP

- increase performance of HTTP
- deployed on many Google servers, apps (Chrome, mobile YouTube app)

![](image/Pasted%20image%2020241208182624.png)

![](image/Pasted%20image%2020241210233241.png)



# 4 Frames

 - QUIC uses various Frame types to handle different types of messages
 - They are the basic “building blocks” of QUIC 
 - STREAM frames carry (reliable) application data 
 - CRYPTO frames used to exchange cryptographic information (e.g. TLS)
 - ACK frames for sending acknowledgements 
 - NEW_TOKEN frames for sending new tokens for future connection
 - Many more in base spec as well as others introduced in extensions
     - Noticeably, DATAGRAM frames allow for sending of unreliable application data


![](image/Pasted%20image%2020241210234354.png)


# 5 Packet 


- ==One or more frames combined get put into a packet==
- Packets must fit into UDP datagrams, but a single datagram can hold several packets
    - QUIC required a minimum UDP datagram size of 1280 bytes
- Packets include headers that uniquely associate them with a specific QUIC connection
- There are different header formats, depending on type of packet
    - Long header packets used for initial establishment of keys
        - Initial, Handshake, Version negotiation, Crypto, 0-RTT etc.
    - Short header packets used for most other packets afterwards


![](image/Pasted%20image%2020241210234812.png)



# 6 QUIC Handshake: Connection establishment 

- adopts approaches we’ve studied in this chapter for connection establishment, error control, congestion control
    - error and congestion control: “Readers familiar with TCP’s loss detection and congestion control will find algorithms here that parallel well-known TCP ones.” 
    - connection establishment: reliability, congestion control, authentication, encryption, state established in one RTT

- QUIC handshake includes TLS handshake; only 1-RTT required before data can be sent
- During handshake, several things are negotiated
    - QUIC version
    - Cryptographic parameters (similar to TLS)
    - Various other transport parameters (max idle timeout, max payload size etc.) 
- Server authenticates itself to client
- All data after handshake is encrypted 
- Like TLS, tickets can be issued to allow for 0-RTT data

Solves issue of duplicate handshake that is required with TLS + TCP



![](image/Pasted%20image%2020241208182905.png)

![](image/Pasted%20image%2020241210234957.png)


![](image/Pasted%20image%2020241210235007.png)


# 7 Head-of-line (HOL) blocking

(a) What is Head-of-line (HOL) blocking ? Briefly explain how HOL blocking can cause issues when loading a web page.


Head-of-Line (HOL) blocking occurs when a packet at the front of a queue prevents other packets behind it from being processed, even if those packets could otherwise proceed. This phenomenon is common in network communication protocols where resources are allocated in a sequential manner.

HOL blocking can manifest in two key contexts:

1. **Switches and Routers:** In network switches, if a packet at the head of a queue is delayed due to congestion or unavailability of the destination, subsequent packets in the queue are also delayed, even if their paths are clear.
2. **Application Protocols (e.g., HTTP/1.1):** When a single request-response pair blocks subsequent requests due to resource contention or serialization of processing.

How HOL Blocking Causes Issues When Loading a Web Page
In the context of web pages, HOL blocking often occurs in **HTTP/1.1** because it processes requests in a sequential manner (over a single TCP connection). Here's how it causes problems:

1. **Sequential Request Handling:**
    - Web pages often consist of multiple resources (e.g., HTML, CSS, JavaScript, images).
    - In HTTP/1.1, multiple requests are serialized over a single TCP connection, meaning a delay in processing one request blocks subsequent requests.
2. **Resource Loading Delays:**
    - If the first resource (e.g., a large image) takes longer to download due to congestion or server processing time, all subsequent resources must wait until the first one is complete.
3. **Impact on User Experience:**
    - The browser cannot render parts of the page that depend on blocked resources (e.g., CSS or JavaScript).
    - This leads to increased page load times and a degraded user experience.

Modern Solutions to Avoid HOL Blocking
To mitigate HOL blocking, newer protocols and techniques have been developed:
1. **HTTP/2:**
    - Supports multiplexing, allowing multiple requests to be sent concurrently over a single connection.
2. **HTTP/3:**
    - Uses QUIC, a UDP-based protocol, which avoids HOL blocking at the transport layer.
3. **Parallel Connections:**
    - Browsers often open multiple TCP connections to fetch resources concurrently.

By addressing HOL blocking, these techniques significantly improve web page load times and overall user experience.



# 8 Streams

可以同时有多个 streams , 并行的, 来传输数据 

- multiple application-level “streams” multiplexed over single QUIC connection
    - separate reliable data transfer, security
    - common congestion control

- QUIC allows endpoints to create multiple streams
- Each stream is an abstraction for a byte stream
- Streams can be unidirectional or bidirectional
- Streams have priorities; allowing for preferable treatment of data
- Each stream has its own flow control (in addition to connection flow control)
- Retransmissions also happen per stream
- Example use cases are one stream per website resource or one stream for data and one for control

Solves issue of HoL blocking and the need for multiple connections to a website


![](image/Pasted%20image%2020241208182914.png)



# 9 Connection migration

- Connections in TCP or UDP are identified by the 5-tuple (Source IP, Source Port, Destination IP, Destination Port, Protocol) 
- Instead, QUIC introduces connectionIDs that allow servers to assign clients unique IDs with a certain lifetime
- Each packet includes that assigned ID  (就是 connectionIDs)
- If the 5-tuple changes (for example due to NAT rebinding or mobility), the connection can be kept alive as the server is still able to associate the client by its assigned connection ID

Solves issue of mobility and the loss of connections if the 5-tuple changes 


# 10 Other mechanisms 

- QUIC Includes many other mechanisms that help it be more efficient, secure and reliable
- Uses Congestion and Flow control like TCP
    - Uses mostly the same algorithms as TCP does (Reno, BBR, Cubic)
    - Also supports ECN (and thus L4S)
- Spin-bit allows for latency measurements
- Keys get rotated to increase security
- Addition of new frames allows for easy extendibility
    - Already exist several major ones, including multipath and datagram
- Some downsides of QUIC compared to TCP:
    - ACKs cause much more overhead; as a result, not suitable for large file transfer (so far)
    - Much, much more complexity, both for implementors and application developers

