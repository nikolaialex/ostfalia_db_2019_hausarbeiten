| [<< Fallbeispiel](06_fallbeispiel.md) | [Inhaltsverzeichnis](02_toc.md) | [Quellenverzeichnis >>](08_quellenverzeichnis.md) |
| ------------------------------------- | ------------------------------- | ------------------------------------------------- |

---

# 5. Zusammenfassung

Im Rahmen dieser Ausarbeitung wurden Herausforderungen betrachtet, die sich in verteilten Systemen ergeben können und in den meisten Systemen immer wieder bewältigt werden müssen.

Grundlegende Herausforderungen ergeben sich bereits bei der Wahl einer geeigneten System- und Softwarearchitektur, wobei die Verwendung von Middleware-Schichten breite Verwendung gefunden hat.

Eng verbunden mit der Wahl der Architekturen ist die Gestaltung und Verteilung der beteiligten Prozesse. Neben der erforderlichen Synchronisation über die einzelnen Knoten hinweg erfordern vor allem die Virtualisierung von Systemen und die Migration von Code eine sorgfältige Handhabung bei der Erstellung verteilter Systeme.

Für die Kommunikation zwischen beteiligten Prozessen/Komponenten wurden Techniken wie Remote Procedure Calls, Object-Replication und zustandslose Message-Oriented-Middleware betrachtet, welche teils aufeinander aufbauen und je nach Anforderung entsprechende Vorteile und Nachteile haben.

Um die Kommunikation zu ermöglichen, ist es notwendig, dass sich die beteiligten Komponenten überhaupt im System finden können. Hierfür wurden die Herausforderungen in der Namensgebung und Namensauflösung genannt.

Haben sich die Prozesse gefunden, müssen sie auch zur richtigen Zeit die richtige Funktion erbringen. Hierzu wurde die Koordination von Prozessen betrachtet, die sich u. a. über die Verwendung logischer Uhren, Mechanismen zum gegenseitigen Ausschluss sowie die Einführung von Koordinatoren erreichen lässt.

Ein besonderer Schwerpunkt lag in dieser Ausarbeitung auf der Betrachtung der Konsistenz und Replikation von Daten. Die Daten werden häufig für Zwecke der Verfügbarkeit und Leistungssteigerung repliziert. Um die Replikate auf dem gleichen, konsistenten Stand zu halten, sind verschiedene Mechanismen betrachtet worden. Nach den datenzentrierten und clientzentrierten Konsistenzmodellen wurden mit den Konsistenzprotokollen Wege der Implementierung der vorgestellten Modelle vorgestellt.

Als letzte Herausforderung wurde schließlich die Fehlerrobustheit eines Systems betrachtet. Neben den eigentlichen Anforderungen, die die Fehlerrobustheit ausmachen, wurde mit dem Two-Phase-Commit-Protokoll ein praktisches Werkzeug gezeigt.

Im anschließenden Fallbeispiel wurde erläutert, wie durch Relevanzkriterien z. B. eine räumliche Partitionierung vorgenommen werden kann, um die zu verarbeitende und zu übertragene Datenmenge effizient zu reduzieren. Zusätzlich wurden hierbei die verschiedenen Optimierungsansätze aufeinander aufbauend gegenübergestellt und auch gegensätzliche Konzepte betrachtet.

Es ist festzuhalten, dass es für alle Herausforderungen auch Lösungen gibt, jedoch ist anhand des Systemdesigns abzuwägen, welche der Konzepte und konkreten Mechanismen die meisten der Anforderungen erfüllen können. Die Vor- und Nachteile müssen gegenübergestellt und Anforderungen priorisiert werden, um ein technologisch und wirtschaftlich optimales verteiltes System zu realisieren.

Die Konzepte und Technologien unterliegen durch variierende Anforderungen und Präferenzen, wie überall in der IT-Welt, stets einem Wandel. Wegen Kompatibilitäts-, Lizenz- und Komplexitätseinschränkungen werden stetig neue Technologien geschaffen, die wiederum neue Konzepte oder Plattformen besser als vorhergehende Technologien realisieren.

---

| [<< Fallbeispiel](06_fallbeispiel.md) | [Inhaltsverzeichnis](02_toc.md) | [Quellenverzeichnis >>](08_quellenverzeichnis.md) |
| ------------------------------------- | ------------------------------- | ------------------------------------------------- |
