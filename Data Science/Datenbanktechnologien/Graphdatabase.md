# Graph-Datenbanken

Graph-Datenbanken basieren auf der mathematischen Graphentheorie und speichern die Daten in einem Modell, welches überwiegend aus Knoten und Kanten besteht. Ein Knoten repräsentiert dabei eine Entität, welche durch Kanten mit anderen Entitäten verbunden sein können, also in Relation zueinander stehen. Es lassen sich somit also stark vernetzte Informationen speichern, wie zum Beispiel Daten aus sozialen Netzwerken.

Die folgende Abbildung zeigt einen Graphen mit Beziehungen zu anderen Entitäten:

![Graph](./images/Graph.png)

Für das Durchsuchen eines Graphen sind sepzialisierte Graphalgorithmen möglich, es wird dabei zwischen der Tiefen- und der Breitensuche unterschieden.

Während bei der Tiefensuche zunächst der tiefere Knoten besucht wird, werden bei der Breitensuche im ersten Schritt alle Nachbarknoten auf einer Ebene durchsucht und erst dann wird auf die nächst tiefere Ebeneb gewechselt.

    TODO: Code aus BA für Breiten und Tiefensuche 

    TODO: Vor- und Nachteile von GD

Im Gegensatz zu relationalen Datenbanken, gehören Graphdatenbanken zur Familie der NoSQL-Datenbanken. [[16](https://www.bigdata-insider.de/graph-datenbanken-a-887332/), [17](https://www.bigdata-insider.de/was-ist-eine-graphdatenbank-a-788834/)]

    TODO: NoQL

[Weiter zu Neo4J](./Neo4J.md)
