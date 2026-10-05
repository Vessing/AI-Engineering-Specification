# Documentation Map

## Zweck dieser Datei

Diese Datei ist die Navigations- und Orientierungskarte für das Analyse-Repository `use-web-analysis/`.

Sie erklärt, welche Dokumentationsbereiche existieren, welche Rolle die einzelnen Ordner und Dateien haben, welche Dokumente gemeinsam gelesen werden sollten und wie Anforderungen von Referenzen bis zur späteren Umsetzung nachvollzogen werden können.

Die Datei richtet sich an:

- Entwickler, die Backend oder Frontend später umsetzen,
- Betreuer, die Projektumfang, Zielbild und Abgrenzung prüfen,
- KI-Agenten, die auf Basis des Analyse-Repositories weitere Dokumente oder Implementierungspläne erstellen,
- neue Leser, die schnell verstehen müssen, wo welche Information liegt.

## Prüfbefund

Die Dokumentationskarte ist als Ziel-Navigation grundsätzlich richtig. Beim Abgleich mit dem aktuellen Dateisystem gibt es aber einen wichtigen Unterschied:

- Die Analysebereiche `00` bis `07` sind vorhanden.
- `08-planning/` ist inzwischen vorhanden, enthält aktuell aber erst `01` bis `03`.
- `09-ocl-extension-analysis/` enthält die systematische OCL-Erweiterungsanalyse. Normative Primärquelle ist `OCL-specification.pdf` im Workspace-Root (OMG OCL 2.4).
- Einige in der Zielstruktur genannte Dateien und Ordner sind noch geplant, aber noch nicht angelegt.
- Der Dashboard-Screenshot `assets/screenshots/00-dashboard-start-page.png` ist vorhanden und muss als erster Einstiegspunkt der Journey berücksichtigt bleiben.

Diese Map unterscheidet deshalb zwischen vorhandenen und geplanten Dokumenten. Geplante Dokumente bleiben bewusst aufgeführt, damit spätere Erstellung und Navigation konsistent bleiben.

## Repository-Gesamtstruktur

Die folgende Struktur beschreibt die Zielstruktur des Analyse-Repositories. Einige Bereiche sind bereits vollständig vorhanden, andere sind als geplante Ergänzungen dokumentiert.

```text
use-web-analysis/
├─ 00-overview/
├─ 01-original-use-reference/
├─ 02-product-and-user-journey/
├─ 03-uml-ocl-domain/
├─ 04-ui-ux-analysis/
├─ 05-backend-analysis/
├─ 06-frontend-analysis/
├─ 07-integration-and-api/
├─ 08-planning/
├─ 09-ocl-extension-analysis/
└─ assets/
   └─ screenshots/
```

Die Struktur trennt bewusst Überblick, Referenzanalyse, Produktanforderungen, Domänenanalyse, UI/UX, Backend, Frontend, Integration, Planung, Post-MVP-OCL-Erweiterungsanalyse und visuelle Screenshot-Assets.

Aktueller Stand:

| Bereich | Status |
|---|---|
| `00-overview/` bis `07-integration-and-api/` | Vorhanden, weitgehend ausgearbeitet. |
| `08-planning/` | Vorhanden mit Roadmap, Backend-Plan und Frontend-Plan; weitere Planungsdateien sind vorgesehen. |
| `09-ocl-extension-analysis/` | OCL-Erweiterungsanalyse mit Bestandsaufnahme, Scope, Featureanalysen, Tests und Roadmap; normative Grundlage ist OMG OCL 2.4. |
| `assets/screenshots/` | Vorhanden, inklusive Dashboard-Screenshot `00-dashboard-start-page.png`. |
| `03-uml-ocl-domain/05-library-example-model.md` | Geplant, aktuell noch nicht angelegt. |
| `07-integration-and-api/06-end-to-end-library-demo.md` | Geplant, aktuell noch nicht angelegt. |
| `08-planning/04-mvp-milestones.md` bis `09-definition-of-done.md` | Geplant, aktuell noch nicht angelegt. |

## Hauptbereiche

| Ordner | Rolle |
|---|---|
| `00-overview/` | Einstieg, Projektzusammenfassung, Ziele, Scope, Nicht-Ziele und Navigationskarte. |
| `01-original-use-reference/` | Analyse des originalen USE-Projekts als fachliche Referenz. Keine technische Migrationsgrundlage. |
| `02-product-and-user-journey/` | Produktvision, screenshotbasierte User Journey, funktionale Anforderungen, MVP-Scope und Akzeptanzkriterien. |
| `03-uml-ocl-domain/` | Fachliche UML/OCL-Domäne, Domänenmodell, OCL-Strategie, Validierungskonzept und Beispielmodell. |
| `04-ui-ux-analysis/` | UI-Struktur, Diagrammansichten, OCL-/Validation-UI und Traceability zu den Screenshots. |
| `05-backend-analysis/` | Backend-Scope, Architektur, Services, OCL Engine, Validation Engine, API, JSON-Format und Tests. |
| `06-frontend-analysis/` | Frontend-Scope, Architektur, Layout, Komponenten, Diagramme, State Management und UI-Tests. |
| `07-integration-and-api/` | Frontend-Backend-Vertrag, API-Flows, DTOs, Error Contract und End-to-End-Demo. |
| `08-planning/` | Roadmap, Backend-/Frontend-Implementierungspläne, Meilensteine, Risiken, Backlog und Definition of Done. |
| `09-ocl-extension-analysis/` | Systematische fachliche und technische Planung der eigenen OCL Engine nach OMG OCL 2.4; USE bleibt ergänzende Kompatibilitäts- und Testreferenz. |
| `assets/` | Screenshots als Referenzmaterial für Analyse und spätere Umsetzung. |

## Dateiübersicht

Hinweis: Die Dateiübersicht enthält sowohl vorhandene Dateien als auch geplante Zieldateien. Geplante Dateien bleiben in der Map enthalten, damit die Navigationskarte weiterhin die vollständige Repository-Zielstruktur beschreibt.

| Pfad | Zweck | Wichtigste Leser | Abhängigkeiten |
|---|---|---|---|
| `00-overview/01-project-summary.md` | Kompakte Projektzusammenfassung. | Alle Leser | Basiskontext, Scope |
| `00-overview/02-goals-scope-and-non-goals.md` | Ziele, Scope, MVP, Post-MVP und Nicht-Ziele. | Alle Leser | Projektzusammenfassung |
| `00-overview/03-documentation-map.md` | Navigationskarte für das Repository. | Alle Leser, KI-Agenten | Gesamte Repository-Struktur |
| `01-original-use-reference/01-original-project-overview.md` | Überblick über das originale USE-Projekt. | Backend, Domäne, Betreuer | Original-USE-Repository |
| `01-original-use-reference/02-use-concepts-reference.md` | Fachliche USE-Konzepte als Referenz. | Domäne, Backend, Produkt | Original-Projektüberblick |
| `01-original-use-reference/03-use-syntax-and-examples.md` | USE-/OCL-Syntax und Beispielmodelle. | OCL, Backend, Tests | Original-USE-Beispiele |
| `01-original-use-reference/04-use-behavior-reference.md` | Verhalten, Validierung und Fehlertypen in USE. | Backend, Validation, Tests | USE-Konzepte, Beispiele |
| `01-original-use-reference/05-reference-relevance-for-new-system.md` | Bewertung der Relevanz für das neue System. | Alle Architekten | Alle USE-Referenzdateien |
| `02-product-and-user-journey/01-target-vision.md` | Produktvision und Zielbild. | Produkt, Betreuer, Entwickler | Project Summary, Screenshots |
| `02-product-and-user-journey/02-screenshot-based-user-journey.md` | User Journey aus Screenshots. | Frontend, Produkt, Tests | `assets/screenshots/*` |
| `02-product-and-user-journey/03-functional-requirements.md` | Funktionale Anforderungen. | Backend, Frontend, Betreuer | User Journey, Scope |
| `02-product-and-user-journey/04-mvp-scope.md` | Konkreter MVP-Scope. | Planung, Entwickler | Goals/Scope, Anforderungen |
| `02-product-and-user-journey/05-acceptance-criteria.md` | Prüfkriterien für MVP-Funktionen. | Tests, Betreuer, Entwickler | MVP-Scope, Screenshots |
| `03-uml-ocl-domain/01-uml-ocl-scope.md` | Fachliche UML/OCL-Abgrenzung. | Domäne, Backend, Frontend | Goals/Scope |
| `03-uml-ocl-domain/02-domain-model.md` | Zentrales fachliches Domänenmodell. | Backend, Frontend, API | UML/OCL-Scope |
| `03-uml-ocl-domain/03-ocl-architecture-and-extension-strategy.md` | OCL-Pipeline und Erweiterungsstrategie. | Backend, OCL, Planung | `OCL-specification.pdf`, Scope; USE ergänzend |
| `03-uml-ocl-domain/04-validation-concept.md` | Konzept für UML- und OCL-Validierung. | Backend, API, Tests | Domänenmodell, OCL-Architektur |
| `03-uml-ocl-domain/05-library-example-model.md` | Geplantes Beispielmodell für Demo und Tests. | Alle Entwickler, Tests | Domänenmodell, Screenshots |
| `04-ui-ux-analysis/01-ui-overview.md` | Überblick über die Weboberfläche. | Frontend, Produkt | Screenshots, Target Vision |
| `04-ui-ux-analysis/02-class-diagram-ui.md` | UI-Anforderungen für Klassendiagramme. | Frontend | Screenshots, Requirements |
| `04-ui-ux-analysis/03-object-diagram-ui.md` | UI-Anforderungen für Objektdiagramme. | Frontend | Screenshots, Validation Concept |
| `04-ui-ux-analysis/04-ocl-and-validation-ui.md` | UI für OCL Editor und Validation Results. | Frontend, OCL | Screenshots, OCL-Architektur |
| `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md` | Mockup-Schritte für UI-wirksame OCL-2.4- und UML-Erweiterungen. | UI/UX, Frontend, Backend, Planung | bestehende Screenshots, Compliance-Matrix, OCL-/UML-Folgeplan |
| `04-ui-ux-analysis/07-m1-ui-baseline.md` | Zur Freigabe vorbereitete M1-Baseline für Workspace-Raster, UI-Zustände, Scroll- und Responsive-Regeln. | UI/UX, Frontend, Planung | Mockup-Roadmap, Screenshots, aktuelles Frontend |
| `04-ui-ux-analysis/08-m2-generalization-and-abstract-classes.md` | M2-Mockup für Generalization Edges, abstrakte Klassen, Mehrfachvererbung und Fehlerzustände. | UI/UX, UML/OCL, Frontend, Backend, API | M1-Baseline, Compliance-Matrix, aktueller DTO-/Domänenstand |
| `04-ui-ux-analysis/09-m3-visibility-namespaces-imports.md` | M3-Mockup für UML-Sichtbarkeit, Namespace-Gruppierung, qualifizierte Namen und modellinterne Imports. | UI/UX, UML/OCL, Frontend, Backend, API | M1-/M2-Baseline, Importflow, Compliance-Matrix |
| `04-ui-ux-analysis/10-redesign-design-principles.md` | Priorisierte Redesign-Vorgaben aus Product-Owner-Interview und Studierendenbefragung für Navigation, Modernisierung, Accessibility und Hilfe. | UI/UX, Produkt, Frontend, Planung | Interview-/Survey-Ergebnisse, alle Mockup-Schritte |
| `04-ui-ux-analysis/11-m4-association-end-properties.md` | M4-Mockup für erweiterte binäre Association-End-Metadaten, Collection-Art, End-Referenzen und Edge-Labelverhalten. | UI/UX, UML/OCL, Frontend, Backend, API | M1-Baseline, Class-Diagram-UI, Compliance-Matrix, aktueller DTO-/Domänenstand |
| `04-ui-ux-analysis/12-m5-qualified-and-nary-associations.md` | M5-Mockup für Qualifierdefinitionen, Qualifierwerte, dynamische Association Ends und n-äre Object Links. | UI/UX, UML/OCL, Frontend, Backend, API | M1-/M4-Baseline, Class-/Object-Diagram-UI, Compliance-Matrix, aktueller DTO-/Domänenstand |
| `04-ui-ux-analysis/13-m6-association-classes-aggregation-composition.md` | M6-Mockup für Association Classes, deren Class Properties und Linkobjekte; Aggregation und Composition werden in der offiziellen Association-Properties-Ansicht gepflegt. | UI/UX, UML/OCL, Frontend, Backend, API | M1-/M4-/M5-Baseline, Class-/Object-Diagram-UI, Compliance-Matrix, aktueller DTO-/Domänenstand |
| `04-ui-ux-analysis/14-m7-operation-signatures-invocation.md` | M7-Mockup für vollständige Operationssignaturen, getrennte Runtime Invocation, typisierte Argumente, Ergebnis und Rollback. | UI/UX, UML/OCL, Frontend, Backend, API | M1-Baseline, Class-/Object-Diagram-UI, Compliance-Matrix, aktueller DTO-/Domänenstand |
| `04-ui-ux-analysis/15-m8-pre-post-before-after.md` | M8-Mockup für Operationsverträge, Precondition-Gates, Postcondition-Auswertung, Vor-/Nachzustand und Rollback. | UI/UX, UML/OCL, Frontend, Backend, API | M7-Invocation, OCL-Kontextanalyse, Compliance-Matrix, aktueller Contract-/Snapshotstand |
| `04-ui-ux-analysis/16-m9-derived-init-body-def.md` | M9-Mockup für Stored/Init/Derived-Attribute, readonly Derived-Werte, OCL-Operation-Bodies sowie Class-/Package-`def`. | UI/UX, UML/OCL, Frontend, Backend, API | M7/M8-Operation Details, OCL-Kontextanalyse, Compliance-Matrix, aktueller Definition-/DTO-Stand |
| `04-ui-ux-analysis/17-m10-enum-datatype-type-picker.md` | M10-Mockup für Enumerations, DataTypes, gemeinsamen kontextbezogenen Type Picker und typisierte Objektwerte. | UI/UX, UML/OCL, Frontend, Backend, API | M3-Namespaces, M7-/M9-Typfelder, Compliance-Matrix, aktueller Typ-/Enum-Stand |
| `04-ui-ux-analysis/18-m11-ocl-compliance-feature-display.md` | M11-Mockup für ehrliche OCL-Subset-Anzeige, Featurestatus, Profildetails und lokale Capability-Diagnostics. | UI/UX, OCL, Frontend, Backend, API | OCL-Compliance-Profil, Compliance-Matrix, OCL-Editor und Error Contract |
| `04-ui-ux-analysis/19-mockup-consistency-and-workflow.md` | Übergreifende Konsistenzprüfung für M1-M11 mit kanonischer Shell, Properties-Hierarchie, Mutationsmuster und End-to-End-Workflow. | UI/UX, Produkt, Frontend, Backend, Planung | M1-M11, aktuelles Frontend, Dashboard-Workflow und Redesign-Prinzipien |
| `04-ui-ux-analysis/48-mockup-file-naming.md` | Verbindliche fachliche Dateinamen, Umbenennungsmatrix und Regeln für auflösbare Mockup-Referenzen ohne Roadmap-Präfixe. | UI/UX, Frontend, Dokumentation | alle Mockups und Analyse-Referenzen |
| `04-ui-ux-analysis/24-unified-explorer-navigation.md` | Definiert den kanonischen Class-Diagram-Explorer mit Package- und Import-Hierarchie, Root-Alternative und Suche. | Frontend, UX, UML, API | konsolidierte Class-, Type- und Import-Mockups |
| `04-ui-ux-analysis/25-unified-bottom-panel-states.md` | Definiert die getrennten Inhalte und Aktivierungsregeln für Console, Diagnostics, Validation Results und Invocation Results. | Frontend, UX, OCL, API, QA | Operations-, Definitions-, Type- und Contract-Mockups |
| `04-ui-ux-analysis/26-unified-object-diagram-workspace.md` | Definiert mit `workspace-object-explorer.html` den kanonischen Object-Diagram-Workspace mit objektbezogenem Explorer, Objektlinks, Properties und gemeinsamem Bottom Panel. | Frontend, UX, UML/OCL, API | Object-Diagram-Analyse, M7/M8 und Bottom-Panel-Referenz |
| `04-ui-ux-analysis/27-create-object-modal.md` | Definiert den Classifier-gesteuerten Create-Object-Dialog mit Initialwerten, Validierung und atomarer Auswahl. | Frontend, UX, UML/OCL, API | Unified Object Diagram und Object Model |
| `04-ui-ux-analysis/28-create-association-modal.md` | Definiert den Create-Association-Dialog mit mindestens zwei Ends, n-ärer Erweiterung und Übergabe an Association Properties. | Frontend, UX, UML/OCL, API | Class Diagram und Association Properties |
| `04-ui-ux-analysis/29-create-object-link-modal.md` | Definiert den Create-Object-Link-Dialog mit Association-Auswahl, endbasierter Objektbelegung, Qualifierwerten und atomarer Snapshot-Aktualisierung. | Frontend, UX, UML/OCL, API | Object Diagram und Object Model |
| `04-ui-ux-analysis/30-object-link-association-sidebar.md` | Vereinheitlicht Object-Link-Auswahl und Endbelegungen; dokumentiert eine einfache Standardansicht sowie eine getrennte Ordered-/Unique-Variante. | Frontend, UX, UML/OCL, API | Object Diagram und Object Link Selection |
| `04-ui-ux-analysis/31-object-properties-validation-and-delete.md` | Definiert Feldvalidierung, Save-Zustände und eine auswirkungsbewusste, atomare Objektlöschung. | Frontend, UX, UML/OCL, API | Object Diagram und Object Lifecycle |
| `04-ui-ux-analysis/40-delete-object-link-modal.md` | Definiert die revisionssichere Löschung eines Object Links einschließlich Association-Class-, Composition- und Validierungsfolgen. | Frontend, UX, UML/OCL, API | Object Diagram, Object Link Selection und Snapshot Lifecycle |
| `04-ui-ux-analysis/41-association-class-instance.md` | Definiert die gemeinsame Runtime-Identität und Darstellung einer Association-Class-Instanz als Object Link und Objektansicht. | Frontend, UX, UML/OCL, API | Object Diagram, Association Classes und Object Link Lifecycle |
| `04-ui-ux-analysis/32-delete-class-modal.md` | Definiert die abhängigkeitsbewusste Klassenlöschung mit Owned Features, externen Modellreferenzen, OCL-Verwendungen und blockierenden Objektinstanzen. | Frontend, UX, UML/OCL, API | Class Diagram und Model Lifecycle |
| `04-ui-ux-analysis/33-invariant-properties.md` | Konsolidiert Invariant-Liste, Context Class, Name, OCL-Ausdruck, Diagnostik, OCL-Editor-Synchronisation und Löschung im Class-Properties-Workflow. | Frontend, UX, UML/OCL, API | Class Diagram, OCL Editor und Validation |
| `04-ui-ux-analysis/20-m10-5-datatype-properties.md` | M10.5-Ergänzung für das Bearbeiten ausgewählter UML-DataTypes mit Details, Value Properties und vollständigen UI-Zuständen. | UI/UX, UML/OCL, Frontend, Backend, API | M10-Type-Workflow, gemeinsamer Type Picker, Compliance-Matrix |
| `04-ui-ux-analysis/21-association-properties.md` | Offizielle konsolidierte Association-Properties-Ansicht für Stammdaten, Ends, Qualifier, Aggregation Kind und Einstieg zur Association Class. | UI/UX, UML/OCL, Frontend, Backend, API | M4-/M5-/M6-Ergebnisse, Class-Diagram-UI, Compliance-Matrix |
| `04-ui-ux-analysis/05-screenshot-traceability.md` | Zuordnung Screenshots zu Anforderungen und Komponenten. | Frontend, Tests, Betreuer | `assets/screenshots/*` |
| `05-backend-analysis/01-backend-scope.md` | Backend-Verantwortung und Grenzen. | Backend | Goals/Scope |
| `05-backend-analysis/02-backend-repository-structure.md` | Zielstruktur des Backend-Repositories. | Backend | Backend-Scope |
| `05-backend-analysis/03-backend-architecture.md` | Backend-Architektur auf hoher Ebene. | Backend, Architekten | Domain Model |
| `05-backend-analysis/04-domain-model-implementation.md` | Umsetzung des Domänenmodells im Backend. | Backend | `03-uml-ocl-domain/02-domain-model.md` |
| `05-backend-analysis/05-project-and-persistence-service.md` | Projektverwaltung und Persistenzkonzept. | Backend | JSON-Format, API |
| `05-backend-analysis/06-uml-model-service.md` | Service für UML-Klassenmodell. | Backend | Domain Model, Validation |
| `05-backend-analysis/07-object-model-service.md` | Service für Objekte, Slots, Links und Snapshots. | Backend | Domain Model, Validation |
| `05-backend-analysis/08-ocl-engine-design.md` | Design der OCL Engine. | Backend, OCL | OCL-Architektur, `OCL-specification.pdf`; USE ergänzend |
| `05-backend-analysis/09-ocl-parser-and-ast.md` | Parser- und AST-Konzept. | Backend, OCL | OCL Engine Design |
| `05-backend-analysis/10-ocl-typechecker.md` | Typprüfung für OCL. | Backend, OCL | Parser/AST, Domain Model |
| `05-backend-analysis/11-ocl-evaluator.md` | Evaluator-Konzept für Snapshots. | Backend, OCL | Typechecker, Object Model |
| `05-backend-analysis/12-validation-service.md` | Validation Service für UML und OCL. | Backend, API, Tests | Validation Concept |
| `05-backend-analysis/13-api-design.md` | Backend-API-Design. | Backend, Frontend | Integration Contract |
| `05-backend-analysis/14-json-project-format.md` | JSON-Projektformat für den MVP. | Backend, Frontend | Domain Model, API |
| `05-backend-analysis/15-error-and-result-model.md` | Fehler- und Ergebnisstruktur. | Backend, Frontend, Tests | Validation Concept, Error Contract |
| `05-backend-analysis/16-original-use-reference-check.md` | Abgleich gegen originale USE-Referenz. | Backend, Betreuer | `01-original-use-reference/*` |
| `05-backend-analysis/17-backend-test-strategy.md` | Backend-Teststrategie. | Backend, QA | USE-Beispiele, Validation |
| `06-frontend-analysis/01-frontend-scope.md` | Frontend-Verantwortung und Grenzen. | Frontend | Goals/Scope, UI Overview |
| `06-frontend-analysis/02-frontend-repository-structure.md` | Zielstruktur des Frontend-Repositories. | Frontend | Frontend-Scope |
| `06-frontend-analysis/03-frontend-architecture.md` | Frontend-Architektur auf hoher Ebene. | Frontend | API Contract |
| `06-frontend-analysis/04-routing-and-layout.md` | Routing, Hauptlayout und Arbeitsbereiche. | Frontend | UI Overview |
| `06-frontend-analysis/05-api-client-and-dtos.md` | API Client und DTO-Verwendung. | Frontend, Backend | DTO Reference, API Contract |
| `06-frontend-analysis/06-diagram-library-decision.md` | Auswahl der Diagrammbibliothek. | Frontend, Architekten | UI-Anforderungen |
| `06-frontend-analysis/07-class-diagram-component.md` | Komponentenkonzept für Klassendiagramme. | Frontend | Class Diagram UI |
| `06-frontend-analysis/08-object-diagram-component.md` | Komponentenkonzept für Objektdiagramme. | Frontend | Object Diagram UI |
| `06-frontend-analysis/09-ocl-editor-ui.md` | OCL Editor UI. | Frontend, OCL | OCL/Validation UI |
| `06-frontend-analysis/10-validation-error-ui.md` | Darstellung von Validierungsfehlern. | Frontend, Tests | Error Contract, Validation Results |
| `06-frontend-analysis/11-properties-panel.md` | Properties Panel für ausgewählte Elemente. | Frontend | Screenshots, UI Overview |
| `06-frontend-analysis/12-modal-dialogs.md` | Modals für Erstellen und Bearbeiten. | Frontend | Screenshots |
| `06-frontend-analysis/13-explorer-sidebar.md` | Explorer Sidebar und Projektstruktur. | Frontend | UI Overview |
| `06-frontend-analysis/14-state-management.md` | Frontend-State und Synchronisation. | Frontend | API Client, Components |
| `06-frontend-analysis/15-screenshot-implementation-mapping.md` | Mapping von Screenshots auf Komponenten. | Frontend, Betreuer | Screenshot Traceability |
| `06-frontend-analysis/16-frontend-test-strategy.md` | Frontend-Teststrategie. | Frontend, QA | Acceptance Criteria, Screenshots |
| `07-integration-and-api/01-frontend-backend-contract.md` | Vertrag zwischen Frontend und Backend. | Backend, Frontend | API Design, DTOs |
| `07-integration-and-api/02-api-flow.md` | Allgemeine API-Abläufe. | Backend, Frontend | API Contract |
| `07-integration-and-api/03-validation-flow.md` | Ablauf von Check Constraints. | Backend, Frontend, Tests | Validation Service, Error Model |
| `07-integration-and-api/04-project-save-load-flow.md` | Speichern und Laden von Projekten. | Backend, Frontend | JSON Project Format |
| `07-integration-and-api/05-ocl-evaluation-flow.md` | Ablauf der OCL-Auswertung. | Backend, OCL | OCL Engine, Validation Flow |
| `07-integration-and-api/06-end-to-end-library-demo.md` | Geplante End-to-End-Demo anhand eines Beispielmodells. | Alle Entwickler, Betreuer | Screenshots, Library Example |
| `07-integration-and-api/07-dto-reference.md` | DTO-Referenz für API und Frontend. | Backend, Frontend | Domain Model, API Contract |
| `07-integration-and-api/08-error-contract.md` | Fehlervertrag zwischen Backend und Frontend. | Backend, Frontend, QA | Error Model, Validation UI |
| `08-planning/01-overall-implementation-roadmap.md` | Gesamt-Roadmap. | Projektplanung, Betreuer | Scope, MVP |
| `08-planning/02-backend-implementation-plan.md` | Backend-Umsetzungsplan. | Backend | Backend Analysis |
| `08-planning/03-frontend-implementation-plan.md` | Frontend-Umsetzungsplan. | Frontend | Frontend Analysis |
| `08-planning/04-mvp-milestones.md` | Geplante MVP-Meilensteine. | Alle Entwickler | MVP Scope, Acceptance Criteria |
| `08-planning/05-iteration-plan.md` | Geplanter iterativer Arbeitsplan. | Projektplanung | Roadmap, Milestones |
| `08-planning/06-risks-and-open-questions.md` | Geplanter Risikokatalog und offene Fragen. | Alle Leser | Alle Bereiche |
| `08-planning/07-post-mvp-roadmap.md` | Geplante Erweiterungen nach dem MVP. | Planung, Betreuer | Goals/Scope |
| `08-planning/08-issue-backlog.md` | Geplantes Backlog für spätere Umsetzung. | Entwickler | Implementation Plans |
| `08-planning/09-definition-of-done.md` | Geplante Definition of Done für Analyse und MVP. | Alle Entwickler, Betreuer | Acceptance Criteria |
| `09-ocl-extension-analysis/01-ocl-current-state.md` | Ausgangsstand und Compliance-Gap der OCL-MVP-Architektur gegenüber OMG OCL 2.4. | Backend, OCL, Planung | `OCL-specification.pdf`, OCL-Domäne, Backend-Analyse, Integration |
| `09-ocl-extension-analysis/02-ocl-extension-scope.md` | Vollständiges OCL-2.4-Coverage-Inventar, Umfang und Grenzen der inkrementellen Erweiterung. | Domäne, Backend, Planung | `OCL-specification.pdf`, Current State, MVP-Scope |
| `09-ocl-extension-analysis/03-ocl-feature-prioritization.md` | Priorisierung der Erweiterungsfeatures nach Nutzen, Abhängigkeiten und Risiko. | OCL, Planung, Tests | Extension Scope, Library-Beispiel |
| `09-ocl-extension-analysis/04-collection-operations.md` | Planung zusätzlicher Collection-Typen und -Operationen. | Backend, OCL, Tests | OCL Engine, Feature-Priorisierung |
| `09-ocl-extension-analysis/05-iterator-expressions.md` | Planung von Iterator-Ausdrücken und Iteratorvariablen. | Backend, OCL, Tests | Collections, Parser, Typechecker, Evaluator |
| `09-ocl-extension-analysis/06-let-if-allinstances.md` | Planung von `let`, `if-then-else` und `allInstances`. | Backend, OCL, Tests | Parser, Typechecker, Snapshot-Evaluation |
| `09-ocl-extension-analysis/07-pre-post-derived-init.md` | Planung von Pre-/Postconditions, derived Attributes und init Values. | Domäne, Backend, API | Domain Model, Validation Flow |
| `09-ocl-extension-analysis/08-ocl-error-handling-and-source-locations.md` | Durchgängiges Diagnose- und Source-Location-Konzept. | Backend, Frontend, API, Tests | Error Contract, Validation UI |
| `09-ocl-extension-analysis/09-ocl-test-strategy.md` | Teststrategie für die erweiterte OCL-Pipeline. | Backend, QA, OCL | Backend-Tests, Library-Beispiel, USE-Referenz |
| `09-ocl-extension-analysis/10-ocl-extension-roadmap.md` | Post-MVP-Ausbaustufen und technische Abhängigkeiten. | Planung, Backend, Betreuer | Priorisierung, alle OCL-Erweiterungsanalysen |
| `09-ocl-extension-analysis/13-ocl-compliance-profile.md` | Versioniertes, maschinenlesbares OCL-2.4-Subset-Profil, Runtime-Limits und reproduzierbare Abschlussreports. | Backend, Frontend, QA, Betreuer | OCL-Roadmap, Teststrategie, Gap-Analyse |
| `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md` | Detaillierte OCL-2.4- und UML-Abhängigkeitsmatrix mit feingranularem Compliance-Status. | OCL, UML, Backend, QA, Betreuer | OCL-Spezifikation, Compliance-Profil, Gap-Analyse |
| `09-ocl-extension-analysis/15-full-ocl-uml-implementation-plan.md` | Backend-Plan B1 bis B47: OCL-/UML-Compliance, Contract-Haertung und abschliessendes Retirement nachweislich unbenutzter Legacy-Mutationspfade nach der Frontendmigration. | Backend, API, QA | vollstaendige Compliance-Matrix, kanonischer Mockup-Index und Frontend-Command-Migration |
| `09-ocl-extension-analysis/16-full-ocl-uml-implementation-coordination.md` | Verbindet Mockup-, Backend- und Frontend-Spur ueber Stage-Gates und Uebergabevertraege. | Planung, Backend, Frontend, QA | Mockup- und Implementierungsplaene |
| `09-ocl-extension-analysis/49-b34-mockup-backend-contract-matrix-v1.md` | Versionierte B34-Inventur aller kanonischen Mockup-Zustaende gegen Domain, Services, API-v1, DTOs, Revisionen und Fehlercodes. | Backend, API, Frontend, QA | kanonischer Mockup-Index, Implementierungs- und Koordinationsplan, realer Backendstand |
| `09-ocl-extension-analysis/50-b35-read-and-result-contracts.md` | B35-Ergebnis für die additive semantische Read-Projektion von Explorer, Vererbung, Features, Werten, Object Links und Diagnostics. | Backend, API, Frontend, QA | B34-Matrix, Mockups, realer Domain-/DTO-Stand |
| `09-ocl-extension-analysis/51-b36-write-delete-and-blocker-contracts.md` | B36-Ergebnis fuer atomare, revisionsgeschuetzte Schreibbefehle, Delete-Impact, explizite Cascades und strukturierte Commandfehler. | Backend, API, Frontend, QA | B34-Matrix, B35-Ergebnis, realer Domain-/Service-/Fehlerstand |
| `09-ocl-extension-analysis/53-b38-explicit-feature-redefinition.md` | B38-Ergebnis fuer explizite Attribute-/Operations-Redefinition, Mehrfachvererbungskonflikte, OCL-Dispatch und revisionsgeschuetzte Vertraege. | Backend, UML/OCL, API, Frontend, QA | B37-Luecke S1, Unified-Generalization-Mockup, Compliance-Matrix |
| `09-ocl-extension-analysis/54-b39-static-classifier-values.md` | B39-Ergebnis fuer statische Features, classifierweite Werte, OCL-Aufloesung, revisionsgeschuetztes Update und Objekt-Slot-Ausschluss. | Backend, UML/OCL, API, Frontend, QA | B37-Luecke S2, Static-Attribute-Mockup, Contract-Matrix |
| `09-ocl-extension-analysis/55-b40-persisted-class-package-definitions.md` | B40-Ergebnis fuer persistierte Class-/Package-Definitionen, Source Ranges, Runtime-Registry sowie revisionsgeschuetzte CRUD- und Blockervertraege. | Backend, UML/OCL, API, Frontend, QA | B37-Luecke S3, Definitions-Mockups, Contract-Matrix |
| `09-ocl-extension-analysis/56-b41-revision-protected-snapshot-commands.md` | B41-Ergebnis fuer revisionsgeschuetztes Object Create, Slot Update und Object-Link Create mit Draft-Erhalt, stabilen Referenzen und atomarer Snapshotmutation. | Backend, API, Frontend, QA | B37-Vertragsstand, Object-Diagram-Mockups, Contract-Matrix |
| `09-ocl-extension-analysis/57-b42-object-link-update-delete-lifecycle.md` | B42-Ergebnis fuer revisionsgeschuetztes Object-Link-Update, strukturierten Delete Impact, Association-Class-Cascades und Legacy-Delete-Absicherung. | Backend, API, Frontend, QA | B41 Snapshot-Commands, Object-Link-Mockups, Contract-Matrix |
| `09-ocl-extension-analysis/58-b43-enumeration-lifecycle.md` | B43-Ergebnis fuer revisionsgeschuetztes Enumeration-CRUD, stabile Literal-IDs, autoritative Read-Projektion und referenzbewussten Delete Impact. | Backend, UML/OCL, API, Frontend, QA | M10, B37-Vertragsstand, Contract-Matrix |
| `09-ocl-extension-analysis/17-full-ocl-uml-frontend-implementation-plan.md` | Frontend-Plan F1 bis F11, F12 sowie Nachholschritte fuer die Umsetzung der Mockups und die Migration vorhandener Workflows auf V2-Commands. | Frontend, UX, QA | Mockups und stabile API-Vertraege |
| `09-ocl-extension-analysis/18-ocl-24-normative-inventory.md` | B1-Inventar mit stabilen IDs fuer OCL-2.4-Syntax, Well-formedness, Semantik, Kontexte, Bibliothekssignaturen und Compliance Points. | Backend, OCL, QA | `OCL-specification.pdf`, Compliance-Matrix |
| `09-ocl-extension-analysis/19-b2-reference-case-feature-mapping.md` | B2-Zuordnung aller nicht erfolgreichen Original-USE-Reference-Cases zu Matrix-ID, Backend-Zielschritt und Blockerklasse. | Backend, OCL, QA, Planung | Reference-Report, Gap-Analyse, Compliance-Matrix |
| `09-ocl-extension-analysis/20-b3-ocl-24-compliance-test-harness.md` | B3-Harness fuer getrennte, datengetriebene und nach Matrix-ID filterbare normative OCL-2.4-Tests. | Backend, OCL, QA, Planung | normatives Inventar, Compliance-Matrix, Teststrategie |
| `09-ocl-extension-analysis/21-b4-uml-generalization-stabilization.md` | B4-Ergebnis für abstrakte Klassen, Mehrfachvererbung, Konfliktdiagnosen, hierarchischen LUB und Subtyp-Snapshots. | Backend, UML/OCL, API, QA, Frontend | M2, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/22-b5-visibility-namespaces-imports.md` | B5-Ergebnis für UML-Sichtbarkeit, Packages, qualifizierte Namen, Importauflösung und Provenienz. | Backend, UML/OCL, API, QA, Frontend | M3, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/23-b6-association-end-metadata.md` | B6-Ergebnis für Association-End-Metadaten, reflexive Rollen und OCL-Navigationstypen. | Backend, UML/OCL, API, QA, Frontend | M4, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/24-b7-qualified-and-nary-associations.md` | B7-Ergebnis für Qualifier, n-äre Associations, endbasierte Links und qualifizierte OCL-Navigation. | Backend, UML/OCL, API, QA, Frontend | M5, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/25-b8-association-classes-composition.md` | B8-Ergebnis für Association Classes, gemeinsame Link-/Objektidentität, Aggregation und Composition-Lebenszyklus. | Backend, UML/OCL, API, QA, Frontend | M6, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/26-b9-operation-invocation-lifecycle.md` | B9-Ergebnis für Operationssignaturen, Dispatch, atomare Invocation, Rollback und Lifecycle-Diff. | Backend, UML/OCL, API, QA, Frontend | M7/M8, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/28-b11-numeric-string-standard-library.md` | B11-Ergebnis für numerische Operatoren, primitive Standardbibliothek, String-Literale, Unicode- und Indexregeln. | Backend, OCL, QA, Frontend | OCL-2.4-Spezifikation, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/29-b12-call-resolution-and-dispatch.md` | B12-Ergebnis für gemeinsame Property-, Operations-, Definitions- und Standard-Library-Auflösung in Typechecker und Evaluator. | Backend, OCL, UML, QA, Frontend | OCL-2.4-Spezifikation, B9/B11, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/30-b13-tuple-enum-datatype-classifier-values.md` | B13-Ergebnis für strukturelle Tuples, qualifizierte Enumerationen, UML-DataTypes und Classifier-Werte. | Backend, OCL, UML, API, QA, Frontend | M10, OCL-2.4-Spezifikation, B12, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/31-b14-collection-literals-and-abstract-semantics.md` | B14-Ergebnis für Collection-Literale, Ranges, Common Types und die abstrakte Collection-Standardbibliothek. | Backend, OCL, QA, Frontend | OCL-2.4-Spezifikation, B13, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/32-b15-concrete-collection-types.md` | B15-Ergebnis für konkrete Set-, Bag-, Sequence- und OrderedSet-Signaturen, Werte und Kombinationen. | Backend, OCL, QA, Frontend | Collection-Analyse, B14, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/33-b16-iterators-and-implicit-collect.md` | B16-Ergebnis für Iterator-Scope, Vierwertlogik, konkrete Iterator-Ergebnisarten und implizite Collect-Kurzformen. | Backend, OCL, QA, Frontend | Iterator-Analyse, B15, Compliance-Matrix, Backend-Implementierungsplan |
| `09-ocl-extension-analysis/34-b17-recursion-closure-iterate-budgets.md` | B17-Ergebnis für rekursive OCL-Ausführung, `closure`, `iterate` und profilierte Ressourcenbudgets. | Backend, OCL, API, QA, Frontend | Iterator-Analyse, Compliance-Profil, B16, Compliance-Matrix |
| `09-ocl-extension-analysis/35-b18-operation-contract-runtime.md` | B18-Ergebnis für persistierte Pre-/Postconditions, atomare Contract-Gates, `result`, `@pre` und `oclIsNew()`. | Backend, OCL, API, QA, Frontend | M8, B9, Compliance-Matrix, Operation Runtime |
| `09-ocl-extension-analysis/36-b19-derived-init-body-def-runtime.md` | B19-Ergebnis für snapshotgebundene Derived-Auswertung, Abhängigkeitsgraph, atomare Init-Werte sowie OCL-Query-Bodies und Class-`def`. | Backend, OCL, API, QA, Frontend | M9, B9/B18, Compliance-Matrix, Definition Runtime |
| `09-ocl-extension-analysis/37-b20-optional-navigation-visibility-profile.md` | B20-Ergebnis für die öffentlichen Profilentscheidungen zu nicht navigierbaren Association Ends und UML-Sichtbarkeits-Bypass. | Backend, OCL, API, QA, Frontend | M3/M4/M11, Compliance-Profil, Compliance-Matrix |
| `09-ocl-extension-analysis/38-b21-state-message-exclusion-profile.md` | B21-Ergebnis und Exclude-Entscheidung für State Machines, `oclInState`, Operation Traces und Message Expressions. | Backend, OCL, API, QA, Frontend | Scope, M11-M13, Compliance-Profil, Compliance-Matrix |
| `09-ocl-extension-analysis/39-b22-parser-evaluator-hardening.md` | B22-Ergebnis für Parser-Fuzzing, begrenzte Recovery, parallele Parsernutzung und vollständige OCL-Runtime-Budgets. | Backend, OCL, API, QA, Frontend | OCL-Teststrategie, Engine-Design, Compliance-Profil, Error Contract |
| `09-ocl-extension-analysis/40-b23-reference-expectation-migration.md` | B23-Ergebnis für die Entkopplung fachlicher Referenzerwartungen von USE-Shelltext und alter Testinfrastruktur. | Backend, OCL, UML, QA, Planung | Reference-Migration, Gap-Analyse, B2-Zuordnung, Compliance-Matrix |
| `09-ocl-extension-analysis/41-b24-ocl-gap-closure-and-regression.md` | B24-Ergebnis für semantische Reference-Vergleiche, USE-Dialektentscheidungen, Gap-Priorisierung und promovierte Regressionen. | Backend, OCL, UML, QA, Planung | Reference-Suite, Compliance-Matrix, B23, B25 |
| `09-ocl-extension-analysis/42-b25-compliance-acceptance-and-profile-v2.md` | B25-Abnahme für das versionierte OCL-Subset-Profil, die ausführbare Profilmatrix und normativ entschiedene Reference-Fälle. | Backend, OCL, API, Frontend, QA, Planung | Compliance-Profil, Matrix, Reference-Suite, F11/F12 |
| `09-ocl-extension-analysis/43-post-b25-reference-gap-closure-plan.md` | Folgeplan B26 bis B33 zur vollständigen Schließung der nach B25 verbleibenden 382 fachlichen Reference-Gaps. | Backend, OCL, UML, QA, Planung | B25-Report, Compliance-Matrix, Reference-Suite, Profil v2 |
| `09-ocl-extension-analysis/44-b29-non-collection-type-gap-closure.md` | B29-Abschluss der nicht-collectionbezogenen Typregeln und bereinigte B29-/B30-Zuordnung. | Backend, OCL, UML, QA | Gap-Report, Typechecker, Standard Library |
| `09-ocl-extension-analysis/45-b30-collection-iterator-type-gap-closure.md` | B30-Abschluss der Collection- und Iterator-Typregeln einschließlich implizitem Collect und Iterator-Kurzformen. | Backend, OCL, QA | Collection-/Iteratoranalyse, Typechecker, Evaluator, Reference-Suite |
| `09-ocl-extension-analysis/46-backend-step-analysis-file-traceability.md` | Zentrales Referenzprotokoll der pro Backend-Schritt tatsächlich gelesenen Analyse-, Mockup- und fachlichen Referenzdateien; historische Rekonstruktionsgrenzen sind gekennzeichnet, zukünftige Schritte werden unmittelbar mit Lesetiefe und Verwendungszweck gepflegt. | Backend, OCL, UML, API, QA, Planung | Documentation Map, Backend-Implementierungsplan, Koordinationsplan, Mockup-Index und schrittspezifische Ergebnisdokumente |
| `09-ocl-extension-analysis/53-frontend-step-analysis-file-traceability.md` | Zentrales Referenzprotokoll fuer F1-F11, F12 und Nachholschritte mit Lesetiefe, Verwendungszweck und Contract-Zuordnung. | Frontend, UI/UX, API, QA, Planung | Documentation Map, Frontend-Implementierungsplan, kanonischer Mockup-Index, Contract-Matrix und schrittspezifische Ergebnisse |
| `09-ocl-extension-analysis/47-b32-structured-result-and-diagnostic-alignment.md` | B32-Abschluss der strukturierten Wert- und Diagnostic-Angleichung mit reproduzierbaren Reference-Zahlen. | Backend, OCL, UML, QA | Implementierungsplan, Gap-Folgeplan, Compliance-Matrix, Reference-Suite |
| `09-ocl-extension-analysis/48-b33-null-gap-acceptance-and-profile-v3.md` | B33-Abschluss mit Null-Gap-Report, begruendeter Legacy-Fixture-Abgrenzung und Compliance-Profil v3. | Backend, OCL, UML, API, Frontend, QA, Planung | Implementierungsplan, Gap-Folgeplan, Compliance-Profil, Compliance-Matrix, Reference-Suite |
| `09-ocl-extension-analysis/49-b34-mockup-backend-contract-matrix-v1.md` | Versionierte Zuordnung aller kanonischen Mockups zu Backend-, API-, DTO- und Frontendverträgen. | Backend, Frontend, API, QA, Planung | Mockup-Index, B34-B36, API-/DTO-Referenz |
| `09-ocl-extension-analysis/50-b35-read-and-result-contracts.md` | B35-Ergebnis für additive Read-Model-, Wert-, Explorer- und Diagnostic-Projektionen. | Backend, Frontend, API, QA | B34-Matrix, Read-Model-Implementierung |
| `09-ocl-extension-analysis/51-b36-write-delete-and-blocker-contracts.md` | B36-Ergebnis für atomare Commands, Revisionen, Delete-Impact, Blocker und Cascades. | Backend, Frontend, API, QA | B34-Matrix, Command-Implementierung |
| `09-ocl-extension-analysis/52-b37-mockup-api-contract-acceptance.md` | Reproduzierbare B37-Abnahme aller kanonischen Mockups; dokumentiert die blockierenden Semantikschritte B38-B40. | Backend, Frontend, API, QA, Planung | B34-B36, Mockup-Index, Frontendplan, Acceptance-Skript |
| `09-ocl-extension-analysis/59-b44-model-feature-association-commands.md` | B44-Abschluss fuer revisionsgeschuetzte Attribute-, Operations- und Association-Commands mit atomarer Draftvalidierung. | Backend, UML, OCL, API, Frontend, QA | B34-Matrix, B37-Abnahme, Feature-/Association-Mockups, Command-Schicht |
| `09-ocl-extension-analysis/60-b45-package-import-lifecycle.md` | B45-Abschluss fuer revisionsgeschuetztes Package-/Import-Rename, Move, Update und Delete mit expliziten Cascades und referenzbewusstem Impact. | Backend, UML, OCL, API, Frontend, QA | M3, B37-Vertragsstand, Contract-Matrix, Package-/Import-Commands |
| `09-ocl-extension-analysis/61-b46-strict-contract-acceptance.md` | Strenge V2-Abnahme der B41-B45-Vertraege; setzt `BACKEND_CONTRACT_READY_V2` und dokumentiert die Uebergabe an F5-F11 und F12. | Backend, API, Frontend, QA, Planung | B41-B45, Contract-Matrix, kanonische Mockups, normale und Reference-Suite |
| `09-ocl-extension-analysis/64-b50-persisted-structured-value-types.md` | B50-Abschluss der rekursiven persistierten DataType-, Tuple- und Collection-Typen fuer Attribute, Classifierwerte und Slots. | Backend, UML, API, Frontend, QA | F10-Luecke, Typresolver, Model-/Snapshot-Commands, Persistenz, Contract-Matrix |
| `09-ocl-extension-analysis/65-b51-datatype-property-delete-lifecycle.md` | B51-Abschluss des revisionsgeschuetzten DataType-Property-Impact/-Delete und des gemeinsamen Gates fuer vollstaendige DataType-Updates. | Backend, UML, API, Frontend, QA | F10N, DataType Properties, rekursive Wertblocker, OCL Source Ranges, Contract-Matrix |
| `09-ocl-extension-analysis/verify-b46-contract-acceptance.ps1` | Ausfuehrbarer B46-Abgleich fuer Matrix, Command-Routen, Testberichte und Reference-Null-Gap-Stand. | Backend, API, QA | B46-Ergebnis, Backend-Testberichte |
| `09-ocl-extension-analysis/b46-contract-acceptance-report.md` | Generierter Bericht der ausfuehrbaren B46-Abnahme. | Backend, API, QA | B46-Acceptance-Skript |
| `assets/screenshots/README.md` | Index und Beschreibung der Screenshots. | Frontend, Produkt, Tests | Screenshot-Dateien |

## Empfohlene Lesepfade

### Projektüberblick

1. `00-overview/01-project-summary.md`
2. `00-overview/02-goals-scope-and-non-goals.md`
3. `00-overview/03-documentation-map.md`

### Original-USE-Referenz

1. `01-original-use-reference/01-original-project-overview.md`
2. `01-original-use-reference/02-use-concepts-reference.md`
3. `01-original-use-reference/03-use-syntax-and-examples.md`
4. `01-original-use-reference/04-use-behavior-reference.md`
5. `01-original-use-reference/05-reference-relevance-for-new-system.md`
6. `05-backend-analysis/16-original-use-reference-check.md`

### Produkt und MVP

1. `02-product-and-user-journey/01-target-vision.md`
2. `02-product-and-user-journey/02-screenshot-based-user-journey.md`
3. `02-product-and-user-journey/03-functional-requirements.md`
4. `02-product-and-user-journey/04-mvp-scope.md`
5. `02-product-and-user-journey/05-acceptance-criteria.md`
6. `08-planning/04-mvp-milestones.md` (geplant)

### UML/OCL-Domäne

1. `03-uml-ocl-domain/01-uml-ocl-scope.md`
2. `03-uml-ocl-domain/02-domain-model.md`
3. `03-uml-ocl-domain/03-ocl-architecture-and-extension-strategy.md`
4. `03-uml-ocl-domain/04-validation-concept.md`
5. `03-uml-ocl-domain/05-library-example-model.md` (geplant)

### UI/UX und neue Screenshots

1. `assets/screenshots/README.md`
2. `02-product-and-user-journey/02-screenshot-based-user-journey.md`
3. `04-ui-ux-analysis/01-ui-overview.md`
4. `04-ui-ux-analysis/02-class-diagram-ui.md`
5. `04-ui-ux-analysis/03-object-diagram-ui.md`
6. `04-ui-ux-analysis/04-ocl-and-validation-ui.md`
7. `04-ui-ux-analysis/05-screenshot-traceability.md`
8. `06-frontend-analysis/15-screenshot-implementation-mapping.md`

### Backend-Entwicklung

1. `00-overview/02-goals-scope-and-non-goals.md`
2. `03-uml-ocl-domain/02-domain-model.md`
3. `03-uml-ocl-domain/04-validation-concept.md`
4. `05-backend-analysis/01-backend-scope.md`
5. `05-backend-analysis/03-backend-architecture.md`
6. `05-backend-analysis/04-domain-model-implementation.md`
7. `05-backend-analysis/08-ocl-engine-design.md`
8. `05-backend-analysis/12-validation-service.md`
9. `05-backend-analysis/13-api-design.md`
10. `05-backend-analysis/17-backend-test-strategy.md`

### Frontend-Entwicklung

1. `02-product-and-user-journey/02-screenshot-based-user-journey.md`
2. `04-ui-ux-analysis/01-ui-overview.md`
3. `06-frontend-analysis/01-frontend-scope.md`
4. `06-frontend-analysis/03-frontend-architecture.md`
5. `06-frontend-analysis/04-routing-and-layout.md`
6. `06-frontend-analysis/07-class-diagram-component.md`
7. `06-frontend-analysis/08-object-diagram-component.md`
8. `06-frontend-analysis/10-validation-error-ui.md`
9. `06-frontend-analysis/14-state-management.md`
10. `06-frontend-analysis/16-frontend-test-strategy.md`

### Integration und API

1. `07-integration-and-api/01-frontend-backend-contract.md`
2. `07-integration-and-api/02-api-flow.md`
3. `07-integration-and-api/03-validation-flow.md`
4. `07-integration-and-api/04-project-save-load-flow.md`
5. `07-integration-and-api/05-ocl-evaluation-flow.md`
6. `07-integration-and-api/06-end-to-end-library-demo.md` (geplant)
7. `07-integration-and-api/07-dto-reference.md`
8. `07-integration-and-api/08-error-contract.md`

### Planung und Umsetzung

1. `08-planning/01-overall-implementation-roadmap.md`
2. `08-planning/02-backend-implementation-plan.md`
3. `08-planning/03-frontend-implementation-plan.md`
4. `08-planning/04-mvp-milestones.md` (geplant)
5. `08-planning/05-iteration-plan.md` (geplant)
6. `08-planning/06-risks-and-open-questions.md` (geplant)
7. `08-planning/07-post-mvp-roadmap.md` (geplant)
8. `08-planning/08-issue-backlog.md` (geplant)
9. `08-planning/09-definition-of-done.md` (geplant)

### OCL-Erweiterung nach dem MVP

1. `OCL-specification.pdf` im Workspace-Root (normative Primärquelle, OMG OCL 2.4)
2. `09-ocl-extension-analysis/01-ocl-current-state.md`
3. `09-ocl-extension-analysis/02-ocl-extension-scope.md`
4. `09-ocl-extension-analysis/03-ocl-feature-prioritization.md`
5. `09-ocl-extension-analysis/04-collection-operations.md`
6. `09-ocl-extension-analysis/05-iterator-expressions.md`
7. `09-ocl-extension-analysis/06-let-if-allinstances.md`
8. `09-ocl-extension-analysis/07-pre-post-derived-init.md`
9. `09-ocl-extension-analysis/08-ocl-error-handling-and-source-locations.md`
10. `09-ocl-extension-analysis/09-ocl-test-strategy.md`
11. `09-ocl-extension-analysis/10-ocl-extension-roadmap.md`

## Verwendung der neuen UI-Screenshots

Die Screenshots unter `assets/screenshots/` sind visuelle Referenz für das Zielsystem.

Sie werden verwendet für:

- User Journey,
- UI-Anforderungen,
- Komponentenmapping,
- MVP-Scope,
- Akzeptanzkriterien,
- End-to-End-Demo.

Besonders relevante Auswertungsdateien sind:

| Datei | Rolle bei der Screenshot-Auswertung |
|---|---|
| `02-product-and-user-journey/02-screenshot-based-user-journey.md` | Leitet Nutzerabläufe aus den Screenshots ab. |
| `04-ui-ux-analysis/05-screenshot-traceability.md` | Verknüpft Screenshots mit UI-Anforderungen und Komponenten. |
| `06-frontend-analysis/15-screenshot-implementation-mapping.md` | Übersetzt sichtbare UI-Bereiche in Frontend-Komponenten. |
| `07-integration-and-api/06-end-to-end-library-demo.md` | Geplante Datei; soll die Screenshots als Zielbild für einen durchgängigen Demo-Ablauf nutzen. |

Die Screenshots müssen nicht pixelgenau nachgebaut werden. Sie sind funktionale und strukturelle Referenzen. Entscheidend ist, welche Arbeitsbereiche, Aktionen, Modals, Panels und Fehlerdarstellungen für den MVP erforderlich sind.

## Verwendung des originalen USE-Projekts

Das originale USE-Projekt wird als fachliche Referenz analysiert. Es ist keine Implementierungsgrundlage.

Besonders relevante Dokumentationsbereiche sind:

| Datei oder Bereich | Zweck |
|---|---|
| `01-original-use-reference/*` | Zentrale Analyse von USE-Konzepten, Syntax, Beispielen, Verhalten und Relevanz. |
| `05-backend-analysis/16-original-use-reference-check.md` | Prüft, ob Backend-Entscheidungen fachlich mit der USE-Referenz vereinbar sind. |
| `03-uml-ocl-domain/*` | Nutzt USE als Orientierung für Domänenbegriffe und Validierungskonzepte. |
| `05-backend-analysis/08-ocl-engine-design.md` | Berücksichtigt USE/OCL-Verhalten bei der OCL-Engine-Konzeption. |
| `05-backend-analysis/17-backend-test-strategy.md` | Nutzt USE-Beispiele und Verhaltenserwartungen als Grundlage für Testfälle. |

Das originale USE-Projekt darf genutzt werden für:

- fachliche Orientierung,
- Syntaxanalyse,
- Beispielmodelle,
- Testfälle,
- Verhaltenserwartungen,
- Fehlermeldungsarten,
- Validierungskonzepte.

Es darf nicht genutzt werden als:

- direkte Codebasis,
- Runtime Dependency,
- Fork-Grundlage,
- Verpflichtung zur vollständigen Feature-Parität im MVP.

## Traceability zwischen Dokumenten

Anforderungen sollen im Repository nachvollziehbar bleiben. Eine fachliche oder visuelle Quelle soll bis zu Architektur, API, Umsetzung und Testfall verfolgt werden können.

Beispiel für Screenshot-basierte Traceability:

```text
Screenshot
-> User Journey
-> Functional Requirement
-> UI-Komponente
-> API Contract
-> Backend Service
-> Testfall
-> MVP-Meilenstein
```

Beispiel für USE-basierte Traceability:

```text
Original-USE-Beispiel
-> USE-Verhaltensreferenz
-> Domänenregel
-> Validation Concept
-> Backend Validation Service
-> Error Contract
-> Backend-Testfall
```

Beispiel für OCL-Erweiterungen:

```text
OCL-Sprachfeature
-> OCL Architecture
-> Parser/AST
-> Typechecker
-> Evaluator
-> API Validation Result
-> Frontend Error UI
-> Post-MVP Roadmap
```

Traceability soll nicht nur als formale Verlinkung verstanden werden. Sie soll helfen, spätere Änderungen kontrolliert durch alle betroffenen Dokumente zu ziehen.

## Pflegehinweise

### Object Diagram value editing

| Dokument | Zweck |
|---|---|
| `04-ui-ux-analysis/42-object-diagram-typed-attribute-values.md` | Typgerechte Object-Slot-Editoren sowie Semantik statischer, gespeicherter und abgeleiteter Attribute; visuelle Referenz: `assets/mockups/object-diagram-typed-attribute-values.html`. |
| `04-ui-ux-analysis/43-class-properties-static-attribute-values.md` | Classifier-bezogene Werte statischer Attribute ohne per-Object Slots; visuelle Referenz: `assets/mockups/class-properties-static-attribute-values.html`. |
| `04-ui-ux-analysis/44-class-properties-feature-redefinition.md` | Explizite Redefinition geerbter Attributes und Operations mit stabilen Feature-IDs, Candidate-Picker und Konfliktdiagnosen; visuelle Referenz: `assets/mockups/class-properties-generalizations-and-redefinitions.html`. |
| `04-ui-ux-analysis/45-object-diagram-derived-association-ends.md` | Runtime-Darstellung für `subsets`, `redefines`, derived und union Association Ends mit read-only Ergebnissen und Link-Provenienz; visuelle Referenz: `assets/mockups/object-properties-associations.html`. |
| `04-ui-ux-analysis/47-unified-object-associations.md` | Vereinheitlichte Referenz für `Object Properties -> Associations`: gespeicherte Object Links, editierbare Endbelegungen sowie read-only derived/union Navigation mit Quellen in einem Workflow; visuelle Referenz: `assets/mockups/object-properties-associations.html`. |
| `04-ui-ux-analysis/46-unified-generalizations-and-redefinitions.md` | Offizielle gemeinsame Class-Properties-Referenz für Mehrfachvererbung, Vererbungskette, geerbte Features und explizite Redefinition; visuelle Referenz: `assets/mockups/class-properties-generalizations-and-redefinitions.html`. |

| Änderung | Zu aktualisierende Bereiche |
|---|---|
| Neuer Screenshot | `assets/screenshots/README.md`, `02-product-and-user-journey/02-screenshot-based-user-journey.md`, `04-ui-ux-analysis/05-screenshot-traceability.md`, ggf. `06-frontend-analysis/15-screenshot-implementation-mapping.md` |
| Neue Scope-Entscheidung | `00-overview/02-goals-scope-and-non-goals.md`, betroffene Analysebereiche und Planungsdateien in `08-planning/` |
| API-Änderung | `07-integration-and-api/*`, `05-backend-analysis/13-api-design.md`, `06-frontend-analysis/05-api-client-and-dtos.md`, `07-integration-and-api/07-dto-reference.md` |
| Neues OCL-Feature | `09-ocl-extension-analysis/*`, `03-uml-ocl-domain/03-ocl-architecture-and-extension-strategy.md`, `05-backend-analysis/08-ocl-engine-design.md`, Parser/Typechecker/Evaluator-Dateien, Integrationsdokumente und `08-planning/07-post-mvp-roadmap.md` |
| Neue Validierungsregel | `03-uml-ocl-domain/04-validation-concept.md`, `05-backend-analysis/12-validation-service.md`, `05-backend-analysis/15-error-and-result-model.md`, `07-integration-and-api/08-error-contract.md` |
| Änderung am MVP | `00-overview/02-goals-scope-and-non-goals.md`, `02-product-and-user-journey/04-mvp-scope.md`, `02-product-and-user-journey/05-acceptance-criteria.md`, `08-planning/04-mvp-milestones.md` |
| Neue Architekturentscheidung | Betroffene Architektur-, Backend-, Frontend-, Integrations- und Planungsdateien aktualisieren. |
| Neues Beispielmodell | `03-uml-ocl-domain/05-library-example-model.md`, `07-integration-and-api/06-end-to-end-library-demo.md`, Teststrategien |

Bei Änderungen an zentralen Konzepten sollten immer mindestens Scope, Domänenmodell, API-Vertrag, betroffene Backend-/Frontend-Dokumente und Tests geprüft werden.

## Zusammenfassung

Diese Dokumentationskarte ist der Einstiegspunkt für die Arbeit mit dem Analyse-Repository. Sie zeigt, wo Überblick, USE-Referenz, Produktanforderungen, UML/OCL-Domäne, UI/UX, Backend, Frontend, Integration, Planung, Post-MVP-OCL-Erweiterungen und Screenshot-Assets dokumentiert werden.

Das Repository soll spätere Implementierung nicht ersetzen, sondern vorbereiten. Es macht nachvollziehbar, welche Anforderungen aus dem originalen USE-Projekt, aus den neuen UI-Screenshots und aus dem MVP-Zielbild entstehen und wie diese Anforderungen in Architektur, API, Komponenten, Validierung und Testplanung überführt werden.
