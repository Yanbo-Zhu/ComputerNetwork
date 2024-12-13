
goal: learn how to build client/server applications that communicate using sockets
socket: door between application process and end-end-transport protocol


![](image/Pasted%20image%2020241208220343.png)


![](../60_01_Intro/image/Pasted%20image%2020241021072237.png)

Two socket types for two transport services:
- UDP: unreliable datagram
- TCP: reliable, byte stream-oriented

Application Example:
1. client reads a line of characters (data) from its keyboard and sends data to server
2. server receives the data and converts characters to uppercase
3. server sends modified data to client
4. client receives modified data and displays line on its screen



# 1 Socket programming with UDP

UDP: no “connection” between client & server
- no handshaking before sending data
- sender explicitly attaches IP destination address and port # to each packet
- receiver extracts sender IP address and port# from received packet

UDP: transmitted data may be lost or received out-of-order

Application viewpoint: UDP provides unreliable transfer of groups of bytes (“datagrams”) between client and server

![](image/Pasted%20image%2020241208220513.png)


![](image/Pasted%20image%2020241208220534.png)


![](image/Pasted%20image%2020241208220540.png)




# 2 Socket programming with TCP

Client must contact server
 - server process must first be running
 - server must have created socket (door) that welcomes client’s contact

Client contacts server by:
- Creating TCP socket, specifying IP address, port number of server process
- When client creates socket: client TCP establishes connection to server TCP

when contacted by client, server TCP creates new socket for server process to communicate with that particular client
• allows server to talk with multiple clients
• source port numbers used to distinguish clients (more in Chap 3)

![](image/Pasted%20image%2020241208220654.png)


![](image/Pasted%20image%2020241208220702.png)


![](image/Pasted%20image%2020241208220709.png)

