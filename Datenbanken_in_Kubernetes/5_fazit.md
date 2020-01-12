# Fazit (Thomas)

- Text ergänzen!
- Tabelle mit Kombinationsmatrix mit Bewertungskriterien (CAP-Theorem)

Kominationsmatrix - CAP-Theorem
Laut dem CAP-Theorem kann ein verteiltes System zwei der folgenden Eigenschaften gleichzeitig erfüllen, jedoch nicht alle drei.[3]

**Konsistenz (C consistency)**
Die Konsistenz der gespeicherten Daten. In verteilten Systemen mit replizierten Daten muss sichergestellt sein, dass nach Abschluss einer Transaktion auch alle Replikate des manipulierten Datensatzes aktualisiert werden. Diese Konsistenz sollte nicht verwechselt werden mit der Konsistenz aus der ACID-Transaktionen, die nur die innere Konsistenz eines Datenbestandes betrifft.

**Verfügbarkeit (A availability)**
Die Verfügbarkeit im Sinne akzeptabler Antwortzeiten. Alle Anfragen an das System werden stets beantwortet.

**Partitionstoleranz (P partition tolerance)**
Die Ausfalltoleranz der Rechner-/Servernetze. Das System arbeitet auch weiter, wenn es partitioniert wird, d. h., wenn Knoten nicht mehr miteinander kommunizieren können (z. B., um die Daten untereinander konsistent zu halten). Dies kann durch den Verlust von Nachrichten, den Ausfall einzelner Netzknoten oder den Abbruch von Verbindungen im Netz (der Partition des Netzes) auftreten.

---

[<< Run Databases in Kubernetes ](4_dbInK8s.md) | [Inhaltsverzeichnis](0_title.md)

---
