# OCL Current State

## Zweck dieser Datei

Diese Datei dokumentiert den aktuellen Stand der OCL-Unterstützung im neuen webbasierten UML/OCL-System. Sie trennt zwischen:

- dem in den Analysedokumenten definierten MVP,
- dem im Repository tatsächlich implementierten Stand,
- bereits vorgesehenen, aber noch nicht umgesetzten Bestandteilen,
- Post-MVP-Features,
- offenen oder widersprüchlich dokumentierten Punkten.

Die Datei ist eine Bestandsaufnahme. Sie priorisiert und entwirft keine neuen OCL-Features. Die spätere Erweiterungsplanung erfolgt in den nachfolgenden Dateien unter `09-ocl-extension-analysis/`.

Stand der Prüfung: 12. August 2026.

### Statuslegende

| Status | Bedeutung |
|---|---|
| `implementiert` | Im Quellcode des neuen Backends oder Frontends vorhanden und durch Code beziehungsweise Tests belegbar. |
| `geplant` | In den Analysedokumenten für den MVP vorgesehen, im geprüften Quellcode aber nicht vollständig belegt. |
| `offen` | Eine Entscheidung oder Umsetzung steht ausdrücklich aus. |
| `Post-MVP` | Bewusst nicht Teil des MVP; Gegenstand späterer Erweiterungsanalysen. |
| `unklar` | Dokumentation und Implementierung sind nicht eindeutig oder stimmen nicht vollständig überein. |

## Analysequellen

### Zentrale Analysedokumente

Ausgangspunkt war die Navigationskarte `00-overview/03-documentation-map.md`. Für die Bestandsaufnahme wurden insbesondere folgende Dokumente herangezogen:

| Bereich | Relevante Dateien | Beitrag zur Bestandsaufnahme |
|---|---|---|
| UML/OCL-Domäne | `03-uml-ocl-domain/01-uml-ocl-scope.md`, `03-uml-ocl-domain/03-ocl-architecture-and-extension-strategy.md`, `03-uml-ocl-domain/04-validation-concept.md` | MVP-Subset, Pipeline, Validierungssemantik und Post-MVP-Abgrenzung. |
| Backend | `05-backend-analysis/08-ocl-engine-design.md` bis `05-backend-analysis/12-validation-service.md`, `05-backend-analysis/15-error-and-result-model.md`, `05-backend-analysis/17-backend-test-strategy.md` | Geplante Komponenten, AST, Typregeln, Evaluator, Fehler- und Testmodell. |
| Integration/API | `07-integration-and-api/01-frontend-backend-contract.md`, `03-validation-flow.md`, `05-ocl-evaluation-flow.md`, `07-dto-reference.md`, `08-error-contract.md` | REST-Endpunkte, DTOs, Validation Results und UI-Mapping. |
| Planung | `08-planning/01-overall-implementation-roadmap.md`, `02-backend-implementation-plan.md`, `03-frontend-implementation-plan.md` | MVP-Umsetzungsreihenfolge und Post-MVP-Abgrenzung. |

Die in der Documentation Map als geplant markierten Dateien `03-uml-ocl-domain/05-library-example-model.md`, `07-integration-and-api/06-end-to-end-library-demo.md` und `08-planning/07-post-mvp-roadmap.md` wurden im Dateisystem nicht gefunden.

### Geprüfter Implementierungsstand

Die Bestandsaufnahme beruht zusätzlich auf dem aktuellen Quellcode in:

- `use-web-backend/src/main/java/de/useweb/backend/ocl/`
- `use-web-backend/src/main/java/de/useweb/backend/validation/`
- `use-web-backend/src/main/java/de/useweb/backend/api/`
- `use-web-backend/src/test/java/de/useweb/backend/ocl/`
- `use-web-backend/src/test/java/de/useweb/backend/validation/`
- `use-web-backend/src/test/java/de/useweb/backend/api/`
- `use-web-frontend/src/features/ocl-editor/`
- `use-web-frontend/src/pages/workspace/components/`
- `use-web-frontend/src/api/`

Damit ist diese Datei genauer als eine reine Wiedergabe der Planungsdokumente: Mehrere dort noch als geplant oder `Should` bezeichnete Bestandteile sind im aktuellen Repository bereits implementiert.

## Aktueller OCL-Scope

OCL wird im neuen System als eigene Backend-Komponente umgesetzt. Das Backend ist die autoritative Stelle für Syntax, Typprüfung, Evaluation und Constraint Validation. Das Frontend erfasst OCL-Text, ruft Backend-Funktionen auf und visualisiert Diagnosen sowie Validation Results.

Der aktuelle Scope konzentriert sich auf Klasseninvarianten gegen einen einzelnen gespeicherten Objekt-Snapshot:

```text
UML-Klassenmodell + OCL-Invarianten + Objekt-Snapshot
                         |
                         v
              Check Constraints / Validate
                         |
                         v
                  Validation Result
```

Aktuell nicht Ziel ist eine Wiederverwendung des originalen USE-Cores. Der geprüfte Backend-Code liegt vollständig in eigenen Packages unter `de.useweb.backend`. Eine Dependency auf `org.tzi.use` wurde im neuen OCL-Quellcode nicht gefunden.

## Geplante OCL-Pipeline

Die Analysedokumente legen folgende Architektur verbindlich fest:

```text
OCL Text
  -> Lexer
  -> Parser
  -> AST
  -> Type Checker
  -> Evaluator
  -> Validation Result
```

Diese Pipeline ist im aktuellen Backend nicht nur geplant, sondern in ihren Kernstufen implementiert:

| Stufe | Aktueller Backend-Stand | Status | Zentrale Pfade |
|---|---|---|---|
| OCL Text | In `OclExpression` und `UmlInvariant` gespeichert; außerdem Teil des Modelltexts. | `implementiert` | `use-web-backend/src/main/java/de/useweb/backend/domain/ocl/OclExpression.java`, `domain/uml/UmlInvariant.java` |
| Lexer | Eigener Lexer mit Tokens und lexikalischen Diagnosen. | `implementiert` | `ocl/lexer/OclLexer.java`, `OclToken.java`, `OclTokenType.java`, `OclLexResult.java` |
| Parser | Eigener rekursiver Parser erzeugt AST und Syntaxdiagnosen. | `implementiert` | `ocl/parser/OclParser.java`, `OclParseResult.java` |
| AST | Eigene, source-range-fähige AST-Knoten für das MVP-Subset. | `implementiert` | `ocl/ast/` |
| Type Checker | Prüft Invarianten gegen Kontextklasse, Attribute, Rollen, Operatoren und Collection-Typen. | `implementiert` | `ocl/typecheck/OclTypeChecker.java`, `OclType.java`, `TypeEnvironment.java` |
| Evaluator | Wertet AST gegen UML-Modell, Objektmodell und `self` aus. | `implementiert` | `ocl/evaluation/OclEvaluator.java`, `EvaluationContext.java`, `ocl/value/` |
| Validation Result | Invariantenvalidator überführt Syntax-, Typ-, Evaluationsfehler und `false` in Validation Errors. | `implementiert` | `validation/rules/OclInvariantValidator.java`, `validation/service/ValidationService.java`, `domain/validation/` |

### Abweichungen zwischen Zielbild und Implementierung

| Thema | Analyse-Zielbild | Aktueller Befund | Status |
|---|---|---|---|
| Typed AST | Typechecker ergänzt oder erzeugt typisierte AST-Informationen. | `OclTypecheckResult` liefert einen Ergebnistyp; persistente Typannotationen am AST wurden nicht gefunden. | `unklar` |
| AST-Persistenz | In Analysen offen: speichern oder pro Prüfung neu erzeugen. | Parser wird bei Typecheck, Evaluate und Validation erneut ausgeführt; AST wird nicht im Projektformat gespeichert. | `implementiert` als aktuelles Verhalten, Architekturentscheidung noch nicht ausdrücklich geschlossen |
| Dependency Injection | OCL-Komponenten als klar getrennte Services. | `OclController` und `OclInvariantValidator` erzeugen Parser, Typechecker und Evaluator teilweise direkt mit `new`. | `unklar` hinsichtlich langfristiger Zielarchitektur |
| Lexer im Validation Flow | Pipeline nennt Lexer explizit. | Der Validator ruft `OclParser` auf; der Parser kapselt die lexikalische Verarbeitung. | `implementiert` |

## Aktuelle MVP-Features

### Feature-Matrix

| OCL-Feature | MVP laut Analyse | Aktuelles Backend | Beleg/Bemerkung | Status |
|---|---:|---:|---|---|
| `self` | Ja | Ja | `SelfExpression`, Token `SELF`, Typechecking und Evaluation gegen Kontextobjekt. | `implementiert` |
| Attributzugriff, z. B. `self.books` | Ja | Ja | `PropertyAccessExpression`; Auflösung gegen UML-Attribute und Snapshot-Slots. | `implementiert` |
| einfache Association Navigation | Ja | Ja | Property-Auflösung kann Association-Rollen finden; Evaluator traversiert Objektlinks. | `implementiert` |
| String-Literal | Ja | Ja | `LiteralType.STRING`, `StringValue`. | `implementiert` |
| Integer-Literal | Ja | Ja | `LiteralType.INTEGER`, `IntegerValue`. | `implementiert` |
| Real-Literal | Ja | Ja | `LiteralType.REAL`, `RealValue`. | `implementiert` |
| Boolean-Literal | Ja | Ja | `LiteralType.BOOLEAN`, `BooleanValue`. | `implementiert` |
| `=`, `<>` | Ja | Ja | `BinaryOperator.EQUAL`, `NOT_EQUAL`; Typechecker und Evaluator vorhanden. | `implementiert` |
| `<`, `<=`, `>`, `>=` | Ja | Ja | Numerische Vergleichsregeln und Evaluation vorhanden. | `implementiert` |
| `and`, `or` | Ja | Ja | Lexer, AST, Typechecker und Evaluator vorhanden. | `implementiert` |
| `not` | Ja | Ja | `UnaryExpression`/`UnaryOperator`; Typprüfung und Evaluation vorhanden. | `implementiert` |
| Klammern | Ja | Ja | `ParenthesizedExpression`. | `implementiert` |
| `size()` | Ja | Ja | `CollectionOperation.SIZE`. | `implementiert` |
| `isEmpty()` | Ja | Ja | `CollectionOperation.IS_EMPTY`. | `implementiert` |
| `notEmpty()` | Ja | Ja | `CollectionOperation.NOT_EMPTY`. | `implementiert` |
| Klasseninvariante muss Boolean ergeben | Ja | Ja | `checkInvariant` prüft den Ergebnistyp. | `implementiert` |
| Auswertung für alle Objekte der Kontextklasse | Ja | Ja | `OclInvariantValidator` filtert Snapshot-Objekte nach `contextClassId`. | `implementiert` |
| deaktivierte Invarianten überspringen | Vorgesehen | Ja | `OclInvariantValidator` prüft `invariant.enabled()`. | `implementiert` |

### Grenzen der vorhandenen MVP-Implementierung

- Die Collection-Repräsentation ist generisch genug für navigierte Objektmengen, unterscheidet im geprüften Typmodell aber noch nicht sichtbar zwischen `Set`, `Bag`, `Sequence` und `OrderedSet`.
- Association Navigation basiert auf Rollen und Objektlinks. Qualifier, Association Classes, n-äre Associations, Vererbung und komplexere Navigationssemantik wurden nicht als implementiert gefunden.
- Numerische Vergleiche unterstützen Integer/Real. Eine vollständige OCL-Zahlen- und Undefined-Semantik ist nicht belegt.
- Boolean `and` und `or` werden im Evaluator als Java-Boolean-Verknüpfungen ausgewertet. Eine vollständige OCL-Logik mit `invalid`/`null` wurde nicht gefunden.
- Collection-Operationen werden mit Klammern geparst. Eine akzeptierte USE-nahe Kurzform wie `->size` ist nicht belegt.

## Aktuelle Nicht-Features

Die folgenden Features gehören nicht zum aktuellen MVP-Subset. Im geprüften neuen Backend wurden dafür weder Lexer-/Parser-Unterstützung noch passende AST-Knoten, Typregeln und Evaluatorpfade gefunden.

| Featuregruppe | Features | Aktueller Status |
|---|---|---|
| Erweiterte Collection Operations | `includes`, `excludes`, `including`, `excluding`, `count`, `union`, `intersection` | `Post-MVP` |
| Iterator Expressions | `forAll`, `exists`, `select`, `reject`, `collect`, `any`, `one` | `Post-MVP` |
| Lokale/bedingte Ausdrücke | `let`, `if-then-else` | `Post-MVP` |
| Modellweite Instanzen | `allInstances` | `Post-MVP` |
| Operation Contracts | Preconditions, Postconditions, `@pre`, `result` | `Post-MVP` |
| Abgeleitete Werte | derived Attributes, init Values | `Post-MVP` |
| OCL-Collection-Arten | `Set`, `Bag`, `Sequence`, `OrderedSet` mit vollständiger Semantik | `Post-MVP` beziehungsweise `nicht gefunden` |
| Vollständige OCL-Wertsemantik | `null`, `invalid`, Undefined-Propagation und vierwertige Boolean-Logik | `offen` |
| Erweiterte Modellierung | Vererbung/Subtyping, Enumerationen, Tupel, benutzerdefinierte OCL-Operationen | `Post-MVP` beziehungsweise `nicht gefunden` |
| Komfortfunktionen | Autocomplete, Quick Fixes, Formatter, AST-Visualisierung | `Post-MVP` beziehungsweise `nicht gefunden` |

`nicht gefunden` bedeutet hier: Im geprüften neuen Backend wurde keine belastbare Implementierung identifiziert. Es bedeutet nicht, dass das originale USE-Projekt das Feature nicht unterstützt.

## Backend-Komponenten

### OCL-Kern

| Komponente | Aufgabe | Aktueller Stand | Status |
|---|---|---|---|
| `OclLexer` | Text in Tokens und lexikalische Diagnosen zerlegen. | Vorhanden; Tokens enthalten Source Ranges. | `implementiert` |
| `OclParser` | Tokens in AST überführen und Syntaxfehler melden. | Vorhanden; unterstützt MVP-Ausdrücke und Operatorpräzedenz. | `implementiert` |
| `ocl.ast.*` | Ausdrucksstruktur modellieren. | Separate Knoten für self, Literale, Property Access, Collection Operation, unary/binary und Klammern. | `implementiert` |
| `OclTypeChecker` | AST gegen UML-Modell und Kontextklasse prüfen. | Vorhanden; enthält Klassen-, Primitive- und Collection-Typen. | `implementiert` |
| `OclEvaluator` | AST gegen Snapshot und Kontextobjekt auswerten. | Vorhanden; unterstützt Slots, Links, MVP-Operatoren und Collection-Grundoperationen. | `implementiert` |
| `ocl.value.*` | Laufzeitwerte kapseln. | Boolean, Integer, Real, String, Object und Collection vorhanden. | `implementiert` |
| `OclDiagnostic` | Phasenspezifische OCL-Diagnose. | Code, Severity, Meldung, Source Range sowie Expected/Actual vorhanden. | `implementiert` |
| `OclParseService` | Parse-Use-Case und AST-Darstellung für API/Modelltext koordinieren. | Vorhanden. | `implementiert` |

### Validation und Domänenintegration

| Komponente | OCL-Bezug | Status |
|---|---|---|
| `UmlInvariant` | Speichert Name, Kontextklasse, Ausdruck und Enabled-Status. | `implementiert` |
| `OclInvariantValidator` | Parse -> Typecheck -> Evaluation pro Kontextobjekt; erzeugt Validation Errors. | `implementiert` |
| `ValidationService` | Koordiniert UML-, Snapshot-, Link-, Multiplizitäts- und OCL-Validierung. | `implementiert` |
| `ValidationErrorFactory` | Erzeugt stabile Validation Errors. | `implementiert` |
| `ValidationErrorCode` | Enthält unter anderem `SYNTAX_ERROR`, `TYPE_ERROR`, `UNKNOWN_CLASS`, `UNKNOWN_ATTRIBUTE`, `INVARIANT_VIOLATION`, `EVALUATION_ERROR`. | `implementiert` |
| `ElementTarget`/`ElementType` | Verknüpft Fehler mit Klasse, Invariante, OCL-Ausdruck, Objekt oder Link. | `implementiert` |

### Error Model und Source Locations

Source Locations sind im Backend bereits vorhanden und daher kein vollständig fehlendes Feature:

- `ocl/diagnostics/SourcePosition.java` führt Zeile, Spalte und Offset.
- `ocl/diagnostics/SourceRange.java` führt Start- und Endposition.
- AST-Knoten tragen Source Ranges.
- `OclDiagnostic` transportiert die Range.
- `api/dto/ocl/SourceRangeDto.java` überträgt sie über REST.
- `OclInvariantValidator` übernimmt Range-Daten in die Details eines Validation Errors.

Offen bleibt die Qualität und Vollständigkeit dieser Ortsangaben für spätere mehrzeilige und verschachtelte Konstrukte. Ebenso ist noch nicht belegt, dass die Validation Results UI Source Ranges direkt als Editor-Markierung verwendet.

## API- und Integration-Bezug

### OCL-Endpunkte

| Endpunkt | Dokumentierter Zweck | Aktueller Backend-Stand | Status |
|---|---|---|---|
| `POST /api/v1/projects/{projectId}/ocl/parse` | Syntax prüfen, Diagnosen und optional AST-Information liefern. | In `OclController` implementiert; delegiert an `OclParseService`. | `implementiert` |
| `POST /api/v1/projects/{projectId}/ocl/typecheck` | Ausdruck gegen Kontextklasse und UML-Modell typprüfen. | Implementiert; Parser wird vorgeschaltet. | `implementiert` |
| `POST /api/v1/projects/{projectId}/ocl/evaluate` | Ausdruck gegen konkretes Kontextobjekt und Snapshot auswerten. | Implementiert; führt Parse, Typecheck und Evaluation aus. | `implementiert` |
| `POST /api/v1/projects/{projectId}/validate` | Gesamtes Projekt beziehungsweise Constraints validieren. | In `ValidationController` implementiert. | `implementiert` |

Die Analysedokumente stufen den separaten Evaluate-Endpunkt teilweise als `Should` oder Post-MVP ein. Da der Endpunkt im aktuellen Backend vorhanden ist, gilt für diese Bestandsaufnahme der Quellcode als aktuellerer Befund.

### OCL-DTOs

Folgende DTOs sind im Backend unter `use-web-backend/src/main/java/de/useweb/backend/api/dto/ocl/` vorhanden:

- `OclParseRequestDto` und `OclParseResponseDto`
- `OclTypecheckRequestDto` und `OclTypecheckResponseDto`
- `OclEvaluateRequestDto` und `OclEvaluateResponseDto`
- `OclDiagnosticDto`
- `SourceRangeDto`
- `OclTokenDto`
- `OclExpressionDto`

Das Frontend besitzt korrespondierende Typen in `use-web-frontend/src/api/dtos/ocl.dto.ts` und API-Aufrufe in `use-web-frontend/src/api/services/oclApi.ts`.

### Festgestellte Vertragsgrenzen

| Punkt | Befund | Status |
|---|---|---|
| Parse-Antwort | Liefert Gültigkeit, Diagnosen, Tokens und eine AST-Darstellung. | `implementiert` |
| Typecheck-Antwort | Liefert Gültigkeit, Result Type und Diagnosen. | `implementiert` |
| Resolved References | In der Analyse vorgesehen; im aktuellen `OclTypecheckResponseDto` nicht belegt. | `geplant`/`nicht gefunden` |
| Evaluate-Antwort | Liefert Gültigkeit, Result Type, Rohwert und Diagnosen. | `implementiert` |
| Batch Evaluation | In aktueller API nicht gefunden. | `Post-MVP` |
| Validation Mode | Frontend-DTO kennt `FULL_PROJECT`, `SNAPSHOT_ONLY`, `OCL_ONLY`; der aktuelle `ValidationService` führt alle Validatoren aus. Eine serverseitige Modusauswertung wurde nicht belegt. | `unklar` |

## Validation-Bezug

Der OCL-Validation-Flow ist vollständig in den allgemeinen Constraint-Check eingebunden:

```text
POST /validate
  -> ValidationController
  -> ValidationService
  -> UML Structure Validation
  -> Snapshot Validation
  -> Link Validation
  -> Multiplicity Validation
  -> OclInvariantValidator
       -> Parser
       -> Type Checker
       -> Evaluator je Kontextobjekt
  -> ValidationResultDto
```

| Ergebnisfall | Backend-Verhalten | Status |
|---|---|---|
| Syntaxfehler | `SYNTAX_ERROR`, Ziele für Invariante/OCL-Ausdruck/Klasse und Source-Daten in Details. | `implementiert` |
| Unbekannte Klasse/Property | `UNKNOWN_CLASS` beziehungsweise `UNKNOWN_ATTRIBUTE`. | `implementiert` |
| Sonstiger Typfehler | `TYPE_ERROR`. | `implementiert` |
| Evaluation nicht möglich | `EVALUATION_ERROR` mit Kontextobjekt und Invariante. | `implementiert` |
| Ausdruck ergibt `false` | `INVARIANT_VIOLATION` mit Objekt-, Invarianten- und Klassenbezug. | `implementiert` |
| Ausdruck ergibt `true` | Kein OCL-Fehler für das Kontextobjekt. | `implementiert` |
| Invariante deaktiviert | Wird übersprungen. | `implementiert` |

Noch offen ist, wie OCL-Evaluation bei bereits vorhandenen strukturellen Snapshot-Fehlern behandelt werden soll. Der aktuelle `ValidationService` sammelt Ergebnisse aller Validatoren in Reihenfolge, bricht vor OCL aber nicht sichtbar ab.

## Frontend-Bezug

### OCL Editor

Der OCL Editor ist im Frontend unter `use-web-frontend/src/features/ocl-editor/components/OclEditorView.tsx` implementiert. Er bietet aktuell:

- eine eigene Workspace-Route `/projects/{projectId}/ocl`,
- einen USE-artigen Modelltext-Editor als Textarea mit Zeilennummern,
- `Apply Changes`,
- ein Diagnostics Panel,
- eine OCL Console,
- Loading-, Error- und Empty-Zustände.

Der Editor verarbeitet derzeit primär vollständigen Modelltext über den Model-Text-Apply-Flow. Die separaten OCL-API-Methoden sind im API-Client vorhanden; eine vollständige Live-Verknüpfung von Parse/Typecheck mit Inline-Markern im Editor wurde im geprüften Editor-Code nicht eindeutig belegt.

| Frontend-Funktion | Status | Pfad/Bemerkung |
|---|---|---|
| OCL-Workspace und Navigation | `implementiert` | `src/app/navigation.ts`, `src/pages/workspace/components/MainWorkspaceView.tsx` |
| Modelltext bearbeiten und anwenden | `implementiert` | `features/ocl-editor/components/OclEditorView.tsx` |
| Diagnostics-Liste | `implementiert` | `OclEditorView.tsx` |
| Separate Parse-/Typecheck-/Evaluate-API-Clients | `implementiert` | `src/api/services/oclApi.ts` |
| Live-Syntaxprüfung beim Tippen | `nicht gefunden` | In Analysen offen beziehungsweise vorgesehen. |
| Source-Range-basierte Unterstreichung im Text | `nicht gefunden` | Textarea statt vollwertiger Code-Editor-Komponente. |
| Autocomplete/Quick Fixes | `Post-MVP` | Nicht gefunden. |

### Validation Results UI

Die Validation Results UI ist unter `use-web-frontend/src/pages/workspace/components/ValidationResultsPanel.tsx` implementiert. Der Frontend-State indexiert Findings nach Element-IDs und kann Diagrammelemente markieren.

Vorhanden sind:

- Status und Summary,
- Fehler-/Warnungs-/Info-Darstellung,
- Detailinformationen und OCL-Ausdruck,
- Auswahl eines Findings,
- Mapping eines `OCL_EXPRESSION`-Targets auf OCL-Ansicht/Invariante,
- Markierungen an Klassen, Invarianten, Objekten und Links,
- Stale-Kennzeichnung nach Modelländerungen.

Ein direktes Springen zur exakten Zeile und Spalte im OCL Editor wurde nicht gefunden.

## Teststatus

### Geplante Teststrategie

`05-backend-analysis/17-backend-test-strategy.md` sieht isolierte Lexer-/Parser-/AST-, Typechecker- und Evaluator-Tests sowie Validation-, API- und Backend-E2E-Tests vor. Das Library-Modell ist das zentrale MVP-Fixture; spätere Iterator- und Vererbungsmodelle sind Post-MVP.

### Tatsächlich vorhandene Backend-Tests

| Testklasse | Abdeckung | Status |
|---|---|---|
| `use-web-backend/src/test/java/de/useweb/backend/ocl/OclLexerParserTest.java` | Tokenisierung, Parsererfolg und Syntaxdiagnosen. | `implementiert` |
| `.../ocl/OclAstParserTest.java` | AST-Struktur, darunter Collection Operation `size`. | `implementiert` |
| `.../ocl/OclTypeCheckerTest.java` | Typregeln, Property-/Navigationsauflösung und Ergebnistypen. | `implementiert` |
| `.../ocl/OclEvaluatorTest.java` | Auswertung gegen Snapshot und Kontextobjekt. | `implementiert` |
| `.../validation/ValidationServiceTest.java` | Gültige/verletzte Invarianten, Syntax-/Typ-/Evaluationsfälle und Gesamtvalidierung. | `implementiert` |
| `.../api/BackendApiControllerTest.java` | Unter anderem Parse- und Validate-Endpunkte. | `implementiert` |
| `.../api/LibraryBackendE2ETest.java` | Library-Workflow über REST und Constraint Validation. | `implementiert` |

Für die Backend-Testklassen liegen außerdem Surefire-Berichte unter `use-web-backend/target/surefire-reports/` vor. Ein erneuter Maven-Testlauf konnte während dieser Dokumentationsprüfung wegen eines lokalen Prozess-/Sandbox-Zugriffsfehlers nicht gestartet werden; deshalb wird hier kein neuer Grünstatus behauptet.

### Tatsächlich vorhandene Frontend-Tests

| Testbereich | Beispielpfad | Status |
|---|---|---|
| OCL Editor | `use-web-frontend/src/features/ocl-editor/__tests__/OclEditorView.test.tsx` | `implementiert` |
| Modelltextdarstellung | `use-web-frontend/src/features/ocl-editor/__tests__/modelText.test.ts` | `implementiert` |
| Validation Results | `use-web-frontend/src/pages/workspace/components/__tests__/ValidationResultsPanel.test.tsx` | `implementiert` |
| Check Constraints | `use-web-frontend/src/pages/workspace/components/__tests__/CheckConstraintsButton.test.tsx` | `implementiert` |
| API-Vertrag/Normalisierung | `use-web-frontend/src/api/api.test.ts` | `implementiert` |

### Noch nicht belegte Tests

| Testlücke | Status |
|---|---|
| Tests für alle Post-MVP-Collection-Operationen und Iteratoren | `Post-MVP` |
| Regressionstests aus komplexeren USE-Beispielen | `geplant`; systematische Übernahme in neue Fixtures nicht gefunden |
| Source-Range-Mapping vom Backend bis zur exakten Editor-Markierung | `nicht gefunden` |
| Vollständige Tests für `null`/`invalid` und OCL-Boolean-Semantik | `offen` |
| API-Tests für Typecheck und Evaluate in derselben Breite wie Parse/Validate | teilweise vorhanden oder `unklar`; vollständige Matrix nicht belegt |

## Bezug zum originalen USE-Projekt

Das originale USE-Projekt ist ausschließlich fachliche Referenz. Es wird weder als Dependency verwendet noch wird Code daraus kopiert. Für die aktuelle Architektur und spätere Gap-Analyse sind folgende konkrete Stellen relevant:

| Referenzpfad im Workspace | Fachliche Relevanz |
|---|---|
| `use/use-core/src/main/java/org/tzi/use/parser/ocl/OCLCompiler.java` | Referenz für OCL-Compile-Ablauf, Parser- und Semantikphasen. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/Evaluator.java` | Referenz für Ausdrucksauswertung. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/ExpForAll.java` | Fachliche Referenz für `forAll`. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/ExpExists.java` | Fachliche Referenz für `exists`. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/ExpAllInstances.java` | Fachliche Referenz für `allInstances`. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/operations/StandardOperationsCollection.java` | Referenz für Collection-Operationen und ihre Signaturen. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/type/CollectionType.java` | Referenz für Collection-Typsemantik. |
| `use/use-core/src/main/java/org/tzi/use/uml/mm/MClassInvariant.java` | Referenz für Invariantenkontext und `self`. |
| `use/use-core/src/main/java/org/tzi/use/uml/sys/MSystemState.java` | Referenz für Systemzustand und Validierung. |

### Relevante Example-Referenzen

| Beispielpfad | Mögliche fachliche Nutzung |
|---|---|
| `use/use-core/src/main/resources/examples/Documentation/Cars/Cars.use` | Kleine Invarianten und einfacher MVP-Regressionsfall. |
| `use/use-core/src/main/resources/examples/Documentation/Demo/Demo.use` | Navigation, Collections und Invarianten. |
| `use/use-core/src/main/resources/examples/Documentation/Imports/LibraryManagement.use` | Bibliotheksnahes, komplexeres Referenzmodell. |
| `use/use-core/src/main/resources/examples/Documentation/Graph/Graph.use` | Navigation und Collection-/Iterator-Ausdrücke in einer Graphdomäne. |
| `use/use-core/src/main/resources/examples/Others/DerivedProperties/derived.use` | Derived-Property-Referenz. |
| `use/use-core/src/main/resources/examples/Papers/1998/WarmerAndKleppe/RoyalAndLoyal.use` | Umfangreicheres OCL-Modell als spätere Regressionstestquelle. |
| `use/use-core/src/main/resources/examples/Papers/1998/RichtersAndGogolla/CarRental.use` | Realistischeres Modell mit komplexeren Constraints. |

Die Beispiele belegen fachliche Möglichkeiten des Originals, nicht die Kompatibilität des neuen Backends. Viele Ausdrücke daraus überschreiten das aktuelle MVP-Subset und können derzeit nicht unverändert als grüne Backend-Tests verwendet werden.

## Bereits feststehende Architekturentscheidungen

| Entscheidung | Stand |
|---|---|
| Eigenständige OCL-Komponente im neuen Java/Spring-Boot-Backend | Festgelegt und umgesetzt. |
| Keine Dependency auf den alten USE-Core | Festgelegt; im geprüften OCL-Code eingehalten. |
| Kein Kopieren alten USE-Codes | Festgelegt; USE wird nur als Referenz genannt. |
| Explizite Pipeline von Lexer bis Validation Result | Festgelegt und im MVP-Kern umgesetzt. |
| Backend ist autoritativ für OCL-Semantik | Festgelegt und umgesetzt. |
| AST statt Regex-Auswertung | Festgelegt und umgesetzt. |
| Typechecking als eigene Phase | Festgelegt und umgesetzt. |
| Evaluation gegen eigenes UML-/Snapshot-Modell | Festgelegt und umgesetzt. |
| Strukturierte Fehlercodes, Element-IDs und Source Ranges | Festgelegt; im Kern umgesetzt. |
| Kleines MVP-Subset, Erweiterungen schrittweise danach | Festgelegt; aktueller Code entspricht dieser Grenze weitgehend. |

## Lücken und offene Punkte

### Fachliche und semantische Fragen

| Offener Punkt | Aktueller Befund | Status |
|---|---|---|
| Fehlende Slot-Werte | Analysen nennen `undefined`, `null`, Validation Error oder nicht auswertbar als Varianten; verbindliche OCL-Semantik ist nicht dokumentiert. | `offen` |
| `invalid`/`null` | Kein vollständiges Wert- und Propagationsmodell gefunden. | `offen` |
| Boolean-Logik | Vollständige OCL-Wahrheitstabellen bei `invalid`/`null` fehlen. | `offen` |
| Collection-Arten | Collection-Elementtyp vorhanden, aber `Set`/`Bag`/`Sequence`/`OrderedSet` nicht ausdifferenziert. | `Post-MVP` |
| Single- versus Multi-Navigation | Verhalten bei inkonsistenten Links oder Multiplicity-Verletzungen ist nicht abschließend geklärt. | `offen` |
| Navigierbarkeit | Ob alle Association Ends navigierbar sind, bleibt in der Analyse offen; aktueller Code orientiert sich an Rollen. | `unklar` |
| Vererbung/Subtyping | Im MVP nicht vorhanden. | `Post-MVP` |

### Parser-, AST- und Typechecker-Fragen

| Offener Punkt | Aktueller Befund | Status |
|---|---|---|
| Kanonische Collection-Syntax | Implementierung belegt Aufrufe mit `()`, Analysen fragen nach USE-naher Kurzform. | `offen` |
| String-Literal-Schreibweise | Die verbindliche öffentliche Syntax ist in den Analysen offen; Parserverhalten muss als Vertrag dokumentiert werden. | `offen` |
| Typed AST | Ergebnistyp vorhanden, Typannotationen/Resolved References am AST nicht gefunden. | `unklar` |
| Fehlerfortsetzung | Parser liefert Diagnosen, aber robuste Recovery für mehrere Fehler in einem Ausdruck ist nicht belegt. | `offen` |
| Mehrzeilige Source Ranges | Datenmodell vorhanden; umfassende Tests für komplexe Ausdrücke nicht gefunden. | `offen` |

### Evaluator- und Validation-Fragen

| Offener Punkt | Aktueller Befund | Status |
|---|---|---|
| Evaluation trotz Strukturfehlern | Validation Service führt alle Validatoren aus; gewünschte Abbruchregel ist offen. | `offen` |
| Mehrere Fehler pro Invariante | Reihenfolge, Deduplizierung und UI-Darstellung sind nicht verbindlich geklärt. | `offen` |
| AST-Caching | Aktuell wird neu geparst; Caching/Invalidierung nicht entschieden. | `offen` |
| Validation Modes | DTO-Konzept nennt Modi, tatsächliche serverseitige Filterung ist nicht belegt. | `unklar` |
| Evaluation Trace | Analysen sehen Debug-/Trace-Informationen perspektivisch vor; produktiver API-Trace nicht gefunden. | `Post-MVP` |

### API- und Frontend-Fragen

| Offener Punkt | Aktueller Befund | Status |
|---|---|---|
| Live-Parse/Typecheck im Editor | API vorhanden, Editorintegration nicht eindeutig gefunden. | `offen` |
| Exakte Source-Markierung | DTO und Backend-Range vorhanden, Editor-Unterstreichung nicht gefunden. | `offen` |
| `resolvedReferences` | Analysedokumente sehen das Feld vor, aktueller Response-Typ belegt es nicht. | `geplant` |
| Autocomplete und Quick Fixes | Nicht implementiert. | `Post-MVP` |
| Vollwertiger Code-Editor | Aktuell Textarea; Syntax Highlighting und robuste Marker fehlen. | `Post-MVP` beziehungsweise `offen` |

### Dokumentationslücken

- Mehrere Analyse-Dokumente formulieren Komponenten noch als Zukunftsdesign, obwohl sie bereits implementiert sind.
- Die geplante Datei `03-uml-ocl-domain/05-library-example-model.md` wurde nicht gefunden.
- Die geplante Datei `07-integration-and-api/06-end-to-end-library-demo.md` wurde nicht gefunden.
- Eine ausgearbeitete Post-MVP-Roadmap `08-planning/07-post-mvp-roadmap.md` wurde nicht gefunden.
- Der aktuelle Implementierungsstand der OCL-Endpunkte ist weiter als einige `Should`-/Post-MVP-Markierungen in den Integrationsdokumenten.

## Zusammenfassung

Die neue Anwendung besitzt bereits eine eigenständige, durchgängige OCL-MVP-Implementierung. Lexer, Parser, eigener AST, Type Checker, Evaluator, Wertmodell, Source Ranges, Invariantenvalidator, Validation Result Mapping und REST-Endpunkte sind im aktuellen Backend vorhanden. Das Frontend verfügt über OCL Editor, OCL-API-Client, Check-Constraints-Flow und Validation Results UI.

Der tatsächlich implementierte Sprachumfang bleibt bewusst klein: `self`, Attribute, einfache Association Navigation, primitive Literale, Vergleiche, Boolean-Operatoren, Klammern sowie `size`, `isEmpty` und `notEmpty`. Die im Basisprompt genannten erweiterten Collection-Operationen, Iteratoren, `let`, `if-then-else`, `allInstances`, Pre-/Postconditions, derived Attributes und init Values sind im neuen Backend nicht implementiert und bleiben Post-MVP.

Die wichtigsten aktuellen Lücken liegen weniger in der Existenz der Pipeline als in ihrer semantischen Tiefe und Integration: vollständige `null`-/`invalid`-Semantik, Collection-Arten, robuste Navigation, Typed-AST-/Resolved-Reference-Informationen, Validation Modes, Fehler-Recovery sowie die exakte Source-Range-Darstellung im OCL Editor sind offen oder nur teilweise belegt.

Das originale USE-Projekt und seine Beispiele bieten dafür eine breite fachliche Referenz. Sie bleiben jedoch ausdrücklich außerhalb der technischen Laufzeitarchitektur: keine USE-Core-Dependency, keine Migration und keine Übernahme alten USE-Codes.
