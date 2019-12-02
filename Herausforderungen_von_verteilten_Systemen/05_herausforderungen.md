[<< Verteilte Systeme](04_verteilte_systeme.md) | [Inhaltsverzeichnis](02_toc.md) | [Fallbeispiele >>](06_fallbeispiele.md)
|-|-|-|

---

# 3. Herausforderungen

In diesem Kapitel sollen die Herausforderungen erläutert werden, denen man bei Entwicklung und Betrieb verteilter Systeme üblicherweise begegnet.

<!-- ## 4.1 Heterogenität / Heterogeneity

## 4.2 Transparenz / Transparency

## 4.3 Offenheit / Openness

## 4.4 Nebenläufigkeit / Concurrency

## 4.5 Sicherheit / Security

## 4.6 Skalierbarkeit / Scalability

## 4.7 Fehlerbahndlung / Failure Handling -->

## 3.1 Architektur

In verteilten Systemen laufen die beteiligten Komponenten per Definition auf mehreren, unterschiedlichen Knoten.
Mit wachsender Anzahl von Komponenten und Knoten steigt die Komplexität dieser Systeme massiv an.

Um diese Komplexität angemessen zu handhaben, wird bei den verteilten Komponenten neben der rein physischen Verteilung die logische Organisation der Komponenten betrachtet. Diese logische Organisation der Komponenten und deren Interaktionen untereinander werden auch als **Software-Architekturen** bezeichnet.

Ein wichtiges Ziel bei der Erstellung von Software-Architekturen in verteilten Systemen ist die Trennung der Applikationen von der zugrunde liegenden Plattform. Hierzu wird häufig eine Schicht eingeführt, die als **middleware** bezeichnet wird und welche der Erlangung von Transparenz dient (siehe dazu den Abschnitt Ziele in [Verteilte Systeme](04_verteilte_systeme.md)).


## 3.2 Prozesse
## 3.3 Kommunikation
## 3.4 Namensgebung
## 3.5 Koordination
## 3.6 Konsistenz und Replikation
## 3.7 Fehlertoleranz
## 3.8 Sicherheit

---
[<< Verteilte Systeme](04_verteilte_systeme.md) | [Inhaltsverzeichnis](02_toc.md) | [Fallbeispiele >>](06_fallbeispiele.md)
|-|-|-|
