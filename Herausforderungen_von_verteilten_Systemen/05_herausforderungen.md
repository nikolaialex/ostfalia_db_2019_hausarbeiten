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
Prozesse sind, wie in Betriebssystemen, auch in verteilten Systemen ein wesentlicher Bestandteil und bilden die Basis für die Kommunikation zwischen verschiedenen Maschinen. Entscheidend ist hierbei die Klärung der Frage, wie die Prozesse organisert sind und ob sie das Konzept von Threads unterstützen.

Threads sind in verteilten Systemen eine Möglichkeit effiziente und performante Server zu entwickeln, die auch bei blockierenden I/O-Operationen die Nutzung der CPU durch mehrere Threads ermöglichen.

Wichtig in verteilten Systemen ist auch das Konzept der Virtualisierung, welche durch die das immer wichtiger werdende Cloud Computing besondere Aufmerksamkeit erfährt. Populäre Techniken zur Virtualisierung erlauben es dem Nutzer, Anwendungen auf ihren favorisierten Betriebssystemen laufen zu lassen und komplette virtuelle verteilte Systeme "in der Cloud" zu erstellen. Dabei ist die Leistungsfähigkeit vergleichbar zu "traditionellen" Anwendungen. Durch diese Flexibilität in der Erstellung und dem Betrieb von virtuellen Systemen sind verschiedenste Cloud-Dienste entstanden, die Infrastruktur (IaaS), Plattformen (PaaS) und Anwendungen (SaaS) sowie weitere Dienste basierend auf virtuellen Umgebungen anbieten.

Die Organisation von verteilten Anwendungen in Clients und Server hat sich als nützlich erwiesen. Clients implementieren hierbei üblicherweise eine Benutzerschnittstelle, welche von einfachen Displays bis hin zu komplexen Anwendungen reicht. Durch das Verstecken von Details über die Kommunikation, Serverstandorte und Serverreplikation wird in der Clientsoftware die angestrebte Verteilungstransparenz erreicht sowie Ausfälle und deren Behebung verdeckt.

Die Server sind in verteilten Systemen meist komplexer als die Clientsoftware. Für sind Fragen des Softwaredesigns zu klären betreffend die Anzahl der angebotenen Dienste, der Zustandslosigkeit der Dienste und ähnliches. Insbesondere bei der Anordnung der Dienste in einem Cluster aus Servern ist Achtung geboten, um die Ausprägung des Systems als Cluster vor den Clients zu verbergen. Hierzu verwenden die Cluster meist einen einzelnen Zugangspunkt, von dem aus die Nachrichten mit den Servern im Cluster ausgetauscht werden. Eine Herausforderung ist es, hierfür eine vollständig verteilte Lösung zufinden.

Eine weiterer wichtiger Aspekt in verteilten Systemen ist die Migration von Code zwischen verschiedenen Computern. Dies umfasst zum Beispiel die Möglichkeit, rechenintensive Anwendungen von den Servern hin zu den Clients zu verlagern, um so die Leistung und Flexibilität zu steigern. Insbesondere in Situationen, in denen die Kommunikation zwischen den Computern teuer und / oder langsam ist, bietet sich dieses Vorgehen an. Ein aktuelles Beispiel für dieses Vorgehen sind moderne Webanwendungen geschrieben in JavaScript, die komplexe Anwendungen bei Aufruf der Website an die Clients ausliefern und dort im Browser ausführen.

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
- veraltete (engl. stale) Abweichungen: gemeint sind zeitliche Unterschiede, mit denen Updates auf die verschiedenen Replikas angewendet wurden
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



### 3.6.3 Replikatverwaltung

Ein grundlegendes Problem in verteilten Systemen ist die Andordnung und Platzierung der Replikate und welche Mechanismen zur Wahrung der Konsistenz eingesetzt werden müssen. Die hierbei auftretenden Probleme lassen sich in zwei Gruppen unterteilen: zum einen kann die Platzierung der Replika-Server betrachtet werden, zum anderen die der eigentlichen Daten.

Die Platzierung der Replika-Server ist heutzutage kein großes Problem mehr aufgrund der Vielzahl großer Datencenter weltweit und der stetig verbesserten Konnektivität.

Bei der Platzierung der Daten können drei Typen mit den jeweiligen Eigenschaften unterschieden werden:

* permanente Replikate: 
  * initiale Menge der Daten im verteilten Data Store
  * geringe Anzahl
  * gespiegelt auf verschiedenen Servern 
* durch Server erzeugte Replikate
  * Kopien der Daten in der Nähe von / auf Servern zur Steigerung der Leistung
  * können Nur-lesend konfiguriert werden, wenn permanente Replikate zum Schreiben verwendet werden
* durch Clients erzeugte Replikate
  * Cache beim Client
  * lokale Kopie der angefragten Daten
  * dienen der Leistungssteigerung.

Ein weiteres Thema der Replikatverwaltung ist die Art und Weise, in der Änderungen im Datenbestand zu den Replikaten propagiert werden. So muss geklärt werden, was eigentlich bei einer Änderung übermittelt wird, ob diese Änderungen aktiv oder passiv übermittelt werden und ob die Replikate individuell oder alle zusammen informiert werden.

Bei Änderungen im Datenbestand lassen sich drei mögliche Fälle der Propagierung unterscheiden. Im ersten Fall werden nur Benachrichtigungen über die Änderung verbreitet. Diese Benachrichtigungen enthalten die Information, dass die nachgelagerten / replizierten Datenbestände nicht mehr valide sind und gegebenenfalls, welcher Teil des Data Store geändert wurde. Wenn Nutzer dieses Datenbestandes über die Änderung benachrichtigt werden, obliegt es der nutzenden Stelle, sich eine aktuelle Version der Daten zu holen.

Die zweite Möglichkeit ist, die veränderten Daten zu den Replikaten zu übertragen. Dies ist selbst bei Aggregierung der Änderungen aufgrund der damit einhergehenden Netzwerklast aber nur sinnvoll, wenn nur selten Änderungen vorkommen und die Daten vielmehr gelesen werden.

Die dritte Möglichkeit ist schließlich, den Replikaten mitzuteilen, welche Operation sie zur Änderung der Daten mit den entsprechenden Parametern durchführen sollen (**active replication**). Dies erfordert, dass jedes Replikat in der Lage ist, aktiv seinen Datenbestand zu ändern. Ein Vorteil des Vorgehens ist, dass Änderungen mit nur geringen Auswirkungen auf die Netzwerklast verbreitet werden können, da die Operation mitsamt ihren Parametern meist nur kleine Einheiten sind. Nachteilig ist, dass die Replikate eine gewisse Rechenleistung aufweisen müssen, um die Änderungen zu verarbeiten.

Eine weitere Designentscheidung, die getroffen werden muss, ist, ob Änderungen zu den Replikaten geschoben (**push**) oder von diesen gezogen (**pull**) werden müssen. Bei push-basierten Lösungen werden die Replikate über Änderungen informiert, auch wenn sie die Daten nicht angefragt haben. Diese Möglichkeit wird u.a. dann genutzt, wenn starke Konsistenz gefordert ist, so z.B. zwischen permanenten Replikaten und durch Server erzeugte Replikate. Im Gegensatz dazu gibt es den pull-basierten Ansatz, bei dem Server und Clients bei den entsprechenden Stellen anfragen, welche Daten zur Zeit aktuell sind.

Die letzte Designentscheidung ist die zwischen der individuellen Benachrichtigung (**unicast**) und der Benachrichtigung aller Teilnehmer (**multicast**). Bei unicast muss ein Server eine Änderung jeweils an alle anderen Server / Clients übermitteln. Dies ist dann sinnvoll, wenn nur ein Teilnehmer über die Änderung informiert werden soll. In den anderen Fällen ist die Verwendung von multicast effizienter, da hier der Server nur eine Nachricht erstellen muss, welche vom Netzwerk an alle Teilnehmer weitergeleitet wird.

### 3.6.4 Konsistenzprotokolle

Die Konsistenzprotokolle stellen die konkrete Implementierung der Konsistenzmodell dar. In Hinblick auf die sequentielle Konsistenz (und ihre Varianten) können die **primary-based** und **replicated-write** Protokolle unterschieden werden. 

Bei den primary-based Protokollen werden alle Änderungen zu einer primären Kopie weitergeleitet, welche anschließend dafür sorgt, dass die Änderungen in der richtigen Reihenfolge vorgenommen werden und entsprechend propagiert werden.

In den replicated-write Protokollen wird eine Änderung an mehrere Replikate gleichzeitig weitergeleitet. In diesem Fall ist die Sicherstellung der korrekten Reihenfolge häufig schwieriger.

Im Folgenden sollen einige Konsistenzprotokolle weiter vorgestellt werden.

#### Primary-based Protokolle

Bei den primary-based Protokollen hat jeder Dateneintrag *x* im Data Store einen sog. **Primary**, also einen zugeordneten Prozess mit der primären Kopie der Daten, welcher für Koordination der Schreiboperationen auf *x* verantwortlich ist. Hierbei kann unterschieden werden zwischen **Primaries**, die fest einem Server zugordnet sind und einer wechselnden Variante, bei der jener Prozess der **Primary** wird, welcher die Schreiboperation initiiert.

In der einfachsten Variante ist der **Primary** einem Server fest zugeordnet. Alle Schreiboperationen, die auf dem Dateneintrag *x* ausgeführt werden sollen, werden zu diesem Server weitergeleitet. Dieser führt dann die Änderung auf seinem lokalem Datenbestand durch und leitet die Änderung anschließend an die übrigen Replikate weiter. Die Aktualisierung der nachgelagerten Replikate wird durch diese dem **Primary** gegenüber bestätigt. Dieser wiederum bestätigt die Änderung gegenüber dem initiierenden Prozess, sobald alle nachgelagerten Änderungen bestätigt wurden. Dieses Verfahren ist in der folgenden Abbildung aus [van Steen, 2017] dargestellt, welche ein primary-based Protokoll in Form eines primary-backup Protokolls wiedergibt.

![Primary-based protocol](./assets/primary_based_protocol.png)

Wie in der Grafik zu sehen ist, geht die angeforderte Schreiboperation bei einem Replika-Server ein (W1). Dieser leitet sie an den **Primary** weiter (W2). Der Primary aktualisiert seinen Datenbestand entsprechend der Operation und leitet sie an die Replikate / Backups weiter (W3). Dieser führen die Änderung ihrerseits durch und bestätigen die Änderung (W4). Sind alle Änderungen bestätigt, bestätigt der **Primary** seinerseits die Operation (W5). Im Gegenzug zu der Schreiboperation können die Leseoperationen lokal von den Replikaten verarbeitet werden.

Dieses Vorgehen kann unter Umständen sehr lange dauern, sodass der Client in einer blockierenden Implementierung einige Zeit auf die Bestätigung der Schreiboperation warten muss. Dafür kann sich der Client aber sicher sein, dass die Schreiboperationen an die jeweiligen Backups propagiert wurde. In einer nicht-blockierenden Implementierung kann der Client zwar sofort nach der Anfrage weitermachen, er kann aber nicht sicherstellen, dass die Schreiboperation entsprechend gesichert wurde.

Die primary-based Protokolle stellen eine relativ simple Implementierung der sequentiellen Konsistenz dar, weil der **Primary** alle eingehenden Operationen in die gewünschte Reihenfolge bringen kann.

In einer als **local-write** bezeichneten Variante der Protokolle sucht der anfragende Prozess auch zunächst den Primary und verlagert diesen anschließend in seine lokale Umgebung. Der Vorteil hierbei ist, dass diese und nachfolgende Schreiboperationen lokal ausgeführt werden können. Dies ist vor allem in Situationen sinnvoll, in denen der Computer vom Netzwerk getrennt ist. Alle Änderungen können lokal ausgeführt werden, solange der Computer offline ist, um dann später an die Replikate propagiert zu werden.

#### Replicated-write Protokolle

Bei den replicated-write Protokollen werden die Schreiboperationen von mehreren Replikaten ausgeführt anstatt nur von einem **Primary**. Bei diesen Protokollen kann unterschieden werden zwischen Protokollen mit **active replication** und **quorum-based** Protokollen.

Bei den Protokollen mit **active replication** ist jedes Replikat mit einem Prozess ausgestattet, welche der Änderungen durchführen kann. Jede Operation wird hierbei zu jedem Replikat gesendet. Problematisch ist hierbei, dass jede Operation auf jedem Replikat in der richtigen Reihenfolge ausgeführt werden muss. In der Praxis können hierfür sog. **Sequencer** eingesetzt werden. Diese sind zentrale Koordinatoren im System, welche zunächst alle Operationen erhalten, diese mit einer eindeutigen Sequenznummer versehen und anschließende an die Replikate propagieren. Dort werden die Operationen dann anhand ihrer Sequenznummer abgearbeitet.

In den **quorum-based** Protokollen wird das Konzept von Wahlen verwendet. Die grundlegende Idee dabei ist es, dass Clients gezwungen sind, die Berechtigung zum Lesen und Schreiben von replizierten Daten anzufragen und zu erhalten. Hierfür benötigen sie die Zustimmung einer Mehrheit von Replikaten.

Dies soll am Beispiel eines verteilten Dateiservers erläutert werden. Um eine Datei zu lesen, von der $N$ Replikate existieren, benötigt ein Client ein **read quorum**, also eine willkürliche Anzahl an $N_R$ Servern. Um eine Datei zu verändern benötigt ein Client entsprechend ein **write quorum** bestehend aus mindestens $N_W$ Servern. Für die Werte von $N_R$ und $N_W$ gelten folgende Einschränkungen:

1. $N_R + N_W > N$
2. $N_W > N / 2$

Nur wenn die benötigte Anzahl von Servern der Operation zugestimmt hat, kann die Datei gelesen oder verändert werden.

#### Clientzentrierte Konsistenzprotokolle

Neben der oben beschriebenen Implementierung der datenzentrierten Konsistenzmodell gibt es auch Implementierungen der clientzentrierten Konsistenzmodelle.

Grundlegend für diese Protokolle ist, dass jeder Schreiboperation eine global eindeutiger ID zugewiesen wird. Diese weist der Server zu, der die Schreibanfrage erhalten hat. Dieser Server ist dann der Ursprung der Operation. Dann werden für jeden Client zwei Mengen an Schreiboperationen vorgehalten. Die eine Menge besteht aus den Schreiboperationen, die für die lesenden Operationen relevant sind (**read set**), die andere besteht aus den Schreiboperationen, welche der Client durchgeführt hat (**write set**).

Auf dieser Basis können die vier oben genannten Konsistenzmodelle implementiert werden.

##### Monotonic Reads

Wenn ein Client eine Leseoperation auf einem Server durchführt, erhält der Server das **read set** des Clients um zu prüfen, ob alle Schreiboperationen in der Menge lokal ausgeführt wurden. Wenn nicht, wird zunächst der Datenbestand mithilfe der anderen Server aktualisiert. Hierzu kann die ID der Operation verwendet werden, die idealerweise eine Referenz auf den ausstellenden Server enthält.

##### Monotonic Writes

Die monotonic write-Konsistenz wird analog zu monotonic reads implementiert. Wenn ein Client eine Schreiboperation initiiert, wird dem angefragten Server das **write set** des Clients übergeben. Damit wird sichergestellt, dass die enthaltenen Schreiboperationen lokal ausgeführt wurden. Anschließend wird die neue Operation dem **write set** hinzugefügt.

##### Read-your-writes

Ähnlich zu den vorherigen Protokollen ist es hier notwendig, dass der Server, auf dem die Leseoperation ausgeführt wird, alle Schreiboperationen des **write sets** dieses Clients lokal ausgeführt hat. Die Schreiboperationen können hierbei ebenfalls von den anderen Servern geladen werden, bevor die Leseoperation ausgeführt wird.

##### Writes-follow-reads

Hierbei wird der angefragte Server zunächst anhand der Schreiboperationen im **read set** aktualisiert. Anschließend wird die Schreiboperation zusammen mit den Operationen aus dem **read set** in das **write set** eingefügt, da die Operationen aus dem **read set** nun ebenfalls relevant für die Schreiboperation sind.

## 3.7 Fehlerrobustheit



### 3.7.1 Process resilience

### 3.7.2 Reliable client-server communication

### 3.7.3 Reliable group communication

### 3.7.4 Distributed commit

### 3.7.5 Recovery



## 3.8 Sicherheit

TODO: weglassen???

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
