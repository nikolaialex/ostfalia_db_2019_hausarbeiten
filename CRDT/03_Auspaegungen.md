# Ausprägungen

Grundsätzlich lassen sich zwei unterschiedliche Arten von Conflict-free Replicated Data Types (CRDTs) identifizieren.

## State-based Convergent Replicated Data Type (CvRDT)

Bei den State-based Convergent Replicated Data Type (CvRDT) tauschen die Replikate Nachrichten mit den aktuellen Zustand des geänderten Objektes aus. Eine Änderung an einem CvRDT erfolgt zunächst lokal, um anschließend den neue Zustand des Objektes an alle anderen Replikate zu übermitteln. Über eine Merge-Operation können die Replikate dann den gespeicherten Zustand mit dem empfangenen neuen Zustand des Objektes zusammenführen. Die mathematische Grundlage dieser Methode sind Halbverbände. Die Merge-Operation muss kommutativ, transitiv und idempotent sein.

Damit die Daten der Replikate konvergieren, müssen folgende Bedingungen erfüllt sein:

1. Die Aktualisierungsfunktion ist monoton steigend.
2. Die möglichen Zustände des Objektes bilden eine partielle Ordnung.

Ein einfaches Beispiel, welches diese Bedingungen erfüllt, wäre eine Ganzzahl, wobei immer der höchste Wert gespeichert werden soll. In diesem Fall wird die partielle Ordnung durch die Größer-Relation (>) definiert. Die Merge-Operation ist das Maximum aus der aktuellen und der neuen Zahl.

Im Beispiel wird auf Knoten x1 der Wert auf 1 geändert, während gleichzeitig auf Knoten x2 der Wert auf 4 geändert wird. Unabhängig von der Reihenfolge in der die Änderungsmitteilung eingehen, konvergiert der Wert auf 4 bei allen drei Replikaten aufgrund der Maximum-Operation.

## Op-based Commutative Replicated Data Type (CmRDT)

Bei den Op-based Commutative Replicated Data Type (CmRDT) werden Änderungen zunächst lokal durchgeführt, um die ausgeführte Operation anschließend als Nachricht an die anderen Replikate zu übermitteln. Die Replikate führen diese Operation dann ebenfalls lokal aus. Bei CmRDTs müssen bestimmte Vorbedingungen erfüllt sein, um die spätere Konsistenz der Daten zu gewährleisten.

1. Jede Änderungsmitteilung wird genau einmal übertragen. Die Nachrichten dürfen also weder mehrfach ankommen, noch verloren gehen.
2. Die Änderungsmitteilung kommen in der Reihenfolge an, in der sie versendet wurden.

Durch die erste Bedingungen wird sichergestellt, dass die Änderungsmitteilung zuverlässig an alle Replikate zugestellt werden. Damit die CmRDTs auf den Replikaten konvergieren, müssen sie auch über jede Änderung informiert werden. Wenn nicht auf allen Replikaten die gleichen Änderungen durchgeführt werden, wird das System inkonsistent.  Änderungsmitteilung dürfen somit nicht verloren gehen oder mehrfach ankommen. Dieser Forderung kann durch die Wahl eines geeigneten Übertragsprotokoll Rechnung getragen werden.

Sämtliche parallel ausführbaren Operationen müssen kommutativ sein. Kommutative Operationen können in beliebiger Reihenfolge ausgeführt werden. Ein einfaches Beispiel ist die Addition. Das Term 1 + 2 + 3 liefert das selbe Ergebnis, wie der Term 1 + 3 + 2. Im Beispiel ist es also egal, ob ein Knoten nun zunächst die Änderung +2 oder die Änderung +3 verarbeitet. Beim Startzustand von 1 konvergiert der Wert bei allen Replikaten auf 6, unabhängig von der Reihenfolge in der die Änderungsmitteilung eingehen. Sofern diese Eigenschaft nicht gegeben ist, spielt die Reihenfolge der Änderungsmitteilung eine  Rolle. In diesem Fall muss das Übertragsprotokoll sicherstellen, dass die zweite Vorbedingung gegeben ist.

## Vergleich CvRDT und CmRDT

Op-based Commutative Replicated Data Type (CmRDT) und State-based Convergent Replicated Data Type (CvRDT) sind gegeneinander austauschbar. Jede Anforderung die mithilfe eines operations-basierten CmRDT umgesetzt werden kann, kann auch als zustandsbasierten CvRDT umgesetzt werden. Die beiden Typen unterscheiden sich in der Praxis jedoch erheblich. Während bei den zustandsbasierten CvRDT in der REgel der Ressourcenbedarf höher ist, da immer der gesamte Zustand übertragen werden muss, haben die operations-basierten CmRDT höhere Anforderungen an das Übertragsprotokoll. Es hängt demnach sehr vom Anwendungsfall ab, welcher Typ in einem konkreten Fall vorzuziehen ist. Um so Vorteilhafter erscheint die Tatsache, dass im Prinzip bei Vorgehensweisen zum Ziel führen.
