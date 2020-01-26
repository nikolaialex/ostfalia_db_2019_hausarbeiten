# Anwendungsbeispiele

## Deep Learning - Gesichtserkennung mit Python und CV2

Eine besondere Form des maschinellen Lernens ist das sogenannte Deep Learning. Verschiedene Dateien wie Bilder, Videos oder Texte werden dabei in der Regel mit Hilfe eines neuronalen Netzes untersucht und klassifiziert. Anwendungsbereiche können beispielsweise Objekterkennung oder Sprachassistenten wie Amazon Alexa sein, deren Implementierung auf Basis von Deep Learning Mechanismen funktioniert. Die Tiefe besteht in der Anzahl von Schichten in einem neuronalen Netz, die bei Deep-Learning Algorithmen signifikant höher ausgeprägt ist als bei normalen Machine Learning Anwendungen. [51]

Umsetzungen von Deep Learning Anwendungen sind bereits heute problemlos möglich - dies soll im Nachfolgenden anhand einer Gesichtserkennung unter der Verwendung verschiedener Python- Bibliotheken demonstriert werden.

Mehrere frei verfügbare Programmierbibilotheken und Module wie OpenCV2 erlauben eine relative einfache Umsetzung. Die Gesichtserkennung folgt dabei nicht über einen einfachen Abgleich der RGB-Werte einzelner Pixel eines Bildes - anhand von 128 Merkmalen im Gesicht wird ein neuronales Netz trainiert, anhand dieser Merkmale Ähnlichkeiten festzustellen.

Die dlib- Bibliothek (vgl. https://github.com/davisking) bietet hierzu ein Framework, dass mit ca. 3 Millionen Gesichtern trainiert ist und eine Klassifizierung, ob sich zwei Gesichter unterscheiden, erlaubt.

Die Klassifizierung der Objekte erfolgt über verschiedene Detektionsmuster wie RCNN, SSD oder Hog. Anhand der bereits erlernten Daten wird die Wahrscheinlichkeit ermittelt, ob ein Objekt dem Suchmuster entspricht.[52] [53]
![Objekterkennung ](images/objectDetection.PNG "Objekterkennungn")


## Sprachassistenten

In den letzten Jahren gibt es eine starke Zunahme an Geräten, die durch die Ein-und  Ausgabe  durch  Sprache  gesteuert  werden  können.  Bereits  im  Jahr 2017  besaßen  36  Millionen  Amerikaner  einen  Sprachassistenten,  über  ein Drittel  der  Nutzer  sahen  den  Sprachassistenten  als  festen  Bestandteil  ihres Alltags. Die  am  meisten  verbreiteten  Lautsprechersysteme  mit  Sprachunterstützung sind  der  Amazon  Echo  sowie  das  Google  Home  System -am  weitesten verbreitet  ist  jedoch  der  Einsatz  von  Sprach- Schnittstellen  auf  Smartphones, bereitgestellt  durch  den  Sprachdienst  Siri  bzw.  den  Google  Assistenten  für Android. [25]

Auch    in    anderen    Bereichen    finden    Sprachassistenten    Anwendung -angefangen von der Telefonhotline bis hin zur Nutzung im Auto zur Navigation, Musikauswahl oder Einstellung der Innenraumtemperatur. Amazon ermöglicht die Integration seines Alexa-Dienstes in Komponenten anderer Hersteller, was von der Autoindustrie flächendeckend genutzt wird. Die Bandbreite der Umsetzung geht dabei von der Abfrage des Benzinpreises über  das  Senden  von  Navigationszielen  von  zu  Hause über  einen  Alexa-Lautsprecher an das Auto bis zur Verwendung des,Sprachservice im Fahrzeug selbst.[54] [55]

Machine bzw. Deep Learning Technologien sind dabei ein Schlüsselfaktor für den Erfolg der Sprachassistenten. Der Amazon Alexa Service, der jede Spracheingabe an einen zentralen Server sendet und dort auswertet, nutzt jede nicht verstandene Eingabe zur Verbesserung der Spracherkennung.[56]

Neben der Fehlererkennung lernen die Amazon Services auch Im Bereich der Personalisierung weiter - seit 2018 kann das System spezifische Aufforderungen und Synonyme  lernen, und nicht bekannte Kommandos für eine bereits bekannte Aktion von selbst erlernen. Benutzen zwei Personen unterschiedliche Wörter für die selbe Aktion, lernt das System dazu.
Neben den zwei aufgezeigten Beispielen verwendete Amazon Alexa Machine Learning Technologien noch für weitere Verbesserungen an ihrem Ökosystem, angefangen von möglichen Verbesserungsvorschlägen bis hin zu signifikanten Verbesserungen im Verstehen natürlicher menschlicher Sprache.[57]

---

[51]
https://de.mathworks.com/discovery/deep-learning.html

[52]
https://cv-tricks.com/object-detection/faster-r-cnn-yolo-ssd/

[53]
https://www.pyimagesearch.com/2018/06/18/face-recognition-with-opencv-python-and-deep-learning/

[54]
https://www.wordstream.com/blog/ws/2018/04/10/voice-search-statistics-2018

[55]
https://www.amazon.de/b?ie=UTF8&node=10536641031

[56]
https://www.forbes.com/sites/bernardmarr/2018/10/05/how-does-amazons-alexa-really-work/

[57]
https://www.wired.com/story/amazon-alexa-2018-machine-learning/

---

[< Algorithmen](Algorithmen.md) | [Datenbanken und Frameworks>](Datenbanken_und_Frameworks.md)
