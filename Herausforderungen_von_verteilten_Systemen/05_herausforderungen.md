[<< Verteilte Systeme](04_verteilte_systeme.md) | [Inhaltsverzeichnis](02_toc.md) | [Fallbeispiele >>](06_fallbeispiele.md)
|-|-|-|

---

# 3. Herausforderungen

In diesem Kapitel sollen die Herausforderungen erläutert werden, denen man bei Entwicklung und Betrieb verteilter Systeme üblicherweise begegnet.

## 3.1 Architektur

In verteilten Systemen werden die beteiligten Komponenten per Definition häufig auf mehreren, unterschiedlichen Knoten ausgeführt. Mit wachsender Anzahl von Komponenten und Knoten steigt die Komplexität dieser Systeme massiv an.

Um diese Komplexität angemessen zu handhaben, wird bei den verteilten Komponenten neben der rein physischen Verteilung die logische Organisation der Komponenten betrachtet. Diese logische Organisation der Komponenten und deren Interaktionen untereinander werden auch als **Software-Architekturen** bezeichnet.

Ein wichtiges Ziel bei der Erstellung von Software-Architekturen in verteilten Systemen ist die Trennung der Applikationen von der zugrunde liegenden Plattform. Hierzu wird häufig eine Schicht eingeführt, die als **middleware** bezeichnet wird und welche der Erlangung von Transparenz dient (siehe dazu den Absatz "Ziele" in [Verteilte Systeme](04_verteilte_systeme.md)).

### 3.1.1 Architectural styles

### 3.1.2 Middleware organization

### 3.1.3 System architecture

### 3.1.4 Example architectures




## 3.2 Prozesse
Prozesse sind in verteilten Systemen die Basis für die Kommunikation zwischen verschiedenen Maschinen.

### 3.2.1 Threads

### 3.2.2 Virtualisierung

### 3.2.3 Client / Server

### 3.2.4 Code Migration

## 3.3 Kommunikation

Der Kommunikation zwischen den beteiligten Komponenten sowie mit der Außenwelt kommt in verteilten Systemem besondere Bedeutung zu. Während in monolithischen Systemen die Kommunikation noch über Funktionsaufrufe und / oder lokale PubSub-Mechanismen u.ä. ausreicht, ist dies bei verteilten Multicomputersystemen nicht mehr gegeben.

Während in herkömmlichen verteilten Systemen die Netzwerkkommunikation noch über Mechanismen der Transportschicht wie einfaches Message Passing erfolgt, werden in middleware-basierten Systemen abstraktere Mechanismen angeboten, um die Kommunikation zu vereinfachen. Diese Mechanismen lassen sich grob in die drei Gruppen **Remote Procedure Calls**, **Message-Oriented Communication** und **Multicast Communication** unterteilen.

### 3.3.1 Remote Procedure Calls



### 3.3.2 Message-Oriented Communication

### 3.3.3 Multicast Communication



## 3.4 Namensgebung

TODO: Vllt rauslassen???

### 3.4.1 Flat naming

### 3.4.2 Structured naming

### 3.4.3 Attributed based naming



## 3.5 Koordination

### 3.5.1 Clock Synchronization

### 3.5.2 Logical Clocks

### 3.5.3 Mutual Exclusion

### 3.5.4 Election Algorithms

### 3.5.5 Location Systems

### 3.5.6 Distributed Event Matching

### 3.5.7 Gossip-based Communication



## 3.6 Konsistenz und Replikation

In verteilten System werden Daten üblicherweise repliziert. Dies hat hauptsächlich zwei Gründe: zum einen soll hierdurch die Verfügbarkeit und Verlässlichkeit des verteilten Systems verbessert werden, zum anderen soll die Leistung dessen gesteigert werden. Ein großes Problem hierbei ist es, die Replikate konsistent zu halten. Sobald ein Replikat geändert wird, unterscheidet es sich von den übrigen Beständen. Um die Konsistenz wieder herzustellen, müssen Änderungen in den übrigen Replikaten übernommen werden, idealerweise ohne, dass diese temporäre Inkonsistenz bemerkt wird. Dies kann je nach Größe und Aufbau des verteilten System zu massiven Leistungsproblemen führen. Eine mögliche Lösung hierfür ist es, anstelle einer sofortigen, unmittelbaren Konsistenz, eine verzögerte Konsistenz in Kauf zu nehmen.

### 3.6.1 Data-centric consistency models

### 3.6.2 Client-centric consistency models

### 3.6.3 Replica Management

### 3.6.4 Consistency Protocols



## 3.7 Fehlertoleranz

### 3.7.1 Process resilience

### 3.7.2 Reliable client-server communication

### 3.7.3 Reliable group communication

### 3.7.4 Distributed commit

### 3.7.5 Recovery



## 3.8 Sicherheit

### 3.8.1 Secure channels

### 3.8.2 Access control

### 3.8.3 Secure naming

### 3.8.4 Security management

[<< Verteilte Systeme](04_verteilte_systeme.md) | [Inhaltsverzeichnis](02_toc.md) | [Fallbeispiele >>](06_fallbeispiele.md)
|-|-|-|

