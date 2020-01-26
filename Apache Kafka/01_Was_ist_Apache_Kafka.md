## Was ist Apache Kafka?

### Die Idee
---
Die Streaming-Plattform Kafka ist als internes LinkedIn-Projekt entstanden und seit 2011 ein Open-Source-Projekt der Firma Confluent (vgl. Bonk, M., Hopfenmüller, B., 2019). Confluent (Gründer: Jay Kreps, Jun Rao und Neha Narkhede) kümmert sich um die Weiterentwicklung von Apache Kafka und seines Ecosystems. Namenhafte Unternehmen wie Adidas und Spotify nutzen Apache Kafka für Monitoring und Analysen, andere wie Zalando und Netflix bauen sogar ihre gesamte Infrastruktur darauf auf (vgl. Wähner, K.).

### Begrifflichkeiten
---
* *Consumer*: Anwendungen, die Daten des Kafka-Clusters lesen
* *Producer*: Anwendungen, die Nachrichten oder Daten in einen Kafka-Cluster schreiben
* *Broker*: Prozesse, die den Nachrichtentransfer abwickeln
* *Kafka-Cluster*: besteht aus einem oder mehreren Brokern
* *Topic*: eine Art Nachrichtenschlange, in die Nachrichten geschrieben werden können
* *Records*: eine Nachrichtenaufzeichnung gespeichert mit einem Schlüssel, Wert und Zeitstempel
* *Partition*: eine Menge von Records, nummeriert über eine Sequenznummer (Offset)


### Anwendungsmöglichkeiten von Kafka
---
Im Allgemeinen kann Apache Kafka drei Funktionen eingesetzt werden: als Messaging-Broker, als Nachrichtenspeicher and als Streaming-Plattform. Kafka ist ein Publish-Subscribe Messaging System, welches den Nachrichtenaustausch zwischen Servern, Applikationen und Prozessen ermöglicht (vgl. Vince, E., Johansson, L., 2019). 

>LinkedIn hat mit Apache Kafka ein skalierbares Messaging System geschaffen, das als universelle Datenschnittstelle zwischen allen Systemen verwendet werden kann, die Daten bereitstellen. (Freiknecht, J., Papp, 2018, S. 449)


#### *Messaging-Broker & Nachrichtenspeicher*
---
Apache Kafka ist effektiv, wenn Systemänderungen eintretten, ohne dass diese Auswirkungen auf das Gesamtsystem haben. Gemeint sind sowohl mögliche Ausfallzeiten, als auch neue Abhängigkeiten oder sogar Softwareanpassungen. 

Was Apache Kafka von einer reinen Nachrichtenwarteschlange unterscheidet, ist die Echtzeiterfassung und -verarbeitung von Daten sowie die hohe Geschwindigkeit, in welcher diese verarbeitet werden. Bei einer Nachrichtenwarteschlange werden die Nachrichten in der Warteschlange gespeichert, sobald sie an der Reihe sind, abgearbeitet und danach gelöscht. So wird jede Nachricht von einem einzelnen Nutzer immer nur einmal verarbeitet. Eine passende Assoziation hierzu ist die Supermarktwarteschlange.



![](https://www.informatik-aktuell.de/fileadmin/_processed_/0/a/csm_Kafka_abb3_Berle_d9f6943e1c.png "Nachrichten-Queue")
*Abb. 1: Nachrichten-Queue*


Im Gegensatz dazu funktioniert  Apache Kafka ähnlich wie ein Fernsehsender: Jeder, der an den Inhalt interessiert ist, kann diesen konsumieren. Dadurch wird eine Nachricht nicht aus dem System entfernt (vgl. Berle, L., 2017):


![](https://www.informatik-aktuell.de/fileadmin/_processed_/2/3/csm_Kafka_abb4_Berle_59f54eee36.png "Apache Kafka")
*Abb.2: Apache Kafka*


Wie man anhand Abbildung 2 erkennen kann, kommunizieren Sender und  Empfänger nicht direkt miteinander, sondern indirekt über *Topics* (so werden die Warteschlangen bei Kafka genannt). Dieses Prinzip ist besser bekannt als "Lose Kopplung" und hat drei Dimensionen - zeitliche, räumliche und logische. Die zeitliche Unabhängigkeit besteht darin, dass der Producer nicht auf den Consumer warten muss, bis dieser seine Nachrichten empfangen hat (vgl. Vince, E., Johansson, L., 2019). Und umgekehrt ist es für einen Consumer nicht mehr wichtig zu wissen, ob, wann und wie die empfangene Nachricht von welchem Producer gesendet worden ist. Die räumliche Entkopplung bezieht sich auf das physikalisch getrennte Befinden von Producer und Consumer, meist an einem oder mehreren Endpunkten von einem oder mehreren Netzwerken. "Logisch" bedeutet in diesem Zusammenhang, dass sich Producer und Consumer sowohl auf verschiedenen Plattformen befinden können, als auch verschiedene Protokolle benutzen können. 


#### *Data Processing*
___
Im Wesentlichen werden zwei Arten der Datenverarbeitung empfohlen: Batch- und Streamverarbeitung.

***Batchverarbeitung***
Einfach erklärt werden die Daten gesammelt und aufbereitet, damit sie anderen Systemen zur Verfügung gestellt werden können. Dabei erfolgt das Exportieren in regelmäßigen Abständen.  Anstatt die Daten in kleineren Paketen zu verarbeiten, wird die Batch-Methode auf Big Data angewendet. Somit kommt es zu Verzögerungen und langen Laufzeiten. Ein weiterer Nachteil dieser Datenverarbeitung sind die unvermeidbaren Fehler, die infolge eines Abbruchs auftreten. Wenn ein Job von einem anderen anhängig ist, so muss man mit einer noch längeren Verzögerung der Ergebnisse rechnen.

***Streamverarbeitung***
Die Stream-Verarbeitung ermöglicht es uns, Daten in Echtzeit zu verarbeiten, sobald sie eintreffen, und Bedingungen innerhalb eines kurzen Zeitraums ab dem Zeitpunkt des Datenempfangs schnell zu erkennen. Mit der Stream-Verarbeitung kann man Daten in Analysetools überspielen, sobald sie generiert wurden, und über sofortige Analyseergebnisse verfügen (Abb. 3).  Apache Kafka, eine der bekanntesten Streaming-Plattform, erreicht eine sehr hohe Fehlertoleranz-Garantie (siehe Kapitel "Die Architektur").

>Die hohe Fehlertoleranz durch das verteilte Commit-Log soll Ausfälle einzelner Clusterknoten abfangen und bei der Disaster Recovery den letzten gültigen Zustand wiederherstellen, ohne dass Nachrichten verloren gehen. (Jacobs, S., 2017)


![](https://miro.medium.com/max/640/1*32pf9w6Sr2Q-79iiWlf4-A.jpeg "Batch Processing vs Real-Time Processing")

*Abb.3: Batch Processing vs Real-Time Processing*

***
[Die Architektur>>](02_Die_Architektur)

***


