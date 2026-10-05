# Error and Result Model

## Zweck dieser Datei

Diese Datei beschreibt das Fehler-, Ergebnis- und Meldungsmodell des neuen Backends. Sie legt fest, wie API-Fehler, Validierungsfehler, OCL-Diagnosen, UML-Modellfehler, Snapshot-Fehler, Multiplizitätsverletzungen und Invariantenverletzungen strukturiert repräsentiert werden.

Das Ziel ist ein einheitliches Modell, das sowohl für Backend-Tests als auch für das React/TypeScript-Frontend nutzbar ist. Fehler müssen maschinenlesbar sein und gleichzeitig verständliche Meldungen für Nutzer liefern.

## Anforderungen an Fehlermodelle

| Anforderung | Beschreibung |
|---|---|
| Maschinenlesbar | Fehler besitzen stabile Codes, Severity, Phase und Elementreferenzen. |
| UI-mappbar | Fehler referenzieren Objekte, Links, Slots, Klassen, Invarianten und OCL-Source-Ranges. |
| Nutzerverständlich | Jeder Fehler besitzt eine klare User-facing Message. |
| Debug-fähig | Technische Details können optional enthalten sein, ohne die UI zu überladen. |
| Trennung von Fehlerarten | API-Fehler sind nicht dasselbe wie fachliche Validation Errors. |
| Stabil testbar | Tests können auf Codes, IDs und Felder prüfen, nicht auf freie Texte. |
| Erweiterbar | Neue OCL-, UML- und Snapshot-Fehler können ergänzt werden. |

Das Frontend muss mit den Ergebnissen mindestens Folgendes tun können:

- Objekte rot markieren,
- Fehler-Badges anzeigen,
- Validation Results Panel füllen,
- Klick auf Fehler dem richtigen Element zuordnen,
- OCL-Editorfehler an passender Stelle anzeigen,
- betroffene Properties Panel Felder hervorheben.

## Ergebnisarten

Das Backend liefert unterschiedliche Ergebnisarten. Sie sollen ähnlich aufgebaut sein, aber nicht vermischt werden.

| Ergebnisart | Zweck | Beispiel |
|---|---|---|
| `ApiErrorResponse` | Technische oder requestbezogene HTTP-Fehler. | Projekt nicht gefunden, ungültiges JSON. |
| `ValidationResult` | Ergebnis eines vollständigen Constraint Checks. | `VALID`, `INVALID`, Fehlerliste. |
| `ValidationError` | Einzelner fachlicher Fehler innerhalb eines Validation Results. | `INVARIANT_VIOLATION`. |
| `OclDiagnostic` | Syntax-, Typ- oder Evaluationsdiagnose für OCL. | `SYNTAX_ERROR`, `TYPE_ERROR`. |
| `ModelTextDiagnostic` | Diagnose für vollständigen USE-ähnlichen Editor-Text. | `UNSUPPORTED_SYNTAX`, `SYNTAX_ERROR`. |
| `OclParseResult` | Ergebnis eines Parse-Endpunkts. | AST Preview oder Syntaxdiagnosen. |
| `OclTypecheckResult` | Ergebnis eines Typecheck-Endpunkts. | Result Type oder Typdiagnosen. |
| `OclEvaluationResult` | Ergebnis einer gezielten OCL-Auswertung. | `BooleanValue(false)`. |
| `CommandResult` | Ergebnis einer Create/Update/Delete-Operation. | erzeugte Klasse oder Domain-Fehler. |

Empfohlene Trennung:

| Situation | Ergebnis |
|---|---|
| HTTP-Request ist technisch ungültig | `ApiErrorResponse` mit passendem HTTP-Status. |
| Projektzustand ist fachlich ungültig | `ValidationResult` mit `errors`. |
| OCL-Ausdruck hat Syntaxfehler beim Editor-Check | `OclParseResult` mit Diagnostics. |
| OCL-Ausdruck verletzt Invariante beim Constraint Check | `ValidationError` im `ValidationResult`. |
| Backend wirft unerwartete Exception | `ApiErrorResponse`, keine fachliche Validation Error. |

## Severity-Modell

| Severity | Bedeutung | MVP-Nutzung |
|---|---|---|
| `ERROR` | Zustand ist ungültig oder Prüfung kann nicht zuverlässig abgeschlossen werden. | Pflicht |
| `WARNING` | Auffälliger Zustand, aber nicht zwingend ungültig. | Optional |
| `INFO` | Hinweis oder Zusatzinformation. | Optional |

MVP-Empfehlung:

- `ERROR` wird aktiv verwendet.
- `WARNING` und `INFO` werden im Modell vorgesehen, aber nur genutzt, wenn klare Produktregeln existieren.
- UI-Farben und Icons werden im Frontend aus `severity` und `code` abgeleitet.

## Error Codes

### Fachliche Error Codes

| Code | Kategorie | Bedeutung | Typische UI-Markierung |
|---|---|---|---|
| `SYNTAX_ERROR` | OCL / Modellnotation | Syntax ist ungültig. | OCL Editor, Invariant Panel |
| `UNSUPPORTED_SYNTAX` | Modellnotation | Syntax ist grundsätzlich lesbar, wird im MVP-Subset aber nicht unterstützt. | OCL Editor, Warning im Apply-Ergebnis |
| `TYPE_ERROR` | UML/OCL Typisierung | Typen passen nicht oder Ausdruck ist nicht boolean. | OCL Editor, Attribut, Klasse |
| `UNKNOWN_CLASS` | Referenzfehler | Klasse existiert nicht. | Klasse, Objekt, Association End |
| `UNKNOWN_ATTRIBUTE` | Referenzfehler | Attribut oder Property existiert nicht. | Attribut, Slot, OCL-Stelle |
| `INVALID_SLOT_VALUE` | Snapshot | Slot-Wert passt nicht zum Attributtyp oder ist unset, wenn benötigt. | Objekt, Slot-Zeile |
| `INVALID_LINK` | Snapshot | Objektlink passt nicht zur Association. | Link, beteiligte Objekte |
| `MULTIPLICITY_VIOLATION` | UML/Snapshot | Linkanzahl verletzt Multiplizität. | Objekt, Link, Association End |
| `INVARIANT_VIOLATION` | OCL/Evaluation | OCL-Invariante ergibt `false`. | Objekt, Invariante |
| `EVALUATION_ERROR` | OCL/Evaluation | Ausdruck kann gegen Snapshot nicht ausgewertet werden. | Objekt, Invariante, OCL-Stelle |

### Technische API Error Codes

| Code | HTTP-Status | Bedeutung |
|---|---:|---|
| `BAD_REQUEST` | 400 | Request ist syntaktisch oder strukturell ungültig. |
| `PROJECT_NOT_FOUND` | 404 | Projekt existiert nicht. |
| `RESOURCE_NOT_FOUND` | 404 | Klasse, Objekt, Link oder andere Ressource existiert nicht. |
| `CONFLICT` | 409 | Änderung kollidiert mit aktuellem Zustand. |
| `UNPROCESSABLE_ENTITY` | 422 | Request ist lesbar, aber fachlich nicht akzeptierbar. |
| `UNSUPPORTED_FORMAT_VERSION` | 400 | JSON-Projektformatversion ist unbekannt. |
| `INTERNAL_ERROR` | 500 | Unerwarteter technischer Fehler. |

Technische Codes dürfen nicht als Constraint-Verletzungen im Validation Results Panel erscheinen. Sie werden in API-Fehlerdialogen oder globalen Error States behandelt.

## API Error Format

`ApiErrorResponse` wird für technische HTTP-Fehler verwendet.

```json
{
  "error": {
    "code": "PROJECT_NOT_FOUND",
    "severity": "ERROR",
    "message": "Project 'project-unknown' was not found.",
    "userMessage": "Das Projekt konnte nicht gefunden werden.",
    "requestId": "req-20260711-001",
    "timestamp": "2026-07-11T20:45:00Z",
    "details": {
      "projectId": "project-unknown"
    }
  }
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `error.code` | Ja | Maschinenlesbarer technischer Fehlercode. |
| `error.severity` | Ja | In der Regel `ERROR`. |
| `error.message` | Ja | Technische oder entwicklernahe Meldung. |
| `error.userMessage` | Nein, empfohlen | Nutzerverständliche Meldung. |
| `error.requestId` | Nein, empfohlen | Korrelation in Logs. |
| `error.timestamp` | Nein | Zeitpunkt des Fehlers. |
| `error.details` | Nein | Strukturierte Zusatzdaten. |

Verwendung:

- `GET /api/v1/projects/unknown` -> `PROJECT_NOT_FOUND`.
- Import mit nicht lesbarem JSON -> `BAD_REQUEST`.
- Import mit unbekannter `formatVersion` -> `UNSUPPORTED_FORMAT_VERSION`.

## Validation Error Format

`ValidationError` ist ein fachlicher Befund innerhalb eines `ValidationResult`.

```json
{
  "id": "error-invariant-max-books-alice",
  "code": "INVARIANT_VIOLATION",
  "severity": "ERROR",
  "phase": "OCL_EVALUATION",
  "message": "Object 'alice' violates invariant 'maxBooks'.",
  "userMessage": "Das Objekt 'alice' verletzt die Invariante 'maxBooks'.",
  "affectedElements": [
    {
      "elementType": "OBJECT",
      "elementId": "obj-alice",
      "path": "objectModel.objects[obj-alice]"
    },
    {
      "elementType": "INVARIANT",
      "elementId": "inv-user-max-books",
      "path": "umlModel.invariants[inv-user-max-books]"
    }
  ],
  "modelElementIds": ["class-user"],
  "objectIds": ["obj-alice"],
  "linkIds": [],
  "slotIds": [],
  "invariantId": "inv-user-max-books",
  "contextClassId": "class-user",
  "contextObjectId": "obj-alice",
  "sourceRange": {
    "expressionId": "expr-user-max-books",
    "startLine": 1,
    "startColumn": 1,
    "endLine": 1,
    "endColumn": 16
  },
  "details": {
    "contextClass": "User",
    "invariantName": "maxBooks",
    "expression": "self.books <= 5",
    "actualValue": false
  }
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Fehler-ID innerhalb eines Validierungslaufs. |
| `code` | Ja | Fachlicher Fehlercode. |
| `severity` | Ja | `ERROR`, `WARNING` oder `INFO`. |
| `phase` | Ja | Validierungsphase, z. B. `OCL_EVALUATION`. |
| `message` | Ja | Entwicklernahe oder präzise Standardmeldung. |
| `userMessage` | Nein, empfohlen | Nutzerverständliche Meldung. |
| `affectedElements` | Nein, empfohlen | Generische Liste betroffener Elemente. |
| `modelElementIds` | Nein | Klassen, Attribute, Associations, Association Ends. |
| `objectIds` | Nein | Betroffene Objekte. |
| `linkIds` | Nein | Betroffene Objektlinks. |
| `slotIds` | Nein | Betroffene Slots. |
| `invariantId` | Nein | Betroffene Invariante. |
| `contextClassId` | Nein | Kontextklasse einer OCL-Prüfung. |
| `contextObjectId` | Nein | `self`-Objekt bei OCL-Evaluation. |
| `sourceRange` | Nein | Position im OCL-Ausdruck oder Textelement. |
| `details` | Nein | Strukturierte Zusatzdaten. |
| `debug` | Nein | Nur bei Debug-Modus oder intern. |

## OCL Error Format

OCL-Fehler treten in drei Phasen auf:

- Syntax,
- Typechecking,
- Evaluation.

Ein gemeinsames `OclDiagnostic`-Format erleichtert die Verwendung in OCL-Endpunkten und im Validation Service.

```json
{
  "code": "TYPE_ERROR",
  "severity": "ERROR",
  "phase": "OCL_TYPE_CHECK",
  "message": "Operator '<=' expects numeric operands but got String and Integer.",
  "userMessage": "Der Operator '<=' kann hier nicht auf einen String angewendet werden.",
  "expressionId": "expr-user-invalid",
  "invariantId": "inv-user-invalid",
  "contextClassId": "class-user",
  "sourceRange": {
    "startLine": 1,
    "startColumn": 11,
    "endLine": 1,
    "endColumn": 13
  },
  "expectedType": "Integer | Real",
  "actualType": "String",
  "modelElementIds": ["attr-user-name"],
  "details": {
    "operator": "<=",
    "leftType": "String",
    "rightType": "Integer"
  }
}
```

OCL-spezifische Felder:

| Feld | Bedeutung |
|---|---|
| `expressionId` | OCL-Ausdruck, auf den sich der Fehler bezieht. |
| `invariantId` | Invariante, falls der Ausdruck Teil einer Invariante ist. |
| `contextClassId` | Klasse, deren Instanzen `self` repräsentiert. |
| `contextObjectId` | Objekt, falls Fehler während Evaluation für ein konkretes `self` entstand. |
| `sourceRange` | Position im OCL-Text. |
| `expectedType` | Erwarteter Typ. |
| `actualType` | Tatsächlicher Typ. |
| `operator` | Betroffener Operator, falls relevant. |
| `expressionText` | Optionaler Ausschnitt des Ausdrucks. |

Beispiel Syntaxfehler:

```json
{
  "code": "SYNTAX_ERROR",
  "severity": "ERROR",
  "phase": "OCL_SYNTAX",
  "message": "Unexpected end of expression after '<='.",
  "userMessage": "Der OCL-Ausdruck endet unerwartet nach '<='.",
  "expressionId": "expr-user-max-books",
  "invariantId": "inv-user-max-books",
  "sourceRange": {
    "startLine": 1,
    "startColumn": 14,
    "endLine": 1,
    "endColumn": 16
  }
}
```

Beispiel Evaluationsfehler:

```json
{
  "code": "EVALUATION_ERROR",
  "severity": "ERROR",
  "phase": "OCL_EVALUATION",
  "message": "Slot value for attribute 'books' is missing on object 'alice'.",
  "userMessage": "Für das Objekt 'alice' fehlt der Wert des Attributs 'books'.",
  "invariantId": "inv-user-max-books",
  "contextClassId": "class-user",
  "contextObjectId": "obj-alice",
  "modelElementIds": ["attr-user-books"],
  "objectIds": ["obj-alice"],
  "slotIds": ["slot-alice-books"]
}
```

## Element Mapping

Element Mapping ist der wichtigste Teil für die UI. Jeder Fehler sollte möglichst konkret referenzieren, was betroffen ist.

### Elementtypen

| Elementtyp | Beschreibung | Beispiel-ID |
|---|---|---|
| `PROJECT` | Projektcontainer. | `project-library` |
| `UML_MODEL` | UML-Modell. | `uml-library` |
| `CLASS` | UML-Klasse. | `class-user` |
| `ATTRIBUTE` | UML-Attribut. | `attr-user-books` |
| `OPERATION` | UML-Operation. | `op-user-can-borrow` |
| `PARAMETER` | Operationsparameter. | `param-book` |
| `ASSOCIATION` | UML-Association. | `assoc-borrows` |
| `ASSOCIATION_END` | Association End / Rolle. | `end-borrows-books` |
| `INVARIANT` | OCL-Invariante. | `inv-user-max-books` |
| `OCL_EXPRESSION` | OCL-Ausdruck. | `expr-user-max-books` |
| `OBJECT` | Objektinstanz. | `obj-alice` |
| `SLOT` | Attributwert. | `slot-alice-books` |
| `OBJECT_LINK` | Objektlink. | `link-alice-moby` |
| `LAYOUT_ELEMENT` | Layoutreferenz. | `layout-class-user` |

### Generisches Elementformat

```json
{
  "elementType": "OBJECT",
  "elementId": "obj-alice",
  "path": "objectModel.objects[obj-alice]",
  "label": "alice : User"
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `elementType` | Ja | Typ des Elements. |
| `elementId` | Ja | Stabile ID. |
| `path` | Nein | Pfad im Projektmodell. |
| `label` | Nein | Nutzerlesbares Label. |

### Modellpfade

Pfade sind optional, aber hilfreich für Debugging und Tests.

| Element | Beispielpfad |
|---|---|
| Klasse | `umlModel.classes[class-user]` |
| Attribut | `umlModel.classes[class-user].attributes[attr-user-books]` |
| Association End | `umlModel.associations[assoc-borrows].ends[end-borrows-books]` |
| Invariante | `umlModel.invariants[inv-user-max-books]` |
| Objekt | `objectModel.objects[obj-alice]` |
| Slot | `objectModel.objects[obj-alice].slots[slot-alice-books]` |
| Link | `objectModel.links[link-alice-moby]` |

## User-facing Messages

Das Backend soll strukturierte Daten liefern. Trotzdem braucht jeder Fehler eine nutzerverständliche Standardmeldung.

| Feld | Zweck |
|---|---|
| `message` | Präzise technische/fachliche Meldung, gut für Tests und Logs. |
| `userMessage` | Verständliche UI-Meldung, frei von internen Details. |
| `details` | Strukturierte Daten, aus denen UI bessere Texte bauen kann. |

Beispiele:

| Code | `message` | `userMessage` |
|---|---|---|
| `UNKNOWN_CLASS` | `Object 'alice' references unknown class 'class-user'.` | `Das Objekt 'alice' verweist auf eine nicht vorhandene Klasse.` |
| `INVALID_SLOT_VALUE` | `Slot 'books' expects Integer but contains String.` | `Der Wert von 'books' passt nicht zum erwarteten Typ Integer.` |
| `MULTIPLICITY_VIOLATION` | `Object 'alice' has 6 links for role 'borrowedBooks', expected 0..5.` | `Das Objekt 'alice' hat zu viele verknüpfte Bücher.` |
| `INVARIANT_VIOLATION` | `Object 'alice' violates invariant 'maxBooks'.` | `Das Objekt 'alice' verletzt die Invariante 'maxBooks'.` |
| `TYPE_ERROR` | `Operator '<=' expects numeric operands.` | `Der OCL-Ausdruck verwendet '<=' mit unpassenden Typen.` |

Empfehlung:

- Das Backend liefert `userMessage` als brauchbaren Standard.
- Das Frontend darf daraus kontextspezifische UI-Texte machen.
- Tests sollten primär auf `code`, IDs und Details prüfen, nicht auf vollständige Message-Texte.

## Debug-Informationen

Debug-Informationen sind optional und sollten nur geliefert werden, wenn sie angefordert werden oder für interne Logs genutzt werden.

```json
{
  "debug": {
    "validator": "OclEvaluator",
    "type": "Boolean",
    "evaluationTrace": [
      {
        "expressionText": "self.books",
        "resultType": "Integer",
        "resultPreview": "6",
        "objectIds": ["obj-alice"]
      },
      {
        "expressionText": "self.books <= 5",
        "resultType": "Boolean",
        "resultPreview": "false",
        "objectIds": ["obj-alice"]
      }
    ],
    "internalCause": "Invariant evaluated to false"
  }
}
```

Regeln:

| Regel | Begründung |
|---|---|
| Debugdaten sind optional. | Responses bleiben klein und UI-freundlich. |
| Keine Stacktraces im normalen API-Response. | Sicherheits- und UX-Gründe. |
| `requestId` für technische Fehler. | Logs können korreliert werden. |
| Evaluation Trace nur bei Bedarf. | Für MVP intern nützlich, UI später. |

## Beispiel: Invariant Violation

Ausgangslage:

```ocl
context User inv maxBooks:
  self.books <= 5
```

Snapshot:

```text
alice : User
books = 6
```

Validation Error:

```json
{
  "id": "error-invariant-max-books-alice",
  "code": "INVARIANT_VIOLATION",
  "severity": "ERROR",
  "phase": "OCL_EVALUATION",
  "message": "Object 'alice' violates invariant 'maxBooks'.",
  "userMessage": "Das Objekt 'alice' verletzt die Invariante 'maxBooks'.",
  "affectedElements": [
    {
      "elementType": "OBJECT",
      "elementId": "obj-alice",
      "path": "objectModel.objects[obj-alice]",
      "label": "alice : User"
    },
    {
      "elementType": "INVARIANT",
      "elementId": "inv-user-max-books",
      "path": "umlModel.invariants[inv-user-max-books]",
      "label": "maxBooks"
    }
  ],
  "modelElementIds": ["class-user", "attr-user-books"],
  "objectIds": ["obj-alice"],
  "linkIds": [],
  "slotIds": ["slot-alice-books"],
  "invariantId": "inv-user-max-books",
  "contextClassId": "class-user",
  "contextObjectId": "obj-alice",
  "sourceRange": {
    "expressionId": "expr-user-max-books",
    "startLine": 1,
    "startColumn": 1,
    "endLine": 1,
    "endColumn": 16
  },
  "details": {
    "contextClass": "User",
    "contextObject": "alice",
    "invariantName": "maxBooks",
    "expression": "self.books <= 5",
    "actualValue": false,
    "observedValues": {
      "self.books": 6
    }
  }
}
```

Frontend-Wirkung:

- `obj-alice` im Objektdiagramm rot markieren.
- Fehler-Badge an `alice : User` anzeigen.
- Validation Results Panel mit `userMessage` füllen.
- Klick auf Fehler fokussiert `obj-alice`.
- Detailansicht kann Invariante `maxBooks` und Ausdruck zeigen.

## Beispiel: Type Error

Ausdruck:

```ocl
self.name <= 5
```

Problem:

`self.name` ist `String`, der Operator `<=` erwartet numerische Operanden.

OCL Diagnostic:

```json
{
  "code": "TYPE_ERROR",
  "severity": "ERROR",
  "phase": "OCL_TYPE_CHECK",
  "message": "Operator '<=' expects numeric operands but got String and Integer.",
  "userMessage": "Der Operator '<=' kann nicht auf das Textattribut 'name' angewendet werden.",
  "expressionId": "expr-user-name-invalid",
  "invariantId": "inv-user-name-invalid",
  "contextClassId": "class-user",
  "sourceRange": {
    "startLine": 1,
    "startColumn": 11,
    "endLine": 1,
    "endColumn": 13
  },
  "expectedType": "Integer | Real",
  "actualType": "String",
  "affectedElements": [
    {
      "elementType": "ATTRIBUTE",
      "elementId": "attr-user-name",
      "path": "umlModel.classes[class-user].attributes[attr-user-name]",
      "label": "User.name"
    },
    {
      "elementType": "OCL_EXPRESSION",
      "elementId": "expr-user-name-invalid",
      "path": "umlModel.invariants[inv-user-name-invalid].expression"
    }
  ],
  "modelElementIds": ["class-user", "attr-user-name"],
  "details": {
    "operator": "<=",
    "leftType": "String",
    "rightType": "Integer"
  }
}
```

Frontend-Wirkung:

- OCL-Editor markiert den Operator oder relevanten Ausdrucksbereich.
- Invariant Properties Panel zeigt Type Error.
- Validation Results Panel gruppiert den Fehler unter OCL/Typecheck.
- Klasse oder Attribut kann sekundär hervorgehoben werden.

## Frontend-Mapping

Das Frontend sollte Fehler nicht anhand von Texten, sondern anhand strukturierter Felder auswerten.

| UI-Funktion | Backend-Felder |
|---|---|
| Objekt rot markieren | `objectIds`, `affectedElements[elementType=OBJECT]` |
| Objektlink markieren | `linkIds`, `affectedElements[elementType=OBJECT_LINK]` |
| Slot-Feld markieren | `slotIds`, `affectedElements[elementType=SLOT]` |
| Klasse im Klassendiagramm markieren | `modelElementIds`, `affectedElements[elementType=CLASS]` |
| Association markieren | `modelElementIds`, `affectedElements[elementType=ASSOCIATION]` |
| Invariant Badge markieren | `invariantId`, `affectedElements[elementType=INVARIANT]` |
| OCL-Fehlerstelle markieren | `sourceRange.expressionId`, `startLine`, `startColumn`, `endLine`, `endColumn` |
| Validation Results Panel füllen | `severity`, `code`, `userMessage`, `details` |
| Klick auf Fehler navigiert | `affectedElements` priorisieren, sonst spezifische ID-Listen |

Priorisierung bei mehreren betroffenen Elementen:

| Fehlercode | Primäres Ziel |
|---|---|
| `INVARIANT_VIOLATION` | `contextObjectId` oder erstes `objectIds` |
| `MULTIPLICITY_VIOLATION` | betroffenes Objekt und Links |
| `INVALID_SLOT_VALUE` | Slot, danach Objekt |
| `INVALID_LINK` | Link, danach beteiligte Objekte |
| `TYPE_ERROR` | OCL Source Range, danach Modellreferenz |
| `SYNTAX_ERROR` | OCL Source Range |
| `UNSUPPORTED_SYNTAX` | Modelltext Source Range |
| `UNKNOWN_CLASS` | referenzierendes Element |
| `UNKNOWN_ATTRIBUTE` | OCL Source Range oder Slot/Attribut |
| `EVALUATION_ERROR` | Kontextobjekt und Invariante |

## Bezug zum originalen USE-Projekt

Das originale USE-Projekt liefert fachliche Referenzen für Fehlerarten und Validierungsausgaben, aber kein API-taugliches Ergebnisformat.

| USE-Bereich | Relevanz | Übertragung |
|---|---|---|
| `ParseErrorHandler` | Syntaxfehler mit Positionen | `SYNTAX_ERROR` mit `sourceRange`. |
| `USECompiler` / `parser.use` | Modelltextsyntax und unsupported Konstrukte | `UNSUPPORTED_SYNTAX` oder `SYNTAX_ERROR` im `model-text/apply` Flow. |
| `SemanticException` | semantische Fehler beim Modell-/OCL-Compile | `TYPE_ERROR`, `UNKNOWN_CLASS`, `UNKNOWN_ATTRIBUTE`. |
| `MSystemState.check(...)` | Zustands- und Invariantenprüfung | `ValidationResult`. |
| `checkStructure(...)` | Struktur- und Multiplizitätsprüfung | `INVALID_LINK`, `MULTIPLICITY_VIOLATION`. |
| `reportMultiplicityViolation(...)` | Textuelle Multiplizitätsmeldung | strukturierte Details mit Counts und Multiplicity. |
| `Evaluator` | OCL-Auswertung gegen Zustand | `INVARIANT_VIOLATION`, `EVALUATION_ERROR`. |
| Konsolen-/Shell-Ausgaben | Nutzerfeedback im Original | Nicht übernehmen; durch JSON Results ersetzen. |

Wichtige Abgrenzung:

- Keine Übernahme textueller USE-Fehlerausgaben als API-Vertrag.
- Keine direkte Kopplung an USE-Exception-Klassen.
- Fehlerarten werden fachlich übernommen, aber neu strukturiert.

## Teststrategie

Das Error and Result Model muss explizit getestet werden.

| Testbereich | Ziel | Beispiel |
|---|---|---|
| API Error Tests | technische Fehler liefern `ApiErrorResponse`. | Projekt nicht gefunden. |
| Validation Result Tests | Constraint Check liefert stabile Fehlerstruktur. | `INVARIANT_VIOLATION`. |
| OCL Diagnostic Tests | Syntax- und Typfehler enthalten Source Range. | `self.books <=`. |
| Element Mapping Tests | Fehler enthalten korrekte IDs. | `objectIds`, `slotIds`, `invariantId`. |
| Severity Tests | Fehler sind korrekt als `ERROR` klassifiziert. | Snapshot-Fehler. |
| Message Tests | `message` und `userMessage` sind vorhanden. | Invariantverletzung. |
| Debug Tests | Debugdaten erscheinen nur bei Option. | `includeDebugInfo = true`. |
| Frontend Contract Tests | TypeScript DTOs können Fehler verarbeiten. | OpenAPI/Schema-Validierung. |
| Regression Tests | USE-nahe Beispiele erzeugen erwartete Codes. | Multiplicity und Invariant Checks. |

Beispiel-Testassertionen:

```text
ValidationResult.status == INVALID
errors[0].code == INVARIANT_VIOLATION
errors[0].objectIds contains obj-alice
errors[0].invariantId == inv-user-max-books
errors[0].severity == ERROR
```

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Sollen `affectedElements` und die spezialisierten ID-Listen dauerhaft parallel existieren? | Parallelität ist redundant, aber frontendfreundlich. |
| Werden `userMessage`-Texte vom Backend lokalisiert oder bleibt Lokalisierung Frontend-Aufgabe? | Einfluss auf i18n-Strategie. |
| Wie detailliert muss `sourceRange` sein: Zeichenoffset oder Zeile/Spalte? | Empfehlung: Zeile/Spalte plus Expression-ID. |
| Sollen technische Details in normalen Responses enthalten sein? | Sicherheits- und UX-Frage. |
| Wie werden Warnungen im MVP erzeugt? | Severity-Modell ist vorbereitet, Regeln fehlen noch. |
| Werden Fehler gruppiert, z. B. Multiplicity und äquivalente OCL-Verletzung? | Einfluss auf Validation Results Panel. |
| Soll `message` stabil genug für Tests sein oder nur `code`/Details? | Empfehlung: Tests auf Codes und Details. |
| Wie werden mehrere OCL-Fehler in einer Invariante sortiert? | Einfluss auf UI und deterministische Tests. |

## Zusammenfassung

Das Backend benötigt ein einheitliches Fehler- und Ergebnismodell, das technische API-Fehler klar von fachlichen Validation Results trennt. Fachliche Fehler müssen Codes, Severity, Phase, Nutzertext, technische Details und präzise Elementreferenzen enthalten.

Für den MVP sind besonders `SYNTAX_ERROR`, `UNSUPPORTED_SYNTAX`, `TYPE_ERROR`, `UNKNOWN_CLASS`, `UNKNOWN_ATTRIBUTE`, `INVALID_SLOT_VALUE`, `INVALID_LINK`, `MULTIPLICITY_VIOLATION`, `INVARIANT_VIOLATION` und `EVALUATION_ERROR` relevant. Diese Struktur ermöglicht dem Frontend, Diagrammelemente zu markieren, OCL-Fehlerstellen anzuzeigen, unsupported Modelltext-Konstrukte im Editor zu markieren und das Validation Results Panel zuverlässig zu füllen.
