

# 1 

Angenommen Sie möchten ein Programm schreiben, das eine Funktion auf einem entfernten Server per Socket-API aufruft und dabei als Argument ein Paar aus ID und Namen übergibt. Das Programm speichert das Paar intern wie hier dargestellt:

```c
typedef struct ValueType {
 int id;
 const char* name;
} ValueType;
```


1. Kann der dargestellte struct ValueType direkt an send() übergeben werden? Spielt die Prozessorarchitektur hierbei eine Rolle?
    1.  Nein, da sonst einfach der Pointer name übertragen werden würde, anstatt der eigentliche Wert. Es muss also ein sog. marshalling des Datentyps stattfinden, also ein überführen  转变 des Datentyps in eine Folge von Bytes. Dafür muss vorher ein Format definiert werden, was beiden Systemen bekannt ist. Dabei muss insbesondere auf die Endianness von Integern geachtet werden, also in welcher Reihenfolge die einzelnen Bytes einer Zahl übertragen werden.
    2. No, it cannot be directly passed to `send()`. If you attempt to do so, only the pointer (e.g., `name`) would be transmitted, rather than the actual value. Therefore, a process called **marshalling** **封送处理** must occur, which involves converting the data type into a sequence of bytes. 
    3. For this to work, a format must be defined in advance that is understood by both systems involved in the communication. Special attention must be paid to the **endianness** 字节存储顺序 of integers—this determines the order in which the bytes of a number are transmitted. For instance, systems might use **little-endian** (least significant byte first) or **big-endian** (most significant byte first), and the difference must be accounted for during serialization to ensure compatibility between systems with different processor architectures.
2. Funktionsaufrufe auf entfernten Systemen werden oft als sog. Web Services über HTTP ausgeführt (siehe REST). Wie löst HTTP die Probleme aus der ersten Teilaufgabe?
    1. HTTP ist ein textbasiertes Protokoll. Zahlen werden also als Text übetragen. Allerdings gibt es auch hier verschiedene Möglichkeiten Zeichen zu enkodieren (UTF-8, ASCII, etc.).  Welcher Zeichensatz verwendet wird, legt der Server fest, wobei der Client mitteilt, welche Zeichensätze er akzeptiert.
3. Wie können komplexe Datenstrukturen übertragen werden, bspw. verkettete Listen oder Binärbäume?
    1. Für komplexe Datenstrukturen muss auch ein Marshalling-Format **封送处理** festgelegt werden. Dabei muss die Datenstruktur in eine flache Darstellung überführt 转变 werden und ggf. Metadaten hinzugefügt werden (z.B. Referenzen auf andere Elemente),  damit die Struktur wieder rekonstruiert werden kann im entfernten System.


