# 5.2 Apache Spark

# 5.2.1 Entstehung

Wie bereits erwähnt, steigt die Menge an zu verarbeitenden Daten enorm. Gleichzeitig werden diese Daten immer unstrukturierter, stammen schon lange nicht mehr nur aus einer, sondern in der Regel gleich aus mehreren Quellen und liegen in unterschiedlichen Formen vor. Das IT-Marktforschungsunternehmen Gartner versteht _„Big data“ als “high-volume, high-velocity and/or high-variety information assets that demand cost-effective, innovative forms of information processing that enable enhanced insight, decision making, and process automation.”_ [[5.2.19](https://www.gartner.com/en/information-technology/glossary/big-data)] Das „Big“ bezeichnet nachdem nicht nur die Menge (Volume), sondern auch das Tempo (Velocity) und die unterschiedlichen Formate (Variety).

Gleichzeitig liefert diese Beschreibung von Garnet auch einen Hinweis, wie diese Daten weiterverarbeitet werden müssen (_„cost-effective, innovative forms of information processing“_ [[5.2.19](https://www.gartner.com/en/information-technology/glossary/big-data)]), um gewisse Ziele (_„enhanced insight, decision making, and process automation“_ [[5.2.19](https://www.gartner.com/en/information-technology/glossary/big-data)]) zu erreichen.

Das von Google eingeführte MapReduce-Verfahren verteilt Berechnungen auf Computerclustern, um diese parallelisiert zu verarbeiten. Auf diesem Algorithmus basiert das 2006 veröffentlichte Apache Hadoop Framework, das seit 2008 zu einem Top-Level-Projekt der Apache Software Foundation geworden ist. Apache Spark wiederum basiert auf Hadoop, weswegen diese beiden Frameworks auch von Apache Spark selbst hinsichtlich Schnelligkeit und anderen Eigenschaft verglichen werden.

Für die Verarbeitung von Big Data kommen verschiedene Tools und Programmiersprachen zum Einsatz. So können Daten, je nach Arbeitsschritt mit verschiedenen Programmiersprachen, bearbeitet werden. Spark entstand in dieser Zeit, in der sich Entwickler bessere Tools zur Verarbeitung dieser großen Datenmengen gewünscht haben. Da Spark gleich mehrere Probleme auf einmal löste, etablierte es sich als ein beliebtes Tool. <a id="Darstellung_521"></a>

![The Complex World of Big Data](../images/5_4.jpg)<br>
***Darstellung 5.2.1:** The Complex World of Big Data, [5.2.2](https://towardsdatascience.com/a-beginners-guide-to-apache-spark-ff301cb4cd92)*

| [&lt;&lt;&lt; Inhaltsverzeichnis](../README.md) | [&lt;&lt; 5.1.2 Praxisbeispiel mit Neo4J Desktop](../Datenbanktechnologien/Neo4j-Example.md) | Apache Spark - Entstehung | [5.2.2 Vorteile &gt;&gt;](./5_2_2_Vorteile.md) |
|------------------------------------------------|---------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------|
