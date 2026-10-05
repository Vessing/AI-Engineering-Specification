# Backend vs Original USE Gap Analysis

## Zweck dieser Datei

Diese Datei vergleicht die aktuelle OCL-Unterstützung des eigenständigen
`use-web-backend` mit drei fachlichen Referenzen:

1. der OCL-Unterstützung des originalen USE-Projekts,
2. den Modellen im Workspace-Ordner `examples/`,
3. der originalen Parser- und Shell-Testbasis.

Der Vergleich ist eine **Gap-Analyse**, keine Migrationsplanung. Der alte
USE-Core wird weder als Dependency verwendet noch wird produktiver USE-Code
kopiert. Maßgeblich für die Zielsemantik bleibt die OCL-Spezifikation; USE
liefert Syntax-, Verhaltens- und Testreferenzen.

## Vergleichsprinzipien

| Prinzip | Konsequenz |
|---|---|
| OCL-Standard vor USE-Dialekt | USE-spezifische Erweiterungen werden separat markiert. |
| Code vor Planungsannahme | Als implementiert gilt nur, was im aktuellen Backend-Code und in Tests erkennbar ist. |
| Referenzteststatus ist vorläufig | Ohne Reference-Test-Harness sind Statuswerte begründete Erwartungen, keine Messergebnisse. |
| Fehlschlagende Tests sind erwünscht | `FAILING_GAP` macht fehlende Syntax oder Semantik sichtbar. |
| Fachliche statt textuelle Gleichheit | `*`-Shellausgaben werden später in strukturierte Assertions übersetzt. |
| Keine verdeckte Abhängigkeit | Originale USE-Klassen werden nicht in produktiven oder neuen Testcode eingebunden. |

## Wichtige Einschränkungen

- Die originale Testbasis wurde nach Dateien und repräsentativen Ausdrücken
  untersucht, aber noch nicht durch ein neues Reference-Test-Harness ausgeführt.
- `PASSING` bedeutet daher in diesem Dokument **voraussichtlich ausführbar**,
  sofern Modell- und Snapshot-Setup mit dem aktuellen Backend-Domänenmodell
  hergestellt werden kann.
- Viele `.in`-Dateien mischen OCL-Ausdrücke mit Shell-, SOIL-, ASSL- oder
  Operationsaufrufen. Der Status gilt jeweils für den genannten Fall, nicht
  automatisch für die komplette Datei.
- Das Workspace enthält sowohl `examples/` als auch `Examples/` mit weitgehend
  gespiegelten Inhalten. In dieser Datei werden Pfade unter `examples/` verwendet.
- Eine vollständige OCL-Konformitätsaussage ist erst nach einem normativen
  OCL-Testprofil und der Inventarisierung aller Referenzfälle möglich.

## Statusmodell

| Status | Bedeutung |
|---|---|
| `PASSING` | Der fachlich extrahierte Fall sollte mit dem aktuellen Backend bereits erfolgreich laufen. |
| `FAILING_GAP` | Der Fall ist extrahierbar, benötigt aber fehlende Syntax, Typregeln oder Evaluation. |
| `FAILING_FORMAT` | Semantik ist abbildbar, die alte Textausgabe ist noch nicht als strukturierte Assertion normalisiert. |
| `FAILING_INFRASTRUCTURE` | Modellimport, Snapshot, Operationstrace oder Test-Harness fehlt. |
| `NON_OCL_OR_SHELL_ONLY` | Der Fall betrifft ausschließlich Shell, GUI, ASSL, SOIL oder alte Laufzeitsteuerung. |
| `UNCLEAR` | Relevanz oder erwartete Semantik muss manuell geprüft werden. |

`FAILING_FORMAT` und `FAILING_INFRASTRUCTURE` beschreiben nur Blocker der
getrennten Reference-Auswertung. Sie begruenden keine Produktanforderung zur
Nachbildung alter USE-Ausgaben, Shellkommandos oder Testinfrastruktur. Jede
Empfehlung in diesem Dokument ist entsprechend als fachliche Extraktion in
eigene Fixtures und strukturierte Assertions zu lesen. Reine
Altsystemfunktionalitaet ist `NON_OCL_OR_SHELL_ONLY`.

## Analysequellen

### Neues System

- `use-web-analysis/00-overview/03-documentation-map.md`
- `use-web-analysis/03-uml-ocl-domain/`
- `use-web-analysis/05-backend-analysis/`
- `use-web-analysis/07-integration-and-api/`
- `use-web-analysis/09-ocl-extension-analysis/01-ocl-current-state.md`
- `use-web-analysis/09-ocl-extension-analysis/03-ocl-feature-prioritization.md`
- `use-web-analysis/09-ocl-extension-analysis/04-collection-operations.md`
- `use-web-analysis/09-ocl-extension-analysis/05-iterator-expressions.md`
- `use-web-analysis/09-ocl-extension-analysis/06-let-if-allinstances.md`
- `use-web-analysis/09-ocl-extension-analysis/07-pre-post-derived-init.md`
- `use-web-analysis/09-ocl-extension-analysis/08-ocl-error-handling-and-source-locations.md`
- `use-web-analysis/09-ocl-extension-analysis/09-ocl-test-strategy.md`
- `use-web-analysis/09-ocl-extension-analysis/10-ocl-extension-roadmap.md`

### Originales USE-Projekt

| Bereich | Konkreter Pfad | Referenznutzen |
|---|---|---|
| OCL-Compiler/Parser | `use/use-core/src/main/java/org/tzi/use/parser/ocl/` | Grammatik, AST-Erzeugung und semantische Diagnose |
| Ausdrucksmodell | `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/` | fachliche Ausdrucks- und Evaluationsreferenz |
| Typmodell | `use/use-core/src/main/java/org/tzi/use/uml/ocl/type/` | Set, Bag, Sequence, OrderedSet, Tuple und Konformität |
| Standardoperationen | `use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/operations/` | Operationssignaturen und Ergebnissemantik |
| Collection-Bibliothek | `.../operations/StandardOperationsCollection.java` | `size`, includes/excludes, count, flatten, Konvertierungen |
| Iteratoren | `.../expr/ExpSelect.java`, `ExpCollect.java`, `ExpAny.java`, `ExpClosure.java` | Scope- und Evaluationsreferenz |
| Alle Instanzen | `.../expr/ExpAllInstances.java` | Snapshotbezug von `allInstances()` |
| Evaluator | `.../expr/Evaluator.java`, `EvalContext.java` | Kontextbindung und Zustandsauswertung |
| Derived Values | `use/use-core/src/main/java/org/tzi/use/uml/sys/DerivedAttributeController.java` | Lebenszyklus abgeleiteter Attribute |

Diese Klassen werden ausschließlich gelesen. Ihre Architektur ist nicht die
Zielarchitektur des neuen Backends.

## Aktueller Stand des neuen Backends

### Implementierte Pipeline

| Pipeline-Stufe | Aktuelle Dateien | Erkennbarer Umfang |
|---|---|---|
| Lexer | `use-web-backend/src/main/java/de/useweb/backend/ocl/lexer/OclLexer.java`, `OclTokenType.java` | `self`, Identifier, String/Integer/Real/Boolean, `and/or/not`, Vergleiche, Punkt, Pfeil, Klammern |
| Parser | `.../ocl/parser/OclParser.java` | Präzedenz für `or`, `and`, Gleichheit, Ordnung, `not`; Property Access; nur drei Collection-Operationen |
| AST | `.../ocl/ast/` | Self, Literal, Property Access, Collection Operation, Unary, Binary, Parenthesized |
| Typechecker | `.../ocl/typecheck/OclTypeChecker.java`, `OclType.java` | Primitive Typen, Klasse, vereinfachte `Collection(T)`, Attribut/Rollenauflösung, Invariant muss Boolean sein |
| Evaluator | `.../ocl/evaluation/OclEvaluator.java`, `EvaluationContext.java` | Snapshotwerte, binäre Association-Navigation, drei Collection-Operationen, Boolean/Vergleich |
| Validation | `.../validation/rules/OclInvariantValidator.java`, `.../validation/service/ValidationService.java` | Parse, Typecheck und Evaluation je Kontextobjekt; strukturierte Findings |
| Diagnostics | `.../ocl/diagnostics/SourcePosition.java`, `SourceRange.java`, `OclDiagnostic.java` | Zeile, Spalte und Offset; AST-Knoten tragen Ranges |
| REST DTOs | `.../api/dto/ocl/`, `.../api/dto/validation/` | Parse-, Typecheck-, Evaluate- und Validation-Ergebnisse mit Diagnostics/Elementbezug |

### Durch aktuelle Tests belegter Umfang

| Testdatei | Beleg |
|---|---|
| `use-web-backend/src/test/java/de/useweb/backend/ocl/OclLexerParserTest.java` | Tokenisierung, `notEmpty()`, Syntaxdiagnose mit Location |
| `.../ocl/OclAstParserTest.java` | AST für `self.books <= 5`, Navigation plus `size()`, `not`, Klammern und `and` |
| `.../ocl/OclTypeCheckerTest.java` | Primitive Vergleiche, unbekannte Property, Collection-`size()`, Boolean-Invariant |
| `.../ocl/OclEvaluatorTest.java` | Slotwerte, String/Boolean-Vergleich, binäre Navigation, `size()`, fehlender Slot als Diagnose |
| `.../validation/ValidationServiceTest.java` | gültige und verletzte Invariante, Syntaxfehler und Mapping auf Objekt/Klasse/Invariante |

## Relevante Example-Dateien

| Datei | Sichtbare Features | Erwartete aktuelle Einordnung |
|---|---|---|
| `examples/Documentation/Demo/Demo.use` | `allInstances`, mehrvariable `forAll`, `implies`, Navigation Chain, `includesAll` | überwiegend `FAILING_GAP` |
| `examples/Documentation/Employee/Employee.use` | Operation Contract und `@pre` | `FAILING_INFRASTRUCTURE`, danach `FAILING_GAP` |
| `examples/Papers/1998/RichtersAndGogolla/CarRental.use` | `select`, String-`substring`, Navigation | `FAILING_GAP` |
| `examples/Papers/2006/GogollaBuettnerRichters/civstat.use` | Enum, pre/post, `@pre`, `if`, `let`, Range, `forAll`, `allInstances`, `implies` | `FAILING_INFRASTRUCTURE` und `FAILING_GAP` |
| `examples/Others/Tree/Tree.use` | Set-Literal, union, includesAll, collect, flatten, iterate, allInstances, Rekursion | `FAILING_GAP` |
| `examples/Others/DerivedProperties/derived.use` | derived Association End, subsets, `select` | zunächst `FAILING_INFRASTRUCTURE` |
| `examples/Documentation/Imports/LibraryManagement.use` | Importstruktur und realistischeres Library-Modell | `FAILING_INFRASTRUCTURE` |
| `examples/Others/RecursiveOperations/RecursiveOperations.use` | Operation Bodies und Rekursion | `FAILING_INFRASTRUCTURE` |
| `examples/StateMachines/CoffeeDispenser/coffeedispenser.use` | State Machine | `NON_OCL_OR_SHELL_ONLY` für State-Machine-Anteile |

## Relevante originale Testbasis

### Parser-Testbasis

Pfad: `use/use-core/src/test/resources/org/tzi/use/parser`

| Artefakt | Bedeutung | Erwartete Nutzung |
|---|---|---|
| `*.use` | positive Modell-/Parserfälle | Modellkontext und extrahierte OCL-Fragmente parsen/typechecken |
| `*.fail` | negative Parser-/Semantikfälle | erwarteten Diagnosecode und Source Range prüfen |
| `imports/*.use` | modulare Importfälle | erst nach eigenem Import-Harness ausführbar |
| `test_expr.in` | Ausdruckseingaben | in einzelne OCL-Servicefälle zerlegen |
| `test_spec.use` | umfangreicher Spezifikationsfall | Featureinventar und spätere Integration |

Konkrete Dateien mit erweiterten OCL-/Kontextkonstrukten sind unter anderem
`t4.use`, `t5.use`, `t5.fail`, `t13.use`, `t13.fail`, `t15.use`, `t16.use`,
`t16.fail`, `t21c.use` und `test_spec.use`. Die genaue Assertion je Datei muss
in Schritt 4/5 der Roadmap inventarisiert werden; der Dateiname allein reicht
nicht für einen belastbaren PASSING-Status.

### Shell-Testbasis

Pfad: `use/use-gui/src/it/resources/testfiles/shell`

| Datei | Repräsentative Fälle | Erwarteter Status |
|---|---|---|
| `t001.in` | Literale, Arithmetik, Vergleiche, `not`, undefined, Standardoperationen | einfache Literale/Vergleiche teilweise `PASSING`; Arithmetik/undefined `FAILING_GAP` |
| `t002.in` | Vererbung, allInstances, Typoperationen, Collection-Literale, select, collect, iterate, Enum | überwiegend `FAILING_GAP`; Modellsetup teilweise `FAILING_INFRASTRUCTURE` |
| `t004.in` | Navigation Chains, implizites collect, flatten, Operation Calls, let, Tuple | `FAILING_GAP` |
| `t038.in` | `C.allInstances` und `C.allInstances()` | `FAILING_GAP` |
| `t084.in` | postcondition, `result`, Call Stack, allInstances | `FAILING_INFRASTRUCTURE`; danach `FAILING_GAP` |

Zeilen mit `?` enthalten Shell-Evaluationen. Direkt folgende `*`-Zeilen
beschreiben erwartete USE-Ausgaben. Ein neues Harness darf beispielsweise
`*-> Set{c1} : Set(C)` nicht blind als String vergleichen, sondern soll Wertart,
Elemente und statischen/dynamischen Typ strukturiert prüfen.

## Vergleichsmatrix

| Bereich | Neues Backend | Original USE/Examples | Hauptstatus | Gap-ID |
|---|---|---|---|---|
| Literale | String, Integer, Real, Boolean | zusätzlich undefined, UnlimitedNatural, Enum, Collection, Tuple | teilweise `PASSING` | `OCL-GAP-001` |
| Boolean | `and`, `or`, `not` | zusätzlich `implies`, `xor`, vierwertige Semantik | `FAILING_GAP` | `OCL-GAP-002` |
| Numerik/String | Vergleiche, keine allgemeine Operationsbibliothek | Arithmetik und umfangreiche Standardoperationen | `FAILING_GAP` | `OCL-GAP-003` |
| Collections | generisches `Collection(T)` | Set, Bag, Sequence, OrderedSet mit eigener Semantik | `FAILING_GAP` | `OCL-GAP-004` |
| Collection-Basis | size/isEmpty/notEmpty | vollständige Collection Library | teilweise `PASSING` | `OCL-GAP-005` |
| Collection-Literale | fehlen | Set/Bag/Sequence/OrderedSet und Ranges | `FAILING_GAP` | `OCL-GAP-006` |
| Iteratoren | fehlen | forAll, exists, select, reject, collect, any, one, iterate, closure | `FAILING_GAP` | `OCL-GAP-007` |
| Navigation | binär, Rollenname, Einzel-/Collectionziel | Chains, implizites collect, Vererbung, qualifier/n-är | teilweise `PASSING` | `OCL-GAP-008` |
| Kontrollausdrücke | fehlen | if/then/else, let | `FAILING_GAP` | `OCL-GAP-009` |
| Modellweite Abfrage | fehlt | allInstances | `FAILING_GAP` | `OCL-GAP-010` |
| OCL-Kontexte | Invarianten | inv, pre, post, body, def, derive, init | Infrastruktur/Gaps | `OCL-GAP-011` |
| Zustandssemantik | aktueller Snapshot | pre-/post-state, `@pre`, `result`, `oclIsNew` | `FAILING_INFRASTRUCTURE` | `OCL-GAP-012` |
| Typoperationen | fehlen | oclIsTypeOf/KindOf/AsType, Type Conformance | `FAILING_GAP` | `OCL-GAP-013` |
| null/invalid | keine vollständige OCL-Wertsemantik | OclVoid/undefined und Invalid-Ausbreitung | `FAILING_GAP` | `OCL-GAP-014` |
| Fehlerpositionen | Ranges vorhanden | umfangreiche Parser-/Semantikdiagnosen | teilweise `PASSING` | `OCL-GAP-015` |
| Validation Mapping | Objekt/Klasse/Invariante strukturiert | USE-Shelltext und Eval-Details | neues Backend stärker UI-orientiert | `OCL-GAP-016` |
| Referenztests | eigene MVP-Tests | große Parser-/Shellbasis | Harness fehlt | `OCL-GAP-017` |
| Modellimport | USE-Subset | vollständige USE-Modelle, Imports und weitere UML-Konstrukte | `FAILING_INFRASTRUCTURE` | `OCL-GAP-018` |

## Syntax-Gaps

| Gap-ID | Fehlende Syntax | Beleg | Auswirkung | Priorität | Roadmap |
|---|---|---|---|---|---:|
| `OCL-GAP-001` | Enum-, undefined-, Collection- und Tuple-Literale | `t001.in`, `t002.in` | viele Shellausdrücke nicht parsebar | hoch | 10, 12, 26, 27 |
| `OCL-GAP-002` | `implies`, `xor` | `Demo.use`, `civstat.use` | reale Invarianten fallen aus | sehr hoch | 11 |
| `OCL-GAP-003` | `+ - * / div mod`, allgemeine Calls, Stringops | `t001.in`, `CarRental.use` | Standardbibliothek stark begrenzt | hoch | 11, 27 |
| `OCL-GAP-006` | Collection-Literale/Ranges | `t001.in`, `t002.in`, `civstat.use` | Iteratoren ohne unabhängige Testquellen | sehr hoch | 12 |
| `OCL-GAP-007` | Iteratorgrammatik und Variablen | `t002.in`, `Demo.use`, `Tree.use` | zentrale OCL-Ausdrücke fehlen | sehr hoch | 17-22 |
| `OCL-GAP-009` | `let`, `if ... endif` | `t004.in`, `civstat.use` | Strukturierung/bedingte Werte fehlen | hoch | 23-24 |
| `OCL-GAP-010` | statischer Typzugriff/allInstances | `t038.in`, `t002.in` | globale Constraints fehlen | sehr hoch | 25 |
| `OCL-GAP-011` | pre/post/body/def/derive/init-Kontexte | `Employee.use`, `derived.use` | nur Invarianten nutzbar | mittel/spät | 28-30 |

## Typechecker-Gaps

| Gap-ID | Aktueller Stand | Fehlende Regel | Referenz | Testidee |
|---|---|---|---|---|
| `OCL-GAP-004` | nur `Collection(T)` | Collection-Kind und Least Common Supertype | `.../ocl/type/{SetType,BagType,SequenceType,OrderedSetType}.java` | Typ jeder Collection-Operation tabellarisch prüfen |
| `OCL-GAP-007` | kein Variablenkontext | Iterator-Scope, Shadowing, Bodytyp, mehrere Iteratorvariablen | `ASTQueryExpression.java`, `Demo.use` | verschachtelte Iteratoren und falscher Bodytyp |
| `OCL-GAP-009` | keine lokale Bindung/Branches | let-Typannotation, Branch-Conformance | `civstat.use` | inkompatible then/else-Typen ablehnen |
| `OCL-GAP-010` | nur self-Kontextklasse | Typliteral und Resultat `Set(T)` | `ExpAllInstances.java` | unbekannte Klasse und Subtypen prüfen |
| `OCL-GAP-013` | keine Vererbung/Typoperationen | Konformität und Castregeln | `t002.in` | `oclIsKindOf` vs `oclIsTypeOf` |
| `OCL-GAP-014` | kein OclVoid/Invalid-Typ | null/invalid-Konformität und Lift-Regeln | `t001.in` | undefined in Vergleich/Boolean prüfen |
| `OCL-GAP-011` | nur Boolean-Invariant | Contractparameter, `result`, derive/init-Zieltyp | `Employee.use`, `derived.use` | Kontextgebundene Symboltabellen |

## Evaluator-Gaps

| Gap-ID | Fehlendes Verhalten | Referenz | Risiko | Roadmap |
|---|---|---|---|---:|
| `OCL-GAP-003` | Arithmetik und Standardoperation Dispatch | `t001.in` | inkonsistente Überladung | 11, 27 |
| `OCL-GAP-004` | Duplikate/Reihenfolge je Collection-Art | `t002.in`, `t004.in` | fachlich falsche Ergebnisse trotz passender Elemente | 12 |
| `OCL-GAP-007` | Scope-Stack und Bodyauswertung je Element | `ExpSelect.java`, `ExpCollect.java` | Shadowing und nested iterator fehlerhaft | 17-22 |
| `OCL-GAP-008` | mehrstufige Navigation/implicit collect | `t004.in` | falsche Ergebnisart und Flattening | 11, 20 |
| `OCL-GAP-010` | Klassenextent aus Snapshot | `t038.in` | stale/inkomplette Objektmenge | 25 |
| `OCL-GAP-012` | Vor-/Nachzustand, result, neue Objekte | `t084.in`, `Employee.use` | Contracts nicht deterministisch prüfbar | 28-29 |
| `OCL-GAP-014` | OCL-konforme null/invalid-Propagation | `t001.in` | Java-null oder technische Exception statt OCL-Wert | 10 |

## Collection-Gaps

| Gap-ID | Featuregruppe | Backend | USE-Beleg | Status | Roadmap |
|---|---|---|---|---|---:|
| `OCL-GAP-005` | `size/isEmpty/notEmpty` | vorhanden, Klammern erforderlich | `t002.in` nutzt `->size` ohne Klammern | teilweise `FAILING_GAP` | 13 |
| `OCL-GAP-019` | includes/excludes/includesAll/excludesAll | fehlt | `Demo.use`, `Tree.use`, `StandardOperationsCollection.java` | `FAILING_GAP` | 14 |
| `OCL-GAP-020` | including/excluding/count | fehlt | `t002.in`, Collection Operations | `FAILING_GAP` | 15 |
| `OCL-GAP-021` | union/intersection/flatten/conversions | fehlt | `Tree.use`, `t004.in` | `FAILING_GAP` | 16 |
| `OCL-GAP-022` | min/max/sum/product und kind-spezifische Ops | fehlt | `StandardOperationsCollection.java`, Shellbasis | `FAILING_GAP` | 27 |

## Iterator-Gaps

| Gap-ID | Iterator | Beispielbeleg | Erwarteter Status | Roadmap |
|---|---|---|---|---:|
| `OCL-GAP-007A` | Infrastruktur/Scope | `t002.in` | `FAILING_GAP` | 17 |
| `OCL-GAP-007B` | forAll/exists | `Demo.use`, `civstat.use` | `FAILING_GAP` | 18 |
| `OCL-GAP-007C` | select/reject | `CarRental.use`, `derived.use` | `FAILING_GAP` | 19 |
| `OCL-GAP-007D` | collect/collectNested | `Tree.use`, `t004.in` | `FAILING_GAP` | 20 |
| `OCL-GAP-007E` | any/one/isUnique/sortedBy | Shell-/Standardbibliothek | `FAILING_GAP` | 21 |
| `OCL-GAP-007F` | iterate/closure | `t002.in`, `Tree.use` | `FAILING_GAP` | 22 |

## Association-Navigation-Gaps

Das Backend löst Rollen binärer Associations auf und wertet vorhandene Links
aus. Damit ist `self.borrowedBooks->size()` im Library-Modell belegt. Es fehlen:

- Navigation Chains über Einzelobjekte und Collections,
- implizites `collect` wie `a.b.c` aus `t004.in`,
- Ergebnisarten abhängig von Ordnung, Eindeutigkeit und Multiplizität,
- Generalisierung an Association Ends,
- n-äre, qualifizierte, Association-Class-, derived-, subset- und union-Ends,
- OCL-konforme Navigation bei null/invalid.

Diese Lücken gehören zu `OCL-GAP-008` und teilweise zu den UML-/Importgaps
`OCL-GAP-018`; reine OCL-Arbeiten dürfen fehlende UML-Strukturen nicht verdecken.

## Kontext-Gaps

| Kontext | Backend | Referenz | Status | Voraussetzung |
|---|---|---|---|---|
| Invariant | implementiert | zahlreiche `.use`-Modelle | teilweise `PASSING` | OCL-Featureumfang erweitern |
| Precondition | fehlt | `Employee.use`, `civstat.use` | `FAILING_INFRASTRUCTURE` | Operation Invocation Model |
| Postcondition | fehlt | `t084.in`, `Employee.use` | `FAILING_INFRASTRUCTURE` | Pre-/Post-Snapshot und Result |
| Operation Body | fehlt | `RecursiveOperations.use` | `FAILING_INFRASTRUCTURE` | Calls, Parameter, Rekursionkontrolle |
| Derived Attribute/End | fehlt | `derived.use` | `FAILING_INFRASTRUCTURE` | Zieltyp, Cache/Lebenszyklus |
| Init Value | fehlt | Parser-/Modellreferenzen | `FAILING_INFRASTRUCTURE` | Objektinitialisierung |
| Additional Definition (`def`) | fehlt | USE-Modelle/Testbasis | `FAILING_GAP`/Infrastruktur | Namensauflösung und Modulkontext |

## Error-Handling-Gaps

| Gap-ID | Positiver Stand | Lücke | Empfehlung |
|---|---|---|---|
| `OCL-GAP-015` | Tokens und AST tragen `SourceRange`; API hat `SourceRangeDto` | keine Iterator-/Argument-Subranges; Fehlercodes noch grob | Ranges bei jeder neuen Grammatikproduktion bewahren |
| `OCL-GAP-014` | Evaluation liefert strukturierte Diagnostics | null/invalid nicht als eigene OCL-Werte modelliert | keine Java-Exception als Fachresultat zulassen |
| `OCL-GAP-016` | Validation mappt Kontextobjekt, Klasse und Invariante | Subexpression/Iteratorvariable/Operation fehlen | `expressionRange`, Feature-/Rollenname und Kontextpfad ergänzen |
| `OCL-GAP-017` | eigene Unit-/Service-/API-Tests vorhanden | kein Reference-Report und keine Statushistorie | separates Harness gemäß `09-ocl-test-strategy.md` |

## Testbasis-Gaps

### Parser-Test-Gaps

| Gap-ID | Beobachtung | Erwarteter Status | Nächste Aktion |
|---|---|---|---|
| `OCL-GAP-017A` | `.use`/`.fail` werden nicht automatisch inventarisiert | `FAILING_INFRASTRUCTURE` | Manifest mit Datei, Fall, Feature und Erwartung erzeugen |
| `OCL-GAP-017B` | negative USE-Diagnostics sind nicht auf neue Codes gemappt | `FAILING_FORMAT` | semantische Fehlerkategorie und Range statt Text vergleichen |
| `OCL-GAP-018` | viele Modelle überschreiten den aktuellen ModelText-Importer | `FAILING_INFRASTRUCTURE` | OCL-Fragmente isolieren oder minimalen Fixture-Builder nutzen |
| `OCL-GAP-017C` | Imports benötigen Dateiauflösung | `FAILING_INFRASTRUCTURE` | kontrollierten Test-Resolver statt USE-Core einführen |

### Shell-Test-Gaps

| Gap-ID | Beobachtung | Erwarteter Status | Nächste Aktion |
|---|---|---|---|
| `OCL-GAP-017D` | `?` und `*` werden noch nicht extrahiert | `FAILING_INFRASTRUCTURE` | Parser für Eingabe-/Erwartungsblöcke bauen |
| `OCL-GAP-017E` | Shellwertformat unterscheidet sich von REST-DTOs | `FAILING_FORMAT` | ExpectedValue/ExpectedDiagnostic normalisieren |
| `OCL-GAP-017F` | `.in` mischt Commands und OCL | `UNCLEAR`/`NON_OCL_OR_SHELL_ONLY` | Befehle klassifizieren und OCL-Blöcke isolieren |
| `OCL-GAP-012` | Operationstraces und Zustandswechsel fehlen | `FAILING_INFRASTRUCTURE` | Contract-Harness erst in Roadmap 28-29 |

## OCL-Evaluation-Gaps aus `.in`-Dateien

| Referenzfall | Fachliche Assertion | Statusprognose | Gap |
|---|---|---|---|
| `t001.in`: `? 42`, `? true`, `? 'aString'` | Wert und primitiver Typ | `PASSING` | - |
| `t001.in`: `? 4 <= 4`, `? not false` | Boolean-/Vergleichssemantik | `PASSING` | - |
| `t001.in`: Arithmetik/undefined | numerische Library bzw. OclVoid | `FAILING_GAP` | `001/003/014` |
| `t002.in`: `A.allInstances` | Extent inklusive Subtypen | `FAILING_GAP` | `010/013` |
| `t002.in`: select/collect/iterate | Iteratorergebnis und Collection-Art | `FAILING_GAP` | `004/007` |
| `t002.in`: `->size` ohne `()` | parameterlose Operation | `FAILING_GAP` | `005` |
| `t004.in`: `a.b.c` | implizites collect und Bag-Ergebnis | `FAILING_GAP` | `008` |
| `t004.in`: `let ... in ...` | lokale Variable | `FAILING_GAP` | `009` |
| `t038.in`: beide allInstances-Schreibweisen | identisches Set | `FAILING_GAP` | `010` |
| `t084.in`: postcondition/result/call stack | Contractdiagnose | `FAILING_INFRASTRUCTURE` | `012` |

## Erwartet fehlschlagende Referenztests

### FAILING_GAP-Liste

- Collection-Literale und Collection-Arten aus `t001.in`, `t002.in`, `t004.in`.
- `implies` in `examples/Documentation/Demo/Demo.use` und `civstat.use`.
- Iteratoren in `t002.in`, `Demo.use`, `CarRental.use` und `Tree.use`.
- `allInstances` in `t002.in` und `t038.in`.
- Navigation Chains und implizites collect in `t004.in`.
- `let`/`if` in `t004.in` und `civstat.use`.
- Typoperationen und Generalisierung in `t002.in`.
- null/invalid-/undefined-Semantik in `t001.in`.

### FAILING_FORMAT-Liste

- Wertausgaben wie `*-> Set{...} : Set(T)`, wenn der neue Evaluator bereits
  denselben fachlichen Wert als DTO liefert.
- Parserfehler aus `.fail`, deren alter Meldungstext nicht den neuen stabilen
  Error Codes entspricht.
- Contract-/Validation-Details, sofern fachlich vorhanden, aber anders gruppiert.

### FAILING_INFRASTRUCTURE-Liste

- Alle noch nicht importierbaren `.use`-Modelle.
- Importfälle unter `parser/imports/` und Shell-`imports/`.
- Operationstraces wie `t084.in`.
- Derived-/Init-Fälle ohne Objektinitialisierungs- bzw. Derived-Lifecycle.
- Shelltests, die vor der OCL-Abfrage SOIL/ASSL/Command-State aufbauen.

### NON_OCL_OR_SHELL_ONLY-Fälle

- GUI-Kommandos, Diagrammlayout (`.clt`, `.olt`) und reine Shelldarstellung.
- ASSL-Generatorläufe (`.assl`) außerhalb extrahierbarer OCL-Prädikate.
- State-Machine-Steuerung und Sequenzdiagrammkommandos.
- Shell-Hilfe, Pluginverwaltung oder Dateisystemkommandos ohne OCL-Semantik.

### UNCLEAR-Fälle

- USE-spezifische Extensions ohne normatives OCL-Pendant.
- Shellfälle mit mehreren Zustandsänderungen, deren relevante Assertion nicht
  eindeutig vom Command-Verhalten trennbar ist.
- Negative `.fail`-Fälle, deren beabsichtigte Fehlerphase noch nicht inventarisiert ist.

## Priorisierte Gap-Liste

| Rang | Gap-ID | Bereich | Priorität | Begründung | Roadmap |
|---:|---|---|---|---|---:|
| 1 | `OCL-GAP-017` | Reference-Harness/Inventar | sehr hoch | macht alle weiteren Fortschritte messbar | 3-8 |
| 2 | `OCL-GAP-015` | Source Locations/Error Codes | sehr hoch | muss vor neuen AST-Knoten stabil sein | 9 |
| 3 | `OCL-GAP-014` | null/invalid | sehr hoch | querschnittliche OCL-Semantik und Robustheit | 10 |
| 4 | `OCL-GAP-002/003/008` | Operatoren, Calls, Navigation | sehr hoch | Grundlage realer Expressions | 11 |
| 5 | `OCL-GAP-004/006` | Collection-Modell/-Literale | sehr hoch | Voraussetzung aller Iteratoren | 12 |
| 6 | `OCL-GAP-005/019/020/021` | Collection Operations | hoch | häufige Standardoperationen und Examples | 13-16 |
| 7 | `OCL-GAP-007` | Iteratoren | sehr hoch | größte fachliche Abdeckung | 17-22 |
| 8 | `OCL-GAP-009/010` | if/let/allInstances | hoch | strukturierte und globale Constraints | 23-25 |
| 9 | `OCL-GAP-013` | Vererbung/Typoperationen | hoch | zahlreiche Originalfälle hängen davon ab | 26-27 |
| 10 | `OCL-GAP-011/012` | zusätzliche OCL-Kontexte | post-MVP | benötigt Operations-/Zustandsmodell | 28-30 |
| 11 | `OCL-GAP-018` | vollständiger Modellimport | separat koordinieren | kein reines OCL-Problem | 2, 32 |

## Empfehlungen für die OCL Extension Roadmap

1. Roadmap-Schritt 2 soll dieses Dokument nicht mehr erstellen, sondern seine
   Gap-IDs validieren, inventarisierte Testfälle ergänzen und Statuszahlen erzeugen.
2. Schritte 3-8 müssen `OCL-GAP-017*` schließen, bevor Featurefortschritt anhand
   der originalen Testbasis behauptet wird.
3. Schritt 9 bleibt vor neuen Sprachfeatures: Source Ranges und stabile Codes
   dürfen bei Iterator-/Collection-Erweiterungen nicht nachträglich angebaut werden.
4. Schritt 11 muss `implies` und die robuste parameterlose Aufrufsyntax explizit
   aufnehmen; beide sind in realen Examples/Testfällen häufig.
5. Schritt 12 muss Collection-Literale und Collection-Kinds zusammen mit dem
   Typ-/Wertmodell behandeln. Ein generisches `Collection(T)` genügt nicht.
6. Jeder Feature-Schritt erhält eine feste `FAILING_GAP`-Auswahl und dokumentiert
   deren Übergang zu `PASSING`.
7. `OCL-GAP-018` wird mit Import-/UML-Roadmaps koordiniert. OCL-Fragmente dürfen
   für Referenztests isoliert werden, ohne vollständigen USE-Import vorzutäuschen.
8. Grüne, deterministische Referenzfälle werden gemäß Roadmap-Schritt 33 zusätzlich
   in die normale Regression übernommen.

## Offene Fragen

| Frage | Entscheidung nötig für |
|---|---|
| Welche OCL-Version und welche optionalen Compliance Points bilden das Zielprofil? | Abgrenzung der Gaps 27/31 |
| Welche USE-Dialektformen, etwa parameterlose Calls ohne `()`, werden kompatibel unterstützt? | Parserprofil und Statusbewertung |
| Wie werden erwartete Collectionwerte reihenfolgeunabhängig bzw. -abhängig normalisiert? | Shell-Reference-Harness |
| Wie wird ein minimales UML-/Snapshot-Fixture aus einer `.use`/`.in`-Datei erzeugt? | `FAILING_INFRASTRUCTURE` abbauen |
| Welche Import-Gaps werden im OCL-Programm gelöst und welche im ModelText-Importer? | `OCL-GAP-018` |
| Wann gilt ein PASSING-Fall als stabil genug für normale Regression? | Governance in Roadmap-Schritt 33 |

## Neubewertung nach B23

B23 hat die technischen Reference-Blocker vollstaendig neu bewertet. 117 der
vormals formatblockierten Faelle sind nun `PASSING`, acht zeigen einen echten
Sprach- oder Semantik-Gap und ein Explain-Fall ist reine Shellfunktionalitaet.
Von den vormals infrastrukturblockierten Faellen sind sechs `PASSING`, 91
zeigen nun einen fachlichen UML-/OCL-Gap und 58 betreffen ausschliesslich alte
Modell-, Import-, Shell- oder Generatorinfrastruktur. Dadurch verbleiben 543
`FAILING_GAP`, aber keine `FAILING_FORMAT`- oder
`FAILING_INFRASTRUCTURE`-Faelle. Diese 543 Faelle werden in B24 fachlich
priorisiert; B23 hat keine produktive Semantik vorweggenommen.

## Zusammenfassung

Das aktuelle Backend besitzt eine saubere, eigenständige OCL-Pipeline und ein
für das MVP nützliches Subset. Besonders positiv sind die separaten Lexer-,
Parser-, AST-, Typechecker- und Evaluator-Komponenten, Source Ranges sowie das
UI-fähige Validation Mapping.

Die größten fachlichen Lücken liegen im Collection-Typ-/Wertmodell, in
Collection-Literalen und -Operationen, Iteratoren, `implies`, Navigation Chains,
`let`, `if`, `allInstances`, null/invalid-Semantik und zusätzlichen OCL-Kontexten.
Die originale Parser- und Shell-Testbasis belegt diese Gaps konkret.

Da das Reference-Test-Harness noch fehlt, sind die Statuswerte in dieser Datei
Prognosen. Die Roadmap muss zuerst alle extrahierbaren Fälle ausführbar und ihre
Fehlschläge sichtbar machen. `FAILING_GAP` ist dabei kein Qualitätsmangel des
Testprogramms, sondern der messbare Arbeitsvorrat für die OCL-Erweiterung.
