# M6 Association Classes und Linkobjekte

## Zweck und Status

Dieses Dokument ist das Ergebnis von Mockup-Roadmap-Schritt M6. Es legt die
Erstellung und Bearbeitung von Association Classes sowie deren Linkobjekte im
Class Diagram und Object Diagram fest. Aggregation und Composition werden nicht
mehr in M6 wiederholt; dafür ist der Unified Association Workspace maßgeblich.

**Status:** `READY_FOR_REVIEW`  
**Mockup:** `assets/mockups/association-class-properties.html`

M6 ist ein Analyse- und Mockup-Schritt. Es wurden keine fachlichen Funktionen
implementiert und keine produktiven Frontend- oder Backend-Dateien verändert.

## Verwendete Analysegrundlagen

| Bereich | Verwendete Dokumente |
|---|---|
| Überblick | `00-overview/03-documentation-map.md` |
| UI/UX | `04-ui-ux-analysis/01-ui-overview.md`, `02-class-diagram-ui.md`, `03-object-diagram-ui.md`, `04-ocl-and-validation-ui.md`, `05-screenshot-traceability.md`, `07-m1-ui-baseline.md`, `10-redesign-design-principles.md`, `11-m4-association-end-properties.md`, `12-m5-qualified-and-nary-associations.md` |
| Domäne | `03-uml-ocl-domain/01-uml-ocl-scope.md`, `02-domain-model.md`, `04-validation-concept.md` |
| Frontend | `06-frontend-analysis/03-frontend-architecture.md`, `06-diagram-library-decision.md`, `07-class-diagram-component.md`, `08-object-diagram-component.md`, `11-properties-panel.md`, `12-modal-dialogs.md`, `14-state-management.md` |
| Integration | `07-integration-and-api/01-frontend-backend-contract.md`, `03-validation-flow.md`, `04-project-save-load-flow.md`, `07-dto-reference.md`, `08-error-contract.md` |
| Compliance | `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`, `15-full-ocl-uml-implementation-plan.md` |

Als visuelle Referenz wurden insbesondere
`02-class-diagram-association-properties.png`,
`06-object-diagram-object-properties.png`,
`07-object-diagram-validation-error.png`,
`10-modal-add-class-association.png` und
`12-object-diagram-association-properties.png` geprüft.

Das aktuelle Frontend wurde anhand der Class-/Object-Diagram-Komponenten,
Association Edges, Properties Panels und Selection-Struktur geprüft. Das
aktuelle Backend besitzt weder `associationClassId` noch `aggregationKind` im
aktiven Modell. `UmlAssociation` und `ObjectLink` sind zudem auf genau zwei Ends
beschränkt. Das originale USE-Projekt war nicht erforderlich, weil Roadmap,
Domänenanalyse und Compliance-Matrix die benötigte UML-Notation hinreichend
festlegen.

## Abgrenzung

M6 umfasst:

- Association-Class-Knoten im Class Diagram,
- gestrichelte Zuordnung zwischen Association und Association Class,
- Wiederverwendung der Class-Editoren für Attribute und Operationen,
- Linkobjekte mit Slotwerten im Object Diagram,
- gemeinsamen Selection Flow von Association, Association Class, Link und
  Linkobjekt.

M6 umfasst keine Aggregation-/Composition-Bearbeitung, Operation Invocation,
Operationsverträge, Vor-/Nachzustände oder einen allgemeinen
Objektlebenszyklus-Editor. Aggregation Kind, Diamanten und zugehörige
Endvalidierung sind bereits im Unified Association Workspace festgelegt.

## Association Class im Class Diagram

### Konsistenz mit dem Unified-Association-Workspace

Für Association Ends und den Einstieg zur Association Class ist
`21-association-properties.md` die führende visuelle
Referenz. M6 verwendet dieselben End-Cards und denselben Empty-State.
`Association Class hinzufügen` öffnet den dort definierten Modal-Dialog; eine
dauerhaft sichtbare zweite Formularspalte ist nicht mehr Teil des Workflows.

Die ausgewählte Association ist im Modal read-only festgelegt. Nach Eingabe von
Name und Namespace erzeugt die Create-Aktion die Association Class und verbindet
sie atomar. Danach erscheint `Association Class öffnen`; Features werden in den
normalen Class-Properties-Tabs bearbeitet. M6 ergänzt diesen gemeinsamen Ablauf
nur um Association-Class-Notation und Linkobjekte.

Eine Association Class verbindet die Semantik einer Association mit den
Features einer Class. Die UI darf sie deshalb weder als normale Class ohne
Association noch als Association mit frei eingebetteten Feldern darstellen.

Das Mockup verwendet:

```text
Student -------- Enrolls -------- Course
                      |
                      | gestrichelt
                      |
              «associationClass»
                   Enrollment
                 grade : Real
```

Die gestrichelte Linie endet am Mittelpunkt der Association und am Rahmen des
Association-Class-Knotens. Sie darf nicht wie ein zusätzliches Association End
an `Student` oder `Course` wirken. Der Knoten trägt eine sichtbare Kennzeichnung
`«associationClass»`, bleibt aber typografisch und strukturell mit normalen
Class Nodes konsistent.

### Erstellungsworkflow

Eine Association Class wird aus dem Kontext einer bereits ausgewählten
Association erstellt:

1. Der Benutzer wählt die Association im Explorer oder auf dem Canvas aus.
2. Die Properties-Hauptseite `Association` zeigt den Empty-Zustand `Keine
   Association Class vorhanden`.
3. Die Aktion `Association Class hinzufügen` öffnet einen kompakten Dialog.
4. Der Dialog fragt nur den Namen ab, weil die ausgewählte Association bereits
   eindeutig feststeht.
5. Nach erfolgreicher Backendvalidierung erscheint der neue
   `«associationClass»`-Knoten und die Properties-Hauptseite wechselt zu
   `Class`, damit Attribute und Operationen bearbeitet werden können.

Die Aktion gehört nicht in das allgemeine Class-Menü und nicht an ein einzelnes
Association End. Ist bereits eine Association Class verbunden, wird statt der
Create-Aktion eine Aktion zum Öffnen der bestehenden Association Class gezeigt.

Das HTML-Mockup dokumentiert den geöffneten Create-Dialog zusätzlich als
dauerhaft sichtbare Referenzansicht. Der Dialog übernimmt Aufbau und
Interaktionshierarchie von `create-class-modal.html`: Name, Namespace,
Visibility, Qualified Name, read-only verknüpfte Association, kompakte initiale
Attribute und Operationssignaturen, UML-Vorschau und einen eindeutigen
Create-Footer. `Add Attribute`, `Add Operation` und die jeweilige Remove-Aktion
sind im interaktiven Dialog ausführbar. Parameter, Multiplizitäten und weitere
Feature-Eigenschaften werden erst nach der Erstellung in den zuständigen Class-
Properties-Tabs gepflegt. Diese Referenzansicht ergänzt den weiterhin
interaktiven Dialog im Haupt-Workspace.

## Properties und Bearbeitung

Association Classes verwenden dieselben Attribute-/Operationseditoren wie
normale Classes. Ein zusätzlicher, nicht mit einem internen ID-Feld
verwechselbarer Association-Picker zeigt die fachlich verknüpfte Association.

| Bereich | Verhalten |
|---|---|
| Class | Name, Attribute und Operationensignaturen bearbeiten |
| Association | verknüpfte Association und Ends anzeigen |
| Invariant | zugehörige Klasseninvarianten anzeigen; Operationsverträge bleiben bei der Operation unter `Class` |
| Association Picker | fachlichen Association-Namen und beteiligte Classifier anzeigen |
| Delete | gemeinsame Auswirkung auf Classifier, Association und Instanzen bestätigen |

Das Umschalten der verknüpften Association ist nur zulässig, wenn das Backend
die bestehende Instanz- und OCL-Auswirkung geprüft hat. Das Frontend nimmt keine
automatische Umdeutung bestehender Linkobjekte vor.

Eine zweite dauerhaft sichtbare Referenzansicht zeigt den Zustand unmittelbar
nach erfolgreicher Erstellung. Sie enthält den Association-Class-Knoten, die
gestrichelte Zuordnung zu `Enrolls` und das vollständige Bearbeitungsfeld
`Class Properties · Details`. Darin sind `Details`, `Attributes` und
`Operations` als dieselben Feature-Tabs wie bei normalen Classes sichtbar. Die
verknüpfte Association ist als read-only Fachreferenz angegeben.

Eine Association Class ist zugleich Classifier und Association. Sie darf daher
als Typ an weiteren Associations teilnehmen und eigene OCL-Invarianten besitzen.
Diese Funktionen werden über die bestehenden Association- und
Invariant-Workflows bearbeitet und benötigen keinen zusätzlichen M6-Feature-Tab.
Der generische `Generalizations`-Tab wird in M6 nicht angeboten: Eine
Association Class darf nicht wie eine beliebige Class oder Association mit
normalen Superclass Candidates verknüpft werden. Eine mögliche Spezialisierung
zwischen kompatiblen Association Classes erfordert einen eigenen, semantisch
eingeschränkten Workflow und ist nicht Bestandteil dieses Mockups.

## Linkobjekt im Object Diagram

Eine Association-Class-Instanz ist nicht ein beliebiges zusätzliches Objekt.
Sie identifiziert zugleich genau einen Link der zugehörigen Association und
besitzt eigene Slots. Das Mockup zeigt daher:

- den normalen Link zwischen den beteiligten Objekten,
- ein benanntes Linkobjekt mit unterstrichenem `objectName : ClassName`,
- eine gestrichelte Linie zwischen Link und Linkobjekt,
- Slotwerte der Association Class,
- eine gekoppelte Selektion.

Klick auf Link, Linkobjekt oder Explorer-Eintrag hebt dieselbe fachliche Instanz
hervor. Das Properties Panel öffnet primär die Linkobjekt-Slots und bietet einen
klaren Wechsel zur Association-Belegung. Interne Objekt- und Link-IDs bleiben
in technischen Details verborgen.

## Zustandsmodell

| Zustand | Darstellung und Verhalten |
|---|---|
| Default | normale Association zeigt noch keine Association-Class-Zuordnung |
| Selected | Association Class, Association oder Linkobjekt teilen eine verbundene Hervorhebung |
| Editing | Class Details und Features werden in den normalen Class Properties bearbeitet |
| Loading | Association Class wird erstellt; die Create-Aktion ist deaktiviert |
| Error | Konflikt nennt Namespace, Class, Association oder Linkobjekt fachlich |
| Disabled | unzulässige Create- oder Save-Aktion besitzt eine sichtbare Begründung |
| Empty | Association besitzt noch keine Association Class; Create-Aktion ist sichtbar |
| Confirmation | Delete- oder Rebind-Auswirkung wird vor einer späteren Mutation zusammengefasst |
| Success | gespeicherte Association-Class-Zuordnung wird bestätigt |

## Responsives Verhalten

Im schmalen Viewport bleibt der Canvas Hauptarbeitsfläche. Association-Class-
Properties öffnen sich als intern scrollbar bleibender Drawer. Die bestehenden
Hauptseiten `Class`, `Association` und `Invariant` bleiben erreichbar, werden aber nicht
gleichzeitig vollständig dargestellt. Association, Association Class und
Linkobjekt verwenden weiterhin denselben Selection-Kontext.

Gestrichelte Zuordnungslinien sind keine frei positionierbaren Labels. Beim
responsiven Routing oder Verschieben müssen sie an ihren semantischen
Endpunkten bleiben.

## Redesign- und Accessibility-Bezug

M6 folgt den priorisierten Redesign-Vorgaben:

- **Einfachheit:** Association Classes verwenden die bekannten Class-Editoren
  und werden direkt aus der ausgewählten Association erstellt.
- **Modernisierung:** ruhige Properties-Segmente, strukturierte Diagnosen und
  eindeutige Selection ersetzen versteckte Desktop-Beziehungen.
- **Accessibility:** UML-Formen werden zusätzlich textuell benannt; Farbe ist
  nie das einzige Signal. Controls sind mindestens 40 Pixel hoch und besitzen
  sichtbare Fokuszustände.
- **Hilfe:** Quick Help erklärt Association Class und Linkobjekt direkt im
  Arbeitskontext, ohne den Canvas dauerhaft zu verdecken.

Berücksichtigt werden `RD-NAV-001` bis `RD-NAV-005`, `RD-VIS-001` bis
`RD-VIS-004` und `RD-A11Y-001` bis `RD-A11Y-005`.

## Compliance-Zuordnung

| Matrix-ID | M6-Bezug |
|---|---|
| `CM-UML-011` | Association Classes und gemeinsame Link-/Objektidentität |
| `CM-OCL-013` | Navigation zu Linkobjekten und deren Properties |
| `CM-UML-007` | zugrunde liegende binäre Association bleibt sichtbar |

`CM-UML-013` und Composition-bezogene Teile von `CM-UML-017` werden im Unified
Association Workspace nachgewiesen und gehören nicht mehr zum M6-Mockup.

## Vorläufige Anforderungen an Domänenmodell, API und DTOs

```ts
interface AssociationClassReferenceDto {
  associationId: string;
  classId: string;
}

interface AssociationClassInstanceDto {
  objectId: string;
  linkId: string;
  classId: string;
  associationId: string;
  slots: SlotDto[];
}
```

| Bereich | Spätere Anforderung |
|---|---|
| Domäne | stabile bidirektionale Beziehung zwischen Association und Association Class |
| Snapshot | gemeinsame Link-/Objektidentität mit genau einer Association-Belegung |
| API | Create, Read, Update und Delete für Association Classes |
| Validierung | Eindeutigkeit, Zuordnung und Linkobject-Identität serverseitig prüfen |
| Error DTO | Association, Class, Link und Linkobjekt strukturiert referenzieren |
| OCL | Association-Class-Properties über die zugehörige Navigation typisieren und auswerten |

## Vorläufige Frontend-Anforderungen

- eigener Association-Class-Node-Typ auf Basis des bestehenden Class Nodes,
- gestrichelte, nicht navigierbare Zuordnungsedge zum Association-Mittelpunkt,
- einheitliches Selection-Mapping für Association, Class, Link und Linkobjekt,
- Wiederverwendung von Attribute-/Operationen-Editoren ohne Operationsaufruf,
- Object-Link-Mapper mit optionaler Linkobjekt-Darstellung,
- strukturierte Create-, Zuordnungs- und Identitätsdiagnosen,
- persistierbares Layout für Association-Class- und Linkobjekt-Knoten,
- kollisionsarmes Routing von Zuordnungslinie, Rollen, Multiplizitäten und Labels.

## Annahmen und offene Entscheidungen

| Art | Punkt |
|---|---|
| Annahme | Eine Association Class referenziert genau eine Association. |
| Annahme | Eine Association-Class-Instanz besitzt gekoppelte Objekt- und Linkidentität. |
| Offen | Ob Rebinding einer Association Class nach vorhandenen Instanzen grundsätzlich verboten wird. |
| Offen | Darstellung einer Association Class an n-ären Associations aus M5. |
| Offen | Vollständige normative OCL-Navigation und Collection-Art für Linkobjekte. |

## Akzeptanzkriterien

- [x] Association und Association-Class-Knoten sind visuell getrennt und durch
  eine gestrichelte Linie verbunden.
- [x] Association Classes verwenden dieselben Attribute-/Operationseditoren wie
  normale Classes.
- [x] `Association Class hinzufügen` ist im Empty-Zustand der ausgewählten
  Association verortet und führt nach Erfolg zu `Class Properties`.
- [x] Der geöffnete Create-Association-Class-Dialog ist als eigene sichtbare
  Referenzansicht dokumentiert.
- [x] Initiale Attribute und Operationssignaturen können im Create-Dialog
  hinzugefügt und wieder entfernt werden.
- [x] Das vollständige Association-Class-Bearbeitungsfeld unter `Class
  Properties · Details` ist als eigene sichtbare Referenzansicht dokumentiert.
- [x] Ein Linkobjekt mit eigenen Slots ist im Object Diagram dargestellt.
- [x] Link, Linkobjekt und Explorer besitzen einen gemeinsamen Selection Flow.
- [x] Namens-, Zuordnungs- und Linkobject-Fehler sind dokumentiert.
- [x] Aggregation und Composition werden nicht redundant in M6 dargestellt.
- [x] Default-, Selected-, Editing-, Loading-, Error-, Disabled-, Empty-,
  Confirmation- und Success-Zustände sind festgelegt.
- [x] Properties und Drawer sind intern scrollbar und responsiv spezifiziert.
- [x] Backend-, API-, DTO-, Frontend- und Compliance-Folgen sind dokumentiert.
- [x] M7- und M8-Funktionen wurden nicht vorgezogen.

## Ergebnis

M6 macht Association Classes als verknüpfte, aber weiterhin klassenartig
bearbeitbare Modellbestandteile verständlich. Linkobjekte behalten ihre
gemeinsame Objekt-/Linksemantik. Aggregation und Composition bleiben vollständig
im Unified Association Workspace dokumentiert, wodurch M6 keine konkurrierende
Association-Properties-Darstellung mehr enthält.
