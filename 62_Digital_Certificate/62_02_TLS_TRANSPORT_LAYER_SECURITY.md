


# 1 THE TCP HANDSHAKE

![](image/Pasted%20image%2020241201001000.png)


- All TCP connection begin with a three-way handshake
- Sequence numbers in both directions identify the position of the first byte in the segment within the overall data stream
- Acknowledgement numbers in both directions confirm the successful receipt of data and indicate the next sequence number the receiver expects to receive
- TCP handshake is initiated by one side with an empty TCP segment (no body) with a set SYN flag
- TCP handshake is acknowledged by the other side with an empty TCP segment with set SYN and ACK flags
- Each connection have a full roundtrip of latency before any application data can be transferred



# 2 TLS Handshake 


![](image/Pasted%20image%2020241201001511.png)

- TLS uses a two-way handshake to negotiate cryptographic parameters to protect connection
- Client starts by sending ClientHello
- Server responds with ServerHello
    - Server selects among the possible options provided by the client
    - Picks highest TLS version supported by both, makes choice of algorithm/ keying material
    - Where ClientHelloincludes lists, ServerHelloonly includes single items
- Optionally: Server can send ClientHelloRetry


# 3 TLS  BASICS


TLS (Transport Layer Security) is a cryptographic protocol designed to provide secure communication over a network

Encryption: Ensures that data exchanged between parties is encrypted, preventing unauthorized access or eavesdropping
Authentication: Verifies the identity of one or both parties (e.g., server authentication using certificates)

Data Integrity: Ensures that data has not been tampered with during transmission using cryptographic checks (e.g., Message Authentication Code)



- TLS –Transport Layer Security
- Successor to SSL, currently at version 1.3 (RFC 8446)
- Used to protect application layer data
    - Eavesdropping, tampering and forgery
    - Provides Authentication, integrity and confidentiality
- Above rest of transport layer, agnostic to underlying transport if it has certain properties (e.g., reliable)
- Uses cryptographic schemes to protect payload (ECDHE)
    - Cryptographic computations etc. not focus of this lecture, just protocol
    
## 3.1 TLS IN THE PROTOCL STACK

![](image/Pasted%20image%2020241201002002.png)



- TLS operates as a middle layer, primarily between TCP and the application layer
- In combination with HTTP 1.1 it is optional, but in combination with HTTP/2 and HTTP/3 it is mandatory
- Using HTTP in a combination with a connection secured via TLS is usually referred to as HTTPS (i.e., HTTPS is not a dedicated protocol, but only the transfer of HTTP messages over TLS)
- QUIC is a new protocol developed by Google with the aim to make connections faster by replacing TCP
- In QUIC, TLS 1.3 is integrated directly into the protocol for securing connections
- TLS 1.3 in combination with QUIC supports a faster handshake, especially with 0-RTT (Zero Round-Trip Time) connections


## 3.2 TLS COMPONENTS

![](image/Pasted%20image%2020241201002246.png)


HANDSHAKE PROTOCOL
- 是用来校验 传输的双方的 身份 是否合格 
- Negotiate TLS protocol version
- Select cryptographic algorithms: cipher suites
- Authenticate by asymmetric cryptography
- Establish secret keys for symmetric encryption

RECORD PROTOCOL
- 是用来加密和解密 http message 的
- Encrypt outgoing messages with the secret keys
- Transmit the encrypted messages
- Decrypt incoming messages with the secret key
- Verify the integrity and authenticity of the messages

## 3.3 TLS Record Protocol and TLS Handshake Protocol 怎么一起工作的 


The TLS Record Protocol and the TLS Handshake Protocol are key components of the Transport Layer Security (TLS) protocol. They work together to establish and maintain a secure communication channel between two endpoints. Here's an overview:

How They Work Together
    The TLS Handshake Protocol runs first to set up the encryption keys, protocols, and other parameters for secure communication.
    Once the handshake is complete, the TLS Record Protocol takes over to encrypt and transport application data securely.


Simplified Example of a TLS Handshake:
1. **Client Hello**:
    - The client sends a list of supported protocols and cryptographic algorithms.
2. **Server Hello**:
    - The server selects compatible options and provides its certificate for authentication.
3. **Key Exchange**:
    - Both parties exchange key material to generate a shared secret.
4. **Finished**:
    - Both sides confirm the handshake, and secure communication begins via the Record Protocol.


---

The **TLS Record Protocol** is responsible for securing and transporting data. It breaks the data into smaller units (records) and ensures that these records are securely delivered to the recipient.

Responsibilities:
1. **Fragmentation**:
    - Divides data into manageable chunks (records).
2. **Compression** (optional):
    - Compresses data before transmission (rarely used in modern TLS implementations).
3. **Integrity and Authenticity**:
    - Adds a Message Authentication Code (MAC) to protect against tampering.
4. **Encryption**:
    - Encrypts the data using the algorithms agreed upon during the handshake phase.
5. **Transmission**:
    - Sends the encrypted data over the network.


Data Flow:
- Application Layer Data → Fragmentation → (Optional: Compression) → Add MAC → Encrypt → Transmit.

---


TLS Handshake Protocol

The **TLS Handshake Protocol** is used to negotiate security parameters and establish a secure connection. It is a key step in the initialization of a TLS session.

Responsibilities:
1. **Protocol and Algorithm Negotiation**:
    - Determines the TLS version (e.g., TLS 1.2, TLS 1.3) and cryptographic algorithms (e.g., AES, RSA).
2. **Authentication**:
    - Authenticates the server and optionally the client using certificates (X.509).
3. **Key Exchange**:
    - Establishes shared secret keys using techniques like Diffie-Hellman or RSA.
4. **Session Establishment**:
    - Completes the handshake by finalizing parameters and enabling encrypted communication.




# 4 TLS RECORD PROTOCOL

- As soon as possible, all traffic will be encrypted
    - Depending on what mode is used (PSK or KE), starting mid handshake
- Secrets exchanged are only part of traffic keys that are used to encrypt
    - Other parts include hash over entire handshake
- Key derivation functions (i.e. HKDF) is used to extend secrets to longer keys for protection
- Keys (should be) updated periodically for various reasons


![](image/Pasted%20image%2020241201002527.png)



- Data is exchanged in records between the communicating parties
- Records are used to transfer both handshake and application traffic
- Header field is mainly maintained for being backwards compatible with previous TLS versions, while information about type of data (invalid, change_cipher_spec, alert, handshake, application_data) and length is encrypted in the payload (apart from initial ClientHello and ServerHello messages)
- The Message Authentication Code (MAC) or Authentication Tag serves as a proof for the authenticity and integrity of the message


## 4.1 WHAT ARE MESSAGE AUTHENTICATION CODES?

- A Message Authentication Code (MAC) is a cryptographic construct to ensure
    - Integrity: The data has not been tampered with
    - Authenticity: The data comes from the claimed sender
- ==A MAC is generated using a secret key shared between two parties==
- The sender computes the MAC over the message and sends both message and MAC
- The Receiver then recalculates the MAC on the received message using the same key – if calculated and received MAC match, the message is verified
- Hash-based Message Authentication Code (HMAC) is an algorithm that uses a cryptographic hash function (like SHA-256) along with a secret to produce the authentication code

---


![](image/Pasted%20image%2020241201003700.png)


## 4.2 AUTHENTICATED ENCRYPTION WITH ASSOCIATED DATA

- Authentic Encryption with Associated Data (AEAD) is a cryptographic technique that combines encryption and authentication into a single, efficient operation
- It ensures confi dentiality, integrity, and authenticity in one step During encryption, it generates an Authentication Tag, which is automatically appended to the message
- During decryption, AEAD decrypts the plaintext and verifi es the Authentication Tags – it then indicates success (1) or failure (0) of this verifi cation
- AEAD includes additional, unencrypted metadata (Associated Data) in the authentication process 
- The metadata is unencrypted and known to both parties

![](image/Pasted%20image%2020241201004431.png)


# 5 KEY AGREEMENT


## 5.1 KEY EXCHANGE

已经有一个 Shared Secret 加密后 传给对方

![](image/Pasted%20image%2020241201004613.png)

- Key exchange is the process of securely transferring a secret (e.g., a random number to derive a symmetric key from or the direct symmetric key) between parties
- One party generates the secret and securely sends it to the other
- Typically one-way (secret sender -> secret receiver)
- Example: RSA key exchange
- Depends on secure transmission and encryption to prevent interception
- The sender controls the key generation


## 5.2 KEY AGREEMENT

![](image/Pasted%20image%2020241201004813.png)


不用交换 Shared secret 了, 直接在自己这边产生 shared secret 

- Key agreement is a method where two parties collaboratively derive a shared secret without directly transmitting it
- Both parties contribute information to generate the shared secret
- Typically interactive (both parties exchange data to compute the secret)
- Examples: Diffi e-Hellman key agreement, Elliptic Curve Diffi e- Hellman
- Relies on mathematical problems (trapdoors) for security; no key is transmitted 
- Both parties have equal participation in key generation


## 5.3 Algorithm 去establish shared secret key


Multiple variants, but only three covered here
- Classical (Finite Set) Diffie-Hellman (DH): operates in the multiplicative group of integers modulo a large prime
- Elliptic Curve Diffie-Hellman (ECDH): uses elliptic curve groups over a fi nite fi eld
- Ephemeral Diffie-Hellman (DHE): uses temporary (ephemeral) keys for every session and may be based on DH or ECDH


### 5.3.1 FINITE SET DH

![](image/Pasted%20image%2020241201010030.png)


![](image/Pasted%20image%2020241201010036.png)




### 5.3.2 ELLIPTIC CURVE DH

![](image/Pasted%20image%2020241201010054.png)



### 5.3.3 ANONYMOUS DH

![](image/Pasted%20image%2020241201010114.png)

# 6 TLS HANDSHAKE



![](image/Pasted%20image%2020241210230240.png)


## 6.1 Packet Formats

![](image/Pasted%20image%2020241210231133.png)


## 6.2 Handshake –Extensions

- In each handshake packet, many extensions fields can be included to further negotiate properties of connection
- Some properties will result in connection failure if the peer doesn't support them, others wont
- Also included in extensions are:
    - Supported crypto algorithms
    - Keying material
    - In TLS1.3 and above, the highest supported TLS version
    - This is done here instead of the version field in the ClientHellofor backwards compatibility in middleboxes

![](image/Pasted%20image%2020241210231248.png)

## 6.3 Key exchange mode

- Client sends list of supported crypto suits/ elliptic curve groups
- Client also sends required crypto bits for each group
    - Doesn't have to be complete, can include a supported group without keying material
    - The server will then reply with HelloRetryRequestto allow the client to generate missing keying material
    - Useful for newer encryption schemes for which peer support is unknown and creating keying material is costly

## 6.4 Pre shared key mode

提前共享 Pre shared key mode

- Can be used to reduce delay by allowing 0-RTT data
    - E.g., Client can send (early) application data with ClientHello
- Can be either issued with NewSessionTicket messages for future connections or provisioned out of band
    - Useful if multiple connections between same server and client are expected
- E.g., multiple HTTP connections for websites etc.
    - Tickets have a certain lifetime as well so they can be used for future sessions

![](image/Pasted%20image%2020241210231918.png)



## 6.5 FULL HANDSHAKE

![](image/Pasted%20image%2020241209005013.png)



A ClientHello message is sent to the server by the client to initiate the TLS handshake
The message contains a random number to prevent replay attacks, the client's key share according to DH or ECDH (calculated
before only for this session), a list of cipher suites and signature algorithms supported by the client
The message may contain optional fi elds like Server Name Indication (in case the server serves multiple domains),
Application-Layer Protocol Negotiation, and a SessionId (for backward compatibility with TLS 1.2)
The message is sent in plaintext and unsigned















