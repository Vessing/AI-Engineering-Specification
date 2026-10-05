# Original USE Reference Check for Backend

## Zweck dieser Datei

Diese Datei prüft aus Backend-Sicht, welche Teile des originalen USE-Projekts als Referenz für das neue Java/Spring-Boot-Backend relevant sind.

Wichtig: Das originale USE-Projekt ist keine technische Grundlage des neuen Backends. Es soll weder geforkt noch als Runtime Dependency verwendet werden. Diese Datei dokumentiert ausschließlich fachliche, syntaktische und verhaltensbezogene Erkenntnisse, die für Analyse, Architektur, Tests und spätere Import-/Export-Funktionen nützlich sind.

## Untersuchungsansatz

Untersucht wurden im Workspace insbesondere:

- Repository- und Modulstruktur des originalen USE-Projekts,
- UML-Metamodell-Packages,
- OCL-Type-, Value-, Expression- und Parserbereiche,
- Grammatikdateien,
- Systemzustand und Snapshot-Klassen,
- Validierungslogik in `MSystemState`,
- SOIL-/Command-Logik für Objektmanipulation,
- Parser-, UML-, OCL-, System- und Value-Tests,
- `.use`- und `.cmd`-Beispielmodelle,
- Dokumentation und Manual-Dateien.

Nutzungskategorien:

| Kategorie | Bedeutung |
|---|---|
| fachliche Referenz | Konzept fachlich analysieren und für neues Modell neu entwerfen. |
| Syntaxreferenz | Syntax, Beispiele oder Grammatik als Orientierung nutzen. |
| Testfallquelle | Beispiele oder Tests als Grundlage eigener Testfälle ableiten. |
| Verhaltenreferenz | Erwartetes Verhalten ableiten, aber neu implementieren. |
| nicht übernehmen | Nicht technisch verwenden, nicht direkt migrieren. |
| später prüfen | Für Post-MVP relevant, im MVP nicht notwendig. |

## Relevante Originalbereiche

| Bereich/Pfad | Kurzbeschreibung | Relevanz neues Backend | Nutzungskategorie | Risiken / Abweichungen |
|---|---|---|---|---|
| `use/use-core` | Fachlicher Kern des originalen Projekts. | Hoch | fachliche Referenz | Keine Dependency; Konzepte nur neu modellieren. |
| `use/use-gui` | Desktop-GUI und Shell-Integration. | Niedrig für Backend | nicht übernehmen | UI-Technologie und Desktop-Workflows nicht relevant. |
| `use/use-assembly` | Packaging/Distribution. | Niedrig | nicht übernehmen | Keine Relevanz für Spring-Boot-Backend. |
| `use/use-core/src/main/java/org/tzi/use/uml/mm` | UML-Modellrepräsentation. | Sehr hoch | fachliche Referenz | Umfang größer als MVP. |
| `use/use-core/src/main/java/org/tzi/use/uml/sys` | Systemzustand, Objekte, Links, Snapshots. | Sehr hoch | Verhaltenreferenz | Enthält auch Derived/State-Machine-Themen. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/type` | OCL-Typsystem. | Hoch | fachliche Referenz | Vollständiger als MVP; nicht kopieren. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/value` | OCL-Werte und Collections. | Hoch | fachliche Referenz | Undefined/Invalid komplexer als MVP. |
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr` | OCL-Ausdrücke und Evaluator. | Sehr hoch | Verhaltenreferenz | Kein Wrapper um alten Evaluator. |
| `use/use-core/src/main/java/org/tzi/use/parser` | Parser-Grundlagen, Fehler, AST, Compiler. | Hoch | Syntaxreferenz | Parserarchitektur nicht übernehmen. |
| `use/use-core/src/main/resources/grammars` | Grammatikfragmente für USE, OCL, SOIL, Shell. | Hoch | Syntaxreferenz | Nur als Referenz; MVP-Parser neu und kleiner. |
| `use/use-core/src/main/java/org/tzi/use/uml/sys/soil` | Objektmanipulation über SOIL-Statements. | Mittel | später prüfen | MVP nutzt REST/JSON, nicht SOIL. |
| `use/use-core/src/main/resources/examples` | Beispielmodelle und Command-Dateien. | Sehr hoch | Testfallquelle | Beispiele müssen auf MVP-Subset reduziert werden. |
| `use/use-core/src/test` und `src/it` | Unit-/Integrationstests. | Hoch | Testfallquelle | Testlogik nicht kopieren, Szenarien ableiten. |
| `use/manual`, `use/documentation`, `README.OCL` | Benutzer- und OCL-Dokumentation. | Mittel | Syntaxreferenz | Manual enthält auch Nicht-MVP-Features. |

## UML-Modellrepräsentation

Das Package `use/use-core/src/main/java/org/tzi/use/uml/mm` enthält die zentrale UML-Modellrepräsentation des originalen USE-Cores.

| Klasse/Package | Zweck im Original | Relevanz für neues Backend | Nutzungskategorie | Hinweise |
|---|---|---|---|---|
| `MModel` | Modellcontainer für Klassen, Associations, Invarianten usw. | Sehr hoch | fachliche Referenz | Referenz für `UmlModel`. |
| `MClass`, `MClassImpl` | UML-Klassen. | Sehr hoch | fachliche Referenz | Referenz für `UmlClass`. |
| `MAttribute` | Attribute mit Typinformationen. | Sehr hoch | fachliche Referenz | Wichtig für Slots und OCL-Attributzugriff. |
| `MOperation` | Operationen, Signaturen und weitergehende Semantik. | Mittel | fachliche Referenz / später prüfen | MVP nur Signaturen. |
| `MAssociation`, `MAssociationImpl` | UML-Associations. | Sehr hoch | fachliche Referenz | Basis für `UmlAssociation`. |
| `MAssociationEnd` | Rollen, Typen, Navigierbarkeit, Multiplizität. | Sehr hoch | fachliche Referenz | Wichtig für OCL-Navigation und Multiplicity Checks. |
| `MMultiplicity` | Multiplizitätsmodell. | Sehr hoch | Verhaltenreferenz | Neues JSON nutzt `lower`, `upper`, `unbounded`. |
| `MClassInvariant` | Klasseninvarianten mit Kontext und OCL-Body. | Sehr hoch | fachliche Referenz | Referenz für `UmlInvariant`. |
| `MPrePostCondition` | Pre-/Postconditions. | Niedrig im MVP | später prüfen | Post-MVP für Operation Contracts. |
| `MGeneralization` | Vererbung. | Niedrig im MVP | später prüfen | Post-MVP. |
| `MAssociationClass` | Assoziationsklassen. | Niedrig im MVP | später prüfen | Post-MVP. |
| `MAggregationKind` | Aggregation/Komposition. | Niedrig im MVP | später prüfen | Post-MVP. |
| `MInvalidModelException` | Modellfehler. | Mittel | Verhaltenreferenz | Fehlerarten in eigenes Error Model übertragen. |
| `ModelFactory` | Erzeugung von Modellobjekten. | Mittel | historische/technische Orientierung | Keine Factory-Struktur übernehmen. |

Auswirkungen auf neues Backend:

- `UmlModel`, `UmlClass`, `UmlAttribute`, `UmlAssociation`, `UmlAssociationEnd`, `Multiplicity` und `UmlInvariant` sollten fachlich an diesen Konzepten ausgerichtet sein.
- Die neue Repräsentation muss kleiner bleiben und für REST/JSON geeignet sein.
- IDs und DTOs sind im neuen System zentrale Konzepte; USE arbeitet stärker objekt- und namebasiert.

## OCL-Syntax und Parser

Relevante Originalbereiche:

| Bereich/Pfad | Zweck im Original | Relevanz neues Backend | Nutzungskategorie | Hinweise |
|---|---|---|---|---|
| `use/use-core/src/main/resources/grammars/ocl/OCL.gpart` | OCL-Grammatikfragment. | Hoch | Syntaxreferenz | Referenz für Syntax und Präzedenz. |
| `use/use-core/src/main/resources/grammars/base/OCLBase.gpart` | Basisregeln für OCL. | Hoch | Syntaxreferenz | Tokens und Grundstruktur prüfen. |
| `use/use-core/src/main/resources/grammars/base/OCLLexerRules.gpart` | Lexer-Regeln. | Hoch | Syntaxreferenz | Keine direkte Übernahme. |
| `use/use-core/src/main/resources/grammars/use/USE.gpart` | `.use`-Modellsprache. | Hoch für Modelltext-Subset und Post-MVP Import | Syntaxreferenz / später prüfen | MVP nutzt JSON als Persistenzformat, verarbeitet aber einen begrenzten USE-ähnlichen Editor-Text. |
| `use/use-core/src/main/resources/grammars/soil/Soil.gpart` | SOIL-Kommandos. | Mittel | später prüfen | Snapshot-Import Post-MVP. |
| `org.tzi.use.parser.ocl.OCLCompiler` | Kompiliert OCL zu Expression. | Hoch | Verhaltenreferenz | Pipeline-Idee, keine Implementierung. |
| `org.tzi.use.parser.use.USECompiler` | Kompiliert `.use`-Modelle. | Hoch für Import/Export | Syntaxreferenz / später prüfen | Nicht im MVP als Runtime verwenden. |
| `org.tzi.use.parser.ParseErrorHandler` | Fehlerbehandlung mit Positionen. | Hoch | Verhaltenreferenz | Referenz für Source Ranges. |
| `org.tzi.use.parser.SemanticException` | Semantische Parser-/Compile-Fehler. | Hoch | Verhaltenreferenz | Referenz für Typ-/Semantikfehler. |
| `org.tzi.use.parser.SrcPos` | Source Positionen. | Hoch | fachliche Referenz | Neues API-Format braucht `sourceRange`. |

Relevante Parser-AST-Klassen:

| Package/Klasse | Relevanz |
|---|---|
| `org.tzi.use.parser.ocl.ASTBinaryExpression` | Vergleichs- und Boolean-Ausdrücke. |
| `ASTUnaryExpression` | `not` und unäre Operatoren. |
| `ASTStringLiteral`, `ASTIntegerLiteral`, `ASTRealLiteral`, `ASTBooleanLiteral` | MVP-Literale. |
| `ASTOperationExpression` | Operation Calls, Collection-Operationen. |
| `ASTQueryExpression` | Iteratoren wie `forAll`, `exists`, `select`, `collect` für Post-MVP. |
| `ASTIfExpression`, `ASTLetExpression` | Post-MVP-OCL. |
| `ASTAllInstancesExpression` | Post-MVP. |
| `org.tzi.use.parser.use.ASTClass`, `ASTAttribute`, `ASTAssociation`, `ASTMultiplicity`, `ASTInvariantClause` | `.use`-Import/Export-Referenz. |

Risiken:

- USE unterstützt deutlich mehr OCL und Modellsyntax als der MVP.
- Eine vollständige Übernahme der Grammatik würde Scope und Komplexität sprengen.
- Die neue OCL Engine sollte bewusst ein kleines, eigenes MVP-Subset implementieren.

## OCL-Auswertung

Relevante Originalbereiche:

| Bereich/Pfad | Zweck im Original | Relevanz neues Backend | Nutzungskategorie | Hinweise |
|---|---|---|---|---|
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr` | OCL-Ausdrucksmodell und Evaluator. | Sehr hoch | Verhaltenreferenz | Fachliches Verhalten analysieren, nicht kopieren. |
| `Expression` | Basisklasse für OCL-Ausdrücke. | Hoch | fachliche Referenz | Referenz für AST/Typed AST. |
| `Evaluator`, `ThreadedEvaluator`, `EvaluatorCallable` | OCL-Auswertung. | Hoch | Verhaltenreferenz | Neues Backend baut eigenen Evaluator. |
| `EvalContext`, `SimpleEvalContext`, `DetailedEvalContext` | Auswertungskontext. | Hoch | fachliche Referenz | Referenz für `EvaluationContext`. |
| `ExpVariable` | Variablen wie `self`. | Hoch | fachliche Referenz | MVP benötigt `self`. |
| `ExpAttrOp` | Attributzugriff. | Hoch | Verhaltenreferenz | Referenz für Slotzugriff. |
| `ExpNavigation` | Association Navigation. | Sehr hoch | Verhaltenreferenz | Wichtig für Objektlink-Navigation. |
| `ExpStdOp` | Standardoperationen. | Hoch | Verhaltenreferenz | Vergleichs-/Boolean-/Collection-Operationen. |
| `ExpConstString`, `ExpConstInteger`, `ExpConstReal`, `ExpConstBoolean` | Literale. | Hoch | fachliche Referenz | MVP-Literale. |
| `ExpForAll`, `ExpExists`, `ExpSelect`, `ExpCollect`, `ExpLet`, `ExpIf`, `ExpAllInstances` | Erweiterter OCL-Sprachumfang. | Mittel | später prüfen | Post-MVP. |
| `MultiplicityViolationException` | Laufzeitfehler bei Navigation/Multiplizität. | Mittel | Verhaltenreferenz | Eigenes Error Model verwenden. |

OCL-Typen:

| Bereich/Pfad | Relevanz |
|---|---|
| `use/use-core/src/main/java/org/tzi/use/uml/ocl/type/Type.java` | Grundtypmodell. |
| `TypeFactory` | Erzeugung von Typen. |
| `StringType`, `IntegerType`, `RealType`, `BooleanType` | MVP-Primitive. |
| `CollectionType`, `SetType`, `SequenceType`, `BagType`, `OrderedSetType` | Collection-Semantik, MVP vereinfacht. |
| `EnumType` | Post-MVP Enumerationen. |
| `VoidType`, `OclAnyType`, `TupleType` | Post-MVP / Typkonformität. |

OCL-Werte:

| Bereich/Pfad | Relevanz |
|---|---|
| `StringValue`, `IntegerValue`, `RealValue`, `BooleanValue` | MVP-Werte. |
| `ObjectValue`, `InstanceValue`, `LinkValue` | Snapshot-Auswertung. |
| `CollectionValue`, `SetValue`, `SequenceValue`, `BagValue`, `OrderedSetValue` | Collection-Auswertung. |
| `UndefinedValue` | Post-MVP für unset/null/invalid-Semantik. |
| `VarBindings` | Variablenkontext, später Iteratoren und `let`. |

Auswirkungen auf neues Backend:

- Die Pipeline `Parser -> AST -> Typechecker -> Evaluator -> Result` ist fachlich bestätigt.
- Das MVP sollte nur `self`, Attributzugriff, einfache Navigation, Literale, Vergleich, Boolean und `size/isEmpty/notEmpty` abdecken.
- USEs differenzierte Collection- und Undefined-Semantik ist Post-MVP und darf den MVP nicht überladen.

## Systemzustände und Snapshots

Das Package `use/use-core/src/main/java/org/tzi/use/uml/sys` ist für das neue Backend besonders relevant, weil es die konkrete Laufzeitwelt des Modells repräsentiert.

| Klasse | Zweck im Original | Relevanz neues Backend | Nutzungskategorie | Hinweise |
|---|---|---|---|---|
| `MSystem` | Systemcontainer mit Modell und Zustand. | Hoch | fachliche Referenz | Neues `Project`/Application Service trennt anders. |
| `MSystemState` | Konkreter Systemzustand/Snapshot. | Sehr hoch | Verhaltenreferenz | Referenz für `ObjectModel`. |
| `MObject`, `MObjectImpl` | Objektinstanz. | Sehr hoch | fachliche Referenz | Referenz für `ObjectInstance`. |
| `MObjectState` | Attributwerte eines Objekts im Zustand. | Sehr hoch | fachliche Referenz | Referenz für `Slot`. |
| `MInstance`, `MInstanceState` | Gemeinsame Basis für Objekte/Links. | Mittel | fachliche Referenz | Neues Modell kann einfacher sein. |
| `MLink`, `MLinkImpl` | Konkreter Link. | Sehr hoch | fachliche Referenz | Referenz für `ObjectLink`. |
| `MLinkEnd` | Link-Ende. | Hoch | fachliche Referenz | Unterstützt end-basiertes JSON-Format. |
| `MLinkSet` | Menge von Links je Association. | Hoch | Verhaltenreferenz | Wichtig für Navigation und Multiplicity. |
| `MLinkObject`, `MLinkObjectImpl` | Association Class Instanz. | Niedrig im MVP | später prüfen | Post-MVP. |
| `DerivedAttributeController`, `DerivedLinkController*` | Derived Semantik. | Niedrig im MVP | später prüfen | Post-MVP. |
| `StatementEvaluationResult` | Ergebnis von State-Manipulation. | Mittel | später prüfen | Für Command-/SOIL-Verhalten. |

Auswirkungen:

- Das neue Backend braucht einen eigenen Snapshot-Service mit Objekten, Slots und Links.
- End-basierte Links im JSON-Format werden durch USE-Konzepte wie `MLinkEnd` fachlich gestützt.
- Derived Attribute, Derived Links und State Machines sind nicht MVP-relevant.

## Validierungslogik

Die zentrale Validierungsreferenz ist `use/use-core/src/main/java/org/tzi/use/uml/sys/MSystemState.java`.

Gefundene relevante Methoden:

| Methode / Bereich | Zweck im Original | Relevanz neues Backend | Nutzungskategorie |
|---|---|---|---|
| `check(PrintWriter out, boolean traceEvaluation, boolean showDetails, boolean allInvariants, List<String> invNames)` | Prüft Struktur und Invarianten. | Sehr hoch | Verhaltenreferenz |
| `checkStructure(PrintWriter out)` / `checkStructure(PrintWriter out, boolean reportAllErrors)` | Prüft Struktur des Zustands. | Sehr hoch | Verhaltenreferenz |
| `checkStructure(MAssociation assoc, PrintWriter out, boolean reportAllErrors)` | Prüft Association-Struktur und Multiplicity. | Sehr hoch | Verhaltenreferenz |
| `reportMultiplicityViolation(...)` | Textuelle Meldung von Multiplizitätsverletzungen. | Hoch | Verhaltenreferenz |
| Invariantenschleife über `MClassInvariant` | Wertet aktive Invarianten aus. | Sehr hoch | Verhaltenreferenz |
| `evaluateInitExpression(...)` | Init Values. | Niedrig im MVP | später prüfen |
| `evaluateDeriveExpression(...)` | Derived Attributes/Links. | Niedrig im MVP | später prüfen |
| `checkStateInvariants(...)` | State Machines. | Nicht relevant | nicht übernehmen |

Relevante Beobachtungen:

- USE prüft zunächst Struktur und danach Invarianten.
- Invarianten werden über Kontextklassen und Instanzen geprüft.
- Multiplizitätsverletzungen werden textuell ausgegeben; das neue Backend muss daraus strukturierte `ValidationError`-Objekte ableiten.
- Im Original können Trace und Details ausgegeben werden; das neue Backend sollte optional Debug-/Trace-Daten anbieten.

Abweichung im neuen Backend:

- Keine `PrintWriter`-Ausgabe als API.
- Ergebnis wird `ValidationResult` mit Codes wie `MULTIPLICITY_VIOLATION` und `INVARIANT_VIOLATION`.
- Fehler müssen Element-IDs enthalten, nicht nur Text.

## Beispiele und Testfälle

### Beispielmodelle

Relevante Beispielbereiche:

| Pfad | Inhalt / Nutzen | Nutzungskategorie |
|---|---|---|
| `use/use-core/src/main/resources/examples/Documentation/Cars/Cars.use` | Einfache Klassen/Assoziationen. | Testfallquelle |
| `use/use-core/src/main/resources/examples/Documentation/Demo/Demo.use` und `Demo.cmd` | Modell plus Snapshot-Kommandos. | Testfallquelle |
| `use/use-core/src/main/resources/examples/Documentation/Employee/Employee.use` | Klassisches OCL-Beispiel. | Testfallquelle |
| `use/use-core/src/main/resources/examples/Documentation/Imports/LibraryManagement.use` | Library-nahes Modell mit Imports. | später prüfen |
| `use/use-core/src/main/resources/examples/Documentation/AssociationClass/AssociationClass.use` | Assoziationsklassen. | später prüfen |
| `use/use-core/src/main/resources/examples/Documentation/AggregationsAndCompositions/*.use` | Aggregation/Komposition. | später prüfen |
| `use/use-core/src/main/resources/examples/Others/ReflexiveAssociation/ReflexiveAssociation.use` | Reflexive Associations. | Testfallquelle / später prüfen |
| `use/use-core/src/main/resources/examples/Others/Tree/Tree.use` | Strukturmodell mit Links. | Testfallquelle |
| `use/use-core/src/main/resources/examples/Others/CarRental/*.use` | Größeres Klassenmodell. | Testfallquelle |
| `use/use-core/src/main/resources/examples/Others/DerivedProperties/derived.use` | Derived Properties. | später prüfen |

### Tests

Relevante Testbereiche:

| Pfad / Klasse | Inhalt | Nutzung |
|---|---|---|
| `use/use-core/src/test/java/org/tzi/use/parser/USECompilerTest.java` | `.use` Parser-Tests. | Syntaxreferenz / Testfallquelle |
| `use/use-core/src/test/resources/org/tzi/use/parser/*.use` und `*.fail` | Positive/negative Parserfälle. | Testfallquelle |
| `use/use-core/src/test/java/org/tzi/use/uml/mm/ModelCreationTest.java` | Modellaufbau. | fachliche Referenz |
| `ModelAPITest.java`, `MMultiplicityTest.java` | Modell- und Multiplicity-Verhalten. | Testfallquelle |
| `use/use-core/src/test/java/org/tzi/use/uml/ocl/expr/EvaluatorTest.java` | Evaluator-Verhalten. | Verhaltenreferenz |
| `ExprNavigationTest.java`, `NavigationTest.java` | Navigation. | Verhaltenreferenz |
| `ExpStdOpTest.java`, `ExpQueryTest.java` | OCL-Operationen und Queries. | Testfallquelle |
| `use/use-core/src/test/java/org/tzi/use/uml/ocl/type/TypeTest.java` | OCL-Typmodell. | Testfallquelle |
| `use/use-core/src/test/java/org/tzi/use/uml/ocl/value/ValueTest.java` | OCL-Werte. | Testfallquelle |
| `use/use-core/src/test/java/org/tzi/use/uml/sys/MSystemStateTest.java` | Systemzustandsverhalten. | Verhaltenreferenz |
| `ObjectCreation.java`, `LinkTest.java`, `DeletionTest.java` | Objekt-/Link-Lifecycle. | Testfallquelle |
| `use/use-core/src/it/java/org/tzi/use/OCLExpressionIT.java` | OCL-Integrationstest. | Testfallquelle |
| `use/use-gui/src/it/resources/testfiles/shell/*.use` | Shell-Testmodelle. | Testfallquelle / später prüfen |

Ableitung für neue Backend-Tests:

- Kleine MVP-Fixtures aus `Cars`, `Demo`, `Employee` und Library-nahen Beispielen ableiten.
- Negative Parserfälle aus `*.fail` nicht vollständig übernehmen, sondern auf MVP-Syntax reduzieren.
- Navigation und Multiplicity anhand eigener JSON-Projekte testen.

## Fehlermeldungen

Relevante Originalbereiche:

| Bereich | Relevanz | Nutzung |
|---|---|---|
| `ParseErrorHandler` | Syntaxfehler und Positionen. | Referenz für `sourceRange`. |
| `SemanticException` | Semantische Fehler beim Kompilieren. | Referenz für `TYPE_ERROR`, `UNKNOWN_CLASS`, `UNKNOWN_ATTRIBUTE`. |
| `MInvalidModelException` | Modellfehler. | Referenz für UML-Modellvalidierung. |
| `MSystemException` | Systemzustandsfehler. | Referenz für Snapshot-/Linkfehler. |
| `reportMultiplicityViolation(...)` in `MSystemState` | Multiplizitätsfehler. | Referenz für `MULTIPLICITY_VIOLATION`. |
| Evaluator-/Expression-Ausnahmen | OCL-Auswertungsfehler. | Referenz für `EVALUATION_ERROR`. |

Abweichung:

- USE gibt viele Fehler textuell aus oder nutzt Exceptions.
- Das neue Backend muss Fehler in ein strukturiertes JSON-Format mit `code`, `severity`, `phase`, `sourceRange`, `objectIds`, `linkIds`, `slotIds`, `modelElementIds` und `invariantId` übersetzen.

## Import/Export

Relevante Bereiche:

| Bereich/Pfad | Zweck | Relevanz | Nutzung |
|---|---|---|---|
| `use/use-core/src/main/resources/grammars/use/USE.gpart` | `.use`-Syntax. | Hoch Post-MVP | Syntaxreferenz |
| `org.tzi.use.parser.use.USECompiler` | Kompiliert `.use`-Modelle. | Hoch Post-MVP | Verhaltenreferenz |
| `org.tzi.use.parser.use.ASTImportStatement` | Import Statements. | Mittel | später prüfen |
| `use/use-core/src/test/resources/org/tzi/use/parser/*imports*.use` | Import-Testfälle. | Mittel | Testfallquelle |
| `use/use-gui/src/it/resources/testfiles/shell/imports/*.use` | Shell-Importfälle. | Mittel | Testfallquelle |
| `.cmd`-Dateien in `examples` | Snapshot-/Command-Erzeugung. | Mittel Post-MVP | Testfallquelle |
| `org.tzi.use.parser.soil.SoilCompiler` und `parser/soil/ast` | SOIL-Parser und AST. | Mittel Post-MVP | später prüfen |
| `org.tzi.use.uml.sys.soil` | SOIL-Ausführung auf Systemzustand. | Mittel Post-MVP | Verhaltenreferenz |

MVP-Entscheidung:

- JSON-Projektformat ist primär.
- Vollständiger `.use` Import/Export ist Post-MVP.
- Der MVP darf einen reduzierten USE-ähnlichen Modelltext-Apply-Flow unterstützen, damit der OCL Editor ganze Modelltexte aus Beispielen anzeigen und anwenden kann.
- `.cmd`/SOIL-Snapshot-Import ist Post-MVP.
- Original-Parser wird nicht eingebettet.

## Relevanzmatrix

| Originalbestandteil | Pfad / Klasse | Backend-Relevanz | Kategorie | Empfehlung |
|---|---|---|---|---|
| UML-Modellcontainer | `uml/mm/MModel` | Sehr hoch | fachliche Referenz | Für `UmlModel` analysieren. |
| Klassen | `MClass`, `MClassImpl` | Sehr hoch | fachliche Referenz | Für `UmlClass` verwenden. |
| Attribute | `MAttribute` | Sehr hoch | fachliche Referenz | Für Slots und Typechecker relevant. |
| Operationen | `MOperation` | Mittel | später prüfen | MVP nur Signaturen. |
| Associations | `MAssociation`, `MAssociationEnd` | Sehr hoch | fachliche Referenz | Rollen, Navigation, Multiplicity. |
| Multiplicity | `MMultiplicity` | Sehr hoch | Verhaltenreferenz | JSON-Struktur neu entwerfen. |
| Invarianten | `MClassInvariant` | Sehr hoch | fachliche Referenz | Kontextklasse und Boolean-Ausdruck. |
| OCL-Typen | `uml/ocl/type/*` | Hoch | fachliche Referenz | MVP-Typmodell ableiten. |
| OCL-Werte | `uml/ocl/value/*` | Hoch | fachliche Referenz | Eigene `OclValue`-Modelle. |
| OCL-Ausdrücke | `uml/ocl/expr/*` | Sehr hoch | Verhaltenreferenz | Eigene AST/Evaluator-Architektur. |
| OCL Parser | `parser/ocl/*`, `OCL.gpart` | Hoch | Syntaxreferenz | MVP-Grammatik neu bauen. |
| `.use` Parser | `parser/use/*`, `USE.gpart` | Mittel | Syntaxreferenz / später prüfen | MVP-Subset für Modelltext-Apply; vollständiger Post-MVP Import/Export. |
| Snapshot | `MSystemState` | Sehr hoch | Verhaltenreferenz | Für `ObjectModel`/Validation analysieren. |
| Objekte | `MObject`, `MObjectState` | Sehr hoch | fachliche Referenz | Für `ObjectInstance`/`Slot`. |
| Links | `MLink`, `MLinkEnd`, `MLinkSet` | Sehr hoch | fachliche Referenz | Für `ObjectLink`. |
| Validation | `MSystemState.check*` | Sehr hoch | Verhaltenreferenz | Für Validation Service. |
| SOIL | `parser/soil`, `uml/sys/soil` | Mittel | später prüfen | Nicht MVP. |
| Beispielmodelle | `src/main/resources/examples` | Sehr hoch | Testfallquelle | MVP-Fixtures ableiten. |
| Tests | `src/test`, `src/it` | Hoch | Testfallquelle | Szenarien ableiten. |
| GUI | `use-gui` | Niedrig | nicht übernehmen | Für Backend irrelevant. |
| Assembly | `use-assembly` | Niedrig | nicht übernehmen | Packaging nicht relevant. |

## Nicht zu übernehmende Bestandteile

| Bestandteil | Warum nicht übernehmen |
|---|---|
| `use-core` als Dependency | Widerspricht Ziel eines neuen Backends und koppelt an alte Architektur. |
| `MModel`, `MSystemState`, `Evaluator` als Runtime-Kern | Zu umfangreich, nicht REST-/JSON-orientiert, schwer in neue Architektur zu integrieren. |
| ANTLR-/Grammatik-Setup des Originals als direkte Basis | MVP braucht kleines eigenes OCL-Subset; direkte Übernahme erhöht Scope. |
| Swing-/Desktop-GUI aus `use-gui` | Neues Frontend ist React/TypeScript. |
| Shell-/SOIL als primäre Bedienlogik | Neues System nutzt REST/API und Web-UI. |
| Textuelle `PrintWriter`-Validierungsausgaben | Neues Backend braucht strukturierte `ValidationResult`-JSONs. |
| Vollständige USE-Feature-Parität | MVP fokussiert Klassendiagramm, Objektdiagramm, Snapshots und OCL-Subset. |
| State Machines / Sequenzdiagramme / Modellanimation | Nicht Teil des Zielumfangs. |

## Auswirkungen auf neues Backend

| Backend-Bereich | Relevante USE-Referenz | Konsequenz |
|---|---|---|
| Domain Model | `uml/mm`, `uml/sys` | Eigene Domain-Klassen an USE-Konzepten ausrichten. |
| UML Model Service | `MModel`, `MClass`, `MAttribute`, `MAssociationEnd`, `MMultiplicity` | Strukturregeln und Referenzen ableiten. |
| Object Model Service | `MSystemState`, `MObject`, `MObjectState`, `MLink` | Snapshot- und Linkmodell klar trennen. |
| OCL Parser | `grammars/ocl`, `parser/ocl` | MVP-Grammatik definieren, Source Ranges berücksichtigen. |
| OCL Typechecker | `uml/ocl/type`, `SemanticException` | Typmodell und Diagnosecodes ableiten. |
| OCL Evaluator | `uml/ocl/expr`, `uml/ocl/value` | Evaluationskontext und Wertmodell neu entwerfen. |
| Validation Service | `MSystemState.check`, `checkStructure` | Ablauf `Structure -> Multiplicity -> Invariants` übernehmen. |
| Error Model | `ParseErrorHandler`, `SemanticException`, Validierungsausgaben | Strukturierte JSON-Fehler statt Text. |
| Teststrategie | `src/test`, `src/it`, `examples` | Eigene MVP-Fixtures und Regressionstests ableiten. |
| Modelltext-Apply | `USE.gpart`, `USECompiler`, `.use` Beispiele | Reduziertes MVP-Subset für Editor-Workflow ableiten; keine USE-Core-Nutzung. |
| Vollständiger Import/Export | `USE.gpart`, `USECompiler`, `.use` Beispiele | Post-MVP planen, nicht MVP. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Welche originalen Beispielmodelle werden als erste MVP-Testfixtures reduziert? | Vorschlag: `Cars`, `Demo`, `Employee`, Library-nahe Imports. |
| Wie viel USE-OCL-Syntax soll der MVP akzeptieren? | Muss bewusst kleiner als Original bleiben. |
| Soll die MVP-Navigation alle Rollen als navigierbar behandeln? | USE kennt differenziertere Navigierbarkeit. |
| Wie wird `UndefinedValue` im MVP behandelt? | Original bietet Semantik, MVP sollte einfach bleiben. |
| Welche `.use`-Features sind für späteren Import zuerst relevant? | Klassen, Attribute, Associations, Invarianten vor Pre/Post und SOIL. |
| Sollen `.cmd`-Dateien später in Snapshots importiert werden? | Post-MVP; SOIL-Umfang klären. |
| Wie werden textuelle USE-Fehler in strukturierte neue Fehlercodes übersetzt? | Mapping muss in Error Contract konkretisiert werden. |

## Zusammenfassung

Das originale USE-Projekt ist aus Backend-Sicht eine sehr wertvolle fachliche Referenz. Besonders relevant sind:

- `org.tzi.use.uml.mm` für UML-Modellkonzepte,
- `org.tzi.use.uml.sys` für Snapshots, Objekte und Links,
- `org.tzi.use.uml.ocl.type`, `value` und `expr` für OCL-Typisierung und Auswertung,
- `org.tzi.use.parser` und `resources/grammars` für Syntax- und Parserreferenzen,
- `MSystemState.check(...)` und `checkStructure(...)` für Validierungsverhalten,
- Beispielmodelle und Tests als Testfallquellen.

Für das neue Backend gilt trotzdem eine klare Abgrenzung: Keine direkte Codeübernahme, kein Fork, keine Runtime Dependency und keine vollständige USE-Feature-Parität im MVP. Die Erkenntnisse aus USE sollen in eine neue, webfähige Architektur mit eigenem Domänenmodell, eigener OCL Engine, eigenem Validation Service und REST/JSON API überführt werden.
