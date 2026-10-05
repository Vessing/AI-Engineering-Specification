# M5 Qualifier und n-äre Associations

## Zweck und Status

Dieses Dokument ist das Ergebnis von Mockup-Roadmap-Schritt M5. Es definiert
die zukünftige Bedienung qualifizierter und n-ärer Associations im Class
Diagram, Object Diagram, OCL-Kontext und Validation Detail.

**Status:** `READY_FOR_REVIEW`  
**Mockup:** `assets/mockups/association-properties-qualified-nary.html`

M5 implementiert keine fachliche Funktion und ändert keinen produktiven Code.

## Verwendete Analysegrundlagen

| Bereich | Verwendete Dokumente |
|---|---|
| Überblick | `00-overview/03-documentation-map.md` |
| UI/UX | `04-ui-ux-analysis/01-ui-overview.md`, `02-class-diagram-ui.md`, `03-object-diagram-ui.md`, `04-ocl-and-validation-ui.md`, `05-screenshot-traceability.md`, `07-m1-ui-baseline.md`, `10-redesign-design-principles.md`, `11-m4-association-end-properties.md` |
| Domäne | `03-uml-ocl-domain/01-uml-ocl-scope.md`, `02-domain-model.md`, `04-validation-concept.md` |
| Frontend | `06-frontend-analysis/03-frontend-architecture.md`, `07-class-diagram-component.md`, `08-object-diagram-component.md`, `11-properties-panel.md`, `12-modal-dialogs.md`, `14-state-management.md` |
| Integration | `07-integration-and-api/01-frontend-backend-contract.md`, `07-dto-reference.md`, `08-error-contract.md` |
| Compliance | `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`, `15-full-ocl-uml-implementation-plan.md` |

Als visuelle Referenz wurden besonders
`02-class-diagram-association-properties.png`,
`10-modal-add-class-association.png`,
`11-modal-add-object-association.png`,
`12-object-diagram-association-properties.png` und
`15-properties-association.png` verwendet.

Das aktuelle Frontend wurde anhand der Association-Modals, Properties Panels,
Diagramm-Mapper und Object-Link-Komponenten geprüft. Im aktuellen Backend
erzwingt `ObjectLink` exakt zwei Ends. Das originale USE-Projekt war für M5
nicht erforderlich, weil die UML-/OCL-Analyse und Compliance-Matrix den
fachlichen Umfang ausreichend festlegen.

## Abgrenzung

M5 umfasst:

- Qualifierdefinitionen an einem Association End,
- konkrete Qualifierwerte an Object Links,
- dynamische Association-End-Listen mit mindestens zwei Ends,
- zentrale Darstellung n-ärer Associations,
- n-äre Object Links,
- Navigationsergebnis und qualifizierte Diagnosen,
- Auswirkungswarnung beim Entfernen eines Ends.

M5 umfasst keine Association Classes, Aggregation, Composition, Ownership- oder
Lebenszyklusregeln. Diese Inhalte gehören zu M6. Eine ausführbare OCL-Syntax für
qualifizierte Navigation wird nicht implementiert; das Mockup zeigt lediglich
den zukünftigen Editor- und Ergebniskontext.

## Fachliches Modell

### Association Ends statt Source und Target

Eine Association besitzt eine geordnete Liste von mindestens zwei Ends. Jedes
End referenziert einen Klassifier und besitzt unter anderem Rollenname und
Multiplizität. `Source` und `Target` sind höchstens Bezeichnungen für die
Darstellung einer binären Association. Sie dürfen nicht das persistierte
Fachmodell n-ärer Associations bestimmen.

### Qualifier

Ein Qualifier gehört zu genau einem Association End. Seine Definition enthält
mindestens einen Namen, einen Typ und eine stabile Reihenfolge. Der konkrete
Qualifierwert gehört dagegen zur Belegung dieses Ends in einem Object Link.

| Ebene | Beispiel | Bedeutung |
|---|---|---|
| UML-Modell | `matriculationNo : String` | Qualifierdefinition am Student-End |
| Snapshot | `matriculationNo = 'S-1042'` | konkreter Wert eines Enrollment-Links |
| OCL | qualifizierte Navigation | Auswahl anhand des Qualifierwerts |

### n-äre Associations

Das Mockup verwendet `Enrollment` zwischen `Student`, `Course` und `Semester`.
Ein zentraler, benannter Association-Knoten verbindet alle drei Ends. Rollen und
Multiplizitäten bleiben an den zugehörigen Klassifierenden sichtbar. Das Object
Diagram belegt alle Ends gemeinsam; eine Folge binärer Links wäre fachlich
nicht gleichwertig.

## Hauptzustand im Class Diagram

Qualifier und n-äre Association Ends werden innerhalb der bestehenden
Properties-Hauptseite `Association` bearbeitet. Die Hauptseiten `Class`,
`Association` und `Invariant` bleiben unverändert; insbesondere entsteht kein
zusätzlicher Haupttab für Qualifier oder n-äre Associations.

Der Desktopzustand behält M1-Workspace, Explorer, Canvas, Properties und Console
bei. `Enrollment` wird als zentraler Knoten dargestellt. Die Properties zeigen
eine scrollbar bleibende, dynamische End-Liste:

1. Der Association-Name wird einmal bearbeitet.
2. Jedes End zeigt dauerhaft den fachlichen Klassennamen.
3. Rolle und Multiplizität werden pro End gepflegt.
4. Qualifier werden innerhalb ihres Ends angelegt, sortiert und gelöscht.
5. `Add Association End` erweitert die Association ohne Source-/Target-Wechsel.
6. Die Diagrammnotation aktualisiert sich erst nach erfolgreicher Speicherung.

Lange Rollen, Multiplizitäten und Qualifierfächer werden kollisionsarm am
jeweiligen End platziert. Bei Platzmangel verschiebt die Routinglogik Labels,
nicht die Diagrammknoten.

## Add Association Modal

Der bekannte binäre Kernweg bleibt schnell: Der Dialog startet mit zwei kompakten
End-Karten. `Add End` ergänzt eine dritte oder weitere Karte und schaltet damit
denselben Dialog in den n-ären Modus. Die UI erzwingt keine getrennten
Source-/Target-Spalten.

| Feld/Aktion | Verhalten |
|---|---|
| Association Name | Pflichtfeld und fachlicher Anzeigename |
| End Card | Classifier, Rolle, Multiplizität und End-Metadaten |
| Add End | fügt ein leeres End nach dem letzten End ein |
| Drag Handle | verändert die stabile End-Reihenfolge |
| Add Qualifier | ergänzt Name und Typ innerhalb genau eines Ends |
| Remove End | verlangt ab drei Ends eine Auswirkungsbestätigung |
| Create Association | bleibt deaktiviert, solange ein End ungültig ist |

Das Entfernen eines Ends aus einer binären Association wird nicht angeboten,
weil eine Association mindestens zwei Ends benötigt.

## Object-Link-Dialog

Der Linkdialog wird aus den Ends der gewählten Association erzeugt. Für jedes
End existiert ein nach Klasse und Rolle beschrifteter Object Picker. Direkt
unter einem qualifizierten End stehen die typisierten Qualifierwerte. Das
Frontend zeigt Objektname und Klasse, nicht interne IDs.

Der Submit ist nur aktiv, wenn jedes End genau passend belegt ist und alle
Qualifierwerte syntaktisch vorvalidiert wurden. Die fachliche Typ-, Link- und
Multiplizitätsvalidierung bleibt im Backend.

## OCL- und Validation-Darstellung

Das OCL-Detail zeigt:

- den verwendeten Association-End-/Rollennamen,
- Qualifiername und erwarteten Typ,
- den abgeleiteten Navigationstyp,
- bei n-ärer Navigation die weiteren gebundenen Ends,
- bei Fehlern fachliche Objekt-, Rollen- und Qualifiernamen.

Interne IDs dürfen als technische Details verfügbar sein, stehen aber nicht in
der primären Fehlermeldung. Ein Qualifierfehler markiert Qualifierfeld, Link und
betroffenes End gemeinsam.

Die konsolidierte Fehleransicht unterscheidet Modelldiagnosen für ungültige
Multiplicity und doppelte Qualifierdefinitionen von Snapshotdiagnosen für
fehlende n-äre Endbelegungen und ungültige konkrete Qualifierwerte. Der Submit
bleibt bei einem Feldfehler deaktiviert; bereits gültige Endbelegungen bleiben
als Draft erhalten.

## Zustandsmodell

| Zustand | Darstellung und Verhalten |
|---|---|
| Default | zwei kompakte End-Karten im Create-Dialog |
| Selected | zentraler Association-Knoten und End-Liste sind hervorgehoben |
| Editing | genau ein End beziehungsweise Qualifier besitzt aktive Felder |
| Loading | Typ-/Objektoptionen zeigen Progress; Submit ist deaktiviert |
| Error | Inline-Fehler nennt End, Rolle, Qualifier oder fehlendes Objekt |
| Disabled | Submit oder End-Löschen ist mit sichtbarer Begründung deaktiviert |
| Empty | End besitzt noch keine Qualifier; `Add Qualifier` bleibt sichtbar |
| Confirmation | End-Löschung listet Auswirkungen auf Links und OCL-Navigation |
| Success | Bestätigung nennt Association, Endanzahl und Qualifieranzahl |

## Responsives Verhalten

Unterhalb des M1-Breakpoints bleiben Explorer und Properties nicht gleichzeitig
neben dem Canvas stehen. Die Association öffnet einen intern scrollbar
bleibenden Properties Drawer. End-Karten sind einklappbar; das ausgewählte End
steht zuerst. Räumliche Begriffe wie links/rechts oder Source/Target werden
durch Classifier- und Rollennamen ersetzt.

## Redesign- und Accessibility-Bezug

M5 folgt den drei priorisierten Redesign-Zielen:

- **Einfachheit:** Binäre Associations bleiben der kompakte Standard; n-äre
  Komplexität erscheint erst durch `Add End`.
- **Modernisierung:** ruhige Karten, klare Zustände und einheitliche Controls
  ersetzen tabellarische Desktop-Dialoge.
- **Accessibility und Hilfe:** ausreichender Kontrast, mindestens 40 Pixel hohe
  Controls, sichtbare Fokuszustände, Inline-Hilfe und fachliche Fehlermeldungen.

Die relevanten Redesign-Anforderungen sind `RD-NAV-001` bis `RD-NAV-005`,
`RD-VIS-001` bis `RD-VIS-004` und `RD-A11Y-001` bis `RD-A11Y-005`.

## Compliance-Zuordnung

| Matrix-ID | M5-Bezug |
|---|---|
| `CM-UML-009` | Qualifierdefinitionen und konkrete Qualifierwerte |
| `CM-UML-010` | dynamische Association Ends und n-äre Object Links |
| `CM-OCL-013` | qualifizierte und n-äre Association-End-Navigation |
| `CM-UML-007` | schneller binärer Erstellungsweg bleibt erhalten |
| `CM-UML-012` | Ends werden über Rollen statt pauschaler Richtung unterschieden |

## Vorläufige Anforderungen an Domänenmodell, API und DTOs

```ts
interface AssociationEndDto {
  id: string;
  classifierId: string;
  roleName: string;
  multiplicity: MultiplicityDto;
  qualifiers: QualifierDefinitionDto[];
}

interface QualifierDefinitionDto {
  id: string;
  name: string;
  type: UmlTypeDto;
  order: number;
}

interface ObjectLinkEndDto {
  associationEndId: string;
  objectId: string;
  qualifierValues: QualifierValueDto[];
}
```

| Bereich | Spätere Anforderung |
|---|---|
| Domäne | Associations erlauben `ends.size >= 2`; Object Links belegen Ends endbasiert |
| Persistenz | End- und Qualifierreihenfolge sowie Werte verlustfrei speichern |
| API | Create/Update/Delete arbeiten mit Endlisten statt Source/Target-Paaren |
| Validierung | Endvollständigkeit, Klassifierpassung, Qualifiertyp und Multiplizität prüfen |
| Fehlervertrag | `associationId`, `associationEndId`, `qualifierId`, `linkId` strukturiert referenzieren |
| OCL | Navigationstyp und Lookup-Semantik für Qualifier und n-äre Ends bestimmen |

Ein Versions-/Revisionstoken wird für konfliktfreie Updates empfohlen. Beim
Entfernen eines Ends muss das Backend Auswirkungen bestimmen oder einen
strukturierten Konflikt zurückgeben. Es darf keine automatische Reduktion
bestehender n-ärer Links auf binäre Links geben.

## Vorläufige Frontend-Anforderungen

- `AddAssociationModal` muss dynamische End-Karten statt fester Source-/Target-
  Felder rendern.
- Mapper und Diagrammtypen benötigen einen zentralen Association-Knoten für
  drei oder mehr Ends.
- Das Object Diagram benötigt n-äre Link-Geometrie und endbasiertes Selection
  Mapping.
- Properties und Linkdialoge müssen Qualifierlisten und -werte typisiert
  darstellen.
- Auto-Layout und Labelrouting müssen Rollen, Multiplizitäten und
  Qualifierfächer kollisionsarm positionieren.
- Optimistische Darstellung darf erst nach erfolgreicher Backend-Antwort als
  gespeichert gelten.

## Annahmen und offene Entscheidungen

| Art | Punkt |
|---|---|
| Annahme | Eine Association besitzt mindestens zwei geordnete Ends. |
| Annahme | Qualifier sind geordnete, typisierte Definitionen eines einzelnen Ends. |
| Annahme | Der zentrale Knoten ist die Standardnotation für n-äre Associations. |
| Offen | Exakte normative OCL-Syntax und Ergebniskardinalität qualifizierter Navigation. |
| Offen | Zulässige Qualifiertypen und Umgang mit mehrwertigen Qualifiern. |
| Offen | Darstellung reflexiver n-ärer Associations bei mehrfach demselben Klassifier. |
| Offen | Ob End-Reihenfolge fachlich relevant oder ausschließlich stabil zu serialisieren ist. |
| Offen | Umfang der serverseitigen Impact-Analyse vor dem Entfernen eines Ends. |

## Akzeptanzkriterien

- [x] Qualifierfach, Qualifierliste und konkrete Linkwerte sind entworfen.
- [x] Eine n-äre Association mit drei Ends besitzt UML-konforme zentrale
  Darstellung.
- [x] Create-Dialog und Properties verwenden dynamische End-Listen.
- [x] Der Object-Link-Dialog belegt alle Ends gemeinsam.
- [x] Binäre Associations bleiben ohne zusätzlichen Modus schnell erstellbar.
- [x] Die UI verwendet für n-äre Fälle keine fachliche Source-/Target-Annahme.
- [x] Löschen eines Ends zeigt eine Auswirkungswarnung.
- [x] Default-, Selected-, Editing-, Loading-, Error-, Disabled-, Empty-,
  Confirmation- und Success-Zustände sind festgelegt.
- [x] Properties und umfangreiche Dialoginhalte sind intern scrollbar.
- [x] Ein schmaler responsiver Zustand ist dokumentiert.
- [x] Compliance-, Backend-, API-, DTO- und Frontend-Folgen sind zugeordnet.
- [x] Multiplizität, Qualifierdefinition, konkreter Qualifierwert und fehlende
  n-äre Endbelegung besitzen getrennte feldbezogene Diagnosen.
- [x] Association Classes, Aggregation und Composition wurden nicht vorgezogen.

## Ergebnis

### Konsistenz mit dem Unified-Association-Workspace

Das M5-Mockup verwendet dieselbe `Association Properties`-Seite wie
`association-properties.html`. End-Cards besitzen einen
einheitlichen Header mit fachlichem Classifier- und Rollennamen, Status-Pill
und Chevron. Rolle, Multiplizität, M4-End-Metadaten und Qualifier werden im
aufgeklappten End bearbeitet. `End hinzufügen` erweitert dieselbe End-Liste;
`Association speichern` bleibt die gemeinsame atomare Abschlussaktion.

Die zentrale n-äre UML-Notation, der Object-Link-Dialog und die
Qualifier-/OCL-Zustände bleiben M5-spezifisch. Sie sind keine abweichende
Navigation, sondern progressive Erweiterungen des Unified-Workflows.

M5 ersetzt die binäre Source-/Target-Annahme durch einen verständlichen,
endbasierten Editor, ohne den einfachen binären Arbeitsablauf zu verschlechtern.
Qualifierdefinitionen und konkrete Linkwerte bleiben sauber getrennt. Die
Mockups bilden die notwendige Grundlage für Compliance-Schritt 7, setzen dessen
Backend- und Frontendfunktionen jedoch noch nicht um.

### Konsistenzkorrektur zu M4

Die visuelle Nachprüfung am 24. August 2026 hat M5 an die verbindliche
M4-Workspace-Shell angeglichen. Header, Projektkontext, Hauptnavigation,
Reihenfolge von `Refresh` und `Check Constraints`, Next-Step-Leiste,
Explorerbreite, Propertiesbreite, echte `Class`-/`Association`-/`Invariant`-
Tabs sowie Console und Validation Results folgen nun demselben Muster.

Der Desktopzustand besitzt keinen horizontalen Überlauf mehr. Die drei Linien
der n-ären Association treffen den zentralen Association-Knoten und die
zugehörigen Klassen; Labels und Qualifierfach liegen oberhalb der Linien und
überdecken keine Properties. Zustands- und Dialogtexte verwenden durchgängig
dieselbe deutsche UI-Sprache wie M4. Im schmalen Viewport bleiben Canvas und
intern scrollbar bleibender Properties-Drawer getrennt.
