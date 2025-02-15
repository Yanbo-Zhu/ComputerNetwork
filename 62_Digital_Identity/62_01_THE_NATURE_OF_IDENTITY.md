
# 1 WHAT IS IDENTITY

A digital identity contains data that uniquely describes a person or thing but also contains information about the subject's relationship to other entities."

## 1.1 BUNDLE VS SUBSTANCE THEORY
- Bundle theory posits 假设 that an object is nothing more than a collection (bundle) of its properties
- Substance theory asserts that an object is composed of a substance  物质 that underlies and supports their properties

![](image/Pasted%20image%2020250215122713.png)

---

Simple Classification Theme

![](image/Pasted%20image%2020250215123055.png)

![](image/Pasted%20image%2020250215123227.png)

- _**Attributes**_ describe information about an entity, specifically of characteristics that are acquired
- _**Preferences**_ represent desires and defaults
- Traits 本质特征: Like attributes, _**traits**_ are features of the entity, but they are inherent rather than acquired
    - Unlike attributes, traits change slowly, if at all

# 2 TIERS OF IDENTITY


![](image/Pasted%20image%2020241031142659.png)

- Andre Durand introduced the concept of _**tiers of identity**_ in 2002
- **Tier 1**
    - Consists of traits associated with the entity that are both timeless and unconditional
    - Examples: name, date of birth, ...
- **Tier 2**
    - Attributes assigned to an entity by others
    - Used to identify the individual but are issued temporarily based on some kind of relationship
    - Examples: email addresses, driver's license, credit card,...
- **Tier 3**
    - Group identities that identify an entity abstractly
    - Examples: German, white male over 50, frequent flier,...


# 3 Trust and Risk 

![](image/Pasted%20image%2020250215131047.png)


Trust
- Relationships in digital identity systems are based on trust between users, service providers, systems, and other entities
- _**Trust**_ refers to the **belief** or **expectation** that an entity will behave reliably, securely, and in a manner that protects the interest of all entities
- ​Trust depends on personal **vulnerability** and **risk-taking**
- The _**risk**_ is that the trustee is an independent agent who may not perform to the trustor's expectations


Risks
- The trustee may mishandle or intentionally disclose personal information of the trustor to unauthorized parties (_**Breach of Confidentiality**_)
- The trustee is different from who they claim to be (_**Misrepresentation of Identity**_)
- The trustee may exploit the trust for malicious purposes (_**Abuse of trust**_)
- The trustee may fail to uphold its end of the agreement (_**Failure to Meet Expectations**_)
- The trustee may not be transparent about their processes, intentions, or security practices, leaving the trustor unaware of potential vulnerabilities (_**Lack of Transparency**_)
- The trustee's system or practices may be compromised by hackers or other malicious actors (_**Compromised Systems**_)
- Relying too heavily on a single trustee creates a single point of failure (_**Dependency Risk**_)


Confidence (risk 的反义词  可信度 )
- _**Confidence**_ refers to the **degree of assurance** or **certainty** that the system or entity will perform its intended function securely and accurately
- ​Confidence depends on internalized expectations derived from knowledge or past experiences
- Confidence focuses on assurance, not risk


Measures of Confidence
- Mandating that trustees comply with strict security requirements, undergo audits, and adhere to qualified trust service standards
- ​Usage of digital certificates by trustees to enable authenticity, confidentiality, and integrity
- ​Inclusion of trust anchors in the relationship between trustee and trustor


Trust anchor
- Trust anchors are fundamental intermediaries that serve as trusted entities or points of reference upon which entities base their trust
- ​They act as reliable authorities that are implicitly trusted by the participants in the system
- ​Trust anchors primarily contribute to confidence by offering verifiable, objective assurance through cryptographic methods and standard compliance
- Trust anchors also contribute to trust through their reputation and by ensuring consistency and reliability



# 4 An Identity Management (IdM)

An Identity Management (IdM) System is a set of services that support the creation, modifi cation, and removal of identities and associated accounts, as well as the authentication and authorization required to access resources."


An Identity Management (IdM) System is a set of services that support the creation, modification, and removal of identities and associated accounts, as well as the authentication and authorization required to access resources."


## 4.1 IDM Functions


Authentication (verify who you are)
is a process to verify the real identity of an entity, which can be an individual, an organization, a thing, a computer program, or a website

- _**Authentication**_ is a process to verify the real identity of an entity, which can be an individual, an organization, a thing, a computer program, or a website
- For transmission, authentication verifies that the sender of the message is the entity it claims to be
- ​When the proof is bidirectional, it is referred to as _**mutual authentication**_
- An _**authentication factor**_ is a piece of information or a method used to verify an entity's identity when they try to access a system or service

- _**Two-factor**_ or _**multi-factor authentication**_ is given if a system or services requires two or more authentication factors
- Groups of authentication factors
    - Something You Know (Knowledge Factor)                
    - Something You Have (Possession Factor)
    - Something You Are (Inherence Factor)
    - Somewhere You Are (Location Factor)
    - Something You Do (Behavioral Factor


---

Identification (tell 一个系同 这个 entity 有什么身份)
is the process of claiming an identity – essentially telling a system or service "who you are"

- _**Identification**_ is the process of claiming an identity – essentially telling a system or service "who you are"
- Involves a unique identifier such as a username, email address, ID number, or digital certificate
- Helps the system understand who is trying to access the system
- Identification is about claiming an identity, while authentication is about proving that the claimed identity is valid
- Identification asks "Who are you?", authentication ask "Can you prove it?"


----

Availability
Availability ensures that information and systems are accessible to authorized users when they need it

- _**Availability**_ ensures that information and systems are accessible to authorized users when they need it
- Focus is on keeping systems up and running and preventing downtime or service interruptions
- Goal is to guarantee that users have timely and reliable access to information and resources

---

Authorization ( 不是赋予权利, 而是 determine what you are allowed to do )
is the process of determining what an authenticated entity is allowed to access and what operations it is allowed to perform

- _**Authorization**_ is the process of determining what an authenticated entity is allowed to access and what operations it is allowed to perform
- Authorization of an entity occurs after authentication
- Typical authorization policies permit access to different resources for different collections of authenticated entities on the basis of roles, groups, or privileges
- _**Roles**_ represent competencies, authorities, responsibilities, or specific duty assignments, while _**groups**_ are formed on the basis of organizational affiliation, e.g., division, department, laboratory, etc.
- Authorization policies may also restrict access based on a global context (e.g., time of day) or transactional context (e.g., no more than a certain number or amount of withdrawals per day)


---

Assertion 
放话说这个系统已经被验证了, 这个放话的过程就是 asseration
declaring that xx has been authenticated 

is a process of a trusted entity declaring or affirming that an individual or system has been authenticated or possesses certain attributes

- _**Assertion**_ is a process of a trusted entity declaring or affirming that an individual or system has been authenticated or possesses certain attributes
- The assertion is given in form of tokens, certificates, or (verifiable) credentials
- Assertion can also convey specific attributes of an entity, such as their role, age, or membership status, without revealing the full identity
- _**Issuing**_ refers to the process by which a trusted entity generates an assertion about an entity
- _**Verifying**_ refers to the process by which the receiving system checks the assertion


---

Auditing (logging entity 被记录的过程)
When an entity accesses a website or queries a database, various pieces of information are recorded or logged, which is called auditing

- When an entity accesses a website or queries a database, various pieces of information are recorded or logged, which is called _**auditing**_
- Audits provide the means to reconstruct what specific actions have occurred and may help security investigators to identify the entity that performed unauthorized actions
- Examples of recorded events are failed login attempts or denied requests to use a resource that may indicate attempts to violate a system's security


---

Confidentiality  机密性
是一种能力 , 确保信息只被授权的人使用 
refers to the ability to ensure that messages and data are available only to those who are authorized to view them

- _**Confidentiality**_ refers to the ability to ensure that messages and data are available only to those who are authorized to view them
- It is achieved by making sure that the connection between the involved parties cannot be intercepted
- For that purpose, data or a transmitted messages is encrypted when sent over an untrusted network so that it is readable only by the entity for whom it is intended

---


Integrity 完整性 
不被修改和盗用 , 通过数字签名保证
Data/message integrity is usually guaranteed by the use of digital signatures
Integrity is the assurance that data or messages are accurate and have not been altered or compromised

- _**Integrity**_ is the assurance that data or messages are accurate and have not been altered or compromised
- It means that data or messages have not been modified without authorization; a message that was sent is the same message that was received
- The integrity function detects and prevents unauthorized creation, modification, or deletion of data or messages
- Data/message integrity is usually guaranteed by the use of _**digital signatures**_



## 4.2 NETWORK TOPOLOGIES

![](image/Pasted%20image%2020250215123738.png)



Centralized Systems
- All nodes are connected to a single, central node
- Central node is responsible for all processing, coordination, and decision making
- **Advantages**
    - Easy to manage and to maintain
    - Efficient coordination
    - Clear chain of command
- **Disadvantages**
    - Single point of failure
    - Central node can become a bottleneck
    - Vulnerable to attacks

Decentralized Systems
- Multiple, central nodes (_**hubs**_), each of which controls a part of the entire systems
- Power is distributed across several hubs, and each hub handles local processing and decision-making
- **Advantages**
    - Improved resilience
    - Load distribution, i.e., hubs can process task in parallel
    - Better scalability than centralized systems
- **Disadvantages**
    - Increased complexity as managing coordination between hubs is more complicated
    - Partial failure guarantees partial operability after a node failure but overall performance of the entire system degrades


Distributed Systems
- No central authority or hub, all nodes are interconnected
- Each node is equally responsible for processing and decision making
- **Advantages**
    - High resilience as no single point of failure
    - High scalability as the system can easily grow by adding more nodes
    - Good load balancing
- **Disadvantages**
    - High complexity for coordinating and achieving consistency across all nodes
    - Coordination between nodes that are interconnected my several hops may result in high latencies
    - More signaling overhead is required between the nodes


---


![](image/Pasted%20image%2020250215124206.png)

- A suitable approach for classifying identity systems are their properties location, control, and topology:
- **Location**
    - Determines whether the components are _**colocated**_ or _**distributed**_
    - Different levels of abstraction, for example, computing centers, physical computers, virtual instances
- **Control**
    - Determines whether the components are _**centralized**_ or _**decentralized**_
    - A system is centralized if under control of a single entity and decentralized if nodes are controlled by different stakeholders
- **Topology**
    - Defines the relationships between nodes
    - In _**hierarchical**_ systems, nodes have superior-inferior relationships, in _**heterarchical**_ systems, nodes have peer-to-peer relationships


# 5 IDM Paradigms


![](image/Pasted%20image%2020250215124452.png)


![](image/Pasted%20image%2020241031143835.png)



# 6 Locus of Control

![](image/Pasted%20image%2020250215124828.png)


The _**locus of control**_ defines who or what controls a digital identity in an IDM system and includes the following factors:
- Who initiates the relationship?
- Who owns the identifier (and who can take it away)?
- Who sets the rules governing interactions?
- Who determines how attributes are shared?


- **Administrative IDM** (silod identity)
    - Built and operated for the purpose of an organization
    - Organization determines the rules of operation, what attributes are allowed, how they are used, and whether and where they can be shared
- **User-centered IDM** (federated identity)
    - Ability to federate an account from one service to another (e.g., by using OpenID or OAuth)
    - The identity holder chooses what account to use and is redirected from the relying party to the identity provider to approve the account sharing
- **Self-sovereign IDM** 
    - An SSI system give entities complete control over what attributes they share in response to queries about them
        -  自主控制 what attribute they share
    - Attributes are stored in wallets, not accounts
    - An entity's identifier is self-issued and cannot be deleted by others


