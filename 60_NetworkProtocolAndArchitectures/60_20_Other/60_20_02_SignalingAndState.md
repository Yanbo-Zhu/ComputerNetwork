
# 1 Separation of Control and Data

- Public switched telephone network
    - Packets switched control network
    - Separate circuit switched call lines
    - Earlier tone based (in band signaling)
- Enterprise Networks:
    - Control to Management Interface of Ethernet Switches
    - Out of Band Control, e.g., dedicated Ethernet port
    - In Band via dedicated VLAN for Management
    - WiFi APs usually connected through a single Ethernet Uplink
        - usually requires In Band Control
- Internet:
    - HTTP: Messages mixes signaling and data: “In Band Signaling”
    - FTP: Dedicated control connection: “Out of Band Signaling”


In Band Signaling 就是 control data 和 message data 会在一起
Out of Band Signaling 就是 control data 在单独的Dedicated control connection 中运输 

## 1.1 HTTP: In Band Signaling

![](image/Pasted%20image%2020250108135753.png)

1. HTTP client initiates TCP connection to HTTP server (process) at tu berlin.de on port 80.
2. HTTP server at host tu berlin.de waiting for TCP connection at port 80. Accepts connection, notifies client.
3. HTTP client sends HTTP request message (containing URL) into TCP connection socket. Message indicates that client wants object menue /home
4. HTTP server receives request message, forms response message containing requested object, and sends message into its socket.
5. HTTP server closes TCP connection.
6. HTTP client receives response message containing html file, displays html. Parsing html file, finds 10 referenced jpeg objects.
7. Steps 1-5 repeated for each of 10 jpeg objects.

---

HTTP Request
Request message typically just a signaling message 
- Contains no data
![](image/Pasted%20image%2020250108135942.png)





HTTP Response
- Response message mixes signaling and data
- Request and response exchanged over single TCP connection

![](image/Pasted%20image%2020250108140104.png)

## 1.2 FTP: Out of Band Signaling

- FTP: “File Transfer Protocol”
    1. FTP client contacts FTP server at port 21
    2. Client obtains authorization over control connection
    3. Client browses remote directory via commands sent over control connection
    4. When server receives file transfer commend server ==opens new TCP data connection to client==
    5. After transferring one file, server closes connection
    6. Server opens 2nd TCP data connection to transfer another file
- Separates control and data connections
- Control connection: Out of band signaling
- FTP server maintains state
    - Current directory
    - Earlier authentication

## 1.3 Why Separate Control Channel?

Why?
- Allows concurrent control + data
- Allows to perform authentication at control level
- Simplifies processing of data/control streams
    - Higher throughput
- Provide QoS appropriate for control/data streams

Why not?
- Separate channels complicate management
- Increases resource requirements
- Can increase latency
- Example: Two TCP connections instead of one in case of HTTP


# 2 Maintaining State

State
- Information stored in nodes by network protocols
- Updated when conditions change
- Stored in multiple nodes (simple example: 2 nodes)
- Often associated with end system generated call or session

Examples: TCP
- Sequence numbers, timer values, RTT estimates

Definition Sender
- Node that generates messages to install state at different node
- Also keeps track of own state

Definition Receiver
- Node that creates, maintains, removes local state
- Based on signaling messages received from sender

## 2.1 Hard-State (HS) and Soft-State(SS)


Hard-State
- State installed at receiver: After receiving a setup message from sender
- State removed at receiver: After receiving a teardown message from sender
- Default assumption: State valid unless told otherwise

In practice
- Failsafe mechanisms
- To remove orphaned/inconsistent state
- E.g. receiver to sender heartbeat ””: Is this state still

---

Soft State
- State installed by receiver
    - On receipt of setup (trigger/install) msg from sender
    - Sender also sends periodic refresh msg (“Keep alive)
    - Indicates receiver to continue to maintain state
- State removed by receiver
    - Via timeout
    - Due to absence of refresh msg from sender
- Default assumption
    - State becomes invalid unless refreshed
- In practice
    - Explicit state removal (teardown) msgs also used
- Examples: IGMP

# 3 Example: State in TCP

TCP has aspects of both principles, soft-state or hard-state
Hard state
- Does explicit connection teardown
- “Fin” messages
Soft state
- Connections will be removed after a timeout
- One timer per connection
- TCP keep alive is optional


## 3.1 Hard-State Signaling
- Reliable signaling
- State removal by request
- Requires additional error handling
    - E.g. Sender failure

![](image/Pasted%20image%2020250108144929.png)


## 3.2 State Signaling

State installed by receiver: Setup message
State needs to be updated: Refresh/Update message
State removed by receiver: Via timeout
Default assumption: State becomes invalid unless refreshed


- Best effort signaling
- Refresh timer causing periodic refresh
    - Sends up to date signaling information
- State time out timer
    - State removal only by time out

![](image/Pasted%20image%2020250108145217.png)


---

Soft-State with Explicit Removal
- Soft state with additional state removal messages
    - Sender send teardown message
    - No ACK from receiver
    - If message gets lost: State removal at receiver due to missing refresh messages
- Also uses install messages and state timeout

![](image/Pasted%20image%2020250108145421.png)


---

Soft-State with Reliable Install
- Install messages are transmitted reliably
    - Sender starts a transmission timeout
    - Receiver sends an ACK on receiving the install message
    - If no ACK, but transmission timeout expire
        - Resend install message
- Receiver informs sender when state is removed due to missing refresh
![](image/Pasted%20image%2020250108145727.png)

---
Soft-State with Reliable Install/Removal
- ACK from receiver on state install and state removal
- Transmission timeout at sender

![](image/Pasted%20image%2020250108145851.png)

---

Soft-State: Refreshes
- Refresh messages serve many purposes
    - Trigger/install: First time state installation
    - Refresh: Refresh state known to exist (“I am still here”)
    - Lack of refresh: Remove state (“ I am gone”)
- Challenge: All refresh messages unreliable
    - Messages can get lost, state will be inconsistent for some time
    - Would like triggers/installs to result in state installation immediately
- Enhancement
    - Add receiver to sender refresh ACK for triggers

# 4 Signaling Spectrum

![](image/Pasted%20image%2020250108150038.png)

# 5 How do we Model/Evaluate?

- Metrics
    - Inconsistency ratio: Fraction time participating nodes disagree
    - Signaling overhead: Average number of messages during session lifetime
- Measures of robustness?
    - Convergence time
- Complexity?

# 6 Simple State Model
![](image/Pasted%20image%2020250108150559.png)

- Two parties: Sender and receiver
    - Separated by a link with …
        - Signaling delay D
        - Loss probability 𝑝
    - Each keeps track of a single state variable
- State updates
    - Update arrivals Poisson distributed
    - Interarrival time of update messages are independent from each another
    - Updates arrive at a rate of 𝜆
- Timers
    - Exponentially distributed
    - Refresh timer value: 𝑇
    - State timer value: 𝑋
- Can be expressed as Markov model

# 7 Model for Soft State

- So we have states and transitions between states
    - Transitions depend on a transition probability
- How do we model a soft state protocol?
    - States?
    - Transitions?
    - Lost messages?
    - Inconsistent states?

![](image/Pasted%20image%2020250108150634.png)

![](image/Pasted%20image%2020250108150658.png)

![](image/Pasted%20image%2020250108150747.png)

![](image/Pasted%20image%2020250108150826.png)

![](image/Pasted%20image%2020250108150935.png)

![](image/Pasted%20image%2020250108150945.png)

# 8 Soft-State: Performance Metrics

Inconsistency ratio
- 𝛿=1−𝜋 , 𝜋 are probabilities for states in sync
- Fraction of time that sender and receiver do not have consistent state values

Signaling overhead
- Γ=1−𝜆+1𝑇𝜇
- Total number of messages required during the lifetime of a session
- Session: Time between state initiation and removal


- SS+ER, SS+RT, SS+RTR require extensions
    - State: Additional orphaned state to enable explicit state removal at the receiver
    - Additional or less timers
    - Additional or different transitions between states
- Transition diagram does not cover false state removals at receiver side
- Parameter settings
    - Mean lifetime 30 min.
    - Refresh timer, T = 5sec
    - State timer, X = 15 sec
    - Update rate 1/20sec
    - Signal loss rate 2%

# 9 Impact of State Lifetime at Sender

Inconsistency ratio 
![](image/Pasted%20image%2020250108151341.png)
- Inconsistency decrease as state life time increases
- Explicit removal improves consistency …

Average signaling message rate 
![](image/Pasted%20image%2020250108151324.png)

# 10 Soft-State: Setting Timer Values

- What’s a good value for refresh/timeout timers?
    - State timeout should be longer than refresh timeout
    - In case a refresh message gets lost
        - ![](image/Pasted%20image%2020250108151538.png)
    - What value of 𝑛 to choose?
- Impact
    - Determines amount of signaling traffic
    - Determines responsiveness to change

- Short timers
    - Fast response to changes
    - More signaling
- Long timers
    - Slow response to changes
    - Less signaling
- Appropriate timer values dictated by …
    - Consequence of slow/fast response
    - Message loss probability

![](image/Pasted%20image%2020250108151651.png)



