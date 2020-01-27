[Inhaltsverzeichnis >>](02_toc.md)

# Aktuelle Data Mining Methoden
Wie bereits erwähnt, gewinnt Data Mining zunehmen an Bedeutung. Es wird einerseits immer einfacher große Mengen an Daten zu sammln und zu speichern, des Weiteren wird es für Unternehmen, getrieben durch den Wettbewerbsdruck, immer wichtiger, Wissen (Vorteile) aus diesen vorhandenen Datenmengen zu erzielen. Diese und weitere Aspekte erfordern spezielle, modernere Methoden, welche große Datenmengen schnell analysieren können, um einen Nutzen oder neue Erkenntnisse aus vorhandenen Daten gewinnen zu können, ohne die Muster im Vorfeld zu kennen. Data Mining im engeren Sinn (siehe Kapitel Einführung) ist der Teil des Prozesses (KDD), in dem die zuvor zusammengetragen Daten verarbeitet werden. Die Quelle der Daten (Textbasiert, SQL, noSQL, RDBMS etc.) ist dabei nicht weiter relevant und somit unterliegt man nicht den Restriktionen dieser (z. B. RDBMS und der definierte Abfragesprache SQL). Folgend werden ausgewählte Data Mining Methoden genauer betrachtet.
>Ein Auszug aus „Google Scholar“ für die Suchbegriffe (siehe Tabelle, Stand 10.01.2020), zeigt die Wichtigkeit von Data Mining und den ausgewählten Methoden:

Suchbegriff|Treffer|
---|--:|
data mining|3.550.000
data mining decision tree|1.140.000
data mining chaid|9.330
data mining cart|420.000
data mining id3|28.000
data mining c4.5|88.500
data mining olap|45.100
data mining neural network|986.000

## Entscheidungsbäume
Unter Entscheidungsbäume versteht man Klassifikationsregeln, die wie ein logischer Baum angeordnet sind. In dem Entscheidungsbaum beschreiben die Knoten Attribute, welche abgefragt werden. Die Kanten, oder auch Äste, beschreiben jeweils eine Entscheidung und verweisen auf einen weiteren Knoten. Der letzte Knoten, an dem keine Kanten mehr vorhanden ist, wird Blatt genannt. Die Blätter wiederum lassen sich in Gruppen einteilen und damit Klassifizieren. Die Wurzel beschreibt dabei das Attribut mit dem höchsten Informationsgehalt.
Folgend wird ein trivialer Entscheidungsbaum dargestellt und erläutert. Bei der Fragestellung ob die Kunden einer Bank einen Immobilienkredit erhalten sollen, stellt die Bank ein paar einfache Regeln auf. Als wichtigste Information wird der Familienstand angesehen. Über weitere Entscheidungen (z. B. das Alter und das Geschlecht) gelangt man letztendlich zu den Blättern des Baumes und der Entscheidungsfindung.

|![Entscheidungsbaum](https://github.com/lastviper/ostfalia_db_2019_hausarbeiten/blob/master/modern_data_mining/images/Entscheidungsbaum.png)|
|:--:|
|*Abbildung 4-1: Entscheidungsbaum*|

Aus den Datenbeständen der Bank lassen sich mit entsprechenden Algorithmen automatische Entscheidungsbäume erstellen. Die Algorithmen können die Attribute und die Regeln ermitteln. Dabei müssen im Mit Hilfe dieser Entscheidungsbäume können sich komplette neue Erkenntnisse über die Entscheidungsfindung herausfinden. Die Menge der möglichen Entscheidungen können schnell unübersichtlich und nicht mehr handelbar werden. Hier zeigt sich sehr gut, warum Data Mining dafür genutzt wird, neue Erkenntnisse aus bereits vorhandenen Daten automatisch zu erzielen. Automatisch generierte Entscheidungsbäume haben den Vorteil, dass sie weiterhin gut lesbar und verständlich sind.
Nachteilig kann sich einerseits die Klassifizierungsgüte und andererseits die Größe des Baumes auswirken. Die Güte kann bei den strikten Regeln des Algorithmus im vergleich zu der reellen Welt leiden. Z. B. können Attribute durch den Algorithmus zur selben Gruppe zugeordnet werden, weil in den Testdaten die Werte übereinstimmen, aber in der reellen Welt keine direkte Beziehung zueinander haben.
Die Baumgröße kann durch mehrere Faktoren zu groß werden. Wenn zu viele Attribute erkannt werden oder die Schwellwerte der Attribute zu gering gewählt ist. Man kann dann den Baum begrenzen und die Schwellwerte anpassen, um die Knotenanzahl pro Objekt zu begrenzen (Min sowie Max).
Einige Algorithmen für Entscheidungsbäume sind:
- CHAID (1964)
- CART (1984)
- ID3 (1986)
- C4.5 (1993)

CHAID (Chi-square Automatic Interaction Detectors) ist der älteste Algorithmus. Ihn zeichnet aus, dass er das Wachstum des Baums stoppt und er mit **kategorial skalierten** Variablen anstatt **metrisch skalierten** Variablen arbeitet. Das bedeutet, dass die Äste in Kategorien (gut, mittel, schlecht) gegliedert sind, anstatt z. B. mit Werten wie >40 Jahr arbeitet. Um die Attribute auszuwählen, wird mittels Chi-Quadrat-Unabhängigkeitstest eine Kennzahl zweier Variablen berechnet (auf die Berechnung wird aus Platzgründen nicht weiter eingegangen), welche die Abhängigkeit der Variablen wiedergibt. Anhand der Kennzahlen werden die Attribute ausgewählt. Der Vorteil bei CHAID besteht in der Kompaktheit der erzeugten Bäume.

CART (Classification and Regression Trees) ist ein rekursiver Partitionsalgorithmus und hat als Besonderheit, dass der Algorithmus nur Binärbäume erzeugt. Das bedeutet, dass jede Verzweigung genau zwei Äste besitzt oder ein Endknoten (Blatt) ist. Der Algorithmus versucht die Attribute so zu unterteilen, dass die verzweigten Knoten, bezogen auf die Gruppierung der Klassen, reiner werden als die Ausgangsmenge. Der Vorteil von CART besteht darin, dass die Attribute mit einem höheren Informationsgehalt in Bezug auf die Zielgröße weiter oben im Baum stehen.

ID3 (Iterative Dichotomiser 3) ist ein weiterer Algorithmus für Entscheidungsbäume. Er eignet sich besonders gut für schnelle Klassifikation großer Datenmengen mit vielen Attributen. Der ID3 verwendet eine Trainingsmenge (z. B. 10% der gesamten Datenmenge, „window-Technik“) um den Entscheidungsbaum zu erstellen. Dieses Verfahren ermöglicht eine schnelle Klassifizierung anhand des ersten Baumes. Falls es Objekte gibt, welche nicht in eine Klassifizierung passen, so wird der Entscheidungsbaum entsprechen angepasst und neu generiert.

#### Ablauf des ID3-Algorithmus
„Der ID3-Algorithmus kann generell wie folgt dargestellt werden:
Nachdem zunächst gemäß der "window-Technik" ein erster Entscheidungsbaum erstellt wurde, wird überprüft, ob alle Beispiele eines Knotens zur gleichen Klasse gehören. Ist dies der Fall, so ist der Entscheidungsbaum fertig.
Wenn nicht, so wird das informativste Merkmal ausgewählt und nach diesem verzweigt. Das heißt mit Hilfe dieses Merkmals werden die Beispiele des betrachteten Knotens in Untergruppen aufgeteilt, wobei in jeder dieser Untergruppen nur Beispiele mit gleichen Merkmalswerten vorkommen. Dies wird so lange wiederholt, bis alle Beispiele in den verschiedenen Untergruppen richtig klassifiziert wurden.
Das informativste Merkmal wird ermittelt, in dem für jedes Merkmal der Beispieldaten (nach dem noch nicht verzweigt wurde) berechnet wird, wie gut es die Daten klassifizieren würde. Diese Klassifikationsgüte wird von ID3 über das Informationskriterium gemessen. Dieses Kriterium quantifiziert die zur Klassifikation benötigte Information, d.h. die Anzahl der Tests, die nötig sind, um ein nicht klassifiziertes Objekt einer Klasse zuzuordnen. Ziel von ID3 ist es, die zur Klassifikation benötigte Information im Entscheidungsbaum zu minimieren.“ Zitat: http://www.optiv.de/Methoden/KlassMet/index.htm?16

Im ersten Schritt wird die Entropie der Wurzel des Baumes ausgerechnet. Die Formel dazu lautet:

*S*: sei die Menge der gesamten Stichprobe (Anzahl Datensätze)
*Z*: sei das Zielattribut nach dem Klassifiziert wird, hier Kreditwürdig „Kredit“ oder „kein Kredit“
*z*: Attributwerte von Z, „Ja“ und „Nein“
*p*: Wahrscheinlichkeit dafür, dass ein z vorliegt, Anzahl aller z geteilt durch
|![ID3 Entropie Wurzel](https://github.com/lastviper/ostfalia_db_2019_hausarbeiten/blob/master/modern_data_mining/images/id3_wurzel.png)|
|:--:|
|*Abbildung 4-2: ID3 Entropie Wurzel*|

Daraus erhält man die Entropie der Wurzel. Danach wird eine Entscheidung aller weiteren Attribute anhand des Gütemaßes „InfoGain“ getroffen. Die Kennzahl InfoGain beinhaltet den Informationsgehalt eines Attributes.
*A*: Attribut, dessen InfoGain berechnet werden soll (für jedes übrige Attribut)
*v*: Platzhalten für alle möglichen Attributwerte von A
*Sv*: Untermenge der Stichprobe S, deren Elemente beim Attribut A den Wert v aufweisen
*#*: Mächtigkeit der Mengen S und Sv
|![ID3 Entropie Attribut](https://github.com/lastviper/ostfalia_db_2019_hausarbeiten/blob/master/modern_data_mining/images/id3_attribut.png)|
|:--:|
|*Abbildung 4-2: ID3 Entropie Attribut*|

Das Attribut mit dem höchsten InfoGain wird ausgewählt und dem Baum hinzugefügt. Dieses Verfahren wird dann für alle restlichen Attribute wiederholt (greedy-Bestensuche).
Um den InfoGain zu berechnen, wird zu allen Attributwerten die Entropie ermittelt und mit der relativen Häufigkeit des Attributs gewichtet und aufsummiert. Somit ergibt sich für jedes Attribut ein InfoGain.
Der Algorithmus ist die Grundlage für weitere Algorithmen und durch seine Einfachheit ein weit verbreitetes Werkzeug. Er hat aber auch einige Nachteile:
-	es werden Attribute mit vielen Attributwerten begünstigt, was einen breiten Baum zur Folge haben kann
-	die greedy-Bestensuche bevorzugt nicht unbedingt den bestmöglichen (kleinsten) Baum
-	da der Baum solang weiterwächst, bis alle Daten so eindeutig wie möglich sind, besteht die Gefahr von Overfitting
C4.5, ebenfalls von Ross Quinlan (ID3), ist ein Algorithmus für Entscheidungsbäume. C4.5 verhält sich nahezu identisch zum CART-Algorithmus, mit der Ausnahme, dass die Beschränkung auf Binäre Äste nicht mehr existent ist. Als Erweiterung zum ID3, welcher ebenfalls von Ross Quinlan stammt, seien folgenden Merkmale bzw. Verbesserungen genannt (Quelle https://de.wikipedia.org/wiki/C4.5):
-	Anwendbarkeit auf diskrete als auch auf Kontinuierliche Attribute
-	Anwendbarkeit auf Trainingsdaten mit fehlenden Attributswerten – Unter Anwendung von C4.5 können nicht verfügbare Merkmalswerte als unbekannt (?) markiert werden. Unbekannte Werte werden bei der Berechnung des Information Gain einfach ignoriert
-	Mögliche Kostengewichtung der Attribute
-	Stutzen (Pruning) des Entscheidungsbaumes nach dessen Erstellung – Um die Anzahl der möglichen Ergebnisklassen der Klassifizierung zu verringern, schneidet C4.5 lange Äste ab. Dies verhindert außerdem Überanpassung an die Trainingsdaten
#### Fazit
Entscheidungsbäume eignen sich je nach Datengrundlage sehr gut für Data Mining vorhaben. Sie sind einfach zu Nutzen und bieten dabei eine gute Nutzbarkeit. Sie helfen dabei, die Daten zu analysieren und es lassen Aussagen über die Nützlichkeit der Daten zu. Zum Beispiel wo sich die Lösung zur gestellten Frage oder dem zu Untersuchenden Problem finden lassen. Sie finden Anwendung in der Wirtschaft sowie in der Wissenschaft. Sie können für Exploration und Datenverarbeitung verwendet werden. Trotz dieser Vielfalt sind die Entscheidungsregeln einfach zu Lesen und ihre hierarchische Anordnung leicht zu verstehen.
## OLAP
Online Analytical Processing ist eine Datenbanktechnologie und wird oftmals in der Wirtschaft in folgenden Bereichen: Controlling, Finanzabteilungen, Vertrieb, Produktion, Personal und Management genutzt. In diesen Bereichen wird OLAP für Berichtswesen und Analysen sowie Planung und Budgetierung eingesetzt. OLAP unterstützt Business Intelligenz und umfasst relationale Datenbanken und das Data Mining. Die Anforderungen sind, im Gegensatz zu den Entscheidungsbäumen, sehr verschieden. Bei einem OLAP System ist der Benutzer in ständiger Interaktion mit dem System. Es werden Fragen gestellt auf diese das System eine Antwort bereit stellt. Die Daten stammen dabei aus vorhanden Datenquellen, meist den operationalen Datenbeständen oder einem Data-Warehouse. Für ein OLAP-System werden die Daten umstrukturiert und in einen OLAP-Würfel gespeichert. Dieser Würfel ermöglicht es, Unternehmerische Kennzahlen wie Umsatz, Lagerbestand, Fertigungsbestand und Verkäufe bzw. Bestellungen (auch Fakten genannt) in Abhängigkeit von verschiedenen Dimensionen wie Zeitraum, Verkäufer, Logistiker, Werk und Produkt darzustellen. Somit lassen sich Abfragen wie:
-	Wieviel Schrauben befinden sich noch im Lager X
-	Welcher Standort hat Produkt Y am häufigsten verkauf
-	Welches Produkt hat in einem bestimmten Zeitraum den geringsten Umsatz
Es gibt verschiedene OLAP Systeme, ROLAP (relational), MOLAP (multidimensional) und HOLAP (hybriden). Alle drei Systeme haben Vor- und Nachteile. Allen gemeinsam ist, dass OLAP-Systeme für das Data Mining sehr interessant sind, wenn es um mehrdimensionale Auswertungen geht, bei denen sehr viele Datensätze anfallen. Hier bieten diese Systeme für Unternehmen einen großen Mehrwert gegenüber anderen Systemen. Im Idealfall sind sie sehr schnell und bieten ein flexibles Berichtswesen, welches eine intuitive Datenanalyse ermöglicht, erlauben einen Mehrbenutzerbetrieb und stellen die Informationen dem Benutzer Transparent zur Verfügung.
## Künstliches neuronales Netz
Mit Künstlichen neuronalen Netzen (KNN) können Muster in Daten finden bzw. erkennen oder komplexe Beziehungen zwischen Eingabe und Ausgabe identifizieren. Sie werden als nichtlineare statistische Datenmodellierungswerkzeuge angesehen. In einem studentischen Projekt sollten Gruppen eine Fragestellung mit Hilfe von künstlichen neuronalen Netzen lösen. Zu Grunde lagen dabei Datenbestände mit teilweise unvollständigen Daten. Das Projekt wurde in der Hochschule Wismar University of Technology, Business and Design, Fachbereich Wirtschaft, in dem Wahlpflichtfach Spezialisierung Wissensmanagement/Wissensbasierte Systeme des Diplomstudiengang Wirtschaftsinformatik durchgeführt. Die Projektgruppen haben alle den JavaNNS zur Analyse genutzt. Die Daten mussten für die Verwendung eines KNN aufbereitet werden, wobei es zu untersuchen gab, ob alle vorhandenen Daten genutzt werden müssen und wie mit fehlenden Daten umgegangen werden muss. Beides kann bei der Güte der Ergebnisse eine große Rolle spielen. Die Gruppen hatten keine weiteren Bedingungen, neben der Verwendung eines künstlichen neuralen Netzwerks. Die Ergebnisse der Gruppen variieren dabei sehr, was den Zielwert betrifft. Interessante Ergebnisse brachte eine Gruppe hervor, welche die Standard-Schwellwerte des Ausgabe-Neurons verändert hatten. Angefangen haben alle Gruppen mit einem vorwärts gerichteten Netz. Beispielhalber sei eine Codierung von 32 Neuronen (Eingabe-Schicht), zwei Neuronen (verdeckte Schicht) und ein Ausgabe-Neuron gegeben. In der Optimierungsphase hat eine Gruppe auf eine innere Schicht verzichtet und damit ein besseres Ergebnis erzielt als zuvor. Weitere interessante Aspekte wurden gemacht, als dass ein Netz, welches aus nur 4 Eingabe-Neuronen besteht, ein sehr gutes Ergebnis erzielt hat. Dieses Schulprojekt soll als Beispiel gelten, was künstliche neuronale Netze im Stande zu leisten sind.  Die Ergebnisse übertrafen die Zielwerte des Data Mining Cups von 2002.
Moderne Data Mining Methoden können in nahezu allen Bereichen, wie z. B. Wissenschaft und Wirtschaft, Anwendung finden bei denen große Datenmengen zugrunde liegen.

Quellenangaben

https://novustat.com/statistik-blog/data-mining-methoden-ueberblick.html
http://www.optiv.de/Methoden/KlassMet/index.htm
http://www.wiete.com.au/journals/GJEE/Publish/vol8no3/Laemmel.pdf
http://www.mathematik.uni-ulm.de/sai/ws03/dm/arbeit/duerr.pdf
https://www.in.th-nuernberg.de/professors/Holl/Personal/DataMining_Bachelor.pdf
https://data-science-blog.com/blog/2017/08/13/entscheidungsbaum-algorithmus-id3/
https://de.wikipedia.org/wiki
http://www.optiv.de/Methoden/KlassMet/index.htm
http://www.is.ovgu.de/is_media/Teaching/Seminar/Classification+Algorithms/Decision+Trees-p-4310.pdf
http://www.wiete.com.au/journals/GJEE/Publish/vol8no3/Laemmel.pdf

***
| [<< Data Mining Methoden](05_methoden.md) |
***
