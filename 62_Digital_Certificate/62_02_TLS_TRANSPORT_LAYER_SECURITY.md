
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

# 3 TLS  BASICS


TLS (Transport Layer Security) is a cryptographic protocol designed to provide secure communication over a network

Encryption: Ensures that data exchanged between parties is encrypted, preventing unauthorized access or eavesdropping
Authentication: Verifies the identity of one or both parties (e.g., server authentication using certificates)

Data Integrity: Ensures that data has not been tampered with during transmission using cryptographic checks (e.g., Message Authentication Code)


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

# 4 TLS RECORD PROTOCOL


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

