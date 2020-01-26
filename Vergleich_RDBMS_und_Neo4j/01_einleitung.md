# 1. Einleitung

Die wachsende Bedeutung von Smartphones im Alltag geht mit einer stetig wachsenden Bedeutung mobiler Anwendungen einher. Soziale Netzwerke und geoinformationstechnische Dienste sind wenige Beispiele, bei denen unentwegt riesige Datenmengen bereitgestellt und abgerufen werden. Moderne Datenbankmanagemnetsysteme werden dabei vor enorme Voraussetzungen gestellt. 

Während relationale Datenbanken (RDBMS) zu den wohl bekanntesten Datenbanksystemen gehören, ist es interessant zu untersuchen, welche anderen gängigen Systeme es jenseits von relationalen Datenbanken noch gibt. Gängige weitere DBMS sind Graphendatenbank. Dabei ist es spannend zu untersuchen, worin mögliche Vor- bzw. Nachteile beider DBMS bestehen.

Neo4j ist ein populäres Graphendatenbanksystem, das von Unternehmen wie Walmart und ebay eingesetzt wird (Neo4j Inc. 2020). Umso interessanter ist es, Neo4j exemplarisch für Graphendatenbanken, den RDBMS gegenüberzustellen. 

Bei dieser Arbeit werden die zwei verschiedenen DBMS-Konzepte zunächst erklärt. Dabei wird auf die grundsätzlichen Schemata, die Modellierung und auch auf die zwei verwendeten Sprachen eingegangen. Vor- bzw. Nachteile der unterschiedlichen Systeme werden genannt. Anschließend sollen anhand eines praktischen Beispiels die wesentlichen Unterschiede vorgestellt werden. Hierzu wird ein reales Projekt der Staatsbibliothek zu Berlin vorgestellt, bei dem es um die Verfügbarmachung von abendländischen mittelalterlichen Handschriften geht. Dieses Projekt soll als Beispiel für tief verschachtelte Datenstrukturen in einer Datenbank stehen. Ein Performance-Test soll im kleineren Rahmen zwar stattfinden, jedoch steht dieser nicht im Mittelpunkt der Ergebnisse, da es hierzu einer weitaus größeren Datenmenge bedarf als in diesem Projekt zur Verfügung steht. Dennoch soll ein Ansatz für eine Performancesteigerung evaluiert werden. Abschließend wird ein Fazit gezogen, das zum einen auf die aus der Literatur untersuchten Ergebnisse und zum anderen auf die selbst erzielten Ergebnisse in dem vorgestellten Projekt Bezug nimmt.

---
| [<< Inhaltsverzeichnis](00-2_toc.md) | Einleitung | [Allgemeine Vorstellung >>](02_allgemein.md) |
|------------------------------------|------------|-------------------------------------|
