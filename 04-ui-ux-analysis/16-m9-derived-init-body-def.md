# M9: Derived, Init, Body und Def

## Status und Zweck

**Status:** `READY_FOR_REVIEW`  
**Verbindliche M9-Mockups:**

- `assets/mockups/class-properties-attributes.html`
- `assets/mockups/class-properties-operation-body.html`
- `assets/mockups/class-properties-definitions.html`
- `assets/mockups/package-properties-definitions.html`

**Operation-Body-Mockup:**
`assets/mockups/class-properties-operation-body.html`

**Attribute-Value-Source-Mockup:**
`assets/mockups/class-properties-attributes.html`

**Package-Definitions-Mockup:**
`assets/mockups/package-properties-definitions.html`

**Class-Definitions-Mockup:**
`assets/mockups/class-properties-definitions.html`

Definition deletion for Class and Package scope is specified in
`assets/mockups/delete-definition-modal.html` and
`04-ui-ux-analysis/38-delete-definition-modal.md`.

Die vier Mockups führen die gemeinsame M2-bis-M8-Shell, den hierarchischen
Explorer und die Class-Untertabs `Details`, `Attributes`, `Operations`,
`Generalizations` und `Definitions` fort. Das frühere M9-Gesamtmockup wurde
entfernt, weil seine fachlich relevanten Inhalte vollständig in die
spezialisierten, workflowbezogenen Darstellungen übernommen wurden.

M9 legt die Benutzeroberfläche für gespeicherte, initialisierte und abgeleitete
Attribute, OCL-Operationsrümpfe sowie zusätzliche Property- und
Operation-Definitionen fest. Die Funktionen werden in die vorhandene
Properties-Struktur integriert; M9 führt keine neue globale Workspace-Ansicht
ein und implementiert keinen produktiven Code.

## Verwendete Grundlagen

| Bereich | Verwendete Dokumente |
|---|---|
| Einstieg | `00-overview/03-documentation-map.md`, `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md` |
| UI/UX | `01-ui-overview.md`, `02-class-diagram-ui.md`, `03-object-diagram-ui.md`, `04-ocl-and-validation-ui.md`, `05-screenshot-traceability.md`, `07-m1-ui-baseline.md`, `10-redesign-design-principles.md` |
| Vorherige Mockups | `14-m7-operation-signatures-invocation.md`, `15-m8-pre-post-before-after.md` und deren HTML-Mockups |
| OCL/UML | `03-uml-ocl-domain/04-ocl-overview.md`, `09-ocl-extension-analysis/07-pre-post-derived-init.md` |
| Architektur und Vertrag | `06-frontend-analysis/03-frontend-architecture.md`, `11-properties-panel.md`, `07-integration-and-api/01-frontend-backend-contract.md`, `05-ocl-evaluation-flow.md` |
| Compliance und Planung | `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`, `15-full-ocl-uml-implementation-plan.md` |

Als visuelle Referenz wurden insbesondere
`01-class-diagram-class-properties.png`,
`03-class-diagram-invariant-properties.png`,
`06-object-diagram-object-properties.png`, `13-ocl-editor.png`,
`16-properties-invariants.png` und `17-new-class.png` verwendet. Sie belegen die
bestehende Workspace-Aufteilung sowie die Properties-Hauptseiten `Class`,
`Association` und `Invariant`.

Das aktuelle Frontend wurde nur hinsichtlich Workspace, Properties Panel,
OCL-Editor und Object Properties geprüft. Im aktuellen Backend wurden
`UmlAttribute`, `UmlOperation`, deren DTOs und Mapper sowie die vorhandene
`ocl.definition`-Komponente geprüft. Produktiver Code wurde nicht verändert.

Das originale USE-Projekt musste für M9 nicht zusätzlich geprüft werden. Die
OCL-Kontextanalyse, Compliance-Matrix und vorhandenen Backend-Verträge waren für
Terminologie und Abgrenzung ausreichend.

## Informationsarchitektur

Die bestehenden Hauptseiten werden nicht erweitert oder ersetzt:

| Properties-Hauptseite | Inhalt in M9 |
|---|---|
| `Class` | Attribute mit Value Source, Operation Details mit `Body`, Class-Definitionen und Zugang zu Package-Definitionen |
| `Association` | unverändert; abgeleitete Association Ends werden nicht in M9 vorgezogen |
| `Invariant` | unverändert; `derive`, `init`, `body` und `def` sind keine Invarianten |

Innerhalb von `Class` gilt folgende Hierarchie:

```text
Class
|- Details
|  `- Class data
|- Attributes
|  `- Attribute Details -> Stored | Init | Derived
|- Operations
|  `- Operation Details -> Signature | Preconditions | Postconditions | Body
|- Generalizations
`- Definitions
   `- Property Definition | Operation Definition
```

Property- und Operation-`def` liegen im eigenen Class-Untertab `Definitions`
beziehungsweise unter `Package Properties -> Definitions`. Sie bleiben vom
allgemeinen Details-Formular und von `Invariant` getrennt.

Damit bleibt die Oberfläche mit den Original-Screenshots und den überarbeiteten
Mockups M1 bis M8 konsistent. Fachlich fortgeschrittene Optionen werden erst im
jeweiligen Detailbereich sichtbar.

## Fachliche Begriffe und Darstellung

| Begriff | Bedeutung | Darstellung |
|---|---|---|
| Stored | normaler Snapshot-Slot | ohne Zusatzsymbol; Wert ist im Object Diagram bearbeitbar |
| Init | OCL-Ausdruck liefert bei Objekterstellung den Anfangswert | Value Source `Init`; Ausdruck am Attribut; erzeugter Slot bleibt danach bearbeitbar |
| Derived | Wert wird aus dem aktuellen Snapshot berechnet | UML-konformes `/attribute`; Badge `Calculated`; Wert ist readonly |
| Body | seiteneffektfreier OCL-Rumpf einer Query-Operation | Untersegment `Body` innerhalb der Operation Details |
| Property `def` | berechnetes OCL-Hilfsmerkmal | Definition mit Name, Rückgabetyp, Scope und Ausdruck |
| Operation `def` | parametrisierte OCL-Hilfsoperation | Definition mit Parametern, Rückgabetyp, Scope und Ausdruck |

Eine Init-Expression und eine Derivation sind gegenseitig ausgeschlossen.
`Init` erzeugt einen gespeicherten Anfangswert; `Derived` erzeugt gerade keinen
autoritativen Slot. Diese Unterscheidung wird nicht nur über Farbe, sondern über
Text, UML-Notation, Bedienbarkeit und Status sichtbar gemacht.

## Entworfene Ansichten

### Desktop-Hauptzustand

Das Klassendiagramm zeigt `Invoice` mit `/total : Real`. Nach Auswahl des
Attributs bleibt die Properties-Hauptseite `Class` aktiv. Die Attribute Details
enthalten Name, Typ und das segmentierte Feld `Stored | Init | Derived`.
`Derived` öffnet den kontextbezogenen OCL-Ausdruckseditor und zeigt Ergebnistyp,
Abhängigkeiten sowie Source Range.

Das einheitliche Mockup `class-properties-attributes.html` führt diese
M9-Festlegung direkt im regulären Attribute-Workflow fort. Es zeigt alle drei
Value Sources, einen Default Value ausschließlich für Stored, den Init-Ausdruck
mit weiterhin editierbarem Ergebnis-Slot sowie Derived mit `/`-Notation,
Derive-Ausdruck und readonly Darstellung im Object Diagram.

### Init-Zustand

`Invoice::retryCount : Integer` verwendet den Init-Ausdruck `0`. Ein Hinweis
stellt klar, dass das Backend diesen Ausdruck genau während der atomaren
Objekterstellung anwendet. Der danach erzeugte Slot bleibt ein normaler,
bearbeitbarer Wert.

### Derived-Wert im Object Diagram

Die Object Properties zeigen `/total = 129.90` mit `Calculated from current
snapshot`. Eingabefeld und Save-Aktion sind deaktiviert. Der Benutzer kann den
Wert und seine Herkunft sehen, ihn aber nicht wie einen Slot überschreiben.

### Operation Body

Der Body liegt in der bereits in M7 und M8 festgelegten Unterstruktur
`Class -> Operation Details -> Body`. Der Editor zeigt erwarteten Rückgabetyp,
ermittelten Ausdruckstyp und Source Diagnostics. M9 legt nur seiteneffektfreie
OCL-Query-Bodies fest; eine imperative Aktionssprache gehört nicht zu diesem
Mockup.

Das vertiefende Mockup `class-properties-operation-body.html` zeigt diesen
Zustand in derselben Shell wie `class-properties-operations.html`. Es enthält
die Operation-Auswahl, den aktiven Body-Untertab, Kontext und erwarteten Typ,
den OCL-Ausdruck, Diagnostics und Save/Cancel/Remove. Für eine Nicht-Query-
Operation ist das Anlegen eines M9-Bodys mit fachlicher Begründung deaktiviert.
Loading, Type Error, Empty, Success und Remove-Confirmation sind als getrennte
Zustände dargestellt.

### Class- und Package-Definitionen

Ein erweiterter Definitionsbereich innerhalb der Class Properties listet
Property- und Operation-`def`. Ein Editor enthält Scope, Kind, Name, Parameter,
Rückgabetyp und Ausdruck. Package-Scope ist als fortgeschrittene Auswahl
sichtbar, wird aber nicht als zusätzliche globale Navigation eingeführt.

Die verbindliche Class-Definition-Darstellung befindet sich in
`assets/mockups/class-properties-definitions.html` unter
`Class Properties -> Definitions`. Die fünf Class-Untertabs `Details`,
`Attributes`, `Operations`, `Generalizations` und `Definitions` werden in den
verbindlichen Class-Properties-Mockups einheitlich angezeigt. Das Details-
Formular enthält keinen doppelten Definitionseditor. Property-Definitionen deaktivieren den
Parametereditor. Operation-Definitionen aktivieren eine geordnete
Parameterliste. Definitionen werden unabhängig vom allgemeinen Klassenformular
gespeichert und vor dem Speichern auf Syntax, Namensauflösung, Scope und
Rückgabetyp geprüft. Loading, Empty, Source Error, Type Error, Success und
Delete Confirmation sind als überprüfbare Zustände dokumentiert.

Die verbindliche Package-Definition-Darstellung befindet sich in
`assets/mockups/package-properties-definitions.html`. Ein Package wird im
Explorer ausgewählt und öffnet innerhalb des bestehenden Class-Diagram-
Workspaces `Package Properties -> Definitions`. Package-Definitionen haben kein
implizites klassenbezogenes `self`. Sie verwenden Parameter, andere sichtbare
Definitionen sowie importierte oder qualifizierte Namen. Das Mockup zeigt eine
Property Definition, eine Operation Definition mit aktivem, geordnetem
Parametereditor sowie Empty, Loading, unbekannten qualifizierten Namen,
unzulässiges `self`, Rückgabetypfehler, Success und Delete Confirmation.

### Fehler und Bestätigung

M9 zeigt mindestens:

- Typabweichung zwischen Ausdruck und Attribut- beziehungsweise Rückgabetyp,
- unbekannte Namen und ungültigen Scope als Source Diagnostic,
- einen Ableitungszyklus mit fachlicher Abhängigkeitskette,
- einen Loading-/Disabled-Zustand während Prüfung und Neuberechnung,
- einen Empty State für fehlende Definitionen,
- eine Bestätigung beim Wechsel von `Stored` zu `Derived`, weil bestehende
  Slotwerte ihre Autorität verlieren,
- eine sichtbare Erfolgsmeldung nach Speichern und Neuberechnung.

### Schmaler Viewport

Das Properties Panel wird als intern scrollbarer Drawer dargestellt. Die
Hauptseiten `Class`, `Association` und `Invariant` bleiben sichtbar. Value
Source und Operation-Untersegmente werden erst innerhalb von `Class` angezeigt.
Diagnosen bleiben direkt am Ausdruck und zusätzlich über den Diagnostics-
Bereich erreichbar.

## Interaktionsregeln

1. Das Auswählen eines Attributs öffnet dessen Details in `Class` und verändert
   nicht die globale Workspace-Ansicht.
2. Der Wechsel der Value Source aktualisiert zunächst nur den Entwurf. Das
   Backend prüft Typ, Abhängigkeiten und Auswirkungen vor dem Anwenden.
3. `Stored -> Derived` verlangt bei vorhandenen Objekten eine
   Auswirkungsbestätigung. `Derived -> Stored` muss später klären, wie Werte für
   existierende Objekte initialisiert werden.
4. Speichern ist bei Syntax-, Typ-, Scope- oder Zyklusfehlern deaktiviert.
5. Nach erfolgreichem Speichern zeigt die UI eine Bestätigung und die neue
   Modellrevision; abgeleitete Objektwerte werden neu geladen.
6. Derived-Werte besitzen im Object Diagram keine editierbare Slot-Aktion.
7. `def` wird im Kontext verwaltet und ist nicht in der Invariant-Liste
   enthalten.
8. Fokus kehrt nach Dialog oder Drawer zum auslösenden Attribut, zur Operation
   oder Definition zurück.

## Compliance-Zuordnung

| Matrix-ID | M9-Abdeckung |
|---|---|
| `CM-CTX-004` | Operation Body, Ergebnistyp und Body-Diagnostic |
| `CM-CTX-005` | Derived Property, readonly Darstellung, Abhängigkeiten und Zyklusdiagnose |
| `CM-CTX-006` | Initial Value als Teil atomarer Objekterstellung |
| `CM-CTX-007` | Property- und Operation-`def` mit Context und Scope |
| `CM-CTX-008` | Package-/Namespace-Kontext, Imports und qualifizierte Namensauflösung |
| `CM-UML-001` | stabile fachliche Referenzen zwischen Attribut, Objektwert und Diagnostic |
| `CM-OCL-002` | Source Location wird für Typ-, Scope- und Rekursionsfehler vorausgesetzt |

`CM-CTX-007` bleibt gemäß Compliance-Matrix partiell, weil eigenständig
persistierte und packageweit sichtbare `def`-Definitionen backendseitig noch
offen sind. `CM-CTX-008` ist für Package-/Namespace-Kontext, Imports und
Sichtbarkeit bereits verifiziert. M9 gestaltet den später benötigten
Definitionsworkflow, implementiert jedoch keine Persistenz oder API.

## Vorläufige Domänen-, API- und DTO-Anforderungen

### Domänenmodell

- Ein Attribut benötigt eine eindeutige Value Source `STORED`, `INIT` oder
  `DERIVED` sowie höchstens den dazu passenden OCL-Ausdruck.
- Init und Derive müssen typgeprüft werden; Derive benötigt eine
  Abhängigkeitsanalyse mit Zyklenerkennung.
- Eine Operation benötigt einen optionalen OCL-Body, dessen Typ zum Rückgabetyp
  konform ist.
- `def` benötigt stabile Identität, Kind, Class-/Package-Scope, Parameter,
  Rückgabetyp und Ausdruck.
- Berechnete Werte dürfen nicht als normale Slots persistiert oder über eine
  Slot-Mutation überschrieben werden.
- Cache oder Memoisierung muss an Modell- und Snapshot-Revision gebunden sein.

### API und DTOs

Ein späterer Vertrag muss mindestens ausdrücken können:

```ts
type AttributeValueSource = 'STORED' | 'INIT' | 'DERIVED';

interface UmlAttributeDto {
  id: string;
  name: string;
  type: string;
  valueSource: AttributeValueSource;
  initExpression?: string;
  deriveExpression?: string;
}

interface UmlOperationDto {
  id: string;
  name: string;
  returnType: string;
  query: boolean;
  bodyExpression?: string;
}

interface OclDefinitionDto {
  id: string;
  scopeKind: 'CLASS' | 'PACKAGE';
  scopeName: string;
  kind: 'PROPERTY_DEF' | 'OPERATION_DEF';
  name: string;
  parameters: Array<{ name: string; type: string }>;
  returnType: string;
  expression: string;
}

interface ComputedPropertyValueDto {
  objectName: string;
  propertyName: string;
  value: unknown;
  valueType: string;
  editable: false;
  source: 'DERIVED';
  revision: number;
}
```

Die Feldnamen sind vorläufig. Der aktuelle Backend-Vertrag enthält bereits
`derived`, `deriveExpression`, `initExpression` und `bodyExpression`; die UI-
Semantik `valueSource`, eigenständige persistierte `def`-DTOs und die explizite
Computed-Value-Metadatenstruktur müssen vor Implementierung abgeglichen werden.
Diagnosen benötigen Code, Severity, nutzerfreundliche Message, fachlichen
Context, Source Range und betroffene Modellrevision.

## Spätere Frontend-Anforderungen

- Die vorhandenen Properties-Hauptseiten bleiben unverändert erhalten.
- Value Source wird als Segmented Control in Attribute Details umgesetzt.
- Ein gemeinsamer OCL-Ausdruckseditor wird kontextabhängig für Init, Derive,
  Body und `def` wiederverwendet.
- Das Frontend führt keine fachliche Evaluation durch und erfindet keine
  berechneten Werte als Fallback.
- Object Properties stellen Derived-Werte readonly und mit textlicher Herkunft
  dar.
- Operation Body bleibt ein Untersegment der Operation Details.
- Definitionslisten und umfangreiche Diagnostics sind intern scrollbar.
- Status darf nicht ausschließlich durch Farbe kommuniziert werden; Fokus,
  Tastaturbedienung, ausreichend große Controls und verständliche Hilfe folgen
  den Redesign-Prinzipien.

## Annahmen und offene Entscheidungen

**Annahmen**

- M9 behandelt Body als seiteneffektfreien OCL-Query-Body.
- Init wird genau bei erfolgreicher atomarer Objekterstellung angewendet.
- Ein Derived-Wert wird aus der aktuellen Snapshot-Revision berechnet.
- Das führende `/` ist die sichtbare UML-Notation für Derived Properties.

**Offen**

- Ob und wie `Derived -> Stored` Werte für bereits existierende Objekte
  materialisiert.
- Ob Package-`def` direkt in den Class Properties oder zusätzlich über einen
  ausgewählten Package-Knoten geöffnet wird.
- Welche Cache-Strategie das Backend verwendet und wie ein veralteter Wert
  ausgewiesen wird.
- Welche Rekursion bei `def` fachlich erlaubt ist und welches Evaluationsbudget
  gilt.
- Ob nicht-query Operations später eine getrennte Aktionssprache erhalten.

## Akzeptanzkriterien

| ID | Kriterium | Ergebnis |
|---|---|---|
| `M9-AC-01` | `Stored`, `Init` und `Derived` sind fachlich und visuell unterscheidbar. | erfüllt |
| `M9-AC-02` | Derived Properties tragen `/`, sind als berechnet bezeichnet und im Object Diagram readonly. | erfüllt |
| `M9-AC-03` | Body liegt innerhalb `Class -> Operation Details` und nicht auf einer neuen Hauptseite. | erfüllt |
| `M9-AC-04` | Property- und Operation-`def` besitzen Class-/Package-Scope und sind von Invarianten getrennt. | erfüllt |
| `M9-AC-05` | Typ-, Scope- und Zyklusfehler besitzen Source Range und fachliche Namen. | erfüllt |
| `M9-AC-06` | Loading, Error, Disabled, Empty, Confirmation und Success sind dargestellt. | erfüllt |
| `M9-AC-07` | Das Properties Panel ist im schmalen Viewport intern scrollbar. | erfüllt |
| `M9-AC-08` | Backendautorität und vorläufige API-/DTO-Anforderungen sind dokumentiert. | erfüllt |
| `M9-AC-09` | Spätere Mockup-Schritte und produktiver Code wurden nicht vorgezogen. | erfüllt |
| `M9-AC-10` | M9 verwendet dieselbe Shell, Explorer-Hierarchie und Class-Properties-Struktur wie M2 bis M8. | erfüllt |

## Abgrenzung

M9 entwirft ausschließlich Derived, Init, Body und `def`. Enum/DataType und der
gemeinsame Type Picker folgen in M10, Compliance-Anzeigen in M11, optionale
State-Machine- und `OclMessage`-Oberflächen in M12/M13 und die übergreifende
responsive beziehungsweise barrierebezogene Abnahme in M14. Der Gesamtstatus
bleibt `MOCKUP`; `BACKEND_READY` wird nicht gesetzt.
