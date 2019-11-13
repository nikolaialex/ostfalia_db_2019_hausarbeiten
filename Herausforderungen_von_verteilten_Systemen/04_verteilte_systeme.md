# 2 Verteilte Systeme

Für das Konzept der verteilten Systeme gibt es in der Literatur, soweit ersichtlich, keine allgemein anerkannte und gültige Definition. Bei [[Steen_2017](#steen_2017)] wird daher eine eher unspezifische Definition verwendet:
> A distributed system is a collection of autonomous computing elements that appears to its users as a single coherent system.

Nach dieser Definition besitzt ein verteiltes System zwei grundlegende Charakteristiken:
* Es handelt sich um eine Ansammlung eigenständer Recheneinheiten bzw. Knoten.
* Diese Ansammlung erscheint für die Nutzer wie ein einheitliches System.

Bei den Knoten kann es sich um viele verschiedene Arten von Computern handeln, wobei das Spektrum von Hochleistungsrechnern bis hin zu Einplatinenrechnern (und kleiner) reicht. Diese Knoten sind dabei grundsätzlich unabhängig voneinander, müssen aber auf verschiedenen Wegen miteinander kommunizieren. Die räumliche Verteilung der Recheneinheiten kann von demselben Mainboard über den gleichen Raum im Rechenzentrum bis hin zu unterschiedlichen Enden der Welt reichen.

Die zweite Eigenschaft, das Erscheinen als ein einheitliches System, erfordert in der extremsten Deutung dieser Eigenschaft, dass sich das verteilte System gegenüber dem Nutzer so verhält als sei es ein einziger Computer. Diese Forderung dürfte in den meisten Fällen jedoch zu weit gehen, sodass im Regelfall die Eigenschaft als erfüllt anzusehen ist, wenn sich das verteilte System den Erwartungen des Nutzers entsprechend verhält.



## 2.1 Hardware

## 2.2 Software

Quellen:
<a name="steen_2017">[Steen_2017]</a> M. van Steen and A. S. Tanenbaum, Distributed systems, Third edition (Version 3.01 (2017)). London: Pearson Education, 2017.
