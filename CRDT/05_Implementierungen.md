# Implementierungen

Das Ziel dieses Kapitels ist es den praktischen Nutzen von CRDTs herauszustellen.

## Industrie Einsatz von CRDTs

Im Folgenden sollen einige Beispiele für die erfolgreiche Nutzung von CRDTs in der Praxis aufgezeigt werden.

### League of Legends

Die Firma "Riot Games" ist Hersteller und Betreiber des Online-Spiels "League of Legends", welches von 100 Millionen aktiven Benutzern pro Monat gespielt wird. Der Kommunikations-Service wurde von einem *MySQL*-Cluster zur NoSQL-Datenbank *Riak* migriert, da sich der Master-Knoten des *MySQL*-Clusters als "Single point of Failure" herausstellte. *Riak* ist ein Key-Value-Store, der die Prinzipien aus Amazons Dynamo-Veröffentlichung als Open-Source-Projekt umsetzt. Unter dem Namen *Riak-Datatypes* besitzt die Datenbank bereits vollständig implementierte CRDTs. Die Daten können somit über beliebige Knoten verteilt werden, jedoch mit denen beim Einsatz von PA-Systemen typischen Herausforderungen. Wenn beispielsweise zwei verschiedene Personen Spieler A gleichzeitig eine Freundschaftsanfrage schicken und diese jeweils von unterschiedlichen Knoten verarbeitet werden, existieren im System für einen gewissen Zeitraum zwei verschiedene Freundeslisten für den Spieler A. Durch die Replikation wird dieser Zustand jedoch nach einem bestimmten Zeitraum automatisch, und dank der CRDTs ohne anwendungsspezifische Konfliktlösungslogik, aufgelöst. [6]

### SoundCloud Roshi

Die Firma *SoundCloud* hat ein verteiltes Speichersystem für Zeitreihen-Ereignisse namens *Roshi* entwickelt. Auf der Plattform *SoundCloud* wird es zur Darstellung von Posts auf der Timeline des Benutzers genutzt. Es basiert auf der NoSQL-Datenbank *Redis*. Es schafft eine zustandslose, verteilte Schicht, über die mehrere Redis-Instanzen zu einem Cluster verbunden werden können. Die Daten werden mithilfe eines CRDTs auf mehrere Redis-Instanzen konfliktfrei repliziert, um die Ausfalltoleranz und Verfügbarkeit zu erhöhen. Der zu diesem Zweck entwickelte CRDT ist ein Last-Write-Wins-Set, mit automatischer Speicherbereinigung. [7]

## JSON-CRDT mit Automerge

*Automerge* ist eine JavaScript-Bibliothek, die ein CRDT in einer JSON-ähnlichen Syntax bereitstellt. Der folgende Quellcode soll die Funktionsweise der *Automerge*-Bibliothek am Beispiel eines Einkaufszettels verdeutlichen, der auf zwei Knoten repliziert wird. [9]

Zunächst wird auf dem ersten Replikat der Einkaufszettel mit einer JSON-Syntax als leere Liste initialisiert. Danach werden der Liste zwei Einträge hinzugefügt. Änderungen werden immer mithilfe der *change*-Funktion der *Automerge*-Bibliothek vorgenommen. Die Funktion erwartet als Parameter ein JSON-Dokument, eine textuelle Beschreibung der Änderung für das Protokoll, sowie die durchzuführende Änderung als Closure-Funktion und gibt ein neues modifiziertes JSON-Dokument zurück.

```js
let replikat1 = Automerge.from({ einkaufsliste: [] })

replikat1 = Automerge.change(replikat1, 'Milch einkaufen', doc => {
  doc.einkaufsliste.push('Milch')
})

replikat1 = Automerge.change(replikat1, 'Eier einkaufen', doc => {
  doc.einkaufsliste.push('Eier')
})

console.log(replikat1);
```

Der Einkaufszettel enthält nun die Einträge **Milch** und **Eier**. Um die Daten auf einen zweiten Knoten zu replizieren, wird das JSON-Dokument über das Netzwerk übertragen. Um den Ressourcenbedarf für das Netzwerk zu verringern, existiert auch eine operationsbasierte Variante, bei der eine Art Änderungsprotokoll versendet wird. Um das Beispiel überschaubar zu halten, soll jedoch die zustandsbasierte Variante betrachtet werden, bei der immer das gesamte JSON-Dokument versendet wird. Da dieser Knoten noch keine Daten enthält, wird er mit den empfangenen Einkaufzettel initialisiert.

```js
let replikat2 = Automerge.init()
replikat2 = Automerge.merge(replikat2, replikat1)
  
console.log(replikat2);
```

Nun enthält auch das zweite Replikat die Einträge **Milch** und **Eier**. Auf dem ersten Knoten wird der Eintrag **Milch** gelöscht, während zur gleichen Zeit auf dem zweiten Knoten der Eintrag **Mehl** hinzugefügt wird.

```js
// Auf dem ersten Knoten
replikat1 = Automerge.change(replikat1, 'Milch gekauft', doc => {
  doc.einkaufsliste.deleteAt(0)
})

console.log(replikat1);

// .. . gleichzeitig auf dem zweiten Knoten
replikat2 = Automerge.change(replikat2, 'Mehl fehlt noch', doc => {
  doc.einkaufsliste.push('Mehl')
})
console.log(replikat2);
```

Auf dem ersten Knoten enthält der Einkaufszettel nur noch den Eintrag Eier, während er auf dem zweiten Knoten die Einträge Milch, Eier und Mehl enthält. Die Knoten übertragen ihren aktuellen Zustand an alle Replikate. Durch die *merge*-Funktion konvergieren die JSON-Dokumente auf allen Knoten. Sie benötigt als Parameter das aktuelle Replikat des JSON-Dokuments auf dem Knoten und das von einem anderen Knoten empfangene Replikat des JSON-Dokuments. Diese beiden JSON-Dokument werden dann zusammengeführt und als neues JSON-Dokumente zurückgegeben.

```js
// Auf dem ersten Knoten
replikat1 = Automerge.merge(replikat1, replikat2)

console.log(replikat1);

// Auf dem zweiten Knoten
replikat2 = Automerge.merge(replikat2, replikat1)

console.log(replikat2);
```

Nach der Replikation enthalten beide Einkauflisten die richtigen Einträge Eier und Mehl. Die *Automerge*-Bibliothek zeigt den praktischen Nutzen von CRDTs, da die Datenhaltung im JSON-Format in aktuellen Anwendungen und Datenbanken stark verbreitet ist. Für derartige Anwendungen bieten CRDTs eine transparante Möglichkeit mit dem Herausforderungen von "Eventual Consistency" umzugehen.

[Nächstes Kapitel](06_Fazit.md)  
