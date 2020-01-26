# Anwendungsbeispiele

## Deep Learning - Gesichtserkennung mit Python und CV2

Eine besondere Form des maschinellen Lernens ist das sogenannte Deep Learning. Verschiedene Dateien wie Bilder, Videos oder Texte werden dabei in der Regel mit Hilfe eines neuronalen Netzes untersucht und klassifiziert. Anwendungsbereiche können beispielsweise Objekterkennung oder Sprachassistenten wie Amazon Alexa sein, deren Implementierung auf Basis von Deep Learning Mechanismen funktioniert. Die Tiefe besteht in der Anzahl von Schichten in einem neuronalen Netz, die bei Deep-Learning Algorithmen signifikant höher ausgeprägt ist als bei normalen Machine Learning Anwendungen. [51]

Umsetzungen von Deep Learning Anwendungen sind bereits heute problemlos möglich - dies soll im Nachfolgenden anhand einer Gesichtserkennung unter der Verwendung verschiedener Python- Bibliotheken demonstriert werden.

Mehrere frei verfügbare Programmierbibilotheken und Module wie OpenCV2 erlauben eine relative einfache Umsetzung. Die Gesichtserkennung folgt dabei nicht über einen einfachen Abgleich der RGB-Werte einzelner Pixel eines Bildes - anhand von 128 Merkmalen im Gesicht wird ein neuronales Netz trainiert, anhand dieser Merkmale Ähnlichkeiten festzustellen.

Die dlib- Bibliothek (vgl. https://github.com/davisking) bietet hierzu ein Framework, dass mit ca. 3 Millionen Gesichtern trainiert ist und eine Klassifizierung, ob sich zwei Gesichter unterscheiden, erlaubt.

Die Klassifizierung der Objekte erfolgt über verschiedene Detektionsmuster wie RCNN, SSD oder Hog. Anhand der bereits erlernten Daten wird die Wahrscheinlichkeit ermittelt, ob ein Objekt dem Suchmuster entspricht.
![Objekterkennung ](images/objectDetection.PNG "Objekterkennungn")[52] [53]

[51]
https://de.mathworks.com/discovery/deep-learning.html

[52]
https://cv-tricks.com/object-detection/faster-r-cnn-yolo-ssd/

[53]
https://www.pyimagesearch.com/2018/06/18/face-recognition-with-opencv-python-and-deep-learning/







