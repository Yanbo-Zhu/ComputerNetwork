

# 1 Übung 1 Zeitsynchronisation


Beantworten Sie die folgenden Fragen rund um Zeitsynchronisation:

1. Wofür ist die Synchronisation von Uhren erforderlich
    1. mehrere Prozesse auf Reihefolge einigen wollen 
        1. zwei (oder mehr) Prozesse sich auf die Reihenfolge von Ergebnissen einigen können.
    2. Zugang zu einer Resource aufteilen Z.B Druck
        1. der Zugang zu eine Ressource (z.B ein Drucker) zeitlich aufgeteilt werden kann.
2. Wie ist es möglich, Uhren in einem verteilten System exakt zu synchronisieren?
    1. exakt nicht möglich , da synchronsation algorithmus  begrenzen Abweichung 
    2. Eine exakte Synchronisation ist niemals möglich. Jedoch kann die Abweichung begrenzt werden durch die periodische Nutzung von Synchronisiationsalgorithmen wie z.B Cristians Algorithmus, Berkeley Algorithmus, NTP oder PTP. 
3. Welche Alternativen zu der Synchronisation von Uhren haben Sie in der Vorlesung kennengelernt?
    1. Lamport-Uhren , Bestimmung der Reihenfolge von Ereignissen 
    2. Wenn die reale Uhrzeit keine Rolle spielt, können Lamport-Uhren verwendet werden zur Bestimmung der Reihenfolge von Ereignissen.



# 2 Übung 2 Berkeley

Für die Synchronisation von 4 verteilten Systemen A, B, C und D soll der Berkeley Algorithmus verwendet werden. 
System A hat einen Time Daemon. 


Zu Beginn des Resynchronisationsintervalls haben die Uhren die folgenden Werte:
A 11550
B 11570
C 11515
D 11525

![](image/Pasted%20image%2020241130224452.png)

1. Was schickt der Time-Daemon an die Systeme?
    1. seine eigne Uhrzeit = 11550
    2. Entweder schickt der Daemon seine eigene Zeit an alle Systeme oder aber nur eine Aufforderung die eigene Zeit zu senden. Beides ist möglich
2. Wie lauten die Antworten der Systeme?
    1.  offset gesendet to time-daemon server
    2. Je nach Umsetzung wird als Antwort der Offset zur empfangenen Zeit geschickt oder nur die eigene Uhrzeit.
    3. B 发送的是 +20
3. Der Time-Daemon verwendet die Mittelwertbildung zur Ermittlung der Uhrzeit. Welchen Wert ermittelt er?
    1. Der Daemon bildet das arithmetische Mittel aller Uhrzeiten inklusive dereigenen Zeit.
    2. average value =  =  (11550 + 11570 + 11515 + 11525) /4 = 111540 
5. Welche Werte werden zurück an die Systeme gesendet?
    1. Der Daemon sendet den Offset zur ermittelten Durchschnittsuhrzeit an jedes System zurück.
    2. Time Daemon server sendet berechneten Offset Z.B  B 得到 30 的矫正,   然后 B 的 uhrzeit 变为 11570 -30 = 11540


# 3 Übung 3 Christians Algorithmus

In einem verteilten System kommt Cristians Algorithmus zum Einsatz, um die Uhren zu synchronisieren. Zu seiner Uhrzeit 10:27:54,0 (Stunden:Minuten:Sekunden) fragt System B bei einen Zeit-Server A nach der Zeit. 

`,0`  is milisecond 

Um 10:28:01,0 Uhr seiner Zeit empfängt B die Antwort von A mit dem Zeitstempel 10:27:37,5.

![](image/Pasted%20image%2020241130224406.png)

1. Was ist die Round-trip time (RTT) zwischen B und A?
    1.  ( 10:28:01,0  - 10:27:54,0 )  =  7 second
    2. on way delay  = RTT /2  = 3,5 s
2. Kann man davon ausgehen, dass diese RTT symmetrisch ist?
    1. Symmetrisch :   to and zruck weg 好事一样
    2. in Realitat, es ist nicht  Symmetrisch.. Aber in Christians Algotithmuss , wir nehmen symmetrisch an 
    3. Generell kann man nicht davon ausgehen, da Hin- und Rückweg der Pakete nicht unbedingt gleich sein müssen, oder bspw. ein Paket evtl. länger in den Puffern von Router liegt wenn das Netzwerk ausgelastet ist. Cristians Algorithmus macht diese Annahme trotzdem.
3. Was ist B’s Schätzung der Zeit von A?
    1. 就是在 b 收到 response 的时候,  计算 这个时候a 的 time system 中 a 在这个时刻 a 的时间 10:27:37,5  + 3,5 s = 10:27:41,0
4. Was ist B’s Offset in Bezug auf die Zeit von A?
    1. offset  =  10:27:41,0 -  10:28:01,0 =  - 20 s
5. Geht die Uhr von B zu schnell oder zu langsam?
    1. 根据 - 20 s,  得到 die Zeit von B zu schnell , A is zu langsam 
6. Angenommen die Zeit von B läuft zu schnell, was muss bei der Anpassung an die Zeit von A beachtet werden?
    1. zurucksetzen hat keine Fall , kene Keine Sinn
    2. B muss seine Zeit verlangsam
    3. A ist Zeit-Server.  deshalb darf es nicht, die Zeit von A muss sich beschleunigen 
    4. Die Uhrzeit von B darf nicht zurückgesetzt werden, da sonst die Ereignisreihenfolge nicht erhalten wird (siehe Geschichte der Zwillingsbrüder). Stattdessen muss die Uhr von B verlangsamt werden bis der gewünschte Offset erreicht wird.


# 4 Übung 4 NTP-Offset*

Erläutern Sie (unterstützt durch eine Skizze) die Berechnung des Offsets zwischen zwei das Network Time Protocol (NTP) verwendenden Servern. 
Was ist jeweils der Hintergrund (z.B. die Motivation, der Ansatz oder die Annahme) Delay und Offset so zu berechnen?



This calculation uses the premise 前提，假设 that transport is symmetric (same delay in both directions) -> in principle the timeline must be an equilateral triangle
Many packet pairs and many sources are averaged over a long time ->  accuracy increases

- NTP client 和 NTP Server 之间如何同步时间 
    - NTP client will regularly poll one or more NTP servers
        - Exchange of several packet pairs (request, reply)
        - Containing originate resp. receive timestamp
    - To synchronize its clock, the client must compute its time offset and round-trip delay (travelling time)

![](image/Pasted%20image%2020241130230057.png)

- T1..T4 sind jeweils die Zeitstempel gemäß der lokalen Uhren
- Delayberechnung: RTT ist die Gesamtverarbeitungszeit (T4 − T1) abzüglich der Verarbeitungszeit auf dem Server (T3 − T2).
- Offsetberechnung: Der Offset ist, per Definition, der Wert der auf die Uhr des Clients addiert werden muss, um die Zeit am Server zu erhalten. Angenommen, das Delay ist symmetrisch (d.h. beide Richtungen benötigen die gleiche Zeit), gilt:

![](image/Pasted%20image%2020250119133309.png)


# 5 Übung 5 Logische Uhren*

Gegeben sei der unten stehende Datenaustausch zwischen den Prozessen P1, P2, P3 und P4. Die Prozesse benutzen jeweils logische Uhren. Diese seien jeweils initial mit Null initialisiert; die gestrichelten Pfeile repräsentieren den Nachrichtenaustausch zwischen Komponenten.

![](image/Pasted%20image%2020250114091442.png)

Nehmen Sie an, dass die Prozesse Lamport-Uhren benutzen, um sich zu synchronisieren. Geben Sie den Lamport-Zeitstempel für jedes Event im Beispiel. Nehmen Sie an, dass jeder Prozess einen Integer als Lamport-Uhr benutzt.

![](image/Pasted%20image%2020250119133323.png)

# 6 Übung 6 NTP

In einem verteilten System kommt NTP zum Einsatz, um die Uhren zu synchronisieren.
1. Wie viele Zeitstempel verwendet NTP? Was ist der Unterschied zum Cristians Algorithmus?
    1. NTP verwendet insgesamt 4 Zeitstempel, wovon 2 auf Client und 2 auf dem Server erfasst werden. 
    2. Damit soll die Verzögerung durch die Verarbeitung auf dem Server besser approximiert werden. 
    3. Außerdem wird der Netzwerkdelay über mehrere Messungen zu verschiedenen Servern ermittelt und der Server mit dem stabilsten Delay wird ausgewählt
2. Der Server ist ein Stratum 1 Server. Was bedeutet das?
    1. Ein Stratum 1 Server ist ein Computer der direkt mit einer Zeitquelle verbunden ist (ohne Netzwerk dazwischen). Stratum 0 ist die Zeitquelle selbst bspw. eine Atomuhr, aber auch eine Funkuhr
    2. ![](image/Pasted%20image%2020250119134756.png)
3. Leiten Sie anhand der Zeitstempel t1, t2, t3 und t4 eine allgemeine Formel für die Round-trip time (RTT) (Vorlesung: Delay δ h) er. Warum fällt der Offset zwischen den Uhren von Client und Server nicht ins Gewicht?
    1. RTT  =  (T4 -T1) - (T3 - T2)
    2. Da nur die Abstände zwischen jeweils lokalen Zeitpunkten der selben Uhr gemessen wird, ist der Offset der Uhren zu einander irrelevant.

