# Data Mining Methoden
Data Mining steht verschiedenen Aufgaben gegenüber, um die Ziele des Prozesses zu erreichen. Die wichtigsten Anforderungen sind die hohe Qualität und Menge der Daten, in denen Data Mining Muster erkennen soll. In ihnen kann Data Mining verschiedene Analysen durchführen, je nach Zieldefinition. Zu diesen Aufgaben gehören unter anderem das Erkennen und Identifizieren von ungewöhnlichen Mustern und Datensätzen oder die Gruppierung von Daten aufgrund ihrer Ähnlichkeit. Insgesamt gibt es viele verschiedenen Methoden, von denen hier einige vorgestellt werden:
## Ausreißer-Erkennung (Outliner Detection)
Wenn innerhalb eines Datensatzes ein Messwert nicht den Erwartungen und den anderen Werten entspricht, handelt es sich um einen Ausreißer. Im Data Mining werden Algorithmen wie der Local Outlier Factor (LOF) verwendet, um solche Ausreißer zu erkennen. Der LOF beispielsweise vergleicht die Dichte eines Punktes mit der Dichte seiner Nachbarn. Verfügt der Nachbar über eine größere Dichte, befindet er sich im Cluster. Ist die Dichte hingegen geringer, wird er als Ausreißer gewertet. Diese Algorithmen werden dazu verwendet, um beispielsweise potentiell betrügerische Kreditkartentransaktionen in einem Volumen von allen Transaktionen (auch validen) zu erkennen (H.-P. Kriegel, 2009). Die Ausreißer-Erkennung ist eng verwandt mit der Clusteranalyse.
## Clustering
Die Clusteranalyse handelt es sich um die Analyse von Daten und die Identifizierung von Gruppen, die ähnlicher sind als andere. Wichtig hierbei ist, dass neue Gruppe gefunden und nicht in bestehenden Klassen gegliedert wird. In einer Datenmenge von verschiedenen Fahrzeugen würde die Clusteranalyse beispielsweise die Fahrzeuge in andere Gruppen einordnen als es Menschen tun. Während wir logischerweise zwischen Fahrrad, Motorrad, PKW und LKW unterscheiden, würde die Clusteranalyse Trikes (Motorrad mit drei Rädern) als eigene Gruppe definieren, Ein Mensch hingegen als Untergruppe der Motorräder. Eine Rikscha würde ebenfalls ein eigenes Cluster bilden, während ein Mensch es eher als Untergruppe der Fahrräder einordnen würde.
Die Clusteranalyse ist oft nicht eindeutig und bildet Cluster, die möglicherweise nicht relevant sind. So könnte die Farbe des Fahrzeuges, die Anzahl der Räder, der verwendete Treibstoff oder die Antriebsart relevant sein. Welche Gruppen gefunden wären, ist abhängig vom verwendeten Algorithmus.

|![Clustering](https://github.com/Averan82/ostfalia_db_2019_hausarbeiten/blob/master/modern_data_mining/images/clustering.png)|
|:--:|
|*Abbildung 1-1: Ergebnis einer Clusteranalyse mit Normalverteilungen, (Quelle: © Chire, CC BY-SA 3.0; Wikipedia)*|

Typische und häufig verwendete Algorithmen zur Clusteranalyse im Data Mining sind beispielsweise der DBSCAN, OPTICS oder der Expectation-Maximization-Alogirthmus.

### Pseudocode: Ursprüngliche Version des DBSCAN
```
DBSCAN(SetOfPoints, Eps, MinPts)
// SetOfPoints is UNCLASSIFIED
ClusterID := nextID(NOISE);
FOR i FROM 1 TO SetOfPoints.size DO
Point := SetOfPoints.get(i);      
   IF Point.ClID = UNCLASSIFIED THEN
      IF ExpandCluster(SetOfPoints, Point, ClusterID, Eps, MinPts) THEN
         ClusterID := nextID(ClusterID)
      END IF
   END IF
END FOR
END;
```
### Wichtigste Funktion im DBSCAN: ExpandCluster
```
ExpandCluster (SetOfPoints, Point, CiId, Eps,MinPts) : Boolean;
   seeds := SetOfPoints.regionQuery (Point, Eps); 
   IF seeds.size<MinPts THEN // no core point
      SetOfPoint.changeCl Id (Point, NOISE); 
      RETURN False;
   ELSE // all points in seeds are density-reachable from Point
      SetOfpoints. changeCiIds (seeds, C1Id); 
      seeds.delete(Point); 
      WHILE seeds <> Empty DO 
         currentP := seeds.first(); 
         result := setofPoints.regionQuery(currentP,Eps);
         IF result.size >= MinPts THEN
            FOR i FROM 1 TO result.size DO
               resultP := result.get(i) 
               IF resultP.CiId
                     IN (UNCLASSIFIED, NOISE} THEN
                  IF resultP.CiId = UNCLASSIFIED THEN 
                     seeds.append(resultP);
                  END IF; 
                  SetOfPoints. changeCiId (resultP, CiId) 
               END IF; // UNCLASSIFIED or NOISE
            END FOR;
         END IF; // result.size >= MinPts
         seeds, delete (currentP) 
      END WHILE; // seeds <> Empty
      RETURN True;
   END IF
END; // ExpandCluster 
```

## Klassifikation
## Assoziation
## Regression
