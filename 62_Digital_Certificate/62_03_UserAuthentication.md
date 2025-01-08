

# 1 SYNCHRONOUS MESSAGE EXCHANGE

- Synchronous application-layer protocol for client/server communication\
- HTTP Request messages are always sent by the client (browser), HTTP Response messages are always sent be the server Assumption: HTTP messages are only exchanged over connections secured with TLS
- HTTP/1.0: published in 1996, contained many performance fl aws
- HTTP/1.1: 1997, enabled keepalive connections and improved caching
- HTTP/2: 2015, improved pay load speed by multiplexing, data compression, prioritization or requests, and server push 
- HTTP/3: 2022, relies on QUIC as transport protocol (up to four times faster than HTTP/1.1)


# 2 AUTHENTICATION FACTORS

> Authentication factors are methods used to verify the identity of a user. They are categorized into three primary types:
KNOWLEDGE, POSSESSION, AND INHERENCE

![](image/Pasted%20image%2020241214193736.png)

possession:  拥有，持有；个人财产，所有物
Inherence:  内在；固有；天赋

- To establish trust between an entity and the system it is trying to access must provide an evidence that it is the entity it claims to be
- A factor refers to a specific type of evidence used to prove an entity's identity
- Authentication factors are independent from each other and are categorized based on the source of evidence

|Factor|How it works|Pros|Cons|
|---|---|---|---|
|Knowledge|User provides a secret stored in the system|Simple and inexpensive to implement|Prone to theft, phishing, or forgetting|
|Possession|User presents an object they own|Difficult ot duplicate or steal without physical access|Can be lost, stolen, or damaged|
|Inherence^|System verifies unique pghysical characteristics|Hard to forge and convenient for users|Expensive to implement; privacy concerns|

---


Single-Factor Authentication (SFA)
- Relies on a single factor, typically knowledge (passwords)
- Less secure because if the single factor is compromised, access is granted


Multi-Factor Authentication (MFA)
- Involves using two (2FA) or more factors from different categories to authenticate a user 
- Example: a combination of a password (knowledge) and a One-Time Password (OTP) from a mobile device (possession)
- MFA significantly enhances security because compromising multiple independent factors is more challenging
- Mandatory for industries that handle financial data, healthcare information, government systems, and sensitive perosnal information

---

Additional factors
- **Location Factor (Somewhere you are)**
    - Refers to geographic or network location of the user
    - Examples: IP addresses, GPS coordinates
- **Behavioral Factor (Something you do)**
    - Relates to unique patterns in the user's behavior
    - Examples: typing speed, mouse movements, usage patterns


## 2.1 **Knowledge Factor ("Something You Know")**

- **Definition**: This factor relies on information that the user knows and can provide to authenticate themselves.
- **Examples**:
    - Passwords
    - PINs
    - Security questions (e.g., "What was the name of your first pet?")
- **Strengths**:
    - Easy to implement and widely used.
- **Weaknesses**:
    - Vulnerable to attacks like phishing, brute force, or social engineering.
    - Users may forget passwords or choose weak ones.

## 2.2 **Possession Factor ("Something You Have")**

- **Definition**: This factor relies on physical objects or digital tokens the user possesses to prove their identity.
- **Examples**:
    - Smartcards
    - Security tokens
    - Mobile phones (e.g., receiving OTPs via SMS or authentication apps)
    - Hardware security keys (e.g., YubiKey)
- **Strengths**:
    - Harder to replicate than knowledge-based factors.
    - Adds an extra layer of security when combined with other factors.
- **Weaknesses**:
    - Risk of loss, theft, or damage to the object.
    - Users need to carry or maintain the device.

## 2.3 **Inherence Factor ("Something You Are")**

- **Definition**: This factor relies on inherent physical or behavioral traits unique to the user.
- **Examples**:
    - Biometric data:
        - Fingerprints
        - Facial recognition
        - Voice recognition
        - Iris/retina scans
    - Behavioral biometrics:
        - Typing patterns
        - Gait analysis
- **Strengths**:
    - Difficult to forge or steal.
    - Convenient for users as they don't need to remember or carry anything.
- **Weaknesses**:
    - Can be affected by injuries or environmental factors (e.g., wet fingers affecting fingerprint scanners).
    - Privacy concerns regarding storage and misuse of biometric data.


## 2.4 Regulations mandating MFA

|Regulation|Institution|Region|Industry|MFA Requirement|Purpose|
|---|---|---|---|---|---|
|Payment Card Industry Data Security Standard (PCI DSS)|PCI SSC (Payment Card Industry Security Standards Council)|Global|Payment|Required for remote access to Cardholder Data Environment|Protect payment card data|
|**General Data Protection Regulation (GDPR)**|EU (European Union)|EU|General|Recommended as part of technical measures|Secure personal data|
|Health Insurance Portability and Accountability Act (HIPAA)|HHS (U.S. Department of Health and Human Services)|USA|Healthcare|Recommended for remote access|Protect health information|
|Cybersecurity Maturity Model Certification (CMMC)|DoD (U.S. Department of Defense)|USA|Defense contractors|Required for handling Controlled Unclassified Information|Secure DoD systems and data|
|**Payment Services Directive 2 (PSD2)**|EU (European Union)|EU|Financial institutions|Required for Strong Customer Authentication (SCA)|Secure online payments|
|Federal Financial Institutions Examination Council (FFIEC) Guidelines|FFIEC (Federal Institutions Examination Council)|USA|Financial services|Required for high-risk transactions|Prevent fraud in banking|
|New York Department of Financial Services Cybersecurity Regulation|NYDFS (New York Department of of Financial Services)|USA (NY)|Financial services|Required for remote and privileged access|Secure financial systems in NY|
|National Institute of Standards and Technologies Special Publication 800-63B|NIST (National Institute of Standards and Technology)|USA|Federal systems|Required for high-assurance authentication|Protect federal systems and sensitive data|


## 2.5 Attacks 

- Password-based attacks
    - Brute-Force Attacks: Repeatedly trying different combinations of usernames and passwords 
    - Credential Stuffing: Using previously breached username-password pairs on multiple services, relying users' reuse of credentials  
    - Dictionary/Rainbow Attacks: Using common words, phrases, or precompiled password lists to guess user passwords 
- Phishing
    - Attackers trick users into providing their credentials by pretending to be a legitimate service, often through deceptive emails, fake websites, or messages​ 
- Man-in-the-middle Attacks
    - Intercepting authentication credentials during transmission, especially when the connection is not encrypted (e.g., lack of HTTPS)
- Replay Attacks
    - Reusing previously intercepted authentication data (e.g., session tokens, credentials) to impersonate a user​ 
- Session Hijacking
    - Stealing active session cookies from the user to impersonate them without knowing their credential
- Social Engineering
    - Manipulating users into revealing sensitive information (e.g., credentials) or performing actions that compromise security
- Key Logging and Malware
    - Using malicious software or hardware to capture keystrokes, screen inputs, or data directly from the user's device
- Insider Threats
    - Employees or trusted individuals with access to authentication systems misuse their access to compromise credentials



# 3 Authentication with Passwords

General Approach
- Most common form of identifier and authentication factor is a username and password
- By entering a username, the user lays claim to an account, and the password is used to authenticate the user
- The system uses the account that the username represents to associate attributes with the account holder

![](image/Pasted%20image%2020241214194249.png)

## 3.1 Basic Authentication

- The use inputs username and password in a browser popup and sends them in the Authorization header field of an HTTP request
- The credentials username:password are sent Base64-encoded
- The server decodes the Base64 string and validates the credential
- On subsequent requests to the same server (or realm), ==the browser remembers the credentials and automatically fills the Authorization header field with the credentials ==


Pros
- Simple to implement
- Supported widely across HTTP clients and servers

Cons
- ==Credentials are sent with every request== 重要的 劣势 
- Insecure unless used over TLS (passwords can be intercepted in plaintext)
- No logout mechanism, i.e., once credentials are cached, they persist for the browser session
- No support for token-based or session-based revocation


Base64: 
- Base64 is an encoding scheme that converts binary data (e.g., bytes) into textual form using only ASCII characters
- Binary data is divided into 6 bits
- If the input length is not a multiple of 3 bytes, padding characters (=) are added at the end
- Each 6-bit group is mapped to a character in the Base64 alphabet, which consists of A-Z, a-z, 0-9, +, /
- Note: Base64 is only an encoding scheme – data that is Base64-encoded can be simply decoded without a key or a shared secret, i.e., Base64 does not encrypt or decrypt data


![](image/Pasted%20image%2020241214194448.png)



## 3.2 Form-based Authentication (I/II)

![](image/Pasted%20image%2020241214195018.png)

- Basic Authentication does not support session cookies and server-side tracking, and prompts the user via a browser-based popup to enter their credentials  
- **_Form-based authentication_** is a better alternative, where users submit their credentials via na HTML login form
- The credentials are sent in the body of an HTTP POST request to the server
- ==The server validates the credentials and creates a session identifiers, which is returned to the browser in a dedicated cookie==
- ==On subsequent requests, the cookie is sent automatically by the browser in the Cookie header==


Advantages
- The server retains control over sessions, allowing activities like tracking activity or session revocation  取消 废除 
- Browsers handle cookie automatically, simplifying client-side implementation

## 3.3 How to store password securely

- If passwords are stored in plaintext in the backend's database and the databased is breached, attackers gain immediate access to all passwords
- If passwords are stored encrypted and the database is breached, attackers might also be able to steal the key for password decryption


![](image/Pasted%20image%2020241214195518.png)

- Storing hashed passwords ensures that even if the database is breached, the passwords are not directly exposed
- ==A hash function transforms a password into a fixed-length string of characters in a way that cannot be reversed to retrieve the original password==
- ==The user sends the password in plain text, whereupon the server hashes it and compares the hash with the hash in the database==
- Note: Before hashing, a salt should be appended (see next slides)


![](image/Pasted%20image%2020241214195741.png)


password 变成 hash using target system's hashing algorithm   , 和 存在 database 中的 hash 去比较 

## 3.4 Dictionary and Brute-Force Attacks

![](image/Pasted%20image%2020241214195943.png)

attacker 先猜 大概有那些 possible passwords  in plaintext , 然后用 target system 的 hashing algorithm 去计算 这些 passwords 的 hash .构成一个 plaintext-hash  对, 储存起来. 
然后attacker截获一个 hash, 通过 和之前的 hash-plaintext pair 比对, 得到 对应的 password in plain text 
- A _**dictionary**_ in password cracking is a precompiled list of possible passwords or commonly used words
- An attacker uses it in a _**dictionary attack**_ to systematically attempt password guesses against the target system
- Each word in the file is hashed during the attack, and its hash is compared with the stolen password hash(es)


---

![](image/Pasted%20image%2020241214200007.png)

- A _**brute-force attack**_ tries every possible combination of characters up to a given length
- They are very computationally expensive, and are usually the least efficient in terms of hashes cracked per processor time, but they always eventually find the password
- Password should be long enough that searching through all possible character strings to find it will took too long to be worthwhile


attacker 尝试    all possible character combination (  passwords  in plaintext 的各种组合)   , 然后用 target system 的 hashing algorithm 去计算 这些 passwords 的 hash .构成一个 plaintext-hash  对, 储存起来. 
然后attacker截获一个 hash, 通过 和之前的 hash-plaintext pair 比对, 得到 对应的 password in plain text 

## 3.5 Lookup and Reverse Lookup Tables

![](image/Pasted%20image%2020241214200147.png)

- A _**lookup table**_ is precomputed table of password hashes and their corresponding plaintext passwords
- The attacker precomputes hashes for a large list of possible passwords (obtained from a dictionary) using the target system's hashing algorithm  
- During an attack, the the attacker simply looks up the hash in the table to find the corresponding plaintext
- Extremely fast because it avoids dynamic hash computation, but requires large storage requirements because all potential hashes for many passwords take significant space


----


![](image/Pasted%20image%2020241214200217.png)

- _**Reverse lookup tables**_ allow an attacker to apply a dictionary or brute-force attack to many hashes at the same time, without pre-computing a lookup table
- The attacker creates a lookup table that maps each password hash from the compromised user account database to a list of users who had that hash  
    - 从database 的 不同users  可能用 同一个密码, 有同样的 hash 
- The attacker then hashes each password guess (based on a dictionary) and uses the lookup table to get a list of users whose password was the attacker's guess
    - 利用之前的 dictionary ,或者 这些hash 的 对应的 password in plaintext.  然后 就有 这个 password 那些 user 再用 
- This attack is especially effective because it is common for many users to have the same password



## 3.6 Rainbow Tables 

- _**Rainbow tables**_ are a specialized type of precompiled lookup table used in password cracking
    - Rainbow Tables 是attacker 用来破解密码的 
- They leverage _**hash chains**_ to reduce the amount of storage needed for precomputed hashes

![](image/Pasted%20image%2020241214200512.png)



How Rainbow Tables work
- A hash chain starts with an initial plaintext (e.g., a possible password)
- The plaintext is hashed with the target system's hash function
- The hash is passed through a ==reduction function==, which converts the hash back into a plaintext-like value
- The process is repeated multiple times, forming a hash chain
- The rainbow table then consists of multiple hash chains
- Instead of storing the every hash and plaintext in the chain, ==rainbow tables only store the start and end plaintexts of each chain==


Reduction function
- A _**reduction function**_ is a deterministic algorithm that maps a hash (e.g., a binary or hexadecimal string) back into a valid plaintext candidate
- It is a mapping from the hash space (e.g., hexadecimal characters) to the plaintext space (e.g., a-z, 0-9)
- It does not create the original plaintext from the hash but generates a plausible new plaintext for the next step in the chain
- It guarantees that each hash value is mapped onto a valid plaintext, but it cannot be excluded that collisions occur, i.e., different hash chains result in the same plaintext-like value
- ==To reduce the probability of collisions, rainbow tables use different reduction functions R1,..., Rn in the columns of the table==



|Brute Force|Dictionary|Lookup Tables|Rainbow Tables|
|---|---|---|---|---|
|**Description**|Systematically tries all possible passwords|Uses a predefined list of common passwords|Precomputed hash-to-plaintext mappings|Precomputed hash chains to reduce storage|
|**Speed**|Slow (real-time computation for each attempt)|Faster than brute-force (limited by dictionary size)|Instant lookup (precomputed)|Slower than lookup tables (recomputes chains)|
|**Storage Requirements**|None|Small (list of potential passwords)|Large (one entry per password-hash-pair)|Medium (start and end of chains only)|
|**Precomputation needed**|No|No|High (compute all password-hash pairs)|High (compute hash chains)|
|**Coverage**|Complete (all possible passwords)|Limited to words in the dictionary|complete (if precomputed for all plaintexts)|Partial (depends on chains and length)|
|**Resistance to Salting**|Effective (computes hashes on the fly)|Effective (computes hashes on the fly)|Useless (precomputed for unsalted hashes)|Useless (precomputed for unsalted hashes)|
|**Scalability**|Scales with computational power|Scales with size of dictionary|Scales poorly (requires large storage)|Scales better (uses less storage)|
|**Typical Use Case**|Cracking short, simple passwords|Guessing common or likely passwords|Reverse-engineering unsalted hashes|Cracking large password spaces with unsalted hashes|


### 3.6.1 Using a Rainbow Table

- A rainbow table only stores the start and end plaintexts of each hash chain 
- To regenerate the original password of a hash, pass the hash of the wanted password (就是 client 方 发送到 server 方的) through the last reduction function and compare the result with all end plaintexts in the table 
- If no match is found, start with the previous reduction function and apply all hash and reduction steps until the last reduction function is reached, and so on
- If a match occurs, the hash chain containing the original password has been found
- From the start plaintext of that chain, apply all hash and reduction functions until the original hash is reached – the input of that hash function is the original password

![](image/Pasted%20image%2020241214201524.png)


## 3.7 Password Salting

![](image/Pasted%20image%2020241214202344.png)


- _**Password salting**_ is technique used to enhance the security of stored passwords ==by adding a unique, random value (_**salt**_) to each password before it is hashed==
- It defends against common attacks like rainbow tables and lookup tables and password reuse across systems
- ==Salts should be long enough (e.g., 16+ bytes), to ensure uniqueness, and created by a secure random number generator ==
    - 这个 salts 不是只包含了一位数字, 而是 很长的数字 
- Salts should never be used across passwords or users – each user's password should have its own equal salt
- Salts are no secrets and can be stored in plaintext in the database along with the database hash
- Password hashing renders lookup and rainbow tables inoperative 无效力的 as they had to generate dedicated tables for each hash, which economically infeasible
- Dictionary and brute-force attacks would be possible as the salt can be obtained together with the password from the database, but salting lengthen passwords and the computations must be utilized for one password only, which is again not economical
    - salting 加长了 passwords, 所以 Dictionary and brute-force attacks is not economical 


## 3.8 Digest Authentication


Digest 理解，领悟；消化

![](image/Pasted%20image%2020241214210526.png)


- _**Digest Authentication**_ is an HTTP authentication protocol designed to improve security over Basic Authentication by not transmitting plaintext password over the network
- Based on a challenge-response mechnanism with cryptographic hashing
- The server requests the client to log in with a **`401 Unauthorized`** message (like in Basic Authentication), containing:
    - 就是说  with a **`401 Unauthorized`** message 也可以成功longin 了 
    - `**realm**`: A string defining the protected domain
    - `**nonce**`: A random, unique value, generated by the server
    - `**qop**` (quality-of-protection): `**auth**` (authentication only) or `**auth-int**` (authentication with integrity)
    - `**opaque**`: used for server-side validation and session integrity


- The client merges `**username**`, `**realm**`, `**password**`, the used HTTP `**method**`, the invoked `**URI**`, `**nonce**`, a nonce counter `**cnonce**`, and `**qop**` in a two-step hash procedure (see figure) and returns the resulting hash to the server
- The server validates the response by calculating its own version of the hash and comparing the calculated with the received response
- As the server does not get the original password during the login, it cannot apply hashing and salting for password storage
- Instead, the server can store passwords in plaintext (not recommended), or it stores the `**A1**` value (not recommended either)
- In case of a database breach, the attacker can get the **`A1`** value along with additional information (`**username**`) and illegitimately login with a user's credentials
- Because of its security risks, Digest Authentication has been deprecated by a lot of software


# 4 ONE-TIME PASSWORDS

## 4.1 Login – Basic Approach

![](image/Pasted%20image%2020250108105411.png)

- _**One-Time Passwords**_ (_**OTPs**_) are a widely used method for secure authentication
- Each OTP is valid for only one authentication attempt
- Unlike static passwords, OTPs change with every use or at regular intervals
- OTPs often complement other security measures, like static passwords, in 2FA/MFA
- OTPs mitigate replay attacks and enhance security
- They are not stored persistently, minimizing the risk from database leaks
- OTPs can be delivered through apps, SMS, email, or hardware devices


## 4.2 Registration with HOTP/TOTP

![](image/Pasted%20image%2020250108105540.png)


- _**HMAC-based OTP**_ (_**HOTP**_) generates OTPs using HMAC-SHA-1 and is **event-based**
- The OTP remains valid until it is used or the counter is incremented
- OTP calculation depends on a shared secret and a counter
- If the counters on both sides get out of sync, the OTP calculation may fail

- _**Time-based OTP**_ (_**TOTP**_) is an extension of HOTP and is **time-based**
- OTPs are valid only for a short time period (e.g., 30 seconds)
- OTP calculation depends on UNIX time
- If clocks on both sides are significantly out of sync, the OTP calculation may fail

![](image/Pasted%20image%2020250108105918.png)

---

HOTP/TOTP Calculation – Detailled View

![](image/Pasted%20image%2020250108105943.png)



## 4.3 Comparison of OTP Schemes


|-|HOTP|TOTP|Email|Push Notification|SMS|
|---|---|---|---|---|---|
|**Mechanism**|Event-based OTP generated via HMAC with a counter|Time-based OTP generated via HMAC with the current time|Server-side generation of OTP and delivery via email|Server-side generation of OTP and delivery via push notification|Server-side generation of OTP and delivery via SMS|
|**Dependency**|Requires a shared secret and synchronized counters|Requires a shared secret and synchronized clocks|Requires email access|Requires a smart phone with push capabilities|Requires a valid phone number|
|**Security Strength**|Strong if the shared secret is secure and the counter is in sync|Strong if the shared secret is secure and the time sync is maintained|Medium – email accounts can be compromised|High – encrypted delivery reduces interception risks|Medium – vulnerable to SIM swapping and SMS interception|
|**Replay Attack Resilience**|Very high – OTP is valid only for a specific counter|Very high – OTP is valid only for a specific time window|High: OTP expires after use or a set time|Very high – valid only for the specific session|Medium – OTP can be intercepted during delivery|
|**Phishing Vulnerability**|Moderate – user might be tricked into providing their OTP to a phishing site|Moderate – user might be tricked into providing their OTP to a phishing site|Moderate – user might be tricked into providing their OTP to a phishing site|Moderate – user might be tricked into providing their OTP to a phishing site|Moderate – user might be tricked into providing their OTP to a phishing site|
|**Interception Risk**|Low – OTP is generated locally|Low – OTP is generated locally|High – email transmission can be intercepted|Low – encrypted channels reduce interception risks|High – SMS messages can be intercepted or redirected|
|**Delivery Latency**|Instant (local generation)|Instant (local generation)|Depends on email server and network delays|Near-instant via push|Depends on carrier network|
|**Offline Capability**|Yes|Yes|No|No|No|
|**Sync Requirements**|Counter synchronization|Time synchronization|No|No|No|




# 5 AUTHENTICATION WITH CHALLENGE/RESPONSE

## 5.1 General Approach

- ==In challenge/response authentication, the server generates a random string (_**challenge**_), and the client is requested to manipulate the string according to some agreed upon rules or algorithms and send it back to the server (_**response**_)==
- **Cryptographic shared secret**: The server sends a challenge and the client creates a message digest using the shared secret and returns it to the server
- **Mathematical calculation**: The server asks the client to perform an agreed upon calculation and returns it to the server
- **Public key cryptography**: The client encrypts the challenge with a private key and the server validates the challenge with the associated public key
- Challenge/response with public key cryptography can be implemented with client certificates or FIDO

![](image/Pasted%20image%2020250108110305.png)

signed response with public key 



![](image/Pasted%20image%2020250108110433.png)

- In password and OTP authentications, the password is sent to the server
- The server needs to store passwords and protects them against theft

![](image/Pasted%20image%2020250108110445.png)

- ==In challenge/response authentication (with public key cryptography), the secret never leaves the client device==  他的优点 
- The server needs to store only the public key, which does not need to be protected against theft

## 5.2 Fido2

![](image/Pasted%20image%2020250108110557.png)

_**FIDO2**_ (_**Fast Identity Online 2**_) is an authentication framework, which is developed by the FIDO Alliance and the World Wide Web Consortium (W3C) and which encompasses the following components

- _**External Authenticator**_: A device (e.g., a USB stick) that generates and stores private keys or
- **_Internal_** (_**platform-based**_) _**Authenticator**_: Integrates into the user's device (smartphone, computer, tablet) leveraging the device's built-in hardware (Trusted Execution Environment, Secured Enclave)
- _**Relying Party**_: The service or website requesting authentication
- _**Client Platform**_: The user's device and browser or OS, acting as intermediary between the authenticator and the relying party
- _**WebAuthn**_ (_**Web Authentication API**_): W3C standard that provides an interface for web developers to register and authenticate users by FIDO-compliant authenticators
- _**CATP**_ (_**Client-to-Authenticator Protocol**_): Defines how external or internal authenticators communicate with the client device

Key Features
- **Passwordless Authentication**: Eliminates the need for passwords by using public-key cryptography for authentication
- **Phishing Resistance**: Authentication is tied to the specific domain of the relying party, preventing credential reuse on malicious sites
- **Cross-Platform Interoperability**: Works on all major browsers, operating systems, and devices
- **Strong Privacy**: No personal information or biometrics are shared with the server
- **Reduced Costs**: Minimizes password-related issues like resets, helpdesk calls, and breaches
- **Better User Experience**: Simplifies login processes with biometrics or security keys
- **Multi-Factor Authentication**: Combines possession (authenticator) with inherent (biometrics) or knowledge (PIN) factors

## 5.3 Registration with Fido2

![](image/Pasted%20image%2020250108111030.png)


- ==The goal of the FIDO2 registration process it to create credentials and send an attestation together with the public key of the credential to the relying party==
- The combination of private and public key is called _**credential**_ in FIDO2
- The private key is sometimes also called a _**passkey**_
- _**Attestation**_ (证明) in WebAuthn refers to the process by which an authenticator proves to a relying party that it is a genuine device made by a trusted manufacturer and that it complies with security standards
- **Basic Attestation**: The authenticator provides a signature using a manufacturer-specific attestation key
- **Self Attestation**: The authenticator signs the attestation statement using the same private key generated for the user
- **Attestation with Anonymity**: Provides proof of authenticity without revealing the device's manufacturer or model
- **None Attestation**: The authenticator provides no attestation information


- When a user registers a new authenticator with a relying party, the authenticator provides an attestation statement along with the public key
- The attestation statement is a cryptographic proof including
- Information about the authenticator model and manufacturer
- A signature created by a key associated with the authenticator's manufacturer
- Metadata that describes the authenticator's capabilities (e.g., biometric support, hardware security, etc.)
- The relying party can verify the attestation statement using the attestation certificate, which is linked to the manufacturer's attestation root key


- _**Device binding**_ refers to the process of establishing a long-lasting relationship between a specific device and a user's account
- Device binding happens when the public key generated by the authenticator is linked to the user's account at the relying party
- Once the relationship is established, the device can authenticate the user without requiring re-registration

---

Javascript Example
Client
```javascript
async function registerWebAuthn() {
  const username = document.getElementById('username').value;

  // Step 1: Fetch registration options from the server
  const response = await fetch('/webauthn/register-options', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username }),
  });

  if (!response.ok) {
    console.error('Failed to fetch registration options');
    return;
  }

  const options = await response.json();

  // Step 2: Decode Base64URL to ArrayBuffer
  options.challenge = Uint8Array.from(atob(options.challenge), c => c.charCodeAt(0));
  options.user.id = Uint8Array.from(atob(options.user.id), c => c.charCodeAt(0));

  // Step 3: Call navigator.credentials.create()   重要 
  const credential = await navigator.credentials.create({ publicKey: options });

  // Step 4: Prepare credential data for server
  const credentialData = {
    id: credential.id,
    rawId: btoa(String.fromCharCode(...new Uint8Array(credential.rawId))),
    response: {
      attestationObject: btoa(String.fromCharCode(...new Uint8Array(credential.response.attestationObject))),
      clientDataJSON: btoa(String.fromCharCode(...new Uint8Array(credential.response.clientDataJSON))),
    },
    type: credential.type,
  };

  // Step 5: Send the credential to the server
  const verificationResponse = await fetch('/webauthn/register', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username, credential: credentialData }),
  });

  const result = await verificationResponse.json();
  console.log('Registration result:', result);
}
```



Server
```javascript
const { generateRegistrationOptions } = require('@simplewebauthn/server');

app.post('/webauthn/register-options', (req, res) => {
  const { username } = req.body;
  const user = getUserFromDatabase(username); // Fetch user or create new user entry
  if (!user) {
    createUserInDatabase(username); // Create new user if not found
  }

  const options = generateRegistrationOptions({
    rpName: 'Example Site',
    rpID: 'example.com',
    userID: user.id, // Use a unique ID for the user
    userName: username,
    attestationType: 'direct',
    authenticatorSelection: {
      userVerification: 'preferred',
    },
  });

  req.session.challenge = options.challenge; // Store challenge for later verification
  res.json(options);
});

```

Server
```javascript
const { verifyRegistrationResponse } = require('@simplewebauthn/server');
app.post('/webauthn/register', async (req, res) => {
  const { username, credential } = req.body;
  const user = getUserFromDatabase(username);
  if (!user) {
    return res.status(400).json({ error: 'User not found' });
  }
  const verification = await verifyRegistrationResponse({
    response: credential,
    expectedChallenge: req.session.challenge,
    expectedOrigin: 'https://example.com',
    expectedRPID: 'example.com',
  });
  if (verification.verified) {
    const { credentialID, publicKey } = verification.registrationInfo;
    saveCredentialForUser(user.id, { credentialID, publicKey }); // Save credential details in database
    res.json({ success: true });
  } else {
    res.json({ success: false });
  }
});
```

## 5.4 Login with Fido2

![](image/Pasted%20image%2020250108111919.png)


- An _**assertion**_ is the response generated during the authentication (login) phase
- It contains the cryptographic proof that the user possesses the private key corresponding to the public key stored by the relying party
- When the user attempts to login, the authenticator creates an assertion to prove they control the credential's private key
- The assertion includes the server-provided challenge, signed by the private key
- The relying party uses the public key stored during registration to verify the signature in the assertion

 Login with Fido2

Javascript Example
Client
```javascript
async function loginWebAuthn() {
  const username = document.getElementById('username').value;

  // Step 1: Fetch authentication options from the server
  const response = await fetch('/webauthn/authenticate-options', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username }),
  });

  if (!response.ok) {
    console.error('Failed to fetch authentication options');
    return;
  }

  const options = await response.json();

  // Step 2: Decode Base64URL to ArrayBuffer
  options.challenge = Uint8Array.from(atob(options.challenge), c => c.charCodeAt(0));
  options.allowCredentials = options.allowCredentials.map(cred => ({
    ...cred,
    id: Uint8Array.from(atob(cred.id), c => c.charCodeAt(0)),
  }));

  // Step 3: Call navigator.credentials.get()
  const assertion = await navigator.credentials.get({ publicKey: options });

  // Step 4: Prepare assertion data for server
  const assertionData = {
    id: assertion.id,
    rawId: btoa(String.fromCharCode(...new Uint8Array(assertion.rawId))),
    response: {
      clientDataJSON: btoa(String.fromCharCode(...new Uint8Array(assertion.response.clientDataJSON))),
      authenticatorData: btoa(String.fromCharCode(...new Uint8Array(assertion.response.authenticatorData))),
      signature: btoa(String.fromCharCode(...new Uint8Array(assertion.response.signature))),
      userHandle: assertion.response.userHandle
          ? btoa(String.fromCharCode(...new Uint8Array(assertion.response.userHandle)))
          : null,
    },
    type: assertion.type,
  };

  // Step 5: Send the assertion to the server
  const verificationResponse = await fetch('/webauthn/authenticate', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username, assertion: assertionData }),
  });

  const result = await verificationResponse.json();
  console.log('Authentication result:', result);
}
```



Server
```javascript
	
const { generateAuthenticationOptions } = require('@simplewebauthn/server');
	
 
	
app.post('/webauthn/authenticate-options', (req, res) => {
	
  const { username } = req.body;
	
 
	
  const user = getUserFromDatabase(username);
	
  if (!user || !user.credentials) {
	
    return res.status(400).json({ error: 'User not found or no credentials registered' });
	
  }
	
 
	
  const options = generateAuthenticationOptions({
	
    allowCredentials: user.credentials.map(cred => ({
	
      id: cred.credentialID,
	
      type: 'public-key',
	
    })),
	
    userVerification: 'preferred',
	
  });
	
 
	
  req.session.challenge = options.challenge; // Store challenge for verification later
	
  res.json(options);
	
});

```

Server
```javascript
const { verifyAuthenticationResponse } = require('@simplewebauthn/server');
	
 
	
app.post('/webauthn/authenticate', async (req, res) => {
	
  const { username, assertion } = req.body;
	
 
	
  const user = getUserFromDatabase(username);
	
  if (!user) {
	
    return res.status(400).json({ error: 'User not found' });
	
  }
	
 
	
  const authenticator = user.credentials.find(cred => cred.credentialID === assertion.id);
	
  if (!authenticator) {
	
    return res.status(400).json({ error: 'Credential not found' });
	
  }
	
 
	
  const verification = await verifyAuthenticationResponse({
	
    response: assertion,
	
    expectedChallenge: req.session.challenge,
	
    expectedOrigin: 'https://example.com',
	
    expectedRPID: 'example.com',
	
    authenticator,
	
  });
	
 
	
  if (verification.verified) {
	
    res.json({ success: true });
	
  } else {
	
    res.json({ success: false });
	
  }
	
});
```



## 5.5 TRUSTED EXECUTION ENVIRONMENT

![](image/Pasted%20image%2020250108112208.png)

- A _**Trusted Execution Environment**_ (_**TEE**_) is a secure area of a processor that provides a safe execution space for sensitive code and data
- TEEs are designed to protect confidentiality and integrity even in the presence of compromised software or an operating system
- The TEE is isolated from the rest of the system, including the OS and other applications, to ensure that sensitive data and code cannot be accessed or tampered by other components
- Applications running in the TEE can execute securely and data processed remains confidential
- TEEs can provide cryptographic proofs (_**attestation**_) to verify that code running in the TEE is untampered and authentic 

Use Cases
- **Authentication**: Safeguarding biometric data like fingerprints or facial recognition 
- **Key Management**: Securely storing and managing cryptographic keys
- **Secure Payment**: Protecting payment credentials and processing payment-related data securely


## 5.6 Authentication Pyramid

![](image/Pasted%20image%2020250108112501.png)





# 6 BIOMETRIC AUTHENTICATION

## 6.1 CATEGORIES OF BIOMETRICS


![](image/Pasted%20image%2020250108112555.png)


- The term biometric comes from two Greek words: _**bios**_=life; _**metron**_=measure
- In information technology and computer science, **_biometric_** is defined as a means of identifying, authenticating, and controlling access by using measurable human biological data

**Physiological Biometrics**
- Related to the shape of body parts
- Examples: fingerprint, face, palm print, hand geometry, retina, iris

![](image/Pasted%20image%2020250108112724.png)

**Behavioral Biometrics**

- Related to the pattern of behaviors or actions of a person
- Examples: voice, handwritten signature, social media usage pattern, typing pattern or keystroke dynamics, heartbeat, walking pattern (gait)



## 6.2 Biometric Properties

|-|Universality|Uniqueness|Collectability|Permanency|Acceptability|
|---|---|---|---|---|---|
|**Fingerprint**|Medium|High|Medium|High|Medium|
|**Face**|High|Medium|High|Medium|High|
|**Iris**|High|High|Medium|High|Low|
|**Signature**|Medium|Low|High|Low|High|
|**Keystroke**|High|Medium|Medium|Low|Medium|
|**Gait**|High|Medium|Low|Low|Medium|

**Universality**
- Looks how common the biometrics is in the general population
- Every person using the system should possess this particular attribute

**Uniqueness**
- The biometric information used in the system must be able to differentiate one person from others

**Collectability** (**Measurability**)  (how easy to capture it )
- Relates to how difficult it is to obtain the required biometric data
- Obtained data should be easy to extract, evaluate, and measure

**Permanency**
- Collected biometrical data should last a lifetime of an individual
- Reduced the number of times the biometric data has to be collected as a person ages

**Acceptability**
- Willingness of the general public to get into the usage of the biometric system


## 6.3 Biometric Authentication Process

![](image/Pasted%20image%2020250108112958.png)

- Biometric data is highly sensitive and should be protected against any form of misuse 
- It should be kept local, i.e., inside the capturing device, and never transmitted to a remote server
- For local protection, biometric data should not be processed in the main operating system and stored in the main memory but in a specially protected Trusted Execution Environment (TEE)/Secure Enclave
- As many steps of the enrollment and authentication process as possible should be executed inside the TEE
- The biometric data derived from enrollment and authentication never leaves the TEE – rather all steps required for processing and matching happen inside the TEE

Enrollment
- Biometric raw data (fingerprint, face, iris, ...) is captured by the sensor and immediately sent to the TEE for processing (e.g., on Apple devices) 
- Inside the TEE (on some devices outside the TEE), a feature extraction algorithm identifies relevant features (e.g., minutiae point for fingerprinting)
- The identified features are encoded in a feature vector, which is compressed and encoded, resulting in a biometric template
- The biometric template is stored inside the TEE

Authentication
- Same steps as for enrollment
- The authentication template is compared with the biometric template by means of a _**similarity metric**_
    - _**Euclidean Distance**_: Measures the straight-line distance between two vectors in the feature space
    - _**Cosine Similarity**_: Measure the angle between the two vectors
- The enrollment template will be decrypted for comparison, or homomorphic encryption is applied, which allows a comparison on encrypted data
- Comparing the authentication with the enrollment template never achieves a 100% match because of inaccuracies of sensor technologies and different conditions during data capturing
- The _**biometric threshold**_ defines the degree of similarity required between the enrollment and the authentication template to grant access
- The threshold is a score that determines the accept/reject decision during authentication



## 6.4 DETERMINING THE BIOMETRIC THRESHOLD

![](image/Pasted%20image%2020250108113123.png)


_**Authentication performance metrics**_ describe the accuracy of the authentication process
- False Acceptance Rate (FAR)
    - Defined as the proportion of unauthorized users (or adversaries) accepted by the biometric system
    - A person not enrolled in the biometric system is given permission to access the system
    - FAR=number of successful authentications by unauthorized users / number of authentication attempts by unauthorized users
- False Rejection Rate (FRR)
    - Defined as the proportion of authorized users that are wrongly denied access by the biometric system
    - A person enrolled in vthe biometric system is not allowed to access the system
    - FRR=number of failed authentication attempts by authorized users  /  number of authentication attempts by authorized users

- If the biometric threshold is set too high (e.g., 95%), the FAR decreases but the FRR increases, which means the security strength increases at the expense of usability
- If the biometric threshold is too low (e.g., 40%), the FAR increases but the FRR decreases, which means that the usability increases at the expense of security



![](image/Pasted%20image%2020250108113315.png)

- To find an appropriate biometric threshold, the considered biometric data is split into test data and biometric templates
- The data from both sets is compared and matched based on different biometric thresholds, and the FAR and FRR values are recorded
- The obtained values are plotted on a graph to determine a suitable biometric threshold