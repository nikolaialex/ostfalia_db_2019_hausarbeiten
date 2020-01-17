Data Science Prozess

[TOC]

* * *

# Datascience und Datenbanktechnologie

Während ein Großteil der Datascience Artikel/Ausbildung sich auf maschinelles Lernen, künstliche Intelligenz und Big Data konzentriert, werden die Vorraussetzungen dafür oftmals nicht behandelt. Bis zu 80% der Aufgaben betreffen die Datenbeschaffung, Strukturierung und Modellierung bzw. zusammenfassend das Datenmanagement. Allen voran ist hierbei die Datenhaltung (data storage) zu nennen. Das erste System wurd 1956 von der IBM bereitgestellt. Hier wollen wir auf den gesamten Prozess eingehen um ein übergreifendes Verständnis zu schaffen. 

## Daten Management
### Data Lake
Für diese Aufgabe wird seit 2010 auch verstärkt auf Data Lakes gesetzt. Diese können sowohl strukturierte, teil-strukturierte  als auch unstrukturierte Daten ablegen.

- Strukturierte Daten
  - Tabellen aus relationalen Datenbanken
- teil-strukturierte Daten
  - CSV-Dateien
  - Log-Dateien
  - XML-Daten
  - JSON-Daten
  - Sensordaten
  - Maschinendaten
- unstrukturierte Daten
  - Dokumente 
    - Emails
    - PDF
    - Office-Dokumente
  - Binärdateien
    - Bilder
    - Audio
    - Video

Das erlaubt insbesondere Daten bereitzustellen für die noch kein Anwendungszweck existiert. Somit stehen neben produktiv genutzten Daten auch ungenutzte Daten bereit. Das hat zum Ziel das sowohl die Entwicklung vereinfacht wird, als auch eine Verknüpfung neuer Daten mit bereits produktiv genutzten Daten.

### Data Warehouse
Im Geschäftsumfeld wird weiterhin stark auf das Data Warehouse gesetzt. Diese werden als höchst zuverlässige Datenquelle gehandelt. Daher sind die Anforderungen an die Qualität der Strukturen und den Wahrheitsgehalt der Daten entsprechend hoch. Die Daten müssen vor der Aufnahme ins Data Warehouse entsprechend der Anforderungen gereinigt und strukturiert werden. Üblicherweise werden die Daten aus relationalen Datenbanksystemen extrahiert.  Aus diesem Grund fällt der Nachteil der starren Strukturen weniger ins Gewicht. Swe gesamte Prozess findet in einer Umgebung statt, was die Bedienung, Überwachung und Konsistenz vereinfacht. Ein weiterer Gewinn ist die Möglichkeit das auch Endanwender Abfragen und Reports erzeugen können. Zum einen weil die Umgebung dafür ausgelegt ist (Anwenderfreundliche GUI Oberfläche, Zugriffssteuerung) und zum anderen weil die Qualität der Daten ein Überschaubares Maß an Kenntnissen der Informatik benötigt.

## Anbindung der Datenquellen 
Die erste Aufgabe besteht im Anschluss der Quelle an das Daten Management Systems. Neben dem einlesen klassicher Datenbanktabellen werden insbesondere bei Data Lakes auch Daten aus Log-dateien (Textformat), CSV-Dateien, Sensoren- und Maschinendaten.

Dazu können neben SQL Abfragen, Restful API, RFC-Konnektoren, API oder Microservices verwendet werden. Da Daten Änderungen unterworfen sind sollten ein regelmäßiges Update eingeplant werden. Die Häufigkeit richtet sich nach der Dynamik der Daten. Dabei sollte auch geprüft werden wie mit Löschungen umzugehen ist. Sollen zum Beispiel Unternehmen die Aufgelöst wurden weiter geführt werden, wie sollen Unternehmen behandelt werden die Fusionieren. 

Bei der Anbindung sollte darauf geachtet werden das:
- die Daten schreibgeschützt abgelegt werden. Eine Änderung sollte nur an Kopien der Daten durchgeführt werden.
- die Daten einem Format abgelegt werden das eine möglichst automatisierte Verarbeitung ermöglicht. Auch unformatierte Textdateien können in einer SQL Datenbank abgelegt werden, neben dem Schlüsselfeld (Dateiname, Datum, etc.) kann der Inhalt in einem Feld/Spalte abgelegt werden (Key/Value Store). 
Es gibt natürlich noch andere Lösungen, die Verwendung eines ELK Stacks (Elasticsearch, Logstash und Kibana), HDF5 Datei (Hierarchical Data Format), Quilt oder eine Raster Datenbank. Eine weiter einfache Möglichkeit ist die Verwendung von sqlLite. 
- die Daten so abgelegt werden das sie im gesamten Team verwendet werden können (NFS, S3, Git-LFS, Quilt, …). 
- die Daten so dokumentiert sind das die Bedeutung der Daten klar wird. Diese Beschreibung ist die Grundlage der späteren Strukturierung und unterstützt die Auswertung bzw. interpretation der Daten. Sie hilft außerdem die Versumpfung des Data Lakes zu einem Data Swamp zu vermeiden.

## Bereinigung der Quelldaten 
Im zweiten Schritt werden die Daten bereinigt. Die für die weitere Verarbeitung notwendigen Features werden identifiziert und als strukturierte Daten bereitgestellt. Dabei muss der Vorgang reversible und wartungsfreundlich bleiben. Da sich die Quelldaten ändern können (neue Versionen der angeschlossenen Systeme (Protokolldateien, zusätzliche Datenfelder/spalten usw.) müssen auch diese Prozessschritte weiterentwickelt werden können. Gegebenenfalls müssen auch die archivierten Quelldaten mit der neuen Version verarbeitet werden können. Gerade bei längeren Projekten spielt dann auch die fortführung der Dokumentation ein gewichtige Rolle.
In der Regel sollten die Quelldaten nich geändert werden, das impliziert das alle Änderungen als Kopie oder View gespeichert werden sollten. Auch das löschen von Daten sollte möglichst über ein Löschkennzeichen vorgenommen werden. Ausnahmen sind rechtliche oder ähnliche Forderungen (zum Beispiel DSGVO) zur Löschung oder Anonymisierung der Daten. 
Um diese Anforderungen umzusetzen müssen die Prozesssachritte maschinell beschrieben und auch mehrere Ansätze parallel vorgehalten werden. Dazu können unter anderem folgende Lösungen verwendet werden:
- Makefiles
- DVC
- SQL Views
- Workflow Management System (Luigi, Airflow, Argo, ..)


## Modellierung der Daten
Das Modellieren der Daten ist eng verwoben mit der Datenbereinigung. Manches Modell ist so stark von dem Bereinigung abhängig das die Unterscheidung im Einzelfall nur einen geringen Mehrwert bringt. Auch weil Änderungen im Modell auch Änderungen in der Bereinigung notwendig machen können. Der Weg zum gewünschten Ergebniss erfordert in der Regel mehrere Experimente. Auch diese Arbeiten werden in dem oben beschriebenen Computation Graph gespeichert. Dadurch bleiben die verschiedenen Parameter und dere Ergebnisse zukünftig Nachvollziehbar und Vergleichbar. Das Gewährleistet das Experimenten Management.
Es gibt dazu unter anderem die folgenden Ansätze.

- Pickle Files
  - Export von Python Objekten in eine Datei, erlaubt das Sichern der Ergebnisse der Experimente zur späteren Analyse und Selektion
- Experiment Management Tools
  - ModelDB
  - TensorBoard
  - Sacred
  - FGLab
  - Hyperdash
  - Floyd Hub
  - Comet.ML
  - DatMo
  - MLFlow
- Die Prozesskette die schon bei der Bereinigung eingesetzt wurde
  - Makefiles
  - DVC
  - SQL Views
  - Workflow Management System

## Produktiveinsatz
Selbstverständlich gibt es viele Möglichkeiten die fertige Lösung produktiv zu nutzen. Die Anforderungen an den produktiven Betrieb sollten, wie in jedem Softwareprojekt, von Anfang an berücksichtigt werden. Neben den Technologien muss auch die Last und Pflege der Anwendung berücksichtigt werden. Der Lebenszyklus einer Software sollte berücksichtigt werden und auch die Möglichkeit einer späteren Migration darf nicht außer Acht gelassen werden.
Die Unterschiede zwischen Entwicklungs- und Produktionsumgebunge sollten beim Testen berücksichtigt werden. Im idealfall steht eine Kopie der Produktionsumgebung zum Testen bereit. Beim Testen müssen auch die Ergebnisse geprüft werden. Auch wenn die Analyse durchläuft muss sichergestellt werden das die errechneten Werte den Erwartungen entsprechen. Wenn die Daten aus der Entwicklung auch im Test verwendet werden kann sichergestellt werden das die Berechnung die exakt gleichen Ergebnisse liefert.
Abhängig von der gewählten Entwicklungsumgebung kann die Lösung zum Beispiel als REST Service oder Python Paket in die Entwicklungsumgebung transferiert werden. Unter anderem erlaubt Flask die Verwendung des trainierten Modells aus einer Pickle-Datei heraus. Ähnliches bietet auch ein TensorFlow Modell über Tensorflow Serving. Unter Java bietet sich die Verwendung der JPMML Bibliothek an, während mit SKompiler ein SQL Query erzeugt das direkt in die Anwendung übernommen werden kann. 

## Überwachung
Wie bei vielen Software Entwicklungen ist die Arbeit nicht mit der Produktivsetzung beendet. Vielmehr ändern sich Gegebenheiten auf die die Anforderungen basieren. Das führt regelmäßig dazu das sich die Anforderungen selbst ebenfalls ändern. 
Die Quelle auf die Daten und Modell basieren können sich ändern weil neue Version oder neue Software eingeführt werden. Es kann sich auch das Verhalten der beobachten Vorgänge bzw. Subjekte verändern. 
Unabhängig davon ob Im Bereich Data Science können neue Features dazukommen.
Nicht zuletzt ist auch die Änderung der Infrastruktur zu berücksichtigen. Werden neue Version des Betriebssystems, der Server- und Programmumgebung eingesetzt muss wieder getestet und gegebenefalls entwickelt werden.
Das Rückt auch die Dokumentation wieder in den Fokus. Wenn andere Kollegen die Wartung und Pflege der Lösung im Betrieb übernehmenbzw. der Entwickler nach Monaten oder Jahren sich wieder mit der Lösung beschäftigen muss, spart die Dokumentation vie Zeit, Ärger und Frust.
Um eine konstante QUalitätskontrolle zu ermöglichen sollten alle Anfragen und Ergebnisse protokolliert werden. Mit diesen Daten kann eine die Genauigkeit und die Justierung im zeitlichen Verlauf kontrolliert werden.
Alle Aufgaben sollten weiterhin dokumentiert werden, sei es das neue Algorithmen oder Daten ausprobiert werden oder einmalige Zusatzauswertungen. Alles sollte an einem zentralen Ort mit einer standardisierten Struktur abgelegt werden.

## Checkliste 
Eine kurze übersicht über notwendige Dienste für ein bzw. mehrere Data Science Projekte

### Projekt Verzeichnis
Ein Verzeichnis in dem alle Dateien zu dem Projekt abgelegt werden. Die Erreichbarkeit für das Team muss ebenso gewährleistet werden wie der Schutz (Berechtigungen) und die Datensicherung. Eine Vereinbarung zur Ordnerstruktur ist dabei hilfreich um langfristig Ordnung zu halten und den Suchaufwand so gering wie möglich zu halten.

### Datenzugriff
Zugriff auf die Daten des Projektes. Unabhängig davon ob es eine SQL Datenbank ist oder ein Verzeichnis mit Dateien. Auch hier gilt das der Zugriff für das Team möglich sein muss, gesteuert werden kann und das regelmäßig Datensicherungen vorgenommen werden.

### Versionierung
Der Quellcode, aber auch Daten, Modelle und dazugehörige Reports und Dokumentation sollten Versioniert werden. Git, Wiki, DVC, Qulit bieten Versionskontrolle an.

### Dokumentation
Das Thema wird of vernachlässigt und dann im Laufe der Zeit bereut. Neben der klasischen Dokumentation können  Metadatan über Features, Sckripte, Datenstrukturen und Modelle maschinenlesbar (automatisiert) abgelegt werden.

### Entwicklungsumgebung
Hier geschieht ein Großteil der Arbeit, verschiedene Ansätze werden ausprobiert und bewertet. Werden die Programme zentral an lokale Clients verteilt oder gibt es eine Server für Jupyter Lab. Auch hier ist wichtig das mit den gleichen VErsionen und Bibliotheken gearbeitet wird. Sonst kommte es zu Kombatibilitätsproblmen. Auch muss der Release neuer Versionen mit dem Entwicklungsprozess abgstimmt werden.

### Hintergrund/Job Verarbeitung
Können alle Aufgaben lokal gelöst werden oder werden große Verarbeitungsaufträge zentral auf einem Server abgearbeitet. Wie wird die Auftragsvarteschlange gesteuert, inklusiver der Wartungsaufträge.

### Computation Graph
Wie wird der Computation Graph beschrieben und auch eine Reproduzierbarkeit sichergestellt.

### Experiment manager
Der Fortschritt beim Training des Modells muss dokumentiert, visualisiert und ausgewertet werden.

### Monitoring 
Auch nach der Produktivsetzung muss die Leistungen dokumentiert werden, idealerweise in einem Zentralen Leitstand (Dashboard).