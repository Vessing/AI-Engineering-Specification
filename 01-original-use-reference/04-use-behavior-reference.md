# USE Behavior Reference

## Zweck dieser Datei

Diese Datei beschreibt fachliches Verhalten des originalen USE-Projekts, das für das neue UML/OCL-Websystem relevant ist.

Es geht nicht darum, alten Code zu übernehmen. Ziel ist, fachlich zu verstehen:

- wie Modelle geladen und interpretiert werden,
- wie UML-Modelle strukturell behandelt werden,
- wie Objektzustände entstehen,
- wie OCL-Ausdrücke ausgewertet werden,
- wie Constraints geprüft werden,
- welche Fehlerfälle auftreten,
- welche Nutzeraktionen im neuen Websystem fachlich nachgebildet werden sollten.

Die Datei fokussiert auf Klassendiagramme, Objektdiagramme, Snapshots und OCL-basierte Validierung. Verhaltensorientierte Diagramme wie State Machines und Sequenzdiagramme werden nur zur Abgrenzung erwähnt.

## Rolle des Originalverhaltens als Referenz

Das originale USE-Projekt ist eine fachliche Verhaltensreferenz. Es zeigt, welche Abläufe in einem UML/OCL-Werkzeug sinnvoll sind:

1. Modell laden oder kompilieren.
2. Modellstruktur prüfen.
3. Objektzustand erzeugen.
4. Attributwerte und Links setzen.
5. OCL-Ausdrücke im Kontext eines Systemzustands auswerten.
6. Struktur- und Invariantenprüfung durchführen.
7. Fehler textuell melden.

Das neue System soll diese fachlichen Abläufe in eine moderne Webarchitektur übersetzen:

```text
Original USE
Textdatei + Shell + Desktop-GUI + Textausgabe

Neues System
Web-UI + REST/JSON + Backend-Validation + strukturierte Results
```

## Modell-Ladeverhalten

Im Original wird eine `.use`-Spezifikation über den USE-Compiler geladen und in ein internes Modell übersetzt.

Wichtige Referenzklassen:

- `../use/use-core/src/main/java/org/tzi/use/parser/use/USECompiler.java`
- `../use/use-core/src/main/java/org/tzi/use/parser/ParseErrorHandler.java`
- `../use/use-core/src/main/java/org/tzi/use/parser/SemanticException.java`
- `../use/use-core/src/main/java/org/tzi/use/parser/Context.java`
- `../use/use-core/src/main/java/org/tzi/use/parser/use/ASTModel.java`

Beobachteter Ablauf in `USECompiler.compileSpecification(...)`:

```text
InputStream/String
-> ANTLRInputStream
-> USELexer
-> CommonTokenStream
-> USEParser
-> ASTModel
-> ASTModel.gen(Context)
-> MModel oder null bei Fehlern
```

Fehler werden über `ParseErrorHandler` und `Context` gezählt. Wenn Parser- oder Semantikfehler auftreten, liefert der Compiler `null` statt eines Modells.

| Verhalten | Original-USE | Neues System | MVP/Post-MVP | Bemerkung |
|---|---|---|---|---|
| Modell laden | `.use`-Text wird lexed, geparst und in `MModel` generiert. | MVP lädt JSON-Projektformat, später optional `.use`. | MVP für JSON, `.use` Post-MVP | Keine direkte USE-Grammatikübernahme. |
| Syntaxfehler | Parser meldet Datei, Zeile, Spalte und Fehlertext. | Strukturierter Fehler mit Position, Code und Message. | MVP für OCL/JSON, `.use` später | Textausgabe in JSON-Fehler übersetzen. |
| Semantikfehler | AST-Generierung nutzt `Context`; bei Fehlern wird kein Modell erzeugt. | Backend validiert Modell semantisch. | MVP | Fehler sollen im UI nachvollziehbar sein. |
| Imports | `compileSpecification(..., URI fileUri, ...)` unterstützt importierte Elemente. | Nicht MVP. | Post-MVP | Relevanz für späteren `.use` Import. |
| Multiplicity Parsing | `USECompiler.compileMultiplicity(...)` kann Multiplizitäten separat kompilieren. | Multiplicity wird als JSON-Struktur modelliert. | MVP | Syntax `1..*` kann später importiert werden. |

## UML-Modellverhalten

Das Originalmodell liegt hauptsächlich unter:

`../use/use-core/src/main/java/org/tzi/use/uml/mm/`

Relevante Klassen:

- `MModel`
- `MClass`, `MClassImpl`
- `MAttribute`
- `MOperation`
- `MAssociation`, `MAssociationImpl`
- `MAssociationEnd`
- `MMultiplicity`
- `MClassInvariant`
- `MGeneralization`
- `MAggregationKind`

Fachliches Verhalten:

- Ein Modell enthält Klassen, Assoziationen, Invarianten und weitere Modellbestandteile.
- Klassen besitzen Attribute und Operationen.
- Attribute besitzen einen Typ.
- Operationen können Signaturen, OCL-Bodies, SOIL-Bodies und Pre-/Postconditions besitzen.
- Assoziationen bestehen aus Association Ends.
- Association Ends definieren beteiligte Klasse, Rolle, Multiplizität, Navigierbarkeit und ggf. erweiterte Eigenschaften.
- Multiplizitäten prüfen, ob eine Kardinalität zulässig ist.

| Verhalten | Original-USE | Neues System | MVP/Post-MVP | Bemerkung |
|---|---|---|---|---|
| Klassen verwalten | Klassen sind Teil von `MModel`. | Klassen im eigenen Domänenmodell. | MVP | Direkt relevant. |
| Attribute typisieren | `MAttribute` hat einen OCL-Typ. | Attribute besitzen primitive Typen im MVP. | MVP | `String`, `Integer`, `Real`, `Boolean`. |
| Operationen behandeln | `MOperation` kann Body und Pre/Post haben. | MVP nur Operationssignaturen. | MVP/Post-MVP | Ausführung bewusst nicht MVP. |
| Assoziationen behandeln | `MAssociation` mit Association Ends. | Eigene Assoziationsstruktur mit Ends. | MVP | Binär priorisieren. |
| Rollen behandeln | Rolle liegt am Association End. | Rollen im Klassendiagramm editierbar. | MVP | Wichtig für OCL-Navigation. |
| Multiplizitäten prüfen | `MMultiplicity.contains(n)` prüft Kardinalität. | Backend prüft Linkzahlen. | MVP | Strukturprüfung muss elementbezogene Fehler liefern. |
| Vererbung | Über `MGeneralization` und vererbte Features. | Nicht im MVP. | Post-MVP | Später relevant. |
| Aggregation/Komposition | `MAggregationKind`, Whole/Part-Prüfung. | Nicht im MVP. | Post-MVP | Später prüfen. |

## Snapshot- und Objektverhalten

Objektzustände und Snapshots werden im Original vor allem unter folgendem Package abgebildet:

`../use/use-core/src/main/java/org/tzi/use/uml/sys/`

Relevante Klassen:

- `MSystem`
- `MSystemState`
- `MObject`, `MObjectImpl`
- `MObjectState`
- `MLink`, `MLinkImpl`
- `MLinkEnd`
- `MLinkSet`
- `StatementEvaluationResult`

Snapshot-verändernde Statements liegen unter:

`../use/use-core/src/main/java/org/tzi/use/uml/sys/soil/`

Besonders relevant:

- `MNewObjectStatement`
- `MAttributeAssignmentStatement`
- `MLinkInsertionStatement`
- `MLinkDeletionStatement`
- `MObjectDestructionStatement`

### Objektinstanzen

`MNewObjectStatement` erzeugt ein neues Objekt einer Klasse. Wenn kein Name angegeben wird, wird ein eindeutiger Name generiert.

Fachliche Ableitung:

- Ein Objekt muss eine Klasse referenzieren.
- Ein Objekt braucht eine Identität.
- Ein Objektname ist sichtbar und sollte eindeutig sein.
- Automatische Namensgenerierung ist nützlich, aber nicht Kern der Semantik.

### Attributwerte

`MAttributeAssignmentStatement` wertet das Zielobjekt und den neuen Wert aus und delegiert die Zuweisung an `MSystem.assignAttribute(...)`.

Fachliche Ableitung:

- Attributwerte gehören zu einem Objektzustand.
- Der Wert muss zum Attribut passen.
- Fehler können entstehen, wenn Objekt, Attribut oder Wert ungültig sind.

### Links

`MLinkInsertionStatement` erzeugt einen Link für eine Assoziation zwischen beteiligten Objekten. Bei Assoziationsklassen kann zusätzlich ein Link-Objekt entstehen.

Fachliche Ableitung:

- Ein Link referenziert eine Assoziation.
- Ein Link enthält beteiligte Objekte in Association-End-Reihenfolge.
- Linkerzeugung muss prüfen, ob die Objekte zu den erwarteten Klassen passen.
- Assoziationsklassen und Qualifier sind Post-MVP.

| Verhalten | Original-USE | Neues System | MVP/Post-MVP | Bemerkung |
|---|---|---|---|---|
| Objekt erzeugen | `!create` bzw. `MNewObjectStatement`. | Objekt im Object Diagram anlegen. | MVP | UI-Aktion statt Shell-Kommando. |
| Attribut setzen | `!set` bzw. `MAttributeAssignmentStatement`. | Wert im Properties Panel setzen. | MVP | Typprüfung im Backend. |
| Link erzeugen | `!insert (...) into Association` bzw. `MLinkInsertionStatement`. | Objektlink im Diagramm anlegen. | MVP | Assoziation und End-Typen prüfen. |
| Objektzustand speichern | `MObjectState` enthält Slots. | Snapshot enthält Slots/Attributwerte. | MVP | JSON-fähig modellieren. |
| Mehrere Zustände/Undo | `MSystem` unterstützt Statements, Undo/Redo und Reset. | Nicht MVP-kritisch. | Post-MVP | Für Web später möglich. |
| Automatische Namen | Tests in `MSystemStateTest` prüfen Namensgenerierung. | Komfortfunktion. | Post-MVP oder MVP optional | Stabile IDs wichtiger als Namen. |

## OCL-Auswertungsverhalten

OCL wird im Original geparst, typisiert und als Ausdrucksmodell ausgewertet.

Relevante Bereiche:

- `../use/use-core/src/main/java/org/tzi/use/parser/ocl/OCLCompiler.java`
- `../use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/`
- `../use/use-core/src/main/java/org/tzi/use/uml/ocl/type/`
- `../use/use-core/src/main/java/org/tzi/use/uml/ocl/value/`

`OCLCompiler.compileExpression(...)` folgt fachlich diesem Ablauf:

```text
OCL input
-> OCLLexer
-> OCLParser
-> ASTExpression
-> ASTExpression.gen(Context)
-> Expression oder null bei Fehlern
```

`Evaluator` wertet anschließend ein `Expression`-Objekt gegen einen `MSystemState` und `VarBindings` aus.

### Kontext und `self`

Bei Klasseninvarianten wird `self` im Kontext der Klasse verwendet. In `MClassInvariant` wird für einfache Invarianten eine Variable `self` vom Typ der Kontextklasse angelegt.

Für das neue System bedeutet das:

- Jede Invariante hat eine Kontextklasse.
- `self` hat den Typ dieser Klasse.
- Bei der Evaluation wird `self` an ein konkretes Objekt gebunden.

### Attributzugriff

Attributzugriff wird im Original über OCL-Ausdrucksklassen wie `ExpAttrOp` abgebildet. Der Zugriff hängt vom Typ des Zielausdrucks ab.

Für das neue System:

- Der Typechecker muss prüfen, ob das Attribut existiert.
- Der Evaluator liest den Wert aus dem Snapshot.
- Fehlende oder undefined Werte müssen definiert behandelt werden.

### Navigation

Navigation wird im Original durch `ExpNavigation` umgesetzt.

Wichtige Beobachtungen aus `ExpNavigation.java`:

- Der Zielausdruck der Navigation muss Objekttyp haben.
- Das Ergebnis kann ein einzelnes Objekt oder eine Collection sein.
- Bei erwarteter Einzel-Navigation und mehr als einem Zielobjekt wird eine `MultiplicityViolationException` ausgelöst.
- Navigation nutzt den aktuellen oder bei `@pre` den vorherigen Systemzustand.

Für den MVP relevant:

- einfache Association Navigation,
- Rollennavigation,
- Single-vs-Collection-Ergebnis,
- strukturierte Fehler bei ungültiger Navigation.

### Collection-Ausdrücke

USE unterstützt viele Collection-Operationen. Für den MVP sind nur einfache Operationen relevant:

- `size`,
- `isEmpty`,
- `notEmpty`.

Post-MVP relevant:

- `forAll`,
- `exists`,
- `select`,
- `collect`,
- `includes`,
- `excludes`,
- `includesAll`,
- `allInstances`.

| Verhalten | Original-USE | Neues System | MVP/Post-MVP | Bemerkung |
|---|---|---|---|---|
| OCL parsen | ANTLR 3 Parser zu AST und Expression. | Eigene OCL-Pipeline. | MVP | Kein Regex-only-Ansatz. |
| OCL typisieren | Während AST-Generierung und Expression-Erzeugung. | Expliziter Typechecker. | MVP | Besser trennbar und API-fähig. |
| OCL evaluieren | `Evaluator` gegen `MSystemState`. | Eigener Evaluator gegen Snapshot. | MVP | Ergebnis als strukturiertes Validation Result. |
| `self` binden | Invariantenkontext erzeugt `self`. | Pro Objekt der Kontextklasse binden. | MVP | Direkt relevant. |
| Navigation | `ExpNavigation` über Association Ends. | Einfache Rollennavigation. | MVP | Komplexere Qualifier später. |
| Undefined | `UndefinedValue`, `OclVoid`. | Vereinfachte MVP-Regeln nötig. | MVP/Post-MVP | Offene Detailfrage. |
| Evaluation Tree | `Evaluator(true)` kann Auswertungsbaum erzeugen. | Nicht MVP. | Post-MVP | Für Debug UI interessant. |

## Constraint- und Invariantenprüfung

Die zentrale Referenz ist:

`../use/use-core/src/main/java/org/tzi/use/uml/sys/MSystemState.java`

Wichtige Methoden:

- `check(PrintWriter out, boolean traceEvaluation, boolean showDetails, boolean allInvariants, List<String> invNames)`
- `checkStructure(PrintWriter out)`
- `checkStructure(MAssociation assoc, PrintWriter out, boolean reportAllErrors)`
- `reportMultiplicityViolation(...)`
- `checkStateInvariants(...)`

Der relevante Ablauf in `MSystemState.check(...)` ist:

```text
checkStructure(...)
-> checking invariants...
-> Invarianten sammeln
-> expandedExpression() je Invariante
-> Evaluator.evalList(...)
-> OK / FAILED / N/A ausgeben
-> optional verletzende Instanzen ausgeben
-> Gesamtstatus zurückgeben
```

Fachliche Kernpunkte:

- Strukturprüfung und Invariantenprüfung sind getrennte Schritte.
- Strukturprüfung prüft unter anderem Multiplizitäten.
- Invarianten werden als boolean Expressions ausgewertet.
- Ein Zustand ist gültig, wenn Strukturprüfung und alle relevanten Invarianten erfolgreich sind.
- Bei fehlgeschlagener Invariante kann USE verletzende Instanzen ermitteln.

| Verhalten | Original-USE | Neues System | MVP/Post-MVP | Bemerkung |
|---|---|---|---|---|
| Struktur vor Invarianten prüfen | `check(...)` ruft zuerst `checkStructure(...)`. | Backend sollte zuerst Snapshot-Struktur prüfen. | MVP | Reduziert Folgefehler. |
| Alle Assoziationen prüfen | `checkStructure` iteriert über Assoziationen. | Alle Links/Association Ends prüfen. | MVP | Binär im MVP. |
| Multiplizitäten prüfen | Linkzahlen werden gegen Multiplicity geprüft. | Strukturierte Multiplicity Errors. | MVP | Mit Objekt-/Linkbezug. |
| Invarianten prüfen | Invarianten werden evaluiert und als OK/FAILED/N/A gemeldet. | Validation Results mit Status. | MVP | Kein reiner Text. |
| Verletzende Instanzen finden | `getExpressionForViolatingInstances()`. | Backend liefert betroffene Objekt-IDs. | MVP | Wichtig für UI-Markierung. |
| Trace Evaluation | Subexpression-Ergebnisse optional ausgeben. | Nicht MVP. | Post-MVP | Debugging-Funktion. |
| Parallel Evaluation | `Evaluator.evalList(...)` kann parallel prüfen. | Nicht MVP-kritisch. | Post-MVP | Erst bei Performancebedarf. |

## Fehlertypen und Fehlermeldungen

Das Original verwendet eine Mischung aus Parserfehlern, Semantikfehlern, Exceptions und textuellen Validierungsmeldungen.

| Fehlertyp | Original-USE | Neues System | MVP/Post-MVP | Bemerkung |
|---|---|---|---|---|
| Syntaxfehler im Modell | `ParseErrorHandler`, ANTLR RecognitionException. | Strukturierter Fehler mit Position. | Post-MVP für `.use`, MVP für OCL-Eingabe | Im MVP primär OCL-Syntax. |
| Semantikfehler im Modell | `SemanticException`, `Context.errorCount`. | Strukturierter Domain Error. | MVP | Z. B. unbekannte Klasse, doppelter Name. |
| Ungültige Multiplizität | `MMultiplicity` wirft bei illegalem Range ggf. Exception. | Validation Error für Multiplicity. | MVP | Im UI am Association End anzeigen. |
| Ungültiges Objekt | `MSystemException` bei Systemaktionen. | Snapshot Validation Error. | MVP | Z. B. Klasse existiert nicht. |
| Ungültiger Attributwert | Zuweisung kann über Systemlogik fehlschlagen. | Type Error am Slot. | MVP | Wert passt nicht zum Attributtyp. |
| Ungültiger Link | `createLink(...)` kann `MSystemException` werfen. | Link Validation Error. | MVP | Falsche Objektklassen oder Linkform. |
| Multiplicity Violation | Textausgabe in `reportMultiplicityViolation(...)`. | `MULTIPLICITY_VIOLATION` mit Elementbezug. | MVP | Im Diagramm und Panel anzeigen. |
| Invariant Violation | Invariante ergibt `false`; Ausgabe `FAILED`. | `INVARIANT_VIOLATION` mit betroffenen Objekten. | MVP | UI-Markierung. |
| Nicht auswertbar | Original kennt `N/A` und `UndefinedValue`. | `EVALUATION_ERROR` oder `NOT_EVALUABLE`. | MVP/Post-MVP | Detailregel offen. |
| Navigation Multiplicity Violation | `MultiplicityViolationException` in `ExpNavigation`. | OCL Evaluation Error. | MVP/Post-MVP | Strukturierte Meldung nötig. |
| Pre/Postcondition Failure | `ppcHandling` Exceptions. | Nicht MVP. | Post-MVP | Später bei Operationen. |

Für das Websystem ist besonders wichtig, Textmeldungen in ein stabiles Fehler- und Ergebnisformat zu übersetzen:

```json
{
  "severity": "ERROR",
  "code": "MULTIPLICITY_VIOLATION",
  "message": "Object john is connected to 0 Department objects, but multiplicity is 1..*.",
  "modelElementId": "associationEnd-worksIn-department",
  "objectIds": ["john"],
  "linkIds": []
}
```

## Nutzeraktionen im Originalsystem

Das Originalsystem ist stark Shell- und Desktop-orientiert. Fachlich relevante Aktionen sind aber direkt auf die Web-UI übertragbar.

| Originalaktion | Beispiel | Neues Websystem | MVP/Post-MVP |
|---|---|---|---|
| Modell öffnen | `open Demo.use` | Projekt laden/importieren. | MVP für JSON, `.use` später |
| Objekt erzeugen | `!create john : Employee` | Objekt im Object Diagram hinzufügen. | MVP |
| Attributwert setzen | `!set john.salary := 4000` | Wert im Properties Panel setzen. | MVP |
| Link erzeugen | `!insert (john,cs) into WorksIn` | Objektassoziation im Diagramm erstellen. | MVP |
| OCL-Ausdruck abfragen | `? self...` oder Query im Shell-Kontext | OCL Editor/Console Query. | Post-MVP teilweise |
| Constraints prüfen | Check-Kommando/Systemprüfung | `Check Constraints` Button. | MVP |
| Ergebnis lesen | Textausgabe OK/FAILED/N/A | Validation Results Panel. | MVP |
| Fehler im Diagramm sehen | Desktop-Views zeigen Zustand, Text meldet Fehler. | Diagramm-Markierung plus Panel. | MVP |

## Relevanz für das neue Backend

Das Backend sollte das fachlich autoritative Verhalten übernehmen:

- Projekt-/Modellstruktur validieren.
- Klassen, Attribute, Operationen, Assoziationen, Rollen und Multiplizitäten prüfen.
- Snapshot mit Objekten, Slots und Links validieren.
- OCL über eine erweiterbare Pipeline verarbeiten.
- `self` und Kontextklasse korrekt binden.
- Attributzugriff und einfache Navigation auswerten.
- Strukturprüfung und OCL-Prüfung in einem `Check Constraints` Ablauf kombinieren.
- Ergebnisse strukturiert und elementbezogen zurückgeben.

Nicht übernehmen:

- USE-Core als Runtime Dependency,
- ANTLR-3-Grammatik als direkte technische Grundlage,
- textbasierte Fehlerausgabe,
- Shell-/SOIL-Ausführungsmodell als Produktkern.

## Relevanz für das neue Frontend

Das Frontend sollte das Originalverhalten nicht technisch kopieren, sondern als moderne Webinteraktion anbieten.

Wichtige Ableitungen:

- Nutzer brauchen eine sichtbare Trennung zwischen Klassendiagramm und Objektdiagramm.
- Objekt- und Linkaktionen müssen einfach ausführbar sein.
- Properties Panels eignen sich für Attribute, Rollen, Multiplizitäten und OCL-Invarianten.
- `Check Constraints` muss sichtbar und zentral auslösbar sein.
- Fehler müssen im Validation Results Panel und im Diagramm erscheinen.
- Textuelle Originalfehler sollten nicht unverändert angezeigt werden; sie müssen verständlich und strukturiert formuliert werden.

## MVP-relevantes Verhalten

Im MVP sollte folgendes Verhalten nachgebildet werden:

- Projekt im MVP-JSON-Format laden/speichern.
- Klassenmodell erstellen und bearbeiten.
- Attribute mit primitiven Typen verwalten.
- Operationen als Signaturen verwalten.
- Binäre Assoziationen mit Rollen und Multiplizitäten verwalten.
- OCL-Invarianten an Klassen definieren.
- Objekte erzeugen.
- Attributwerte setzen.
- Objektlinks erzeugen.
- Snapshot-Struktur prüfen.
- Multiplizitäten prüfen.
- OCL-MVP-Subset parsen, typisieren und auswerten.
- Invarianten gegen Snapshot prüfen.
- Gültigen Gesamtzustand erkennen.
- Ungültigen Gesamtzustand erkennen.
- Fehler mit Objekt-/Link-/Invariant-/Association-End-Bezug ausgeben.
- Fehler visuell im Objektdiagramm und textuell im Validation Panel anzeigen.

## Post-MVP-relevantes Verhalten

Post-MVP relevant sind:

- `.use` Import,
- `.use` Export,
- `.cmd` oder Snapshot-Import,
- mehrere Snapshots,
- Undo/Redo,
- automatische Namensgenerierung,
- erweiterte OCL-Operationen,
- `forAll`, `exists`, `select`, `collect`,
- `allInstances`,
- `let`,
- `if-then-else`,
- Pre-/Postconditions,
- `@pre` und `result`,
- derived attributes,
- init values,
- Vererbung,
- Enumerationen,
- Aggregation/Komposition,
- Association Classes,
- Qualifier,
- subsets/redefines,
- detaillierte OCL-Evaluation-Traces.

## Bewusst abweichendes Verhalten im neuen System

| Verhalten | Original-USE | Neues System | Grund |
|---|---|---|---|
| Eingabeformat | `.use` und Shell-/Command-Dateien. | MVP: JSON-Projektformat und Web-UI. | Schnellerer MVP, klare API. |
| Fehlerausgabe | Text über `PrintWriter`. | Strukturierte JSON-Ergebnisse. | Web-UI braucht Elementbezug. |
| UI | Desktop-GUI und Shell. | React/TypeScript-Webfrontend. | Ziel ist modernes Websystem. |
| OCL-Umfang | Breites OCL-Feature-Set. | MVP-Subset. | Scope kontrollieren. |
| Validierungsdarstellung | Textuell, teils Desktop-Views. | Panel plus Diagramm-Markierung. | Bessere Nutzerführung. |
| Operationen | Ausführbare Operationen, SOIL, Pre/Post möglich. | MVP nur Signaturen. | Verhaltensmodellierung nicht MVP. |
| Snapshot-Erzeugung | Kommandos wie `!create`, `!set`, `!insert`. | Direkte UI-Aktionen und JSON. | Webgerechte Bedienung. |
| Codebasis | USE-Core. | Eigene Backend-/Frontend-Implementierung. | Keine Migration, keine Dependency. |

## Offene Fragen

- Soll der MVP bei fehlenden Attributwerten einen Typfehler, einen Undefined-Wert oder eine eigene Validation Warning erzeugen?
- Soll `Check Constraints` abbrechen, wenn Strukturfehler gefunden werden, oder zusätzlich OCL-Invarianten prüfen?
- Welche Fehlercodes sollen im ersten Error Contract verbindlich sein?
- Wie genau werden betroffene Objekte bei Invariantenverletzungen ermittelt, wenn die Invariante nicht nur `self` betrifft?
- Soll das Backend für MVP-Invarianten pro Objekt evaluieren oder intern eine `allInstances`-ähnliche Expansion nutzen?
- Soll einfache Navigation bei Multiplizität `0..1` ein einzelnes Objekt oder optional/undefined liefern?
- Soll die UI bereits während der Eingabe OCL-Syntaxfehler prüfen oder erst beim `Check Constraints`?
- Welche USE-Testfälle aus `use-core/src/test/java/org/tzi/use/uml/ocl/expr/` passen exakt zum MVP-OCL-Subset?

## Zusammenfassung

Das originale USE-Projekt zeigt ein klares fachliches Verhalten: Modelle werden geladen, in interne Strukturen übersetzt, Objektzustände werden erzeugt, Links und Attributwerte verändern Snapshots, OCL-Ausdrücke werden gegen Systemzustände ausgewertet und Constraints werden über Strukturprüfung plus Invariantenprüfung validiert.

Für das neue Websystem sind besonders relevant:

- Lade- und Fehlerprinzip aus `USECompiler` und `OCLCompiler`,
- Modellstruktur aus `uml/mm`,
- Snapshot-Verhalten aus `uml/sys`,
- Objekt-/Attribut-/Link-Aktionen aus `uml/sys/soil`,
- OCL-Evaluation aus `uml/ocl/expr`,
- Struktur- und Invariantenprüfung aus `MSystemState`.

Das neue System soll dieses Verhalten fachlich aufnehmen, aber bewusst anders umsetzen: mit eigenem Backend, eigenem Frontend, REST/JSON, strukturierter Fehlerausgabe und einem begrenzten, erweiterbaren MVP-OCL-Subset.
