# USE Syntax and Examples

## Zweck dieser Datei

Diese Datei dokumentiert Syntax und Beispiele aus dem originalen USE-Projekt, die als fachliche Referenz für das neue UML/OCL-Websystem dienen können.

Sie beschreibt:

- relevante `.use`-Syntax für Klassenmodelle,
- OCL-Invarianten und Ausdrucksformen,
- Snapshot-/Objektzustandsaufbau über `.cmd`-Dateien,
- geeignete Beispielmodelle,
- MVP-relevante Syntax,
- Post-MVP-Syntax,
- Ableitungen für Tests und späteren `.use` Import/Export.

Wichtig: Das neue System muss im MVP nicht vollständig kompatibel zur originalen USE-Syntax sein. Die Beispiele dienen der Analyse, nicht als technische Migrationsvorgabe.

## Rolle der USE-Syntax für das neue System

Die originale USE-Syntax ist für das neue System vor allem aus drei Gründen relevant:

1. Sie zeigt, wie UML/OCL-Konzepte textuell formuliert werden können.
2. Sie liefert konkrete Beispielmodelle für Anforderungen, Validierung und Testfälle.
3. Sie kann später als Grundlage für `.use` Import/Export dienen.

Für den MVP ist jedoch ein eigenes JSON-Projektformat vorgesehen. Die USE-Syntax ist daher im MVP keine verbindliche Eingabesyntax, sondern eine Referenz für fachliche Inhalte und Testfälle.

| Nutzung | Bedeutung |
|---|---|
| Anforderungen | Beispiele zeigen, welche Modellierungskonzepte praktisch vorkommen. |
| MVP-Scope | Kleine Beispiele helfen, einen vertikalen Durchstich zu definieren. |
| OCL-Subset | Invarianten zeigen, welche OCL-Ausdrücke häufig und nützlich sind. |
| Tests | `.use`- und `.cmd`-Dateien können in reduzierte Testfälle übersetzt werden. |
| Import/Export | Grammatik und Beispiele helfen später bei `.use`-Kompatibilität. |

## Gefundene Beispielquellen im Originalprojekt

Die Analyse hat folgende relevante Quellen im Originalprojekt identifiziert:

| Quelle | Inhalt | Relevanz |
|---|---|---|
| `../use/use-core/src/main/resources/examples/` | Dokumentations-, Paper-, Generator-, SOIL-, Metamodel- und sonstige Beispiele. | Hauptquelle für Beispielmodelle. |
| `../use/use-core/src/main/resources/examples/Documentation/` | Besonders gut verständliche Modelle wie `Demo`, `Cars`, `Employee`, `AssociationClass`. | Sehr relevant für MVP und Post-MVP. |
| `../use/use-core/src/main/resources/examples/Documentation/Demo/` | Company-Beispiel mit Klassen, Assoziationen, Invarianten und Snapshot-Kommandos. | Sehr relevant für MVP-Testfälle. |
| `../use/use-core/src/main/resources/examples/Documentation/Imports/` | Import-Beispiele, u. a. Library-ähnliche Modelle. | Relevant für Post-MVP Import/Export. |
| `../use/use-core/src/main/resources/examples/Documentation/AggregationsAndCompositions/` | Aggregation-/Komposition-Beispiele. | Post-MVP. |
| `../use/use-core/src/test/resources/org/tzi/use/parser/` | Parser-Testmodelle für Syntaxvarianten und Fehlerfälle. | Relevant für spätere Parser-/Importtests. |
| `../use/use-gui/src/it/resources/testfiles/shell/` | Shell-Integrationstestdateien mit `.use`-Modellen. | Testfallquelle, teilweise komplex. |
| `../use/use-core/src/main/resources/grammars/` | ANTLR-Grammatikfragmente für USE, OCL, SOIL, Shell. | Syntaxreferenz. |

Gefundene Größenordnung aus lokaler Suche:

| Bereich | Anzahl |
|---|---:|
| `.use` Dateien unter `use-core/src/main/resources/examples/` | 85 |
| `.cmd` Dateien unter `use-core/src/main/resources/examples/` | 185 |
| `.use` Dateien unter `use-core/src/test/resources/` | 57 |
| `.use` Dateien unter `use-gui/src/it/resources/testfiles/` | 145 |

## Klassendefinitionen

Die grundlegende USE-Modellstruktur beginnt mit `model` und enthält danach Klassen, Assoziationen und Constraints.

Beispiel aus `../use/use-core/src/main/resources/examples/Documentation/Demo/Demo.use`:

```use
model Company

class Employee
attributes
  name : String
  salary : Integer
end

class Department
attributes
  name : String
  location : String
  budget : Integer
end
```

Relevante Syntax:

```use
model <ModelName>

class <ClassName>
attributes
  <attributeName> : <Type>
operations
  <operationName>(<paramName> : <Type>) : <ReturnType>
end
```

In der Grammatik ist diese Struktur in `../use/use-core/src/main/resources/grammars/base/USEBase.gpart` erkennbar, unter anderem bei `model`, `classDefinition`, `attributeDefinition` und `operationDefinition`.

| Syntaxelement | Beschreibung | MVP-Relevanz | Nutzung |
|---|---|---|---|
| `model` | Start einer USE-Spezifikation mit Modellname. | Referenz | Für späteren Import relevant. |
| `class` | Definition einer UML-Klasse. | Hoch | Direkt auf neues Klassendiagramm abbildbar. |
| `attributes` | Abschnitt für Attribute. | Hoch | Direkt MVP-relevant. |
| `operations` | Abschnitt für Operationen. | Mittel | Im MVP nur Signaturen. |
| `end` | Abschluss einer Klasse oder Assoziation. | Referenz | Für Importparser später relevant. |

## Attribute und Typen

Attribute verwenden im Original die Form:

```use
<attributeName> : <Type>
```

Beispiel aus `Cars.use`:

```use
class Car
attributes
  mileage : Integer
operations
  increaseMileage(kilometers : Integer)
end
```

MVP-relevante Typen:

- `String`,
- `Integer`,
- `Real`,
- `Boolean`.

Post-MVP-relevante Typen und Konstrukte:

- Enumerationen,
- benutzerdefinierte Datentypen,
- Collection-Typen,
- `UnlimitedNatural`,
- `OclAny`,
- `OclVoid`,
- Tuple-Typen.

| Datei/Pfad | Inhalt | Relevante Syntax | MVP-Relevanz | Nutzung |
|---|---|---|---|---|
| `examples/Documentation/Demo/Demo.use` | Company-Modell. | `name : String`, `salary : Integer`, `budget : Integer`. | Hoch | Testmodell für Klassen und Attribute. |
| `examples/Documentation/Cars/Cars.use` | Minimalmodell. | `mileage : Integer`. | Sehr hoch | Sehr kleiner Parser-/Domänentest. |
| `examples/monitoring/Employee.use` | Person/Company-Modell. | `salary : Real`, `age : Integer`. | Mittel | Typbeispiele und Operationen. |
| `examples/soil/projectworld/projectworld.use` | Komplexeres Projektmodell. | Attribute, Enumeration-ähnliche Werte, Operationen. | Niedrig für MVP | Post-MVP-Referenz. |

## Operationen

Operationen werden in `.use`-Dateien als Signaturen oder mit weiterem Verhalten über Constraints/SOIL verwendet.

MVP-relevanter Ausschnitt:

```use
operations
  increaseMileage(kilometers : Integer)
```

Beispiel mit Rückgabetyp aus `../use/use-core/src/main/resources/examples/monitoring/Employee.use`:

```use
operations
  raiseSalary(rate : Real) : Real
```

Post-MVP-Beispiel mit Pre-/Postconditions:

```use
context Person::raiseSalary(rate : Real) : Real
  post raiseSalaryPost:
    salary = salary@pre * (1.0 + rate)
  post resultPost:
    result = salary
```

| Syntaxelement | Beschreibung | MVP-Relevanz | Hinweis |
|---|---|---|---|
| `operationName()` | Operation ohne Parameter. | Mittel | Als Signatur im Klassendiagramm. |
| `operationName(p : Type)` | Operation mit Parametern. | Mittel | Als Signatur im MVP möglich. |
| `: ReturnType` | Rückgabetyp. | Mittel | Als Signatur speichern. |
| `pre` / `post` | Pre-/Postconditions. | Post-MVP | Nicht Teil des MVP-OCL-Subsets. |
| `@pre` | Zugriff auf alten Wert. | Post-MVP | Nicht MVP. |
| `result` | Ergebnisvariable in Postconditions. | Post-MVP | Nicht MVP. |

## Assoziationen, Rollen und Multiplizitäten

Einfache Assoziationen verwenden im Original die Form:

```use
association <AssociationName> between
  <ClassName>[<Multiplicity>] role <roleName>
  <ClassName>[<Multiplicity>] role <roleName>
end
```

Im `Demo.use`-Beispiel treten Assoziationen auch ohne explizite Rollen auf:

```use
association WorksIn between
  Employee[*]
  Department[1..*]
end
```

Beispiel mit Rollen aus `Employee.use`:

```use
association WorksFor between
  Person[*] role employee
  Company[0..1] role employer
end
```

Die Grammatik in `USEBase.gpart` beschreibt Association Ends sinngemäß als:

```text
id "[" multiplicity "]" [ "role" id ] ...
```

Multiplizitätsbeispiele:

```use
Employee[*]
Department[1..*]
Company[0..1]
Person[1..*]
```

| Syntaxelement | Beschreibung | MVP-Relevanz | Nutzung |
|---|---|---|---|
| `association` | Normale UML-Assoziation. | Hoch | Direkt MVP-relevant. |
| `between` | Beginn der Association Ends. | Hoch als Referenz | Späterer Import. |
| `[1]`, `[0..1]`, `[*]`, `[1..*]` | Multiplizitäten. | Hoch | Direkt für Validation. |
| `role` | Rollenname für Navigation. | Hoch | Direkt MVP-relevant. |
| `ordered` | Geordnete Association Ends. | Post-MVP | Nicht MVP. |
| `subsets`, `union`, `redefines` | Erweiterte UML-Beziehungseigenschaften. | Post-MVP | Nicht MVP. |

### Aggregation und Komposition

Aggregation und Komposition sind im Original eigene Syntaxvarianten.

Beispiel aus `../use/use-core/src/main/resources/examples/Documentation/AggregationsAndCompositions/nocycle01.use`:

```use
composition AC between
  A[0..*] role parent
  A[0..*] role child
end
```

Diese Syntax ist für den MVP nicht erforderlich, aber relevant für Post-MVP-Erweiterungen.

### Assoziationsklassen

Beispiel aus `../use/use-core/src/main/resources/examples/Documentation/AssociationClass/AssociationClass.use`:

```use
associationclass WorksFor
between
  Company[0..1] role employer
  Person[1..*] role employee
attributes
  salary : Integer
end
```

Assoziationsklassen sind nicht Teil des MVP. Sie sollten für spätere Modellierungsfähigkeit und `.use` Import/Export als Post-MVP-Thema dokumentiert werden.

## Invarianten und OCL-Ausdrücke

Invarianten werden im Original typischerweise im `constraints`-Abschnitt definiert.

Grundform:

```use
constraints

context <ClassName>
  inv <InvariantName>:
    <OCL expression>
```

Ein sehr kleines Beispiel aus `Cars.use`:

```use
constraints

context Car
  inv MileageNotNegative:
    self.mileage >= 0
```

Beispiele aus `Demo.use`:

```use
context Department
  inv MoreEmployeesThanProjects:
    self.employee->size >= self.project->size
```

```use
context Project
  inv BudgetWithinDepartmentBudget:
    self.budget <= self.department.budget
```

Post-MVP-Beispiele aus `Demo.use`:

```use
context Employee
  inv MoreProjectsHigherSalary:
    Employee.allInstances->forAll(e1, e2 |
      e1.project->size > e2.project->size
        implies e1.salary > e2.salary)
```

```use
context Project
  inv EmployeesInControllingDepartment:
    self.department.employee->includesAll(self.employee)
```

| OCL-Syntax | Beschreibung | MVP-Relevanz | Hinweis |
|---|---|---|---|
| `context Class` | Kontext einer Invariante. | Hoch | Direkt MVP-relevant. |
| `inv Name:` | Benannte Invariante. | Hoch | Direkt MVP-relevant. |
| `self` | Kontextobjekt. | Hoch | Direkt MVP-relevant. |
| `self.attribute` | Attributzugriff. | Hoch | Direkt MVP-relevant. |
| `self.role` | Association Navigation. | Hoch | Direkt MVP-relevant, einfach halten. |
| `->size` | Collection-Größe. | Hoch | MVP-relevant. |
| `->isEmpty`, `->notEmpty` | Collection-Leerheitsprüfung. | Hoch | MVP-relevant. |
| `=`, `<>`, `<`, `<=`, `>`, `>=` | Vergleichsoperatoren. | Hoch | MVP-relevant. |
| `and`, `or`, `not`, `implies` | Boolean-Operatoren. | Hoch | MVP-relevant. |
| `forAll`, `exists` | Quantoren. | Post-MVP | Architektur vorbereiten. |
| `includes`, `includesAll`, `excludes` | Collection Membership. | Post-MVP | In Beispielen häufig, aber nicht MVP-Pflicht. |
| `allInstances` | Zugriff auf alle Instanzen. | Post-MVP | Nicht im MVP-Subset. |
| `let`, `if then else` | Erweiterte OCL-Ausdrücke. | Post-MVP | Nicht MVP. |
| `@pre`, `result` | Postcondition-Kontext. | Post-MVP | Nicht MVP. |

## Objektzustände und Snapshots

Objektzustände werden im Original oft über `.cmd`-Dateien aufgebaut. Diese Kommandos sind für das neue System keine Zielsyntax, aber eine sehr gute Referenz für Snapshot-Daten.

Beispiel aus `../use/use-core/src/main/resources/examples/Documentation/Demo/Demo.cmd`:

```use
!create cs:Department
!set cs.name := 'Computer Science'
!set cs.location := 'Bremen'
!set cs.budget := 10000

!create john : Employee
!set john.name := 'john'
!set john.salary := 4000

!insert (john,cs) into WorksIn
```

Relevante Snapshot-Konzepte:

| Kommando/Syntax | Bedeutung im Original | Relevanz für neues System | Nutzung |
|---|---|---|---|
| `!create obj:Class` | Objektinstanz erzeugen. | Hoch | Entspricht Objekt im Objektdiagramm. |
| `!set obj.attr := value` | Attributwert setzen. | Hoch | Entspricht Slot-Bearbeitung. |
| `!insert (a,b) into Association` | Objektlink erzeugen. | Hoch | Entspricht Link im Objektdiagramm. |
| `!delete`, weitere Shell-/SOIL-Kommandos | Zustand verändern. | Niedrig für MVP | Nicht Zielsyntax. |
| `? <OCL expression>` | OCL-Ausdruck abfragen. | Mittel | Post-MVP für OCL-Konsole interessant. |

Für das neue System sollten `.cmd`-Dateien nicht direkt übernommen werden. Sie können aber in JSON-Snapshots übersetzt werden:

```json
{
  "objects": [
    { "id": "cs", "className": "Department" },
    { "id": "john", "className": "Employee" }
  ],
  "slots": [
    { "objectId": "cs", "attribute": "name", "value": "Computer Science" },
    { "objectId": "john", "attribute": "salary", "value": 4000 }
  ],
  "links": [
    { "association": "WorksIn", "objectIds": ["john", "cs"] }
  ]
}
```

## Relevante Beispielmodelle

| Datei/Pfad | Inhalt | Relevante Syntax | MVP-Relevanz | Nutzung |
|---|---|---|---|---|
| `../use/use-core/src/main/resources/examples/Documentation/Cars/Cars.use` | Sehr kleines Modell `Car` mit Attribut, Operation und Invariante. | `class`, `attributes`, `operations`, `context`, `inv`, `self.mileage >= 0`. | Sehr hoch | Minimaler MVP-Test für Klasse, Attribut, Operation, Invariante. |
| `../use/use-core/src/main/resources/examples/Documentation/Demo/Demo.use` | Company-Modell mit `Employee`, `Department`, `Project`, Assoziationen und Invarianten. | Klassen, Attribute, Assoziationen, Multiplizitäten, Navigation, `size`, Vergleiche. | Hoch | Sehr guter Ausgangspunkt für MVP-Demo, ggf. OCL reduzieren. |
| `../use/use-core/src/main/resources/examples/Documentation/Demo/Demo.cmd` | Snapshot für das Company-Modell. | `!create`, `!set`, `!insert`. | Hoch | Grundlage für Objekt-/Link-Testfall und Validation Demo. |
| `../use/use-core/src/main/resources/examples/monitoring/Employee.use` | Person/Company mit Rollen, Operationen, Pre-/Postconditions. | Rollen, `Real`, Operationen, `pre`, `post`, `@pre`, `result`. | Mittel | Operationensignaturen MVP, Pre/Post Post-MVP. |
| `../use/use-core/src/main/resources/examples/Documentation/Imports/LibraryManagement.use` | Library-ähnliches Modell mit Imports und Association Class. | `import`, `associationclass`, Rollen, Attribute. | Niedrig für MVP, hoch für Post-MVP | Gute Referenz für spätere Library-Demo und Import/Export. |
| `../use/use-core/src/main/resources/examples/Documentation/AssociationClass/AssociationClass.use` | Assoziationsklassen mit Attributen. | `associationclass`, `between`, Rollen, Attribute. | Post-MVP | Referenz für Assoziationsklassen. |
| `../use/use-core/src/main/resources/examples/Documentation/AggregationsAndCompositions/nocycle01.use` | Kompositionsbeispiel. | `composition`, Rollen, Multiplizitäten. | Post-MVP | Referenz für Aggregation/Komposition. |
| `../use/use-core/src/main/resources/examples/soil/projectworld/projectworld.use` | Komplexes Projektmodell mit Operationen, Invarianten und Collection-Ausdrücken. | `forAll`, `exists`, `select`, `isEmpty`, Enumeration-Literale. | Niedrig für MVP | Post-MVP-OCL-Referenz. |
| `../use/use-core/src/main/resources/examples/soil/civstat/civstat.use` | Personen-/Civil-Status-Modell mit vielen OCL-Constraints. | `allInstances`, `forAll`, `isUndefined`, Rollen. | Niedrig bis mittel | Spätere Testfallbibliothek. |
| `../use/use-core/src/test/resources/org/tzi/use/parser/*.use` | Parser-Testmodelle. | Syntaxvarianten, Fehlerfälle, Vererbung, Pre/Post. | Mittel | Spätere Parser-/Importtests. |

## MVP-relevante Syntax

Für den MVP ist folgende Syntax fachlich relevant, auch wenn sie nicht zwingend als Textsyntax importiert werden muss:

```use
model <Name>

class <ClassName>
attributes
  <attributeName> : String
  <attributeName> : Integer
  <attributeName> : Real
  <attributeName> : Boolean
operations
  <operationName>(<paramName> : <Type>) : <ReturnType>
end

association <AssociationName> between
  <ClassA>[<multiplicity>] role <roleA>
  <ClassB>[<multiplicity>] role <roleB>
end

constraints

context <ClassName>
  inv <InvariantName>:
    self.<attributeOrRole> <operator> <literalOrNavigation>
```

MVP-relevante OCL-Beispiele:

```ocl
self.mileage >= 0
```

```ocl
self.employee->size >= self.project->size
```

```ocl
self.budget <= self.department.budget
```

```ocl
self.orders->isEmpty()
```

```ocl
self.items->notEmpty()
```

Hinweis: Originalbeispiele verwenden teilweise `->size` ohne Klammern. Für das neue System muss entschieden werden, ob im MVP nur `size()` oder auch `size` akzeptiert wird. Der Basiskontext nennt `size`, `isEmpty` und `notEmpty`; die konkrete Schreibweise sollte in der OCL-Architektur festgelegt werden.

## Post-MVP-relevante Syntax

Folgende Syntax ist in USE-Beispielen sichtbar, aber nicht MVP-Pflicht:

```ocl
ClassName.allInstances->forAll(x | ...)
```

```ocl
self.collection->exists(x | ...)
```

```ocl
self.collection->select(x | ...)
```

```ocl
self.collection->collect(x | ...)
```

```ocl
if condition then value1 else value2 endif
```

```ocl
let x : Type = expression in expression
```

```use
context Class::operation(p : Type) : ReturnType
  pre conditionName: ...
  post conditionName: ...
```

```use
associationclass WorksFor
between
  Company[0..1] role employer
  Person[1..*] role employee
attributes
  salary : Integer
end
```

```use
composition AC between
  A[0..*] role parent
  A[0..*] role child
end
```

## Syntax, die nicht im MVP unterstützt werden muss

Folgende Syntax muss im MVP nicht unterstützt werden:

- `import ... from ...`,
- `associationclass`,
- `aggregation`,
- `composition`,
- `abstract class`,
- Vererbung mit `class B < A`,
- Mehrfachvererbung,
- `pre` und `post`,
- `@pre`,
- `result`,
- Operation Bodies,
- SOIL-Anweisungen,
- State-Machine-Syntax,
- Generator-/ASSL-Syntax,
- Qualifier,
- `ordered`,
- `subsets`,
- `union`,
- `redefines`,
- `allInstances`,
- `forAll`,
- `exists`,
- `select`,
- `collect`,
- `includesAll`,
- komplexe Undefined-/Null-Syntax wie `oclUndefined(...)` oder `isUndefined()`.

Einige dieser Elemente sind fachlich wichtig für spätere Ausbaustufen, sollen aber den MVP nicht vergrößern.

## Ableitungen für das neue Projekt

Aus den USE-Beispielen ergeben sich folgende Anforderungen und Architekturhinweise:

| Ableitung | Begründung | Zielbereich |
|---|---|---|
| Klassen, Attribute und Assoziationen müssen als zusammenhängendes Modell validiert werden. | Beispiele wie `Demo.use` zeigen, dass OCL-Navigation auf Modellstruktur basiert. | Backend-Domänenmodell |
| Rollen sollten explizit modelliert werden. | Rollen wie `employee` und `employer` sind Grundlage für Navigation. | Klassendiagramm, OCL |
| Multiplizitäten müssen maschinenlesbar repräsentiert werden. | Syntax wie `[1..*]` und `[0..1]` ist zentral für Linkvalidierung. | Validation Engine |
| OCL braucht Typechecking vor Evaluation. | Ausdrücke wie `self.department.budget` hängen von Navigationstypen ab. | OCL Engine |
| Validation Results müssen Elementbezug enthalten. | Fehler sollen im Objektdiagramm markiert werden. | API, Frontend |
| Snapshot-Daten sollten Objekte, Slots und Links trennen. | `.cmd`-Dateien zeigen genau diese drei Konzepte. | JSON-Projektformat |
| MVP-Testmodell sollte aus kleinen USE-Beispielen abgeleitet werden. | `Cars.use` und reduzierte `Demo.use` sind geeignet. | Teststrategie |

## Ableitungen für Testfälle

Geeignete Testfallgruppen:

| Testfallgruppe | Quelle | Erwartete Nutzung |
|---|---|---|
| Minimalmodell mit einer Klasse | `Documentation/Cars/Cars.use` | Parser-/Domänenmodelltest, einfache Invariante. |
| Klassenmodell mit mehreren Klassen | `Documentation/Demo/Demo.use` | Klassendiagramm- und JSON-Modelltest. |
| Snapshot-Aufbau | `Documentation/Demo/Demo.cmd` | Objekt-, Slot- und Link-Testdaten. |
| Multiplicity Violation | Reduzierte Variante von `Demo.use`/`Demo.cmd` | Validation-Test für fehlende oder zu viele Links. |
| OCL-Attributvergleich | `Cars.use`, `Demo.use` | Evaluator-Test für primitive Werte. |
| OCL-Navigation | `Demo.use` | Evaluator-Test für einfache Rollen-Navigation. |
| OCL-Collection `size` | `Demo.use` | MVP-OCL-Test. |
| Pre-/Postconditions | `monitoring/Employee.use` | Post-MVP-Test. |
| Association Class | `Documentation/AssociationClass/AssociationClass.use` | Post-MVP-Test. |
| Import-Syntax | `Documentation/Imports/*.use` | Späterer Import-/Export-Test. |

Beispiel für einen MVP-Testfall aus `Cars.use`:

```use
class Car
attributes
  mileage : Integer
end

context Car
  inv MileageNotNegative:
    self.mileage >= 0
```

Mögliche Testdaten:

| Objekt | Attributwert | Erwartung |
|---|---:|---|
| `car1:Car` | `mileage = 10` | Invariante erfüllt |
| `car2:Car` | `mileage = -1` | Invariante verletzt |

## Ableitungen für späteren `.use` Import/Export

Für späteren `.use` Import/Export sind folgende Originalquellen besonders relevant:

| Quelle | Bedeutung |
|---|---|
| `../use/use-core/src/main/resources/grammars/base/USEBase.gpart` | Zentrale USE-Modellsyntax. |
| `../use/use-core/src/main/resources/grammars/ocl/OCL.gpart` | OCL-Grammatik. |
| `../use/use-core/src/main/resources/grammars/base/OCLBase.gpart` | Gemeinsame OCL-Regeln. |
| `../use/use-core/src/main/resources/grammars/base/OCLLexerRules.gpart` | Lexer-Regeln. |
| `../use/use-core/src/main/resources/examples/Documentation/*.use` | Realistische Importbeispiele. |
| `../use/use-core/src/test/resources/org/tzi/use/parser/*.use` | Syntax- und Fehlerfalltests. |

Mögliche Import-Stufen:

| Stufe | Unterstützte Syntax | Ziel |
|---|---|---|
| Import Stufe 1 | Klassen, Attribute, primitive Typen. | Einfacher Klassendiagrammimport. |
| Import Stufe 2 | Binäre Assoziationen, Rollen, Multiplizitäten. | Vollständiger MVP-Klassenmodellimport. |
| Import Stufe 3 | Invarianten im MVP-OCL-Subset. | Validierbare Modelle importieren. |
| Import Stufe 4 | `.cmd`-basierte Objektzustände. | Snapshot-Import. |
| Import Stufe 5 | Post-MVP-OCL und erweiterte UML-Konzepte. | Breitere USE-Kompatibilität. |

Für Export gilt: Das neue JSON-Modell muss nicht verlustfrei in volle USE-Syntax exportierbar sein. Export sollte später auf klar definierte unterstützte Teilmengen begrenzt werden.

## Offene Fragen

- Soll der MVP-OCL-Parser sowohl `->size` als auch `->size()` akzeptieren?
- Sollen Association Roles im MVP verpflichtend sein, auch wenn USE-Beispiele wie `Demo.use` teilweise keine expliziten Rollen angeben?
- Wie werden aus USE abgeleitete Rollennamen bestimmt, wenn sie im Original fehlen?
- Soll ein späterer `.cmd` Import unterstützt werden oder nur `.use` Import?
- Welche reduzierte Variante von `Demo.use` soll als Standard-MVP-Demo dienen?
- Wie wird `undefined` im MVP behandelt, wenn Attributwerte fehlen?
- Sollen String-Literale im MVP wie USE mit einfachen Anführungszeichen geschrieben werden?
- Welche Syntaxfehler aus `use-core/src/test/resources/org/tzi/use/parser/` sollen später als Importtests übernommen werden?

## Zusammenfassung

Das originale USE-Projekt enthält umfangreiche Syntax- und Beispielquellen für UML/OCL-Modelle. Für den MVP des neuen Websystems sind besonders relevant:

- `class`,
- `attributes`,
- einfache `operations` als Signaturen,
- `association`,
- `between`,
- Rollen,
- Multiplizitäten,
- `constraints`,
- `context`,
- `inv`,
- `self`,
- Attributzugriff,
- einfache Navigation,
- Vergleiche,
- Boolean-Operatoren,
- `size`,
- `isEmpty`,
- `notEmpty`,
- Snapshot-Konzepte aus `!create`, `!set` und `!insert`.

Die besten unmittelbaren Referenzen sind `Cars.use`, `Demo.use` und `Demo.cmd`. Komplexere Beispiele wie `Employee.use`, `LibraryManagement.use`, `AssociationClass.use`, Aggregation-/Kompositionsbeispiele und Parser-Testdateien sind wertvoll für Post-MVP, Teststrategie und späteren `.use` Import/Export.

Das neue System sollte diese Beispiele nicht technisch übernehmen, sondern daraus fachliche Anforderungen, Testfälle und klar abgegrenzte Syntax-Teilmengen ableiten.
