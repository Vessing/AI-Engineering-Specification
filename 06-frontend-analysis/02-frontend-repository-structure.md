# Frontend Repository Structure

## Zweck dieser Datei

Diese Datei schlägt eine sinnvolle Repository- und Ordnerstruktur für das neue React/TypeScript-Frontend des webbasierten UML/OCL-Systems vor.

Sie beschreibt:

- wie das Frontend-Repository grundsätzlich aufgebaut sein soll,
- welche Build- und Tooling-Entscheidungen naheliegen,
- wie Pages, Features, Komponenten, API-Client, DTOs, State Management, Styling, Tests und Mockdaten getrennt werden,
- wo Diagrammkomponenten, Modals und Validation UI liegen,
- wie die Struktur wartbar, testbar und erweiterbar bleibt.

Das Frontend entsteht als eigenes neues Repository. Es wird nicht Teil des originalen USE-Repositories, übernimmt keine alte Desktop-GUI und verwendet den alten USE-Core nicht als Dependency.

## Repository-Ziele

| Ziel | Bedeutung für die Struktur |
|---|---|
| Eigenständiges Frontend-Repository | Klare Trennung von Analyse-Repository, Backend und originalem USE-Projekt. |
| React/TypeScript | Komponenten, Hooks, DTOs und State werden typisiert umgesetzt. |
| Diagrammzentrierte Anwendung | Diagrammkomponenten brauchen eigene Feature-Bereiche für Klassen- und Objektdiagramm. |
| Backend-Anbindung über REST/JSON | API Client und DTOs werden zentral gepflegt. |
| Saubere State-Trennung | Backend-Daten, View State, Selektion, Layout und Validation Results werden getrennt modelliert. |
| Testbarkeit | Unit-, Component-, Integration- und E2E-Tests erhalten klare Orte. |
| Erweiterbarkeit | Post-MVP-Funktionen wie OCL Autocomplete, Undo/Redo oder `.use` Import sollen ergänzbar bleiben. |

## Top-Level-Struktur

Empfohlene Top-Level-Struktur für ein neues Frontend-Repository:

```text
use-web-frontend/
├─ README.md
├─ package.json
├─ package-lock.json
├─ vite.config.ts
├─ tsconfig.json
├─ tsconfig.app.json
├─ tsconfig.node.json
├─ eslint.config.js
├─ prettier.config.js
├─ index.html
├─ public/
│  ├─ favicon.svg
│  └─ mock-assets/
├─ src/
│  ├─ app/
│  ├─ pages/
│  ├─ features/
│  ├─ components/
│  ├─ api/
│  ├─ state/
│  ├─ types/
│  ├─ utils/
│  ├─ styles/
│  ├─ test/
│  ├─ mocks/
│  ├─ main.tsx
│  └─ vite-env.d.ts
├─ tests/
│  ├─ e2e/
│  └─ fixtures/
├─ docs/
│  ├─ frontend-decisions.md
│  └─ api-contract-notes.md
└─ .github/
   └─ workflows/
      └─ ci.yml
```

### Begründung

| Bereich | Zweck |
|---|---|
| `src/app/` | App-Shell, Routing, Layout, Provider und globale Initialisierung. |
| `src/pages/` | Route-nahe Seiten wie Workspace, Project Open und ggf. Settings. |
| `src/features/` | Fachliche Featurebereiche wie Class Diagram, Object Diagram, OCL Editor, Validation. |
| `src/components/` | Wiederverwendbare UI-Komponenten ohne starke Fachlogik. |
| `src/api/` | REST Client, DTOs, API Errors und Mapping. |
| `src/state/` | Globaler App-/Projekt-/UI-State. |
| `src/types/` | Gemeinsame TypeScript-Typen, Domain View Models und IDs. |
| `src/utils/` | Kleine Hilfsfunktionen ohne Frameworkbindung. |
| `src/styles/` | Globale Styles, Design Tokens und Layoutvariablen. |
| `src/test/` | Test Utilities für Component- und Unit-Tests. |
| `src/mocks/` | Mockdaten und Mock API Responses für Entwicklung und Tests. |
| `tests/e2e/` | Browsernahe E2E-Tests, z. B. mit Playwright. |

## Build- und Tooling-Ansatz

### Empfehlung: Vite

Vite ist als Build-Tool für das neue Frontend naheliegend.

| Kriterium | Bewertung |
|---|---|
| React-Unterstützung | Sehr gut, Standard-Setup verfügbar. |
| TypeScript-Unterstützung | Sehr gut. |
| Entwicklungsserver | Schnell und einfach. |
| Testintegration | Gut mit Vitest kombinierbar. |
| Wartbarkeit | Schlankes Setup, breite Community. |
| MVP-Eignung | Hoch. |

Empfohlene Basis:

```text
React + TypeScript + Vite
```

Empfohlene ergänzende Tools:

| Tool | Zweck | MVP |
|---|---|---|
| TypeScript | Statische Typisierung. | Ja |
| ESLint | Codequalität und Regeln. | Ja |
| Prettier | Einheitliche Formatierung. | Ja |
| Vitest | Unit- und Hook-Tests. | Ja |
| React Testing Library | Component Tests. | Ja |
| Playwright | E2E-Tests für Kernworkflow. | Sollte |
| MSW | API Mocking für Frontend-Tests. | Sollte |

## TypeScript-Konfiguration

TypeScript sollte strikt genug konfiguriert werden, damit DTO-Mapping, IDs und Validation Results zuverlässig bleiben.

Empfohlene Prinzipien:

- `strict: true`
- keine impliziten `any`-Typen,
- explizite DTO-Typen für alle API Responses,
- stabile ID-Typen als Type Aliases oder branded types,
- getrennte Typen für Backend-DTOs und Frontend View Models,
- keine fachliche Semantik in untypisierten JSON-Objekten.

Beispiel:

```ts
export type ProjectId = string;
export type UmlClassId = string;
export type ObjectInstanceId = string;

export interface ProjectDto {
  id: ProjectId;
  name: string;
  umlModel: UmlModelDto;
  objectModel: ObjectModelDto;
}

export interface DiagramSelection {
  kind: "class" | "association" | "invariant" | "object" | "objectLink";
  id: string;
}
```

## Source-Struktur

Empfohlene `src/`-Struktur:

```text
src/
├─ app/
│  ├─ App.tsx
│  ├─ AppProviders.tsx
│  ├─ AppRoutes.tsx
│  ├─ AppLayout.tsx
│  └─ navigation.ts
├─ pages/
│  ├─ workspace/
│  │  ├─ WorkspacePage.tsx
│  │  └─ WorkspacePage.test.tsx
│  ├─ project-open/
│  │  └─ ProjectOpenPage.tsx
│  └─ not-found/
│     └─ NotFoundPage.tsx
├─ features/
│  ├─ class-diagram/
│  ├─ object-diagram/
│  ├─ ocl-editor/
│  ├─ explorer/
│  ├─ properties-panel/
│  ├─ validation/
│  ├─ console/
│  └─ project/
├─ components/
│  ├─ ui/
│  ├─ layout/
│  ├─ forms/
│  └─ feedback/
├─ api/
├─ state/
├─ types/
├─ utils/
├─ styles/
├─ test/
└─ mocks/
```

## Feature-Struktur

Featurebereiche bündeln fachliche UI-Komponenten, Hooks, lokale Typen und Tests.

```text
src/features/
├─ class-diagram/
│  ├─ components/
│  ├─ hooks/
│  ├─ modals/
│  ├─ types.ts
│  ├─ classDiagramMapper.ts
│  └─ index.ts
├─ object-diagram/
│  ├─ components/
│  ├─ hooks/
│  ├─ modals/
│  ├─ types.ts
│  ├─ objectDiagramMapper.ts
│  └─ index.ts
├─ ocl-editor/
│  ├─ components/
│  ├─ hooks/
│  ├─ types.ts
│  └─ index.ts
├─ validation/
│  ├─ components/
│  ├─ hooks/
│  ├─ validationMapping.ts
│  ├─ types.ts
│  └─ index.ts
├─ explorer/
├─ properties-panel/
├─ console/
└─ project/
```

| Feature | Zweck | Typische Inhalte |
|---|---|---|
| `class-diagram` | Klassendiagramm-Canvas und Klassenmodell-Interaktion. | `ClassDiagramView`, `ClassNode`, `AssociationEdge`, Add-Modals. |
| `object-diagram` | Objektdiagramm und Snapshot-Interaktion. | `ObjectDiagramView`, `ObjectNode`, `ObjectLinkEdge`, Object-Link-Modal. |
| `ocl-editor` | OCL-Invarianten anzeigen und bearbeiten. | `OclEditorView`, `OclExpressionInput`, Diagnostics. |
| `validation` | Validation Results und Fehler-Mapping. | `ValidationResultsPanel`, `ValidationErrorItem`, `ValidationBadge`. |
| `explorer` | Sidebar für Modell- und Snapshot-Navigation. | `ExplorerSidebar`, Tree Items, Create Actions. |
| `properties-panel` | Kontextabhängige Detailbearbeitung. | Class, Association, Invariant, Object und Link Panels. |
| `project` | Projektaktionen und Projektstatus. | Load/Save, Import/Export, Project Header. |
| `console` | UI-Protokollierung. | Console Panel, Log Entries. |

## Komponentenstruktur

Wiederverwendbare UI-Komponenten sollten von fachlichen Feature-Komponenten getrennt bleiben.

```text
src/components/
├─ ui/
│  ├─ Button.tsx
│  ├─ IconButton.tsx
│  ├─ Tabs.tsx
│  ├─ Tooltip.tsx
│  ├─ Dialog.tsx
│  ├─ Select.tsx
│  ├─ TextInput.tsx
│  └─ Badge.tsx
├─ layout/
│  ├─ TopBar.tsx
│  ├─ Sidebar.tsx
│  ├─ SplitPane.tsx
│  ├─ BottomPanel.tsx
│  └─ PanelHeader.tsx
├─ forms/
│  ├─ Field.tsx
│  ├─ FieldError.tsx
│  ├─ TypeSelect.tsx
│  └─ MultiplicityInput.tsx
└─ feedback/
   ├─ LoadingState.tsx
   ├─ EmptyState.tsx
   └─ ErrorMessage.tsx
```

### Diagrammkomponenten

Diagrammkomponenten sollten nicht im allgemeinen `components/`-Ordner liegen, sondern in den jeweiligen Features.

```text
src/features/class-diagram/components/
├─ ClassDiagramView.tsx
├─ ClassDiagramCanvas.tsx
├─ ClassNode.tsx
├─ AssociationEdge.tsx
├─ InvariantBadge.tsx
├─ DiagramToolbar.tsx
└─ ClassDiagramView.test.tsx

src/features/object-diagram/components/
├─ ObjectDiagramView.tsx
├─ ObjectDiagramCanvas.tsx
├─ ObjectNode.tsx
├─ ObjectLinkEdge.tsx
├─ ValidationBadge.tsx
├─ InvalidObjectHighlight.tsx
└─ ObjectDiagramView.test.tsx
```

Begründung:

- Klassen- und Objektdiagramme haben ähnliche Canvas-Anforderungen, aber unterschiedliche fachliche Elemente.
- Eine spätere gemeinsame Diagrammabstraktion kann in `src/features/diagram-core/` oder `src/components/diagram/` entstehen, wenn echte Wiederverwendung sichtbar wird.
- Die konkrete Diagrammbibliothek ist noch offen und sollte in `06-diagram-library-decision.md` entschieden werden.

## API-Client und DTOs

API-Code sollte zentral liegen, damit REST-Endpunkte, DTOs und Fehlerformate konsistent bleiben.

```text
src/api/
├─ client/
│  ├─ httpClient.ts
│  ├─ apiError.ts
│  └─ requestConfig.ts
├─ dtos/
│  ├─ project.dto.ts
│  ├─ uml.dto.ts
│  ├─ objectModel.dto.ts
│  ├─ ocl.dto.ts
│  ├─ validation.dto.ts
│  └─ error.dto.ts
├─ services/
│  ├─ projectApi.ts
│  ├─ umlModelApi.ts
│  ├─ objectModelApi.ts
│  ├─ oclApi.ts
│  └─ validationApi.ts
├─ mappers/
│  ├─ projectMapper.ts
│  ├─ umlMapper.ts
│  ├─ objectModelMapper.ts
│  └─ validationMapper.ts
└─ index.ts
```

| Bereich | Zweck |
|---|---|
| `client/` | Technische HTTP-Schicht, JSON Parsing, Fehlernormalisierung. |
| `dtos/` | Exakte TypeScript-Repräsentation der Backend-DTOs. |
| `services/` | Fachlich benannte API-Funktionen pro Backend-Bereich. |
| `mappers/` | Umwandlung zwischen DTOs und frontendnahen View Models. |

### DTO vs View Model

Backend-DTOs und Frontend View Models sollten getrennt bleiben.

```text
Backend DTO
  -> API Mapper
  -> Frontend View Model
  -> UI State
```

Beispiel:

```ts
export interface ValidationErrorDto {
  code: string;
  severity: "INFO" | "WARNING" | "ERROR";
  message: string;
  elementType?: string;
  elementId?: string;
}

export interface ValidationErrorViewModel {
  code: string;
  severity: "info" | "warning" | "error";
  title: string;
  details: string;
  target?: DiagramTarget;
}
```

## State Management

Der Frontend-State besteht aus mehreren klar getrennten Bereichen.

| State-Bereich | Inhalt | Persistenz |
|---|---|---|
| Project Data State | Projekt, UML-Modell, Object Model, Invarianten. | Backend/JSON |
| Layout State | Node-Positionen, Zoom, Pan, Panelgrößen. | Projektformat oder lokal |
| Selection State | Aktuell selektiertes Diagramm-/Explorer-Element. | Lokal |
| Editor State | Offene Tabs, OCL Editor Inhalt, Dirty Flags. | Lokal und optional Projekt |
| Modal State | Offene Modals und Formularzustände. | Lokal |
| Validation State | Letztes ValidationResult, Fehlerfilter, ausgewählter Fehler. | Lokal, optional temporär im Projekt |
| Async State | Loading, Saving, API Errors. | Lokal |

Empfohlene Struktur:

```text
src/state/
├─ appStore.ts
├─ projectStore.ts
├─ selectionStore.ts
├─ layoutStore.ts
├─ validationStore.ts
├─ uiStore.ts
└─ selectors.ts
```

Mögliche Technologien:

| Option | Eignung | Hinweis |
|---|---|---|
| Zustand | Hoch | Schlank, gut für UI- und Projekt-State. |
| Redux Toolkit | Hoch | Strukturiert, gut bei größerem Team und komplexer Historie. |
| React Context + Hooks | Mittel | Für MVP möglich, kann bei Diagramm-State unübersichtlich werden. |
| TanStack Query | Hoch für Server State | Sehr geeignet für API Fetching und Cache; nicht Ersatz für Diagramm-UI-State. |

Annahme: Für den MVP ist eine Kombination aus TanStack Query für Server State und Zustand oder Redux Toolkit für UI-/Diagramm-State sinnvoll.

## Styling-Struktur

Die Styling-Struktur sollte konsistente Layouts und wiederverwendbare UI-Bausteine unterstützen.

```text
src/styles/
├─ globals.css
├─ tokens.css
├─ layout.css
├─ diagrams.css
└─ themes.css
```

| Datei | Zweck |
|---|---|
| `globals.css` | Reset, Basis-Typografie, globale Regeln. |
| `tokens.css` | Farben, Abstände, Schriftgrößen, Border-Radien, Shadows. |
| `layout.css` | App-Shell, Panels, Split Layout. |
| `diagrams.css` | Gemeinsame Styles für Diagramm-Nodes, Edges und Fehlerzustände. |
| `themes.css` | Spätere Theme-Varianten. |

Die konkrete Styling-Technologie ist offen. Geeignete Optionen:

| Option | Eignung | Bemerkung |
|---|---|---|
| CSS Modules | Hoch | Einfach, komponentennah, gut testbar. |
| Plain CSS mit Tokens | Hoch | Für MVP ausreichend und transparent. |
| Tailwind CSS | Mittel bis hoch | Produktiv, aber Designkonsistenz muss streng geführt werden. |
| CSS-in-JS | Mittel | Zusätzliche Abhängigkeit; nur bei klarer Teampräferenz. |

## Teststruktur

Tests sollten nah an Komponenten liegen, größere E2E-Tests jedoch im `tests/`-Ordner.

```text
src/
├─ features/
│  ├─ class-diagram/
│  │  ├─ components/
│  │  │  ├─ ClassNode.tsx
│  │  │  └─ ClassNode.test.tsx
│  │  └─ classDiagramMapper.test.ts
│  ├─ validation/
│  │  ├─ validationMapping.ts
│  │  └─ validationMapping.test.ts
│  └─ object-diagram/
│     └─ components/
│        ├─ ObjectNode.tsx
│        └─ ObjectNode.test.tsx
├─ api/
│  ├─ mappers/
│  │  └─ validationMapper.test.ts
│  └─ services/
│     └─ projectApi.test.ts
└─ test/
   ├─ renderWithProviders.tsx
   ├─ testProjectFactory.ts
   └─ mockServer.ts

tests/
├─ e2e/
│  ├─ library-workflow.spec.ts
│  ├─ validation-error.spec.ts
│  └─ class-diagram.spec.ts
└─ fixtures/
   ├─ library-project.json
   └─ invalid-library-project.json
```

| Testart | Ziel | MVP |
|---|---|---|
| Unit Tests | Mapper, Reducer/Stores, Utility-Funktionen. | Ja |
| Component Tests | Nodes, Panels, Modals, Validation Results. | Ja |
| API Mock Tests | API Client und Fehlernormalisierung. | Ja |
| Integration Tests | Workflow innerhalb der App-Shell mit Mock API. | Sollte |
| E2E Tests | Library-Workflow im Browser. | Sollte |
| Visual Regression | Diagramm- und Layoutvergleich. | Post-MVP |

## Mockdaten

Mockdaten sind notwendig für Entwicklung, Storybook-ähnliche Komponentenarbeit und Tests.

```text
src/mocks/
├─ projects/
│  ├─ libraryProject.ts
│  ├─ emptyProject.ts
│  └─ invalidLibraryProject.ts
├─ validation/
│  ├─ validResult.ts
│  ├─ invariantViolationResult.ts
│  └─ multiplicityViolationResult.ts
├─ handlers/
│  ├─ projectHandlers.ts
│  ├─ validationHandlers.ts
│  └─ oclHandlers.ts
└─ browser.ts
```

| Mock | Zweck |
|---|---|
| `libraryProject` | Zentraler MVP-Demozustand mit `User`, `Book`, `Borrows` und Invariante. |
| `invalidLibraryProject` | Fehlerzustand für `alice.books = 6`. |
| `invariantViolationResult` | Testdaten für Validation Results Panel und Object Highlighting. |
| `multiplicityViolationResult` | Testdaten für Link-/Association-Fehler. |
| `projectHandlers` | Mock Service Worker Handler für API-Entwicklung ohne Backend. |

## Beispielhafte Dateiübersicht

Eine konkrete MVP-Dateiübersicht könnte so aussehen:

```text
src/features/class-diagram/
├─ components/
│  ├─ ClassDiagramView.tsx
│  ├─ ClassDiagramCanvas.tsx
│  ├─ ClassNode.tsx
│  ├─ AssociationEdge.tsx
│  └─ InvariantBadge.tsx
├─ modals/
│  ├─ AddClassModal.tsx
│  ├─ AddAssociationModal.tsx
│  └─ AddInvariantModal.tsx
├─ hooks/
│  ├─ useClassDiagramSelection.ts
│  └─ useClassDiagramLayout.ts
├─ classDiagramMapper.ts
├─ types.ts
└─ index.ts

src/features/object-diagram/
├─ components/
│  ├─ ObjectDiagramView.tsx
│  ├─ ObjectDiagramCanvas.tsx
│  ├─ ObjectNode.tsx
│  ├─ ObjectLinkEdge.tsx
│  ├─ ValidationBadge.tsx
│  └─ InvalidObjectHighlight.tsx
├─ modals/
│  ├─ AddObjectModal.tsx
│  └─ AddObjectAssociationModal.tsx
├─ hooks/
│  ├─ useObjectDiagramSelection.ts
│  └─ useObjectDiagramLayout.ts
├─ objectDiagramMapper.ts
├─ types.ts
└─ index.ts

src/features/properties-panel/
├─ PropertiesPanel.tsx
├─ ClassPropertiesPanel.tsx
├─ AssociationPropertiesPanel.tsx
├─ InvariantPropertiesPanel.tsx
├─ ObjectPropertiesPanel.tsx
└─ ObjectLinkPropertiesPanel.tsx

src/features/validation/
├─ components/
│  ├─ ValidationResultsPanel.tsx
│  ├─ ValidationErrorItem.tsx
│  ├─ ValidationErrorDetails.tsx
│  └─ ValidationBadge.tsx
├─ hooks/
│  └─ useValidationResults.ts
├─ validationMapping.ts
├─ types.ts
└─ index.ts
```

## Abgrenzung zum Backend

| Thema | Frontend-Repository | Backend-Repository |
|---|---|---|
| React-Komponenten | Ja | Nein |
| Diagramm-Rendering | Ja | Nein |
| UI-State und Selektion | Ja | Nein |
| API Client | Ja | Nein |
| Backend-DTO-Definition als TypeScript-Typ | Ja, als Client-Vertrag | Nein, Backend definiert Java DTOs |
| Domänenlogik | Nur View Models und UI-nahe Ableitungen | Ja |
| UML-/OCL-Validierung | Nein, nur UI-Vorvalidierung | Ja |
| OCL Parser/Typechecker/Evaluator | Nein | Ja |
| Projektpersistenz | Nein, nur API-Aufruf | Ja |
| Layoutdaten | UI erzeugt und bearbeitet | Backend speichert als Projektbestandteil |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Welche Diagrammbibliothek wird gewählt? | Beeinflusst Ordnerstruktur für Nodes, Edges, Layout und Tests. |
| Wird TanStack Query eingesetzt? | Beeinflusst API- und State-Struktur. |
| Zustand oder Redux Toolkit? | Beeinflusst Stores, DevTools und Undo/Redo-Fähigkeit. |
| Wird Storybook genutzt? | Kann für Nodes, Panels und Modals wertvoll sein, ist aber kein MVP-Muss. |
| Werden DTOs manuell gepflegt oder aus OpenAPI generiert? | Beeinflusst `src/api/dtos/` und Build-Prozess. |
| Wie werden Design Tokens definiert? | Beeinflusst Styling, Konsistenz und spätere Themes. |
| Wird `.use` Import/Export im Frontend nur über Backend-Endpunkte oder lokal vorbereitet? | Sollte Backend-getrieben bleiben. |

## Zusammenfassung

Das neue Frontend sollte als eigenständiges React/TypeScript-Repository mit Vite aufgebaut werden. Die Struktur trennt App-Shell, Pages, fachliche Features, wiederverwendbare UI-Komponenten, API Client, DTOs, State Management, Styles, Tests und Mockdaten.

Die wichtigsten fachlichen Featurebereiche sind:

- `class-diagram`,
- `object-diagram`,
- `ocl-editor`,
- `explorer`,
- `properties-panel`,
- `validation`,
- `project`,
- `console`.

Diagrammkomponenten gehören in die jeweiligen Diagramm-Features, während API-DTOs und Mapper zentral unter `src/api/` liegen. Backend-DTOs und Frontend View Models bleiben getrennt. Tests werden komponentennah organisiert; vollständige Workflows liegen als E2E-Tests unter `tests/e2e/`.

Diese Struktur unterstützt den MVP-Workflow aus Klassendiagramm, Objektdiagramm, OCL-Invarianten und Backend-basierter Validierung, ohne das Frontend mit Backend-Semantik oder alter USE-Technik zu vermischen.
