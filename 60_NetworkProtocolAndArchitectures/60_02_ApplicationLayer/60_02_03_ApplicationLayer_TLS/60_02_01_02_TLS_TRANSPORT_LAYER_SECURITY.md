
来自于 Digital Certifcate 这门课的课件 

# 1 TLS Transport Layer Security (Securing TCP)

TSL implemented in application layer , TLS 是在  application layer 这层实施的 TLS (Transport Layer Security): Secure TCP
- widely deployed security protocol above the transport layer
    - supported by almost all browsers, web servers: https (port 443)
- provides:
    - confidentiality: via symmetric encryption
    - integrity: via cryptographic hashing
    - authentication: via public key cryptography
- history:
    - early research, implementation: secure network programming, secure sockets
    - secure socket layer (SSL) deprecated [2015]
    - TLS 1.3: RFC 8846 [2018]


- **_Transport Layer Security_** (_**TLS**_) erweitert TCP um _Vertraulichkeit_, Datenintegrität sowie um Client- und Server-Authentisierung und Authentifizierung
- Vorgängerbezeichnung ist als **_Secure Socket Layer_** (SSL) bekannt
    - Versionen: SSL 1.0 bis 3.1
    - TLS 1.0 entspricht SSL 3.1
    - Spezifikation von TLS 1.3 seit Januar 2016
- **_Hypertext Transfer Protocol Secure_** (HTTPS) basiert auf einer mit TLS gesicherten TCP-Verbindung - keine semantischen Unterschiede zu HTTP


![](image/Pasted%20image%2020241101202204.png)

![](image/Pasted%20image%2020250207105326.png)

![](image/Pasted%20image%2020241101202312.png)


![](image/Pasted%20image%2020241101202345.png)


# 2 TLS Eigenschaft 


Authentisierung und Authentifizierung
- **_Authentisierung_** ist der Nachweis einer Person, dass sie tatsächlich diejenige Person ist, die sie vorgibt zu sein.  就是自己的身份证
- Methoden der Authentisierung
    - Geheime Informationen, die nur der Person bekannt sind (z.B. Passwort, nicht-öffentlicher Schlüssel)
    - Identifizierungsgegenstand (z.B. Personalausweis)
    - Biometrische Merkmale (z.B. Fingerabdruck)
- **_Authentifizierung_** ist die Überprüfung der behaupteten Authentisierung durch die Person oder Entität gegenüber der die Authentisierung erfolgt ist. 就是别人检查你的身份证
- Beachte: die Begriffe Authentifizierung und Authentisierung werden oftmals synonym verwendet


Vertraulichkeit
- **_Vertraulichkeit_** bezeichnet den Schutz vor unbefugter Preisgabe von Informationen
- Vertraulichkeit einer Nachricht wird erreicht, wenn nur die autorisierten Personen bzw. Entitäten Zugriff auf die Nachricht haben und das Lesen der Nachricht durch Dritte verhindert wird


Integrität
- _**Integrität**_ bedeutet, dass Daten nicht unbemerkt, z.B. durch Angreifer, verändert werden können
- Alle Änderungen an Daten müssen für den Empfänger erkennbar sein



# 3 Transport-layer security: what’s needed?

let’s build a toy TLS protocol, t-tls, to see what’s needed!
we’ve seen the “pieces” already:
- handshake: Alice, Bob use their certificates, private keys to authenticate each other, exchange or create shared secret
- key derivation: Alice, Bob use shared secret to derive set of keys
- data transfer: stream data transfer: data as a series of records
    - not just one-time transactions
- connection closure: special messages to securely close connection

---

1 t-tls handshake phase:
- Bob establishes TCP connection with Alice
- Bob verifies that Alice is really Alice
- Bob sends Alice a master secret key (MS), used to generate all other keys for TLS session
- potential issues:
• 3 RTT before client can start receiving data (including TCP handshake)
![](image/Pasted%20image%2020250108200836.png)


2 t-tls: cryptographic keys

- considered bad to use same key for more than one cryptographic function
    - different keys for message authentication code (MAC) and encryption
- four keys:
    - Kc : encryption key for data sent from client to server
    - Mc : MAC key for data sent from client to server
    - Ks : encryption key for data sent from server to client
    - Ms : MAC key for data sent from server to client
- keys derived from key derivation function (KDF)
    - takes master secret and (possibly) some additional random data to create new keys


3 t-tls: encrypting data
- recall: TCP provides data byte stream abstraction
- Q: can we encrypt data in-stream as written into TCP socket?
    - A: where would MAC go? If at end, no message integrity until all data received and connection closed!
    - solution: break stream in series of “records”
        - each client-to-server record carries a MAC, created using Mc
        - receiver can act on each record as it arrives
- t-tls record encrypted using symmetric key, Kc, passed to TCP:

![](image/Pasted%20image%2020250108201109.png)


- possible attacks on data stream?
    - re-ordering: man-in middle intercepts TCP segments and reorders (manipulating sequence `#s` in unencrypted TCP header)
    - replay
- solutions:
    - use TLS sequence numbers (data, TLS-seq-# incorporated into MAC)
    - use nonce

4 t-tls:connection close
- truncation attack:
    - attacker forges TCP connection close segment
    - one or both sides thinks there is less data than there actually is
- solution: record types, with one type for closure
    - type 0 for data; type 1 for close
- MAC now computed using data, type, sequence `#`
![](image/Pasted%20image%2020250108201810.png)





# 4 TCP HANDSHAKE


![](../../../62_Digital_Identity/image/Pasted%20image%2020241201001000.png)


- All TCP connection begin with a three-way handshake
- Sequence numbers in both directions identify the position of the first byte in the segment within the overall data stream
- Acknowledgement numbers in both directions confirm the successful receipt of data and indicate the next sequence number the receiver expects to receive
- TCP handshake is initiated by one side with an empty TCP segment (no body) with a set SYN flag
- TCP handshake is acknowledged by the other side with an empty TCP segment with set SYN and ACK flags
- Each connection have a full roundtrip of latency before any application data can be transferred



# 5 TLS Handshake 


![](../../../62_Digital_Identity/image/Pasted%20image%2020241201001511.png)

- TLS uses a two-way handshake to negotiate cryptographic parameters to protect connection
- Client starts by sending ClientHello
- Server responds with ServerHello
    - Server selects among the possible options provided by the client
    - Picks highest TLS version supported by both, makes choice of algorithm/ keying material
    - Where ClientHelloincludes lists, ServerHelloonly includes single items
- Optionally: Server can send ClientHelloRetry


# 6 TLS  BASICS


> TLS (Transport Layer Security) is a cryptographic protocol designed to provide secure communication over a network
> TLS provides an API that any application can use

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


## 6.1 TLS IN THE PROTOCL STACK

![](../../../62_Digital_Identity/image/Pasted%20image%2020241201002002.png)

![](image/Pasted%20image%2020250108202257.png)

- TLS operates as a middle layer, primarily between TCP and the application layer
- In combination with HTTP 1.1 it is optional, but in combination with HTTP/2 and HTTP/3 it is mandatory
- Using HTTP in a combination with a connection secured via TLS is usually referred to as HTTPS (i.e., HTTPS is not a dedicated protocol, but only the transfer of HTTP messages over TLS)
- QUIC is a new protocol developed by Google with the aim to make connections faster by replacing TCP
- In QUIC, TLS 1.3 is integrated directly into the protocol for securing connections
- TLS 1.3 in combination with QUIC supports a faster handshake, especially with 0-RTT (Zero Round-Trip Time) connections


## 6.2 TLS COMPONENTS

![](../../../62_Digital_Identity/image/Pasted%20image%2020241201002246.png)


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

## 6.3 TLS Record Protocol and TLS Handshake Protocol 怎么一起工作的 


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


# 7 TLS RECORD PROTOCOL

- As soon as possible, all traffic will be encrypted
    - Depending on what mode is used (PSK or KE), starting mid handshake
- Secrets exchanged are only part of traffic keys that are used to encrypt
    - Other parts include hash over entire handshake
- Key derivation functions (i.e. HKDF) is used to extend secrets to longer keys for protection
- Keys (should be) updated periodically for various reasons


![](../../../62_Digital_Identity/image/Pasted%20image%2020241201002527.png)


- Data is exchanged in records between the communicating parties
- Records are used to transfer both handshake and application traffic
- Header field is mainly maintained for being backwards compatible with previous TLS versions, while information about type of data (invalid, change_cipher_spec, alert, handshake, application_data) and length is encrypted in the payload (apart from initial ClientHello and ServerHello messages)
- The ==Message Authentication Code== (MAC) or Authentication Tag serves as a proof for the authenticity and integrity of the message


## 7.1 WHAT ARE MESSAGE AUTHENTICATION CODES? (MAC)

==mac 类似于  sign 在 plaintext 后面的  digital signature ==

Sender 端: 
用 在 tls handshake 阶段 交换和计算所得的 secret key shared , 
1. shared key + hmac 算法 去 产生 MESSAGE AUTHENTICATION CODES (MAC)
2.  shared key  去 加密 plaintext  产生 ciphertext 
将这两个加在一起, 发送给 receiver 

Receiver 端: 
用 server 端的 HMAC 和 用 在 tls handshake 阶段 交换和计算所得的 secret key shared 去计算 receiver 端的 mac, 
然后看 这个 mac 和 从 Sender 收到的 mac 一不一致 , 如果一致, 则receiver 端 收到的 sender 端的 ciphertext  is verified 


- A Message Authentication Code (MAC) is a cryptographic construct to ensure
    - Integrity: The data has not been tampered with
    - Authenticity: The data comes from the claimed sender
- ==A MAC is generated using a secret key shared between two parties==
- The sender computes the MAC over the message and sends both message and MAC
- The Receiver then recalculates the MAC on the received message using the same key – if calculated and received MAC match, the message is verified
- Hash-based Message Authentication Code (HMAC) is an algorithm that uses a cryptographic hash function (like SHA-256) along with a secret to produce the authentication code

![](../../../62_Digital_Identity/image/Pasted%20image%2020241211112415.png)


## 7.2 AUTHENTICATED ENCRYPTION WITH ASSOCIATED DATA (AEAD)

- Authentic Encryption with Associated Data (AEAD) is a cryptographic technique that combines encryption and authentication into a single, efficient operation
- ==It ensures confidentiality, integrity, and authenticity in one step During encryption, it generates an Authentication Tag, which is automatically appended to the message==
- During decryption, AEAD decrypts the plaintext and verifies the Authentication Tags – it then indicates success (1) or failure (0) of this verification
- AEAD includes additional, unencrypted metadata (Associated Data) in the authentication process 
- The metadata is unencrypted and known to both parties

![](../../../62_Digital_Identity/image/Pasted%20image%2020241201004431.png)

