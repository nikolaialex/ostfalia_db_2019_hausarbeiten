# Beispiele für CRDTs

In diesem Kapitel sollen Beispiele für übliche Umsetzungen von CRDTs aufgeführt werden. [4, 5, 10]

## Counter

Als einfaches Beispiel für ein CRDT soll hier ein Zähler (engl. Counter) herangezogen werden. Selbst bei einer  einfachen Datenstruktur kann es in verteilten Datenbanksystemen zu Konflikten kommen.

### G-Counter

Der G-Counter (Grow-Only-Counter) ist ein Zähler, dessen Wert monoton steigt. Diese Datenstruktur kann beispielweise dazu genutzt werden, die Anzahl der Abrufe eines Artikels (Page Impressions) auf einer News-Webseite oder die Likes zu einem Beitrag in einem sozialen Netzwerk zu speichern.

In der operations-basierten Umsetzung als CmRDT werden die Inkrement-Operationen über das Netzwerk an alle Knoten gesendet. Die Knoten führen dann die empfangene Operation lokal aus. Das Netzwerkprotokoll muss dabei sicherstellen, dass weder Nachrichten verloren gehen, noch mehrfach zugestellt werden.

Die Umsetzung als zustandsbasierten CvRDT muss diese Vorbedingungen nicht erfüllen. Anstatt den Wert des Zählers zu speichern, werden die Inkrementierungen jedes Knotens einzeln in einem Vektor abgelegt. Der Wert des Zählers entspricht der Summe aller Einzelwerte. Jeder Knoten aktualisiert nur eine bestimmte Stelle im Vektor. Der erste Knoten verändert nur den Wert an der ersten Stelle; der zweite verändert nur den an zweiter Stelle, usw. Bei der Replikation wird für jeden Einzelwert das Maximum aus dem aktuellen und dem neuen Wert ermittelt. Da der Wert nur wachsen kann, konvergiert er auf allen Replikaten.

![Counter CRDT](img/Counter.png)

*Beispiel A*

Im Beispiel A hat der Zähler den Wert 2. Auf zwei verschiedenen Knoten wird gleichzeitig der Wert des Zähler inkrementiert. Diese Änderung erfolgt zunächst lokal an der jeweiligen Stelle des Vektors. Danach wird die Replikation durchgeführt, indem der neue Zustand an den jeweils anderen Knoten gesendet wird. Mithilfe der Merge-Operation wird aus diesen Werten dann der aktuelle Wert berechnet, indem für jede Stelle des Vektors das Maximum ermittelt wird. Diese Berechnung soll im Folgenden für die erste Stelle des Vektors dargestellt werden. Das Replikat 1 enthält den Wert 3 und das Replikat 2 enthält den Wert 2. Das Maximum dieser beiden Werte ist 3. Daher enthalten nach der Merge-Operation beide Replikate an dieser Stelle des Vektors den Wert 3. Für die zweite Stelle des Vektors wird dieselbe Berechnung mit dem Wert 1 als Ergebnis durchgeführt. Somit enthält der Zähler nach der Replikation den Wert 4.

### PN-Counter

Ein PN-Counter (Positive-Negative-Counter) unterstützt neben der Inkrementierungen auch die Dekrementierungen des Zählers. Der PN-Counter demonstriert, wie durch die Verbindung zweier CRDTs ein neues CRDT höherer Ordnung entsteht. Hierbei kommen zwei G-Counter zum Einsatz. Der erste positive G-Counter speichert die Inkrementierungen, während der zweite negative G-Counter die Dekrementierungen speichert. Die Aktualisierung der Werte und die Replikation ändert sich gegenüber dem zuvor beschriebenen nicht. Der Wert des Zählers entspricht der Differenz der Werte beider G-Counter und konvergiert auf allen Replikaten, indem der Wert des negativen G-Counters vom Wert des positiven G-Counters subtrahiert wird.

## Register

Ein Register ist eine einfache Datenstruktur, die einen Wert speichern kann. Dieser Wert kann dann zu einem späteren Zeitpunkt aus dem Register ausgelesen werden. Wenn das Register parallel auf zwei unterschiedliche Werte gesetzt wird, tritt ein Konflikt auf.

### LWW-Register

Die wohl einfachste Umsetzung ist das LWW-Register (Last-Write-Wins). Neuere Werte überschreiben dabei einfach die älteren Werte im Register. Hierzu muss neben dem Wert auch ein Zeitstempel gespeichert werden. Die Aktualisierung des Registers erfolgt zunächst lokal. Bei der Replikation wird der geänderte Wert inklusive Zeitstempel dann an alle anderen Knoten übertragen. Die Merge-Funktion prüft anhand des Zeitstempels, ob der empfangenen oder der aktuell gespeicherte Wert neuer ist und aktualisiert gegebenenfalls anschließend das Register. Der vermeintlich ältere Wert geht bei diesem Vorgang verloren.

In einem verteilten System ist es aber oft nicht möglich, mit Sicherheit zu bestimmen, welcher Schreibvorgang der Letzte war. Wenn die Knoten nicht miteinander kommunizieren können, können auch ihre Uhren nicht synchronisiert werden. Diesem Problem kann mit Lösungen wie Vektor-Clocks begegnet werden. Trotz dieser Einschränkungen reicht diese Strategie für viele Anwendungsfälle aus. Die NoSQL-Datenbank *Cassandra* nutzt beispielsweise diese Methode zur Replikation.

## Sets

Ein Set ist eine Datenstruktur, die mehrere Elemente enthält, wobei jedes Element nicht mehrfach im Set vorkommen darf. Sie eignet sich zum Beispiel zum Speichern einer Todo-Liste oder eines Einkaufszettels. Elemente können mithilfe der add-Operation hinzugefügt und über die remove-Operation wieder entfernt werden. Die Operationen sind allerdings nicht kommutativ. Wird ein Element zunächst hinzugefügt und anschließend entfernt ist das Ergebnis ein anderes, als würden die Operationen in umgekehrter Reihenfolge ausgeführt werden.

### G-Set

Das G-Set (Grow-Only-Set) verzichtet auf die remove-Operation. Können lediglich Elemente hinzugefügt und nicht wieder entfernt werden, erhält man ein CRDT. Die Änderung erfolgt zunächst lokal. Bei der Replikation wird der neue Zustand an die anderen Knoten gesendet. Als Merge-Operation dient eine Vereinigung des lokalen Sets mit dem empfangenen Set. Hierdurch enthalten beide Replikate die gleichen Elemente.

### OR-Set

Die dem OR-Set (Observed-Remove-Set) zugrundeliegende Idee ähnelt dem zuvor vorgestellten PN-Counter. Intern besteht das OR-Set aus zwei G-Sets: Das erste G-Set A enthält die hinzugefügten Elemente, das zweite G-Set R hält die entfernten Elemente vor.

Jedes Element bekommt ein eindeutiges internes Identifikationskennzeichen, z.B. eine GUID. Gespeichert wird also ein Tupel aus dem eigentlichen Inhaltselement und dem Identifikationskennzeichen. Die add-Operation fügt dieses Tupel in das G-Set A ein. Die remove-Operation fügt das Element in das G-Set R ein. Der Wert des OR-Set wird durch die Differenzmenge von A und R ermittelt. Anschließend wird die Replikation der beiden G-Sets, wie oben beschrieben, ausgeführt.

![Set CRDT](img/Sets.png)

*Beispiel B1*

Im Beispiel B1 wird auf dem ersten Knoten das Element b hinzugefügt. Zur gleichen Zeit wird auf dem zweiten Knoten das Element c hinzugefügt und das Element a entfernt. Die Operationen werden zunächst lokal ausgeführt. Bei der Replikation wird der neue Zustand an den jeweils anderen Knoten gesendet. Mithilfe der Vereinigung als Merge-Operation konvergieren die Werte beider G-Sets A und R auf allen Replikaten. Im Fall des G-Sets A wird {a0,b1} mit {a0,c2} zu {a0,b1,c2} vereinigt. Dasselbe geschieht für das G-Set R, indem die leere Menge {} mit {a0} vereinigt wird. Der Wert des OR-Sets wird durch die Differenzmenge {a0,b1,c2} und {a0} gebildet. Somit enthält das G-Set nach der Replikation den Wert {b1,c2}.

![Sets with ID CRDT](img/Sets-with-Id.png)

*Beispiel B2*

Wird das gleiche Inhaltselement ein zweites Mal hinzugefügt, erhält es ein neues Identifikationskennzeichen. Das OR-Set enthält dann zwei Tupel mit demselben Inhalt, aber unterschiedlichen Identifikationskennzeichen. Durch diesen Sachverhalt ist es möglich, ein bereits gelöschtes Element noch einmal hinzuzufügen. Im oben dargestellten Beispiel wird das Element a ein weiteres Mal hinzugefügt.

## CRDTs höherer Ordnung

Wie bereits am Beispiel des PN-Counters und des OR-Sets verdeutlicht, können CRDTs zu CRDTs höherer Ordnung zusammengesetzt werden. In [8] zeigen Martin Kleppmann and Alastair R. Beresford, wie CRDTs für geordnete Listen, Maps und Register zu einem CRDT für JSON-Datenstrukturen zusammengesetzt werden können. In aktuellen Anwendungen ist die Datenhaltung im JSON-Format stark verbreitet. Der JSON-CRDT unterstützt beliebig verschachtelte Strukturen und erlaubt asynchrone Änderungen an den Daten auf beliebigen Replikaten. Die Replikate senden Änderungsmitteilungen als Operationen an andere Replikate. Gleichzeitige Operationen sind kommutativ. Somit ist sichergestellt, dass die Replikate konvergieren, ohne dass eine anwendungsspezifische Konfliktlösungslogik erforderlich ist. Die formale Semantik des JSON-CRDTs ist so gestaltet, dass kein Daten durch gleichzeitige Änderungen verloren gehen. Die Autoren zeigen jedoch, dass die derzeitige Implementierung der Merge-Funktion in speziellen Situationen für Anwendungsprogrammierer, die eher mit sequentiellen Programmen vertraut sind, zu überraschenden Ergebnissen führen kann. Im nächsten Abschnitt wird auf die Java-Script-Implementierung des JSON-CRDTs mit dem Namen *Automerge* eingegangen.

[Nächstes Kapitel](05_Implementierungen.md)
