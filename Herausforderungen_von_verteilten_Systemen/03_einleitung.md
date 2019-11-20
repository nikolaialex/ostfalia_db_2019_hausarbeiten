# 1 Einleitung

**TODO: Überarbeiten**

Seit der Erfindung von Computern gibt es den Wunsch, deren Leistung immer weiter zu steigern und diese für immer mehr Zwecke einzusetzen. Die Leistungsteigerung eines einzelnen Computers erreicht dabei in den jüngerer Zeit immer weiter die Grenzen des physikalisch machbaren und wirtschaftlich vertretbaren. Daher wurden schon früh Anstrengungen unternommen, die zu erledigenden Aufgaben aufzuteilen und von mehreren Computern parallel verarbeiten zu lassen. Man spricht dann von verteilten Systemen.

Dem simplen Prinzip, zur Erledigung der Aufgaben immer mehr Einheiten miteinander zu verbinden, stehen die hiermit einhergehenden Schwierigkeiten gegenüber. So steigt die Komplexität in der Kommunikation und Koordination bei immer mehr Rechnern im Verbund stetig an. Weitere Probleme betreffen die Konsistenz der Daten und deren Verteilung / Replikation auf die beteiligten Systeme.

Einen informativen (und unterhaltsamen) Beitrag über die "Schrecken in verteilten Systemen" gibt Andrew Godwin in seinem Talk bei der PyCon Australia 2017:

<a href="http://www.youtube.com/watch?feature=player_embedded&v=jx1Hkxe64Xs
" target="_blank"><img src="http://img.youtube.com/vi/jx1Hkxe64Xs/0.jpg"
alt="Horrors of Distributed Systems" width="240" height="180" border="10" /></a>

Im Rahmen dieser Ausarbeitung sollen die Herausforderungen betrachtet werden, die bei verteilten Systemen zu bewältigen sind. Der Schwerpunkt wird hierbei auf der Datenhaltung liegen.

In [Kapitel 2](04_verteilte_systeme.md) werden zunächst Eigenschaften verteilter Systeme vorgestellt, um die Grundlage für die darauffolgenden Kapitel zu legen. Das [Kapitel 3](05_herausforderungen.md) untersucht dann die einzelnen oben genannten Herausforderungen. In [Kapitel 4](06_fallbeispiele.md) erläutern Fallbeispiele die Herausforderungen und die Überwindung dieser aus praktischer Sicht, bevor die Zusammenfassung in [Kapitel 5](07_zusammenfassung.md) die Ausarbeitung beendet.
