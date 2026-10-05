# Original Project Overview

## Zweck dieser Datei

Diese Datei gibt einen strukturierten Überblick über das originale USE-Projekt im Workspace als Referenzpunkt für das neue UML/OCL-Websystem.

Sie dokumentiert:

- was das originale Projekt grundsätzlich ist,
- welche Bereiche fachlich relevant sind,
- welche Repository-Teile als Syntax-, Verhalten- oder Testfallreferenz dienen können,
- welche Bereiche nur historisch oder technisch interessant sind,
- welche Teile nicht übernommen werden sollen,
- wie die Erkenntnisse in Backend, Frontend, Teststrategie und Planung des neuen Systems einfließen.

Wichtig: Diese Datei ist keine Migrationsplanung. Das originale USE-Projekt ist Referenz, aber keine technische Grundlage.

## Rolle des originalen USE-Projekts

USE steht für "UML-based Specification Environment". Laut `use/README.md` ist USE ein System zur Spezifikation von Informationssystemen auf Basis eines UML-Subsets. Modelle werden textuell beschrieben, OCL-Ausdrücke definieren zusätzliche Integritätsbedingungen, Systemzustände beziehungsweise Snapshots können erzeugt und gegen Constraints geprüft werden.

Für das neue Websystem ist USE fachlich besonders relevant, weil es genau die zentralen Konzepte verbindet, die auch im MVP benötigt werden:

- UML-Klassenmodelle,
- Assoziationen und Multiplizitäten,
- OCL-Invarianten,
- Objektzustände/Snapshots,
- Constraint Checking,
- OCL-Auswertung gegen konkrete Objektzustände,
- Beispielmodelle und Testfälle.

Die Rolle ist jedoch klar begrenzt:

> Das originale USE-Projekt wird untersucht, aber nicht geforkt, nicht migriert und nicht als Runtime Dependency verwendet.

Die neue Anwendung soll ein eigenständiges Backend, ein eigenständiges Frontend, ein eigenes Domänenmodell, eine eigene OCL-Verarbeitung und eine eigene Validierungslogik erhalten.

## Grundlegender Projektüberblick

Das originale Projekt liegt im Workspace unter `../use/`.

Aus der Analyse der Build- und Quellstruktur ergibt sich:

| Eigenschaft | Beobachtung |
|---|---|
| Projekttyp | Java-basiertes Maven-Multi-Modul-Projekt |
| Version laut Root-`pom.xml` | `7.5.0` |
| Java-Version laut Maven-Konfiguration | `21` |
| Hauptmodule | `use-core`, `use-gui`, `use-assembly` |
| Parser-Technologie | ANTLR 3 über `antlr-runtime` und `antlr3-maven-plugin` |
| UI-Technologien | Swing- und JavaFX-basierte Desktop-GUI |
| Zentrale Fachbereiche | UML-Metamodell, Systemzustände, OCL, Parser, Shell-/SOIL-Kommandos |
| Beispielmaterial | Viele `.use`, `.cmd`, `.invs`, `.testsuite` Dateien unter `use-core/src/main/resources/examples/` und Testressourcen |

Die README beschreibt USE als Werkzeug, in dem textuelle Spezifikationen geladen, Snapshots erzeugt und OCL-Constraints automatisch geprüft werden. Dieser fachliche Ablauf ist für das neue Websystem relevant, auch wenn die technische Umsetzung neu entstehen soll.

## Repository-Struktur

| Bereich/Pfad | Zweck im Original | Relevanz für neues System | Nutzungskategorie | Bemerkung |
|---|---|---|---|---|
| `../use/pom.xml` | Maven-Root-Projekt mit Modulen `use-assembly`, `use-core`, `use-gui`. | Zeigt grobe Modultrennung im Original. | historische/technische Orientierung | Keine Übernahme der Maven-Struktur geplant. |
| `../use/use-core/` | Kernlogik für UML, OCL, Parser, Systemzustände, Shell/SOIL, Beispiele und Tests. | Wichtigster Analysebereich. | fachliche Referenz | Nicht als Dependency verwenden. |
| `../use/use-gui/` | Desktop-GUI mit Swing/JavaFX, Diagrammansichten, Shell-Integration. | Hilfreich zur Einordnung alter UI-Konzepte. | historische/technische Orientierung | Nicht in React migrieren. |
| `../use/use-assembly/` | Verpackung/Distribution. | Für neues Websystem kaum relevant. | nicht übernehmen | Kein Bezug zum MVP. |
| `../use/manual/` | Markdown-Dokumentation, Quick Tour, Entwicklerhinweise, Plugin-Hinweise. | Nützlich für fachliche Abläufe und Nutzerbegriffe. | fachliche Referenz | Qualität laut README teilweise durch automatische Konvertierung eingeschränkt. |
| `../use/documentation/` und `../use/docs/` | Ergänzende Dokumentation. | Kann für Kontext geprüft werden. | später prüfen | Nicht primäre Quelle für MVP-Analyse. |
| `../use/README.md` | Projektbeschreibung, Getting Started, zentrale Konzepte. | Gute Einstiegreferenz. | fachliche Referenz | Enthält kompakte Beschreibung von Modellen, Snapshots und OCL. |
| `../use/README.OCL` | Hinweise zu OCL-Semantik und USE-Erweiterungen. | Relevant für OCL-Abgrenzung. | Syntaxreferenz / Verhaltenreferenz | Enthält z. B. Hinweise zu existential invariants und mehreren Invariantenvariablen. |
| `../use/ChangeLog`, `../use/NEWS` | Historie des Originalprojekts. | Nur bei Detailfragen zu Verhalten/Entwicklung relevant. | historische/technische Orientierung | Nicht für MVP-Scope maßgeblich. |

## Build- und Modulstruktur

Das originale Projekt ist ein Maven-Multi-Modul-Projekt.

| Modul | Pfad | Zweck | Relevanz |
|---|---|---|---|
| Root | `../use/pom.xml` | Aggregiert `use-assembly`, `use-core`, `use-gui`. | Zeigt, dass fachlicher Kern und GUI getrennte Module sind. |
| Core | `../use/use-core/pom.xml` | Enthält Fachlogik, Parsergenerierung, Tests und Beispiele. | Hauptquelle für Domänen-, OCL- und Validierungsanalyse. |
| GUI | `../use/use-gui/pom.xml` | Hängt von `use-core` ab und enthält Swing/JavaFX-Desktop-GUI. | UI-Verhalten teilweise interessant, technische Umsetzung nicht übernehmen. |
| Assembly | `../use/use-assembly/pom.xml` | Verpackung der Anwendung. | Für neues Websystem nicht relevant. |

Im `use-core`-Build werden Grammatikfragmente zu ANTLR-Grammatiken zusammengeführt. Beispiele:

- `src/main/resources/grammars/ocl/OCL.gpart`
- `src/main/resources/grammars/use/USE.gpart`
- `src/main/resources/grammars/base/OCLBase.gpart`
- `src/main/resources/grammars/base/OCLLexerRules.gpart`
- `src/main/resources/grammars/soil/Soil.gpart`
- `src/main/resources/grammars/shell/ShellCommand.gpart`

Diese Build-Logik ist für das neue System nicht direkt zu übernehmen. Sie ist aber eine wichtige Syntaxreferenz für die Analyse von `.use`-Dateien und OCL-Ausdrücken.

## Relevante fachliche Bereiche

| Bereich/Pfad | Zweck im Original | Relevanz für neues System | Nutzungskategorie | Bemerkung |
|---|---|---|---|---|
| `use-core/src/main/java/org/tzi/use/uml/mm/` | UML-Modellstruktur: Klassen, Attribute, Operationen, Assoziationen, Multiplizitäten, Invarianten. | Sehr hoch. | fachliche Referenz | Wichtig für neues Domänenmodell, aber keine Codeübernahme. |
| `use-core/src/main/java/org/tzi/use/uml/sys/` | Laufzeit-/Systemzustände: Objekte, Objektzustände, Links, Linksets, Snapshots, Systemaktionen. | Sehr hoch. | fachliche Referenz / Verhaltenreferenz | Wichtig für Objektdiagramm und Snapshot-Konzept. |
| `use-core/src/main/java/org/tzi/use/uml/ocl/` | OCL-Ausdrücke, Typen, Werte, Standardoperationen, Evaluation. | Sehr hoch. | Syntaxreferenz / Verhaltenreferenz | Wichtig für OCL-Pipeline des neuen Backends. |
| `use-core/src/main/java/org/tzi/use/parser/` | Parser-Kontext, Fehlerbehandlung, AST, USE-/OCL-/SOIL-Compiler. | Hoch. | Syntaxreferenz | Relevanz für eigene Parser-Konzeption. |
| `use-core/src/main/resources/grammars/` | ANTLR-Grammatikfragmente für USE, OCL, SOIL, Shell, Testsuite. | Hoch. | Syntaxreferenz | Als Referenz für Syntax, nicht als technische Basis. |
| `use-core/src/main/resources/examples/` | Beispielmodelle, Kommandodateien, Invarianten, Testsuites. | Hoch. | Testfallquelle | Besonders relevant für MVP-Beispielmodelle und spätere Testbibliothek. |
| `use-core/src/test/` und `use-core/src/it/` | Unit- und Integrationstests. | Mittel bis hoch. | Testfallquelle / Verhaltenreferenz | Geeignet zur Ableitung von Testfällen. |

## Relevante technische Bereiche

| Bereich/Pfad | Zweck im Original | Relevanz für neues System | Nutzungskategorie | Bemerkung |
|---|---|---|---|---|
| `org.tzi.use.parser.ocl.OCLCompiler` | Kompiliert OCL-Ausdrücke aus Text. | Relevant für konzeptionelle OCL-Pipeline. | Syntaxreferenz | Nicht übernehmen, aber Ablauf analysieren. |
| `org.tzi.use.parser.use.USECompiler` | Kompiliert `.use`-Spezifikationen. | Relevant für spätere `.use` Import-Idee. | Syntaxreferenz / später prüfen | Im MVP ist JSON-Projektformat wichtiger. |
| `org.tzi.use.parser.ParseErrorHandler` | Sammlung/Ausgabe von Parserfehlern. | Relevant für Fehlerstruktur. | Verhaltenreferenz | Neues System braucht strukturierte JSON-Fehler. |
| `org.tzi.use.parser.SemanticException` | Semantische Fehler im Parsing/Kompilieren. | Relevant für Error Contract. | Verhaltenreferenz | Fehlerarten als Referenz, nicht Klassen übernehmen. |
| `org.tzi.use.uml.ocl.expr.Evaluator` | Wertet OCL-Expression-Objekte gegen Systemzustand aus. | Sehr relevant für eigenes Evaluator-Konzept. | Verhaltenreferenz | Nicht als Runtime Engine verwenden. |
| `org.tzi.use.uml.ocl.expr.EvalContext` | Kontext für OCL-Auswertung. | Relevant für Snapshot-basierte Evaluation. | fachliche Referenz | Neues Backend braucht eigenes EvalContext-Konzept. |
| `org.tzi.use.uml.ocl.expr.MultiplicityViolationException` | Fehler bei Navigation/Multiplicity-Verstößen. | Relevant für Fehlerarten. | Verhaltenreferenz | Für strukturiertes Validation Result adaptieren. |
| `org.tzi.use.uml.ocl.type.*` | OCL-Typsystem. | Relevant für Typechecker-Konzept. | fachliche Referenz | MVP-Subset deutlich kleiner halten. |
| `org.tzi.use.uml.ocl.value.*` | OCL-Werte: Boolean, Integer, Real, String, Collections, Objects. | Relevant für eigenes Value Model. | fachliche Referenz | Keine Klassenübernahme. |

## UML-Modellrepräsentation

Die zentrale UML-Modellrepräsentation liegt unter:

`../use/use-core/src/main/java/org/tzi/use/uml/mm/`

Wichtige Klassen und Konzepte:

| Klasse/Datei | Zweck im Original | Relevanz für neues System | Nutzungskategorie |
|---|---|---|---|
| `MModel.java` | Repräsentiert ein gesamtes Modell. | Grundlage für eigenes Projekt-/Modelldomänenobjekt. | fachliche Referenz |
| `MClass.java`, `MClassImpl.java` | UML-Klasse. | Direkt relevant für Klassendiagramm. | fachliche Referenz |
| `MAttribute.java` | Klassenattribute. | Direkt relevant für Attribute mit primitiven Typen. | fachliche Referenz |
| `MOperation.java` | Operationen. | Relevant als Signaturen im MVP, ausführbare Semantik später. | fachliche Referenz |
| `MAssociation.java`, `MAssociationImpl.java` | Assoziationen. | Direkt relevant für Beziehungen im Klassendiagramm. | fachliche Referenz |
| `MAssociationEnd.java` | Assoziationsenden, Rollen, Multiplizitäten, Navigierbarkeit. | Sehr relevant für Rollen und Multiplicity Checks. | fachliche Referenz |
| `MMultiplicity.java` | Multiplizitätsmodell. | Sehr relevant für MVP-Validierung. | fachliche Referenz / Verhaltenreferenz |
| `MClassInvariant.java` | OCL-Invarianten im Kontext einer Klasse. | Sehr relevant für Invariant-Konzept. | fachliche Referenz / Verhaltenreferenz |
| `MPrePostCondition.java` | Pre-/Postconditions. | Post-MVP-relevant. | später prüfen |
| `MAssociationClass.java` | Assoziationsklassen. | Post-MVP-relevant. | später prüfen |
| `MGeneralization.java` | Vererbung. | Post-MVP-relevant. | später prüfen |
| `MAggregationKind.java` | Aggregation/Komposition. | Post-MVP-relevant. | später prüfen |

Beobachtung: Das Originalmodell ist deutlich breiter als der MVP. Für das neue System sollten nur die MVP-relevanten Konzepte initial übernommen werden: Klassen, Attribute, Operationssignaturen, Assoziationen, Rollen, Multiplizitäten und Invarianten.

## OCL-bezogene Bereiche

Die OCL-bezogenen Bereiche verteilen sich auf Parser, Ausdrucksmodell, Typen, Werte und Standardoperationen.

| Bereich/Pfad | Zweck im Original | Relevanz für neues System | Nutzungskategorie | Bemerkung |
|---|---|---|---|---|
| `use-core/src/main/resources/grammars/ocl/OCL.gpart` | OCL-Grammatikfragment. | Syntaxreferenz. | Syntaxreferenz | Nur für Analyse, nicht kopieren. |
| `use-core/src/main/resources/grammars/base/OCLBase.gpart` | Gemeinsame OCL-Regeln. | Syntaxreferenz. | Syntaxreferenz | Für MVP-Subset prüfen. |
| `use-core/src/main/resources/grammars/base/OCLLexerRules.gpart` | Lexer-Regeln. | Syntaxreferenz. | Syntaxreferenz | Hilfreich für Tokenisierung. |
| `use-core/src/main/java/org/tzi/use/parser/ocl/` | AST-Klassen und `OCLCompiler`. | Parser-/AST-Referenz. | Syntaxreferenz | Enthält viele Features außerhalb MVP. |
| `use-core/src/main/java/org/tzi/use/uml/ocl/expr/` | OCL-Ausdrucksmodell und Evaluator. | Sehr relevant. | Verhaltenreferenz | Referenz für AST-/Evaluator-Konzepte. |
| `use-core/src/main/java/org/tzi/use/uml/ocl/type/` | OCL-Typsystem. | Relevant für Typechecker. | fachliche Referenz | MVP-Typsystem kleiner halten. |
| `use-core/src/main/java/org/tzi/use/uml/ocl/value/` | OCL-Werte und Collections. | Relevant für Evaluator-Ergebnisse. | fachliche Referenz | Für JSON-Resultate neu modellieren. |
| `use-core/src/main/java/org/tzi/use/uml/ocl/expr/operations/` | Standardoperationen für Boolean, Number, String, Collection usw. | Relevant für MVP-Operationen `size`, `isEmpty`, `notEmpty`. | Verhaltenreferenz | Nur ausgewählte Operationen im MVP. |
| `../use/README.OCL` | Hinweise zu OCL-Semantik und USE-Erweiterungen. | Relevant für Abgrenzung und spätere Erweiterungen. | Verhaltenreferenz | Enthält Features außerhalb MVP. |

Konkrete OCL-Ausdrucksklassen, die als Referenz für das MVP und spätere Erweiterungen interessant sind:

- `Expression.java`
- `ExpVariable.java`
- `ExpAttrOp.java`
- `ExpNavigation.java`
- `ExpStdOp.java`
- `ExpConstString.java`
- `ExpConstInteger.java`
- `ExpConstReal.java`
- `ExpConstBoolean.java`
- `ExpForAll.java`
- `ExpExists.java`
- `ExpSelect.java`
- `ExpCollect.java`
- `ExpLet.java`
- `ExpIf.java`
- `ExpAllInstances.java`

Für das MVP sind besonders `self`, Attributzugriff, einfache Association Navigation, Literale, Vergleichsoperatoren, Boolean-Operatoren und einfache Collection-Operationen relevant.

## Validierungs- und Evaluationsbereiche

Die wichtigste Klasse für den kombinierten Validierungsablauf ist:

`use-core/src/main/java/org/tzi/use/uml/sys/MSystemState.java`

In `MSystemState` wurden folgende relevante Methoden und Verhaltensbereiche gefunden:

| Bereich/Klasse | Zweck im Original | Relevanz für neues System | Nutzungskategorie | Bemerkung |
|---|---|---|---|---|
| `MSystemState.check(...)` | Prüft Struktur und Invarianten eines Systemzustands. | Sehr hoch. | Verhaltenreferenz | Entspricht fachlich `Check Constraints`. |
| `MSystemState.checkStructure(...)` | Prüft modellinhärente Constraints, u. a. Multiplizitäten. | Sehr hoch. | Verhaltenreferenz | Grundlage für Multiplicity Checks. |
| `MSystemState.checkStructure(MAssociation, ...)` | Prüft einzelne Assoziation. | Hoch. | Verhaltenreferenz | Relevant für Objektlinks und Rollen. |
| `MSystemState.reportMultiplicityViolation(...)` | Meldet Multiplizitätsverletzungen. | Hoch. | Verhaltenreferenz | Fehlertext als Referenz für neue strukturierte Fehler. |
| `MSystemState.checkWholePartLink(...)` | Prüft Whole/Part-Hierarchien. | Post-MVP. | später prüfen | Relevant bei Aggregation/Komposition. |
| `MClassInvariant.expandedExpression()` | Expandiert Invariante zu global auswertbarem Ausdruck. | Hoch. | Verhaltenreferenz | USE nutzt intern `allInstances()->forAll(...)` bzw. `exists`. |
| `MClassInvariant.getExpressionForViolatingInstances()` | Erzeugt Ausdruck für verletzende Instanzen. | Hoch. | Verhaltenreferenz | Sehr relevant für Fehler-Markierung im Objektdiagramm. |
| `Evaluator.eval(...)` | Wertet einen OCL-Ausdruck aus. | Sehr hoch. | Verhaltenreferenz | Referenz für eigenen Evaluator. |
| `Evaluator.evalList(...)` | Wertet mehrere Ausdrücke, optional parallel, aus. | Mittel. | historische/technische Orientierung | Parallelisierung nicht MVP-kritisch. |
| `MultiplicityViolationException` | Signalisiert Multiplicity-Probleme während Evaluation/Navigation. | Hoch. | Verhaltenreferenz | Für Error Contract prüfen. |

Wichtige fachliche Beobachtung: USE prüft zuerst strukturelle Modell-/Snapshot-Regeln wie Multiplizitäten und anschließend OCL-Invarianten. Dieses Muster ist für das neue Backend sinnvoll:

```text
Snapshot
-> Strukturprüfung
-> OCL-Invariantenprüfung
-> strukturierte Validation Results
-> visuelle Markierung im Frontend
```

## Snapshot- und Objektzustandsbereiche

Die Snapshot-/Objektzustandslogik liegt vor allem unter:

`../use/use-core/src/main/java/org/tzi/use/uml/sys/`

| Klasse/Datei | Zweck im Original | Relevanz für neues System | Nutzungskategorie |
|---|---|---|---|
| `MSystem.java` | Laufendes System mit Modell, aktuellem Zustand und Operationen. | Referenz für System-/Projektkontext. | fachliche Referenz |
| `MSystemState.java` | Konkreter Systemzustand/Snapshot mit Objekten und Links. | Sehr relevant. | fachliche Referenz / Verhaltenreferenz |
| `MObject.java`, `MObjectImpl.java` | Objektinstanzen einer Klasse. | Direkt relevant für Objektdiagramm. | fachliche Referenz |
| `MObjectState.java` | Objektzustand mit Attributwerten. | Direkt relevant für Slots/Attributwerte. | fachliche Referenz |
| `MInstance.java`, `MInstanceState.java` | Gemeinsame Instanzabstraktion. | Relevant für eigenes Modell, aber vereinfachbar. | fachliche Referenz |
| `MLink.java`, `MLinkImpl.java` | Objektlink einer Assoziation. | Direkt relevant für Objektlinks. | fachliche Referenz |
| `MLinkEnd.java` | Link-Ende mit Objekt und Assoziationsende. | Relevant für Linkmodell. | fachliche Referenz |
| `MLinkSet.java` | Sammlung von Links pro Assoziation. | Relevant für Navigation und Multiplicity Checks. | Verhaltenreferenz |
| `MDataTypeValue.java`, `MDataTypeValueState.java` | Datentypwerte. | Nicht MVP-kritisch. | später prüfen |
| `events/*` | Ereignisse bei Objekt-/Link-/Attributänderungen. | Für Web-MVP nicht zentral. | historische/technische Orientierung |

Für das neue Websystem sollte die Snapshot-Idee fachlich übernommen werden, aber in einem eigenen JSON-fähigen Modell:

- `Project`,
- `UmlModel`,
- `ClassDiagram`,
- `ObjectDiagram` oder `Snapshot`,
- `ObjectInstance`,
- `Slot`,
- `ObjectLink`,
- `ValidationResult`.

## GUI- und Desktop-Bereiche

Die GUI liegt unter:

`../use/use-gui/src/main/java/org/tzi/use/`

Besonders auffällige Bereiche:

| Bereich/Pfad | Zweck im Original | Relevanz für neues System | Nutzungskategorie | Bemerkung |
|---|---|---|---|---|
| `use-gui/src/main/java/org/tzi/use/gui/views/diagrams/classdiagram/` | Desktop-Klassendiagramm. | Fachliche UI-Orientierung möglich. | historische/technische Orientierung | Nicht nach React migrieren. |
| `use-gui/src/main/java/org/tzi/use/gui/views/diagrams/objectdiagram/` | Desktop-Objektdiagramm. | Fachliche UI-Orientierung möglich. | historische/technische Orientierung | Neue Screenshots sind wichtiger. |
| `use-gui/src/main/java/org/tzi/use/gui/views/evalbrowser/` | Evaluation Browser. | Interessant für Darstellung von OCL-Auswertung. | später prüfen | Kann Ideen für Debugging liefern. |
| `use-gui/src/main/java/org/tzi/use/gui/views/selection/` | Auswahlansichten. | Geringe bis mittlere Relevanz. | historische/technische Orientierung | Neues Frontend erhält eigene Sidebar/Panels. |
| `use-gui/src/main/java/org/tzi/use/gui/views/diagrams/behavior/` | Sequenz-/Kommunikationsdiagramme. | Nicht im Zielumfang. | nicht übernehmen | Außerhalb MVP. |
| `use-gui/src/main/java/org/tzi/use/gui/views/diagrams/statemachine/` | State-Machine-Diagramme. | Nicht im Zielumfang. | nicht übernehmen | Außerhalb MVP. |
| `use-gui/src/main/java/org/tzi/use/main/gui/swing/` | Swing-Einstieg. | Keine Relevanz für Web-UI. | nicht übernehmen | Alte Desktop-GUI nicht migrieren. |
| `use-gui/src/main/java/org/tzi/use/main/gui/fx/` | JavaFX-Einstieg. | Keine Relevanz für Web-UI. | nicht übernehmen | Neue React-UI statt JavaFX. |

Für das neue Frontend sind die neuen Screenshots unter `assets/screenshots/` maßgeblicher als die alte USE-GUI. Die Original-GUI kann höchstens helfen, fachliche Begriffe oder alte Interaktionsmuster zu verstehen.

## Beispiele, Tests und Dokumentation

Das Originalprojekt enthält umfangreiches Beispiel- und Testmaterial.

Gefundene Größenordnung:

| Bereich | Gefundene Dateien |
|---|---:|
| `.use` Dateien unter `use-core/src/main/resources/examples/` | 85 |
| `.cmd` Dateien unter `use-core/src/main/resources/examples/` | 185 |
| `.use` Dateien unter `use-core/src/test/resources/` | 57 |
| `.use` Dateien unter `use-gui/src/it/resources/testfiles/` | 145 |

Diese Zahlen sind Ergebnis einer lokalen Dateisuche mit `rg --files` im Workspace.

Relevante Beispielbereiche:

| Bereich/Pfad | Zweck im Original | Relevanz für neues System | Nutzungskategorie | Bemerkung |
|---|---|---|---|---|
| `use-core/src/main/resources/examples/Documentation/Demo/Demo.use` | Einfaches Company-Modell mit Klassen, Assoziationen und Invarianten. | Sehr hoch. | Testfallquelle | Guter Kandidat für MVP-Demo oder reduzierte Variante. |
| `use-core/src/main/resources/examples/Documentation/Employee/Employee.use` | Dokumentationsbeispiel. | Hoch. | Testfallquelle | Für Klassendiagramm/Objektdiagramm prüfen. |
| `use-core/src/main/resources/examples/Documentation/HowToCheckUMLAndOCLModelsWithUSE/` | Beispiele zur Prüfung von UML/OCL-Modellen. | Hoch. | Verhaltenreferenz / Testfallquelle | Besonders relevant für Validierungsabläufe. |
| `use-core/src/main/resources/examples/Papers/2006/GogollaBuettnerRichters/` | Umfangreiche Forschungs-/Publikationsbeispiele, u. a. `civstat.use`, `bigamy.invs`, `AllTests.testsuite`. | Mittel bis hoch. | Testfallquelle | Teilweise zu komplex für MVP. |
| `use-core/src/main/resources/examples/Others/Subsets/` | Beispiele für subsets/redefines/composition. | Post-MVP. | später prüfen | Relevant für spätere Erweiterungen. |
| `use-core/src/main/resources/examples/StateMachines/` | State-Machine-Beispiele. | Nicht MVP-relevant. | nicht übernehmen | Diagrammtyp nicht im Zielumfang. |
| `use-core/src/test/java/org/tzi/use/uml/mm/` | Tests für Modellstruktur, Multiplizität, Assoziationsklassen. | Hoch. | Testfallquelle | Für Backend-Teststrategie prüfen. |
| `use-core/src/test/java/org/tzi/use/uml/sys/` | Tests für Systemzustände, Links, Objekterzeugung. | Hoch. | Testfallquelle | Relevant für Snapshot-Validierung. |
| `use-core/src/test/java/org/tzi/use/uml/ocl/expr/` | Tests für OCL-Ausdrücke, Navigation, Evaluation. | Hoch. | Testfallquelle | Relevant für OCL-MVP-Subset. |
| `use-core/src/it/java/org/tzi/use/OCLExpressionIT.java` | Integrationstest für OCL-Ausdrücke. | Hoch. | Testfallquelle | Für OCL-Testfälle prüfen. |
| `manual/main.md` | Anwenderdokumentation/Quick Tour. | Mittel. | fachliche Referenz | Hilft bei User Journey und Begriffen. |
| `manual/developer.md` | Entwicklerdokumentation. | Niedrig bis mittel. | historische/technische Orientierung | Keine neue Architektur daraus ableiten. |
| `manual/testsuite.md` | Testsuite-Dokumentation. | Mittel. | Testfallquelle | Für spätere Testfallbibliothek prüfen. |

Ein konkretes Beispiel ist `Documentation/Demo/Demo.use`. Es enthält Klassen wie `Employee`, `Department`, `Project`, Assoziationen wie `WorksIn`, `WorksOn`, `Controls` und OCL-Invarianten mit Navigation und `size`. Dieses Beispiel passt fachlich gut zum Zielsystem, muss für das MVP aber auf das unterstützte OCL-Subset reduziert werden, falls es Features wie `forAll` oder `includesAll` nutzt.

## Was als Referenz genutzt werden sollte

Folgende Aspekte sollten aktiv ausgewertet werden:

| Referenzaspekt | Quelle | Nutzung im neuen System |
|---|---|---|
| Grundkonzepte von Klassen, Attributen, Operationen, Assoziationen und Multiplizitäten | `uml/mm/*` | Domänenmodell und JSON-Projektformat |
| Objektzustände, Attributwerte und Links | `uml/sys/*` | Snapshot- und Objektdiagramm-Konzept |
| Invariantenmodell | `MClassInvariant.java` | OCL-Invariant-Konzept und Validation Results |
| Strukturprüfung | `MSystemState.checkStructure(...)` | Multiplicity Checks im Backend |
| Invariantenprüfung | `MSystemState.check(...)` | `Check Constraints` Ablauf |
| OCL-Ausdrucksmodell | `uml/ocl/expr/*` | Eigene AST-/Evaluator-Konzeption |
| OCL-Typen und Werte | `uml/ocl/type/*`, `uml/ocl/value/*` | Typechecker und Value Model |
| OCL-/USE-Grammatik | `resources/grammars/*` | Syntaxanalyse, spätere `.use`-Kompatibilität |
| Parser-/Semantikfehler | `parser/*` | Error Contract und strukturierte Fehler |
| Beispielmodelle | `resources/examples/*` | Demo, Testfälle, Akzeptanzszenarien |
| Tests | `src/test/*`, `src/it/*` | Backend-Teststrategie |

## Was nicht übernommen werden sollte

Folgende Bereiche sollen nicht direkt übernommen werden:

| Bereich | Warum nicht übernehmen? | Alternative im neuen System |
|---|---|---|
| USE-Core als Dependency | Das Projekt soll unabhängig und neu entstehen. | Eigenes Spring-Boot-Backend mit eigenem Domänenmodell. |
| USE-Code als Fork | Würde technische Migration statt Neuentwicklung erzeugen. | Fachliche Erkenntnisse dokumentieren und neu modellieren. |
| ANTLR-3-Grammatiken als technische Basis | Alte Build-/Parsertechnologie und großer Featureumfang. | Eigene OCL-Pipeline für MVP-Subset, später erweiterbar. |
| Swing-/JavaFX-GUI | Ziel ist ein modernes Webfrontend. | React/TypeScript-Frontend. |
| Shell-/SOIL-Kommandos als Produktkern | MVP fokussiert Modellierung und Validierung über Web-UI. | Eigene Web-Interaktionen und REST/JSON API. |
| Sequenzdiagramm-/State-Machine-Bereiche | Nicht Teil des Zielumfangs. | Nicht implementieren. |
| ASSL/Generatorbereiche | Nicht Teil des MVP. | Später nur prüfen, falls Testdatengenerierung relevant wird. |
| Plugin-/Runtime-System | Für MVP nicht erforderlich. | Schlanke Backend-/Frontend-Architektur. |
| Alte Fehlerausgabe als Textformat | Websystem braucht strukturierte Ergebnisse. | JSON-basiertes Error- und Validation-Result-Modell. |

## Bedeutung für das neue Backend

Für das Backend ist das originale USE-Projekt vor allem fachliche und semantische Referenz.

Wichtige Ableitungen:

- Das Backend sollte ein eigenes Domänenmodell für Klassen, Attribute, Operationen, Assoziationen, Rollen, Multiplizitäten, Invarianten, Objekte, Slots und Links definieren.
- `MSystemState.check(...)` ist ein wichtiger Referenzpunkt für den Ablauf von `Check Constraints`.
- Strukturprüfungen sollten vor oder gemeinsam mit OCL-Invarianten geprüft werden.
- OCL sollte über eine Pipeline aus Lexer, Parser, AST, Typechecker, Evaluator und Validation Result verarbeitet werden.
- Fehler sollten nicht als reine Textausgabe modelliert werden, sondern als strukturierte Ergebnisse mit Elementbezug.
- Beispielmodelle aus USE können als Grundlage für Backend-Tests dienen, müssen aber auf MVP-Features reduziert oder kategorisiert werden.

Nicht abzuleiten ist eine direkte Klassenstruktur. Das neue Backend darf nicht versuchen, `MModel`, `MSystemState` oder `Evaluator` technisch nachzubauen. Es sollte die fachlichen Konzepte verstehen und in eine moderne, webfähige Architektur übersetzen.

## Bedeutung für das neue Frontend

Für das Frontend ist das originale USE-Projekt nur eingeschränkt relevant.

Relevante Punkte:

- Die Existenz von Class Diagram View und Object Diagram View bestätigt die fachliche Trennung zwischen Modellstruktur und Snapshot.
- Die alte GUI kann Hinweise geben, welche Informationen in Diagrammen sichtbar sein können.
- Der Evaluation Browser kann langfristig Ideen für OCL-Debugging oder detaillierte Auswertung liefern.

Nicht relevant für die Umsetzung:

- Swing-/JavaFX-Komponenten,
- Desktop-Fensterlogik,
- alte Layout- und Diagrammimplementierung,
- Shell-zentrierte Bedienung,
- alte Plugin-/Runtime-Struktur.

Für das neue Frontend sind die neuen Screenshots unter `assets/screenshots/` maßgeblich. Das Originalprojekt dient höchstens als ergänzende fachliche Referenz.

## Bedeutung für Teststrategie und Planung

Das originale USE-Projekt ist eine wichtige Quelle für Testfälle und Verhaltenserwartungen.

Potenzielle Testquellen:

- `.use`-Modelle aus `use-core/src/main/resources/examples/`,
- `.cmd`-Dateien mit Objekt-/Link-Aufbau,
- `.invs`-Dateien mit zusätzlichen Invarianten,
- `.testsuite`-Dateien,
- Parser-Testressourcen unter `use-core/src/test/resources/org/tzi/use/parser/`,
- Unit-Tests unter `use-core/src/test/java/org/tzi/use/uml/mm/`,
- Unit-Tests unter `use-core/src/test/java/org/tzi/use/uml/sys/`,
- OCL-Tests unter `use-core/src/test/java/org/tzi/use/uml/ocl/`.

Für die Planung sollte jedes Beispielmodell einer Kategorie zugeordnet werden:

| Kategorie | Beschreibung |
|---|---|
| MVP-kompatibel | Kann mit dem MVP-OCL-Subset und MVP-UML-Scope abgebildet werden. |
| MVP-kompatibel nach Reduktion | Enthält einzelne Post-MVP-Features, kann aber vereinfacht werden. |
| Post-MVP | Benötigt Features wie `forAll`, `exists`, Vererbung, Aggregation, Pre-/Postconditions oder `.use` Import. |
| Nicht Zielumfang | Bezieht sich auf State Machines, Sequenzdiagramme, Generatoren oder Modellanimation außerhalb des MVP. |

Diese Kategorisierung sollte später in einer Testfallbibliothek dokumentiert werden.

## Offene Fragen

- Welche `.use`-Beispiele sind klein genug, um direkt als MVP-Demo zu dienen?
- Soll `Documentation/Demo/Demo.use` reduziert als Standardbeispiel verwendet werden oder wird ein eigenes Library-Beispielmodell erstellt?
- Welche USE-Fehlermeldungen sollen als Vorlage für neue strukturierte Fehlerkategorien dienen?
- Wie stark soll spätere `.use` Import-/Export-Kompatibilität an der Originalgrammatik orientiert sein?
- Welche OCL-Operationen aus `StandardOperationsCollection` sind für den MVP zwingend nötig?
- Wie sollen USE-Konzepte wie existential invariants oder mehrere Invariantenvariablen eingeordnet werden: Post-MVP oder Nicht-Ziel?
- Welche Tests aus `use-core/src/test/java/org/tzi/use/uml/ocl/expr/` passen exakt zum MVP-OCL-Subset?
- Welche Teile von SOIL-Kommandodateien können als Vorlage für Snapshot-Aufbau dienen, ohne SOIL selbst zu übernehmen?

## Zusammenfassung

Das originale USE-Projekt ist ein umfangreiches Java-basiertes UML/OCL-Werkzeug mit Maven-Multi-Modul-Struktur, eigenem UML-Metamodell, OCL-Parser, OCL-Auswertung, Systemzuständen, Desktop-GUI, Beispielen und Tests.

Für das neue Websystem ist vor allem `use-core` fachlich relevant. Besonders wichtig sind:

- `uml/mm` für Klassenmodell-Konzepte,
- `uml/sys` für Objekte, Links und Snapshots,
- `uml/ocl` für OCL-Ausdrücke, Typen, Werte und Evaluation,
- `parser` und `resources/grammars` für Syntaxreferenz,
- `resources/examples` und Tests als Testfallquelle.

Nicht übernommen werden sollen der USE-Core, die alte Desktop-GUI, die alte Runtime-Struktur, die Build-/Parsertechnik als direkte Grundlage oder nicht zum MVP gehörende Diagramm- und Animationsfunktionen.

Die Erkenntnisse aus USE sollen in ein neues Backend, ein neues Frontend, eine eigene OCL Engine, eine eigene Validation Engine und eine strukturierte MVP-Planung übersetzt werden.
