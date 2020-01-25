## Die Architektur

Abb. 4 gibt einen ersten groben Überblick über Apache Kafka, die lose Kopplung von Systemen und die Möglichkeit, Daten in Echtzeit zu verarbeiten oder zu transformieren. In der Mitte sieht man Apache Kafka - eine Art Bindeglied zwischen Produzenten und Konsumenten. Apache Kafka besteht typischerweise aus einem Cluster, der wiederum aus einem, zwei oder mehreren Brokern besteht. 

![](https://static.javatpoint.com/tutorial/kafka/images/apache-kafka-architecture3.png "Apache Kafka Architektur")

*Abb. 4 Apache Kafka Architektur*

***Produzent***

Die Produzenten sind verantwortlich, Records zu erstellen und diese an den Broker in ein Topic (bzw. an eine Partition in einem Topic) zu verschicken. Es können beliebig viele Produzenten zu beliebig vielen Topics Rekords versenden.

***Konsument***

Konsumenten können ein oder mehrere Topics abonnieren und die darin enthaltenen Records verarbeiten. 

>Ein wichtiges Faktum ist hier, dass die Nachrichten nicht per Push-Verfahren an die Consumer geliefert werden, sondern dass die Consumer die Nachrichten aktiv beim Topic abholen. (Berle, L., 2017)

Das ermöglicht das Lesen der Rekords in der Geschwindigkeit, in welcher diese verarbeitet werden. Der Konsument kann auch die Partition bestimmen, von welcher er die Record abliest. Konsumenten können Datensätze je nach Konfiguration unterschiedlich schnell verarbeiten (vgl. Vince, E., Johansson, L., 2019).

***Broker***

Der Broker ist nur ein Computerprogramm, das heruntergeladen und installiert werden muss. Jeder Broker in einem Cluster braucht eine eindeutige Broker-ID, entweder automatisch generiert als fortlaufende Sequenz oder selbst vergeben über broker-id. Die Kommunikation von Produzenten und Konsumenten mit dem Broker erfolgt über Records. Seine Aufgabe ist diese Records zu speichern, replizieren und zu löschen und diese an die Konsumenten zu liefern. 

***Topic***

Alle Kafka-Datensätze (Records) sind in Topics unterteilt, welche über mehrere Broker repliziert werden können. Die Daten innerhalb eines Topic können verschiedenen Typs sein - String, JSON etc. Die Records werden von einem Produzenten einem bestimmten Topic zugewiesen und dann von einem Konsumenten abonniert. Gemäß der herrschenden Datenaufbewahrungsrichtlinien werden die Records in den Topics beibehalten bis entweder die Aufbewahrungsfrist oder die Größenbeschränkung überschritten wird (vgl. Vince, E., Johansson, L., 2019). Dabei unterscheidet Kafka zwischen "Normal Topics" and "Compacted Topics". 

>Normal Topics werden für einen gewissen Zeitraum gespeichert und dürfen eine definierte Speichergröße nicht überschreiten. Sind Zeitraum oder Speicherlimit überschritten, darf Apache Kafka alte Nachrichten löschen. Compacted Topics unterliegen weder einer Zeitbeschränkung noch einer Speicherplatzlimitierung. (Luber, S., Kitzel, N., 2018)

***Partition***

Ein Topic besteht aus mehreren Partitionen (auch "commit log" genannt) und wie schon erwähnt bestimmen die Producer die Verteilung der Daten zwischen den Partitionen. Eine Partition enthält einen Teil der gesamten Nachrichten des Topics. Das erhöht die Parallelisierbarkeit, da zu jedem Topic immer nur eine Partition auf einem Broker gespeichert ist. An dieser Stelle muss der Begriff "Replizieren" im Zusammenhang mit den Apache Kafka-Partitionen eingeführt und erläutert werden. Dadurch werden Repliken der Partitionen auf mehrere Broker gespeichert, sodass ein Topic "über mehrere physische Maschinen verteilt werden" (Jacobs, S., 2017). Jede replizierte Partition existiert in Form von *Master (leader)* und *Slave (follower)*. Die Master-Partition kontrolliert die Lese- und Schreiboperationen während die Slaves passiv bleiben und die Daten nur replizieren. Falls ein Master abstürzt, wird sein Platz von einer der Salzes übernommen, was der Grund für die Fehlertoleranz von Kafka ist. 

***ZooKeeper***

In einem ZooKeeper werden Informationen zum Kafka-Cluster und Details zu den Konsumenten gespeichert. Er verwaltet die Broker, indem er eine Liste davon führt. Außerdem ist ein ZooKeeper dafür verantwortlich, einen Master für die Partitionen auszuwählen. Wenn Änderungen bei den Broker oder den Topics eintreten, sendet der ZooKeeper Benachrichtigungen an Apache Kafka. Ein Zookeeper hat einen Leader-Server, der alle Schreibvorgänge abwickelt. Die übrigen Server sind die Follower, die alle Lesevorgänge abwickeln. Ein Benutzer interagiert jedoch nicht direkt mit dem Zookeeper, sondern über den Broker. Kein Kafka-Server kann ohne einen Zookeeper-Server ausgeführt werden. Es ist obligatorisch, den Zookeeper-Server auszuführen (vgl. https://www.javatpoint.com/apache-kafka-architecture)

>Apache ZooKeeper ist eine verteilte hierarchische Schlüssel-Werte-Datenbank (Key Value Store). Diese Technologie wird dazu verwendet, den Status verschiedener verteilter Systeme zu synchronisieren. In anderen Worten: Kafka nutzt ZooKeeper, um einzelne Broker zu synchronisieren (Vince, E., Johansson, L., 2019).

## Ein einfaches Beispiel 
Die ursprüngliche Idee für Apache Kafkas Einsatz bestand darin, die Aktivitäten der Nutzern auf einer  Website zu tracken. Damit sind sowohl die Seitenaufrufe als auch die Suchen, das Hoch- und Runterladen von Dateien und andere Aktionen gemeint.

Durch das Einloggen auf einer Website, das Klicken auf einen Button oder das Hochladen eines Fotos, wird ein Tracking-Ereignis ausgelöst. Informationen zu diesem Ereignis (Records) werden in ein bestimmtes Kafka-Topic eingefügt. In diesem Beispiel werden wir zwischen zwei Topics unterscheiden: der einen wird *click* und der andere *upload* genannt (vgl. Vince, E., Johansson, L., 2019).

Die Partitionierung basiert auf der Benutzer-ID. Ein Benutzer mit der ID 0 wird der Partition 0 zugeordnet, und ein Benutzer mit der ID 1 wird Partition 1 zugeordnet. Der Topic *click* wird in drei Partitionen (drei Benutzer auf zwei verschiedenen Maschinen aufgeteilt.

Es passiert folgendes:
1. Der Benutzer mit ID 0 klickt auf einen Button auf der Website.
2. Der Datensatz wird zum Topic *click* und Partition 0 gepusht.
3. Somit wird die Nachricht ihrem Topic hinzugefügt und die Partition wird erhöht.
4. Broker 1, in dem sich der Master von Partition 0 befindet, repliziert die Nachricht auf Broker 2 und 3.
5. Der Consumer kann eine Nachricht vom Topic *click* abonnieren.



![](https://www.cloudkarafka.com/img/blog/kafka-webtracking.png)

***

[<< Was ist Apache Kafka](01_Was_ist_Apache_Kafka) | [Quellen >>](03_Quellen)

***
