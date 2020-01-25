## 3.1 Daten Management <a id="3.1_Daten_Management"></a>
### 3.1.1 Data Lake
Für diese Aufgabe wird seit 2010 auch verstärkt auf Data Lakes gesetzt. Diese können sowohl strukturierte, teil-strukturierte  als auch unstrukturierte Daten ablegen.

- Strukturierte Daten
  - Tabellen aus relationalen Datenbanken
- teil-strukturierte Daten
  - CSV-Dateien
  - Log-Dateien
  - XML-Daten
  - JSON-Daten
  - Sensordaten
  - Maschinendaten
- unstrukturierte Daten
  - Dokumente 
    - Emails
    - PDF
    - Office-Dokumente
  - Binärdateien
    - Bilder
    - Audio
    - Video

Das erlaubt insbesondere Daten bereitzustellen, für die noch kein Anwendungszweck existiert. Somit stehen neben produktiv genutzten Daten auch ungenutzte Daten bereit. Das hat zum Ziel, dass sowohl die Entwicklung vereinfacht wird, als auch eine Verknüpfung neuer Daten mit bereits produktiv genutzten Daten.

### 3.1.2 Data Warehouse
Im Geschäftsumfeld wird weiterhin stark auf das Data Warehouse gesetzt. Diese werden als höchst zuverlässige Datenquelle gehandelt. Daher sind die Anforderungen an die Qualität der Strukturen und den Wahrheitsgehalt der Daten entsprechend hoch. Die Daten müssen vor der Aufnahme ins Data Warehouse entsprechend der Anforderungen gereinigt und strukturiert werden. Üblicherweise werden die Daten aus relationalen Datenbanksystemen extrahiert. Aus diesem Grund fällt der Nachteil der starren Strukturen weniger ins Gewicht. Der gesamte Prozess findet in einer Umgebung statt, was die Bedienung, Überwachung und Konsistenz vereinfacht. Ein weiterer Gewinn ist die Möglichkeit, dass auch Endanwender Abfragen und Reports erzeugen können. Zum einen weil die Umgebung dafür ausgelegt ist (anwenderfreundliche GUI Oberfläche, Zugriffssteuerung) und zum anderen weil die Qualität der Daten ein überschaubares Maß an Kenntnissen der Informatik benötigt.

### 3.1.3 Gegenüberstellung der Eigenschaften von Data Warehous und Data Lake <a id="Darstellung_31"></a>

| Eigenschaften    | Data Warehouse                                                                                  | Data Lake                                                                                                        |
|-------------------|-------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| Daten              | Relational und transaktionsorientierte Datenbank eines Anwendungessystems oder Anwendung | Nicht-Relationale und relationale Daten aus verschiedensten Quellen |
| Schema            | Die Schema werden bei der Entwicklung festgelegt                                       | Die Schemen werden erst bei der Analyse entworfen                                                                 |
| Preis-Leistung | Schnelle Auswertung unter Verwendung kostenspieliger Speicherlösungen | Schnelle Ergebnisse trotz VErwendung günstiger Speicherlösungen                                                              |
| Daten Qualität      | Akkurat gepflegte und strukturierte Daten die auch als Wahrheitsquelle dient (SPOT - Single Point of Truth)                             | Alle Arten von Daten die gepflegt und strukturiert sein können oder auch nicht                                                     |
| Benutzer             | Business Analyst                                                                               | Data Scientists,  Data Developer und Business Analysts                                      |
| Auswertungen         | Berichtslauf im Hintergrund, Business Intelligence und  Berichtsvisualisierungen                                                          | Machine Learning, Vohersagemodellierung, data discovery und Profiling                                             |

***Darstellung 3.1:** Tabelle zur Gegenüberstellung der Eigenschaften von Data Warehous und Data Lake*

| [&lt;&lt;&lt; Inhaltsverzeichnis](../README.md) | [&lt;&lt; 3 Data Science Prozess](./030_Data_Science_Prozess.md) | Daten Management | [3.2 Der Prozess &gt;&gt;](./032_Der_Prozess.md) |
|------------------------------------------------|---------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------|
