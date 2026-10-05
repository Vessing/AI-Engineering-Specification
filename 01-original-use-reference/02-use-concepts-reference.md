# USE Concepts Reference

## Zweck dieser Datei

Diese Datei dokumentiert die wichtigsten fachlichen Konzepte aus dem originalen USE-Projekt, die für das neue UML/OCL-Websystem relevant sind.

Sie beschreibt die Konzepte auf Analyseebene. Sie ist keine Implementierungsanleitung und keine Aufforderung, Code aus USE zu übernehmen. Das neue System soll fachlich von USE lernen, aber technisch eigenständig entstehen.

Der Fokus liegt auf Konzepten für:

- UML-Klassendiagramme,
- UML-Objektdiagramme,
- Snapshots/Systemzustände,
- OCL-Invarianten,
- Constraint Validation.

Sequenzdiagramme, State Machines, Modellanimation, SOIL-Ausführung, Generatoren und weitere verhaltensorientierte Funktionen werden nur erwähnt, wenn sie zur Abgrenzung wichtig sind.

## Fachliche Rolle von USE als Referenz

USE ist im Originalprojekt ein Java-basiertes UML/OCL-Werkzeug. Es verbindet textuelle UML-Modellspezifikation, OCL-Constraints, Systemzustände und Constraint-Prüfung.

Für das neue Websystem ist USE besonders wertvoll als Referenz für:

- zentrale Modellbegriffe,
- Syntax und Beispiele,
- OCL-Auswertung,
- Multiplizitätsprüfung,
- Snapshot-Prüfung,
- Fehlerarten,
- Testfälle.

Die fachliche Rolle ist bewusst von der technischen Rolle getrennt:

| Aspekt | Einordnung |
|---|---|
| Fachliche Konzepte | Als Referenz verwenden |
| Syntaxbeispiele | Als Referenz verwenden |
| Verhalten bei Validierung | Als Referenz verwenden |
| Beispielmodelle und Tests | Als Testfallquelle prüfen |
| Java-Klassenstruktur | Nicht übernehmen |
| USE-Core | Nicht als Dependency verwenden |
| Desktop-GUI | Nicht migrieren |

## UML-Klassenmodell

Das Klassenmodell ist im Originalprojekt hauptsächlich unter folgendem Package abgebildet:

`../use/use-core/src/main/java/org/tzi/use/uml/mm/`

Der Package-Name `mm` steht hier für Modell-/Metamodellkonzepte. Für das neue System ist dieser Bereich die wichtigste fachliche Referenz für Klassendiagramme.

### Klassen

Im Original repräsentieren `MClass` und `MClassImpl` UML-Klassen. Klassen besitzen Attribute, Operationen, Assoziationsbezüge und können in weitere Modellkonzepte wie Vererbung oder State Machines eingebunden sein.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| UML-Klasse | Modelliert einen Klassifier mit Namen, Attributen, Operationen und Beziehungen. Referenz: `MClass.java`, `MClassImpl.java`. | Zentral für Class Diagram View. | MVP | Im neuen System als eigenes Domänenobjekt `Class` oder `UmlClass` modellieren. |
| Klassenname | Identifiziert eine Klasse im Modell. | Zentral für Modellierung, Navigation und OCL-Kontext. | MVP | Eindeutigkeit innerhalb eines Modells prüfen. |
| Klassenzugehörigkeit von Objekten | Objekte referenzieren ihre Klasse. Referenz: `MObject.cls()`. | Zentral für Object Diagram View. | MVP | Objektinstanzen müssen eine gültige Klasse besitzen. |
| Erweiterte Klassifierkonzepte | USE kennt weitere Klassifier-/Datentypkonzepte. | Für MVP begrenzen. | Post-MVP | Nur übernehmen, wenn konkreter Bedarf entsteht. |

### Attribute

Attribute werden im Original durch `MAttribute` repräsentiert.

Wichtige Beobachtungen aus `MAttribute.java`:

- Ein Attribut gehört zu einem Classifier.
- Ein Attribut hat einen Namen und einen OCL-Typ.
- Attribute können optional derive expressions besitzen.
- Attribute können optional init expressions besitzen.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Attribut | Teil eines Classifiers mit Name und Typ. Referenz: `MAttribute.java`. | Zentral für Klassendiagramm und OCL-Attributzugriff. | MVP | MVP unterstützt primitive Attributtypen. |
| Attributtyp | Typ aus OCL-/USE-Typsystem. | Wichtig für Typechecking und Wertprüfung. | MVP | Zunächst `String`, `Integer`, `Real`, `Boolean`. |
| Slot/Attributwert | Objektzustand enthält Werte pro Attribut. Referenz: `MObjectState`. | Zentral für Objektdiagramm. | MVP | Im neuen System als Slot oder AttributeValue modellieren. |
| Derived Attribute | Attribut mit derive expression. | Erweiterung. | Post-MVP | Architektur vorbereiten, aber nicht MVP. |
| Init Expression | Initialwert über OCL-Ausdruck. | Erweiterung. | Post-MVP | Nicht initial notwendig. |

### Operationen

Operationen werden im Original durch `MOperation` repräsentiert.

Wichtige Beobachtungen aus `MOperation.java`:

- Operationen haben Parameterlisten.
- Operationen können einen Rückgabetyp besitzen.
- Operationen können OCL-Ausdrucksbodies haben.
- Operationen können SOIL-Statements mit Seiteneffekten haben.
- Operationen können Pre- und Postconditions besitzen.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Operationssignatur | Name, Parameter und optionaler Rückgabetyp. Referenz: `MOperation.java`. | Relevant für Klassendiagramm. | MVP | Im MVP nur als Signatur modellieren. |
| Operation Body als OCL | Seiteneffektfreier Ausdruck. | Nicht Kern des MVP. | Post-MVP | Später für derived behavior oder Queries prüfbar. |
| Operation Body als SOIL | Ausführbare Anweisung mit möglichen Seiteneffekten. | Nicht relevant für MVP. | Nicht relevant / später prüfen | Keine SOIL-Übernahme geplant. |
| Preconditions | Bedingungen vor Operationsausführung. Referenz: `MPrePostCondition.java`. | Spätere OCL-Erweiterung. | Post-MVP | Nicht MVP. |
| Postconditions | Bedingungen nach Operationsausführung. | Spätere OCL-Erweiterung. | Post-MVP | Nicht MVP. |

### Typen

Das OCL-/USE-Typsystem liegt unter:

`../use/use-core/src/main/java/org/tzi/use/uml/ocl/type/`

`TypeFactory.java` zeigt eingebaute Typen wie:

- `Integer`,
- `UnlimitedNatural`,
- `String`,
- `Boolean`,
- `Real`,
- `OclAny`,
- `OclVoid`,
- Collection-Typen wie `Set`, `Sequence`, `Bag`, `OrderedSet`,
- `EnumType`,
- `TupleType`.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Primitive Typen | `Integer`, `Real`, `String`, `Boolean`. | Zentral für Attribute, Literale und Typechecking. | MVP | MVP sollte diese vier Typen unterstützen. |
| `UnlimitedNatural` | UML/OCL-Typ für natürliche Zahlen inklusive unbegrenzt. | Für Multiplizität intern interessant. | Post-MVP / Referenz | Im MVP kann `*` als Multiplicity-Obergrenze separat modelliert werden. |
| Collection-Typen | `Set`, `Sequence`, `Bag`, `OrderedSet`, `Collection`. | Relevant für Navigation und OCL-Collection-Operationen. | MVP begrenzt, Post-MVP erweitert | MVP braucht einfache Collections für Navigation und `size/isEmpty/notEmpty`. |
| EnumType | Enumerationen. | Fachlich sinnvoll, aber nicht MVP-kritisch. | Post-MVP | Später für domänenspezifische Werte. |
| TupleType | Komplexes OCL-Feature. | Nicht MVP-relevant. | Post-MVP / später prüfen | Nicht initial planen. |
| `OclAny`, `OclVoid` | OCL-Basistypen und Undefined/Null-Semantik. | Für vollständigeres OCL wichtig. | Post-MVP | MVP kann vereinfachte Undefined-Regeln definieren. |

### Assoziationen

Assoziationen werden im Original durch `MAssociation`, `MAssociationImpl` und `MAssociationEnd` repräsentiert.

Eine Assoziation besteht aus mehreren Association Ends. Ein Association End speichert unter anderem:

- Zielklasse,
- Rollenname,
- Multiplizität,
- Aggregationsart,
- Ordnung,
- Navigierbarkeit,
- Qualifier,
- derived/subsets/redefines Informationen.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Assoziation | Beziehung zwischen Klassen. Referenz: `MAssociation.java`, `MAssociationImpl.java`. | Zentral für Klassendiagramm und Objektlinks. | MVP | Zunächst binäre Assoziationen priorisieren. |
| Association End | Ende einer Assoziation mit Klasse, Rolle und Multiplizität. Referenz: `MAssociationEnd.java`. | Zentral für Rollen und Navigation. | MVP | Im neuen System explizit als `AssociationEnd` modellieren. |
| Navigierbarkeit | Association Ends können navigierbar sein. | Relevant für OCL-Navigation. | MVP vereinfacht | Im MVP ggf. alle Rollen navigierbar behandeln oder explizit modellieren. |
| N-äre Assoziationen | Original kann breiter sein als MVP. | Für MVP nicht zwingend. | Post-MVP / später prüfen | MVP sollte binär starten. |
| Association Class | Assoziation mit Klassencharakter. | Nicht MVP-kritisch. | Post-MVP | Später prüfen. |

### Rollen

Rollen sind im Original Teil von `MAssociationEnd`. Der Rollenname ist wichtig für OCL-Navigation und für die Lesbarkeit von Diagrammen.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Rollenname | Name eines Association Ends. | Zentral für OCL-Navigation, Properties Panel und Diagrammansicht. | MVP | Muss im Klassendiagramm editierbar sein. |
| Rollenbasierte Navigation | OCL kann über Rollennamen navigieren. | Zentral für MVP-OCL-Subset. | MVP | Beispiel: `self.department` oder `self.orders`. |
| Qualifier | Association Ends können Qualifier besitzen. | Nicht MVP-kritisch. | Post-MVP / später prüfen | Nicht initial einplanen. |

### Multiplizitäten

Multiplizitäten werden im Original durch `MMultiplicity` repräsentiert.

Wichtige Beobachtungen:

- `*` wird intern als `MANY = -1` abgebildet.
- Es gibt Convenience-Werte für `0..*`, `1..*`, `1`, `0..1`.
- Eine Multiplizität kann aus Ranges bestehen.
- Ranges prüfen, ob eine Kardinalität enthalten ist.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Multiplicity Range | Untere und obere Grenze, z. B. `0..1`, `1`, `1..*`. Referenz: `MMultiplicity.java`. | Zentral für Linkvalidierung. | MVP | MVP sollte `lower`, `upper`, `unbounded` modellieren. |
| `*` / unbounded | Unbegrenzte obere Grenze. | Zentral. | MVP | Nicht zwingend als `-1`; im JSON besser explizit. |
| Mehrere Ranges | USE kann mehrere Bereiche verwalten. | Selten für MVP nötig. | Post-MVP / später prüfen | MVP kann zunächst einen Bereich pro Ende nutzen. |
| Multiplicity Check | Prüft Anzahl verlinkter Objekte gegen Multiplizität. | Zentral. | MVP | Backend-validiert. |

### Vererbung

Vererbung wird im Original unter anderem durch `MGeneralization` und Klassenmethoden zu Eltern-/Kindbeziehungen repräsentiert.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Generalisierung | Beziehung zwischen Ober- und Unterklasse. Referenz: `MGeneralization.java`. | Fachlich relevant, aber MVP-erweiternd. | Post-MVP | MVP kann ohne Vererbung starten. |
| Geerbte Attribute/Operationen | USE berücksichtigt geerbte Merkmale. | Für Typechecking und Snapshots später wichtig. | Post-MVP | Erhöht Komplexität von OCL und Modellvalidierung. |
| Polymorphe Navigation | Navigation und allInstances können Vererbung betreffen. | Später relevant. | Post-MVP | Nicht MVP. |

### Enumerationen und Datentypen

USE unterstützt Enumerationen und Datentypen. Relevante Klassen sind unter anderem:

- `MDataType.java`,
- `MDataTypeImpl.java`,
- `EnumType.java`,
- `EnumValue.java`,
- `TypeFactory.mkEnum(...)`.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Enumerationen | Benannte Menge von Literalen. | Sinnvoll für realistische Modelle. | Post-MVP | Nicht im MVP erforderlich. |
| Datentypen | Eigene Datentypkonzepte. | Für MVP nicht zentral. | Post-MVP / später prüfen | Zunächst primitive Typen reichen. |
| Enum-Literale in OCL | Werte aus Enumeration-Typen. | Später relevant. | Post-MVP | Nicht MVP. |

## Objektmodell und Snapshots

Das Objektmodell und die Zustandsverwaltung liegen im Original vor allem unter:

`../use/use-core/src/main/java/org/tzi/use/uml/sys/`

Für das neue Websystem ist dieser Bereich die wichtigste Referenz für Objektdiagramme und Snapshots.

### Objektinstanzen

Im Original ist `MObject` eine Instanz einer Klasse. Ein Objekt hat:

- eine Klasse,
- einen Namen,
- eine Objektidentität,
- einen Wert für OCL-Auswertung,
- einen Zustand in einem bestimmten `MSystemState`.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Objektinstanz | Instanz einer Klasse. Referenz: `MObject.java`, `MObjectImpl.java`. | Zentral für Object Diagram View. | MVP | Objekt braucht ID, Name und Klassenreferenz. |
| Objektidentität | Im Original sind Objekte über Namen vergleichbar; `MObject` verweist über Zustände hinweg auf dasselbe Objekt. | Wichtig für Snapshots. | MVP | Neues System sollte stabile IDs verwenden, Namen als sichtbares Label. |
| Existenz in Zustand | Objekt kann in einem Systemzustand existieren oder nicht. | Später für mehrere Snapshots wichtig. | MVP vereinfacht | Im MVP vermutlich ein Snapshot mit vorhandenen Objekten. |

### Slots und Attributwerte

`MObjectState` enthält Slots für Attributwerte. Im Konstruktor werden Attribute initial mit `UndefinedValue` belegt.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Object State | Zustand eines Objekts in einem Systemzustand. Referenz: `MObjectState.java`. | Zentral. | MVP | Kann im MVP als Objekt mit Slots modelliert werden. |
| Slot | Wert pro Attribut. | Zentral für Attributwert-Eingabe. | MVP | Muss typgeprüft werden. |
| Undefined Value | Nicht gesetzter Wert. Referenz: `UndefinedValue`. | Wichtig für Validierung. | MVP vereinfacht / Post-MVP genauer | MVP sollte klare Regeln für fehlende Werte definieren. |
| Init Values | Attributwerte können über init expressions initialisiert werden. | Erweiterung. | Post-MVP | Nicht MVP. |

### Objektlinks

Im Original ist `MLink` eine Instanz einer Assoziation. Ein Link verweist auf:

- die beschreibende Assoziation,
- Link Ends,
- beteiligte Objekte,
- optionale Qualifier,
- ggf. Virtual-/Derived-Status.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Objektlink | Instanz einer Assoziation. Referenz: `MLink.java`, `MLinkImpl.java`. | Zentral für Objektdiagramm. | MVP | Link verbindet Objekte passend zu Association Ends. |
| Link End | Objekt am Ende eines Links. Referenz: `MLinkEnd.java`. | Relevant für strukturierte Validierung. | MVP | Neues System kann vereinfachte Link-End-Struktur nutzen. |
| LinkSet | Sammlung von Links pro Assoziation. Referenz: `MLinkSet.java`. | Wichtig für Navigation und Multiplicity Checks. | MVP | Backend kann Links nach Assoziation indexieren. |
| Qualifier Values | Werte zur qualifizierten Navigation. | Nicht MVP. | Post-MVP / später prüfen | Nicht initial. |
| Virtual/Derived Links | Links, die nicht direkt vom Nutzer erzeugt wurden. | Erweiterung. | Post-MVP | Nicht MVP. |

### Systemzustände

`MSystemState` repräsentiert im Original einen konkreten Zustand des Systems.

Relevante Aufgaben:

- Objekte verwalten,
- Objektzustände verwalten,
- Links verwalten,
- Navigation ermöglichen,
- Struktur prüfen,
- Invarianten prüfen,
- Derived Values aktualisieren.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Systemzustand/Snapshot | Konkreter Zustand mit Objekten, Slots und Links. Referenz: `MSystemState.java`. | Zentral. | MVP | Im neuen System als Snapshot oder ObjectDiagram. |
| Mehrere Zustände über Zeit | USE kann Zustände im Rahmen von Animation verändern. | Für MVP nicht nötig. | Post-MVP | MVP kann mit einem aktiven Snapshot starten. |
| Zustandsänderungen/Events | Original hat Event- und SOIL-Strukturen. | Für Web-MVP nicht zentral. | Nicht relevant / später prüfen | Kein Fokus auf Modellanimation. |

## OCL-Konzepte

OCL-Konzepte liegen im Original in mehreren Bereichen:

- Parser: `../use/use-core/src/main/java/org/tzi/use/parser/ocl/`
- Grammatik: `../use/use-core/src/main/resources/grammars/ocl/`
- Ausdrucksmodell: `../use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/`
- Typen: `../use/use-core/src/main/java/org/tzi/use/uml/ocl/type/`
- Werte: `../use/use-core/src/main/java/org/tzi/use/uml/ocl/value/`

### Invarianten

Invarianten werden im Original durch `MClassInvariant` repräsentiert.

Wichtige Beobachtungen:

- Eine Invariante gehört zu einem Classifier.
- Der Body muss boolean sein.
- Ohne explizite Variable wird `self` als Variable angelegt.
- Die Invariante wird intern zu einem global auswertbaren Ausdruck expandiert.
- Reguläre Invarianten werden zu `Class.allInstances()->forAll(...)`.
- Existential invariants werden zu `Class.allInstances()->exists(...)`.
- USE kann verletzende Instanzen über einen generierten Ausdruck ermitteln.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Klasseninvariante | Boolean-OCL-Ausdruck im Kontext einer Klasse. Referenz: `MClassInvariant.java`. | Zentral. | MVP | Wichtigster OCL-Constraint-Typ im MVP. |
| Invariantenname | Qualifizierbar über Klasse und Name. | Wichtig für Validation Results. | MVP | Ergebnis sollte `className` und `invariantName` enthalten. |
| Boolean Body | Body muss boolean sein. | Zentral für Typechecking. | MVP | Typechecker muss dies prüfen. |
| Expanded Expression | Internes Ausweiten auf alle Instanzen. | Sehr relevant. | MVP konzeptionell | Neues System kann pro Objekt evaluieren oder intern ähnlich expandieren. |
| Violating Instances | Ermittlung verletzender Objekte. | Sehr relevant für UI-Fehlermarkierung. | MVP | Neues Backend sollte betroffene Objekt-IDs liefern. |
| Existential Invariants | USE-spezifische Erweiterung. | Nicht MVP. | Post-MVP / später prüfen | In `README.OCL` erwähnt. |
| Mehrere Invariantenvariablen | USE unterstützt z. B. `context p1, p2:Person`. | Nicht MVP. | Post-MVP | Erhöht OCL-Komplexität. |

### Kontext und self

Im Original wird bei einfachen Klasseninvarianten `self` als implizite Variable verwendet.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Kontextklasse | Klasse, für die eine Invariante definiert ist. | Zentral. | MVP | Invariant gehört zu genau einer Klasse. |
| `self` | Implizite Variable vom Typ der Kontextklasse. | Zentral. | MVP | Muss im Parser/Typechecker bekannt sein. |
| Explizite Variablen | USE unterstützt mehrere Kontextvariablen. | Erweiterung. | Post-MVP | Nicht MVP. |

### Attributzugriff

Attributzugriff wird im Original über OCL-Ausdrucksklassen wie `ExpAttrOp` und AST-Klassen im Parser umgesetzt.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| `self.attribute` | Zugriff auf Attribut des Kontextobjekts. | Zentral. | MVP | Typechecker prüft Existenz und Typ. |
| Zugriff auf navigiertes Objekt | Attributzugriff nach Navigation. | Relevant. | MVP begrenzt | Nur einfache Navigation und eindeutige Typregeln. |
| Derived Attribute Evaluation | Attribut kann über derive expression berechnet werden. | Erweiterung. | Post-MVP | Nicht MVP. |

### Navigation

Navigation ist im Original über Association Ends, Rollen und `ExpNavigation` abgebildet.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Rollennavigation | Navigation über Rolle einer Assoziation. | Zentral. | MVP | Beispiel: `self.department`. |
| Collection-Ergebnis | Navigation über mehrwertige Rolle liefert Collection. | Zentral für `size/isEmpty/notEmpty`. | MVP | Typechecker muss Single vs Collection unterscheiden. |
| Multiplicity Violation bei Navigation | Original kennt `MultiplicityViolationException`. | Relevant für Error Contract. | MVP / Post-MVP | MVP sollte Navigationsfehler strukturiert melden. |
| Qualifizierte Navigation | Navigation mit Qualifiern. | Nicht MVP. | Post-MVP | Später prüfen. |
| Navigation mit Vererbung | Bei Vererbung komplexer. | Nicht MVP. | Post-MVP | Später prüfen. |

### Collection-Ausdrücke

USE unterstützt viele OCL-Collection-Ausdrücke. Für den MVP wird nur ein kleines Subset benötigt.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| `size` | Anzahl Elemente einer Collection. | Zentral. | MVP | Wichtig für einfache Invarianten. |
| `isEmpty` | Prüft leere Collection. | Zentral. | MVP | MVP-OCL-Subset. |
| `notEmpty` | Prüft nicht-leere Collection. | Zentral. | MVP | MVP-OCL-Subset. |
| `forAll` | Quantifizierung. Referenz: `ExpForAll`. | Wichtig, aber nicht MVP-Subset. | Post-MVP | Architektur vorbereiten. |
| `exists` | Existenzquantifizierung. Referenz: `ExpExists`. | Wichtig, aber nicht MVP-Subset. | Post-MVP | Architektur vorbereiten. |
| `select` | Filtern. Referenz: `ExpSelect`. | Erweiterung. | Post-MVP | Nicht MVP. |
| `collect` | Projektion. Referenz: `ExpCollect`. | Erweiterung. | Post-MVP | Nicht MVP. |
| `includes/includesAll` | Membership-Prüfungen. | Erweiterung. | Post-MVP | `Demo.use` nutzt `includesAll`, daher für MVP ggf. reduzieren. |

### Typechecking

Das Original verbindet Parser-AST, Modellkontext und OCL-Typsystem. Typechecking ist in AST-Generierung und Ausdruckserstellung eingebettet.

Für das neue System soll Typechecking explizit als Architekturphase verstanden werden:

```text
Lexer -> Parser -> AST -> Type Checker -> Evaluator -> Validation Result
```

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Typprüfung von Attributzugriff | Attribut muss in Klasse existieren. | Zentral. | MVP | Fehlermeldung strukturiert zurückgeben. |
| Typprüfung von Operationen | Operation muss für Typ erlaubt sein. | Zentral für Collection-Operationen. | MVP begrenzt | Nur MVP-Operationen prüfen. |
| Boolean-Invariante | Invariante muss boolean sein. | Zentral. | MVP | Muss validiert werden. |
| Collection-Typen | Navigation kann Collection ergeben. | Zentral. | MVP begrenzt | Nur einfache Collection-Operationen. |
| Undefined/Null-Semantik | Original hat `UndefinedValue`, `OclVoid`. | Für vollständiges OCL wichtig. | Post-MVP / MVP vereinfacht | MVP-Regeln klar dokumentieren. |

### Evaluation

Die OCL-Evaluation erfolgt im Original über `Evaluator`, `EvalContext`, `Value`-Objekte und den aktuellen `MSystemState`.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Evaluator | Wertet OCL-Ausdrücke aus. Referenz: `Evaluator.java`. | Zentral. | MVP | Eigene Engine bauen. |
| EvalContext | Kontext aus Systemzustand und Variablenbindungen. | Zentral. | MVP | Braucht `self`, Snapshot, Modell. |
| Value Model | BooleanValue, IntegerValue, StringValue, CollectionValue usw. | Zentral. | MVP begrenzt | JSON-kompatibel neu modellieren. |
| Evaluation mehrerer Invarianten | Original kann Liste auswerten. | Relevant für `Check Constraints`. | MVP | Backend validiert alle aktiven Invarianten. |
| Detaillierte Subexpression-Auswertung | Original kann Details ausgeben. | Optional. | Post-MVP | Für Debugging später interessant. |

## Validierungskonzepte

Validierung ist im Original eng mit `MSystemState` verbunden.

Wichtige Referenz:

`../use/use-core/src/main/java/org/tzi/use/uml/sys/MSystemState.java`

### Modellvalidierung

Modellvalidierung betrifft die Struktur des Klassenmodells selbst.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Eindeutige Namen | Klassen, Attribute, Rollen und Invarianten müssen sinnvoll identifizierbar sein. | Zentral. | MVP | Neue Regeln explizit definieren. |
| Attributtyp gültig | Attributtyp muss existieren. | Zentral. | MVP | Primitive Typen prüfen. |
| Association Ends gültig | Assoziationsenden referenzieren existierende Klassen. | Zentral. | MVP | Backend-validiert. |
| Multiplicity Syntax gültig | Untere/obere Grenze müssen gültig sein. | Zentral. | MVP | `lower <= upper` oder `upper = *`. |
| Vererbungskonsistenz | Original unterstützt Vererbung. | Nicht MVP. | Post-MVP | Später ergänzen. |

### Snapshot-Validierung

Snapshot-Validierung prüft, ob Objektzustände zum Klassenmodell passen.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Objektklasse existiert | Objekt muss Instanz einer Klasse sein. | Zentral. | MVP | Fehler an Objekt binden. |
| Slot passt zu Attribut | Attributwert muss zu existierendem Attribut gehören. | Zentral. | MVP | Fehlende/zusätzliche Slots behandeln. |
| Slot-Typ korrekt | Wert muss zum Attributtyp passen. | Zentral. | MVP | Beispiel: Integer darf kein String sein. |
| Link passt zu Assoziation | Link muss Objekte passend zu Association Ends verbinden. | Zentral. | MVP | Fehler an Link binden. |
| Mehrere Snapshots | Originalzustände können über Zeit verändert werden. | Erweiterung. | Post-MVP | MVP kann ein aktiver Snapshot sein. |

### Multiplizitätsprüfung

Im Original prüft `MSystemState.checkStructure(...)`, ob die Anzahl der Links zu den Multiplizitäten der Association Ends passt.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Linkzählung pro Objekt/Assoziationsende | Anzahl verbundener Objekte wird gegen Multiplizität geprüft. | Zentral. | MVP | Grundlage für Object Diagram Validation. |
| Fehlermeldung bei Verletzung | Original gibt Text wie Multiplicity constraint violation aus. | Relevant. | MVP | Neues System braucht strukturierte Fehler. |
| Whole/Part-Prüfung | Original prüft Kompositionshierarchien. | Nicht MVP. | Post-MVP | Erst mit Aggregation/Komposition relevant. |
| Subsets/Redefines | Original prüft erweiterte Association-End-Regeln. | Nicht MVP. | Post-MVP | Nicht initial. |

### Invariantenauswertung

Im Original prüft `MSystemState.check(...)` nach der Strukturprüfung die Invarianten. Dabei werden alle relevanten `MClassInvariant`-Instanzen ausgewertet.

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Alle Invarianten prüfen | Systemzustand wird gegen aktive Invarianten geprüft. | Zentral. | MVP | `Check Constraints` soll alle MVP-Invarianten prüfen. |
| OK/FAILED/N/A | Original unterscheidet erfolgreiche, fehlgeschlagene und nicht auswertbare Invarianten textuell. | Relevant. | MVP | Neues System braucht Statusmodell. |
| Verletzende Instanzen anzeigen | Original kann Instanzen ermitteln, die Invariante verletzen. | Sehr relevant. | MVP | Wichtig für visuelle Markierung. |
| Subexpression Trace | Original kann Details ausgeben. | Optional. | Post-MVP | Für spätere Diagnose. |

### Fehlertypen

Das Original gibt viele Fehler textuell aus oder nutzt Exceptions. Für das neue Websystem müssen daraus strukturierte Fehlerkategorien abgeleitet werden.

| Fehlertyp | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Syntaxfehler | Parserfehler über `ParseErrorHandler`. | Zentral für OCL Editor. | MVP | Zeile/Spalte erfassen. |
| Semantikfehler | Z. B. unbekannte Namen, Typfehler, ungültige Modellstruktur. | Zentral. | MVP | Strukturiertes Error Model. |
| Multiplicity Violation | Verletzung von Multiplizitäten. | Zentral. | MVP | An Objekt/Link/AssociationEnd binden. |
| Invariant Violation | OCL-Invariante ergibt false. | Zentral. | MVP | An Invariante und betroffene Objekte binden. |
| Evaluation Error | Ausdruck kann nicht ausgewertet werden. | Zentral. | MVP | Beispiel: ungültige Navigation oder undefined. |
| Pre/Postcondition Failure | Verletzung von Operation Constraints. | Nicht MVP. | Post-MVP | Referenz: `ppcHandling`. |
| State Machine Invariant | State-Machine-bezogene Verletzung. | Nicht Zielumfang. | Nicht relevant | Nicht für neues Zielsystem. |

## Konzept-Relevanzmatrix

| Konzept | Beschreibung im Original | Relevanz für neues System | MVP/Post-MVP | Hinweise |
|---|---|---|---|---|
| Klasse | UML-Klasse mit Attributen, Operationen und Beziehungen. | Sehr hoch | MVP | Kern des Klassendiagramms. |
| Attribut | Name und Typ, optional derive/init. | Sehr hoch | MVP, derive/init Post-MVP | MVP nur einfache Attribute. |
| Operation | Signatur, optional Body, Pre-/Postconditions. | Mittel | MVP als Signatur, Rest Post-MVP | Keine Ausführung im MVP. |
| Primitive Typen | Integer, Real, String, Boolean. | Sehr hoch | MVP | Grundlage für Slots und OCL. |
| Collection-Typen | Set, Sequence, Bag, OrderedSet. | Mittel bis hoch | MVP begrenzt, Rest Post-MVP | MVP nur einfache Navigationsergebnisse. |
| Assoziation | Beziehung zwischen Klassen. | Sehr hoch | MVP | Binär priorisieren. |
| Rolle | Association-End-Name. | Sehr hoch | MVP | Wichtig für Navigation. |
| Multiplizität | Kardinalitätsbereich. | Sehr hoch | MVP | Grundlage für Linkvalidierung. |
| Vererbung | Generalisierung/Spezialisierung. | Mittel | Post-MVP | Nicht initial. |
| Enumeration | Benannte Literalmenge. | Mittel | Post-MVP | Nicht initial. |
| Objekt | Instanz einer Klasse. | Sehr hoch | MVP | Kern des Objektdiagramms. |
| Slot | Attributwert eines Objekts. | Sehr hoch | MVP | Typprüfung nötig. |
| Objektlink | Instanz einer Assoziation. | Sehr hoch | MVP | Kern für Snapshot und Navigation. |
| Snapshot/Systemzustand | Menge aus Objekten, Slots und Links. | Sehr hoch | MVP | MVP mit einem Snapshot. |
| Invariante | Boolean-OCL-Constraint im Klassenkontext. | Sehr hoch | MVP | Wichtigster OCL-Constraint. |
| `self` | Kontextobjekt in OCL. | Sehr hoch | MVP | Muss vom Typechecker gesetzt werden. |
| Navigation | Zugriff über Assoziationsrollen. | Sehr hoch | MVP begrenzt | Einfache Association Navigation. |
| `forAll`, `exists` | Quantoren. | Hoch | Post-MVP | Architektur vorbereiten. |
| `select`, `collect` | Collection-Ausdrücke. | Mittel | Post-MVP | Nicht MVP. |
| Pre-/Postconditions | Operationsconstraints. | Mittel | Post-MVP | Nicht MVP. |
| State Machines | Verhaltensmodellierung. | Niedrig | Nicht relevant | Nicht Zielumfang. |
| Sequence Diagrams | Verhaltens-/Interaktionsdiagramme. | Niedrig | Nicht relevant | Nicht Zielumfang. |
| SOIL | Imperative Zustandsänderungssprache. | Niedrig für MVP | Nicht relevant / später prüfen | Keine Übernahme geplant. |

## MVP-relevante Konzepte

Für den MVP sollen folgende Konzepte fachlich nachgebildet werden:

- Klassen,
- Attribute,
- primitive Typen `String`, `Integer`, `Real`, `Boolean`,
- Operationen als Signaturen,
- binäre Assoziationen,
- Rollen,
- einfache Multiplizitäten,
- Objekte,
- Slots/Attributwerte,
- Objektlinks,
- ein aktiver Snapshot,
- OCL-Invarianten,
- Kontextklasse und `self`,
- Attributzugriff,
- einfache Association Navigation,
- String-/Integer-/Real-/Boolean-Literale,
- Vergleichsoperatoren,
- Boolean-Operatoren,
- Klammern,
- `size`,
- `isEmpty`,
- `notEmpty`,
- Typechecking für das MVP-OCL-Subset,
- OCL-Evaluation gegen Snapshot,
- Multiplicity Checks,
- Invariant Checks,
- strukturierte Validation Results,
- Elementbezug für Fehlerdarstellung im Objektdiagramm.

## Post-MVP-relevante Konzepte

Folgende Konzepte sind fachlich relevant, aber nicht Bestandteil des MVP:

- Vererbung,
- Enumerationen,
- eigene Datentypen,
- Aggregation und Komposition,
- Assoziationsklassen,
- N-äre Assoziationen,
- Qualifier,
- subsets/redefines,
- derived attributes,
- init values,
- operation bodies,
- preconditions,
- postconditions,
- mehrere Snapshots,
- `forAll`,
- `exists`,
- `select`,
- `collect`,
- `includes` / `excludes` / `includesAll`,
- `let`,
- `if-then-else`,
- `allInstances`,
- erweiterte Undefined-/Null-Semantik,
- detaillierte OCL-Evaluation-Traces.

## Nicht relevante Konzepte

Folgende Originalkonzepte sind für das Zielsystem oder den MVP nicht relevant:

- Sequenzdiagramme,
- Kommunikationsdiagramme,
- State Machines,
- Aktivitäts- oder andere verhaltensorientierte Diagramme,
- vollständige Modellanimation,
- SOIL als auszuführende Sprache im MVP,
- ASSL/Generatorfunktionen,
- Plugin-/Runtime-System des Desktop-Werkzeugs,
- Swing-/JavaFX-GUI-Konzepte,
- Distribution/Assembly des Originalprojekts.

Diese Bereiche können historisch interessant sein, sollen aber den Scope des neuen Websystems nicht erweitern.

## Unterschiede zum geplanten Websystem

| Thema | Original USE | Neues Websystem |
|---|---|---|
| Technische Basis | Java-Desktop-/Shell-Werkzeug mit `use-core` und `use-gui`. | Neues Java/Spring-Boot-Backend und React/TypeScript-Frontend. |
| Modellformat | Textuelle `.use`-Spezifikation. | MVP zunächst eigenes JSON-Projektformat. |
| UI | Swing/JavaFX und Shell-zentrierte Bedienung. | Webbasierte Diagramm- und Panel-UI nach neuen Screenshots. |
| OCL-Umfang | Breites OCL-/USE-Feature-Set. | MVP nur begrenztes, erweiterbares OCL-Subset. |
| Validierungsausgabe | Stark textorientiert, teilweise Exceptions. | Strukturierte Validation Results mit Elementbezug. |
| Snapshots | Systemzustände im Rahmen von Animation/Kommandos. | Objektdiagramm/Snapshot als Web-Arbeitsbereich. |
| Operationen | Signaturen, OCL-Bodies, SOIL-Bodies, Pre-/Postconditions möglich. | MVP nur Signaturen. |
| Erweiterte UML-Konzepte | Vererbung, Aggregation, Komposition, State Machines usw. | MVP fokussiert Klassen-/Objektdiagramm und Invarianten. |
| Code-Wiederverwendung | Originalimplementierung. | Keine direkte Wiederverwendung. |

## Zusammenfassung

Das originale USE-Projekt liefert eine sehr gute fachliche Referenz für das neue UML/OCL-Websystem. Besonders relevant sind die Konzepte aus:

- `uml/mm` für Klassenmodell, Attribute, Operationen, Assoziationen, Rollen, Multiplizitäten und Invarianten,
- `uml/sys` für Objekte, Slots, Links und Systemzustände,
- `uml/ocl` für OCL-Ausdrücke, Typen, Werte und Evaluation,
- `parser` und `resources/grammars` für Syntax- und Parserreferenz,
- `examples` und Tests für spätere Testfallableitung.

Für den MVP sollen nur die Konzepte übernommen werden, die den vertikalen Kernablauf tragen: Klassendiagramm erstellen, Objektdiagramm/Snapshot erstellen, OCL-Invarianten definieren und Constraints prüfen.

Alles Weitere bleibt Post-MVP oder außerhalb des Zielumfangs. Die wichtigste Architekturentscheidung bleibt: USE ist fachliche Referenz, aber keine technische Grundlage.
