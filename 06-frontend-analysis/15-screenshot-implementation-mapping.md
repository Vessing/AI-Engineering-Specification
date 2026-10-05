# Screenshot Implementation Mapping

## Zweck dieser Datei

Diese Datei bildet die Screenshots der geplanten Weboberfläche auf konkrete Frontend-Bausteine ab. Sie beschreibt je Screenshot, welche Komponenten, DTOs, State-Bereiche, API-Endpunkte und MVP-Anforderungen für die Umsetzung benötigt werden.

Das Mapping dient als Brücke zwischen visueller Zielreferenz und späterer React/TypeScript-Implementierung. Es ist keine pixelgenaue UI-Spezifikation, sondern eine strukturierte Ableitung für Komponentenentwurf, State Management, API-Integration und MVP-Planung.

## Mapping-Prinzip

Die Screenshots werden entlang folgender Kette ausgewertet:

```text
Screenshot
-> sichtbare UI-Bereiche
-> Nutzeraktion
-> Frontend-Komponenten
-> DTOs
-> State
-> API-Endpunkte
-> Validierungsbezug
-> MVP-Anforderung
```

Grundannahmen:

- Das Frontend rendert Diagramme, Formulare, Panels und Validierungsergebnisse.
- Das Backend bleibt fachliche Quelle für Projektpersistenz, UML-/OCL-Semantik und Constraint-Validierung.
- Frontend-State und Backend-DTOs werden getrennt, aber über stabile IDs verbunden.
- Layoutinformationen werden im Frontend erzeugt und über das Projektformat persistiert.
- Validierungsfehler müssen über `classId`, `associationId`, `invariantId`, `objectId` oder `linkId` auf UI-Elemente gemappt werden.

Hinweis zur Ablage: Alle berücksichtigten Screenshots liegen unter `assets/screenshots/`. Die Bildlinks in dieser Datei verwenden diese normalisierten Pfade.

## Screenshot-Übersicht

| Nr. | Ziel-Dateiname | Aktueller Pfad | Kurztitel | Primäre View | MVP-Relevanz |
|---|---|---|---|---|---|
| 00 | `00-dashboard-start-page.png` | `../assets/screenshots/00-dashboard-start-page.png` | Dashboard Start Page | Dashboard | Hoch |
| 18 | `18-create-new-projects.png` | `../assets/screenshots/18-create-new-projects.png` | Create New Project | Dashboard Modal / Projektstart | Hoch |
| 14 | `14-open-existing-project.png` | `../assets/screenshots/14-open-existing-project.png` | Open Existing Project | Dashboard Modal / Import | Hoch/Should |
| 19 | `19-projects.png` | `../assets/screenshots/19-projects.png` | All Projects | Projektliste | Should |
| 01 | `01-class-diagram-class-properties.png` | `../assets/screenshots/01-class-diagram-class-properties.png` | Class Properties | Class Diagram | Hoch |
| 02 | `02-class-diagram-association-properties.png` | `../assets/screenshots/02-class-diagram-association-properties.png` | Association Properties | Class Diagram | Hoch |
| 03 | `03-class-diagram-invariant-properties.png` | `../assets/screenshots/03-class-diagram-invariant-properties.png` | Invariant Properties | Class Diagram / OCL | Hoch |
| 04 | `04-class-diagram-new-class-selected.png` | `../assets/screenshots/04-class-diagram-new-class-selected.png` | New Class Selected | Class Diagram | Hoch |
| 06 | `06-object-diagram-object-properties.png` | `../assets/screenshots/06-object-diagram-object-properties.png` | Object Properties | Object Diagram | Hoch |
| 07 | `07-object-diagram-validation-error.png` | `../assets/screenshots/07-object-diagram-validation-error.png` | Validation Error | Object Diagram / Validation | Hoch |
| 08 | `08-modal-add-class.png` | `../assets/screenshots/08-modal-add-class.png` | Add Class Modal | Modal / Class Diagram | Hoch |
| 09 | `09-modal-add-invariant.png` | `../assets/screenshots/09-modal-add-invariant.png` | Add Invariant Modal | Modal / OCL | Hoch |
| 10 | `10-modal-add-class-association.png` | `../assets/screenshots/10-modal-add-class-association.png` | Add Class Association Modal | Modal / Class Diagram | Hoch |
| 11 | `11-modal-add-object-association.png` | `../assets/screenshots/11-modal-add-object-association.png` | Add Object Association Modal | Modal / Object Diagram | Hoch |
| 12 | `12-object-diagram-association-properties.png` | `../assets/screenshots/12-object-diagram-association-properties.png` | Object Association Properties | Object Diagram | Hoch |
| 13 | `13-ocl-editor.png` | `../assets/screenshots/13-ocl-editor.png` | OCL Editor | OCL Editor | Hoch |
| 15 | `15-properties-association.png` | `../assets/screenshots/15-properties-association.png` | Class Related Associations | Class Diagram Properties | Hoch |
| 16 | `16-properties-invariants.png` | `../assets/screenshots/16-properties-invariants.png` | Class Related Invariants | Class Diagram Properties | Hoch |
| 17 | `17-new-class.png` | `../assets/screenshots/17-new-class.png` | New Class Properties Segment | Class Diagram Properties | Hoch |

Hinweis: `13-ocl-editor.png` ist vorhanden und wird als echte Screenshot-Quelle für die OCL Editor View verwendet.

### Beispielvorschauen

![Dashboard Start Page](../assets/screenshots/00-dashboard-start-page.png)

![Create New Project](../assets/screenshots/18-create-new-projects.png)

![Open Existing Project](../assets/screenshots/14-open-existing-project.png)

![All Projects](../assets/screenshots/19-projects.png)

![Class Diagram Class Properties](../assets/screenshots/01-class-diagram-class-properties.png)

![Object Diagram Validation Error](../assets/screenshots/07-object-diagram-validation-error.png)

![Add Class Association Modal](../assets/screenshots/10-modal-add-class-association.png)

![OCL Editor](../assets/screenshots/13-ocl-editor.png)

![Class Properties Association Segment](../assets/screenshots/15-properties-association.png)

## Detailliertes Mapping je Screenshot

| Screenshot | Kurztitel | Sichtbare UI-Bereiche | Sichtbare Komponenten | Nutzeraktionen | Benötigte Frontend-Komponenten | Benötigte DTOs | Benötigter State | Benötigte API-Endpunkte | Validierungsbezug | MVP-Relevanz | Offene Fragen |
|---|---|---|---|---|---|---|---|---|---|---|---|
| `00-dashboard-start-page.png` | Dashboard Start Page | Header, Logo, Produkttitel, Benutzeravatar, `Create New Model`, `Open Existing`, Recent Projects, `Learn & Support`. | `USE` Logo, `+ Start Project`, Open Existing Card, Recent Project Cards, `View all`, Documentation, Examples. | Neues Modell starten, bestehendes Projekt öffnen/importieren, Recent Project öffnen, Dokumentation oder Beispiele öffnen. | `DashboardPage`, `CreateNewModelCard`, `CreateNewProjectModal`, `OpenExistingCard`, `RecentProjectsSection`, `RecentProjectCard`, `LearnSupportSection`, `DocumentationLink`, `ExamplesLink`. | `CreateProjectRequestDto`, `ProjectDto`, `ProjectSummaryDto`, `ImportProjectRequestDto`, `ImportResultDto`. | `dashboardState`, `recentProjectsQuery`, `createProjectFormState`, `createProjectMutation`, `importProjectState`, `navigationState`, `apiLoadingState`. | `POST /api/v1/projects`, `GET /api/v1/projects/recent`, `GET /api/v1/projects/{projectId}`, `POST /api/v1/projects/import`, optional `POST /api/v1/projects/import/use`. | Kein direkter Constraint-Bezug; Einstieg erzeugt oder lädt Projektzustand, der später validiert wird. | Hoch für Dashboard und `Start Project`; Should für Recent Projects und `.use` Import. | Sind Recent Projects im MVP echte Backend-Daten oder Demo-/Mockdaten? Ist `.use` Import aktiv oder nur sichtbarer Einstieg? |
| `18-create-new-projects.png` | Create New Project | Dashboard-Overlay oder Formular, Eingabe für Projektname, Submit, Cancel/Close. | Project Name Field, Create/Start Button, Cancel/Close, Formularfehler. | Projektname eingeben, Projekt erstellen, leeren Namen korrigieren, abbrechen. | `CreateNewProjectModal`, `ProjectNameInput`, `CreateProjectButton`, `CancelButton`, `FormErrorMessage`. | `CreateProjectRequestDto`, `ProjectDto`, `ApiErrorDto`. | `createProjectFormState`, `createProjectMutation`, `dashboardState`, `navigationState`, `consoleState`. | `POST /api/v1/projects` | Kein direkter Constraint-Bezug; erzeugt Projektmetadaten und initialen leeren Modellzustand. | Hoch | Welche Namensregeln gelten final: Eindeutigkeit, maximale Länge, erlaubte Zeichen? |
| `14-open-existing-project.png` | Open Existing Project | Dashboard-Overlay, Modal, `Local File`, Upload-/Drag-and-drop-Fläche, `Cancel`, `Open Project`. | Modal-Titel, Dateiupload, Dropzone, Hinweis `Supported format: .use`, primärer Submit. | `Open Existing` klicken, `.use`-Datei auswählen oder droppen, `Open Project` auslösen, Importdiagnosen lesen. | `OpenExistingProjectModal`, `FileDropZone`, `LocalFileTab`, `OpenProjectButton`, `ImportDiagnosticsList`, `ModalShell`, `ConsolePanel`. | `ApplyModelTextRequestDto`, `ApplyModelTextResponseDto`, `ImportUseRequestDto`, `ImportResultDto`, `ProjectDto`, `DiagnosticDto`. | `modalState`, `importFileState`, `importDiagnosticsState`, `apiMutationState`, `projectQuery`, `navigationState`, `consoleState`. | MVP-nah: `POST /api/v1/projects` + `POST /api/v1/projects/{projectId}/model-text/apply`; später `POST /api/v1/projects/import/use`. | Import-/Syntax-/Typecheck-Diagnosen müssen auf Datei, Zeile/Spalte und später OCL Editor abbildbar sein. | Hoch/Should | Soll bei Warnungen zuerst Class Diagram oder OCL Editor geöffnet werden? |
| `19-projects.png` | All Projects | Header mit Zurück-Icon, Seitentitel `All Projects`, Suchfeld, `Filter`, `Open`, `+ New Project`, Projektkarten-Grid. | Projektkarten für `University System`, `Hotel Management`, `Bank ATM`, `E-Commerce System`, `Library Catalog`, `Car Rental`. | Projekte suchen, filtern, vorhandenes Projekt öffnen, neues Projekt starten, zurück zum Dashboard. | `ProjectsPage`, `ProjectSearchInput`, `ProjectFilterButton`, `ProjectCardGrid`, `ProjectListCard`, `ProjectListActions`, `CreateNewProjectModal`. | `ProjectSummaryDto`, `ProjectDto`, `CreateProjectRequestDto`, `ApiErrorDto`. | `projectListQuery`, `projectSearchState`, `projectFilterState`, `selectedProjectId`, `createProjectFormState`, `navigationState`, `apiLoadingState`. | `GET /api/v1/projects`, `GET /api/v1/projects/{projectId}`, `POST /api/v1/projects` | Kein direkter Constraint-Bezug; geöffnete Projekte liefern später UML-/Snapshot-/OCL-Daten für Validierung. | Should | Sollen Suche und Filter direkt gegen das Backend laufen oder im MVP clientseitig über geladene Summaries? |
| `01-class-diagram-class-properties.png` | Class Properties | Top Bar, Navigation Tabs, Explorer Sidebar, Diagram Canvas, Properties Panel, Bottom Panel, Console, Validation Results, Quick Help | Klassenkarten, Association-Kante, Explorer-Gruppen, Class Properties Form | Klasse im Canvas oder Explorer auswählen; Klassendaten ansehen oder ändern; Constraint Check auslösen | `AppShell`, `TopBar`, `MainNavigationTabs`, `ExplorerSidebar`, `ClassDiagramPage`, `ClassDiagramCanvas`, `UmlClassNode`, `UmlAssociationEdge`, `ClassPropertiesPanel`, `BottomPanel`, `CheckConstraintsButton` | `ProjectDto`, `UmlClassDto`, `UmlAttributeDto`, `UmlOperationDto`, `UmlAssociationDto`, `LayoutDto`, `ValidationResultDto` | `projectQuery`, `activeView`, `selectionState`, `diagramLayoutState`, `propertiesPanelState`, `validationState`, `consoleState` | `GET /api/v1/projects/{projectId}`, `PUT /api/v1/projects/{projectId}`, `PUT /api/v1/projects/{projectId}/classes/{classId}`, `POST /api/v1/projects/{projectId}/validate` | Klassen, Attribute und Operationen sind Grundlage für OCL-Typechecking, Objekt-Slots und Snapshot-Validierung. | Hoch | Welche Class-Properties sind inline editierbar und welche nur per Modal? |
| `02-class-diagram-association-properties.png` | Association Properties | Class Diagram Canvas, Association Edge, Explorer Associations, Properties Panel | selektierte Association, Rollenfelder, Source/Target Class, mittiges Association-Label, Endlabels | Association auswählen; Name, Rollen oder Multiplizitäten bearbeiten | `ClassDiagramPage`, `UmlAssociationEdge`, `AssociationPropertiesPanel`, `ExplorerSidebar`, `EdgeSelectionOverlay` | `UmlAssociationDto`, `UmlAssociationEndDto`, `MultiplicityDto`, `UmlClassDto`, `LayoutDto` | `selectionState`, `projectDraftState`, `diagramEdgeState`, `propertiesPanelState`, `validationStaleState` | `POST /api/v1/projects/{projectId}/associations`, `PUT /api/v1/projects/{projectId}/associations/{associationId}`, `POST /api/v1/projects/{projectId}/validate` | Rollen und Multiplizitäten werden für Association Navigation, Linkvalidierung und Multiplicity Checks benötigt und müssen als Endlabels am Canvas sichtbar sein. | Hoch | Wie werden Endlabels bei kurzen Kanten ohne Überlappung positioniert? |
| `03-class-diagram-invariant-properties.png` | Invariant Properties | Class Diagram View, Explorer Invariants, Properties Panel, OCL-Ausdruck | selektierte Invariante, Invariant Name, OCL Expression | Invariante auswählen; Ausdruck ansehen oder ändern; optional Syntax/Typecheck auslösen | `InvariantListItem`, `InvariantBadge`, `InvariantPropertiesPanel`, `OclExpressionInput`, `OclFeedbackMessage`, `ExplorerSidebar` | `UmlInvariantDto`, `OclExpressionDto`, `OclDiagnosticDto`, `UmlClassDto`, `ValidationResultDto` | `selectionState`, `oclEditorState`, `projectDraftState`, `validationState`, `apiMutationState` | `POST /api/v1/projects/{projectId}/invariants`, `PUT /api/v1/projects/{projectId}/invariants/{invariantId}`, `POST /api/v1/projects/{projectId}/ocl/parse`, `POST /api/v1/projects/{projectId}/ocl/typecheck`, `POST /api/v1/projects/{projectId}/validate` | OCL-Invarianten sind Kern des Constraint Checks; Fehler müssen auf `invariantId`, Kontextklasse und ggf. Objekt zeigen. | Hoch | Wird beim Speichern sofort parse/typecheck ausgeführt oder erst beim Constraint Check? |
| `04-class-diagram-new-class-selected.png` | New Class Selected | Class Diagram Canvas, Explorer Classes, Class Properties Panel | neu angelegte Klasse, selektierter Class Node | Klasse erstellen; neue Klasse automatisch selektieren; Position verschieben | `AddClassModal`, `UmlClassNode`, `ClassPropertiesPanel`, `ExplorerSidebar`, `DiagramLayoutController` | `CreateClassRequestDto`, `UmlClassDto`, `LayoutNodeDto` | `modalState`, `selectionState`, `diagramLayoutState`, `projectDraftState`, `apiMutationState` | `POST /api/v1/projects/{projectId}/classes`, `PUT /api/v1/projects/{projectId}`, `PUT /api/v1/projects/{projectId}/classes/{classId}` | Neue Klassen können Kontext für Invarianten und Typ für Objektinstanzen werden. | Hoch | Soll der Class Node nach Erstellung automatisch eine Default-Position erhalten oder per Klick platziert werden? |
| `06-object-diagram-object-properties.png` | Object Properties | Object Diagram View, Explorer Objects, Object Canvas, Object Properties Panel | Objektkarten, Objektname mit Typ, Slot-Werte, Object Link | Objekt auswählen; Slot-Werte bearbeiten; Objekt verschieben | `ObjectDiagramPage`, `ObjectDiagramCanvas`, `ObjectNode`, `SlotValueList`, `ObjectPropertiesPanel`, `ObjectLinkEdge`, `ExplorerSidebar` | `ObjectModelDto`, `ObjectInstanceDto`, `SlotDto`, `UmlClassDto`, `UmlAttributeDto`, `ObjectLinkDto`, `LayoutDto` | `projectQuery`, `objectModelState`, `selectionState`, `slotEditState`, `diagramLayoutState`, `validationState` | `POST /api/v1/projects/{projectId}/objects`, `PUT /api/v1/projects/{projectId}/objects/{objectId}`, `PUT /api/v1/projects/{projectId}`, `POST /api/v1/projects/{projectId}/validate` | Slot-Werte sind Eingabe für Type Checks, OCL Evaluation und Invariant Checks. | Hoch | Gibt es ein separates `AddObjectModal`, obwohl kein Screenshot dafür vorliegt? |
| `07-object-diagram-validation-error.png` | Validation Error | Object Diagram, Validation Results Panel, Bottom Panel, Fehler-Badge, Check Constraints Button | rot markiertes Objekt, Error Badge, Fehlerliste mit Invariantverletzung | `Check Constraints` klicken; Fehler anklicken; betroffenes Objekt fokussieren | `CheckConstraintsButton`, `ValidationResultsPanel`, `ValidationSummary`, `ValidationErrorList`, `ValidationErrorItem`, `ValidationBadge`, `InvalidObjectHighlight`, `FocusTargetResolver`, `ObjectNode` | `ValidationResultDto`, `ValidationErrorDto`, `ElementTargetDto`, `ObjectInstanceDto`, `UmlInvariantDto` | `validationState`, `errorMappingState`, `selectionState`, `focusState`, `activeBottomPanelTab`, `consoleState`, `apiMutationState` | `POST /api/v1/projects/{projectId}/validate`, optional `GET /api/v1/projects/{projectId}` nach Validierung | Zentraler MVP-Bezug: `INVARIANT_VIOLATION` muss auf `objectId` und `invariantId` gemappt werden. | Hoch | Soll ein Klick auf den Fehler automatisch die passende View öffnen, zoomen und selektieren? |
| `08-modal-add-class.png` | Add Class Modal | Modal Layer über Class Diagram, Formularfelder, Submit/Cancel | Class Name, Attribute-Liste, Operationen-Liste | Klasse mit Attributen und Operationen anlegen; Pflichtfelder prüfen | `AddClassModal`, `ModalShell`, `TextField`, `AttributeEditor`, `OperationSignatureEditor`, `SubmitButton`, `FormErrorMessage` | `CreateClassRequestDto`, `CreateAttributeDto`, `CreateOperationDto`, `UmlClassDto` | `modalState`, `formState`, `formValidationState`, `apiMutationState`, `projectDraftState` | `POST /api/v1/projects/{projectId}/classes`, optional `PUT /api/v1/projects/{projectId}` | Attribute beeinflussen spätere Slot-Erzeugung und OCL-Typprüfung. | Hoch | Sollen Attribute und Operationen direkt im Add-Modal vollständig erfasst werden oder zunächst nur der Klassenname? |
| `09-modal-add-invariant.png` | Add Invariant Modal | Modal Layer, Context Class Dropdown, Name-Feld, OCL Expression | Kontextklasse, Invariant Name, OCL-Ausdruck | Invariante definieren; Kontextklasse wählen; Ausdruck speichern | `AddInvariantModal`, `ContextClassSelect`, `OclExpressionInput`, `OclFeedbackMessage`, `ModalShell` | `CreateInvariantRequestDto`, `UmlInvariantDto`, `UmlClassDto`, `OclDiagnosticDto` | `modalState`, `formState`, `oclDraftState`, `apiMutationState`, `validationStaleState` | `POST /api/v1/projects/{projectId}/invariants`, optional `POST /api/v1/projects/{projectId}/ocl/parse`, `POST /api/v1/projects/{projectId}/ocl/typecheck` | Syntax- und Typecheck-Fehler können beim Speichern oder beim Constraint Check angezeigt werden. | Hoch | Soll das Modal Backend-Diagnosen live anzeigen oder erst nach Submit? |
| `10-modal-add-class-association.png` | Add Class Association Modal | Modal Layer, Association Name, Source/Target Class, Rollen | Dropdowns für Klassen und Rollenfelder | Association zwischen Klassen anlegen; Rollen und Multiplizitäten erfassen | `AddAssociationModal`, `ClassSelect`, `RoleInput`, `MultiplicityInput`, `ModalShell` | `CreateAssociationRequestDto`, `UmlAssociationDto`, `UmlAssociationEndDto`, `MultiplicityDto`, `UmlClassDto` | `modalState`, `formState`, `projectDraftState`, `apiMutationState`, `diagramEdgePreviewState` | `POST /api/v1/projects/{projectId}/associations`, optional `PUT /api/v1/projects/{projectId}` | Associations ermöglichen Object Links, Navigation und Multiplicity Checks. | Hoch | Werden Multiplizitäten im Modal zwingend erfasst oder im Properties Panel ergänzt? |
| `11-modal-add-object-association.png` | Add Object Association Modal | Modal Layer über Object Diagram, Source Object, Target Object, Association | Dropdowns für Objektinstanzen und Association | Objektlink anlegen; kompatible Association auswählen; Link speichern | `AddObjectAssociationModal`, `ObjectSelect`, `AssociationSelect`, `ObjectLinkPreview`, `ModalShell` | `CreateObjectLinkRequestDto`, `ObjectInstanceDto`, `UmlAssociationDto`, `ObjectLinkDto` | `modalState`, `formState`, `objectModelState`, `apiMutationState`, `validationStaleState` | `POST /api/v1/projects/{projectId}/links`, optional `POST /api/v1/projects/{projectId}/validate` | Linkvalidierung und Multiplicity Checks hängen direkt von diesen Daten ab. | Hoch | Soll die UI nur gültige Object/Association-Kombinationen anbieten oder bewusst ungültige Links zur Validierung zulassen? |
| `12-object-diagram-association-properties.png` | Object Association Properties | Object Diagram Canvas, selektierter Object Link, Properties Panel, Explorer Associations | Link-Kante, Source Object, Target Object, Association, mittiges Link-Label | Object Link auswählen; Linkdaten ansehen oder ändern; Link löschen | `ObjectLinkEdge`, `ObjectAssociationPropertiesPanel`, `ExplorerSidebar`, `LinkSelectionOverlay` | `ObjectLinkDto`, `ObjectInstanceDto`, `UmlAssociationDto`, `ElementTargetDto`, `ValidationErrorDto` | `selectionState`, `objectModelState`, `propertiesPanelState`, `diagramEdgeState`, `validationState` | `PUT /api/v1/projects/{projectId}/links/{linkId}`, `DELETE /api/v1/projects/{projectId}/links/{linkId}`, `POST /api/v1/projects/{projectId}/validate` | Fehler können auf `linkId`, Source/Target Object oder Association-Ende verweisen; Association-Name muss mittig am Link sichtbar sein. | Hoch | Wie werden Link-Endinformationen und Fehler gleichzeitig lesbar am Edge dargestellt? |
| `13-ocl-editor.png` | OCL Editor | OCL Editor Tab, textuelle Editorfläche, Zeilennummern, `Apply Changes`, `Check Constraints`, Refresh/Save, Bottom Panel mit Console und Validation Results. | Vollständiger Modelltext wie in `.use`-Beispielen: `model Library`, Klassen, Attribute, Operationen, Association `Borrows`, Abschnitt `constraints`, Console Logs. | OCL Editor öffnen, vollständigen Modell-/OCL-Text bearbeiten, `Apply Changes` klicken, Save/Refresh nutzen, `Check Constraints` auslösen. | `OclEditorPage`, `ModelTextEditor`, `LineNumberGutter`, `ApplyChangesButton`, `OclEditorActions`, `ConsolePanel`, `ValidationResultsPanel`. | `ProjectDto`, `ModelTextDto`, `ApplyModelTextRequestDto`, `ApplyModelTextResponseDto`, `OclParseRequestDto`, `OclParseResponseDto`, `OclTypecheckRequestDto`, `OclTypecheckResponseDto`, `ValidationErrorDto`. | `modelTextDraftState`, `oclDiagnosticsState`, `validationState`, `apiMutationState`, `consoleState`, `dirtyState`. | `GET /api/v1/projects/{projectId}`, `PUT /api/v1/projects/{projectId}`, `POST /api/v1/projects/{projectId}/model-text/apply`, `POST /api/v1/projects/{projectId}/ocl/parse`, `POST /api/v1/projects/{projectId}/ocl/typecheck`, `POST /api/v1/projects/{projectId}/validate`. | Syntax-, Typecheck- und Invariantfehler werden über Textpositionen sowie `classId`, `associationId`, `invariantId` und optional `objectId` gemappt. | Hoch | MVP unterstützt nur den definierten UML/OCL-Subset; nicht unterstützte `.use`-Syntax aus Beispielen wird als Diagnose angezeigt. |
| `15-properties-association.png` | Class Related Associations | Class Diagram, Explorer, selektierter Class Node, Properties Panel mit Segment `Association`. | Segment Control `Class`, `Association`, `Invariant`; Related Association `Borrows`; Add Association. | Bei selektierter Klasse `Association` öffnen, zugehörige Association ansehen oder auswählen, neue Association mit Klassenkontext starten. | `ClassPropertiesPanel`, `ClassPropertiesSegmentControl`, `RelatedAssociationList`, `RelatedAssociationItem`, `AddAssociationButton`, `AssociationPropertiesPanel`. | `ProjectDto`, `UmlClassDto`, `UmlAssociationDto`, `UmlAssociationEndDto`, `MultiplicityDto`. | `selectionState`, `propertiesPanelState`, `projectState`, `modalState`. | Anzeige ohne Request; Add/Detail nutzt `POST /api/v1/projects/{projectId}/associations`, `PUT /api/v1/projects/{projectId}/associations/{associationId}`. | Related Associations liefern OCL-Navigationsrollen und Multiplicity-Regeln. | Hoch | Bleibt Class Node visuell selektiert oder wechselt die Selektion beim Klick auf Related Association sofort zur Edge? |
| `16-properties-invariants.png` | Class Related Invariants | Class Diagram, Explorer, selektierter Class Node, Properties Panel mit Segment `Invariant`. | Segment Control, Invariant Name, OCL Expression Preview, Add Invariant. | Bei selektierter Klasse `Invariant` öffnen, zugehörige Invariante ansehen oder auswählen, neue Invariante mit Kontextklasse starten. | `ClassPropertiesPanel`, `ClassPropertiesSegmentControl`, `RelatedInvariantList`, `RelatedInvariantItem`, `AddInvariantButton`, `InvariantPropertiesPanel`. | `ProjectDto`, `UmlClassDto`, `UmlInvariantDto`, `OclDiagnosticDto`, `ValidationErrorDto`. | `selectionState`, `propertiesPanelState`, `projectState`, `modalState`, `validationState`. | Anzeige ohne Request; Add/Detail nutzt `POST /api/v1/projects/{projectId}/invariants`, `PUT /api/v1/projects/{projectId}/invariants/{invariantId}`. | Invarianten sind Eingabe fuer OCL-Typechecking und Constraint Validation; Fehler werden ueber `invariantId` gemappt. | Hoch | Ist der OCL-Ausdruck im Related-Segment editierbar oder nur Vorschau? |
| `17-new-class.png` | New Class Properties Segment | Class Diagram, neue selektierte Klasse, Properties Panel mit Segment `Class`, Console. | Class Name, Attributes, Operations, Add Attribute, Add Operation. | Klasse erstellen, neue Klasse direkt bearbeiten, Attribute oder Operationen nachträglich hinzufügen. | `AddClassModal`, `ClassNode`, `ClassPropertiesPanel`, `AttributeEditor`, `OperationSignatureEditor`, `ExplorerSidebar`. | `CreateClassRequestDto`, `UmlClassDto`, `UmlAttributeDto`, `UmlOperationDto`, `LayoutDto`. | `selectionState`, `projectState`, `diagramLayoutState`, `propertiesPanelState`, `consoleState`. | `POST /api/v1/projects/{projectId}/classes`, `PUT /api/v1/projects/{projectId}/classes/{classId}`, optional `PUT /api/v1/projects/{projectId}`. | Neue Klassen koennen Kontext fuer Invarianten und Typ fuer Objektinstanzen werden. | Hoch | Naming-Regeln fuer neue Klassen muessen final mit Backend-Validierung abgestimmt werden. |

## Komponentenmatrix

| Komponente | Zweck | Screenshots | Wichtige Props/Eingaben | State-Bezug | MVP |
|---|---|---|---|---|---|
| `DashboardPage` | Startseite für Projektstart, Import, Recent Projects und Support. | `00`, `18` | Project Summaries, Loading States, Support Links | `dashboardState`, `recentProjectsQuery` | Ja |
| `ProjectsPage` | Vollständige Projektliste für `View all`. | `19` | Project Summaries, Such- und Filterzustand | `projectListQuery`, `projectSearchState` | Should |
| `ProjectSearchInput` | Filtert Projektkarten nach Name oder Beschreibung. | `19` | Suchtext, Änderungs-Callback | `projectSearchState` | Should |
| `ProjectFilterButton` | Einstieg in spätere Projektfilter. | `19` | verfügbare Filter, aktiver Filterstatus | `projectFilterState` | Later |
| `ProjectCardGrid` | Rendert Projektkarten responsiv. | `19` | `ProjectSummaryDto[]` | `projectListQuery` | Should |
| `ProjectListCard` | Zeigt ein Projekt und öffnet es. | `19` | Project Summary, Open Callback | `navigationState` | Should |
| `CreateNewProjectModal` | Erfasst Projektname und startet Backend-Projektanlage. | `18` | initialer Name, Submit Callback, API-Fehler | `createProjectFormState`, `createProjectMutation` | Ja |
| `OpenExistingProjectModal` | Lokale `.use`-Datei auswählen und Import/Model-Text-Apply auslösen. | `14` | Datei, Importstatus, Diagnosen, Submit Callback | `modalState`, `importFileState`, `importDiagnosticsState` | Ja/Should |
| `FileDropZone` | Datei per Klick oder Drag-and-drop übernehmen. | `14` | akzeptierte Dateitypen, ausgewählte Datei, Fehler | `importFileState` | Should |
| `CreateNewModelCard` | Primärer Einstieg zum neuen Projekt. | `00` | Button Label, Beschreibung, Create Callback | `createProjectMutation` | Ja |
| `OpenExistingCard` | Einstieg zu JSON Open und später `.use` Import. | `00` | Import Status, File Picker State | `importProjectState` | Ja/Should |
| `RecentProjectsSection` | Liste zuletzt verwendeter oder Demo-Projekte. | `00` | `ProjectSummaryDto[]` | `recentProjectsQuery` | Should |
| `RecentProjectCard` | Einzelnes Recent Project öffnen. | `00` | `projectId`, Name, Metadaten | `navigationState` | Should |
| `LearnSupportSection` | Documentation und Examples erreichbar machen. | `00` | Links, Feature Flags | lokaler UI State | Should |
| `AppShell` | Globales Layout mit Top Bar, Navigation, Sidebars und Panels. | `01`, `06`, `07` | `projectId`, `activeView`, `validationSummary` | `activeView`, `panelState` | Ja |
| `TopBar` | Globale Aktionen wie Save, Refresh und Check Constraints. | `01`, `06`, `07` | `projectName`, `dirtyState`, `validationStatus` | `apiMutationState`, `validationState` | Ja |
| `MainNavigationTabs` | Wechsel zwischen Class Diagram, Object Diagram und OCL Editor. | `01`, `06`, `07` | aktive View, Projekt-ID | `activeView` oder URL | Ja |
| `ExplorerSidebar` | Gruppierte Navigation durch Modell- und Snapshot-Elemente. | `01`, `02`, `03`, `06`, `12` | Klassen, Associations, Invarianten, Objekte, Links | `selectionState`, `validationState` | Ja |
| `ClassDiagramPage` | Container für Klassendiagramm, Toolbar und Kontextpanels. | `01`, `02`, `03`, `04`, `15`, `16`, `17` | UML-Modell, Layout, Selection | `diagramLayoutState`, `selectionState` | Ja |
| `ClassDiagramCanvas` | Rendert Klassen und Associations. | `01`, `02`, `03`, `04` | Nodes, Edges, Layout, Markierungen | `diagramState`, `validationState` | Ja |
| `UmlClassNode` | UML-Klassenkarte mit Attributen und Operationen. | `01`, `04`, `17` | Klasse, Attribute, Operationen, Status | `selectionState`, `layoutState` | Ja |
| `ClassPropertiesSegmentControl` | Segmentwechsel zwischen `Class`, `Association` und `Invariant` bei selektierter Klasse. | `15`, `16`, `17` | selektierte Klasse, aktive Segment-ID | `propertiesPanelState`, lokal oder UI Store | Ja |
| `RelatedAssociationList` | Zeigt Associations der selektierten Klasse. | `15` | `UmlAssociationDto[]`, selektierte Klasse | `selectionState`, `modalState` | Ja |
| `RelatedInvariantList` | Zeigt Invarianten der selektierten Klasse. | `16` | `UmlInvariantDto[]`, selektierte Klasse | `selectionState`, `modalState` | Ja |
| `UmlAssociationEdge` | Custom Edge mit mittigem Association-Namen sowie Rollen und Multiplizitaeten an beiden Enden. | `01`, `02`, `10` | Association, Enden, Rollen, Multiplizitäten | `selectionState`, `edgeState`, `layoutState` | Ja |
| `InvariantBadge` | Zeigt Invarianten in der Nähe der Klasse oder als Diagrammelement. | `03` | Invariante, Kontextklasse, Diagnose | `selectionState`, `validationState` | Ja |
| `OclEditorPage` | Zentrale OCL-Hauptview für textuelle Modell-/OCL-Bearbeitung. | `13` | Modelltext, Drafts, Diagnostics, Console Events | `modelTextDraftState`, `oclDiagnosticsState`, `consoleState` | Ja |
| `ModelTextEditor` | Bearbeitet USE-ähnlichen Modell-/OCL-Text. | `13` | Text, Cursor, Zeilen, Diagnostics | `modelTextDraftState`, `apiMutationState` | Ja |
| `LineNumberGutter` | Zeigt Zeilennummern für Orientierung und Fehler-Mapping. | `13` | Zeilenanzahl, Diagnostic Ranges | `modelTextDraftState`, `oclDiagnosticsState` | Ja |
| `ApplyChangesButton` | Übernimmt Editor-Draft in Projektzustand/API. | `13` | Dirty State, Mutation Status | `apiMutationState`, `dirtyState` | Ja |
| `OclDiagnosticsPanel` | Zeigt Syntax-, Typ- und Validierungsdiagnosen zum Editor oder zur Invariante. | `13`, `07` | OCL Diagnostics, Validation Errors | `oclDiagnosticsState`, `validationState` | Ja |
| `ObjectDiagramPage` | Container für Objektdiagramm und Snapshot-Interaktion. | `06`, `07`, `12` | Objektmodell, UML-Modell, Layout | `objectModelState`, `diagramState` | Ja |
| `ObjectNode` | Objektkarte mit Name, Typ und Slot-Werten. | `06`, `07` | Objekt, Klasse, Slots, Fehlerstatus | `selectionState`, `validationState` | Ja |
| `ObjectLinkEdge` | Custom Edge zwischen Objektinstanzen mit mittigem Association-Namen und technisch vorbereiteten Endinformationen. | `06`, `11`, `12` | Link, Association, Source/Target, Rollen/Enden | `selectionState`, `validationState`, `edgeState` | Ja |
| `PropertiesPanel` | Rendert Details zur aktuellen Selektion. | `01`, `02`, `03`, `06`, `12` | selektiertes Element, Projektmodell | `selectionState`, `formState` | Ja |
| `AddClassModal` | Erstellt neue Klassen. | `08` | initiale Werte, Primitive Typen | `modalState`, `formState` | Ja |
| `AddInvariantModal` | Erstellt OCL-Invarianten. | `09` | Kontextklassen, OCL-Ausdruck | `modalState`, `oclDraftState` | Ja |
| `AddAssociationModal` | Erstellt Klassenassoziationen. | `10` | Klassen, Rollen, Multiplizitäten | `modalState`, `formState` | Ja |
| `AddObjectAssociationModal` | Erstellt Objektlinks. | `11` | Objekte, Associations | `modalState`, `formState` | Ja |
| `ValidationResultsPanel` | Zeigt strukturierte Validation Results. | `07` | `ValidationResultDto` | `validationState`, `focusState` | Ja |
| `ValidationBadge` | Markiert fehlerhafte Objekte, Links, Klassen oder Invarianten. | `07` | Fehleranzahl, Severity | `validationState` | Ja |

## State-Matrix

| State-Bereich | Enthaltene Daten | Herkunft | Persistenz | Screenshots | Verantwortliche Frontend-Schicht |
|---|---|---|---|---|---|
| Dashboard State | Ladezustand, Importdialog, Create-New-Project-Dialog, Support-Link-Zustand, ausgewähltes Recent Project | Frontend/API | temporär | `00`, `18`, `14` | Dashboard Feature |
| Create Project Form State | Projektname, Dirty Flag, Pflichtfeldfehler, Submitstatus | Frontend/API | temporär | `18` | Dashboard Feature |
| Import File State | ausgewählte `.use`-Datei, Dateiname, Dateigröße, lokaler Validierungsfehler, gelesener Modelltext | Frontend File API | temporär | `14` | Dashboard Import Feature |
| Import Diagnostics State | Backend-Diagnosen zu `.use`-/Model-Text-Import | Backend/API | temporär, optional Console | `14`, `13` | Import/OCL Feature |
| Recent Projects State | Projekt-Summaries oder Demo-Projekte | Backend oder Mockdaten | optional | `00` | TanStack Query oder Mock Store |
| Project List State | vollständige Projektliste, Suchtext, Filterstatus, selektiertes Projekt | Backend/API + Frontend | temporär | `19` | TanStack Query plus lokaler UI-State |
| Server State | Projekt, UML-Modell, Objektmodell, Invarianten, gespeichertes Layout | Backend | Backend/JSON-Projektformat | alle | TanStack Query |
| Project Draft State | lokale Formular- oder Diagrammänderungen vor Save/Mutation | Frontend | temporär | `01`, `02`, `04`, `06`, `08`, `10`, `11` | Zustand oder lokaler Feature-State |
| Diagram Layout State | Node-Positionen, Edge-Layout, Viewport optional | Frontend | ja, als Layoutdaten | `01`, `04`, `06`, `07`, `12` | Diagram Feature Store |
| Selection State | aktuell selektierte Klasse, Association, Invariante, Objekt oder Link | Frontend | temporär | `01`, `02`, `03`, `04`, `06`, `12` | globaler UI Store |
| Modal State | geöffnetes Modal, initiale Kontextwerte | Frontend | temporär | `08`, `09`, `10`, `11` | UI Store oder lokaler State |
| Form State | Eingabewerte, Dirty Flags, Formularfehler | Frontend | temporär | `01`, `02`, `03`, `06`, `08`, `09`, `10`, `11`, `12` | React Hook Form oder lokaler Reducer |
| Model Text Draft State | Textueller Modell-/OCL-Draft, Dirty Flag, Cursor, Diagnosen | Frontend + Backenddiagnosen | temporär bis Apply/Save | `13` | OCL Feature Store |
| OCL Editor State | Editor-Draft, Diagnosefilter, Apply-/Prüfstatus | Frontend/API | temporär | `13` | OCL Feature Store |
| Validation State | letztes Ergebnis, Fehlerindex, Severity Counts | Backend + Frontend-Mapping | temporär | `07` | TanStack Mutation + Zustand |
| Error Mapping State | Map von Element-ID zu Fehlern | abgeleitet aus Validation Results | temporär | `07`, `12` | Selector/Derived State |
| API Loading/Error State | Query- und Mutationstatus | API Client | temporär | alle interaktiven Screens | TanStack Query |
| Console State | technische und fachliche UI-Logs | Frontend | temporär, optional Session | `01`, `07` | UI Store |

### Beispiel: Selection State

```ts
type Selection =
  | { kind: "class"; classId: string }
  | { kind: "association"; associationId: string }
  | { kind: "invariant"; invariantId: string }
  | { kind: "object"; objectId: string }
  | { kind: "objectLink"; linkId: string }
  | { kind: "none" };
```

### Beispiel: Validation Error Mapping

```ts
type ValidationErrorIndex = {
  byObjectId: Record<string, ValidationErrorDto[]>;
  byClassId: Record<string, ValidationErrorDto[]>;
  byAssociationId: Record<string, ValidationErrorDto[]>;
  byInvariantId: Record<string, ValidationErrorDto[]>;
  byLinkId: Record<string, ValidationErrorDto[]>;
};
```

## API-Matrix

| API-Endpunkt | Zweck im Frontend | Auslöser/Screenshots | Wichtige DTOs | MVP |
|---|---|---|---|---|
| `POST /api/v1/projects` | Neues Projekt vom Dashboard mit eingegebenem Namen starten. | `00`, `18` | `CreateProjectRequestDto`, `ProjectDto` | Ja |
| `POST /api/v1/projects/{projectId}/model-text/apply` | Lokale `.use`-Datei aus Open Existing Modal als Modelltext anwenden. | `14`, `13` | `ApplyModelTextRequestDto`, `ApplyModelTextResponseDto`, `ProjectDto`, Diagnostics | Should |
| `POST /api/v1/projects/import/use` | Direkter vollständiger USE-Import. | `14` | `ImportUseRequestDto`, `ImportResultDto` | Post-MVP |
| `GET /api/v1/projects/recent` | Recent Projects laden. | `00` | `ProjectSummaryDto[]` | Should |
| `GET /api/v1/projects` | Vollständige Projektliste für `View all` laden. | `19` | `ProjectSummaryDto[]` | Should |
| `POST /api/v1/projects/import` | MVP-JSON-Projekt importieren. | `00` | `ImportProjectRequestDto`, `ImportResultDto` | MVP/Should |
| `POST /api/v1/projects/import/use` | `.use`-Datei importieren. | `00` | `UseImportRequestDto`, `ImportResultDto` | Should/Post-MVP |
| `GET /api/v1/projects/{projectId}` | Projekt inklusive UML-Modell, Objektmodell, Invarianten und Layout laden. | Projekt öffnen, alle Views | `ProjectDto` | Ja |
| `PUT /api/v1/projects/{projectId}` | Gesamten Projektstand oder Layoutdaten speichern. | Save, Drag & Drop, Layoutspeicherung | `ProjectDto`, `LayoutDto` | Ja |
| `POST /api/v1/projects/{projectId}/classes` | Klasse erstellen. | `08`, `04` | `CreateClassRequestDto`, `UmlClassDto` | Ja |
| `PUT /api/v1/projects/{projectId}/classes/{classId}` | Klasse, Attribute oder Operationen bearbeiten. | `01`, `04` | `UpdateClassRequestDto`, `UmlClassDto` | Ja |
| `POST /api/v1/projects/{projectId}/associations` | Klassenassoziation erstellen. | `10`, `02` | `CreateAssociationRequestDto`, `UmlAssociationDto` | Ja |
| `PUT /api/v1/projects/{projectId}/associations/{associationId}` | Association, Rollen oder Multiplizitäten ändern. | `02` | `UpdateAssociationRequestDto` | Ja |
| `POST /api/v1/projects/{projectId}/invariants` | OCL-Invariante erstellen. | `09`, `03` | `CreateInvariantRequestDto`, `UmlInvariantDto` | Ja |
| `PUT /api/v1/projects/{projectId}/invariants/{invariantId}` | Invariante ändern. | `03`, `13` | `UpdateInvariantRequestDto`, `OclExpressionDto` | Ja |
| `POST /api/v1/projects/{projectId}/objects` | Objektinstanz erstellen. | fachlich erforderlich, kein eigener Screenshot | `CreateObjectRequestDto`, `ObjectInstanceDto` | Ja |
| `PUT /api/v1/projects/{projectId}/objects/{objectId}` | Objektname, Klasse oder Slot-Werte ändern. | `06` | `UpdateObjectRequestDto`, `SlotDto` | Ja |
| `POST /api/v1/projects/{projectId}/links` | Objektlink erstellen. | `11`, `12` | `CreateObjectLinkRequestDto`, `ObjectLinkDto` | Ja |
| `PUT /api/v1/projects/{projectId}/links/{linkId}` | Objektlink ändern. | `12` | `UpdateObjectLinkRequestDto` | Ja |
| `DELETE /api/v1/projects/{projectId}/links/{linkId}` | Objektlink löschen. | `12` | `ApiResultDto` | Sollte |
| `POST /api/v1/projects/{projectId}/validate` | Vollständigen Constraint Check ausführen. | `07`, Top Bar | `ValidationResultDto` | Ja |
| `POST /api/v1/projects/{projectId}/ocl/parse` | OCL-/Modelltext syntaktisch prüfen. | `03`, `09`, `13` | `OclParseRequestDto`, `OclDiagnosticDto` | Sollte |
| `POST /api/v1/projects/{projectId}/ocl/typecheck` | OCL-/Modelltext fachlich typprüfen. | `03`, `09`, `13` | `OclTypecheckRequestDto`, `OclDiagnosticDto` | Sollte |

## MVP-Anforderungen aus Screenshots

| ID | MVP-Anforderung | Screenshot-Quelle | Akzeptanzhinweis |
|---|---|---|---|
| `MVP-FE-000` | Dashboard ist der erste Einstiegspunkt der Anwendung. | `00` | `/` oder `/dashboard` zeigt Dashboard mit Start-, Open-, Recent- und Support-Bereichen. |
| `MVP-FE-000A` | `Start Project` öffnet die Projektnamenerfassung. | `00`, `18` | Nach Klick erscheint `CreateNewProjectModal` oder ein äquivalentes Formular. |
| `MVP-FE-000B` | Neues Projekt wird mit eingegebenem Namen erstellt und öffnet das Klassendiagramm. | `18` | Nach gültigem Namen wird `POST /api/v1/projects` aufgerufen und zu `/projects/{projectId}/class-diagram` navigiert. |
| `MVP-FE-000C` | `View all` öffnet eine All-Projects-Seite. | `19` | Projektliste zeigt Suchfeld, Filter, Projektkarten, `Open` und `+ New Project`; echte serverseitige Filter sind Post-MVP. |
| `MVP-FE-001` | Die App besitzt eine Shell mit Top Bar, Tabs, Explorer, Canvas, Properties Panel und Bottom Panel. | `01`, `06`, `07` | Nutzer kann zwischen Class Diagram und Object Diagram wechseln und sieht konsistente Panels. |
| `MVP-FE-002` | Klassendiagramm rendert Klassen mit Attributen und Operationen. | `01`, `04`, `08` | Klassenkarten zeigen Name, Attribute und Operationen und sind selektierbar. |
| `MVP-FE-003` | Klassen können über ein Modal erstellt werden. | `08`, `04` | Nach Submit erscheint die Klasse im Explorer und Canvas und ist selektiert. |
| `MVP-FE-004` | Associations können erstellt, angezeigt und selektiert werden. | `02`, `10` | Association Edge zeigt Label/Rollen und öffnet das passende Properties Panel. |
| `MVP-FE-005` | Invarianten können erstellt, angezeigt und bearbeitet werden. | `03`, `09` | Invariant Name, Kontextklasse und OCL Expression werden gespeichert und im Explorer sichtbar. |
| `MVP-FE-005A` | OCL Editor ist als eigene Hauptview nutzbar. | `13` | OCL Editor zeigt textuellen Modell-/OCL-Editor, Zeilennummern, `Apply Changes`, Console und Validierungsaktionen. |
| `MVP-FE-006` | Objektdiagramm rendert Objektinstanzen mit Typ und Slot-Werten. | `06` | Objektkarte zeigt `name : Class` und Attribute/Slots. |
| `MVP-FE-007` | Objektlinks können erstellt, angezeigt und selektiert werden. | `11`, `12` | Link verbindet zwei Objekte und referenziert eine Association. |
| `MVP-FE-008` | Properties Panel reagiert auf Selektion. | `01`, `02`, `03`, `06`, `12`, `15`, `16`, `17` | Auswahl von Klasse, Association, Invariante, Objekt oder Link zeigt jeweils passende Felder; bei Klassenselektion sind Related Associations und Invarianten erreichbar. |
| `MVP-FE-009` | `Check Constraints` löst Backend-Validierung aus. | `07` | Button ruft `POST /api/v1/projects/{projectId}/validate` auf und aktualisiert Validation State. |
| `MVP-FE-010` | Validierungsfehler werden im Diagramm und Panel angezeigt. | `07` | Betroffenes Objekt hat roten Rahmen/Badge; Panel zeigt Fehlerdetails. |
| `MVP-FE-011` | Klick auf Validation Result fokussiert betroffenes Element. | `07` | Frontend löst Ziel über `objectId`, `linkId`, `classId` oder `invariantId` auf. |
| `MVP-FE-012` | Layoutdaten werden stabil über Element-IDs verwaltet. | `01`, `04`, `06`, `12` | Node-Positionen bleiben nach Speichern/Laden erhalten. |

## Validierungsbezug

Validierungsergebnisse aus dem Backend müssen für das Frontend nicht nur textuell, sondern auch navigierbar sein.

| Fehlerart | Typischer Zielverweis | UI-Darstellung | Relevante Screenshots |
|---|---|---|---|
| `SYNTAX_ERROR` | `invariantId`, OCL-Position, Zeile/Spalte | OCL Input Fehlermarkierung, OCL Editor, Validation Results | `03`, `09`, `13` |
| `TYPE_ERROR` | `invariantId`, `classId`, OCL-Position, Zeile/Spalte | Invariant Properties, OCL Editor, Validation Results | `03`, `09`, `13` |
| `UNKNOWN_CLASS` | `classId` oder fehlender Name | Explorer/Properties Panel, Validation Results | `01`, `04` |
| `UNKNOWN_ATTRIBUTE` | `classId`, `invariantId` | OCL Feedback, Validation Results | `03`, `09` |
| `INVALID_SLOT_VALUE` | `objectId`, `attributeId` | Object Properties Feldfehler, Object Node Badge | `06`, `07` |
| `INVALID_LINK` | `linkId`, `associationId` | Object Link Edge Highlight, Properties Panel | `11`, `12` |
| `MULTIPLICITY_VIOLATION` | `associationId`, `objectId`, `linkId` | Edge/Object Badge, Validation Results | `07`, `12` |
| `INVARIANT_VIOLATION` | `invariantId`, `objectId`, `classId` | roter Rahmen am Objekt, Validation Results | `07` |
| `EVALUATION_ERROR` | `invariantId`, `objectId` | Validation Results, optional Console | `07` |

Beispiel für ein frontendfreundliches Mapping:

```json
{
  "status": "INVALID",
  "summary": {
    "errorCount": 1,
    "warningCount": 0,
    "infoCount": 0
  },
  "errors": [
    {
      "code": "INVARIANT_VIOLATION",
      "severity": "ERROR",
      "message": "Invariant maxBooks is violated for object alice.",
      "targets": [
        { "elementType": "OBJECT", "elementId": "obj-alice" },
        { "elementType": "INVARIANT", "elementId": "inv-max-books" },
        { "elementType": "CLASS", "elementId": "class-user" }
      ],
      "context": {
        "contextClass": "User",
        "contextObject": "alice",
        "expression": "self.books <= 5"
      }
    }
  ]
}
```

## Offene Fragen

| Frage | Betroffener Bereich | Relevanz |
|---|---|---|
| Liegen Screenshots langfristig unter `assets/screenshots/` oder bleiben sie im aktuellen `assets/`-Ordner? | Dokumentation, Build, Links | Mittel |
| Wird die aktive View über URL-Routing oder nur über internen Tab-State verwaltet? | Routing, Deep Links, Validation Focus | Hoch |
| Werden Änderungen sofort per API gespeichert oder zunächst lokal gesammelt und per Save persistiert? | State Management, API, UX | Hoch |
| Soll das Frontend beim Erstellen von Associations und Object Links nur gültige Auswahlmöglichkeiten anbieten? | Forms, Backend-Validierung | Hoch |
| Soll OCL im Modal live gegen Backend parse/typecheck geprüft werden? | OCL UI, API Last, Feedback | Mittel |
| Welche `.use`-Syntax aus den Beispielen wird im MVP unterstützt? | OCL Editor Umsetzung, API-Zuschnitt | Hoch |
| Werden Validation Results nach jeder Modelländerung automatisch als stale markiert? | Validation UI, UX | Hoch |
| Wie detailliert muss Edge Labeling für Rollen, Multiplizitäten und Fehler im MVP sein? | Diagrammkomponenten | Mittel |
| Gibt es im MVP ein Add-Object-Modal, obwohl kein Screenshot dafür vorhanden ist? | Object Diagram Scope | Hoch |

## Zusammenfassung

Die Screenshots decken den vollständigen Frontend-MVP ab: Klassendiagramm, Objektdiagramm, Properties Panel, Explorer, Modals, OCL-Invarianten, Check Constraints und Validation Results. Für die Umsetzung sind besonders stabile IDs, getrennte DTO-/State-Modelle, ein zentrales Selection Model, persistierbare Layoutdaten und ein robustes Validation Error Mapping entscheidend.

Für den MVP sollte die Implementierung zuerst folgende vertikale Kette absichern:

```text
Projekt laden
-> Klassendiagramm anzeigen
-> Klassen/Associations/Invarianten erstellen
-> Objektdiagramm anzeigen
-> Objekte/Links bearbeiten
-> Check Constraints
-> Validation Results anzeigen
-> betroffene Diagrammelemente markieren und fokussieren
```

Damit wird die visuelle Zielvorstellung aus den Screenshots direkt in Komponenten, State, API und Akzeptanzkriterien überführt.
