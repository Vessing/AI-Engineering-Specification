# Error Contract

## Zweck dieser Datei

Diese Datei definiert den Fehlervertrag zwischen Frontend und Backend fuer das neue UML/OCL-Websystem.

Der Vertrag beschreibt:

- wie technische API-Fehler uebertragen werden,
- wie fachliche Validierungsfehler uebertragen werden,
- wie OCL-Syntax-, Typecheck- und Evaluation-Fehler abgebildet werden,
- wie Invariantverletzungen und Multiplicity Violations strukturiert werden,
- wie Fehler auf UI-Elemente im Class Diagram, Object Diagram, OCL Editor, Properties Panel und Validation Results Panel gemappt werden.

Der zentrale UI-Referenzpunkt ist der Screenshot `07-object-diagram-validation-error.png`, der einen fehlgeschlagenen Constraint Check mit markiertem Objekt und Validation Results zeigt.

![Object Diagram Validation Error](../assets/screenshots/07-object-diagram-validation-error.png)

## Fehlerarten

| Fehlerart | Beschreibung | Entsteht typischerweise in | API-Transport | UI-Darstellung |
|---|---|---|---|---|
| `API_ERROR` | Technischer oder protokollbezogener Fehler ausserhalb fachlicher Validierung | Controller, API Client, Persistence | `ApiErrorDto` mit HTTP-Status | Toast, Error Banner, Dialog, Console |
| `VALIDATION_ERROR` | Sammelkategorie fuer fachliche Validierungsfehler | Validation Service | `ValidationResultDto.errors[]` | Validation Results Panel, Diagramm-Markierung |
| `SYNTAX_ERROR` | OCL-Ausdruck kann nicht geparst werden | OCL Parser | `OclDiagnosticDto` oder `ValidationErrorDto` | OCL Editor Marker, Invariant Properties |
| `UNSUPPORTED_SYNTAX` | Modelltext ist lesbar, nutzt aber Konstrukte ausserhalb des MVP-Subsets | Model Text Parser / Import Service | `OclDiagnosticDto`, `ValidationErrorDto` oder Apply-Diagnostics | OCL Editor Marker, Warning im Apply-Ergebnis |
| `TYPE_ERROR` | OCL-Ausdruck oder Modellwert verletzt Typregeln | OCL Typechecker, Snapshot Validator | `OclDiagnosticDto` oder `ValidationErrorDto` | OCL Editor, Properties Panel, Validation Results |
| `UNKNOWN_CLASS` | Referenzierte Klasse existiert nicht | UML/Snapshot Validation | `ValidationErrorDto` | Explorer, Properties Panel |
| `UNKNOWN_ATTRIBUTE` | Referenziertes Attribut existiert nicht | OCL Typechecker, Slot Validation | `ValidationErrorDto` | OCL Editor, Object Properties |
| `INVALID_SLOT_VALUE` | Objekt-Slot passt nicht zum Attributtyp | Snapshot Validation | `ValidationErrorDto` | Object Properties, Object Node |
| `INVALID_LINK` | Objektlink passt nicht zur Association | Link Validation | `ValidationErrorDto` | Object Edge, Properties Panel |
| `ASSOCIATION_CLASS_IDENTITY_VIOLATION` | Link und Association-Class-Objekt besitzen keine eindeutige kompatible Eins-zu-eins-Identität | Link Validation / Object Model Service | `ValidationErrorDto` oder `ApiErrorDto` | Object Edge, Linkobjekt, Properties Panel |
| `COMPOSITE_OWNERSHIP_VIOLATION` | Ein Composite Part besitzt mehrere Whole-Objekte | Link Validation / Object Model Service | `ValidationErrorDto` oder `ApiErrorDto` | Object Diagram, Validation Results |
| `COMPOSITION_CYCLE` | Der gerichtete Composition-Graph enthält einen Zyklus | Link Validation / Object Model Service | `ValidationErrorDto` oder `ApiErrorDto` | Object Diagram, Validation Results |
| `STALE_PROJECT_REVISION` | Invocation basiert nicht auf der aktuellen Projektrevision | Operation Invocation Service | `ApiErrorDto` | Invocation Dialog, Reload-Aktion |
| `UNKNOWN_RECEIVER` | Receiver-Objekt existiert nicht | Operation Invocation Service | `ApiErrorDto` | Receiver-Auswahl |
| `OPERATION_RESOLUTION_ERROR` | Operation ist unbekannt, inkompatibel oder Dispatch ist mehrdeutig | Operation Resolver | `ApiErrorDto` | Operation Details, Invocation Dialog |
| `ABSTRACT_OPERATION` | Eine abstrakte Operation wurde direkt aufgerufen | Operation Invocation Service | `ApiErrorDto` | deaktivierte Invoke-Aktion |
| `OPERATION_IMPLEMENTATION_MISSING` | Für die aufgelöste Operation ist noch keine ausführbare Backend-Semantik registriert | Operation Registry | `ApiErrorDto` | Invocation Dialog |
| `MISSING_ARGUMENT` / `UNKNOWN_ARGUMENT` / `DUPLICATE_ARGUMENT` | Parameterbindung ist unvollständig oder widersprüchlich | Operation Invocation Service | `ApiErrorDto` | Argumentfeld |
| `ARGUMENT_TYPE_MISMATCH` | Argumenttyp ist nicht mit dem Parametertyp kompatibel | Operation Invocation Service | `ApiErrorDto` | Argumentfeld |
| `OUT_PARAMETER_HAS_INPUT` | Für einen reinen `out`-Parameter wurde ein Eingabewert gesendet | Operation Invocation Service | `ApiErrorDto` | Argumentfeld |
| `PRECONDITION_VIOLATION` | Ein aktiver Precondition-Contract ist falsch; die Operation wurde nicht ausgeführt | Operation Contract Runtime | `OperationContractResultDto` | Contract und Invocation Dialog |
| `POSTCONDITION_VIOLATION` | Ein aktiver Postcondition-Contract ist falsch; der Candidate State wurde verworfen | Operation Contract Runtime | `OperationContractResultDto` | Contract und Before-/After-Ansicht |
| `CONTRACT_TYPE_ERROR` / `CONTRACT_NOT_BOOLEAN` | Contract ist im Operationskontext nicht typkorrekt oder nicht Boolean | Operation Contract Runtime | `OperationContractResultDto` | OCL-Editor mit Source Range |
| `CONTRACT_EVALUATION_ERROR` / `CONTRACT_CONTEXT_ERROR` | Contract kann mit Zustand oder Result-Slot nicht ausgewertet werden | Operation Contract Runtime | `OperationContractResultDto` | Invocation Result und OCL-Editor |
| `MULTIPLICITY_VIOLATION` | Linkanzahl verletzt eine Multiplizitaet | Multiplicity Validator | `ValidationErrorDto` | Object Node/Edge, Validation Results |
| `INVARIANT_VIOLATION` | OCL-Invariante ergibt fuer ein Kontextobjekt `false` | OCL Evaluator | `ValidationErrorDto` | Object Node, Validation Results |
| `EVALUATION_ERROR` | OCL-Auswertung bricht fachlich/technisch ab | OCL Evaluator | `ValidationErrorDto` | Validation Results, OCL Details |
| `AMBIGUOUS_REFLEXIVE_ROLE` | Reflexive Association verwendet an beiden Ends denselben Rollennamen | UML Model Service | `ApiErrorDto` mit Association-ID | Association Properties |
| `UNKNOWN_SUBSETS_END` / `UNKNOWN_REDEFINES_END` | Referenziertes Association End existiert nicht | UML Model Service | `ApiErrorDto` mit End-IDs | Association End Properties |
| `INCOMPATIBLE_SUBSETS_END` / `INCOMPATIBLE_REDEFINES_END` | Endtyp oder Multiplizität ist nicht konform | UML Model Service | `ApiErrorDto` mit End-IDs | Association End Properties |
| `SUBSETS_CYCLE` / `REDEFINES_CYCLE` | Association-End-Referenzen bilden einen Zyklus | UML Model Service | `ApiErrorDto` mit End-ID | Association End Properties |

## Severity-Modell

| Severity | Bedeutung | Frontend-Verhalten |
|---|---|---|
| `ERROR` | Zustand ist fachlich oder technisch ungueltig. | Rot markieren, Error Count erhoehen, Validation Status `INVALID` oder `ERROR`. |
| `WARNING` | Zustand ist nutzbar, aber auffaellig oder unvollstaendig. | Gelb markieren, Warning Count erhoehen, kein harter Blocker. |
| `INFO` | Hinweis ohne Fehlercharakter. | Im Panel oder Console anzeigen, keine Diagramm-Fehlerdarstellung. |

MVP-Entscheidung: Die Kernvalidierung nutzt primaer `ERROR`. `WARNING` und `INFO` sollten im Format vorgesehen werden, muessen aber im MVP nur einfach dargestellt werden.

## Allgemeines Fehlerformat

Fachliche Fehler und OCL-Diagnostics sollen ein gemeinsames Grundschema verwenden. So kann das Frontend unabhaengig vom Ursprung dieselben UI-Mechanismen fuer Listen, Marker und Fokus verwenden.

| Feld | Typ | Pflicht | Beschreibung |
|---|---|---:|---|
| `id` | `string` | Ja | Stabile Fehler-ID innerhalb eines Ergebnisses. |
| `kind` | `string` | Ja | Grobe Fehlerart, z. B. `VALIDATION_ERROR`, `API_ERROR`. |
| `code` | `string` | Ja | Maschinenlesbarer Fehlercode. |
| `severity` | `ERROR \| WARNING \| INFO` | Ja | Schweregrad. |
| `message` | `string` | Ja | Technisch praezise Kurzmeldung. |
| `userMessage` | `string` | Nein | UI-taugliche Meldung fuer Nutzer. |
| `technicalMessage` | `string` | Nein | Detail fuer Debugging, Logs oder Entwickleransicht. |
| `elementType` | `string` | Nein | Primaer betroffenes Element, z. B. `OBJECT`. |
| `elementId` | `string` | Nein | Primaere Element-ID. |
| `relatedElementIds` | `string[]` | Nein | Weitere betroffene IDs. |
| `targets` | `ElementTargetDto[]` | Ja fuer Validation | Strukturierte Mapping-Ziele fuer UI-Fokus und Markierung. |
| `contextClassId` | `string` | Nein | Kontextklasse bei OCL/Invarianten. |
| `contextObjectId` | `string` | Nein | Kontextobjekt bei OCL-Evaluation. |
| `invariantId` | `string` | Nein | Betroffene Invariante. |
| `expression` | `string` | Nein | Betroffener OCL-Ausdruck. |
| `location` | `OclLocationDto` | Nein | Position im OCL-Text. |
| `details` | `object` | Nein | Maschinenlesbare Zusatzdaten. |
| `suggestedFix` | `string` | Nein | Optionaler Hinweis auf moegliche Korrektur. |

Beispielhafte Zielreferenz:

```json
{
  "elementType": "OBJECT",
  "elementId": "obj-alice",
  "path": "objectModel.objects[obj-alice]"
}
```

## API Error

`API_ERROR` beschreibt Fehler, bei denen der Request selbst nicht erfolgreich verarbeitet werden konnte. Beispiele sind `PROJECT_NOT_FOUND`, `INVALID_PROJECT_FORMAT`, `UNAUTHORIZED`, `CONFLICT` oder unerwartete Serverfehler.

API-Fehler werden nicht als `ValidationResultDto` geliefert, sondern als `ApiErrorDto` mit passendem HTTP-Status.

| HTTP-Status | Typischer Code | Beispiel |
|---:|---|---|
| `400` | `BAD_REQUEST` | Request DTO ist syntaktisch ungueltig. |
| `404` | `PROJECT_NOT_FOUND` | Projekt-ID existiert nicht. |
| `409` | `PROJECT_VERSION_CONFLICT` | Speichern kollidiert mit neuerer Version. |
| `422` | `INVALID_PROJECT_FORMAT` | Importiertes JSON ist formal ungueltig. |
| `500` | `INTERNAL_SERVER_ERROR` | Unerwarteter Backendfehler. |

Beispiel:

```json
{
  "kind": "API_ERROR",
  "code": "PROJECT_NOT_FOUND",
  "severity": "ERROR",
  "message": "Project project-library-demo was not found.",
  "userMessage": "Das Projekt konnte nicht gefunden werden.",
  "technicalMessage": "No persisted project exists for id project-library-demo.",
  "path": "/api/v1/projects/project-library-demo",
  "timestamp": "2026-07-15T10:20:00Z",
  "requestId": "req-20260715-001",
  "details": {
    "projectId": "project-library-demo"
  }
}
```

Frontend-Verhalten:

- Kein Diagramm-Highlighting, wenn keine fachlichen Targets vorhanden sind.
- Anzeige als Toast, Banner oder Dialog.
- Eintrag in der Console mit `requestId`.
- Bei `409` kann ein Konflikt-Dialog mit Reload/Overwrite-Option angezeigt werden.

## Validation Error

`VALIDATION_ERROR` ist der Container fuer fachliche Fehler aus `Check Constraints`. Der HTTP-Status bleibt im Normalfall `200`, auch wenn der Modellzustand ungueltig ist. Der fachliche Status steht in `ValidationResultDto.status`.

```json
{
  "status": "INVALID",
  "summary": {
    "errorCount": 1,
    "warningCount": 0,
    "infoCount": 0
  },
  "errors": [
    {
      "id": "err-maxbooks-alice",
      "kind": "VALIDATION_ERROR",
      "code": "INVARIANT_VIOLATION",
      "severity": "ERROR",
      "message": "Invariant maxBooks is violated for object alice.",
      "userMessage": "alice verletzt die Invariante maxBooks.",
      "elementType": "OBJECT",
      "elementId": "obj-alice",
      "relatedElementIds": ["inv-user-maxBooks", "class-user"],
      "contextClassId": "class-user",
      "contextObjectId": "obj-alice",
      "invariantId": "inv-user-maxBooks",
      "expression": "self.books <= 5",
      "targets": [
        { "elementType": "OBJECT", "elementId": "obj-alice" },
        { "elementType": "INVARIANT", "elementId": "inv-user-maxBooks" }
      ],
      "details": {
        "actualValue": false
      }
    }
  ]
}
```

Frontend-Verhalten:

- `ValidationResultsPanel` zeigt Error Count und Fehlerliste.
- `ObjectDiagramCanvas` markiert alle Targets mit `elementType = OBJECT` oder `OBJECT_LINK`.
- `ExplorerSidebar` kann betroffene Klassen, Invarianten oder Objekte mit Indikator versehen.
- Klick auf den Fehler fokussiert das primaere `elementId`.

## OCL Error

OCL-Fehler treten in drei Situationen auf:

1. Beim direkten Parse/Typecheck im OCL Editor.
2. Beim Speichern einer Invariante.
3. Beim vollstaendigen `Check Constraints`.

| OCL-Fehler | Code | Primaeres Target | UI-Ort |
|---|---|---|---|
| Syntaxfehler | `SYNTAX_ERROR` | `INVARIANT` oder `OCL_EXPRESSION` | OCL Editor, Invariant Modal |
| Nicht unterstützte Modelltext-Syntax | `UNSUPPORTED_SYNTAX` | `OCL_EXPRESSION` oder `PROJECT` mit `sourceRange` | OCL Editor |
| Typfehler | `TYPE_ERROR`, `UNKNOWN_ATTRIBUTE`, `UNKNOWN_CLASS` | `INVARIANT`, `CLASS`, `ATTRIBUTE` | OCL Editor, Class Diagram |
| Evaluationsfehler | `EVALUATION_ERROR` | `INVARIANT`, `OBJECT` | Validation Results |

Beispiel fuer OCL Diagnostic:

```json
{
  "valid": false,
  "diagnostics": [
    {
      "id": "ocl-diag-001",
      "kind": "VALIDATION_ERROR",
      "code": "SYNTAX_ERROR",
      "severity": "ERROR",
      "message": "Expected expression after operator <=.",
      "userMessage": "Der OCL-Ausdruck ist unvollstaendig.",
      "elementType": "INVARIANT",
      "elementId": "inv-user-maxBooks",
      "invariantId": "inv-user-maxBooks",
      "expression": "self.books <=",
      "location": {
        "startLine": 1,
        "startColumn": 13,
        "endLine": 1,
        "endColumn": 13
      },
      "targets": [
        {
          "elementType": "OCL_EXPRESSION",
          "elementId": "inv-user-maxBooks",
          "path": "umlModel.invariants[inv-user-maxBooks].expression"
        }
      ],
      "suggestedFix": "Ergaenze einen rechten Vergleichswert, z. B. 5."
    }
  ]
}
```

## Invariant Violation

Eine `INVARIANT_VIOLATION` liegt vor, wenn eine syntaktisch korrekte und typgepruefte Invariante fuer ein Kontextobjekt den Wert `false` ergibt.

| Feld | Erwarteter Inhalt |
|---|---|
| `code` | `INVARIANT_VIOLATION` |
| `contextClassId` | Klasse der Invariante |
| `contextObjectId` | Objekt, fuer das die Invariante fehlschlaegt |
| `invariantId` | verletzte Invariante |
| `expression` | OCL-Ausdruck |
| `targets` | mindestens `OBJECT` und `INVARIANT` |
| `details.actualValue` | `false` |

Beispiel:

```json
{
  "id": "err-maxbooks-alice",
  "kind": "VALIDATION_ERROR",
  "code": "INVARIANT_VIOLATION",
  "severity": "ERROR",
  "message": "Invariant maxBooks evaluated to false for alice.",
  "userMessage": "alice verletzt die Invariante maxBooks: self.books <= 5.",
  "technicalMessage": "Evaluation result for inv-user-maxBooks on obj-alice was Boolean(false).",
  "elementType": "OBJECT",
  "elementId": "obj-alice",
  "relatedElementIds": ["inv-user-maxBooks", "class-user"],
  "contextClassId": "class-user",
  "contextObjectId": "obj-alice",
  "invariantId": "inv-user-maxBooks",
  "expression": "self.books <= 5",
  "targets": [
    { "elementType": "OBJECT", "elementId": "obj-alice" },
    { "elementType": "INVARIANT", "elementId": "inv-user-maxBooks" },
    { "elementType": "CLASS", "elementId": "class-user" }
  ],
  "details": {
    "actualValue": false,
    "contextObjectName": "alice",
    "contextClassName": "User",
    "invariantName": "maxBooks"
  },
  "suggestedFix": "Reduziere den Wert von books oder passe die Invariante an."
}
```

## Multiplicity Violation

Eine `MULTIPLICITY_VIOLATION` liegt vor, wenn die Anzahl der Objektlinks an einem Association-Ende nicht zur definierten Multiplizitaet passt.

| Feld | Erwarteter Inhalt |
|---|---|
| `code` | `MULTIPLICITY_VIOLATION` |
| `elementType` | `OBJECT`, `OBJECT_LINK` oder `ASSOCIATION_END` |
| `elementId` | Primaer betroffenes Objekt oder Link |
| `relatedElementIds` | Association, Association-Ende, beteiligte Links |
| `details.expectedMultiplicity` | z. B. `1..*` |
| `details.actualCount` | Anzahl gefundener Links |

Beispiel:

```json
{
  "id": "err-borrows-min-user",
  "kind": "VALIDATION_ERROR",
  "code": "MULTIPLICITY_VIOLATION",
  "severity": "ERROR",
  "message": "Multiplicity 1..* is violated at role borrowedBooks.",
  "userMessage": "alice hat zu wenige Links fuer die Rolle borrowedBooks.",
  "elementType": "OBJECT",
  "elementId": "obj-alice",
  "relatedElementIds": [
    "assoc-borrows",
    "assocend-borrows-book"
  ],
  "targets": [
    { "elementType": "OBJECT", "elementId": "obj-alice" },
    { "elementType": "ASSOCIATION", "elementId": "assoc-borrows" },
    { "elementType": "ASSOCIATION_END", "elementId": "assocend-borrows-book" }
  ],
  "details": {
    "roleName": "borrowedBooks",
    "expectedMultiplicity": "1..*",
    "actualCount": 0
  },
  "suggestedFix": "Erstelle einen passenden Objektlink oder passe die Multiplizitaet an."
}
```

## Element Mapping

Das Mapping ist entscheidend fuer den Screenshot `07-object-diagram-validation-error.png`: Das Backend liefert fachliche IDs, das Frontend findet damit Nodes, Edges, Properties und Editorstellen.

| Backend Target | Frontend-Ziel | Darstellung |
|---|---|---|
| `PROJECT` | Projektansicht, Dashboard, Console | Banner oder Console-Eintrag |
| `CLASS` | Class Node, Explorer Class Item | Markierung im Klassendiagramm |
| `ATTRIBUTE` | Class Node Attribute Row, Properties Field | Feldfehler oder Zeilenmarkierung |
| `OPERATION` | Class Node Operation Row | Zeilenmarkierung |
| `ASSOCIATION` | Class Diagram Edge | Edge-Farbe, Properties Panel |
| `ASSOCIATION_END` | Edge Label oder Association Properties | Label-/Feldmarkierung |
| `INVARIANT` | Invariant Badge, OCL Editor, Explorer | Badge, Editor-Panel, Properties |
| `OBJECT` | Object Node | Roter Rahmen, Fehler-Badge |
| `SLOT` | Object Node Slot Row, Object Properties | Feldfehler |
| `OBJECT_LINK` | Object Diagram Edge | Edge-Farbe, Fehler-Badge |
| `OCL_EXPRESSION` | OCL Editor Textbereich | Inline Marker, Squiggle, Error Message |

Mapping-Regeln:

1. `elementId` ist das primaere Fokusziel.
2. `targets[0]` ist das bevorzugte Fokusziel, wenn `elementId` fehlt.
3. `relatedElementIds` duerfen fuer Zusatzmarkierungen genutzt werden, aber nicht als primaere Navigation.
4. Wenn ein Target in der aktiven View nicht sichtbar ist, wechselt das Frontend zur passenden View oder zeigt einen Hinweis.
5. Layoutdaten werden nur fuer Fokus und Zentrierung verwendet, nicht fuer fachliche Interpretation.

## OCL Location Mapping

OCL-Fehler benoetigen eine Textposition, damit das Frontend den betroffenen Ausdruck markieren kann.

| Feld | Bedeutung |
|---|---|
| `startLine` | 1-basierte Startzeile |
| `startColumn` | 1-basierte Startspalte |
| `endLine` | 1-basierte Endzeile |
| `endColumn` | 1-basierte Endspalte |
| `offsetStart` | Optionaler 0-basierter Zeichenoffset |
| `offsetEnd` | Optionaler 0-basierter Zeichenoffset |

Beispiel:

```json
{
  "location": {
    "startLine": 1,
    "startColumn": 6,
    "endLine": 1,
    "endColumn": 13,
    "offsetStart": 5,
    "offsetEnd": 12
  }
}
```

Frontend-Verhalten:

- Im MVP reichen Zeile und Spalte fuer einfache Textfelder.
- Bei spaeterem Editor mit Syntax Highlighting sollten Offsets ergaenzt werden.
- Wenn keine Location vorhanden ist, wird der gesamte Ausdruck markiert.

## Frontend-Darstellung

| UI-Bereich | Fehlerquelle | Darstellung |
|---|---|---|
| Object Diagram | `OBJECT`, `OBJECT_LINK`, `SLOT` Targets | Roter Rahmen, Badge, Edge-Highlight |
| Class Diagram | `CLASS`, `ATTRIBUTE`, `ASSOCIATION`, `INVARIANT` Targets | Node-/Edge-Markierung, Invariant Badge |
| Properties Panel | Primaeres selektiertes Target | Feldnahe Fehlermeldung |
| Validation Results Panel | Alle Validation Errors | Liste, Count, Detailbereich |
| OCL Editor | `OCL_EXPRESSION`, `INVARIANT` mit Location | Inline Marker, Fehlermeldung |
| Console | API-Fehler, Validierungsstart/-ende, technische Details | Logeintrag mit Severity |
| Dashboard | Projekt-/Importfehler | Banner, Toast, Importdialog-Fehler |

### Diagramm-Markierung

MVP-Verhalten:

- `ERROR` auf Objekt: roter Rahmen und Fehler-Badge.
- `ERROR` auf Link: rote oder hervorgehobene Edge.
- `WARNING`: gelber Indikator.
- `INFO`: keine starke Diagramm-Markierung, optional Info-Badge.

### Validation Results Panel

Das Panel zeigt:

- Gesamtstatus: `VALID`, `INVALID` oder `ERROR`,
- Anzahl Fehler/Warnungen/Hinweise,
- Liste der Meldungen,
- primaere betroffene Elemente,
- Kontextdaten, z. B. Invariante, Objekt, Ausdruck,
- optional `suggestedFix`.

### OCL Editor

Der OCL Editor zeigt:

- Syntaxfehler direkt am Ausdruck,
- Type Errors am betroffenen Teilbereich,
- betroffene Kontextklasse,
- Backend-Diagnostic als fachliche Quelle.

Das Frontend darf einfache Vorvalidierung machen, z. B. leeres Pflichtfeld. Die fachliche OCL-Wahrheit liegt beim Backend.

### Klick auf Fehler

Beim Klick auf einen Fehler:

1. Frontend liest `elementId` oder `targets[0]`.
2. Frontend bestimmt die passende View.
3. Falls noetig, wechselt es in Class Diagram, Object Diagram oder OCL Editor.
4. Es fokussiert und selektiert das Element.
5. Es oeffnet das passende Properties Panel.
6. Bei OCL-Location wird der betroffene Textbereich markiert.

### Console Logs

Console Logs sind UI-Protokolle, keine fachliche Wahrheit.

| Ereignis | Console-Beispiel |
|---|---|
| Validation gestartet | `Check Constraints started for project Library.` |
| Validation erfolgreich | `Validation completed: valid.` |
| Validation mit Fehlern | `Validation completed: 1 error.` |
| API Error | `API_ERROR PROJECT_NOT_FOUND req-20260715-001.` |
| OCL Parse Error | `SYNTAX_ERROR in invariant maxBooks.` |
| Model Text Warning | `UNSUPPORTED_SYNTAX: import statements are not supported in the MVP.` |

## Beispiel: maxBooks-Verletzung

Dieses Beispiel entspricht dem im Screenshot sichtbaren Muster: Ein Objekt im Object Diagram wird als fehlerhaft markiert, und das Validation Results Panel zeigt die Ursache.

```json
{
  "status": "INVALID",
  "summary": {
    "errorCount": 1,
    "warningCount": 0,
    "infoCount": 0,
    "checkedInvariantCount": 1,
    "checkedObjectCount": 2
  },
  "errors": [
    {
      "id": "err-maxbooks-alice",
      "kind": "VALIDATION_ERROR",
      "code": "INVARIANT_VIOLATION",
      "severity": "ERROR",
      "message": "Invariant maxBooks evaluated to false for alice.",
      "userMessage": "alice verletzt die Invariante maxBooks: self.books <= 5.",
      "technicalMessage": "Slot attr-user-books has value 6; comparison 6 <= 5 evaluated to false.",
      "elementType": "OBJECT",
      "elementId": "obj-alice",
      "relatedElementIds": [
        "slot-alice-books",
        "inv-user-maxBooks",
        "class-user"
      ],
      "contextClassId": "class-user",
      "contextObjectId": "obj-alice",
      "invariantId": "inv-user-maxBooks",
      "expression": "self.books <= 5",
      "targets": [
        { "elementType": "OBJECT", "elementId": "obj-alice" },
        { "elementType": "SLOT", "elementId": "slot-alice-books" },
        { "elementType": "INVARIANT", "elementId": "inv-user-maxBooks" }
      ],
      "details": {
        "leftValue": 6,
        "operator": "<=",
        "rightValue": 5,
        "actualValue": false
      },
      "suggestedFix": "Setze books auf einen Wert kleiner oder gleich 5."
    }
  ]
}
```

Erwartetes Frontend-Verhalten:

- `obj-alice` erhaelt roten Rahmen.
- `obj-alice` zeigt einen Fehler-Badge.
- Validation Results Panel zeigt `maxBooks` und `alice`.
- Klick auf den Fehler fokussiert `alice : User`.
- Properties Panel kann den Slot `books` hervorheben.

## Beispiel: Type Error

```json
{
  "valid": false,
  "diagnostics": [
    {
      "id": "err-type-name-leq",
      "kind": "VALIDATION_ERROR",
      "code": "TYPE_ERROR",
      "severity": "ERROR",
      "message": "Operator <= cannot be applied to String and Integer.",
      "userMessage": "Der Vergleich ist ungueltig: name ist ein String, 5 ist eine Zahl.",
      "technicalMessage": "BinaryExpression '<=' expected numeric operands but got String and Integer.",
      "elementType": "INVARIANT",
      "elementId": "inv-user-invalidName",
      "contextClassId": "class-user",
      "invariantId": "inv-user-invalidName",
      "expression": "self.name <= 5",
      "location": {
        "startLine": 1,
        "startColumn": 11,
        "endLine": 1,
        "endColumn": 13
      },
      "targets": [
        {
          "elementType": "OCL_EXPRESSION",
          "elementId": "inv-user-invalidName",
          "path": "umlModel.invariants[inv-user-invalidName].expression"
        },
        {
          "elementType": "ATTRIBUTE",
          "elementId": "attr-user-name"
        }
      ],
      "details": {
        "leftType": "String",
        "operator": "<=",
        "rightType": "Integer"
      },
      "suggestedFix": "Nutze einen Stringvergleich mit = oder <> oder vergleiche ein numerisches Attribut."
    }
  ]
}
```

## Beispiel: Syntax Error

```json
{
  "valid": false,
  "diagnostics": [
    {
      "id": "err-syntax-incomplete-comparison",
      "kind": "VALIDATION_ERROR",
      "code": "SYNTAX_ERROR",
      "severity": "ERROR",
      "message": "Unexpected end of expression.",
      "userMessage": "Der OCL-Ausdruck ist unvollstaendig.",
      "technicalMessage": "Parser expected expression after '<=' at line 1, column 13.",
      "elementType": "INVARIANT",
      "elementId": "inv-user-maxBooks",
      "contextClassId": "class-user",
      "invariantId": "inv-user-maxBooks",
      "expression": "self.books <=",
      "location": {
        "startLine": 1,
        "startColumn": 13,
        "endLine": 1,
        "endColumn": 13
      },
      "targets": [
        {
          "elementType": "OCL_EXPRESSION",
          "elementId": "inv-user-maxBooks",
          "path": "umlModel.invariants[inv-user-maxBooks].expression"
        }
      ],
      "suggestedFix": "Ergaenze einen Vergleichswert, z. B. self.books <= 5."
    }
  ]
}
```

## Screenshot-Bezug

| Screenshot | Relevante Fehlerdarstellung | Ableitung fuer Error Contract |
|---|---|---|
| `07-object-diagram-validation-error.png` | Object Diagram mit fehlerhaftem Objekt, Fehler-Badge und Validation Results Panel | `ValidationErrorDto` braucht `contextObjectId`, `invariantId`, `targets`, `elementId` und maschinenlesbaren `code`. |

Aus dem Screenshot abgeleitete MVP-Anforderungen:

- Fehlerhafte Objekte muessen anhand ihrer `objectId` markiert werden koennen.
- Validation Results muessen lesbare Meldungen und maschinenlesbare Codes enthalten.
- Ein Fehler muss mindestens ein primaeres Ziel haben.
- Fehlerliste und Diagramm muessen bidirektional verbunden sein.
- OCL-Invariantverletzungen muessen sowohl auf das Objekt als auch auf die Invariante verweisen.

## MVP-Anforderungen

| ID | Anforderung | Prioritaet |
|---|---|---|
| `ERR-MVP-001` | Backend liefert technische API-Fehler als `ApiErrorDto`. | MVP |
| `ERR-MVP-002` | Backend liefert fachliche Validierungsfehler als `ValidationResultDto`. | MVP |
| `ERR-MVP-003` | Jeder Validierungsfehler hat `code`, `severity`, `message` und mindestens ein Target. | MVP |
| `ERR-MVP-004` | `INVARIANT_VIOLATION` verweist auf `contextObjectId`, `contextClassId` und `invariantId`. | MVP |
| `ERR-MVP-005` | `SYNTAX_ERROR` und `TYPE_ERROR` koennen eine OCL-Location enthalten. | MVP |
| `ERR-MVP-005A` | `UNSUPPORTED_SYNTAX` enthaelt Source Range im vollständigen Modelltext. | MVP |
| `ERR-MVP-006` | Frontend markiert fehlerhafte Objekte im Object Diagram. | MVP |
| `ERR-MVP-007` | Validation Results Panel zeigt Fehlerliste und Details. | MVP |
| `ERR-MVP-008` | Klick auf Fehler fokussiert das primaere Zielelement. | MVP |
| `ERR-MVP-009` | Console protokolliert Validierungs- und API-Fehler. | MVP |

## Post-MVP-Erweiterungen

| Erweiterung | Beschreibung |
|---|---|
| Quick Fixes | `suggestedFix` wird zu strukturierten Fix-Actions erweitert. |
| Multi-Snapshot-Fehler | Fehler koennen auf bestimmte Snapshots oder Vergleichszustaende verweisen. |
| Editor-Integration | OCL Location nutzt Offsets, Tokenbereiche und Syntax Highlighting. |
| Fehlerhistorie | Validation Results koennen versioniert oder verglichen werden. |
| Kollaboration | Fehler koennen Nutzeraktionen oder Bearbeitern zugeordnet werden. |
| Lokalisierung | `userMessage` wird ueber Message Keys statt festem Text geliefert. |
| Problem Details | `ApiErrorDto` kann an RFC 9457 angelehnt werden. |

## B22-Härtungsdiagnosen

| Code | Phase | Bedeutung |
|---|---|---|
| `INVALID_OCL_INPUT` | Parser | Der direkte Parseraufruf erhielt keinen Ausdruck. |
| `SOURCE_LIMIT_EXCEEDED` | Parser | Der OCL-Quelltext überschreitet `maxSourceCharacters`. |
| `DIAGNOSTIC_LIMIT_EXCEEDED` | Parser | Weitere unabhängige Lexer-/Parserbefunde wurden nach `maxDiagnostics` begrenzt. |
| `RESULT_LIMIT_EXCEEDED` | Evaluation | Eine Collection-Ergebnismenge überschreitet `maxResultElements`. |

Alle Codes werden über den bestehenden `OclDiagnosticDto`-Vertrag mit Phase,
Severity und Source Range transportiert. Sie sind fachliche OCL-Diagnostics und
keine unstrukturierten HTTP-500-Fehler.

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Soll `ValidationErrorDto` langfristig in `ValidationErrorDto` umbenannt werden? | Sinnvoll, wenn Warnings und Infos gleichwertig behandelt werden. |
| Werden `message` und `userMessage` vom Backend lokalisiert oder im Frontend uebersetzt? | Relevant fuer Internationalisierung. |
| Soll das Backend strukturierte Quick Fixes statt freiem `suggestedFix` liefern? | Relevant fuer spaetere UX-Erweiterungen. |
| Soll ein API-Fehler mit fachlichen Teilfehlern auch `targets` enthalten duerfen? | Relevant fuer Importfehler und JSON-Formatfehler. |
| Wie detailliert darf `technicalMessage` im Produktivbetrieb sein? | Relevant fuer Sicherheit und Debugging. |

## Zusammenfassung

Der Error Contract trennt technische API-Fehler von fachlichen Validierungsergebnissen. API-Fehler werden als `ApiErrorDto` transportiert; `Check Constraints` liefert dagegen auch bei fachlich ungueltigen Modellen ein strukturiertes `ValidationResultDto`.

Fuer das Frontend sind stabile IDs, `targets`, `contextObjectId`, `invariantId` und OCL-Positionsdaten entscheidend. Damit kann die UI fehlerhafte Objekte im Diagramm markieren, Meldungen im Validation Results Panel anzeigen, OCL-Stellen im Editor hervorheben und Nutzer per Klick zum betroffenen Element fuehren.
## B36 Commandfehler

Die additive Command-API verwendet `EXPECTED_REVISION_REQUIRED`,
`STALE_MODEL_REVISION`, `STALE_SNAPSHOT_REVISION`, `DRAFT_REQUIRED`,
`INVALID_DRAFT`, `ELEMENT_NOT_FOUND`, `OCL_COMPILE_FAILED`,
`NARY_END_REQUIRED` und `DELETE_BLOCKED`. HTTP 409 kennzeichnet Revision- und
Delete-Konflikte; fachliche Draftvalidierung bleibt HTTP 400, fehlende stabile
Ziele HTTP 404. Jeder Fehler erhaelt `details.draft` und, soweit vorhanden,
strukturierte `targets` oder `blockers`.
## B37 Contract-Acceptance

Die B37-Abnahme vom 28. August 2026 bestätigt den bestehenden B35-/B36-
Fehlerkatalog ohne Korrektur. B38 ergänzt `INVALID_FEATURE_KIND`,
`UNKNOWN_REDEFINED_FEATURE`, `INVALID_REDEFINITION_OWNER`,
`INCOMPATIBLE_REDEFINED_FEATURE`, `DUPLICATE_FEATURE_ID` und
`AMBIGUOUS_INHERITED_FEATURE`. Der letzte Code ist ein HTTP-409-Konflikt;
ungültige Art, Ziele oder Konformität sind HTTP 400, fehlende lokale Features
HTTP 404 und veraltete Revisionen HTTP 409. Fehler erhalten den vollständigen
Draft und strukturierte Ziele für Classifier, lokales Feature,
Redefinitionsziele und gegebenenfalls Supertypes. Definitionsfehler verwenden
die nachfolgend dokumentierten, durch B40 implementierten Codes.

## B39 Statische Features

`STATIC_VALUE_TYPE_MISMATCH`, `STATIC_CONTEXT_SELF_REFERENCE`,
`CLASSIFIER_VALUE_REQUIRES_STATIC_ATTRIBUTE` und
`DERIVED_STATIC_VALUE_READ_ONLY` sind Draftvalidierungen mit HTTP 400.
`STATIC_FEATURE_HAS_NO_OBJECT_SLOT` verhindert Objekt-Slots für statische
Attribute. `ELEMENT_NOT_FOUND` und `STALE_MODEL_REVISION` behalten ihre
bestehende HTTP-404-/HTTP-409-Bedeutung. OCL-Ergebnisse können
`UNKNOWN_STATIC_FEATURE` oder `STATIC_VALUE_UNSET` als strukturierte
Evaluation-Diagnostics liefern. Commandfehler enthalten den vollständigen
Attribut-Draft und stabile Class-/Attribute-Referenzen.

## B40 Definitionen

`OCL_DEFINITION_COMPILE_FAILED`, `OCL_DEFINITION_PARAMETER_CONFLICT` und
`PACKAGE_DEFINITION_SELF_NOT_ALLOWED` sind HTTP-400-Draftvalidierungen.
`OCL_DEFINITION_OWNER_NOT_FOUND` und `OCL_DEFINITION_NOT_FOUND` sind HTTP 404.
`OCL_DEFINITION_SIGNATURE_CONFLICT`, `OCL_DEFINITION_CYCLE`,
`DELETE_BLOCKED` und `STALE_MODEL_REVISION` sind HTTP 409. Fehler enthalten
`details.draft` sowie stabile Definition-, Owner-, Parameter- oder Blocker-IDs;
Compile-Diagnostics enthalten Source Ranges.

## B41 Snapshot-Commands

`EXPECTED_REVISION_REQUIRED` ist HTTP 400 und
`STALE_SNAPSHOT_REVISION` HTTP 409. Fachliche Object-/Slot-/Link-Validation
verwendet die bestehenden Domaincodes, insbesondere `TYPE_ERROR`,
`UNKNOWN_CLASS`, `UNKNOWN_ATTRIBUTE`, `INVALID_SLOT_VALUE`, `INVALID_LINK`,
`ASSOCIATION_CLASS_IDENTITY_VIOLATION`, `COMPOSITE_OWNERSHIP_VIOLATION` und
`COMPOSITION_CYCLE`. Unbekannte referenzierte Projekte oder Snapshot-Elemente
werden als HTTP 404 transportiert; echte Link-/Composition-Konflikte als HTTP
409. Jeder B41-Fehler enthaelt `details.draft` und strukturierte
`details.targets` mit stabiler ID, fachlichem Namen und Feldpfad.

## B42 Object-Link-Lifecycle

| Code | HTTP | Bedeutung |
|---|---:|---|
| `EXPECTED_REVISION_REQUIRED` | 400 | Update oder Delete enthaelt keine Snapshotrevision. |
| `INVALID_CASCADE_SELECTION` | 400 | Die ausgewaehlte Reference-ID ist im aktuellen Impact nicht erlaubt. |
| `ELEMENT_NOT_FOUND` | 404 | Projekt oder Object Link existiert nicht. |
| `STALE_SNAPSHOT_REVISION` | 409 | Link oder Snapshot wurde seit Laden des Drafts geaendert. |
| `DELETE_BLOCKED` | 409 | Eine erforderliche Cascade fehlt oder ein Fremdlink referenziert das Linkobjekt. |
| `INVALID_LINK` | 400/404/409 | End-, Object-, Qualifier-, Duplicate- oder Association-Class-Validierung schlug fehl. |
| `OBJECT_LINK_DUPLICATE` | 409 | Eine Endzuordnung verletzt die `unique`-Semantik. |
| `MULTIPLICITY_VIOLATION` | 409 | Die Mutation ueberschreitet die obere Multiplizitaetsgrenze im betroffenen Bindungs-/Qualifierbereich. |
| `COMPOSITE_OWNERSHIP_VIOLATION` | 409 | Das Update erzeugt mehrfache Composite-Ownership. |
| `COMPOSITION_CYCLE` | 409 | Das Update erzeugt einen Composition-Zyklus. |

Updatefehler enthalten `details.draft` und strukturierte Targets.
Delete-Konflikte enthalten Delete-Draft, `currentImpact` und `blockers`.

## B43 Enumeration-Lifecycle

| Code | HTTP | Bedeutung |
|---|---:|---|
| `INVALID_ENUMERATION` | 400 | Name, Sichtbarkeit, Package, Literal-ID oder eindeutiger Literalname ist ungueltig. |
| `EXPECTED_REVISION_REQUIRED` | 400 | Enumeration-Mutation enthaelt keine Modellrevision. |
| `ELEMENT_NOT_FOUND` | 404 | Enumeration oder Literal existiert nicht. |
| `ENUMERATION_REFERENCED` | 409 | Ein verwendeter Enumerationstyp soll umbenannt oder in ein anderes Package verschoben werden. |
| `ENUMERATION_LITERAL_REFERENCED` | 409 | Ein verwendetes Literal soll ueber Update entfernt oder umbenannt werden. |
| `DELETE_BLOCKED` | 409 | Typ-, Slot-, Qualifier-, Classifierwert- oder OCL-Referenzen blockieren Delete. |
| `STALE_MODEL_REVISION` | 409 | Das Modell wurde seit Laden des Enumeration-Drafts geaendert. |

Fehler enthalten den vollstaendigen Draft und je nach Fall `literalId`,
`blockers`, `targets`, erwartete und aktuelle Revision. Blocker verwenden
stabile Element- und Reference-IDs sowie fachliche Namen und Feldpfade.

## B44 Feature- und Association-Drafts

| Code | HTTP | Bedeutung |
|---|---:|---|
| `OCL_FEATURE_COMPILE_FAILED` | 400 | Init/Derive, Operationsbody oder Contract besitzt Syntax-/Typfehler; `diagnostics` enthaelt Source Ranges. |
| `OCL_FEATURE_TYPE_MISMATCH` | 400 | Ergebnis eines Feature-Ausdrucks konformiert nicht zum Attribut-, Rueckgabe- oder Boolean-Contracttyp. |
| `OPERATION_PARAMETER_CONFLICT` | 400 | Parameternamen im vollstaendigen Operations-Draft sind nicht eindeutig. |
| `INVALID_OPERATION_CONTRACT_KIND` | 400 | Contract-Art ist weder `PRE` noch `POST`. |
| `ABSTRACT_OPERATION_BODY_NOT_ALLOWED` | 400 | Eine abstrakte Operation besitzt einen Body. |
| `STATIC_CONTEXT_SELF_REFERENCE` | 400 | Ein statisches Feature referenziert `self`. |
| `NARY_END_REQUIRED` | 400 | Eine Association besitzt weniger als zwei Ends. |
| `ELEMENT_NOT_FOUND` | 404 | Class, Feature, Association oder referenziertes Element existiert nicht. |
| `STALE_MODEL_REVISION` | 409 | Das Modell wurde seit Laden des Drafts geaendert. |

Alle B44-Commandfehler enthalten `details.draft`. Compilefehler enthalten
`details.diagnostics` und `details.targets`; sonstige Domainfehler werden durch
die Command-Schicht um stabile Ziele, Feldpfade und fachliche Namen ergaenzt.

## B45 Package- und Import-Lifecycle

| Code | HTTP | Bedeutung |
|---|---:|---|
| `INVALID_PACKAGE` | 400 | Package-ID oder qualifizierter Name ist syntaktisch ungueltig. |
| `INVALID_IMPORT` | 400 | Import-ID, Package-Referenz oder Alias ist ungueltig. |
| `EXPECTED_REVISION_REQUIRED` | 400 | Package-/Import-Mutation enthaelt keine Modellrevision. |
| `INVALID_CASCADE_SELECTION` | 400 | Eine ausgewaehlte Reference-ID ist im aktuellen Impact nicht cascadefaehig. |
| `ELEMENT_NOT_FOUND` | 404 | Package oder Import existiert nicht. |
| `DUPLICATE_NAMESPACE` | 409 | Ein Package mit demselben qualifizierten Namen existiert bereits. |
| `DUPLICATE_IMPORT_ALIAS` | 409 | Der Alias ist im importierenden Package nicht eindeutig. |
| `IMPORT_CYCLE` | 409 | Der geaenderte Import erzeugt einen gerichteten Package-Importzyklus. |
| `PACKAGE_CYCLE` | 409 | Ein Package wuerde unter sich selbst oder einen eigenen Nachfahren verschoben. |
| `QUALIFIED_NAME_CONFLICT` | 409 | Rename/Move erzeugt einen qualifizierten Classifier-Namenskonflikt. |
| `STALE_MODEL_REVISION` | 409 | Das Modell wurde seit Laden des Drafts geaendert. |
| `DELETE_BLOCKED` | 409 | Erforderliche Cascades fehlen oder Typ-, Generalization-, Definition- beziehungsweise OCL-/Importreferenzen bleiben bestehen. |

Fehler enthalten `details.draft` und strukturierte `targets`; Delete-Konflikte
enthalten `target`, `blockers` und die aktuelle Revision. Alle Referenzen
verwenden stabile Reference-/Element-IDs, fachliche Namen, Feldpfade und eine
Relation wie `OWNED_CLASSIFIER`, `TYPED_BY_PACKAGE`, `REFERENCES_PACKAGE` oder
`REFERENCES_IMPORT`.

## B46 V2-Fehlervertragsabnahme

B46 fuehrt keine neuen Fehlercodes ein. Die vorhandenen B41-B45-Codes sind
fuer Validation, Not Found, Domain Conflict, Delete Blocker, ungueltige
Cascade-Auswahl sowie Modell- und Snapshot-Revision Conflict abgenommen.
Commandfehler enthalten den vollstaendigen Draft, stabile Targets und die
fachlich relevante Revision; Delete-Konflikte enthalten aktuellen Impact und
Blocker. Direkte Legacy-Routen bleiben kompatibel, gelten ohne diesen
strukturierten Umschlag jedoch nicht als V2-Schreibvertrag.

## B48-Fehlervertraege

- `ASSOCIATION_CLASS_ALREADY_BOUND` (`409`): Die Association besitzt bereits
  eine Association Class.
- `ASSOCIATION_CLASS_IDENTITY_VIOLATION` (`409`): Association, Linkobjekt,
  Classifier oder `associationClassObjectId` bilden keine konsistente
  Association-Class-Identitaet.
- `INVALID_SLOT_VALUE`, `OBJECT_LINK_DUPLICATE`, `MULTIPLICITY_VIOLATION`,
  `COMPOSITE_OWNERSHIP_VIOLATION`, `ELEMENT_NOT_FOUND`,
  `STALE_MODEL_REVISION` und `STALE_SNAPSHOT_REVISION` gelten unveraendert.
- Jeder Fehler bleibt seiteneffektfrei und gibt den vollstaendigen
  Aggregatedraft unter `details.draft` sowie stabile Ziele unter
  `details.targets` zurueck.

## B49 Operation-Delete-Fehlervertrag

Der generische Operation-Delete liefert nicht mehr `OWNER_REQUIRED`. Die
Owner Class wird aus der stabilen `operationId` ermittelt.

| Code | HTTP | Bedeutung |
|---|---:|---|
| `EXPECTED_REVISION_REQUIRED` | 400 | Der Delete-Draft enthaelt keine Modellrevision; Draft und aktueller Impact werden zurueckgegeben. |
| `INVALID_CASCADE_SELECTION` | 400 | Eine ausgewaehlte Referenz ist im aktuellen Operation-Impact nicht cascadefaehig. |
| `ELEMENT_NOT_FOUND` | 404 | Die Operation existiert projektweit nicht; `details.target` referenziert die angeforderte Operation. |
| `DELETE_BLOCKED` | 409 | Body-, Contract-, Invariant-, Definition-, Redefinitions- oder Attributausdrucksreferenzen bestehen fort. |
| `STALE_MODEL_REVISION` | 409 | Die Modellrevision ist veraltet; vollstaendiger Draft und `currentImpact` bleiben erhalten. |

Alle Targets und Blocker enthalten stabile Element-/Reference-IDs,
fachliche Namen, Feldpfade und Relationen. Validation, Not Found, Blocker,
ungueltige Cascade-Auswahl und Revision Conflict sind seiteneffektfrei.

## B50 Fehlervertrag fuer strukturierte Werttypen

B50 verwendet bestehende Fehlercodes und erweitert deren strukturierte
Details, statt parallele Codes einzufuehren:

| Code | HTTP | B50-Bedeutung |
|---|---:|---|
| `TYPE_ERROR` | 400 | Attribut- oder DataType-Property-Typ ist syntaktisch ungueltig, unbekannt, mehrdeutig oder zyklisch. |
| `STATIC_VALUE_TYPE_MISMATCH` | 400 | Statischer Classifierwert konformiert rekursiv nicht zum Attributtyp. |
| `INVALID_SLOT_VALUE` | 400 | Initialer oder aktualisierter Slotwert konformiert rekursiv nicht zum Attributtyp. |
| `ELEMENT_NOT_FOUND` | 404 | Classifier, Attribut, DataType, Object oder Slot existiert nicht. |
| `DELETE_BLOCKED` | 409 | Direkte oder verschachtelte Typreferenzen blockieren DataType Delete. |
| `STALE_MODEL_REVISION` | 409 | Modellmutation basiert auf einer veralteten Modellrevision. |
| `STALE_SNAPSHOT_REVISION` | 409 | Object-/Slotmutation basiert auf einer veralteten Snapshotrevision. |

Typ- und Wertfehler liefern `reason`, `expectedType`, `actualValue` und einen
rekursiven `fieldPath`, soweit anwendbar. Commandfehler enthalten ausserdem
den vollstaendigen Draft und stabile `targets` fuer Classifier, Attribute,
DataType, Object und Slot. Delete-Konflikte enthalten Ziel, Blocker und
aktuelle Revision. Alle Validation-, Not-Found-, Delete- und Revisionfehler
sind seiteneffektfrei.

## B51 DataType-Property-Delete-Fehlervertrag

B51 fuehrt keinen neuen Fehlercode ein:

| Code | HTTP | B51-Bedeutung |
|---|---:|---|
| `EXPECTED_REVISION_REQUIRED` | 400 | Der Delete-Draft enthaelt keine Modellrevision. |
| `INVALID_CASCADE_SELECTION` | 400 | Eine Cascade wurde ausgewaehlt; Property-Delete erlaubt keine automatische Migration oder Cascade. |
| `ELEMENT_NOT_FOUND` | 404 | Owner-DataType oder owner-lokale Property-ID existiert nicht. |
| `DELETE_BLOCKED` | 409 | Persistierte rekursive Classifier-/Slotwerte oder OCL-Propertyzugriffe bestehen fort; dies gilt auch fuer entfernte IDs im Full-DataType-Update. |
| `STALE_MODEL_REVISION` | 409 | Die Modellrevision ist veraltet. |

Fehler enthalten den vollstaendigen Draft. Delete Blocked und Revision
Conflict enthalten `currentImpact`; Blocker referenzieren Property,
Owner-DataType, Attribute, Object/Slot oder OCL-Owner stabil und fachlich.
OCL-Blocker besitzen zusaetzlich `sourceRange`. Alle Fehler sind atomar und
seiteneffektfrei.
