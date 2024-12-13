
# 1 HTTP

Beantworten Sie im Kontext vom Hypertext Transfer Protocol (HTTP) folgende Fragen:
1. Welche HTTP Methoden gibt es und was machen diese?
2. Welche HTTP Methoden sind idempotent?
3. Was sind persistente und nicht-persistente Verbindungen?
4. HTTP ist ein zustandsloses Protokoll. Was bedeutet das? Mit welcher Technik können Server z.B. trotzdem den Warenkorb einer Nutzers speichern?

## 1.1 Welche HTTP Methoden gibt es und was machen diese?

Get : get Rssource 
Delete: Loscht Ressource 
Head, wie get, aber nur get information of  Header 
==Put==:   Erstellen neue Resource, die vorher nicht exisitieren.  Wenn die Rosurece existiert, ersetztet Ressource , old daten erstenz 
==Post==:  Erstellen neue Resource.  Wenn die Rosurece steh, hangt an  Ressource ,  (student schon ein Note, dan post, dann student hat zwei Note )


## 1.2 Welche HTTP Methoden sind idempotent?

Beim mhermailigen Ausfuhren, erhalten wir das gleiche Ergebnise  auf Server Seite 
Nur Get und Put, Head, Delete, PUT  sind idempotent 
Post ist nicht idempotent, da immer Daten hangen, nicht ersetzten 


## 1.3 Was sind persistent und nicht-persistente Verbindungen? 

persistent : Verbindung bleibt nach anfrage bestehen 
nicht persistent : server schließt Verbinding, nachdem er Antwort empfangen habt.   Neue Request , setzen eine neuer verbidnungsaufbau 


## 1.4 HTTP ist ein zustandsloses Protokoll. Was bedeutet das? Mit welcher Technik können Server z.B. trotzdem den Warenkorb einer Nutzers speichern?

Stateless Protokoll
Sever speichert Statusinformation der vergangenen Session gar nicht 

Server Speichert keinen Client-State. Anfrage sind unabhangig  

Client speichert State selber. Der Zustand und Info wird in form von Cookie speichert. 
Client sendet Statusinformation mit Cookies, Cache. 


# 2 Rest-Methode

Gegeben sei ein RESTful Webservice, der Teilnehmer in einer Veranstaltung verwaltet. Der Service läuft auf dem Rechner mit dem DNS-Namen api.tkn.tu-berlin.de. Folgendes Verhalten ist bereits vorgegeben und implementiert, dabei sind `<courseName> und <student>` jeweils Platzhalter für die jeweilige Veranstaltung bzw. den Teilnehmer

![](image/Pasted%20image%2020241126164040.png)

## 2.1 Ist eine der Ressourcen eine Collection-URL und falls ja, geben sie die komplettevURL an (inkl. verwendender Zugriffsmethode und Hostnamen)?

`Get https://api.tkn.tu-berlin.de/xourses/name1 `

weil  get den Resource, Dann get eine Sammelung von Teilnehmer zuruck. 

## 2.2 Der Webservice soll von Ihnen nun um die Funktionalität erweitert werden, die Klausurnote eines Teilnehmers zu ändern. Welche HTTP-Methode muss dazu auf welche URL aufgerufen werden?


Put /courses/courseName1/student1 



## 2.3 Was wäre allgemein der Inhalt (Payload) der Anfrage aus Teilaufg. 2.

Put /courses/courseName1/kilian/5.0


```

# Header 
Content-Type: application/json
Authorization: Bearer <access_token>

# 
{
  "id": 123,
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "phone": "123-456-7890"
}


```


## 2.4 Der Webservice soll von Ihnen nun um die Funktionalität erweitert werden, einen neuen Teilnehmer zur Veranstaltung hinzuzufügen, welcher vorher noch nicht angemeldet war. Welche HTTP-Methode muss dazu auf welche URL aufgerufen werden?


Post 

/courses/courseName1


# 3 Client-Server-Architektur

Diskutieren Sie Vor- und Nachteile der Client-Server-Architektur, auch im Vergleich zu Peer-to-Peer Systemen.


Nachteile:
- Server ist single Point of Failure 
    - Serverausfall
- Engpasse Latenzen moglich
- Zentrale Datenleaks moglich

Vorteil
- Client und Server kann seperat entworfen werden
    -  Client kann viele Anfrageen gleichzeitig bearbeiten
- Skaliert besser 
- Server is leicht zu modifizieren
- zentrale Wartung durch Server
- Server setzen Sicherheitsstandards um 
    - nicht immer 

## 3.1 Client-Server-Architektur

## 3.2 Peer-To-Peer

Vorteil:
- jeder ist eine gleichwertiger Peer
    - Peer kann Services anbieten und anfrage

Vorteile:
- Lastverteilung auf mehrere Geräte mgl.
- Clients können auf die Dienste des Servers aus Entfernung zugreifen
- Client und Server können separat entworfen werden
- Einfaches, wohlbekanntes Programmierparadigma: Funktionsaufruf
- Auf den Server kann gleichzeitig von mehreren Clients zugegriffen werden
- Einfache Ermöglichung der Modifikation des Servers

Nachteile:
- Server kann Engpass werden
- Signifikante Latenz der Kommunikation möglich
- Ressourcen (z.B. Festplattenplatz) der Clients werden evtl. verschwende
- Finden des Servers für einen bestimmten Service nicht trivial
- Der Service kann deaktiviert sein wenn ein Server ausfällt



# 4 Iperf

Im Internet gibt es Server, die einen kostenlosen Service zur Durchsatzmessung anbieten.
1. Wählen Sie einen der Server aus und schätzen Sie zunächst Round-trip-Time anhand des angegebenen Standorts ab. Gehen Sie dabei davon aus, dass sich die Signale durchschnittlich mit 200 000kms−1 ausbreiten.
2. Verwenden Sie nun das Kommandozeilenprogramm ping um die Round-Trip-Time (RTT) zwischen Ihrem Rechner und dem gewählten Server zu messen. Versuchen Sie die Unterschiede zu Ihrer Schätzung zu erklären.
3. Verwenden Sie das Kommando `iperf3 -c [HOSTNAME]` um mit dem iperf3 Programm den Durchsatz von ihrem Rechner zu dem oben gewählten Server über Streaming-Sockets zu messen.
4. Berechnen Sie, wie viele Daten bei einer Streaming-Socket-Verbindung zwischen ihrem Rechner und dem oben genannten Server durchschnittlich zu jeder Zeit unterwegs sind.
5. Wo sind diese Daten gespeichert?

---

The `iperf3` command is a widely used tool to measure and analyze network performance. It is particularly useful for testing bandwidth, latency, and packet loss between two devices. 

Here's an explanation and common usage examples:

## 4.1 **General Overview**

`iperf3` operates in a **client-server architecture**:
- **Server mode**: Waits for incoming connections.
- **Client mode**: Sends data to the server for testing.

After running a test, `iperf3` provides detailed statistics:
- **Bandwidth**: Amount of data transmitted per second (e.g., `500 Mbps`).
- **Packet Loss (UDP)**: Indicates the number of lost packets.
- **Latency (UDP)**: Displays the delay in milliseconds.

## 4.2 **Start the Server** on a server 

```
iperf3 -s
```

- `-s`: Starts `iperf3` in server mode.
- By default, the server listens on port `5201`.

Optional:
- Specify a different port:  `iperf3 -s -p 12345`

## 4.3 **Run the Client** on another server 

`iperf3 -c <server_ip>`
- `-c <server_ip>`: Specifies the IP address or hostname of the server.
- By default, the client connects to port `5201`.

`iperf3 -c 192.168.1.10`

## 4.4 `iperf3 -c www.google.com` will not work  是没有效果的 

1. **`iperf3` Requires a Compatible Server**:
    - `iperf3` operates in a **client-server architecture**, ==where a server must run `iperf3` in server mode (`iperf3 -s`) to accept connections from a client.==
    - Google does not run an `iperf3` server at `www.google.com` or any of its public IPs. They only host web services, not network performance testing services.
2. **Unmatched Protocols**:
    - `iperf3` uses its own protocol to test network performance. If the target server doesn't understand this protocol, the connection will fail.





# 5 Socket RPC
Angenommen Sie möchten ein Programm schreiben, das eine Funktion auf einem entfernten Server per Socket-API aufruft und dabei als Argument ein Paar aus ID und Namen übergibt. 

Das Programm speichert das Paar intern wie hier dargestellt:

```
1 typedef struct ValueType {
2 int id;
3 const char* name;
4 } ValueType;
```


1. Kann der dargestellte struct ValueType direkt an send() übergeben werden? Spielt die Prozessorarchitektur hierbei eine Rolle?
2. Funktionsaufrufe auf entfernten Systemen werden oft als sog. Web Services über HTTP ausgeführt (siehe REST). Wie löst HTTP die Probleme aus der ersten Teilaufgabe?
3. Wie können komplexe Datenstrukturen übertragen werden, bspw. verkettete Listen oder Binärbäume?



# 6 Netcat

nc
发送一个 request 到 entfernt Server 
request content 直接写在命令里面 

Das Programm netcat (kurz nc) ist ein einfaches Netzwerkprogramm, das eine Verbindung zu einem entfernten Server aufbaut und die Eingabe von stdin über das Netzwerk an den entfernten Server sendet. 
Das Programm kann auch als Server fungieren, wobei auf eingehende Verbindungen gewartet wird und alle empfangenen Daten nach stdout geschrieben werden. Netcat ist deshalb gut geeignet, um die eigene Implementierung von einfachen Netzwerkprotokollen zu testen. 

Probieren Sie deshalb die folgenden Anwendungsbeispiele aus:
Benutzen Sie das nc Kommandozeilen-Programm und rufen Sie eine Webseite auf der Konsole ab. 


---

```
echo -n -e "GET / HTTP/1.1\r\n\
Host: whatismyip.akamai.com\r\n\
Connection: close\r\n\r\n" | nc whatismyip.akamai.com 80
```


| 是一个 pipe 

cmd 1 | cmd 2
stdout of cmd1 ist stdin of cmd2 

- **`echo -n -e`**:
    - `-n`: Verhindert, dass ein Zeilenumbruch am Ende der Ausgabe hinzugefügt wird.
    - `-e`: Ermöglicht die Interpretation von Escape-Sequenzen (z. B. `\r`, `\n`).
- **`"GET / HTTP/1.1\r\nHost: whatismyip.akamai.com\r\nConnection: close\r\n\r\n"`**:
    - Baut die HTTP-GET-Anfrage zusammen:
        - `GET / HTTP/1.1`: Fordert die Root-Resource `/` mit HTTP/1.1 an.
        - `Host: whatismyip.akamai.com`: Gibt den Zielhost an.
        - `Connection: close`: Signalisiert, dass die Verbindung nach der Antwort geschlossen werden soll.
            - ==expizit angegeben, ansonsten wird das Connection nicht automatische geschlossen== 
        - `\r\n`: Signalisiert das Ende einer Zeile im HTTP-Protokoll.
        - Doppeltes `\r\n` markiert das Ende des Header-Blocks.
- **`| nc whatismyip.akamai.com 80`**:
    - Der Pipe-Operator (`|`) leitet die Ausgabe des `echo`-Kommandos an `nc` (netcat) weiter.
    - `nc whatismyip.akamai.com 80`: Stellt eine TCP-Verbindung zu `whatismyip.akamai.com` auf Port 80 her und sendet die Anfrage.


---


```
yzh@BNB23350:~$ echo -n -e "GET / HTTP/1.1\r\n\
Host: whatismyip.akamai.com\r\n\
Connection: close\r\n\r\n" | nc whatismyip.akamai.com 80


下面的是得到的response 
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 13
Expires: Tue, 26 Nov 2024 19:44:36 GMT
Cache-Control: max-age=0, no-cache, no-store
Pragma: no-cache
Date: Tue, 26 Nov 2024 19:44:36 GMT
Connection: close
```










