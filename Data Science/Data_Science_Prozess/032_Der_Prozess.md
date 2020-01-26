## 3.2 Der Prozess <a id="3.2_Der_Prozess"></a>

### 3.2.1 Anbindung der Datenquellen

Die erste Aufgabe besteht im Anschluss der Quelle an das Daten Management Systems. Neben dem Einlesen klassischer Datenbanktabellen werden, insbesondere bei Data Lakes, auch Daten aus Log-Dateien (Textformat), CSV-Dateien, Sensoren- und Maschinendaten gesammelt.

Dazu können neben SQL Abfragen, Restful [API](../Technologien/API.md), RFC-Konnektoren, API oder [Microservices](../Technologien/Microservice.md) verwendet werden. Da Daten Änderungen unterworfen sind, sollten ein regelmäßiges Update eingeplant werden. Die Häufigkeit richtet sich nach der Dynamik der Daten. Dabei sollte auch geprüft werden, wie mit Löschungen umzugehen ist. Werden Daten über Unternehmen gespeichert muss geklärt werden wie mit den Daten umgegangen wird wenn das Unternehmen zum Beispiel aufgelöst oder mit einem anderen fusioniert wurde.

Bei der Anbindung sollte darauf geachtet werden, dass:

- die Daten schreibgeschützt abgelegt werden. Eine Änderung sollte nur an Kopien der Daten durchgeführt werden.
- die Daten in einem Format abgelegt werden, das eine möglichst automatisierte Verarbeitung ermöglicht. Auch unformatierte Textdateien können in einer SQL Datenbank abgelegt werden. Neben dem Schlüsselfeld (Dateiname, Datum, etc.) kann der Inhalt in einem Feld/Spalte abgelegt werden (Key/Value Store).
Es gibt natürlich noch andere Lösungen, die Verwendung eines ELK Stacks (Elasticsearch, Logstash und Kibana), HDF5 Datei (Hierarchical Data Format), Quilt oder eine Raster Datenbank. Eine weitere einfache Möglichkeit ist die Verwendung von sqlLite.
- die Daten so abgelegt werden das sie im gesamten Team verwendet werden können (NFS, S3, Git-LFS, Quilt, …).
- die Daten so dokumentiert sind, dass die Bedeutung der Daten klar wird. Diese Beschreibung ist die Grundlage der späteren Strukturierung und unterstützt die Auswertung bzw. Interpretation der Daten. Sie hilft außerdem, die Versumpfung des Data Lakes zu einem Data Swamp zu vermeiden.

### 3.2.2 Bereinigung der Quelldaten

Im zweiten Schritt werden die Daten bereinigt. Die für die weitere Verarbeitung notwendigen Features werden identifiziert und als strukturierte Daten bereitgestellt. Dabei muss der Vorgang reversible und wartungsfreundlich bleiben. Da sich die Quelldaten ändern können (neue Versionen der angeschlossenen Systeme, Protokolldateien, zusätzliche Datenfelder/-spalten usw.), müssen auch diese Prozessschritte weiterentwickelt werden können. Gegebenenfalls müssen auch die archivierten Quelldaten mit der neuen Version verarbeitet werden können. Gerade bei längeren Projekten spielt dann auch die Fortführung der Dokumentation eine gewichtige Rolle.
In der Regel sollten die Quelldaten nicht geändert werden, das impliziert, dass alle Änderungen als Kopie oder View gespeichert werden sollten. Auch das Löschen von Daten sollte möglichst über ein Löschkennzeichen vorgenommen werden. Ausnahmen sind rechtliche oder ähnliche Forderungen (zum Beispiel DSGVO) zur Löschung oder Anonymisierung der Daten.
Um diese Anforderungen umzusetzen müssen die Prozesssachritte maschinell beschrieben und auch mehrere Ansätze parallel vorgehalten werden. Dies wird Computation Graph genannt. Zusätzlich wird die Möglichkeit geschaffen verschiedene Varianten zu speichern und die Effektivität zu vergleichen. Der Computation Graph kann auch in den weiteren Prozessschritten erweitert werden.
Dazu können unter anderem folgende Lösungen verwendet werden:

- Makefiles
- DVC
- SQL Views
- Workflow Management System (Luigi, Airflow, Argo, ..)

### 3.2.3 Modellierung der Daten

Das Modellieren der Daten ist eng mit der Datenbereinigung verwoben. Manches Modell ist so stark von dem Bereinigung abhängig, dass die Unterscheidung im Einzelfall nur einen geringen Mehrwert bringt. Auch weil Änderungen im Modell Änderungen in der Bereinigung notwendig machen können. Der Weg zum gewünschten Ergebnis erfordert in der Regel mehrere Experimente. Auch diese Arbeiten werden in dem oben beschriebenen Computation Graph gespeichert. Dadurch bleiben die verschiedenen Parameter und deren Ergebnisse zukünftig nachvollziehbar und vergleichbar. Das Gewährleistet das Experimenten Management.
Es gibt dazu unter anderem die folgenden Ansätze:

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
- Die Prozesskette, die schon bei der Bereinigung eingesetzt wurde
  - Makefiles
  - DVC
  - SQL Views
  - Workflow Management System

### 3.2.4 Produktiveinsatz

Selbstverständlich gibt es viele Möglichkeiten, die fertige Lösung produktiv zu nutzen. Die Anforderungen an den produktiven Betrieb sollten, wie in jedem Softwareprojekt, von Anfang an berücksichtigt werden. Neben den Technologien muss auch die Last und Pflege der Anwendung berücksichtigt werden. Der Lebenszyklus einer Software sollte berücksichtigt werden und auch die Möglichkeit einer späteren Migration darf nicht außer Acht gelassen werden.
Die Unterschiede zwischen Entwicklungs- und Produktionsumgebungen sollten beim Testen berücksichtigt werden. Im Idealfall steht eine Kopie der Produktionsumgebung zum Testen bereit. Beim Testen müssen auch die Ergebnisse geprüft werden. Auch wenn die Analyse durchläuft muss sichergestellt werden, dass die errechneten Werte den Erwartungen entsprechen. Indem die Daten aus der Entwicklung auch im Test verwendet werden kann sichergestellt werden, dass die Berechnung die exakt gleichen Ergebnisse liefert.
Abhängig von der gewählten Entwicklungsumgebung kann die Lösung zum Beispiel als REST Service oder Python Paket in die Entwicklungsumgebung transferiert werden. Unter anderem erlaubt Flask die Verwendung des trainierten Modells aus einer Pickle-Datei heraus. Ähnliches bietet auch ein TensorFlow Modell über TensorFlow Serving. Unter Java bietet sich die Verwendung der JPMML Bibliothek an, während mit SKompiler ein SQL Query erzeugt das direkt in die Anwendung übernommen werden kann.

### 3.2.5 Überwachung

Wie bei vielen Entwicklungsprojekten ist die Arbeit nicht mit der Produktivsetzung beendet. Vielmehr ändern sich Gegebenheiten, auf denen die Anforderungen basieren. Das führt regelmäßig dazu, dass sich die Anforderungen selbst ebenfalls ändern.
Die Quelle, auf denen die Daten und das Datenmodell basieren, können sich ändern, weil zum Beispiel, dass eine neue Version oder eine neue Software eingeführt wird. Es kann sich auch das Verhalten der beobachteten Vorgänge beziehungsweise Subjekte verändern.
Unabhängig davon, können im Bereich [Data Science](../Data_Science_Allgemein/01_Was_ist_Data_Science.md) neue Features  entwickelt werden.
Nicht zuletzt ist auch die Änderung der Infrastruktur zu berücksichtigen. Werden neue Versionen des Betriebssystems, der Server- und Programmumgebung eingesetzt, muss erneut getestet und gegebenenfalls entwickelt werden.
Das rückt auch die Dokumentation wieder in den Fokus. Wenn andere Kollegen die Wartung und Pflege der Lösung im Betrieb übernehmen beziehungsweise der Entwickler nach Monaten oder Jahren sich wieder mit der Lösung beschäftigen muss, spart die Dokumentation viel Zeit, Ärger und Frust.
Um eine konstante Qualitätskontrolle zu ermöglichen, sollten alle Anfragen und Ergebnisse protokolliert werden. Mit diesen Daten kann die Genauigkeit und die Justierung im zeitlichen Verlauf kontrolliert werden.
Alle Aufgaben sollten weiterhin dokumentiert werden, sei es, dass neue Algorithmen oder Daten ausprobiert werden oder einmalige Zusatzauswertungen vorgenommen werden müssen. Alles sollte an einem zentralen Ort mit einer standardisierten Struktur abgelegt werden.

| [&lt;&lt;&lt; Inhaltsverzeichnis](../README.md) | [&lt;&lt; 3.1 Daten Management](./031_Daten_Management.md) | Der Prozess | [3.3 Checkliste &gt;&gt;](./033_Checkliste.md) |
|------------------------------------------------|---------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------|
