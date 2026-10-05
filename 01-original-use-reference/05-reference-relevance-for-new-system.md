# Reference Relevance for the New System

## Zweck dieser Datei

Diese Datei bewertet, welche Erkenntnisse aus dem originalen USE-Projekt konkret für das neue UML/OCL-Websystem relevant sind.

Sie bildet die Brücke zwischen:

- originalem USE-Projekt,
- neuem Java/Spring-Boot-Backend,
- neuem React/TypeScript-Frontend,
- API/Integration,
- MVP,
- Post-MVP-Roadmap,
- Teststrategie.

Die Datei wiederholt nicht die gesamte Referenzanalyse. Sie priorisiert die wichtigsten Erkenntnisse aus:

- `01-original-project-overview.md`,
- `02-use-concepts-reference.md`,
- `03-use-syntax-and-examples.md`,
- `04-use-behavior-reference.md`.

## Bewertungslogik

Die Relevanz wird mit folgenden Kategorien bewertet:

| Kategorie | Bedeutung |
|---|---|
| Direkt fachlich übernehmen | Das Konzept soll fachlich im neuen System vorkommen, aber technisch neu umgesetzt werden. |
| Als Syntaxreferenz verwenden | Die originale USE-Syntax dient als Referenz für spätere Import-/Export- oder Parserentscheidungen. |
| Als Verhaltenreferenz verwenden | Das beobachtete Verhalten dient als fachliche Orientierung für neue Services, Validierung oder UI-Abläufe. |
| Als Testfallquelle verwenden | Originalbeispiele oder Tests können in neue Testfälle übersetzt werden. |
| Nur langfristig relevant | Für den MVP nicht nötig, aber wichtig für Post-MVP-Roadmap. |
| Bewusst anders lösen | Fachliches Ziel bleibt relevant, technische oder UX-Lösung soll anders sein. |
| Nicht relevant | Für Zielsystem oder Scope nicht wichtig. |
| Nicht übernehmen | Nicht als Code, Dependency, Architektur oder GUI migrieren. |

Grundregel:

> USE liefert Referenzen für Fachlichkeit, Syntax, Verhalten und Tests. Das neue System übernimmt keine technische Implementierung aus USE.

## Zusammenfassung der wichtigsten Erkenntnisse

| Erkenntnis | Konsequenz für das neue System |
|---|---|
| Der Kernnutzen von USE liegt im Zusammenspiel aus Klassenmodell, Snapshot und OCL-Constraints. | MVP muss genau diesen vertikalen Ablauf demonstrieren. |
| `uml/mm` trennt fachlich Klassenmodell-Konzepte von Systemzuständen. | Neues Backend sollte ebenfalls Modellstruktur und Snapshot-Struktur sauber trennen. |
| `uml/sys/MSystemState` prüft zuerst Struktur und danach Invarianten. | Validation Service sollte Strukturfehler und OCL-Fehler getrennt, aber in einem Ergebnis zurückgeben. |
| Invarianten sind boolean OCL-Ausdrücke im Kontext einer Klasse. | MVP braucht Invarianten mit Kontextklasse, `self`, Typechecking und Evaluation. |
| USE kann verletzende Instanzen für Invarianten bestimmen. | Backend sollte betroffene Objekt-IDs für UI-Markierung liefern. |
| Originalfehler sind stark textorientiert. | Neues System braucht strukturierte Validation Results und Error Contracts. |
| `.use`- und `.cmd`-Beispiele sind wertvolle Testquellen. | Beispiele sollen kategorisiert und in MVP-/Post-MVP-Testfälle übersetzt werden. |
| Die alte Desktop-GUI ist nicht Zielbild. | Frontend orientiert sich primär an neuen Screenshots, nicht an Swing/JavaFX. |

## Relevanzmatrix

| Bereich | Original-USE-Referenz | Backend-Relevanz | Frontend-Relevanz | API/DTO-Relevanz | MVP | Post-MVP | Empfohlene Nutzung | Nicht übernehmen |
|---|---|---|---|---|---|---|---|---|
| UML-Klassenmodell | `use-core/src/main/java/org/tzi/use/uml/mm/` | Sehr hoch: Grundlage für eigenes Domänenmodell. | Hoch: Class Diagram View. | Hoch: Projekt-/Model-DTOs. | Ja | Ja | Direkt fachlich übernehmen. | Keine `MModel`-/`MClass`-Codeübernahme. |
| Klassen | `MClass.java`, `MClassImpl.java` | Sehr hoch. | Sehr hoch. | Hoch. | Ja | Ja | Direkt fachlich übernehmen. | Keine Originalklassen verwenden. |
| Attribute | `MAttribute.java` | Sehr hoch: Name, Typ, später init/derive. | Sehr hoch: Properties Panel. | Hoch: Class/Attribute DTO. | Ja | Ja | Direkt fachlich übernehmen, MVP vereinfachen. | Derived/init nicht im MVP erzwingen. |
| Operationen | `MOperation.java` | Mittel: Signaturen im MVP. | Mittel: Anzeige/Bearbeitung. | Mittel: Operation DTO. | Teilweise | Ja | Signaturen direkt fachlich übernehmen. | Operation Bodies, SOIL, Pre/Post nicht im MVP. |
| Assoziationen | `MAssociation.java`, `MAssociationImpl.java` | Sehr hoch. | Sehr hoch. | Hoch: Association DTO. | Ja | Ja | Direkt fachlich übernehmen, binär starten. | N-äre/advanced features nicht initial. |
| Rollen | `MAssociationEnd.java` | Sehr hoch: Navigation und Linkvalidierung. | Sehr hoch: Properties Panel und Labels. | Hoch: AssociationEnd DTO. | Ja | Ja | Direkt fachlich übernehmen. | Keine implizite USE-Rollenableitung ungeprüft übernehmen. |
| Multiplizitäten | `MMultiplicity.java`, `MSystemState.checkStructure(...)` | Sehr hoch: Multiplicity Checks. | Hoch: Eingabe/Fehleranzeige. | Hoch: Multiplicity DTO, Error DTO. | Ja | Ja | Direkt fachlich und als Verhaltenreferenz übernehmen. | Interne `MANY = -1`-Repräsentation nicht übernehmen. |
| Vererbung | `MGeneralization.java`, Parserbeispiele `class B < A` | Mittel. | Mittel. | Mittel. | Nein | Ja | Nur langfristig relevant. | Nicht im MVP einbauen. |
| Enumerationen | `EnumType.java`, `EnumValue.java`, `TypeFactory.mkEnum(...)` | Mittel. | Mittel. | Mittel. | Nein | Ja | Nur langfristig relevant. | Nicht MVP. |
| OCL-Invarianten | `MClassInvariant.java` | Sehr hoch. | Hoch: Invariant Editor/Panel. | Hoch: Constraint DTO, Validation Result. | Ja | Ja | Direkt fachlich übernehmen, technisch neu. | Keine `MClassInvariant`-Codeübernahme. |
| OCL-Ausdrücke | `uml/ocl/expr/*`, `parser/ocl/*` | Sehr hoch. | Mittel: OCL Editor Feedback. | Hoch: Parse/Validation Results. | Ja, Subset | Ja | Verhalten- und Syntaxreferenz. | Kein vollständiger OCL-Clone im MVP. |
| OCL-Typechecking | AST-Generierung, `uml/ocl/type/*` | Sehr hoch. | Mittel: Fehlermeldungen anzeigen. | Hoch: Error Contract. | Ja | Ja | Als Verhaltenreferenz verwenden. | Nicht im Frontend-only lösen. |
| OCL-Evaluation | `Evaluator.java`, `EvalContext`, `Value` | Sehr hoch. | Niedrig bis mittel: Ergebnisanzeige. | Hoch: Evaluation Result DTO. | Ja | Ja | Als Verhaltenreferenz verwenden. | Evaluator nicht als Dependency verwenden. |
| Objektinstanzen | `MObject.java`, `MObjectImpl.java` | Sehr hoch. | Sehr hoch: Object Diagram. | Hoch: Object DTO. | Ja | Ja | Direkt fachlich übernehmen. | Namensbasierte Identität nicht als einzige Identität übernehmen. |
| Slots | `MObjectState.java` | Sehr hoch. | Sehr hoch: Properties Panel. | Hoch: Slot DTO. | Ja | Ja | Direkt fachlich übernehmen. | `UndefinedValue`-Semantik nicht ungeklärt kopieren. |
| Objektlinks | `MLink.java`, `MLinkImpl.java`, `MLinkEnd.java` | Sehr hoch. | Sehr hoch. | Hoch: Link DTO. | Ja | Ja | Direkt fachlich übernehmen. | Link Objects/Qualifier nicht im MVP. |
| Snapshots/Systemzustände | `MSystemState.java` | Sehr hoch. | Sehr hoch: Object Diagram View. | Hoch: Snapshot DTO. | Ja | Ja | Direkt fachlich übernehmen, Web-gerecht modellieren. | Shell-/Animation-Modell nicht übernehmen. |
| Multiplicity Checks | `MSystemState.checkStructure(...)`, `reportMultiplicityViolation(...)` | Sehr hoch. | Hoch: Fehler markieren. | Sehr hoch: Error DTO. | Ja | Ja | Als Verhaltenreferenz verwenden. | Textausgabe nicht übernehmen. |
| Invariant Checks | `MSystemState.check(...)`, `MClassInvariant.expandedExpression()` | Sehr hoch. | Hoch: Validation Panel. | Sehr hoch: Validation Result DTO. | Ja | Ja | Als Verhaltenreferenz verwenden. | Parallel-/Trace-Details nicht MVP. |
| Fehlermeldungen | `ParseErrorHandler`, `SemanticException`, `MSystemState` Textausgaben | Hoch. | Sehr hoch: verständliche Darstellung. | Sehr hoch: Error Contract. | Ja | Ja | Fehlerarten übernehmen, Format neu. | PrintWriter-/Textformat nicht übernehmen. |
| `.use` Syntax | `resources/grammars/base/USEBase.gpart`, Beispiele | Mittel. | Niedrig im MVP. | Mittel für Import. | Nein | Ja | Als Syntaxreferenz verwenden. | Kein MVP-Zwang zur Kompatibilität. |
| `.use` Import/Export | Grammars, Parser-Tests, Examples | Mittel. | Mittel. | Hoch. | Nein | Ja | Später stufenweise planen. | Nicht vor MVP priorisieren. |
| GUI/Frontend-Verhalten | `use-gui/src/main/java/...` | Niedrig. | Mittel als historische Orientierung. | Niedrig. | Nein | Teilweise | Nur einzelne Nutzerabläufe prüfen. | Swing/JavaFX nicht migrieren. |
| Beispielmodelle | `resources/examples/*`, z. B. `Demo.use`, `Cars.use`, `Demo.cmd` | Hoch. | Hoch für Demo-Flows. | Mittel. | Ja, reduziert | Ja | Als Testfallquelle verwenden. | Komplexe Beispiele nicht ungefiltert übernehmen. |
| Tests | `use-core/src/test/*`, `use-core/src/it/*` | Hoch. | Mittel. | Mittel. | Ja, ausgewählt | Ja | Als Testfallquelle verwenden. | Alte Testframeworks/Setups nicht übernehmen. |

## MVP-Relevanzmatrix

| MVP-Thema | USE-Referenz | MVP-Relevanz | Konkrete Ableitung |
|---|---|---|---|
| Klassen erstellen | `MClass`, `Demo.use`, `Cars.use` | Sehr hoch | Backend- und Frontend-Modell für Klassen. |
| Attribute mit primitiven Typen | `MAttribute`, `TypeFactory`, `Cars.use` | Sehr hoch | Attribute mit `String`, `Integer`, `Real`, `Boolean`. |
| Operationen als Signaturen | `MOperation`, `Cars.use`, `Employee.use` | Mittel | Keine Body-Ausführung im MVP. |
| Assoziationen | `MAssociation`, `Demo.use` | Sehr hoch | Binäre Assoziationen im Klassendiagramm. |
| Rollen | `MAssociationEnd`, `Employee.use` | Sehr hoch | Rollen für UI und OCL-Navigation. |
| Multiplizitäten | `MMultiplicity`, `MSystemState.checkStructure` | Sehr hoch | Linkanzahl gegen Multiplicity prüfen. |
| OCL-Invarianten | `MClassInvariant`, `Cars.use`, `Demo.use` | Sehr hoch | Klassenbezogene Constraints mit `self`. |
| OCL-MVP-Subset | `ExpAttrOp`, `ExpNavigation`, `ExpStdOp`, Beispiele | Sehr hoch | Attributzugriff, einfache Navigation, Boolean/Vergleich, `size/isEmpty/notEmpty`. |
| Objekte | `MObject`, `MNewObjectStatement`, `Demo.cmd` | Sehr hoch | Object Diagram Nodes. |
| Slots | `MObjectState`, `MAttributeAssignmentStatement`, `Demo.cmd` | Sehr hoch | Attributwerte im Snapshot. |
| Objektlinks | `MLink`, `MLinkInsertionStatement`, `Demo.cmd` | Sehr hoch | Object Diagram Edges. |
| Check Constraints | `MSystemState.check(...)` | Sehr hoch | Backend-Endpoint für Struktur- und Invariantenprüfung. |
| Fehler-Markierung | `reportMultiplicityViolation`, `getExpressionForViolatingInstances()` | Sehr hoch | Validation Results mit betroffenen Elementen. |
| Beispiel-Demo | `Cars.use`, reduzierte `Demo.use`, `Demo.cmd` | Hoch | Grundlage für MVP-Demo und Tests. |

## Post-MVP-relevante Referenzen

| Post-MVP-Thema | USE-Referenz | Warum später relevant? |
|---|---|---|
| `.use` Import | `USEBase.gpart`, `USECompiler`, Parser-Testressourcen | Ermöglicht Wiederverwendung vorhandener Modelle als Eingabedaten. |
| `.use` Export | `.use` Beispiele, Grammatik | Ermöglicht Interoperabilität mit USE-naher Syntax. |
| `.cmd` Snapshot-Import | `Demo.cmd`, Shell-Testdateien, SOIL/Statement-Klassen | Kann Objektzustände aus Originalbeispielen importierbar machen. |
| Vererbung | `MGeneralization`, Parserbeispiele `class B < A` | Für realistischere UML-Modelle. |
| Enumerationen | `EnumType`, `EnumValue`, `projectworld.use` | Für domänenspezifische Werte. |
| Aggregation/Komposition | `MAggregationKind`, Aggregation/Composition-Beispiele | Für Whole/Part-Constraints. |
| Assoziationsklassen | `MAssociationClass`, `AssociationClass.use` | Für Beziehungen mit eigenen Attributen. |
| Pre-/Postconditions | `MPrePostCondition`, `Employee.use`, `ppcHandling` | Für Operation Constraints. |
| Derived Attributes | `MAttribute.getDeriveExpression()` | Für berechnete Werte. |
| Init Values | `MAttribute.getInitExpression()`, `MObjectState.initialize(...)` | Für automatische Objektinitialisierung. |
| Erweiterte OCL-Features | `ExpForAll`, `ExpExists`, `ExpSelect`, `ExpCollect`, `ExpLet`, `ExpIf` | Für größere OCL-Abdeckung. |
| Evaluation Trace | `DetailedEvalContext`, Evaluation Browser | Für Debugging und didaktische Auswertung. |
| Undo/Redo | `MSystemStateTest`, `MSystem` Statement-Historie | Für bessere Modellierungs-UX. |

## Backend-Auswirkungen

| Bereich | Auswirkung auf Backend | USE-Referenz | Priorität |
|---|---|---|---|
| Domänenmodell | Eigene Aggregate für Modell, Klassen, Attribute, Assoziationen, Invarianten, Snapshot, Objekte, Slots, Links. | `uml/mm`, `uml/sys` | MVP |
| Projektformat | JSON statt `.use`, aber fachlich kompatibel zu Kernkonzepten. | `Demo.use`, `Demo.cmd` | MVP |
| OCL Engine | Eigene Pipeline: Lexer, Parser, AST, Typechecker, Evaluator. | `parser/ocl`, `uml/ocl/expr` | MVP |
| Typechecking | Backend muss OCL und Modell semantisch prüfen. | `OCLCompiler`, `TypeFactory`, AST-Klassen | MVP |
| Validation Service | Strukturprüfung plus Invariantenprüfung. | `MSystemState.check`, `checkStructure` | MVP |
| Error Model | Fehlercodes und Elementbezüge statt Textausgabe. | `ParseErrorHandler`, `reportMultiplicityViolation` | MVP |
| Teststrategie | USE-Beispiele in neue Backend-Tests übersetzen. | `Cars.use`, `Demo.use`, `Demo.cmd`, `src/test/*` | MVP/Post-MVP |

## Frontend-Auswirkungen

| Bereich | Auswirkung auf Frontend | USE-Referenz | Priorität |
|---|---|---|---|
| Class Diagram View | Klassen, Attribute, Operationen, Assoziationen, Rollen und Multiplizitäten sichtbar/editierbar machen. | `Demo.use`, `Cars.use`, `Employee.use` | MVP |
| Object Diagram View | Objekte, Slots und Objektlinks darstellen. | `Demo.cmd`, `MObject`, `MLink` | MVP |
| Properties Panel | Ausgewählte Elemente bearbeitbar machen. | USE-Konzepte, neue Screenshots | MVP |
| OCL/Invariants UI | Invarianten pro Klasse erfassen. | `MClassInvariant`, Beispiele | MVP |
| Validation Results | Strukturierte Fehler verständlich anzeigen. | USE-Fehlerarten | MVP |
| Diagramm-Markierung | Betroffene Objekte/Links/Association Ends markieren. | Verletzende Instanzen, Multiplicity Errors | MVP |
| Alte GUI | Höchstens historische Orientierung. | `use-gui/*` | Nicht priorisieren |

## API- und DTO-Auswirkungen

| API/DTO-Bereich | USE-Erkenntnis | Konsequenz für neues DTO/API-Design |
|---|---|---|
| `ProjectDto` | USE-Modell plus Snapshot gehören zusammen. | Projekt enthält UML-Modell, OCL-Constraints und Snapshot-Daten. |
| `ClassDto` | Klassen sind Kernobjekte. | Name, Attribute, Operationssignaturen, Diagrammposition. |
| `AttributeDto` | Attribute haben Namen und Typen. | Primitive Typen im MVP, später derived/init metadata. |
| `AssociationDto` | Assoziationen haben Ends. | Ends mit Klasse, Rolle, Multiplicity, ggf. Navigierbarkeit. |
| `MultiplicityDto` | USE nutzt Ranges und `*`. | MVP: `lower`, `upper`, `unbounded`. |
| `InvariantDto` | Invariante hat Kontextklasse, Name, Body. | Kontext, Text, Parse-/Typecheck-Status. |
| `SnapshotDto` | `MSystemState` enthält Objekte und Links. | Objekte, Slots, Links getrennt modellieren. |
| `ValidationResultDto` | USE meldet OK/FAILED/N/A textuell. | Strukturierter Status, Fehlerliste, betroffene Element-IDs. |
| `ValidationErrorDto` | USE-Fehler enthalten fachliche Infos. | `code`, `severity`, `message`, `elementIds`, `objectIds`, `linkIds`, `sourceRange`. |
| Import-/Export-DTOs | `.use` ist später relevant. | Separate Endpoints für Import/Export, nicht Kern-MVP. |

## Auswirkungen auf OCL-Architektur

| OCL-Aspekt | USE-Relevanz | Entscheidung für neues System |
|---|---|---|
| Parser statt Regex | USE nutzt Grammatik, AST und Expression-Modell. | Neues System ebenfalls mit expliziter Pipeline. |
| Kontextklasse und `self` | `MClassInvariant` bindet `self`. | MVP-Invarianten immer mit Kontextklasse und `self`. |
| Attributzugriff | `ExpAttrOp`-ähnliches Verhalten. | Typechecker prüft Attribut am Kontexttyp. |
| Navigation | `ExpNavigation` nutzt Association Ends. | MVP unterstützt einfache Rollennavigation. |
| Collection-Ergebnisse | Navigation kann Collection ergeben. | MVP braucht Collection-Grundtyp für `size/isEmpty/notEmpty`. |
| Typechecking | USE prüft semantisch beim AST-Gen. | Neues System trennt Typechecker explizit. |
| Evaluation | `Evaluator` gegen `MSystemState`. | Neuer Evaluator gegen eigenes Snapshot-Modell. |
| Violating Instances | USE kann verletzende Instanzen bestimmen. | Backend muss betroffene Objekte liefern. |
| Erweiterbarkeit | USE unterstützt viel mehr OCL. | MVP klein halten, AST erweiterbar entwerfen. |

## Auswirkungen auf Validierung

| Validierungsbereich | USE-Verhalten | Relevanz für neues System |
|---|---|---|
| Strukturprüfung | `MSystemState.checkStructure(...)` prüft u. a. Multiplizitäten. | MVP braucht eigenen Structure Validation Schritt. |
| Invariantenprüfung | `MSystemState.check(...)` prüft Invarianten nach Strukturprüfung. | MVP-Validation Flow sollte dies spiegeln. |
| Multiplicity Errors | Original meldet Association, Objekt, Zielklasse, Rollenende und erwartete Multiplicity. | Error DTO sollte diese Informationen strukturiert enthalten. |
| Invariant Errors | Original meldet Invariante und kann verletzende Instanzen ausgeben. | Error DTO sollte Invariant ID und betroffene Objekt-IDs enthalten. |
| Evaluation Errors | Original nutzt Exceptions/Undefined/N/A. | Neues System braucht klare Statuswerte. |
| Gesamtstatus | Original gibt boolean valid zurück und Textbericht aus. | Neues System braucht `VALID`, `INVALID`, ggf. `NOT_EVALUABLE`. |

## Auswirkungen auf Tests

| Testquelle | Relevanz | Empfohlene Nutzung |
|---|---|---|
| `Documentation/Cars/Cars.use` | Sehr hoch | Minimaltest für Klasse, Attribut, Operationensignatur, einfache Invariante. |
| `Documentation/Demo/Demo.use` | Sehr hoch | MVP-Demo für Klassen, Assoziationen, Multiplizitäten, OCL-Navigation. |
| `Documentation/Demo/Demo.cmd` | Sehr hoch | Snapshot-Testdaten für Objekte, Slots, Links. |
| `monitoring/Employee.use` | Mittel | Post-MVP-Tests für Pre/Post; MVP nur Signaturen/Rollen/Typen. |
| `Documentation/AssociationClass/AssociationClass.use` | Mittel | Post-MVP-Test für Assoziationsklassen. |
| `Documentation/AggregationsAndCompositions/*` | Mittel | Post-MVP-Test für Whole/Part-Regeln. |
| `use-core/src/test/resources/org/tzi/use/parser/*.use` | Mittel bis hoch | Spätere Import-/Parser-Fehlerfälle. |
| `use-core/src/test/java/org/tzi/use/uml/ocl/expr/*` | Hoch | OCL-MVP-Subset-Tests ableiten. |
| `use-core/src/test/java/org/tzi/use/uml/sys/*` | Hoch | Snapshot-, Link- und Multiplicity-Testfälle ableiten. |

## Bewusst anders zu lösende Bereiche

| Bereich | Original-USE | Neues System | Grund |
|---|---|---|---|
| Technische Basis | Java-Core plus Desktop-GUI. | Neues Spring-Boot-Backend und React/TypeScript-Frontend. | Webfähigkeit und klare Modernisierung. |
| Projektformat im MVP | `.use` plus `.cmd`. | JSON-Projektformat. | Schneller MVP und klare API-Verträge. |
| Fehlerausgabe | Text über `PrintWriter`. | Strukturierte JSON Validation Results. | UI braucht Elementbezug und stabile Codes. |
| OCL-Implementierung | Volleres USE/OCL-Modell. | Kleines MVP-Subset, erweiterbar. | Scope kontrollieren. |
| UI-Bedienung | Shell, Swing/JavaFX. | Web-UI nach neuen Screenshots. | Zielbild ist nicht alte Desktop-GUI. |
| Snapshot-Erzeugung | Shell-/SOIL-Kommandos. | Direkte UI-Aktionen und API-Aufrufe. | Webgerechte Nutzung. |
| Operationen | Bodies, SOIL, Pre/Post möglich. | MVP nur Signaturen. | Verhaltensmodellierung ist nicht Zielumfang. |

## Nicht zu übernehmende Bestandteile

| Bestandteil | Warum nicht übernehmen? | Zulässige Nutzung |
|---|---|---|
| `use-core` als Dependency | Widerspricht Ziel eines neuen Systems. | Fachliche Referenz. |
| Fork des USE-Projekts | Würde technische Migration statt Neuentwicklung erzeugen. | Keine. |
| Java-Klassen aus `uml/mm`, `uml/sys`, `uml/ocl` | Zu eng an Originalarchitektur gebunden. | Konzepte und Verhalten analysieren. |
| ANTLR-3-Grammatik als direkte technische Basis | Alter Parserstack und zu breiter Sprachumfang. | Syntaxreferenz für späteren Import. |
| Swing-/JavaFX-GUI | Nicht webfähig und nicht Zielbild. | Historische UI-Orientierung. |
| Shell-/SOIL-Ausführungsmodell | MVP fokussiert Webmodellierung und Validierung. | `.cmd` als Snapshot-Testdatenquelle. |
| State-Machine- und Sequenzdiagrammcode | Nicht Teil des Zielumfangs. | Keine MVP-Nutzung. |
| ASSL/Generatoren | Nicht Teil des MVP. | Später höchstens für Testdatengenerierung prüfen. |
| Plugin-/Runtime-System | Für MVP unnötig. | Keine. |
| Textuelle Fehlerausgabe | Nicht geeignet für Web-UI/API. | Fehlerarten und Inhalte ableiten. |

## Offene Fragen

- Welche reduzierte Variante von `Demo.use` wird als offizielles MVP-Beispielmodell verwendet?
- Soll `Cars.use` als erster Backend/OCL-Testfall formal nachgebaut werden?
- Welche Fehlercodes sind für den MVP verbindlich: `SYNTAX_ERROR`, `TYPE_ERROR`, `MULTIPLICITY_VIOLATION`, `INVARIANT_VIOLATION`, `EVALUATION_ERROR`?
- Soll das MVP OCL-Schreibweisen aus USE wie `->size` ohne Klammern akzeptieren oder eine eigene kanonische Schreibweise verlangen?
- Wie werden fehlende Attributwerte im MVP behandelt: als `undefined`, als Validation Error oder als nicht auswertbarer Ausdruck?
- Soll `.use` Import direkt nach dem MVP oder deutlich später priorisiert werden?
- Welche Teile der Originaltests sind rechtlich und praktisch geeignet, um sie als fachliche Testvorlagen zu dokumentieren?

## Zusammenfassung

Die Original-USE-Analyse liefert klare Prioritäten für das neue Websystem:

- Direkt fachlich relevant für den MVP sind Klassen, Attribute, Operationensignaturen, Assoziationen, Rollen, Multiplizitäten, Objekte, Slots, Links, Snapshots, OCL-Invarianten, Typechecking, Evaluation, Multiplicity Checks und Invariant Checks.
- Als Verhaltenreferenz besonders wichtig sind `MSystemState.check(...)`, `MSystemState.checkStructure(...)`, `MClassInvariant`, `ExpNavigation` und `Evaluator`.
- Als Syntax- und Testreferenz besonders wichtig sind `Cars.use`, `Demo.use`, `Demo.cmd`, die OCL-/USE-Grammatikfragmente und ausgewählte Tests unter `use-core/src/test`.
- Nicht übernommen werden der USE-Core, die alte Desktop-GUI, Shell-/SOIL als Produktkern, Parser-/Buildtechnik, State-Machine-/Sequenzdiagrammteile und textuelle Fehlerausgabe.

Die zentrale Brücke lautet: USE definiert fachliche Erwartungen, das neue System definiert eine neue technische Umsetzung.
