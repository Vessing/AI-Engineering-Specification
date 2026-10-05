# UML/OCL Scope

## Zweck dieser Datei

Diese Datei beschreibt den fachlichen UML/OCL-Scope des neuen Websystems.

Sie legt fest:

- welche UML-Bestandteile für das Zielsystem relevant sind,
- welche UML-Bestandteile bewusst nicht betrachtet werden,
- welches OCL-Subset im MVP unterstützt werden soll,
- welche OCL-Features später ergänzt werden können,
- wie UML-Modell, Objektdiagramm, Snapshot und OCL fachlich zusammenhängen,
- welche Bereiche des originalen USE-Projekts als fachliche Referenz dienen.

Die Datei beschreibt Domänenumfang und Abgrenzung. Sie ist keine Implementierungsspezifikation und keine Aufforderung, Code aus dem originalen USE-Projekt zu übernehmen.

## Fachlicher Fokus

Der fachliche Kern des neuen Systems ist die Verbindung von Strukturmodellierung und Constraint-Validierung:

```text
UML-Klassenmodell
-> UML-Objektdiagramm / Snapshot
-> OCL-Invarianten
-> Constraint Validation
-> Validation Results
```

Das Klassenmodell definiert die zulässige Struktur. Der Snapshot enthält konkrete Objektinstanzen und Links. OCL-Invarianten formulieren Bedingungen über diese Objekte. Die Validierung prüft, ob der Snapshot zur Modellstruktur passt und ob alle relevanten OCL-Invarianten erfüllt sind.

Für den MVP steht nicht die vollständige UML- oder OCL-Abdeckung im Vordergrund, sondern ein kleiner, konsistenter und erweiterbarer fachlicher Kern.

## UML-Scope

### Relevante UML-Bestandteile

| UML-Bestandteil | MVP-Status | Beschreibung | Referenz aus USE | Hinweise |
|---|---|---|---|---|
| Klassen | Muss | Zentrale Typen des Klassenmodells. | `MClass`, `MClassImpl` | Grundlage für Klassendiagramm, Objekte und OCL-Kontext. |
| Attribute | Muss | Benannte Eigenschaften einer Klasse mit Typ. | `MAttribute` | MVP mit primitiven Typen. |
| Operationen als Signaturen | Muss | Name, Parameter und Rückgabetyp ohne Ausführung. | `MOperation` | Keine Bodies, keine Pre-/Postconditions im MVP. |
| Assoziationen | Muss | Beziehungen zwischen Klassen. | `MAssociation`, `MAssociationImpl` | MVP fokussiert binäre Assoziationen. |
| Rollen | Muss | Namen der Association Ends. | `MAssociationEnd` | Wichtig für UI-Beschriftung und OCL-Navigation. |
| Multiplizitäten | Muss | Kardinalitätsregeln an Association Ends. | `MMultiplicity` | Grundlage für Multiplicity Checks. |
| Objekte | Muss | Instanzen von Klassen im Snapshot. | `MObject`, `MObjectImpl` | Grundlage des Objektdiagramms. |
| Slots / Attributwerte | Muss | Konkrete Attributwerte eines Objekts. | `MObjectState` | Werte müssen zum Attributtyp passen. |
| Objektlinks | Muss | Instanzen von Assoziationen zwischen Objekten. | `MLink`, `MLinkEnd` | Grundlage für Navigation und Multiplizitätsprüfung. |
| Snapshots | Muss | Konkreter Objektzustand eines Projekts. | `MSystemState` | MVP mit einem aktiven Snapshot. |
| Vererbung | Später | Generalisierung/Spezialisierung von Klassen. | `MGeneralization` | Erweitert Typechecking und Objektvalidierung. |
| Enumerationen | Später | Benannte Literaltypen. | `EnumType`, `EnumValue` | Sinnvoll für realistischere Modelle. |
| Aggregation/Komposition | Später | Spezielle Whole/Part-Assoziationsformen. | `MAggregationKind`, Composition-Beispiele | Nicht MVP, später für stärkere UML-Semantik. |
| Assoziationsklassen | Später | Assoziationen mit eigenen Attributen. | `MAssociationClass` | Fachlich relevant, aber komplexer als MVP. |

### Nicht relevante UML-Bestandteile

| UML-Bestandteil | Status | Begründung |
|---|---|---|
| Sequenzdiagramme | Nicht relevant | Der Zielumfang fokussiert Strukturmodellierung und Snapshot-Validierung, nicht Interaktionsabläufe. |
| State Machines | Nicht relevant | Verhaltenszustände und Zustandsautomaten sind nicht Teil des MVP oder Zielkerns. |
| Aktivitätsdiagramme | Nicht relevant | Prozess- und Ablaufmodellierung liegt außerhalb des Scopes. |
| Deploymentdiagramme | Nicht relevant | Technische Verteilungsstruktur ist kein fachlicher Modellierungsumfang des Tools. |
| Komponentendiagramme | Nicht relevant | Komponentenarchitektur ist nicht Teil der UML/OCL-Validierungsdomäne des MVP. |
| Vollständige Modellanimation | Nicht MVP | USE kann Zustände über Kommandos verändern; das neue System fokussiert zunächst direkte Snapshot-Bearbeitung. |

## OCL-Scope

### OCL im MVP

Im MVP wird OCL ausschließlich für Klasseninvarianten verwendet. Eine Invariante gehört zu einer Kontextklasse und wird gegen alle passenden Objekte im aktuellen Snapshot geprüft.

| OCL-Bestandteil | MVP-Status | Beispiel | Zweck |
|---|---|---|---|
| Klasseninvarianten | Muss | `context User inv maxBooks` | Hauptform von Constraints im MVP. |
| Kontextklasse | Muss | `User` | Bestimmt den Typ von `self`. |
| `self` | Muss | `self` | Aktuelles Objekt der Kontextklasse. |
| Attributzugriff | Muss | `self.books` | Zugriff auf Slot-Werte des Objekts. |
| Einfache Association Navigation | Muss | `self.borrowedBooks` | Navigation über Rollen und Objektlinks. |
| String-Literale | Muss | `'Alice'` | Vergleich mit String-Attributen. |
| Integer-Literale | Muss | `5` | Zählwerte und numerische Constraints. |
| Real-Literale | Muss | `12.5` | Numerische Constraints mit Real-Werten. |
| Boolean-Literale | Muss | `true`, `false` | Boolesche Attribute und Bedingungen. |
| Vergleichsoperatoren | Muss | `self.books <= 5` | Prüfung von Attributen und berechneten Werten. |
| Boolean-Operatoren | Muss | `and`, `or`, `not` | Zusammengesetzte Bedingungen. |
| Klammern | Muss | `(self.books <= 5)` | Gruppierung und Lesbarkeit. |
| `size` | Muss | `self.borrowedBooks->size() <= 5` | Anzahl navigierter Objekte prüfen. |
| `isEmpty` | Muss | `self.borrowedBooks->isEmpty()` | Leere Linkmengen prüfen. |
| `notEmpty` | Muss | `self.borrowedBooks->notEmpty()` | Nicht-leere Linkmengen prüfen. |

Die OCL-Verarbeitung soll fachlich als Pipeline verstanden werden:

```text
Lexer -> Parser -> AST -> Type Checker -> Evaluator -> Validation Result
```

Auch wenn das MVP-Subset klein ist, soll es nicht als Sammlung einfacher Regex-Regeln entstehen. Der fachliche Scope verlangt eine erweiterbare OCL-Verarbeitung.

### OCL nach dem MVP

| OCL-Bestandteil | Post-MVP-Relevanz | Grund |
|---|---|---|
| `forAll` | Hoch | Zentrale Quantifizierung über Collections. |
| `exists` | Hoch | Häufige Existenzprüfung in Constraints. |
| `select` | Mittel bis hoch | Filterung von Collection-Ausdrücken. |
| `collect` | Mittel bis hoch | Projektion über Collections. |
| `includes` / `excludes` | Mittel | Membership-Prüfungen für Collections. |
| `let` | Mittel | Lesbare Zwischenwerte in komplexeren Ausdrücken. |
| `if-then-else` | Mittel | Bedingte Constraint-Logik. |
| `allInstances` | Hoch | Klassenweite Ausdrücke und globale Constraints. |
| Preconditions | Mittel | Constraints vor Operationsausführung. |
| Postconditions | Mittel | Constraints nach Operationsausführung. |
| Derived Attributes | Hoch | Berechnete Attribute über OCL. |
| Init Values | Mittel | Initialwerte für Slots über OCL. |
| Erweiterte Undefined-/Invalid-Semantik | Hoch | Für vollständigeres OCL-Verhalten notwendig. |

## Zusammenhang zwischen UML und OCL

OCL-Ausdrücke sind nicht isoliert. Sie beziehen sich auf das UML-Modell und werden gegen einen konkreten Snapshot ausgewertet.

| UML/OCL-Element | Rolle im Zusammenspiel |
|---|---|
| Klasse | Definiert den Kontext einer Invariante und den Typ von Objekten. |
| Attribut | Kann in OCL über `self.attribute` gelesen werden. |
| Operation | Wird im MVP nur als Signatur modelliert; spätere OCL-Pre-/Postconditions können daran gebunden werden. |
| Association | Definiert erlaubte Beziehungen zwischen Klassen. |
| Rolle | Dient als Navigationsname in OCL. |
| Multiplizität | Wird unabhängig von OCL strukturell validiert. |
| Objekt | Konkrete Instanz, für die Invarianten geprüft werden. |
| Slot | Liefert den konkreten Wert für Attributzugriffe. |
| Objektlink | Liefert Navigationspfade und Linkmengen. |
| Snapshot | Bewertungsgrundlage für OCL und Strukturvalidierung. |
| Invariante | OCL-Bedingung, die für alle Objekte der Kontextklasse gelten muss. |

Beispielhafter Zusammenhang:

```ocl
context User inv maxBooks:
  self.borrowedBooks->size() <= 5
```

Fachlich bedeutet das:

- `User` ist eine Klasse im Klassenmodell.
- `borrowedBooks` ist eine Rolle an einer Assoziation.
- `self` ist ein konkretes `User`-Objekt im Snapshot.
- Objektlinks bestimmen, welche `Book`-Objekte navigiert werden.
- `size()` zählt die navigierten Bücher.
- Das Ergebnis muss für jedes `User`-Objekt `true` sein.

## Snapshot-Konzept

Ein Snapshot ist ein konkreter Zustand eines UML-Modells.

Im MVP enthält ein Snapshot:

- Objektinstanzen,
- Objekt-IDs und sichtbare Objektnamen,
- Referenzen auf Klassen,
- Slots mit Attributwerten,
- Objektlinks als Instanzen von Assoziationen,
- optional Diagrammpositionen für die Darstellung.

Der Snapshot ist nicht das Klassenmodell selbst. Er ist ein konkreter Zustand, der gegen das Klassenmodell geprüft wird.

| Prüfbereich | Beispiel |
|---|---|
| Objektstruktur | Existiert die Klasse des Objekts? |
| Slot-Struktur | Gibt es für jeden Slot ein passendes Attribut? |
| Slot-Typen | Passt der Wert `6` zu einem `Integer`-Attribut? |
| Link-Struktur | Passt ein Link zur gewählten Association? |
| Multiplizität | Hat ein Objekt zu viele oder zu wenige Links? |
| Invarianten | Erfüllt jedes Objekt der Kontextklasse alle relevanten OCL-Invarianten? |

Im MVP wird ein aktiver Snapshot pro Projekt als ausreichend betrachtet. Mehrere benannte Snapshots sind ein Post-MVP-Thema.

## MVP-Scope

| Bereich | Muss im MVP enthalten sein | Nicht im MVP enthalten |
|---|---|---|
| UML-Klassenmodell | Klassen, Attribute, primitive Typen, Operationensignaturen, Assoziationen, Rollen, Multiplizitäten | Vererbung, Enumerationen, Aggregation/Komposition, Assoziationsklassen |
| Objektmodell | Objekte, Slots, Attributwerte, Objektlinks, ein aktiver Snapshot | Mehrere Snapshots, Objektlebenszyklus, Modellanimation |
| OCL | Invarianten, `self`, Attributzugriff, einfache Navigation, Vergleiche, Boolean-Operatoren, `size/isEmpty/notEmpty` | Vollständiges OCL, Quantoren, `let`, `if-then-else`, `allInstances` |
| Validierung | Strukturprüfung, Slot-Typprüfung, Linkprüfung, Multiplizitätsprüfung, Invariantenauswertung | Vollständige USE-Feature-Parität, Pre-/Postcondition-Validierung |
| Ergebnisdarstellung | Strukturierte Validation Results mit Elementbezug | Vollständiger Evaluation Trace |

## Post-MVP-Scope

| Erweiterung | Fachliche Motivation | Abhängigkeit |
|---|---|---|
| Vererbung | Realistischere Klassendiagramme und polymorphe Modelle. | Erweiterung von Typmodell, Objektvalidierung und OCL-Typechecking. |
| Enumerationen | Domänenspezifische Werte statt reiner Strings. | Erweiterung von Typmodell, Slot-Editoren und OCL-Literalen. |
| Aggregation/Komposition | Präzisere Whole/Part-Semantik. | Erweiterung von Association Ends und Strukturvalidierung. |
| Assoziationsklassen | Beziehungen mit eigenen Attributen. | Erweiterung von Klassen-, Association- und Objektmodell. |
| Mehrere Snapshots | Vergleich oder Verwaltung mehrerer Objektzustände. | Stabiles Snapshot-Modell und Persistenzkonzept. |
| Erweiterte OCL-Collections | Ausdrucksstärkere Invarianten. | Erweiterbarer Parser, AST, Typechecker und Evaluator. |
| Pre-/Postconditions | Operationale Constraints. | Operationen müssen über Signaturen hinaus fachlich nutzbar werden. |
| Derived Attributes | Berechnete Modellwerte. | OCL-Auswertung außerhalb reiner Invariant Checks. |
| Init Values | Automatische Initialisierung von Slots. | Regeln für Objektanlage und fehlende Werte. |
| `.use` Import/Export | Interoperabilität mit USE-naher Syntax. | Stabiles internes Projektformat. |

## Nicht-Ziele

| Nicht-Ziel | Begründung |
|---|---|
| Sequenzdiagramme | Nicht Teil des strukturellen UML/OCL-Kerns. |
| State Machines | Verhaltensorientierter Scope, nicht Ziel des MVP. |
| Aktivitätsdiagramme | Ablaufmodellierung liegt außerhalb des geplanten Systems. |
| Deploymentdiagramme | Infrastrukturmodellierung ist nicht fachlicher Kern. |
| Komponentendiagramme | Kompositionsstruktur von Softwarekomponenten ist nicht Zielumfang. |
| Vollständige OCL-Parität im MVP | Zu großer Scope; OCL wird schrittweise erweitert. |
| Vollständige USE-Kompatibilität im MVP | USE ist Referenz, nicht Ziel einer 1:1-Neuimplementierung. |
| Direkte USE-Core-Nutzung | Widerspricht der Leitentscheidung eines neuen Systems. |
| Alte USE-GUI | Nicht Webzielbild und nicht Grundlage der neuen UX. |
| Vollständige Modellanimation | Für den vertikalen MVP-Durchstich nicht erforderlich. |

## Referenz zum originalen USE-Projekt

Das originale USE-Projekt dient als fachliche Referenz für Konzepte, Verhalten, Syntax und Testfälle.

| Originalbereich | Relevanz für neuen Scope | Nutzung |
|---|---|---|
| `use-core/src/main/java/org/tzi/use/uml/mm/` | Klassenmodell, Attribute, Operationen, Assoziationen, Rollen, Multiplizitäten, Invarianten. | Fachliche Referenz für UML-Metamodell. |
| `use-core/src/main/java/org/tzi/use/uml/sys/` | Objekte, Objektzustände, Links, Systemzustände, Strukturprüfung. | Fachliche Referenz für Snapshot und Validierung. |
| `use-core/src/main/java/org/tzi/use/uml/ocl/expr/` | OCL-Ausdrucksmodell. | Referenz für OCL-Konzepte und Erweiterbarkeit. |
| `use-core/src/main/java/org/tzi/use/uml/ocl/type/` | OCL- und UML-nahe Typen. | Referenz für Typechecking. |
| `use-core/src/main/java/org/tzi/use/uml/ocl/value/` | Werte für OCL-Auswertung. | Referenz für Evaluationsmodell. |
| `use-core/src/main/java/org/tzi/use/parser/` | Parser- und AST-Strukturen. | Syntax- und Architekturreferenz, keine Codeübernahme. |
| `use-core/src/main/resources/examples/` | Beispielmodelle und Snapshots. | Testfall- und Demo-Referenz. |
| `use-core/src/test/` und `use-core/src/it/` | Parser-, OCL- und Systemtests. | Quelle für spätere Regressionstests. |
| `use-gui/` | Desktop-GUI. | Höchstens historische Orientierung, nicht übernehmen. |

Wichtige Leitlinie:

> Das neue System übernimmt Konzepte, Verhaltenserwartungen und Testideen aus USE, aber keinen USE-Code, keine USE-Core-Dependency und keine direkte GUI-Migration.

## Scope-Entscheidungen

| Entscheidung | Begründung | Konsequenz |
|---|---|---|
| Klassendiagramme und Objektdiagramme sind Kernscope. | Sie tragen den vertikalen Ablauf von Modellierung bis Validierung. | Andere Diagrammarten bleiben ausgeschlossen. |
| Invarianten sind der einzige OCL-Constraint-Typ im MVP. | Sie reichen für den Kernworkflow und sind gut mit Snapshots validierbar. | Pre-/Postconditions folgen später. |
| Operationen sind im MVP nur Signaturen. | Ausführbare Operationen würden Modellanimation und Zustandsänderungen erfordern. | Keine Operation Bodies im MVP. |
| Ein aktiver Snapshot reicht im MVP. | Der Constraint-Workflow lässt sich damit vollständig zeigen. | Mehrere Snapshots werden Post-MVP. |
| Primitive Typen reichen initial. | Sie decken die wichtigsten Attribut- und OCL-Beispiele ab. | Enumerationen und eigene Datentypen folgen später. |
| OCL wird klein, aber pipeline-basiert gestartet. | Erweiterbarkeit ist wichtiger als ein Regex-basierter Schnellweg. | Lexer, Parser, AST, Typechecker und Evaluator werden konzeptionell getrennt. |
| USE ist Referenz, nicht Implementierungsbasis. | Ziel ist ein neues Websystem. | Keine technische Migration und keine Runtime Dependency. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Wie genau werden fehlende Slot-Werte im MVP behandelt? | Einfluss auf OCL-Evaluation und Validation Results. |
| Soll Navigation im MVP immer über Rollennamen erfolgen oder auch über Klassennamen erlaubt sein? | Einfluss auf OCL-Syntax und Typechecker. |
| Werden Association Ends im MVP explizit navigierbar/nicht navigierbar modelliert? | Einfluss auf OCL-Navigation. |
| Welche Multiplicity-Schreibweisen sind im MVP verbindlich? | Einfluss auf UI, JSON-Format und Validation Service. |
| Soll das MVP OCL-Schreibweisen aus USE wie `->size` ohne Klammern akzeptieren? | Einfluss auf Parser-Kompatibilität und Dokumentation. |
| Wie stark soll Undefined-/Invalid-Semantik im MVP vereinfacht werden? | Einfluss auf Evaluator und Fehlertypen. |
| Welches reduzierte USE-Beispielmodell wird als primäre fachliche Demo verwendet? | Einfluss auf Tests, Akzeptanz und Dokumentation. |

## Zusammenfassung

Der fachliche UML/OCL-Scope des neuen Systems ist bewusst fokussiert:

- UML-Klassenmodelle mit Klassen, Attributen, Operationensignaturen, Assoziationen, Rollen und Multiplizitäten,
- UML-Objektdiagramme als Snapshots mit Objekten, Slots und Objektlinks,
- OCL-Invarianten als MVP-Constraint-Typ,
- ein kleines, erweiterbares OCL-Subset,
- strukturierte Validierung von Modell, Snapshot, Multiplizitäten und Invarianten.

Nicht relevant sind verhaltensorientierte UML-Diagramme wie Sequenzdiagramme, State Machines und Aktivitätsdiagramme. Das originale USE-Projekt bleibt eine wichtige fachliche Referenz, wird aber nicht technisch übernommen.
