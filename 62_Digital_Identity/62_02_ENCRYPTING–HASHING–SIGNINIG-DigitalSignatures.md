

![](image/Pasted%20image%2020250215133543.png)

# 1 SYMMETRIC和ASYMMETRIC 差异 
SYMMETRIC  private key 和 public key 是同一个 
ASYMMETRIC  private key 和 public key 是两个不同的
# 2 SYMMETRIC ENCRYPTION


![](image/Pasted%20image%2020250215133926.png)

- **_Symmetric-key encryption_** (**_symmetric encryption_**, **_shared-key encryption_** or **_secret-key cryptography_**) ​uses the same key for both encryption and decryption, i.e., both communication share the same key
- Symmetric encryption is generally much faster and less computationally expensive than asymmetric encryption
- Main challenge is securely sharing and managing the secret key
- In systems with many users, each pair of users needs a unique key, which leads to a large number of keys that must be securely managed

Common Symmetric Encryption Algorithms
- **AES (Advanced Encryption Standard)**: Widely used in modern applications due to its level of security and efficiency
- **DES (Data Encryption Standard)**: Older and less secure compared to AES, therefore outdated
- **3DES (Triple DES)**: Improvement on DES that applies the DES algorithm three times to increase security
- **Blowfish**: Known for its speed and effectiveness in some scenarios

## 2.1 AES (Advanced Encryption Standard)

AES (Advanced Encryption Standard) is a symmetric, block-based encryption algorithm used for encrypted browsing, wireless security, file encryption, and processor security

![](image/Pasted%20image%2020250215134303.png)



生成 AES Key WITH OPENSSL 

![](image/Pasted%20image%2020241031145542.png)

![](image/Pasted%20image%2020241031145634.png)

 AES with The crypto-Module of Node.js (I/III)
- The **crypto** module in Node.js provides a set of cryptographic functionalities including key generation, encryption, decryption, hashing, and digital signatures
- It wraps several cryptographic function provided by **OpenSSL**
- Supports both symmetric (AES) and asymmetric (RSA, ECDSA) encryption algorithms
- Includes APIs for key derivation  and password hashing


```js
const crypto = require('crypto');

// Message to be encrypted
const message = 'This is a secret message to be encrypted with AES-CBC';

// Generate a random AES key (32 bytes for AES-256)
const aesKey = crypto.randomBytes(32);

// Generate a random initialization vector (IV) (16 bytes for AES-CBC)
const iv = crypto.randomBytes(16);

// Encrypt the message using AES-CBC
function encryptMessage(message, aesKey, iv) {
  const cipher = crypto.createCipheriv('aes-256-cbc', aesKey, iv);
  let encryptedMessage = cipher.update(message, 'utf8', 'hex');
  encryptedMessage += cipher.final('hex');
  
  return encryptedMessage;
}

// Decrypt the message using AES-CBC
function decryptMessage(encryptedMessage, aesKey, iv) {
  const decipher = crypto.createDecipheriv('aes-256-cbc', aesKey, iv);
  let decryptedMessage = decipher.update(encryptedMessage, 'hex', 'utf8');
  decryptedMessage += decipher.final('utf8');
  
  return decryptedMessage;
}

// Encrypt the message
const encryptedMessage = encryptMessage(message, aesKey, iv);

// Convert the encrypted message and IV to base64 for easier output
console.log('Encrypted Message (hex):', encryptedMessage);
console.log('AES Key (hex):', aesKey.toString('hex'));
console.log('IV (hex):', iv.toString('hex'));

// Decrypt the message
const decryptedMessage = decryptMessage(encryptedMessage, aesKey, iv);

// Print the decrypted message
console.log('Decrypted Message:', decryptedMessage); 
```


# 3 ASYMMETRIC ENCRYPTION

## 3.1 Overview 

![](image/Pasted%20image%2020250215134843.png)


- **_Asymmetric-key encryption_** (_**a**_**__**symmetric**_ encryption_**, **_public-key-encryption_**) ​uses two different keys for encryption and decryption: _**public key**_ and _**private key**_
- The keys are mathematically related but not identical
- ​The public key can be openly shared with anyone, but the private key is only known to the key holder   
- Public key is used to encrypt data, while the private key is used to decrypt it
- Asymmetric encryption solves the major problem of symmetric encryption: the secure key distribution, i.e., the public key can be transmitted in plaintext
- In systems with many users, each pair of users needs a unique key, which leads to a large number of keys that must be securely managed
- Asymmetric encryption is computationally more expensive than symmetric encryption because of the complexity of mathematical functions involved
- Typically used for exchanging symmetric keys or small amounts of sensitive data, generation of signatures, and key exchange


Common aSymmetric Encryption Algorithms
- **RSA (Rivest–Shamir–Adleman)**: One of the most commonly used public-key algorithms, widely used for secure data transmission and signatures
- **ECC (Elliptic Curve Cryptography)**: Provides the same level of security as RSA but with smaller key sizes, leading to better efficiency
- **Diffie-Hellman**: Used for securely deriving symmetric keys over a public channel
- **ElGamal**: Based on Diffie-Hellman and often used in hybrid encryption systems


## 3.2 Modular arithmetic 

没用 就没有做笔记 


## 3.3 RSA

- **_RSA_** (**_Rivest–Shamir-Adleman_**) is one of the most widely used asymmetric encryption algorithms
- RSA's security is based on the difficulty of factoring large prime numbers
- _**Integer factorization problem**_: Calculating n=p⋅qn=p⋅q when pp and qq are selected primes is easy, but finding pp and qq from nn is computationally hard
- Typical RSA key sizes are 2048 or 4096
- Security of RSA increases with the key size, but larger keys make encryption and decryption slower

![](image/Pasted%20image%2020250215135433.png)

Key Generation
![](image/Pasted%20image%2020250215135709.png)


---

RSA with The crypto-Module of Node.js
```js
const crypto = require('crypto');

// Generate an RSA key pair
const { publicKey, privateKey } = crypto.generateKeyPairSync('rsa', {
  modulusLength: 2048, // Length of the RSA key in bits
  publicKeyEncoding: {
    type: 'spki', // Public Key format (SPKI)
    format: 'pem'
  },
  privateKeyEncoding: {
    type: 'pkcs8', // Private Key format (PKCS8)
    format: 'pem'
  }
});

// Print the keys
console.log('Private Key:\n', privateKey);
console.log('Public Key:\n', publicKey);

// Message to be encrypted
const message = 'This is a secret message to be encrypted with RSA-OAEP';

// Encrypt the message using RSA-OAEP with SHA-256
const encryptedMessage = crypto.publicEncrypt({
  key: publicKey,
  padding: crypto.constants.RSA_PKCS1_OAEP_PADDING, // Use OAEP padding
  oaepHash: 'sha256' // Hash function for OAEP padding
}, Buffer.from(message));

// Convert the encrypted message to base64 for easier output
console.log('Encrypted Message (base64):', encryptedMessage.toString('base64'));

// Decrypt the message using RSA-OAEP with SHA-256
const decryptedMessage = crypto.privateDecrypt({
  key: privateKey,
  padding: crypto.constants.RSA_PKCS1_OAEP_PADDING, // Use OAEP padding
  oaepHash: 'sha256' // Hash function for OAEP padding
}, encryptedMessage);

// Print the decrypted message
console.log('Decrypted Message:', decryptedMessage.toString('utf8'));
```


### 3.3.1 生成RSA key WITH OPENSSL

![](image/Pasted%20image%2020241031152338.png)


![](image/Pasted%20image%2020241031152318.png)

![](image/Pasted%20image%2020241031152352.png)

![](image/Pasted%20image%2020241031152427.png)


## 3.4 Elliptic Curve Cryptography (ECC) key 

- _**Elliptic Curve Cryptography**_ (**_ECC_**) is modern public-key cryptography approach based on the algebraic structure of elliptic curves over _**finite elements**_
- Higher levels of security with smaller key sizes than RSA
- Based on the _**Weierstrass form**_, which is a standard equation used to describe elliptic curves:
    - y2=x3+a⋅x+by2=x3+a⋅x+b

- where aa and bb are constants that satisfy a specific condition to ensure that the curve is smooth (without cusps and self-intersection) 
- Figure shows the curve's points over the set of real numbers, while ECC is based on finite elements, for example integers (modn)(modn), where nn is a prime number.

### 3.4.1 Key Generation

Step 1: Choose an Elliptic curve

Before generating a key, a curve must be selected. Well-known curves include
- secp256k1 (used by Bitcoin)
- prime256v1 (also known as P-256)
- Curve25519 (used by modern cryptography libraries)

Step 2: Key Generation
- _**Private key**_: A randomly chosen number in the range `[1,n−1]`, where nn is the order of the base point GG on the chosen elliptic curve
- Note: The _**order of a point**_ PP on an elliptic curve is the smallest positive integer mm with m⋅P=Om⋅P=O, where OO is the _**point at infinity**_ or the _**identity element**_ for this group
- _**Public key**_: The result of P=d⋅GP=d⋅G, where
    - PP is the public key,
    - dd is the private key (a large random integer), and
    - GG is the base point (generator) on the curve.



### 3.4.2 ECC with OpenSSL 


![](image/Pasted%20image%2020241031152630.png)


![](image/Pasted%20image%2020241031152643.png)



# 4 Digital Signatures


![](image/Pasted%20image%2020250215140704.png)


- ==A digital signature is a cryptographic technique used to verify the authenticity and integrity of digital messages or documents==
- Widely used in secure communication, electronic contracts, software distribution, and other areas


- **Integrity**: Any modification to the message leads to a different hash, and received and calculated signatures won't match
- ​**Authentication**: Since only the signer has access to the private key, the digital signature authenticates the sender's identity
- **Non-repudiation**: The signer cannot later deny having sent the message, because the signature can only be created using their private key


Key components of a digital signature system (重要)
- **Hash function**: Function that generates a fixed-size digest from the input message, which acts like fingerprint of that message
    - hashed data
- ​**Private key**: The signer uses their private key to encrypt the digest and create the digital signature
    - signed/encrypted  with key 
- ​**Public key**: The recipient uses the signer's public key to decrypt the digital signature and verify its authenticity and integrity



Use cases of digital signatures
- **Secure Web Browsing**: Digitale signatures guarantee authenticity and integrity of messages in protocols like SSL/TLS
- ​**Secure Email**: Digital signatures are used in protocols like S/MIME (Secure/Multipurpose Internet Mail Extensions) to ensure that emails are authentic and have not been tampered with
- **Software Distribution**: Software developers sign their programs to assure users that their software has not been altered or manipulated since its creation
- **Digital Certificates**: Certification authorities (CAs) use digital signatures to issue digital certificates, which are used to verify the identity of communication partners in the Internet  
- ​**Electronic Contracts**: Digital Signatures are used in legal documents to provide authentication and prevent the signer from denying that they signed the contract
- **Verifiable Credentials**: Digital signature are used by the issuers of verifiable credentials to assert that a claim about an identity holder is true



## 4.1 Hashing 

- ==A _**cryptographic hash function**_ is a mathematical algorithm that transforms an input (message) of arbitrary length* into a fixed-size string of bytes, usually in hexadecimal format==
- The output is called a _**hash value**_ or _**message digest**_


![](image/Pasted%20image%2020250215141103.png)


Properties of Hash Functions
- **Deterministic**: The same input will always produce the same hash
- **Quick to compute**: Hash functions are designed to be fast
- **Pre-image resistance**: Hash function art one-way function, i.e., given a hash value, it should be computationally infeasible to find the original input
- **Avalanche effect**: Small change in the input, results in a large change of the output 
- **Collision resistance**: It should be very hard to find two different inputs that produce the same hash value
- **Second pre-image resistance**: Given an input and its hash, it should be difficult to find another input that produces the same hash



Use Cases of cryptographic Hash Functions
- **Data Integrity Verification**: Used to verify whether downloaded files match the original file
- **Digital Signatures**: A messages or document is hashed, and the hash is signed with a private key
- **Password hashing**: Instead of storing passwords in plaintext or ciphertext, systems store password hashes
- **Cryptocurrencies**: Blockchains rely on cryptographic hash functions to verify transactions and secure the blockchain through proof-of-work mechanisms 
- **Message Authentication Codes (MAC)**: Used in combination with secret keys to verify the integrity of a message


Common Cryptographic Hash Functions
- **SHA-256 and SHA-512 (Secure Hash Algorithm 256/512-bit)**: Part of the SHA-2 family, it produces a 256/512-bit digest and is commonly used in cryptocurrencies, digital signatures, and SSL/TLS certificates
- **MD5 (Message Digest 5)**: An older and less secure function, MD5 generates a 128-bit hash (not recommended for security-sensitive applications due to vulnerabilities, i.e., collisions)
- **SHA-1 (Secure Hash Algorithm 1)**: Produces a 160-bit hash, but is meanwhile considered insecure



## 4.2 Signing with RSA – RSA-PSS

![](image/Pasted%20image%2020250215141305.png)

- _**RSA Probabilistic Signature Scheme**_ (**RSA-PSS**) adds extra padding bits with a random component to the hashed message MM because of similar issues as with textbook RSA for encryption
- Like in RSA-OAEP, a **_pseudorandom number generator_** (PRNG) ensures the indistinguishability and non-malleability of message hashes by making the encryption probabilistic
- Padding algorithm works similar as for RSA-OAEP but it adapted to the specific hash sizes and security needs of digital signatures



![](image/Pasted%20image%2020250215141628.png)



 RSA-PSS with the crypto module of node.js
```js
 const crypto = require('crypto');

// Generate an RSA key pair
const { publicKey, privateKey } = crypto.generateKeyPairSync('rsa', {
  modulusLength: 2048, // Length of the RSA key in bits
  publicKeyEncoding: {
    type: 'spki', // Public Key format (SPKI)
    format: 'pem'
  },
  privateKeyEncoding: {
    type: 'pkcs8', // Private Key format (PKCS8)
    format: 'pem'
  }
});

// Print the keys
console.log('Private Key:\n', privateKey);
console.log('Public Key:\n', publicKey);

// Message to be signed
const message = 'This is a message to be signed with RSA-PSS';

// Sign the message using RSA-PSS
const signature = crypto.sign('sha256', Buffer.from(message), {
  key: privateKey,
  padding: crypto.constants.RSA_PKCS1_PSS_PADDING, // Use PSS padding
  saltLength: crypto.constants.RSA_PSS_SALTLEN_DIGEST // Salt length equal to the hash size (SHA-256)
});

// Convert the signature to base64 for easier output
console.log('Signature (base64):', signature.toString('base64'));

// Verify the signature using the corresponding public key
const isVerified = crypto.verify('sha256', Buffer.from(message), {
  key: publicKey,
  padding: crypto.constants.RSA_PKCS1_PSS_PADDING,
  saltLength: crypto.constants.RSA_PSS_SALTLEN_DIGEST
}, signature);

console.log('Signature valid:', isVerified);
```



## 4.3 Signing with ECC


- The _**Elliptic Curve Digital Signature Algorithm**_ (ECDSA) ist the most common method for signing data in ECC and widely used in protocols like SSL/TLS and in blockchain systems
- Private key is used for signing, and the public key is used for verifying the signature


 ECDSA with the Crypto-Module of Node.js
```js
const crypto = require('crypto');

// Generate an ECC key pair using the P-256 curve (also known as prime256v1)
const { publicKey, privateKey } = crypto.generateKeyPairSync('ec', {
  namedCurve: 'prime256v1', // Named elliptic curve
  publicKeyEncoding: {
    type: 'spki', // Public key encoding format (SPKI)
    format: 'pem'
  },
  privateKeyEncoding: {
    type: 'pkcs8', // Private key encoding format (PKCS8)
    format: 'pem'
  }
});

// Print the keys
console.log('Private Key:\n', privateKey);
console.log('Public Key:\n', publicKey);

// Message to be signed
const message = 'This is a message to be signed using ECDSA';

// Sign the message using ECDSA (Elliptic Curve Digital Signature Algorithm)
const signature = crypto.sign('sha256', Buffer.from(message), {
  key: privateKey,
  dsaEncoding: 'der' // Use DER encoding for the signature (commonly used with ECDSA)
});

// Convert the signature to base64 for easier output
console.log('Signature (base64):', signature.toString('base64'));

// Verify the signature using the public key
const isVerified = crypto.verify('sha256', Buffer.from(message), {
  key: publicKey,
  dsaEncoding: 'der'
}, signature);

// Print whether the verification was successful
console.log('Signature valid:', isVerified); 
```



![](image/Pasted%20image%2020250215141948.png)


## 4.4 RSA and ECC comparision 

|Criteria|RSA|ECC|
|---|---|---|
|Mathematical Basis|Integer factorization problem|Elliptic Curve Discrete Logarithm Probleme|
|Key Size|Larger (2048–4096 bits or more)|Smaller (224-521 bits)|
|Performance|Slower, more resource-intensive|Faster, more efficient|
|Security per Bit|Requires large key sizes for security|Higher security with smaller key sizes|
|Efficiency|Higher resource and power usage|Lower resource and power usage|
|Adoption|Widespread and established|Gaining popularity|
|Quantum Resistance|Vulnerable to quantum attacks|Vulnerable to quantum attacks|


- The difficulty of solving the Elliptic Curve Discrete Logarithm Problem grows exponentially with key size
- The difficulty of factoring large prime numbers grows sub-exponentially (faster than polynominal but slower than exponential growth) with key size
- For a given increase in key size, ECC gains more security than RSA and thus allows much smaller key sizes for the same security level

- RSA has larger key sizes and is based on comparatively expensive modular exponentiation
- ECC has smaller key sizes and is based on comparatively cheap point multiplication
- ECC requires less computational resources than RSA and is much more power efficient


