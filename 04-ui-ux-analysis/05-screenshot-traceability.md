# Screenshot Traceability

## Zweck dieser Datei

Diese Datei dokumentiert die Nachverfolgbarkeit zwischen den UI-Screenshots, den daraus abgeleiteten Nutzeraktionen, funktionalen Anforderungen, Frontend-Komponenten, Backend-Funktionen, API-Daten und MVP-Entscheidungen.

Sie dient als Brücke zwischen visueller Referenz und späterer Umsetzung. Entwickler, Betreuer und KI-Agenten können anhand dieser Datei nachvollziehen, warum eine Anforderung existiert, welcher Screenshot sie stützt und welche Systembereiche davon betroffen sind.

Wichtig: Die Screenshots sind keine pixelgenaue Spezifikation. Sie sind strukturelle und funktionale Referenzen für User Journey, UI-Aufbau, MVP-Scope und Akzeptanzkriterien.

## Traceability-Prinzip

Traceability wird in diesem Analyse-Repository entlang folgender Kette verstanden:

```text
Screenshot
-> sichtbarer UI-Bereich
-> Nutzeraktion
-> erwartete Systemreaktion
-> funktionale Anforderung
-> Frontend-Komponente
-> API-Vertrag
-> Backend-Service
-> Validierungsbezug
-> MVP- oder Post-MVP-Entscheidung
```

Jede UI-Annahme sollte mindestens auf eine der folgenden Quellen zurückführbar sein:

- Screenshot,
- Original-USE-Referenz,
- fachliche UML/OCL-Ableitung,
- MVP-Scope-Entscheidung,
- technische Architekturentscheidung.

## Screenshot-Übersicht

Hinweis zur Ablage: Alle berücksichtigten Screenshots liegen unter `assets/screenshots/`. Die Bildlinks in dieser Datei verwenden diese normalisierten Pfade.

| Nr. | Datei im Zielkontext | Aktueller relativer Pfad | Kurztitel | Primärer UI-Bereich | MVP-Relevanz |
|---|---|---|---|---|---|
| 00 | `00-dashboard-start-page.png` | `../assets/screenshots/00-dashboard-start-page.png` | Dashboard Start Page | Dashboard, Projektstart, Recent Projects, Learn & Support | Hoch |
| 18 | `18-create-new-projects.png` | `../assets/screenshots/18-create-new-projects.png` | Create New Project | Projektstart, Projektname, Create Project | Hoch |
| 14 | `14-open-existing-project.png` | `../assets/screenshots/14-open-existing-project.png` | Open Existing Project | Modal, lokaler `.use` Import, Datei-Upload, Importdiagnosen | Hoch/Should |
| 19 | `19-projects.png` | `../assets/screenshots/19-projects.png` | All Projects | Projektliste, Suche, Filter, Projektkarten | Should |
| 01 | `01-class-diagram-class-properties.png` | `../assets/screenshots/01-class-diagram-class-properties.png` | Class Properties | Class Diagram, Explorer, Properties Panel | Hoch |
| 02 | `02-class-diagram-association-properties.png` | `../assets/screenshots/02-class-diagram-association-properties.png` | Association Properties | Class Diagram, Association Properties | Hoch |
| 03 | `03-class-diagram-invariant-properties.png` | `../assets/screenshots/03-class-diagram-invariant-properties.png` | Invariant Properties | Class Diagram, Invariant Properties | Hoch |
| 04 | `04-class-diagram-new-class-selected.png` | `../assets/screenshots/04-class-diagram-new-class-selected.png` | New Class Selected | Class Diagram, Class Properties | Hoch |
| 06 | `06-object-diagram-object-properties.png` | `../assets/screenshots/06-object-diagram-object-properties.png` | Object Properties | Object Diagram, Object Properties | Hoch |
| 07 | `07-object-diagram-validation-error.png` | `../assets/screenshots/07-object-diagram-validation-error.png` | Validation Error | Object Diagram, Validation Results | Hoch |
| 08 | `08-modal-add-class.png` | `../assets/screenshots/08-modal-add-class.png` | Add Class Modal | Modal, Class Diagram | Hoch |
| 09 | `09-modal-add-invariant.png` | `../assets/screenshots/09-modal-add-invariant.png` | Add Invariant Modal | Modal, OCL/Invariants | Hoch |
| 10 | `10-modal-add-class-association.png` | `../assets/screenshots/10-modal-add-class-association.png` | Add Class Association Modal | Modal, Class Diagram | Hoch |
| 11 | `11-modal-add-object-association.png` | `../assets/screenshots/11-modal-add-object-association.png` | Add Object Association Modal | Modal, Object Diagram | Hoch |
| 12 | `12-object-diagram-association-properties.png` | `../assets/screenshots/12-object-diagram-association-properties.png` | Object Association Properties | Object Diagram, Link Properties | Hoch |
| 13 | `13-ocl-editor.png` | `../assets/screenshots/13-ocl-editor.png` | OCL Editor | textueller Modell-/OCL-Editor, Apply Changes, Console | Hoch |
| 15 | `15-properties-association.png` | `../assets/screenshots/15-properties-association.png` | Class Related Associations | Class Diagram, Class Properties Segment `Association` | Hoch |
| 16 | `16-properties-invariants.png` | `../assets/screenshots/16-properties-invariants.png` | Class Related Invariants | Class Diagram, Class Properties Segment `Invariant` | Hoch |
| 17 | `17-new-class.png` | `../assets/screenshots/17-new-class.png` | New Class Properties Segment | Class Diagram, Class Properties Segment `Class` | Hoch |

Hinweis: `13-ocl-editor.png` liegt vor und belegt die OCL Editor View als textbasierte Modell-/OCL-Arbeitsansicht.

### Vorschauen

![Dashboard Start Page](../assets/screenshots/00-dashboard-start-page.png)

![Create New Project](../assets/screenshots/18-create-new-projects.png)

![Open Existing Project](../assets/screenshots/14-open-existing-project.png)

![All Projects](../assets/screenshots/19-projects.png)

![Class Diagram Class Properties](../assets/screenshots/01-class-diagram-class-properties.png)

![Object Diagram Validation Error](../assets/screenshots/07-object-diagram-validation-error.png)

![Add Invariant Modal](../assets/screenshots/09-modal-add-invariant.png)

![OCL Editor](../assets/screenshots/13-ocl-editor.png)

![Class Properties Association Segment](../assets/screenshots/15-properties-association.png)

![Class Properties Invariant Segment](../assets/screenshots/16-properties-invariants.png)

## Traceability-Matrix

| Screenshot | Kurzbeschreibung | Sichtbarer UI-Bereich | Nutzeraktion | Systemreaktion | Abgeleitete Anforderungen | Frontend-Komponenten | Backend-Funktionen | Benötigte API-Daten | Validierungsbezug | MVP-Relevanz | Offene Fragen |
|---|---|---|---|---|---|---|---|---|---|---|---|
| `00-dashboard-start-page.png` | Dashboard mit USE-Logo, `Create New Model`, `Open Existing`, Recent Projects und Learn & Support. | Dashboard / Start Page, Header, Projektstartkarten, Recent Projects, Support Links. | `Start Project` klicken, `Open Existing` wählen, Recent Project öffnen, Documentation oder Examples öffnen. | `Start Project` öffnet den Create-New-Project-Dialog; andere Aktionen öffnen Import-/Projektlisten- oder Support-Flows. | `FR-DASH-001`, `FR-DASH-002`, `FR-DASH-002A`, `FR-DASH-003`, `FR-PROJECT-IMPORT-001`, `FR-RECENT-001`, `FR-RECENT-002`, `FR-SUPPORT-001`, `FR-SUPPORT-002`. | `DashboardPage`, `CreateNewModelCard`, `CreateNewProjectModal`, `OpenExistingCard`, `RecentProjectsList`, `RecentProjectCard`, `LearnSupportSection`, `DocumentationLink`, `ExamplesLink`. | Project Service, Project Import Service, Recent Projects API, Example Project Service. | Projektmetadaten, Recent Project Summaries, `CreateProjectRequestDto`, Import Result. | Kein direkter Constraint-Bezug; Einstieg in Projektzustand, der später validiert wird. | Hoch für `Start Project`; Should für Recent Projects und `.use` Import. | Sind Recent Projects echte Backend-Daten oder zunächst Mock-/Demo-Daten? Ist `.use` Import im MVP aktiv oder nur sichtbar? |
| `18-create-new-projects.png` | Dialog/Formular zum Erstellen eines neuen Projekts mit Projektname. | Dashboard Overlay oder Create-New-Project-Formular, Projektname-Feld, Submit und Cancel/Close. | Nutzer gibt einen Projektnamen ein und bestätigt die Projektanlage oder bricht ab. | Frontend validiert Pflichtfeld, ruft `POST /api/v1/projects` auf und navigiert bei Erfolg ins Klassendiagramm. | `FR-DASH-002`, `FR-DASH-002A`, `FR-PROJ-001`, `AC-DASH-002`, `AC-DASH-002A`, `AC-DASH-002B`, `AC-DASH-003`. | `CreateNewProjectModal`, `ProjectNameInput`, `CreateProjectButton`, `CancelButton`, `FormErrorMessage`. | Project Service, Persistence Service. | `CreateProjectRequestDto.name`, `ProjectDto.project.id`, `ProjectDto.project.name`, `ApiErrorDto`. | Kein direkter Constraint-Bezug; Projektname ist Projektmetadatum und erscheint später in Workspace/Recent Projects. | Hoch | Welche Namensregeln gelten neben nicht leer: Eindeutigkeit, maximale Länge, erlaubte Zeichen? |
| `14-open-existing-project.png` | Modal `Open Existing Project` für lokale `.use`-Datei. | Dashboard Overlay, Modal Layer, `Local File`, Upload-/Drag-and-drop-Fläche, `Cancel`, `Open Project`. | Nutzer klickt `Open Existing`, wählt oder droppt eine `.use`-Datei und klickt `Open Project`. | Frontend liest Dateiinhalt als Modelltext, ruft Import-/Apply-Flow auf und zeigt Projekt oder Importdiagnosen. | `FR-PROJECT-IMPORT-001`, `FR-PROJECT-IMPORT-002`, `FR-PROJECT-IMPORT-003`, `FR-PROJECT-IMPORT-004`, `FR-IO-004`, `AC-IMPORT-002` bis `AC-IMPORT-005`. | `OpenExistingProjectModal`, `FileDropZone`, `LocalFileTab`, `ImportDiagnosticsList`, `OpenProjectButton`, `ConsolePanel`. | Project Import Service, Project Service, Model Text Parser/Importer, OCL Parser/Typechecker, später `UseImportService`. | Dateiinhalt, `ApplyModelTextRequestDto` oder `ImportUseRequestDto`, `ImportResultDto`, Diagnostics, optional `ProjectDto`. | Direkt für Import-/Syntax-/Typecheck-Diagnosen; indirekt für spätere Constraint-Validierung. | Hoch/Should: UI ist sichtbar; vollständige USE-Kompatibilität bleibt Post-MVP. | Soll bei Diagnosen das Modal offen bleiben oder direkt der OCL Editor mit Modelltext geöffnet werden? |
| `19-projects.png` | Seite `All Projects` mit Suchfeld, `Filter`, `Open`, `+ New Project` und Projektkarten. | Projektliste / All Projects, Header, Search, Filter, Actions, Project Cards. | Nutzer sucht Projekte, öffnet ein Projekt, startet ein neues Projekt oder kehrt zurück. | Frontend lädt Project Summaries, filtert/sucht lokal oder serverseitig und navigiert bei Projektauswahl in den Workspace. | `FR-RECENT-003`, `FR-PROJ-007`, `FR-PROJ-008`, `FR-PROJ-009`, `FR-PROJ-010`, `FR-PROJ-011`, `AC-PROJECTS-001` bis `AC-PROJECTS-006`. | `ProjectsPage`, `ProjectSearchInput`, `ProjectFilterButton`, `ProjectCardGrid`, `ProjectListCard`, `ProjectListActions`, `CreateNewProjectModal`. | Project Service, Project List API, Persistence Service. | `ProjectSummaryDto[]`, Such-/Filterparameter, `ProjectDto`, `CreateProjectRequestDto`. | Kein direkter Constraint-Bezug; lädt Projektzustände, die später im Workspace validiert werden. | Should; wichtig für Dashboard-`View all`, nicht blockierend für Kernvalidierung. | Sind Suche und Filter im MVP clientseitig oder serverseitig? Gibt es Pagination bei vielen Projekten? |
| `01-class-diagram-class-properties.png` | Klassendiagramm mit `Book`, `User`, Association `Borrows`, Explorer, Properties Panel für Klasse. | Top Bar, Class Diagram View, Explorer Sidebar, Diagram Canvas, Properties Panel, Console, Validation Results, Quick Help. | Nutzer wählt eine Klasse im Diagramm oder Explorer aus. | Klasse wird selektiert; Properties Panel zeigt Klassendaten, Attribute und Operationen. | `FR-CLASS-002`, `FR-CLASS-005`, `FR-CLASS-006`, `FR-CLASS-009`, `FR-CLASS-017`, `FR-UI-001`. | `ClassDiagramView`, `ExplorerSidebar`, `ClassNode`, `ClassPropertiesPanel`, `BottomPanel`. | Project Service, UML Model Service, Layout Service. | Projekt-ID, Klassen, Attribute, Operationen, Assoziationen, Invarianten, Layoutpositionen. | Indirekt: Klassen, Attribute und Operationen sind Grundlage für OCL-Typechecking und Snapshot-Validierung. | Hoch | Welche Attribute und Operationen sind direkt im Panel editierbar und welche nur in Detaildialogen? |
| `02-class-diagram-association-properties.png` | Association ist ausgewählt; Properties Panel zeigt Association Name, Source/Target Class und Source/Target Role. | Class Diagram Canvas, Association Edge, Properties Panel, Explorer Associations. | Nutzer wählt eine Association aus oder bearbeitet deren Eigenschaften. | Association-Kante wird selektiert; Properties Panel zeigt Enden, Klassen und Rollen. | `FR-CLASS-012`, `FR-CLASS-013`, `FR-CLASS-014`, `FR-CLASS-015`, `FR-CLASS-018`. | `AssociationEdge`, `AssociationPropertiesPanel`, `ExplorerSidebar`. | UML Model Service, Association Service, Validation Service. | Association-ID, Name, Source/Target Class IDs, Rollen, Multiplizitäten, Layoutdaten. | Rollen und Multiplizitäten sind relevant für Navigation und Multiplicity Checks. | Hoch | Multiplizitäten sind im Screenshot nicht klar sichtbar; sie müssen im MVP dennoch modelliert werden. |
| `03-class-diagram-invariant-properties.png` | Invariante `maxBooks` ist ausgewählt; Panel zeigt Invariant Name und OCL Expression. | Class Diagram View, Invariant im Explorer, Invariant Properties Panel. | Nutzer wählt oder bearbeitet eine OCL-Invariante. | Invariante wird selektiert; Ausdruck ist sichtbar und editierbar. | `FR-OCL-001`, `FR-OCL-002`, `FR-OCL-004`, `FR-OCL-013`, `FR-OCL-014`, `UI-OCL-002`, `UI-OCL-003`. | `InvariantBadge`, `InvariantPropertiesPanel`, `OclExpressionInput`, `ExplorerSidebar`. | OCL Service, UML Model Service, Validation Service. | Invariant-ID, Kontextklasse, Name, OCL Expression, OCL-Diagnosen. | Direkt: OCL-Ausdruck wird später gegen Snapshots geprüft. | Hoch | Soll der Kontext im Properties Panel explizit editierbar sein oder nur im Modal? |
| `04-class-diagram-new-class-selected.png` | Neu erstellte Klasse `Libary` ist selektiert und im Canvas sichtbar. | Class Diagram Canvas, Class Properties Panel, Explorer Classes. | Nutzer erstellt eine Klasse oder wählt eine neue Klasse aus. | Neue Klasse erscheint im Diagramm, Explorer und Properties Panel. | `FR-CLASS-001`, `FR-CLASS-002`, `FR-CLASS-004`, `FR-CLASS-017`, `FR-UI-002`. | `AddClassModal`, `ClassNode`, `ClassPropertiesPanel`, `ExplorerSidebar`. | UML Model Service, Project Service, Layout Service. | Class-ID, Name, Attribute, Operationen, Position, Selection State. | Indirekt: neue Klassen sind mögliche Kontextklassen für Invarianten und Typen für Objekte. | Hoch | Screenshot zeigt `Libary`; es ist offen, ob Schreibfehler lokal akzeptiert oder über Naming-Regeln geprüft werden. |
| `06-object-diagram-object-properties.png` | Objektdiagramm mit `mobyDick : Book` und `alice : User`; Properties Panel zeigt Objekt und Slots. | Object Diagram View, Object Node, Object Properties Panel, Explorer Objects. | Nutzer wechselt ins Objektdiagramm und wählt ein Objekt aus. | Objekt wird selektiert; Slots/Attributwerte werden angezeigt und bearbeitbar. | `FR-OBJ-001`, `FR-OBJ-002`, `FR-OBJ-003`, `FR-OBJ-005`, `FR-OBJ-006`, `FR-OBJ-012`. | `ObjectDiagramView`, `ObjectNode`, `ObjectPropertiesPanel`, `SlotEditor`, `ExplorerSidebar`. | Object Model Service, Snapshot Service, UML Model Service. | Object-ID, Objektname, Class-ID, Slots, Attributdefinitionen, Layoutposition. | Direkt: Slot-Werte fließen in Invarianten und Typvalidierung ein. | Hoch | Gibt es im MVP ein separates Add-Object-Modal, obwohl es nicht im Screenshot sichtbar ist? |
| `07-object-diagram-validation-error.png` | Nach Constraint Check ist `alice : User` rot markiert; Validation Results zeigt `1 Error` mit Invariante `maxBooks`. | Object Diagram View, Validation Results Panel, Fehler-Badge, Top Bar Check Constraints. | Nutzer klickt `Check Constraints`. | Backend-validierte Fehler erscheinen im Panel und markieren betroffene Objekte. | `FR-VAL-001`, `FR-VAL-004`, `FR-VAL-005`, `FR-VAL-006`, `FR-ERR-001`, `FR-ERR-002`, `UI-OCL-004`, `UI-OCL-005`, `UI-OCL-006`. | `CheckConstraintsButton`, `ValidationResultsPanel`, `ValidationErrorItem`, `ValidationBadge`, `InvalidObjectHighlight`, `ObjectNode`. | Validation Service, OCL Engine, Object Model Service, Multiplicity Validator. | ValidationResult, Fehlercode, Severity, Message, Invariant-ID, Objekt-ID, Kontextklasse, Expression. | Zentral: Invariantverletzung und visuelle Fehlerdarstellung sind MVP-Kern. | Hoch | Muss ein Klick auf den Fehler automatisch ins Objektdiagramm wechseln und das Element zentrieren? |
| `08-modal-add-class.png` | Modal `Add New Class` mit Class Name, Attributes und Operations. | Modal Layer, Class Diagram Kontext. | Nutzer legt eine neue Klasse mit Eigenschaften an. | Nach Bestätigung wird Klasse erstellt und im Modell sichtbar. | `FR-CLASS-001`, `FR-CLASS-004`, `FR-CLASS-005`, `FR-CLASS-006`, `FR-CLASS-009`. | `AddClassModal`, `AttributeEditor`, `OperationSignatureEditor`, `ClassDiagramView`. | UML Model Service, Project Service. | Klassenname, Attribute mit Typen, Operationen als Signaturen. | Indirekt: Attribute und Operationen beeinflussen Typechecking und Objekt-Slots. | Hoch | Wie werden mehrere Attribute/Operationen im Modal hinzugefügt, sortiert oder gelöscht? |
| `09-modal-add-invariant.png` | Modal `Add Invariant` mit Context Class, Invariant Name und OCL Expression. | Modal Layer, OCL/Invariants Kontext. | Nutzer erstellt eine neue Invariante. | Invariante wird gespeichert, im Explorer sichtbar und der Kontextklasse zugeordnet. | `FR-OCL-001`, `FR-OCL-004`, `FR-OCL-005`, `FR-OCL-013`, `FR-OCL-014`, `UI-OCL-001`. | `AddInvariantModal`, `OclExpressionInput`, `OclFeedbackMessage`, `ExplorerSidebar`. | OCL Service, UML Model Service, Validation Service. | Kontextklassen, Invariant Name, Expression, optionale Syntaxdiagnosen. | Direkt: Invariante ist Prüfgegenstand für `Check Constraints`. | Hoch | Wird OCL beim Speichern syntaktisch geprüft oder erst beim Constraint Check? |
| `10-modal-add-class-association.png` | Modal `Add Association` für Klassenassoziation mit Name, Source/Target Class und Rollen. | Modal Layer, Class Diagram Kontext. | Nutzer erstellt eine Klassenassoziation. | Association erscheint als Kante und im Explorer; Rollen werden für Navigation verfügbar. | `FR-CLASS-012`, `FR-CLASS-013`, `FR-CLASS-014`, `FR-CLASS-015`, `FR-CLASS-018`. | `AddAssociationModal`, `AssociationEdge`, `AssociationPropertiesPanel`. | Association Service, UML Model Service, Validation Service. | Association Name, Source/Target Class IDs, Rollen, Multiplizitäten. | Direkt: Association-Navigation und Multiplicity Checks hängen davon ab. | Hoch | Multiplizitätsfelder fehlen im Screenshot; Scope muss entscheiden, ob sie im Modal oder Properties Panel erfasst werden. |
| `11-modal-add-object-association.png` | Historisches binäres Modal `Add Association`; aktuelle Zielausprägung ist `Create Object Link`. | Modal Layer, Object Diagram Kontext. | Nutzer erstellt einen Objektlink über die endbasierte Belegung aus `29-create-object-link-modal.md`. | Link erscheint im Objektdiagramm und wird dem Snapshot hinzugefügt. | `FR-OBJ-008`, `FR-OBJ-009`, `FR-OBJ-011`, `FR-VAL-002`, `FR-VAL-003`. | `CreateObjectLinkModal`, `ObjectLinkEdge`, `ObjectDiagramView`. | Object Model Service, Snapshot Service, Association Compatibility Check. | Association ID, geordnete Endbelegungen, Qualifierwerte, Link-ID und Revision. | Direkt: Endtypen, Qualifier, Link-Kompatibilität und Multiplizitäten werden validiert. | Hoch | Screenshot bleibt visuelle Bestandsreferenz; die aktuelle endbasierte Struktur steht in `assets/mockups/create-object-link-modal.html`. |
| `12-object-diagram-association-properties.png` | Historische binäre Object-Link-Properties; aktuelle Zielausprägung liegt in `Object Properties -> Associations`. | Object Diagram Canvas, Object Link Edge, Object Properties. | Nutzer wählt oder bearbeitet einen Objektlink. | Link wird selektiert; Properties zeigt modellierte Association, alle Endbelegungen und Qualifierwerte. | `FR-OBJ-008`, `FR-OBJ-009`, `FR-OBJ-010`, `FR-OBJ-011`, `FR-VAL-003`. | `ObjectLinkEdge`, `ObjectPropertiesPanel`, `ExplorerSidebar`. | Object Model Service, Validation Service, UML Model Service. | Link-ID, Association-ID, endbasierte Objektzuordnungen, Qualifierwerte, Validierungsstatus. | Direkt: ungültige Links und Multiplicity Violations müssen auf Link/Objekt mappbar sein. | Hoch | Verbindliche Zielstruktur: `assets/mockups/object-link-association-sidebar.html`. |
| `13-ocl-editor.png` | OCL Editor mit dunkler, zeilenbasierter Editorfläche für vollständigen USE-ähnlichen Modelltext. | OCL Editor Tab, Texteditor, `Apply Changes`, `Check Constraints`, Refresh/Save, Bottom Panel mit Console und Validation Results. | Nutzer öffnet den `OCL Editor`, bearbeitet den gesamten Modelltext und klickt `Apply Changes` oder `Check Constraints`. | Der gesamte Modelltext wird angewendet; Backend-Diagnosen/Validierungsergebnisse werden angezeigt; Console protokolliert Lade- und Änderungsereignisse. | `FR-OCL-016`, `FR-OCL-017`, `FR-OCL-018`, `FR-OCL-019`, `FR-INT-005`, `AC-OCL-001` bis `AC-OCL-005`. | `OclEditorView`, `ModelTextEditor`, `LineNumberGutter`, `ApplyChangesButton`, `CheckConstraintsButton`, `ConsolePanel`, `ValidationResultsPanel`. | OCL Service, Model Text Parser/Importer, OCL Parser, OCL Typechecker, Validation Service, Project Service. | Vollständiger Projekttext, `UmlModelDto`, `UmlInvariantDto`, `UmlClassDto`, Parse-/Typecheck-Diagnosen, `ValidationErrorDto`, Console Events. | Direkt: Syntax-, Typ- und Invariantfehler müssen auf Textpositionen und Modell-IDs gemappt werden. | Hoch | MVP verarbeitet nur den definierten UML/OCL-Subset; nicht unterstützte `.use`-Syntax aus Beispielen wird strukturiert diagnostiziert. |
| `15-properties-association.png` | Klasse ist selektiert; Properties Panel zeigt Segment `Association` mit zugehöriger Association `Borrows`. | Class Diagram Canvas, Explorer, selektierter Class Node, Properties Panel mit Segment Control. | Nutzer klickt bei selektierter Klasse auf das Segment `Association` oder wählt eine gelistete Association aus. | Related Associations der Klasse werden angezeigt; Klick auf eine Association selektiert diese und öffnet Association Properties. | `FR-CLASS-012`, `FR-CLASS-017`, `FR-UI-001`, `PROP-MVP-014`, `PROP-MVP-015`. | `ClassPropertiesPanel`, `ClassPropertiesSegmentControl`, `RelatedAssociationList`, `AssociationPropertiesPanel`, `ExplorerSidebar`. | UML Model Service, Project Service. | `UmlClassDto`, `UmlAssociationDto`, `UmlAssociationEndDto`, `MultiplicityDto`, stabile IDs. | Associations sind Grundlage für OCL-Navigation, Object Links und Multiplizitätsvalidierung. | Hoch | Soll die Klasse visuell selektiert bleiben, während eine Association im Related-Segment nur fokussiert wird, oder wechselt die fachliche Selektion sofort zur Association? |
| `16-properties-invariants.png` | Klasse ist selektiert; Properties Panel zeigt Segment `Invariant` mit Invariante und OCL-Ausdruck. | Class Diagram Canvas, Explorer, selektierter Class Node, Properties Panel mit Invariant-Segment. | Nutzer klickt bei selektierter Klasse auf `Invariant` oder wählt eine Invariante aus. | Invarianten der Kontextklasse werden angezeigt; Klick auf eine Invariante selektiert diese und öffnet Invariant Properties. | `FR-OCL-001`, `FR-OCL-002`, `FR-OCL-004`, `FR-UI-001`, `PROP-MVP-014`, `PROP-MVP-016`. | `ClassPropertiesPanel`, `ClassPropertiesSegmentControl`, `RelatedInvariantList`, `InvariantPropertiesPanel`, `OclExpressionPreview`. | UML Model Service, OCL Service, Validation Service. | `UmlClassDto`, `UmlInvariantDto`, OCL Expression, `contextClassId`. | Invarianten sind direkte Eingabe für OCL-Typechecking und Constraint Validation. | Hoch | Soll das Invariant-Segment den OCL-Ausdruck nur anzeigen oder bereits inline editierbar machen? |
| `17-new-class.png` | Neue Klasse `Libary` ist selektiert; Properties Panel zeigt Segment `Class` mit Name, Attributen, Operationen und Add-Aktionen. | Class Diagram Canvas, Explorer, Class Properties Panel, Console. | Nutzer erstellt eine Klasse und ergänzt anschließend Attribute oder Operationen. | Neue Klasse wird im Canvas selektiert; Properties Panel bietet direkten Zugriff auf Name, Attribute, Operationen und Add-Aktionen. | `FR-CLASS-001`, `FR-CLASS-004`, `FR-CLASS-005`, `FR-CLASS-006`, `FR-CLASS-009`, `PROP-MVP-014`. | `AddClassModal`, `ClassNode`, `ClassPropertiesPanel`, `AttributeEditor`, `OperationSignatureEditor`, `ExplorerSidebar`. | UML Model Service, Project Service, Layout Service. | `CreateClassRequestDto`, `UmlClassDto`, `UmlAttributeDto`, `UmlOperationDto`, Layoutposition, Selection State. | Neue Klasse kann später Kontextklasse für Invarianten oder Typ für Objekte sein. | Hoch | Soll `Libary` als freier Nutzername akzeptiert werden oder war dies ein Tippfehler, der durch Naming-Regeln geprüft werden soll? |

## Abgeleitete MVP-Anforderungen

| Bereich | Abgeleitete MVP-Anforderung | Screenshot-Quelle | Primäre FR-Referenzen |
|---|---|---|---|
| Dashboard | Anwendung startet mit Dashboard; Nutzer kann den Projektstart öffnen. | `00` | `FR-DASH-001`, `FR-DASH-002` |
| Projektstart | Start Project erfasst einen Projektnamen und führt erst nach erfolgreicher Anlage ins Klassendiagramm; Open Existing ist sichtbar und öffnet ein Importmodal. | `00`, `18`, `14` | `FR-DASH-002A`, `FR-DASH-003`, `FR-PROJ-001`, `FR-PROJ-002`, `FR-PROJECT-IMPORT-001` |
| `.use` Import | Lokale `.use`-Datei kann ausgewählt, als Modelltext übernommen und diagnostiziert werden. | `14`, `13` | `FR-PROJECT-IMPORT-002`, `FR-PROJECT-IMPORT-003`, `FR-PROJECT-IMPORT-004`, `FR-IO-004` |
| Recent/Support | Recent Projects, Documentation und Examples sind als Einstiege sichtbar. | `00` | `FR-RECENT-001`, `FR-SUPPORT-001`, `FR-SUPPORT-002` |
| Projektliste | `View all` öffnet eine All-Projects-Seite mit Projektkarten, Suche und New-Project-Einstieg. | `19` | `FR-RECENT-003`, `FR-PROJ-007`, `FR-PROJ-008`, `FR-PROJ-010`, `FR-PROJ-011` |
| Projektzustand | Projekt muss Klassenmodell, OCL-Invarianten, Snapshot und Layout gemeinsam laden/speichern. | `01`, `06`, `07` | `FR-PROJ-002`, `FR-PROJ-003`, `FR-PROJ-005` |
| Klassendiagramm | Klassen müssen erstellt, selektiert, angezeigt und bearbeitet werden. | `01`, `04`, `08` | `FR-CLASS-001`, `FR-CLASS-002`, `FR-CLASS-017` |
| Class Properties Segmente | Bei selektierter Klasse müssen allgemeine Klassendaten, zugehörige Associations und zugehörige Invarianten erreichbar sein. | `15`, `16`, `17` | `PROP-MVP-014`, `PROP-MVP-015`, `PROP-MVP-016` |
| Erweiterungsregel für Properties | Spätere UML-/OCL-Funktionen werden in `Class`, `Association` oder `Invariant` eingeordnet. Neue gleichrangige Hauptseiten sind nicht vorgesehen; Operationsdetails dürfen innerhalb von `Class` eigene Untersegmente besitzen. | `15`, `16`, `17` | Mockup-Roadmap M2 bis M9 |
| Attribute | Attribute mit primitiven Typen müssen angezeigt und bearbeitet werden. | `01`, `08` | `FR-CLASS-005`, `FR-CLASS-006`, `FR-CLASS-007` |
| Operationen | Operationen werden als Signaturen erfasst und angezeigt. | `01`, `08` | `FR-CLASS-009` |
| Klassenassoziationen | Associations mit Rollen und Multiplizitäten müssen modellierbar sein. | `02`, `10` | `FR-CLASS-012`, `FR-CLASS-013`, `FR-CLASS-014`, `FR-CLASS-015` |
| OCL-Invarianten | Invarianten brauchen Kontextklasse, Namen und OCL Expression. | `03`, `09` | `FR-OCL-001`, `FR-OCL-002`, `FR-OCL-004` |
| OCL Editor | Modell-/OCL-Text muss zentral angezeigt, bearbeitet, angewendet und mit Diagnosen versehen werden. | `13` | `FR-OCL-016`, `FR-OCL-017`, `FR-OCL-018`, `FR-OCL-019` |
| Objektmodell | Objekte müssen mit Typ und Slot-Werten angezeigt und bearbeitet werden. | `06` | `FR-OBJ-001`, `FR-OBJ-002`, `FR-OBJ-005`, `FR-OBJ-006` |
| Objektlinks | Object Links müssen auf Basis von Klassenassoziationen erstellbar und bearbeitbar sein. | `11`, `12` | `FR-OBJ-008`, `FR-OBJ-009`, `FR-OBJ-011` |
| Constraint Check | Nutzer kann Validierung zentral auslösen. | `01`, `07` | `FR-VAL-001` |
| Fehlerdarstellung | Fehler werden im Diagramm und im Validation Results Panel angezeigt. | `07` | `FR-VAL-006`, `FR-ERR-001`, `FR-ERR-002` |

## Abgeleitete Frontend-Komponenten

| Komponente | Zweck | Screenshot-Quelle | MVP-Relevanz |
|---|---|---|---|
| `DashboardPage` | Startseite für Projektstart, Import, Recent Projects und Support. | `00` | Hoch |
| `ProjectsPage` | Vollständige Projektliste mit Suche, Filter und Projektkarten. | `19` | Should |
| `ProjectSearchInput` | Sucht oder filtert Projekte nach Name/Beschreibung. | `19` | Should |
| `ProjectFilterButton` | Einstieg in spätere Projektfilter. | `19` | Later |
| `ProjectCardGrid` | Grid-Layout für alle Projektkarten. | `19` | Should |
| `ProjectListCard` | Öffnet ein Projekt aus der vollständigen Liste. | `19` | Should |
| `OpenExistingProjectModal` | Lokale `.use`-Dateien auswählen, Import starten und Diagnosen anzeigen. | `14` | Hoch/Should |
| `FileDropZone` | Datei per Klick oder Drag-and-drop übernehmen. | `14` | Should |
| `CreateNewModelCard` | Startet ein neues Projekt. | `00` | Hoch |
| `OpenExistingCard` | Einstieg zu JSON-Open und später `.use` Import. | `00` | Hoch/Should |
| `RecentProjectsList` | Zeigt zuletzt verwendete oder Demo-Projekte. | `00` | Should |
| `LearnSupportSection` | Verlinkt Documentation und Examples. | `00` | Should |
| `TopBar` | Navigation und globale Aktionen wie `Check Constraints`, Save und Refresh. | `01`, `06`, `07` | Hoch |
| `MainNavigationTabs` | Wechsel zwischen Class Diagram, Object Diagram und OCL Editor. | `01`, `06`, `07` | Hoch |
| `ExplorerSidebar` | Navigation durch Classes, Associations, Invariants, Objects und Object Links. | `01`, `02`, `03`, `06`, `12` | Hoch |
| `ClassDiagramView` | Hauptansicht für Klassendiagramme. | `01`, `02`, `03`, `04` | Hoch |
| `ClassNode` | Darstellung einer UML-Klasse mit Attributen, Operationen und Invariantenhinweis. | `01`, `03`, `04` | Hoch |
| `AssociationEdge` | Darstellung einer Klassenassoziation. | `01`, `02` | Hoch |
| `ClassPropertiesPanel` | Bearbeitung ausgewählter Klassen. | `01`, `04` | Hoch |
| `AssociationPropertiesPanel` | Bearbeitung ausgewählter Associations. | `02` | Hoch |
| `InvariantPropertiesPanel` | Bearbeitung von Invarianten und OCL-Ausdrücken. | `03` | Hoch |
| `OclEditorView` | Zentrale OCL-Hauptansicht für textuelle Modell-/OCL-Bearbeitung. | `13` | Hoch |
| `ModelTextEditor` | Bearbeitet USE-ähnlichen Modelltext mit Klassen, Associations und Constraints. | `13` | Hoch |
| `LineNumberGutter` | Zeigt stabile Zeilennummern für Orientierung und spätere Fehlerpositionen. | `13` | Hoch |
| `ApplyChangesButton` | Übernimmt Editoränderungen in den Projektzustand. | `13` | Hoch |
| `OclDiagnosticsPanel` | Zeigt Parse-, Typecheck- und Validation-Diagnosen zum Text oder zur Invariante. | `13`, `07` | Hoch |
| `AddClassModal` | Anlage neuer Klassen. | `08` | Hoch |
| `AddInvariantModal` | Anlage neuer Invarianten. | `09` | Hoch |
| `AddAssociationModal` | Anlage neuer Klassenassoziationen. | `10` | Hoch |
| `ObjectDiagramView` | Hauptansicht für Snapshots und Objektdiagramme. | `06`, `07`, `12` | Hoch |
| `ObjectNode` | Darstellung eines Objekts mit Typ und Slots. | `06`, `07` | Hoch |
| `ObjectLinkEdge` | Darstellung eines Objektlinks. | `06`, `12` | Hoch |
| `ObjectPropertiesPanel` | Bearbeitung von Objektname, Typ und Slots. | `06` | Hoch |
| `ObjectPropertiesPanel -> Associations` | Bearbeitung oder Anzeige eines Objektlinks. | `12`; Zielmockup `object-link-association-sidebar.html` | Hoch |
| `CreateObjectLinkModal` | Endbasierte Anlage eines Objektlinks. | `11`; Zielmockup `create-object-link-modal.html` | Hoch |
| `CheckConstraintsButton` | Auslösen der Backend-Validierung. | `01`, `07` | Hoch |
| `ValidationResultsPanel` | Anzeige strukturierter Validierungsergebnisse. | `07` | Hoch |
| `ValidationBadge` | Sichtbare Fehleranzahl oder Fehlerindikator am Diagrammelement. | `07` | Hoch |
| `InvalidObjectHighlight` | Visuelle Markierung fehlerhafter Objekte oder Links. | `07` | Hoch |

## Abgeleitete Backend-Anforderungen

| Backend-Bereich | Abgeleitete Funktion | Screenshot-Quelle | MVP-Relevanz |
|---|---|---|---|
| Project Service | Neues Projekt vom Dashboard erzeugen und bestehende Projekte öffnen. | `00` | Hoch |
| Project Import Service | JSON-Projekte öffnen und `.use` Import vorbereiten. | `00` | Should |
| Recent Projects API | Recent Projects liefern oder Demo-/Mockdaten bereitstellen. | `00` | Should |
| Project List API | Vollständige Projektliste für `View all` liefern. | `19` | Should |
| Project Service | Projektzustand mit UML-Modell, Invarianten, Snapshot und Layout speichern/laden. | `01`, `06`, `07` | Hoch |
| UML Model Service | Klassen, Attribute, Operationen und Associations verwalten. | `01`, `02`, `04`, `08`, `10` | Hoch |
| Association Service | Association Ends, Rollen und Multiplizitäten konsistent verwalten. | `02`, `10` | Hoch |
| OCL Service | Invarianten mit Kontextklasse, Name, Ausdruck und textuellem Modell-/OCL-Editor verwalten. | `03`, `09`, `13` | Hoch |
| OCL Engine | OCL-/Modelltext parsen, typprüfen und gegen Snapshots auswerten. | `03`, `07`, `09`, `13` | Hoch |
| Object Model Service | Objekte, Slots und Objektlinks verwalten. | `06`, `11`, `12` | Hoch |
| Snapshot Service | aktuellen Objektzustand als validierbaren Snapshot bereitstellen. | `06`, `07`, `12` | Hoch |
| Validation Service | UML-, Snapshot-, Multiplicity- und OCL-Validierung ausführen. | `07` | Hoch |
| Error Mapping | Validation Errors auf Klassen, Invarianten, Objekte oder Links referenzieren. | `07`, `12` | Hoch |
| Layout Service | Diagrammpositionen und UI-Referenzen persistieren. | `01`, `04`, `06`, `07` | Hoch |

## API-Bezug

| API-Bereich | Benötigte Daten | Betroffene Screenshots | Beispielhafte Funktion |
|---|---|---|---|
| Projekt laden | Projektmetadaten, UML-Modell, Snapshot, Layout, Invarianten | `01`, `06`, `07` | `GET /api/v1/projects/{id}` |
| Projektliste laden | Project Summaries, Such-/Filterparameter, letzte Änderung | `19` | `GET /api/v1/projects` |
| Projekt speichern | vollständiger Projektzustand oder differenzierte Änderungen | `01`, `04`, `06` | `PUT /api/v1/projects/{id}` |
| Klasse erstellen/ändern | Class-ID, Name, Attribute, Operationen, Layout | `01`, `04`, `08` | `POST /api/v1/projects/{id}/classes` |
| Association erstellen/ändern | Association-ID, Name, Enden, Rollen, Multiplizitäten | `02`, `10` | `POST /api/v1/projects/{id}/associations` |
| Invariante erstellen/ändern | Invariant-ID, Kontextklasse, Name, OCL Expression | `03`, `09`, `13` | `POST /api/v1/projects/{id}/invariants` |
| Objekt erstellen/ändern | Object-ID, Name, Class-ID, Slots, Layout | `06` | `POST /api/v1/projects/{id}/snapshots/current/objects` |
| Objektlink erstellen/ändern | Link-ID, Source Object, Target Object, Association-ID | `11`, `12` | `POST /api/v1/projects/{id}/snapshots/current/links` |
| Constraints prüfen | Projekt-ID oder aktueller Projektzustand | `07` | `POST /api/v1/projects/{id}/validate` |
| Validierungsergebnis abrufen | Status, Fehlerliste, Severity, Codes, Elementreferenzen | `07` | Antwort von `POST /api/v1/projects/{id}/validate` |
| OCL prüfen | Modell-/OCL-Text, Kontextklasse, Ausdruck, Source Range, Diagnosen | `03`, `09`, `13` | `POST /api/v1/projects/{id}/ocl/parse`, `POST /api/v1/projects/{id}/ocl/typecheck` |

Die Endpunktnamen sind konzeptionelle Platzhalter. Der verbindliche API-Vertrag wird in `07-integration-and-api/` definiert.

## Validierungsbezug

| Validierungsfall | Screenshot-Bezug | Betroffene UI | Backend-Verantwortung | Benötigtes Error Mapping |
|---|---|---|---|---|
| OCL-Syntaxfehler | `03`, `09`, `13` | OCL Expression Input, OCL Editor, Invariant Properties, Validation Results | OCL Parser | Invariant-ID, Expression, Source Range, Zeile/Spalte |
| OCL-Typefehler | `03`, `09`, `13` | OCL Editor, OCL Feedback, Validation Results | OCL Type Checker | Invariant-ID, Kontextklasse, betroffener Ausdrucksteil, Zeile/Spalte |
| Invariantverletzung | `07` | Object Node, Validation Badge, Validation Results | OCL Evaluator, Validation Service | Objekt-ID, Invariant-ID, Kontextklasse |
| Multiplizitätsverletzung | `02`, `10`, `11`, `12` | Object Link, Object Node, Validation Results | Multiplicity Validator | Association-ID, Association-End-ID, Object-ID oder Link-ID |
| Ungültiger Slot-Wert | `06` | Slot Editor, Object Node, Properties Panel | Snapshot Validator, Type Validator | Object-ID, Attribute-ID, Slot-ID |
| Ungültiger Objektlink | `11`, `12` | Add Object Association Modal, Object Link Edge | Object Link Validator | Link-ID, Source Object ID, Target Object ID, Association-ID |
| Unbekannte Klasse oder Rolle | `02`, `03`, `09`, `10` | Properties Panel, Modal, OCL Feedback | UML Model Service, OCL Type Checker | Class-ID, Association-ID, Role Name, Invariant-ID |

## Offene Fragen

| Frage | Betroffene Screenshots | Bedeutung |
|---|---|---|
| Wo werden Multiplizitäten im UI erfasst, wenn sie in `10` nicht sichtbar sind? | `02`, `10` | MVP braucht Multiplizitäten für Constraint Validation. Anzeige am Canvas ist Pflicht: Rollen und Multiplizitäten werden als Association-Endlabels dargestellt; Erfassung erfolgt mindestens im Properties Panel oder über erweiterte Modal-Felder. |
| Gibt es ein eigenes Modal zum Erstellen von Objekten? | `06` | Objekte sind MVP-Pflicht, aber kein Add-Object-Screenshot liegt vor. |
| Welche `.use`-Syntax aus den Beispielen wird im MVP unterstützt? | `13`, `examples/**/*.use` | Der Editor zeigt vollständige `.use`-Dateien; der Parser unterstützt zunächst den MVP-Subset und meldet nicht unterstützte Konstrukte klar. |
| Wann erfolgt OCL-Syntax- und Typechecking: beim Speichern, live oder nur bei `Check Constraints`? | `03`, `09`, `07` | Beeinflusst API-Design und UX. |
| Wie stark sollen Diagrammelemente bei Fehlern hervorgehoben werden? | `07` | Betrifft Akzeptanzkriterien und Barrierefreiheit. |
| Klickt ein Validation Result automatisch in den passenden Tab? | `07` | Betrifft Navigation zwischen Class Diagram, Object Diagram und OCL Editor. |
| Werden Console und Validation Results getrennt gespeichert oder nur UI-seitig angezeigt? | `01`, `07` | Betrifft Projektformat und Event-Protokollierung. |
| Sind Screenshot-Texte wie `Libary` echte Beispieldaten oder Platzhalter? | `04` | Betrifft Testdaten und fachliche Beispielmodelle. |

## Zusammenfassung

Die Screenshots decken den vollständigen MVP-Arbeitsablauf ab: Klassenmodell betrachten und erweitern, Assoziationen und Invarianten erfassen, in den Snapshot wechseln, Objekte und Objektlinks bearbeiten, Constraints prüfen und Fehler visuell sowie textuell nachvollziehen.

Für die Umsetzung sind drei Traceability-Punkte besonders wichtig:

- Jede sichtbare Modelloperation benötigt stabile IDs, damit Explorer, Canvas, Properties Panel, API und Backend konsistent bleiben.
- Jede Validierungsmeldung muss auf konkrete Modell- oder Snapshot-Elemente referenzieren können.
- Die Screenshots begründen den MVP vertikal: Class Diagram, Object Diagram, OCL-Invarianten und Validation Results müssen zusammen funktionieren, auch wenn spätere Komfortfunktionen wie Autocomplete, Live-Typechecking oder mehrere Snapshots erst Post-MVP folgen.
