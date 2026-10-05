# Unified Bottom Panel States

## Zweck und Status

**Status:** `REFERENCE_MOCKUP_COMPLETE`

Das Referenzmockup `assets/mockups/workspace-bottom-panel.html` definiert die
vier stabilen Bottom-Panel-Tabs der Modellierungsoberfläche. Es konsolidiert die
Ergebnisdarstellungen aus `class-properties-operation-body.html`,
`package-properties-definitions.html`, `operation-contracts.html`,
`classifier-type-picker.html` und
`datatype-properties.html`.

Diese Datei ist die verbindliche Bottom-Panel-Referenz für Class Diagram,
Object Diagram und OCL Editor. Einzelmockups dürfen den aktiven Tab und dessen
fachlichen Inhalt variieren, aber nicht Tabfolge, Höhenlogik, Typografie,
Unterstreichung des aktiven Tabs oder das dreispaltige Ergebnisraster neu
gestalten. Die eingebetteten Object-Diagram-Mockups verwenden dafür gemeinsam
`assets/mockups/workspace-bottom-panel-shell.css`; ihre objektbezogenen
Explorer und Properties Panels bleiben davon unberührt.

## Gemeinsame Struktur

Alle Zustände verwenden dieselbe untere Workspace-Fläche und dieselbe
Tabreihenfolge: `Console`, `Diagnostics`, `Validation Results`,
`Invocation Results`. Der aktive Tab erhält eine blaue Unterstreichung. Der
Inhalt ist intern scrollbar und verändert die Höhe von Canvas und Properties
nicht. Ergebniszeilen zeigen links die Art oder den Status, mittig fachliche
Namen und Meldungen sowie rechts Source-, Revision- oder Aktionsmetadaten.

## Console

Die Console ist ein chronologisches Aktivitätsprotokoll. Sie zeigt Auswahl-,
Save-, Import-, Refresh- und vergleichbare Workspace-Ereignisse. Sie verwendet
fachliche Namen und qualifizierte Namen, aber keine internen IDs als primäre
Information. Ein Ereignis darf auf das zugehörige Element navigieren.

Die Console ersetzt weder strukturierte Diagnostics noch Validation Results.

## Diagnostics

Diagnostics enthalten Parser-, Typechecker- und lokale semantische Befunde aus
dem gerade bearbeiteten Ausdruck oder Modellelement. Dazu gehören Severity,
fachliche Meldung, OCL-/UML-Kontext, erwarteter und tatsächlicher Typ sowie eine
Source Range, soweit Quelltext beteiligt ist. Ein Befund navigiert zum Feld
oder Quelltextbereich.

Erfolgreiche lokale Prüfungen dürfen als `VALID` erscheinen. Snapshotweite
Invariantenergebnisse gehören trotzdem nicht hierher.

## Validation Results

Validation Results entstehen durch `Check Constraints` und beziehen sich auf
eine konkrete persistierte Modell- und Snapshotrevision. Die Zusammenfassung
nennt Anzahl geprüfter, erfüllter, verletzter und nicht auswertbarer
Constraints. Das gemeinsame Result Set umfasst:

- ausgewertete Invarianten mit Context Classifier und betroffenem Objekt oder
  betroffener Objektmenge,
- statische Parser-, Typ- und Auflösungsbefunde persistierter Pre- und
  Postconditions,
- statische Parser-, Typ- und Auflösungsbefunde persistierter Class- und
  Package-Definitions.

Ein konkretes Laufzeitergebnis einer Pre- oder Postcondition gehört weiterhin
zu `Invocation Results`. Validation Results zeigen bei Contracts nur, ob die
persistierte Definition im Projektkontext fachlich prüfbar ist.

Jede Zeile trägt eine stabile Referenz auf Owner, OCL-Quelle und optional das
betroffene Snapshotobjekt. Abhängig vom Ergebnis stehen `Open in Class
Diagram`, `Open in Object Diagram` und `Open in OCL Editor` zur Verfügung:

- `Open in Class Diagram` selektiert die besitzende Klasse, Operation oder
  Definition in den passenden Properties.
- `Open in Object Diagram` selektiert das von einer Invariantenverletzung
  betroffene Objekt.
- `Open in OCL Editor` öffnet den Ausdruck und fokussiert die Source Range.

## Invocation Results

Invocation Results entstehen nach der Ausführung einer Operation im Object
Diagram. Sie nennen Receiver, Argumente, Return Value beziehungsweise Fehler,
Precondition- und Postcondition-Ergebnisse sowie Before und Candidate After.
Bei einer fehlgeschlagenen Precondition wird die Ausführung als blockiert
angezeigt. Bei einer fehlgeschlagenen Postcondition wird die atomare
Zurücknahme einschließlich neuer oder gelöschter Objekte dokumentiert.

Dieser Tab wird nach einer Invocation automatisch aktiviert und ersetzt keine
allgemeine Constraint-Validierung.

## Aktivierungsregeln

| Auslöser | Automatisch aktiver Tab |
|---|---|
| Auswahl, Save, Import oder Refresh ohne Fehler | `Console` |
| Parser-, Typ- oder lokaler semantischer Befund | `Diagnostics` |
| `Check Constraints` abgeschlossen | `Validation Results` |
| Operation Invocation abgeschlossen oder blockiert | `Invocation Results` |

Automatisches Aktivieren darf den Nutzer nicht aus einem gerade untersuchten
Fehlerzustand reißen. Neue Ergebnisse erhalten dann einen sichtbaren Zähler,
bis der Benutzer den Tab öffnet.

## Anforderungen an API und Frontend

Strukturierte Einträge benötigen stabile Ergebnis-ID, Constraint Kind
(`INVARIANT`, `PRECONDITION`, `POSTCONDITION`, `DEFINITION`), Severity,
fachliche Owner- und Elementreferenz, verständlichen Namen, Nachricht,
Modell-/Snapshotrevision und optional Source Range, Objektbezug, Invocation-ID
und State-Diff. Navigationsziele werden aus stabilen Referenzen gebildet, nicht
aus formatiertem Meldungstext. Das Frontend darf Kategorien und Semantik nicht
aus Shelltext ableiten.

## Akzeptanzkriterien

| ID | Kriterium |
|---|---|
| `BOTTOM-01` | Alle vier Tabs verwenden dieselbe Reihenfolge und Panelgeometrie. |
| `BOTTOM-02` | Console enthält ausschließlich nachvollziehbare Workspace-Ereignisse. |
| `BOTTOM-03` | Diagnostics enthalten Kontext und Source Range, soweit vorhanden. |
| `BOTTOM-04` | Validation Results sind an Modell- und Snapshotrevision gebunden. |
| `BOTTOM-05` | Invocation Results zeigen Receiver, Contracts und atomaren State-Ausgang. |
| `BOTTOM-06` | Einträge navigieren über stabile Referenzen zu fachlichen Elementen. |
| `BOTTOM-07` | Umfangreiche Inhalte scrollen innerhalb des Bottom Panels. |
| `BOTTOM-08` | Validation Results führen Invarianten, statische Contract-Befunde und Definitionsfehler in einem revisionsgebundenen Result Set zusammen. |
| `BOTTOM-09` | Ergebnisse öffnen abhängig von ihren Referenzen Class Diagram, Object Diagram oder OCL Editor; der OCL Editor fokussiert eine vorhandene Source Range. |
| `BOTTOM-10` | Laufzeitresultate konkreter Pre-/Postcondition-Auswertungen bleiben ausschließlich unter Invocation Results. |
