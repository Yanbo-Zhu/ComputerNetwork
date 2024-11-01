
# 1 AIMD Additive Increase Multiplicative Decrease 

approach: senders can increase sending rate until packet loss (congestion) occurs, then decrease sending rate on loss event


![](image/Pasted%20image%2020241101201639.png)


Multiplicative decrease detail: sending rate is
- Cut in half on loss detected by triple duplicate ACK (TCP Reno)
- Cut to 1 MSS (maximum segment size) when loss detected by timeout (TCP Tahoe)

# 2 TCP CUBIC

![](image/Pasted%20image%2020241101201911.png)

increase W as a function of the cube of the distance between current time and K

![](image/Pasted%20image%2020241101202104.png)




