# M7 Operation Invocation im Object Diagram

## Entscheidung

Die eigentliche Operation Invocation beginnt im Object Diagram. Das Class
Diagram bleibt für die Modellierung der Operationssignatur zuständig. Diese
Aufteilung folgt der bestehenden Trennung zwischen UML-Typstruktur und
konkretem Snapshot.

**Verbindliches Invocation-Mockup:**
`assets/mockups/object-diagram-operation-invocation.html`

Die Signaturbearbeitung ist verbindlich in
`assets/mockups/class-properties-operations.html` dokumentiert. Das frühere
kombinierte Mockup wurde entfernt, weil seine Invocation-Aktion im
Class-Properties-Panel dem festgelegten Object-Diagram-Workflow widersprach und
seine relevanten Signaturfunktionen vollständig übernommen wurden.

## Workflow

1. Der Benutzer öffnet das Object Diagram.
2. Er wählt ein konkretes Objekt im Explorer oder auf dem Canvas aus.
3. Das ausgewählte Objekt wird zum eindeutigen Receiver.
4. `Object Properties` verwendet die fachliche Struktur `Object`,
   `Associations` und `Operations`. Unter `Operations` werden nur Operationen, die für den
   Laufzeittyp des Receivers verfügbar sind.
5. Der Benutzer wählt eine Operation und gibt Werte für `in`- und
   `inout`-Parameter ein.
6. `Operation ausführen` startet genau eine atomare Invocation.
7. Ergebnis, Out-Werte und betroffene Objektänderungen bleiben im
   Snapshot-Kontext sichtbar.

## Dialogregel

Die reguläre Invocation verwendet kein zusätzliches Eingabe- oder
Ergebnismodal. Receiver, Operation und Argumente liegen im Bereich
`Object Properties > Operations`. Während der Ausführung ersetzt dort ein
Loading-Zustand die Eingabeaktion. Anschließend erscheinen Erfolg, Ergebnis,
Out-Werte, Fehler oder Rollback im selben Bereich; umfangreiche Details können
zusätzlich in der Console beziehungsweise im Ergebnisbereich stehen.

Ein fokussierter Bestätigungsdialog ist nur zulässig, wenn eine serverseitige
Auswirkungsvorschau vor der Ausführung eine ausdrückliche Zustimmung verlangt,
beispielsweise bei Änderungen an mehreren Objekten oder Links. Dieser Dialog
zeigt die Auswirkungen und bietet ausschließlich Abbruch oder bestätigte
Ausführung an.

## Mehrere kompatible Objekte

Mehrere Objekte desselben Typs werden getrennt im Explorer und Canvas gezeigt.
Nur das ausdrücklich ausgewählte Objekt ist Receiver. Die UI wählt niemals
automatisch das erste Objekt und führt eine Operation nicht implizit auf allen
Objekten aus. Objektparameter verwenden einen eigenen Picker und dürfen auf ein
anderes kompatibles Objekt verweisen.

Bei keinem ausgewählten Receiver bleibt der Operationsbereich deaktiviert und
fordert zur Objektauswahl auf. Bei genau einem vorhandenen kompatiblen Objekt
darf dieses nach einer expliziten Benutzeraktion ausgewählt, aber nicht
automatisch ausgeführt werden.

## Abgrenzung zum Class Diagram

Im Class Diagram werden Name, Sichtbarkeit, Parameter, Richtungen, Rückgabetyp,
`abstract` und `query` bearbeitet. Pre-/Postconditions und Body folgen in M8
und M9 ebenfalls im Modellkontext. Das Class Diagram führt keine Operation auf
einem Snapshot aus.

## Akzeptanzkriterien

- [x] `Object Diagram` ist im Invocation-Hauptzustand aktiv.
- [x] Ein konkretes Objekt ist sichtbar als Receiver ausgewählt.
- [x] Mehrere kompatible Objekte bleiben getrennt auswählbar.
- [x] Operationsliste und Argumentfelder liegen in `Object Properties`.
- [x] Die Hauptsegmente heißen `Object`, `Associations` und `Operations`;
  konkrete Beziehungsinstanzen werden innerhalb von `Associations` als
  `Object Links` bezeichnet.
- [x] Eine Invocation besitzt genau einen Receiver.
- [x] Es gibt keine implizite Batch-Ausführung.
- [x] Ergebnis und Objektänderungen verbleiben im Object-Diagram-Kontext.
- [x] Es gibt kein redundantes Eingabe- oder Ergebnismodal.
- [x] Ein Modal bleibt auf eine erforderliche Auswirkungsbestätigung begrenzt.
- [x] Desktop und schmaler Viewport besitzen keinen horizontalen Überlauf.

## Auswirkungen

Die Navigation muss beim Öffnen des Operationsbereichs den ausgewählten
`objectId`-Receiver erhalten. Der Invocation-Vertrag selbst bleibt fachlich
unverändert: Receiver-ID, Operation-ID, typisierte Argumente und erwartete
Snapshotrevision werden an das Backend übertragen. Produktiver Code wird durch
diese Mockup-Entscheidung noch nicht verändert.
