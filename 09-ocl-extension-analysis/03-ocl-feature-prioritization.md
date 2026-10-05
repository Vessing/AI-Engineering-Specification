# OCL Feature Prioritization

## Zweck dieser Datei

Diese Datei priorisiert die Erweiterung der eigenständigen OCL-Komponente des neuen Backends. Die Reihenfolge richtet sich nach fachlichem Nutzen, technischen Voraussetzungen und vertikaler Umsetzbarkeit über die gesamte Pipeline:

```text
OCL Text
-> Lexer
-> Parser
-> AST
-> Type Checker
-> Evaluator
-> Validation Result
-> REST API
-> OCL Editor / Validation UI
```

Normative Grundlage ist `OCL-specification.pdf` im Workspace-Root: OMG Object Constraint Language 2.4, Dokument `formal/2014-02-03`. Maßgeblich sind insbesondere:

- Clause 8: Abstract Syntax,
- Clause 9: Concrete Syntax,
- Clause 10: Semantics,
- Clause 11: OCL Standard Library,
- Clause 12: Verwendung von OCL-Ausdrücken in UML-Modellen.

Das originale USE-Projekt und seine Beispiele dienen nur als nachgeordnete Kompatibilitäts- und Regressionstestquelle. Sie bestimmen weder die OCL-Semantik noch die Priorität eines standardisierten Features.

Die Priorisierung ist keine Behauptung vollständiger OCL-2.4-Konformität. Ein Feature gilt erst als unterstützt, wenn Syntax, AST, Well-formedness, Typprüfung, Evaluation, Diagnosen und relevante Integrationspfade zusammen umgesetzt und getestet sind.

## Priorisierungskriterien

### Bewertungsmaßstab

| Wert | Bedeutung für Relevanz/Nutzen | Bedeutung für Komplexität/Aufwand/Risiko |
|---:|---|---|
| 1 | sehr gering | sehr gering |
| 2 | gering | gering |
| 3 | mittel | mittel |
| 4 | hoch | hoch |
| 5 | sehr hoch | sehr hoch |

Ein hoher Nutzenwert verbessert die Priorität. Hohe Komplexität, hoher Testaufwand oder hohes Risiko verschieben ein Feature dagegen nach hinten, sofern es nicht selbst eine zwingende Grundlage bildet.

### Kriterien

| Kriterium | Leitfrage | Gewichtung |
|---|---|---:|
| Fachliche Relevanz | Wie häufig und grundlegend ist das Feature für UML/OCL-Regeln? | sehr hoch |
| MVP-/Post-MVP-Nutzen | Verbessert es den bestehenden Invarianten- und Constraint-Check-Workflow unmittelbar? | hoch |
| Technische Abhängigkeit | Bereitet es viele spätere Features vor oder benötigt es erst andere Subsysteme? | sehr hoch |
| Parser-Komplexität | Benötigt es neue Grammatik, Präzedenz, Scopes oder Deklarationsformen? | mittel |
| Typechecker-Komplexität | Benötigt es neue Typen, Konformitätsregeln, Overload-Auflösung oder lokale Bindungen? | sehr hoch |
| Evaluator-Komplexität | Benötigt es neue Werte, Scopes, mehrere Zustände, Iteration oder Plattformkontext? | sehr hoch |
| Testaufwand | Wie breit sind Wahrheits-, Typ-, `null`-/`invalid`-, Collection- und Integrationsfälle? | hoch |
| UI/API-Auswirkung | Reichen bestehende DTOs und Editorflächen oder entstehen neue Kontexte und Resultate? | mittel |
| Risiko | Wie wahrscheinlich sind spätere Semantikbrüche oder schwer erkennbare Fehlresultate? | sehr hoch |

### Entscheidungsregeln

1. Semantische Grundlagen stehen vor Komfort- und Ausdrucksfeatures.
2. Typ- und Wertmodell stehen vor Operationen, die diese Typen erzeugen.
3. Allgemeine Call-Auflösung steht vor einer langen Liste hart codierter Operationen.
4. Iterator-Scope wird einmal gemeinsam für Parser, Typechecker und Evaluator eingeführt.
5. Neue OCL-Kontexte wie `post`, `derive` oder `body` folgen erst nach stabiler Ausdruckssemantik.
6. Optionale OCL-2.4-Evaluation-Compliance-Points werden gesondert ausgewiesen.
7. Source Locations, spezifische Diagnosen und Regressionstests wachsen in jeder Stufe mit.

## Feature-Übersicht

### Aktueller Ausgangspunkt

Das Backend unterstützt derzeit im Kern:

- `self`,
- primitive Literale für `String`, `Integer`, `Real` und `Boolean`,
- Attribute und einfache rollenbasierte Association Navigation,
- `=`, `<>`, `<`, `<=`, `>`, `>=`,
- `and`, `or`, `not`,
- Klammern,
- `size()`, `isEmpty()` und `notEmpty()` auf Collections,
- Klasseninvarianten gegen einen Snapshot.

Nicht vollständig vorhanden sind insbesondere OCL-2.4-`null`/`invalid`, Collection-Arten, Standard-Library-Auflösung, Arithmetik, `xor`, `implies`, Iteratoren, Kontrollausdrücke, Typoperationen und weitere Constraint-Kontexte.

### Prioritätsstufen

| Stufe | Bezeichnung | Ziel |
|---|---|---|
| `P0` | Semantische Stabilisierung | Bestehendes MVP auf belastbare OCL-2.4-Grundlagen stellen. |
| `P1` | Ausdrucks- und Typfundament | Allgemeine Calls, Operatoren, Typkonformität und Navigation vorbereiten. |
| `P2` | Collection- und Standardbibliothekskern | Collection-Arten und häufige nicht-iterierende Operationen umsetzen. |
| `P3` | Iteratorenkern | Lokale Iteratorvariablen, `forAll`, `exists`, `select`, `reject`, `collect`. |
| `P4` | Kontroll- und globale Ausdrücke | `if`, `let`, erweiterte Iteratoren und optional `allInstances()`. |
| `P5` | Erweitertes Typ- und Wertmodell | Enums, Collection Literals, Tuples, Typoperationen, Generalisierung. |
| `P6` | Neue Constraint-Kontexte | `pre`, `post`, `@pre`, `result`, `oclIsNew`, `body`, `derive`, `init`, `def`. |
| `P7` | Vollständige Standardbibliothek | Geordnete Collections, Konvertierungen, `iterate`, `closure` und übrige Library-Operationen. |
| `P8` | Optionale/erweiterte Compliance | `OclMessage`, Sichtbarkeit, nicht navigierbare Associations, XMI separat. |

## Abhängigkeitsanalyse

```mermaid
flowchart TD
    P0[P0: null/invalid, Diagnosen, Präzedenz] --> P1[P1: Typkonformität, Calls, Navigation]
    P1 --> P2[P2: Collection-Arten und Basisoperationen]
    P1 --> A[Arithmetik, String, Boolean]
    P2 --> P3[P3: Iterator-Scope und Iteratorenkern]
    P3 --> P4[P4: if, let, erweiterte Iteratoren]
    P2 --> L[Collection Literals]
    P4 --> AI[allInstances optional]
    P1 --> G[Generalisierung und Subtyping]
    G --> T[P5: Typoperationen]
    G --> AI
    L --> TU[P5: Tuple und erweiterte Werte]
    P4 --> P6[P6: neue Constraint-Kontexte]
    P6 --> PRE[@pre, result, oclIsNew]
    P3 --> P7[P7: iterate, closure, vollständige Library]
    P5 --> P7
    P6 --> P8[P8: OclMessage und optionale Compliance]
```

### Zentrale Abhängigkeiten

| Feature | Benötigt zwingend | Bereitet vor |
|---|---|---|
| `null`/`invalid` | eigenes Wert- und Typmodell | sämtliche korrekte OCL-Auswertung |
| Allgemeine Operation Calls | AST für Argumentlisten, Signaturauflösung | Standard Library, Modelloperationen, Autocomplete |
| Navigation Chains | Property-Auflösung und Kardinalität | Iteratoren, globale und abgeleitete Regeln |
| Collection-Arten | Typkonformität, Ordnung, Duplikate | Collection-Produzenten, `collect`, Literale, Konvertierungen |
| Iterator-Scope | lokale Bindungen und Evaluation Context | alle Iteratoren und später `let` |
| Generalisierung/Subtyping | UML-Domänenmodell und Property-Auflösung | Typoperationen, korrektes `allInstances`, Branch-Type-Join |
| Zwei-Snapshot-Kontext | Operation Invocation und Zustandsmodell | `@pre`, Postconditions, `oclIsNew` |
| Operation-Call-Kontext | Parameter, Rückgabetyp, Query-/Non-Query-Regeln | `body`, `pre`, `post`, `result` |
| Tuple-Modell | Tuple-Typ, Literal, Parts, Equality | komplexe Queries und Standard-Library-Verträge |

## Priorisierungsmatrix

Die Komplexitätswerte beschreiben den Aufwand relativ zur vorhandenen MVP-Engine. `UI/API` bewertet notwendige Vertrags- und Darstellungsänderungen.

| Reihenfolge | Featuregruppe | Fachlich | Nutzen | Parser | Typechecker | Evaluator | Tests | UI/API | Risiko | Stufe |
|---:|---|---:|---:|---:|---:|---:|---:|---:|---:|---|
| 1 | Diagnosecodes, vollständige Source Ranges, Recovery-Grundlage | 5 | 5 | 3 | 3 | 2 | 4 | 4 | 3 | P0 |
| 2 | OCL-2.4-Operatorpräzedenz und allgemeine Call-AST-Struktur | 5 | 5 | 4 | 3 | 2 | 4 | 2 | 4 | P0 |
| 3 | `OclVoid`/`null`, `OclInvalid`/`invalid`, Propagation und Boolean-Tabellen | 5 | 5 | 2 | 5 | 5 | 5 | 3 | 5 | P0 |
| 4 | Typkonformität, gemeinsamer Typ und Standard-Library-Signaturauflösung | 5 | 5 | 2 | 5 | 3 | 5 | 3 | 5 | P1 |
| 5 | Robuste Property Calls und Navigation Chains | 5 | 5 | 3 | 5 | 4 | 5 | 3 | 5 | P1 |
| 6 | Arithmetik, unäres Minus, `div`, `mod` | 5 | 4 | 3 | 3 | 3 | 4 | 2 | 3 | P1 |
| 7 | Vollständige Boolean-Operationen: `xor`, `implies` | 5 | 5 | 2 | 3 | 4 | 5 | 2 | 4 | P1 |
| 8 | OclAny-Grundoperationen: Equality, Undefined-/Invalid- und Typabfragen als Infrastruktur | 5 | 5 | 2 | 4 | 4 | 5 | 3 | 4 | P1 |
| 9 | String-Standardoperationen | 4 | 4 | 2 | 3 | 3 | 4 | 2 | 3 | P1/P2 |
| 10 | Collection-Typhierarchie: Collection, Set, Bag, Sequence, OrderedSet | 5 | 5 | 3 | 5 | 5 | 5 | 3 | 5 | P2 |
| 11 | Collection Queries: `includes`, `excludes`, `includesAll`, `excludesAll`, `count` | 5 | 5 | 2 | 4 | 4 | 5 | 2 | 4 | P2 |
| 12 | Collection-Produzenten: `including`, `excluding`, `union`, `intersection`, `symmetricDifference` | 4 | 4 | 2 | 4 | 5 | 5 | 2 | 5 | P2 |
| 13 | Collection-Aggregate `sum`, `min`, `max` sowie kartesisches `product(c2)` | 4 | 4 | 2 | 4 | 4 | 5 | 2 | 4 | P2 |
| 14 | Iteratorvariablen, lokale Scopes und allgemeiner Iterator-AST | 5 | 5 | 5 | 5 | 5 | 5 | 4 | 5 | P3 |
| 15 | `forAll`, `exists` | 5 | 5 | 2 | 4 | 5 | 5 | 3 | 5 | P3 |
| 16 | `select`, `reject` | 5 | 5 | 2 | 4 | 4 | 5 | 3 | 4 | P3 |
| 17 | `collect` und implizites Collect bei Navigation | 5 | 5 | 3 | 5 | 5 | 5 | 3 | 5 | P3 |
| 18 | `any`, `one`, `isUnique` | 4 | 4 | 2 | 4 | 4 | 5 | 3 | 4 | P4 |
| 19 | `if-then-else-endif` | 5 | 4 | 4 | 5 | 4 | 5 | 3 | 4 | P4 |
| 20 | `let-in` | 5 | 4 | 5 | 5 | 4 | 5 | 4 | 4 | P4 |
| 21 | `allInstances()` | 4 | 5 | 3 | 5 | 5 | 5 | 4 | 5 | P4, optionaler Compliance Point |
| 22 | Enum-Typen und Enum-Literale | 4 | 4 | 4 | 4 | 3 | 4 | 4 | 3 | P5 |
| 23 | Collection Types und Collection Literals einschließlich Ranges | 5 | 4 | 5 | 5 | 4 | 5 | 3 | 4 | P5 |
| 24 | Generalisierung, Subtyping und Zugriff auf überschriebene Properties | 5 | 5 | 3 | 5 | 4 | 5 | 4 | 5 | P5/UML-Abhängigkeit |
| 25 | `oclIsTypeOf`, `oclIsKindOf`, `oclAsType`, `oclType` | 4 | 4 | 3 | 5 | 4 | 5 | 3 | 4 | P5 |
| 26 | Tuple Types, Tuple Literals und Tuple-Part-Zugriff | 3 | 3 | 5 | 5 | 4 | 5 | 4 | 4 | P5 |
| 27 | Benutzerdefinierte `def`-Properties und Query-Operationen | 4 | 4 | 5 | 5 | 5 | 5 | 5 | 5 | P6 |
| 28 | Operation Body Expressions | 4 | 4 | 4 | 5 | 5 | 5 | 5 | 5 | P6 |
| 29 | Preconditions | 4 | 3 | 4 | 5 | 4 | 5 | 5 | 4 | P6 |
| 30 | Postconditions und `result` | 5 | 4 | 4 | 5 | 5 | 5 | 5 | 5 | P6 |
| 31 | `@pre` und `oclIsNew()` | 4 | 3 | 4 | 5 | 5 | 5 | 5 | 5 | P6, optionaler Compliance Point |
| 32 | Initial- und Derived Values für Attribute/Association Ends | 4 | 4 | 4 | 5 | 5 | 5 | 5 | 5 | P6 |
| 33 | Geordnete Collection-Operationen: `append`, `prepend`, `insertAt`, `at`, `first`, `last`, `indexOf`, Subranges | 4 | 3 | 2 | 4 | 5 | 5 | 3 | 4 | P7 |
| 34 | Collection-Konvertierungen und `flatten` | 4 | 4 | 2 | 5 | 5 | 5 | 3 | 5 | P7 |
| 35 | `sortedBy` | 3 | 3 | 2 | 4 | 5 | 5 | 3 | 4 | P7 |
| 36 | `iterate` | 4 | 3 | 5 | 5 | 5 | 5 | 4 | 5 | P7 |
| 37 | `closure` | 4 | 3 | 3 | 5 | 5 | 5 | 4 | 5 | P7 |
| 38 | Package Context und Pathnames | 3 | 3 | 4 | 4 | 2 | 4 | 4 | 3 | P7/Importintegration |
| 39 | Sichtbarkeit und Navigation über nicht navigierbare Associations | 3 | 2 | 2 | 5 | 4 | 5 | 4 | 5 | P8, optionaler Compliance Point |
| 40 | `OclMessage`, `^`, `^^`, Message Result Access | 2 | 1 | 5 | 5 | 5 | 5 | 5 | 5 | P8, optionaler Compliance Point |
| 41 | XMI-Austausch von OCL | 2 | 1 | 1 | 2 | 1 | 5 | 5 | 4 | Separates Vorhaben |

## Empfohlene Umsetzungsreihenfolge

### Phase 0: Compliance-Baseline festhalten

Vor der ersten Erweiterung wird eine maschinenlesbare Coverage-Matrix eingeführt. Sie weist für jede Featuregruppe getrennt aus:

- Syntax Compliance,
- Well-formedness und Typechecking,
- Evaluation Compliance,
- optionalen OCL-2.4-Compliance-Point,
- API-/UI-Unterstützung,
- Teststatus.

Das Produkt darf nur „OCL-2.4-basiert“ beziehungsweise „unterstützt ein OCL-2.4-Subset“ angeben, solange nicht alle relevanten Compliance Points erfüllt sind.

### Phase 1: Semantische und diagnostische Grundlagen

Reihenfolge:

1. Source Ranges für alle bestehenden Knoten und Diagnosen härten.
2. Operatorpräzedenz und Linksassoziativität gemäß OCL 2.4 fixieren.
3. Allgemeine Operation-Call- und Argumentstruktur im AST einführen.
4. `null`/`invalid` als eigene Typen und Werte modellieren.
5. OCL-2.4-Wahrheitstabellen für `and`, `or`, `xor`, `implies`, `not` implementieren.
6. Typkonformität und gemeinsamen Ergebnistyp zentralisieren.
7. Standard-Library-Signaturauflösung statt verteilter Sonderfälle einführen.

Diese Phase hat höchste Priorität. Ohne sie würden spätere Features zwar syntaktisch funktionieren, aber bei leeren, undefinierten oder ungültigen Werten falsche Ergebnisse liefern.

### Phase 2: Calls, Navigation und primitive Standardbibliothek

Reihenfolge:

1. Robuste Property- und Operation-Call-Auflösung.
2. Mehrstufige Navigation mit korrekter Single-/Collection-Kardinalität.
3. Arithmetik einschließlich unärem Minus, `div` und `mod`.
4. `xor` und `implies`.
5. OclAny-Grundoperationen wie Gleichheit und Undefined-/Invalid-Prüfung.
6. String-Operationen wie `size`, `concat`, `substring`, Case-Konvertierung und numerische Konvertierungen gemäß Standard Library.

USE-Kurzformen wie parameterlose Calls ohne `()` sind kein Teil dieser Kernphase. Sie können anschließend in einem separaten Kompatibilitätsprofil normalisiert werden.

### Phase 3: Collection-Typmodell und einfache Collection-Operationen

Zuerst werden `Collection`, `Set`, `Bag`, `Sequence` und `OrderedSet` mit Typkonformität, Ordnung und Duplikatsemantik umgesetzt. Danach folgen:

1. `includes`, `excludes`, `includesAll`, `excludesAll`, `count`,
2. `including`, `excluding`,
3. `union`, `intersection`, `symmetricDifference`,
4. `sum`, `min`, `max` sowie das kartesische `product(c2)`,
5. bestehende Operationen `size`, `isEmpty`, `notEmpty` auf dem neuen Wertmodell.

Collection-produzierende Operationen dürfen nicht vor der Entscheidung über Ergebnisart, Ordnung und Duplikate implementiert werden.

### Phase 4: Iterator-Infrastruktur und Iteratorenkern

Die Iterator-Grammatik und lokale Scope-Infrastruktur werden gemeinsam eingeführt. Danach ist die Reihenfolge:

1. `forAll` und `exists`,
2. `select` und `reject`,
3. `collect` einschließlich Result-Elementtyp und standardisierter Flattening-Semantik,
4. implizites Collect bei Navigation über Collections,
5. `any`, `one`, `isUnique`.

`forAll` und `exists` stehen vor Filter- und Transformationsoperationen, weil sie keine neue Ergebnis-Collection konstruieren und den Scope-Mechanismus mit geringerem Typaufwand validieren.

### Phase 5: Kontrollausdrücke und globale Queries

1. `if-then-else-endif` mit Branch-Type-Join und Evaluation nur des gewählten Zweigs.
2. `let-in` mit lexikalischem Scope, Shadowing und optionaler Typangabe.
3. `allInstances()` gegen den aktuell validierten Snapshot.

`allInstances()` ist nach OCL 2.4 ein optionaler Evaluation-Compliance-Point. Vor seiner Umsetzung müssen Snapshot-Grenze, Klassenidentität, Performance und spätere Subklassenbehandlung feststehen.

### Phase 6: Erweitertes Typ- und Literalmodell

1. Enum-Typen und Enum-Literale.
2. Collection Type Expressions und Collection Literals einschließlich Ranges.
3. Generalisierung und Subtyping im UML-Modell.
4. Typoperationen `oclIsTypeOf`, `oclIsKindOf`, `oclAsType`, `oclType`.
5. Tuple Types, Tuple Literals, Tuple Parts und Equality.

Generalisierung ist kein bloßes OCL-Feature. Sie benötigt zuerst belastbare Änderungen am UML-Domänenmodell, an Navigation, Snapshot-Instanzmengen und Property-Auflösung.

### Phase 7: Neue OCL-Kontexte

Reihenfolge:

1. Benutzerdefinierte `def`-Properties und Query-Operationen.
2. Operation Body Expressions.
3. Preconditions.
4. Postconditions mit Parametern und `result`.
5. Zwei-Snapshot-Evaluation für `@pre`.
6. `oclIsNew()`.
7. Initial Values.
8. Derived Values für Attribute und Association Ends.

Pre-/Postconditions werden nicht durch den normalen `Check Constraints`-Button allein ausgelöst. Sie benötigen Operation Invocation, Parameterbindungen und bei Postconditions einen Vor- und Nachzustand. `@pre` und `oclIsNew()` sind optionale Evaluation-Compliance-Points.

### Phase 8: Vollständige Collection- und Standardbibliotheksabdeckung

Nach stabilen Collection-Arten und Iteratoren folgen:

- geordnete Operationen und Subranges,
- Collection-Konvertierungen,
- `flatten`,
- `sortedBy`,
- `iterate` mit Akkumulator-Scope,
- `closure` mit Zyklusbehandlung und definierter Ergebnisordnung,
- verbleibende OclAny-, primitive und Collection-Standardoperationen aus Clause 11.

Die konkrete Library-Liste wird direkt aus der OCL-2.4-Standardbibliothek als maschinenlesbarer Signaturkatalog abgeleitet. Dadurch bleiben Typechecker und Evaluator synchron.

### Phase 9: Optionale und separate Compliance-Bereiche

Zuletzt oder als eigenständige Vorhaben:

- Package Context und vollständige Pathname-Auflösung,
- Zugriff auf private/protected Features,
- Navigation über nicht navigierbare Associations,
- `OclMessage` und Message Expressions,
- XMI Compliance.

`OclMessage` setzt ein Operations-/Signal-Trace-Modell voraus, das im aktuellen Produkt nicht existiert. XMI ist unabhängig vom REST-/JSON-Projektformat und darf die OCL-Engine-Roadmap nicht blockieren.

## Frühe technische Grundlagen

| Grundlage | Konkretes Ergebnis | Akzeptanzkriterium |
|---|---|---|
| Coverage Registry | Feature- und Compliance-Matrix im Backend/Testcode | Unterstützungsstatus ist reproduzierbar und nicht nur Dokumentationstext. |
| Generalisierte Calls | AST-Knoten mit Source, Operation, Argumenten und Range | Neue Standardoperationen benötigen keinen eigenen Parser-Sonderweg. |
| Typkatalog | OCL-Typen und Konformitätsservice | Typechecker und Evaluator verwenden dieselbe Typidentität. |
| Standard-Library Registry | Signaturen, Typparameter, Ergebnistypregeln | Unbekannte/falsch typisierte Calls liefern spezifische Diagnosen. |
| Wertmodell | `null`, `invalid`, primitive, Objekt- und Collection-Werte | Keine Java-`null`-Ausnahmen als OCL-Semantik. |
| Scope-Modell | Kontext-, Iterator-, Let- und Operationsvariablen | Typechecker und Evaluator lösen Namen identisch auf. |
| Source-Modell | Range für Knoten, Operator, Variable und Argument | Editor kann Fehler exakt fokussieren. |
| Evaluation Context | Snapshot, `self`, lokale Bindungen; später Vor-/Nachzustand | Kontext wird explizit und unveränderlich weitergereicht. |

## Mittlere Erweiterungsstufe

Die mittlere Stufe umfasst P2 bis P5. Sie liefert den größten praktischen Nutzen für bestehende Klasseninvarianten:

```ocl
context User inv MaxAvailableBooks:
  self.borrowedBooks
    ->select(book | book.available)
    ->size() <= 5

context User inv HasKnownBook:
  Book.allInstances()->exists(book |
    self.borrowedBooks->includes(book))
```

Sie benötigt keine neue Hauptansicht. Der vorhandene OCL Editor und die Validation Results reichen strukturell aus, müssen aber Source Ranges, lokalen Variablenscope, Ergebnistypen und verständliche Iteratorfehler darstellen können.

## Späte Erweiterungsstufe

Die späte Stufe umfasst Features mit neuen UML- oder Laufzeitkonzepten:

| Featurebereich | Warum spät |
|---|---|
| Generalisierung und Typoperationen | Ändert UML-Typmodell, Navigation und Instanzmengen. |
| Tuples | Neues strukturelles Typ- und Wertmodell sowie API-Repräsentation. |
| Operation Bodies und `def` | Benötigen Aufrufauflösung, Parameter, Rekursion und Zyklenschutz. |
| Pre-/Postconditions | Benötigen Operationsereignis und zwei Zustände. |
| Derived/Init | Verändert Objektanlage, Slot-Semantik, Persistenz und Anzeige. |
| `iterate` | Akkumulatortyp, zweiter lokaler Scope und komplexe Typinferenz. |
| `closure` | Transitive Auswertung, Zyklen, Ordnung und Performance. |
| `OclMessage` | Benötigt ein bislang nicht vorhandenes Message-/Trace-Modell. |
| XMI | Eigenes Austauschformat außerhalb des REST-/JSON-MVP. |

## Parallel sinnvolle Verbesserungen

| Verbesserung | Start | Fortführung |
|---|---|---|
| Source Locations und spezifische Diagnosen | P0 | bei jedem neuen AST-Knoten und Call |
| Parser Recovery | P0 | besonders bei Iteratoren, `let`, Contracts |
| OCL-Editor-Marker | P0/P1 | mit lokalen Scopes und Contracts erweitern |
| Resolved References | P1 | Grundlage für Navigation, Hover und Autocomplete |
| Standard-Library-Autocomplete | P1 | automatisch aus Signaturkatalog erzeugen |
| Evaluation Trace | P2 | für Iteratoren und Postconditions ausbauen |
| Performance-Budgets | P2 | für verschachtelte Iteratoren und `allInstances` verschärfen |
| Compliance-Tests | P0 | pro Stufe erweitern |
| USE-Kompatibilitätstests | nach jeweiligem Standardfeature | getrennt vom OCL-2.4-Kern halten |
| API-Rückwärtskompatibilität | P0 | additive DTO-Felder bevorzugen |

## UI- und API-Auswirkungen

| Stufe | API-Auswirkung | Frontend-Auswirkung |
|---|---|---|
| P0/P1 | präzisere Diagnostics, Typ- und Range-Daten | Inline-Fehler, Editorfokus, Ergebnistyp |
| P2 | Collection-Art und strukturierte Werte in Evaluate Responses | Collection-Ergebnis lesbar darstellen |
| P3/P4 | Iterator-/Scope-Diagnosen, optional Evaluation Details | Iteratorvariable markieren, kontextbezogenes Autocomplete |
| P5 | Enum-, Collection-Literal- und Tuple-Werte in DTOs | strukturierte Ergebnisdarstellung und Typinformationen |
| P6 | ConstraintKind, Operation Context, Parameter, Vor-/Nachzustand | neue Formulare/Panels für Contracts; nicht nur Invariant-UI |
| P7 | überwiegend bestehende OCL-Endpunkte | zusätzliche Library-Signaturen und komplexere Traces |
| P8 | Message-/Trace- und gegebenenfalls XMI-Endpunkte | neue UI nur bei tatsächlichem Produkt-Scope |

Neue Ausdrucksfeatures benötigen grundsätzlich keine eigenen REST-Endpunkte. Die vorhandenen Parse-, Typecheck-, Evaluate- und Validate-Flows bleiben die fachlichen Eintrittspunkte. Neue Endpunkte sind erst für neue Ausführungskontexte wie Operations-Contracts oder XMI sinnvoll.

## Vollständigkeitsmatrix des OCL-2.4-Umfangs

Diese Matrix verhindert, dass seltenere standardisierte Bereiche aus der Planung verschwinden.

| OCL-2.4-Bereich | Enthalten | Priorität/Behandlung |
|---|---|---|
| Zeichen, Schlüsselwörter, Kommentare, Escapes | ja | P0/P1 |
| Context, Package Context, Pathnames | ja | Invarianten vorhanden; Package/Pathnames P7 |
| Invariants | ja | MVP vorhanden, Semantik härten |
| Primitive Literale und Typen | ja | MVP plus `UnlimitedNatural` später |
| Enum Types/Literals | ja | P5 |
| Collection Types/Literals/Ranges | ja | P2/P5 |
| Tuple Types/Literals/Parts | ja | P5 |
| `null`, `invalid`, OclVoid, OclInvalid | ja | P0, zwingend früh |
| Attribute-/Association-/Operation Calls | ja | P1 |
| Navigation Shorthands und implizites Collect | ja | P3 |
| Association Classes und Qualified Associations | ja | nach entsprechender UML-Unterstützung |
| Überschriebene Properties/Supertypes | ja | P5 mit Generalisierung |
| OclAny-Operationen | ja | P1 bis P5 |
| Numeric Standard Library | ja | P1/P2 |
| Boolean Standard Library | ja | P0/P1 |
| String Standard Library | ja | P1/P2 |
| Collection Standard Library | ja | P2 bis P7 |
| `select`, `reject`, `collect` | ja | P3 |
| `forAll`, `exists` | ja | P3 |
| `any`, `one`, `isUnique`, `sortedBy` | ja | P4/P7 |
| `iterate`, `closure` | ja | P7 |
| `if`, `let` | ja | P4 |
| `allInstances()` | ja | P4, optionaler Evaluation-Compliance-Point |
| Typoperationen und Casts | ja | P5 |
| `def`, Body Expressions | ja | P6 |
| Preconditions/Postconditions/`result` | ja | P6 |
| `@pre`, `oclIsNew()` | ja | P6, optionaler Evaluation-Compliance-Point |
| Initial/Derived Values | ja | P6 |
| OclMessage und Message Expressions | ja | P8, optionaler Evaluation-Compliance-Point |
| Sichtbarkeit/nicht navigierbare Associations | ja | P8, optionale Compliance-Entscheidung |
| XMI Compliance | ja | separates Vorhaben |
| Basic OCL/Essential OCL | ja | bei Compliance-Profil und Metamodellabgrenzung dokumentieren |

## Risiken der Reihenfolge

| Risiko | Auswirkung | Gegenmaßnahme |
|---|---|---|
| Features vor `null`/`invalid` | Falsche Wahrheitsergebnisse und Java-Ausnahmen | P0 als harte Voraussetzung behandeln. |
| Generische Collection bleibt zu lange bestehen | Falsche Ordnung, Duplikate und Rückgabetypen | Collection-Typhierarchie vor Produzenten und `collect`. |
| Operationen werden hart codiert | Parser, Typechecker und Evaluator driften auseinander | Zentrale Standard-Library Registry. |
| Iterator-Scope wird mehrfach implementiert | Unterschiedliche Namensauflösung | Gemeinsames unveränderliches Scope-Modell. |
| `allInstances` kommt vor Subtyping-Entscheidung | Spätere semantische Änderung | MVP-Snapshotgrenze dokumentieren; Subklassenfall explizit versionieren. |
| Contracts werden wie Invarianten behandelt | Falscher Trigger und fehlender Vorzustand | Eigener Operation-Validation-Flow. |
| USE-Kompatibilität vermischt sich mit OCL-Kern | Dialekt wird unbeabsichtigt zur Semantikquelle | getrennte Parserprofile und Tests. |
| Zu breite gleichzeitige Umsetzung | Fehlerursachen werden schwer isolierbar | vertikale, einzeln abnehmbare Feature-Slices. |
| UI zeigt komplexe Werte nur als String | Verlust von Typ- und Mappinginformationen | strukturierte Result DTOs für Collection/Tuple/Objekt. |
| Compliance wird zu früh behauptet | Falsche Produkterwartung | öffentliche Coverage-Matrix und präzise Bezeichnung als Subset. |

## Entscheidungsempfehlung

Die klare Empfehlung lautet:

1. **Nicht direkt mit `forAll` beginnen.** Zuerst OCL-2.4-`null`/`invalid`, Typkonformität, allgemeine Calls, Präzedenz und Navigation stabilisieren.
2. **Danach Collection-Arten und einfache Collection-Operationen umsetzen.** Sie schaffen das Wert- und Typfundament für Iteratorergebnisse.
3. **Iteratoren als gemeinsame Infrastruktur entwickeln.** Erst `forAll`/`exists`, danach `select`/`reject` und `collect`.
4. **`if`, `let` und optional `allInstances()` anschließend ergänzen.** Sie bauen auf Scope-, Typ- und Collection-Semantik auf.
5. **Generalisierung, Enums, Literale, Tuples und Typoperationen als erweitertes Typmodell behandeln.**
6. **Pre-/Postconditions, Operation Bodies, Derived und Init als neue OCL-Kontexte planen.** Sie sind keine kleinen Parserfeatures.
7. **`iterate`, `closure`, OclMessage und XMI bewusst spät beziehungsweise separat umsetzen.**
8. **Diagnosen, Source Locations, Tests und OCL-Editor-Mapping ab P0 parallel fortführen.**

Der erste empfohlene Implementierungsmeilenstein endet nach P3. Er liefert eine belastbare Invarianten-Engine mit standardnaher Grundsemantik, Collection-Arten und den wichtigsten Iteratoren, ohne bereits Operation Contracts oder Verhaltensmodelle vorauszusetzen.

## Offene Fragen

| Frage | Auswirkung auf die Priorisierung |
|---|---|
| Soll das erste Ziel Syntax- und Evaluation-Compliance für ein dokumentiertes OCL-2.4-Profil sein? | Bestimmt Definition of Done und Testbreite. |
| Wird `UnlimitedNatural` im UML-Multiplicity-/OCL-Typmodell früh benötigt? | Kann P1/P2 erweitern. |
| Welche OCL-2.4-Versionsteile gelten bei erkannten Inkonsistenzen zwischen informativem Text und normativen Clauses? | Normative Clause und dokumentierte Interpretation müssen festgelegt werden. |
| Welche Collection-Art liefert Association Navigation abhängig von `ordered` und `unique`? | Voraussetzung für P2 und P3. |
| Wie werden fehlende Snapshot-Slots auf `null`, `invalid` oder strukturelle Validation Errors abgebildet? | Zentrale P0-Entscheidung. |
| Wird Generalisierung vor `allInstances()` umgesetzt oder wird zunächst ein Profil ohne Subklassen definiert? | Beeinflusst P4/P5. |
| Sollen Package Context und externe OCL-Dateien unterstützt werden? | Beeinflusst Parser, Imports und Editor. |
| Sind Operation Invocation und Vor-/Nachzustände Produktziele? | Entscheidet, ob P6 umgesetzt wird. |
| Sind Message Expressions und XMI echte Produktziele oder nur dokumentierte Nicht-Ziele? | Entscheidet P8. |
| Welche USE-Kurzformen sollen als optionales Importprofil akzeptiert werden? | Darf die OCL-2.4-Kernpriorisierung nicht verändern. |

## Zusammenfassung

Die technisch sinnvolle Reihenfolge beginnt nicht bei einzelnen sichtbaren Operationen, sondern bei der OCL-Semantik: Diagnosen, Präzedenz, `null`/`invalid`, Typkonformität, allgemeine Calls und Navigation. Darauf folgen das vollständige Collection-Typmodell, einfache Collection-Operationen und anschließend Iteratoren.

`forAll` und `exists` sind die ersten Iteratoren, `select`, `reject` und `collect` folgen auf derselben Scope-Infrastruktur. `if`, `let` und `allInstances()` bilden die nächste Stufe. Generalisierung, Enums, Collection Literals, Tuples und Typoperationen erweitern danach das Typmodell. Pre-/Postconditions, `@pre`, `result`, `oclIsNew`, Operation Bodies, Derived und Init benötigen neue fachliche Kontexte und kommen deshalb später.

Der gesamte OCL-2.4-Umfang bleibt in der Vollständigkeitsmatrix berücksichtigt. Seltene oder optionale Bereiche werden nicht ignoriert, aber bewusst als späte oder separate Compliance-Vorhaben eingeordnet. USE bleibt eine ergänzende Kompatibilitäts- und Regressionstestquelle; normative Syntax und Semantik stammen ausschließlich aus `OCL-specification.pdf`.
