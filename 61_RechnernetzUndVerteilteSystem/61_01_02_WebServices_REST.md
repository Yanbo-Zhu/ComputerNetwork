
"Representation State Transfer is intended to evoke an image of how a well-designed Web application behaves: a network of web pages (a virtual state-machine), where the user progresses through an application by selecting links (state transitions), resulting in the next page (representing the next state of the application) being transferred to the user and rendered for their use."


- **_Representational States Transfer_** (**_REST_**) ist ein Paradigma für die Gestaltung von Web-basierten **_Programmierschnittstellen_** (**_Application Programming Interfaces_, _APIs_**), vornehmlich für die Maschine-zu-Maschine-Kommunikation
- Alternative zu anderen Web-Service-Technologien wie SOAP und XML-Remote-Procedure-Call
- Webdienste, die dem REST-Paradigma folgen, werden als **_RESTful_** bezeichnet

Rest 的优势:
- Rest quest , weg von http format sein, 不需要像 restless service 一样, 不需要写httpp boday 中的 格式和内容, 
- 只需要 在 uri 中写上想要拿到的资源就好 

![](images/Pasted%20image%2020250124192320.png)


- Dienstanfragen und -antworten basieren auf der Repräsentation von Ressourcen
- Ein **_Ressource_** ist jede Art von kohärenter und sinnvoller Information (z.B. eine Liste oder Elemente einer Liste), die über eine Adresse (URI) zur Verfügung gestellt werden kann
- Eine **_Repräsentation_** ist die Darstellungsform einer Ressource (z.B. HTML, XML oder JSON)
- Eine REST-konformer Server kann je nach Anforderung verschiedene Repräsentationen einer Ressource ausliefern
- Veränderungen von Ressourcen erfolgen i.d.R. über eine Repräsentation
- Jede Ressource verfügt über eine eigene, dedizierte Adresse in Form eines Uniform Resource Locators (URL)
- RESTful Services definieren keine eigenen Methoden und Operationen in ihrer API, sondern verwenden die _**Verben**_ von HTTP



# 1 Ansatz

- Dienstanfragen und -antworten basieren auf der Repräsentation von Ressourcen
- Eine _**Ressource**_ ist jede Art von kohärenter und sinnvoller Information (z.B. eine Liste oder Elemente einer Liste), die über eine URI zur Verfügung gestellt werden kann
- Eine Repräsentation ist die Darstellungsform einer Ressource (z.B. HTML, XML oder JSON)


# 2 Representational State Transfer (REST)


- An alternative to SOAP/WSDL-based Web services
- Set of architectural principles by which one can design Web services that focus on a system's resources, including ==how resource states are addressed and transferred over HTTP==
- Service access by direct use of HTTP methods in accordance to the semantics defined in RFC 2616.
- One-to-one mapping between create, read, update, and delete (CRUD) operations and HTTP methods
    - To create a resource on the server (append), use HTTP POST method
    - To retrieve a resource, use HTTP GET method
    - To change the state of a resource or to replace, use HTTP PUT method
    - To remove or delete a resource, use HTTP DELETE method


REST
- REST is an architectural style for distributed systems.
- REST defines a series of constraints for distributed systems that together achieve the properties of:
    - Simplicity, scalability, modifiable, performance, visibility (to monitoring), portability and reliability.
- A system that exhibits all defined constraints is RESTful!


# 3 Understanding REST

Representational State Transfer:
-  The Resource:
    - A resource is any information that can be named: documents, images, services, people, and collections.
- Resources have state:
    - State may change over time.
- Resources have identifiers:
    - A resource is anything important enough to be referenced.
- Resources expose a uniform interface:
    - System architecture simplified, visibility improved,
    - Encourages independent evolution of implementations.

- On request, a resource may transfer a representation of its state to a client:
    - Necessitates a client-server architecture.
- A client may transfer a proposed representation to a resource:
    - Manipulation of resources through representations.
- Representations returned from the server should link to additional application state:
    - Clients may follow a proposed link and assume a new state Hypermedia as the engine of application state.


 - Stateless interactions:
     - Each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server.
     - Stateless 就是说不会记得之前的 session 的状态 
- Statelessness necessitates self-descriptive messages:
    - Standard media types,
    - Meta-data and control-data.
 - Uniform interface + Stateless + Self-descriptive = Cacheable
     - Cacheable necessitates a layered-system.



## 3.1 Stateful vs. Stateless Design

![](image/Pasted%20image%2020241110131417.png)

- Stateful design: 
    - ==Server keeps Client-related state== (previousPage)
- Stateless design: 
    - ==the Client has to include explicit state information in the call, the Server does not keep any client-related state==




## 3.2 Uniform Interface

- The semantics of the methods is constant and independent from the particular resource that is being addressed
- Evolution of services goes via new resources rather than calls
- Simplifies integration of services from different providers (service mashing)
- Simplifies error recovery by end systems and intermediate systems (caches) who can repeat idempotent calls automatically without having access / understanding of the “payload”
- Accompanied by a set of “Unified Status Codes”

![](image/Pasted%20image%2020241110131717.png)



Unified Status Codes
![](image/Pasted%20image%2020241110131751.png)


## 3.3 Narrow interface service design

就是设立一定的规则, 将 interface 的设计规则都确定好 , 

The usage of just the HTTP methods in REST is an example of a more general class of service designs based on “narrow” interfaces 

Narrow interface service design
- Limited set of common methods / procedures / functions
- Services defined through the resources / arguments passed to the narrow method interface
- Service design is focused on the resources, not on the functions


![](image/Pasted%20image%2020241110132020.png)



# 4 Restless versus Restful

![](images/Pasted%20image%2020250124190126.png)

- Wiederholung von Kapitel 1: Ein **_Webdienst_** ist ein Programm, das über das Internet oder ein Intranet anderen Programmen Funktionalitäten zur Verfügung stellt 
- Ein **_Dienstkonsument_** ist die Software, die einen Dienst aufruft
- Eine **_API_** (**_Application Programming Interface_**, **_Programmierschnittstelle_**) ist die syntaktische Spezifikation der Operationen und ihrer Parameter, die ein Dienst anbietet
- Ein **_Dienstzugangspunkt_** (**_Endpunkt_**) definiert das Protokoll für den Nachrichtenaustausch und die Adresse, unter welcher der Dienst mit seiner API erreichbar ist
- Beispiele für Dienstzugangspunkte
    - HTTP und URI
    - TCP und IP-Adresse
    - SMS und Mobiltelefonnummer

Restlease API 是通过一个统一的 Dienst-API  捉取资源
Restful API, 是将 想要捉取的资源的信息 写在uri中, 然后通过 uri 直接捉取对应的资源 




## 4.1 Beispiel eines Restless-Dienstes


- Gesamte API wird über eine JURL als Endpunkt angesprochen
- Anfragen werden immer mittels HTTP POST gesendet
    - 因为 post 方法, 在 http body 才可以写入信息 , get 则不行 
    - Restful Service 用 get 是因为 urI 中直接写入 daten的信息了. 
- HTTP-Body enthält aufgerufene Methode und Parameter

---
往 某个 api (就是统一的 DienstAPI)  发送 Post 请求,   
- 请求中的 service 为 "customer.get" 或者 filter 为 customer_id: 5376 
- 请求中的 service 为 customer.create 

![](images/Pasted%20image%2020250124191812.png)

![](images/Pasted%20image%2020250124191834.png)


# 5 Wiederverwendung von HTTP-Methoden
 
![](images/Pasted%20image%2020250124193116.png)



# 6 Rest-Parameter

![](images/Pasted%20image%2020250124193224.png)

Path-Parameter
- Platzhalter für den variablen Teil eines URL-Pfades
- Zeigt auf eine bestimmte Ressource in einer Kollektion von Ressourcen
- Wird durch einen Wert ersetzt, wenn der Client einen Request an die API sendet


Header-Parameter
- Werden im Header einer HTTP-Anfrage transportiert
- Werden zu Kontroll- und Steuerzwecken verwendet, z.B. Authentifizierung, Mitteilung über die URL einer Ressource, Anzeige von Inhaltsformaten, usw.


Query-Parameter
- 就是 get xx 
- Schlüssel/Wertpaare, die am Ende einer URL mit einem Fragezeichen angehängt werden
- Werden üblicherweise verwendet, um Filter- oder Suchkriterien auf einer mit GET angefragten Kollektion von Ressourcen zu definieren


# 7 Ressource-Typen und Enthaltenseinsbeziehungen (I/II)

![](images/Pasted%20image%2020250124193511.png)


- Ein **_RESTful Service_** ist eine Kollektion von Ressourcen
- Eine **_Ressource_** ist ein kohärenter Datensatz basierend auf einem abstrakten Datenmodell, der im Kontext der Webanwendung von Relevanz ist
- Ein **_Ressourcen-Typ_** ist eine Kollektion von Ressourcen, die auf demselben Datenmodell basieren, d.h. Ressourcen desselben Typs werden durch dieselben Attribute beschrieben, aber unterscheiden sich durch die Attributwerte voneinander

- Eine Ressource kann ein oder mehrere **_Unterressourcen_** oder **_Kindressourcen_** (_**Sub Resources**, **Child Resources**_) enthalten
- Eine Ressource kann keine oder eine übergeordnete **_Superressource_** oder **_Elternressource_** (**_Super Resource_**) haben
- Kindressourcen und ihre Elternressource begründen eine **_Enthaltenseinsbeziehung_**


URL-Bildung anhand von Enthaltenseinsbeziehungen (I/II)
- Eine RESTful-Dienst hat keinen einzelnen Endpunkt, stattdessen verfügt jede Ressource über einen eigenen, durch eine URL spezifizierten Endpunkt
- Die URL setzt sich aus der Domain des RESTful-Dienstes und dem Pfad der Ressource zusammen
- Der Pfad der URL setzt sich aus den Namen der Ressource und ihrer Superressourcen zusammen

Beispiele
- `**/donaldduck/contacts/**` repräsentiert die Ressource der Kontakte des Nutzers Donald Duck
- `**/donaldduck/contacts/daisyduck**` repräsentiert einen einzelnen Kontakt des Nutzers Donald Duck 
- `**/donaldduck/timeline/3451895437**` repräsentiert eine Nachricht in der Timeline des Nutzers Donald Duck

![](images/Pasted%20image%2020250202191728.png)


# 8 Repräsentationen 


- Eine **_Repräsentation_** oder Darstellung ist eine serialisierte Ressource, d.h. eine Zeichenkette basierend auf einem Darstellungsformat
- Jede Ressource eines RESTful-Dienstes kann verschiedene Darstellungen haben
- Ein **_Darstellungsformat_** definiert die Syntax einer Darstellung
- Übliche Darstellungsformate: **_Java Script Object Notation_** (**_JSON_**) and **_eXtended Markup Language_** (**_XML_**)

Eine Ressource in JSON-Repräsentation
![](images/Pasted%20image%2020250202191851.png)

Eine Ressource in XML-Repräsentation
![](images/Pasted%20image%2020250202191904.png)


# 9 Verwendung von HTTP-Verben

- RESTful-Dienste basieren auf HTTP und verwenden HTTP-Methoden als Standardvokabular der API (siehe Tabelle)
- Beachte: REST ist nicht auf HTTP beschränkt, sondern kann auch mit anderen Protokollen wie **_CoAP_** (**_Constrained Application Protocol_**) genutzt werden
- HTTP-Methoden, die keine Daten verändern, werden als **_sicher_** (**_safe_**) bezeichnet
- Sichere Methoden: `**GET**` und `**HEAD**`
- HTTP-Methoden, die bei wiederholten Aufruf (mit den gleichen Parametern) das gleiche Ergebnis erzielen, werden als **_idempotent_** bezeichnet 
- Idempotente Methoden: `**GET**`, `**PUT**` und `**DELETE**`

| Anfragemethode | CRUD-Operation | Beschreibung                                                                                                                                                                                                                                                                              |
| -------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `**GET**`      | Read           | Fordert die angegebene Ressource vom Server an. Der Body der Anfrage ist leer.                                                                                                                                                                                                            |
| `**POST**`     | Create         | Fügt eine neue Ressource unterhalb einer angegebenen Ressource ein. Da die neue Methode noch keine URL besitzt, wird die URL der Elternressource angegeben. Als Ergebnis der Operation wird die URL der neu angelegten Ressource zurückgegeben. Der HTTP-Body enthält die neue Ressource. |
| `**PUT**`      | Update         | Die durch die URL spezifizierte Ressource wird mit den Daten aus dem Rumpf aktualisiert oder es wird eine neue Ressource mit den Daten aus dem Rumpf angelegt, falls die angegeben URL nicht existiert. Der HTTP-Body enthält die aktualisierte Ressource.                                |
| `**DELETE**`   | Delete         | Löschen einer Resource, keine Daten im Body einer Anfrage. Der HTTP-Body ist leer.                                                                                                                                                                                                        |


PUT versus POST
Put: 更新原有的 
Post: 之前有的不更新, 而是 insert 一个新的 

![](images/Pasted%20image%2020250202192415.png)

| Anfragemethode               | Beschreibung                                                                                                                                                                                                            |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `**PUT /users**`             | Eine Ressource, welche eine Menge von Nutzern repräsentiert, wird mit den Daten im HTTP-Body aktualisiert, wenn die Ressource bereits existiert oder unter `**/users**` angelegt wenn sie nicht existiert               |
| `**PUT /users/donaldduck**`  | Eine Ressource, die den Nutzer Donald Duck repräsentiert, wird mit den Daten im HTTP-Body aktualisiert oder unter `**/users/donaldduck**` neu angelegt                                                                  |
| `**POST /users/**`           | Legt eine neue Ressource unter der Elternressource `**/users**` mit den Daten aus dem HTTP-Body an. Der Name der Ressource wird von dem aufrufenden Client im `**Location**`-Header-Feld der Antwort mitgeteilt.        |
| `**POST /users/donaldduck**` | Legt eine neue Ressource unter der Elternressource `**/users/donaldduck**` mit den Daten aus dem HTTP-Body an. Der Name der Ressource wird dem aufrufenden Client im `**Location**`-Header-Feld der Antwort mitgeteilt. |


# 10 State Transfer


Application State  (Client 端的)
- Der **_Zustand einer Anwendung_** (**_Application State_**) korrespondiert mit der im im Client geladenen Ressource (entsprich im WWW der geladenen Webseite)
- Zustandsübergänge entsprechen Verweisen denen der Client folgt, um weitere Ressourcen zu laden
![](images/Pasted%20image%2020250202193431.png)



Resource State  (Server 端的)
- Der **_Ressourcen-Zustand_** (**_Resource State_**) entspricht dem Zustand, den Daten oder dem Wert aller Ressourcen eines Webservers
- Zustandsübergänge ergeben sich durch die Aktualisierung oder die Löschung von existierenden Ressourcen sowie die Einrichtung neuer Ressourcen

![](images/Pasted%20image%2020250202193620.png)

---


State Transfer
- Der Zustand der Anwendung liegt im Client - Der Server kann durch Senden von Repräsentationen Zustandsübergänge auslösen und den Zustand der Anwendung beeinflussen
- Der Ressourcen-Zustand liegt im Server - Der Client kann durch Senden durch Repräsentationen den Zustand beeinflussen


