# Full OCL and UML Mockup Roadmap

## Zweck

Diese Roadmap definiert die Mockups, die als geschlossene Konzeptionsphase vor
der Umsetzung der UI-wirksamen Backend- und Frontend-Features aus
`09-ocl-extension-analysis/15-full-ocl-uml-implementation-plan.md` erstellt und
fachlich freigegeben werden müssen. Sie erweitert die vorhandenen Screenshots
für Dashboard, Class Diagram, Object Diagram, Properties Panel, Modals, OCL
Editor und Validation Results. Die bestehende Workspace-Struktur bleibt erhalten.

Neue Mockups werden nur dort verlangt, wo neue Interaktionen, Auswahlzustände,
Diagrammnotationen oder fachliche Eingaben entstehen. Reine Parser-,
Typechecker-, Evaluator- oder Test-Harness-Arbeiten benötigen keine neuen
Ansichten.

Die Umsetzungsreihenfolge lautet verbindlich:

1. Alle für das beschlossene Zielprofil erforderlichen Mockup-Schritte werden
   erstellt.
2. Fachliche Felder, Interaktionen, UML-Notation, Zustände und Fehlerszenarien
   werden geprüft und freigegeben.
3. Aus den freigegebenen Mockups werden Domänenmodell, API-Verträge und DTOs
   abgeleitet beziehungsweise bestätigt.
4. Erst danach beginnt die Umsetzung der zugehörigen Backend-Features.
5. Die Frontend-Umsetzung folgt auf Basis derselben freigegebenen Mockups und
   Backend-Verträge.

M12 und M13 wurden mit Backend-Schritt B21 ausdrücklich als `OUT_OF_SCOPE`
beschlossen. Sie werden im aktuellen Zielprofil nicht als Mockups umgesetzt.

## Gestaltungsregeln

Alle Schritte folgen den priorisierten und verbindlichen Vorgaben aus
`04-ui-ux-analysis/10-redesign-design-principles.md`:

1. Einfachheit, Anfängerfreundlichkeit und leichte Navigation haben höchste
   Priorität, ohne fortgeschrittene USE-Funktionalität zu entfernen.
2. Die visuelle Oberfläche wird modernisiert, behält aber USE-Identität,
   UML-Notation und arbeitsorientierte Informationsdichte.
3. Kontrast, Lesbarkeit, Tastaturzugang und kontextbezogene Hilfe werden in
   jedem Mockup berücksichtigt und nicht erst nachträglich ergänzt.

- Neue Funktionen werden in Class Diagram, Object Diagram, OCL Editor,
  Properties Panel und bestehenden Modals integriert.
- Das Class-Diagram-Properties-Panel behält in allen Mockups die drei
  Hauptseiten `Class`, `Association` und `Invariant` aus den Screenshots 15 bis
  17. Neue Features erzeugen keine weiteren gleichrangigen Hauptseiten:
  Klassen-, Generalisierungs-, Attribut-, Operations- und Contractfunktionen
  liegen unter `Class`; Association-Erweiterungen liegen unter `Association`;
  Klasseninvarianten bleiben unter `Invariant`.
- Unterseiten wie `Signature`, `Preconditions`, `Postconditions` und später
  `Body` erscheinen nur innerhalb eines ausgewählten Operationsbereichs der
  Seite `Class`. Sie ersetzen nicht die drei Properties-Hauptseiten.
- Properties Panels bleiben intern scrollbar und behalten die vorhandene
  Segmentstruktur.
- UML-Notation wird nicht durch frei erfundene Symbole ersetzt.
- Diagrammelemente bleiben verschiebbar; neue Edge- und Labelarten müssen mit
  gespeicherten Layoutdaten funktionieren.
- Dialoge zeigen Pflichtfelder, Fehler, Loading, Erfolg und Abbruch.
- Für jede neue Ansicht werden Default-, Selected-, Editing-, Error- und
  Disabled-Zustände geprüft, soweit sie fachlich vorkommen.
- Jeder Schritt dokumentiert einen einfachen Kernweg, progressive Offenlegung
  fortgeschrittener Optionen, sichtbares Aktionsfeedback und einen
  kontextbezogenen Hilfeeinstieg.

## Schrittübersicht

| Mockup-Schritt | Thema | Bezug zum OCL-/UML-Plan | Priorität |
|---|---|---|---|
| M1 | Bestehende UI-Baseline und Komponentenraster | alle UI-Schritte | P0 |
| M2 | Generalisierung und abstrakte Klassen | Schritt 4 | P0 |
| M3 | Sichtbarkeit, Namespaces und Imports | Schritt 5 | P1 |
| M4 | Erweiterte Association-End-Properties | Schritt 6 | P0 |
| M5 | Qualifier und n-äre Associations | Schritt 7 | P1 |
| M6 | Association Classes und Linkobjekte | Schritt 8 | P1 |
| M7 | Operation Signatures und Invocation | Schritt 9 | P0 |
| M8 | Pre-/Postcondition und Vor-/Nachzustand | Schritt 18 | P0 |
| M9 | Derived, Init, Body und Def | Schritt 19 | P1 |
| M10 | Enum-, DataType- und Type-Auswahl | Schritt 13 | P1 |
| M11 | OCL Compliance und Featureanzeige | Schritte 20 und 25 | P1 |
| M12 | State Machine und `oclInState` | `NOT_REQUIRED`, durch B21 ausgeschlossen | P3 |
| M13 | Operation Trace und `OclMessage` | `NOT_REQUIRED`, durch B21 ausgeschlossen | P3 |
| M14 | Responsive, Accessibility und visuelle Abnahme | vor Frontend-Umsetzung je Paket | P0 |

**Fortschritt:** M1 bis M11 sind als `READY_FOR_REVIEW` dokumentiert. M12 und
M13 sind durch die B21-Exclude-Entscheidung `NOT_REQUIRED`; M14 ist durch die
reale F12-Desktop-Abnahme vom 1. September 2026 abgeschlossen.

**Übergreifende Konsistenzprüfung:** M1 bis M11 wurden gemeinsam auf Shell,
Properties-Hierarchie, Begriffe, Aktionen, Responsive-Verhalten und den
durchgehenden Workflow geprüft. Das Ergebnis steht in
`04-ui-ux-analysis/19-mockup-consistency-and-workflow.md`. Die Feature-Mockups
verwenden dieselbe Anwendungsshell und Interaktionslogik; wechselnde
Beispieldomänen sind lediglich fachliche Szenarien.

## M1: Bestehende UI-Baseline und Komponentenraster

**Status:** `READY_FOR_REVIEW`  
**Ergebnis:** `04-ui-ux-analysis/07-m1-ui-baseline.md` und
`assets/mockups/workspace-ui-baseline.html`

**Überarbeitung:** Die M1-Artefakte bilden den geführten Kernworkflow, die
progressive Offenlegung fortgeschrittener Werkzeuge, gestufte Hilfe sowie
messbare Regeln für Kontrast, Lesbarkeit, Bedienflächen und Tastaturfokus ab.

### Ziel

Die neuen Mockups verwenden dieselben Abmessungen und Interaktionsmuster wie die
bereits implementierte Anwendung.

### Zu erstellen

- annotierte Baseline des Class Diagram Workspace,
- annotierte Baseline des Object Diagram Workspace,
- annotierte Baseline des OCL Editors,
- Raster für Top Bar, Tabs, Canvas, Properties Panel und Bottom Panel,
- Liste wiederverwendbarer Formularfelder, Segmented Controls, Tabellen,
  Dialoge und Diagnostics.

### Referenzen

`01`, `02`, `03`, `06`, `12`, `13`, `15`, `16` und `17` aus
`assets/screenshots/`.

### Akzeptanz

Neue Mockups ändern nicht unbemerkt Navigation, Panelbreiten oder den Workflow
Dashboard -> Class Diagram -> Object Diagram -> Check Constraints.

## M2: Generalisierung und abstrakte Klassen

**Status:** `READY_FOR_REVIEW`  
**Ergebnis:** `04-ui-ux-analysis/08-m2-generalization-and-abstract-classes.md`
und `assets/mockups/class-properties-generalizations-and-redefinitions.html`

**Offizielle konsolidierte Mockups:**
`assets/mockups/class-properties-generalizations.html`,
`assets/mockups/class-properties-generalizations-add-supertype.html`

**Überarbeitung:** Der M2-Kernweg verwendet die verständliche Hauptaktion
`Vererbung hinzufügen`, erklärt `Generalisierung` als UML-Fachbegriff, legt
Mehrfachvererbung progressiv offen und übernimmt die messbaren Accessibility-,
Fokus- und Hilferegeln aus M1. Generalisierung bleibt ein Unterbereich von
`Class`; ein zusätzlicher Properties-Haupttab wurde entfernt.

### Zustände

1. Class Diagram mit einer Superklasse und zwei Unterklassen.
2. Selektierte Generalization Edge.
3. Class Properties mit `Abstract class` unter `Details` und
   Supertype-Auswahl unter `Generalizations`.
4. Mehrfachvererbung mit mehreren Supertypes.
5. Fehlerzustand für Generalisierungszyklus oder unzulässigen Konflikt.

### Notation und Interaktion

- Generalization Edge mit hohlem Dreieck an der Superklasse.
- Abstrakte Klasse und gegebenenfalls abstrakte Operationen werden in
  UML-üblicher Form kenntlich gemacht.
- Edge-Selektion öffnet Generalization Properties, ohne Association Properties
  zu imitieren.
- Der Create-Flow unterstützt die Supertype-Auswahl. Bestehende Beziehungen
  werden bestätigt entfernt und bei Bedarf neu angelegt; ein direkter
  Endpunktwechsel bleibt eine spätere Erweiterung.

### Akzeptanz

Richtung der Vererbung, abstrakter Status, Mehrfachvererbung und Fehlermeldungen
sind ohne Kenntnis interner IDs verständlich.

## M3: Sichtbarkeit, Namespaces und Imports

**Status:** `READY_FOR_REVIEW`  
**Ergebnis:** `04-ui-ux-analysis/09-m3-visibility-namespaces-imports.md` und
die offiziellen Vollansichten `assets/mockups/class-properties-details.html`,
`assets/mockups/class-properties-attributes.html`,
`assets/mockups/class-properties-operations.html` und
`assets/mockups/project-imports.html`.

**Überarbeitung:** Der M3-Kernweg priorisiert ausgeschriebene Sichtbarkeit,
ordnet Namespaces und Imports als fortgeschrittene Modellstruktur ein, zeigt
Quick Help zu UML-Symbolen und trennt nutzerfreundliche Importfehler von
technischen Details. Die messbaren M1-Regeln sind übernommen.

### Zustände

- Attribute und Operationen mit `public`, `protected`, `private` und Package
  Visibility,
- Class Properties mit Visibility-Auswahl,
- Explorer mit Package-/Namespace-Gruppierung,
- Importdialog mit Datei, Alias beziehungsweise Namespace und Konfliktdiagnose,
- OCL Editor mit qualifiziertem Namen und Source-Location-Fehler.

### Akzeptanz

Visibility wird kompakt mit UML-Symbolen dargestellt, im Properties Panel aber
als zugängliches Auswahlfeld ausgeschrieben. Importfehler nennen Quelle und
Konflikt, ohne den Workspace zu verlassen.

## M4: Erweiterte Association-End-Properties

**Status:** `READY_FOR_REVIEW`  
**Ergebnis:** `04-ui-ux-analysis/11-m4-association-end-properties.md` und
`assets/mockups/association-properties.html` als konsolidierte visuelle
Referenz für den Association-Properties-Workflow aus M4 bis M6

**Tatsächliches Ergebnis:** M4 zeigt beide Association Ends getrennt, priorisiert
Rollen und Multiplizitäten und legt zusätzliche End-Metadaten progressiv offen.
Die Collection-Art aus `ordered` und `unique`, ID-basierte Referenzen für
`subsets`/`redefines`, strukturierte Fehler, responsive Drawer und stabile
Edge-Labels sind dokumentiert. Aggregation bleibt ein deaktivierter M6-Hinweis.

### Zustände

Das bestehende Association Properties Panel wird für jedes End um folgende
Felder erweitert:

- Rollenname und Multiplizität,
- `ordered`,
- `unique`,
- `navigable`,
- `derived`,
- `union`,
- `subsets`,
- `redefines`,
- Aggregation Kind zunächst nur als vorbereiteter Slot für M6.

### Interaktion

- Source- und Target-End bleiben getrennt erkennbar.
- `subsets` und `redefines` verwenden referenzierte Enden statt Freitext.
- inkompatible Kombinationen werden inline erklärt.
- Association Name bleibt in der Edge-Mitte; Rollen und Multiplizitäten bleiben
  an den jeweiligen Enden befestigt.

### Akzeptanz

Lange End-Metadaten sind scrollbar, überdecken den Canvas nicht und verändern
die Edge-Labelpositionen beim Verschieben von Klassen nicht unkontrolliert.

## M5: Qualifier und n-äre Associations

**Status:** `READY_FOR_REVIEW`  
**Ergebnisdokument:** `04-ui-ux-analysis/12-m5-qualified-and-nary-associations.md`  
**Mockup:** `assets/mockups/association-properties-qualified-nary.html`

**Tatsächliches Ergebnis:** M5 ersetzt die feste Source-/Target-Struktur durch
eine dynamische, endbasierte Association-Liste, lässt den binären Zwei-End-
Arbeitsablauf aber kompakt. Qualifierdefinitionen am UML-End und konkrete
Qualifierwerte am Object Link sind getrennt dargestellt. Ein zentraler
Association-Knoten, ein n-ärer Object-Link-Dialog, OCL-/Validation-Details,
Impact Confirmation und ein responsiver Properties Drawer sind festgelegt.
Alle neuen Association-Felder bleiben innerhalb der bestehenden Hauptseite
`Association`.
Die visuelle Nachprüfung vom 24. August 2026 gleicht M5 vollständig an die
M4-Shell an und behebt Properties-Überlauf, abgeschnittene Tabs, getrennte
Diagrammlinien, abweichende Headeraktionen und gemischte UI-Sprache.
`CM-UML-009`, `CM-UML-010`, `CM-OCL-013`, `CM-UML-007` und `CM-UML-012` sind
zugeordnet. Association Classes, Aggregation und Composition bleiben M6.

### Qualifier-Mockups

- Association End mit Qualifier-Fach am Zielknoten,
- Qualifier-Liste mit Name, Typ und Reihenfolge,
- Object-Link-Dialog mit konkreten Qualifierwerten,
- Navigationsergebnis im OCL Editor beziehungsweise Validation Detail.

### n-äre Association-Mockups

- Class Diagram mit zentralem Association-Knoten und mindestens drei Ends,
- Add Association Modal mit dynamischer End-Liste,
- Properties Panel pro End,
- Object Diagram mit n-ärem Link und beteiligten Objekten,
- Löschen eines Ends mit Auswirkungswarnung.

### Akzeptanz

Die UI erzwingt nicht länger Source/Target für n-äre Fälle. Binäre Associations
bleiben weiterhin schnell und kompakt erstellbar.

## M6: Association Classes und Linkobjekte

**Status:** `READY_FOR_REVIEW`  
**Ergebnisdokument:** `04-ui-ux-analysis/13-m6-association-classes-aggregation-composition.md`  
**Mockup:** `assets/mockups/association-class-properties.html`

**Führende Association-Properties-Ansicht:**
`04-ui-ux-analysis/21-association-properties.md` und
`assets/mockups/association-properties.html` verbinden Association-Stammdaten,
Ends, Qualifier, Aggregation Kind und den Einstieg zur Association Class in
einem konsistenten Workspace. Die frühere M6.5-Zwischenansicht wurde vollständig
in diese offizielle Dokumentationsansicht konsolidiert und entfernt.

**Tatsächliches Ergebnis:** M6 zeigt eine Association Class als bekannten
Class Node mit zusätzlicher Kennzeichnung und gestrichelter Zuordnung zur
Association. Im Object Diagram sind Link, Linkobjekt, eigene Slots und
gekoppelte Selektion entworfen. Create-, Namens-, Zuordnungs- und
Linkobject-Identitätszustände sowie ein responsiver Drawer sind dokumentiert.
Die Properties-Hauptseiten bleiben `Class`, `Association` und `Invariant`;
Association-Class-Features werden je nach selektiertem Modellteil in `Class`
oder `Association` eingebettet.
Die überarbeitete Fassung verortet `Association Class hinzufügen` ausdrücklich
im Empty-Zustand der ausgewählten Association. Aggregation und Composition
werden nicht mehr in M6 wiederholt; ihre End-Eigenschaften und Validierungen
sind im Unified Association Workspace dokumentiert. `CM-UML-011`,
`CM-OCL-013` und `CM-UML-007` sind M6 zugeordnet. Operation Invocation und
Vor-/Nachzustände bleiben M7/M8.

### Association Class

- Empty-Zustand der ausgewählten Association mit `Association Class
  hinzufügen`,
- Class Diagram mit gestrichelter Verbindung zwischen Association und
  Association-Class-Knoten,
- Class Properties für Attribute und Operationen der Association Class,
- Object Diagram mit Linkobjekt und eigenen Slotwerten,
- gemeinsamer Selection Flow zwischen Link, Linkobjekt und Klasse.

### Aggregation und Composition: ausgelagert

Diese Inhalte werden ausschließlich in
`21-association-properties.md` und `association-properties.html` gepflegt. Sie
sind kein
Bestandteil des M6-Mockups mehr.

### Akzeptanz

Association Class und normale Klasse sind visuell unterscheidbar, verwenden aber
dieselben Attribute-/Operationseditoren. Link, Linkobjekt und Association Class
bleiben fachlich gekoppelt auswählbar.

## M7: Operation Signatures und Invocation

**Status:** `READY_FOR_REVIEW`  
**Ergebnisdokument:** `04-ui-ux-analysis/14-m7-operation-signatures-invocation.md`  
**Verbindliche Mockups:**
`assets/mockups/class-properties-operations.html` für die Signaturbearbeitung
und `assets/mockups/object-diagram-operation-invocation.html` für die Runtime
Invocation. Die Signatur bleibt unter `Class`; Receiver-Auswahl, Argumente,
Ausführung und Ergebnis liegen im `Object Diagram`. Das ergänzende
Invocation-Dokument ist
`04-ui-ux-analysis/22-m7-object-diagram-invocation.md`.

**Tatsächliches Ergebnis:** M7 trennt `MODELL · Signatur bearbeiten` sichtbar
von `LAUFZEIT · Operation ausführen`. Operationsproperties enthalten Sichtbarkeit,
Abstract, Query, Rückgabetyp und geordnete Parameter mit Direction. Der
Operationsbereich im Object Diagram zeigt Receiver und typisierte Argumente;
Loading, Success, Result, Validation, Empty, Disabled, Confirmation und
vollständiger Rollback sind festgelegt. Eingabe und Ergebnis benötigen kein
zusätzliches Modal. Nur eine erforderliche Auswirkungsbestätigung verwendet
einen fokussierten Dialog. Console und Result verwenden fachliche Namen. Die vorläufigen
Verträge für Signatur, Dispatch, atomare Ausführung, Out Values und Revision
sind dokumentiert. Signatur und Operation Details liegen als Unterbereich der
bestehenden Hauptseite `Class`. `CM-UML-015`, `CM-UML-016`, `CM-OCL-011` und die
partielle Lebenszyklusabhängigkeit `CM-UML-017` sind zugeordnet.
Pre-/Postconditions und Operation Bodies bleiben M8/M9.

Die visuelle Korrektur vom 24. August 2026 gleicht M7 an die gemeinsame
Mockup-Shell an. Echte `Class`-/`Association`-/`Invariant`-Segmente ersetzen die
CSS-Pseudotabs; Parameterfelder bleiben innerhalb des intern scrollbaren
Properties Panels. Desktop und schmaler Viewport besitzen keinen horizontalen
Überlauf.

### Zustände

- Operation Properties mit Visibility, Abstract, Query, Parametern und
  Rückgabetyp,
- Parametereditor mit Reihenfolge, Name, Typ und Direction,
- Object-Properties-Operationsbereich mit Receiver und typisierten Argumenten,
- Loading-, Success-, Result-, Validation- und Rollback-Zustand,
- Bestätigungsdialog ausschließlich für weitreichende Auswirkungen,
- Console-Eintrag mit Operation, Receiver und Ergebnis ohne interne IDs im
  Vordergrund.

### Akzeptanz

Der Benutzer erkennt klar den Unterschied zwischen dem Bearbeiten einer
Operationssignatur und dem Ausführen einer Operation.

## M8: Pre-/Postcondition und Vor-/Nachzustand

**Status:** `READY_FOR_REVIEW`  
**Ergebnisdokument:** `04-ui-ux-analysis/15-m8-pre-post-before-after.md`  
**Verbindliches Mockup:**
`assets/mockups/operation-contracts.html`. Es übernimmt die
M2-bis-M7-Class-Properties-Hierarchie und hält Pre-/Post-Ergebnisse im
Object-Diagram-Kontext statt in regulären Ergebnis-Modals. Ausführliche
Runtime-Ergebnisse erscheinen im unteren Workspace-Tab `Invocation Results`,
getrennt von `Console` und den invariantenorientierten `Validation Results`.

**Tatsächliches Ergebnis:** M8 ergänzt innerhalb der bestehenden Properties-
Hauptseite `Class` die Operation Details um getrennte Precondition- und
Postcondition-Untersegmente mit Name, Enabled, OCL-Ausdruck,
Kontext und Source Diagnostic. Eine verletzte Precondition blockiert die
Ausführung ohne After State. Postconditions prüfen einen isolierten Candidate
After State gegen den immutable Before State; Verletzungen zeigen Delta und
vollständigen Rollback. `result`, `@pre`, `oclIsNew()`, geänderte, neue und
gelöschte Objekte sowie Desktop- und responsive Zustände sind dokumentiert.
`CM-CTX-002`, `CM-CTX-003`, `CM-OCL-025`, `CM-UML-015` bis `CM-UML-017` sind
zugeordnet. Operation Body, Derived, Init und Def bleiben M9.

### Zustände

- bestehende Properties-Hauptseiten `Class`, `Association`, `Invariant`,
- Operation Details innerhalb von `Class` mit Untersegmenten `Signature`,
  `Preconditions`, `Postconditions` und `Body`,
- Constraint Editor mit Name, OCL-Ausdruck, Enabled und Diagnostic,
- Invocation Dialog mit fehlgeschlagener Precondition,
- Side-by-side oder umschaltbare Before-/After-Snapshot-Ansicht,
- hervorgehobene geänderte, neue und gelöschte Objekte,
- Postcondition Result mit `@pre`, `result` und `oclIsNew()`-Bezug.

### Akzeptanz

Vor- und Nachzustand werden nicht als zwei unabhängige Projekte dargestellt.
Fehler nennen Operation, Constraint und betroffene fachliche Namen.

## M9: Derived, Init, Body und Def

**Status:** `READY_FOR_REVIEW`  
**Ergebnisdokument:** `04-ui-ux-analysis/16-m9-derived-init-body-def.md`  
**Verbindliche Mockups:**

- `assets/mockups/class-properties-attributes.html`
- `assets/mockups/class-properties-operation-body.html`
- `assets/mockups/class-properties-definitions.html`
- `assets/mockups/package-properties-definitions.html`

Sie führen Shell, Explorer und Class-/Package-Properties-Hierarchie aus M2 bis
M8 fort. Das frühere M9-Gesamtmockup wurde nach der vollständigen Übernahme
seiner fachlichen Inhalte entfernt.

**Operation-Body-Mockup:**
`assets/mockups/class-properties-operation-body.html`. Es zeigt den Body als
Untertab der ausgewählten Query-Operation im Stil der einheitlichen Class
Properties sowie Typprüfung, Diagnostics und relevante Bearbeitungszustände.

**Attribute-Value-Source-Mockup:**
`assets/mockups/class-properties-attributes.html`. Es integriert `Stored`,
`Init` und `Derived` einschließlich bedingter Ausdrucksfelder, `/`-Notation
und readonly Derived-Werten in den regulären Attribute-Workflow.

**Package-Definitions-Mockup:**
`assets/mockups/package-properties-definitions.html`. Es zeigt den Einstieg über
einen Package-Knoten im Explorer, Package Properties mit Property- und
Operation-Definitionen, den aktiven Parametereditor und Package-Scope ohne
implizites klassenbezogenes `self`.

**Class-Definitions-Mockup:**
`assets/mockups/class-properties-definitions.html`. Es führt `Definitions` als
fünften Class-Properties-Tab neben `Details`, `Attributes`, `Operations` und
`Generalizations` ein. Class-Definitionen werden dort mit `self`, Rückgabetyp,
geordneten Operationsparametern und OCL-Ausdruck verwaltet; `Details` enthält
keinen doppelten Definitionseditor mehr.

**Tatsächliches Ergebnis:** M9 integriert `Stored`, `Init` und `Derived` in die
Attribute Details der vorhandenen Properties-Hauptseite `Class`. Derived-
Attribute tragen UML-konform `/`, erscheinen im Object Diagram als
`Calculated` und sind dort readonly. Der OCL-Body ergänzt die in M7/M8
festgelegten Operation Details als Untersegment `Body`. Property- und
Operation-`def` werden in einem erweiterten Class-/Package-Definitionsbereich
verwaltet und bleiben von Invarianten getrennt. Desktop-, Typfehler-, Zyklus-,
Loading-, Disabled-, Empty-, Bestätigungs-, Success- und responsive Zustände
sind dokumentiert. `CM-CTX-004` bis `CM-CTX-008`, `CM-UML-001` und
`CM-OCL-002` sind zugeordnet. Package-/Namespace-Auflösung ist backendseitig
verifiziert; eigenständig persistierte und packageweit sichtbare
`def`-Definitionen bleiben unter `CM-CTX-007` eine spätere Backendanforderung.

### Zustände

- Attribute Properties mit Value Source `Stored`, `Init` oder `Derived`,
- OCL Expression Editor für Init und Derive,
- Operation Body Editor,
- Class-/Package-Definitionen für Property- und Operation-`def`,
- Read-only-Anzeige eines berechneten Derived-Werts im Object Diagram,
- Rekursions- und Typfehler mit Source Range.

### Akzeptanz

Gespeicherte und berechnete Werte sind eindeutig unterscheidbar. Derived-Werte
können nicht wie normale Slots überschrieben werden.

## M10: Enum-, DataType- und Type-Auswahl

**Status:** `READY_FOR_REVIEW`  
**Ergebnisdokument:** `04-ui-ux-analysis/17-m10-enum-datatype-type-picker.md`  
**Verbindliche Mockups:**

- `assets/mockups/classifier-type-picker.html`
- `assets/mockups/datatype-properties.html`

Das Unified-Mockup legt Enumeration, Erstellung, Type Picker und typisierte
Objektwerte fest. Das DataType-Properties-Mockup ergänzt den vollständigen
Details- und Value-Properties-Workflow. Die historische Ausgangsfassung wurde
nach vollständiger Übernahme entfernt.

**Tatsächliches Ergebnis:** M10 ergänzt im Class Diagram getrennte Explorer-
Gruppen und UML-konforme Canvas-Darstellungen für Classes, Enumerations und
DataTypes. Create-/Edit-Zustände für geordnete Enum-Literale und DataType-Value-
Properties sowie ein gemeinsamer, suchbarer und kontextbezogener Type Picker
sind festgelegt. Kurze Namen bleiben Standard; qualifizierte Namen erscheinen
bei Mehrdeutigkeit. Nicht verfügbare Typen bleiben mit Begründung sichtbar.
Enum- und strukturierte DataType-Werte sind für das Object Diagram entworfen.
Error-, Loading-, Disabled-, Empty-, Confirmation-, Success- und responsive
Zustände sind dokumentiert. `CM-UML-005`, `CM-UML-006`, `CM-OCL-017`,
`CM-CTX-008`, `CM-UML-001` und `CM-OCL-002` sind zugeordnet.
Die konsolidierte Fassung verwendet dieselbe Explorer- und Workspace-Struktur
wie M2 bis M9. Bei Auswahl einer Enumeration ersetzt `Enumeration Properties`
das Klassenpanel vollständig und bietet nur `Details` und `Literals`. Eine
Enumeration kann außerdem direkt aus dem Type Picker erstellt werden; danach
wird der Attributentwurf wiederhergestellt und der neue Typ vorausgewählt.

### Zustände

- Create Enum Dialog und Enum Properties mit geordneter Literalliste,
- Create DataType Dialog,
- Explorer-Gruppen für Classes, Enums und DataTypes,
- gemeinsamer Type Picker mit Suche und Kategorien,
- Objektwerteditor für Enum-Literale und benutzerdefinierte Value Types.

### Akzeptanz

Type Picker zeigt qualifizierte Namen nur bei Mehrdeutigkeit vollständig und
verhindert nicht auswählbare Typen mit verständlicher Begründung.

## M10.5: DataType Properties

**Status:** `READY_FOR_REVIEW`  
**Ergebnisdokument:** `04-ui-ux-analysis/20-m10-5-datatype-properties.md`  
**Mockup:** `assets/mockups/datatype-properties.html`

**Tatsächliches Ergebnis:** M10.5 ergänzt M10 um den vollständigen
Bearbeitungszustand eines ausgewählten DataTypes. `DataType Properties`
ersetzt das rechte Panel und trennt `Details` von `Value Properties`.
Type-Picker-Nutzung, Referenzprüfung, Speichern sowie Empty-, Loading-, Error-,
Success- und Confirmation-Zustände sind festgelegt. Es wurden keine späteren
Mockup-Schritte oder produktiven Features vorgezogen.

## M11: OCL Compliance und Featureanzeige

**Status:** `READY_FOR_REVIEW`  
**Ergebnisdokument:** `04-ui-ux-analysis/18-m11-ocl-compliance-feature-display.md`  
**Mockup:** `assets/mockups/ocl-compliance-feature-display.html`

**Tatsächliches Ergebnis:** M11 ergänzt den vorhandenen OCL Editor um eine
kompakte, ehrliche Anzeige `OCL 2.4 · Subset`. Ein intern scrollbarer
Detaildialog zeigt Profil-ID, OCL-/API-Version, Compliance Claim, Runtime-
Limits, Suche und die klar getrennten Status `SUPPORTED`, `PARTIAL`,
`NOT_SUPPORTED` und `OUT_OF_SCOPE`. `PARTIAL` nennt verfügbaren Umfang und
Grenzen, ohne den Editor pauschal zu blockieren. Nicht unterstützte Syntax
erhält eine lokale Source-Diagnostic mit Featurebezug. Loading-, Offline-,
Schema-/Empty-, Retry- und responsive Zustände sind dokumentiert. Die Anzeige
projiziert `OCL-PROFILE-001` bis `OCL-PROFILE-014` und verweist auf die
feingranulare Compliance-Matrix, ohne eine zweite Statuswahrheit zu erzeugen.

### Zustände

- kompakte Feature-/Compatibility-Anzeige im OCL Editor,
- Detaildialog für `SUPPORTED`, `PARTIAL`, `NOT_SUPPORTED` und `OUT_OF_SCOPE`,
- Hinweis bei Verwendung eines nicht unterstützten Features,
- Profilversion und OCL-Version in einem About-/Engine-Detailbereich,
- Offline-/Fehlerzustand für `GET /api/v1/ocl/profile`.

### Akzeptanz

Die Anzeige behauptet nie vollständige OCL-Compliance. `PARTIAL` wird nicht wie
`SUPPORTED` dargestellt und blockiert den Editor nicht pauschal.

## M12: State Machine und `oclInState` (optional)

**Status:** `NOT_REQUIRED` im aktuellen Zielprofil (B21).

Dieser Mockup-Schritt wird nur ausgeführt, wenn State Machines in das Zielprofil
aufgenommen werden.

### Zustände

- eigener State-Machine-Tab oder fachlich begründete Unteransicht,
- States, Initial State und Transitions,
- Transition Properties mit Guard und Effect,
- aktiver Zustand eines Objekts im Object Diagram,
- OCL Editor mit `oclInState` und zugehöriger Diagnostic.

### Akzeptanz

State Machines werden nicht als zusätzliche Association-Variante dargestellt.

## M13: Operation Trace und `OclMessage` (optional)

**Status:** `NOT_REQUIRED` im aktuellen Zielprofil (B21).

Dieser Schritt setzt ein Operation-Trace-Domänenmodell voraus.

### Zustände

- zeitlich geordnete Operation Calls,
- Caller, Receiver, Operation, Argumente und Result,
- Message Expression im OCL Editor,
- gefilterte Validation Results für `OclMessage`, `^` und `^^`,
- große Trace-Mengen und leere Trace-Ansicht.

### Akzeptanz

Trace Details sind scrollbar, zeitlich eindeutig und zeigen fachliche Namen
statt primär technischer IDs.

## M14: Responsive, Accessibility und visuelle Abnahme

**Abnahmestatus:** `IMPLEMENTED` durch F12 am 1. September 2026 fuer den
verbindlich abgegrenzten Desktop-Viewport. Schmale Viewports sind gemaess
Frontendplan nicht Teil dieser Abnahme. Fokus, Tastatursteuerung, Quick Help,
lange Fachnamen, interne Scrollbereiche und Ueberlaufsicherheit sind mit
Komponententests und realem Desktop-Playwright nachgewiesen.

### Prüfungen

- Desktop-Viewport; schmale Viewports sind fuer F12 `NOT_APPLICABLE`,
- Properties Panel intern scrollbar,
- Dialoge ohne abgeschnittene Felder,
- Tastaturreihenfolge und Fokusmanagement,
- Labels für Icon Buttons und UML-Symbole,
- Kontrast für Selection, Error, Warning und Disabled,
- lange Klassen-, Rollen-, Operations- und Constraintnamen,
- keine Überlappung von Nodes, Edge Labels, Badges oder Bottom Panel.
- Nachweis der Kriterien `RD-NAV-*`, `RD-VIS-*` und `RD-A11Y-*` aus den
  verbindlichen Redesign-Prinzipien.
- Kontextbezogene Hilfe ist in jeder Hauptview per Tastatur und Pointer
  erreichbar.
- Fortgeschrittene Funktionen bleiben erreichbar, überladen aber den
  Einsteiger-Kernweg nicht.

### Abnahme

Jedes Mockup-Paket erhält eine Zustandsliste und eine Screenshot-ID. Erst nach
fachlicher Freigabe werden API-/DTO- und Frontend-Implementierungsschritte
finalisiert.

## Keine neuen Mockups erforderlich

| Technischer Schritt | Begründung |
|---|---|
| Normatives Inventar und Reference-Zuordnung | reine Analyse-/Testmetadaten |
| Compliance-Test-Harness | keine Benutzerinteraktion |
| `null`-/`invalid`- und Vierwertlogik | bestehende Diagnostics und Validation Results reichen |
| numerische und String-Bibliothek | bestehender OCL Editor reicht |
| Collection- und Iteratorsemantik | bestehender OCL Editor reicht |
| Call Resolution intern | keine neue Interaktion, solange Signaturen unverändert bleiben |
| Parser-/Evaluator-Budgets | bestehende Diagnostics; nur Meldungstexte prüfen |
| Format-/Infrastructure-Gaps | Reference-Harness, nicht Produkt-UI |
| Regression Promotion | Testprozess |

## Empfohlene Mockup-Pakete

| Paket | Mockup-Schritte | Freigabe vor Implementierungsschritten |
|---|---|---|
| A: UML Type System | M1-M3, M10 | Backend B4-B5/B13; Frontend F1-F3/F10 |
| B: Associations | M4-M6 | Backend B6-B8; Frontend F4-F6 |
| C: Operation Runtime | M7-M9 | Backend B9/B18-B19; Frontend F7-F9 |
| D: Compliance UI | M11 | Backend B20/B25; Frontend F11 |
| E: Optional OCL | M12-M13 `NOT_REQUIRED` | B21 abgeschlossen; kein Frontendschritt vorgesehen |
| Q: Quality Gate | M14 | Frontend F12 und gemeinsame Abnahme |

Die Pakete strukturieren die Mockup-Arbeit, erlauben aber keinen vorzeitigen
Beginn einzelner Backend-Pakete. Das Backend-Stage-Gate ist erst erfüllt, wenn
die Pakete A bis D und Q freigegeben sind und für Paket E eine dokumentierte
Include-/Exclude-Entscheidung vorliegt.

## Stage-Gate vor der Backend-Umsetzung

Vor dem ersten Backend-Feature-Schritt müssen folgende Ergebnisse vorliegen:

- freigegebene Desktop-Mockups für alle verpflichtenden Schritte M1 bis M11,
- responsive und barrierebezogene Prüfung gemäß M14,
- dokumentierte Entscheidung zu den optionalen Schritten M12 und M13,
- eindeutige Zuordnung der sichtbaren Felder zu fachlichen Domänenbegriffen,
- dokumentierte Create-, Edit-, Delete-, Error- und Confirmation-Flows,
- abgestimmte UML-Notation für Knoten, Kanten, Enden und Labels,
- vorläufige DTO- und API-Anforderungen als Annotationen, jedoch noch keine
  Backend-Implementierung,
- Freigabevermerk mit Datum, Status und offenen Abweichungen je Mockup-Paket.

Erst wenn diese Liste erfüllt ist, wechselt die Planung von `MOCKUP` nach
`BACKEND_READY`.

## Ergebnisartefakte

Für jeden Mockup-Schritt entstehen mindestens:

1. Desktop-Mockup des Hauptzustands,
2. relevante Edit-, Error- und Confirmation-Zustände,
3. Annotationen für fachliche Felder und Interaktionen,
4. Zuordnung zu DTOs und Backendfähigkeiten,
5. Zuordnung zu Matrix-IDs aus
   `14-full-ocl-uml-compliance-matrix.md`,
6. Akzeptanzkriterien für spätere Playwright-Screenshots.

## Zusammenfassung

Die wichtigsten neuen Mockups sind Generalisierung, erweiterte Association-Enden,
Operation Invocation, Pre-/Postconditions und Vor-/Nachsnapshots. Die
Mockup-Phase wird vollständig abgeschlossen, bevor neue Backend-Features
umgesetzt werden. State Machine und Operation Trace müssen zuvor entweder
entworfen oder ausdrücklich aus dem Zielprofil ausgeschlossen werden. Reine
OCL-Engine-Semantik wird in den bestehenden OCL Editor und die bestehenden
Validation Results integriert und benötigt keine zusätzliche Hauptansicht.
