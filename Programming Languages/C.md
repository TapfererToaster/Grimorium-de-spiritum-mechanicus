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

# Datentypen
## Zeichen

| Datentyp      | Wertebereich | Speichergröße | Anwendung                                                                                                                                                                      |
| ------------- | ------------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| char          | 1 Zeichen    | 1 Byte        | Zeichen werden durch einfache Anführungszeichen eingeschlossen. Intern wird das Zeichen als Wert des entsprechenden ASCII-Zeichens gespeichert, z. B. 65 für den Buchstaben a. |
| signed char   | -128 bis 127 | 1 Byte        | Es können auch Ganzzahlen gespeichert werden.                                                                                                                                  |
| unsigned char | 0 bis 255    | 1 Byte        | Es können nur positive Zahlen einschließlich der Null gespeichert werden.                                                                                                      |

## Ganzzahlen

| **Datentyp**           | **Wertebereich**                                                                             | **Speichergröße**    |
| ---------------------- | -------------------------------------------------------------------------------------------- | -------------------- |
| short int              | -32768 bis 32767                                                                             | 2 Byte               |
| unsigned short int     | 0 bis 65535                                                                                  | 2 Byte               |
| int                    | Maschinenabhängig:<br>-32768 bis 32767 (16 Bit)<br>-2,147,483,648 bis 2,147,483,647 (32 Bit) | <br>2 Byte<br>4 Byte |
| unsigned int           | Maschinenabhängig:<br>0 bis 65535 (16 Bit)<br>0 bis 4,294,967,295 (32 Bit)                   | <br>2 Byte<br>4 Byte |
| long int               | -2,147,483,648 bis 2,147,483,647 (32 Bit)                                                    | 4 Byte               |
| unsigned long int      | 0 bis 4,294,967,295 (32 Bit)                                                                 | 4 Byte               |
| long long int          | -9,223,372,036,854,775,808 bis 9,223,372,036,854,775,807 (64 Bit)                            | 8 Byte               |
| unsigned long long int | 0 bis 8,446,744,073,709,551,615 (64 Bit)                                                     | 8 Byte               |
## Gleitkomma Zahlen

| Datentyp    | Wertebereich                           | Genauigkeit                              | Speichergröße                               |
| ----------- | -------------------------------------- | ---------------------------------------- | ------------------------------------------- |
| float       | $1,18*10^{-38}$ bis $3,4*10^{38}$      | 7 Stellen                                | 4 Byte                                      |
| double      | $2,23*10^{-308}$ bis $1,79*10^{308}$   | 15 Stellen                               | 8 Byte                                      |
| long double | $3,37*10^{-4932}$ bis $1,18*10^{4932}$ | 18 bis 33 Stellen<br>(plattformabhängig) | 10 bzw. 12 oder 16 Byte (plattformabhängig) |
# Variablen
 Bevor man Variablen benutzen kann muss man diese deklarieren, also einen Datentyp und Bezeichner (Namen) festlegen. Zusätzlich muss die Variable definiert werden, ihr muss noch ein Speicherbereich zugeteilt werden. 

> [!NOTE]
>  Jede Definition ist auch eine Deklaration.

## Definition
Variablen können nach folgendem Muster definiert werden:
- `Datentyp name;`
- `Datentyp name1, name2;`
- `Datentyp name = ausdruck;`

```C
char antwort;
int zahl1, zahl2;
int ergebnis = zahl1 + zahl2
```

Werte werden mit `=` den Variablen zugewiesen:
```C
int zahl = 17; 
```

Beispiel
```C
int main(void)
{
	int zahl1 = 3;
	int zahl2 = 5;
	int ergebnis = zahl1 + zahl2;
	return 0;
}
```

# Konstanten 
Konstanten werden für Werte benutzt die sich im Laufe des Programms nicht ändern, etwa physikalische oder andere Größen. 
Konstanten werden mit `const` definiert:
```C
const double pi = 3.14;
const double mwst = 0.19;
const char zeichen = 'J';
```

**Beispiel**
```C
include <stdio.h>
int main(void)
{
	const double pi = 3.14;
	double radius = 14.0;
	
	printf("Der Kreisumfang beträgt: %f.\n", 2*pi*radius);
	getchar();
	return 0;
}
```

>[!note]
>`getchar()` wird benutzt um nach der Ausgabe der Werte auf das drücken der Eingabetaste gewartet, damit die Ausgabe nicht sofort beendet wird.

## Übung
Ein Programm das Temperaturangaben con Kelvin (K) in °C umrechnet.
```C
#include <stdio.h>

int main(void)
{
    const double temperaturKelvin = 14.0;
    const double umrechnung = temperaturKelvin - 273.15;

    printf("Die Temperatur in Celsius betraegt: %f.\n", umrechnung);
    getchar();
}
```

# Operatoren
## Arithmetische Operatoren

| **Operation**  | **Operator** | **Code**                             |
| -------------- | ------------ | ------------------------------------ |
| Addition       | `+`          | `summe = summand1 + summand2;`       |
| Subtration     | `-`          | `differenz = minunend - subtrahend;` |
| Multiplikation | `*`          | `produkt = faktor1 * faktor2;`       |
| Division       | `/`          | `quotient = dividend / divisor;`     |
| Modulo         | `%`          | `rest = dividend % divisor;`         |
**Beispiel** 
Addition
```C
#include <stdio.h>
int main(void)
{
	int summand1, summand2, summe;
	summand1 = 18;
	summand2 = 56;
	printf("Addition zweier Zahlen: \n\n");
	printf("Der erste Summand ist a = %d. \n", summand1);
	printf("Der zweite Summand ist b = %d. \n", summand2);
	summe = summand1 + summand2;
	printf("Das Ergebnis ist: %d + %d = %d. \n", summand1, summand2, summe);
	getchar();
	return 0;
}
```

Division mit Rest
```C
#include <studio.h>
int main(void)
{
	int dividend, divisor, quotient, rest;
	dividend = 56;
	divisor = 18;
	printf("\n\nDivision zweier Zahlen:\n\n");
	printf("Der Dividend ist a = %d.\n", dividend);
	printf("Der Divisor ist b = %d.\n", divisor);
	quotient = dividend / divisor;
	printf("Das Ergebnis lautet: %d : %d = %d", dividend, divisor, quotient);
	rest = dividend % divisor;
	printf("Rest: %d\n\n\n", rest);
	getchar();
	return 0;
```

## Relationale Operatoren

| **Operator** | **Code**         | **Wahr, wenn**                 |
| ------------ | ---------------- | ------------------------------ |
| `>`          | `zahl1 > zahl2`  | zahl1 größer als zahl2         |
| `<`          | `zahl1 < zahl2`  | zahl1 kleiner als zahl2        |
| `>=`         | `zahl1 >= zahl2` | zahl1 größer oder gleich zahl2 |
| `<=`         | `zahl1 <= zahl2` | zahl1 kleiner der gleich zahl2 |
| `==`         | `zahl1 == zahl2` | zahl1 gleich zahl2             |
| `!=`         | `zahl1 != zahl2` | zahl1 ungleich zahl2           |
## Logische Operatoren

| **Operator** | **Name**        | **Code**                   | **Wahr, wenn**                                  |
| ------------ | --------------- | -------------------------- | ----------------------------------------------- |
| `&&`         | Logisches UND   | `condition1 && condition2` | condition1 und condition2 sind wahr             |
| \|\|         | Logisches ODER  | condition1 \|\| conditon2  | condition1 oder condition2 oder beide sind wahr |
| `!`          | Logisches NICHT | `!condition`               | condition ist falsch                            |
|              |                 |                            |                                                 |
## Bitoperatoren
Mit Bitoperatoren kann man auf die binäre Darstellung von Zahlen zugreifen und Operationen durchführen.
![[Bitoperatoren.png|554x116]]
![[Bitoperatoren 2.png.png]]

## Zuweisungsoperatoren
