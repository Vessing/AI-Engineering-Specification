# Backend Implementation Plan

## Zweck dieser Datei

Diese Datei beschreibt einen konkreten Implementierungsplan für das neue Java/Spring-Boot-Backend des webbasierten UML/OCL-Systems.

Der Plan ist nicht primär nach Epics gegliedert, sondern nach umsetzbaren Backend-Schritten. Jeder Schritt beschreibt Ziel, Aufgaben, erwartetes Ergebnis, betroffene Packages, DTOs, Abhängigkeiten, Testfälle, Risiken, Akzeptanzkriterien und relevante Analyse-Dateien.

Das Backend ist die fachliche Semantikschicht des Zielsystems. Es verwaltet Projekte, UML-Modelle, Objektmodelle/Snapshots, OCL-Invarianten und Validierungsergebnisse. Es stellt REST/JSON-Endpunkte für das React/TypeScript-Frontend bereit.

## Backend-Planungsprinzipien

| Prinzip | Konsequenz für die Umsetzung |
|---|---|
| Neues Backend statt USE-Core | Kein Fork, keine Migration, keine Runtime Dependency auf das originale USE-Projekt. |
| Backend als fachliche Wahrheit | UML-Struktur, Snapshot-Konsistenz, Multiplicity Checks und OCL-Validierung laufen im Backend. |
| REST/JSON als Integrationsgrenze | Frontend und Backend kommunizieren über klar dokumentierte DTOs. |
| JSON-Projektformat im MVP | MVP-Speichern/Laden nutzt ein eigenes JSON-Format. Der OCL Editor kann zusätzlich vollständigen USE-ähnlichen Modelltext speichern und über einen begrenzten Apply-Flow anwenden; vollständiger `.use` Import bleibt perspektivisch. |
| Stabile IDs | Klassen, Attribute, Associations, Invarianten, Objekte, Slots und Links bekommen stabile IDs. |
| OCL nicht per Regex | OCL wird über Lexer, Parser, AST, Typechecker und Evaluator verarbeitet. |
| Kleine, testbare Schritte | Jede Schicht wird isoliert testbar aufgebaut. |
| Erweiterbarkeit vor Vollständigkeit | MVP-Subset sauber bauen, Post-MVP-Konstrukte vorbereiten. |

## Schrittübersicht

| Schritt | Thema | Kernziel | MVP-Relevanz |
|---:|---|---|---|
| 1 | Backend-Projekt initialisieren | Spring-Boot-Projekt startfähig machen | Hoch |
| 2 | Package- und Schichtenstruktur | Architekturgrenzen im Code anlegen | Hoch |
| 3 | Domänenmodell umsetzen | Project, UML, Snapshot, Validation fachlich modellieren | Hoch |
| 4 | JSON-Projektformat und DTOs | API-/Persistenzformat definieren und serialisieren | Hoch |
| 5 | Project Service | Projekte anlegen, laden, speichern | Hoch |
| 6 | Dashboard-nahe Funktionen | Create Project und Recent Projects vorbereiten | Mittel/Hoch |
| 6a | Create Project Name Contract | `CreateProjectRequestDto.name` als Pflichtfeld validieren und initiale Projektmetadaten konsistent erzeugen | Hoch |
| 6b | All Projects / Project List API | vollständige Projektliste für `19-projects.png` als `ProjectSummaryDto[]` bereitstellen | Should |
| 7 | UML Model Service | Klassenmodell fachlich verwalten | Hoch |
| 8 | Object Model / Snapshot Service | Objekte, Slots und Links verwalten | Hoch |
| 9 | OCL Lexer und Parser-Grundlage | OCL-Text in Tokens und parsebare Struktur überführen | Hoch |
| 10 | OCL AST-Struktur | Ausdrucksbaum für MVP-Subset modellieren | Hoch |
| 11 | OCL Typechecker | OCL gegen UML-Kontext typprüfen | Hoch |
| 12 | OCL Evaluator | OCL gegen Snapshot auswerten | Hoch |
| 13 | Validation Service | UML-, Snapshot-, Multiplicity- und OCL-Prüfung koordinieren | Hoch |
| 14 | REST API | Projekt-, UML-, Objekt-, OCL- und Validation-Endpunkte bereitstellen | Hoch |
| 14a | Delete-Operationen und Cascade-Regeln | Klassen, Attribute, Operationen, Associations, Objekte und Links fachlich konsistent löschen | Hoch |
| 15 | Error- und Result-Modell | API-Fehler und Validation Results vereinheitlichen | Hoch |
| 16 | Tests und Library-Szenario | Backend-Funktionalität regressionssicher machen | Hoch |
| 16a | Modelltext-Apply-Flow | OCL-Editor-Text und lokale `.use` Dateien aus Open Existing speichern, anwenden und diagnostizieren | Hoch |


## Schritt 1: Backend-Projekt initialisieren

### Ziel

Ein neues, eigenständiges Spring-Boot-Backend wird initialisiert. Es ist kein Teil des originalen USE-Repositories und verwendet den USE-Core nicht als Dependency.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Projekt erzeugen | Neues Java/Spring-Boot-Projekt mit Maven oder Gradle anlegen. |
| Java-Version festlegen | Eine aktuelle LTS-Version wählen, z. B. Java 21. |
| Spring Dependencies wählen | Spring Web, Validation, Jackson, Test, optional Actuator. |
| Basiskonfiguration | Application-Klasse, `application.yml`, Testprofil. |
| Build-Skripte | `mvn test` oder `gradle test` lauffähig machen. |
| Codeformat | Formatter/Linter-Konventionen vorbereiten. |
| README-Grundlage | Startbefehle, Architekturhinweis, keine USE-Core-Abhängigkeit dokumentieren. |

### Erwartetes Ergebnis

- Backend startet lokal.
- Ein einfacher Health- oder Root-Endpunkt kann beantwortet werden.
- Tests laufen.
- Keine Abhängigkeit auf originale USE-Artefakte existiert.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `com.example.useweb` oder finaler Projektpackage-Name | Root Package |
| `config` | Spring-Konfiguration |
| `api` | Erste Controller-Schicht |

### Benötigte DTOs

| DTO | Nutzung |
|---|---|
| `ApiErrorDto` | Noch optional, später für Fehlerantworten. |
| `ProjectSummaryDto` | Noch nicht implementieren, aber als späterer Dashboard-DTO beachten. |

### Abhängigkeiten

- Entscheidung für Build-System.
- Entscheidung für Java-Version.
- Backend-Repository muss getrennt vom Analyse-Repository entstehen.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-SETUP-001` | Applikationskontext startet. |
| `BE-SETUP-002` | Build läuft ohne Fehler. |
| `BE-SETUP-003` | Kein USE-Core als Dependency im Build vorhanden. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Projektsetup wird überladen | Nur MVP-relevante Dependencies aufnehmen. |
| Package-Name muss später geändert werden | Früh finalen Namen festlegen oder Refactoring einplanen. |

### Akzeptanzkriterien

| Kriterium | Erfüllt, wenn |
|---|---|
| Startfähig | Backend startet lokal ohne Fachlogik. |
| Testfähig | Ein Basistest läuft im Build. |
| Eigenständig | Keine technische USE-Abhängigkeit vorhanden. |

### Relevante Analyse-Dateien

- `05-backend-analysis/01-backend-scope.md`
- `05-backend-analysis/02-backend-repository-structure.md`
- `05-backend-analysis/03-backend-architecture.md`
- `08-planning/01-overall-implementation-roadmap.md`

### MVP-Relevanz

Hoch. Dieser Schritt ist die technische Grundlage für alle weiteren Backend-Arbeiten.

## Schritt 2: Package- und Schichtenstruktur

### Ziel

Die Backend-Schichten werden im Code sichtbar getrennt, damit API, Application Services, Domain, OCL, Validation, Persistence und Error Handling nicht vermischt werden.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Schichten anlegen | Packages für `api`, `application`, `domain`, `ocl`, `validation`, `persistence`, `error`, `config` erstellen. |
| Domain-Unterpakete | `domain.uml`, `domain.snapshot`, `domain.project`, `domain.validation` anlegen. |
| OCL-Unterpakete | `ocl.lexer`, `ocl.parser`, `ocl.ast`, `ocl.typecheck`, `ocl.evaluation`, `ocl.diagnostics`, `ocl.value` anlegen. |
| DTO-Schicht | `api.dto` oder `contract.dto` für REST-DTOs anlegen. |
| Mapper-Ort | `api.mapper` oder `application.mapper` festlegen. |
| Teststruktur spiegeln | Unit-, Service- und API-Testpakete entsprechend vorbereiten. |

### Erwartetes Ergebnis

Eine leere, aber klare Backend-Struktur liegt vor. Neue Klassen haben einen eindeutigen Zielort.

### Betroffene Packages/Services

```text
src/main/java/.../
├─ api/
│  ├─ controller/
│  ├─ dto/
│  └─ mapper/
├─ application/
├─ domain/
│  ├─ project/
│  ├─ uml/
│  ├─ snapshot/
│  └─ validation/
├─ ocl/
│  ├─ lexer/
│  ├─ parser/
│  ├─ ast/
│  ├─ typecheck/
│  ├─ evaluation/
│  ├─ diagnostics/
│  └─ value/
├─ validation/
├─ persistence/
├─ error/
└─ config/
```

### Benötigte DTOs

Noch keine vollständige Implementierung. DTO-Package muss aber für folgende Gruppen vorbereitet sein:

- Projekt-DTOs,
- UML-DTOs,
- Objektmodell-DTOs,
- OCL-DTOs,
- Validation-DTOs,
- Error-DTOs,
- Layout-DTOs.

### Abhängigkeiten

- Schritt 1.
- Repository- und Package-Entscheidung.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-ARCH-001` | Package-Struktur ist vorhanden. |
| `BE-ARCH-002` | API-Schicht hängt nicht direkt von Persistenzdetails ab. |
| `BE-ARCH-003` | OCL-Komponenten sind in eigene Packages getrennt. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Zu viele Packages ohne Nutzen | Struktur an dokumentierten Services ausrichten. |
| DTOs und Domain werden vermischt | Mapper-Schicht von Anfang an vorsehen. |

### Akzeptanzkriterien

- Neue Feature-Klassen haben klare Zielpackages.
- OCL-Pipeline ist strukturell vorbereitet.
- DTOs, Domain und Persistence sind getrennt.

### Relevante Analyse-Dateien

- `05-backend-analysis/02-backend-repository-structure.md`
- `05-backend-analysis/03-backend-architecture.md`
- `07-integration-and-api/07-dto-reference.md`

### MVP-Relevanz

Hoch. Eine saubere Struktur reduziert spätere Kopplung.

## Schritt 3: Domänenmodell umsetzen

### Ziel

Das fachliche Kernmodell für Projekte, UML-Modelle, Objektmodelle/Snapshots und Validierung wird als Backend-Domain umgesetzt.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Project modellieren | `Project`, Metadaten, Referenzen auf UML-Modell, Object Model und Layout. |
| UML-Modell modellieren | `UmlModel`, `UmlClass`, `UmlAttribute`, `UmlOperation`, `UmlParameter`. |
| Association-Modell | `UmlAssociation`, `UmlAssociationEnd`, `Multiplicity`. |
| Invariantenmodell | `UmlInvariant` mit Kontextklasse und OCL-Ausdruck. |
| Snapshot-Modell | `ObjectModel`, `ObjectInstance`, `Slot`, `ObjectLink`. |
| Validation-Modell | `ValidationResult`, `ValidationError`, `ElementTarget`, Severity. |
| ID-Konzept | Stabile IDs als Value Objects oder Strings mit zentralen Regeln. |
| Primitive Typen | `String`, `Integer`, `Real`, `Boolean` modellieren. |

### Erwartetes Ergebnis

Das Backend kann ein Library-artiges Modell im Speicher als Domain-Objekte repräsentieren, ohne API oder Persistenz zu benötigen.

### Betroffene Packages/Services

| Package | Inhalt |
|---|---|
| `domain.project` | `Project`, `ProjectMetadata` |
| `domain.uml` | Klassen, Attribute, Operationen, Associations, Invarianten |
| `domain.snapshot` | Objekte, Slots, Links, Snapshot |
| `domain.validation` | Validation Result, Fehler, Targets |

### Benötigte DTOs

DTOs werden noch nicht zwingend implementiert, aber Domain muss auf folgende DTO-Gruppen mappbar sein:

- `ProjectDto`
- `UmlModelDto`
- `UmlClassDto`
- `UmlAssociationDto`
- `UmlInvariantDto`
- `ObjectModelDto`
- `ObjectInstanceDto`
- `ObjectLinkDto`
- `ValidationResultDto`

### Abhängigkeiten

- Schritt 2.
- Domain Model aus Analyse.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-DOM-001` | `Project` kann UML- und Object Model enthalten. |
| `BE-DOM-002` | `UmlClass` kann Attribute und Operationen enthalten. |
| `BE-DOM-003` | `UmlAssociation` hat genau zwei MVP-Enden mit Multiplicity. |
| `BE-DOM-004` | `UmlInvariant` referenziert eine Kontextklasse. |
| `BE-DOM-005` | `ObjectInstance` referenziert eine Klasse und enthält Slots. |
| `BE-DOM-006` | `ObjectLink` referenziert Association und beteiligte Objekte. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Domain wird API-getrieben statt fachlich | Domain-Klassen unabhängig von DTOs halten. |
| Post-MVP-Felder überfrachten MVP | Optionale Erweiterungen zurückstellen oder nullable/optional halten. |
| IDs werden inkonsistent | ID-Erzeugung und Validierung zentralisieren. |

### Akzeptanzkriterien

- Domain-Objekte können Library-Modell ausdrücken.
- UML-Modell und Snapshot sind klar getrennt.
- Invarianten gehören zum UML-Modell, werden aber später gegen Snapshots ausgewertet.
- Validation Targets können Modell- und UI-relevante IDs aufnehmen.

### Relevante Analyse-Dateien

- `03-uml-ocl-domain/02-domain-model.md`
- `05-backend-analysis/04-domain-model-implementation.md`
- `07-integration-and-api/07-dto-reference.md`

### MVP-Relevanz

Sehr hoch. Das Domänenmodell ist Grundlage für Services, OCL, Validation und API.

## Schritt 4: JSON-Projektformat und DTO-Grundlagen

### Ziel

Das Backend kann Projektzustände in das dokumentierte JSON/DTO-Format übertragen und aus diesem Format laden.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| DTOs implementieren | Records/Klassen für Projekt-, UML-, Objekt-, OCL-, Validation-, Error- und Layout-DTOs anlegen. |
| Mapper implementieren | Domain-zu-DTO und DTO-zu-Domain-Mapping. |
| JSON-Serialisierung | Jackson-Konfiguration und Tests für Projektformat. |
| Modelltext-DTOs | `ModelTextDto`, `ApplyModelTextRequestDto` und `ApplyModelTextResponseDto` für den OCL Editor vorbereiten. |
| Schema-Version | `metadata.schemaVersion` prüfen und schreiben. |
| Layoutdaten | `LayoutDto`, `NodeLayoutDto`, `EdgeLayoutDto` ohne fachliche Semantik abbilden. |
| Importvalidierung | Ungültige JSON-Strukturen früh erkennen. |

### Erwartetes Ergebnis

Ein vollständiges Library-Projekt kann als JSON serialisiert, wieder geladen und auf Domain-Objekte gemappt werden.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `api.dto` | DTO-Records |
| `api.mapper` | Mapping zwischen DTO und Domain |
| `persistence` | JSON Serializer/Deserializer |
| `error` | Formatfehler |

### Benötigte DTOs

- `ProjectDto`
- `ProjectMetadataDto`
- `ProjectSummaryDto`
- `UmlModelDto`
- `UmlClassDto`
- `UmlAttributeDto`
- `UmlOperationDto`
- `UmlParameterDto`
- `UmlAssociationDto`
- `UmlAssociationEndDto`
- `MultiplicityDto`
- `UmlInvariantDto`
- `ObjectModelDto`
- `ObjectInstanceDto`
- `SlotDto`
- `ObjectLinkDto`
- `LayoutDto`
- `NodeLayoutDto`
- `EdgeLayoutDto`
- `ModelTextDto`
- `ApplyModelTextRequestDto`
- `ApplyModelTextResponseDto`
- `ApiErrorDto`

### Abhängigkeiten

- Schritt 3.
- DTO-Referenz.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-JSON-001` | Leeres Projekt wird serialisiert und deserialisiert. |
| `BE-JSON-002` | Library-Projekt bleibt nach Roundtrip fachlich gleich. |
| `BE-JSON-003` | Unbekannte oder fehlende Pflichtfelder erzeugen Formatfehler. |
| `BE-JSON-004` | Layoutdaten werden erhalten, aber nicht fachlich interpretiert. |
| `BE-JSON-005` | Ungültige `schemaVersion` wird erkannt. |
| `BE-JSON-006` | Vollständiger `modelText` bleibt nach JSON-Roundtrip erhalten. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| DTOs werden zu Domain-Modellen | Mapper konsequent trennen. |
| Layoutdaten beeinflussen Semantik | Validation darf Layoutdaten ignorieren. |
| JSON-Format ändert sich unkontrolliert | Versionierung und Snapshot-Tests verwenden. |

### Akzeptanzkriterien

- `ProjectDto` kann vollständiges MVP-Projekt tragen.
- JSON Roundtrip funktioniert.
- Formatfehler werden als `ApiErrorDto` oder Importfehler strukturiert gemeldet.
- Layoutdaten werden gespeichert und geladen.

### Relevante Analyse-Dateien

- `05-backend-analysis/14-json-project-format.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/07-dto-reference.md`
- `07-integration-and-api/08-error-contract.md`

### MVP-Relevanz

Sehr hoch. Ohne JSON/DTO-Grundlagen ist keine stabile Integration möglich.

## Schritt 5: Project Service

### Ziel

Der `ProjectService` verwaltet den Projektlebenszyklus: neues Projekt erstellen, Projekt laden, Projekt speichern, Projekt aktualisieren und Projekt exportieren.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Neues Projekt | Leeres Projekt mit Metadaten, leerem UML-Modell, leerem Object Model und Layout erzeugen. |
| Beispielprojekt optional | Library-Projekt als Demo-/Testprojekt erzeugen. |
| Laden | Projekt anhand ID aus Persistence laden. |
| Speichern | Projektzustand persistieren. |
| Aktualisieren | Vollständiges `ProjectDto` oder Domain-Projekt ersetzen. |
| Export | Projekt als JSON bereitstellen. |
| Validierung beim Laden | Formatversion, Pflichtfelder und ID-Konsistenz grob prüfen. |

### Erwartetes Ergebnis

Das Backend kann Projekte für Dashboard und Projektansicht verwalten. `Start Project` kann ein neues Projekt erzeugen.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `application.ProjectService` | Projektlebenszyklus |
| `persistence.ProjectRepository` | Speicherung |
| `persistence.JsonProjectSerializer` | JSON-Import/Export |
| `api.mapper.ProjectMapper` | DTO-Mapping |

### Benötigte DTOs

- `ProjectDto`
- `ProjectMetadataDto`
- `ProjectSummaryDto`
- `ApiErrorDto`

### Abhängigkeiten

- Schritt 4.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-PROJ-001` | Neues Projekt enthält leeres UML- und Object Model. |
| `BE-PROJ-002` | Projekt kann gespeichert und wieder geladen werden. |
| `BE-PROJ-003` | Nicht existierende Projekt-ID erzeugt `PROJECT_NOT_FOUND`. |
| `BE-PROJ-004` | Export liefert gültiges JSON-Projektformat. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Persistenzentscheidung blockiert MVP | In-memory oder dateibasierte Repository-Abstraktion nutzen. |
| Projektzustand wird inkonsistent gespeichert | Speichern nur über zentrale Service-/Repository-Schicht. |

### Akzeptanzkriterien

- `createProject()` liefert ein gültiges Projekt.
- `loadProject(projectId)` liefert gespeicherten Zustand.
- `saveProject(project)` persistiert UML-, Snapshot- und Layoutdaten.
- Fehler werden strukturiert ausgegeben.

### Relevante Analyse-Dateien

- `05-backend-analysis/05-project-and-persistence-service.md`
- `07-integration-and-api/04-project-save-load-flow.md`
- `07-integration-and-api/02-api-flow.md`

### MVP-Relevanz

Hoch. Projektstart, Save und Load sind MVP-Grundlage.

## Schritt 6: Dashboard-nahe Backend-Funktionen

### Ziel

Backend-Funktionen unterstützen die Dashboard / Start Page: neues Projekt starten, bestehendes Projekt öffnen/importieren und Recent Projects anzeigen.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Create Project API vorbereiten | `Start Project` sendet einen Projektnamen; Backend validiert `CreateProjectRequestDto.name`, erzeugt ein neues Projekt und gibt `ProjectDto` oder `ProjectSummaryDto` zurück. |
| Recent Projects | Liste zuletzt verwendeter Projekte als `ProjectSummaryDto[]`. |
| Open Existing JSON | JSON-Projekt importieren und als Projekt verfügbar machen. |
| Open Existing `.use` Datei | Lokale `.use` Dateien aus dem Dialog `14-open-existing-project.png` als Textquelle an den begrenzten Modelltext-Apply-Flow übergeben. |
| Modelltext Apply vorbereiten | `Apply Changes` aus dem OCL Editor und `Open Project` aus dem Open-Existing-Dialog nehmen vollständigen USE-ähnlichen Text entgegen und verarbeiten das MVP-Subset. |
| `.use` Import vorbereiten | Vollständigen `.use` Import als Post-MVP-Schnittstelle definieren, aber MVP nicht blockieren. |
| Dashboard-Fehler | Import- und Ladefehler als `ApiErrorDto` zurückgeben. |

### Erwartetes Ergebnis

Das Frontend kann das Dashboard an das Backend anbinden. Recent Projects können im MVP notfalls mock- oder dateibasiert geliefert werden.

Für den neuen Open-Existing-Screenshot gilt: Das Backend muss lokale `.use` Dateien im MVP nicht vollständig USE-kompatibel importieren. Es soll den Dateiinhalt als `modelText` annehmen, das dokumentierte MVP-Subset anwenden und nicht unterstützte Syntax als strukturierte Diagnostics zurückgeben.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `application.ProjectService` | Create/Open |
| `application.RecentProjectService` | Recent Projects |
| `application.ImportService` | JSON Import, lokale `.use` Datei als Textquelle orchestrieren; vollständiger `.use` Import später |
| `application.ModelTextApplicationService` | Modelltext speichern/anwenden |
| `modeltext.*` | MVP-Subset aus vollständigem Editor-Text erkennen |
| `persistence.ProjectRepository` | Projektliste |

### Benötigte DTOs

- `ProjectDto`
- `ProjectSummaryDto`
- `ApiErrorDto`
- optional `ImportProjectRequestDto`
- optional `ImportProjectResponseDto`
- `ApplyModelTextRequestDto`
- `ApplyModelTextResponseDto`

### Abhängigkeiten

- Schritt 5.
- Dashboard-Flow aus Produkt- und Integration-Dokumentation.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-DASH-001` | Neues Projekt kann für Dashboard erzeugt werden. |
| `BE-DASH-002` | Recent Projects liefert sortierte Projektzusammenfassungen. |
| `BE-DASH-003` | JSON Import mit gültigem Projekt funktioniert. |
| `BE-DASH-004` | Ungültiges JSON erzeugt `INVALID_PROJECT_FORMAT`. |
| `BE-DASH-005` | `.use` Import-Endpunkt ist im MVP nicht fälschlich als vollständig implementiert markiert. |
| `BE-DASH-006` | `model-text/apply` verarbeitet einen reduzierten Library-Modelltext und liefert Diagnostics für unsupported Syntax. |
| `BE-DASH-007` | Dateiinhalt aus `Open Existing Project` kann mit `sourceName` und `sourceFormat = "use"` an `model-text/apply` übergeben werden. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Recent Projects erfordern Datenbank | Für MVP einfache Repository-Liste oder Mockdaten akzeptieren. |
| `.use` Import wächst in MVP | Modelltext-Apply klar vom vollständigen `.use` Import trennen. |

### Akzeptanzkriterien

- Dashboard kann `Start Project` über Backend auslösen.
- Recent Projects können geliefert oder bewusst als Should gekennzeichnet werden.
- JSON Import ist möglich oder klar als MVP-Option dokumentiert.
- Modelltext-Apply ist für den OCL Editor und für lokale `.use` Dateien aus dem Open-Existing-Dialog vorbereitet.
- Vollständiger `.use` Import ist als Post-MVP-Erweiterung vorbereitet.

### Relevante Analyse-Dateien

- `02-product-and-user-journey/02-screenshot-based-user-journey.md`
- `02-product-and-user-journey/04-mvp-scope.md`
- `07-integration-and-api/04-project-save-load-flow.md`
- `07-integration-and-api/02-api-flow.md`

### MVP-Relevanz

Mittel bis hoch. `Start Project` ist MVP; echte Recent Projects und `.use` Import können Should/Post-MVP sein.

## Schritt 6a: Create Project Name Contract

### Ziel

Der Backend-Vertrag für den neuen Create-New-Project-Flow aus `18-create-new-projects.png` wird explizit abgesichert. Das Backend akzeptiert keine anonymen Projekte mehr, sondern validiert `CreateProjectRequestDto.name` als Pflichtfeld und gibt ein initialisiertes `ProjectDto` mit konsistenten Projektmetadaten zurück.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Request DTO prüfen | `CreateProjectRequestDto` enthält mindestens `name`; optionale Felder wie `description` oder `templateId` bleiben erlaubt. |
| Namensnormalisierung | Name trimmen; leere oder nur aus Leerzeichen bestehende Namen ablehnen. |
| Validierungsfehler | Fehlenden oder ungültigen Namen als strukturierten `ApiErrorDto` zurückgeben. |
| Project Service anpassen | Neues Projekt mit angegebenem Namen initialisieren. |
| Projektmetadaten | `ProjectMetadataDto.name`, `createdAt`, `updatedAt` und `formatVersion` konsistent setzen. |
| Recent Projects | Neu angelegte Projekte mit Namen in Summaries aufnehmen, sofern Recent Projects im Repository unterstützt werden. |
| Contract-Beispiele | Beispielpayloads in Tests oder API-Dokumentation mit `name` verwenden. |

### Erwartetes Ergebnis

Der Dashboard-Flow ist backendseitig eindeutig:

```text
POST /api/v1/projects
{ "name": "Library Model" }
-> ProjectDto mit project.id und project.name = "Library Model"
```

Leere Namen führen zu einem fachlich verständlichen API-Fehler und nicht zu einem Projekt `Untitled` oder zu stillschweigender Fallback-Benennung.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `api.dto.project.CreateProjectRequestDto` | Request-Vertrag für Projektanlage. |
| `api.controller.ProjectController` | Request validieren und Fehlerformat liefern. |
| `application.ProjectService` | Projekt mit Namen initialisieren. |
| `persistence.ProjectRepository` | Projekt speichern und für Recent Projects bereitstellen. |
| `api.error` oder `error` | `ApiErrorDto` für ungültige Eingaben. |

### Benötigte DTOs

- `CreateProjectRequestDto`
- `ProjectDto`
- `ProjectMetadataDto`
- `ProjectSummaryDto`
- `ApiErrorDto`

### Abhängigkeiten

- Schritt 4: JSON-Projektformat und DTO-Grundlagen.
- Schritt 5: Project Service.
- Schritt 6: Dashboard-nahe Backend-Funktionen.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-CREATE-PROJECT-001` | `POST /api/v1/projects` mit `name = "Library Model"` liefert `ProjectDto` mit gleichem Namen. |
| `BE-CREATE-PROJECT-002` | Name mit führenden/folgenden Leerzeichen wird getrimmt gespeichert. |
| `BE-CREATE-PROJECT-003` | Leerer Name wird mit strukturiertem API-Fehler abgelehnt. |
| `BE-CREATE-PROJECT-004` | Nur Whitespace als Name wird abgelehnt. |
| `BE-CREATE-PROJECT-005` | Neues Projekt enthält leeres UML-Modell, leeren Snapshot und initiale Layoutdaten. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Frontend und Backend verwenden unterschiedliche Namensregeln | Im MVP nur `trim().isBlank()` als harte Regel; weitere Regeln als offene Frage dokumentieren. |
| Bestehende Tests nutzen implizites `Untitled` | Tests auf explizite Namen umstellen. |
| Fehlerformat ist noch uneinheitlich | Vorhandenes `ApiErrorDto` nutzen und später in Schritt 15 vereinheitlichen. |

### Akzeptanzkriterien

- Backend erstellt Projekte nur mit nicht leerem Namen.
- `ProjectDto.project.name` entspricht dem validierten Request-Namen.
- Ungültige Namen erzeugen keinen Projektzustand.
- Frontend kann die Fehlermeldung im Create-New-Project-Dialog anzeigen.

### Relevante Analyse-Dateien

- `assets/screenshots/README.md`
- `02-product-and-user-journey/03-functional-requirements.md`
- `06-frontend-analysis/12-modal-dialogs.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/02-api-flow.md`
- `07-integration-and-api/04-project-save-load-flow.md`
- `07-integration-and-api/07-dto-reference.md`

### MVP-Relevanz

Hoch. Ohne diesen Schritt kann der neue Dashboard-Create-Flow aus Screenshot `18-create-new-projects.png` nicht zuverlässig gegen das Backend umgesetzt werden.

## Schritt 6b: All Projects / Project List API

### Ziel

Das Backend stellt eine schlanke Projektlisten-Schnittstelle für die All-Projects-Seite aus `19-projects.png` bereit. Das Frontend kann darüber alle verfügbaren Projekte laden, anzeigen und daraus ein Projekt öffnen, ohne jeweils vollständige `ProjectDto`-Payloads laden zu müssen.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Project List Endpoint | `GET /api/v1/projects` bereitstellen oder vorhandenen Endpoint auf den dokumentierten Vertrag prüfen. |
| Project Summary DTO | `ProjectSummaryDto` mit `id`, `name`, optional `description` und `updatedAt` liefern. |
| Sortierung | Projekte zunächst nach `updatedAt` absteigend sortieren. |
| Suchparameter vorbereiten | Optional `search` als Query-Parameter akzeptieren; im MVP darf Suche auch clientseitig erfolgen. |
| Filter abgrenzen | `Filter` aus dem Screenshot als UI-Einstieg dokumentieren; serverseitige Filterlogik bleibt Post-MVP, falls nicht bereits einfach verfügbar. |
| Öffnen vorbereiten | Sicherstellen, dass jede Summary-ID mit `GET /api/v1/projects/{projectId}` ladbar ist. |
| Fehlerformat | Fehler bei Repository-/Formatproblemen als `ApiErrorDto` zurückgeben. |

### Erwartetes Ergebnis

Die All-Projects-Seite kann folgende API verwenden:

```text
GET /api/v1/projects
-> ProjectSummaryDto[]

GET /api/v1/projects/{projectId}
-> ProjectDto
```

Damit ist der Workflow aus `19-projects.png` backendseitig unterstützt:

```text
Dashboard
-> View all
-> All Projects
-> Projektkarte klicken
-> Projekt laden
-> Class Diagram
```

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `api.controller.ProjectController` | `GET /api/v1/projects` bereitstellen. |
| `api.dto.project.ProjectSummaryDto` | schlanke Karten-/Listenansicht. |
| `api.mapper.ProjectMapper` | Domain-Projekt zu Summary mappen. |
| `application.ProjectService` | Projektliste abrufen und sortieren. |
| `persistence.ProjectRepository` | alle gespeicherten Projekte oder Metadaten liefern. |

### Benötigte DTOs

- `ProjectSummaryDto`
- optional `ProjectListResponseDto`, falls später Pagination eingeführt wird
- `ApiErrorDto`

### Abhängigkeiten

- Schritt 4: DTO-Grundlagen.
- Schritt 5: Project Service.
- Schritt 6: Dashboard-nahe Backend-Funktionen.
- Schritt 6a: konsistente Projektnamen und Metadaten.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-PROJECT-LIST-001` | `GET /api/v1/projects` liefert eine Liste von `ProjectSummaryDto`. |
| `BE-PROJECT-LIST-002` | Neu erstellte Projekte erscheinen mit Name und ID in der Projektliste. |
| `BE-PROJECT-LIST-003` | Projektliste ist nach `updatedAt` absteigend sortiert. |
| `BE-PROJECT-LIST-004` | Jede gelieferte `id` kann über `GET /api/v1/projects/{projectId}` geladen werden. |
| `BE-PROJECT-LIST-005` | Optionaler Suchparameter `search` filtert nach Name/Beschreibung oder wird bewusst ignoriert und dokumentiert. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Vollständige Projekte werden unnötig für Karten geladen | Summary-Mapping nutzen und keine großen UML-/Snapshot-Payloads an `/projects` zurückgeben. |
| Filter/Pagination vergrößern Scope | Im MVP nur einfache Liste und optional Suche; komplexe Filter Post-MVP. |
| Demo- und echte Daten vermischen sich | API klar als Backend-Datenquelle behandeln; Demo-Fallback nur im Frontend kennzeichnen. |

### Akzeptanzkriterien

- `GET /api/v1/projects` liefert eine frontendtaugliche Projektliste.
- Summaries enthalten nutzerverständliche Namen statt nur technische IDs.
- `description` und `updatedAt` sind verfügbar oder als optional dokumentiert.
- Die Liste unterstützt den All-Projects-Flow aus `19-projects.png`.

### Relevante Analyse-Dateien

- `assets/screenshots/README.md`
- `02-product-and-user-journey/02-screenshot-based-user-journey.md`
- `02-product-and-user-journey/03-functional-requirements.md`
- `04-ui-ux-analysis/05-screenshot-traceability.md`
- `05-backend-analysis/13-api-design.md`
- `06-frontend-analysis/15-screenshot-implementation-mapping.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/02-api-flow.md`
- `07-integration-and-api/04-project-save-load-flow.md`

### MVP-Relevanz

Should. Der Endpoint ist nicht für den Kern-MVP `Start Project -> Class Diagram -> Object Diagram -> Check Constraints` zwingend, aber für die im Screenshot sichtbare Projektverwaltung notwendig.

## Schritt 7: UML Model Service

### Ziel

Der `UmlModelService` verwaltet Klassenmodell-Operationen und stellt Konsistenzregeln für UML-Elemente bereit.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Klassen verwalten | Erstellen, ändern, löschen. |
| Attribute verwalten | Primitive Typen, Namen, IDs, Zuordnung zu Klasse. |
| Operationen verwalten | Signaturen mit Parametern und Rückgabetyp. |
| Associations verwalten | Association mit zwei Enden im MVP. |
| Rollen erfassen | `roleName` je Association-Ende. |
| Multiplizitäten erfassen | `lower`, `upper`, `*`. |
| Invarianten zuordnen | Kontextklasse und OCL-Ausdruck speichern. |
| Strukturregeln | Doppelte Namen, fehlende Klassenreferenzen, ungültige Multiplicity prüfen. |

### Erwartetes Ergebnis

Das Backend kann das UML-Klassenmodell des MVP fachlich verwalten und für OCL-Typechecking sowie Snapshot-Validierung bereitstellen.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `application.UmlModelService` | Klassenmodelloperationen |
| `domain.uml` | UML-Domainobjekte |
| `validation.UmlStructureValidator` | Strukturprüfung |

### Benötigte DTOs

- `UmlModelDto`
- `UmlClassDto`
- `UmlAttributeDto`
- `UmlOperationDto`
- `UmlParameterDto`
- `UmlAssociationDto`
- `UmlAssociationEndDto`
- `MultiplicityDto`
- `UmlInvariantDto`
- `ValidationErrorDto`

### Abhängigkeiten

- Schritte 3 bis 5.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-UML-001` | Klasse `User` kann erstellt werden. |
| `BE-UML-002` | Attribut `books : Integer` kann hinzugefügt werden. |
| `BE-UML-003` | Operation als Signatur kann hinzugefügt werden. |
| `BE-UML-004` | Association `Borrows` mit Rollen und Multiplizitäten kann erstellt werden. |
| `BE-UML-005` | Invariante `maxBooks` wird Kontextklasse `User` zugeordnet. |
| `BE-UML-006` | Association mit unbekannter Klasse erzeugt Fehler. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| UML-Service übernimmt OCL-Aufgaben | OCL-Ausdrücke speichern, aber nicht dort parsen/evaluieren. |
| Löschen erzeugt verwaiste Referenzen | Referenzprüfung oder gezielte Fehlerausgabe implementieren. |

### Akzeptanzkriterien

- UML-MVP-Elemente sind per Service verwaltbar.
- Service verhindert oder meldet ungültige Referenzen.
- OCL Typechecker kann Klassen, Attribute und Rollen aus dem UML-Modell lesen.

### Relevante Analyse-Dateien

- `03-uml-ocl-domain/01-uml-ocl-scope.md`
- `03-uml-ocl-domain/02-domain-model.md`
- `05-backend-analysis/06-uml-model-service.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 8: Object Model / Snapshot Service

### Ziel

Der `ObjectModelService` verwaltet Objektinstanzen, Slots, Objektlinks und den validierbaren Snapshot-Zustand.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Objekt erstellen | Objektname, Klasse und ID verwalten. |
| Objekt löschen | Referenzierte Links und Slots behandeln. |
| Slots verwalten | Attributwerte setzen und typbezogen speichern. |
| Objektlinks erstellen | Link über Association und Association-Enden anlegen. |
| Objektlinks löschen | Snapshot konsistent aktualisieren. |
| Snapshot bereitstellen | Für Validation Service und OCL Evaluator zugänglich machen. |
| Grundvalidierung | Unbekannte Klasse, unbekanntes Attribut, ungültiger Slot-Wert, ungültiger Link erkennen. |

### Erwartetes Ergebnis

Das Backend kann einen Snapshot erzeugen und als Grundlage für Multiplicity Checks und OCL-Evaluation bereitstellen.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `application.ObjectModelService` | Snapshot-Operationen |
| `domain.snapshot` | Objekte, Slots, Links |
| `validation.SnapshotValidator` | Snapshot-Regeln |

### Benötigte DTOs

- `ObjectModelDto`
- `ObjectInstanceDto`
- `SlotDto`
- `ObjectLinkDto`
- `UmlAssociationDto`
- `ValidationErrorDto`

### Abhängigkeiten

- Schritt 7.
- UML-Modell muss Klassen, Attribute und Associations bereitstellen.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-OBJ-001` | Objekt `alice : User` kann erstellt werden. |
| `BE-OBJ-002` | Slot `books = 6` kann gesetzt werden. |
| `BE-OBJ-003` | Slot-Wert mit falschem Typ erzeugt `INVALID_SLOT_VALUE`. |
| `BE-OBJ-004` | Objektlink `Borrows(alice, mobyDick)` kann erstellt werden. |
| `BE-OBJ-005` | Link mit Objekt falscher Klasse erzeugt `INVALID_LINK`. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Snapshot wird mit UML-Modell vermischt | Domain-Packages und Services getrennt halten. |
| Null-/Unset-Semantik bleibt unklar | MVP-Regel dokumentieren und testen. |

### Akzeptanzkriterien

- Snapshot kann Objekte, Slots und Links ausdrücken.
- Ungültige Objektzustände werden strukturiert erkannt.
- OCL Evaluator kann auf Slotwerte und Objektlinks zugreifen.

### Relevante Analyse-Dateien

- `03-uml-ocl-domain/04-validation-concept.md`
- `05-backend-analysis/07-object-model-service.md`
- `07-integration-and-api/03-validation-flow.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 9: OCL Lexer und Parser-Grundlage

### Ziel

Die OCL-Verarbeitung wird als echte Pipeline begonnen: OCL-Text wird tokenisiert und nach MVP-Grammatik geparst. Es wird keine Regex-Auswertung geplant.

Zusätzlich wird die Grenze zur Modelltext-Verarbeitung festgelegt: Der OCL-Parser verarbeitet nur OCL-Ausdrücke. Vollständiger USE-ähnlicher Editor-Text wird durch eine separate `modeltext`-Komponente vorverarbeitet, die Klassen, Associations und Invarianten extrahiert und OCL-Fragmente an den OCL-Parser übergibt.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Tokenmodell | Identifier, `self`, Literale, Operatoren, Punkt, Pfeil, Klammern, Keywords. |
| Source Position | Tokens tragen Zeile, Spalte und optional Offsets. |
| Lexer | OCL-Text in Token Stream überführen. |
| Parser-Grundlage | Recursive Descent oder geeignete Parserstruktur für MVP-Subset. |
| ModelTextParser-Grundlage | Separaten Parser/Scanner für `model`, `class`, `attributes`, `association`, `constraints` als MVP-Subset anlegen. |
| Operatorpräzedenz | `not`, Vergleich, `and`, `or`, Klammern berücksichtigen. |
| Fehlerdiagnosen | Syntaxfehler mit Location und Message erzeugen. |

### Erwartetes Ergebnis

MVP-OCL-Ausdrücke können lexikalisch und syntaktisch verarbeitet werden. Syntaxfehler sind strukturiert meldbar. Vollständiger Editor-Text kann für das unterstützte Modelltext-Subset vorverarbeitet werden, ohne vollständige USE-Kompatibilität zu behaupten.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `ocl.lexer` | Tokenisierung |
| `ocl.parser` | Parser |
| `ocl.diagnostics` | Syntaxdiagnosen |
| `modeltext.parser` | Modelltext-Subset und Extraktion von Invarianten |
| `modeltext.diagnostics` | Unsupported Syntax und Source Ranges im vollständigen Text |

### Benötigte DTOs

- `OclParseRequestDto`
- `OclParseResponseDto`
- `OclDiagnosticDto`
- `SourceRangeDto`
- `ApplyModelTextRequestDto`
- `ApplyModelTextResponseDto`

### Abhängigkeiten

- Schritt 2.
- OCL-MVP-Subset ist bestätigt.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-OCL-LEX-001` | `self.books <= 5` erzeugt erwartete Tokens. |
| `BE-OCL-LEX-002` | String-Literal `''` wird erkannt. |
| `BE-OCL-LEX-003` | `->size()` wird als Pfeil plus Identifier plus Klammern erkannt. |
| `BE-OCL-PARSE-001` | `self.books <= 5` ist syntaktisch gültig. |
| `BE-OCL-PARSE-002` | `self.books <=` erzeugt `SYNTAX_ERROR` mit Location. |
| `BE-MODEL-TEXT-001` | Vollständiger Library-Modelltext extrahiert Klassen, Association und Invariante. |
| `BE-MODEL-TEXT-002` | `import` im Modelltext erzeugt `UNSUPPORTED_SYNTAX` mit Location. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Parser wächst unkontrolliert | MVP-Grammatik strikt begrenzen. |
| Fehlerpositionen fehlen | Source Positions von Anfang an in Tokens führen. |
| OCL- und Modelltextparser werden vermischt | Separate Packages und Tests erzwingen. |

### Akzeptanzkriterien

- MVP-Ausdrücke können geparst werden.
- Syntaxfehler liefern `SYNTAX_ERROR`, Message und Location.
- Parser hängt noch nicht vom Snapshot ab.
- Modelltext-Parser unterstützt nur das dokumentierte MVP-Subset und meldet unsupported Konstrukte.

### Relevante Analyse-Dateien

- `03-uml-ocl-domain/03-ocl-architecture-and-extension-strategy.md`
- `05-backend-analysis/08-ocl-engine-design.md`
- `05-backend-analysis/09-ocl-parser-and-ast.md`
- `07-integration-and-api/05-ocl-evaluation-flow.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 10: OCL AST-Struktur

### Ziel

Der Parser erzeugt einen expliziten AST, der später typgeprüft und ausgewertet werden kann.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| AST-Basis | `OclAstNode` mit Source Range. |
| Self Expression | `SelfExpression`. |
| Zugriff | `AttributeAccessExpression`, `AssociationNavigationExpression` oder allgemeiner `PropertyAccessExpression`. |
| Literale | `LiteralExpression` für String, Integer, Real, Boolean. |
| Operatoren | `BinaryExpression`, `UnaryExpression`. |
| Collections | `CollectionOperationExpression` für `size`, `isEmpty`, `notEmpty`. |
| Parentheses | Optional `ParenthesizedExpression` oder nur Strukturwirkung im Parser. |
| AST-Serialisierung optional | Für Debug/Parse Response vereinfachte AST-Ansicht. |

### Erwartetes Ergebnis

Der Parser liefert einen AST, der unabhängig vom ursprünglichen Text typgeprüft und ausgewertet werden kann.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `ocl.ast` | AST-Knoten |
| `ocl.parser` | Parser erzeugt AST |

### Benötigte DTOs

- `OclParseResponseDto`
- optional vereinfachtes `ast`-Feld als Debugstruktur.

### Abhängigkeiten

- Schritt 9.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-OCL-AST-001` | `self.books <= 5` erzeugt `BinaryExpression`. |
| `BE-OCL-AST-002` | `self.borrowedBooks->size() <= 5` erzeugt Collection Operation. |
| `BE-OCL-AST-003` | `not self.available` erzeugt `UnaryExpression`. |
| `BE-OCL-AST-004` | Klammern ändern AST-Struktur korrekt. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| AST ist zu spezifisch für MVP | Erweiterbare Knotenbasis und Visitor/Handler-Struktur vorsehen. |
| Navigation und Attributzugriff werden zu früh unterschieden | Im Parser syntaktisch allgemein bleiben, im Typechecker auflösen. |

### Akzeptanzkriterien

- Alle MVP-Ausdrücke haben AST-Repräsentationen.
- AST-Knoten tragen Source Ranges.
- Post-MVP-Knoten können ergänzt werden.

### Relevante Analyse-Dateien

- `05-backend-analysis/09-ocl-parser-and-ast.md`
- `05-backend-analysis/08-ocl-engine-design.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 11: OCL Typechecker

### Ziel

Der Typechecker prüft OCL-Ausdrücke gegen UML-Modell und Kontextklasse. Jede Invariante muss im MVP einen Boolean-Ausdruck ergeben.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Typmodell | Primitive Types, Class Types, Collection Types. |
| Type Environment | Kontextklasse, `self`, Attribute, Rollen. |
| Attributzugriff | Existenz und Typ von Attributen prüfen. |
| Association Navigation | Rollen gegen Associations auflösen, Collection-Typ bestimmen. |
| Operatorregeln | Vergleichs- und Boolean-Operatoren prüfen. |
| Collection-Regeln | `size`, `isEmpty`, `notEmpty` nur auf Collections erlauben. |
| Invariant-Regel | Gesamtausdruck muss Boolean ergeben. |
| Type Diagnostics | `TYPE_ERROR`, `UNKNOWN_ATTRIBUTE`, `UNKNOWN_CLASS` mit Targets und Location. |

### Erwartetes Ergebnis

OCL-Ausdrücke werden semantisch geprüft. Ungültige Ausdrücke werden nicht evaluiert, sondern als strukturierte Diagnosen zurückgegeben.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `ocl.typecheck` | Typechecker, Type Environment, Typregeln |
| `ocl.ast` | Eingabe |
| `domain.uml` | UML-Typinformationen |
| `ocl.diagnostics` | Typecheck-Fehler |

### Benötigte DTOs

- `OclTypecheckRequestDto`
- `OclTypecheckResponseDto`
- `OclDiagnosticDto`
- `ElementTargetDto`

### Abhängigkeiten

- Schritt 10.
- Schritt 7.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-OCL-TYPE-001` | `self.books <= 5` ist gültig, wenn `books : Integer`. |
| `BE-OCL-TYPE-002` | `self.name <= 5` erzeugt `TYPE_ERROR`. |
| `BE-OCL-TYPE-003` | `self.unknown` erzeugt `UNKNOWN_ATTRIBUTE`. |
| `BE-OCL-TYPE-004` | `self.borrowedBooks->size() <= 5` ist gültig bei Collection-Navigation. |
| `BE-OCL-TYPE-005` | Invariante mit Ergebnis `Integer` ist ungültig. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| UML-Rollen und Attribute kollidieren | Auflösungsregeln dokumentieren und testen. |
| Collection-Typen werden unscharf | `CollectionType(elementType)` explizit modellieren. |

### Akzeptanzkriterien

- Typechecker erkennt unbekannte Attribute und falsche Operatoren.
- Gültige MVP-Invarianten ergeben Typ Boolean.
- Typecheck-Ergebnis ist unabhängig von konkreten Objektwerten.

### Relevante Analyse-Dateien

- `05-backend-analysis/10-ocl-typechecker.md`
- `03-uml-ocl-domain/03-ocl-architecture-and-extension-strategy.md`
- `07-integration-and-api/05-ocl-evaluation-flow.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 12: OCL Evaluator

### Ziel

Der Evaluator wertet getypte OCL-Ausdrücke gegen einen konkreten Snapshot und ein Kontextobjekt aus.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Evaluation Context | `self`, Kontextobjekt, UML-Modell, Snapshot. |
| OCL Values | Boolean, Integer, Real, String, Object, Collection. |
| Attributzugriff | Slot-Wert des Kontextobjekts lesen. |
| Navigation | Objektlinks entlang Association-Rollen traversieren. |
| Operatoren | Vergleichs- und Boolean-Auswertung. |
| Collections | `size`, `isEmpty`, `notEmpty`. |
| Evaluation Errors | Fehlende Slots, inkonsistente Links oder unerwartete Werte melden. |
| Trace optional | Debug-Informationen für Fehlermeldungen vorbereiten. |

### Erwartetes Ergebnis

Für `alice : User` mit `books = 6` ergibt `self.books <= 5` den Wert `false`.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `ocl.evaluation` | Evaluator, Evaluation Context |
| `ocl.value` | Typisierte OCL-Werte |
| `domain.snapshot` | Objekte, Slots, Links |
| `domain.uml` | Association- und Attributinformationen |

### Benötigte DTOs

- `OclEvaluateRequestDto`
- `OclEvaluateResponseDto`
- `OclDiagnosticDto`

### Abhängigkeiten

- Schritt 11.
- Schritt 8.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-OCL-EVAL-001` | `self.books <= 5` ergibt `false` bei `books = 6`. |
| `BE-OCL-EVAL-002` | `self.name <> ''` ergibt `true` bei `name = 'Alice'`. |
| `BE-OCL-EVAL-003` | `self.available = false` ergibt erwarteten Boolean. |
| `BE-OCL-EVAL-004` | `self.borrowedBooks->size() <= 5` nutzt Objektlinks. |
| `BE-OCL-EVAL-005` | Fehlender Slot erzeugt `EVALUATION_ERROR`. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Navigation wird falsch interpretiert | Association-Enden und Rollen konsequent über IDs auflösen. |
| Null/Unset-Werte sind unklar | MVP-Regel für fehlende Werte festlegen. |
| Evaluator übernimmt Typechecker-Aufgaben | Evaluation nur nach erfolgreichem Typecheck ausführen. |

### Akzeptanzkriterien

- Evaluator kann MVP-Ausdrücke auswerten.
- Evaluation nutzt Snapshot, nicht nur UML-Modell.
- Fehler werden strukturiert als `EVALUATION_ERROR` zurückgegeben.

### Relevante Analyse-Dateien

- `05-backend-analysis/11-ocl-evaluator.md`
- `05-backend-analysis/08-ocl-engine-design.md`
- `03-uml-ocl-domain/04-validation-concept.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 13: Validation Service

### Ziel

Der `ValidationService` koordiniert alle Validierungsebenen für `Check Constraints`.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Projektstruktur prüfen | Pflichtbereiche, IDs, Referenzen. |
| UML-Modell prüfen | Klassen, Attribute, Associations, Rollen, Multiplicities. |
| Snapshot prüfen | Objekte, Slots, Links. |
| Linkvalidierung | Object Links gegen Associations. |
| Multiplicity Checks | Linkanzahlen je Association-Ende prüfen. |
| OCL-Pipeline integrieren | Parse, Typecheck, Evaluation je Invariante. |
| Ergebnis aggregieren | `ValidationResult` mit Summary und Fehlerliste erzeugen. |
| UI-Mapping | Fehler auf Objekte, Links, Invarianten, Klassen, Slots referenzieren. |

### Erwartetes Ergebnis

Ein vollständiger Constraint Check liefert `VALID`, `INVALID` oder `ERROR` mit strukturierten Fehlern.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `validation.ValidationService` | Orchestrierung |
| `validation.UmlStructureValidator` | UML-Regeln |
| `validation.SnapshotValidator` | Snapshot-Regeln |
| `validation.MultiplicityValidator` | Multiplicity Checks |
| `validation.OclInvariantValidator` | OCL-Invarianten |
| `domain.validation` | Ergebnisobjekte |

### Benötigte DTOs

- `ValidationRequestDto`
- `ValidationResultDto`
- `ValidationSummaryDto`
- `ValidationErrorDto`
- `ElementTargetDto`

### Abhängigkeiten

- Schritte 7 bis 12.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-VAL-001` | Gültiges Library-Snapshot liefert `VALID`. |
| `BE-VAL-002` | `alice.books = 6` bei `self.books <= 5` liefert `INVARIANT_VIOLATION`. |
| `BE-VAL-003` | Falscher Slottyp liefert `INVALID_SLOT_VALUE`. |
| `BE-VAL-004` | Ungültiger Link liefert `INVALID_LINK`. |
| `BE-VAL-005` | Verletzte Multiplizität liefert `MULTIPLICITY_VIOLATION`. |
| `BE-VAL-006` | OCL-Syntaxfehler verhindert nur diese Invariante, nicht alle Prüfungen. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Eine fehlerhafte Invariante blockiert alle Validierungen | Fehler isolieren und Ergebnis aggregieren. |
| Frontend kann Fehler nicht zuordnen | Targets und IDs verpflichtend testen. |
| Validation Service wird zu monolithisch | Subvalidatoren trennen. |

### Akzeptanzkriterien

- `Check Constraints` ist fachlich vollständig für MVP.
- `ValidationResultDto` enthält Summary und Fehlerliste.
- `INVARIANT_VIOLATION` enthält `contextObjectId`, `contextClassId`, `invariantId`.
- Multiplicity und OCL-Fehler sind unterscheidbar.

### Relevante Analyse-Dateien

- `03-uml-ocl-domain/04-validation-concept.md`
- `05-backend-analysis/12-validation-service.md`
- `07-integration-and-api/03-validation-flow.md`
- `07-integration-and-api/08-error-contract.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 14: REST API

### Ziel

Das Backend stellt die dokumentierten REST/JSON-Endpunkte für Frontend und Integration bereit.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Project Controller | Create, Load, Save, Export, Import. |
| Dashboard Controller | Recent Projects, optional Examples. |
| Model Text Controller | `GET /model-text` und `POST /model-text/apply` für den vollständigen OCL-Editor-Text. |
| UML Controller | Klassen, Attribute, Operationen, Associations, Invarianten. |
| Object Controller | Objekte, Slots, Links. |
| OCL Controller | Parse, Typecheck, optional Evaluate. |
| Validation Controller | `POST /api/v1/projects/{projectId}/validate`. |
| DTO Mapping | Controller nutzen DTOs, Services nutzen Domain. |
| API-Versionierung | `/api/v1`. |
| HTTP-Statusregeln | API Error vs fachliches Validation Result trennen. |

### Erwartetes Ergebnis

Das React-Frontend kann alle MVP-Backendfunktionen über REST/JSON nutzen.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `api.controller` | REST Controller |
| `api.dto` | Request/Response DTOs |
| `api.mapper` | DTO/Domain Mapper |
| `application.*Service` | Fachliche Services |
| `error` | Exception Handling |

### Benötigte DTOs

- alle DTOs aus `07-dto-reference.md`, besonders:
  - `ProjectDto`
  - `ProjectSummaryDto`
  - `ModelTextDto`
  - `ApplyModelTextRequestDto`
  - `ApplyModelTextResponseDto`
  - UML-DTOs
  - Objektmodell-DTOs
  - OCL-Request/Response-DTOs
  - `ValidationRequestDto`
  - `ValidationResultDto`
  - `ApiErrorDto`

### Abhängigkeiten

- Schritte 5 bis 13.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-API-001` | `POST /api/v1/projects` erstellt Projekt. |
| `BE-API-002` | `GET /api/v1/projects/{id}` lädt Projekt. |
| `BE-API-003` | `PUT /api/v1/projects/{id}` speichert Projekt. |
| `BE-API-004` | `POST /api/v1/projects/{id}/validate` liefert Validation Result. |
| `BE-API-005` | OCL Parse Endpoint liefert Syntaxdiagnose. |
| `BE-API-006` | Nicht existierendes Projekt liefert `ApiErrorDto`. |
| `BE-API-007` | `POST /api/v1/projects/{id}/model-text/apply` liefert aktualisiertes Projekt und Diagnostics. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Zu viele feingranulare Endpunkte im MVP | Vollprojekt-Save plus Kernmutationen priorisieren. |
| API ändert sich während Frontend-Integration | DTO-Referenz als Contract pflegen. |

### Akzeptanzkriterien

- MVP-Endpunkte sind lauffähig.
- JSON Requests/Responses entsprechen DTO-Referenz.
- `model-text/apply` akzeptiert vollständigen Editor-Text und meldet unsupported Syntax strukturiert.
- Fachlich ungültige Validierung liefert HTTP 200 mit `ValidationResultDto`.
- Technische/API-Fehler liefern `ApiErrorDto`.

### Relevante Analyse-Dateien

- `05-backend-analysis/13-api-design.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/02-api-flow.md`
- `07-integration-and-api/07-dto-reference.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 14a: Delete-Operationen und Cascade-Regeln

### Ziel

Das Backend stellt fachlich konsistente Löschoperationen für Modell- und Snapshot-Elemente bereit. Löschen ist keine reine UI-Aktion, sondern muss abhängige Elemente, Layoutdaten, Validation Results und stabile Referenzen kontrolliert behandeln.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Klassen löschen | `DELETE /api/v1/projects/{projectId}/classes/{classId}` entfernt die Klasse und im MVP kaskadierend ihre Attribute, Operationen, Associations, Invarianten, Objekte, Slots und betroffenen Links. |
| Attribute löschen | `DELETE /api/v1/projects/{projectId}/classes/{classId}/attributes/{attributeId}` entfernt das Attribut und alle Slots dieses Attributs in passenden Objekten. |
| Operationen löschen | `DELETE /api/v1/projects/{projectId}/classes/{classId}/operations/{operationId}` entfernt die Operation-Signatur. |
| Associations löschen | `DELETE /api/v1/projects/{projectId}/associations/{associationId}` entfernt die Association und alle Object Links dieser Association. |
| Objekte löschen | `DELETE /api/v1/projects/{projectId}/objects/{objectId}` entfernt das Objekt, seine Slots und alle Links, die dieses Objekt referenzieren. |
| Objektlinks löschen | `DELETE /api/v1/projects/{projectId}/links/{linkId}` entfernt den konkreten Snapshot-Link. |
| Invarianten löschen | `DELETE /api/v1/projects/{projectId}/invariants/{invariantId}` entfernt die Invariante und bereinigt zugehörige Validation Results. |
| Layout bereinigen | Node-/Edge-Layoutdaten für gelöschte Elemente entfernen. |
| Validation Results bereinigen | Validation Errors, die gelöschte IDs referenzieren, verwerfen oder als stale behandeln. |
| Konfliktmodell festlegen | MVP nutzt Cascade Delete; Post-MVP kann optional blockierendes Delete mit Auswirkungsübersicht ergänzen. |

### Erwartetes Ergebnis

Frontend und API können Elemente löschen, ohne dass verwaiste Referenzen im Projektzustand bleiben. Das zurückgegebene Projekt ist nach jeder Delete-Operation strukturell konsistent und kann anschließend erneut validiert werden.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `application.uml.UmlModelService` | Löschen von Klassen, Attributen, Operationen, Associations und Invarianten. |
| `application.snapshot.ObjectModelService` | Löschen von Objekten, Slots und Objektlinks. |
| `application.project.ProjectService` | Projekt laden, Mutation ausführen, Projekt speichern. |
| `api.controller` | Delete-Endpunkte bereitstellen. |
| `api.mapper` | Aktualisiertes `ProjectDto` oder Statusantwort mappen. |
| `domain.layout` | Layoutreferenzen gelöschter Nodes/Edges entfernen. |
| `domain.validation` | Veraltete Validation Targets bereinigen. |
| `error` | `NOT_FOUND`, `CONFLICT` und ungültige Referenzen strukturiert melden. |

### Benötigte DTOs

- `ProjectDto`
- `ApiErrorDto`
- `ValidationResultDto`
- optional `DeleteImpactDto` als Post-MVP-Erweiterung

### Abhängigkeiten

- Schritt 7: UML Model Service.
- Schritt 8: Object Model / Snapshot Service.
- Schritt 14: REST API.
- Schritt 15: Error- und Result-Modell für finale Fehlervereinheitlichung.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-DELETE-001` | Klasse `User` löschen entfernt abhängige Invarianten und Objekte dieser Klasse. |
| `BE-DELETE-002` | Attribut `books` löschen entfernt Slots `books` aus `User`-Objekten. |
| `BE-DELETE-003` | Association `Borrows` löschen entfernt zugehörige Object Links. |
| `BE-DELETE-004` | Objekt `alice` löschen entfernt Links mit `alice` als Endpunkt. |
| `BE-DELETE-005` | Nicht existierende ID liefert strukturierten API-Fehler. |
| `BE-DELETE-006` | Layoutdaten gelöschter Nodes/Edges werden nicht mehr zurückgegeben. |
| `BE-DELETE-007` | Nach Delete enthält `ValidationResultDto` keine Targets auf gelöschte Elemente. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Cascade Delete entfernt mehr als erwartet | Frontend-Confirm-Dialog und optional später `DeleteImpactDto`. |
| Verwaiste Referenzen bleiben im Snapshot | Referenzbereinigung serviceübergreifend testen. |
| OCL-Invarianten referenzieren gelöschte Attribute | Im MVP abhängige Invarianten bei Klassenlöschung entfernen; bei Attributlöschung spätere Validierung mit `UNKNOWN_ATTRIBUTE` erlauben oder gezielt warnen. |
| Frontend-State und Backend-State laufen auseinander | Delete-Response als aktualisiertes `ProjectDto` bevorzugen. |

### Akzeptanzkriterien

- Alle MVP-Modell- und Snapshot-Elemente können per REST gelöscht werden.
- Das Backend erzeugt nach Delete keine verwaisten Links, Slots oder Layoutreferenzen.
- Delete-Endpunkte liefern entweder aktualisiertes `ProjectDto` oder eindeutig dokumentiertes `204 No Content`.
- Fehlerfälle nutzen `ApiErrorDto`.
- Das alte USE-Projekt wird nicht als Dependency verwendet.

### Relevante Analyse-Dateien

- `05-backend-analysis/06-uml-model-service.md`
- `05-backend-analysis/07-object-model-service.md`
- `05-backend-analysis/13-api-design.md`
- `05-backend-analysis/15-error-and-result-model.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/02-api-flow.md`
- `07-integration-and-api/07-dto-reference.md`
- `07-integration-and-api/08-error-contract.md`

### MVP-Relevanz

Hoch. Ohne Delete-Operationen können Nutzer Fehlmodellierungen nicht sauber korrigieren; die Diagramm-UI wäre im MVP nur eingeschränkt nutzbar.

## Schritt 15: Error- und Result-Modell

### Ziel

API-Fehler, OCL-Diagnostics und Validation Results werden vereinheitlicht, sodass das Frontend Fehler zuverlässig darstellen und fokussieren kann.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Error Codes definieren | `API_ERROR`, `SYNTAX_ERROR`, `TYPE_ERROR`, `INVALID_SLOT_VALUE`, usw. |
| Severity implementieren | `ERROR`, `WARNING`, `INFO`. |
| API Exception Handling | Zentraler Spring Exception Handler für `ApiErrorDto`. |
| Validation Error Builder | Einheitliche Erstellung von `ValidationErrorDto`. |
| OCL Diagnostics Mapping | Parser-/Typechecker-/Evaluator-Fehler auf Error Contract mappen. |
| Element Targets | `elementType`, `elementId`, `path`, OCL Location. |
| User Messages | Lesbare Fehlermeldungen für UI bereitstellen. |

### Erwartetes Ergebnis

Alle Fehler folgen einem dokumentierten Format. Das Frontend muss keine Freitexte parsen.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `error` | API Error Handling |
| `domain.validation` | Validation Errors |
| `ocl.diagnostics` | OCL-Fehler |
| `validation` | Error-Erzeugung |

### Benötigte DTOs

- `ApiErrorDto`
- `ValidationErrorDto`
- `ValidationResultDto`
- `ElementTargetDto`
- `OclDiagnosticDto`
- `SourceRangeDto`

### Abhängigkeiten

- Schritte 9 bis 14a.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-ERR-001` | `PROJECT_NOT_FOUND` liefert `ApiErrorDto`. |
| `BE-ERR-002` | `SYNTAX_ERROR` enthält OCL Location. |
| `BE-ERR-003` | `TYPE_ERROR` enthält Invariant Target. |
| `BE-ERR-004` | `INVARIANT_VIOLATION` enthält Object und Invariant Target. |
| `BE-ERR-005` | `MULTIPLICITY_VIOLATION` enthält Association und Object Target. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Fehlertexte statt Codes werden verwendet | Codes in Tests prüfen. |
| UI-Mapping fehlt | Mindestens ein Target pro Validation Error erzwingen. |
| Technische Details leaken | `technicalMessage` bewusst begrenzen. |

### Akzeptanzkriterien

- Fehlercodes sind konsistent.
- Validation Errors enthalten UI-Ziele.
- OCL Errors enthalten Source Range, wenn möglich.
- API Error und Validation Error sind klar getrennt.

### Relevante Analyse-Dateien

- `05-backend-analysis/15-error-and-result-model.md`
- `07-integration-and-api/08-error-contract.md`
- `07-integration-and-api/07-dto-reference.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 16: Backend-Tests und Library-Szenario

### Ziel

Die Backend-Fachlogik wird durch Unit-, Service-, API- und End-to-End-Tests abgesichert. Das Library-Szenario dient als zentraler MVP-Regressionstest.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Domain Tests | Klassen, Associations, Snapshots, IDs. |
| JSON Tests | Roundtrip, Formatversion, ungültige Projekte. |
| Service Tests | Project, UML Model, Object Model. |
| OCL Tests | Lexer, Parser, AST, Typechecker, Evaluator. |
| Model Text Tests | Vollständigen USE-ähnlichen Editor-Text anwenden und unsupported Syntax diagnostizieren. |
| Validation Tests | Multiplicity, Invariants, Snapshot Errors. |
| API Tests | Controller Requests/Responses. |
| Library E2E | User/Book/Borrows/maxBooks/alice/mobyDick. |
| Original-USE-Referenz | Beispielmodelle als Testfallquelle prüfen, nicht Code übernehmen. |

### Erwartetes Ergebnis

Der Backend-MVP ist regressionssicher genug, um mit dem Frontend integriert zu werden.

### Betroffene Packages/Services

Alle Backend-Packages.

### Benötigte DTOs

Alle MVP-DTOs, besonders:

- `ProjectDto`
- `ValidationResultDto`
- `ValidationErrorDto`
- `OclParseResponseDto`
- `OclTypecheckResponseDto`
- `OclEvaluateResponseDto`

### Abhängigkeiten

- Schritte 1 bis 15.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-E2E-001` | Klasse `User` erstellen. |
| `BE-E2E-002` | Attribut `books : Integer` erstellen. |
| `BE-E2E-003` | Klasse `Book` erstellen. |
| `BE-E2E-004` | Association `Borrows` erstellen. |
| `BE-E2E-005` | Invariante `self.books <= 5` speichern. |
| `BE-E2E-006` | Objekt `alice : User` mit `books = 6` erstellen. |
| `BE-E2E-007` | Objekt `mobyDick : Book` erstellen. |
| `BE-E2E-008` | Link `Borrows(alice, mobyDick)` erstellen. |
| `BE-E2E-009` | `Check Constraints` liefert `INVARIANT_VIOLATION`. |
| `BE-E2E-010` | Validation Result referenziert `objectId` von `alice`. |
| `BE-E2E-011` | Vollständiger Modelltext erzeugt dieselben Library-Bestandteile wie der strukturierte API-Workflow. |
| `BE-E2E-012` | Modelltext mit `import` oder `associationclass` liefert `UNSUPPORTED_SYNTAX`, ohne USE-Core zu verwenden. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Tests decken nur Happy Path ab | Fehlerfälle je Schicht definieren. |
| OCL-Tests sind zu grob | Lexer, Parser, Typechecker und Evaluator separat testen. |
| Original-USE-Beispiele werden als Kompatibilitätsversprechen missverstanden | Nur als Referenz- und Testfallquelle kennzeichnen. |

### Akzeptanzkriterien

- Library-E2E-Test läuft stabil.
- OCL-Pipeline ist schichtweise getestet.
- Modelltext-Apply-Flow ist als MVP-Subset getestet.
- Validation Errors enthalten erwartete Codes und Targets.
- API-Tests prüfen zentrale Endpunkte.

### Relevante Analyse-Dateien

- `05-backend-analysis/17-backend-test-strategy.md`
- `01-original-use-reference/03-use-syntax-and-examples.md`
- `01-original-use-reference/05-reference-relevance-for-new-system.md`
- `07-integration-and-api/03-validation-flow.md`

### MVP-Relevanz

Sehr hoch.

## Schritt 16a: Modelltext-Apply-Flow

### Ziel

Der Backend-MVP wird um den neuen OCL-Editor- und Open-Existing-Workflow erweitert: Das Frontend kann vollständigen USE-ähnlichen Modelltext aus der OCL Editor View oder aus einer lokalen `.use` Datei im `Open Existing Project` Dialog an das Backend senden. Das Backend speichert den Text als `modelText`, verarbeitet beim expliziten `Apply Changes` beziehungsweise `Open Project` nur das dokumentierte MVP-Subset und gibt strukturierte Diagnostics für nicht unterstützte Syntax zurück.

Dieser Schritt ist bewusst kein vollständiger `.use` Import. Er ist ein begrenzter Apply-Flow für den Editor-Screenshot `13-ocl-editor.png`, den Open-Existing-Screenshot `14-open-existing-project.png` und für reduzierte Modelltexte aus den originalen `.use`-Beispielen.

### Konkrete Aufgaben

| Aufgabe | Beschreibung |
|---|---|
| Projektmodell erweitern | `Project` beziehungsweise Projekt-DTO um optionalen `modelText` erweitern. |
| DTOs ergänzen | `ModelTextDto`, `ApplyModelTextRequestDto`, `ApplyModelTextResponseDto` und Diagnostics-Felder finalisieren. |
| Modelltext-Service anlegen | `ModelTextApplicationService` für Laden, Speichern und Anwenden des Editor-Texts erstellen. |
| ModelTextParser MVP | Parser/Scanner für `model`, `class`, `attributes`, optional `operations`, binäre `association`, Rollen/Multiplizitäten und `constraints` bauen. |
| OCL-Extraktion | `context ... inv ...`-Blöcke aus dem Modelltext extrahieren und OCL-Ausdrücke an die bestehende OCL-Pipeline übergeben. |
| Domain Mapping | Erkannte Klassen, Attribute, Associations und Invarianten in das bestehende Domain-Modell mappen. |
| ID-Regeln | Aus Namen stabile oder deterministische IDs erzeugen, ohne bestehende IDs unnötig zu ändern. |
| Unsupported Syntax | `import`, `associationclass`, komplexe Vererbung und weitere Nicht-MVP-Konstrukte als `UNSUPPORTED_SYNTAX` diagnostizieren. |
| REST-Endpunkte | `GET /api/v1/projects/{projectId}/model-text` und `POST /api/v1/projects/{projectId}/model-text/apply` bereitstellen. |
| Open-Existing-Quelle | Request-Felder wie `sourceName`, `sourceFormat = "use"` und optional `sourceOrigin = "open-existing"` unterstützen, damit Diagnostics auf die hochgeladene Datei bezogen werden können. |
| Persistenz | `modelText` im JSON-Projektformat speichern und beim Laden wiederherstellen. |
| Tests | Unit-, Service- und API-Tests für Apply, Roundtrip und Diagnostics ergänzen. |

### Erwartetes Ergebnis

Der Backend-Stand unterstützt den vollständigen textbasierten Editor-Workflow:

```text
OCL Editor
-> Apply Changes
-> POST /api/v1/projects/{projectId}/model-text/apply
-> ModelTextApplicationService
-> ModelTextParser
-> Domain Mapping
-> ProjectDto + Diagnostics
```

Ein reduzierter Library-Modelltext erzeugt nach `Apply Changes` dieselben fachlichen Projektbestandteile wie der strukturierte API-Workflow: Klassen `User` und `Book`, Association `Borrows`, Invariante `maxBooks`.

### Betroffene Packages/Services

| Package/Service | Zweck |
|---|---|
| `domain.project` | `Project` enthält optionalen `modelText`. |
| `api.dto.project` oder `api.dto.modeltext` | `ModelTextDto`, Apply Request/Response. |
| `application.modeltext` | Orchestriert Laden, Speichern und Anwenden des Modelltexts. |
| `modeltext.parser` | Erkennt das USE-ähnliche MVP-Subset. |
| `modeltext.mapping` | Mappt Modelltext-Struktur auf Domain-Objekte. |
| `modeltext.diagnostics` | Diagnostics inklusive `UNSUPPORTED_SYNTAX` und Source Range. |
| `ocl.parser`, `ocl.typecheck` | Verarbeiten extrahierte OCL-Ausdrücke. |
| `api.controller` | REST-Endpunkte für Modelltext. |
| `persistence.json` | Speichert und lädt `modelText`. |
| `error` | Einheitliche Diagnose- und Fehlercodes. |

### Benötigte DTOs

- `ModelTextDto`
- `ApplyModelTextRequestDto`
- `ApplyModelTextResponseDto`
- `OclDiagnosticDto`
- `SourceRangeDto`
- `ProjectDto`
- `ApiErrorDto`

Beispiel Request:

```json
{
  "modelText": "model Library\n\nclass User\nattributes\n  books : Integer\nend\n\nclass Book\nattributes\n  title : String\nend\n\nassociation Borrows between\n  User[0..1] role borrower\n  Book[0..*] role borrowedBooks\nend\n\nconstraints\ncontext User inv maxBooks:\n  self.books <= 5\n",
  "format": "USE_TEXT",
  "mode": "REPLACE_SUPPORTED_MODEL"
}
```

Beispiel Response:

```json
{
  "success": true,
  "status": "APPLIED",
  "project": {
    "id": "project-library",
    "name": "Library"
  },
  "diagnostics": []
}
```

Beispiel für nicht unterstützte Syntax:

```json
{
  "success": false,
  "status": "NOT_APPLIED",
  "diagnostics": [
    {
      "code": "UNSUPPORTED_SYNTAX",
      "severity": "WARNING",
      "message": "Import statements are not supported by the MVP model text apply flow.",
      "sourceRange": {
        "startLine": 1,
        "startColumn": 1,
        "endLine": 1,
        "endColumn": 29
      }
    }
  ]
}
```

### Abhängigkeiten

- Schritt 3: Domänenmodell.
- Schritt 4: DTOs und JSON-Projektformat.
- Schritt 5: Project Service.
- Schritt 7: UML Model Service.
- Schritt 9 bis 11: OCL Lexer, Parser und Typechecker für extrahierte Invarianten.
- Schritt 14: REST API.
- Schritt 15: Error- und Result-Modell.

### Testfälle

| Testfall-ID | Beschreibung |
|---|---|
| `BE-MODEL-TEXT-APPLY-001` | `modelText` wird im Projekt gespeichert und beim Laden wiederhergestellt. |
| `BE-MODEL-TEXT-APPLY-002` | Reduzierter Library-Modelltext erzeugt Klassen `User` und `Book`. |
| `BE-MODEL-TEXT-APPLY-003` | Association `Borrows` mit Rollen und Multiplizitäten wird erkannt. |
| `BE-MODEL-TEXT-APPLY-004` | `context User inv maxBooks: self.books <= 5` erzeugt eine `UmlInvariant`. |
| `BE-MODEL-TEXT-APPLY-005` | Extrahierter OCL-Ausdruck wird über die bestehende OCL-Pipeline geparst. |
| `BE-MODEL-TEXT-APPLY-006` | `import Date from "Dates.use"` erzeugt `UNSUPPORTED_SYNTAX`. |
| `BE-MODEL-TEXT-APPLY-007` | `associationclass` erzeugt `UNSUPPORTED_SYNTAX` oder klaren Post-MVP-Hinweis. |
| `BE-MODEL-TEXT-APPLY-008` | API-Endpunkt `POST /model-text/apply` liefert aktualisiertes `ProjectDto` und Diagnostics. |
| `BE-MODEL-TEXT-APPLY-009` | Ungültiger Modelltext liefert strukturierte Source Ranges. |
| `BE-MODEL-TEXT-APPLY-010` | Kein alter USE-Core ist als Dependency beteiligt. |
| `BE-MODEL-TEXT-APPLY-011` | Eine über `Open Existing Project` geladene `.use` Datei wird als `modelText` gespeichert und mit Diagnostics beantwortet. |

### Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| Der Schritt wird als vollständiger `.use` Import missverstanden. | Name und Tests auf `MVP-Subset` und `UNSUPPORTED_SYNTAX` ausrichten. |
| Modelltext-Parser und OCL-Parser werden vermischt. | Separate Packages `modeltext.*` und `ocl.*` verwenden. |
| IDs ändern sich bei jedem Apply. | Deterministische ID-Erzeugung aus Namen und vorhandene IDs wiederverwenden. |
| Apply überschreibt Objektmodell ungewollt. | Im MVP nur UML-Modell/Invarianten anwenden; Snapshot nur bewusst unverändert lassen oder klar dokumentieren. |
| Nicht unterstützte Syntax geht still verloren. | Unsupported Konstrukte immer als Diagnostics zurückgeben. |

### Akzeptanzkriterien

- `GET /api/v1/projects/{projectId}/model-text` liefert den gespeicherten Modelltext.
- `POST /api/v1/projects/{projectId}/model-text/apply` akzeptiert vollständigen Editor-Text.
- Derselbe Apply-Endpunkt kann lokalen `.use` Dateiinhalt aus dem Open-Existing-Dialog als `modelText` verarbeiten.
- Unterstützte Klassen, Attribute, Associations und Invarianten werden ins Domain-Modell übernommen.
- Extrahierte OCL-Ausdrücke nutzen die bestehende OCL-Pipeline.
- Nicht unterstützte `.use`-Konstrukte erzeugen `UNSUPPORTED_SYNTAX` mit Source Range.
- JSON Roundtrip erhält `modelText`.
- Es gibt keine Dependency auf den originalen USE-Core.

### Relevante Analyse-Dateien

- `04-ui-ux-analysis/04-ocl-and-validation-ui.md`
- `06-frontend-analysis/09-ocl-editor-ui.md`
- `06-frontend-analysis/15-screenshot-implementation-mapping.md`
- `05-backend-analysis/05-project-and-persistence-service.md`
- `05-backend-analysis/08-ocl-engine-design.md`
- `05-backend-analysis/09-ocl-parser-and-ast.md`
- `05-backend-analysis/13-api-design.md`
- `05-backend-analysis/14-json-project-format.md`
- `05-backend-analysis/15-error-and-result-model.md`
- `05-backend-analysis/17-backend-test-strategy.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/07-dto-reference.md`
- `07-integration-and-api/08-error-contract.md`

### MVP-Relevanz

Hoch. Dieser Schritt ist notwendig, damit der im Screenshot sichtbare OCL Editor fachlich sinnvoll mit dem Backend integriert werden kann. Ohne diesen Schritt könnte das Frontend zwar Text anzeigen, aber `Apply Changes` hätte keinen belastbaren Backend-Vertrag.


## Zusammenfassung

Die Backend-Implementierung sollte zuerst ein eigenständiges Spring-Boot-Fundament schaffen, danach Domain, DTOs und JSON-Projektformat stabilisieren und anschließend die fachlichen Services aufbauen. Die OCL Engine wird ausdrücklich als echte Pipeline geplant: Lexer, Parser, AST, Typechecker und Evaluator.

Der zentrale MVP-Durchstich ist erreicht, wenn ein Projekt mit Klassenmodell, Invariante, Snapshot und Objektlinks gespeichert, geladen und validiert werden kann. Das Library-Szenario mit `alice : User`, `books = 6` und `self.books <= 5` ist der wichtigste Backend-End-to-End-Test, weil es Domain Model, OCL Engine, Validation Service, Error Contract und API zusammenführt.
