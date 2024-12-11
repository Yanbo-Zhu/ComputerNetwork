


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
- The ==Message Authentication Code== (MAC) or Authentication Tag serves as a proof for the authenticity and integrity of the message


## 4.1 WHAT ARE MESSAGE AUTHENTICATION CODES? (MAC)

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

![](image/Pasted%20image%2020241211112415.png)


## 4.2 AUTHENTICATED ENCRYPTION WITH ASSOCIATED DATA (AEAD)

- Authentic Encryption with Associated Data (AEAD) is a cryptographic technique that combines encryption and authentication into a single, efficient operation
- ==It ensures confidentiality, integrity, and authenticity in one step During encryption, it generates an Authentication Tag, which is automatically appended to the message==
- During decryption, AEAD decrypts the plaintext and verifies the Authentication Tags – it then indicates success (1) or failure (0) of this verification
- AEAD includes additional, unencrypted metadata (Associated Data) in the authentication process 
- The metadata is unencrypted and known to both parties

![](image/Pasted%20image%2020241201004431.png)


# 5 KEY AGREEMENT

两方 如何 计算 secret key shared (shared key )

常用的 key agreement 方法是 DH or ECDH 
将自己的公钥交给对方, 然后用自己的私钥+对方的公钥 产生  secret key shared (shared key )

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


不用交换 Shared secret 了, 通过交换双方的公钥私钥 直接在自己这边产生 shared secret 

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

Finite Set DH is based on the Discrete Logarithm Problem

![](image/Pasted%20image%2020241211114528.png)

![](image/Pasted%20image%2020241211114536.png)


![](image/Pasted%20image%2020241201010036.png)


### 5.3.2 ELLIPTIC CURVE DH

ECDH (Elliptic Curve Diffi e-Hellman) is based on Elliptic Curve Cryptography (ECC) and an equation of the form


![](image/Pasted%20image%2020241211114605.png)

![](image/Pasted%20image%2020241211114615.png)



### 5.3.3 ANONYMOUS DH

Anonymous DH, as shown before, is vulnerable to a man-in-the-middle attack

An eavesdropper simply needs to intercept messages and pretend to be Bob towards Alice and Alice towards Bob

![](image/Pasted%20image%2020241211114628.png)

![](image/Pasted%20image%2020241211114644.png)

### 5.3.4 AUTHENTICATED DH

Authenticated DH equips the two parties with both a private and a public key, thereby allowing Alice and Bob to sign their messages in order to stop the attacker from sending messages on their behalf


![](image/Pasted%20image%2020241211114802.png)

![](image/Pasted%20image%2020241211114947.png)

![](image/Pasted%20image%2020241211120608.png)


# 6 TLS 1.2  HANDSHAKE

![](image/Pasted%20image%2020241211132353.png)

![](image/Pasted%20image%2020241210230240.png)



## 6.1 FULL HANDSHAKE


### 6.1.1 Client Hello 

![](image/Pasted%20image%2020241209005013.png)


交换什么 
1. sender 支持那些  cipher suites 
2. sender 支持哪些 key exchange groups
3. sender 支持那些 signature algorithms 
4. sender 的公钥 
5. 一个 random number to prevent replay attacks

---


两方 如何 计算 secret key shared (shared key )

常用的 key agreement 方法是 DH or ECDH 
将自己的公钥交给对方, 然后用自己的私钥+对方的公钥 产生  secret key shared (shared key )


---


- A ClientHello message is sent to the server by the client to initiate the TLS handshake
- The message contains 
    - a **random number** to prevent replay attacks, 
    - ==the client's key share according to DH or ECDH== (calculated before only for this session), 
    - a list of cipher suites and signature algorithms supported by the client
- The message may contain optional fields like Server Name Indication (in case the server serves multiple domains), Application-Layer Protocol Negotiation, and a SessionId (for backward compatibility with TLS 1.2)
- The message is sent in plaintext and unsigned


### 6.1.2 ServerHello 

![](image/Pasted%20image%2020241211111555.png)


交换什么 
1. reviver 选择那个  cipher suites, key exchange groups  用作于 之后的信息纯属 
2. sender 支持哪些 key exchange groups
3. reviver 的公钥 
4. 同样的 random number to prevent replay attacks

然后双方 根据 跟紫的 private key 和 public key 马上计算 shared key 

---

- ==The server calculates an own key share (based on DH or ECDH)== and includes it in the ServerHello message
- The message also contains a random generated by the server as well as the selected cipher suite and key exchange
- After reception of the ServerHello,==both sides calculate the key share from their own private key and the received public key==
- Note: The private keys and , the derived public keys and , the key share , and all keys derived from it must be deleted after session termination to guarantee Perfect Forward Secrecy
- The ServerHello message is sent in plaintext and unsigned



### 6.1.3 generate client and server-side handshake/application/finish keys through key derivation fucntion 

![](image/Pasted%20image%2020241211121829.png)

- After having determined the key share S , ==both sides use a Key Derivation Function, , and other data to generate client and server-side handshake keys for encryption and decryption of further handshake messages==
- The handshake keys are ephemeral keys and need to be deleted after session termination
- All following handshake messages are encrypted from here on
- The server sends an EncryptedExtension message with further details of the session
- If the server wants to authenticate the client with a certificate, it sends a Certifi cateRequest message (optional)

#### 6.1.3.1 KEY DERIVATION FUNCTION


SIMPLISTIC VIEW

![](image/Pasted%20image%2020241211122010.png)

DETAILLED VIEW
![](image/Pasted%20image%2020241211122116.png)


### 6.1.4 server sends its own certificate to client  and ServerFinished message 

![](image/Pasted%20image%2020241211122533.png)

Certificate交换  
- The server sends its own certificate, which includes its identity and its long-term public key, and all intermediate certificates in a Certificate message

CertificateVerfy 
- The client checks the validity of the certificate ==(domain name, validity period, CA's signature, the integrity of the certificate chain, the availability of Signed Certificate Timestamps, and the revocation status with CRL or OCSP,...)==
- The server authenticates towards the client with a signature built by the current Handshake Transcript Hash signed with the long-term private key that belongs to its public key, contained in the certificate
    - Singature 的产生 是通过 Handshake Transcript Hash 去加密 一个the private key of server, 
    - 把这个 signature交给client 
- The Handshake Transcript Hash is a concatenation of all handshake message sent so far, in plaintext
- The client validates the received signature – the server is now authenticated

ServerFinished
- The server compiles a ServerFinished message, which contains a HMAC built by the current Handshake Transcript Hash signed with the server-side handshake key
    -  计算得到 HMAC , 将 这个 HMAC 交给 client, 
    - HMAC 是用来以后计算 产生 MAC 的 
    - shared key + hmac 算法 去 产生 MESSAGE AUTHENTICATION CODES (MAC)
- The client validates the received HMAC
- Note: the signature is used to authenticate the server, while the HMAC guarantees the integrity of the session and the correct derivation of the ephemeral keys

 Certificate and CertificateVerify can be omitted in some versions of the handshake


### 6.1.5 server rqeust client certificate and client send ClientFinished Message  

这个交换过程和 server sends its own certificate to client  的过程类似 

![](image/Pasted%20image%2020241211124004.png)

- If the server has requested a client certificate, the exchange of message for sending the certificate and the signature as well as all checks are mirrored to the procedures carried out for the server certificate
- Finally, client-side and server-side ephemeral application keys (to be deleted after session termination) are determined with the Key Derivation Function, key share , and other data
- The exchange of application data, encrypted and decrypted with the application keys, starts


## 6.2 Packet Formats

![](image/Pasted%20image%2020241210231133.png)


## 6.3 Handshake –Extensions

- In each handshake packet, many extensions fields can be included to further negotiate properties of connection
- Some properties will result in connection failure if the peer doesn't support them, others wont
- Also included in extensions are:
    - Supported crypto algorithms
    - Keying material
    - In TLS1.3 and above, the highest supported TLS version
    - This is done here instead of the version field in the ClientHellofor backwards compatibility in middleboxes

![](image/Pasted%20image%2020241210231248.png)

## 6.4 Key exchange mode

- Client sends list of supported crypto suits/ elliptic curve groups
- Client also sends required crypto bits for each group
    - Doesn't have to be complete, can include a supported group without keying material
    - The server will then reply with HelloRetryRequest to allow the client to generate missing keying material
    - Useful for newer encryption schemes for which peer support is unknown and creating keying material is costly

# 7 TLS 1.3 

![](image/Pasted%20image%2020241211132359.png)

![](image/Pasted%20image%2020241211132353.png)

## 7.1 TLS 1.3 CONNECTION WITH OPENSSL

![](image/Pasted%20image%2020241211125643.png)


## 7.2 TLS1.3 通过 pre-shared key 和 0-RTT 优化 TLS 1.2的 handshake process 



### 7.2.1 Abbreviated handshake WITH pre-shared key(PSK) RESUMPTION

实现快速连接 

- ==It reduces the overhead of the full handshake by skipping steps like certificate validation and key exchange, leveraging a previously established shared secret.==
- This is commonly used in scenarios like session resumption to improve performance and reduce latency while maintaining security.
- 使用从前 session 中 产生的 shared secret  key 

---

- To enable faster reconnection, client and server do not always go through the full handshake, but perform an abbreviated handshake using using pre-shared key (PSK) resumption
- A PSK is a cryptographic key shared between client and server after the previous handshake
- Two handshake types
    - PSK-Only Handshake: The PSK is the sole basis for authentication, no certificates are used and DH is not applied
    - PSK with DHE: Combines PSK with Ephemeral Diffi e-Hellman key agreement to provide forward secrecy


- Can be used to reduce delay by allowing 0-RTT data
    - E.g., Client can send (early) application data with ClientHello
- Can be either issued with NewSessionTicket messages for future connections or provisioned out of band
    - Useful if multiple connections between same server and client are expected
- E.g., multiple HTTP connections for websites etc.
    - Tickets have a certain lifetime as well so they can be used for future sessions

![](image/Pasted%20image%2020241211125739.png)



![](image/Pasted%20image%2020241210231918.png)


#### 7.2.1.1 **How PSK Resumption Works**

Pre-Shared Key (PSK) Setup
- A shared secret is established during a previous session handshake.
- This secret is derived from the key exchange (e.g., Diffie-Hellman) and is securely stored as a session ticket or session ID.

Client Initiates Resumption
- The client sends a **ClientHello** message, including the session ticket or PSK identity.
- The PSK identity indicates which shared secret the client wants to use.

Server Validates and Agrees
- If the server recognizes the PSK identity and validates it, it responds with a **ServerHello**, confirming the use of the shared PSK.
- Both client and server use the PSK to derive new session keys for encryption.

Key Derivation
- New session keys are derived from the shared PSK and other handshake parameters (e.g., random values from the ClientHello and ServerHello).
- These keys are used for securing the resumed session.

Finished Messages
- Both client and server exchange "Finished" messages to confirm the handshake.
- Secure communication resumes immediately without requiring a full handshake.

#### 7.2.1.2 Message Flow: Abbreviated PSK Handshake

ClientHello:
    Includes the PSK identity and supported cipher suites.
ServerHello:
    Confirms the use of the PSK and selects the cipher suite.
Key Derivation:
    Both sides derive session keys using the PSK and exchanged random values.
Finished:
    Client and server verify the handshake using the derived keys.
Secure Data Transmission:
    Encrypted communication resumes using the new session keys.


### 7.2.2 0-RTT HANDSHAKE

![](image/Pasted%20image%2020241211131015.png)

- A 0-RTT (Zero Round-Trip Time) handshake allows a client to send data immediately after the ClientHello message without waiting for the server's response
- It reduces latency, making connections faster, especially for applications requiring low delays, such as web browsing, APIs, and real-time communication
- ==The 0-RTT handshake relies on a PSK established during a prior connection==
- Application-data is encrypted with a PSK-derived key
- Advantages: reduced latency and efficiency
- Disadvantages: Risk of replay attacks and no forward secrecy


## 7.3 TLS 1.3 CIPHER SUITES

![](image/Pasted%20image%2020241211131415.png)


- A cipher suite is a standardized set of algorithms used to secure communications in protocols like TLS
- ==A cipher suite for TLS 1.2 defines==
    - the key agreement algorithm (RSA, DH, ECDHE), 
    - the authentication algorithm (RSA, ECDSA), 
    - the encryption algorithm (AES, ChaCha20, RC4) as well as its encryption strength and mode, 
    - the Message Authentication Code algorithm
- In TLS 1.3 the cipher suites are simplified and ==only specify the encryption algorithm and hash function==
- TLS 1.3 only uses DHE and PSK as key agreement algorithms, while the required authentication algorithms depend on the used certificates

![](image/Pasted%20image%2020241211131948.png)


# 8 不同 TLS 1.2 vs 1.3 vs Quic 



![](image/Pasted%20image%2020241211132037.png)

![](image/Pasted%20image%2020241211132353.png)

![](image/Pasted%20image%2020241211132359.png)

![](image/Pasted%20image%2020241211132153.png)


---

QUIC: 

![](image/Pasted%20image%2020241211132221.png)

![](image/Pasted%20image%2020241211132413.png)