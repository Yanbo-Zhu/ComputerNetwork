

# 1 Overview 

HTTP: hypertext transfer protocol
- Web’s application layer protocol
- client/server model:
    - client: browser that requests, receives, (using HTTP protocol) and “displays” Web objects
    - server: Web server sends (using HTTP protocol) objects in response to requests

![](image/Pasted%20image%2020241208215536.png)




# 2 URL 


![](image/Pasted%20image%2020241031221639.png)



> stateless: server maintains no information about past client requests 

# 3 HTTP use TCP 


![](../../60_01_Intro/image/Pasted%20image%2020241021073237.png)

HTTP ist ein zustandsloses Protokoll. 
Das bedeutet, dass der Server keine Informationen über vorherige Anfragen speichert. Jeder Request wird unabhängig behandelt. Falls Sitzungsinformationen benötigt werden, müssen zusätzliche Mechanismen wie Cookies verwendet werden.

HTTP uses TCP:
- client initiates TCP connection (creates socket) to server, port 80
- server accepts TCP connection from client
- HTTP messages (application-layer protocol messages) exchanged between browser (HTTP client) and Web server (HTTP server)
- TCP connection closed

Non-persistent HTTP
1. TCP connection opened
2. at most one object sent over TCP connection
3. TCP connection closed
downloading multiple objects required multiple connections

Persistent HTTP
- TCP connection opened to a server
- multiple objects can be sent over single TCP connection between client, and that server
- TCP connection closed




## 3.1 Non-persistent HTTP



![](../../60_01_Intro/image/Pasted%20image%2020241021073527.png)

![](../../60_01_Intro/image/Pasted%20image%2020241021073534.png)


## 3.2 Persistent HTTP

Non-persistent HTTP issues:
- requires 2 RTTs per object
- OS overhead for each TCP connection
- browsers often open multiple parallel TCP connections to fetch referenced objects in parallel


Persistent HTTP (HTTP1.1):
- server leaves connection open after sending response
- subsequent HTTP messages between same client/server sent over open connection
- client sends requests as soon as it encounters a referenced object
- as little as one RTT for all the referenced objects (cutting response time in half)


# 4 HTTP Request message format



![](image/Pasted%20image%2020241031223127.png)


![](image/Pasted%20image%2020241031223136.png)

![](../../60_01_Intro/image/Pasted%20image%2020241021073742.png)




![](../../60_01_Intro/image/Pasted%20image%2020241021073735.png)


# 5 HTTP Request Method 


![](image/Pasted%20image%2020241208203219.png)


- GET: 
    - ruft Daten vom Server ab. (Beispiel: Abrufen einer Webseite)
    - (for sending data to server)
    - Abfragen einer Ressource
    - 请求资源数据，通常只读。
    - include user data in URL field of HTTP GET request message (following a '?'): www. some site . com/animalsearch?monkeys &banana
- POST: 
    - sendet Daten an den Server, z. B. um eine neue Ressource zu erstellen. (Beispiel: Absenden eines Formulars)
    - Generell: Anhängen/erweitern von Daten einer bestehenden Ressource oder erstellen einer neuen Ressource die noch nicht definiert ist. Jedoch: Semantik laut RFC 7321 nicht näher festgelegt.
    - 提交数据，创建资源或执行某些处理。
    - 可以在原来的数据的后面新加入数据, 不为 idempotent
    - web page often includes form input
    - user input sent from client to server in entity body of HTTP POST request message
- PUT: 
    - aktualisiert oder erstellt eine Ressource. (Beispiel: Hochladen oder Ersetzen von Daten)
    - uploads new file (object) to server
    - completely replaces file that exists at specified URL with content in entity body of POST HTTP request message
    - Erstellen oder Ersetzen einer Ressource (unter einer gegebenen URI) mit der mitgeschickten Repräsentation in der Nachricht
    - 更新或创建资源，发送完整的资源信息。
    -  彻底覆盖原来的数据, 为 idempotent
- DELETE: 
    - löscht eine Ressource. (Beispiel: Entfernen eines Datensatzes)
    - requests headers (only) that would be returned if specified URL were requested with an HTTP GET method.
    - Entfernen einer Ressource
- HEAD: 
    - holt nur die Header-Informationen der Ressource. (Beispiel: Prüfen, ob eine Datei existiert)
    - 类似 GET，但只获取响应头。
    - Identisch zu GET, außer dass der Server nur mit einem Header antwortet
- OPTIONS: fragt die unterstützten HTTP-Methoden des Servers ab.
- PATCH: Teilaktualisierung einer Ressource.


- **GET**:
    - 用于从服务器获取数据。
    - 请求的数据通过 URL 传递（查询参数或路径）。
    - 通常是幂等的（多次执行同一个请求，服务器返回的结果应该相同）。
    - 不会对服务器上的数据造成修改，仅用于请求数据。
    - 响应中会包含完整的数据（比如 HTML 页面、JSON 等）。
    
    **例子**: 请求获取用户信息  
    `GET /users/123`
    
- **POST**:
    - 用于向服务器提交数据。
    - 数据通常在请求体中传递（不像 GET 使用 URL 参数）。
    - 一般用于创建或提交新的资源或处理表单数据。
    - 非幂等（同样的 POST 请求可以创建多个相同的资源）。
    - 响应可能会返回操作结果、生成的资源 ID，或其他状态信息。
    
    **例子**: 提交一个新用户  
    `POST /users`  
    请求体: `{ "name": "John", "age": 30 }`
    
- **HEAD**:
    - 类似于 GET，但只返回 HTTP 响应头而不返回响应体。
    - 用于检查资源的元信息，比如验证资源是否存在，检查内容类型、大小等。
    - 常用于在下载或获取数据前检查资源的状态。
    
    **例子**: 检查用户信息是否存在  
    `HEAD /users/123`
    
- **PUT**:
    - 用于更新服务器上的资源（或者如果资源不存在，创建资源）。
    - 数据通常在请求体中传递，表示完整的资源。
    - 幂等的（多次执行同一个 PUT 请求，结果不会改变服务器的状态）。
    - 通常用于替换现有资源的全部内容。
    
    **例子**: 更新用户信息  
    `PUT /users/123`  
    请求体: `{ "name": "John", "age": 31 }`


## 5.1 Welche Methoden sind idempotent bzw. safe

|   |   |   |   |
|---|---|---|---|
|Method|Safe|Idempotent|Cacheable|
|[GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET)|Yes|Yes|Yes|
|[HEAD](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD)|Yes|Yes|Yes|
|[OPTIONS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS)|Yes|Yes|No|
|[TRACE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/TRACE)|Yes|Yes|No|
|[PUT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT)|No|Yes|No|
|[DELETE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE)|No|Yes|No|
|[POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)|No|No|Conditional|
|[PATCH](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH)|No|No|Conditional|
|[CONNECT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/CONNECT)|No|No|No|

- Idempotente Methoden:
    - Eine Methode ist idempotent, wenn mehrfaches Ausführen denselben Zustand erzeugt wie einmaliges Ausführen.
    - Idempotent: GET, PUT, DELETE, HEAD, OPTIONS
- Safe-Methoden:
    - Eine Methode ist safe, wenn sie keine Änderungen auf dem Server verursacht.
    - Safe: GET, HEAD, OPTIONS

## 5.2 Warum ist PUT idempotent, wenn die Anfrage eine Ressource hochlädt oder ersetzt? 


PUT ist idempotent, weil mehrfache Anfragen denselben Zustand herstellen. POST ist nicht idempotent, weil jede Anfrage neue Ressourcen oder Änderungen erzeugen kann.



PUT
- Definition: Eine HTTP-Methode ist idempotent, wenn das mehrfache Wiederholen derselben Anfrage keinen neuen Effekt hat. Das bedeutet, dass die Ressource nach einem zweiten, dritten oder x-ten PUT-Request denselben Zustand hat wie nach dem ersten Request.
- PUT lädt eine Ressource hoch oder ==ersetzt sie vollständig.==
- Wenn man dieselbe Ressource mit denselben Daten erneut hochlädt, wird die Ressource nicht verändert, weil der Zustand identisch bleibt.

Beispiel 1: Datei hochladen mit PUT
- Angenommen, man verwendet PUT, um die Datei example.txt mit folgendem Inhalt auf einen Server hochzuladen: Hallo Welt
- Beim ersten PUT-Request wird die Datei erstellt oder überschrieben.
- Wenn man genau denselben Request (mit Hallo Welt als Inhalt) erneut ausführt, wird die Datei ersetzt, aber das Endergebnis bleibt unverändert: Die Datei enthält immer noch "Hallo Welt".

**Beispiel 2: Ressource aktualisieren**
- Angenommen, man aktualisiert sein Benutzerprofil mit einer PUT-Anfrage:
```js
PUT /users/123
{
  "name": "Alice",
  "age": 25
}
```

- Beim ersten PUT-Request wird das Profil von Benutzer 123 aktualisiert.
- Beim zweiten PUT-Request (mit denselben Daten) bleibt das Profil unverändert, weil die Änderungen identisch sind.

---

POST
Warum ist POST nicht idempotent?
Im Gegensatz dazu ist POST nicht idempotent, weil jeder POST-Request eine neue Ressource erstellt oder eine andere nicht deterministische Aktion ausführt.
Beispiel: Blogpost erstellen mit POST
```
POST /posts
{
  "title": "Mein erster Blog",
  "content": "Hallo Welt"
}
```

Beim ersten POST-Request wird ein neuer Blog-Eintrag erstellt (z. B. id=1).
Wenn man denselben POST-Request erneut ausführt, wird ==ein weiterer Eintrag erstellt== (z. B. id=2). Das Ergebnis ist also nicht dasselbe wie nach dem ersten Request.


# 6 Header Field 


![](image/Pasted%20image%2020241208203522.png)

- **_HTTP Header Fields_** transportieren wichtige Argumente und Parameter für die Verarbeitung einer HTTP-Nachricht beim Empfänger
- Werden im Header einer HTTP-Nachricht direkt nach der Anfrage- bzw. Antwortzeile in Form von Schlüssel/Wert-Paaren (Key/value pairs) übertragen


BEISPIELE FÜR HTTP-HEADER FIELDS
![](image/Pasted%20image%2020250106115320.png)

## 6.1 Artern von Header Fields

![](image/Pasted%20image%2020250106115415.png)


**Standardized Header Fields**
- Müssen in allen HTTP-Implementierungen unterstützt werden
- Grundlage bildet der RFC 2616 mit Erweiterungen in RFC 4229 der IETF

**Customized Header Fields**
- Entwickler können eigene Header Fields für ihre Anwendungen definieren
- (veraltete) Konvention: Kennzeichnung durch den Präfix "X-"

**Request Header Fields**
- Werden in HTTP-Anfragen übertragen
- Enthalten Informationen über die angeforderte Ressource oder über den Client, der die Ressource anfordert

**Response Header Fields**
- Werden in HTTP-Antworten übertragen
- Liefern Informationen über die Antwort oder über den Server

**End-to-End Header Fields**
- Sind für den finalen Empfänger der Nachricht bestimmt
- Müssen durch Gateways unverändert weitergeleitet werden

**Hop-by-Hop Header Fields**
- Sind für einen Empfänger auf einer Teilstrecke der Ende-zu-Ende-Verbindung bestimmt, z.B. ein Gateway
- Werden in der Regel nicht weitergeleitet

**Forbidden Header Fields**
- Dürfen nicht durch JavaScript gelesen, verändert oder gesetzt werden
- Grund: Manipulation durch JavaScript ermöglicht Angriffe

**Representation Header Fields**
- Enthalten Informationen über die im Body enthaltene Ressource, z.B. MIME type oder über die verwendete Codierung und Komprimierung


## 6.2 Accept UND Content-Type-HEADER

Header Fields einer HTTP-Anfrage (Auszug)
![](image/Pasted%20image%2020250106115808.png)

Header Fields einer zugehörigen HTTP-Antwort (Auszug)
![](image/Pasted%20image%2020250106115820.png)

- `**Accept**`-Header einer Anfrage gibt an, welche MIME-Typen der Client verarbeiten kann und bevorzugt
- `**image/avif**` (AV1 Image File Format) ist ein neues Bildformat mit einer hohen Kompressionseffizienz
- `**image/webp**` ist ein von Google entwickeltes Bildformat
- `**image/apng**` (Animated Portable Network Graphics) ist eine Erweiterung von PNG
- `**image/svg+xml**` ist ein XML-basiertes Vektorformat
- `**image/***` bedeutet, dass der Client alle MIME-Typen akzeptieren kann, die unter image/ fallen
- `***/*; q=0.8**` bedeutet, dass der Client alle Medientypen mit einer Priorität von 0.8 (von 0..1) akzeptiert
- `**Content-Type**`-Header gibt den MIME-Typ der Inhalte im Body der Antwort an

# 7 Cookies 

为什么需要 Cookies: HTTP GET/response interaction is stateless
> Web sites and client browser use cookies to maintain some state between transactions

Client-side state maintenance
- Client stores small (?) state on behalf of server
- Client sends state in future requests to the server
Can provide authentication


What cookies can be used for: Cookies 的用途 
▪ authorization
▪ shopping carts
▪ recommendations
▪ user session state (Web e-mail)

---


![](image/Pasted%20image%2020250106120046.png)

- HTTP ist ein zustandsloses Protokoll: Seitenaufrufe vom selben Browser können durch das Protokoll nicht zugeordnet werden
- Behelfsmaßnahme: Einführung von Cookies
- Kleine Dateneinheiten mit Textinformationen
- Austausch von Cookies zwischen Webserver und Browser (und umgekehrt) zur Erkennung wiederkehrender Nutzer
- Anwendungen
    - Einkaufswagen auf eCommerce-Seiten
    - Vermeidung expliziter Nutzerauthentifizierung durch Passworteingabe bei jedem Seitenaufruf
    - Maßschneiderung von Webseiten
    - Tracking von Nutzern


## 7.1 Set-Cookie UND Cookie-HEADER


![](image/Pasted%20image%2020241031224142.png)


four components:
1) cookie header line of HTTP response message
2) cookie header line in next HTTP request message
3) cookie file kept on user’s host, managed by user’s browser
4) back-end database at Web site

![](image/Pasted%20image%2020241021103143.png)



![](image/Pasted%20image%2020250106120612.png)

- Cookies werden durch `**Set-Cookie**` in der HTTP-Antwort durch den Server im Browser gesetzt
- Nachfolgende Anfragen an denselben Server enthalten das gesetzte Cookie in `**Cookie**`
- Werden in Cookies SessionIDs übertragen, dürfen diese niemals in vorhersehbarer Reihenfolge  z.B. aufsteigend, absteigend, usw.) generiert werden
- SessionIDs sollten immer mit "sicheren" Zufallszahlengeneratoren erzeugt werden



## 7.2 Der Cookie-Lebenszyklus

![](image/Pasted%20image%2020250106121542.png)

## 7.3 Cookie-Attribute 

![](image/Pasted%20image%2020250106121749.png)

Cookie-Attribute 
- `**Domain**`-Attribut legt fest, zu welcher Domain ein gesetztes Cookie gehört
- Wenn `**Domain**` gesetzt ist, wird das Cookie dieser Domain und ihrer Subdomains gegenüber offengelegt
- Wenn `**Domain**` nicht definiert ist, wird das Cookie nur der Domain gegenüber offengelegt, die es gesetzt hat, aber nicht in ihren Subdomains

- `**Path**`-Attribut definiert den Pfad innerhalb der Domain, für den das Cookie gültig ist
- Das Cookie wird nur an Anfragen an diesen Pfad und dessen Unterverzeichnisse angehängt
- Wenn `**Path**` nicht definiert ist, wird das Cookie nur an Anfragen an diesen Pfad und dessen Unterverzeichnisse gesendet

- `**Max-Age**` definiert Lebensdauer des Cookies in Sekunden
- Gibt die Lebensdauer relativ zum Setzen des Cookies an
- `**Expires**` definiert einen spezifischen Zeitstempel zu dem das Cookie abläuft
- Wenn beide Attribute gesetzt sind, unterstützen moderne Browser `**Max-Age**`
- `**Max-Age**` und `**Expires**` werden nur bei persistenten Cookies gesetzt, nicht bei Session Cookies

- `**HttpOnly**` verhindert den Zugriff auf das Cookie durch JavaScript

- Bei gesetztem `**Secure**`-Attribut erfolgt die Übertragung an den Server nur über einer gesicherte Verbindung

## 7.4 Cookies-Arten
 - Session Cookies (Transient Cookies)
    - Gültigkeit nur für die Dauer einer Websitzung
    - Löschen von Session Cookies nach Beenden des Browsers
    - Verwahrung im Hauptspeicher des Browsers
    - Definition durch Weglassen der `**Expires**`- und `**Max-Age**`-Attribute
- Persistent Cookies (Tracking Cookies)
    - Dauerhafte Aufbewahrung durch Speicherung auf der Festplatte
    - Definition durch Definition von `**Expires**` oder `**Max-Age**`
- First-Party Cookies
    - Cookies, welche durch HTTP-Antworten der aufgerufenen Domain gesetzt und an diese gesendet  werden
- Third-Party Cookies
    - Cookies von Drittanbieter
    - Werden beim Abruf von Drittanbieterinhalten gesetzt, die in einer Webseite der First-Party eingebettet ist, z.B. per `**iframe**`-Element


---
3RD-PARTY-COOKIES

![](image/Pasted%20image%2020250106122721.png)


# 8 Referer UND Referrer-Policy

![](image/Pasted%20image%2020250106123009.png)

- Ein **_Referrer_** (**_Verweiser_**) 推荐人  bezeichnet eine Webseite von der eine andere Resource geladen wird
- Das `**Referer**`_-Feld_ in der HTTP-Anfrage bezeichnet die Herkunft (Origin) der verweisenden Webseite und enthält optional Pfade und Query String der verweisenden Seite
- Beachte: `**Referer**` ist ein immer noch angewandter Schreibfehler im Feldnamen
- Die `**_Herkunft_**` (**_Origin_**) spezifiziert das Schema (HTTP oder HTTPS), den Hostnamen und den Port einer Ressource
- Als **_Same-Origin-Request_** bezeichnet man eine Anfrage einer Webseite von Origin A an eine Resource von Origin A
- Als **_Cross-Origin-Request_** bezeichnet man eine Anfrage einer Webseite von Origin A an eine Resource von Origin B 
- Die `**Referrer-Policy**` spezifiziert in der Antwort der verweisenden Webseite die Bedingungen unter denen der Referrer in nachfolgenden Anfragen gesendet wird


Referrer-Policies

![](image/Pasted%20image%2020250106123319.png)


## 8.1 Same Origin Policy 

![](image/Pasted%20image%2020250106123549.png)

- **_Same Origin Policy_** (**_Gleiche-Herkunft-Richtlinie_**) untersagt  不准,禁止 JavaScript und CSS auf Ressourcen zuzugreifen, die von einer anderen Herkunft (Origin) sind 
- SOP unterbindet 不允许,禁止 , dass ein JavaScript-Programm einer Webseite eines Angreifers auf existierende Sitzungen eines Nutzers im Rahmen einer Webanwendung zugreifen kann, bei der Session Cookies verwendet werden
- Problem: SOP kann durch alternative Angriffsszenarien wie Cross Site Scripting umgangen werden
- Problem: SOP ist für viele Anwendungen zu restriktiv, da es oftmals gewünscht ist, Daten aus unterschiedlichen Origins in einer Webanwendung (Mashup) zu kombinieren 
- Lösung: Cross Origin Resource Sharing



## 8.2 Cross Origin Resource Sharing

![](image/Pasted%20image%2020250106123842.png)


- **_Cross-Origin Resource Sharing_** (**_CORS_**) ermöglicht JavaScript-Programmen das Absetzen von Cross-Origin Requests und den Empfang assoziierter Antworten 
- `**Origin**` zeigt der Cross-Origin die Herkunft des JavaScript-Programms an
- `**Access-Control-Allow-Origin**` zeigt dem Script an, ob der Zugriff auf die Ressource erlaubt ist
- Hinweis: `**Access-Control-Allow-Origin: ***` gestattet den Zugriff durch Scripts beliebiger Origins
- Weitere Konfigurationsmöglichkeiten durch zusätzliche `**Access-Control-***`-Felder
- Beispiel: `**Access-Control-Allow-Method: GET**` erlaubt nur das Absetzen von `**GET**`-Anfragen
- Preflight Requests sind Anfragen, die mittels `**OPTIONS**`-Methode gesendet werden um zu überprüfen, ob eine Anfrage erlaubt ist, jedoch ohne Daten zu senden




# 9 Response Code/ Status Code 
https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
https://www.semrush.com/blog/http-status-codes/

1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#informational_responses) (`100` – `199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses) (`200` – `299`)
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) (`300` – `399`)
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses) (`400` – `499`)
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (`500` – `599`)

![](image/Pasted%20image%2020241208203205.png)

![](image/Pasted%20image%2020241031223214.png)


- 1xx (Information): Hinweis, dass der Request bearbeitet wird. 
    - The server acknowleges and is processing the request 
    - 100 Continue: Anfrage kann fortgesetzt werden.
    - 101 Switching Protocols: Protokollwechsel wird akzeptiert.
- 2xx (Erfolg): Anfrage wurde erfolgreich bearbeitet.  
    - The server successfully received, understood, and processed the request 
    - 200 OK: Erfolgreiche Anfrage.
    - 201 Created: Ressource wurde erfolgreich erstellt.
    - 204 No Content: Erfolgreiche Anfrage, aber keine Inhalte zurückgegeben.
- 3xx (Umleitung): Ressource wurde verschoben oder umgeleitet. 
    - The server received the request, but there's a redirect to somewhere else (or, in rare cases, some additional action other than a redirect must be come
    - 301 Moved Permanently: Ressource dauerhaft verschoben.
    - 302 Found: Temporäre Umleitung.
    - 304 Not Modified: Ressource nicht geändert (Cache).
- 4xx (Client-Fehler): Fehler durch fehlerhafte Anfrage.
    - The server couldn't find (or reach) the page or website. This is an error on the site's side.
    - 400 Bad Request: Ungültige Anfrage.
    - 401 Unauthorized: Authentifizierung erforderlich.
    - 403 Forbidden: Zugriff verweigert.
    - 404 Not Found: Ressource nicht gefunden.
- 5xx (Server-Fehler): Fehler auf dem Server.
    - The client made a valid request but the server failed to complete the request 
    - 500 Internal Server Error: Allgemeiner Serverfehler.
    - 502 Bad Gateway: Fehler bei der Weiterleitung.
    - 503 Service Unavailable: Server ist vorübergehend nicht verfügbar.



# 10 Web Caches (Proxy Servers)

browser sends all HTTP requests to cache
- f object in cache: cache returns object to client
- else cache requests object from origin server, caches received object, then returns object to client

Why Web caching?
- reduce response time for client request, cache is closer to client
- reduce traffic on an institution’s access link
- Internet is dense with caches


Conditional GET through Cache 
Cachce 中有  up-to-date cached version 就不需要再去  server 端找最新的数据了 

![](image/Pasted%20image%2020241021103343.png)



# 11 HTTP/HTTPS协议

![](image/Pasted%20image%2020240216165526.png)

![](image/Pasted%20image%2020240216170154.png)


# 12 HTTP is Stateless

![](image/Pasted%20image%2020241031223605.png)


Stateless protocol (e.g., GET, PUT, DELETE)
- Each request-response exchange treated independently
- Servers not required to retain state

Still, requests are not idempotent
- Consider GET then PUT then GET vs. GET then DELETE then GET

This is good as it improves scalability on the server-side
- Don’t have to retain info across requests
- Can handle higher rate of requests
- Order of requests doesn’t matter


This is also bad as some applications need persistent state
- Need to uniquely identify user or store temporary info
- e.g., shopping cart, user preferences/profiles, usage tracking,


----

persistent : Verbindung bleibt nach anfrage bestehen 
nicht persistent : server schließt Verbinding, nachdem er Antwort empfangen habt.   Neue Request , setzen eine neuer verbidnungsaufbau 

Zustandslos bedeutet in diesem Fall, dass **der Server keine Informationen über den Client-State speichert und verwaltet**==. Stattdessen muss der Client bei jeder Anfrage bzw. bei jedem Aufruf explizit Informationen zu seinem Status mitübergeben. ==

对应的例子 
Um Informationen, wie zum Beispiel einen Warenkorb, zu speichern, kann der Server den Warenkorb in einer Datenbank mit einer eindeutigen ID speichern. Wenn der Client also den ersten Artikel zum Warenkorb hinzufügt, kann der Server eine neue Bestellung in der Datenbank anlegen, und die entsprechende ID dem Client als Antwort zusenden. Möchte der Client nun den nächsten Artikel dem Warenkorb hinzufügen, muss er die entsprechende Warenkorb ID mit übergeben, damit der Server den Artikel der Bestellung in der Datenbank hinzufügen kann. Bei jedem weiteren Aufruf wird vom Client nun immer diese ID als Zusatzinformation mitübergeben, wodurch der Server den Warenkorb bestimmen kann.


# 13 persistenten Verbindungen

Bei persistenten Verbindungen wird die einer HTTP-Anfrage zugrundeliegende ==TCP-Verbindung vom Server offen gehalten==, auch nachdem sie ausgeführt wurde. Danach kann Sie wieder verwendet werden für Folgeanfragen, um Ressourcen vom gleichen Server abzufragen (z.B. eingebettete Bilder, Skripte, Stylesheets, etc.). Dadurch spart man sich den zeitaufwändigen Verbindungsaufbau gleich mehrfach.





