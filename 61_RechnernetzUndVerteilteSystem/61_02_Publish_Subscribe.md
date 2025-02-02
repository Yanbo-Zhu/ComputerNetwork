

# 1 Motivation

- Traditional client/server communication model (using, e.g., RPC, message queue, shared memory, etc.)
    - Synchronous, tightly-coupled request invocations.
    - Very restrictive for distributed applications, especially for Wide Area Network (WAN) and mobile environments.
    - When nodes/links fail, system is affected.
    - Fault tolerance must be built in to support this.
- Require a more flexible and de-coupled communication style that offers anonymous and asynchronous mechanisms.

# 2 publish/subscribe system?

- Distributed pub/sub system is a communication paradigm that allows freedom in the distributed system by the decoupling of communication entities in terms of time, space and synchronization
- An event service system that is asynchronous, anonymous and loosely-coupled.
- Ability to quickly adapt in a dynamic environment.

Basic system model for publish/subscribe
Key components of pub/sub:
- Publishers: Publishers generate event data and publishes them.
- Subscribers: Subscribers submit their subscriptions and process the events received.
- Event Service: It’s the mediator/broker that filters and routes events from publishers to interested subscribers.

![](image/Pasted%20image%2020241120142526.png)




Example of pub/sub – Stock Quote Service
![](image/Pasted%20image%2020241120142543.png)



![](image/Pasted%20image%2020241120142635.png)


# 3 Interaction models

Interaction models in distributed systems can be classified according to
- who initiated the interaction
- how the communication partner is addressed
- Provider: provides data or functionality
- Anonymous Request/Reply: provider is selected by communication system and not specified directly (e.g., IP Anycast)


![](image/Pasted%20image%2020241120142646.png)


## 3.1 Concepts: Callbacks


Synchronous (remote) method calls often used to emulate behavior of event-based systems
- See also: Observer Design Pattern
- Frequently used in UI toolkits; example:
    - ![](image/Pasted%20image%2020241120142917.png)
- P&C coupled in space and time, decoupled in flow
- Producers have to take care of subscription management and error handling


## 3.2 Concepts: Message Queues

- Each message has only one consumer
- Receiver acknowledges successful processing of message
- No timing dependencies between sender and receiver
- Queue stores message (persistently), until
    - It is read by a consumer
    - The message expires (leases, TTL)


![](image/Pasted%20image%2020241120143118.png)

## 3.3 Concepts: Publish/Subscribe

Here: Topic-based Publish/Subscribe
- Interested parties can subscribe to a topic (channel)
- Applications post messages explicitly to specific topics
- Each message may have multiple receivers
- Full decoupling in space, time, and flow

![](image/Pasted%20image%2020241120143358.png)


# 4 Terms

- Event: Any happening in the real world or any kind of state change inside an information system that is observable
- Notification: The reification (ger. Verdinglichung) of an event as a data structure
- Message: Transport container for notifications and control messages


# 5 Classification


- Messaging domain
    - Point-to-point (producer à consumer)
    - Subscription-based pub/sub
    - Advertisement-based pub/sub
- Subscription mechanism
    - Channel-based (=topic-based) subscription
    - Content-based subscription
    - Subject-based subscription (limited form of content-based sub.)
- Server topology
    - Single server, hierarchical, acyclic peer-to-peer, generic peer-to-peer
- Event data model
    - Untyped vs. typed vs. object-oriented
- Event filters
    - Expressiveness and flexibility of subscription language
        - Simple expressions
        - SQL-like query language
    - Evaluated in event system (router/broker network)
- Note: scalability « expressiveness tradeoff
    - Simple expressions permit filter merging « better scalability

Features
n Scalability
n Security
n Client mobility
n Transparent vs. native vs. external
n Disconnection
n Quality of Service (QoS)
n Reliability & response time (real-time constraints)
n Transactions
n Exception handling


# 6 Addressing

## 6.1 Channel-based addressing (=topic-based)

- Interested parties can subscribe to a channel (analogous to mailing list)
- Application (producer) posts messages explicitly to a specific channel
- Channel Identifier is only part of message visible to event service
- There is no interplay between two different channels

![](image/Pasted%20image%2020241120143946.png)


- Extension: topic hierarchies (e.g., SwiftMQ)
    - `<roottopic>.<subtopic>.<subsubtopic>`
- Messages are published to addressed node and all subnodes
    - iit.sales -> iit.sales.US, iit.sales.EU
- Subscribing means receiving messages addressed to this node, all parent nodes and all sub nodes:
    - Subscription to iit.sales
    - Receives from: iit, iit.sales, iit.sales.US, iit.sales.EU
    - But not from: iit.projects
    - Subscriber receives each message only once


## 6.2 Subject-based addressing

- Limited form of content-based subscription
- Notifications contain a well-known attribute – the subject – that determines their address
- Subscriptions express interest in subjects by some form of expressions to be evaluated against the subject
- Subject is
    - List of strings (e.g., TIB/Rendezvous, JEDI)
    - Properties: typed key/value-pairs (e.g., Java Messaging Service (JMS))
- Subject (= header of notification) is visible to event service, remaining information is opaque
- Subscription is
    - (Limited form of) regular expressions over strings (TIB, JEDI) or subset of SQL92 queries (JMS)
- Filtering is done in the event system (router/broker network)!


## 6.3 Content-based addressing

- Domain of filters extended to the whole content of notification (i.e., payload)
- More freedom in encoding data upon which filters can be applied
- More information for event service to set up routing information

![](image/Pasted%20image%2020241120144621.png)


![](image/Pasted%20image%2020241120144645.png)

Example 
![](image/Pasted%20image%2020241120144841.png)
## 6.4 Type-based addressing

- Similar to channel-based pub/sub with hierarchies
- Supports subtype tests (instanceof)
- Good integration of middleware & language, type safety

![](image/Pasted%20image%2020241120144743.png)




# 7 Topic vs. Content-based Subscription

Topic-based
- Recipients (consumer) are known a-priori
- Many efficient implementations exist
- Limited expressiveness

Content-based
- Cannot determine recipients before publication
- More flexible
- More general
- Much more difficult to implement efficiently


# 8 Subscription Mechanisms

![](image/Pasted%20image%2020241120145008.png)



# 9 Distributed Event Systems

![](image/Pasted%20image%2020241120145030.png)

![](image/Pasted%20image%2020241120145024.png)


# 10 







