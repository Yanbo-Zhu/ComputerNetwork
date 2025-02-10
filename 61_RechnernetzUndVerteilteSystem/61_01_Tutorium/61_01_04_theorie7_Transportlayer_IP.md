
# 1 Fluss- und Überlastkontrolle*

Beantworten Sie die folgenden Fragen im Kontext von Transportprotokollen:
1. Was ist der Unterschied zwischen Flusskontrolle und Überlastkontrolle?
2. Über welchen Mechanismus wird Flusskontrolle in TCP implementiert?
3. Über welchen Mechanismus wird Überlastkontrolle in TCP implementiert?
4. Inwiefern hängen die beiden Aspekte zusammen?


# 2 NAT*

Angenommen Sie greifen auf das Internet über einen Router zu. Beantworten Sie die folgenden Fragen:
5. Sie schalten Ihren Rechner an und erhalten automatisch eine IP-Adresse. Die dafür benutzte Technologie nennt sich DHCP. Wie sieht die initiale Anfrage nach einer Adresse aus?
6. Sie haben eine Adresse zugewiesen bekommen. Wie lange ist diese gültig?
7. Was macht Ihr Rechner, damit die Adresse länger gültig bleibt? Wann tut er dies?
8. Ihr Router stellt das Internet über NAT bereit. Was bedeutet das?


---
new Server  <-> Router 
- 0.0.0.0 0-> 255.255.255.255: new Server  告诉 router 新加入一个 设备 
-  127.65.5.1  <- DHCP offen   <- 127.65.0.1.  : router 告诉 server  127.65.5.1 你可以用 
- 127.65.5.1 > DHCP request -> 127.65.0.1   : 这个 server 向 router 申请 127.65.5.1 这个地址 
- 127.65.5.1 <- DHCP Acknowledge  <- 127.65.0.1  : router 项 server 发送 ACK, 确认它能够使用这个地址 


---

Sie haben eine Adresse zugewiesen bekommen. Wie lange ist diese gültig?
Lease Time:  (  server 在 router 中被储存, ip addresee gueltige Zeit )

---

 Was macht Ihr Rechner, damit die Adresse länger gültig bleibt? Wann tut er dies?

ip addresee gultige zeit 一直保存下去 
nach 50% lease time, client schicket router ein DHCP request time 


---

Ihr Router stellt das Internet über NAT bereit. Was bedeutet das?
ISP -> ip addresse of isp (127.0.0.1)  -> Router  -> A, B, C  client
Subnetmask  使用在 127.0.0.1 上面 :  IP-addresse (of a or b  or c ) & subnetzmask  = global addresse of ISP. 



# 3 IPv4*
Beantworten Sie im Kontext von IP die folgenden Fragen:
2. Was sind Class A, B und C Netze? Welcher Teil der Adresse gehört jeweils zu Host bzw. Netzwerk?
3. Überprüfen Sie, ob die IPv4-Adresse 149.77.115.54 im Netzwerk 149.77.112.0 mit der Netzwerkmaske 255.255.252.0 liegt.
4. Nennen Sie zwei Ansätze, mit dem Problem der knappen Internet-Adressen umzugehen.
5. Wie hilft das heute verwendete Classless Inter-Domain Routing (CIDR), das Problem zu lösen?

----
Was sind Class A, B und C Netze? Welcher Teil der Adresse gehört jeweils zu Host bzw. Netzwerk?

(netzwerk bit platzerhalt)/(host bit platzerhalt)   一共 32 位

Class A:  netzwerk hat 8 bit 
Class b:  netzwerk hat 16 bit 

---

Überprüfen Sie, ob die IPv4-Adresse 149.77.115.54 im Netzwerk 149.77.112.0 mit der Netzwerkmaske 255.255.252.0 liegt: ja 

 IPv4-Adresse  149.77.115.54 =   10010101.01001101.01110011.00110110
Netzwerkmaske 255.255.252.0 = 11111111.11111111.11111100.00000000
Netzwerk 149.77.112.0 =             10010101.01001101.01110000.00000000

----

Nennen Sie zwei Ansätze, mit dem Problem der knappen Internet-Adressen umzugehen.
1 NAT
2 IPv6

---
Wie hilft das heute verwendete Classless Inter-Domain Routing (CIDR), das Problem zu lösen?
Netzwerk 149.77.112.0 =             10010101.01001101.01110000.00000000
-> 149.77.112.0/22 , noch 10 bit frei   就是这个netwerk 下 有 2^10  个可用的 ip addresse 


# 4 Full-Stack*

Sie haben in der Vorlesung wichtige Komponenten, aus denen das Internet zusammengesetzt ist, kennengelernt. 
Beantworten Sie in diesem Kontext die folgenden Fragen und machen Sie sich klar, wo die Technologien jeweils verwendet werden:
6. Grenzen Sie die folgenden Begriffe gegeneinander ab. Auf welcher Ebene des ISO/OSI Modells arbeiten die jeweiligen Elemente?
    1. Repeater
    2. Hub
    3. Bridge
    4. Switch
    5. Router
7. Was ist eine IP-Adresse, was eine MAC-Adresse?
8. Welches sind die speziellen IPv4-Adressen für Loopback und (limited) Broadcast?
9. Wie funktioniert ARP?
10. Nennen und erklären Sie die 4 Arten von Delay in einem Netz mit Paket-Vermittlung.
11. Was ist ein Autonomes System (AS)?
12. Was ist der Unterschied zwischen Peering und Transit?
13. Was ist der Unterschied zwischen Routing und Forwarding?


---

- Network layer: ip addresse 
    - Router:
        - ip-addresse-> Router -> A  (192.0.0.1) , B, C
        - A, B, C 处于同一个 Autonomous system (locale Netz) 中 , 他们有一个  global addresse (172.0.0.1) 
        - Router 有一个 Routing tabelle.   通过 ARP  将 收到的信息 中的 ziel-ipaddresse  翻译成 mac addresse, 然后 从送到 192.0.0.1 对应的 A 上面 
        - _ARP_（Address Resolution Protocol，地址解析协议）是用来将IP地址解析为MAC地址的协议。主机或三层网络设备上会维护一张_ARP_表，用于存储IP地址和MAC地址的映射关系
        - Router 通过 mac addresse 去定位 target server  ,  ip-addresse , 所以用到 arp 
        - router 内部有 cache 用来储存  两个 server 之间的 verbindung 的关系 
- Link Layer:  MAC addresse
    - Bridge: A -> Bridge -> B.     Bridge kann einzige  socket haben .   kann nur unicast machen 
        - A und B muss innerhalb autonomous system (dassalbe lokale Netz ) befinden.  Bridge 才能工作 .  通过Bridge传播的话 必须有对方的 mac-addressen 
    - Switcher 能够判断 发给 那个 end-server , 区别于 HUB
        -  A -> Switcher -> B, C, D   . 能同时发给某个个 server, switcher kann multiple socket haben 
        - 通过Switcher传播的话 必须有对方的 mac-addressen 
- Physicales Layer
    - 0,1 bit werte -> transportiert 
    - Repeater 就是信号增强器 
    - Hub: 
        - zwischen Gerate unterscheiden:   A -> Hub -> B, C, D .  
        - Hub 的定义就是 hub 无发区别 bcd 三个 end-server 的区别. 他会采取 boradcast 的方式 发给所有的 server . Switcher 不能 unicast 传播 
        - 采用的 addresse 是 255.255.255.255 就是发给所有的 

![](image/Pasted%20image%2020250210220224.png)


---

DHCP  
- MAC-addresse 是statish
- IP-addresse ist dynamische , 是通过 DHCP 分配的 
    - hierarchical aufgebaut 

---
Welches sind die speziellen IPv4-Adressen für Loopback und (limited) Broadcast?
- Loopback:  127.0.0.1  , 也是  localhost
- Boardcast: 255.255.255.255, schicken an alle server 
- 0.0.0.0  , 如果 一个 server 还没有被分配 ip addresse, 他就会 是 0.0.0.0     . boardcost 的时候 也会发给 0.0.0.0 的 

---

Nennen und erklären Sie die 4 Arten von Delay in einem Netz mit Paket-Vermittlung.
- Processing Delay: Zeit benoetigt Feldern zu erkenen (Header 中的信息 分析, 验证错误,  ziel-addresse 分析)  und das Paket an Zielsystem zu schicken  
- Queuing Delay: Zeit , die das packet im Puffern beleibt bis das Packet geschickt werden aknn 
- Transmission Delay:   Packetlaenger/ Bandbertre .  Die Zeit benotigen , um ein Packet zu schicken 
- Propagation Delay: laenge des Kanales /  Senderate . Die Zeit 

----

Was ist ein Autonomes System (AS)?
ein ISP hat meherer AS 

![](image/Pasted%20image%2020250210220500.png)

----
Was ist der Unterschied zwischen Peering und Transit?

- **Peering** ist ideal für große Netzwerke oder Content-Anbieter, die viel Traffic haben und sich direkt mit anderen großen Netzwerken verbinden möchten.
- **Transit** ist notwendig für Netzwerke, die vollen Internet-Zugang benötigen und sich nicht ausschließlich auf Peering verlassen können.

1 Peering
两个 属于不同 ISP 的 AS, direkt verbindne 
Peering ist eine direkte Verbindung zwischen zwei Netzwerken (Autonome Systeme, AS), um Daten direkt auszutauschen, ohne einen dritten Anbieter (ISP) zu nutzen. Es erfolgt in der Regel **kostenlos oder auf Gegenseitigkeit** (Settlement-Free Peering), wenn beide Netzwerke einen gegenseitigen Nutzen haben.
- **Typen von Peering:**
    - **Private Peering**: Direkte Verbindung zwischen zwei Netzwerken in einem Rechenzentrum über dedizierte Links.
    - **Public Peering**: Verbindung über einen Internet Exchange Point (IXP), wo mehrere Netzwerke ihre Daten austauschen.
- **Vorteile:**
    - Reduzierte Latenz, da Daten direkt ausgetauscht werden.
    - Weniger Abhängigkeit von Transit-Anbietern (ISPs).
    - Kostenersparnis, da kein dritter Anbieter involviert ist.
- **Nachteil:**
    - Erfordert oft eine Vereinbarung zwischen beiden Parteien und technische Ressourcen zur Umsetzung.


2 Transit

Ein Netzwerk kann  mit ein AS of telekom 这个 ISP erst kommmunizieren , nur das Netzwerk schon mit Telekom 这个 ISP eine Vertrag abgeschlossen haben (das Netwerk hat schon ISP Telekom gemietet  ) 
只有当签订协约了,  这个 新的 Netzwerk 才能和 Telekem 这个 ISP 中的 AS 联系 

Transit ist ein kommerzieller Dienst, bei dem ein Netzbetreiber (ISP oder Tier-1-Netzwerk) einem kleineren Netzwerk Zugang zum gesamten oder einem Teil des Internets ermöglicht. Dafür wird **eine Gebühr gezahlt**.
- **Typen von Transit:**
    - **Full Transit**: Der ISP bietet Zugang zum gesamten Internet.
    - **Partial Transit**: Der ISP bietet Zugang zu bestimmten Netzwerken oder Regionen.
- **Vorteile:**
    - Zugang zum gesamten Internet, nicht nur zu Peering-Partnern.
    - Keine individuellen Peering-Vereinbarungen nötig.
- **Nachteil:**
    - Transit-Kosten können hoch sein.
    - Höhere Latenz, da Daten über mehrere Hops geroutet werden können.


----

Was ist der Unterschied zwischen Routing und Forwarding?

- Intraprotocoll
    - BGP:  Border Gateway protocoll
- Interprotocol (innerhalb eines AS)
    - OSPF: open shortest Path First 

![](image/Pasted%20image%2020250210220450.png)



Routing
- ip to ip addresse weiterleiten 

Forwarding 
- Packet Header : zielgerat

# 5 DHCP*

Angenommen, Ihr Rechner befindet sich in einem Zustand, in dem er – bis auf seine eigene MAC-Adresse – über keine weiteren Information über das Netzwerk zu dem er sich gerade verbunden hat, verfügt.

Nun soll untersucht werden, welche Schritte nötig sind, bevor Ihr Rechner eine Verbindung zum Standardgateway und somit zum Internet aufbauen kann. Wir haben Ihnen dazu eine Trace-File auf ISIS bereitgestellt.
14. Welches Protokoll ist dafür verantwortlich, dass Ihr Rechner eine IP-Adresse bekommt? Welche Pakete dieses Protokolls finden Sie in dem Trace-File? Was ist in diesen Paketen als Source- und Destination IP-Adresse eingetragen und warum? Welche Adresse möchte der Rechner gerne bekommen? Wird diesem Wunsch entsprochen? Wie lange ist die Adresse gültig? Was ist der zuständige DNS-Server?
15. Was sind die Pakete 1 und 4-7 und wozu dienen sie?
16. Nun verfügt der Rechner über alle nötigen Information, um Daten ins Internet zu schicken. Er ruft jetzt eine Webseite über eine URL auf. Nach welchem Hostnamen wird zunächst per DNS gefragt? Was ist die entsprechende Antwort des DNS-Servers?
17. Nun wird der HTTP-Server kontaktiert. Welche vollständige URL wird angefragt? Welcher Browser wird genutzt? Welche Server-Software antwortet? Handelt es sich um eine persistente Verbindung?
18. Nun wird eine zweite Verbindung aufgebaut. Welcher DNS-Name wird diesmal aufgelöst? Auf welchen Ports (Client/Server) wird die Verbindung aufgebaut? Welchem Protokoll entspricht das standardmäßig? Schauen Sie sich den Datenaustausch zwischen Server und Client an. Warum können Sie keine sinnvollen Daten erkennen?
19. Nicht nur Sie können Ihren eigenen Netzwerkverkehr mitschneiden, sondern auch jeder andere, der sich in Ihrem Netzwerk befindet, bzw. in Reichweite ihrer WLAN Karte aufhält. Welche potentiellen Sicherheitsrisiken fallen Ihnen bei der Verwendung von Protokollen wie z.B. HTTP, FTP, SMTP, etc. ein?




# 6 TIME-WAIT

RFC11221 sieht vor, dass Sockets nach dem Schließen im TIME-WAIT Zustand verbleiben

```
When a connection is closed actively, it MUST linger in TIME-WAIT state for a
time 2xMSL (Maximum Segment Lifetime). However, it MAY accept a new SYN
from the remote TCP to reopen the connection directly from TIME-WAIT state,
if it: [. . . ]
```

Die MSL ist laut TCP 120 s, entsprechend verweilen Sockets für eine signifikante Zeit. Dies führt dazu, dass Anwendungen den Socket in diesem Zeitraum nicht verwenden können, da die Socket Address already in use ist.

Warum sieht TCP diesen Zustand vor? In welchem Fall ist das Verweilen des Sockets sinnvoll?



# 7 CRC

Gegeben sei das Generatorpolynom $x^5+x^4+x^1+x^0$. Sie wollen den Bitstring 10010101011 verschicken.
20. Welche CRC-Checksumme müssen Sie an den Bitstring anhängen?
21. Wie wird auf Empfängerseite ein empfangenes Paket überprüft?
