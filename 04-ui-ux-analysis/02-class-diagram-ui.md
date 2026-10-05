# Class Diagram UI

## Zweck dieser Datei

Diese Datei beschreibt Anforderungen und Verhalten der Klassendiagramm-UI des neuen UML/OCL-Websystems.

Sie fokussiert auf:

- Darstellung von Klassen, Attributen, Operationen und Invarianten,
- Darstellung und Bearbeitung von Assoziationen,
- Rollen und Multiplizitäten,
- Explorer-Verhalten im Class-Diagram-Kontext,
- Properties Panel für Klassen, Associations und Invarianten,
- Modals zum Erstellen von Klassen, Associations und Invarianten,
- Interaktion mit Backend/API,
- Layoutspeicherung,
- Validierungsbezug.

Diese Datei ist eine UI/UX-Analyse. Sie beschreibt keine Implementierung und keine konkrete Diagrammbibliothek.

Hinweis zur Ablage: Die Zielstruktur nennt `assets/screenshots/*.png`. Im aktuellen Repository liegen die Screenshots real unter `assets/screenshots/*.png`. Die Links in dieser Datei nutzen die aktuell vorhandenen Pfade.

## Relevante Screenshots

| Screenshot | Relevanz |
|---|---|
| `01-class-diagram-class-properties.png` | Klassendiagramm mit selektierter Klasse, Explorer, Canvas, Class Properties, Console. |
| `02-class-diagram-association-properties.png` | Association-Selektion und Association Properties. |
| `03-class-diagram-invariant-properties.png` | Invariant-Selektion, OCL-Ausdruck und Invariant Properties. |
| `04-class-diagram-new-class-selected.png` | Neue Klasse im Canvas selektiert und editierbar. |
| `08-modal-add-class.png` | Modal zum Erstellen einer Klasse. |
| `09-modal-add-invariant.png` | Modal zum Erstellen einer Invariante. |
| `10-modal-add-class-association.png` | Modal zum Erstellen einer Klassenassoziation. |
| `15-properties-association.png` | Properties Panel einer selektierten Klasse mit aktivem Segment `Association`. |
| `16-properties-invariants.png` | Properties Panel einer selektierten Klasse mit aktivem Segment `Invariant`. |
| `17-new-class.png` | Neue Klasse ist selektiert; Segment `Class` zeigt Name, Attribute, Operationen und Add-Aktionen. |

![Class Diagram Class Properties](../assets/screenshots/01-class-diagram-class-properties.png)

![Class Diagram Association Properties](../assets/screenshots/02-class-diagram-association-properties.png)

![Class Diagram Invariant Properties](../assets/screenshots/03-class-diagram-invariant-properties.png)

![New Class Selected](../assets/screenshots/04-class-diagram-new-class-selected.png)

![Class Properties Association Segment](../assets/screenshots/15-properties-association.png)

![Class Properties Invariant Segment](../assets/screenshots/16-properties-invariants.png)

![New Class Properties](../assets/screenshots/17-new-class.png)

## Rolle des Klassendiagramms

Das Klassendiagramm ist die zentrale View für das UML-Modell. Es definiert die fachliche Struktur, gegen die später Snapshots und OCL-Invarianten validiert werden.

Das Klassendiagramm ist im MVP verantwortlich für:

- Klassen,
- Attribute,
- Operationen als Signaturen,
- Assoziationen,
- Rollen,
- Multiplizitäten,
- Invarianten,
- Layout der Klassen und Associations,
- Auswahl und Bearbeitung von Modellbestandteilen.

Fachlich ist das Klassendiagramm die Grundlage für:

| Folgefunktion | Abhängigkeit vom Klassendiagramm |
|---|---|
| Objektdiagramm | Objekte sind Instanzen von Klassen; Slots basieren auf Attributen. |
| Objektlinks | Objektlinks basieren auf Assoziationen. |
| OCL-Typechecking | Attribute und Rollen werden gegen das UML-Modell geprüft. |
| Multiplizitätsvalidierung | Association Ends liefern Kardinalitätsregeln. |
| Invariantenauswertung | Invarianten besitzen eine Kontextklasse. |

## Klassendiagramm-Canvas

Der `ClassDiagramCanvas` stellt die Klassenstruktur visuell dar.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Klassen, Associations und Invariantenreferenzen als Diagramm sichtbar machen. |
| Angezeigte Daten | `UmlClass`, `UmlAttribute`, `UmlOperation`, `UmlAssociation`, `UmlInvariant`, `LayoutInformation`. |
| Nutzeraktionen | Klasse auswählen, Association auswählen, Invariante auswählen, Klassen verschieben, ggf. Canvas bewegen/zoomen. |
| Backend-Daten | IDs, Namen, Attribute, Operationen, Association Ends, Rollen, Multiplizitäten, Invarianten. |
| Lokaler Zustand | Aktuelle Selektion, Hover-Zustand, Drag-Zustand, Viewport, Zoom, temporäre Positionen. |
| Screenshot-Bezug | `01`, `02`, `03`, `04` |

Canvas-Anforderungen:

- Klassen werden als stabile Knoten dargestellt.
- Assoziationen werden als Kanten zwischen Klassen dargestellt.
- Selektierte Elemente werden eindeutig hervorgehoben.
- Änderungen im Properties Panel aktualisieren die Canvas-Darstellung.
- Neue Klassen erscheinen nach Erstellung direkt im Canvas.
- Layoutpositionen müssen speicherbar sein.

## Klassenkarte

Eine Klassenkarte entspricht einer `UmlClass`.

| Bereich der Klassenkarte | Inhalt |
|---|---|
| Kopf | Klassenname, z. B. `User`, `Book`, `Libary`. |
| Attribute | Liste von Attributen mit Typ, z. B. `name : String`, `books : Integer`. |
| Operationen | Liste von Signaturen, z. B. `borrow() : Void`. |
| Invarianten | Referenzen wie `inv: maxBooks`. |
| Selektion | Blaue oder anderweitig klare Hervorhebung. |

![New Class Selected](../assets/screenshots/04-class-diagram-new-class-selected.png)

Anforderungen:

| Anforderung | MVP | Hinweis |
|---|---|---|
| Klassenname anzeigen | Ja | Muss mit Explorer und Properties Panel synchron sein. |
| Attribute anzeigen | Ja | Name und Typ reichen im MVP. |
| Operationen anzeigen | Ja | Nur Signaturen, keine Ausführung. |
| Invariantenreferenzen anzeigen | Ja | Mindestens Name der Invariante. |
| Selektionszustand anzeigen | Ja | Auswahl muss eindeutig sichtbar sein. |
| Fehlerzustand anzeigen | Sollte | Z. B. bei OCL-Typefehler oder Modellstrukturfehler. |
| Drag/Positionierung | Ja | Layoutdaten müssen aktualisiert werden können. |

## Attribute und Operationen

Attribute und Operationen sind innerhalb der Klassenkarte und im Class Properties Panel sichtbar.

![Class Properties](../assets/screenshots/01-class-diagram-class-properties.png)

### Attribute

| Aspekt | Beschreibung |
|---|---|
| Daten | `id`, `name`, `type`, optional Reihenfolge. |
| MVP-Typen | `String`, `Integer`, `Real`, `Boolean`. |
| Nutzeraktionen | Hinzufügen, Bearbeiten, ggf. Löschen. |
| Validierungsbezug | Slots im Objektdiagramm und OCL-Attributzugriffe hängen von Attributen ab. |

### Operationen

| Aspekt | Beschreibung |
|---|---|
| Daten | `id`, `name`, `parameters`, `returnType`. |
| MVP-Verhalten | Nur Signaturen; keine Bodies, keine Pre-/Postconditions. |
| Nutzeraktionen | Hinzufügen, Bearbeiten, ggf. Löschen. |
| Validierungsbezug | Im MVP kein Auswertungsbezug; später relevant für Pre-/Postconditions. |

Offene UI-Frage:

Die Screenshots zeigen `Add Attribute` und `Add Operation`, aber keine entsprechenden Modals. Es muss entschieden werden, ob Attribute und Operationen inline im Properties Panel oder über eigene Dialoge erstellt werden.

## Assoziationen

Assoziationen verbinden Klassen im Klassendiagramm.

![Association Properties](../assets/screenshots/02-class-diagram-association-properties.png)

| Aspekt | Beschreibung |
|---|---|
| Zweck | Fachliche Beziehungen zwischen Klassen modellieren. |
| Daten | `UmlAssociation`, `UmlAssociationEnd`, Rollen, Multiplizitäten. |
| Darstellung | Kante zwischen Klassen, Association Name mittig als Label; Rollen und Multiplizitaeten als Endlabels an beiden Association-Enden. |
| Nutzeraktionen | Association auswählen, erstellen, bearbeiten, ggf. löschen. |
| Backend-Bezug | Grundlage für Objektlinks, OCL-Navigation und Multiplizitätsprüfung. |
| Screenshot-Bezug | `02`, `10` |

Association-Anforderungen:

- Association Name mittig auf der Kante sichtbar.
- Source Class und Target Class sichtbar oder editierbar.
- Source Role und Target Role editierbar.
- Source Role und Target Role als Endlabels am Canvas sichtbar.
- Multiplizitaeten muessen fachlich modellierbar und als Endlabels am Canvas sichtbar sein.
- Association-Auswahl öffnet Association Properties.
- Association wird im Explorer unter `Associations` geführt.

## Rollen und Multiplizitäten

Rollen und Multiplizitäten sind Teil der Association Ends.

| Konzept | UI-Anforderung | Fachliche Bedeutung |
|---|---|---|
| Source Role | Eingabefeld oder Dropdown im Properties Panel/Modal; Anzeige als Endlabel am Source-Ende. | Rollenname für Lesbarkeit und ggf. OCL-Navigation. |
| Target Role | Eingabefeld oder Dropdown im Properties Panel/Modal; Anzeige als Endlabel am Target-Ende. | Wichtig für Ausdrücke wie `self.borrowedBooks`. |
| Source Multiplicity | Muss im MVP erfassbar und am Source-Ende sichtbar sein. | Struktur- und Linkvalidierung. |
| Target Multiplicity | Muss im MVP erfassbar und am Target-Ende sichtbar sein. | Struktur- und Linkvalidierung. |

Der Screenshot `10-modal-add-class-association.png` zeigt:

- Association Name,
- Source Class,
- Target Class,
- Source Role,
- Target Role.

Er zeigt keine Multiplizitätsfelder. Da Multiplizitäten MVP-Pflicht sind, gibt es drei mögliche UI-Entscheidungen:

| Option | Beschreibung | Bewertung |
|---|---|---|
| Im Add Association Modal ergänzen | Multiplizitäten werden direkt beim Erstellen gesetzt. | Fachlich vollständig, aber Dialog wird größer. |
| Nach Erstellung im Properties Panel bearbeiten | Modal bleibt klein, Association wird zunächst mit Defaults erzeugt. | Gut für Fokus, braucht klare Defaults. |
| Eigener Advanced-Bereich | Rollen sofort, Multiplizitäten aufklappbar. | Kompromiss, aber etwas komplexer. |

Empfehlung für MVP:

- Add Association Modal darf einfache Defaults setzen.
- Association Properties muss Multiplizitäten sichtbar und editierbar machen.
- Der Canvas muss Association-Endlabels anzeigen: Rolle und Multiplizitaet am Source-Ende sowie Rolle und Multiplizitaet am Target-Ende.
- Defaults müssen dokumentiert sein, z. B. `0..*` an beiden Enden.

## Invarianten

Invarianten sind OCL-Constraints im Kontext einer Klasse.

![Invariant Properties](../assets/screenshots/03-class-diagram-invariant-properties.png)

The consolidated redesign reference for listing, selecting, editing, validating
and deleting Invariants is `33-invariant-properties.md` with the mockup
`assets/mockups/invariant-properties.html`.

| Aspekt | Beschreibung |
|---|---|
| Zweck | OCL-Bedingung an eine Kontextklasse binden. |
| Daten | `id`, `name`, `contextClassId`, `OclExpression.text`, `enabled`. |
| Darstellung | Explorer-Eintrag und Referenz an der Kontextklasse, z. B. `inv: maxBooks`. |
| Nutzeraktionen | Invariante erstellen, auswählen, OCL-Ausdruck bearbeiten, ggf. löschen. |
| Backend-Bezug | OCL Parser, Typechecker, Evaluator, Validation Service. |
| Screenshot-Bezug | `03`, `09` |

UI-Anforderungen:

- Kontextklasse muss sichtbar sein.
- Invariant Name muss sichtbar und editierbar sein.
- OCL Expression muss sichtbar und editierbar sein.
- Invariante muss im Explorer auffindbar sein.
- Kontextklasse muss im Diagramm auf die Invariante verweisen.
- OCL-Fehler müssen später auf die Invariante und Source Range abbildbar sein.

## Explorer-Verhalten

Im Class Diagram zeigt die Explorer Sidebar die Modellstruktur.

| Gruppe | Inhalt | Aktionen |
|---|---|---|
| `Classes` | Alle `UmlClass`-Elemente. | Auswählen, neue Klasse erstellen. |
| `Associations` | Alle `UmlAssociation`-Elemente. | Auswählen, neue Association erstellen. |
| `Invariants` | Alle `UmlInvariant`-Elemente. | Auswählen, neue Invariante erstellen. |

Explorer-Anforderungen:

- Auswahl einer Klasse selektiert den ClassNode im Canvas.
- Auswahl einer Association selektiert die AssociationEdge.
- Auswahl einer Invariante selektiert die Invariante und zeigt Invariant Properties.
- `+`-Aktionen öffnen passende Modals.
- Explorer nutzt IDs für Selektion, Namen für Anzeige.
- Umbenennungen im Properties Panel aktualisieren Explorer-Einträge.

## Properties Panel

Das Properties Panel ist typabhängig.

### ClassPropertiesPanel

| Feld/Bereich | Zweck | MVP |
|---|---|---|
| Class Name | Klassenname anzeigen und bearbeiten. | Ja |
| Attributes | Attribute anzeigen, hinzufügen, bearbeiten. | Ja |
| Operations | Operationensignaturen anzeigen, hinzufügen, bearbeiten. | Ja |
| Add Attribute | Neues Attribut erstellen. | Ja |
| Add Operation | Neue Operationensignatur erstellen. | Ja |
| Segment `Class` | Allgemeine Klassendaten, Attribute und Operationen anzeigen. | Ja |
| Segment `Association` | Zugehörige Associations der selektierten Klasse anzeigen und auswählbar machen. | Ja |
| Segment `Invariant` | Zugehörige Invarianten der selektierten Klasse anzeigen und auswählbar machen. | Ja |

Die Screenshots `15-properties-association.png`, `16-properties-invariants.png` und `17-new-class.png` zeigen, dass eine Klassenselektion nicht auf reine Stammdaten beschränkt ist. Das Class Properties Panel ist der Einstiegspunkt in alle direkt zur Klasse gehörenden Modellbestandteile:

- Attribute und Operationen im Segment `Class`,
- Associations, bei denen die Klasse an einem Association-Ende beteiligt ist, im Segment `Association`,
- Invarianten mit `contextClassId` der selektierten Klasse im Segment `Invariant`.

Klickt der Nutzer in einem Related-Segment auf eine konkrete Association oder Invariante, wird diese fachlich selektiert und das dedizierte Properties Panel geöffnet.

### AssociationPropertiesPanel

| Feld/Bereich | Zweck | MVP |
|---|---|---|
| Association Name | Name anzeigen und bearbeiten. | Ja |
| Source Class | Source-Klasse anzeigen oder bearbeiten. | Ja |
| Target Class | Target-Klasse anzeigen oder bearbeiten. | Ja |
| Source Role | Rollenname anzeigen und bearbeiten. | Ja |
| Target Role | Rollenname anzeigen und bearbeiten. | Ja |
| Source Multiplicity | Kardinalität am Source-Ende bearbeiten. | Ja, auch wenn im Screenshot nicht sichtbar |
| Target Multiplicity | Kardinalität am Target-Ende bearbeiten. | Ja, auch wenn im Screenshot nicht sichtbar |

### InvariantPropertiesPanel

| Feld/Bereich | Zweck | MVP |
|---|---|---|
| Invariant Name | Invariantennamen anzeigen und bearbeiten. | Ja |
| Context Class | Kontextklasse anzeigen; ggf. änderbar. | Ja |
| OCL Expression | OCL-Ausdruck anzeigen und bearbeiten. | Ja |
| OCL Diagnostics | Syntax-/Typecheck-Feedback anzeigen. | Should |

Properties-Verhalten:

- Änderungen erzeugen Dirty State.
- Speichern oder Auto-Save-Verhalten muss später konkretisiert werden.
- Fehler aus Backend/API werden feldbezogen angezeigt, wenn möglich.
- Auswahlwechsel darf ungespeicherte Änderungen nicht still verlieren.

## Modale Dialoge

### AddClassModal

![Add Class Modal](../assets/screenshots/08-modal-add-class.png)

| Feld | Beschreibung |
|---|---|
| Class Name | Pflichtfeld für neuen Klassennamen. |
| Create Class | Erzeugt Klasse. |
| Close/Cancel | Bricht Erstellung ab. |

Erwartetes Verhalten:

- Klassenname darf nicht leer sein.
- Klassenname muss eindeutig sein.
- Nach Erstellung erscheint die Klasse im Explorer und Canvas.
- Neue Klasse ist direkt selektiert.

### AddAssociationModal

![Add Association Modal](../assets/screenshots/10-modal-add-class-association.png)

| Feld | Beschreibung |
|---|---|
| Association Name | Name der neuen Association. |
| Source Class | Auswahl existierender Klasse. |
| Target Class | Auswahl existierender Klasse. |
| Source Role | Rollenname am Source-Ende. |
| Target Role | Rollenname am Target-Ende. |
| Create Association | Erzeugt Association. |

Erwartetes Verhalten:

- Source und Target müssen existierende Klassen sein.
- Rollen dürfen nicht leer sein, wenn sie für OCL-Navigation verwendet werden.
- Multiplizitäten müssen im MVP entweder im Modal oder anschließend im Properties Panel gesetzt werden.
- Nach Erstellung erscheint die Association im Explorer und Canvas.

### AddInvariantModal

![Add Invariant Modal](../assets/screenshots/09-modal-add-invariant.png)

| Feld | Beschreibung |
|---|---|
| Context Class | Auswahl der Kontextklasse. |
| Invariant Name | Name der Invariante. |
| OCL Expression | OCL-Ausdruck der Invariante. |
| Add Invariant | Erzeugt Invariante. |

Erwartetes Verhalten:

- Context Class muss existieren.
- Invariant Name darf nicht leer sein.
- OCL Expression darf nicht leer sein.
- Nach Erstellung erscheint die Invariante im Explorer und an der Kontextklasse.
- OCL-Syntax-/Typecheck-Feedback kann direkt oder spätestens bei `Check Constraints` erscheinen.

## Nutzeraktionen

| Nutzeraktion | UI-Auslöser | Systemreaktion |
|---|---|---|
| Klasse auswählen | Klick auf ClassNode oder Explorer-Eintrag | ClassNode wird markiert, ClassPropertiesPanel öffnet. |
| Klasse erstellen | `+` bei Classes oder Add-Class-Aktion | AddClassModal öffnet; neue Klasse wird erzeugt. |
| Klasse bearbeiten | ClassPropertiesPanel | Canvas und Explorer aktualisieren sich. |
| Zugehörige Associations anzeigen | `Association`-Segment im ClassPropertiesPanel | Alle Associations der selektierten Klasse werden aufgelistet; Klick selektiert Association. |
| Zugehörige Invarianten anzeigen | `Invariant`-Segment im ClassPropertiesPanel | Alle Invarianten der selektierten Klasse werden aufgelistet; Klick selektiert Invariante. |
| Association auswählen | Klick auf Kante oder Explorer-Eintrag | AssociationEdge wird markiert, AssociationPropertiesPanel öffnet. |
| Association erstellen | `+` bei Associations | AddAssociationModal öffnet; neue Kante wird erzeugt. |
| Association bearbeiten | AssociationPropertiesPanel | Rollen, Klassen, Multiplizitäten und Labels aktualisieren sich. |
| Invariante auswählen | Explorer oder Klassenreferenz | InvariantPropertiesPanel öffnet. |
| Invariante erstellen | `+` bei Invariants | AddInvariantModal öffnet; Invariante wird erzeugt. |

Der kanonische Dialog für `Klasse erstellen` ist
`assets/mockups/create-class-modal.html`.
| OCL-Ausdruck bearbeiten | InvariantPropertiesPanel | OCL Text wird gespeichert oder als Dirty State geführt. |
| Layout ändern | Klasse verschieben | `LayoutInformation` wird aktualisiert. |
| Constraints prüfen | `Check Constraints` | Backend validiert; Fehler werden im Panel und ggf. Diagramm angezeigt. |

## Backend-Datenbedarf

Die Klassendiagramm-UI benötigt aus dem Backend mindestens:

| Datenobjekt | Benötigte Felder |
|---|---|
| `UmlModel` | `id`, `name`, `classes`, `associations`, `invariants`. |
| `UmlClass` | `id`, `name`, `attributes`, `operations`. |
| `UmlAttribute` | `id`, `name`, `type`, Reihenfolge. |
| `UmlOperation` | `id`, `name`, `parameters`, `returnType`, Reihenfolge. |
| `UmlAssociation` | `id`, `name`, `ends`. |
| `UmlAssociationEnd` | `id`, `classId`, `roleName`, `multiplicity`, `navigable`. |
| `Multiplicity` | `lower`, `upper`, `unbounded`, `raw`. |
| `UmlInvariant` | `id`, `name`, `contextClassId`, `expression`, `enabled`. |
| `LayoutInformation` | Positionen und ggf. Größen von Klassen und Kantenlabels. |
| `ValidationResult` | Fehler mit `modelElementIds`, `invariantId`, `sourceRange`. |

## API-Bezug

Mögliche API-Fähigkeiten für die Klassendiagramm-UI:

| API-Fähigkeit | Zweck |
|---|---|
| Projekt laden | Initiales `UmlModel` und Layout laden. |
| Klasse erstellen | Neue `UmlClass` erzeugen. |
| Klasse aktualisieren | Name, Attribute, Operationen ändern. |
| Klasse löschen | Klasse entfernen oder abhängigkeitsbewusst blockieren. |

The dedicated dependency-aware deletion workflow is specified in
`32-delete-class-modal.md`. Owned Features are removed with the Class, while
external model references and active snapshot instances require explicit
resolution.
| Association erstellen | Neue `UmlAssociation` mit Ends erzeugen. |
| Association aktualisieren | Rollen, Multiplizitäten und Enden ändern. |
| Association löschen | Association entfernen oder abhängigkeitsbewusst blockieren. |
| Invariante erstellen | Neue `UmlInvariant` erzeugen. |
| Invariante aktualisieren | Name, Kontext, OCL-Text ändern. |
| Invariante löschen | Invariante entfernen. |
| Layout speichern | Positionen und Diagrammzustand speichern. |
| OCL prüfen | Syntax-/Typecheck-Diagnostics liefern. |
| Constraints prüfen | Gesamtvalidierung auslösen. |

## State-Bezug

Lokaler UI-State ist notwendig, darf aber fachliche Daten nicht dauerhaft ersetzen.

| State | Zweck |
|---|---|
| `activeView` | Aktuell `Class Diagram`. |
| `selectedElementId` | Aktuelle Auswahl in Explorer/Canvas/Properties. |
| `selectedElementType` | Klasse, Association oder Invariante. |
| `openModal` | Aktives Create-Modal. |
| `formDrafts` | Noch nicht gespeicherte Properties-Eingaben. |
| `diagramViewport` | Pan/Zoom-Zustand. |
| `dragState` | Temporäre Position beim Verschieben. |
| `dirtyState` | Ungespeicherte Änderungen. |
| `validationResult` | Letztes Validierungsergebnis. |
| `highlightedElementIds` | Aus Fehlern abgeleitete Markierungen. |

Persistente fachliche Daten sollten aus Projekt/API-Daten stammen:

- Klassen,
- Attribute,
- Operationen,
- Associations,
- Invarianten,
- Layoutpositionen.

## Validierungsbezug

Das Klassendiagramm ist Ursprung vieler Validierungsfehler.

| Fehlerart | Bezug im Klassendiagramm | UI-Darstellung |
|---|---|---|
| Unbekannte Klasse | Association End, Invariante, Objektinstanz. | Properties-Fehler, Explorer-Badge. |
| Unbekanntes Attribut | OCL-Ausdruck oder Slot im Objektdiagramm. | Invariant Properties markieren. |
| Ungültige Multiplizität | Association End. | AssociationPropertiesPanel und ggf. Edge markieren. |
| OCL-Syntaxfehler | Invariante. | OCL-Ausdruck markieren. |
| OCL-Typefehler | Invariante, Attribut, Rolle. | Invariant Properties und ggf. Klasse/Association markieren. |
| Invariant-Verletzung | Invariante und Kontextklasse. | Invariant/Context sichtbar; konkrete Objekte im Object Diagram. |
| Multiplicity Violation | Association End. | Association markieren; konkrete Links/Objekte im Object Diagram. |

Wichtig:

- Nicht jede Validierungsverletzung wird direkt im Klassendiagramm sichtbar.
- Invariant- und Multiplicity-Verletzungen entstehen oft erst im Snapshot.
- Das Klassendiagramm muss trotzdem den Modellbestandteil zeigen, der den Constraint definiert.

## MVP-Anforderungen

| Bereich | MVP-Anforderung |
|---|---|
| Canvas | Klassen, Associations und Invariantenreferenzen anzeigen. |
| Selektion | Klasse, Association und Invariante auswählbar. |
| Explorer | Gruppen `Classes`, `Associations`, `Invariants`. |
| Class Properties | Name, Attribute, Operationen anzeigen und bearbeiten. |
| Class Related Access | Bei Klassenselektion Segmente für allgemeine Klassendaten, zugehörige Associations und zugehörige Invarianten anbieten. |
| Extension Placement | Neue Klassen- und Operationsfunktionen innerhalb `Class`, neue Association-Funktionen innerhalb `Association` und Klasseninvarianten innerhalb `Invariant` anordnen; keine zusätzlichen gleichrangigen Properties-Hauptseiten einführen. |
| Association Properties | Name, Source/Target Class, Rollen und Multiplizitäten bearbeiten. |
| Association Canvas Labels | Association-Name mittig; Rollen und Multiplizitaeten an beiden Enden anzeigen. |
| Invariant Properties | Name, Kontextklasse, OCL-Ausdruck bearbeiten. |
| Add Class | Modal für neue Klasse. |
| Add Association | Modal für Klassenassoziation. |
| Add Invariant | Modal für Invariante. |
| Layout | Klassenpositionen speichern. |
| Backend | Änderungen über API oder Projektzustand persistieren. |
| Validierung | OCL-/Modellfehler auf relevante Elemente mappen. |

## Post-MVP-Erweiterungen

| Erweiterung | Nutzen |
|---|---|
| Inline-Attribut-/Operationserstellung | Weniger Modals, schnelleres Modellieren. |
| Rich OCL Editor im Properties Panel | Syntax Highlighting und Autocomplete. |
| Auto-Layout | Bessere Lesbarkeit größerer Modelle. |
| Undo/Redo | Sicheres Experimentieren beim Modellieren. |
| Vererbung im Diagramm | Generalization Edges. |
| Enumerationen | Eigene Knoten oder Typbereiche. |
| Aggregation/Komposition | Spezielle Association-End-Notation. |
| Assoziationsklassen | Kombination aus Edge und ClassNode. |
| Fehlernavigation | Klick auf Validation Result fokussiert Modellbestandteil im Class Diagram. |
| Refactoring-Unterstützung | Umbenennen mit OCL-Ausdrucksaktualisierung. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Wie werden Association-Endlabels bei sehr kurzen Kanten ohne Überlappung positioniert? | Rollen und Multiplizitaeten sind MVP-Pflicht am Canvas; offen ist nur das konkrete Collision-/Overflow-Verhalten. |
| Wie werden Attribute und Operationen hinzugefügt: inline oder per Modal? | Betrifft Properties Panel UX. |
| Darf eine Invariante im Add Modal syntaktisch ungültig gespeichert werden? | Betrifft OCL-Feedback und Workflow. |
| Wird der OCL Editor mit Invariant Properties synchronisiert? | Betrifft Navigation und State. |
| Wie werden Klassen gelöscht, wenn Objekte oder Invarianten darauf verweisen? | Betrifft Backend-Regeln und UI-Fehler. |
| Wie wird Layout gespeichert: automatisch, per Save oder als Teil jedes Updates? | Betrifft API und Dirty State. |
| Welche Diagrammbibliothek wird verwendet? | Betrifft Canvas-Verhalten, Kantenlabels und Layoutspeicherung. |

## Zusammenfassung

Die Klassendiagramm-UI ist der zentrale Modellierungsbereich des Systems. Sie zeigt und bearbeitet die fachliche Struktur, auf der Objektdiagramm, OCL-Typechecking und Constraint Validation aufbauen.

Für den MVP muss sie Klassen, Attribute, Operationensignaturen, Associations, Rollen, Multiplizitäten und Invarianten darstellen und bearbeitbar machen. Explorer, Canvas und Properties Panel müssen synchron arbeiten. Modals unterstützen fokussierte Erstellung von Klassen, Associations und Invarianten. Validierungsergebnisse müssen auf die Modellbestandteile des Klassendiagramms zurückführbar sein, auch wenn konkrete Verletzungen häufig im Objektdiagramm sichtbar werden.
