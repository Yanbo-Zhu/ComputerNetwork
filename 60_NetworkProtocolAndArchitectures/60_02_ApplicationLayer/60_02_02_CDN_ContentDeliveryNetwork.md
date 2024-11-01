


DNS resolution of site gives different answers to clients Tell each client the site is the nearest replica (map client IP)




![](image/Pasted%20image%2020241031225046.png)


Verteilung der Anfragen
- Server-basierte HTTP Redirection: Server liefert aufgrund der IP-Adresse des Clients einen geeigneten anderen Server, erfordert zusätzliche RTT, Gefahr der Überlast für Server
- Client-nahe HTTP-Redirection: z.B. durch Web-Proxy, schwieriger zu verwirklichen
- DNS-basierte Redirection: DNS-Server bildet den Domain-Namen des Servers auf die IP-Adresse eines geeigneten Servers ab
- URL-Rewriting: Server liefert Basisseite, die URLs der eingebetteten Objekte werden umgeschrieben, mit dem Domain-Namen eines geeigneten anderen Servers
- kommerzielle CDNs verwenden meist Kombination aus DNS-basierter Redirection und URL-Rewriting

