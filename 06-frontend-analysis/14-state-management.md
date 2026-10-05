# State Management

## Zweck dieser Datei

Diese Datei beschreibt das State-Management-Konzept des React/TypeScript-Frontends. Sie legt fest, welche Zustände aus dem Backend kommen, welche Zustände UI-lokal sind, welche Daten persistiert werden und welche Daten nur temporär für Interaktion, Selektion, Layout, Validierung und Konsolenanzeige existieren.

Das Ziel ist ein klares, wartbares State-Modell für eine Anwendung mit Diagramm-Canvas, Properties Panel, Explorer Sidebar, OCL Editor, Modals, Validation Results und REST/JSON-Backend.

## State-Kategorien

| Kategorie | Beispiele | Quelle | Persistenz | Empfohlene Verwaltung |
|---|---|---|---|---|
| Server State | Projekt, UML-Modell, Objektmodell, Invarianten, gespeichertes Layout | Backend | Backend/Projektformat | TanStack Query |
| Client/UI State | aktive View, Panelzustand, Sidebar-Gruppen, Hover | Frontend | lokal, optional Browser Storage | Zustand |
| Diagram State | Viewport, Drag-Zustand, temporäre Edge-Erstellung | Frontend | teils temporär, Layout teils persistiert | Zustand + Diagrammkomponente |
| Selection State | selektierte Klasse, Association, Invariante, Objekt, Link | Frontend | temporär | Zustand |
| Modal State | geöffnetes Modal, initiale Kontextwerte | Frontend | temporär | Zustand oder lokales `useState` |
| Validation State | letztes `ValidationResult`, Fehler-Mapping, stale-Status | Backend + Frontend-Mapping | temporär, optional nicht persistiert | TanStack Mutation + Zustand |
| API State | Loading, Error, Mutation Status | API Client | temporär | TanStack Query |
| Console State | UI-/Validierungslogs | Frontend | temporär, optional Session | Zustand |

Grundregel: Fachliche Daten liegen im Project State und kommen vom Backend. UI-Zustände leiten sich daraus ab oder referenzieren fachliche Elemente über stabile IDs.

## Server State

Server State umfasst Daten, deren fachliche Quelle das Backend ist.

| Datenbereich | Beispiele | Persistiert |
|---|---|---|
| Projektmetadaten | `projectId`, Name, Version, Formatversion | Ja |
| UML-Modell | Klassen, Attribute, Operationen, Associations, Rollen, Multiplizitäten | Ja |
| OCL-Modell | Invarianten mit Kontextklasse und Ausdruck | Ja |
| Objektmodell | Objekte, Slots, Objektlinks, Snapshot | Ja |
| Layoutdaten | Positionen von Klassen und Objekten, Viewport optional | Ja |
| Validation Results | letzte Constraint-Ergebnisse | Im MVP eher Nein |

TanStack Query ist für Server State geeignet, weil es laut offizieller Dokumentation auf Fetching, Caching, Synchronisierung und Aktualisierung von Server State ausgerichtet ist und typische Probleme wie Caching, Deduplizierung, Stale-Daten und Hintergrundaktualisierung adressiert. Siehe Quellen am Ende dieser Datei.

Typische Query Keys:

```ts
const projectKeys = {
  all: ["projects"] as const,
  detail: (projectId: ProjectId) => ["projects", projectId] as const,
  validation: (projectId: ProjectId) => ["projects", projectId, "validation"] as const
};
```

## Client/UI State

Client/UI State ist nur für die Bedienung der Oberfläche relevant.

| State | Zweck | Persistieren? |
|---|---|---|
| `activeView` | `class-diagram`, `object-diagram`, `ocl` | URL oder lokal |
| `activeBottomPanelTab` | `console`, `validation-results` | lokal |
| `expandedExplorerGroups` | offene Sidebar-Gruppen | optional lokal |
| `propertiesPanelSegment` | aktiver Abschnitt im Properties Panel | lokal |
| `hoveredElement` | Hover im Diagramm | Nein |
| `focusedFromValidation` | Element nach Klick auf Fehler | Nein |
| `validationResultStale` | zeigt veraltete Ergebnisse nach Änderungen | Nein |

Dieser State sollte nicht im Backend gespeichert werden, außer es entsteht später ein bewusstes Workspace-/Preference-Konzept.

## Diagram State

Diagram State ist teils temporär und teils persistierbar.

| State | Beschreibung | Persistenz |
|---|---|---|
| Node-Positionen | Positionen von Klassen und Objekten | Ja, als Layoutdaten |
| Viewport | Pan/Zoom je Diagramm | Optional |
| Drag State | aktuell verschobenes Element | Nein |
| Temporary Edge | laufende Verbindungserstellung | Nein |
| Selection Highlight | sichtbare Markierung ausgewählter Elemente | Nein, abgeleitet |
| Validation Markers | Fehlerrahmen und Badges | Nein, aus Validation State abgeleitet |

Layoutdaten sind keine UML-Semantik. Sie werden getrennt vom fachlichen Modell gespeichert, referenzieren aber stabile Element-IDs.

Beispiel:

```ts
type LayoutState = {
  classDiagram: {
    nodePositions: Record<UmlClassId, XYPosition>;
    viewport?: ViewportState;
  };
  objectDiagram: {
    nodePositions: Record<ObjectInstanceId, XYPosition>;
    viewport?: ViewportState;
  };
};
```

## Selection State

Selection State steuert Canvas, Explorer Sidebar, Properties Panel und Fehlernavigation.

```ts
type Selection =
  | { view: "class-diagram"; type: "class"; id: UmlClassId }
  | { view: "class-diagram"; type: "association"; id: UmlAssociationId }
  | { view: "class-diagram"; type: "invariant"; id: UmlInvariantId }
  | { view: "object-diagram"; type: "object"; id: ObjectInstanceId }
  | { view: "object-diagram"; type: "objectLink"; id: ObjectLinkId }
  | { view: "ocl"; type: "invariant"; id: UmlInvariantId }
  | null;
```

Regeln:

- Selektion enthält nur IDs und Typen, keine kopierten Objekte.
- Canvas, Explorer und Properties Panel lesen denselben Selection State.
- Bei gelöschtem Element wird die Selektion zurückgesetzt.
- Klick auf Validation Result kann die View wechseln und Selektion setzen.
- OCL Editor und Class Diagram müssen Invarianten-Selektion gemeinsam verstehen.

## Validation State

Validation State speichert das letzte Backend-Ergebnis und daraus abgeleitete UI-Mappings.

```ts
type ValidationState = {
  result: ValidationResultDto | null;
  stale: boolean;
  selectedErrorId: string | null;
  markersByElementId: Record<string, ValidationMarker[]>;
  lastCheckedAt?: string;
};

type ValidationMarker = {
  errorId: string;
  code: ValidationErrorCode;
  severity: "error" | "warning" | "info";
  targetType: "class" | "association" | "invariant" | "object" | "objectLink" | "slot";
};
```

Mapping-Regeln:

| Backend-Feld | UI-Mapping |
|---|---|
| `classIds` | Class Nodes, Explorer Classes, Class Properties |
| `associationIds` | Association Edges, Explorer Associations |
| `invariantId` | Invariant Badges, OCL Editor, Invariant Properties |
| `objectIds` | Object Nodes, Explorer Objects |
| `linkIds` | ObjectLink Edges, Explorer Object Links |
| `contextObjectId` | primärer Fokus bei Invariantverletzung |
| `sourceRange` | OCL Editor Source Marker, Post-MVP |

Nach Änderungen am Projektzustand sollte `stale = true` gesetzt werden, bis `Check Constraints` erneut ausgeführt wurde.

## Modal State

Modal State hält nur den aktuell geöffneten Dialog und optionale Initialwerte.

```ts
type ModalState =
  | { type: "addClass"; initialPosition?: XYPosition }
  | { type: "addInvariant"; contextClassId?: UmlClassId }
  | { type: "addClassAssociation"; sourceClassId?: UmlClassId; targetClassId?: UmlClassId }
  | { type: "addObject"; classId?: UmlClassId }
  | { type: "addObjectAssociation"; associationId?: UmlAssociationId; sourceObjectId?: ObjectInstanceId }
  | null;
```

Formularwerte können im Modal lokal per `useState` oder `useReducer` liegen. Nur wenn ein Draft über mehrere Komponenten hinweg geteilt wird, sollte er in einen Store.

## Layout State

Layout State wird aus Diagrammaktionen aktualisiert und später als Teil des Projektformats gespeichert.

Empfohlene Strategie:

- Drag & Drop aktualisiert sofort lokalen Layout State.
- Canvas rendert aus Layout State.
- Persistenz erfolgt über Projekt-Save oder debounced Layout-Save.
- Fachliche Modelländerungen und Layoutänderungen bleiben getrennt.
- Gelöschte Elemente entfernen zugehörige Layoutdaten.

```ts
type LayoutPatch = {
  diagram: "classDiagram" | "objectDiagram";
  elementId: string;
  position: XYPosition;
};
```

## Console State

Console Logs sind UI-nahe Ereignisse, keine fachliche Projektdatenquelle.

| Log-Typ | Beispiel |
|---|---|
| API | `Project saved successfully.` |
| Validation | `Check Constraints finished: 1 error.` |
| OCL | `Typecheck failed for maxBooks.` |
| Import/Export | `Project JSON exported.` |
| Debug | technische Details, optional nur Dev Mode |

```ts
type ConsoleLogEntry = {
  id: string;
  timestamp: string;
  level: "info" | "warning" | "error" | "debug";
  source: "api" | "validation" | "ocl" | "ui";
  message: string;
};
```

## API Loading und Error State

API Loading und technische Fehler sollten nicht manuell in vielen Komponenten dupliziert werden.

| Zustand | Verwaltung |
|---|---|
| Projekt lädt | TanStack Query `isPending` / `isLoading` |
| Projekt konnte nicht geladen werden | TanStack Query `error` |
| Save läuft | Mutation State |
| Constraint Check läuft | Mutation State |
| technische API-Fehler | API Error View Model |
| fachliche Validation Errors | `ValidationResult`, nicht API Error |

Wichtig: Eine verletzte Invariante ist kein technischer API-Fehler. Sie ist ein fachliches Ergebnis und gehört ins Validation Results Panel.

## State-Management-Optionen

| Option | Vorteile | Nachteile | Eignung für dieses Projekt |
|---|---|---|---|
| `useState` / `useReducer` | keine zusätzliche Bibliothek, gut für lokale Formulare und kleine Komponenten | schwer bei globaler Synchronisation von Canvas, Explorer, Panel, Validation | Gut lokal, nicht als Gesamtstrategie |
| Context API | offiziell, geeignet für begrenzte globale Werte | kann bei häufigen Updates schnell unübersichtlich werden; Provider-Struktur wächst | Gut für Theme/Config, nicht ideal für Diagramm-/Selection-State |
| Zustand | kleiner Store, hook-basiert, wenig Boilerplate, selektive Subscriptions | weniger strikt als Redux, Disziplin bei Store-Struktur nötig | Sehr gut für UI-, Selection-, Layout- und Modal-State |
| Redux Toolkit | sehr robust, DevTools, klare Actions/Reducer, Entity Adapter | mehr Setup und Boilerplate als Zustand | Gut bei sehr großem Team oder komplexem Audit/Undo; für MVP eher schwergewichtig |
| TanStack Query | stark für Server State, Caching, Loading/Error, Mutations, Invalidierung | kein UI-State-Store für Selektion, Modals, Canvas | Sehr gut für Backend-/Projektzustand |
| TanStack Query + Zustand | klare Trennung Server State und UI State | zwei Konzepte müssen sauber abgegrenzt werden | Empfehlung |

## Empfehlung

Empfehlung für den MVP: **TanStack Query + Zustand**.

Begründung:

- TanStack Query verwaltet Backend-Daten, Laden, Caching, Mutations und Stale State.
- Zustand verwaltet UI-lokale Zustände wie Selektion, Modals, Layout-Drafts, Validation Mapping und Console Logs.
- React `useState`/`useReducer` bleibt für rein lokale Formularzustände sinnvoll.
- Context API wird nur für statische App-Konfiguration oder Provider genutzt.
- Redux Toolkit wird nicht als MVP-Standard empfohlen, weil die Anwendung zwar komplexe UI-Zustände hat, aber nicht zwingend eine globale Redux-Action-Historie benötigt.

Abgrenzung:

| Zustand | Tool |
|---|---|
| Projekt laden/speichern | TanStack Query |
| CRUD-Mutations für Klassen, Objekte, Links | TanStack Query Mutations |
| aktives Diagramm, Selektion, Modal | Zustand |
| Layout Drafts | Zustand, Persistenz über Mutation |
| Validation Result Response | Mutation liefert Ergebnis; Mapping in Zustand |
| Formularfelder in Modal/Panel | lokales `useState`/`useReducer`, bei Bedarf Zustand |
| Console Logs | Zustand |

## Beispiel-State-Struktur

```ts
// Server State über TanStack Query
function useProject(projectId: ProjectId) {
  return useQuery({
    queryKey: ["projects", projectId],
    queryFn: () => projectApi.getProject(projectId)
  });
}

function useValidateProject(projectId: ProjectId) {
  const setValidationResult = useUiStore((s) => s.setValidationResult);

  return useMutation({
    mutationFn: () => validationApi.validateProject(projectId),
    onSuccess: (result) => setValidationResult(result)
  });
}
```

```ts
// Client/UI State über Zustand
type UiStore = {
  activeView: "class-diagram" | "object-diagram" | "ocl";
  selection: Selection;
  modal: ModalState;
  layoutDraft: LayoutState;
  validation: ValidationState;
  consoleLogs: ConsoleLogEntry[];

  setActiveView(view: UiStore["activeView"]): void;
  select(selection: Selection): void;
  openModal(modal: ModalState): void;
  closeModal(): void;
  updateLayout(patch: LayoutPatch): void;
  setValidationResult(result: ValidationResultDto): void;
  markValidationStale(): void;
  addConsoleLog(entry: ConsoleLogEntry): void;
};
```

Abgeleitete View Models sollten über Selector-Funktionen entstehen, nicht als duplizierter State:

```ts
function buildClassDiagramViewModel(
  project: ProjectDto,
  layout: LayoutState,
  validation: ValidationState,
  selection: Selection
): ClassDiagramViewModel {
  // erzeugt Nodes, Edges, Markierungen und Selection Flags aus vorhandenen Daten
}
```

## Synchronisation mit Backend

Updates an das Backend sollten über API-Client und TanStack Mutations laufen.

| Aktion | Frontend-Ablauf |
|---|---|
| Klasse erstellen | Mutation ausführen, Query Cache aktualisieren oder Projekt neu laden, neue Klasse selektieren |
| Attribut ändern | lokaler Draft, Save-Mutation, Validation stale setzen |
| Objekt verschieben | Layout Draft aktualisieren, später Layout/Projekt speichern |
| Slot-Wert ändern | lokaler Draft, Mutation, Validation stale setzen |
| Invariante ändern | Draft speichern, optional parse/typecheck, Validation stale setzen |
| Element löschen | Delete-Mutation ausführen, aktualisiertes `ProjectDto` übernehmen oder Projekt neu laden, Selection/Layout/Validation Targets bereinigen |
| Check Constraints | Validate-Mutation, Validation State ersetzen, Bottom Panel öffnen |
| Projekt laden | Query lädt ProjectDto, UI Store behält nur UI-State |

MVP-Strategie:

- Für größere Änderungen kann zunächst der komplette `ProjectDto` gespeichert werden.
- Feingranulare Mutations sind sinnvoll, wenn Backend-Endpunkte stabil sind.
- Nach jeder fachlichen Änderung werden Validation Results als veraltet markiert.
- Nach erfolgreichem Save kann Query Cache aktualisiert oder invalidiert werden.

## MVP-Anforderungen

| ID | Anforderung | Priorität |
|---|---|---|
| `STATE-MVP-001` | Projektzustand wird als Server State aus dem Backend geladen. | MVP |
| `STATE-MVP-002` | UI State ist von Backend-DTOs getrennt. | MVP |
| `STATE-MVP-003` | Selection State synchronisiert Canvas, Explorer, Properties Panel und Validation Results. | MVP |
| `STATE-MVP-004` | Layout State speichert Positionen getrennt von fachlicher Semantik. | MVP |
| `STATE-MVP-005` | Validation Results werden strukturiert gespeichert und auf Element-IDs gemappt. | MVP |
| `STATE-MVP-006` | API Loading/Error State wird zentral behandelt. | MVP |
| `STATE-MVP-007` | Modals verwenden lokalen oder zentralen Modal State. | MVP |
| `STATE-MVP-008` | Änderungen markieren Validation Results als stale. | MVP |
| `STATE-MVP-009` | Console Logs können Validierungs- und API-Ereignisse anzeigen. | Should |
| `STATE-MVP-010` | Delete-Mutations bereinigen Selection, Layout Drafts und Validation Targets für gelöschte IDs. | MVP |

## Post-MVP-Erweiterungen

| Erweiterung | State-Auswirkung |
|---|---|
| Undo/Redo | eigener History State oder Command Log |
| Autosave | debounced Mutations, Konfliktbehandlung |
| mehrere Snapshots | aktiver Snapshot State und Layout je Snapshot |
| Projektversionierung | Version/Revision im Server State |
| Collaboration | Remote-Updates, Konflikte, Presence State |
| OCL Live Typecheck | debounced Query/Mutation je Invariante |
| Source-Range Diagnostics | präziser OCL Editor State |
| Persistierte UI Preferences | lokale Speicherung von Sidebar/Panel/Theme |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Speichert der MVP komplette Projekte oder feingranulare Patches? | Beeinflusst Query-Invalidierung und Optimistic Updates. |
| Wird Layout automatisch oder nur per Save gespeichert? | Beeinflusst Mutations und Dirty State. |
| Werden Validation Results persistiert oder immer neu berechnet? | Beeinflusst Server State und Projektformat. |
| Braucht das MVP Undo/Redo? | Könnte Redux Toolkit oder Command Pattern relevanter machen. |
| Wie groß werden Projekte im typischen Einsatz? | Beeinflusst Normalisierung und Performance. |
| Gibt es mehrere Snapshots bereits im MVP? | Beeinflusst Object Model State. |

## Zusammenfassung

Das Frontend sollte Server State und UI State konsequent trennen. Backend-Daten wie Projekt, UML-Modell, Objektmodell, Invarianten und gespeichertes Layout werden über TanStack Query verwaltet. UI-nahe Zustände wie Selektion, Modals, Layout-Drafts, Validation-Mapping und Console Logs werden über Zustand verwaltet.

Diese Kombination passt zur Anwendung, weil sie asynchronen REST/JSON-Server-State sauber behandelt und gleichzeitig die stark interaktive Diagrammoberfläche mit einem leichten, selektiven UI-Store unterstützt.

## Quellen

- React Docs: Managing State, insbesondere Strukturierung, Reducer und Context: <https://react.dev/learn/managing-state>
- TanStack Query Docs: Server State, Fetching, Caching und Synchronisierung: <https://tanstack.com/query/latest/docs/framework/react/overview>
- Zustand Repository/Docs: hook-basierter, kleiner UI-State-Store: <https://github.com/pmndrs/zustand>
- Redux Toolkit Docs: Standardweg für Redux-Logik und RTK Query: <https://redux-toolkit.js.org/introduction/getting-started>
