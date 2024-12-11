

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


- GET: ruft Daten vom Server ab. (Beispiel: Abrufen einer Webseite)
- POST: sendet Daten an den Server, z. B. um eine neue Ressource zu erstellen. (Beispiel: Absenden eines Formulars)
- PUT: aktualisiert oder erstellt eine Ressource. (Beispiel: Hochladen oder Ersetzen von Daten)
- DELETE: löscht eine Ressource. (Beispiel: Entfernen eines Datensatzes)
- HEAD: holt nur die Header-Informationen der Ressource. (Beispiel: Prüfen, ob eine Datei existiert)
- OPTIONS: fragt die unterstützten HTTP-Methoden des Servers ab.
- PATCH: Teilaktualisierung einer Ressource.

- `GET`：请求资源数据，通常只读。
- `POST`：提交数据，创建资源或执行某些处理。
- `HEAD`：类似 GET，但只获取响应头。
- `PUT`：更新或创建资源，发送完整的资源信息。

POST method:
- 可以在原来的数据的后面新加入数据, 不为 idempotent
- web page often includes form input
- user input sent from client to server in entity body of HTTP POST request message

GET method (for sending data to server):
include user data in URL field of HTTP GET request message (following a '?'):
www. some site . com/animalsearch?monkeys &banana

HEAD method:
requests headers (only) that would be returned if specified URL were requested with an HTTP GET method.

PUT method:
- 彻底覆盖原来的数据, 为 idempotent
- uploads new file (object) to server
- completely replaces file that exists at specified URL with content in entity body of POST HTTP request message


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




# 7 Repsone Code/ Status Code 
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


# 8 Cookies 

为什么需要 Cookies: HTTP GET/response interaction is stateless
> Web sites and client browser use cookies to maintain some state between transactions


Client-side state maintenance
- Client stores small (?) state on behalf of server
- Client sends state in future requests to the server
Can provide authentication


four components:
1) cookie header line of HTTP response message
2) cookie header line in next HTTP request message
3) cookie file kept on user’s host, managed by user’s browser
4) back-end database at Web site

![](image/Pasted%20image%2020241021103143.png)


What cookies can be used for: Cookies 的用途 
▪ authorization
▪ shopping carts
▪ recommendations
▪ user session state (Web e-mail)



![](image/Pasted%20image%2020241031224142.png)




# 9 Web Caches (Proxy Servers)

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



# 10 HTTP/HTTPS协议

![](image/Pasted%20image%2020240216165526.png)

![](image/Pasted%20image%2020240216170154.png)


# 11 HTTP is Stateless

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
