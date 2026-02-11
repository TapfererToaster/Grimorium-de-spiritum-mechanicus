https://github.com/codr7/hacktical-c
# Quellcode zu Maschinenprogramm
![[Quellcode zu Maschinenprogramm.png|497x113]]
1. Zuerst werden die Header-Dateien durch den Präcompiler in die  Quellcodedatei eingefügt, das ermöglicht das Einbinden von Standardbibliotheken und fremde Funktionen in das Programm. Die Header-Datei liefert Informationen die an anderer Stelle bereitgestellt werden. Der *Präcompiler* wird über *Direktiven* gesteuert, Befehle die im Quellcode mit dem Zeichen `#` beginnen. 
   >[!note]
   >Der *Präcompiler* fügt die Header Datei in den Quelltext ein und führt weitere Vorbereitungen für den Compiler aus, damit dieser den Quelltext richtig übersetzt.
   >Anweisungen die im Quell text für den Precompiler gedacht sind werden mit `#` gekennzeichnet.
2. Im folgenden Schritt wird der Quellcode in eine Objektdatei übersetzt. Diese sind Maschinenprogramme deren Lage im Speicher des Computers noch durch den *Linker* angepasst werden muss.
3. Im letzten Schritt werden noch weitere Objektdateien mit dem Hauptprogramm verbunden (z.B. weitere Bibliotheken). Das Programm kann jetzt vom Prozessor ausgeführt werden.

# Struktur eines C Programms
![[Struktur C Programm.png]]

## Hallo Welt
```C
#include <stdio.h>
int main(void)
{
	printf("Hallo Welt");
	return 0;
}
```

- mit `#include <stdio.h>` wird die Header Datei oder auch Standardbibliothek `stdio.h` eingefügt die für die Funktion `printf` gebraucht wird
- In der Hauptfunktion `main` werden die auszuführenden Befehle geschrieben, der Parameter `void` kennzeichnet, dass es keinen Übergabewert geben soll
- Nach erfolgreicher Ausführung der Funktion wird der Wert 0 an das OS gegeben, bei Fehlern wird ein Wert != 0 zurückgegeben.

## Kommentare
Kommentare werden mit den Zeichen `/*` begonnen und mit `*/` geschlossen, seit dem C99-Standard kann man Zeilenweise Kommentare mit `//` kennzeichnen.
```C
/* Das ist ein Kommentar*/ printf("Das wird ausgeführt");
// Das ist ein Kommentar für die ganze Zeile printf("Das wird nicht ausgeführt");
```

