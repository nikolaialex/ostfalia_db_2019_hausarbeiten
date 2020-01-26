## 3.3 Checkliste <a id="3.3_Checkliste"></a>

Im Folgenden wird eine kurze Übersicht über notwendige Dienste für ein beziehungsweise mehrere Data Science Projekte gegeben.

### 3.3.1 Projektverzeichnis

Ein Verzeichnis, in dem alle Dateien zu dem Projekt abgelegt werden. Die Erreichbarkeit für das Team muss ebenso gewährleistet werden, wie der Schutz (Berechtigungen) und die Datensicherung. Eine Vereinbarung zur Ordnerstruktur ist dabei hilfreich, um langfristig Ordnung und den Suchaufwand so gering wie möglich zu halten.

### 3.3.2 Datenzugriff

Unabhängig davon, ob es sich um eine SQL Datenbank oder ein Verzeichnis mit Dateien handelt. Auch hier gilt, dass der Zugriff ermöglicht wird, gesteuert werden kann und dass regelmäßig Datensicherungen vorgenommen werden.

### 3.3.3 Versionierung

Der Quellcode, aber auch Daten, Modelle und Reports sowie Dokumentationen sollten versioniert werden. Beispielhaft seien an dieser Stelle Git, Wiki, DC und Qulit genannt, die eine Versionskontrolle anbieten.

### 3.3.4 Dokumentation

Das Thema wird oft vernachlässigt und dann im Laufe der Zeit bereut. Neben der klassischen Dokumentation können Metadaten über Features, Skripte, Datenstrukturen und Modelle maschinenlesbar (automatisiert) abgelegt werden.

### 3.3.5 Entwicklungsumgebung

Hier geschieht ein Großteil der Arbeit, verschiedene Ansätze werden ausprobiert und bewertet. Entweder werden die Programme zentral an lokale Clients verteilt oder man verwendet Serverdienste wie zum Beispiel  JupyterLab. Auch hier ist wichtig, dass mit den gleichen Versionen und Bibliotheken gearbeitet wird, es kann sonst zu Kompatibilitätsproblemen kommen. Außerdem muss auch  der Release neuer Versionen mit dem Entwicklungsprozess abgestimmt werden.

### 3.3.6 Jobverwaltung

Wird wenig Rechenleistung benötigt können die Aufgaben direkt am eigenen Rechner gelöst werden. Sind jedoch umfangreiche Rechenoperationen notwendig, lohnt sich der Einsatz leistungsfähiger Server und einer Software zur Verwaltung und Steuerung der Verarbeitungs- und Wartungsaufgaben.

### 3.3.7 Computation Graph

Wie wird der Computation Graph beschrieben und auch eine Reproduzierbarkeit sichergestellt.

### 3.3.8 Experiment manager

Der Fortschritt beim Training des Modells muss dokumentiert, visualisiert und ausgewertet werden.

### 3.3.9 Monitoring

Auch nach der Produktivsetzung muss die Leistungen dokumentiert werden, idealerweise in einem zentralen Leitstand (Dashboard).

| [&lt;&lt;&lt; Inhaltsverzeichnis](../README.md) | [&lt;&lt; 3.2 Der Prozess](./032_Der_Prozess.md) | Der Prozess | [4 Technologien &gt;&gt;](../Technologien/Technologien.md) |
|------------------------------------------------|---------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------|
