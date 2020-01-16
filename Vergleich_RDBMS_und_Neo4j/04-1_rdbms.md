## 3.1 RDBMS
Ein Relational Database Management System (RDBMS) bezeichnet grundsätzlich ein Database Management System (DBMS), welches auf dem relationalen Modell von E. F. Codd [01] basiert. Innerhalb dieses Systems können Daten anhand einer vordefinierten Struktur in Datenbankobjekten gespeichert werden, die innerhalb des RDBMS durch Tabellen dargestellt werden. RDBMS dient als Basis für die Structured Query Language (SQL) und findet unter anderem Anwendung in den Datenbanksystemen Oracle, MySQL sowie Postgres.

### 3.1.1 Schema 
Eine Tabelle bildet die einfachste Form von Datenspeicherung innerhalb dieses Systems und beschreibt jeweils eine Gruppierung von schemagleichen Daten, organisiert durch mehrere Reihen und Spalten. Die nachfolgende Tabelle veranschaulicht ein Datenbankobjekt anhand einer (vereinfachten) Kundendatenbank eines Online-Shops:


| customerID   | firstname | lastname | email 
| --- | --- | ---  | ---
| 1 | Joseph | Müller | jmueller50@t-online.de 
| 2 | Max | Gerdes | gerdes1956@web.de 
| 3 | Martina | Schröder | mschroed@gmail.com
| 4 | Hans | Meyer | hameyer@arcor.de

*Tabelle: customer_data*

Ein einzelner Datensatz wird innerhalb dieser Struktur als Tupel, Record oder Reihe bezeichnet. Die zuvor dargestelle Kundendatenbank besitzt insgesamt vier solcher Tupel. Die Spalten der Tabelle stehen  hierbei für die einzelnen Merkmale (Attribute) und sind jeweils maßgeblich für die eigentliche Datenspeicherung, hier werden pro Spalte gleich strukturierte Informationen über die eizelnen Tupel (in unserem Beispiel ID, Vorname, Nachname sowie E-Mail Adresse) gespeichert. Die ID dient hierbei als sogenannter Schlüssel, der zur eindeutigen Identifikation des Tupels benötigt wird und nicht mehrfach vergeben werden darf. 

Mehrere Tabellen können miteinander verknüpft werden, sodass innerhalb des Systems Relationen entstehen. Um dies zu verdeutlichen, sei zusätzlich zur Kundendatenbank eine Produktdatenbank gegeben:

| productID   | name | inStock | price 
| --- | --- | ---  | ---
| 1 | iPhone 6 | 23 | 399 
| 2 | Nokia 3310 | 50 | 129 
| 3 | Samsung S8 | 31 | 499

*Tabelle: product_data*

Bestellt ein Kunde nun in diesem Online-Shop ein Produkt, können diese Bestellungen als Relation zwischen den Kunden und den Produkten dargestellt werden:

| orderID   | customerID | productID | orderDate | paymentStatus
| --- | --- | ---  | --- | ---
| 1 | 4 | 3 | 2019-05-25 | complete
| 2 | 3 | 2 | 2019-05-27 | processing

*Tabelle: order_data*

Durch die eindeutigen Schlüssel zur Indentifikation der entsprechenden Datensätze können die einzelnen Bestellungen einem Kunden (hat bestellt) und einem Produkt (wurde gekauft) zugeordnet werden.

### 3.1.2 Constraints
Die sogenannten Constraints sind vordefinierte Regeln, die jeweils auf einzelne Merkmale (Attribute) oder auch auf gesamte Tabellen bei Erzeugung definiert und durch das RDBMS kontrolliert werden. Zu speichernde Daten, die nicht auf die vordefinierten Constraints passen, werden abgewiesen. Folgende Constraints werden häufig in SQL eingesetzt:

* **NOT NULL** - Eine Spalte kann keine NULL Werte enthalten
* **UNIQUE** - Alle Werte einer Spalte sind unterschiedlich
* **PRIMARY KEY** - Kombination aus **NOT NULL** und **UNIQUE**, dient als Identifikation einzelner Tupel
* **FOREIGN KEY** - Identifiziert ein Tupel in einer anderen Tabelle
* **DEFAULT** - Setzt einen Standardwert für eine Spalte, wenn kein Wert mitgegeben wurde
* **CHECK** - Überprüft, ob alle Werte einer Spalte einer Bedingung entsprechen
* **INDEX** - Baut einen Index auf, um Abfragen schneller bearbeiten zu können

### 3.1.3 Integrität 
Innerhalb jedes RDBMS lassen sich durch die zuvor genannten Contraints folgende Kategorien definieren, die die Integrität der Daten gewährleisten:
* **Entity Integrety** - Es existieren keine Duplikate in einer Tabelle
* **Domain Integrety** - Spalten werden nach Datentyp, Format oder Länge der Werte eingeschränkt
* **Referntial Integretiy** - Tupel, die durch andere Tupel über **FOREIGN KEY** referenziert werden, können nicht gelöscht werden

### 3.1.4 Sprache, Operationen und Syntax
SQL wurde 1987 durch die International Organization for Standardization (ISO)als offizielle, domänenspezifische Sprache für RDBMS ernannt und basierte ursprünglich auf relationaler Algebra und Kalkülausdrücken. SQL war die erste kommerzielle Sprache, die das relationale Modell von E. F. Codd [01] aufgriff. Nachfolgend sind die relevantesten Statements dargestellt, die mit SQL realisiert werden können:

* **SELECT** - Liest Daten
* **UPDATE** - Aktualisiert Daten
* **DELETE** - Löscht Daten
* **INSERT INTO** - Fügt Daten ein
* **CREATE (DATABASE/TABLE/INDEX)** - Erstellt Datenbank/Tabelle/Suchindex
* **ALTER (DATABASE/TABLE/INDEX)** - Verändert Datenbank/Tabelle/Suchindex
* **DROP TABLE** - Löscht Tabelle

Neben Statements gibt es zudem Anweisungen und logische Operatoren, um Ergebnisse einer Datenbankabfrage einzugrenzen:

* **WHERE** - Gibt Ergebnisse zurück, die auf Bedingung passen
* **AND/OR/NOT** - Kombination mit **WHERE** um Ergebnisse weiter einzugrenzen

Beispielabfrage, um am Beispiel des Online-Shops Bestellungen zu selektieren, deren Bezahlung überfällig ist:
~~~~sql
SELECT orderID
FROM order_data
WHERE paymentStatus = 'pending' AND orderDate <= '2019-06-01';
~~~~

Selektierte Rückgaben können nachfolgend durch die Statements **GROUP BY** nach Attributen zusammengefasst, und durch **ORDER BY** nach Attributen in beliebiger Reihenfolge sortiert werden.

Fernen können durch **JOIN** Anweisungen Reihen von zwei oder mehrere Tabelle anhand ihrer Relation kombiniert werden:

~~~~sql
SELECT order_data.orderID, customer_data.email, order_data.orderDate
FROM order_data
INNER JOIN customer_data ON order_data.customerID = customer_data.customerID;
~~~~

Die resultierende Kombination sieht am Beispiel des Online-Shops wie folgt aus:

| orderID   | email | orderDate 
| --- | --- | ---  
| 1 | hameyer@arcor.de | 2019-05-25 
| 2 | mschroed@gmail.com | 2019-05-27 
