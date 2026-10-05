# OCL Error Handling and Source Locations

## Zweck dieser Datei

Diese Datei definiert ein durchgängiges Fehler- und Source-Location-Konzept für die OCL-Pipeline:

```text
OCL Text
-> Lexer
-> Parser
-> AST
-> Typechecker
-> Evaluator
-> Validation Result
-> Frontend
```

OCL-Fehler müssen fachlich korrekt, maschinenlesbar und für Nutzer verständlich sein. Das Frontend muss sie im OCL Editor markieren, im Properties Panel einem Modellelement zuordnen und im Validation Results Panel anzeigen können.

Normative Grundlage für Syntax, Well-formedness, Typkonformität und Evaluation ist **OMG OCL 2.4**. Fehlercodes, Meldungstexte und Source Ranges sind produktspezifische Darstellungen dieser Regeln. Das originale USE-Projekt dient nur als ergänzende Referenz für Fehlersituationen und Regressionstests.

## Anforderungen an OCL-Fehler

| Anforderung | Konsequenz |
|---|---|
| Phasengenau | Lexer-, Parser-, Typ-, Evaluations- und Validationfehler bleiben unterscheidbar. |
| Quelltextgenau | Diagnose markiert möglichst das kleinste ursächliche Textsegment. |
| UI-mappbar | Invariante, Klasse, Attribut, Operation und Kontextobjekt sind über stabile IDs referenzierbar. |
| Nutzerverständlich | `userMessage` verwendet Modellnamen statt primär technischer IDs. |
| Technisch diagnostizierbar | `technicalMessage`, Code und strukturierte Details bleiben erhalten. |
| Mehrsprachig vorbereitbar | Code und Details sind stabil; Texte dürfen lokalisiert werden. |
| Fehlerfortsetzung | Ein Fehler verhindert nicht unnötig weitere unabhängige Diagnosen. |
| Versionssicher | Eine Diagnose kann keinem inzwischen geänderten Editorinhalt zugeordnet werden. |
| Datenschutzbewusst | Debug-Daten enthalten nicht automatisch vollständige Snapshots oder sensible Werte. |
| OCL-konform | `null` und `invalid` werden nicht pauschal mit technischen Fehlern gleichgesetzt. |

### Trennung der Ergebnisarten

| Ergebnisart | Bedeutung | HTTP-Verhalten |
|---|---|---|
| `ApiErrorDto` | Request, Transport oder Server konnte nicht regulär verarbeitet werden | passender 4xx/5xx-Status |
| `OclDiagnosticDto` | Parse-/Typecheck-/Einzelauswertungsdiagnose eines OCL-Endpunkts | reguläre Response, sofern Request technisch gültig |
| `ValidationErrorDto` | Finding innerhalb eines vollständigen Validation Runs | reguläre Validation Response |
| `INVARIANT_VIOLATION` | korrekt geparste und ausgewertete Invariante ergibt `false` | kein technischer API-Fehler |

Ein fachlich ungültiges Modell ist kein HTTP-500-Fehler. Unerwartete Exceptions werden dagegen als `ApiErrorDto` behandelt und serverseitig mit einer Correlation ID protokolliert.

## Fehlerarten

| Kategorie | Entstehung | Beispiel |
|---|---|---|
| Lexer Error | Zeichen oder Literal kann nicht tokenisiert werden | ungültiges Zeichen, nicht geschlossener String |
| Parser/Syntax Error | Tokenfolge passt nicht zur Grammatik | fehlende `)`, fehlendes `endif` |
| Name Resolution Error | Modell- oder Variablenreferenz unbekannt | `self.boks`, unbekannte Iteratorvariable |
| Type Error | Ausdruck verletzt OCL-Typregeln | `self.name <= 5` |
| Operation Resolution Error | Operation fehlt oder Signatur passt nicht | `->szie()`, falsche Argumentzahl |
| Evaluation Error | typgeprüfter Ausdruck kann im konkreten Kontext nicht regulär ausgewertet werden | inkonsistenter Snapshot |
| OCL `invalid` | fachlicher OCL-Wert beziehungsweise Fehlerwert | Division durch null, invalid Navigation gemäß Semantik |
| OCL `null` | definierter fehlender Wert | optionale Navigation ohne Ziel |
| Constraint Violation | Boolean-Constraint ergibt `false` | `alice.books = 6` bei `<= 5` |
| Unsupported Feature | syntaktisch erkannt, aber nicht im aktivierten Profil | noch nicht implementiertes `closure` |

`null`, `invalid`, `false` und technische Exceptions sind nicht austauschbar. Ob ein `invalid`-Wert als Evaluation Finding erscheint, hängt vom Constraint-Kontext und OCL-2.4-Propagation ab.

## Error Codes

### Lexer und Parser

| Code | Bedeutung |
|---|---|
| `INVALID_CHARACTER` | Zeichen ist an dieser Position nicht zulässig. |
| `UNTERMINATED_STRING` | Stringliteral endet nicht. |
| `INVALID_ESCAPE_SEQUENCE` | Escape-Sequenz ist ungültig. |
| `INVALID_NUMERIC_LITERAL` | Zahl kann nicht gemäß OCL-Syntax gelesen werden. |
| `UNEXPECTED_TOKEN` | aktuelles Token passt nicht zur Grammatik. |
| `MISSING_TOKEN` | erwartetes Token fehlt. |
| `SYNTAX_ERROR` | stabiler Obercode, falls kein spezifischer Parsercode transportiert wird. |
| `UNSUPPORTED_SYNTAX` | Syntax ist erkannt, aber im aktiven OCL-Profil nicht verfügbar. |

### Namens- und Typprüfung

| Code | Bedeutung |
|---|---|
| `TYPE_ERROR` | allgemeine Typregel verletzt. |
| `UNKNOWN_CLASS` | Typ-/Klassenreferenz existiert nicht. |
| `UNKNOWN_ATTRIBUTE` | Attribut existiert am Receivertyp nicht. |
| `UNKNOWN_ASSOCIATION_ROLE` | navigierte Association Role existiert nicht. |
| `UNKNOWN_VARIABLE` | lokale Variable oder Parameter unbekannt. |
| `INVALID_ITERATOR_VARIABLE` | Deklaration, Scope oder Typ einer Iteratorvariable ungültig. |
| `INVALID_ITERATOR_BODY_TYPE` | Iterator-Body hat nicht den erwarteten Typ. |
| `UNKNOWN_OPERATION` | Operation existiert für den Source-Typ nicht. |
| `INVALID_OPERATION` | Operation existiert, ist im Kontext aber unzulässig. |
| `INVALID_ARGUMENT_COUNT` | Anzahl der Argumente passt nicht. |
| `INVALID_ARGUMENT_TYPE` | Argumenttyp passt nicht zur Signatur. |
| `AMBIGUOUS_OPERATION` | Overload-Auflösung ist nicht eindeutig. |
| `INVALID_COLLECTION_OPERATION` | Operation ist für diese Collection-Art unzulässig. |
| `INCOMPATIBLE_BRANCH_TYPES` | `if`-Zweige besitzen keinen gültigen gemeinsamen Typ. |
| `CONSTRAINT_NOT_BOOLEAN` | Invariante, Pre- oder Postcondition ergibt nicht `Boolean`. |

### Evaluation und Validation

| Code | Bedeutung |
|---|---|
| `EVALUATION_ERROR` | allgemeiner kontrollierter Evaluationsfehler. |
| `UNDEFINED_VALUE` | benötigter Wert fehlt außerhalb zulässiger OCL-`null`-Semantik. |
| `INVALID_NAVIGATION` | Navigation kann im konkreten Zustand nicht ausgeführt werden. |
| `NULL_NAVIGATION` | Navigation auf `null`, soweit sie zu einem Finding führt. |
| `INVALID_VALUE` | Ausdruck ergibt OCL `invalid` und der Kontext kann nicht erfolgreich abgeschlossen werden. |
| `ITERATION_LIMIT_EXCEEDED` | Evaluationsbudget eines Iterators überschritten. |
| `DERIVATION_CYCLE` | Zyklus bei Derived-Property-Auswertung. |
| `SNAPSHOT_REQUIRED` | erforderlicher Snapshot fehlt. |
| `INVARIANT_VIOLATION` | Invariante ergibt für ein Kontextobjekt `false`. |
| `PRECONDITION_VIOLATION` | Precondition ergibt `false`. |
| `POSTCONDITION_VIOLATION` | Postcondition ergibt `false`. |

Codes sind Bestandteil des API-Vertrags. Umbenennungen erfordern API-Versionierung oder eine kompatible Mapping-Schicht.

## Severity-Modell

| Severity | Bedeutung | Beispiel | UI |
|---|---|---|---|
| `ERROR` | Ausdruck/Constraint ist ungültig oder verletzt | Syntaxfehler, Typfehler, Invariantenverletzung | rote Markierung, Error Badge |
| `WARNING` | auswertbar, aber problematisch oder eingeschränkt | Shadowing, teures `allInstances()` | gelbe Markierung |
| `INFO` | fachliche Zusatzinformation | verwendetes Compatibility-Profil | neutrale Information |

`INVARIANT_VIOLATION` ist `ERROR`, obwohl keine technische Störung vorliegt. Ein `UNSUPPORTED_SYNTAX` kann während eines Imports `WARNING` sein, darf aber für eine erforderliche Constraint-Auswertung zu `ERROR` eskalieren. Severity wird daher anhand des Flows festgelegt, nicht allein anhand des Codes.

## Source Location Modell

### Kanonischer Range

Das bestehende Backend besitzt bereits:

```java
public record SourcePosition(int line, int column, int offset) {}
public record SourceRange(SourcePosition start, SourcePosition end) {}
```

Der API-Vertrag lautet:

```json
{
  "startLine": 1,
  "startColumn": 6,
  "startOffset": 5,
  "endLine": 1,
  "endColumn": 10,
  "endOffset": 9
}
```

| Feld | Basis | Semantik |
|---|---:|---|
| `startLine`/`endLine` | 1-basiert | Zeile im OCL-Quelldokument |
| `startColumn`/`endColumn` | 1-basiert | Spalte im OCL-Quelldokument |
| `startOffset`/`endOffset` | 0-basiert | absolute Position im Text |
| Ende | exklusiv | Range `[startOffset, endOffset)` |

**Verbindliche Entscheidung:** Offsets und Spalten werden als UTF-16-Codeeinheiten gezählt, passend zu Java-`String` und JavaScript-/Monaco-Modellen. Tabs bleiben ein Zeichen; ihre visuelle Breite ist Editorangelegenheit. Zeilenumbrüche müssen beim Eingang normalisiert oder in der Dokumentversion unverändert gehalten werden.

### Erweiterter Source-Bezug

Ein Range allein genügt nicht, wenn mehrere Invarianten oder vollständiger Modelltext bearbeitet werden:

```json
{
  "sourceId": "invariant-max-books",
  "sourceKind": "INVARIANT_EXPRESSION",
  "documentVersion": 17,
  "range": {
    "startLine": 1,
    "startColumn": 6,
    "startOffset": 5,
    "endLine": 1,
    "endColumn": 10,
    "endOffset": 9
  }
}
```

| Feld | Zweck |
|---|---|
| `sourceId` | Invariante, Attributdefinition, Contract oder Modelltext eindeutig identifizieren |
| `sourceKind` | Editor/Properties-Ziel bestimmen |
| `documentVersion` | veraltete Diagnosen nach Textänderung erkennen |
| `range` | Markierung im konkreten Text |

Alternativ zur Version kann zusätzlich ein `contentHash` verwendet werden. Ohne Übereinstimmung darf das Frontend die Diagnose nicht blind an der alten Position markieren.

### Token Index

`tokenIndex` ist für Parser Recovery und interne Tests nützlich, aber kein stabiler UI-Vertrag:

- Whitespace- oder Kommentaränderungen verschieben Tokenindizes.
- Lexer-Versionen können Token anders schneiden.
- Editoren arbeiten mit Textpositionen.

Deshalb bleibt `tokenIndex` optional in technischen Details. Canonical Mapping verwendet Source Range plus Source ID/Version.

### AST Node Location

Jeder AST-Knoten besitzt einen Gesamtrange. Komplexe Knoten benötigen Teilranges:

| AST-Knoten | notwendige Teilranges |
|---|---|
| Property Access | Receiver, Propertyname, Gesamtausdruck |
| Operation Call | Source, Operationsname, jedes Argument |
| Binary Expression | linke Seite, Operator, rechte Seite |
| Iterator | Source, Iteratorname, Variable, Body |
| `let` | Variable, Typ, Initializer, Body |
| `if` | Condition, Then-Zweig, Else-Zweig |
| Constraint | Kontext, Name, Expression |

Der Typechecker meldet den kleinsten ursächlichen Range, nicht immer den Gesamtknoten.

### Expression Range vs. Modelltext

Wird nur der Ausdruck `self.books <= 5` an einen Endpoint gesendet, beginnt sein Range bei Zeile 1/Spalte 1. Liegt derselbe Ausdruck im vollständigen Modelltext, muss der Import-/Modellparser den Range auf das Gesamtdokument beziehen oder eine Basisposition bereitstellen. Beide Koordinatensysteme dürfen nicht vermischt werden.

## Allgemeines Fehlerformat

```json
{
  "id": "diag-9f0d",
  "kind": "OCL_DIAGNOSTIC",
  "phase": "TYPECHECK",
  "code": "UNKNOWN_ATTRIBUTE",
  "severity": "ERROR",
  "message": "Unknown property 'boks' on type User.",
  "userMessage": "Die Klasse 'User' besitzt kein Attribut oder keine Rolle 'boks'.",
  "technicalMessage": "PropertyResolver could not resolve User::boks.",
  "source": {
    "sourceId": "invariant-max-books",
    "sourceKind": "INVARIANT_EXPRESSION",
    "documentVersion": 17,
    "range": {
      "startLine": 1,
      "startColumn": 6,
      "startOffset": 5,
      "endLine": 1,
      "endColumn": 10,
      "endOffset": 9
    }
  },
  "elementType": "INVARIANT",
  "elementId": "invariant-max-books",
  "contextClassId": "class-user",
  "contextObjectId": null,
  "invariantId": "invariant-max-books",
  "relatedElementIds": ["class-user"],
  "expected": ["books", "borrowedBooks"],
  "actual": "boks",
  "details": {
    "receiverType": "User",
    "symbolKind": "PROPERTY"
  },
  "suggestedFix": "Prüfe, ob 'books' gemeint ist."
}
```

### Felder

| Feld | Pflicht | Zweck |
|---|---:|---|
| `id` | ja | stabile Finding-ID innerhalb einer Response |
| `kind` | ja | `OCL_DIAGNOSTIC`, `VALIDATION_ERROR` oder andere Ergebnisart |
| `phase` | ja für OCL | `LEXER`, `PARSER`, `TYPECHECK`, `EVALUATION`, `VALIDATION` |
| `code` | ja | stabiler maschinenlesbarer Fehlercode |
| `severity` | ja | Darstellung und Filterung |
| `message` | ja | kurze neutrale Standardmeldung |
| `userMessage` | ja | lokalisierbare fachliche Meldung |
| `technicalMessage` | optional | Debugging ohne Stacktrace im UI |
| `source` | optional | Source ID, Version und Range |
| Elementreferenzen | optional | Diagramm-/Properties-Mapping |
| `expected`/`actual` | optional | Parser- und Typecheckdetails |
| `details` | optional | codeabhängige strukturierte Werte |
| `suggestedFix` | optional | vorsichtiger Korrekturhinweis |

`suggestedFix` ist kein automatischer Patch. Spätere Quick Fixes benötigen strukturierte Text Edits mit Dokumentversion und Range.

## Lexer- und Parser-Fehler

### Lexer

Der Lexer kennt die exakte Zeichenposition. Er soll:

- ungültiges Segment markieren,
- wenn möglich nach dem Segment fortsetzen,
- kein leeres Range liefern,
- bei nicht geschlossenem String vom öffnenden Quote bis zum Zeilen-/Dateiende markieren.

```ocl
self.name = 'Alice
             ^^^^^^
```

### Parser

Parserdiagnosen benötigen:

- tatsächliches Token,
- erwartete Tokenmenge in nutzerlesbarer Form,
- Insert-/Delete-Hinweis nur bei hoher Sicherheit,
- Recovery Point, damit Folgefehler begrenzt bleiben.

```json
{
  "phase": "PARSER",
  "code": "MISSING_TOKEN",
  "userMessage": "Nach dem Ausdruck fehlt eine schließende Klammer.",
  "expected": [")"],
  "actual": "<EOF>",
  "suggestedFix": "Ergänze ')'."
}
```

Folgefehler, die nur aus einem vorherigen Syntaxfehler entstehen, sollten unterdrückt oder über eine `relatedDiagnosticId` gruppiert werden.

## Typechecker-Fehler

Typechecker-Diagnosen sollen den semantisch relevanten Teil markieren:

| Problem | Range |
|---|---|
| unbekanntes Property | Propertyname |
| falscher Operator | Operator oder Gesamtausdruck mit Operanddetails |
| falsches Argument | konkretes Argument |
| unbekannte Variable | Identifier |
| Iterator-Body nicht Boolean | Body |
| Invariante nicht Boolean | gesamte Invariantenexpression |

Fehlerfortsetzung verwendet einen internen Error Type, damit nach einem unbekannten Attribut nicht zusätzlich beliebig viele irreführende Operatorfehler entstehen.

## Evaluator-Fehler

Evaluatorfehler entstehen an einem typgeprüften AST gegen einen konkreten Evaluation Context. Sie benötigen zusätzlich:

- Kontextobjekt und lesbaren Objektnamen,
- Snapshot-/Projektversion,
- AST-Range,
- optional relevante Iteratorbindung,
- sichere Vorschau beteiligter Werte,
- keine vollständigen internen Stacktraces in der API.

### `null` und `invalid`

| Situation | Behandlung |
|---|---|
| OCL-Ausdruck ergibt regulär `null` | normaler OCL-Wert, solange Kontext ihn zulässt |
| OCL-Ausdruck ergibt `invalid` | gemäß OCL-Propagation weiterverarbeiten |
| Constraint-Endergebnis ist `invalid` | strukturiertes Evaluation Finding, nicht `false` |
| Java Exception aufgrund eines Bugs | `ApiErrorDto`/serverseitiger Fehler, nicht als OCL `invalid` tarnen |
| Snapshot ist inkonsistent | `EVALUATION_ERROR` oder vorgelagerter Snapshot Finding |

`NULL_NAVIGATION` und `INVALID_NAVIGATION` werden nur erzeugt, wenn das OCL-Profil und der konkrete Kontext dies als Diagnose verlangen. Sie dürfen die normative Wertsemantik nicht ersetzen.

## Invariant Violations

Eine Invariantenverletzung liegt nur vor, wenn:

1. der Ausdruck syntaktisch gültig ist,
2. der Ausdruck typgeprüft `Boolean` ergibt,
3. die Evaluation regulär abgeschlossen wird,
4. das Ergebnis für ein Kontextobjekt `false` ist.

`invalid` ist ein Evaluation Error/Finding und keine normale `INVARIANT_VIOLATION`.

```json
{
  "id": "finding-max-books-alice",
  "kind": "VALIDATION_ERROR",
  "code": "INVARIANT_VIOLATION",
  "severity": "ERROR",
  "userMessage": "Das Objekt 'alice : User' verletzt die Invariante 'maxBooks': books ist 6, erlaubt sind höchstens 5.",
  "elementType": "OBJECT",
  "elementId": "object-alice",
  "contextClassId": "class-user",
  "contextObjectId": "object-alice",
  "invariantId": "invariant-max-books",
  "expression": "self.books <= 5",
  "location": {
    "startLine": 1,
    "startColumn": 1,
    "startOffset": 0,
    "endLine": 1,
    "endColumn": 16,
    "endOffset": 15
  },
  "details": {
    "invariantName": "maxBooks",
    "objectName": "alice",
    "attributeName": "books",
    "actualValue": 6,
    "expectedDescription": "kleiner oder gleich 5"
  },
  "suggestedFix": "Reduziere 'books' oder passe die Invariante an."
}
```

Die UI zeigt Namen; IDs bleiben für Navigation und Markierung erhalten.

## Mapping auf `ValidationErrorDto`

Das aktuelle Backend-DTO enthält bereits zentrale Felder:

```text
id, kind, code, severity,
message, userMessage, technicalMessage,
elementType, elementId, relatedElementIds,
contextClassId, contextObjectId, invariantId,
expression, location, targets, details, suggestedFix
```

### Empfohlene Weiterentwicklung

| Bedarf | Änderung |
|---|---|
| OCL-Phase unterscheiden | optionales Feld `phase` |
| Editorversion absichern | `sourceId`, `sourceKind`, `documentVersion` oder `SourceReferenceDto` |
| Primär- und Sekundärpositionen | `location` plus optional `relatedLocations` |
| Lokalisierung | perspektivisch `messageKey` und strukturierte Argumente |
| Quick Fixes | später `fixes[]` mit versionierten Text Edits |

`targets` kann intern weiterhin für generisches Elementmapping genutzt werden. Da es im UI bereits als verwirrend bewertet wurde, darf der technische Feldname nicht sichtbar gerendert werden. Das Frontend zeigt stattdessen fachliche Bereiche wie „Betroffenes Objekt“, „Invariante“ oder „Attribut“.

### Mapping-Regeln

| OCL-Ergebnis | primäres Element | zusätzliche Referenzen |
|---|---|---|
| Syntaxfehler einer Invariante | Invariante | Kontextklasse, Source Range |
| unbekanntes Attribut | Invariante/OCL-Ausdruck | Klasse, optional Attributkandidat |
| Evaluation Error | Kontextobjekt | Invariante, Snapshot |
| Invariantenverletzung | Kontextobjekt | Invariante, Klasse |
| Derived-Fehler | Attribut | Klasse, Kontextobjekt |
| Contractfehler | Operation/Contract | Receiverobjekt, Invocation |

## Zusammenhang mit dem API Error Contract

Der bestehende API Error Contract bleibt gültig:

- technisch ungültiger HTTP-Request: `ApiErrorDto`,
- regulärer OCL-Endpunkt mit Diagnosen: `OclDiagnosticDto`,
- vollständige Constraint-Prüfung: `ValidationResultDto.findings[]`,
- unerwarteter Backendfehler: `ApiErrorDto` mit Correlation ID.

Die gleiche fachliche Ursache kann je nach Endpoint in unterschiedlichen Hüllen erscheinen, Code, Severity, Message und Source Reference müssen aber konsistent bleiben. Ein zentraler Diagnostic Mapper verhindert divergierende Texte und Codes.

## Mapping auf Frontend

### OCL Editor

| Diagnosefeld | Verwendung |
|---|---|
| Source ID/Version | richtigen Editor und Textstand auswählen |
| Range | Marker/Underline setzen |
| Severity | Farbe und Markerart |
| Code | Icon, Filter und Detaildarstellung |
| User Message | Hover und Problems List |
| Suggested Fix | optionaler Hinweis, nicht automatisch anwenden |

Bei veralteter Dokumentversion wird die Diagnose aus dem Editor entfernt oder als veraltet gekennzeichnet. Sie darf nicht auf den inzwischen verschobenen Text zeigen.

### Properties Panel

- Invariantenfehler öffnen Invariant Properties und fokussieren das Ausdrucksfeld.
- Derived-/Init-Fehler öffnen das betroffene Attribute Property.
- Contractfehler öffnen Operation und Contract.
- unbekannte Association Role kann zusätzlich die Association Properties fokussieren.

### Validation Results Panel

Ein Eintrag zeigt kompakt:

1. Severity und fachlichen Fehlertyp,
2. Invarianten-/Attribut-/Operationsname,
3. Objektname, sofern vorhanden,
4. `userMessage`,
5. optional Source Preview und Suggested Fix.

Technische IDs, `targets` und interne Details werden nicht als primärer Fließtext angezeigt.

### Diagramm

Fehlerbadges werden anhand deduplizierter Findings pro Element berechnet. Ein Finding mit mehreren Targets darf nicht mehrfach am selben Objekt gezählt werden. Klick auf das Finding selektiert und fokussiert das primäre Element.

## Beispiele

### Unbekanntes Attribut

```ocl
self.boks <= 5
     ^^^^
```

```json
{
  "phase": "TYPECHECK",
  "code": "UNKNOWN_ATTRIBUTE",
  "severity": "ERROR",
  "userMessage": "Die Klasse 'User' besitzt kein Attribut oder keine Rolle 'boks'.",
  "location": {
    "startLine": 1,
    "startColumn": 6,
    "startOffset": 5,
    "endLine": 1,
    "endColumn": 10,
    "endOffset": 9
  },
  "details": {
    "receiverType": "User",
    "propertyName": "boks"
  },
  "suggestedFix": "Prüfe, ob 'books' gemeint ist."
}
```

### Falscher Operandentyp

```ocl
self.name <= 5
          ^^
```

```json
{
  "phase": "TYPECHECK",
  "code": "TYPE_ERROR",
  "severity": "ERROR",
  "userMessage": "Der Operator '<=' erwartet numerische Operanden, erhielt aber String und Integer.",
  "details": {
    "operator": "<=",
    "leftType": "String",
    "rightType": "Integer",
    "expectedTypes": ["Integer", "Real"]
  },
  "suggestedFix": "Verwende für Texte '=' oder '<>' oder wähle ein numerisches Attribut."
}
```

### Falscher Iterator-Body

```ocl
self.borrowedBooks->forAll(b | b.title)
                                   ^^^^^^^
```

```json
{
  "phase": "TYPECHECK",
  "code": "INVALID_ITERATOR_BODY_TYPE",
  "severity": "ERROR",
  "userMessage": "Der Body von 'forAll' muss Boolean ergeben, liefert aber String.",
  "details": {
    "iterator": "forAll",
    "variableName": "b",
    "expectedType": "Boolean",
    "actualType": "String"
  },
  "suggestedFix": "Formuliere eine Bedingung, z. B. b.title <> ''."
}
```

### Invariantenverletzung

```ocl
self.books <= 5
```

Für `alice : User` mit `books = 6`:

```json
{
  "phase": "VALIDATION",
  "code": "INVARIANT_VIOLATION",
  "severity": "ERROR",
  "userMessage": "'alice : User' verletzt 'maxBooks': books ist 6, erlaubt sind höchstens 5.",
  "contextObjectId": "object-alice",
  "invariantId": "invariant-max-books",
  "details": {
    "objectName": "alice",
    "invariantName": "maxBooks",
    "attributeName": "books",
    "actualValue": 6
  }
}
```

## Testfälle

### Source Location

| ID | Fall | Erwartung |
|---|---|---|
| `OCL-LOC-001` | einzeiliges unbekanntes Property | Range markiert nur Propertyname |
| `OCL-LOC-002` | mehrzeiliger Iterator-Body | Start-/Endzeile korrekt |
| `OCL-LOC-003` | Unicode vor Fehler | UTF-16-Offset stimmt mit Editor überein |
| `OCL-LOC-004` | CRLF-Modelltext | Range bleibt reproduzierbar |
| `OCL-LOC-005` | EOF-Fehler | leerer Endrange an EOF zulässig |
| `OCL-LOC-006` | vollständiger Modelltext | globale statt expression-lokale Position |
| `OCL-LOC-007` | Text nach Request geändert | Diagnoseversion wird als veraltet erkannt |
| `OCL-LOC-008` | AST-Teilrange | Operator/Argument statt Gesamtausdruck markiert |

### Diagnosephasen

| ID | Fall | Erwartung |
|---|---|---|
| `OCL-ERR-001` | ungültiges Zeichen | Lexer-Code und Range |
| `OCL-ERR-002` | fehlende Klammer | Parserdiagnose mit Expected Token |
| `OCL-ERR-003` | `self.boks` | `UNKNOWN_ATTRIBUTE` |
| `OCL-ERR-004` | `self.name <= 5` | Typen in Details |
| `OCL-ERR-005` | unbekannte Association Role | `UNKNOWN_ASSOCIATION_ROLE` |
| `OCL-ERR-006` | falsche Operationsargumente | Argumentrange und Signaturdetails |
| `OCL-ERR-007` | Iterator-Body ist String | `INVALID_ITERATOR_BODY_TYPE` |
| `OCL-ERR-008` | Constraint evaluiert `invalid` | Evaluation Finding, keine Violation |
| `OCL-ERR-009` | Constraint evaluiert `false` | `INVARIANT_VIOLATION` |
| `OCL-ERR-010` | unerwartete Java Exception | API Error und Correlation ID |

### Frontend/API

| ID | Fall | Erwartung |
|---|---|---|
| `OCL-UI-001` | Klick auf OCL Finding | richtiger Editor und Range fokussiert |
| `OCL-UI-002` | Klick auf Violation | Objekt und Invariante fokussiert |
| `OCL-UI-003` | Finding mit Targets | technischer Feldname nicht sichtbar |
| `OCL-UI-004` | mehrere Findings am Objekt | Badge zählt dedupliziert korrekt |
| `OCL-API-001` | Parse Endpoint | `OclDiagnosticDto` konsistent |
| `OCL-API-002` | Validate Endpoint | gleicher Code in `ValidationErrorDto` |
| `OCL-API-003` | veraltete Dokumentversion | kein falscher Marker |

## API-Auswirkungen

### Kurzfristig

- Semantik von `SourceRangeDto` verbindlich dokumentieren.
- OCL-Codes zentral als Enums/Katalog verwalten.
- Mapper von internen Diagnostics zu `OclDiagnosticDto` und `ValidationErrorDto` vereinheitlichen.
- `userMessage` und strukturierte Details ausbauen.
- unerwartete Exceptions in `ApiErrorDto` überführen.

### Mittelfristig

```json
{
  "sourceReference": {
    "sourceId": "invariant-max-books",
    "sourceKind": "INVARIANT_EXPRESSION",
    "documentVersion": 17,
    "range": {}
  },
  "relatedLocations": [],
  "messageKey": "ocl.unknownProperty",
  "messageArguments": {
    "receiverType": "User",
    "propertyName": "boks"
  }
}
```

Diese Ergänzungen müssen versioniert und mit dem Frontend-DTO abgestimmt werden. Source Text selbst wird nicht mehrfach in jeder Diagnose übertragen; `expression` bleibt nur dort, wo es für unabhängige Validation Results nötig ist.

## Bezug zum aktuellen Backend

Bereits vorhanden sind:

- `SourcePosition(line, column, offset)`,
- `SourceRange(start, end)`,
- Source Ranges an AST-Knoten und Tokens,
- `SourceRangeDto` mit Zeile, Spalte und Offset,
- `OclDiagnosticDto`,
- `ValidationErrorDto`,
- Codes wie `SYNTAX_ERROR`, `TYPE_ERROR`, `UNKNOWN_ATTRIBUTE`, `EVALUATION_ERROR` und `INVARIANT_VIOLATION`.

Noch zu verbessern sind insbesondere:

- vollständige Code-Taxonomie,
- getrennte Phase,
- Source ID und Dokumentversion,
- Teilranges komplexer AST-Knoten,
- `UNKNOWN_ASSOCIATION_ROLE` und operationsspezifische Codes,
- OCL-konforme `null`/`invalid`-Diagnostik,
- konsistente nutzerfreundliche Meldungen,
- gemeinsame Mapper und Fehlerfortsetzung.

## Risiken

| Risiko | Auswirkung | Gegenmaßnahme |
|---|---|---|
| Offsets sind nicht spezifiziert | Marker liegen bei Unicode falsch | UTF-16 und End-exklusiv verbindlich |
| Diagnose ohne Source-Version | Marker zeigt nach Edit auf falschen Text | Version/Hash mitsenden |
| jede Phase nutzt eigene Codes | Frontend-Mapping wird instabil | zentraler Codekatalog |
| technischer Text ist User Message | UI unverständlich und nicht lokalisierbar | getrennte Meldungsfelder |
| Folgefehler werden ungefiltert gemeldet | überladenes Results Panel | Error Type, Recovery und Gruppierung |
| `null`, `invalid` und Exception vermischt | falsche OCL-Semantik | explizites Wert- und Fehlerkonzept |
| Suggested Fix ist unzuverlässig | Nutzer übernimmt falsche Änderung | nur bei hoher Sicherheit, sonst weglassen |
| IDs werden prominent angezeigt | geringe Lesbarkeit | Namen anzeigen, IDs nur fürs Mapping |
| Debug Details enthalten Snapshotdaten | Datenschutz-/Response-Risiko | Whitelist und Größenlimit |

## Offene Fragen

| Frage | Auswirkung |
|---|---|
| Wird `SourceReferenceDto` als neues DTO eingeführt oder `SourceRangeDto` erweitert? | API-Versionierung |
| Welche Dokumentversion liefert der OCL Editor bei jedem Request? | Stale Diagnostic Handling |
| Werden Meldungen serverseitig lokalisiert oder über `messageKey` im Frontend? | i18n-Verantwortung |
| Welche OCL-`invalid`-Ergebnisse werden als Findings angezeigt? | Evaluation/Validation |
| Wie viele Folgefehler werden pro Expression maximal geliefert? | UX und Performance |
| Werden strukturierte Quick Fixes unterstützt? | Editorfunktion |
| Wie werden mehrere relevante Source Ranges dargestellt? | Related Locations |
| Welche Details dürfen Evaluation Traces enthalten? | Datenschutz und Debugging |
| Soll `ValidationErrorDto` langfristig neutraler `ValidationFindingDto` heißen? | Warnings/Infos im gleichen Modell |

## Zusammenfassung

Ein belastbares OCL-Fehlermodell benötigt mehr als Code und Text. Jede Diagnose braucht eine eindeutige Phase, stabile Severity, getrennte technische und nutzerfreundliche Meldungen, strukturierte Details sowie eine präzise Source Reference mit Textversion.

Der kanonische Range verwendet 1-basierte Zeilen und Spalten, 0-basierte UTF-16-Offsets und ein exklusives Ende. Tokenindizes bleiben intern. Jeder AST-Knoten und relevante Teilbereich trägt Source Ranges, damit Lexer, Parser, Typechecker und Evaluator die kleinste ursächliche Stelle markieren können.

`OclDiagnosticDto`, `ValidationErrorDto` und `ApiErrorDto` bleiben getrennte Hüllen mit konsistentem Codekatalog. Das Frontend zeigt fachliche Namen und gezielte Meldungen, nutzt IDs nur für Navigation und rendert technische Felder wie `targets` nicht direkt. OCL 2.4 bestimmt die Semantik von `null`, `invalid`, Typfehlern und Constraint-Ergebnissen; die Produktarchitektur macht diese Semantik editor- und validationstauglich.
