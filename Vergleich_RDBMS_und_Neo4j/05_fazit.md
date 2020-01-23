# 5. Fazit

Die praktische Entwicklungsarbeit an einem Anwendungsbeispiel mit Neo4j brachte Erkenntnisse im direkten Vergleich zu relationalen Datenbanksystemen, wie sie aus der Theorie mithilfe der Literatur dem Team nicht offensichtlich geworden wären. Auch wenn sich die Abfragesprache Cypher sehr an SQL orientiert, wurde sie im Team als leichter erlernbar und intuitiver im Umgang empfunden. Das liegt unter anderem daran, dass Cypher auf dem ASCII-Art-Modell beruht. Die einfachere Handhabung der Sprache beruht nicht zuletzt auch darauf, dass bei der Ausarbeitung von Abfragen, der Nutzer eines RDBMS Kenntnisse über die Struktur haben muss. Bei Neo4j hingegen sind generische Abfragen (Wildcard) über Beziehungen möglich. Während bei RDBMS die Daten nicht ohne weiteres im Nachhinein erforschbar sind, kann man bei Neo4j die Struktur der Daten in Erfahrung bringen. Die Datenstruktur der Neo4j Datenbank ist beliebig und einfach erweiterbar, wohingegen die Struktur des RDBMS hingegen nur mit höherem Aufwand und einer Datenmigration erweitert werden kann.

Die Performance einer Neo4j-Datenbank kann durch die Vergabe von Indizes beeinträchtigt werden. Bei relationalen Datenbanken haben Indizes große positive Auswirkung auf die Lese- aber auch negative Auswirkung auf die Schreibgeschwindigkeit. Die in der einschlägigen Literatur genannten Performance-Einbußen unter Verwendung großer SQL JOIN Abfragen und eines Datenumfang von mehreren Millionen Datensätzen bei RDBMS, sind im Teamprojekt nicht nachvollziehbar gewesen. Als Projektergebnis war die Schreibgeschwindigkeit für Postgres ab 1.2 Mio Datensätzen dramatisch eingebrochen. Unsere Vermutung ist, das die mit der gegebenen Docker Infrastruktur im Zusammenhang steht. Die Lesegeschwindigkeit beider Systeme verhielt sich mit steigender Datenanzahl proportional zueinander. Bezogen auf unser Projektsetup zeigte sich aber auch, dass die Graphendatenbank einen Geschwindigkeitsvorteil gegenüber der RDBMS Postgres Datenbank hat. 

Insgesamt lässt sich festhalten, dass beide Systeme im Rahmen der Anwendungsentwicklung unter Einsatz eines Objektmappers mehr Gemeinsamkeiten als Unterschiede hatten als ursprünglich vermutet. So verfolgen beide das ACID-Theorem, die verwendeten Objektmapper Frameworks bieten sehr gute Unterstützung und Die Konfiguration der Datenbanken ist im Rahmen von Perfromanceoptimierungen sehr ähnlich gewesen.

---
| [<< Untersuchungen](04_untersuchungen.md) | Fazit | [Literatur >>](06_literature.md) |
|------------------------------------|------------|-------------------------------------|


