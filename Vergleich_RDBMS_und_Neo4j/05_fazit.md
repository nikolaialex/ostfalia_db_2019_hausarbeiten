#5. Fazit

Die praktische Arbeit mit Neo4j brachte Erkenntnisse im direkten Vergleich zu relationalen Datenbanksystemen, wie sie aus der  Theorie mithilfe der Literatur dem Team nicht offensichtlich geworden wäre. Auch wenn sich die Abfragesprache Cypher sehr an SQL orientiert, wurde sie im Team als einfacher im Erlernen empfunden. Das liegt unter anderem daran, dass Cypher auf dem ASCII-Art-Modell beruht. Die einfachere Handhabung der Sprache beruht nicht zuletzt auch darauf, dass bei der Ausarbeitung von Abfragen, der Nutzer eines RDBMS Kenntnisse über die Struktur haben muss. Bei Neo4j hingegen sind generische Abfragen (Wildcard) über Beziehungen möglich. Während bei RDBMS die Datenmodelle nicht ohne weiteres im Nachhinein erforschbar sind, kann man bei Neo4j die Struktur der Daten in Erfahrung bringen. Neo4j ist beliebig erweiterbar, RDBMS hingegen nicht.

Die Performance einer Neo4j-Datenbank kann durch die Vergabe von Indizes beeinträchtigt werden. Bei relationalen Datenbanken ist dies nicht von Belang. Die in der einschlägigen Literatur genannten Performance-Einbußen unter Verwendung viele JOINS und eines Datenumfang von mehreren Millionen Datensätzen bei RDBMS, sind im Teamprojekt nicht nachvollziehbar gewesen. Als Projektergebnis war die Schreibgeschwindigkeit für Postgres ab 1.2 Mio Datensätzen eingebrochen (Infrastrukturabhängig). Die Lesegeschwindigkeit war proportional dazu.

Insgesamt lässt sich festhalten, dass beide Systeme mehr gemeinsam, als ursprünglich vermutet wurde. So verfolgen beide das ACID-Theorem. Die Konfiguration der Datenbanken ist auch ähnlich gewesen.

---
| [<< Untersuchungen](04_untersuchungen.md) | Fazit | [Literatur >>](06_literature.md) |
|------------------------------------|------------|-------------------------------------|


