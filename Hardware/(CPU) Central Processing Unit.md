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
Der RAM wird benutzt zur Speicherung von Programmen die ausgeführt werden, und den Daten die von ihnen verarbeitet werden.
Früher wurden der RAM mit derselben Taktfrequenz wie das motherboard betrieben. bei modernen PCs jedoch mit einem vielfachen des motherboards.

>[!note]
> Falls der Arbeitsspeicher nicht ausreicht, um die Daten der aktuell geladenen Programme zu speichern, werden Daten die nicht dringend benötigt werden aus dem RAM auf die Festplatte verschoben. Dies wird je nach Betriebssystem *Swapping* oder *Paging* genannt. Moderne Prozessoren unterstützen diese Speicherverwaltung durch eine *Memory Management Unit (MMU)*.  
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

# Threading
Beim *Threading* werden mehrere Aufgaben (*Threads*) parallel von einem Prozessor bearbeitet. 