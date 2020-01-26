|[<< Inhaltsverzeichnis](02_toc.md) | [Verteilte Systeme >>](04_verteilte_systeme.md)|
|-|-|
---

# 1. Einleitung

Seit der Einführung von Computern für wirtschaftliche, wissenschaftliche und vor allem militärische Zwecke (vgl. militärisch-industriell-wissenschaftlicher Komplex nach [Harari, 2018]) findet ein Rennen um immer leistungsfähigere Computer statt. Ziele dieses Rennens sind vor allem
- das Lösen großer Probleme in immer kürzerer Zeit
- das Lösen großer Problem in gleicher Zeit mit höherer Effizienz und Präzision
- die Erlangung von Wettbewerbsvorteilen.

Die Leistungssteigerung eines einzelnen Computers erreicht dabei in jüngerer Zeit und in Abkehr vom Mooreschen Gesetz [Moore, 1998] immer mehr die Grenzen des physikalisch Machbaren und wirtschaftlich Vertretbaren. Daher wurden schon früh Anstrengungen unternommen, die zu erledigenden Aufgaben aufzuteilen und von mehreren Computern parallel verarbeiten zu lassen. Zur Skalierung werden die Aufgaben in verteilten Systemen aufgeteilt (vgl. Amdahlsches Gesetz nach [Amdahl, 1967]).

Dem simplen Prinzip, zur Erledigung der Aufgaben immer mehr Einheiten miteinander zu verbinden, stehen die hiermit einhergehenden Schwierigkeiten gegenüber. So steigt die Komplexität in der Kommunikation und Koordination bei immer mehr Rechnern im Verbund stetig an. Weitere Probleme sind die Datenkonsistenz und die Verteilung und Replikation der Daten auf die beteiligten Systeme.

Erschwerend kommt hinzu, dass auch in verteilten Systemen bestimmte, wichtige Aspekte eingehalten werden sollten, die für alle Softwaresysteme gelten. Dies sind nach [Kleppmann, 2017] insbesondere die Aspekte der
- Zuverlässigkeit (engl. reliability): ein System sollte auch dann noch die korrekte Funktionalität in angemessener Leistung erbringen, wenn es durch Hardware-, Software- oder menschliche Fehler beeinträchtigt ist
- Skalierbarkeit (engl. scalability): wenn ein System immer größer wird im Sinne von Datenvolumen, Datenverkehr oder Komplexität, sollte es in angemessener Weise mit dem Wachstum umgehen können
- Wartbarkeit (engl. maintainability): auch nach vielen Änderungen im Laufe der Zeit durch viele unterschiedliche Entwickler sollte es das System noch ermöglichen, dass diese produktiv am System arbeiten können.


Einen informativen (und unterhaltsamen) Beitrag über die "Schrecken in verteilten Systemen" gibt Andrew Godwin in seinem Talk bei der PyCon Australia 2017 [Godwin, 2017]:

<a href="http://www.youtube.com/watch?feature=player_embedded&v=jx1Hkxe64Xs
" target="_blank"><img src="http://img.youtube.com/vi/jx1Hkxe64Xs/0.jpg"
alt="Horrors of Distributed Systems" width="240" height="180" border="10"/></a>

Im Rahmen dieser Ausarbeitung sollen die Herausforderungen betrachtet werden, die bei verteilten Systemen zu bewältigen sind. Der Schwerpunkt wird hierbei auf der Datenhaltung und Replikation liegen.

In [Kapitel 2](04_verteilte_systeme.md) werden zunächst Eigenschaften verteilter Systeme vorgestellt, um die Grundlage für die darauffolgenden Kapitel zu legen. Das [Kapitel 3](05_herausforderungen.md) untersucht die einzelnen oben genannten Herausforderungen. In [Kapitel 4](06_fallbeispiel.md) werden anhand eines umfangreichen Fallbeispiels die Herausforderungen und die Überwindung dieser aus praktischer Sicht erläutert, bevor die Zusammenfassung in [Kapitel 5](07_zusammenfassung.md) die Ausarbeitung beendet.

---
[Harari, 2018]: Y. N. Harari, Eine kurze Geschichte der Menschheit, 30. Auflage. München: Pantheon, 2018.

[Moore, 1998]: G. E. Moore, “Cramming More Components Onto Integrated Circuits,” Proc. IEEE, vol. 86, no. 1, pp. 82–85, Jan. 1998, doi: 10.1109/JPROC.1998.658762.

[Godwin, 2017]: A. Godwin, “Horros of Distributed Systems,” 04-Aug-2017, URL: https://youtu.be/jx1Hkxe64Xs

[Amdahl, 1967]: G. M. Amdahl, “Validity of the single processor approach to achieving large scale computing capabilities,” in Proceedings of the April 18-20, 1967, spring joint computer conference on - AFIPS ’67 (Spring), Atlantic City, New Jersey, 1967, p. 483, doi: 10.1145/1465482.1465560.

[Kleppmann, 2017]: M. Kleppmann, Designing data-intensive applications: the big ideas behind reliable, scalable, and maintainable systems, First edition. Boston: O’Reilly Media, 2017.

---
|[<< Inhaltsverzeichnis](02_toc.md) | [Verteilte Systeme >>](04_verteilte_systeme.md)|
|-|-|
