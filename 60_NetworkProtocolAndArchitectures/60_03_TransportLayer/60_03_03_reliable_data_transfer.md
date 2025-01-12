
![](image/Pasted%20image%2020241208170717.png)


![](image/Pasted%20image%2020241208170846.png)


# 1 rdt1.0: reliable transfer over a reliable channel



underlying channel perfectly reliable
- no bit errors
- no loss of packets

separate FSMs for sender, receiver:
- sender sends data into underlying channel
- receiver reads data from underlying channel

![](image/Pasted%20image%2020241208171019.png)



# 2 rdt2.0: channel with bit errors


- underlying channel may flip bits in packet
    - checksum (e.g., Internet checksum) to detect bit errors
- the question: how to recover from errors?
- underlying channel may flip bits in packet
    - checksum to detect bit errors
- the question: how to recover from errors?
    - acknowledgements (ACKs): receiver explicitly tells sender that pkt received OK
    - negative acknowledgements (NAKs): receiver explicitly tells sender that pkt had errors
    - sender retransmits pkt on receipt of NAK
    - stop and wait: sender sends one packet, then waits for receiver response


## 2.1 FSM specification

![](image/Pasted%20image%2020241208171342.png)


Note: “state” of receiver (did the receiver get my message correctly?) isn’t known to sender unless somehow communicated from receiver to sender
that’s why we need a protocol!

![](image/Pasted%20image%2020241208171554.png)

![](image/Pasted%20image%2020241208171603.png)


## 2.2 rdt2.0 has a fatal flaw!

what happens if ACK/NAK corrupted?
- sender doesn’t know what happened at receiver!
- can’t just retransmit: possible duplicate

handling duplicates:
- sender retransmits current pkt if ACK/NAK corrupted
- sender adds sequence number to each pkt
- receiver discards (doesn’t deliver up) duplicate pkt

stop and wait: sender sends one packet, then waits for receiver response


# 3 rdt2.1: 
add adds sequence number with comprision of rdt 2.0 

sender, handling garbled ACK/NAKs
![](image/Pasted%20image%2020241208172314.png)


receiver, handling garbled ACK/NAKs
![](image/Pasted%20image%2020241208172353.png)


sender:
- seq # added to pkt
- two seq. ``#s (0,1)`` will suffice. Why?
- must check if received ACK/NAK corrupted
- twice as many states
    - state must “remember” whether “expected” pkt should have seq # of 0 or 1


receiver:
- must check if received packet is duplicate
    - state indicates whether 0 or 1 is expected `pkt seq #`
- note: receiver can not know if its last ACK/NAK received OK at sender


# 4 rdt2.2: a NAK-free protocol


- same functionality as rdt2.1, using ACKs only 
- instead of NAK, receiver sends ACK for last pkt received OK
    - receiver must explicitly include seq # of pkt being ACKed
- duplicate ACK at sender results in same action as NAK: retransmit current pkt
- As we will see, TCP uses this approach to be NAK-free

![](image/Pasted%20image%2020241208172903.png)


# 5 rdt3.0: channels with errors and loss

New channel assumption: underlying channel can also lose packets (data, ACKs)
- checksum, sequence `#s`, ACKs, retransmissions will be of help … but not quite enough

How do humans handle lost sender-to-receiver words in conversation?

Approach: sender waits “reasonable” amount of time for ACK
- retransmits if no ACK received in this time
- if pkt (or ACK) just delayed (not lost):
    - retransmission will be duplicate, but seq `#s` already handles this!
    - receiver must specify seq # of packet being ACKed timeout
- use countdown timer to interrupt after “reasonable” amount of time

## 5.1 rdt3.0 sender

![](image/Pasted%20image%2020241208173214.png)

![](image/Pasted%20image%2020241208173326.png)


## 5.2 rdt3.0 in action

![](image/Pasted%20image%2020241208173630.png)

![](image/Pasted%20image%2020241208173638.png)



## 5.3 stop-and-wait operation

![](image/Pasted%20image%2020241208173719.png)

![](image/Pasted%20image%2020241208173712.png)


![](image/Pasted%20image%2020241208173730.png)

## 5.4 pipelined protocols operation


![](image/Pasted%20image%2020241208174104.png)


# 6 Go-Back-N 


The Go-Back-N protocol is an Automatic Repeat Request (ARQ) protocol used in networking to ensure reliable data transmission. It is part of the sliding window protocols family, enabling efficient data transfer while incorporating error control mechanisms.

How the Go-Back-N Protocol Works
1. **Window Size (Sliding Window):**
    - The sender maintains a **sending window** of up to NNN packets. This means the sender can transmit up to NNN unacknowledged packets simultaneously.
    - Typically, the receiver has a receiving window of size 1, meaning it processes packets strictly in order.
2. **Transmission:**
    - The sender transmits packets consecutively without waiting for an acknowledgment (**ACK**) for each packet, as long as the number of unacknowledged packets is within the window size (≤N\leq N≤N).
3. **Error Detection:**
    - If the receiver detects a corrupted packet (using checksums) or receives a packet out of order, it discards the faulty or out-of-sequence packet and may send a **negative acknowledgment (NAK)** or repeat the last correct **ACK**.
4. **Error Recovery:**
    - Upon detecting a missing or corrupted packet (or after a timeout), the sender retransmits the **last acknowledged packet** and all subsequent packets, effectively "going back NNN" packets.

Key Features of Go-Back-N
- **Efficiency:** Allows continuous transmission of multiple packets before receiving individual ACKs, improving throughput in reliable networks.
- **Drawback:** In the event of an error, it retransmits all packets from the last acknowledged one, which may lead to inefficiencies in networks with high latency or low error rates.


Example
- **Sender:** Sends packets P1,P2,P3,P4,…P1, P2, P3, P4, \dotsP1,P2,P3,P4,… up to NNN.
- **Receiver:** Acknowledges P1,P2P1, P2P1,P2. If P3P3P3 is corrupted, it discards P3P3P3 and all subsequent packets, sending an ACK for P2P2P2.
- **Sender:** Upon receiving the ACK for P2P2P2, retransmits P3,P4,…P3, P4, \dotsP3,P4,….

This approach ensures data integrity while balancing efficiency with error correction.


## 6.1 Sender 


![](image/Pasted%20image%2020241208174144.png)

![](image/Pasted%20image%2020241208182950.png)


## 6.2 receiver 


![](image/Pasted%20image%2020241208174226.png)

![](image/Pasted%20image%2020241208183003.png)


## 6.3 Go-Back-N in action 


从 highest highest in-order seq 的 已经 acknowledged 的 pakcage 的下一个重发, 一面的全部重发, 不管 用没有接受成功 

![](image/Pasted%20image%2020241208174250.png)


# 7 Selective repeat

- receiver individually acknowledges all correctly received packets
    - buffers packets, as needed, for eventual in-order delivery to upper layer
- sender times-out/retransmits individually for unACKed packets
    - sender maintains timer for each unACKed pkt
- sender window
    - N consecutive seq `#s `
    - limits seq `#s` of sent, unACKed packets

--- 

sender, receiver windows
![](image/Pasted%20image%2020241208174730.png)


sender
- data from above:
    - if next available seq # in window, send packet timeout(n):
- resend packet n, restart timer
- ACK(n) in `[sendbase,sendbase+N]`:
    - mark packet n as received
    - if n smallest unACKed packet, advance window base to next unACKed seq  #

receiver
- packet n in `[rcvbase, rcvbase+N-1]`
    - send ACK(n)
    - out-of-order: buffer
    - in-order: deliver (also deliver buffered, in-order packets), advance window to next not-yetreceived packet
- packet n in `[rcvbase-N,rcvbase-1]`
    - ACK(n)
- otherwise:
    - ignore

![](image/Pasted%20image%2020241208175238.png)

![](image/Pasted%20image%2020241208175328.png)


![](image/Pasted%20image%2020241208175344.png)


