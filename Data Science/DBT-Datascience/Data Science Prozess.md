Data Science Prozess

[TOC]

* * *

# Datascience und Datenbanktechnologie

Während ein Großteil der Datascience Artikel/Ausbildung sich auf maschinelles Lernen, künstliche Intelligenz und Big Data konzentriert, werden die Vorraussetzungen dafür oftmals nicht behandelt. Bis zu 80% der Aufgaben betreffen die Datenbeschaffung, Strukturierung und Modellierung bzw. zusammenfassend das Datenmanagement. Allen voran ist hierbei die Datenhaltung (data storage) zu nennen. Das erste System wurd 1956 von der IBM bereitgestellt. Hier wollen wir auf den gesamten Prozess eingehen um ein übergreifendes Verständnis zu schaffen. 

## Daten Management
### Data Lake
Für diese Aufgabe wird seit 2010 auch verstärkt auf Data Lakes gesetzt. Diese können sowohl strukturierte, teil-strukturierte  als auch unstrukturierte Daten ablegen.

- Strukturierte Daten
-- Tabellen aus relationalen Datenbanken
- teil-strukturierte Daten
-- CSV-Dateien
-- Log-Dateien
-- XML-Daten
-- JSON-Daten
-- Sensordaten
-- Maschinendaten
- unstrukturierte Daten
-- Dokumente 
--- Emails
--- PDF
--- Office-Dokumente
-- Binärdateien
--- Bilder
--- Audio
--- Video

Das erlaubt insbesondere Daten bereitzustellen für die noch kein Anwendungszweck existiert. Somit stehen neben produktiv genutzten Daten auch ungenutzte Daten bereit. Das hat zum Ziel das sowohl die Entwicklung vereinfacht wird, als auch eine Verknüpfung neuer Daten mit bereits produktiv genutzten Daten.

### Data Warehouse
Im Geschäftsumfeld wird weiterhin stark auf das Data Warehouse gesetzt. Diese werden als höchst zuverlässige Datenquelle gehandelt. Daher sind die Anforderungen an die Qualität der Strukturen und den Wahrheitsgehalt der Daten entsprechend hoch. Die Daten müssen vor der Aufnahme ins Data Warehouse entsprechend der Anforderungen gereinigt und strukturiert werden. Üblicherweise werden die Daten aus relationalen Datenbanksystemen extrahiert.  Aus diesem Grund fällt der Nachteil der starren Strukturen weniger ins Gewicht. Swe gesamte Prozess findet in einer Umgebung statt, was die Bedienung, Überwachung und Konsistenz vereinfacht. Ein weiterer Gewinn ist die Möglichkeit das auch Endanwender Abfragen und Reports erzeugen können. Zum einen weil die Umgebung dafür ausgelegt ist (Anwenderfreundliche GUI Oberfläche, Zugriffssteuerung) und zum anderen weil die Qualität der Daten ein Überschaubares Maß an Kenntnissen der Informatik benötigt.

## Anbindung der Datenquellen 
Die erste Aufgabe besteht im Anschluss der Quelle an das Daten Management Systems. Neben dem einlesen klassicher Datenbanktabellen werden insbesondere bei Data Lakes auch Daten aus Log-dateien (Textformat), CSV-Dateien, Sensoren- und Maschinendaten.

Dazu können neben SQL Abfragen, Restful API, RFC-Konnektoren, API oder Microservices verwendet werden. Da Daten Änderungen unterworfen sind sollten ein regelmäßiges Update eingeplant werden. Die Häufigkeit richtet sich nach der Dynamik der Daten. Dabei sollte auch geprüft werden wie mit Löschungen umzugehen ist. Sollen zum Beispiel Unternehmen die Aufgelöst wurden weiter geführt werden, wie sollen Unternehmen behandelt werden die Fusionieren. Im Hinblick auf die DSGVO  muss auch geprüft werden ob Datensätze aus rechtlichen Gründen gelöscht bzw. anonymisiert werden müssen. 

Bei der Anbindung sollte darauf geachtet werden das:
- die Daten schreibgeschützt abgelegt werden. Eine Änderung sollte nur an Kopien der Daten durchgeführt werden.
- die Daten einem Format abgelegt werden das eine möglichst automatisierte Verarbeitung ermöglicht. Auch unformatierte Textdateien können in einer SQL Datenbank abgelegt werden, neben dem Schlüsselfeld (Dateiname, Datum, etc.) kann der Inhalt in einem Feld/Spalte abgelegt werden (Key/Value Store). 
Es gibt natürlich noch andere Lösungen, die Verwendung eines ELK Stacks (Elasticsearch, Logstash und Kibana), HDF5 Datei (Hierarchical Data Format), Quilt oder eine Raster Datenbank. Eine weiter einfache Möglichkeit ist die Verwendung von sqlLite. 
- die Daten so abgelegt werden das sie im gesamten Team verwendet werden können (NFS, S3, Git-LFS, Quilt, …). 
- die Daten so dokumentiert sind das die Bedeutung der Daten klar wird. Diese Beschreibung ist die Grundlage der späteren Strukturierung und unterstützt die Auswertung bzw. interpretation der Daten. Sie hilft außerdem die Versumpfung des Data Lakes zu einem Data Swamp zu vermeiden.

## Bereinigung der Quelldaten 
Im zweiten Schritt werden die Daten bereinigt. Die für die weitere Verarbeitung notwendigen Features werden identifiziert und als strukturierte Daten bereitgestellt. Dabei muss der Vorgang reversible und wartungsfreundlich bleiben. Da sich die Quelldaten ändern können (neue Versionen der angeschlossenen Systeme (Protokolldateien, zusätzliche Datenfelder/spalten usw.) müssen auch diese Prozessschritte weiterentwickelt werden können. Gegebenenfalls müssen auch die archivierten Quelldaten mit der neuen Version verarbeitet werden können. 
Um diese Anforderungen umzusetzen müssen die Prozesssachritte maschinell beschrieben und auch mehrere Ansätze parallel vorgehalten werden. Dazu können unter anderem folgende Lösungen verwendet werden:
- Makefiles
- DVC
- SQL Views
- Workflow Management System (Luigi, Airflow, Argo, ..)

## Modellierung der Daten
Das Modellieren der Daten ist eng verwoben mit der Datenbereinigung. Manches Modell ist so stark von dem Bereinigung abhängig das die Unterscheidung im Einzelfall nur einen geringen Mehrwert bringt. Auch weil Änderungen im Modell auch Änderungen in der Bereinigung notwendig machen können. Der Weg zum gewünschten Ergebniss erfordert in der Regel mehrere Experimente. Die Parameter und Ergebnisse sollten auch zukünftig Nachvollziehbar und Vergleichbar bleiben. Das Gewährleistet das Experimenten Management Auch diese Arbeiten werden in dem oben beschriebenen Computational Graph gespeichert.

## Produktiveinsatz

## Überwachung
SQLite file
an SQL database, the ELK stack, an HDF5 file or a raster database

