# Backend Repository Structure

## Zweck dieser Datei

Diese Datei schlägt eine sinnvolle Repository-, Build- und Package-Struktur für das neue Backend des webbasierten UML/OCL-Systems vor.

Das Backend soll als eigenständiges Java/Spring-Boot-Projekt entstehen. Es wird nicht in das originale USE-Repository integriert, nicht aus dem alten USE-Code geforkt und verwendet den alten USE-Core nicht als Dependency.

Die Struktur verfolgt vier Ziele:

- klare Trennung zwischen API, Application Services, Domain, OCL, Validation und Persistence,
- testbare Fachlogik ohne UI-Abhängigkeit,
- erweiterbare OCL-Verarbeitung mit Lexer, Parser, AST, Typechecker und Evaluator,
- stabile Grundlage für REST/JSON-Kommunikation mit dem React/TypeScript-Frontend.

## Repository-Ziele

| Ziel | Bedeutung für die Struktur |
|---|---|
| Eigenständiges Backend-Repository | Das Backend besitzt eigenes Build-System, eigene Versionierung und eigene Tests. |
| Spring-Boot-kompatible Struktur | Standardisierte `src/main/java`, `src/main/resources`, `src/test/java` Struktur. |
| Keine USE-Core-Abhängigkeit | Original-USE dient nur als Referenz, nicht als Runtime-Komponente. |
| Domänenorientierte Packages | UML, Snapshot, OCL und Validation werden fachlich getrennt. |
| Erweiterbare OCL-Engine | OCL-Phasen liegen in separaten Packages und sind isoliert testbar. |
| REST/JSON-Vertrag | DTOs und Controller sind klar von Domain-Objekten getrennt. |
| Testbarkeit | Parser, Typechecker, Evaluator, Validation und API können separat getestet werden. |

## Top-Level-Struktur

Vorgeschlagener Name des späteren Backend-Repositories:

```text
use-web-backend/
```

Vorgeschlagene Top-Level-Struktur:

```text
use-web-backend/
├─ README.md
├─ pom.xml
├─ .gitignore
├─ .editorconfig
├─ src/
│  ├─ main/
│  │  ├─ java/
│  │  │  └─ de/example/useweb/backend/
│  │  │     ├─ UseWebBackendApplication.java
│  │  │     ├─ api/
│  │  │     ├─ application/
│  │  │     ├─ domain/
│  │  │     ├─ ocl/
│  │  │     ├─ validation/
│  │  │     ├─ persistence/
│  │  │     ├─ error/
│  │  │     └─ config/
│  │  └─ resources/
│  │     ├─ application.yml
│  │     ├─ application-dev.yml
│  │     ├─ application-test.yml
│  │     └─ examples/
│  └─ test/
│     ├─ java/
│     │  └─ de/example/useweb/backend/
│     │     ├─ api/
│     │     ├─ application/
│     │     ├─ domain/
│     │     ├─ ocl/
│     │     ├─ validation/
│     │     └─ persistence/
│     └─ resources/
│        ├─ fixtures/
│        ├─ ocl/
│        ├─ projects/
│        └─ snapshots/
└─ docs/
   ├─ api.md
   ├─ json-project-format.md
   └─ validation-errors.md
```

Der Package-Prefix `de/example/useweb/backend` ist ein Platzhalter. Im späteren Implementierungsrepository sollte er durch den tatsächlichen Organisations- oder Projektpackage-Namen ersetzt werden.

## Build-System

Für den MVP ist Maven naheliegend, weil das Zielsystem Java/Spring Boot ist und das originale USE-Projekt ebenfalls Maven verwendet. Das ist aber keine technische Kopplung an USE.

Empfohlene Maven-Struktur:

```text
use-web-backend/
├─ pom.xml
└─ src/
   ├─ main/
   │  ├─ java/
   │  └─ resources/
   └─ test/
      ├─ java/
      └─ resources/
```

Alternative: Gradle ist ebenfalls möglich, falls das Team Gradle bevorzugt. Für die Analyse wird Maven als Default angenommen.

| Entscheidung | Empfehlung | Begründung |
|---|---|---|
| Build-System | Maven | Gute Spring-Boot-Unterstützung, einfache CI-Integration, vertraut im Java-Umfeld. |
| Modulstruktur im MVP | Single Module | Der MVP bleibt überschaubar; Package-Grenzen reichen zunächst aus. |
| Multi-Modul später | Optional | Bei wachsender OCL-Engine oder separater API-Library denkbar. |
| Java-Version | Aktuelle LTS-Version | Sollte mit Spring-Boot-Zielversion abgestimmt werden. |
| Testframework | JUnit 5 | Standard für Spring Boot und fachliche Unit-Tests. |
| API-Tests | Spring MockMvc oder WebTestClient | Endpunkte ohne externes Frontend testbar machen. |

## Package-Struktur

Vorgeschlagene Java-Package-Struktur:

```text
de.example.useweb.backend/
├─ api/
│  ├─ controller/
│  ├─ dto/
│  └─ mapper/
├─ application/
│  ├─ project/
│  ├─ uml/
│  ├─ snapshot/
│  ├─ ocl/
│  ├─ modeltext/
│  └─ validation/
├─ domain/
│  ├─ project/
│  ├─ uml/
│  ├─ snapshot/
│  ├─ ocl/
│  ├─ validation/
│  └─ layout/
├─ ocl/
│  ├─ lexer/
│  ├─ parser/
│  ├─ ast/
│  ├─ typecheck/
│  ├─ evaluation/
│  ├─ diagnostics/
│  └─ value/
├─ modeltext/
│  ├─ parser/
│  ├─ mapping/
│  └─ diagnostics/
├─ validation/
│  ├─ model/
│  ├─ rules/
│  ├─ result/
│  └─ service/
├─ persistence/
│  ├─ project/
│  ├─ json/
│  └─ repository/
├─ error/
└─ config/
```

Diese Struktur trennt bewusst fachliche Domäne, Anwendungsfälle, technische API-DTOs, OCL-Engine und Validierungslogik.

## API-Schicht

Die API-Schicht ist die REST/JSON-Grenze zum Frontend. Sie sollte keine fachliche Logik enthalten, sondern Requests validieren, DTOs mappen und Application Services aufrufen.

```text
api/
├─ controller/
│  ├─ ProjectController.java
│  ├─ UmlModelController.java
│  ├─ SnapshotController.java
│  ├─ OclController.java
│  └─ ValidationController.java
├─ dto/
│  ├─ project/
│  ├─ uml/
│  ├─ snapshot/
│  ├─ ocl/
│  ├─ validation/
│  └─ common/
└─ mapper/
   ├─ ProjectDtoMapper.java
   ├─ UmlDtoMapper.java
   ├─ SnapshotDtoMapper.java
   ├─ OclDtoMapper.java
   └─ ValidationDtoMapper.java
```

| API-Bestandteil | Aufgabe |
|---|---|
| Controller | HTTP-Endpunkte bereitstellen. |
| DTOs | JSON-Vertrag mit dem Frontend definieren. |
| Mapper | DTOs in Domain/Application-Objekte übersetzen. |
| Request Validation | Pflichtfelder, einfache Formatregeln und JSON-Struktur prüfen. |
| Error Response Handling | Fachliche und technische Fehler in einheitliche API-Fehler übersetzen. |

DTOs sollten nicht identisch mit Domain-Objekten sein. Das hält die API stabiler und erlaubt interne Domänenänderungen ohne sofortige Frontend-Breaks.

## Application-Schicht

Die Application-Schicht koordiniert Anwendungsfälle. Sie enthält keine HTTP-Details und sollte auch keine UI-Annahmen treffen.

```text
application/
├─ project/
│  ├─ ProjectApplicationService.java
│  ├─ CreateProjectCommand.java
│  └─ SaveProjectCommand.java
├─ uml/
│  ├─ UmlModelApplicationService.java
│  ├─ CreateClassCommand.java
│  ├─ UpdateClassCommand.java
│  ├─ CreateAssociationCommand.java
│  └─ UpdateAssociationCommand.java
├─ snapshot/
│  ├─ SnapshotApplicationService.java
│  ├─ CreateObjectCommand.java
│  ├─ UpdateSlotCommand.java
│  └─ CreateObjectLinkCommand.java
├─ ocl/
│  ├─ OclApplicationService.java
│  ├─ CreateInvariantCommand.java
│  └─ CheckOclExpressionCommand.java
└─ validation/
   ├─ ValidationApplicationService.java
   └─ CheckConstraintsCommand.java
```

| Service | Hauptaufgabe |
|---|---|
| `ProjectApplicationService` | Projektlebenszyklus koordinieren. |
| `UmlModelApplicationService` | Klassenmodell-Operationen ausführen. |
| `SnapshotApplicationService` | Objektmodell/Snapshot-Operationen ausführen. |
| `OclApplicationService` | Invarianten und OCL-Diagnosen koordinieren. |
| `ValidationApplicationService` | vollständigen Constraint Check auslösen. |

## Domain-Schicht

Die Domain-Schicht enthält das fachliche Modell des neuen Systems. Sie ist unabhängig von Spring MVC, JSON-DTOs und Datenbankdetails.

```text
domain/
├─ project/
│  ├─ Project.java
│  └─ ProjectId.java
├─ uml/
│  ├─ UmlModel.java
│  ├─ UmlClass.java
│  ├─ UmlAttribute.java
│  ├─ UmlOperation.java
│  ├─ UmlParameter.java
│  ├─ UmlAssociation.java
│  ├─ UmlAssociationEnd.java
│  ├─ Multiplicity.java
│  └─ PrimitiveType.java
├─ snapshot/
│  ├─ ObjectModel.java
│  ├─ Snapshot.java
│  ├─ ObjectInstance.java
│  ├─ Slot.java
│  └─ ObjectLink.java
├─ ocl/
│  ├─ UmlInvariant.java
│  └─ OclExpression.java
├─ validation/
│  ├─ ValidationResult.java
│  ├─ ValidationError.java
│  ├─ ValidationSeverity.java
│  └─ ValidationErrorCode.java
└─ layout/
   ├─ LayoutInformation.java
   ├─ NodeLayout.java
   └─ EdgeLayout.java
```

Wichtige Prinzipien:

- Domain-Objekte besitzen stabile IDs.
- UML-Modell und Snapshot bleiben getrennt.
- OCL-Invarianten gehören fachlich zum UML-Modell.
- Validation Results referenzieren Domain-Elemente über IDs.
- Layoutdaten sind UI-relevant, aber keine fachliche UML-Semantik.

## OCL-Schicht

Die OCL-Schicht ist bewusst separat von der allgemeinen Validation-Schicht. Sie bildet die interne OCL-Pipeline ab.

```text
ocl/
├─ lexer/
│  ├─ OclLexer.java
│  ├─ OclToken.java
│  ├─ OclTokenType.java
│  └─ SourcePosition.java
├─ parser/
│  ├─ OclParser.java
│  ├─ ParseResult.java
│  └─ OclParseException.java
├─ ast/
│  ├─ OclAstNode.java
│  ├─ SelfExpression.java
│  ├─ PropertyAccessExpression.java
│  ├─ NavigationExpression.java
│  ├─ CollectionOperationExpression.java
│  ├─ LiteralExpression.java
│  ├─ ComparisonExpression.java
│  └─ BooleanExpression.java
├─ typecheck/
│  ├─ OclTypeChecker.java
│  ├─ TypeEnvironment.java
│  ├─ TypedExpression.java
│  └─ OclTypeError.java
├─ evaluation/
│  ├─ OclEvaluator.java
│  ├─ EvaluationContext.java
│  ├─ EvaluationResult.java
│  └─ OclEvaluationError.java
├─ diagnostics/
│  ├─ OclDiagnostic.java
│  ├─ OclDiagnosticCode.java
│  └─ OclDiagnosticSeverity.java
└─ value/
   ├─ OclValue.java
   ├─ BooleanValue.java
   ├─ IntegerValue.java
   ├─ RealValue.java
   ├─ StringValue.java
   ├─ ObjectValue.java
   └─ CollectionValue.java
```

Diese Struktur unterstützt das MVP-Subset und spätere Erweiterungen:

| OCL-Phase | MVP-Aufgabe | Erweiterbarkeit |
|---|---|---|
| Lexer | Tokens für `self`, Literale, Operatoren, Navigation und Collection-Aufrufe erzeugen. | Neue Keywords und Symbole ergänzen. |
| Parser | AST für einfache Ausdrücke erzeugen. | Neue Grammatikregeln und AST-Knoten ergänzen. |
| AST | Ausdrucksstruktur repräsentieren. | Iterator-, Let- und If-Knoten hinzufügen. |
| Typechecker | Attribute, Rollen, Operatoren und Rückgabetypen prüfen. | OCL-Typsystem erweitern. |
| Evaluator | Ausdruck gegen Snapshot auswerten. | Iteratoren, allInstances und komplexe Collections ergänzen. |
| Diagnostics | Syntax-, Typ- und Evaluationsfehler strukturieren. | Source Ranges und Quick Fixes ergänzen. |

## Validation-Schicht

Die Validation-Schicht orchestriert fachliche Prüfungen. Sie nutzt Domain-Objekte und bei OCL-Invarianten die OCL-Schicht.

```text
validation/
├─ service/
│  ├─ ValidationService.java
│  └─ ConstraintCheckService.java
├─ rules/
│  ├─ UmlStructureValidator.java
│  ├─ SnapshotValidator.java
│  ├─ SlotValueValidator.java
│  ├─ ObjectLinkValidator.java
│  ├─ MultiplicityValidator.java
│  └─ InvariantValidator.java
├─ model/
│  ├─ ValidationContext.java
│  └─ ValidationTarget.java
└─ result/
   ├─ ValidationResultBuilder.java
   └─ ValidationErrorFactory.java
```

| Validator | Aufgabe |
|---|---|
| `UmlStructureValidator` | Klassen, Attribute, Typen, Associations und Invarianten strukturell prüfen. |
| `SnapshotValidator` | Objekte, Slots und Referenzen auf das UML-Modell prüfen. |
| `SlotValueValidator` | Attributwerte gegen Attributtypen prüfen. |
| `ObjectLinkValidator` | Objektlinks gegen Associations und Klassentypen prüfen. |
| `MultiplicityValidator` | Linkanzahlen gegen Association-End-Multiplizitäten prüfen. |
| `InvariantValidator` | OCL-Invarianten gegen alle Objekte der Kontextklasse auswerten. |

## Persistence-Schicht

Im MVP kann Persistenz bewusst einfach bleiben. Das Ziel ist ein JSON-basiertes Projektformat, das später durch Datenbankpersistenz ergänzt werden kann.

```text
persistence/
├─ project/
│  ├─ ProjectRepository.java
│  ├─ InMemoryProjectRepository.java
│  └─ FileProjectRepository.java
├─ json/
│  ├─ ProjectJsonReader.java
│  ├─ ProjectJsonWriter.java
│  ├─ ProjectJsonSchemaValidator.java
│  └─ ProjectJsonVersion.java
└─ repository/
   └─ RepositoryException.java
```

| Persistenztyp | MVP-Eignung | Bemerkung |
|---|---|---|
| In-Memory | Gut für frühe Entwicklung und Tests. | Nicht ausreichend für reale Nutzung. |
| File-basiertes JSON | Gut für MVP und Import/Export. | Passt zum JSON-Projektformat. |
| Datenbank | Post-MVP oder späterer MVP-Ausbau. | Sinnvoll bei Multi-User, Versionierung oder größerer Persistenz. |

## Error Handling

Fehlerbehandlung sollte zentralisiert werden, damit API-Antworten konsistent bleiben.

```text
error/
├─ ApiErrorResponse.java
├─ ApiErrorCode.java
├─ GlobalExceptionHandler.java
├─ DomainException.java
├─ NotFoundException.java
├─ ConflictException.java
├─ ValidationException.java
└─ InvalidProjectFormatException.java
```

| Fehlerart | Beispiel | API-Ziel |
|---|---|---|
| API-Fehler | ungültiger Request Body | verständliche HTTP-Fehlerantwort |
| Domain-Fehler | Klasse existiert nicht | strukturierter fachlicher Fehler |
| Projektformatfehler | JSON nicht kompatibel | Import/Load-Fehler mit Details |
| OCL-Fehler | Syntax- oder Typecheck-Problem | OCL Diagnostic oder Validation Error |
| Constraint-Fehler | Invariantverletzung | Teil von `ValidationResult`, kein technischer Exception-Fall |

Constraint-Verletzungen sollten nicht als technische Exceptions behandelt werden. Sie sind fachliche Validierungsergebnisse.

## Teststruktur

Tests sollten die Schichten getrennt prüfen und später End-to-End-Flows absichern.

```text
src/test/java/de/example/useweb/backend/
├─ api/
│  ├─ ProjectControllerTest.java
│  ├─ UmlModelControllerTest.java
│  ├─ SnapshotControllerTest.java
│  └─ ValidationControllerTest.java
├─ application/
│  ├─ ProjectApplicationServiceTest.java
│  ├─ UmlModelApplicationServiceTest.java
│  └─ ValidationApplicationServiceTest.java
├─ domain/
│  ├─ uml/
│  └─ snapshot/
├─ ocl/
│  ├─ lexer/
│  │  └─ OclLexerTest.java
│  ├─ parser/
│  │  └─ OclParserTest.java
│  ├─ typecheck/
│  │  └─ OclTypeCheckerTest.java
│  └─ evaluation/
│     └─ OclEvaluatorTest.java
├─ validation/
│  ├─ MultiplicityValidatorTest.java
│  ├─ InvariantValidatorTest.java
│  └─ ConstraintCheckServiceTest.java
└─ persistence/
   └─ ProjectJsonRoundTripTest.java
```

Testressourcen:

```text
src/test/resources/
├─ fixtures/
│  ├─ library-valid.project.json
│  ├─ library-invalid-max-books.project.json
│  └─ invalid-link.project.json
├─ ocl/
│  ├─ valid-expressions.txt
│  ├─ syntax-errors.txt
│  └─ type-errors.txt
├─ projects/
│  └─ minimal-project.json
└─ snapshots/
   ├─ valid-snapshot.json
   └─ invalid-multiplicity-snapshot.json
```

| Testtyp | Ziel |
|---|---|
| Unit-Tests | Lexer, Parser, Typechecker, Evaluator und einzelne Validatoren isoliert prüfen. |
| Application-Service-Tests | Anwendungsfälle ohne HTTP testen. |
| API-Tests | REST-Verträge und JSON-Strukturen prüfen. |
| Roundtrip-Tests | Projekt speichern und laden ohne Informationsverlust prüfen. |
| Regressionstests | Aus Original-USE-Beispielen abgeleitete Fälle absichern. |

## Beispielhafte Klassenübersicht

| Package | Beispielklasse | Zweck |
|---|---|---|
| `api.controller` | `ValidationController` | REST-Endpunkt für `Check Constraints`. |
| `api.dto.validation` | `ValidationResultDto` | JSON-Antwort für Validierungsergebnisse. |
| `api.mapper` | `ValidationDtoMapper` | Mapping von Domain Result zu DTO. |
| `application.validation` | `ValidationApplicationService` | Koordiniert den Constraint Check. |
| `domain.uml` | `UmlClass` | Fachliche UML-Klasse. |
| `domain.snapshot` | `ObjectInstance` | Objekt im Snapshot. |
| `domain.ocl` | `UmlInvariant` | OCL-Invariante mit Kontextklasse. |
| `domain.validation` | `ValidationError` | Strukturierter fachlicher Fehler. |
| `ocl.lexer` | `OclLexer` | Zerlegt OCL-Text in Tokens. |
| `ocl.parser` | `OclParser` | Erzeugt AST aus Tokens. |
| `ocl.ast` | `ComparisonExpression` | AST-Knoten für Vergleichsausdrücke. |
| `ocl.typecheck` | `OclTypeChecker` | Prüft OCL-Ausdrücke gegen UML-Modell. |
| `ocl.evaluation` | `OclEvaluator` | Wertet OCL gegen Snapshot aus. |
| `modeltext.parser` | `ModelTextParser` | Erkennt vollständigen USE-ähnlichen Editor-Text für das unterstützte MVP-Subset. |
| `modeltext.mapping` | `ModelTextToDomainMapper` | Überführt erkannte Klassen, Associations und Invarianten in das Domain-Modell. |
| `validation.rules` | `MultiplicityValidator` | Prüft Linkanzahlen gegen Multiplizitäten. |
| `persistence.json` | `ProjectJsonReader` | Lädt MVP-Projektformat. |
| `error` | `GlobalExceptionHandler` | Einheitliche API-Fehlerantworten. |

## Abgrenzung zum originalen USE-Projekt

Die vorgeschlagene Struktur orientiert sich fachlich an USE-Konzepten, übernimmt aber keine technische Struktur aus dem Originalprojekt.

| Original-USE-Bereich | Neue Struktur | Abgrenzung |
|---|---|---|
| `org.tzi.use.uml.mm` | `domain.uml` | Fachliche Konzepte ähnlich, Implementierung neu. |
| `org.tzi.use.uml.sys` | `domain.snapshot` | Snapshot-Konzepte ähnlich, Datenmodell neu. |
| `org.tzi.use.uml.ocl.expr` | `ocl.ast`, `ocl.evaluation` | OCL-Verarbeitung neu und MVP-begrenzt. |
| `org.tzi.use.uml.ocl.type` | `ocl.typecheck`, `ocl.value` | Typsystem neu und für Erweiterung vorbereitet. |
| `org.tzi.use.parser.ocl` | `ocl.lexer`, `ocl.parser` | OCL-Ausdrucksparser neu; USE-Grammatiken nur Referenz. |
| `org.tzi.use.parser.use` | `modeltext.parser` | Referenz für vollständige Modelltextsyntax; MVP unterstützt nur ein begrenztes Subset. |
| `use-gui` | kein Backend-Package | Desktop-GUI wird nicht migriert. |
| `.use` Dateien | `modeltext` + `persistence.json` im MVP | Editor kann vollständigen Text zeigen; vollständiger `.use` Import/Export erst Post-MVP. |

Diese Abgrenzung ist wichtig: Das neue Backend ist kein Fork, kein technischer Wrapper um USE und keine Migration des alten Cores. USE bleibt Referenz für Konzepte, Syntax, Verhalten und Testfälle.

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Maven oder Gradle final? | Beeinflusst Build-Dateien, CI und Plugin-Auswahl. |
| Single Module oder Multi Module ab Projektstart? | Single Module ist pragmatischer; Multi Module kann spätere Grenzen erzwingen. |
| Welcher finale Java-Package-Prefix wird verwendet? | Muss vor Implementierungsstart festgelegt werden. |
| Wird Persistenz im MVP file-basiert oder datenbankbasiert umgesetzt? | Beeinflusst `persistence` Package und Tests. |
| Wird ein Parsergenerator verwendet oder ein handgeschriebener Parser für das MVP-Subset? | Beeinflusst `ocl.parser` und Build-System. |
| Werden DTOs versioniert, z. B. unter `api.v1`? | Relevant für langfristige API-Stabilität. |
| Soll Layout als Teil des Projektformats oder als separate UI-Metadaten gespeichert werden? | Beeinflusst Domain- und Persistence-Struktur. |

## Zusammenfassung

Das neue Backend sollte als eigenständiges Spring-Boot-Repository mit klarer Schichtung entstehen. Die vorgeschlagene Struktur trennt API, Application Services, Domain, OCL, Validation, Persistence, Error Handling und Configuration.

Besonders wichtig ist die separate OCL-Schicht. Lexer, Parser, AST, Typechecker und Evaluator werden bewusst getrennt, damit das MVP-Subset stabil implementiert und später erweitert werden kann. Die Struktur bleibt fachlich von USE inspiriert, übernimmt aber weder den alten USE-Core noch dessen Desktop-GUI oder Projektstruktur.
