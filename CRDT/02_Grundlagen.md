# Grundlagen

## Das CAP-Theorem

Das CAP-Theorem von Eric Brewer besagt, dass lediglich zwei der folgenden drei wünschenswerten Eigenschaften in einem verteilten System gleichzeitig vollständig erfüllt seien können.

- Konsistenz (Consistency): Alle Knoten des verteilten Systems liefern zu einem bestimmten Zeitpunkt die gleichen Daten.
- Verfügbarkeit (Availability): Das verteilte System als Ganzes ist jederzeit in der Lage Anfragen zu verarbeiten, egal ob es sich um Lese- oder Schreiboperation handelt.
- Ausfalltoleranz (Partition Tolerance): Das verteilte System kann auch bei Ausfall einzelner Knoten als Ganzes weiter arbeiten.

Ausfalltoleranz (Partition Tolerance) ist für Cloud-Systeme von entscheidender Bedeutung, da die Knoten häufig über mehrere Rechenzentren weltweit verteile sind. Netzwerkprobleme sind somit wesentlich wahrscheinlicher, als innerhalb eines einzelnen Rechenzentrums.

Eine Netzwerkpartition bezeichnet eine vorübergehende Aufteilung des Netzwerks in relativ unabhängige Teilnetze z.B. aufgrund des Ausfalls von Netzwerkgeräten. Hierdurch können einzelne Konten des verteilten Systems nicht mehr miteinander kommunizieren.

In [...] zeigt Eric Brewer den Zusammenhang zwischen Netzwerklatenz und -partitionen auf. Er argumentiert, dass es in der Praxis keinen Unterschied macht, ob die Verbindung nun tatsächlich getrennt, oder nur zu langsam ist. Im Fall eines Timeouts muss das System eine fundamentale Entscheidung treffen: Entweder es bricht den Vorgang ab und verringert damit die Verfügbarkeit (C), oder es fährt mit dem Vorgang fort und riskiert damit Inkonsistenzen (A).

## BASE-Konsistenzmodell

Befeuert durch die Erkenntnisse des CAP-Theorem entstanden in den letzten Jahren ein Vielzahl neuer Datenbank-Konzepte und -Systeme, die oft unter dem Begriff NoSQL zusammengefasst werden. Konkrete verteilte Datenbank-Systeme müssen also immer einen Kompromiss zwischen den drei Eigenschaften des CAP-Theorems eingehen. Es können somit drei verschiedene Klassen identifiziert werden:

- CA-Systeme (Konsistenz und Verfügbarkeit)
- CP-Systeme (Konsistenz und Ausfalltoleranz)
- PA-Systeme (Ausfalltoleranz und Verfügbarkeit)

Die meisten NoSQL-Datenbanken können als PA-Systeme klassifiziert werden. Beispiele hierfür sind Amazon Dynamo, Riak, Cassandra. Der Datenbestand wird auf viele Knoten verteilt und repliziert. Die Systeme sind hochverfügbar, da sie den Ausfall einzelner Knoten kompensieren können.

Allerdings werden diese Vorteile durch die Aufgabe des ACID-Konsistenzmodell (atomicity, consistency, isolation, durability) erkauft, welches in klassischen relationalen Datenbankmanagementsystemen (RDBMS) zum Einsatz kommt. Das ACID-Konsistenzmodell garantiert, dass sich die Datenbank nach Abschluss einer Transaktion in einem konsistenten Zustand befindet. Beispielsweise sollte nach der Überweisung eines Betrags von einem Konto auf ein anderes, jeder anfragende Client den aktualisierten Kontostand auf beiden Konten sehen.

Da in keinem System völlig auf Konsistenz verzichtet werden kann, führte Eric Brewer das weichere BASE-Konsistenzmodell ein, welches auch für PA-Systeme anwendbar ist. Das Akronym steht für:

- Basically Available: Die Verfügbarkeit des Systems ist wichtiger als die Konsistenz.
- Soft State: Nach Transaktionsende wird die Konsistenz fließend (weich) erreicht.
- Eventual Consistency: Schlussendlich sind die Daten konsistent.

## "Eventual Consistency"

Der Begriff "Eventual Consistency" beschreibt, dass nicht jeder Knoten zwingend zu jedem Zeitpunkt den aktuellsten Wert zurückliefert. Ein bekanntes Beispiel für ein System das "Eventual Consistency" implementiert, ist das Domain Name System (DNS). Das System ist Hochverfügbarkeit und weist eine hohe Toleranz gegenüber dem Ausfall einzelner DNS-Server auf. Der Datenbestand wird über eine vielzahl von Servern verteilt und repliziert. Bei der Änderung eines DNS-Eintrags kann es eine gewisse Zeit dauern, bis die aktualisierte Information allen Clients zur Verfügung steht.

Auf das System kann jederzeit sowohl lesend als auch schreibend zugegriffen werden. Unter bestimmten Bedingungen liefert eine lesende Abfrage auf einem Knoten jedoch nicht das Ergebnis einer kürzlich abgeschlossenen schreibenden Operation auf einem anderen Knoten. Wenn keine weiteren Aktualisierungen vorgenommen werden, werden alle Knoten nach Ablauf eines unbestimmten Zeitraums den selben konsistenten Datenbestand aufweisen.

Wenn auf einem Knoten Daten aktualisiert werden, müssen die Replikate über ein Netzwerkprotokoll, das häufig als Gossip-Protokoll bezeichnet wird, über diesen Vorgang benachrichtigt werden. Daraufhin versuchen die Replikate die Änderungen am Datenbestand ebenfalls durchzuführen. In Falle eines Netzwerkausfalls  kann nicht sichergestellt werden, dass die Änderungsmitteilung alle beteiligten Knoten zeitnah erreicht. Werden Änderungen asynchron auf verschiedenen Knoten vorgenommen, kann dies zu Inkonsistenzen führen, sofern die einzelnen Änderungen nicht miteinander vereinbar sind. Man spricht in diesem Fall von einem Konflikt. Viele NoSQL-Datenbanken halten in diesem Fall beide Versionen der Daten vor und überlassen dem Nutzer (oder der Clientanwendung) bei der nächsten Lese-Abfrage die Korrektur der Daten. Diese Methode wird als Read-Repair bezeichnet und findet zum Beispiel in Amazons Dynamo Datenbank Anwendung.

## "Strong Eventual Consistency"

Nichtsdestotrotz ist das Auflösen von Konflikten nach wie vor schwierig und fehleranfällig. Daher gehen die, in dieser Arbeit betrachteten Conflict-free Replicated Data Types (CRDTs) einen anderen Weg. Ihnen liegt eine Variation der "Eventual Consistency" zugrunde. Die "Strong Eventual Consistency" garantiert, dass alle Knoten, die die selben Änderungsmitteilung verarbeitet haben, sofort den selben konsistenten Datenbestand aufweisen. Konflikte werden demnach grundsätzlich ausgeschlossen. Diese Eigenschaft kann durch die im weiteren Verlauf dieser Arbeit vorgestellten Conflict-free Replicated Data Types (CRDTs) erzielt werden. Obwohl auch hier Änderungen asynchron vorgenommen werden, konvergieren alle Replikaten nachweislich in einen gemeinsamen konsistenten Zustand.
