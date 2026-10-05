# Frontend Implementation Plan

> **Planungshinweis:** Dieses Dokument bleibt der technische Basis- und
> MVP-Plan. Fuer die Umsetzung des erweiterten Full-OCL/UML-Umfangs ist
> `09-ocl-extension-analysis/17-full-ocl-uml-frontend-implementation-plan.md`
> verbindlich. Die aktuellen Mockup-Dateien und ihre fachlichen
> Analyse-Dokumente stehen in
> `04-ui-ux-analysis/48-mockup-file-naming.md`. Bei Abweichungen haben diese
> beiden neueren Dokumente Vorrang.

## Zweck dieser Datei

Diese Datei beschreibt einen konkreten Implementierungsplan für das neue React/TypeScript-Frontend des webbasierten UML/OCL-Systems.

Der Plan ist nicht nach Epics gegliedert, sondern nach umsetzbaren Frontend-Schritten. Jeder Schritt beschreibt Ziel, Aufgaben, erwartetes Ergebnis, betroffene Komponenten, benötigte API-Daten, benötigten State, Screenshot-Bezug, Abhängigkeiten, Testfälle, Risiken, Akzeptanzkriterien und MVP-Relevanz.

Der erste sichtbare MVP-Workflow startet auf dem Dashboard:

```text
Dashboard
-> Start Project
-> Projektname eingeben
-> Class Diagram
-> Object Diagram
-> OCL / Invariants
-> Check Constraints
-> Validation Results
```

## Frontend-Planungsprinzipien

| Prinzip | Konsequenz für die Umsetzung |
|---|---|
| Dashboard zuerst | Die Anwendung startet nicht im Klassendiagramm, sondern auf der Dashboard / Start Page. |
| Backend als fachliche Quelle | Das Frontend zeigt, editiert und visualisiert; UML/OCL-Semantik und Validation kommen vom Backend. |
| DTOs und Frontend-State trennen | API-DTOs werden typisiert übernommen, aber nicht blind als UI-State missbraucht. |
| Stabile IDs nutzen | Selektion, Properties Panel, Diagramm-Nodes und Validation Errors werden über IDs verbunden. |
| Layoutdaten persistierbar halten | Node-Positionen und optional Edge-/Viewport-Daten werden als Layoutdaten gespeichert. |
| Screenshot-nahe Struktur | Screenshots leiten Komponenten, Journey und Akzeptanz ab, sind aber keine pixelgenaue Spezifikation. |
| Diagrammbibliothek früh entscheiden | Class/Object Diagram sind MVP-kritisch; die Bibliothek darf nicht spät offen bleiben. |
| Fehler-Mapping zentralisieren | Validation Results müssen Diagramm, Panel, OCL Editor und Console konsistent steuern. |

## Schrittübersicht

| Schritt | Thema | Kernziel | MVP-Relevanz |
|---:|---|---|---|
| 1 | Frontend-Projekt initialisieren | React/TypeScript/Vite-Projekt startfähig machen | Hoch |
| 2 | Grundlayout, Routing und App Shell | Dashboard- und Projektworkspace-Routen vorbereiten | Hoch |
| 3 | Dashboard / Start Page | Startseite aus Screenshot umsetzen | Hoch |
| 3a | All Projects Page | Projektliste aus `19-projects.png` für `View all` umsetzen | Should |
| 4 | Projektstart über Start Project | Dashboard mit Backend/Mock-Projektstart verbinden | Hoch |
| 4a | Create New Project Dialog | Projektnamen aus Screenshot `18-create-new-projects.png` erfassen und validieren | Hoch |
| 5 | API Client und DTOs | REST/JSON-Kommunikation typisiert aufbauen | Hoch |
| 5a | Backend Integration Smoke Test | API Client gegen echtes Backend prüfen und Mock-Umschaltung absichern | Hoch |
| 5b | Open Existing `.use` Import Flow | Dashboard-Dialog für lokale `.use` Dateien mit Backend-Apply-Flow verbinden | Hoch/Should |
| 6 | State Management | Server-, UI-, Selection-, Layout- und Validation-State strukturieren | Hoch |
| 7 | Diagrammbibliothek | React Flow prüfen, entscheiden und integrieren | Hoch |
| 8 | Class Diagram View | UML-Klassenmodell visuell bearbeiten | Hoch |
| 9 | Class Properties Panel | Klassen, Associations und Invarianten rechts bearbeiten | Hoch |
| 10 | Modals | Klasse, Association und Invariante erstellen | Hoch |
| 11 | Object Diagram View | Snapshot mit Objekten, Slots und Links anzeigen | Hoch |
| 12 | Object Properties Panel | Objekte und Objektlinks bearbeiten | Hoch |
| 12a | Delete-Aktionen | Klassen, Attribute, Operationen, Associations, Objekte, Links und Invarianten per UI löschen | Hoch |
| 13 | OCL Editor UI | Textuellen Modell-/OCL-Editor mit Apply Changes umsetzen | Hoch |
| 14 | Check Constraints API | Backend-Validierung auslösen | Hoch |
| 15 | Validation Results UI | Fehlerliste und Details anzeigen | Hoch |
| 16 | Fehler-Markierung im Diagramm | Objekte/Links/Elemente anhand Validation Results markieren | Hoch |
| 17 | End-to-End Library Demo | Dashboard bis Invariantverletzung absichern | Hoch |
| 18 | Tests und UI-Stabilisierung | Komponenten-, State-, API- und E2E-Tests finalisieren | Hoch |

## Schritt 1: Frontend-Projekt initialisieren

### Ziel

Ein neues, eigenständiges React/TypeScript-Frontend wird initialisiert. Es ist kein Teil des originalen USE-Repositories und übernimmt keine alte Desktop-GUI.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Vite-Projekt erstellen | React + TypeScript als Basis verwenden. |
| Tooling einrichten | ESLint, Formatter, Test Runner, TypeScript Strict Mode. |
| Basisstruktur anlegen | `src/app`, `src/features`, `src/components`, `src/api`, `src/state`, `src/types`, `src/test`. |
| Testbasis | Vitest und React Testing Library vorbereiten. |
| Styling-Basis | CSS Modules, Tailwind oder projektweite CSS-Strategie entscheiden. |
| Asset-Zugriff | Screenshot-Referenzen nur in Analyse; echte App-Assets separat organisieren. |
| README | Startbefehle und Architekturhinweis dokumentieren. |

### Erwartetes Ergebnis

Das Frontend startet lokal und rendert eine minimale App. Tests laufen.

### Betroffene Komponenten

| Komponente | Zweck |
|---|---|
| `App` | Root-Komponente |
| `AppProviders` | Query Client, Router, globale Stores |
| `main.tsx` | Einstiegspunkt |

### Benötigte API-Daten

Noch keine echten API-Daten. Es kann ein Mock-Projekt vorbereitet werden.

### Benötigter State

| State | Beschreibung |
|---|---|
| `apiLoadingState` | später über Query/Mutation |
| `appShellState` | zunächst minimal |

### Relevante Screenshots

| Screenshot | Nutzung |
|---|---|
| alle Screenshots | Noch keine Umsetzung, aber visuelle Zielreferenz für spätere Schritte. |

### Abhängigkeiten

- Entscheidung für Vite/React/TypeScript.
- Eigenes Frontend-Repository.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-SETUP-001` | App rendert ohne Fehler. |
| `FE-SETUP-002` | TypeScript Build läuft. |
| `FE-SETUP-003` | Basistest läuft. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Setup wird zu komplex | Nur MVP-relevante Tools aufnehmen. |
| Styling-Entscheidung blockiert Umsetzung | Pragmatismus: klare CSS-Strategie wählen und dokumentieren. |

### Akzeptanzkriterien

- `npm run dev` oder äquivalenter Startbefehl funktioniert.
- `npm test` oder äquivalenter Testbefehl funktioniert.
- TypeScript ist aktiv und streng genug konfiguriert.

### MVP-Relevanz

Hoch.

## Schritt 2: Grundlayout, Routing und App Shell

### Ziel

Routing und App Shell werden so vorbereitet, dass Dashboard und Projektworkspace klar getrennt sind.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Router einrichten | `/`, `/dashboard`, `/projects`, `/projects/:projectId/class-diagram`, `/projects/:projectId/object-diagram`, `/projects/:projectId/ocl`. |
| Dashboard Layout | Eigene Page ohne Workspace-Panels. |
| Workspace Layout | Top Bar, Navigation Tabs, Explorer Sidebar, Main View, Properties Panel, Bottom Panel. |
| Route Guards | Projekt-ID prüfen und Loading/Error State darstellen. |
| Navigation Tabs | Class Diagram, Object Diagram, OCL Editor. |
| Bottom Panel | Console und Validation Results als Tabs vorbereiten. |
| Modal Layer | Globaler Layer für Modals. |

### Erwartetes Ergebnis

Die App kann zwischen Dashboard und Projektworkspace navigieren. Der Workspace zeigt die Grundstruktur aus den Diagramm-Screenshots.

### Betroffene Komponenten

| Komponente | Zweck |
|---|---|
| `DashboardPage` | Startseite |
| `ProjectsPage` | vollständige Projektliste vor dem Workspace |
| `WorkspaceLayout` | Projektarbeitsbereich |
| `TopBar` | Projektname, Tabs, Save, Refresh, Check Constraints |
| `ExplorerSidebar` | Navigation durch Modellobjekte |
| `PropertiesPanel` | selektionsabhängige Details |
| `BottomPanel` | Console und Validation Results |
| `ModalLayer` | Dialoge |

### Benötigte API-Daten

- `ProjectDto` für Projektworkspace.
- `ProjectSummaryDto[]` später für Dashboard.

### Benötigter State

| State | Beschreibung |
|---|---|
| `activeRoute` | Aktuelle View über URL. |
| `projectQuery` | Geladenes Projekt. |
| `selectionState` | Aktuell selektiertes Element. |
| `panelState` | Bottom Panel, Properties Panel, aktive Tabs. |
| `modalState` | Aktives Modal. |

### Relevante Screenshots

| Screenshot | Nutzung |
|---|---|
| `00-dashboard-start-page.png` | Dashboard als eigene Layout-Ebene. |
| `19-projects.png` | All-Projects-Seite als eigene Projektverwaltungsroute. |
| `01-class-diagram-class-properties.png` | Workspace mit Top Bar, Sidebar, Canvas, Properties, Bottom Panel. |
| `06-object-diagram-object-properties.png` | Object Diagram im gleichen Workspace. |
| `07-object-diagram-validation-error.png` | Bottom Panel mit Validation Results. |

![Dashboard Start Page](../assets/screenshots/00-dashboard-start-page.png)

### Abhängigkeiten

- Schritt 1.
- Routing-Analyse.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-ROUTE-001` | `/` rendert Dashboard. |
| `FE-ROUTE-001A` | `/projects` rendert All Projects ohne Workspace-Sidebars. |
| `FE-ROUTE-002` | `/projects/demo/class-diagram` rendert Workspace. |
| `FE-ROUTE-003` | Tabs navigieren zu Class/Object/OCL Views. |
| `FE-ROUTE-004` | Workspace zeigt Top Bar, Sidebar, Main View, Properties und Bottom Panel. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Dashboard wird in Workspace gezwängt | Dashboard als eigene Page behandeln. |
| Routing und Tab State konkurrieren | URL-Routing für Hauptviews bevorzugen. |

### Akzeptanzkriterien

- Dashboard ist erster sichtbarer Einstieg.
- Projektworkspace ist erst nach Projektstart/Projektöffnung sichtbar.
- App Shell kann viewabhängige Inhalte aufnehmen.

### MVP-Relevanz

Hoch.

## Schritt 3: Dashboard / Start Page

### Ziel

Die Dashboard / Start Page aus `00-dashboard-start-page.png` wird als erste nutzbare Oberfläche umgesetzt.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Header | Logo, Produkttitel `USE`, Untertitel `UML-based Specification Environment`, Avatar. |
| Create New Model Card | Karte mit Beschreibung und `+ Start Project` Button. |
| Create New Project Dialog | Dialog/Formular aus `18-create-new-projects.png` vorbereiten; Projektname als Pflichtfeld. |
| Open Existing Card | Öffnet den `Open Existing Project` Dialog für lokale `.use` Dateien; vollständige `.use` Kompatibilität bleibt Post-MVP. |
| Open Existing Project Modal | Datei auswählen oder per Drag & Drop ablegen, unterstütztes Format `.use` anzeigen und Import/Apply auslösen. |
| Recent Projects Section | University System, Hotel Management, Bank ATM oder Backend-Daten anzeigen. |
| View all | Navigiert zu `/projects` und öffnet die All-Projects-Seite aus `19-projects.png`. |
| Learn & Support | Documentation und Examples Links anzeigen. |
| Empty/Loading States | Recent Projects Loading, keine Projekte, Importfehler. |

### Erwartetes Ergebnis

Nutzer sehen beim Öffnen der Anwendung ein Dashboard mit den im Screenshot sichtbaren Startoptionen.

### Betroffene Komponenten

| Komponente | Zweck |
|---|---|
| `DashboardPage` | Container |
| `DashboardHeader` | Logo, Titel, Avatar |
| `CreateNewModelCard` | Neues Projekt starten |
| `CreateNewProjectModal` | Projektname erfassen und Submit vorbereiten |
| `OpenExistingCard` | Projekt öffnen/importieren |
| `RecentProjectsSection` | Recent Projects |
| `RecentProjectCard` | Einzelnes Projekt |
| `LearnSupportSection` | Documentation und Examples |

### Benötigte API-Daten

- `ProjectSummaryDto[]` für Recent Projects.
- Optional Mockdaten, wenn Backend noch nicht bereit ist.

### Benötigter State

| State | Beschreibung |
|---|---|
| `recentProjectsQuery` | Recent Projects laden. |
| `createProjectMutation` | Noch nicht angebunden oder Mock. |
| `createProjectFormState` | Projektname, Feldfehler und Dialogzustand. |
| `importProjectState` | Datei/Importdialog. |
| `dashboardErrorState` | Fehler bei Recent Projects oder Import. |

### Relevante Screenshots

| Screenshot | Nutzung |
|---|---|
| `00-dashboard-start-page.png` | Vollständige Dashboard-Struktur. |
| `18-create-new-projects.png` | Projektnamenerfassung nach `Start Project`. |
| `19-projects.png` | Zielansicht für `View all`. |

### Abhängigkeiten

- Schritt 2.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-DASH-001` | Dashboard zeigt Logo, Titel und Untertitel. |
| `FE-DASH-002` | `Create New Model` Card mit `Start Project` ist sichtbar. |
| `FE-DASH-002A` | Klick auf `Start Project` öffnet die Projektnamenerfassung. |
| `FE-DASH-002B` | Leerer Projektname blockiert Submit oder zeigt Feldfehler. |
| `FE-DASH-003` | `Open Existing` Card ist sichtbar. |
| `FE-DASH-006` | Klick auf `Open Existing` öffnet den `Open Existing Project` Dialog. |
| `FE-DASH-007` | Der Dialog akzeptiert nur `.use` Dateien und zeigt bei falschem Format einen klaren Fehler. |
| `FE-DASH-004` | Recent Projects Section ist sichtbar. |
| `FE-DASH-004A` | Klick auf `View all` navigiert zur All-Projects-Seite oder ist klar als vorbereitet markiert. |
| `FE-DASH-005` | Documentation und Examples Links sind sichtbar. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Dashboard wird nur statisch | Interaktionspunkte von Anfang an als Komponenten mit Callbacks bauen. |
| Recent Projects blockieren MVP | Fallback auf Demo-/Mockdaten zulassen. |

### Akzeptanzkriterien

- Dashboard entspricht strukturell dem Screenshot.
- `Start Project`, `Open Existing`, Recent Projects, Documentation und Examples sind sichtbar.
- `Start Project` öffnet die Projektnamenerfassung aus `18-create-new-projects.png`.
- `Open Existing` öffnet einen Dialog zum lokalen `.use` Upload mit Cancel/Open-Project-Aktion.
- Noch nicht implementierte Funktionen sind nicht irreführend als vollständig nutzbar dargestellt.

### MVP-Relevanz

Hoch. Dashboard ist erster MVP-Schritt.

## Schritt 3a: All Projects Page

### Ziel

Die All-Projects-Seite aus `19-projects.png` wird als Ziel von `View all` umgesetzt. Sie zeigt alle verfügbaren Projekte als Karten und erlaubt Suche, Filtereinstieg, Öffnen und Start eines neuen Projekts.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Route anbinden | `/projects` rendert `ProjectsPage` ohne Workspace-Sidebars. |
| Projektliste laden | `GET /api/v1/projects` oder vorläufige Project Summary Daten verwenden. |
| Suchfeld | Projekte nach Name/Beschreibung mindestens clientseitig filtern. |
| Filter Button | Sichtbaren Einstieg vorbereiten; komplexe Filterlogik bleibt Post-MVP. |
| Projektkarten | Name, Beschreibung und letzten Änderungszeitpunkt anzeigen. |
| Projekt öffnen | Klick auf Karte oder `Open` lädt Projekt und navigiert zu `/projects/{projectId}/class-diagram`. |
| Neues Projekt | `+ New Project` öffnet denselben Create-New-Project-Dialog wie Dashboard. |
| Zurücknavigation | Back-Icon führt zum Dashboard zurück. |

### Erwartetes Ergebnis

`View all` führt nicht ins Leere, sondern auf eine projektverwaltende Liste mit den im Screenshot sichtbaren Hauptelementen.

### Betroffene Komponenten

| Komponente | Zweck |
|---|---|
| `ProjectsPage` | Container für die Projektliste. |
| `ProjectSearchInput` | Suchfeld für Projektkarten. |
| `ProjectFilterButton` | Einstieg in spätere Filter. |
| `ProjectCardGrid` | Responsives Grid für Projektkarten. |
| `ProjectListCard` | Einzelne Projektkarte mit Open-Verhalten. |
| `ProjectListActions` | `Open` und `+ New Project`. |
| `CreateNewProjectModal` | Wiederverwendung des Projektstartdialogs. |

### Benötigte API-Daten

| API | DTO | Nutzung |
|---|---|---|
| `GET /api/v1/projects` | `ProjectSummaryDto[]` | Projektliste laden. |
| `GET /api/v1/projects/{projectId}` | `ProjectDto` | Projekt aus Karte öffnen. |
| `POST /api/v1/projects` | `CreateProjectRequestDto`, `ProjectDto` | Neues Projekt aus der Liste starten. |

### Benötigter State

| State | Beschreibung |
|---|---|
| `projectListQuery` | geladene Project Summaries. |
| `projectSearchState` | Suchbegriff. |
| `projectFilterState` | vorbereiteter Filterzustand. |
| `selectedProjectId` | optional für `Open` Button. |
| `createProjectFormState` | Dialogzustand für `+ New Project`. |

### Relevante Screenshots

| Screenshot | Nutzung |
|---|---|
| `19-projects.png` | All-Projects-Seite mit Suche, Filter, Projektkarten, `Open`, `+ New Project`. |
| `18-create-new-projects.png` | Dialog bei `+ New Project`. |

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-PROJECTS-001` | `/projects` rendert Seitentitel, Suche, Filter, `Open`, `+ New Project` und Projektkarten. |
| `FE-PROJECTS-002` | Suchfeld filtert Projektkarten clientseitig. |
| `FE-PROJECTS-003` | Klick auf Projektkarte lädt Projekt und navigiert ins Class Diagram. |
| `FE-PROJECTS-004` | `+ New Project` öffnet Create-New-Project-Dialog. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Projektliste wird mit Workspace verwechselt | `/projects` als eigene Page ohne Explorer/Properties/Bottom Panel umsetzen. |
| Filterlogik vergrößert Scope | Filter Button vorbereiten, detaillierte Filter Post-MVP. |
| Backend liefert noch keine vollständige Liste | UI mit `ProjectSummaryDto[]` bauen; Demo-Daten nur als klarer Fallback. |

### Akzeptanzkriterien

- `View all` vom Dashboard öffnet `/projects`.
- All-Projects-Seite entspricht strukturell `19-projects.png`.
- Projektkarten zeigen fachliche Namen und Beschreibungen, keine technischen IDs.
- Projektöffnung navigiert in den Workspace.

### MVP-Relevanz

Should. Die Seite vervollständigt den Dashboard-Projektverwaltungsflow, blockiert aber nicht den Kern-MVP `Start Project -> Class Diagram -> Object Diagram -> Check Constraints`.

## Schritt 4: Projektstart über Start Project

### Ziel

Der Button `Start Project` öffnet die Projektnamenerfassung. Erst nach gültigem Projektnamen erzeugt das Frontend ein neues Projekt und navigiert anschließend ins Klassendiagramm.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Create Project Form Submit | Projektname trimmen, Pflichtfeld validieren und Submit nur bei gültigem Namen zulassen. |
| Create Project Mutation | `POST /api/v1/projects` mit `CreateProjectRequestDto.name` oder Mock-Äquivalent anbinden. |
| Erfolgsnavigation | Nach Response zu `/projects/{projectId}/class-diagram`. |
| Loading State | Button während Erstellung sperren und Ladezustand zeigen. |
| Error State | API-Fehler als Dashboard-Banner oder Toast darstellen. |
| Initial Project | Leeres Projekt mit leerem UML-/Object Model und Layout laden. |
| Console Entry optional | Projektstart später im Workspace protokollieren. |

### Erwartetes Ergebnis

Der erste MVP-Flow funktioniert:

```text
Dashboard -> Start Project -> Projektname eingeben -> Class Diagram
```

### Betroffene Komponenten

- `CreateNewModelCard`
- `CreateNewProjectModal`
- `DashboardPage`
- `ProjectApiClient`
- `Router`
- `ClassDiagramPage`

### Benötigte API-Daten

- `ProjectDto`
- optional `CreateProjectRequestDto`
- `ApiErrorDto`

### Benötigter State

| State | Beschreibung |
|---|---|
| `createProjectMutation` | Lade-/Erfolgs-/Fehlerzustand. |
| `createProjectFormState` | Projektname, Dirty Flag, Feldfehler. |
| `projectQueryCache` | Neues Projekt im Cache ablegen. |
| `navigationState` | Zielroute. |

### Relevante Screenshots

| Screenshot | Nutzung |
|---|---|
| `00-dashboard-start-page.png` | `Start Project` Button. |
| `18-create-new-projects.png` | Projektname als Pflichtfeld vor der Anlage. |
| `04-class-diagram-new-class-selected.png` | Zielansicht nach Projektstart, später mit Klasse. |

### Abhängigkeiten

- Schritte 2 und 3.
- API Client kann zunächst Mock sein, wird in Schritt 5 sauber aufgebaut.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-START-001` | Klick auf `Start Project` öffnet den Create-New-Project-Dialog. |
| `FE-START-001A` | Submit ohne Projektname wird verhindert oder zeigt Feldfehler. |
| `FE-START-002` | Erfolgreiche Response nach gültigem Projektnamen navigiert ins Class Diagram. |
| `FE-START-003` | Fehlerhafte Response zeigt Fehlermeldung auf Dashboard. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Frontend wartet auf fertiges Backend | Mock API verwenden. |
| Navigation passiert ohne Projektcache | Response direkt in Query Cache schreiben oder nach Navigation neu laden. |

### Akzeptanzkriterien

- Nutzer kann vom Dashboard ins Klassendiagramm gelangen.
- Projekt-ID ist in der Route sichtbar.
- API-/Mockfehler werden verständlich angezeigt.

### MVP-Relevanz

Sehr hoch.

## Schritt 4a: Create New Project Dialog

### Ziel

Der neue Screenshot `18-create-new-projects.png` wird als eigener Projektstartschritt umgesetzt. `+ Start Project` legt kein anonymes Projekt mehr direkt an, sondern öffnet eine Projektnamenerfassung. Erst nach gültigem Namen wird `POST /api/v1/projects` ausgelöst.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Dialog öffnen | Klick auf `+ Start Project` in `CreateNewModelCard` öffnet `CreateNewProjectModal` oder ein gleichwertiges Formular. |
| Projektname-Feld | Eingabefeld für Projektname mit Fokus beim Öffnen bereitstellen. |
| Pflichtfeldvalidierung | Leere oder nur aus Leerzeichen bestehende Namen blockieren Submit und zeigen einen Feldfehler. |
| Submit Payload | `CreateProjectRequestDto` mit getrimmtem `name` erzeugen. |
| Backend-Mutation | `projectApi.createProject({ name })` aufrufen. |
| Erfolgsnavigation | Nach `ProjectDto` zu `/projects/{projectId}/class-diagram` navigieren. |
| Fehlerzustand | API-Fehler im Dialog anzeigen; Dialog offen lassen. |
| Cancel/Close | Dialog schließen, ohne Projekt anzulegen. |

### Erwartetes Ergebnis

Der sichtbare Projektstart entspricht dem Screenshot:

```text
Dashboard
-> + Start Project
-> Create New Project Dialog
-> Projektname eingeben
-> Projekt erstellen
-> /projects/{projectId}/class-diagram
```

### Betroffene Komponenten

| Komponente | Anpassung |
|---|---|
| `DashboardPage` | hält Dialog-State und startet Navigation nach Erfolg. |
| `CreateNewModelCard` | ruft nur `openCreateProjectDialog()` auf. |
| `CreateNewProjectModal` | neues oder ausgebautes Dialogformular. |
| `ProjectApiClient` | nutzt `CreateProjectRequestDto.name`. |
| `Router` | navigiert nach erfolgreicher Projektanlage. |

### Benötigte API-Daten

- `CreateProjectRequestDto`
- `ProjectDto`
- `ApiErrorDto`

### Benötigter State

| State | Beschreibung |
|---|---|
| `createProjectFormState` | `name`, `dirty`, `fieldErrors`. |
| `createProjectMutation` | Loading, Success, Error. |
| `navigationState` | Zielroute nach erfolgreicher Anlage. |

### Relevante Screenshots

| Screenshot | Nutzung |
|---|---|
| `00-dashboard-start-page.png` | Auslöser `+ Start Project`. |
| `18-create-new-projects.png` | Dialog/Formular, Pflichtfeld Projektname. |

### Abhängigkeiten

- Schritt 3: Dashboard / Start Page.
- Schritt 4: Projektstart-Flow.
- Schritt 5 oder vorhandener API Client für `projectApi.createProject`.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-CREATE-PROJECT-001` | Klick auf `+ Start Project` öffnet den Dialog. |
| `FE-CREATE-PROJECT-002` | Submit ohne Namen wird blockiert oder zeigt Feldfehler. |
| `FE-CREATE-PROJECT-003` | Submit mit Name ruft `projectApi.createProject({ name })` auf. |
| `FE-CREATE-PROJECT-004` | Erfolgreiche Response navigiert zu `/projects/{projectId}/class-diagram`. |
| `FE-CREATE-PROJECT-005` | API-Fehler bleibt im Dialog sichtbar. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Dialog dupliziert Logik aus Schritt 4 | Schritt 4 als Flow betrachten, Schritt 4a als konkrete UI-Umsetzung. |
| Backend lehnt Namen anders ab als Frontend | Frontend nur minimale Pflichtfeldprüfung, Backend-Fehler anzeigen. |
| Projekt wird trotz Cancel angelegt | API-Aufruf ausschließlich im Submit-Handler auslösen. |

### Akzeptanzkriterien

- `Start Project` öffnet keinen Workspace ohne vorherige Namenseingabe.
- Projektname ist Pflicht.
- Backend erhält den Namen im `CreateProjectRequestDto`.
- Nach Erfolg wird das Klassendiagramm des neuen Projekts geöffnet.

### Relevante Analyse-Dateien

- `assets/screenshots/README.md`
- `02-product-and-user-journey/02-screenshot-based-user-journey.md`
- `02-product-and-user-journey/03-functional-requirements.md`
- `04-ui-ux-analysis/05-screenshot-traceability.md`
- `06-frontend-analysis/12-modal-dialogs.md`
- `06-frontend-analysis/15-screenshot-implementation-mapping.md`
- `07-integration-and-api/02-api-flow.md`

### MVP-Relevanz

Hoch. Der Screenshot macht die Projektnamenerfassung zum sichtbaren Bestandteil des Dashboard-MVP.

## Schritt 5: API Client und DTOs

### Ziel

Der Frontend-API-Client und die TypeScript-DTOs werden passend zum Backend-Vertrag aufgebaut.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| DTOs definieren | TypeScript-Interfaces aus DTO-Referenz und aktuellem `use-web-backend` übernehmen. Wichtig: `ProjectDto` enthält die Projekt-ID unter `project.id`, `ObjectModelDto` nutzt `id`, und `PUT /projects/{projectId}` erwartet direkt ein `ProjectDto`. |
| API Client | Projekt-, UML-, Object-, OCL- und Validation-Clients anlegen. |
| Error Handling | `ApiErrorDto` normalisieren. |
| Validation Result Types | `ValidationResultDto`, `ValidationErrorDto`, `ElementTargetDto`. |
| Mock API | MSW oder einfache Mock-Schicht für parallele Entwicklung; Mockdaten müssen dieselbe DTO-Form wie `use-web-backend` verwenden. |
| Query Keys | Stabile Keys für Projekte, Recent Projects, OCL Checks. |
| Request Helpers | JSON Headers, Base URL, Fehlerparse; Default für das lokale Backend ist `http://localhost:8080/api/v1`, passend zu `mvn spring-boot:run`. |

### Erwartetes Ergebnis

Frontend kann typisiert mit Backend oder Mock API sprechen.

Der API Client muss für Schritt 5 bereits mit dem tatsächlich vorhandenen Backend-Vertrag kompatibel sein:

| Backend-Endpoint | Request | Response | Frontend-Hinweis |
|---|---|---|---|
| `GET /api/v1/health` | keiner | `{ status, service, timestamp }` | für Smoke-Test in Schritt 5a |
| `POST /api/v1/projects` | `CreateProjectRequestDto` | `ProjectDto` | Projekt-ID aus `response.project.id` lesen |
| `GET /api/v1/projects/recent` | optional `?limit=5` | `ProjectSummaryDto[]` | Dashboard-Recent-Projects |
| `GET /api/v1/projects` | optional später `?search=...` | `ProjectSummaryDto[]` | All-Projects-Seite |
| `POST /api/v1/projects/import` | `ImportProjectRequestDto` | `ImportProjectResponseDto` | JSON-Import MVP, `.use` später |
| `POST /api/v1/projects/{projectId}/model-text/apply` | `ApplyModelTextRequestDto` | `ApplyModelTextResponseDto` | lokaler `.use` Inhalt aus `Open Existing Project` als begrenzter Modelltext-Apply-Flow |
| `GET /api/v1/projects/{projectId}` | keiner | `ProjectDto` | Projekt laden |
| `PUT /api/v1/projects/{projectId}` | direkt `ProjectDto` | `ProjectDto` | kein Wrapper `{ project: ... }` senden |
| `GET /api/v1/projects/{projectId}/export` | keiner | JSON-String | Export-Flow |

### Betroffene Komponenten

| Bereich | Dateien/Module |
|---|---|
| API | `src/api/projectApi.ts`, `umlApi.ts`, `objectApi.ts`, `oclApi.ts`, `validationApi.ts` |
| DTOs | `src/types/api.ts` oder `src/api/dtos/*` |
| Mocks | `src/test/mocks/*` |

### Benötigte API-Daten

- alle MVP-DTOs aus `07-dto-reference.md`.

### Benötigter State

- Server State über TanStack Query empfohlen.
- Mutation State für Create/Update/Validate.

### Relevante Screenshots

| Screenshot | API-Bezug |
|---|---|
| `00-dashboard-start-page.png` | Create Project, Recent Projects, Import. |
| `14-open-existing-project.png` | Lokale `.use` Datei auswählen, hochladen und als Modelltext anwenden. |
| `08-modal-add-class.png` | Klasse erstellen. |
| `10-modal-add-class-association.png` | Association erstellen. |
| `11-modal-add-object-association.png` | Object Link erstellen. |
| `07-object-diagram-validation-error.png` | Validate. |

### Abhängigkeiten

- DTO-Referenz.
- Error Contract.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-API-001` | `createProject()` verarbeitet `ProjectDto`. |
| `FE-API-002` | `fetchProject()` mappt Projektdaten korrekt. |
| `FE-API-003` | `validateProject()` liefert `ValidationResultDto`. |
| `FE-API-004` | API-Fehler wird als `ApiErrorDto` interpretiert. |
| `FE-API-005` | `applyModelText()` sendet lokalen `.use` Inhalt mit `sourceName` und verarbeitet Diagnostics. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| DTOs driften vom Backend ab | DTO-Referenz und Contract Tests nutzen. |
| API-Client vermischt UI-State | API-Client nur für Requests/Responses verwenden. |

### Akzeptanzkriterien

- Alle MVP-API-Aktionen sind als Funktionen vorhanden.
- DTOs sind typisiert.
- Mock API kann Dashboard und Library-Demo bedienen.
- Der API Client kennt den begrenzten Modelltext-Apply-Endpunkt für lokale `.use` Dateien aus dem Open-Existing-Dialog.

### MVP-Relevanz

Sehr hoch.

## Schritt 5a: Backend Integration Smoke Test

### Ziel

Der in Schritt 5 aufgebaute API Client wird erstmals gegen das echte Java/Spring-Boot-Backend im Repository `use-web-backend` geprüft. Ziel ist ein früher Integrationsnachweis, dass Frontend, Backend, API-Basis-URL, CORS, DTOs und Fehlerformat grundsätzlich zusammenpassen.

Dieser Schritt liegt bewusst zwischen API Client und State Management. Dadurch werden Vertragsprobleme sichtbar, bevor Server State, Query Cache und komplexere UI-Flows auf instabilen Annahmen aufgebaut werden.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Backend-Repository verwenden | Als Integrationsziel wird ausdrücklich das lokale Backend `use-web-backend` genutzt, nicht das originale USE-Projekt. |
| Backend lokal starten | `use-web-backend` wird mit `mvn spring-boot:run` gestartet; der aktuelle Spring-Boot-Default ist Port `8080`. |
| Mock-Umschaltung prüfen | `VITE_USE_MOCK_API=false` oder äquivalente Konfiguration aktiviert echte HTTP-Requests. |
| Backend Base URL konfigurieren | `VITE_API_BASE_URL` auf das lokale Spring-Boot-Backend setzen, z. B. `http://localhost:8080/api/v1`. |
| Create Project Smoke Test | `POST /api/v1/projects` mit `CreateProjectRequestDto` gegen echtes Backend ausführen. |
| Load Project Smoke Test | `GET /api/v1/projects/{projectId}` für die erzeugte Projekt-ID ausführen. |
| Save Project Smoke Test | `PUT /api/v1/projects/{projectId}` mit direktem `ProjectDto` prüfen; das Backend erwartet keinen `SaveProjectRequestDto`-Wrapper. |
| Recent Projects prüfen | `GET /api/v1/projects/recent` testen; der Endpoint ist im aktuellen `use-web-backend` vorhanden. |
| All Projects prüfen | `GET /api/v1/projects` testen; die Response muss als `ProjectSummaryDto[]` für `ProjectsPage` nutzbar sein. |
| Error Contract prüfen | Mindestens ein technischer Fehler, z. B. unbekannte Projekt-ID, wird als `ApiErrorDto` normalisiert. |
| CORS prüfen | Browserzugriff vom Frontend-Dev-Server auf Backend-Dev-Server verifizieren. |
| Dashboard-Flow prüfen | `Dashboard -> Start Project -> /projects/{projectId}/class-diagram` mit echtem Backend ausführen. |
| Fallback dokumentieren | Wenn Backend-Endpoint fehlt, bleibt Mock aktiv und die Abweichung wird als offener Integrationspunkt dokumentiert. |

### Erwartetes Ergebnis

Das Frontend kann wahlweise mit Mock API oder echtem Backend laufen. Der Dashboard-Startflow funktioniert gegen das Backend mindestens für `POST /projects` und liefert eine echte Projekt-ID unter `ProjectDto.project.id`, die zur Class Diagram Route führt.

```text
vite dev
+ Spring Boot Backend
+ VITE_USE_MOCK_API=false
-> Dashboard
-> Start Project
-> POST /api/v1/projects
-> ProjectDto.project.id
-> /projects/{projectId}/class-diagram
```

### Betroffene Komponenten

| Komponente/Modul | Zweck |
|---|---|
| `apiConfig` | Umschaltung zwischen Mock API und echtem Backend. |
| `httpClient` | Echte HTTP-Kommunikation, JSON Headers und Fehlernormalisierung. |
| `projectApi` | `createProject`, `getProject`, `saveProject`, `getRecentProjects`, `listProjects`. |
| `DashboardPage` | nutzt `projectApi.createProject()` für den Startflow. |
| `AppRoutes` / Router | navigiert nach erfolgreicher Response zur Projektansicht. |
| Backend Project API | liefert echte `ProjectDto`-Responses und `ApiErrorDto`-Fehler. |

### Benötigte API-Daten

- `CreateProjectRequestDto`
- `ProjectDto`
- `ProjectSummaryDto[]`
- `ImportProjectResponseDto`
- `ApiErrorDto`

### Benötigter State

| State | Beschreibung |
|---|---|
| `createProjectMutation` | einfacher Lade-/Fehlerzustand im Dashboard. |
| `projectResponse` | erzeugtes oder geladenes `ProjectDto`; noch kein globaler Server State aus Schritt 6. |
| `apiErrorState` | Anzeige technischer Fehler im Dashboard oder in Testausgaben. |
| `mockMode` | Konfiguration, ob Mock API oder echte API verwendet wird. |

### Relevante Screenshots

| Screenshot | Integrationsbezug |
|---|---|
| `00-dashboard-start-page.png` | Start Project als Auslöser des Integrationsflows. |
| `19-projects.png` | `View all` nutzt `GET /api/v1/projects` und rendert Project Summaries. |
| `04-class-diagram-new-class-selected.png` | Zielbereich nach erfolgreichem Projektstart; konkrete Diagrammfunktion kommt später. |

### Abhängigkeiten

- Schritt 5.
- `use-web-backend` muss lokal startbar sein.
- `POST /api/v1/projects`, `GET /api/v1/projects`, `GET /api/v1/projects/{projectId}`, `PUT /api/v1/projects/{projectId}`, `GET /api/v1/projects/recent`, `POST /api/v1/projects/import` und `GET /api/v1/health` sind im aktuellen `use-web-backend` vorhanden oder durch Backend-Schritt 6b als unmittelbare Project-Service-Schnittstelle vorgesehen.
- Falls spätere fachliche Endpunkte noch nicht vollständig sind, wird nur der projektbezogene Smoke Test durchgeführt und fehlende Feature-Endpunkte bleiben dokumentierte offene Punkte.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-BE-INT-001` | Frontend kann Mock API per Konfiguration deaktivieren. |
| `FE-BE-INT-002` | `projectApi.createProject()` sendet echten `POST /api/v1/projects` Request und verarbeitet `ProjectDto`. |
| `FE-BE-INT-003` | Dashboard navigiert mit echter Backend-Projekt-ID ins Class Diagram. |
| `FE-BE-INT-004` | `projectApi.getProject()` kann erzeugtes Projekt laden, sofern Backend Persistenz anbietet. |
| `FE-BE-INT-005` | API-Fehler wird aus echter Backend-Response als `ApiErrorDto` normalisiert. |
| `FE-BE-INT-006` | `projectApi.listProjects()` lädt `ProjectSummaryDto[]` für die All-Projects-Seite. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Backend und Frontend nutzen abweichende DTOs | Abgleich mit `07-integration-and-api/07-dto-reference.md` und früh korrigieren. |
| CORS blockiert lokalen Browserzugriff | Backend-CORS-Konfiguration für Frontend-Dev-Origin ergänzen. |
| Backend-Endpunkte sind noch nicht vollständig | Mock API bleibt aktivierbar; fehlende Endpunkte explizit als Integrationslücken dokumentieren. |
| Integration wird mit State Management vermischt | In diesem Schritt nur Smoke Tests und einfache Response-Verarbeitung, kein globaler Query/Store-Aufbau. |

### Akzeptanzkriterien

- Mock API und echte API sind per Konfiguration umschaltbar.
- `Start Project` kann gegen das Backend ausgeführt werden, sobald das Backend lokal läuft.
- Ein erfolgreiches `ProjectDto` führt zur Route `/projects/{projectId}/class-diagram`.
- Technische Backendfehler werden über den API Client normalisiert und UI-tauglich angezeigt oder testbar gemacht.
- Offene Backend-Endpunkte sind dokumentiert, ohne die Frontend-Weiterarbeit zu blockieren.

### MVP-Relevanz

Hoch. Ohne diesen Schritt bleibt die API-Schicht nur theoretisch; mit ihm wird der erste echte Dashboard-zu-Backend-zu-Workspace-Durchstich abgesichert.

## Schritt 5b: Open Existing `.use` Import Flow

### Ziel

Der Dashboard-Dialog aus `14-open-existing-project.png` wird als eigener Import-/Apply-Flow umgesetzt. Nutzer können eine lokale `.use` Datei auswählen oder per Drag & Drop ablegen. Das Frontend liest den Dateiinhalt, erzeugt oder nutzt ein Projekt und sendet den Text an das Backend `use-web-backend`.

Dieser Schritt ist bewusst kein vollständiger USE-Import im Frontend. Das Frontend transportiert Dateiinhalt, Herkunft und UI-Zustand. Das Backend entscheidet, welches Modelltext-Subset angewendet werden kann und welche Diagnostics zurückgegeben werden.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Dashboard-Aktion anbinden | Klick auf `Open Existing` öffnet `OpenExistingProjectModal`. |
| File Dropzone | `.use` Datei per Dateiauswahl oder Drag & Drop annehmen. |
| Dateiformat prüfen | Nur `.use` Dateien zulassen; falsche Formate lokal mit UI-Fehler blockieren. |
| Dateiinhalt lesen | Datei per Browser-API als Text lesen; keine fachliche USE-Auswertung im Frontend. |
| Projektkontext erzeugen | Falls noch kein Projekt existiert, über `createProject()` ein Zielprojekt erzeugen. |
| Modelltext anwenden | `applyModelText(projectId, { modelText, sourceName, sourceFormat: "use", sourceOrigin: "open-existing" })` aufrufen. |
| Diagnostics anzeigen | Backend-Diagnostics im Modal, in der Console oder nach Navigation im OCL Editor anzeigen. |
| Erfolgsnavigation | Bei erfolgreichem Apply ins Class Diagram navigieren; bei Teilunterstützung Diagnostics sichtbar lassen. |
| Abgrenzung dokumentieren | Vollständige USE-Kompatibilität bleibt Post-MVP und wird nicht im Frontend simuliert. |

### Erwartetes Ergebnis

Der Open-Existing-Flow ist nutzbar und gegen das echte Backend integrierbar:

```text
Dashboard
-> Open Existing
-> Open Existing Project Modal
-> Library.use auswählen
-> Dateiinhalt lesen
-> POST /api/v1/projects/{projectId}/model-text/apply
-> ProjectDto + Diagnostics
-> Class Diagram oder Diagnostics-Anzeige
```

### Betroffene Komponenten

| Komponente/Modul | Zweck |
|---|---|
| `DashboardPage` | öffnet den Dialog und verarbeitet Erfolgsnavigation. |
| `OpenExistingProjectModal` | Upload-UI, Validierung, Loading/Error/Diagnostics. |
| `FileDropZone` | Datei auswählen, Drag & Drop, Formatprüfung. |
| `projectApi` | `createProject()` und `applyModelText()` nutzen. |
| `apiConfig` | echter Backend-Endpoint aus `use-web-backend`. |
| `ConsoleLogPanel` oder temporärer Import-Feedback-Bereich | Importstatus und Diagnostics anzeigen. |

### Benötigte API-Daten

- `CreateProjectRequestDto`
- `ProjectDto`
- `ApplyModelTextRequestDto`
- `ApplyModelTextResponseDto`
- `OclDiagnosticDto` oder kompatible ModelText-Diagnostics
- `ApiErrorDto`

### Benötigter State

| State | Beschreibung |
|---|---|
| `openExistingModalState` | offen/geschlossen, Upload-Modus, Fehlerzustand. |
| `importFileState` | Dateiname, Dateigröße, Dateiinhalt, Formatvalidierung. |
| `applyModelTextMutation` | Loading, Erfolg, Backend-Diagnostics, API-Fehler. |
| `importDiagnosticsState` | temporär im Modal oder nach Navigation im OCL Editor/Console sichtbar. |
| `navigationTarget` | Zielroute nach erfolgreichem Apply. |

Dieser State darf zunächst lokal im Dashboard/Modal bleiben. Die saubere globale Einordnung erfolgt in Schritt 6.

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `00-dashboard-start-page.png` | `Open Existing` als Einstieg. |
| `14-open-existing-project.png` | Modal, Local File Tab, Dropzone, `Open Project`, `Cancel`. |
| `13-ocl-editor.png` | Ziel für Diagnostics oder spätere Textbearbeitung nach Teilimport. |
| `04-class-diagram-new-class-selected.png` | Zielbereich nach erfolgreichem Apply. |

### Abhängigkeiten

- Schritt 3: Dashboard / Start Page.
- Schritt 5: API Client und DTOs.
- Schritt 5a: echtes Backend `use-web-backend` ist erreichbar und CORS/API-Basis funktionieren.
- Backend-Schritt 16a: `model-text/apply` muss lokale `.use` Dateiquellen als Modelltext verarbeiten.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-OPEN-USE-001` | Klick auf `Open Existing` öffnet `OpenExistingProjectModal`. |
| `FE-OPEN-USE-002` | `.use` Datei wird akzeptiert und Dateiname wird angezeigt. |
| `FE-OPEN-USE-003` | Nicht-`.use` Datei wird lokal blockiert. |
| `FE-OPEN-USE-004` | `Open Project` liest Dateiinhalt und sendet `applyModelText()` mit `sourceName`, `sourceFormat` und `sourceOrigin`. |
| `FE-OPEN-USE-005` | Backend-Diagnostics werden sichtbar angezeigt. |
| `FE-OPEN-USE-006` | Erfolgreiche Response navigiert zur Class Diagram Route. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Nutzer erwarten vollständigen `.use` Import | UI-Text und Diagnostics klar machen: unterstütztes Subset, vollständige Kompatibilität später. |
| FileReader-Fehler oder leere Datei | Lokale Fehlerzustände vor Backend-Request anzeigen. |
| Apply-Endpunkt noch nicht vorhanden | Flow hinter klarer Fehlermeldung lassen; API-Lücke in Schritt 16a Backend umsetzen. |
| Diagnostics gehen nach Navigation verloren | Diagnostics in Schritt 6 in Import-/Console-State aufnehmen. |

### Akzeptanzkriterien

- `Open Existing` öffnet den Dialog aus `14-open-existing-project.png`.
- Eine lokale `.use` Datei kann ausgewählt oder per Drag & Drop gesetzt werden.
- Falsche Dateitypen werden ohne Backend-Request abgelehnt.
- Der Dateiinhalt wird nicht im Frontend fachlich interpretiert, sondern an `use-web-backend` gesendet.
- Der Request enthält `sourceName`, `sourceFormat = "use"` und `sourceOrigin = "open-existing"`.
- Backend-Diagnostics sind für Nutzer sichtbar.
- Bei erfolgreichem Apply führt der Flow ins Class Diagram.

### MVP-Relevanz

Hoch/Should. Der Dialog ist durch den Screenshot Teil des Zielbilds. Die technische MVP-Entscheidung bleibt: lokaler `.use` Inhalt wird als Modelltext-Subset verarbeitet; vollständige USE-Kompatibilität ist Post-MVP.

## Schritt 6: State Management

### Ziel

Server State, UI State, Selection State, Layout State, Modal State und Validation State werden klar getrennt.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Server State | Projekt und Recent Projects mit TanStack Query verwalten. |
| UI Store | Zustand oder Context für Selection, Panels, Modals, Console. |
| Validation State | Letztes `ValidationResultDto` und abgeleitete Fehlerindizes. |
| Layout State | Node-Positionen, Edge-Labelpositionen, Dirty Flags. |
| Selection State | Klasse, Association, Invariante, Objekt, Object Link. |
| Import State | Lokale `.use` Datei, Importstatus und Diagnostics aus `Open Existing Project` in UI-taugliche Zustände überführen. |
| Derived Selectors | `errorsByObjectId`, `errorsByInvariantId`, `selectedElement`. |
| Dirty State | Änderungen an Projekt/Layout/Formularen erkennen. |

### Erwartetes Ergebnis

Komponenten greifen konsistent auf Projekt-, UI- und Validierungszustände zu.

### Betroffene Komponenten

- `AppProviders`
- `projectQueries`
- `useSelectionStore`
- `useUiStore`
- `useValidationStore`
- `useDiagramLayoutStore`
- `useImportStore` oder Import-Slice im UI Store
- `ConsoleStore`

### Benötigte API-Daten

- `ProjectDto`
- `LayoutDto`
- `ValidationResultDto`
- `ValidationErrorDto`
- `ApplyModelTextResponseDto`
- `OclDiagnosticDto` oder ModelText-Diagnostic DTO

### Benötigter State

| State | Persistenz | Zweck |
|---|---|---|
| Server State | Backend | Projekt und Recent Projects. |
| Selection State | temporär | Properties Panel und Canvas-Fokus. |
| Modal State | temporär | Aktive Dialoge. |
| Import State | temporär | `.use` Datei, Upload-/Apply-Status und Diagnostics. |
| Layout State | Projektformat | Diagrammpositionen. |
| Validation State | temporär | Fehlerliste und Markierungen. |
| Console State | temporär | UI-Logs. |

### Relevante Screenshots

| Screenshot | State-Bezug |
|---|---|
| `01-class-diagram-class-properties.png` | Selection + Properties. |
| `02-class-diagram-association-properties.png` | Edge Selection. |
| `06-object-diagram-object-properties.png` | Object Selection + Slot Editing. |
| `07-object-diagram-validation-error.png` | Validation State + Error Mapping. |
| `14-open-existing-project.png` | Import File State + Diagnostics State. |

### Abhängigkeiten

- Schritt 5a.
- Schritt 5b.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-STATE-001` | Selektion einer Klasse aktualisiert Properties Panel. |
| `FE-STATE-002` | Validation Result erzeugt Fehlerindex nach Object ID. |
| `FE-STATE-003` | Layoutänderung markiert Projekt als dirty. |
| `FE-STATE-004` | Modal State öffnet und schließt AddClassModal. |
| `FE-STATE-005` | Import State speichert lokale `.use` Datei, Loading State und Diagnostics getrennt vom Projektzustand. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Globaler Store wird zu groß | Server State und UI State trennen. |
| Validation State wird manuell dupliziert | Derived Selectors statt redundanter Kopien verwenden. |

### Akzeptanzkriterien

- Selection, Validation und Layout sind voneinander getrennt.
- Import File State und Import Diagnostics sind klar vom fachlichen Projektzustand getrennt.
- Fehler-Mapping ist aus Validation Results ableitbar.
- State-Struktur unterstützt Class und Object Diagram.

### MVP-Relevanz

Sehr hoch.

## Schritt 7: Diagrammbibliothek auswählen und integrieren

### Ziel

Die Diagrammbibliothek wird final entschieden und in die App integriert.

Empfehlung aus der Analyse: React Flow ist für den MVP die bevorzugte Option, weil Custom Nodes, Custom Edges, React-Integration, TypeScript, Drag & Drop, Selektion und Layoutspeicherung gut zu den Screenshots passen.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Entscheidungscheck | React Flow gegen MVP-Kriterien prüfen. |
| Installation | Diagrammbibliothek und Styles integrieren. |
| Basiscanvas | Panning, Zooming, Selection, Dragging aktivieren. |
| Node-/Edge-Typen | UML Class Node, Object Node sowie eigene Custom Edges `UmlAssociationEdge` und `ObjectLinkEdge` vorbereiten. |
| Association-Labels | Association-Name mittig auf der Kante rendern; Rollen und Multiplizitaeten an Source- und Target-Ende rendern. |
| Layoutspeicherung | Positionsänderungen in `LayoutDto` überführen. |
| Selection Bridge | Canvas Selection mit globalem Selection State verbinden. |
| Fehlerstyles | Node/Edge Error States technisch vorbereiten. |

### Erwartetes Ergebnis

Ein einfacher Diagramm-Canvas kann Nodes und Edges anzeigen, verschieben, selektieren und Layoutdaten liefern.

### Betroffene Komponenten

- `DiagramCanvasBase`
- `ClassDiagramCanvas`
- `ObjectDiagramCanvas`
- `UmlClassNode`
- `ObjectNode`
- `UmlAssociationEdge`
- `ObjectLinkEdge`

### Benötigte API-Daten

- `LayoutDto`
- `NodeLayoutDto`
- `EdgeLayoutDto`
- `UmlClassDto`
- `UmlAssociationDto`
- `ObjectInstanceDto`
- `ObjectLinkDto`

### Benötigter State

- `diagramLayoutState`
- `selectionState`
- `validationState`

### Relevante Screenshots

| Screenshot | Bibliotheksanforderung |
|---|---|
| `01-class-diagram-class-properties.png` | Custom Class Nodes und Association Edges. |
| `02-class-diagram-association-properties.png` | Edge Selection. |
| `06-object-diagram-object-properties.png` | Custom Object Nodes. |
| `07-object-diagram-validation-error.png` | Fehler-Markierung. |
| `12-object-diagram-association-properties.png` | Object Link Edge Selection. |

### Abhängigkeiten

- Schritt 6.
- Diagramm-Analyse.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-DIAG-001` | Node wird gerendert. |
| `FE-DIAG-002` | Node kann verschoben werden. |
| `FE-DIAG-003` | Edge kann selektiert werden. |
| `FE-DIAG-004` | Layoutposition wird gespeichert. |
| `FE-DIAG-005` | Association Edge zeigt Mittel-Label und zwei Endlabel-Gruppen. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| React Flow reicht für Edge Labels nicht aus | Früh Prototyp mit Rollen/Multiplizitäten bauen. |
| Diagramm-State koppelt zu stark an DTOs | View Models für Nodes/Edges einführen. |
| Default Edges werden versehentlich als MVP-Lösung genutzt | In Schritt 7 eigene `UmlAssociationEdge` und `ObjectLinkEdge` als Pflicht definieren. |

### Akzeptanzkriterien

- Bibliotheksentscheidung ist dokumentiert.
- Basiscanvas funktioniert.
- Selection und Layoutdaten sind angebunden.

### MVP-Relevanz

Sehr hoch.

## Schritt 8: Class Diagram View

### Ziel

Die Class Diagram View zeigt Klassen, Attribute, Operationen, Associations und Invarianten.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Class Nodes | Klassenkarten mit Name, Attributen und Operationen. |
| Association Edges | Kanten zwischen Klassen mit Namen, Rollen und Multiplizitäten. |
| Invariantendarstellung | Badge oder eigenes Element für Kontextklasse. |
| Explorer-Synchronisation | Auswahl im Explorer fokussiert Canvas-Element. |
| Drag & Drop | Klassen positionieren und Layout aktualisieren. |
| Empty State | Leeres Projekt verständlich darstellen. |
| Toolbar Actions | Add Class, Add Association, Add Invariant auslösen. |

### Erwartetes Ergebnis

Nutzer können das UML-Klassenmodell visuell betrachten und Elemente selektieren.

### Betroffene Komponenten

- `ClassDiagramPage`
- `ClassDiagramCanvas`
- `UmlClassNode`
- `UmlAssociationEdge`
- `InvariantBadge`
- `ClassDiagramToolbar`
- `ExplorerSidebar`

### Benötigte API-Daten

- `ProjectDto`
- `UmlModelDto`
- `UmlClassDto`
- `UmlAttributeDto`
- `UmlOperationDto`
- `UmlAssociationDto`
- `UmlInvariantDto`
- `LayoutDto`

### Benötigter State

- `projectQuery`
- `selectionState`
- `diagramLayoutState`
- `validationState`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `01-class-diagram-class-properties.png` | Klassenkarte und Properties-Kopplung. |
| `02-class-diagram-association-properties.png` | Association Edge und Auswahl. |
| `03-class-diagram-invariant-properties.png` | Invariantendarstellung. |
| `04-class-diagram-new-class-selected.png` | Neue Klasse selektiert. |

### Abhängigkeiten

- Schritte 5 bis 7.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-CLASS-001` | Klassen werden als Nodes angezeigt. |
| `FE-CLASS-002` | Attribute und Operationen werden in Class Node angezeigt. |
| `FE-CLASS-003` | Association wird als Edge angezeigt. |
| `FE-CLASS-004` | Klick auf Klasse setzt Selection State. |
| `FE-CLASS-005` | Dragging aktualisiert Layout State. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Class Node wird visuell überladen | MVP-Darstellung kompakt halten. |
| Association Labels überlappen | MVP mit einfachem Label starten, Post-MVP verbessern. |

### Akzeptanzkriterien

- Class Diagram View ist nutzbar.
- Klassen, Attribute, Operationen und Associations sind sichtbar.
- Selektion treibt Properties Panel.

### MVP-Relevanz

Sehr hoch.

## Schritt 9: Properties Panel für Klassendiagramm

### Ziel

Das Properties Panel zeigt und bearbeitet selektionsabhängig Klasse, Association oder Invariante im Klassendiagramm.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Selection Resolver | Selektion auf passendes Panel mappen. |
| Class Properties | Name, Attribute, Operationen anzeigen/bearbeiten. |
| Class Properties Segmente | Bei selektierter Klasse Segmente `Class`, `Association` und `Invariant` bereitstellen. |
| Related Associations | Associations der selektierten Klasse anzeigen und per Klick selektierbar machen. |
| Related Invariants | Invarianten der selektierten Klasse anzeigen und per Klick selektierbar machen. |
| Association Properties | Name, Enden, Rollen, Multiplizitäten anzeigen/bearbeiten. |
| Invariant Properties | Name, Kontextklasse, OCL Expression anzeigen/bearbeiten. |
| Formularvalidierung | Pflichtfelder, primitive Typen, Rollennamen. |
| Update-Strategie | Debounced Update oder explizites Save festlegen. |
| Fehleranzeige | Backend-/Formfehler feldnah anzeigen. |

### Erwartetes Ergebnis

Nutzer können selektierte UML-Elemente im rechten Panel bearbeiten.

### Betroffene Komponenten

- `PropertiesPanel`
- `ClassPropertiesPanel`
- `ClassPropertiesSegmentControl`
- `RelatedAssociationList`
- `RelatedInvariantList`
- `AssociationPropertiesPanel`
- `InvariantPropertiesPanel`
- `PropertyField`
- `TypeSelect`
- `MultiplicityEditor`

### Benötigte API-Daten

- `UmlClassDto`
- `UmlAttributeDto`
- `UmlOperationDto`
- `UmlAssociationDto`
- `UmlInvariantDto`
- `OclDiagnosticDto`

### Benötigter State

- `selectionState`
- `formState`
- `projectDraftState`
- `apiMutationState`
- `validationState`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `01-class-diagram-class-properties.png` | Class Properties. |
| `02-class-diagram-association-properties.png` | Association Properties. |
| `03-class-diagram-invariant-properties.png` | Invariant Properties. |
| `15-properties-association.png` | Klasse selektiert; Segment `Association` zeigt zugehörige Associations. |
| `16-properties-invariants.png` | Klasse selektiert; Segment `Invariant` zeigt zugehörige Invarianten. |
| `17-new-class.png` | Neue Klasse selektiert; Segment `Class` zeigt Name, Attribute, Operationen und Add-Aktionen. |

### Abhängigkeiten

- Schritt 8.
- API Client für Updates.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-PROP-CD-001` | Klasse selektieren zeigt Class Properties. |
| `FE-PROP-CD-002` | Association selektieren zeigt Association Properties. |
| `FE-PROP-CD-003` | Invariante selektieren zeigt Invariant Properties. |
| `FE-PROP-CD-004` | Ungültiger Klassenname zeigt Formularfehler. |
| `FE-PROP-CD-005` | Bei selektierter Klasse kann zwischen `Class`, `Association` und `Invariant` gewechselt werden. |
| `FE-PROP-CD-006` | Related Association auswählen setzt Association-Selektion. |
| `FE-PROP-CD-007` | Related Invariant auswählen setzt Invariant-Selektion. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Autosave erzeugt viele API-Calls | Debounce oder explizites Save prüfen. |
| Form State überschreibt Server State | Draft State sauber isolieren. |

### Akzeptanzkriterien

- Panel-Inhalt folgt Selektion.
- Bei Klassenselektion sind allgemeine Klassendaten, zugehörige Associations und zugehörige Invarianten über Segmente erreichbar.
- Änderungen können lokal oder über API persistiert werden.
- Fehler sind feldnah sichtbar.

### MVP-Relevanz

Hoch.

## Schritt 10: Modals für Klasse, Association und Invariante

### Ziel

Die zentralen Erstellungsdialoge aus den Screenshots werden umgesetzt.

### Konkrete Aufgaben

| Modal | Aufgaben |
|---|---|
| `AddClassModal` | Klassenname, Attribute, Operationen; Submit erzeugt Klasse. |
| `AddAssociationModal` | Association Name, Source Class, Target Class, Rollen, Multiplizitäten. |
| `AddInvariantModal` | Kontextklasse, Invariant Name, OCL Expression. |
| Gemeinsame Modal Shell | Titel, Close, Cancel, Submit, Loading, Fehler. |
| Formularvalidierung | Pflichtfelder und einfache UI-Regeln. |
| Erfolgsverhalten | Modal schließen, Element selektieren, Canvas aktualisieren. |

### Erwartetes Ergebnis

Nutzer können Klassen, Associations und Invarianten über Dialoge erstellen. Der Dashboard-Dialog `OpenExistingProjectModal` ist bewusst nicht Teil dieses Schritts, sondern wird in Schritt 5b umgesetzt.

### Betroffene Komponenten

- `ModalShell`
- `AddClassModal`
- `AddAssociationModal`
- `AddInvariantModal`
- `AttributeEditor`
- `OperationSignatureEditor`
- `ContextClassSelect`
- `OclExpressionInput`

### Benötigte API-Daten

- `UmlClassDto`
- `UmlAssociationDto`
- `UmlInvariantDto`
- `OclParseResponseDto`
- `OclTypecheckResponseDto`

### Benötigter State

- `modalState`
- `formState`
- `apiMutationState`
- `selectionState`
- `projectQueryCache`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `08-modal-add-class.png` | Add Class Modal. |
| `09-modal-add-invariant.png` | Add Invariant Modal. |
| `10-modal-add-class-association.png` | Add Association Modal. |

### Abhängigkeiten

- Schritte 5, 8, 9.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-MODAL-001` | Add Class Modal öffnet und schließt. |
| `FE-MODAL-002` | Klasse wird nach Submit erstellt und selektiert. |
| `FE-MODAL-003` | Add Association Modal zeigt Klassen-Dropdowns. |
| `FE-MODAL-004` | Add Invariant Modal speichert OCL-Ausdruck. |
| `FE-MODAL-005` | Pflichtfeldfehler werden angezeigt. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Dialoge enthalten zu viele Felder | MVP-Formulare auf Pflichtumfang reduzieren. |
| OCL-Livecheck blockiert Invariantenerstellung | Parse/Typecheck optional oder asynchron machen. |

### Akzeptanzkriterien

- Alle drei Modals sind nutzbar.
- Nach Erstellung erscheint das neue Element im Explorer und Diagramm.
- Neues Element wird sinnvoll selektiert.

### MVP-Relevanz

Sehr hoch.

## Schritt 11: Object Diagram View

### Ziel

Die Object Diagram View zeigt Objektinstanzen, Slot-Werte und Objektlinks.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Object Nodes | Objektname und Typ anzeigen, z. B. `alice : User`. |
| Slot-Werte | Attribute und Werte in Objektkarte anzeigen. |
| Object Link Edges | Links zwischen Objekten darstellen. |
| Explorer | Objects und Associations/Object Links anzeigen. |
| Layout | Objektpositionen verschiebbar und speicherbar machen. |
| Add Object | Fachlich erforderlich, auch wenn kein Screenshot explizit vorhanden ist. |
| Add Object Association | Modal für Objektlink vorbereiten/anbinden. |

### Erwartetes Ergebnis

Nutzer können Snapshot-Daten im Objektdiagramm sehen und selektieren.

### Betroffene Komponenten

- `ObjectDiagramPage`
- `ObjectDiagramCanvas`
- `ObjectNode`
- `SlotValueList`
- `ObjectLinkEdge`
- `AddObjectModal`
- `AddObjectAssociationModal`

### Benötigte API-Daten

- `ObjectModelDto`
- `ObjectInstanceDto`
- `SlotDto`
- `ObjectLinkDto`
- `UmlClassDto`
- `UmlAssociationDto`
- `LayoutDto`

### Benötigter State

- `objectModelState`
- `selectionState`
- `diagramLayoutState`
- `validationState`
- `modalState`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `06-object-diagram-object-properties.png` | Objektkarte mit Slots. |
| `11-modal-add-object-association.png` | Object Link Erstellung. |
| `12-object-diagram-association-properties.png` | Object Link Edge und Properties. |

### Abhängigkeiten

- Schritte 5 bis 7.
- UML-Modell muss Klassen und Associations bereitstellen.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-OBJ-001` | Objekt `alice : User` wird angezeigt. |
| `FE-OBJ-002` | Slot-Werte werden angezeigt. |
| `FE-OBJ-003` | Object Link wird als Edge angezeigt. |
| `FE-OBJ-004` | Objektselektion aktualisiert Properties Panel. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Objektanlage fehlt im Screenshot und wird vergessen | `AddObjectModal` als fachlich notwendige MVP-Komponente einplanen. |
| Slots passen nicht zu Klassendefinition | Backend-validierte Semantik nutzen, Frontend nur UI-Vorvalidierung. |

### Akzeptanzkriterien

- Object Diagram zeigt Objekte, Typen, Slots und Links.
- Objekte und Links sind selektierbar.
- Layout kann gespeichert werden.

### MVP-Relevanz

Sehr hoch.

## Schritt 12: Properties Panel für Objektdiagramm

### Ziel

Das Properties Panel zeigt und bearbeitet selektionsabhängig Objektinstanzen und Objektlinks.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Object Properties | Objektname, Klasse, Slots anzeigen/bearbeiten. |
| Slot Editor | Werte für String, Integer, Real, Boolean. |
| Object Association Properties | Association, Source Object, Target Object anzeigen. |
| Link Bearbeitung | Link ändern oder löschen, sofern MVP-API vorhanden. |
| Fehleranzeige | Slot- und Linkfehler feldnah anzeigen. |

### Erwartetes Ergebnis

Nutzer können Objektwerte setzen und Objektlinks kontrollieren.

### Betroffene Komponenten

- `ObjectPropertiesPanel`
- `SlotValueEditor`
- `ObjectAssociationPropertiesPanel`
- `ObjectSelect`
- `AssociationSelect`

### Benötigte API-Daten

- `ObjectInstanceDto`
- `SlotDto`
- `ObjectLinkDto`
- `UmlClassDto`
- `UmlAttributeDto`
- `UmlAssociationDto`
- `ValidationErrorDto`

### Benötigter State

- `selectionState`
- `formState`
- `objectModelState`
- `apiMutationState`
- `validationState`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `06-object-diagram-object-properties.png` | Object Properties. |
| `12-object-diagram-association-properties.png` | Object Link Properties. |

### Abhängigkeiten

- Schritt 11.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-PROP-OD-001` | Objektselektion zeigt Object Properties. |
| `FE-PROP-OD-002` | Slot `books` kann bearbeitet werden. |
| `FE-PROP-OD-003` | Linkselektion zeigt Object Association Properties. |
| `FE-PROP-OD-004` | Ungültiger Slotwert zeigt Formularfehler oder Backendfehler. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Slot-Typvalidierung wird im Frontend zu fachlich | Nur einfache Eingabeprüfung, Backend bleibt Wahrheit. |
| Linkbearbeitung wird komplex | MVP kann Link anzeigen/erstellen priorisieren, Bearbeitung reduzieren. |

### Akzeptanzkriterien

- Slotwerte können gesetzt werden.
- Object Link Properties sind sichtbar.
- Fehler aus Validation Results können auf Slots/Links gezeigt werden.

### MVP-Relevanz

Hoch.

## Schritt 12a: Delete-Aktionen für Modell- und Snapshot-Elemente

### Ziel

Das Frontend bietet konsistente Löschaktionen für selektierte UML- und Snapshot-Elemente. Die fachliche Löschung erfolgt über das Backend; das Frontend steuert Nutzerinteraktion, Bestätigung, API-Aufruf und State-Bereinigung.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Delete Controls im Properties Panel | Delete-Buttons für Klasse, Association, Invariante, Objekt und Objektlink ergänzen. |
| Attribute löschen | In den Class Properties einzelne Attribute entfernen können. |
| Operationen löschen | In den Class Properties einzelne Operationen entfernen können. |
| Explorer-Aktionen | Delete-Aktion für Einträge in Classes, Associations, Invariants, Objects und Object Links vorbereiten. |
| Canvas-Tastaturaktion | Optional `Delete`/`Backspace` für selektierte Nodes/Edges unterstützen. |
| Confirm Dialog | Vor destruktiven Aktionen bestätigen lassen und Cascade-Folgen verständlich benennen. |
| API-Anbindung | Delete-Methoden des API Clients verwenden und aktualisiertes `ProjectDto` in den State übernehmen. |
| Selection Cleanup | Auswahl leeren oder auf sinnvollen Nachfolger setzen, wenn das selektierte Element gelöscht wurde. |
| Validation Cleanup | Validation Errors für gelöschte IDs aus dem Frontend-State entfernen oder nach Backend-Response ersetzen. |
| Layout Cleanup | Lokale Node-/Edge-Layoutdaten gelöschter Elemente entfernen. |

### Erwartetes Ergebnis

Nutzer können fehlerhaft angelegte Klassen, Attribute, Operationen, Associations, Objekte, Objektlinks und Invarianten entfernen. Diagramm, Explorer, Properties Panel und Validation Results bleiben danach konsistent.

### Betroffene Komponenten

| Komponente | Zweck |
|---|---|
| `ClassPropertiesPanel` | Klasse, Attribute und Operationen löschen. |
| `AssociationPropertiesPanel` | Association löschen. |
| `InvariantPropertiesPanel` | Invariante löschen. |
| `ObjectPropertiesPanel` | Objekt löschen. |
| `ObjectAssociationPropertiesPanel` | Objektlink löschen. |
| `ExplorerSidebar` | Delete-Aktionen für Listeneinträge. |
| `ConfirmDeleteDialog` | Bestätigung und Hinweis auf Cascade Delete. |
| `DiagramCanvasBase` | Optional Tastatur-Delete für selektierte Nodes/Edges. |
| `apiClient` | Delete-Endpunkte kapseln. |
| `projectStore` / `selectionStore` / `validationStore` | State nach Delete bereinigen. |

### Benötigte API-Daten

- `ProjectDto`
- `ApiErrorDto`
- optional später `DeleteImpactDto`

### Benötigter State

| State | Beschreibung |
|---|---|
| `selectionState` | Erkennt, welches Element gelöscht werden soll. |
| `modalState` | Steuert Confirm Dialog. |
| `apiMutationState` | Loading/Error während Delete. |
| `projectState` | Übernimmt aktualisiertes Projekt nach Backend-Delete. |
| `validationState` | Entfernt stale Validation Targets. |
| `diagramLayoutState` | Entfernt lokale Layoutdaten gelöschter Nodes/Edges. |

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `01-class-diagram-class-properties.png` | Delete-Aktion im Class Properties Panel. |
| `02-class-diagram-association-properties.png` | Delete-Aktion für Association Properties. |
| `03-class-diagram-invariant-properties.png` | Delete-Aktion für Invariant Properties. |
| `06-object-diagram-object-properties.png` | Delete-Aktion für Object Properties. |
| `12-object-diagram-association-properties.png` | Delete-Aktion für Object Link Properties. |

### Abhängigkeiten

- Schritt 5: API Client und DTOs.
- Schritt 6: State Management.
- Schritt 8 bis 12: Diagramm-Views und Properties Panels.
- Backend-Schritt 14a: Delete-Endpunkte und Cascade-Regeln.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-DELETE-001` | Klasse selektieren, Delete bestätigen, Klasse verschwindet aus Canvas und Explorer. |
| `FE-DELETE-002` | Attribut aus Class Properties löschen, Attribut verschwindet aus Klassenkarte. |
| `FE-DELETE-003` | Operation aus Class Properties löschen, Operation verschwindet aus Klassenkarte. |
| `FE-DELETE-004` | Association löschen, Edge verschwindet aus Canvas und Explorer. |
| `FE-DELETE-005` | Objekt löschen, Node und zugehörige Object Links verschwinden. |
| `FE-DELETE-006` | Objektlink löschen, Edge verschwindet und Properties Panel wird geleert. |
| `FE-DELETE-007` | Delete-API-Fehler wird im Formular oder in der Console angezeigt. |
| `FE-DELETE-008` | Validation Results referenzieren nach Delete keine gelöschten UI-Elemente mehr. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Nutzer löscht versehentlich abhängige Elemente | Confirm Dialog mit Cascade-Hinweis. |
| Frontend entfernt lokal anders als Backend | Backend-Response als Quelle für aktualisierten Projektzustand nutzen. |
| Validation Badges bleiben sichtbar | Validation State nach Delete gezielt bereinigen. |
| Tastatur-Delete kollidiert mit Texteingaben | Keyboard Shortcut nur aktivieren, wenn kein Input/Textarea fokussiert ist. |

### Akzeptanzkriterien

- Delete-Aktionen sind für alle MVP-Elementtypen erreichbar.
- Vor destruktiven Aktionen erscheint ein Confirm Dialog.
- Nach erfolgreichem Delete sind Canvas, Explorer, Properties Panel, Layout State und Validation State konsistent.
- API-Fehler werden strukturiert angezeigt.
- Fachliche Cascade-Regeln werden nicht im Frontend entschieden, sondern aus dem Backend-Verhalten übernommen.

### MVP-Relevanz

Hoch. Ohne Löschfunktionen können Nutzer Modellierungsfehler nicht korrigieren und der MVP-Workflow bleibt in der Praxis blockierend.

## Schritt 13: OCL Editor UI und Invariantendarstellung

### Ziel

Frontend unterstützt OCL-Invarianten in Modal, Properties Panel und einer vollwertigen OCL Editor View.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| OCL Editor View | Eigene Hauptansicht mit textuellem Modell-/OCL-Editor, Zeilennummern, `Apply Changes`, Console und Diagnosen. |
| Model Text Editor | Vollständigen USE-ähnlichen Modelltext aus `.use`-Dateien mit Klassen, Attributen, Operationen, Associations, Constraints und perspektivisch Imports anzeigen und bearbeiten. |
| Invariant Properties | Name, Kontextklasse, Ausdruck. |
| OCL Input | Texteditor-Komponente für den MVP, zunächst ohne komplexes Syntax Highlighting. |
| Parse/Typecheck Feedback | Backend-Diagnostics anzeigen, wenn Endpoint verfügbar. |
| Invariant Badges | Invarianten im Klassendiagramm sichtbar machen. |
| OCL Error Location | Syntax-/Typefehler am Ausdruck anzeigen. |
| View-Synchronisation | Der gesamte Modelltext wird nach `Apply Changes` mit Projektzustand, Diagrammen, Explorer, Properties Panel und Validation State synchronisiert. |

### Erwartetes Ergebnis

Nutzer können den textuellen Modell-/OCL-Stand im OCL Editor ansehen, bearbeiten, über `Apply Changes` übernehmen und OCL-/Modellfeedback aus Backend-Diagnosen sehen. Die Ansicht ist kein Platzhalter; sie ist über `/projects/{projectId}/ocl` erreichbar.

### Betroffene Komponenten

- `OclEditorPage`
- `ModelTextEditor`
- `LineNumberGutter`
- `ApplyChangesButton`
- `OclDiagnosticsPanel`
- `OclEditorActions`
- `InvariantPropertiesPanel`
- `OclExpressionInput`
- `OclFeedbackMessage`
- `InvariantBadge`
- `AddInvariantModal`

### Benötigte API-Daten

- `UmlInvariantDto`
- `UmlClassDto`
- `OclParseRequestDto`
- `OclParseResponseDto`
- `OclTypecheckRequestDto`
- `OclTypecheckResponseDto`
- `OclDiagnosticDto`

### Benötigter State

- `oclDraftState`
- `oclDiagnosticsState`
- `modelTextDraftState`
- `selectionState`
- `validationState`
- `apiMutationState`
- `validationStaleState`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `03-class-diagram-invariant-properties.png` | Invariant Properties. |
| `09-modal-add-invariant.png` | Add Invariant Modal. |
| `07-object-diagram-validation-error.png` | OCL-Ergebnis im Validation Panel. |
| `13-ocl-editor.png` | OCL Editor View mit textuellem Modell-/OCL-Editor, Zeilennummern, `Apply Changes`, Console und Validierungsaktionen. |

Hinweis: `13-ocl-editor.png` ist vorhanden und ist die verbindliche visuelle Referenz für Schritt 13.

### Abhängigkeiten

- Schritte 5, 8, 9, 10.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-OCL-001` | Invariante kann erstellt werden. |
| `FE-OCL-002` | Kontextklasse kann ausgewählt werden. |
| `FE-OCL-003` | OCL-Ausdruck wird gespeichert. |
| `FE-OCL-004` | Syntaxfehler wird am Ausdruck angezeigt. |
| `FE-OCL-005` | Invariant Badge ist im Klassendiagramm sichtbar. |
| `FE-OCL-006` | OCL Editor View rendert Modelltext, Zeilennummern, `Apply Changes`, Console und Diagnosen. |
| `FE-OCL-007` | `Apply Changes` markiert Editoränderungen als angewendet oder zeigt Backend-Diagnosen. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| OCL Editor wird zu umfangreich | MVP mit einfachem Texteditor, `Apply Changes` und Backend-Diagnostics starten; Syntax Highlighting bleibt Post-MVP. |
| Frontend versucht OCL fachlich zu validieren | Nur Backend-Diagnostics anzeigen. |
| Nicht alle `.use`-Konstrukte aus Beispielen sind MVP-fähig | Editor zeigt vollständigen Modelltext; Parser verarbeitet den MVP-Subset und meldet nicht unterstützte Konstrukte strukturiert. |

### Akzeptanzkriterien

- Invarianten können erstellt und bearbeitet werden.
- OCL Editor View ist über den OCL-Tab erreichbar und zeigt textuellen Modell-/OCL-Editor, Zeilennummern, `Apply Changes`, Console und Diagnosen.
- OCL-Ausdruck bleibt mit Kontextklasse verbunden.
- Backend-Diagnostics können angezeigt werden.
- Nach `Apply Changes` bleiben OCL Editor, Explorer, Properties Panel und Klassendiagramm konsistent oder zeigen Backend-Diagnosen.

### MVP-Relevanz

Hoch.

## Schritt 14: Check Constraints API-Anbindung

### Ziel

Der Button `Check Constraints` ruft die Backend-Validierung auf und speichert das Ergebnis im Validation State.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Button anbinden | Top Bar `CheckConstraintsButton`. |
| API Mutation | `POST /api/v1/projects/{projectId}/validate`. |
| Loading State | Button deaktivieren, Spinner/Status anzeigen. |
| Stale Handling | Alte Validation Results während Request markieren oder ersetzen. |
| Response Handling | `ValidationResultDto` in Validation State schreiben. |
| Error Handling | `ApiErrorDto` in Console/Banner anzeigen. |
| Bottom Panel | Nach Response Validation Results öffnen. |

### Erwartetes Ergebnis

Constraint Check funktioniert von jeder Projektansicht aus.

### Betroffene Komponenten

- `TopBar`
- `CheckConstraintsButton`
- `validationApi`
- `ValidationStateProvider`
- `BottomPanel`
- `ConsoleLogPanel`

### Benötigte API-Daten

- `ValidationRequestDto`
- `ValidationResultDto`
- `ValidationErrorDto`
- `ApiErrorDto`

### Benötigter State

- `validationState`
- `apiMutationState`
- `consoleState`
- `bottomPanelState`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `01-class-diagram-class-properties.png` | Check Constraints in Top Bar. |
| `07-object-diagram-validation-error.png` | Ergebnis nach Check. |

### Abhängigkeiten

- Schritt 5.
- Backend Validation API oder Mock.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-CHECK-001` | Klick ruft Validation API auf. |
| `FE-CHECK-002` | Loading State wird angezeigt. |
| `FE-CHECK-003` | Response aktualisiert Validation State. |
| `FE-CHECK-004` | API Error wird in Console/Banner angezeigt. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Lokale Änderungen sind nicht gespeichert | Save-before-validate oder Draft-Validation klar entscheiden. |
| Button ist in falschen Views nicht verfügbar | Top Bar viewübergreifend halten. |

### Akzeptanzkriterien

- `Check Constraints` löst Backend-Validierung aus.
- Ergebnisse sind im Frontend-State verfügbar.
- Bottom Panel zeigt Validation Results.

### MVP-Relevanz

Sehr hoch.

## Schritt 15: Validation Results UI

### Ziel

Validation Results werden im Bottom Panel verständlich und navigierbar angezeigt.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Summary | Error/Warning/Info Count anzeigen. |
| Fehlerliste | `ValidationErrorDto[]` rendern. |
| Detailansicht | Code, Message, Kontextobjekt, Invariante, Ausdruck, Suggested Fix. |
| Empty/Valid State | Gültiger Zustand klar anzeigen. |
| Gruppierung optional | Nach Severity oder Elementtyp. |
| Klickverhalten | Fehler fokussiert Zielobjekt oder Zielausdruck. |
| Console Logs | Validation gestartet/beendet protokollieren. |

### Erwartetes Ergebnis

Nutzer können Validierungsfehler textuell verstehen und von dort zum betroffenen Element springen.

### Betroffene Komponenten

- `ValidationResultsPanel`
- `ValidationSummary`
- `ValidationErrorList`
- `ValidationErrorItem`
- `ValidationErrorDetails`
- `FocusTargetResolver`
- `ConsoleLogPanel`

### Benötigte API-Daten

- `ValidationResultDto`
- `ValidationErrorDto`
- `ElementTargetDto`

### Benötigter State

- `validationState`
- `errorMappingState`
- `focusState`
- `selectionState`
- `bottomPanelState`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `07-object-diagram-validation-error.png` | Validation Results Panel mit Fehlerliste. |

### Abhängigkeiten

- Schritt 14.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-VALUI-001` | Fehlerliste zeigt `INVARIANT_VIOLATION`. |
| `FE-VALUI-002` | Valid State zeigt keine Fehler. |
| `FE-VALUI-003` | Klick auf Fehler setzt Selection State. |
| `FE-VALUI-004` | Fehlerdetails zeigen Invariante und Ausdruck. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Fehlertexte sind nicht hilfreich | `userMessage`, Code und Kontext zusammen anzeigen. |
| Klick findet kein UI-Element | Fallback anzeigen und Mapping testen. |

### Akzeptanzkriterien

- Validation Results Panel zeigt Fehler verständlich.
- Fehler sind anklickbar.
- Valid/Invalid/Error States sind unterscheidbar.

### MVP-Relevanz

Sehr hoch.

## Schritt 16: Fehler-Markierung im Diagramm

### Ziel

Diagrammelemente werden anhand strukturierter Validation Results visuell markiert.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Error Index | Errors nach `objectId`, `linkId`, `classId`, `invariantId`, `associationId` indexieren. |
| Object Highlight | Fehlerhafte Objekte mit rotem Rahmen markieren. |
| Validation Badge | Fehleranzahl/Severity am Node anzeigen. |
| Edge Highlight | Fehlerhafte Links/Associations hervorheben. |
| Explorer Indicator | Fehlerindikatoren optional in Sidebar anzeigen. |
| Fokusverhalten | Klick aus Validation Results zentriert und selektiert Element. |
| Clearing | Neuer erfolgreicher Check entfernt alte Markierungen. |

### Erwartetes Ergebnis

Der Screenshot `07-object-diagram-validation-error.png` ist funktional nachbildbar: fehlerhaftes Objekt plus Validation Results.

### Betroffene Komponenten

- `ObjectNode`
- `ObjectLinkEdge`
- `UmlClassNode`
- `UmlAssociationEdge`
- `InvariantBadge`
- `ValidationBadge`
- `InvalidObjectHighlight`
- `FocusTargetResolver`

### Benötigte API-Daten

- `ValidationErrorDto`
- `ElementTargetDto`
- `ObjectInstanceDto`
- `ObjectLinkDto`
- `UmlClassDto`
- `UmlInvariantDto`

### Benötigter State

- `validationState`
- `errorMappingState`
- `selectionState`
- `diagramViewportState`

### Relevante Screenshots

| Screenshot | Umsetzung |
|---|---|
| `07-object-diagram-validation-error.png` | Roter Rahmen, Badge, Panel-Verknüpfung. |
| `12-object-diagram-association-properties.png` | Object Link als mögliches Fehlerziel. |
| `03-class-diagram-invariant-properties.png` | Invariante als mögliches Fehlerziel. |

### Abhängigkeiten

- Schritte 11, 15.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-ERRMAP-001` | `contextObjectId=obj-alice` markiert Object Node. |
| `FE-ERRMAP-002` | Fehler-Badge zeigt Anzahl. |
| `FE-ERRMAP-003` | Klick auf Fehler fokussiert Objekt. |
| `FE-ERRMAP-004` | Neuer Valid State entfernt Markierungen. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Error Mapping wird pro Komponente dupliziert | Zentralen Selector/Resolver bauen. |
| Markierung überlagert Text | CSS mit stabilen Abständen und Badge-Position testen. |

### Akzeptanzkriterien

- Fehlerhafte Objekte sind eindeutig sichtbar.
- Fehlerliste und Diagramm sind gekoppelt.
- Markierungen verschwinden bei gültigem Ergebnis.

### MVP-Relevanz

Sehr hoch.

## Schritt 17: End-to-End Library Demo

### Ziel

Der zentrale MVP-Workflow wird im Frontend als demonstrierbares Szenario abgesichert.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Demo-Daten | Library-Projekt als Mock oder Backend-Fixture. |
| Workflow | Dashboard -> Start Project -> Projektname eingeben -> Class Diagram -> Object Diagram -> Check Constraints. |
| Modellierung | `User`, `Book`, `Borrows`, `maxBooks`. |
| Snapshot | `alice : User`, `books = 6`, `mobyDick : Book`, Link. |
| Validation | `INVARIANT_VIOLATION` anzeigen. |
| Fokus | Fehlerklick fokussiert `alice : User`. |
| Save/Load | Optional Projektzustand inklusive Layout laden/speichern. |

### Erwartetes Ergebnis

Die Anwendung kann den MVP live demonstrieren.

### Betroffene Komponenten

Alle MVP-Komponenten:

- Dashboard,
- App Shell,
- Class Diagram,
- Object Diagram,
- OCL UI,
- Properties Panel,
- Modals,
- Validation Results,
- Error Mapping.

### Benötigte API-Daten

- vollständiges `ProjectDto`,
- `ValidationResultDto`,
- Library-Fixture.

### Benötigter State

Alle MVP-State-Bereiche.

### Relevante Screenshots

| Screenshot | Demo-Bezug |
|---|---|
| `00-dashboard-start-page.png` | Start. |
| `04-class-diagram-new-class-selected.png` | Modellierung. |
| `06-object-diagram-object-properties.png` | Snapshot. |
| `09-modal-add-invariant.png` | Invariante. |
| `11-modal-add-object-association.png` | Objektlink. |
| `07-object-diagram-validation-error.png` | Fehleranzeige. |

### Abhängigkeiten

- Schritte 1 bis 16.
- Schritt 12a für vollständige Korrekturworkflows im Demo-Modell.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-E2E-001` | Dashboard ist Startseite. |
| `FE-E2E-002` | Start Project öffnet die Projektnamenerfassung und navigiert nach gültigem Submit ins Class Diagram. |
| `FE-E2E-003` | Library-Modell wird angezeigt oder erstellt. |
| `FE-E2E-004` | Object Diagram zeigt `alice : User`. |
| `FE-E2E-005` | Check Constraints zeigt `INVARIANT_VIOLATION`. |
| `FE-E2E-006` | `alice : User` wird markiert. |
| `FE-E2E-007` | Klick auf Fehler fokussiert Objekt. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Demo hängt von instabilem Backend ab | Mock-Szenario und Backend-Szenario getrennt testbar halten. |
| Workflow ist zu lang für Tests | Playwright-Test modular aufbauen. |

### Akzeptanzkriterien

- Demo kann reproduzierbar durchlaufen werden.
- Fehleranzeige entspricht funktional dem Screenshot.
- Frontend nutzt strukturierte Backend-Ergebnisse, keine Textauswertung.

### MVP-Relevanz

Sehr hoch.

## Schritt 18: Frontend-Tests und UI-Stabilisierung

### Ziel

Das Frontend wird durch Komponenten-, State-, API-Mock- und E2E-Tests stabilisiert.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Komponenten-Tests | Dashboard, Nodes, Edges, Properties Panel, Modals, Validation Panel. |
| State-Tests | Selection, Validation Error Index, Layout State. |
| API-Client-Tests | Mock Responses, ApiErrorDto, ValidationResultDto. |
| E2E-Tests | Library Demo mit Playwright. |
| Accessibility Basics | Fokus, Buttons, Dialoge, Fehleranzeigen. |
| Responsive Checks | Dashboard und Workspace auf sinnvoller Mindestbreite prüfen. |
| UI-Stabilisierung | Textüberläufe, Badge-Positionen, leere Zustände, Loading States. |

### Erwartetes Ergebnis

Der Frontend-MVP ist stabil genug für Integration und Demo.

### Betroffene Komponenten

Alle MVP-Komponenten.

### Benötigte API-Daten

- Mockdaten für `ProjectDto`, `ProjectSummaryDto`, `ValidationResultDto`, `ApiErrorDto`.

### Benötigter State

Alle MVP-State-Bereiche.

### Relevante Screenshots

Alle Screenshots, besonders:

- `00-dashboard-start-page.png`,
- `01-class-diagram-class-properties.png`,
- `06-object-diagram-object-properties.png`,
- `07-object-diagram-validation-error.png`,
- `08-modal-add-class.png`,
- `09-modal-add-invariant.png`,
- `10-modal-add-class-association.png`,
- `11-modal-add-object-association.png`,
- `12-object-diagram-association-properties.png`.

### Abhängigkeiten

- Schritte 1 bis 17.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `FE-TEST-001` | Dashboard rendert Startoptionen. |
| `FE-TEST-002` | Add Class Modal erstellt Klasse. |
| `FE-TEST-003` | Class Properties folgt Selektion. |
| `FE-TEST-004` | Object Properties bearbeitet Slotwerte. |
| `FE-TEST-005` | Check Constraints ruft API auf. |
| `FE-TEST-006` | Validation Results Panel zeigt Fehler. |
| `FE-TEST-007` | Fehlerhaftes Objekt wird markiert. |
| `FE-TEST-008` | E2E Library Demo läuft. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Diagrammtests sind fragil | Komponentenlogik und Mapping getrennt von Canvas testen. |
| E2E hängt an Timing | Mock API und stabile Selektoren verwenden. |
| UI-Regressionen bleiben unbemerkt | Screenshot-nahe Storybook/Playwright-Szenarien Post-MVP prüfen. |

### Akzeptanzkriterien

- Kernkomponenten sind getestet.
- Library E2E läuft.
- Validation Error Mapping ist regressionsgesichert.
- Dashboard bis Validation Results ist stabil bedienbar.

### MVP-Relevanz

Sehr hoch.

## Frontend-MVP-Schnitt

Der Frontend-MVP ist erreicht, wenn folgende Kriterien erfüllt sind:

| Bereich | MVP-Kriterium |
|---|---|
| Dashboard | Start Page mit `Start Project`, `Open Existing`, Open-Existing-Dialog für lokale `.use` Dateien, Recent Projects und Support-Einstiegen ist sichtbar. |
| All Projects | `View all` öffnet eine Projektliste aus `19-projects.png`; Suche ist mindestens clientseitig möglich. |
| Projektstart | `Start Project` erfasst einen Projektnamen und führt nach erfolgreicher Projektanlage ins Class Diagram. |
| Routing | Class Diagram, Object Diagram und OCL Editor sind erreichbar. |
| Class Diagram | Klassen, Attribute, Operationen, Associations und Invarianten werden angezeigt und selektiert. |
| Object Diagram | Objekte, Slotwerte und Objektlinks werden angezeigt und selektiert. |
| Properties Panel | Klasse, Association, Invariante, Objekt und Objektlink haben passende Panels. |
| Modals | Klasse, Association, Invariante und Objektlink können erstellt werden. |
| OCL | OCL-Ausdrücke können erfasst und mit Invarianten verbunden werden. |
| Check Constraints | Frontend ruft Backend-Validation auf. |
| Validation Results | Fehler werden textuell angezeigt. |
| Diagramm-Fehler | Fehlerhafte Objekte werden visuell markiert. |
| Tests | Komponenten- und E2E-Kernfälle sind abgesichert. |

Nicht zwingend im Frontend-MVP:

- vollständiger `.use` Import,
- echtes Recent-Project-Backend, falls Persistenz noch einfach ist,
- serverseitige Filter, Sortierung und Pagination für All Projects,
- OCL Autocomplete,
- vollständiges Syntax Highlighting,
- Undo/Redo,
- kollaboratives Arbeiten,
- komplexes Auto-Layout,
- vollständige Responsive-Optimierung für kleine Mobilgeräte.

## Frontend-Post-MVP

| Erweiterung | Umsetzungspfad |
|---|---|
| `.use` Import UI | Importdialog mit Diagnostics und Mapping auf neues Projektformat. |
| Recent/All Projects vollständig | Backendbasierte Projektliste mit Suche, Filter, Sortierung, Pagination und Thumbnails. |
| OCL Syntax Highlighting | Editor-Komponente mit Token-/Diagnostic-Unterstützung. |
| OCL Autocomplete | Backend-gestützte Vorschläge für Attribute, Rollen und Operationen. |
| Undo/Redo | Command- oder State-History für Modelländerungen. |
| Auto Layout | Optionaler Layoutalgorithmus für Klassen und Objekte. |
| Mehrere Snapshots | Snapshot-Auswahl und Vergleichsansichten. |
| Vererbung/Enums | UI-Elemente für Generalization und Enumerations. |
| Erweiterte Validation UX | Gruppierung, Filter, Quick Fixes, Error History. |
| Storybook | Komponentenbibliothek für Nodes, Panels, Modals und Fehlerzustände. |

## Risiken

| Risiko | Auswirkung | Gegenmaßnahme |
|---|---|---|
| Diagrammbibliothek wird zu spät entschieden | Class/Object Diagram verzögern sich | React Flow früh prototypisch integrieren. |
| Frontend dupliziert Backend-Semantik | Inkonsistente Validierung | Nur UI-Vorvalidierung, Backend-Diagnostics anzeigen. |
| Error Mapping wird uneinheitlich | Fehler können nicht fokussiert werden | Zentralen `FocusTargetResolver` und Error Index bauen. |
| Dashboard-Flow wird vergessen | MVP startet falsch im Klassendiagramm | Dashboard als Route `/` und ersten E2E-Schritt definieren. |
| DTOs driften vom Backend ab | API-Integration bricht | DTO-Referenz und Mockdaten aktuell halten. |
| Diagrammtext überläuft | UI wirkt instabil | Node-Größen, Overflow-Regeln und Tests definieren. |
| Recent Projects blockieren MVP | Dashboard verzögert sich | Mockdaten oder Should-Scope nutzen. |
| Validation UI wird nur textuell | Screenshot-Anforderung verfehlt | Diagramm-Markierung als eigener Schritt 16. |

## Zusammenfassung

Die Frontend-Implementierung sollte mit Vite/React/TypeScript, Routing und Dashboard beginnen. Der erste sichtbare MVP-Durchstich ist `Dashboard -> Start Project -> Projektname eingeben -> Class Diagram`. Danach folgen API Client, State Management und Diagrammbibliothek als technische Grundlage für Class Diagram und Object Diagram.

Für den MVP ist React Flow die empfohlene Diagrammbibliothek, sofern ein früher Prototyp Custom Nodes, Custom Edges, Labels, Selektion, Drag & Drop und Fehlerzustände bestätigt. Die wichtigsten Integrationspunkte sind `ProjectDto`, `LayoutDto`, `ValidationResultDto` und `ValidationErrorDto`, weil sie Diagramm, Properties Panel und Validation Results verbinden.

Der Frontend-MVP ist demonstrierbar, wenn das Library-Szenario vom Dashboard bis zur `INVARIANT_VIOLATION` im Object Diagram mit roter Markierung und Validation Results Panel funktioniert.
