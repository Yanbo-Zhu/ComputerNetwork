
# 1 Fluss- und Überlastkontrolle*

Beantworten Sie die folgenden Fragen im Kontext von Transportprotokollen:
1. Was ist der Unterschied zwischen Flusskontrolle und Überlastkontrolle?
2. Über welchen Mechanismus wird Flusskontrolle in TCP implementiert?
3. Über welchen Mechanismus wird Überlastkontrolle in TCP implementiert?
4. Inwiefern hängen die beiden Aspekte zusammen?


# 2 NAT

Angenommen Sie greifen auf das Internet über einen Router zu. Beantworten Sie die
folgenden Fragen:
1. Sie schalten Ihren Rechner an und erhalten automatisch eine IP-Adresse. Die dafür benutzte Technologie nennt sich DHCP. Wie sieht die initiale Anfrage nach einer Adresse aus?
2. Sie haben eine Adresse zugewiesen bekommen. Wie lange ist diese gültig?
3. Was macht Ihr Rechner, damit die Adresse länger gültig bleibt? Wann tut er dies?
4. Ihr Router stellt das Internet über NAT bereit. Was bedeutet das?


# 3 IPv4
Beantworten Sie im Kontext von IP die folgenden Fragen:
1. Was sind Class A, B und C Netze? Welcher Teil der Adresse gehört jeweils zu Host
bzw. Netzwerk?
2. Überprüfen Sie, ob die IPv4-Adresse 149.77.115.54 im Netzwerk 149.77.112.0
mit der Netzwerkmaske 255.255.252.0 liegt.
1. Nennen Sie zwei Ansätze, mit dem Problem der knappen Internet-Adressen umzugehen.
2. Wie hilft das heute verwendete Classless Inter-Domain Routing (CIDR), das Problem zu lösen?


# 4 Full-Stack*

Sie haben in der Vorlesung wichtige Komponenten, aus denen das Internet zusammengesetzt ist, kennengelernt. Beantworten Sie in diesem Kontext die folgenden Fragen und machen Sie sich klar, wo die Technologien jeweils verwendet werden:
1. Grenzen Sie die folgenden Begriffe gegeneinander ab. Auf welcher Ebene des ISO/OSI Modells arbeiten die jeweiligen Elemente?
    1. • Repeater
    2. • Hub
    3. • Bridge
    4. • Switch
    5. • Router
2. Was ist eine IP-Adresse, was eine MAC-Adresse?
3. Welches sind die speziellen IPv4-Adressen für Loopback und (limited) Broadcast?
4. Wie funktioniert ARP?
5. Nennen und erklären Sie die 4 Arten von Delay in einem Netz mit Paket-Vermittlung.
6. Was ist ein Autonomes System (AS)?
7. Was ist der Unterschied zwischen Peering und Transit?
8. Was ist der Unterschied zwischen Routing und Forwarding?

# 5 DHCP*

Angenommen, Ihr Rechner befindet sich in einem Zustand, in dem er – bis auf seine
eigene MAC-Adresse – über keine weiteren Information über das Netzwerk zu dem er sich gerade verbunden hat, verfügt.

Nun soll untersucht werden, welche Schritte nötig sind, bevor Ihr Rechner eine Verbindung zum Standardgateway und somit zum Internet aufbauen kann. Wir haben Ihnen dazu eine Trace-File auf ISIS bereitgestellt.
1. Welches Protokoll ist dafür verantwortlich, dass Ihr Rechner eine IP-Adresse bekommt? Welche Pakete dieses Protokolls finden Sie in dem Trace-File? Was ist in diesen Paketen als Source- und Destination IP-Adresse eingetragen und warum? Welche Adresse möchte der Rechner gerne bekommen? Wird diesem Wunsch entsprochen? Wie lange ist die Adresse gültig? Was ist der zuständige DNS-Server?
2. Was sind die Pakete 1 und 4-7 und wozu dienen sie?
3. Nun verfügt der Rechner über alle nötigen Information, um Daten ins Internet zu schicken. Er ruft jetzt eine Webseite über eine URL auf. Nach welchem Hostnamen wird zunächst per DNS gefragt? Was ist die entsprechende Antwort des DNSServers?
4. Nun wird der HTTP-Server kontaktiert. Welche vollständige URL wird angefragt? Welcher Browser wird genutzt? Welche Server-Software antwortet? Handelt es sich um eine persistente Verbindung?
5. Nun wird eine zweite Verbindung aufgebaut. Welcher DNS-Name wird diesmal aufgelöst? Auf welchen Ports (Client/Server) wird die Verbindung aufgebaut? Welchem Protokoll entspricht das standardmäßig? Schauen Sie sich den Datenaustausch zwischen Server und Client an. Warum können Sie keine sinnvollen Daten erkennen?
6. Nicht nur Sie können Ihren eigenen Netzwerkverkehr mitschneiden, sondern auch jeder andere, der sich in Ihrem Netzwerk befindet, bzw. in Reichweite ihrer WLAN Karte aufhält. Welche potentiellen Sicherheitsrisiken fallen Ihnen bei der Verwendung von Protokollen wie z.B. HTTP, FTP, SMTP, etc. ein?



# 6 TIME-WAIT

RFC11221 sieht vor, dass Sockets nach dem Schließen im TIME-WAIT Zustand verbleiben

```
When a connection is closed actively, it MUST linger in TIME-WAIT state for a
time 2xMSL (Maximum Segment Lifetime). However, it MAY accept a new SYN
from the remote TCP to reopen the connection directly from TIME-WAIT state,
if it: [. . . ]
```

Die MSL ist laut TCP 120 s, entsprechend verweilen Sockets für eine signifikante Zeit.
Dies führt dazu, dass Anwendungen den Socket in diesem Zeitraum nicht verwenden
können, da die Socket Address already in use ist.

Warum sieht TCP diesen Zustand vor? In welchem Fall ist das Verweilen des Sockets
sinnvoll?

# 7 CRC
Gegeben sei das Generatorpolynom $x^5+x^4+x^1+x^0$. Sie wollen den Bitstring 10010101011 verschicken.
1. Welche CRC-Checksumme müssen Sie an den Bitstring anhängen?
2. Wie wird auf Empfängerseite ein empfangenes Paket überprüft?
