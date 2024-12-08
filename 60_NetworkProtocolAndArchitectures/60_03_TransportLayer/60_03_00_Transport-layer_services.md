
# 1 Transport Layer包含了什么 

- provide logical communication between application processes running on different hosts
- transport protocols actions in end systems:
    - sender: breaks application messages into segments, passes to network layer
    - receiver: reassembles segments into messages, passes to application layer
- two transport protocols available to Internet applications
    - TCP, UDP


# 2 Transport Layer 是用来干什么的 

- Ultimate goal of transport layer:  provide efficient, reliable, and cost-effective data transmission service to its users (=> processes in application layer)
- Transport layer makes use of services provided by network layer
- Transport entity:
    - Software (hardware) within transport layer that does the actual work
    - Located in either operating system kernel, library package bound into network applications, separate user process, or inside network interface card



# 3 network layer and Transport Layer
network layer: 
- logical communication between hosts
- provides end-to-end packet delivery using datagrams or virtual circuits


transport layer: 
- logical communication between processes
- relies on, enhances, network layer services
- Transport layer builds on network layer to provide data transport from a process on a source machine to a process on a destination machine with a desired level of reliability (independent of used physical network)
- It provides the abstractions that applications need to use the network

if transport layer service is so similar to network layer service, why are there two distinct layers? (重要)
- Transport code runs entirely on users’ machines, but network layer mostly runs on routers (operated by carrier) => what happens if network layer offers inadequate service? (packet loss, router crash) => user in no control of network layer
- Existence of transport layer makes it possible for the transport service to be more reliable than the underlying network!!
- Practical issues: transport primitives can be implemented as calls to library procedures to make them independent of the network primitives (different network service calls in connectionless Ethernet vs. connection-oriented WiMAX) => application programmers can write code according to a standard set of primitives

The key practical issue here is the abstraction of transport primitives to ensure network-independence for application developers.

- **Transport Primitives as Library Procedures**  
    Transport primitives (e.g., `send`, `receive`, `connect`, `disconnect`) are implemented as part of a library, rather than being hardwired to specific network protocols or services. This abstraction provides a **uniform interface** regardless of the underlying network technology.
- **Network Independence**  
    Different network technologies (e.g., Ethernet, which is connectionless, versus WiMAX, which is connection-oriented) use distinct sets of service calls. By mapping these network-specific operations to a **standard set of primitives**, the complexity of network-specific details is hidden from application programmers.
- **Benefits for Application Development**
    - **Portability**: Code written using the abstract transport primitives can run on any network type without modification.
    - **Ease of Use**: Application developers can focus on application logic without worrying about low-level network details.
    - **Flexibility**: The underlying implementation can change (e.g., from WiMAX to Ethernet) without requiring changes to application code.

Example:
Suppose an application developer uses a `connect()` primitive to establish communication.
- On a WiMAX network, `connect()` might internally map to a sequence of service calls for connection establishment.
- On Ethernet, `connect()` might map to a no-op or a different procedure since Ethernet is inherently connectionless.



# 4 **传输原语（Transport Primitives）** 


是计算机网络中用于描述传输层提供的基本操作的一组标准化接口。它们通常由操作系统或网络协议库实现，提供了抽象的传输功能，隐藏了底层网络协议的复杂性

传输原语的主要功能：
1. **连接控制**
    - **connect（连接）**：发起并建立到远程主机的连接（用于面向连接的网络，例如 TCP）。
    - **disconnect（断开连接）**：终止连接会话。
2. **数据传输**
    - **send（发送）**：向远程主机发送数据。
    - **receive（接收）**：从远程主机接收数据。
3. **错误控制和通知**
    - 提供网络故障、连接超时或数据传输失败的反馈。


为什么需要传输原语？
传输原语将网络通信的复杂性封装起来，提供了一种标准化的方式让程序员在开发应用程序时，不必直接处理底层协议（如 TCP/IP 或 UDP）的细节。这种抽象使得应用程序可以**跨多种网络协议**工作，而无需更改代码。


假设一个应用程序需要传输文件：

- 开发者调用 `connect()` 建立连接（在 TCP 网络上，这可能触发三次握手）。
- 调用 `send()` 发送文件数据。
- 调用 `receive()` 接收确认或其他响应。
- 最后调用 `disconnect()` 关闭连接。

对于底层网络（例如以太网或 WiMAX），这些原语可能映射到完全不同的操作，但对应用程序开发者来说，接口始终是统一的。

通过这种抽象，开发人员可以**专注于应用逻辑**，而不是底层网络实现的差异性。


---
英文解释 什么是 Transport Service Primitives 

- Primitives that applications might call to transport data for a simple (hypothetical) connection-oriented service:
    - Bare bone but sufficient allowing application programs to establish, use, and then release connections:
- Usage of primitives: client-server model
    - Client calls CONNECT, SEND, RECEIVE, DISCONNECT
    - Server calls LISTEN, RECEIVE, SEND, DISCONNECT

![](image/Pasted%20image%2020241206105317.png)

![](image/Pasted%20image%2020241206105324.png)

---

State diagram for a simple connection-oriented service:
- Solid lines (right) show client state sequence
- Dashed lines (left) show server state sequence
- Transitions in italics are due to segment arrivals

Note: model is quite unsophisticated => next a more realistic model will be presented

![](image/Pasted%20image%2020241206105622.png)


## 4.1 transport service interface

- To allow users to access transport service, transport layer must provide some operations to application programs => transport service interface
- Real networks can lose packets => network service is generally unreliable
- But connection-oriented transport service is reliable
- Purpose of transport layer— provide a reliable service on top of an unreliable network!
    - Goal of connection-oriented transport service: hiding imperfections of network service so that user processes can just assume the existence of an `error-free bit stream` even when they are on `different machines`!



# 5 Connection and Connectionless-oriented transport service

There are also two types of transport service:
- Connection-oriented transport service is similar to the connection-oriented network service
    - In both cases, connections have three phases: establishment, data transfer, and release
    - Addressing and flow control are also similar in both layers
- Connectionless transport service is also very similar to the connectionless network service
    - But meaningless to provide a connectionless transport service on top of a connection-oriented network service


- TCP: Transmission Control Protocol
    - reliable, in-order delivery
    - congestion control
    - flow control
    - connection setup
- UDP: User Datagram Protocol
    - unreliable, unordered delivery
    - no-frills extension of “best-effort” IP



# 6 Sockets: the Transport Layer API

- Sockets = another set of transport primitives => used, e.g., for TCP
- Sockets provide an API for programming networks at the transport layer.
- A socket is an endpoint of a two-way communication link between two processes located on the same machine - or located on different machines connected by a network.
- Network communication using Sockets is similar to file I/O:
    - Socket handle is treated like file handle
    - The streams used in file I/O operation are also applicable to socket-based I/O
- Socket-based communication principles are programming language independent!
- The success of this API is based on its abstraction of all possible used network protocols/underlying network topology.
- A socket is bound to a port number so that the transport protocol can identify the application (= process) that data destined to be sent.



## 6.1 Socket Layer

![](image/Pasted%20image%2020241206110251.png)



## 6.2 Caution: Sockets support multiple Domains


- Domain defined while socket is created!
    - int socket(int domain, int type, int protocol)
- domain is PF_UNIX, PF_INET, PF_OSI, etc.
    - PF_INET is for communication on the internet to IP addresses
- type is either SOCK_STREAM (connection oriented, reliable), SOCK_DGRAM, or SOCK_RAW
    - originally more types have been envisioned 想象，预想
- protocol specifies the protocol used. It is usually 0 to say we want to use the default protocol for the chosen domain and type (note IP 4 vs. IP 6)
- While nowadays INET Domain/protocols prevail 占优势，占上风, the approach is more general.


## 6.3 Berkeley Sockets for Client-Server

Berkeley sockets is a Unix API for Internet sockets and Unix domain sockets, used for inter-process communication (IPC).

![](image/Pasted%20image%2020241206111032.png)

# 7 Addressing(Port )

- port 的另外一个叫法Transport Service Access Point

- When an application process wishes to set up a connection to a remote application process, it must specify which one to connect to:
    - Same with connectionless transport: to whom should each message be sent?
- Method used: define transport addresses to which processes can listen for connection requests
    - Internet: endpoints called ports
    - Generic term is TSAP (Transport Service Access Point)
- Transport layer adds TSAPs
- Multiple clients and servers can run on a host with a single network (IP) address
- TSAPs are ports for TCP/UDP
![](image/Pasted%20image%2020241206111527.png)

A possible scenario for a transport connection is as follows:
1. Mail server process attaches itself to TSAP 1522 on host 2 to wait for an incoming call, e.g., LISTEN call
2. Application process on host 1 wants to send an email message, so it attaches itself to TSAP 1208 and issues a CONNECT request => request specifies TSAP 1208 on host 1 as source & TSAP 1522 on host 2 as destination => action results in a transport connection being established between application process and server
3. Application process sends mail message
4. Mail server responds to say that it will deliver the message
5. Transport connection is released



--- 

Port 
- Port is represented by a positive (16-bit) integer value
- Some ports have been reserved to support common/well known services: http 80/tcp; ftp 21/tcp; telnet 23/tcp; smtp 25/tcp;
- User level process/services generally use port number value >= 1024
    - 如果 使用了<1024 的port,  会报错, 因为没有 bereichtigung. 只有 kernel level 的 process 才可以使用 < 1024 的port 

![](image/Pasted%20image%2020241206111929.png)

![](image/Pasted%20image%2020241206111939.png)


# 8 Transport protocols resemble the data link protocols

- Both have to deal with error control, sequencing, and flow control, …
- But there is significant differences - the environments in which the two protocols operate
    - a) Data link layer: two routers communicate directly via a physical channel (wired or wireless)
    - b) Transport layer: physical channel is replaced by the entire network!

![](image/Pasted%20image%2020241206112649.png)














