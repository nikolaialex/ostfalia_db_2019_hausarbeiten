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

```
TODO: Überarbeiten
Während in herkömmlichen verteilten Systemen die Netzwerkkommunikation noch über Mechanismen der Transportschicht wie einfaches Message Passing erfolgt, werden in middleware-basierten Systemen abstraktere Mechanismen angeboten, um die Kommunikation zu vereinfachen.
```

 Diese Mechanismen lassen sich grob in die drei Gruppen **Remote Procedure Calls**, **Message-Oriented Communication** und **Multicast Communication** unterteilen.

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

In verteilten System werden Daten üblicherweise repliziert. Dies hat hauptsächlich zwei Gründe: zum einen soll hierdurch die Verfügbarkeit und Verlässlichkeit des verteilten Systems verbessert werden, zum anderen soll die dessen Leistung gesteigert werden.

Ein großes Problem hierbei ist es, die Replikate konsistent zu halten. Sobald ein Replikat geändert wird, unterscheidet es sich von den übrigen Beständen. Um die Konsistenz wieder herzustellen, müssen Änderungen in den übrigen Replikaten übernommen werden, idealerweise ohne, dass diese temporäre Inkonsistenz bemerkt wird. Dies kann je nach Größe und Aufbau des verteilten System zu massiven Leistungsproblemen führen.

Eine mögliche Lösung ist es, anstelle einer sofortigen, unmittelbaren Konsistenz, eine verzögerte Konsistenz in Kauf zu nehmen und diese kontinuierlich wiederherzustellen. Die Unterschiede zwischen dem beabsichtigten und dem tatsächlichen Zustand der Daten können als Abweichungen bezeichnet werden. Diese Abweichungen treten insbesondere in Form von drei Typen auf:

-  numerische Abweichungen: Abweichungen zwischen den Replikas bezogen auf Werte
- veraltete (für engl. stale) Abweichungen: gemeint sind zeitliche Unterschiede, mit denen Updates auf die verschiedenen Replikas angewendet wurden
- Abweichungen in der Reihenfolge der Operationen: bezeichnet die Anzahl an ausstehenden Schreiboperationen je Server, welche noch nicht mit den anderen Replika-Servern synchronisiert wurden; hierbei kann es vorkommen, dass zeitlich jüngere Operation vor zeitlich älteren vorgenommen wurden

Um den verschiedenen Anforderungen an die Konsistenz gerecht zu werden, gibt es diverse Konsistenzmodelle und verschiedene Wege, diese zu implementieren. Im Folgenden sollen verschiedene Modell betrachtet werden.

### 3.6.1 Datenzentrierte Konsistenzmodelle

Traditionell wurde Konsistenz in Bezug auf Lese- und Schreiboperationen betrachtet, welche auf verteilten und gemeinsamen Datenbanken, Dateisystemen oder Speicher durchgeführt wurden. Für diese Datenhaltungsoptionen kann auch der allgemeinere Begriff **Data Store** verwendet werden.

Grundsätzlich wird in diesen Modellen davon ausgegangen, dass Prozesse, die Zugriff auf die Daten haben, eine lokale oder nahe Kopie des gesamten Data Store haben. Schreiboperationen werden dann zu den anderen Kopien verbreitet. Ein Konsistenzmodell ist aus dieser Sicht ein Vertrag zwischen den Prozessen und dem Data Store. Solange die Prozesse sich an bestimmte Regeln halten, verspricht der Data Store korrekt zu arbeiten und bei Lese-Operationen das Ergebnis der letzten Schreiboperationen zurückzugeben.

Viele Modelle aus dieser Kategorie beschäftigen sich mit der **korrekten Anordnung von Operationen** auf gemeinsam genutzten, verteilten Daten. Im Kern geht es bei diesen Modellen darum, dass die Replikate sich auf eine global gültige, konsistente Reihenfolge der Operationen einigen müssen.

#### Sequentielle Konsistenz

Ein wichtiges Modell der datenzentrierten Modelle ist das Modell der sequentiellen Konsistenz nach [Lamport, 1979]. Nach diesem Modell ist jede gültige Verschränkung von Lese- und Schreiboperationen für nebenläufige Prozesse auf unterschiedlichen Maschinen akzeptabel, solange alle Prozesse dieselbe Verschränkung sehen.

Als Beispiel für dieses Modell soll die nachfolgende Grafik aus [van Steen, 2017] dienen.

![sequential_consistency_lamport](./assets/sequential_consistency_lamport.jpeg)

In der linken Abbildung (a) wird ein sequentiell konsistenter Zustand angezeigt. Prozess 1 schreibt zunächst den Wert a, zeitlich später schreibt Prozess 2 den Wert b. Prozess 3 und 4 lesen zunächst den Wert b, anschließend den Wert a. Nach dem Modell des sequentiellen Konsistenz gilt dieser Zustand als gültig, da die Prozesse 3 und 4 die gleiche Sicht auf die Daten haben.

Im Gegensatz dazu verdeutlicht die rechte Abbildung (b) einen inkonsistenten Zustand. Prozess 3 und Prozess 4 haben eine unterschiedliche Sicht auf die Daten und gehen von unterschiedlichen Werten aus.

#### Letztendliche Konsistenz

Eine weitere Form der datenzentrierten Konsistenz ist die der **letztendlichen Konsistenz** (engl. eventual consistency) nach [Vogels, 2009]. Hiermit ist gemeint, dass alle Replikate im Data Store letztendlich den zuletzt geänderten Wert zurückgeben, solange dieser nicht verändert wird. Im Kern bedeutet dies, dass eine Änderung eines Objektes garantiert irgendwann an alle Replikate propagiert wird, also dass alle Replikate letztendlich zu einem Zustand konvergieren.

Dieses Modell kann aus Client-Sicht Nachteile haben. Ein Client wird bei Benutzung des Systems mit einem Replikat verbunden sein und Änderungen veranlassen. Verbindet sich der Client dann über ein anderes Gerät oder an einem anderen Ort mit dem Systems, kann der Client mit einem anderen Replikat verbunden sein, auf dem die Änderungen noch nicht angelangt sind. Dies kann zu inkonsistentem Verhalten führen.



### 3.6.2 Clientzentrierte Konsistenzmodelle

Um die Nachteile der *eventual consistency* aufzuwiegen, wurden clientzentrierte Konsistenzmodelle entwickelt. Diese garantieren im Kern dafür, dass Zugriffe eines einzelnen Clients auf den Data Store konsistent sind. Diese Garantie gilt jedoch *nicht* für nebenläufige Zugriffe mehrerer Clients.

Die clientzentrierten Modelle wurden im Rahmen von Arbeiten an mobilen Datensystemen entwickelt, bei denen davon ausgegangen wurde, dass die Netzwerkverbindung unzuverlässig sei und Leistungsprobleme habe. Hierbei wurden insbesondere folgende vier verschiedene Konsistenzmodelle erstellt (deren Übersetzung aus dem Englischen der Übersichtlichkeit wegen nicht erfolgt ist) :

* **monotonic reads**: sobald ein Prozess den Wert eines Eintrags gelesen hat, werden alle nachfolgenden Lesezugriffe dieses Prozesses nur einen Wert zurückgeben, welcher genauso alt oder jünger ist wie der erste Wert
* **monotonic writes**: eine Schreiboperation eines Prozesses auf einem Eintrag wird garantiert abgeschlossen, bevor eine weitere Scheiboperation desselben Prozessen erfolgen kann
* **read-your-writes**: hat ein Prozess einen Eintrag mit einer bestimmten Version geschrieben, dann werden nachfolgende Lesezugriffe garantiert keine ältere als diese Version lesen
* **writes follow reads**: hat ein Prozess einen Wert für einen Eintrag gelesen, so werden nachfolgende Schreiboperationen garantiert auf dem gelesenen Wert oder einem jüngeren Wert ausgeführt



### 3.6.3 Replica Management

Ein grundlegendes Problem in verteilten Systemen ist die Andordnung und Platzierung der Replikate und welche Mechanismen zur Wahrung der Konsistenz eingesetzt werden müssen. Die hierbei auftretenden Probleme lassen sich in zwei Gruppen unterteilen: zum einen kann die Platzierung der Replika-Server betrachtet werden, zum anderen die der eigentlichen Daten.

Die Platzierung der Replika-Server ist heutzutage kein großes Problem mehr aufgrund der Vielzahl großer Datencenter weltweit und der stetig verbesserten Konnektivität.

Schwieriger ist da die angemessene Platzierung der Daten.



### 3.6.4 Consistency Protocols

- konkrete Implementierung von Konsistenzmodellen in Form von Konsistenzprotokollen
- continuous consistency
- Primary-based protocols
- Replicated-write protocols
- cache-coherence protocols



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

---

[Lamport, 1979]: Lamport, “How to Make a Multiprocessor Computer That Correctly Executes Multiprocess Programs,” IEEE Trans. Comput., vol. C–28, no. 9, pp. 690–691, Sep. 1979, doi: 10.1109/TC.1979.1675439.

[van Steen, 2017]: M. van Steen and A. S. Tanenbaum, Distributed systems, Third edition (Version 3.01 (2017)). London: Pearson Education, 2017.

[Vogels, 2009]: W. Vogels, “Eventually Consistent,” Queue, vol. 6, no. 6, p. 14, Oct. 2008, doi: 10.1145/1466443.1466448.

---
[<< Verteilte Systeme](04_verteilte_systeme.md) | [Inhaltsverzeichnis](02_toc.md) | [Fallbeispiele >>](06_fallbeispiele.md)
|-|-|-|
