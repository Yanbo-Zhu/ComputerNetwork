
![](image/Pasted%20image%2020241208182554.png)

# 1 理论 

QUIC 就是给 UDP 加入 error and congestion control 和 connection establishment 

application-layer protocol, on top of UDP
- increase performance of HTTP
- deployed on many Google servers, apps (Chrome, mobile YouTube app)

![](image/Pasted%20image%2020241208182624.png)


- adopts approaches we’ve studied in this chapter for connection establishment, error control, congestion control
    - error and congestion control: “Readers familiar with TCP’s loss detection and congestion control will find algorithms here that parallel well-known TCP ones.” 
    - connection establishment: reliability, congestion control, authentication, encryption, state established in one RTT
- multiple application-level “streams” multiplexed over single QUIC connection
    - separate reliable data transfer, security
    - common congestion control


![](image/Pasted%20image%2020241208182905.png)


![](image/Pasted%20image%2020241208182914.png)
