Die CPU ist ein komplexer integrierter Schaltkreis, der aus bis zu mehreren Millionen Transistoren besteht, welche mit fotolithografischen Verfahren auf Siliziumscheiben aufgebracht werden.
Der CPU wird auf der Hauptplatine (Motherboard) in einem Sockel aufgesteckt. Jedoch nicht jede CPU passt auf jeden Sockel.
# Aufbau 
>[!note]
>Multicore-Prozessoren haben diese Komponenten teilweise je einmal pro Kern.

**Arithmetic-Logical Unit (ALU)**
- führt mathematische Operationen und logische Verknüpfungen durch
- moderne Prozessoren haben getrennte ALUs für ganzzahlige Operationen und Fließkommaoperationen (*Floating Point Unit*)

**Register**
- spezialisierte Speicherzellen im Prozessorkern
- Werte mit der die ALU rechnet werden hier abgelegt
 
**Steuerwerk (Control Unit)**
- kontrolliert die Ausführung von Programmen und initiiert andere Steuerungsfunktionen
- Der *Befehlszeiger* ist ein spezielles Register das auf die Speicheradresse des nächsten Programmbefehls zeigt, bei einem Sprung im Programm muss der Befehlszeiger auf die richtige neue Adresse gesetzt werden
- Der *Stack Pointer* ist ein weiteres Register, das auf die aktuelle Position auf dem Stack zeigt 

**Befehlstabelle (Instruction Table)**
- ermöglicht die Decodierung von Maschinenbefehlen; jeder Befehl hat einen bestimmten numerischen Wert, je nach Befehlsnummer werden verschiedene Schaltungen aktiviert, die ein bestimmtes Verhalten der CPU auslösen

**Busse (Datenleitungen)**
- verbindet die CPU mit anderen Komponenten des Motherboard, gehört jedoch nicht zum Prozessorkern und ist unabhängig von der Kernanzahl nur einmal vorhanden
- Der *Datenbus* dient zum Austausch von Daten mit dem Arbeitsspeicher
- Der *Adressbus* überträgt die Speicheradressen von Daten
- Der *Steuerbus* steuert die Peripherieanschlüsse

**Arbeitsspeicher - Random Access Memory (RAM)**
Der [[Arbeitsspeicher]] wird zur Speicherung von Programmen, die ausgeführt werden, und den Daten die von ihnen verarbeitet werden, benutzt.
Früher wurden der RAM mit derselben Taktfrequenz wie das motherboard betrieben. Bei modernen PCs jedoch mit einem vielfachen des motherboards.

## Cache
Caches werden als kleine, aber sehr schnelle Zwischenspeicher benutzt, in denen Daten oder Befehle gespeichert werden können um sie kurz danach zu benutzen. Sie werden verwendet um den großen Unterschied der Taktfrequenz von Arbeitsspeicher und CPU zu überbrücken.
Programmierer haben keinen großen Einfluss, welche Daten im Cache gespeichert werden, diese Entscheidung wird von der *Sprungvorhersage (Branch Prediction)* gesteuert. Der Prozessor berechnet während der Ausführung von Programmen wohin der nächste Sprung im Programm erfolgt und entscheidet darauf welche Daten oder Programmteile im Cache gespeichert werden.
 
**Level-1-Cache**
Der L1 Cache befindet sich im Prozessorkern und hat dieselbe Taktrate wie die CPU. Mehrkern-CPUs haben jeweils einen Level-1-Cache pro Kern. Die Größe des L1-Cahes befindet sich im KiB Bereich bis 128 KiB und wird vom Prozessor vornehmlich bei der Ausführung sehr kurzer Schleifen aus wenigen Befehlen.

**Level-2-Cache**
Ursprünglich war der L2-Cache außerhalb des Prozessorgehäuse auf dem motherboard, seit dem Pentium 2 befindet er sich im Prozessorinneren, ist jedoch kein Teil des Kerns. Ein Prozessor hat unabhängig von der Kernanzahl einen L2-Cache. Der L2-Cache hat eine vielfach höhere Taktrate als der Arbeitsspeicher, jedoch mit einer geringeren als die CPU betrieben. Im Vergleich mit dem L1-Cache ist der  L2-Cache größer, mit bis zu 1.024 KiB.  

**Level-3-Cache**
Die meisten modernen Prozessoren verfügen über einen L3-Cache, der weiter entfernt vom Prozessor ist, langsamer arbeitet als ein L2 Cache und einen Speicher im MiB Bereich, von bis zu 16 MiB, hat.
# Leistungsmerkmale
>[!note] Moores Law
>Als Maßstab für Prozessoren und integrierte Schaltkreise gilt *Moores Law*, benannt nach Gordon Moore einer der Gründer von Intel, laut dem verdoppelt sich die Integrationsdichte von IC (integrated circuits) alle 18 bis 24 Monate. Dies hat bedeutet eine höhere Leistungsfähigkeit für die Prozessoren.

## Wortbreite
Das wichtigste Merkmal für die Leistungsfähigkeit eines Mikroprozessors ist die Wortbreite, welche angibt aus wie viele Bits ein Maschinenwort des Prozessors besteht. Je breiter ein Maschinenwort ist, desto mehr unterschiedliche Zustände oder Werte kann der Prozessor in einem Durchgang bearbeiten, dies beeinflusst unter anderem die Größe des direkt addressierbaren Arbeitsspeicher, die Größe von Ganzzahlen und die Genauigkeit von Fließkommazahlen.
Die Wortbreite hat folgenden Einfluss auf die Komponenten:
- **Register**
  Die Wortbreite betrifft die Rechenfähigkeit der ALU, genauer die mögliche Größe von Ganzzahlen und die Genauigkeit von Fließkommawerten.
- **Datenbus**
  Die Breite des Datenbusses bestimmt wie viele Bits gleichzeitig aus dem Arbeitsspeicher gelesen oder in ihn geschrieben werden können. Dieser Wert betrifft direkt den Datenaustausch mit Programmen und wird deshalb als Wertangabe für den Prozessor selbst verwendet.
- **Adressbus**
  Die Breite des Adressbusses regelt die maximale Größe der Speicheradressen und bestimmt wie viel Arbeitsspeicher ein Prozessor adressieren kann.
- **Steuerbus**
  Die Breite des Steuerbusses bestimmt mit welchen Peripherieanschlüssen der Prozessor umgehen kann. 
  Die Einführung der 32-Bit-Prozessoren ermöglichte die Entwicklung von Schnittstellen wie PCI und AGP.

## Taktfrequenz
Die Taktfrequenz (*Clock Rate*) gibt an wie viele Takte (Zyklen) pro Sekunde die CPU ausführt. Während jedem Takt kann die CPU eine Anweisung verarbeiten, worauf man schließt wie viele Befehle oder wie schnell Befehle von der CPU bearbeitet werden können.
Die Taktfrequenz wird nicht von der CPU sondern mit einer Steckbrücke (*Jumper*) oder DIP-Schalter auf dem Motherboard bestimmt, indem ein Multiplikator eingestellt wird der die Taktfrequenz des Motherboards (*Front Side Bus Clock Rate*) multipliziert und der CPU zuweist.
>[!Beispiel]
>Das Motherboard ist mit 133 MHz getaktet und der Multiplikator ist auf 20 eingestellt, dies führt zu einer CPU Taktrate von 2,66 GHz.

>[!warning]
>Jeder Prozessor hat eine vom Hersteller angegeben maximale Taktfrequenz, es ist möglich den Prozessor über diesen Wert zu takten (*overclocking*), führt aber zu einem erhöhten Verschleiß.

Die Taktfrequenz sagt weniger über die Leistung einer CPU aus, als manche meinen. 
- Je nach Komplexität der Befehle dauert deren Ausführung mehrere Taktzyklen
- Prozessoren verbringen einen Großteil der Zeit mit warten, da die erforderlichen Daten nicht schnell genug übertragen werden können (z.B. von Ein-und Ausgabe)
- unterschiedliche Prozessorfamilien bearbeiten verschiedene Probleme nicht mit der gleichen Effizienz.

Die tatsächliche Effizienz von CPUs lässt sich besser bestimmen durch:
- **Million Instructions per Second (MIPS)**
  Die Anzahl von Befehlen die in einer Sekunde ausgeführt werden können. 
  Diese werden durch Benchmark-Test mit Hersteller unabhängigen Benchmark-Programmen ermittelt.
- **Floating Point Operations per Second (FLOPS)**
  Die Anzahl von durchführbaren Fließkommaoperationen pro Sekunde. 
  Wichtig für 3D-Grafik, Video- und Audioperformance. 
# Architektur
## Complex Instruction Set Computer (CISC)
*CISC* versuchen komplexe Anweisungen durch einzelne Prozessorinstruktionen auszuführen, haben also einen komplexen Befehlssatz.
Prozessoren von Intel und AMD sind die Hauptvertreter der CISC-Architektur, jedoch wurden auch RISC-artige Aspekte in die Prozessoren integriert um die Effizienz zu verbessern. So werden die komplexen CISC-Befehle in einzelne RISC-artige Mikroinstruktionen zerlegt und anschließend vom Prozessorkern mit höherer Effizienz ausgeführt. 
## Reduced Instruction Set Computer (RISC)
*RISC* versuchen die Struktur des Prozessors zu vereinfachen, indem der Befehlssatz auf wenige schnell und einfach auszuführende Befehle reduziert werden. Komplexe Anweisungen können durch mehrere einfache Befehle ausgeführt werden. Außerdem lassen sich die Befehle in effiziente *Pipelines* (Warteschlangen) anordnen.  
Ein Nachteil der RISC-Architektur ist, dass die CPU einen größeren Arbeitsspeicher benötigt, da ein Maschinenprogramm aus mehreren Einzelbefehlen besteht und diese mehr Platz brauchen.
# Arbeitsweise
Schematisch arbeitet der Prozessor bei der Ausführung eines Programms folgt:
1. Der aktuelle Befehl wird aus dem Programm gelesen und die Stelle wird durch den Befehlszeiger des Prozessors angezeigt
2. Der Prozessor liest den Befehl aus der Befehlstabelle und nimmt die passende Anzahl darauffolgender Bytes als Parameter; der Befehlszeiger rückt hinter das letzte Parameter-Byte um den nächsten Befehl zu lesen
3. Der Befehl wird ausgeführt; hier kann es zum Lesen von Daten aus dem Arbeitsspeicher, der Ansteuerung von Peripherieschnittstellen, Rechnen in der ALU oder die Durchführung eines Sprungs kommen.
4. Falls ein Sprung stattfindet wird der Befehlszeiger an die angegebene Stelle gesetzt, ansonsten geht es mit dem nächsten Befehl weiter.

## Sprünge
Es gibt zwei Arten von Sprüngen:
- *unbedingte Sprünge*
Der Sprung wird immer ausgeführt sobald der Befehl gelesen wird
- *bedingter Sprung*
Der Sprung wird nur ausgeführt wenn eine bestimmte Bedingung erfüllt wird. Diese Bedingung wird mit dem Zustand eines *Flag-Register*, ein Status-Bit das durch Vergleiche, Fehler oder direkte Manipulation gesetzt werden, angezeigt. 

>[!note]
>Beim Sprung in ein Unterprogramm, wird die auf den Sprungbefehl folgende Adresse auf dem *Stack* gespeichert, um nach der Ausführung des Programms an die Richtige Stelle im Programm zurück zu springen. Der Stack ist ein Stapelspeicher der nach dem Last-in-first-out Prinzip, das heißt das die zuletzt hinzugefügte Adresse im Stack beim erfolgen des Rücksprungbefehls ausgelesen wird. Der aktuelle Abschluß des Stack wird von von einem speziellen Steuerregister, dem *Stack Pointer*, angezeigt.
>
## Interrupts
*Interrupts* werden bei Ein- und Ausgabevorgängen benutzt, da die Anfragen der Hardware an den Prozessor asynchron erfolgen, muss der Prozessor die Hardware in regelmäßigen Intervallen abfragen und das laufende Programm unterbrechen.

# Maschinenbefehle
Maschinenbefehle sind einfache Befehle die den Maschineninstruktionen und der Maschinensprache sehr nahe kommen aber immer noch verständlich für Menschen sind.
Ein Beispiel ist die Assembler-Sprache, die jedoch von Prozessorherstellern oft proprietäre Befehle haben. 
Typische Befehle sind:
- `MOV`: Lese den Wert aus der Speicherzelle X und lege sie in das Rechenregister Y ab.
```
MOV Y, X 
```
- `ADD`: Addiere den Wert 10 zum Inhalt des Rechenregisters X
```
ADD X, 10
```
- `CMP`: Vergleiche den Inhalt des Registers X mit dem Wert 20
```
CMP X, 20
```
- `JE`: Falls der Vergleichsbefehl gleich ergeben hat springe zu der Programmadresse X; JE steht für "jump if equal"
```
JE X
```

# Threading
Beim *Threading* werden mehrere Aufgaben (*Threads*) parallel von einem Prozessor bearbeitet. 