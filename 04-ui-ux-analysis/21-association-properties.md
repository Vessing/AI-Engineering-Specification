# Association Properties

The destructive action `Delete Association` is specified separately in
`assets/mockups/delete-association-modal.html` and
`04-ui-ux-analysis/34-delete-association-modal.md`.

## Zweck

Diese offizielle Dokumentationsansicht konsolidiert den vollständigen
Association-Properties-Workflow aus M4, M5 und M6. Sie ist kein neuer
Implementierungsschritt und ersetzt deren fachliche Traceability nicht.

**Mockup:** `assets/mockups/association-properties.html`

**Runtime-Ableitungsansicht:**
`assets/mockups/object-properties-associations.html`, fachlich
beschrieben in `45-object-diagram-derived-association-ends.md`.

The preceding creation flow is documented in
`28-create-association-modal.md`. It creates the stable association and then
hands the selected result to this complete properties editor.

## Einheitlicher Workflow

1. Der Benutzer wählt eine Association im Explorer oder auf dem Canvas aus.
2. Die Hauptseite `Association` zeigt Name und eine dynamische Liste fachlich
   benannter Ends.
3. Jedes End enthält Classifier, Rolle, Multiplizität, Navigationseigenschaften,
   `subsets`, `redefines`, Qualifier und `Aggregation kind`.
4. `End hinzufügen` erweitert denselben Editor von einer binären zu einer
   n-ären Association.
5. `Association Class hinzufügen` erscheint auf Association-Ebene, solange
   keine Association Class verknüpft ist.
6. `Association speichern` validiert und speichert Association und Ends
   gemeinsam.

### Association-Class-Modal

`Association Class hinzufügen` öffnet einen kompakten Modal-Dialog. Die bereits
ausgewählte Association wird mit fachlichem Namen und ihren End-Classifiern
read-only angezeigt und nicht erneut über einen freien Picker gesucht. Der
Benutzer vergibt den Namen und wählt einen Namespace. Mit
`Association Class erstellen` wird die Class atomar erzeugt und mit genau
dieser Association verbunden.

Nach Erfolg schließt der Dialog, der Empty-State zeigt die verknüpfte
Association Class und die Aktion wechselt zu `Association Class öffnen`.
Initiale Attribute und Operationssignaturen können bereits im Create-Dialog
angelegt werden. Die vollständige Bearbeitung erfolgt anschließend unter
`Class Properties · Attributes` und `Class Properties · Operations`. Der
Association-Workspace erhält keinen zweiten Feature-Editor für die Association
Class. Ein allgemeiner Generalizations-Tab wird für Association Classes nicht
angeboten.

## Integrierte M4-End-Metadaten

Jedes Association End enthält zusätzlich `navigable`, `ordered`, `unique`,
`derived`, `union`, `subsets` und `redefines`. Rolle und Multiplizität bleiben
der einfache Kernweg. `Union`, `Subsets` und `Redefines` liegen unter
`Fortgeschrittene End-Metadaten`.

`union` ist deaktiviert, solange `derived` nicht aktiv ist. Wird `derived`
ausgeschaltet, wird ein gesetztes `union` zurückgenommen oder die Änderung vor
dem Speichern bestätigt. `subsets` und `redefines` verwenden auswählbare
Association Ends im Format `Association · roleName`; interne IDs werden nicht
angezeigt.

Für mehrwertige Ends zeigt das Panel die OCL-Navigationsergebnisart:

| Ordered | Unique | Ergebnis |
|---|---|---|
| false | true | `Set(T)` |
| false | false | `Bag(T)` |
| true | false | `Sequence(T)` |
| true | true | `OrderedSet(T)` |

Bei einer oberen Multiplizitätsgrenze von eins bleibt das Ergebnis unabhängig
von `ordered` und `unique` einzelwertig. Die Vorschau ist abgeleitete Anzeige;
die normative Typableitung bleibt Aufgabe des Backend-Typecheckers.

Das Association-Properties-Mockup dokumentiert außerdem Editing, Loading, Inline Error,
Disabled, Empty, Confirmation und Success. Lange Endlabels bleiben an ihren
Ends verankert und werden umgebrochen oder gekürzt, ohne den Canvas oder
Interaktionsflächen zu überdecken.

Feldbezogene Modelldiagnosen markieren das konkrete Association End und Feld.
Das Mockup zeigt `ASSOCIATION_END_MULTIPLICITY_INVALID`,
`ASSOCIATION_END_UNIQUE_CONFLICT` und
`ASSOCIATION_END_QUALIFIER_DUPLICATE`. Object-Link- und
Association-Class-Instanzfehler gehören nicht in diesen Modelleditor, sondern
in die Object-Diagram-Zustände.

`ordered` und `unique` sind keine gegenseitig ungültigen UML-Schalter. Die vier
Kombinationen bestimmen Set, Bag, Sequence oder OrderedSet. Eine
feldbezogene Konfliktdiagnose entsteht erst, wenn vorhandene Links eine neu
gewählte Unique-Semantik verletzen oder eine Ordered-Migration keine
konsistente Positionierung besitzt.

## Fachliche Zuordnung

- `None`, `Shared` und `Composite` gehören zum konkreten Association End.
- Qualifierdefinitionen gehören ebenfalls zum konkreten Association End.
- Eine Association Class gehört zur Association und wird nach dem Erstellen
  unter der Hauptseite `Class` weiterbearbeitet.
- Rolle, Multiplizität und Endmarker bleiben am jeweiligen Classifier-Ende des
  Diagramms verankert.
- Ein drittes oder weiteres End verwendet denselben Endabschnitt und erzeugt
  keinen separaten n-ären Editor.

## Akzeptanzkriterien

- [x] `Association` ist als aktive Properties-Hauptseite sichtbar.
- [x] Association-Stammdaten und Ends befinden sich in einem Panel.
- [x] Erweiterte End-Metadaten, Qualifier und Aggregation Kind sind demselben
  Endmodell zugeordnet.
- [x] `derived`, davon abhängiges `union`, `subsets` und `redefines` sind für
  jedes End im gemeinsamen Editor dargestellt.
- [x] Die Collection-Vorschau unterscheidet `Set`, `Bag`, `Sequence`,
  `OrderedSet` und einzelwertige Navigation.
- [x] M4-Loading-, Error-, Disabled-, Empty-, Confirmation- und
  Success-Zustände sind in der Unified-Datei enthalten.
- [x] `End hinzufügen` und `Association Class hinzufügen` liegen auf
  Association-Ebene und sind fachlich voneinander getrennt.
- [x] `Association Class hinzufügen` öffnet einen Modal-Dialog mit festgelegter
  Association, Name, Namespace, Cancel und atomarer Create-Aktion.
- [x] Nach erfolgreicher Erstellung ersetzt `Association Class öffnen` den
  Empty-State und der Canvas zeigt die gestrichelte Zuordnung.
- [x] Der Properties-Bereich ist intern scrollbar.
- [x] Desktop und schmaler Viewport verwenden denselben Workflow.
- [x] Die Datei ist als offizielle Association-Properties-Referenz gekennzeichnet.
- [x] Multiplizität, Ordered/Unique und Qualifierdefinitionen besitzen
  feldbezogene Fehler mit stabiler Association-End-Referenz.

## Abgrenzung

Die Datei begründet keine neuen Domänen-, API- oder DTO-Anforderungen. Sie
übernimmt die bereits in `11-m4-association-end-properties.md` dokumentierten
M4-Anforderungen als gemeinsame visuelle Referenz. Die fachlichen Verträge,
Compliance-Zuordnungen und Backend-Typechecker-Regeln bleiben bis zu einer
gesonderten Dokumentkonsolidierung im M4-Analysedokument maßgeblich.
Produktiver Frontend- und Backend-Code bleiben unverändert.
Die Runtime-Referenz begründet insbesondere keine bereits vollständige
Backend-Semantik für `CM-UML-014`; sie spezifiziert deren erwartete UI- und
Provenienzdarstellung.
