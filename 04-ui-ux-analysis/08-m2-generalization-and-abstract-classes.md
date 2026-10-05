# M2 Generalisierung und abstrakte Klassen

## Zweck und Status

Dieses Dokument beschreibt das Ergebnis von Mockup-Roadmap-Schritt M2. Das
Mockup erweitert ausschließlich das Klassendiagramm um die Darstellung und
Bearbeitung von Generalisierungen, abstrakten Klassen und mehreren direkten
Supertypen.

**Status:** `READY_FOR_REVIEW`  
**Gesamtstatus:** `MOCKUP`, noch nicht `BACKEND_READY`.

**Überarbeitungsstand:** M2 wendet die überarbeitete M1-Baseline und die drei
verbindlichen Redesign-Prioritäten nun auch sichtbar auf Generalisierung und
abstrakte Klassen an.

Die konsolidierte offizielle Darstellung liegt unter
`assets/mockups/class-properties-generalizations-and-redefinitions.html` und
ist in `46-unified-generalizations-and-redefinitions.md` beschrieben. Die
ergänzende Detaildarstellung `assets/mockups/class-properties-generalizations.html`
sowie folgende Vollansichten schließen die M2-Sonderfälle:
Vollansichten schließen die M2-Sonderfälle:

- `assets/mockups/class-properties-generalizations-add-supertype.html`
- `assets/mockups/delete-generalization-modal.html`

Das frühere eigenständige M2-Mockup wurde in die konsolidierte Darstellung
überführt und ist keine separate Datei mehr.

`class-properties-generalizations-add-supertype.html` ist ausdrücklich der
Folgezustand nach `Vererbung hinzufügen`. Bereits vorhandene Beziehungen stehen
unter `Current direct supertypes`; erst danach werden auswählbare und begründet
deaktivierte `Superclass candidates` angezeigt.

Das offizielle `class-properties-generalizations-and-redefinitions.html` verbindet den
Normalzustand und die Mehrfachvererbung. Die ausgewählte Klasse legt die
Subclass fest. Klickbare Direct-Supertype-Beziehungen öffnen darunter
`Generalization Details` und `Inherited Features`; `Vererbung hinzufügen`
öffnet die Kandidatenauswahl als Modal.
Die `Inherited Features` innerhalb von
`class-properties-generalizations.html` zeigen die effektiv geerbten Attribute
und Operationen der jeweils ausgewählten Beziehung getrennt. Sie nennen die
definierende Superclass, sind in der Subclass read-only und navigieren zur
besitzenden Klasse. Owned, inherited, redefined und conflicting Features dürfen
dabei nicht gleichgesetzt werden.
Das Mockup zeigt dieses Modal zusätzlich als dauerhaft sichtbaren
Dokumentationsabschnitt unterhalb des interaktiven Workspace, damit Felder,
Kandidatenzustände und Aktionen ohne vorherige Interaktion prüfbar bleiben.

Canvas-Kanten und Einträge unter `Current direct supertypes` steuern dieselbe
Generalization-Auswahl. `Remove Generalization` öffnet einen
referenzbewussten Bestätigungsdialog aus
`assets/mockups/delete-generalization-modal.html`. Er nennt die konkrete
Beziehung, behält beide Klassen sowie andere direkte Supertypen bei und
blockiert das Entfernen bei Redefinitionen oder anderen typabhängigen
Referenzen. Die verbleibende effektive Hierarchie wird nach dem Löschen neu
berechnet. Ein weiterer sichtbarer
Dokumentationsabschnitt zeigt Loading während der atomaren Validierung, eine
Klasse ohne direkte Supertypen und eine Kandidatensuche ohne Treffer.

Das offizielle Mockup enthält außerdem strukturierte sichtbare Diagnosen für
`UNKNOWN_SUPERCLASS`, `GENERALIZATION_CYCLE` und
`INHERITED_FEATURE_CONFLICT`. `SeniorTeachingAssistant` erscheint in der
Kandidatenauswahl deaktiviert, weil die Klasse bereits ein Nachfahre von
`TeachingAssistant` ist und als Superclass einen Zyklus erzeugen würde. Die
Quick Help erklärt das ungefüllte Dreieck, Subclass, Superclass sowie die
Abgrenzung zur Association. Das Feld `Inheritance chain` zeigt die transitive
Kette der ausgewählten Beziehung und folgt der Auswahl aus Canvas oder Liste.

## Verwendete Quellen

### Analyse-Dokumente

- `00-overview/03-documentation-map.md`
- `03-uml-ocl-domain/01-uml-ocl-scope.md`
- `03-uml-ocl-domain/02-domain-model.md`
- `04-ui-ux-analysis/01-ui-overview.md`
- `04-ui-ux-analysis/02-class-diagram-ui.md`
- `04-ui-ux-analysis/05-screenshot-traceability.md`
- `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md`
- `04-ui-ux-analysis/07-m1-ui-baseline.md`
- `04-ui-ux-analysis/10-redesign-design-principles.md`
- `06-frontend-analysis/03-frontend-architecture.md`
- `06-frontend-analysis/07-class-diagram-component.md`
- `06-frontend-analysis/11-properties-panel.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/07-dto-reference.md`
- `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`
- `09-ocl-extension-analysis/15-full-ocl-uml-implementation-plan.md`

### Screenshots

| Screenshot | Übernommenes Muster |
|---|---|
| `01-class-diagram-class-properties.png` | Workspace-Aufteilung und Class Properties |
| `02-class-diagram-association-properties.png` | selektierbare Kante mit eigenem Properties-Kontext |
| `04-class-diagram-new-class-selected.png` | Node-Selektion und blaue Kontur |
| `15-properties-association.png` | segmentiertes Properties Panel |
| `16-properties-invariants.png` | lange, intern scrollbar bleibende Properties |
| `17-new-class.png` | Class-Properties-Formular und Create-/Edit-Muster |

## Desktop-Hauptzustand

Das Class Diagram zeigt eine abstrakte Oberklasse `Person` und die Unterklassen
`Student`, `Employee` und `TeachingAssistant`. Die Generalization Edge endet mit
einem ungefüllten Dreieck an der Oberklasse. Der Pfeil zeigt damit von der
Unterklasse zur Oberklasse.

Abstrakte Klassen werden doppelt kenntlich gemacht:

1. Der Klassenname wird kursiv dargestellt.
2. Über dem Namen steht `«abstract»` als zugängliche Textkennzeichnung.

Farbe allein ist keine Bedeutungsträgerin. Attribute und Operationen behalten
die bestehenden Klassenkompartimente.

## Explorer und Auswahl

Der Class-Diagram-Explorer erhält zusätzlich die Gruppe `Generalizations`.
Ein Eintrag verwendet fachliche Namen im Format `Subclass → Superclass`.

| Auswahl | Canvas | Properties Panel |
|---|---|---|
| abstrakte Klasse | blaue Klassenkontur | Class Properties; `Abstract class` unter `Details`, direkte Oberklassen unter `Generalizations` |
| konkrete Klasse | blaue Klassenkontur | Class Properties mit ausgeschaltetem Abstract-Toggle |
| Generalization Edge | blaue Kante und Dreieckkontur | eigenes Segment `Generalization` |
| Validation Result | betroffene Klasse beziehungsweise Edge | Fehlerdetails mit fachlichen Klassennamen |

Explorer, Canvas, Properties und Validation Results müssen dasselbe Element
selektieren. Eine Generalisierung besitzt deshalb eine eigene stabile Identität
oder eine eindeutig normalisierte Kombination aus Subclass und Superclass.

## Class Properties

Die Hauptnavigation entspricht unverändert den Referenzscreenshots und besteht
aus `Class`, `Association` und `Invariant`. Generalisierung erhält keine vierte
gleichrangige Properties-Seite. `Abstract class` liegt als allgemeines
Klassenmerkmal ausschließlich unter `Class Properties -> Details`.
`Current direct supertypes` und die Bearbeitung von Generalisierungen liegen
unter `Class Properties -> Generalizations`.

Das bestehende segmentierte Panel wird um `Generalization` ergänzt. Im Segment
`Class` erscheinen folgende M2-Felder:

| Feld | Control | Regel |
|---|---|---|
| `Abstract class` | Toggle unter `Details` | aktiviert beziehungsweise deaktiviert den abstrakten Status; nicht im Generalizations-Reiter duplizieren |
| `Direct supertypes` | geordnete visuelle Chip-Liste mit Auswahl | zeigt nur direkte Oberklassen; keine internen IDs |
| `Add supertype` | Command Button | öffnet eine suchbare Klassenauswahl |

Die sichtbare Reihenfolge mehrerer Supertype-Chips definiert keine eigene
Linearisierungs- oder Konfliktregel. Die fachliche Auflösung bleibt im Backend.

## Generalization-Unterbereich in Class Properties

Bei Auswahl einer Generalization Edge zeigt das Panel:

- `Subclass` als fachlichen Klassennamen,
- `Superclass` als fachlichen Klassennamen,
- beide Enden in der ersten Ausbaustufe nur lesbar,
- `Remove generalization` mit Bestätigung,
- gegebenenfalls strukturierte Backend-Diagnostics.

Die Enden einer bestehenden Generalisierung werden in der ersten Ausbaustufe
nicht direkt geändert. Für eine andere Superclass entfernt der Benutzer die
bestehende Beziehung nach Bestätigung und legt über `Vererbung hinzufügen`
eine neue Beziehung an. Beide Mutationen werden jeweils atomar validiert; ein
ungültiger oder halb gespeicherter Zwischenzustand erscheint nicht im Canvas.
Ein späterer direkter Replace-Flow darf ergänzt werden, ist aber keine
Voraussetzung für M2.

## Zustände

| Zustand | Darstellung |
|---|---|
| Default | normale Generalization Edges, abstrakte Klasse ohne Selektion |
| Selected | Kante und Dreieck blau; Generalization Properties geöffnet |
| Editing | vorhandene Beziehung ausgewählt oder Add-Supertype-Modal aktiv; bestehende Enden bleiben read-only |
| Loading | die ausgelöste Add- oder Remove-Aktion ist deaktiviert; bestehende Kante bleibt sichtbar |
| Error: cycle | Diagnose nennt beteiligte Klassen und verhindert das Hinzufügen |
| Error: unknown superclass | fehlender fachlicher Name wird angezeigt |
| Error: conflict | Konflikt nennt Feature und konkurrierende Supertypen, soweit Backend verfügbar |
| Disabled | Selbstreferenz und bereits direkte Supertypen nicht auswählbar |
| Empty | `No direct supertypes` statt leerem unbeschriftetem Bereich |
| Confirmation | Erfolgsmeldung nach Add; Bestätigung vor Remove |

Die Zustände `Selected`, `Loading`, `Empty`, `Error` und `Confirmation` sind im
offiziellen `class-properties-generalizations-and-redefinitions.html` sichtbar beziehungsweise
interaktiv umgesetzt. Das Add-Supertype-Mockup bleibt als fokussierte
Zusatzansicht derselben Kandidaten- und Diagnoseverträge erhalten.

## Mehrfachvererbung und Konflikte

Mehrere direkte Supertypen werden unterstützt. Das Mockup legt keine selbst
erfundene C3- oder Prioritätsreihenfolge fest. Bei mehrdeutiger Attribut- oder
Operationsauflösung zeigt die UI die strukturierte Backend-Diagnose und erlaubt
keine lokale fachliche Entscheidung durch Drag-and-drop-Reihenfolge.

Ein Konflikt wird mit folgenden nutzerorientierten Angaben dargestellt:

- betroffene Subclass,
- konkurrierende Supertypen,
- Name und Art des mehrdeutigen Features,
- mögliche Korrektur, sofern das Backend eine liefert.

## Abstrakte Klassen im Object Diagram

M2 entwirft keine neue Object-Diagram-Ansicht. Als fachliche Folge wird aber
festgelegt: Eine abstrakte Klasse darf im Add-Object-Flow nicht als direkt
instanziierbarer Typ angeboten werden. Wird ein bestehender Typ nachträglich
abstrakt, muss das Backend einen Konflikt mit vorhandenen direkten Instanzen
melden. Die endgültige Objekt-Fehlerdarstellung bleibt im vorhandenen
Validation- und Properties-Muster.

## Responsive Verhalten

Im schmalen Viewport bleibt der Class-Diagram-Canvas sichtbar. Explorer und
Properties erscheinen entsprechend M1 als getrennte Drawer. Die Auswahl einer
Generalization Edge öffnet den Generalization-Properties-Drawer. Das Dreieck
und die Kante bleiben auch bei Zoom und Pan verbunden; das Panel überdeckt
nicht gleichzeitig mit dem Explorer den gesamten Canvas.

## Anfängerfreundlichkeit, Modernisierung und Hilfe

Der einfache M2-Kernweg lautet:

1. Eine vorhandene Klasse auswählen oder eine Klasse erstellen.
2. Den abstrakten Status bei Bedarf unter `Class Properties -> Details`
   setzen.
3. Unter `Class Properties -> Generalizations` die Aktion
   `Vererbung hinzufügen` ausführen. Die ausgewählte Klasse ist dabei bereits
   als Unterklasse festgelegt.
4. Eine zulässige Oberklasse anhand ihres fachlichen Namens auswählen und die
   neue UML-Kante unmittelbar im Canvas prüfen.
5. Bei Bedarf `Check Constraints` ausführen.

Die Benutzeroberfläche verwendet zunächst den verständlichen Ausdruck
`Vererbung` und ergänzt `Generalisierung` als UML-Fachbegriff in Hilfe und
Tooltip. Damit bleibt das Werkzeug fachlich präzise, ohne vorauszusetzen, dass
Anfänger den Begriff Generalisierung bereits kennen.

Mehrfachvererbung ist unter `Fortgeschrittene Option` progressiv offengelegt.
Sie wird nicht entfernt und bleibt vollständig erreichbar. Konfliktdetails
erscheinen erst, wenn eine Eingabe tatsächlich einen Konflikt erzeugt. Eine
kurze Meldung beschreibt dann das Problem mit Klassennamen; technische Details
bleiben aufklappbar.

Quick Help erklärt das ungefüllte Dreieck, `«abstract»`, Oberklasse und
Unterklasse direkt im Class Diagram. Die Hilfe ist schließbar, intern scrollbar
und erhält beim Öffnen den Fokus. Beim Schließen kehrt der Fokus zur
Hilfe-Aktion zurück.

M2 übernimmt die messbaren Vorgaben aus M1: Knotentext ist mindestens `13 px`,
kompakte UML-Metadaten und Edge Labels sind mindestens `12 px`, Formtexte sind
mindestens `14 px`, Desktop-Aktionen mindestens `40 px` und Aktionen im schmalen
Viewport mindestens `44 px` hoch. Normaler Text erreicht mindestens `4,5:1`
Kontrast; relevante UI-Grenzen und große Schrift erreichen mindestens `3:1`.
Ein sichtbarer Fokusindikator ist mindestens `3 px` stark.

Die Notation bleibt neben Farbe durch Dreiecksform, Linienstil, Text und
`«abstract»` verständlich. Damit adressiert M2 `RD-NAV-001` bis `RD-NAV-005`,
`RD-VIS-001` bis `RD-VIS-004` sowie `RD-A11Y-001` bis `RD-A11Y-005`.

## Compliance-Zuordnung

| Matrix-ID | M2-Bezug |
|---|---|
| `CM-UML-002` | Generalisierung, abstrakte Klassen, Zyklen und Subtyping |
| `CM-UML-003` | mehrere direkte Supertypen und Konfliktdiagnosen |
| `CM-OCL-005` | spätere LUB-Auswirkung bei `if-then-else`; keine neue M2-Editorfunktion |
| `CM-OCL-014` | spätere Typoperationen über Hierarchien |
| `CM-OCL-015` | spätere `allInstances()`-Semantik einschließlich Untertypen |

Das Mockup markiert keine dieser IDs als technisch erfüllt. Es definiert nur
ihre spätere Benutzeroberfläche und die benötigten Verträge.

## Anforderungen an Backend, API und DTOs

Der aktuelle Code besitzt bereits `abstractClass`/`abstract` und
`superClassIds`. Für die spätere Umsetzung sind folgende Verträge zu bestätigen
oder zu ergänzen:

| Bereich | Anforderung |
|---|---|
| Class DTO | `abstract` und `superClassIds` konsistent zwischen Java und TypeScript benennen oder explizit mappen |
| Create/Update | abstrakten Status und direkte Supertypen atomar schreiben |
| Generalization Identity | stabile Edge-ID oder deterministische normalisierte Identität für Selection und Layout |
| Diagnostics | Zyklus, unbekannte Oberklasse, Selbstreferenz und Featurekonflikt strukturiert liefern |
| Delete | Entfernen einer Generalisierung getrennt vom Löschen einer Klasse ermöglichen |
| Snapshot | direkte Instanziierung abstrakter Klassen verhindern |
| Type System | transitive Subtypprüfung, LUB und Mehrfachvererbung zentral bereitstellen |
| OCL | `allInstances`, Typoperationen und Dispatch dieselbe Hierarchie verwenden lassen |

Vorläufig empfohlene Diagnoseziele sind `subClassId`, `superClassId` und
gegebenenfalls `featureId`; die UI zeigt dazu aufgelöste Namen.

## Anforderungen an das Frontend

- `UmlClassNodeData` muss abstrakten Status enthalten und semantisch darstellen.
- Der Diagrammmapper muss Generalization Edges getrennt von Associations erzeugen.
- Selection State benötigt den Typ `generalization`.
- React Flow benötigt einen eigenen Generalization-Edge-Typ mit ungefülltem
  Dreieck am Superclass-Ende.
- Class Properties bindet `abstract` und direkte Supertypen ein.
- Eine selektierte Generalization Edge fokussiert den Generalization-Unterbereich
  der Hauptseite `Class` und verwendet kein Association-Formular.
- Edge-Layout und Markerdefinitionen müssen referenziell stabil sein, damit
  keine Update-Schleife entsteht.
- Create-/Update-Aktionen zeigen Loading, Success und strukturierte Fehler.
- Der Add-Object-Type-Picker schließt abstrakte Klassen aus oder kennzeichnet sie
  nachvollziehbar als nicht auswählbar.

## Annahmen und offene Entscheidungen

| Typ | Eintrag |
|---|---|
| Annahme | Direkte Supertypen werden an der Klasse gepflegt; transitive Supertypen sind read-only ableitbar. |
| Annahme | Abstrakter Status wird mit Kursivschrift und `«abstract»` angezeigt. |
| Offen | separate `GeneralizationDto` mit eigener ID oder `superClassIds` als alleinige Persistenzform |
| Offen | ob Create Class bereits Supertypen anbietet oder diese nur nach Erstellung gepflegt werden |
| Später | optionale API-Operation zum direkten atomaren Ersetzen einer Generalisierung; aktuell Remove plus Add |
| Offen | genaue Darstellung geerbter Attribute und Operationen im Klassenknoten; nicht Teil von M2 |

## Interne Akzeptanzprüfung

- [x] Superclass-Richtung endet mit ungefülltem Dreieck.
- [x] Abstrakte Klasse ist nicht ausschließlich durch Farbe erkennbar.
- [x] Allgemeine Klassendaten und der Generalization-Unterbereich sind innerhalb
  der bestehenden Hauptseite `Class` klar getrennt.
- [x] Mehrere direkte Supertypen sind darstellbar.
- [x] Zyklus- und Konfliktfehler verwenden fachliche Namen.
- [x] Entfernen besitzt einen Confirmation-Zustand.
- [x] Responsive Drawer-Verhalten ist dokumentiert.
- [x] Backend-/DTO- und Frontendfolgen sind dokumentiert, aber nicht umgesetzt.
- [x] M3 und spätere Features wurden nicht vorweggenommen.
- [x] Produktiver Frontend- und Backend-Code blieb unverändert.
- [x] Einfacher Kernweg, progressive Offenlegung und Quick Help sind festgelegt.
- [x] Das visuelle Mockup zeigt die primäre Aktion `Vererbung hinzufügen` und
  erklärt den UML-Fachbegriff `Generalisierung` kontextbezogen.
- [x] Mehrfachvererbung ist als fortgeschrittene, weiterhin erreichbare Option
  dargestellt.
- [x] Schrift-, Control-, Kontrast- und Fokusanforderungen aus M1 sind für M2
  konkretisiert.
- [ ] Fachliche Freigabe durch den Auftraggeber steht aus.
- [ ] `BACKEND_READY` bleibt offen.

## Abgrenzung

Visibility, Namespaces, Imports, Association-End-Metadaten, Qualifier,
Association Classes und Operationsausführung gehören nicht zu M2. Geerbte
Features werden in M2 weder inline bearbeitet noch redefiniert.
