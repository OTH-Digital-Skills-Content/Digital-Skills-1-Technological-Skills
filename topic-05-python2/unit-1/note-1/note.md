---
order: 2

icon:
  type: fluent:notepad-28-regular
  color: orange
---

Note Challenge

Debugging, Listen und Algorithmen

[[toc]]



> Alle Codebeispiele für diese Challenge sind auf replit.com abrufbar:
> [https://replit.com/@mheckner/Digital-Skills04Python2#main.py](https://replit.com/@mheckner/Digital-Skills04Python2#main.py)
> Mit einem Klick auf "Fork Repl" rechts oben können Sie diese in ein eignes Repl übertragen und auch Änderungen ausprobieren.



# Eigene Funktionen mit Rückgabewerten

Funktionen können nicht nur Ausgaben erzeugen, sondern auch Ergebnisse zurückliefen. Beispielsweise liefert die Funktion ```get_string``` den von der Kommandozeile eingelesenen String als Ergebnis zurück.

Funktionen erzeugen Rückgabewerte durch das Schlüsselwort ```return```. Das folgende Beispiel liest zwei Zahlen von der Kommandozeile ein und berechnet das Produkt der beiden Zahlen in einer eigenen Funktion (```calculator1.py```):

```python
from cs50 import get_int

def main():
  x = get_int("x: ")
  y = get_int("y: ")

  sum = calculate_sum(x, y)
  print(sum)

def calculate_sum(x, y):
  sum = x + y
  return sum

if __name__ == "__main__":
  main()
```

Das folgende Programm liest eine positive Zahl von der Kommandozeile ein (```positive.py```):

```python
from cs50 import get_int

def main():
  positive_num = get_positive_int()
  print(positive_num)

def get_positive_int():
  while True:
    num = get_int("Enter positive number: ")
    if num >= 0:
      return num

if __name__ == "__main__":
  main()
```



# Debugging

**Bugs** sind Fehler in Programmen, die dazu führen, dass das Programm etwas anderes tut als von den Entwicklern vorgesehen. **Debugging** ist der Prozess diese Bugs zu finden und zu beheben.

Der Name Bug geht möglicherweise auf ein echtes Insekt zurück, das 1947 in dem Supercomputer Harvard Mark II gefunden wurde und das einen Fehler des Supercomputers verursachte ([https://en.wikipedia.org/wiki/Harvard_Mark_II](https://en.wikipedia.org/wiki/Harvard_Mark_II)):

![05_first_bug](./img/05_first_bug.jpeg)

Das folgende Programm ist fehlerhaft (= buggy) und soll **drei** Blöcke (d.h. ```#```-Zeichen) ausgeben (```buggy.py```):

~~~python
def main():
  i = 0
  while (i <= 3):
    print("#")
    i += 1

if __name__ == "__main__":
  main()
~~~

Das Programm lässt sich über die Shell ausführen, enthält aber einen logischen Fehler, da es 4 Blöcke ausgibt:

~~~shell
$ python3 buggy.py 
#
#
#
#
~~~

Möglicherweise wird der Fehler bereits deutlich, aber für das Debugging, lässt sich temporär eine weitere ```print```-Funktion in den Code integrieren:

~~~python
def main():
  i = 0
  while (i <= 3):
    print("#")
    i += 1
    print(f"i is: {i}")

if __name__ == "__main__":
  main()
~~~

Die Ausgabe des Programms zeigt jetzt, dass ```i``` mit dem Wert 0 gestartet ist und der Wert erhöht wurde, bis ```i``` den Wert 4 angenommen hat:

~~~shell
$ python3 buggy.py 
#
i is: 1
#
i is: 2
#
i is: 3
#
i is: 4
~~~

Um den Wert 4 anzunehmen, muss die Schleife mit dem Startwert 3 für ```i``` begonnen haben. Dies ist ein Durchlauf zu viel. Ändert man die Bedingung von ```(i <= 3)``` auf ```(i < 3)``` funktioniert das Programm wie vorgesehen, und der Bug wurde identifiziert und behoben. 

Die vorgestellte Art Fehler durch ```print``` Anweisungen zu finden bezeichnet man in allen Programmiersprachen als **Printlining**. 

Zusätzlich bietet replit einen **Debugger**, d.h. ein Werkzeug das es erlaubt den Code während der Laufzeit Schritt für Schritt nachzuverfolgen und Variablen und andere Informationen anzusehen.

Zuerst klickt man dafür im Editor von replit in der vierten Zeile auf die Spalte links von den Zeilennnummern des Texteditors, sodass ein blauer Punkt erscheint:

![05_replit_debugger](./img/05_replit_debugger.png)

Um den Debugger in replit aufzurufen, klickt man anschließend auf das Pfeilsymbol in der linken Seitenleiste von replit:

![05_replit_breakpoint_set](./img/05_replit_breakpoint_set.png)

Der Debugger zeigt an, dass ein Breakpoint in ```buggy.py``` angelegt wurde.

Um den Programm mit dem Debugger auszuführen klickt man jetzt auf den blauen *Play-Button* (anstatt das Programm über die Shell zu starten). Das Programm bleibt anschließend am blauen Punkt (= **Breakpoint**) stehen, und erlaubt einen Blick in das Programm zur Laufzeit.

**Achtung: ** Der Debugger in replit befindet sich noch im Betastadium, d.h. es ist nicht möglich beliebige Dateien zu debuggen. Um den Debugger nutzen zu können, muss der fehlerhafte Code in eine Datei ```main.py``` kopiert werden!

Eine weitere Möglichkeit, um Fehler in Programmen ist **Rubber-Duck-Debugging**. ([https://en.wikipedia.org/wiki/Rubber_duck_debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging)), bei dem man sich als Entwickler dazu zwingt den eigenen Code einem Quietscheentchen (oder einem anderen Objekt erklärt). Durch das laute Durchsprechen des eigenen Codes fallen einem häufig Fehler auf.

# Listen

Das folgende Beispiel berechnet den Durchschnitt aus drei Variablen (```score0.py```):

~~~python
def main():
  score1 = 72
  score2 = 73
  score3 = 33

  average = (score1 + score2 + score3) / 3
  print(f"Durchschnitt: {average}")

if __name__ == "__main__":
  main()
~~~

Das Programm gibt den Durchschnitt korrekt aus, muss aber für jeden Wert der für die Berechnung des Durchschnitts verwendet werden soll eine neue Variable anlegen. Je mehr Werte in die Berechnung einfließen, desto unhandlicher und unübersichtlicher wird das Programm.

Python bietet zur Lösung dieses Problems den Datentyp ```list```, der es ermöglicht eine Sequenz von mehreren Werten nacheinander in einer einzigen Variable abzuspeichern.

Der Code ```scores = [72, 73, 33]``` erzeugt eine Liste aus drei Werten in Python. Auf diese Werte kann später beispielsweise mit ```scores[0]``` wieder zugegriffen werden (```score1.py```):

~~~python
def main():
  scores = [72, 73, 33]

  average = (scores[0] + scores[1] + scores[2]) / 3
  print(f"Durchschnitt: {average}")

if __name__ == "__main__":
  main()
~~~

```scores[0]``` bezeichnet dabei das erste Element (Listen fangen bei ```0``` an zu zählen), während ```scores[2]``` sich auf das dritte Element (=```33```) bezieht.

Häufig möchte man die Werte von 2 Variablen vertauschen. Das folgende Codebeispiel definiert zwei Variablen ```a``` und ```b```, und versucht die beiden Werte zu tauschen (```swap_buggy.py```):

```python
def main():
  a = 5
  b = 3
  a = b
  b = a

  print(f"Swapped variables a: {a} b: {b}")

if __name__ == "__main__":
  main()
```

Das Programm funktioniert nicht wie erwartet, da zwar die Variable ```a```  zunächst den korrekten Wert ```3``` der Variable ```b``` erhält, danach aber die Variable ```b``` nicht den korrekten ursprünglichen Wert ```5``` von ```a``` erhält sondern den neuen Wert ```3``` aus der Variable ```b``` Die Ausgabe des Programms zeigt den Fehler:

```shell
$ python3 swap_buggy.py 
Swapped variables a: 3 b: 3
```

Das folgende Beispiel ```swap.py``` zeigt das korrekte Tauschen von zwei Werten einer Liste. Das Problem des Überschreibens einer der beiden Variablen wird mit einer Hilsvariable ```temp``` gelöst. Diese speichert den Wert zwischen und kann dann später wieder ausgelesen werden.

```python
def main():
  a = 5
  b = 3
  temp = a
  a = b
  b = temp
  print(f"Swapped variables a: {a} b: {b}")

if __name__ == "__main__":
  main()
```

Auch in einer Liste lassen sich Elemente mit einer Hilfsvariable vertauschen (```swap_list.py```):

```python
def main():
  scores = [73, 72, 33]
  
  temp = scores[0]
  scores[0] = scores[2]
  scores[2] = temp

  print(f"Swappeds list elements: {scores}")

if __name__ == "__main__":
  main()
```

Die Ausgabe des Programms zeigt die korrekt vertauschten Werte der Liste an den Positionen ```0``` und ```2```:

```shell
$ python3 swap.py 
Swappeds list elements: [33, 72, 73]
```

Das Beispiel ```score2.py``` zeigt wie sich Listen verlängern lassen:

~~~python
from cs50 import get_int

def main():
  scores = []

  score1 = get_int("Score: ")
  score2 = get_int("Score: ")
  score3 = get_int("Score: ")

  scores.append(score1)
  scores.append(score2)
  scores.append(score3)
  
  average = (scores[0] + scores[1] + scores[2]) / 3
  print(f"Durchschnitt: {average}")

if __name__ == "__main__":
  main()
~~~

Das Programm liest zunächst drei Werte ein und speichert diese in den Variablen ```score1```, ```score2```, und ```score3```. 

Die Liste ```scores``` ist in Python ein **Objekt**. Objekte sind Variablen die einerseits mehr als einen Wert speichern können (die Liste speichert beispielsweise mehrere Werte) die andererseits auch Funktionen bieten, die nur zu diesen Variablen gehören. Eine solche Funktion ist ```append```, die als Parameter ein neues Element erwartet, das der Liste ```scores``` hinzugefügt werden soll. Funktionen die zu Objekten gehören müssen im Code per Punkt getrennt direkt hinter dem Objektnamen stehen (hier: ```scores0.append(score1)```).

Die Funktion ```append``` fügt der Liste ein neues Element hinzu und verlängert diese um jeweils dieses neue Element.

Das Design des obigen Programms ist suboptimal, da wieder für jeden neuen Wert eine neue Variable angelegt werden muss. Das Programm lässt sich wie folgt umschreiben (```score3.py```):

~~~python
from cs50 import get_int

NUM_SCORES = 3

def main():
  scores = []
  for i in range(NUM_SCORES):
    score = get_int("Score: ")
    scores.append(score)      

  average = (scores[0] + scores[1] + scores[2]) / NUM_SCORES
  print(f"Durchschnitt: {average}")

main()
~~~

Zu Beginn wird eine Konstante ```NUM_SCORES``` definiert, in der die Anzahl der Werte, die das Programm einlesen und in der Liste speichern soll abgelegt wird. Anschließend werden die Nutzer mit ```for i in range(NUM_SCORES):``` diesem Wert entsprechend oft (hier: 3) aufgefordert eine neue Zahl einzugeben, und diese Zahl wird der Liste ```scores``` hinzugefügt. Zur Berechnung des Durchschnitts wird ```NUM_SCORES``` dann erneut verwendet.

Das Design ist bei der Berechnung des Durchschnitts auf jedes Element der Liste einzelnn zugegriffen werden muss. Verändert sich der Wert von ```NUM_SCORES```, dann mus auch die Berechnung des Durchschnitts angepasst werden. 

Python bietet zur Lösung dieses Problems die Funktionen ```sum()``` und ```len()``` an (```score4.py```):

~~~python
from cs50 import get_int

NUM_SCORES = 3

def main():
  scores = []
  for i in range(NUM_SCORES):
    score = get_int("Score: ")
    scores.append(score)      

  average = sum(scores) / len(scores)
  print(f"Durchschnitt: {average}")

if __name__ == "__main__":
  main()
~~~

```sum()``` berechnet die Summe aller Elemente der Liste, und ```len()``` zählt die Elemente der Liste. Das Design des Programms ist jetzt besser, da lediglich der Wert von ```NUM_SCORES``` an einer Stelle geändert werden muss und das Einlesen der Werte und die Berechnung passen sich automatisch an.

Suchen in einer Liste. Das folgende Beispiel erstellt eine Liste von Wörtern, fragt die Nutzer nach dem Wort und gibt anschließend aus, ob das Wort in der Liste enthalten ist oder nicht (```names.py```):

~~~shell
from cs50 import get_string

NAMES = ["Alex", "Bruce", "Charles"]

def main():
  name = get_string("Enter name: ")
  if name in NAMES:
    print("Korrekter Name!")
  else:
    print("Diesen Namen kenne ich nicht!")

if __name__ == "__main__":
  main()
~~~

Der Code ```if name in NAMES``` sucht den Namen im Dictionary und gibt ```True``` zurück, wenn der Name enthalten ist und ```False``` wenn nicht. 

# Dictionaries

In Listen lassen sich Daten abspeichern und anhand der Position abrufen. Manchmal möchte man aber Daten (= **Werte**) nicht anhand einer Position sondern anhand einer bestimmten Zeichenkombination (= **Key**) abfragen. Ein Beispiel dafür ist ein Telefonbuch: Der Key entspricht dem Namen nach dem man sucht und der Wert ist die Telefonnumer. Python bietet dazu eine besondere Liste, das **Dictionary**.

Das folgende Beispiel zeigt eine Implementierung eines Telefonbuchs mithilfe eines Dictionaries in Python (```phonebook0.py```):

~~~python
# Implements a phone book

from cs50 import get_string

phonebook = {
    "Polizei": 110,
    "Feuerwehr": 112
}

def main():
  # Search for name
  name = get_string("Name: ")
  if name in phonebook:
      print(f"Number: {phonebook[name]}")
    
if __name__ == "__main__":
  main()
~~~

Werte lassen sich über die Keys auch direkt aus dem Dictionary mit ```phonebook.get("Polizei")``` abfragen. Als Ergebnis erhält man den unter dem Key gespeicherten Wert (hier: ```110```).

Fragt man Werte für Keys aus dem Dictionary an, die dort nicht existieren, gibt die Funktion ```get``` den Wert ```None```zurück (```phonebook1.py```):

~~~python
from cs50 import get_string

phonebook = {
    "Polizei": 110,
    "Feuerwehr": 112
}

def main():
  police_number = phonebook.get("Polizei")
  print(f"Polizei: {police_number}")

  invalid_key = phonebook.get("Notruf")
  print(f"Invalid Key: {invalid_key}")
    
if __name__ == "__main__":
  main()
~~~

Die Ausgabe des Programms ist wie folgt:

~~~shell
$ python phonebook1.py 
Polizei: 110
Invaild Key: None
~~~

# Strings

String sind in Python Sequenzen aus einzelnen Buchstaben. Das folgende Programm ermittelt die Länge eines Strings (```string0.py```):

~~~python
from cs50 import get_string

def main():
  name = get_string("Name eingeben: ")
  length = len(name)
  print(f"Länge: {length}")

main()
~~~

Der folgende Code geht einen String Zeichen für Zeichen durch und gibt jeden Buchstaben als Großbuchstaben aus (```string1.py```):

~~~python
from cs50 import get_string

def main():
  before = get_string("Before:  ")
  print("After:  ", end="")
  for c in before:
      print(c.upper(), end="")
  print()

if __name__ == "__main__":
  main()
~~~

Zuerst wird ein String in der Variable ```before``` eingelesen. Anschließend wird am Beginn einer neuen Zeile *After:* ausgegeben, jedoch ohne eine neue Zeile zu beginnen (vgl. ```print("After:  ", end="")```).

Die Schleife ```for c in before:``` geht jedes Zeichen des Strings einzeln durch, d.h. der Wert von ```c``` nimmt in innerhalb der Schleife jeweils einen anderen Buchstaben des Strings an. Die Funktion ```upper()``` wird durch das Objekt String in Python bereitgestellt und gibt den Buchstaben als Großbuchstabe zurück.

Achtung: ```upper()``` wandelt die Zeichen des Strings nicht um, da Strings in Python **immutable**, d.h. unveränderlich sind. Die Ausgabe des Strings ist demnach im folgenden Beispiel unverändert (```string2.py```): 

~~~python
from cs50 import get_string

def main():
  before = get_string("Before:  ")
  print("After:  ", end="")
  for c in before:
      print(c.upper(), end="")
  print()
  print(f"String: {before}")

if __name__ == "__main__":
  main()
~~~

Ausgabe:

~~~shell
$ python3 string2.py 
Before:  hallo
After:  HALLO
String: hallo
~~~

Der folgende Code wandelt einen String in Großbuchstaben um (```string3.py```):

~~~python
from cs50 import get_string

def main():
  before = get_string("Before:  ")
  print(f"Before:  {before}")
  after = before.upper()
  print(f"After:  {after}")

if __name__ == "__main__":
  main()
~~~

Der ursprüngliche String ```before``` bleibt aber unverändert (Strings sind immutable): Python erzeugt einen neuen String ```after``` und kopiert dort die Zeichen als Großbuchstaben.

Ausgabe:

~~~shell
$ python3 string3.py 
Before:  hallo
Before:  hallo
After:  HALLO
~~~

Eine häufige Aufgabe ist es Strings in einzelne Wörter zu zerlegen (```string4.py```):

~~~python
from cs50 import get_string

def main():
  sentence = get_string("Sentence to split:  ")
  words = sentence.split()

  for word in words:
    print(f"Word: {word}")

if __name__ == "__main__":
  main()
~~~

Die Ausgabe des Programms ist wie folgt:

~~~shell
$ python3 string4.py 
Sentence to split:  Hello, it's me
Word: Hello,
Word: it's
Word: me
~~~

Das folgende Programm (```string5.py```) zeigt, wie man das Vorkommen von Zeichen in einem String zählen kann:

~~~python
from cs50 import get_string

def main():
  sentence = get_string("Sentence:  ")
  count = sentence.count("a")
  print(count)

if __name__ == "__main__":
  main()
~~~

Ruft man das Programm auf, erhält man folgenden Ausgabe:

~~~shell
python string5.py 
Sentence:  hallo
1
~~~

```a``` kommt einmal im Wort hallo vor, wird hello eingebeben, ist die Ausgabe 0.

# Konvertieren von Strings

Eine häufiges Programmierproblem ist die Suche von Strings in einer Liste von Strings. Das folgende Beispiel liest eine Zeichenkette von der Kommandozeile ein und überprüft, ob diese in einer Liste von Zeichenketten enthalten ist. Um das Programm gegen Fehleingaben der Nutzer robuster zu machen, wird die eingegebene Zeichenkette mit ```s.lower()``` in Kleinbuchstaben umgewandelt. Hat ```s``` beispielsweise zuerst den Wert ```JA```, wird dieser in ```ja``` umgewandelt (```string6.py```).

~~~python
from cs50 import get_string

def main():
  s = get_string("Do you agree? ")

  s = s.lower()
  
  if s in ["y", "yes"]:
      print("Agreed.")
  elif s in ["n", "no"]:
      print("Not agreed.")

if __name__ == "__main__":
  main()
~~~



# Parameter von der Shell

Python-Programmen lassen sich auch Variablen direkt von der Shell übergeben. Dazu wird in Python die Bibliothek ```sys```benötigt.

Der folgende Code überprüft, ob beim Aufruf des Programms ein Parameter von der Shell übergeben wurde (```argv0.py```):

~~~python
# Prints a command-line argument

from sys import argv

def main():
  if len(argv) == 2:
      print(f"hello, {argv[1]}")
  else:
      print("hello, world")

if __name__ == "__main__":
  main()
~~~

Zuerst wird die Liste ```argv``` aus der Bibliothek ```sys``` importiert. Diese Liste enthält die Parameter die von der Kommandozeile an den Python Interpreter übergeben wurden. Der folgenden Aufruf...

~~~shell
$ python3 argv0.py Markus
~~~

führt zu folgenden Werten in ```argv```: ```argv[0]```entspricht dem Python-Skript (hier: ```argv.py```) und ```argv[1]``` dem ersten durch Leerzeichen abgetrennten String nach dem Skriptnamen (hier: ```Markus```).

Die Ausgabe des Programms mit obigen Aufruf ist demnach wie folgt:

~~~shell
$ python3 argv0.py Markus
hello, Markus
~~~

Programme lassen sich beenden, wenn eine bestimmte Bedingung eintrifft (```argv1.py```):

~~~python
# Prints a command-line argument

from sys import argv, exit

def main():
  if len(argv) != 2:
    print("Missing command-line argument")
    exit(1)

  print(f"hello, {argv[1]}")
  exit(0)

if __name__ == "__main__":
  main()
~~~

Wird das Programm ohne Parameter auf der Shell ausgeführt, erhält man die folgende Fehlermeldung:

~~~shell
$ python3 argv1.py 
Missing command-line argument
~~~

Erst mit einem Parameter beendet sich das Programm nicht und gibt den Parameter aus:

~~~shell
$ python3 argv1.py hello
hello, hello
~~~

Die Parameter der Funktion exit signalisieren dabei einen Fehler (```1```) oder ein normales Ende des Programms (```0```).

Das obige Programm lässt sich umschreiben, um klarer zu kennzeichnen aus welcher Bibliothek ```argv``` und ```exit()``` stammen (```argv2.py```):

~~~python
# Prints a command-line argument

import sys

def main():
  if len(sys.argv) != 2:
    print("Missing command-line argument")
    sys.exit(1)

  print(f"hello, {sys.argv[1]}")
  sys.exit(0)

if __name__ == "__main__":
  main()
~~~

Das Voranstellen von ```sys.``` vor ```argv``` bzw. ```exit()``` kennzeichnen diese als zur Bibliothek ```sys``` zugehörig. 


# Dokumentation
Die offizielle [Python-Dokumentation](https://docs.python.org/3/) eräutert die in Python bereits vorhandenen Funktionen. Beispielsweise findet man dort alle Methoden zur Stringbearbeitung:  [https://docs.python.org/3/library/stdtypes.html#string-methods](https://docs.python.org/3/library/stdtypes.html#string-methods). Auf dieser Dokumentation findet sich auch eine Beschreibung, wie man Fließkommazahlen runden kann: [https://docs.python.org/3/library/functions.html?highlight=round#round](https://docs.python.org/3/library/functions.html?highlight=round#round)



Basierend auf CS50 von David J. Malan (Harvard University, 2022).

[Impressum Digital Skills](https://tutors.dev/course/technological-skills)