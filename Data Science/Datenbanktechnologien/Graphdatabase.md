# 5.1 Graph-Datenbanken

Graph-Datenbanken basieren auf der mathematischen Graphentheorie und speichern die Daten in einem Modell, welches überwiegend aus Knoten und Kanten besteht. Ein Knoten repräsentiert dabei eine Entität, welche durch Kanten mit anderen Entitäten verbunden sein können, also in Relation zueinander stehen. Es lassen sich somit also stark vernetzte Informationen speichern, wie zum Beispiel Daten aus sozialen Netzwerken oder Daten über Kunden.

Die folgende Abbildung zeigt einen Graphen mit Beziehungen zu anderen Entitäten:

![Graph](../images/Graph.png)
*Abbildung 4.8: Eigene Abbildung eines Graphen mit Beziehungen zu anderen Entitäten*

Für das Durchsuchen eines Graphen sind sepzialisierte Graphalgorithmen möglich, es wird dabei zwischen der Tiefen- und der Breitensuche unterschieden.

Während bei der Tiefensuche zunächst der tiefere Knoten besucht wird, werden bei der Breitensuche im ersten Schritt alle Nachbarknoten auf einer Ebene durchsucht und erst dann wird auf die nächst tiefere Ebeneb gewechselt.

Nachfolgend werden die Vor- und Nachteile von Graph-Datenbanken gelistet [[16](https://www.bigdata-insider.de/was-ist-eine-graphdatenbank-a-788834/), [17](https://www.ionos.de/digitalguide/hosting/hosting-technik/graphdatenbank/)]:

Vorteile | Beschreibung |
| :----: | :----: |
| die Strukturen sind flexibel und agil | es gibt keine einheitliche Abfragesprache |
| die Darstellung der Beziehungen sind anschaulich und übersichtlich | schlechte Skalierung, da alle Daten auf einem Server gespeichert werden müssen |
| die Abfragegeschwindigkeit ist von der Anzahl der konkreten Beziehungen abhängig und nicht von der Gesamtmenge der Daten | |
| trotz komplexer Suchanfragen und stark vernetzter Beziehungen bleibt die Abfragegeschwindigkeit hoch | |

Im Gegensatz zu relationalen Datenbanken, gehören Graphdatenbanken zur Familie der NoSQL-Datenbanken. [[16](https://www.bigdata-insider.de/was-ist-eine-graphdatenbank-a-788834/), [18](https://www.bigdata-insider.de/graph-datenbanken-a-887332/)]

NoSQL steht dabei für *Not only SQL*. Mehr über NoSQL kann unter <https://aws.amazon.com/de/nosql/> nachgelesen werden.

[Zurück zu Datenbanken](./Datenbanken.md) || [Weiter zu Neo4J](./Neo4J.md)
