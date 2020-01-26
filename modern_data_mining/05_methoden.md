[Inhaltsverzeichnis >>](02_toc.md)

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

#### Pseudocode: Ursprüngliche Version des DBSCAN
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
#### Wichtigste Funktion im DBSCAN: ExpandCluster
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

Der OPTICS-Algorithmus verwendet das gleiche Grundprinzip wie DBSCAN, kann jedoch stattdessen Cluster mit unterschiedliche Dichte erkennen. Er ordnet die Punkte eines Datensatzes so an, dass räumlich nahe Punkte aufeinander folgen und speichert die Distanz. Bei einer Ausgabe als Diagramm zeigt die y-Achse, wie dicht der Punkt zum Nachbarn ist, je geringer der y-Wert, desto dichter der Punkt. So entstehen Täler, welche die Cluster zeigen.
|![OPTICS](https://github.com/Averan82/ostfalia_db_2019_hausarbeiten/blob/master/modern_data_mining/images/OPTICS.png)|
|:--:|
|*Abbildung 1-2: Ergebnis OPTICS, (Quelle: © Chire, Gemeinfrei; Wikipedia)*|

#### Pseudocode: OPTICS
```
OPTICS(DB, eps, MinPts)
     for each point p of DB
        p.reachability-distance = UNDEFINED
     for each unprocessed point p of DB
        N = getNeighbors(p, eps)
        mark p as processed
        output p to the ordered list
        Seeds = empty priority queue
        if (core-distance(p, eps, Minpts) != UNDEFINED)
           update(N, p, Seeds, eps, Minpts)
           for each next q in Seeds
              N' = getNeighbors(q, eps)
              mark q as processed
              output q to the ordered list
              if (core-distance(q, eps, Minpts) != UNDEFINED)
                 update(N', q, Seeds, eps, Minpts)

```
#### Update-Funktion im OPTICS-Algorithmus
```
update(N, p, Seeds, eps, Minpts)
     coredist = core-distance(p, eps, MinPts)
     for each o in N
        if (o is not processed)
           new-reach-dist = max(coredist, dist(p,o))
           if (o.reachability-distance == UNDEFINED) // o is not in Seeds
               o.reachability-distance = new-reach-dist
               Seeds.insert(o, new-reach-dist)
           else               // o in Seeds, check for improvement
               if (new-reach-dist < o.reachability-distance)
                  o.reachability-distance = new-reach-dist
                  Seeds.move-up(o, new-reach-dist)

```
## Klassifikation
Ähnlich wie die Clusteranalyse ist auch die Klassifikation im Data Mining. Hier geht es darum Objekte in Gruppen zu ordnen, die bereits bekannt sind. Während die Clusteranalyse neue Gruppen (Klassen) findet, ist hier die Einordnung in bestehende Klassen relevant.
Es gibt verschiedene Arten von Klassifikationsverfahren, die sich durch ihre Eigenschaften unterscheiden lassen. Statistische und verteilungsfreie Verfahren wie der Bayes-Klassifikator basieren auf Wahrscheinlichkeiten, manuelle und automatische Verfahren hingegen entscheiden nach erlernten Strukturen und sind daher ein Teilgebiet vom maschinellen Lernen.
Der Bayes-Klassifikator ist schnell berechenbar und hat eine gute Erkennungsrate, wenn die Attribute zwischen den Objekte nicht zu stark korrelieren. Er bestimmt die Zugehörigkeit eines Objektes zu einer Klasse anhand von Attributen.

#### Mathematische Definition
*b*: Bayes-Klassifikator  
*f*: f-dimensionaler reelweretiger Raum  
*C*: Menge von Klassen  

|![bayes-klassifikator](https://github.com/Averan82/ostfalia_db_2019_hausarbeiten/blob/master/modern_data_mining/images/bayes.png)|
|:--:|
|*Abbildung 1-3: Mathemtische Definition des naiven Bayes-Klassifikators*|

## Assoziation
In der Assoziationsanalyse steht die Suche nach starken Regeln im Vordergrund. Beispielsweise wird mit ihr das Kaufverhalten von Personen analysiert und Korrelationen zwischen Gemeinsamkeiten beschrieben. Im Alltag kommen wir mit den Ergebnissen einer Assoziationsanalyse über das Crossmarketing in Berührung, wenn wir online einkaufen. Alle Produkte, die in unserem Warenkorb gespeichert sind, sind in irgendeiner Form mit anderen Produkten verknüpft, die in Verbindung mit unseren Produkten zusammen gekauft wurden. Wir erhalten dann diese Ergebnisse als weitere Kaufvorschläge.  

|![assoziation](https://github.com/Averan82/ostfalia_db_2019_hausarbeiten/blob/master/modern_data_mining/images/assoziation.png)|  
|:--:|  
|*Abbildung 1-4: Amazons Kaufempfehlungen anhand getätigter Einkäufe*|  

Ähnlich verfahren die Algorithmen auch mit Produkten, die miteinander verwandt sind. Wer beispielsweise eine Digitalkamera kauft, bekommt auch gleich die passende Speicherkarte angeboten.

Zu der Berechnung solcher Assoziationen wird häufig der Apriori-Algorithmus eingesetzt. Als Voraussetzung muss eine bestimmte Form der Daten vorliegen. Es braucht eine Menge möglicher Items, eine Datenbasis mit Transaktionen und jede Transaktion fasst eine Menge von Items zusammen. Die Assoziationsregel benötigt drei Kenngrößen, um Ergebnisse zu liefern:
Der Support zeigt die relative Häufigkeit des Auftretens der Regel. Es berechnet entsprechend, wie häufig eine Transaktion vorgekommen ist, auf die die Regel zutrifft. Um beim Beispiel Digitalkamera und Speicherkarte zu bleiben: Der Support zeigt an, wie häufig beide Produkte zusammen gekauft wurden.
Der Konfidenz berechnet, wie hoch der Anteil der Anteil der Transaktionen ist, in denen beide Produkte vorkommen. Die Berechnung ist die Anzahl aller Transaktionen, auf die die Regel zutrifft (Support), durch die Anzahl der Transaktionen, die nur das erste der Produkte enthält. 
Der Lift ist die Kenngröße die anzeigt, um welches vielfache der Konfidenzwert den Erwartungswert übertrifft, er zeigt also, wie groß die Bedeutung der Regel ist.
Der Apriori-Algorithmus kämpft mit zahlreichen Problemen, er wächst exponentiell bei wachsender Anzahl der Produkte, die geprüft werden müssen. Dies führte dazu, dass er immer mehr vom FPGrowth-Algorithmus abgelöst wird. Dieser benötigt zur Berechnung eine komprimierte Auflistung aller Transaktionen als Frequent Pattern-Baum. 
#### Pseudocode FPGrowth-Algorithmus
```
Input: Frequent Pattern Tree tree, Frequent Itemset I
Output: vollständige Menge häufiger Itemsets
If tree hat nur einen Pfad P then 
     foreach Kombination K von Knoten in P do
       return K *vereinigt* I mit support = minimaler Support der Items in K end
else 
     foreach Item i in Tabelle F (in umgekehrter Häufigkeitsreihenfolge) do
       K = i *vereinigt* I mit support = Support von i;
       Erstelle Musterbasis von K und reduzierten FP-Baum FPK;
       If FPK nicht leer then FP-Growth (FPK,K)
     end
end
```

***
| [<< Einführung in Data Mining ](04_einfuehrung.md) | [ Aktuelle Data Mining Methoden >>](09_aktuelle.md) |
***
