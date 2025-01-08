
provides datagram-level encryption, authentication, integrity
for both user traffic and control traffic (e.g., BGP, DNS messages)

- IKE message exchange for algorithms, secret keys, SPI numbers
- either AH or ESP protocol (or both)
    - AH provides integrity, source authentication
    - ESP protocol (with AH) additionally provides encryption
- IPsec peers can be two end systems, two routers/firewalls, or a router/firewall and an end system

# 1 two security modes


- transport mode:
    - only datagram payload is encrypted, authenticated
    - ![](image/Pasted%20image%2020250108205342.png)
- tunnel mode:
    - entire datagram is encrypted, authenticated
    - encrypted datagram encapsulated in new datagram with new IP header, tunneled to destination
    - ![](image/Pasted%20image%2020250108205348.png)


# 2 Two IPsec protocols

- Authentication Header (AH) protocol [RFC 4302]
    - provides source authentication & data integrity but not confidentiality  保密性，机密性
- Encapsulation Security Protocol (ESP) [RFC 4303]
    - provides source authentication, data integrity, and confidentiality
    - more widely used than AH

# 3 Security associations (SAs)

- before sending data, security association (SA) established from sending to receiving entity (directional)
- ending, receiving entitles 使有资格，使有权 maintain state information about SA
    - recall: TCP endpoints also maintain state info
    - IP is connectionless; IPsec is connection-oriented!
![](image/Pasted%20image%2020250108205825.png)

R1 stores for SA:
- 32-bit identifier: Security Parameter Index (SPI)
- origin SA interface (200.168.1.100)
- destination SA interface (193.68.2.23)
- type of encryption used
- encryption key
- type of integrity check used
- authentication key


# 4 IPsec datagram

![](image/Pasted%20image%2020250108210103.png)

- ESP trailer: padding for block ciphers
- ESP header:
    - SPI, so receiving entity knows what to do
    - sequence number, to thwart replay attacks
- MAC in ESP auth field created with shared secret key


# 5 ESP tunnel mode: actions

![](image/Pasted%20image%2020250108210445.png)

at R1:
- appends ESP trailer to original datagram (which includes original header fields!)
- encrypts result using algorithm & key specified by SA
- appends ESP header to front of this encrypted quantity
- creates authentication MAC using algorithm and key specified in SA
- appends MAC forming payload
- creates new IP header, new IP header fields, addresses to tunnel endpoint


# 6 IPsec sequence numbers

- for new SA, sender initializes seq. # to 0
- each time datagram is sent on SA:
    - sender increments seq # counter
    - places value in seq # field
- goal:
    - prevent attacker from sniffing and replaying a packet
    - receipt of duplicate, authenticated IP packets may disrupt service
- method:
    - destination checks for duplicates
    - doesn’t keep track of all received packets; instead uses a window


# 7 IPsec security databases

Security Policy Database (SPD) (SPD: “how” to do it)
- policy: for given datagram, sender needs to know if it should use IP sec
- policy stored in security policy database (SPD)
- needs to know which SA to use
    - may use: source and destination IP address; protocol number


Security Assoc. Database (SAD)  (SAD: “what” to do)
- endpoint holds SA state in security association database (SAD)
- when sending IPsec datagram, R1 accesses SAD to determine how to process datagram
- when IPsec datagram arrives to R2, R2 examines SPI in IPsec datagram, indexes SAD with SPI, processing
- datagram accordingly.


# 8 IKE: Internet Key Exchange

previous examples: manual establishment of IPsec SAs in IPsec endpoints:
Example SA:
```
SPI: 12345
Source IP: 200.168.1.100
Dest IP: 193.68.2.23
Protocol: ESP
Encryption algorithm: 3DES-cbc
HMAC algorithm: MD5
Encryption key: 0x7aeaca…
HMAC key:0xc0291f…
```

manual keying is impractical for VPN with 100s of endpoints
instead use IPsec IKE (Internet Key Exchange)

PSK and PKI
authentication (prove who you are) with either
pre-shared secret (PSK) or
with PKI (pubic/private keys and certificates).

PSK: both sides start with secret
run IKE to authenticate each other and to generate IPsec SAs (one in each direction), including encryption, authentication keys

PKI: both sides start with public/private key pair, certificate
run IKE to authenticate each other, obtain IPsec SAs (one in each direction).
similar with handshake in SSL.

IKE has two phases
- phase 1: establish bi-directional IKE SA
    - note: IKE SA different from IPsec SA
    - aka ISAKMP security association
- phase 2: ISAKMP is used to securely negotiate IPsec pair of SAs

phase 1 has two modes: aggressive mode and main mode
- aggressive mode uses fewer messages
- main mode provides identity protection and is more flexible
