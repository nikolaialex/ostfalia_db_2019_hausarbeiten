# 4. Herausforderungen

In diesem Kapitel sollen die Herausforderungen erläutert werden, denen man bei Entwicklung und Betrieb verteilter Systeme üblicherweise begegnet.

<!-- ## 4.1 Heterogenität / Heterogeneity

## 4.2 Transparenz / Transparency

## 4.3 Offenheit / Openness

## 4.4 Nebenläufigkeit / Concurrency

## 4.5 Sicherheit / Security

## 4.6 Skalierbarkeit / Scalability

## 4.7 Fehlerbahndlung / Failure Handling -->

## 4.1 Architektur

In verteilten Systemen laufen die beteiligten Komponenten per Definition auf mehreren, unterschiedlichen Knoten.
Mit wachsender Anzahl von Komponenten und Knoten steigt die Komplexität dieser Systeme massiv an.

Um diese Komplexität angemessen zu handhaben, wird bei den verteilten Komponenten neben der rein physischen Verteilung die logische Organisation der Komponenten betrachtet. Diese logische Organisation der Komponenten und deren Interaktionen untereinander werden auch als **Software-Architekturen** bezeichnet.

Ein wichtiges Ziel bei der Erstellung von Software-Architekturen in verteilten Systemen ist die Trennung der Applikationen von der zugrunde liegenden Plattform. Hierzu wird häufig eine Schicht eingeführt, die als **middleware** bezeichnet wird und welche der Erlangung von Transparenz dient (siehe dazu den Abschnitt Ziele in [Verteilte Systeme](04_verteilte_systeme.md)).


## 4.2 Prozesse
## 4.3 Kommunikation
## 4.4 Namensgebung
## 4.5 Koordination
## 4.6 Konsistenz und Replikation
## 4.7 Fehlertoleranz
## 4.8 Sicherheit

---
[<< Verteilte Systeme](04_verteilte_systeme.md) | [Inhaltsverzeichnis](02_toc.md) | [Fallbeispiele >>](06_fallbeispiele.md)
