# Grundlagen

Das Ziel dieses Kapitels ist es nicht die vorgestellten Konzepte vollständig und lehrbuchartig zu beschreiben, sondern vielmehr eine Grundlage für ein gemeinsames Verständnis dieses Beitrags zu schaffen.

## Das CAP-Theorem

Das CAP-Theorem von Eric Brewer besagt, dass lediglich zwei der folgenden drei wünschenswerten Eigenschaften in einem verteilten System gleichzeitig vollständig erfüllt seien können. [1]

- Konsistenz (Consistency): Alle Knoten des verteilten Systems liefern zu einem bestimmten Zeitpunkt die gleichen Daten.
- Verfügbarkeit (Availability): Das verteilte System als Ganzes ist jederzeit in der Lage Anfragen zu verarbeiten, egal ob es sich um Lese- oder Schreiboperation handelt.
- Ausfalltoleranz (Partition Tolerance): Das verteilte System kann auch bei Ausfall einzelner Knoten als Ganzes weiter arbeiten.

Ausfalltoleranz (Partition Tolerance) ist für Cloud-Systeme von entscheidender Bedeutung, da die Knoten häufig über mehrere Rechenzentren verteilt und über das Internet miteinander verbunden sind. Netzwerkprobleme sind somit wesentlich wahrscheinlicher, als innerhalb eines einzelnen Rechenzentrums.

Eine Netzwerkpartition bezeichnet eine vorübergehende Aufteilung des Netzwerks in relativ unabhängige Teilnetze z.B. aufgrund des Ausfalls von Netzwerkgeräten. Hierdurch können einzelne Knoten des verteilten Systems nicht mehr miteinander kommunizieren.

In [2] zeigt Eric Brewer den Zusammenhang zwischen Latenz und Partitionen in einem Computernetzwerk auf. Er argumentiert, dass es in der Praxis keinen Unterschied macht, ob die Verbindung nun tatsächlich getrennt, oder nur zu langsam ist. Im Fall eines Timeouts muss das System eine fundamentale Entscheidung treffen: Entweder es bricht den Vorgang ab und verringert damit die Verfügbarkeit (C), oder es fährt mit dem Vorgang fort und riskiert damit Inkonsistenzen (A).

## BASE-Konsistenzmodell

Befeuert durch die Erkenntnisse des CAP-Theorem entstanden in den letzten Jahren eine Vielzahl neuer Datenbanksysteme, die oft unter dem Begriff NoSQL zusammengefasst werden. Konkrete verteilte Datenbanksysteme müssen immer einen Kompromiss zwischen den drei Eigenschaften des CAP-Theorems eingehen. Es können somit drei verschiedene Klassen identifiziert werden:

- CA-Systeme (Konsistenz und Verfügbarkeit)
- CP-Systeme (Konsistenz und Ausfalltoleranz)
- PA-Systeme (Ausfalltoleranz und Verfügbarkeit)


Die meisten NoSQL-Datenbanken können als PA-Systeme klassifiziert werden. Beispiele hierfür sind *Amazon Dynamo*, *Riak* oder *Cassandra*. Der Datenbestand wird auf viele Knoten verteilt und repliziert. Die Systeme sind hochverfügbar, da sie den Ausfall einzelner Knoten kompensieren können.

Allerdings werden diese Vorteile durch die Aufgabe des ACID-Konsistenzmodells (atomicity, consistency, isolation, durability) erkauft, welches in klassischen relationalen Datenbankmanagementsystemen (RDBMS) zum Einsatz kommt. Das ACID-Konsistenzmodell garantiert, dass sich die Datenbank nach Abschluss einer Transaktion in einem konsistenten Zustand befindet. Beispielsweise sollte nach der Überweisung eines Betrags von einem Konto auf ein anderes, jeder anfragende Client den aktualisierten Kontostand auf beiden Konten sehen.

Da kein praxistaugliches System völlig auf Konsistenz verzichten kann, führte Eric Brewer das weichere BASE-Konsistenzmodell ein, welches auch für PA-Systeme anwendbar ist. Das Akronym steht für:

- Basically Available: Die Verfügbarkeit des Systems ist wichtiger als die Konsistenz.
- Soft State: Nach Transaktionsende wird die Konsistenz fließend (weich) erreicht.
- Eventual Consistency: Schlussendlich sind die Daten konsistent.

## "Eventual Consistency"

Der Begriff "Eventual Consistency" beschreibt, dass nicht jeder Knoten zwingend zu jedem Zeitpunkt den aktuellsten Wert zurückliefert. Ein bekanntes Beispiel für ein System das "Eventual Consistency" implementiert, ist das Domain Name System (DNS). Das System ist hochverfügbar, da es eine hohe Toleranz gegenüber dem Ausfall einzelner DNS-Server aufweist. Der Datenbestand wird über eine Vielzahl von Servern verteilt und repliziert. Bei der Änderung eines DNS-Eintrags kann es eine gewisse Zeit dauern, bis die aktualisierte Information allen Clients zur Verfügung steht. [3]

Auf das System kann jederzeit sowohl lesend als auch schreibend zugegriffen werden. Unter bestimmten Bedingungen liefert eine lesende Abfrage auf einem Knoten jedoch nicht das Ergebnis einer kürzlich abgeschlossenen schreibenden Operation auf einem anderen Knoten. Wenn keine weiteren Aktualisierungen vorgenommen werden, weisen alle Knoten nach Ablauf eines unbestimmten Zeitraums denselben konsistenten Datenbestand auf.

Wenn auf einem Knoten Daten aktualisiert werden, müssen die Replikate über ein Netzwerkprotokoll, das häufig als Gossip-Protokoll bezeichnet wird, über diesen Vorgang benachrichtigt werden. Daraufhin versuchen die Replikate die Änderungen am Datenbestand ebenfalls durchzuführen. In Falle eines Netzwerkausfalls  kann nicht sichergestellt werden, dass die Änderungsmitteilung alle beteiligten Knoten zeitnah erreicht. Werden Änderungen asynchron auf verschiedenen Knoten vorgenommen, kann dies zu Inkonsistenzen führen, sofern die einzelnen Änderungen nicht miteinander vereinbar sind. Man spricht in diesem Fall von einem Konflikt. Viele NoSQL-Datenbanken halten in diesem Fall beide Versionen der Daten vor und überlassen dem Nutzer (oder der Clientanwendung) bei der nächsten Lese-Abfrage die Korrektur der Daten. Diese Methode wird als Read-Repair bezeichnet und findet zum Beispiel in der *Amazon Dynamo* Datenbank Anwendung.

## "Strong Eventual Consistency"

Nichtsdestotrotz ist das Auflösen von Konflikten nach wie vor schwierig und fehleranfällig. Daher gehen die in dieser Arbeit betrachteten Conflict-free Replicated Data Types (CRDTs) einen anderen Weg. Ihnen liegt eine Variation der "Eventual Consistency" zugrunde. Die "Strong Eventual Consistency" garantiert, dass alle Knoten, die die selben Änderungsmitteilungen verarbeitet haben, sofort den selben konsistenten Datenbestand aufweisen. Konflikte werden demnach grundsätzlich ausgeschlossen. Obwohl auch hier Änderungen asynchron vorgenommen werden, konvergieren alle Replikaten schlussendlich und nachweislich. [4, 5]

[Nächstes Kapitel](03_Auspaegungen.md)  
