# 4.2 Application Programming Interface

Bei einer API handelt es sich um eine Programmierschnittstelle, bei der Informationen zwischen einer Anwendung und einzelnen Programmteilen standardisiert ausgetauscht werden können.

In der Regel wird eine API im Zusammenhang mit einer ausführlichen Dokumentation der einzelnen Funktionen, der genauen Syntax und möglichen Parametern bereitgestellt, wie Zum Beispiel die Web-API von PayPal: <https://developer.paypal.com/docs/api/overview/> [[4.5](https://www.dev-insider.de/was-ist-eine-api-a-583923/)]

Mit Hilfe von APIs werden einfache Möglichkeiten zur Anbindung der eigenen Infrastruktur über die Entwicklung cloudnativer Anwendungen geboten. Außerdem wird so die Möglichkeit geschaffen, die Daten gemeinsam mit Kunden, anderen externen Nutzern oder Partnern zu verwenden.

Die folgende Darstellung demonstriert eine solche API [[4.6](https://www.redhat.com/de/topics/api/what-are-application-programming-interfaces)]<a id="Darstellung_42"></a>:

![API](../images/API.png)
***Darstellung 4.2:** Screenshot zur Verwendung von APIs [[4.6](https://www.redhat.com/de/topics/api/what-are-application-programming-interfaces)]*

Es wird dabei zwischen den folgenden Typklassen unterscheiden:

- Funktionierende APIs
- Dateiorientierte APIs
- Objektorientierte APIs
- Protokollorientierte APIs

In dem folgenden Git-Repository wurde eine REST-API in der Programmiersprache Go (<https://golang.org/>) von der Autorin Sarah-Désirée Weber entwickelt: <https://github.com/wesade/API>

Eine erste Dokumentation ist unter [docs/swagger.json](https://github.com/wesade/API/blob/master/docs/swagger.json) zu finden. (<https://swagger.io/>)

REST steht dabei für REpresentational State Transfer und beschreibt die Art und Weise der Kommunikation zwischen verteilten Systemen. Unter REST-API ist also eine Programmierschnittstelle zu verstehen, die die Kommunikation zwischen Client und Server beschreibt und sich dabei an den Paradigmen und das Verhalten des World Wide Web (WWW) orientiert. [[4.7](https://www.cloudcomputing-insider.de/was-ist-eine-rest-api-a-611116/)]

| [&lt;&lt;&lt; Inhaltsverzeichnis](../README.md) | [&lt;&lt; 4.1 Architetkurmuster Microservice](./Microservice.md) | Application Programming Interface | [4.3 Cloud Computing &gt;&gt;](./Cloud.md) |
|------------------------------------------------|---------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------|
