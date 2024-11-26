
# 1 TCP Traffic Analysis
## 1.1 a
(a) In Wireshark, start a traffic capture on your local network interface.
Load the web page https://www.tu.berlin/.    141.23.73.70  (通过ping得到 )
Wait for 10 seconds, then stop the capture.
How many packets do you get in total?
How many of them contain TCP? (Hint: Use a display filter.)   display filter: tcp 
How many of them contain HTTP/1.1? (Hint: Use a display filter.)   `http && http.request.version == "HTTP/1.1"`
Include the display filters you use in your solution.


141.23.73.70 在这里被 resolve 成了 stats.tu-berlin.de
![](image/Pasted%20image%2020241120201518.png)

可以看出来 tcp connection setup (3 handshake) 的节奏
- 我的笔记本 给 141.23.73.70 发一个 request with flag SYN (SYN 的 bit 位置 值为1), SYN 代表要开始建立 tcp connection 了
- 141.23.73.70 给 我的笔记本 发一个 repsone with flag SYN, ACK , 是对上面的 syn 的 request 的回应 
- 我的笔记本 给 141.23.73.70 发一个 request with flag ACK=1 , 表示 第三次握手 



## 1.2 b 
(b) Now set a display filter to show only packets that correspond to a single TCP connection of your web page request and response.
Include the display filter you use in your solution and briefly explain what it means.

查找某个具体的 tcp single connection 的方式 
方法1
tcp.stream eq X (using the TCP stream number internally assigned by wireshark)
通过 wireshark 找到第一个 entry 对应的 tcp connection 的 stream 为 19 

![](image/Pasted%20image%2020241120202145.png)

使用它作为 filter 
![](image/Pasted%20image%2020241120202250.png)

方法2
tcp.port == Y , using client source port  , the client uses a different port number for every connection 

通过报文内容 找到 source port ist 54095 
![](image/Pasted%20image%2020241120202439.png)

## 1.3 c
(c) Analyze the obtained data by marking packets belonging to:
![](image/Pasted%20image%2020241120210103.png)

• the TCP connection setup
TCP SYN, SYN/ACK, ACK for connection setup 
tcp.flags.syn == 1 && tcp.flags.ack == 1

• the transmission of the HTTP request,
GET /pic-10kb-4.html
http.request.method == "GET"
![](image/Pasted%20image%2020241120210218.png)

• the transmission of the HTTP response
http.response.code == 200
![](image/Pasted%20image%2020241120210251.png)

• the tear-down of the connection.
TCP FIN, FIN/ACK, ACK for connection setup 
tcp.flags.fin == 1 && tcp.flags.ack == 1

![](image/Pasted%20image%2020241120210415.png)
# 2 DNS

1
Start a new traffic capture and load the page https://www.tu.berlin/.

2
Set a display filter for DNS, and find the queries for tu.berlin. 

208.67.222.222 为 dns server which serves for tu.berlin domain 

dns.qry.name matches "tu.berlin"
![](image/Pasted%20image%2020241120203807.png)


dns.a == 141.23.73.70
在 dns respone 中 包含一个 a record with value 141.23.73.70
![](image/Pasted%20image%2020241120202907.png)
![](image/Pasted%20image%2020241120203856.png)

3
What records does the browser ask for, and what replies does it get?

可以看到 cname record 和 a record 
![](image/Pasted%20image%2020241120204203.png)



4
Click on one of the DNS replies and find the IP address in the Answers section. Remove the DNS display filter again, and look at the packets coming afterwards to and from one of the IP addresses associated with https://www.tu.berlin/. (Hint: You can use a display filter.)
 
What TCP connections do you see? What can you see in them, and what can you not see?
Why?
(No need for a screenshot here, if you can explain it in text.)

Answer: you can see the answer as HTTP/1.1 301 Method Moved, which says that the document has moved to https://inet.tu-berlin.de 
Then you can see a new TCP connection being established and a TLS handshake
The rest of the communication is encrypted , so you can not see the content of the web page 


# 3 DNS 例子2


In dieser Aufgabe versuchen wir nachzuvollziehen wie ein Aufruf von YouTube abläuft und wie das verwendete CDN arbeitet. Rufen Sie dazu ein YouTube Video auf, während Sie den Netzwerktraffic mit Wireshark mitschneiden.

Hinweis: Durch gecachten Inhalt kann es sein, dass sie nicht die volle Anfrage sehen, falls Sie die Website bereits besucht haben. Um dies zu vermeiden starten Sie gegebenenfalls das System vor dem Test neu. Eure Teampartner erhalten unter Umständen andere Ergebnisse, vergleicht diese.


1. Welche DNS Anfragen werden ausgelöst? Welcher der Hosts liefert vermutlich die eigentliche Website aus? Welcher Host stellt die Videodaten bereit?
2. Wieso werden mehr Anfragen als nur youtube.com gestellt?
3. Welche CDN Mechanismen werden verwendet?
4. In der Vorlesung wurde besprochen, dass große Datenmengen (wie z.B. Videos) möglichst nah beim Nutzer gespeichert werden um Netzwerkkapazität zu sparen. Versuchen Sie herauszufinden, wo sich der Server befindet, der diese Daten ausliefert. 2 Überprüfen Sie ob dieser Standort plausibel ist, beispielsweise indem Sie die Latenz via ping testen.







