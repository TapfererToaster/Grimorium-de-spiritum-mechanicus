Der Arbeitsspeicher enthält die aktuell ausgeführten Programme und die Daten die von diesen verarbeitet werden und besteht aus *Random Access Memory (RAM)* Speicherbausteinen. Die Bezeichnung "Random Access" bedeutet "wahlfreier Zugriff" und bedeutet:
- die Inhalte des Speichers können gelesen und verändert werden (im Gegensatz zu *Read-only Memory (ROM)*)
- Es kann auf jedes Byte in beliebiger Reihenfolge zugegriffen werden (im Gegensatz zum *sequentiellen Zugriff*)

>[!note]
> Falls der Arbeitsspeicher nicht ausreicht, um die Daten der aktuell geladenen Programme zu speichern, werden Daten die nicht dringend benötigt werden aus dem RAM auf die Festplatte verschoben. Dies wird je nach Betriebssystem *Swapping* oder *Paging* genannt. Moderne Prozessoren unterstützen diese Speicherverwaltung durch eine *Memory Management Unit (MMU)*.  

RAM ist *flüchtiger* Speicher, das bedeutet, dass der Inhalt des Speichers geht verloren, wenn die Stromversorgung zum Speicher beendet wird.
Zu unterscheiden sind zwei Arten von RAM:
- *Dynamic RAM (DRAM)*:
  jede Speicherstelle muss mit jedem Taktzyklus aufgefrischt werden (*refresh*) 
- *Static RAM (SRAM)*:
  SRAM braucht nur eine anliegende Spannung, er ist schneller als DRAM, aber verbraucht mehr Strom und ist teurer; er wird nur für *Caches* benutzt. 

>[!note]
>RAM ist in einzelnen 1 Byte großen Speicherzellen mit individuellen Adressen organisiert. Wie diese Speicher adressiert werden, hängt vom Prozessor und indirekt vom Betriebssystem ab.
>

Gängige Bauformen für RAM sind:
- *Synchronous Dynamic RAM (SD-RAM)*
  bestehen aus *Double Inline Memory Modules (DIMM)* und sind baugleich mit DDR-RAM; der Zugriff auf den Speicher erfolgte mit der Taktfrequenz des Motherboards
- *Double Data Rate-RAM (DDR-RAM)* und *DDR-SD-RAM*
bestehen auch aus DIMM und haben 168 Kontakte (Pins); die Speicherbausteine arbeiten mit der doppelten Datenrate als SD-RAM und können pro Taktzyklus doppelt so viele Inhalte aufnehmen oder abgeben
- *Rambus-RAM (RD-RAM)*
  werden in From von *Rambus Inline Memory Module (RIMM)* geliefert und haben 184 Pins; sie werden mit einem festen Multiplikator betrieben, nur von der Firma hergestellt und benötigen spezielle Motherboards mit besonderem Datenbus.
  