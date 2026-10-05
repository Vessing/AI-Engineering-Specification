# Mockup Consistency and End-to-End Workflow

## Zweck und Status

**Status:** `CONSISTENCY_REVIEWED`  
**Geltungsbereich:** M1 bis M11

Diese Datei definiert den gemeinsamen Rahmen aller bisher erstellten Mockups.
Die einzelnen Mockups zeigen unterschiedliche fachliche Beispielszenarien, aber
dieselbe Anwendung, Navigation und Bedienlogik. Sie sind deshalb keine elf
unabhängigen Oberflächen und auch keine lineare Modellmigration desselben
Beispielprojekts.

## Ergebnis der Konsistenzprüfung

| Prüfbereich | Vereinheitlichte Festlegung |
|---|---|
| Anwendungsshell | Logo, fachlicher Projektname, `Class Diagram`, `Object Diagram`, `OCL Editor`, `Refresh`, `Check Constraints` |
| Logo | Nur das vorhandene Logo; der zusätzliche Text `USE` erscheint nicht im Workspace-Header. Ein Klick führt zum Dashboard. |
| Projektidentität | Der sichtbare Projektname steht neben dem Logo. Interne Projekt-IDs erscheinen höchstens als Tooltip oder technische Detailinformation. |
| Hauptworkflow | Dashboard -> Start Project -> Class Diagram -> Object Diagram -> Check Constraints |
| OCL Editor | Ergänzendes Werkzeug im Haupttab; Vollbreite ohne Explorer und Properties Sidebar |
| Object Diagram | `assets/mockups/workspace-object-explorer.html` definiert die kanonische Shell mit objektbezogenem Explorer und Object Properties. `object-diagram-workspace.html` bleibt das detaillierte Auswahl- und Linkbeispiel. |
| Properties | Im Class Diagram bleiben die Hauptseiten `Class`, `Association` und `Invariant` stabil. |
| Operationen | `Signature`, `Preconditions`, `Postconditions` und `Body` sind Untersegmente von `Class`, keine globalen Tabs. |
| Attribute | `Stored`, `Init` und `Derived` sind Untersegmente der Attribute Details in `Class`. |
| Associations | Ends, Qualifier, n-äre Enden, Association Class und Aggregation Kind liegen in `Association`. |
| Invarianten | OCL-Invarianten liegen in `Invariant`; `pre`, `post`, `body`, `derive`, `init` und `def` werden dort nicht vermischt. |
| Class Explorer | Das Referenzmockup `assets/mockups/workspace-model-explorer.html` verwendet immer einen `Project root` und je Import einen read-only `Import root`. Darunter liegen Packages sowie Classes, Enumerations und DataTypes; Classifier ohne Package liegen direkt unter ihrer Root. |
| Object Explorer | Das Referenzmockup `assets/mockups/workspace-object-explorer.html` gliedert den aktuellen Objektzustand ausschließlich in `Objects` und `Object Links`. Model Packages und Classifier werden dort nicht dupliziert. |
| Class-Diagram-Aktionen | Oben im Canvas liegt eine kompakte, farblich und textlich differenzierte Toolbar für `Class`, `Association`, `Invariant`, `Enumeration` und `DataType`; das Plus bei `Project model` ergänzt `Create Package`. |
| Bottom Panel | `assets/mockups/workspace-bottom-panel.html` definiert `Console`, `Diagnostics`, `Validation Results` und `Invocation Results` in derselben intern scrollbaren Fläche. Jeder Tab besitzt eine getrennte fachliche Aufgabe und eine eindeutige Aktivierungsregel. |
| Fachliche Autorität | Das Frontend zeigt Entwurf, Auswahl und Ergebnisse; Backend und API validieren und persistieren fachliche Änderungen. |
| Sprache | Sichtbare Produktaktionen verwenden konsistentes Englisch; deutschsprachige Analyseannotation bleibt außerhalb der simulierten Anwendung. |
| Status | Status wird mit Text und gegebenenfalls Icon/Rahmen gezeigt, nie ausschließlich über Farbe. |

## Kanonische Anwendungsshell

```text
+--------------------------------------------------------------------------+
| Logo | Project: <name> | Class Diagram | Object Diagram | OCL Editor     |
|                                      Refresh | Check Constraints          |
+--------------------------------------------------------------------------+
```

Die Reihenfolge ist in allen Mockups verbindlich. `Refresh` ist eine sekundäre
Aktion. `Check Constraints` bleibt die hervorgehobene globale Aktion. Quick
Help erscheint kontextbezogen am Arbeitsbereich oder im Drawer und ersetzt
nicht wechselweise `Refresh` im Header.

Auf schmalen Viewports dürfen die drei Haupttabs horizontal scrollen. Explorer,
Properties, Hilfe und umfangreiche Ergebnisse öffnen in getrennten Drawern. Ein
Drawer überdeckt nicht unkontrolliert einen anderen, ist intern scrollbar und
gibt den Fokus an seine auslösende Aktion zurück.

## Kanonische Auswahl- und Properties-Logik

| Auswahl | Aktive Properties-Seite | Erwarteter Inhalt |
|---|---|---|
| Class oder Generalization | `Class` | allgemeine Klasse, Generalization, Attribute, Operationen, Definitions |
| Attribute | `Class` | Attribute Details einschließlich Type und Value Source |
| Operation | `Class` | Operation Details mit Signature/Pre/Post/Body |
| Association oder Association End | `Association` | allgemeine Beziehung, Ends, Multiplicity, Qualifier, Aggregation Kind |
| Association Class | `Class` oder `Association` | beide Aspekte sind explizit erreichbar; die Auswahl bleibt dieselbe fachliche Identität |
| Invariant | `Invariant` | Name, Context, Enabled, OCL Expression und Diagnostics |
| Object | Object-Properties `Object` | Classifier, Slots und berechnete Values |
| Object Link | Object-Properties `Association` | Association, Endbelegung und Qualifierwerte |

Explorer, Canvas und Properties synchronisieren immer dieselbe Auswahl. Ein
Wechsel der Properties-Seite löscht die Auswahl nicht. Fachliche Namen stehen
im Vordergrund; IDs dienen nur stabilen Referenzen im Vertrag.

Für M6 erscheint `Association Class hinzufügen` nur auf der Properties-Seite
`Association`, wenn die ausgewählte Association noch keine Association Class
besitzt. `Aggregation kind` ist keine Create-Aktion, sondern eine Eigenschaft
des ausgewählten Association Ends auf derselben Properties-Seite.

## Durchgehender Nutzerworkflow

### 1. Dashboard und Projektstart

Der Benutzer öffnet ein vorhandenes Projekt oder erstellt eines mit einem
Namen. Danach startet die Anwendung im Class Diagram. Das Logo führt jederzeit
zum Dashboard zurück; nicht gespeicherte Entwürfe benötigen davor eine
Bestätigung.

### 2. UML-Struktur im Class Diagram

M2 bis M6 und M10 erweitern denselben Arbeitsschritt:

1. Classes, Enumerations und DataTypes in ihrer Package-Hierarchie im Explorer erstellen oder auswählen.
2. Eigenschaften im rechten Panel bearbeiten.
3. Generalizations und Associations im Canvas anlegen.
4. Erweiterte Association Ends, Qualifier, n-äre Associations und Association
   Classes erst bei Bedarf öffnen.
5. Backenddiagnostik direkt am Feld und zusätzlich im Bottom Panel anzeigen.
6. Nach erfolgreicher Mutation eine sichtbare Bestätigung und neue Revision
   übernehmen.

### 3. Verhalten und OCL-Kontexte

Das offizielle Übersichts-Mockup `assets/mockups/class-properties-overview.html`
verbindet M2, M3 und M7. `Details`, `Attributes`, `Operations` und
`Generalizations` liegen als untergeordnete Bereiche innerhalb von `Class`;
`Association` und `Invariant` bleiben gleichrangige Hauptsegmente.
Für die Einzelprüfung liegen dieselben vier Zustände zusätzlich in
`class-properties-details.html`, `class-properties-attributes.html`,
`class-properties-operations.html` und `class-properties-generalizations.html` vor.
M8 führt diese Hierarchie in `operation-contracts.html` fort:
`Operations` bleibt aktiv und wird für den ausgewählten Vorgang durch
`Signature`, `Preconditions`, `Postconditions` und `Body` vertieft.
Dieselbe Vertiefung ist bereits im Operations-Einzelmockup sichtbar, wobei
`Signature` aktiv ist und `Body` bis M9 deaktiviert bleibt.
M9 führt die Struktur in den verbindlichen Mockups
`class-properties-attributes.html`, `class-properties-operation-body.html`,
`class-properties-definitions.html` und
`package-properties-definitions.html` fort: `Attributes` enthält `Stored`,
`Init` und `Derived`; `Operations` aktiviert `Body`. Property- und
Operation-`def` liegen im fünften Class-Untertab `Definitions` oder unter
`Package Properties > Definitions`. Das Class-Details-Formular enthält keinen
doppelten Definitionseditor.
Das verbindliche Mockup `class-properties-attributes.html` zeigt normale
Attributfelder und alle drei Value Sources in einem durchgängigen
Attributworkflow.

M7 bis M9 bleiben innerhalb der ausgewählten Class:

1. Operationssignatur bearbeiten.
2. Für eine Invocation in das Object Diagram wechseln und dort zuerst ein
   Receiver-Objekt auswählen.
3. Preconditions und Postconditions im Operation-Detailbereich bearbeiten.
4. Body, Derived, Init und `def` in ihren jeweiligen Unterbereichen bearbeiten.
5. Typecheck, Source Diagnostics und Auswirkungen vor dem Anwenden prüfen.

Die Runtime Invocation ist klar als Runtime-Aktion im Object Diagram
gekennzeichnet und verändert nicht still die Operationssignatur. Bei mehreren
kompatiblen Objekten bestimmt ausschließlich die explizite Objektselektion den
Receiver. Argumenteingabe und Loading bleiben im `Operations`-Bereich der
Object Properties beziehungsweise im mobilen Drawer. Eine kompakte
Statusmeldung verweist nach Abschluss auf den automatisch aktivierten unteren
Tab `Invocation Results`; dort liegen Ergebnis, Contractprüfungen, Fehler,
Before/Candidate After und Rollback.
Ein Modal wird nur für eine ausdrücklich erforderliche Bestätigung
weitreichender Objekt- oder Linkänderungen verwendet.

### 4. Snapshot im Object Diagram

Der Benutzer erzeugt Objekte, pflegt Slots und legt Object Links an. Enum-Werte
verwenden eine Literalauswahl, DataType-Werte einen strukturierten Value Editor
und Derived-Werte eine readonly Darstellung. Objekt- und Linkauswahl verwenden
dieselben Eigenschaften- und Scrollregeln wie das Class Diagram.

### 5. OCL Editor

Der OCL Editor öffnet im Haupttab und verwendet die volle Breite. `Apply
Changes` aktualisiert das Modell erst nach Backendprüfung und bestätigt den
tatsächlichen Apply-Status. M11 ergänzt nur die kompakte, lesende
OCL-Subset-Anzeige; die Featureliste wird bei Bedarf in einem Dialog oder Drawer
geöffnet.

### 6. Check Constraints und Ergebnisse

`Check Constraints` prüft die aktuelle persistierte Modell- und
Snapshotrevision. Validation Results nennen Invariante, fachliches Objekt,
Attribut oder Association und vermeiden interne IDs im Vordergrund. Ein Klick
auf ein Ergebnis synchronisiert View, Canvas/Editor und Properties mit der
betroffenen Stelle.

## Gemeinsames Mutationsmuster

```text
Select element
-> Edit draft
-> Local required-field feedback
-> Submit to backend
-> Backend validation
-> Success: replace project state + show confirmation
-> Failure: retain draft + show structured diagnostic
```

`Cancel` verwirft ausschließlich den aktuellen Entwurf. Eine bestätigte
Mutation darf danach nicht durch lokale Fallback-Daten ersetzt werden. Layout-
Positionen können automatisch gespeichert werden; fachliche Create-, Edit- und
Delete-Aktionen bleiben durch ihre benannte Aktion nachvollziehbar und zeigen
einen Erfolgs- oder Fehlerzustand.

## Einheitliche Begriffe und Aktionen

| Verwenden | Nicht parallel verwenden |
|---|---|
| `Class`, `Association`, `Invariant` | `Constraints` als Name derselben Properties-Seite |
| `Check Constraints` | `Validate`, wenn dieselbe globale Aktion gemeint ist |
| `Apply Changes` im OCL Editor | `Save Model Text` für denselben Vorgang |
| `Save attribute/association/operation/...` | generisches `OK` ohne Wirkung |
| `Cancel` | `Close`, wenn ein Entwurf verworfen wird |
| `Close` | `Cancel`, wenn nur eine Ergebnisansicht geschlossen wird |
| `Calculated` und UML `/name` | editierbarer Slot für Derived-Werte |
| `OCL 2.4-based subset` | `OCL compliant` ohne Einschränkung |

## Konsistenzmatrix M1 bis M11

| Schritt | Ort im Workflow | Einstieg | Rückkehr/Weiterführung |
|---|---|---|---|
| M1 | gesamte Shell | Dashboard/Projekt | verbindliche Grundlage aller Schritte |
| M2 | Class Diagram -> Class | Class oder Generalization auswählen | Modell speichern, weiter zu Objects/OCL |
| M3 | Class Diagram -> Class/Explorer | Class, Package oder Import auswählen | qualifizierte Namen in allen Folgeschritten |
| M4 | Class Diagram -> Association | Association/End auswählen | Object Links und OCL Navigation |
| M5 | Class/Object Diagram -> Association | Association oder Link auswählen | qualifizierte/n-äre Navigation |
| M6 | Class/Object Diagram -> Class/Association | Association Class oder Composition auswählen | Linkobjekt und Lifecycle-Auswirkung |
| M7 | Class Diagram -> Class -> Operation; Object Diagram -> Object -> Operations | Signatur modellieren; Receiver auswählen | atomare Runtime Invocation auf genau einem Objekt |
| M8 | Class -> Operation -> Pre/Post | Operation Contract öffnen | Invocation Result und Rollback |
| M9 | Class -> Attribute/Operation/Definitions | Feature auswählen | berechnete Object Values und Calls |
| M10 | Class Diagram -> Model Types | Enum/DataType oder Typfeld auswählen | typisierte Attribute, Parameter und Slots |
| M11 | OCL Editor -> View support | Subset-Anzeige oder Diagnostic öffnen | zurück zur Source Range im Editor |

## Geprüfte Abweichungen und Korrekturen

- Der globale Header verwendet nun in den Feature-Mockups das vorhandene Logo,
  einen sichtbaren Projektnamen, `Refresh` und `Check Constraints`.
- Globale Quick-Help-Buttons wurden aus dem Header entfernt beziehungsweise als
  sekundäre Refresh-Aktion vereinheitlicht; Hilfe bleibt kontextbezogen.
- `Invariant` ist der verbindliche Name der dritten Class-Diagram-Properties-
  Seite. Der ältere sichtbare Begriff `Constraints` wird nicht weitergeführt.
- M7 bis M9 verwenden dieselbe verschachtelte Operation-Detailstruktur.
- M7 verwendet nach der visuellen Korrektur dieselbe Shell, echte
  Properties-Hauptsegmente und dieselben internen Scrollregeln wie die
  übergreifende Association-Dokumentationsansicht. Parameterzeilen laufen nicht
  mehr horizontal aus dem Properties Panel.
- M10 integriert Modelltypen in den bestehenden Class-Diagram-Workflow.
- M10 verwendet ein auswahlabhängiges Properties Panel. Eine Enumeration
  öffnet ausschließlich `Enumeration Properties` mit `Details` und `Literals`;
  die Klassenreiter werden nicht parallel angezeigt.
- `Create Enumeration` im Type Picker erhält den aktuellen Formularkontext,
  kehrt nach dem Erstellen zum Attribut zurück und wählt den neuen Typ aus.
- M10.5 ergänzt denselben Auswahlmechanismus für DataTypes. Die Auswahl eines
  DataTypes öffnet `DataType Properties` mit `Details` und `Value Properties`;
  Class-, Association- und Invariant-Reiter bleiben verborgen.
- M11 respektiert die bereits festgelegte Vollbreite des OCL Editors.
- M5 verwendet nach der visuellen Nachprüfung dieselbe Shell wie M4. Der
  Properties-Bereich läuft nicht mehr horizontal über, die drei
  Association-Linien verbinden Klassen und zentralen Knoten, und alle
  Dialog- und Zustandsbegriffe folgen derselben UI-Sprache.

Die Feature-Mockups verwenden bewusst verschiedene Beispieldomänen wie
University, Library, Banking und Billing, um die jeweilige Funktion lesbar zu
zeigen. Dies ändert nicht Shell oder Workflow und bedeutet nicht, dass beim
Navigieren automatisch zwischen Projekten gewechselt wird.

## Offene Punkte

- M12 und M13 benötigen noch eine Include-/Exclude-Entscheidung und dürfen die
  Hauptnavigation nur bei fachlicher Aufnahme erweitern.
- M14 muss die konsolidierten Mockups gemeinsam visuell, responsiv und
  barrierebezogen abnehmen.
- Vor Frontendumsetzung sollen echte interaktive Tabs in allen Prototypen durch
  `role="tablist"`, `role="tab"` und `aria-selected` spezifiziert werden.
- Die genaue Warnung beim Dashboard-Wechsel mit offenem Formularentwurf muss in
  M14 abschließend geprüft werden.

## Akzeptanzkriterien

| ID | Kriterium | Ergebnis |
|---|---|---|
| `CONS-01` | Alle Feature-Mockups folgen derselben globalen Shell. | erfüllt |
| `CONS-02` | Properties besitzen eine eindeutige Auswahl- und Seitenhierarchie. | erfüllt |
| `CONS-03` | Operation, Association und Invariant werden nicht als konkurrierende globale Views modelliert. | erfüllt |
| `CONS-04` | Der Workflow von Projektstart bis Validation Results ist vollständig beschrieben. | erfüllt |
| `CONS-05` | OCL Editor bleibt vollbreit und M11 progressiv offengelegt. | erfüllt |
| `CONS-06` | Fachliche Mutationen besitzen konsistente Save-, Cancel-, Success- und Error-Semantik. | erfüllt |
| `CONS-07` | Responsive Drawer- und Fokusregeln gelten übergreifend. | erfüllt |
| `CONS-08` | Spätere M12-M14-Funktionen wurden nicht vorweggenommen. | erfüllt |

## Abgrenzung

Diese Prüfung vereinheitlicht ausschließlich die vorhandenen Analyse- und
Mockup-Artefakte M1 bis M11. Sie implementiert keine Frontend- oder
Backendfunktion und setzt den Gesamtstatus nicht auf `BACKEND_READY`.
