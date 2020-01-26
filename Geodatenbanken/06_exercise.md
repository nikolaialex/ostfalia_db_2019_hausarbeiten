# Praktische Beispiele

Zur Festigung der theoretishen Grnudlagen soll nun beispielhaft eine Geodatenbank erstellt und einige praktische Beispiele gezeigt werden.

## Vorrausetzungen

Zum durchführen der Übung müssen einige Vorraustzungen erfüllt werden bzw. müssen verschiedene Tool installiert werden. Diese sollen kurz erläutert werden.<br/> 
**ACHTUNG:** Bei dem von mir eingesetzten Betriebssystem handelt es sich um die Linux Distribution **Ubuntu**. Für andere Betriebssystem wie Windows oder MacOS sind zahlreiche Installationsanweisungen für die eingesetzten Tools im Internet vorhanden.

## Python 3

<img align="right" width="300" height="200" src="img/python-logo.png">
Bei [Python](https://www.python.org/) handelt es sich um eine höhere, interpretierte Programmiersprache, die sich vor allem durch Einfachheit und gute Lesbarkeit auszeichnet. Die Python-Code wurde mittels Jupyter Notebook erstellt und verwendet.

### Module

Python zeichnet sich durch seine Modularität aus. So ist es relativ einfach möglich, bereits vorhandene Klassen und Funktionen zu verwenden. Folgende Module müssen zur Ausführung der Skripte installiert sein.

**Installation von Modulen**
Sollten Module fehlen, können diese bequem per *pip* bzw. *pip3*, einem Paketverwaltungsprogramm für Python-Pakete, installiert werden.

- **os:** Operating System Interface. Modul zum Zugriff auf das Dateissystem. Verwendet zum einfachen erstellen von absoluten und relativen Pfaden. In der Regel Teil der Python Standardinstallation. 
- **sqlite3:** DB-API. Modul zum einfachen Zugriff auf die SqLite Datenbank. Bietet Methoden zum Schreiben, Lesen, Erstellen und Löschen von Datenbanken. In der Regel Teil der Python Standardinstallation. 
- **pandas:** Python Data Analyses Library. Modul zur Datenanalyse. Vor allem im Gebiet der Data Science weit verbreitet. Kein Teil der Standardinstallation *pip install pandas*
- **keplergl:** Modul zur Darstellung von Geodaten. Ausführliche Beschreibung siehe Abschnitt [Visualisierung](#visualisierung)

### SqLite und SpatiaLite

Zur Datenhaltung wird das gemeinfreie[1] Datenbanksystem SqLite eingesetzt. Dieses gilt als das weltweit meißtgenutzte Datenbanksystem[2] und kommt ohne weitere Serveranwendung aus. Neben der *Stand Alone*-Eigenschaft ist es vor allem durch seine Schnelligkeit und Robustheit in verschiedenen Szenarien einsetzbar[3].

```
sudo apt-get update
sudo apt-get install spatialite-bin
sudo apt-get install spatialite-gui
```

Neben SqLite wird SpatiaLite[4], eine Geo-Datenbank-Erweiterung, vorrausgetzt. Diese Erweiterung ermöglich den Einsatz von geografischen Objekten und Funktionen. Die Erweiteurng wird in der praktischen Übung dynamisch geladen.
```
sudo apt-get install libsqlite3-mod-spatialite
```

### Visualisierung

Zur Visualisierung wird die datenunabhängige, webbasierte und leistungsstarke kepler.gl[quelle](https://eng.uber.com/keplergl/) verwendet. Dieser basiert auf deck.gl, einem WebGL-Framework, für die visuelle Darstellung großer Datensätze. Kepler.GL wurde 2018 von dem Uber Team über die Bereitstellung des Codes (open source) [quelle](https://medium.com/vis-gl/exploring-geospatial-data-with-kepler-gl-cf655839628f) der Allgemeinheit zur Verfügung gestellt.  Über das oben genannten Python Modul is teine Integration problemos möglich. Eine [User Guide ist](https://github.com/keplergl/kepler.gl/blob/master/docs/keplergl-jupyter/user-guide.md) unter in dem Github Repository von keppler.gl verfügbar.

## Erstellung und Befüllung der Datenbank

Nach dem die technischen Vorrausetzungen geschaffen wurde kann nun begonnen werden mit der Datenbank zu arbeiten. Dafür müssen vorab Daten in die Datenbank eingefügt werden. Dazu befinden sich im Unterverzeichnis [data](data/) einige CSV-Dateien mit Infromationen der Stadt New York. Diese Daten sind frei verfügbar und können über verschiedene Portale wie beispielsweise [Open NY](https://data.ny.gov/widgets/i9wp-a4ja) bezogen werden.

- **nyc_parking_lots.csv** beinhaltete Informationen über Parkplätze in New York. Die Infromationen liegen als *MultiPolygone* zur Verfügung.
- **nyc_school_locations.csv** beinhaltete Informationen über Schulen in New York. Die Schulorte besitzen den Geodatentypen *Punkt*
- **nyc_subway_entrances.csv** beinhaltete Informationen über die Eingänge zur U-Bahn in New York. Diese besitzen den Datentyp *Punkt*
- **nyc_subway lines.csv** beinhaltete Informationen über die U-Bahn-Linien in New York. Die Koordinaten der U-Bahn-Linien besitzen den Typ *LineString*
- **nyc_vehicle_collisions_2019.csv** beinhaltete Informationen über Verkehrsunfälle im Jahr 2019 in New York. Die Unfallstelle wird dabei über den Datentyp *Punkt* dargestellt.
- **nypd_complaints_2010.csv** beinhaltete Informationen über polizeiliche Beschwerden in New York aus dem Jahr 2010. Die Beschwerden sind jeweils einem bestimmten *Punkte* zugeordnet.

## Start praktisches Beispiel

Zur Wiederholung einige nützliche Links:

- [SpatiaLite Dokumentation](http://www.gaia-gis.it/gaia-sins/spatialite-sql-4.3.0.html)
- [Well-known text repräsentation for Geometrien](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry)
- [Kepler GL Jupyter](https://github.com/keplergl/kepler.gl/blob/master/docs/keplergl-jupyter/user-guide.md)

```python
import os
import sqlite3
import pandas as pd
from keplergl import KeplerGl

BASE_DIR = os.getcwd()
DATA_DIR = os.path.join(BASE_DIR, 'data')
RESULTS_DIR = os.path.join(BASE_DIR, 'results')
DB_FILE = os.path.join(BASE_DIR, 'demo.db')
```

## Datenbefüllung

Im folgenden werden Hilfsfunktionen zur Befüllung der Tabellen mit den Daten der csv-Dateien bereitgestellt. Diese Methoden müssen nur einmalig durch geführt werden. Die Hilfsfunktionen zur Befüllung der TAbellen ist für alle csv-Dateien ähnlich. Sollte die Tabelle bereits bestehen, wird diese gelöscht und neu erstellt. Eine Besonderheit liegt dabei bei der Funktion `AddGeometryColumn()` diese erzeugt Anhand der angegebnen Parameter eine `geom`-Spalte die die verarbeitetenden Geodaten enthält. Zum Schluss wird die Tabelle anhand der csv-Datei befüllt mithilfe weitere weiterer Funktionen (z.B. `ST_GeomFromText()`) befüllt.
Um die Ausgabe zu optimeren, wird für alle Tabellen die  `geom`-Spalte indexiert(`CreateSpatialIndex()`).

```python
def create_parking_lots_table(conn: sqlite3.Connection):
    cursor = conn.cursor()
    cursor.execute("""DROP TABLE IF EXISTS parking_lots""")
    cursor.execute("""CREATE TABLE parking_lots (id INTEGER PRIMARY KEY)""")
    cursor.execute("""SELECT AddGeometryColumn("parking_lots", "geom", 4326, "MULTIPOLYGON", 'XY')""")
    df = pd.read_csv(os.path.join(DATA_DIR, 'nyc_parking_lots.csv'), low_memory=True, skip_blank_lines=True, encoding='utf-8')
    for key, value in df.iterrows():
        cursor.execute(
            """INSERT INTO parking_lots (geom) VALUES (ST_GeomFromText(?, 4326))""",
            (value[0],))
    conn.commit()

    cursor.execute("""DELETE FROM parking_lots WHERE geom IS NULL""")
    conn.commit()

    cursor.execute("""SELECT CreateSpatialIndex('parking_lots', 'geom')""")
    conn.commit()
```

```python
def create_complaints_table(conn: sqlite3.Connection):
    cursor.execute("""DROP TABLE IF EXISTS complaints""")
    cursor.execute("""CREATE TABLE complaints (id INTEGER,date TEXT,time TEXT,description TEXT)""")
    cursor.execute("""SELECT AddGeometryColumn("complaints", "geom", 4326, "POINT", 'XY')""")

    df = pd.read_csv(os.path.join(DATA_DIR, 'nypd_complaints_2010.csv'), low_memory=True, skip_blank_lines=True, encoding='utf-8')
    for key, value in df.iterrows():
        cursor.execute(
            """INSERT INTO complaints (id, date, time, description, geom) VALUES (?, ?, ?, ?, ST_GeomFromText(?, 4326))""",
            (value[0], value[1], value[2], value[3], 'POINT(%s %s)' % (value[5], value[4])))
    conn.commit()

    cursor.execute("""DELETE FROM complaints WHERE geom IS NULL""")
    conn.commit()

    cursor.execute("""SELECT CreateSpatialIndex('complaints', 'geom')""")
    conn.commit()
```

```python
def create_schools_table(conn: sqlite3.Connection):
    cursor.execute("""DROP TABLE IF EXISTS schools""")
    cursor.execute("""CREATE TABLE schools (id INTEGER PRIMARY KEY, name TEXT,type TEXT,category TEXT)""")
    cursor.execute("""SELECT AddGeometryColumn("schools", "geom", 4326, "POINT", 'XY')""")

    df = pd.read_csv(os.path.join(DATA_DIR, 'nyc_school_locations.csv'), low_memory=True, skip_blank_lines=True, encoding='utf-8')
    for key, value in df.iterrows():
        cursor.execute(
            """INSERT INTO schools (name, type, category, geom) VALUES (?, ?, ?, ST_GeomFromText(?, 4326))""",
            (value[3], value[6], value[7], 'POINT(%s %s)' % (value[17], value[18])))
    conn.commit()

    cursor.execute("""DELETE FROM schools WHERE geom IS NULL""")
    conn.commit()

    cursor.execute("""SELECT CreateSpatialIndex('schools', 'geom')""")
    conn.commit()
```

```python
def create_vehicle_collisions_table(conn: sqlite3.Connection):
    cursor = conn.cursor()
    cursor.execute("""DROP TABLE IF EXISTS vehicle_collisions""")
    cursor.execute("""CREATE TABLE vehicle_collisions (
                        id INTEGER,
                        date TEXT,
                        time TEXT,
                        number_persons_injured INTEGER,
                        number_persons_killed INTEGER,
                        number_pedestrians_injured INTEGER,
                        number_pedestrians_killed INTEGER,
                        number_cyclist_injured INTEGER,
                        number_cyclist_killed INTEGER,
                        number_motorist_injured INTEGER,
                        number_motorist_killed INTEGER,
                        vehicle_type_code_1 TEXT,
                        vehicle_type_code_2 TEXT,
                        contributing_factor_vehicle_1 TEXT,
                        contributing_factor_vehicle_2 TEXT)""")
    conn.commit()

    df = pd.read_csv(os.path.join(DATA_DIR, 'nyc_vehicle_collisions_2019.csv'), low_memory=True, skip_blank_lines=True, encoding='utf-8')
    for key, value in df.iterrows():
        cursor.execute(
            """INSERT INTO vehicle_collisions VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ST_GeomFromText(?, 4326))""",
            (value[13], value[0], value[1], value[3], value[4], value[5], value[6], value[7], value[8], value[9],
             value[10], value[14], value[15], value[11], value[12], value[2]))
    conn.commit()

    cursor.execute("""DELETE FROM vehicle_collisions WHERE geom IS NULL""")
    conn.commit()

    cursor.execute("""SELECT CreateSpatialIndex('vehicle_collisions', 'geom')""")
    conn.commit()
```

```python
def create_subway_lines_table(conn: sqlite3.Connection):
    cursor = conn.cursor()
    cursor.execute("""DROP TABLE IF EXISTS subway_lines""")
    cursor.execute("""CREATE TABLE subway_lines (id INTEGER PRIMARY KEY, name TEXT)""")
    cursor.execute("""SELECT AddGeometryColumn("subway_lines", "geom", 4326, "LINESTRING", 'XY')""")
    df = pd.read_csv(os.path.join(DATA_DIR, 'nyc_subway_lines.csv'), low_memory=True, skip_blank_lines=True, encoding='utf-8')
    for key, value in df.iterrows():
        cursor.execute(
            """INSERT INTO subway_lines (name, geom) VALUES (?, ST_GeomFromText(?, 4326))""",
            (value[4], value[0]))
    conn.commit()

    cursor.execute("""DELETE FROM subway_lines WHERE geom IS NULL""")
    conn.commit()

    cursor.execute("""SELECT CreateSpatialIndex('subway_lines', 'geom')""")
    conn.commit()
```

```python
def create_subway_entrances_table(conn: sqlite3.Connection):
    cursor = conn.cursor()
    cursor.execute("""DROP TABLE IF EXISTS subway_entrances""")
    cursor.execute("""CREATE TABLE subway_entrances (id INTEGER PRIMARY KEY, name TEXT, line TEXT)""")
    cursor.execute("""SELECT AddGeometryColumn("subway_entrances", "geom", 4326, "POINT", 'XY')""")
    df = pd.read_csv(os.path.join(DATA_DIR, 'nyc_subway_entrances.csv'), low_memory=True, skip_blank_lines=True, encoding='utf-8')
    for key, value in df.iterrows():
        cursor.execute(
            """INSERT INTO subway_entrances (name, line, geom) VALUES (?, ?, ST_GeomFromText(?, 4326))""",
            (value[2], value[4], value[3]))
    conn.commit()

    cursor.execute("""DELETE FROM subway_entrances WHERE geom IS NULL""")
    conn.commit()

    cursor.execute("""SELECT CreateSpatialIndex('subway_entrances', 'geom')""")
    conn.commit()
```

Das folgende Snippt lädt die SpatiaLite-Erweiterung uznd prüft, ob Aktivierung erfolgreich war.

**ACHTUNG:** Der Code `conn.load_extension('mod_spatialite.so')`ist zum Laden der Erweiterung auf Linux-Systemen (SO = Shared Object). Um die Erweiterung unter Windows oder MacOS zu laden muss dieser Parameter angepasst werden. 


```python
    with sqlite3.connect(DB_FILE) as conn:
        conn.enable_load_extension(True)
        conn.load_extension('mod_spatialite.so')
        conn.execute('SELECT InitSpatialMetaData();')

        cursor = conn.cursor()
        cursor.execute("""PRAGMA TABLE_INFO('spatial_ref_sys')""")
        for row in cursor:
            print(row)

        cursor.execute("""SELECT * FROM spatial_ref_sys WHERE srid in (4326, 3857)""")
        for row in cursor:
            print(row)
        print('\n')

        cursor.execute("""SELECT * FROM geometry_columns""")
        for row in cursor:
            print(row)
        print('\n')
```

Hilfsmethode zum befüllen der Datenbanken, diese können erst nach dem Laden der SpatiaLite-Erweiterung durchgeführt werden.

```python
        create_vehicle_collisions_table(conn)
        create_parking_lots_table(conn)
        create_complaints_table(conn)
        create_schools_table(conn)
        create_subway_lines_table(conn)
        create_subway_entrances_table(conn)
```

### Hilfsfunktion zur Virtualisierung

Die Funktion save_kepler_map erstellt anhand eines Cursor einen panda-DataFrame und rendert diesen mithilfe des KeplerGl-Moduls in eine Kartendarstellung.

```python

def save_kepler_map(file_name, layer_name, cursor, columns):
    df = pd.DataFrame(cursor.fetchall())
    df.columns = columns 
    map_1 = KeplerGl(height=500)    
    map_1.add_data(df, layer_name)
    map_1.save_to_html(file_name=file_name)
```

## Spatialite Dependencies

Abfragen des Referenzkoordinatensystem zu bestimmten SRID und der aktuellen Version der SpatiaLite-Erweiterung

```python
cursor = conn.cursor()
cursor.execute("""
    SELECT 
        srid, 
        auth_name, 
        auth_srid, 
        ref_sys_name 
    FROM spatial_ref_sys 
    WHERE srid IN (4326, 3857)
""")
for row in cursor:
    print(row)
```

    (3857, 'epsg', 3857, 'WGS 84 / Pseudo-Mercator')
    (4326, 'epsg', 4326, 'WGS 84')
    

```python
cursor.execute("""SELECT spatialite_version()""").fetchone()
```
    ('4.3.0a',)

```python
cursor.execute("""SELECT proj4_version()""").fetchone()
```
    ('Rel. 4.9.3, 15 August 2016',)

## Erste Abfragen

In folgenden Abschnitt sollen einige Funktionen und die daraus resultierenden Ergebnisse angezeigt werden.

```python
cursor.execute("""SELECT name, geom FROM schools LIMIT 1""")
cursor.fetchall()
```

    [('P.S. 001 The Bergen',
      b'\x00\x01\xe6\x10\x00\x00M\xa1\xf3\x1a\xbb\x80R\xc0\xf2\xb8\xa8\x16\x11SD@M\xa1\xf3\x1a\xbb\x80R\xc0\xf2\xb8\xa8\x16\x11SD@|\x01\x00\x00\x00M\xa1\xf3\x1a\xbb\x80R\xc0\xf2\xb8\xa8\x16\x11SD@\xfe')]

**ACHTUNG:** Die Spalte `geom` gibt eine die Koordinaten als Hexadezimale codierte Version zurück. Diese ist für Menschen nicht bzw. schwer lessbar, vereinfacht ide Maschinenverarbeitung aber deutlich.

Wenn mit der Geometry gearbeitet werden soll können folgende Funktionen hilfreich sein (Auszug)
- `ST_AsText(geom)`: dump geometry als WKT
- `AsGeoJSON`: dump geometry als GeoJSON
- `AsEWKT(geom)`: dump geometry als erweitert WKT, beinhaltet SRID Information
- `AsGML(geom)`: dump geometry als GML
- `ST_X(geom)`: gibt die X-Koordinate der Geometrie zurück
- `ST_Y(geom)`: gibt die Y-Koordinate der Geometrie zurück

**WICHTIG:** Funktionen die mit dem Präfix `ST_` starten sind vom OGC definierte Standard und sollten für alle Geodatenbanken-System verfügbar sein. Wenn möglich sollte nur vom OGC soezizfizerite Funktionen verwenden werden. Dies soll die Abhängigkeit von speziellen Geodatenbanken reduzieren und die Portierfähigkeit erhöhen. 

```python
cursor.execute("""
    SELECT 
        name,
        ST_AsText(geom), 
        AsEWKT(geom), 
        AsGeoJSON(geom), 
        AsGML(geom), 
        ST_X(geom), 
        ST_Y(geom) 
    FROM schools 
    LIMIT 1
""")
for row in cursor:
    name, wkt, ewkt, geojson, gml, x, y = row
    print(name)
    print(wkt)
    print(ewkt)
    print(geojson)
    print(gml)
    print(x)
    print(y)
```

    P.S. 001 The Bergen
    POINT(-74.01142 40.648959)
    SRID=4326;POINT(-74.01142 40.648959)
    {"type":"Point","coordinates":[-74.01142,40.648959]}
    <gml:Point srsName="EPSG:4326"><gml:coordinates>-74.01142,40.648959</gml:coordinates></gml:Point>
    -74.01142
    40.648959000000005

## Räumliche Abfragen

```python
cursor.execute("""
    SELECT 
        ST_NPoints(geom), 
        CoordDimension(geom), 
        ST_NDims(geom), 
        ST_GeometryType(geom),
        GeodesicLength(geom),
        GreatCircleLength(geom),
        ST_AsText(ST_Centroid(geom)),
        ST_Area(ST_Transform(geom, 32618)), -- WGS 84 / UTM zone 18N
        ST_Area(geom), 
        ST_AsText(ST_Envelope(geom))
    FROM parking_lots LIMIT 1
""")
for row in cursor:
    num_points, coord_dim, dim, geom_type, geo_length, circle_length, centroid, area_1, area_2, envelope = row
    print(num_points)
    print(coord_dim)
    print(dim)
    print(geo_length)
    print(circle_length)
    print(centroid)
    print(area_1)
    print(area_2)
    print(envelope)
```

    23
    XY
    2
    237.2323336767035
    237.11900134169153
    POINT(-73.97916 40.690211)
    3033.8538505971646
    3.234090609387721e-07
    POLYGON((-73.979515 40.689908, -73.978813 40.689908, -73.978813 40.690496, -73.979515 40.690496, -73.979515 40.689908))
    

## Beispiel #1: Größter Parkplatz in NY

Dies Übung soll die flächenmäßig 10 größten Parkplätze New Yorks finden und anzeigen.

```python
cursor.execute("""
    SELECT id, ST_Area(ST_Transform(geom, 32618)) area, ST_AsText(geom) 
    FROM parking_lots 
    ORDER BY ST_Area(geom) DESC LIMIT 10
""")
save_kepler_map(os.path.join(RESULTS_DIR, 'parking_lots.html'), 'parking_lots', cursor, ['id', 'area', 'geom'])
```
Ergebnisvorschau

![res_img_ny_parking](img/res_im_ny_parking.png)

Um das komplette Ergebnis zu sehen kann die von Kepler.gl erstellte [HTML-Datei](res/parking_lots.html) verwendet werden.

**ACHTUNG:** Um die HTML-Datei komplette zu rendern muss eine Verbindung zum Internet bestehen. Für die korrekte Anzeige der HTML Datei muss diese separat im Browser angezeigt werden, da Github sonst den QUellocode der HTML-Datei anzeigt. 

## Beispiel #2: Gefährlichsten Schulzonen in NY

Mit der folgenden Abfrage lassen sich die gefährlichsten Schulen in New York ermitteln. Dazu werden die Schulstandorte mit den räumlichen Nachbarn aus der Beshwerdedatenbank der Polizei kombiniert. Zusätzlich wird ein Risikofaktor berechnet.

```python
cursor.execute("""
    SELECT name, risk_factor, ST_X(geom), ST_Y(geom) FROM (
        SELECT s.name, COUNT(0) as risk_factor, s.geom FROM schools s INNER JOIN complaints c
        ON ST_Distance(s.geom, c.geom) * 111195.0802 < 500.0 AND c.rowid IN (
            SELECT rowid FROM SpatialIndex 
            WHERE f_table_name = 'complaints' 
            AND f_geometry_column = 'geom' 
            AND search_frame = BuildCircleMbr(ST_X(s.geom), ST_Y(s.geom), 500.0 / 111195.0802, 4326)
        )
        GROUP BY s.name
    ) ORDER BY risk_factor DESC;
""")
save_kepler_map(os.path.join(RESULTS_DIR, 'schools.html'), 'schools', cursor, ['name', 'risk_factor', 'longitude', 'latitude'])
```

**INFORMATION:** Für geografische Geometrien gibt die Funktion `ST_Distance` die Distanz in Gradeinheiten an. Um einen ungeähren Wert in Metern zu erhalten wird die Ausgabe mit dem folgenden Faktor multipliziert `(Earth Mean Radius in Meters) x PI/180 = 111195.0802`.

Ergebnisvorschau

![res_img_schools](img/res_img_schools.png)

Um das komplette Ergebnis zu sehen kann die von Kepler.gl erstellte [HTML-Datei](res/schools.html) verwendet werden.

**ACHTUNG:** Um die HTML-Datei komplette zu rendern muss eine Verbindung zum Internet bestehen. Für die korrekte Anzeige der HTML Datei muss diese separat im Browser angezeigt werden, da Github sonst den QUellocode der HTML-Datei anzeigt.

## Datenstruktur der Tabellen

Die folgenden Kommandos sollen eine Übersicht über die zugrundeligenden Datensturktur vermitteln

```python
cursor.execute("""PRAGMA table_info('complaints')""")
cursor.fetchall()
```

    [(0, 'id', 'INTEGER', 0, None, 0),
     (1, 'date', 'TEXT', 0, None, 0),
     (2, 'time', 'TEXT', 0, None, 0),
     (3, 'description', 'TEXT', 0, None, 0),
     (4, 'geom', 'POINT', 0, None, 0)]

```python
cursor.execute("""PRAGMA table_info('parking_lots')""")
cursor.fetchall()
```

    [(0, 'id', 'INTEGER', 0, None, 1), (1, 'geom', 'MULTIPOLYGON', 0, None, 0)]

```python
cursor.execute("""PRAGMA table_info('schools')""")
cursor.fetchall()
```

    [(0, 'id', 'INTEGER', 0, None, 1),
     (1, 'name', 'TEXT', 0, None, 0),
     (2, 'type', 'TEXT', 0, None, 0),
     (3, 'category', 'TEXT', 0, None, 0),
     (4, 'geom', 'POINT', 0, None, 0)]


```python
cursor.execute("""PRAGMA table_info('subway_entrances')""")
cursor.fetchall()
```
[(0, 'id', 'INTEGER', 0, None, 1),
    (1, 'name', 'TEXT', 0, None, 0),
    (2, 'line', 'TEXT', 0, None, 0),
    (3, 'geom', 'POINT', 0, None, 0)]


```python
cursor.execute("""PRAGMA table_info('subway_lines')""")
cursor.fetchall()
```

    [(0, 'id', 'INTEGER', 0, None, 1),
     (1, 'name', 'TEXT', 0, None, 0),
     (2, 'geom', 'LINESTRING', 0, None, 0)]

```python
cursor.execute("""PRAGMA table_info('vehicle_collisions')""")
cursor.fetchall()
```

    [(0, 'id', 'INTEGER', 0, None, 0),
     (1, 'date', 'TEXT', 0, None, 0),
     (2, 'time', 'TEXT', 0, None, 0),
     (3, 'number_persons_injured', 'INTEGER', 0, None, 0),
     (4, 'number_persons_killed', 'INTEGER', 0, None, 0),
     (5, 'number_pedestrians_injured', 'INTEGER', 0, None, 0),
     (6, 'number_pedestrians_killed', 'INTEGER', 0, None, 0),
     (7, 'number_cyclist_injured', 'INTEGER', 0, None, 0),
     (8, 'number_cyclist_killed', 'INTEGER', 0, None, 0),
     (9, 'number_motorist_injured', 'INTEGER', 0, None, 0),
     (10, 'number_motorist_killed', 'INTEGER', 0, None, 0),
     (11, 'vehicle_type_code_1', 'TEXT', 0, None, 0),
     (12, 'vehicle_type_code_2', 'TEXT', 0, None, 0),
     (13, 'contributing_factor_vehicle_1', 'TEXT', 0, None, 0),
     (14, 'contributing_factor_vehicle_2', 'TEXT', 0, None, 0),
     (15, 'geom', 'POINT', 0, None, 0)]

---

| #   | Literatur            |
| --- |-----------------------------------------------------------------------------------------------------------------------------------|
| [1] | **Wikipedia**: *Gemeinfreiheit*, [https://de.wikipedia.org/wiki/Gemeinfreiheit](https://de.wikipedia.org/wiki/Gemeinfreiheit), aufgerufen am 20.01.2020  |
| [2] | **SqLite**: *Most deployed*, [[www.geosci.usyd.edu.au](https://www.sqlite.org/mostdeployed.html)](https://www.sqlite.org/mostdeployed.html), aufgerufen am 10.01.2020  |
| [3] | **SqLite**: *SqLite*, [https://www.sqlite.org/index.html](https://www.sqlite.org/index.html), aufgerufen am 10.01.2020  |