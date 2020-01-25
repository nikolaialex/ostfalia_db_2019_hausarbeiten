# 4.1 Architetkurmuster Microservice

Rund 70 Prozent der europäischen und US-amerikanischen Unternehmen wollen ihre Softwaremonolithen aufbrechen und setzen dabei auf den Einsatz von Microservices.
Microservices machen nicht nur die IT schneller und agiler, sondern steigern auch die Releases pro Woche um das Vierfache. [[4.1](https://www.computerwoche.de/a/microservices-machen-die-it-schneller-und-agiler,3329517)]

Eine Software soll dabei aus kleinen unabhängigen und eigenständigen (modularen) Services bestehen, die über sorgfältig und schlank definierte [APIs](./API.md) kommunizieren. Ein Microservice übernimmt also unabhängige und kleine Teilaufgaben und ist dabei nach außen hin isoliert.

Folgende Eigenschaften hat ein Microservice:

- Eigenständigkeit - jeder Service einer Software wird eigenständig behandelt, das heißt dieser kann entwickelt, bereitgestellt, betrieben und skaliert werden, ohne dabei andere Services zu beeinträchtigen

- Spezialisierung - jeder Service löst ein konkretes Problem - sollte ein Service an Komplexität gewinnen, kann dieser in kleinere Services unterteilt werden

Die Vorteile von Microservices überwiegen den Nachteilen, wie der nachfolgenden Auflistunge zu entnehmen ist [[4.2](https://aws.amazon.com/de/microservices/), [4.3](https://t3n.de/news/was-sind-eigentlich-microservices-1005903/)]<a id="Darstellung_41"></a>:

Vorteile | Beschreibung |
| :----: | :----: |
| Agilität | "Microservices unterstützen eine Organisation von kleinen, unabhängigen Teams, die jeweils die Verantwortung für ihre eigenen Services übernehmen. Teams agieren in einem kleinen und klar definierten Kontext und haben die Möglichkeit, eigenverantwortlich und schneller zu arbeiten. Dadurch werden die Entwicklungszyklen verkürzt. Sie profitieren in erheblichem Maße von der Gesamtleistung der Organisation." |
| Flexible Skalierung  | "Microservices ermöglichen es, jeden Service unabhängig voneinander zu skalieren, um die Nachfrage nach der von ihm unterstützten Anwendungsfunktion zu decken. Auf diese Weise können Teams die Infrastrukturanforderungen anpassen, die Kosten einer Funktion genau messen und die Verfügbarkeit aufrecht erhalten, wenn ein Service eine Nachfragesteigerung verzeichnet." |
| Einfache Bereitstellung | "Microservices ermöglichen eine kontinuierliche Integration und Bereitstellung, wodurch es einfacher wird, neue Konzepte auszuprobieren und zurückzunehmen, wenn etwas nicht funktioniert. Die niedrigen Ausfallkosten ermöglichen Experimente, erleichtern die Aktualisierung von Code und verkürzen die Markteinführungszeit für neue Funktionen." |
| Technologische Flexibilität | "Microservices-Architekturen folgen nicht dem Ansatz einer Bereitstellung von Einheitslösungen. Die Teams haben die Freiheit, das beste Tool zur Lösung ihrer spezifischen Probleme auszuwählen. Infolgedessen können Teams, die Microservices entwickeln, das beste Tool für die jeweilige Aufgabe wählen." Außerdem besteht die Möglichkeit, verschiedene Technologien und Programmiersprachen in Betracht zu ziehen.|
| Wiederverwendbarer Code | "Die Aufteilung der Software in kleine, klar definierte Module ermöglicht es Teams, Funktionen für verschiedene Zwecke zu nutzen. Ein für eine bestimmte Funktion geschriebener Service kann auch als Baustein für einen anderen Funktionsumfang verwendet werden. Dies ermöglicht es einer Anwendung, auf sich selbst zurückzugreifen, da Entwickler neue Funktionen erstellen können, ohne Code von Grund auf neu zu schreiben." |
| Resilienz | "Die Serviceunabhängigkeit erhöht die Ausfallsicherheit einer Anwendung. In einer monolithischen Architektur kann der Ausfall einer einzelnen Komponente zum Ausfall der gesamten Anwendung führen. Mit Microservices überwinden Anwendungen einen kompletten Serviceausfall, indem sie die Funktionalität beeinträchtigen und nicht die gesamte Anwendung zum Absturz bringen." Somit bieten Microservices eine hohe Stabilität. |

***Darstellung 4.1:** Tabelle mit Vorteilen und jeweiliger Beschreibung von Microservices*

<a id="Darstellung_42"></a>
Nachteile | Beschreibung |
| :----: | :----: |
| erhöhte Latenzzeit | Durch die Anzahl der verteilten Services und dem  Kommunikationsweg über verschiedene Server wird die Latenzzeit erhöht. |
| Verletzung des DRY-Prinzip* | Durch die unabhängige Entwicklung der Teams, besteht die Gefahr, Codeabschnitte an mehreren Stellen wieder zu verwenden. |

***Darstellung 4.2:** Tabelle mit Nachteilen und jeweiliger Beschreibung von Microservices*

**Don't repeat yourself*

Zwei weitere Nachteile sind, dass der Aufwand durch die Unabhängigkeit und Isolation erhöht wird und jeder Microservice einen eigenen Deployment-Prozess benötigt. [[4.2](https://aws.amazon.com/de/microservices/), [4.3](https://t3n.de/news/was-sind-eigentlich-microservices-1005903/)]

Die folgende Darstellung stellt die Architektur eines Monolithen der Architektur von Microservices gegenüber [[4.4](https://www.redhat.com/de/topics/microservices/what-are-microservices)] <a id="Darstellung_43"></a>:

![Architekturgegenüberstellung Monolith vs. Microservices](../images/monolithic-vs-microservices.png)
***Darstellung 4.3:** Screenshot zur Gegenüberstellung von Monolithen versus Microservices [[4.4](https://www.redhat.com/de/topics/microservices/what-are-microservices)]*

Ein Monolith ist eine Software-Architektur, bei der alle Prozesse in einem einzigen und untrennbaren sowie homogenen Gebilde verbunden sind. Bereits der Ausfall einer einzelnen Komponente kann zum Ausfall des gesamten Systems führen.

| [&lt;&lt;&lt; Inhaltsverzeichnis](../README.md) | [&lt;&lt; 4 Technologien](././Technologien.md) | Architetkurmuster Microservice | [4.2 Application Programming Interface &gt;&gt;](./API.md) |
|------------------------------------------------|---------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------|
