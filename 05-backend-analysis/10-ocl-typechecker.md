# OCL Typechecker

## Zweck dieser Datei

Diese Datei beschreibt den konzeptionellen OCL Typechecker des neuen Backends. Sie definiert, wie OCL-Ausdrücke nach dem Parsen gegen das UML-Modell typisiert werden, welche Typregeln im MVP gelten und wie Typfehler strukturiert an Validation Service, API und Frontend weitergegeben werden.

Der Typechecker ist Teil der eigenen OCL Engine des neuen Systems. Das originale USE-Projekt dient als fachliche Referenz für OCL-Typisierung, Ausdruckssemantik und Fehlersituationen, wird aber nicht als Codebasis oder Runtime Dependency übernommen.

## Rolle des Typecheckers

Der Typechecker liegt zwischen Parser und Evaluator:

```text
OCL Text
-> Lexer
-> Parser
-> AST
-> Typechecker
-> Typed AST / Type Result
-> Evaluator
-> Evaluation Result
-> Validation Result
```

Seine Hauptaufgabe ist die semantische Prüfung eines syntaktisch gültigen OCL-Ausdrucks. Der Parser erkennt nur die Struktur eines Ausdrucks. Der Typechecker entscheidet dagegen, ob diese Struktur im Kontext des aktuellen UML-Modells gültig ist.

| Aufgabe | Beschreibung | MVP-Relevanz |
|---|---|---|
| Kontext prüfen | Prüft, ob die Kontextklasse einer Invariante existiert. | Pflicht |
| `self` typisieren | Weist `self` den Typ der Kontextklasse zu. | Pflicht |
| Attribute auflösen | Prüft, ob referenzierte Attribute an der Klasse existieren. | Pflicht |
| Navigation auflösen | Prüft, ob Association Navigation über Rollen zulässig ist. | Pflicht |
| Operatoren prüfen | Prüft, ob Operatoren auf die Operandentypen passen. | Pflicht |
| Collection-Operationen prüfen | Prüft `size`, `isEmpty`, `notEmpty` auf Collection-Typen. | Pflicht |
| Invariantentyp prüfen | Prüft, ob der Gesamtausdruck einer Invariante `Boolean` ergibt. | Pflicht |
| Typed AST erzeugen | Annotiert AST-Knoten mit Typen und aufgelösten Modellreferenzen. | Pflicht |
| Diagnose erzeugen | Liefert strukturierte Typfehler mit Source Range und Modellbezug. | Pflicht |

Der Typechecker wertet keine Objektzustände aus. Er benötigt keinen Snapshot und trifft keine Aussage darüber, ob eine Invariante in einem konkreten Objektmodell erfüllt ist. Diese Aufgabe liegt beim Evaluator und Validation Service.

## Typmodell

Das Backend benötigt ein explizites OCL-Typmodell. Dieses Typmodell verbindet primitive OCL-Typen, UML-Klassentypen und Collection-Typen mit dem fachlichen Domänenmodell.

### MVP-Typen

| Typ | Beschreibung | Beispiel | Verwendung |
|---|---|---|---|
| `String` | Zeichenkettenwert | `'Moby Dick'`, `self.name` | Attribute, Gleichheit |
| `Integer` | Ganzzahliger Wert | `5`, `self.books` | Attribute, `size`, numerische Vergleiche |
| `Real` | Dezimalzahl | `4.5`, `self.rating` | Attribute, numerische Vergleiche |
| `Boolean` | Wahrheitswert | `true`, `false`, `self.available` | Invarianten, Boolean-Operatoren |
| `ClassType` | Typ einer UML-Klasse | `User`, `Book` | `self`, Navigation, Objektbezug |
| `CollectionType<T>` | Menge oder Sammlung von Elementen eines Typs | `Collection<Book>` | mehrwertige Association Navigation |

Für den MVP reicht ein vereinfachter Collection-Typ aus. Ob intern zwischen `Set`, `Bag`, `Sequence` und `OrderedSet` unterschieden wird, kann später entschieden werden. Fachlich sollte die API jedoch nicht so gestaltet werden, dass diese Unterscheidung später unmöglich wird.

### Perspektivische Typen

| Typ | Post-MVP-Nutzung | Hinweis |
|---|---|---|
| `EnumType` | Enumerationen und enum-basierte Attribute | Relevant bei UML-Enumerationen. |
| `OclVoid` / `null` | optionale Werte und fehlende Referenzen | Für 0..1 Navigation und Null-Semantik zu klären. |
| `OclInvalid` | Fehlerhafte Auswertung | Wichtig für präzise Evaluation nach OCL-Semantik. |
| `TupleType` | komplexere OCL-Ausdrücke | Nicht im MVP. |
| spezifische Collection-Arten | `Set`, `Bag`, `Sequence`, `OrderedSet` | Relevant für erweiterte OCL-Collection-Semantik. |

### Typrepräsentation

Konzeptionell kann das Backend ein internes Modell wie dieses verwenden:

```text
OclType
  PrimitiveType(String | Integer | Real | Boolean)
  ClassType(classId, className)
  CollectionType(elementType)
  EnumType(enumId, enumName)        // Post-MVP
  InvalidType                       // interne Fehlerfortsetzung
```

`InvalidType` ist hilfreich, damit der Typechecker nach einem Fehler weiterprüfen und mehrere Diagnosen in einem Durchlauf zurückgeben kann.

## Kontext und self

Jede Invariante besitzt eine Kontextklasse. Im MVP wird eine OCL-Invariante immer gegen Instanzen genau dieser Klasse geprüft.

Beispiel:

```ocl
context User inv maxBooks:
  self.borrowedBooks->size() <= 5
```

Der Typechecker erhält dafür mindestens:

| Eingabe | Zweck |
|---|---|
| `UmlModel` | Enthält Klassen, Attribute, Assoziationen, Rollen und Invarianten. |
| `UmlInvariant` | Enthält Name, Kontextklasse und OCL-Ausdruck. |
| `AstNode` | Ergebnis des Parsers. |
| `TypeEnvironment` | Enthält aktuell sichtbare Variablen, im MVP mindestens `self`. |

Regeln:

| Regel | Ergebnis bei Erfolg | Fehlerfall |
|---|---|---|
| Die Kontextklasse muss existieren. | `self` erhält `ClassType(contextClass)`. | `UNKNOWN_CLASS` |
| `self` ist im Invariantenkontext immer definiert. | `SelfExpression` wird mit `ClassType` annotiert. | Nur Fehler, wenn Kontext fehlt. |
| Der Gesamtausdruck der Invariante muss `Boolean` sein. | Invariante ist typkorrekt. | `TYPE_ERROR` oder `NON_BOOLEAN_INVARIANT` |

## Attributzugriff

Attributzugriffe werden im MVP über Punktnotation beschrieben:

```ocl
self.name
self.books
self.available
```

Der Typechecker löst den Namen rechts vom Punkt gegen die Klasse des linken Ausdrucks auf.

| Ausdruck | Voraussetzung | Ergebnistyp | Fehlerfall |
|---|---|---|---|
| `self.name` | Kontextklasse besitzt Attribut `name : String`. | `String` | `UNKNOWN_ATTRIBUTE` |
| `self.books` | Kontextklasse besitzt Attribut `books : Integer`. | `Integer` | `UNKNOWN_ATTRIBUTE` |
| `self.available` | Kontextklasse besitzt Attribut `available : Boolean`. | `Boolean` | `UNKNOWN_ATTRIBUTE` |
| `x.name` | `x` hat `ClassType`. | Typ des Attributs | `TYPE_ERROR`, wenn `x` kein Klassentyp ist |

Wenn ein Name sowohl als Attribut als auch als navigierbare Rolle existiert, muss eine eindeutige Auflösungsregel definiert werden. Für den MVP wird empfohlen, solche Mehrdeutigkeiten als Fehler zu melden, statt implizit eine Seite zu bevorzugen.

## Association Navigation

Association Navigation erlaubt im MVP einfache Navigation von einem Objekt zu verbundenen Objekten:

```ocl
self.borrowedBooks
self.borrowedBooks->size()
self.borrowedBooks->notEmpty()
```

Die Navigation wird über Association Ends beziehungsweise Rollennamen aufgelöst. Der Typechecker prüft:

1. Der linke Ausdruck hat einen `ClassType`.
2. Es gibt eine Association, deren Quellseite zur linken Klasse passt.
3. Der Navigationsname entspricht einer erreichbaren Rolle oder einem Association-End-Namen.
4. Die Zielmultiplizität bestimmt, ob ein einzelner `ClassType` oder ein `CollectionType<ClassType>` entsteht.

| Zielmultiplizität | Ergebnistyp im MVP | Beispiel |
|---|---|---|
| `1` | `ClassType<Target>` | `self.owner` |
| `0..1` | `ClassType<Target>` mit offener Optional-Semantik | `self.currentLoan` |
| `0..*`, `1..*`, `*`, `2..5` | `CollectionType<ClassType<Target>>` | `self.borrowedBooks` |

Für den MVP kann angenommen werden, dass alle modellierten Rollen navigierbar sind. Wenn das Domänenmodell später explizite Navigierbarkeit unterstützt, muss der Typechecker diese Eigenschaft auswerten.

| Fehler | Ursache | Beispiel |
|---|---|---|
| `UNKNOWN_ATTRIBUTE` oder `UNKNOWN_ROLE` | Name kann weder als Attribut noch als Rolle aufgelöst werden. | `self.unknown` |
| `TYPE_ERROR` | Navigation wird auf einem primitiven Wert versucht. | `self.name.books` |
| `AMBIGUOUS_PROPERTY` | Name ist als Attribut und Rolle interpretierbar. | `self.books` bei Attribut und Rolle `books` |
| `UNKNOWN_CLASS` | Association verweist auf nicht vorhandene Klasse. | beschädigtes Modell |

## Operator-Typregeln

Der Typechecker prüft alle Operatoren anhand der Operandentypen. Das Ergebnis eines Vergleichs oder Boolean-Ausdrucks ist im Erfolgsfall `Boolean`.

### Vergleichsoperatoren

| Operator | Zulässige Operandentypen | Ergebnistyp | Beispiele |
|---|---|---|---|
| `=` | gleiche oder kompatible Typen | `Boolean` | `self.name = 'Alice'`, `self.available = false` |
| `<>` | gleiche oder kompatible Typen | `Boolean` | `self.name <> ''` |
| `<` | `Integer`/`Real` kompatibel | `Boolean` | `self.books < 5` |
| `<=` | `Integer`/`Real` kompatibel | `Boolean` | `self.books <= 5` |
| `>` | `Integer`/`Real` kompatibel | `Boolean` | `self.rating > 3.5` |
| `>=` | `Integer`/`Real` kompatibel | `Boolean` | `self.books >= 0` |

Für den MVP wird empfohlen:

| Entscheidung | Empfehlung |
|---|---|
| `Integer` mit `Integer` | gültig |
| `Real` mit `Real` | gültig |
| `Integer` mit `Real` | gültig durch numerische Kompatibilität |
| `String <= Integer` | ungültig |
| `Boolean < Boolean` | ungültig |
| `ClassType = ClassType` | nur bei gleichem oder kompatiblem Klassentyp gültig |
| `CollectionType = CollectionType` | im MVP nur bei gleicher Elementtypstruktur prüfen oder zunächst nicht unterstützen |

### Boolean-Operatoren

| Operator | Operandentypen | Ergebnistyp | Beispiel |
|---|---|---|---|
| `and` | `Boolean`, `Boolean` | `Boolean` | `self.active and self.verified` |
| `or` | `Boolean`, `Boolean` | `Boolean` | `self.available or self.reserved` |
| `not` | `Boolean` | `Boolean` | `not self.available` |

Andere Operandentypen führen zu `TYPE_ERROR`.

## Collection-Typregeln

Collection-Operationen werden im MVP mit Pfeilnotation geschrieben:

```ocl
self.borrowedBooks->size()
self.borrowedBooks->isEmpty()
self.borrowedBooks->notEmpty()
```

| Operation | Voraussetzung | Ergebnistyp | Beispiel |
|---|---|---|---|
| `size()` | linker Ausdruck ist `CollectionType<T>` | `Integer` | `self.borrowedBooks->size()` |
| `isEmpty()` | linker Ausdruck ist `CollectionType<T>` | `Boolean` | `self.borrowedBooks->isEmpty()` |
| `notEmpty()` | linker Ausdruck ist `CollectionType<T>` | `Boolean` | `self.borrowedBooks->notEmpty()` |

Fehlerfälle:

| Ausdruck | Fehler | Grund |
|---|---|---|
| `self.name->size()` | `TYPE_ERROR` | `name` ist `String`, keine Collection. |
| `self.books->notEmpty()` | `TYPE_ERROR` | `books` ist `Integer`, keine Collection. |
| `self.borrowedBooks->unknown()` | `TYPE_ERROR` oder `UNKNOWN_OPERATION` | Operation ist im MVP nicht unterstützt. |

Im MVP sollte `size`, `isEmpty` und `notEmpty` nur auf Collection-Typen gültig sein. String-Längen über `size()` sollten nicht implizit unterstützt werden, damit die Semantik klar bleibt.

## Invarianten müssen Boolean ergeben

Eine OCL-Invariante ist nur gültig, wenn ihr Root-Ausdruck den Typ `Boolean` besitzt.

| Ausdruck | Root-Typ | Gültig als Invariante? | Bemerkung |
|---|---|---|---|
| `self.books <= 5` | `Boolean` | ja | Vergleich liefert Boolean. |
| `self.borrowedBooks->notEmpty()` | `Boolean` | ja | Collection-Operation liefert Boolean. |
| `self.name` | `String` | nein | Kein Wahrheitswert. |
| `self.books` | `Integer` | nein | Kein Wahrheitswert. |
| `self.borrowedBooks` | `CollectionType<Book>` | nein | Keine boolesche Bedingung. |

Ein nicht-boolescher Root-Typ wird als strukturierter Typfehler zurückgegeben. Je nach finalem Fehlerkatalog kann dafür `TYPE_ERROR` mit Detail `NON_BOOLEAN_INVARIANT` oder ein eigener Fehlercode verwendet werden.

## Fehlermodell

Typfehler müssen so zurückgegeben werden, dass Backend, API und Frontend sie eindeutig weiterverarbeiten können. Das Frontend soll Fehler im OCL Editor, im Invariant Properties Panel und im Validation Results Panel anzeigen können.

### Diagnosefelder

| Feld | Zweck |
|---|---|
| `code` | Maschinenlesbarer Fehlercode, z. B. `TYPE_ERROR`. |
| `message` | Verständliche Fehlermeldung für UI und Logs. |
| `severity` | Im MVP typischerweise `ERROR`. |
| `invariantId` | Bezug zur betroffenen Invariante. |
| `contextClassId` | Kontextklasse der Invariante. |
| `sourceRange` | Position im OCL-Ausdruck, soweit vom Parser verfügbar. |
| `expectedType` | Erwarteter Typ, falls relevant. |
| `actualType` | Tatsächlicher Typ, falls relevant. |
| `modelElementIds` | Betroffene Klassen, Attribute, Associations oder Rollen. |
| `phase` | `TYPE_CHECK`. |

### Fehlercodes

| Fehlercode | Bedeutung | Beispiel |
|---|---|---|
| `UNKNOWN_CLASS` | Kontextklasse oder referenzierte Klasse existiert nicht. | Invariante verweist auf gelöschte Klasse. |
| `UNKNOWN_ATTRIBUTE` | Attribut oder Property kann nicht aufgelöst werden. | `self.unknown` |
| `UNKNOWN_ROLE` | Association-Rolle kann nicht aufgelöst werden. | `self.loans`, wenn keine Rolle existiert |
| `AMBIGUOUS_PROPERTY` | Name ist nicht eindeutig auflösbar. | Attribut und Rolle mit gleichem Namen |
| `TYPE_ERROR` | Typregel verletzt. | `self.name <= 5` |
| `UNKNOWN_OPERATION` | Operation wird nicht unterstützt oder existiert nicht. | `self.books->sum()` im MVP |
| `NON_BOOLEAN_INVARIANT` | Invariantenausdruck ergibt keinen Boolean. | `self.name` |

Beispiel für einen Typfehler:

```json
{
  "code": "TYPE_ERROR",
  "severity": "ERROR",
  "phase": "TYPE_CHECK",
  "message": "Operator '<=' erwartet numerische Operanden, erhielt String und Integer.",
  "invariantId": "inv-user-max-books",
  "contextClassId": "class-user",
  "sourceRange": {
    "startLine": 1,
    "startColumn": 11,
    "endLine": 1,
    "endColumn": 15
  },
  "expectedType": "Integer | Real",
  "actualType": "String",
  "modelElementIds": ["attr-user-name"]
}
```

Beispiel für eine unbekannte Property:

```json
{
  "code": "UNKNOWN_ATTRIBUTE",
  "severity": "ERROR",
  "phase": "TYPE_CHECK",
  "message": "Die Property 'unknown' ist für die Klasse 'User' nicht definiert.",
  "invariantId": "inv-user-example",
  "contextClassId": "class-user",
  "sourceRange": {
    "startLine": 1,
    "startColumn": 6,
    "endLine": 1,
    "endColumn": 13
  },
  "expectedType": null,
  "actualType": "ClassType(User)",
  "modelElementIds": ["class-user"]
}
```

## Integration mit Parser

Der Parser liefert einen syntaktischen AST, aber keine fachlich aufgelösten Modellreferenzen. Der Typechecker verarbeitet diesen AST und erzeugt daraus einen typisierten AST oder ein Type Result.

| Parser-Ergebnis | Typechecker-Aufgabe |
|---|---|
| `SelfExpression` | Auf `ClassType(contextClass)` setzen. |
| `AttributeAccessExpression` | Property gegen Attribute und Rollen auflösen. |
| `AssociationNavigationExpression` | Association-End bestimmen und Ergebnistyp ableiten. |
| `LiteralExpression` | Literaltyp bestimmen. |
| `BinaryExpression` | Operandentypen prüfen und Ergebnistyp setzen. |
| `UnaryExpression` | Operandentyp prüfen und Ergebnistyp setzen. |
| `CollectionOperationExpression` | Collection-Typ prüfen und Operation typisieren. |

Der Parser sollte Source Ranges an AST-Knoten liefern. Ohne diese Information kann das Frontend Fehler nur auf Invariantenebene anzeigen, nicht präzise an der fehlerhaften Ausdrucksstelle.

## Integration mit Evaluator

Der Evaluator sollte nur typisierte Ausdrücke auswerten. Dadurch muss er nicht erneut erraten, ob `books` ein Attribut oder eine Rolle ist.

Ein Typed AST sollte enthalten:

| Information | Nutzen für Evaluator |
|---|---|
| Ergebnistyp jedes Knotens | Verhindert uneindeutige Runtime-Auswertung. |
| aufgelöste Attribut-ID | Zugriff auf Slot-Werte im Snapshot. |
| aufgelöste Association-ID und Association-End-ID | Navigation über Objektlinks. |
| Kontextklasse | Auswahl der Objektinstanzen für Invariantenauswertung. |
| unterstützte Operation | Direkte Auswertung von `size`, `isEmpty`, `notEmpty`. |

Wenn der Typechecker Fehler findet, sollte der Evaluator für diese Invariante nicht ausgeführt werden. Der Validation Service gibt dann Typfehler zurück, statt eine fachlich unsichere Evaluation zu starten.

## Bezug zum originalen USE-Projekt

Das originale USE-Projekt ist für den Typechecker besonders als fachliche Referenz relevant. Im Original sind OCL-Typen, Ausdrücke und semantische Prüfungen über mehrere Bereiche verteilt.

| Originalbereich | Relevanz für neues System | Nutzung |
|---|---|---|
| `use-core/src/main/java/org/tzi/use/uml/ocl/type/` | Typmodell, primitive Typen, Klassentypen, Collection-Typen, Enum-Typen | fachliche Referenz |
| `use-core/src/main/java/org/tzi/use/uml/ocl/expr/` | Ausdrucksarten, Navigationsausdrücke, Operatorausdrücke | Verhalten- und Konzeptreferenz |
| `use-core/src/main/java/org/tzi/use/parser/ocl/` | OCL-Syntax und semantische Verarbeitung | Syntax- und Verhaltenreferenz |
| `use-core/src/main/java/org/tzi/use/parser/SemanticException.java` | Semantische Fehlerbehandlung | Referenz für Fehlerkategorien |
| `use-core/src/main/java/org/tzi/use/uml/mm/MClassInvariant.java` | Invarianten mit Kontextklasse und OCL-Ausdruck | fachliche Referenz |
| `use-core/src/main/java/org/tzi/use/uml/mm/MAttribute.java` | Attribute im Klassenmodell | fachliche Referenz |
| `use-core/src/main/java/org/tzi/use/uml/mm/MAssociationEnd.java` | Rollen, Multiplizitäten und Navigation | fachliche Referenz |

Diese Bereiche dürfen nicht direkt übernommen werden. Für das neue Backend werden eigene Typklassen, eigene AST-Knoten, eigene Diagnoseobjekte und eigene Typechecking-Regeln entworfen.

## Beispiele

Die folgenden Beispiele beziehen sich auf ein Library-Modell mit Klassen `User` und `Book`, einem Attribut `User.name : String`, einem Attribut `User.books : Integer`, einem Attribut `Book.available : Boolean` und einer mehrwertigen Navigation `User.borrowedBooks : Collection<Book>`.

| Ausdruck | Erwartetes Ergebnis | Begründung |
|---|---|---|
| `self.books <= 5` | gültig, `Boolean` | `books` ist `Integer`, `5` ist `Integer`, `<=` liefert `Boolean`. |
| `self.name <= 5` | ungültig | `String` kann nicht numerisch mit `Integer` verglichen werden. |
| `self.unknown` | ungültig | Property existiert nicht an der Kontextklasse. |
| `self.borrowedBooks->size() <= 5` | gültig, `Boolean` | Navigation liefert Collection, `size()` liefert `Integer`. |
| `self.borrowedBooks->notEmpty()` | gültig, `Boolean` | `notEmpty()` auf Collection liefert `Boolean`. |
| `self.borrowedBooks` | ungültig als Invariante | Root-Typ ist `CollectionType<Book>`, nicht `Boolean`. |
| `self.name <> ''` | gültig, `Boolean` | String-Gleichheitsvergleich ist zulässig. |
| `self.available = false` | gültig, wenn Kontextklasse `available : Boolean` besitzt | Boolean-Gleichheit ist zulässig. |

Beispiel einer erfolgreichen Typisierung:

```text
Expression: self.borrowedBooks->size() <= 5

SelfExpression
  type = ClassType(User)

PropertyAccessExpression("borrowedBooks")
  sourceType = ClassType(User)
  resolvedAssociationEnd = User.borrowedBooks
  type = CollectionType(ClassType(Book))

CollectionOperationExpression("size")
  sourceType = CollectionType(ClassType(Book))
  type = Integer

LiteralExpression(5)
  type = Integer

BinaryExpression("<=")
  leftType = Integer
  rightType = Integer
  type = Boolean
```

## Teststrategie

Der Typechecker sollte mit fokussierten Unit- und Integrationstests abgesichert werden. Die Tests müssen nicht von der späteren UI abhängen.

| Testbereich | Beispiele | Ziel |
|---|---|---|
| Literaltypen | `'x'`, `5`, `4.2`, `true` | Korrekte Basistypisierung prüfen. |
| Kontext und `self` | Invariante auf `User` | `self` erhält `ClassType(User)`. |
| Attributzugriff | `self.name`, `self.books`, `self.unknown` | Auflösung und Fehlerfälle prüfen. |
| Association Navigation | `self.borrowedBooks` | Rollenauflösung und Collection-Typ prüfen. |
| Vergleichsoperatoren | `self.books <= 5`, `self.name <= 5` | Typkompatibilität prüfen. |
| Boolean-Operatoren | `self.active and true`, `self.name and true` | Boolean-Regeln prüfen. |
| Collection-Operationen | `size`, `isEmpty`, `notEmpty` | Operationen nur auf Collections erlauben. |
| Invariant Root Type | `self.name`, `self.books <= 5` | Boolean-Pflicht prüfen. |
| Diagnosen | Source Range, Code, erwarteter/tatsächlicher Typ | API- und UI-Nutzbarkeit prüfen. |
| Parser-Integration | AST aus echtem OCL-Text | Zusammenspiel Parser + Typechecker prüfen. |

Für die Tests sollte ein kleines Library-Fixture-Modell verwendet werden, das Klassen, primitive Attribute, eine mehrwertige Association und mehrere Invarianten enthält. Später können originale USE-Beispielmodelle als fachliche Testfallquelle dienen, ohne deren Implementierung zu übernehmen.

## Post-MVP-Erweiterungen

Der Typechecker muss so entworfen werden, dass spätere OCL-Konstrukte ergänzt werden können.

| Erweiterung | Zusätzliche Typechecker-Aufgabe |
|---|---|
| `forAll`, `exists` | Iteratorvariablen, Scopes und Boolean-Body prüfen. |
| `select`, `collect` | Elementtypen, Ergebnis-Collection-Typen und Iterator-Scope bestimmen. |
| `includes`, `excludes` | Elementtyp gegen Collection-Elementtyp prüfen. |
| `if then else endif` | Condition muss Boolean sein, Branch-Typen kompatibel machen. |
| `let` | Lokale Variable mit Typ in Type Environment eintragen. |
| `allInstances` | Klassenreferenz auflösen, Collection aller Instanzen typisieren. |
| Operation Calls | Signaturauflösung, Parameterprüfung, Rückgabetyp bestimmen. |
| Pre-/Postconditions | Kontext mit `self`, Parametern, Rückgabewert und ggf. `@pre`. |
| Derived Attributes | Ausdruckstyp muss zum deklarierten Attributtyp passen. |
| Init Values | Initialwert muss zum Attributtyp passen. |
| Vererbung | Subtyping, geerbte Attribute, polymorphe Navigation. |
| Enumerationen | Enum-Literale und Enum-Vergleiche prüfen. |
| OCL Undefined/Invalid | Fehlerfortsetzung und dreiwertige/invalid-nahe Semantik klären. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Wie werden Property-Namen aufgelöst, wenn Attribut und Rolle gleich heißen? | Wichtig für eindeutige Diagnose und Modellierungsregeln. |
| Soll der MVP zwischen `Set`, `Bag`, `Sequence` und `Collection` unterscheiden? | Beeinflusst spätere OCL-Kompatibilität. |
| Wie strikt wird `Integer`/`Real`-Kompatibilität behandelt? | Relevant für numerische Vergleiche und Literale. |
| Wie wird `0..1` Navigation typisiert? | Optionalitäts- und Undefined-Semantik muss später geklärt werden. |
| Gibt es einen separaten Typecheck-Endpunkt oder nur Validierung über `Check Constraints`? | Beeinflusst OCL Editor Feedback. |
| Welche Source-Range-Genauigkeit muss der Parser liefern? | Wichtig für präzise UI-Fehleranzeige. |
| Werden Fehlercodes `UNKNOWN_ROLE` und `NON_BOOLEAN_INVARIANT` eigenständig oder als Detail von `TYPE_ERROR` modelliert? | Muss mit Error Contract abgestimmt werden. |

## Zusammenfassung

Der OCL Typechecker ist die zentrale semantische Kontrollinstanz zwischen Parser und Evaluator. Er typisiert `self`, Attribute, Association Navigation, Operatoren und Collection-Operationen gegen das UML-Modell und stellt sicher, dass Invarianten im MVP einen booleschen Ausdruck ergeben.

Für das neue Backend ist ein eigener Typechecker notwendig, damit OCL nicht als String- oder Regex-Prüfung endet und später systematisch erweitert werden kann. Das originale USE-Projekt liefert dafür wichtige fachliche Orientierung zu Typen, Ausdrücken und semantischen Fehlern, bleibt aber reine Referenz.
