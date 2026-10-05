# API Design

## Zweck dieser Datei

Diese Datei beschreibt das REST-API-Design des neuen Java/Spring-Boot-Backends. Die API bildet den Vertrag zwischen Backend und dem separaten React/TypeScript-Frontend.

Sie definiert Ressourcen, Endpunkte, DTOs, Fehlerformate, Validierungsantworten und MVP-Abgrenzungen. Ziel ist eine API, die das Frontend für UML-Klassendiagramme, Objektdiagramme, OCL-Invarianten und Constraint-Validierung zuverlässig nutzen kann.

## API-Ziele

| Ziel | Beschreibung |
|---|---|
| Frontend-freundlicher Vertrag | React/TypeScript kann Projekte, Modellbestandteile, Snapshots und Validierungsergebnisse direkt verarbeiten. |
| Klare Ressourcengrenzen | Projekte, UML-Modell, Objektmodell, OCL und Validierung sind unterscheidbare API-Bereiche. |
| Stabile IDs | Alle bearbeitbaren Elemente besitzen stabile IDs für Diagramm-Mapping und Validation Results. |
| Strukturierte Fehler | Das Frontend muss keine Fehlermeldungstexte parsen. |
| Zentrale Semantik im Backend | OCL-Typechecking, OCL-Evaluation und Constraint Validation bleiben Backend-Verantwortung. |
| MVP-tauglich | Der MVP kann mit wenigen Endpunkten starten, ohne spätere feingranulare Endpunkte zu blockieren. |
| Erweiterbar | MVP-nahe Modelltextverarbeitung und Post-MVP-Funktionen wie vollständiger `.use` Import/Export, mehrere Snapshots oder erweiterte OCL-Prüfung bleiben möglich. |

## API-Prinzipien

| Prinzip | Entscheidung |
|---|---|
| REST/JSON | Kommunikation erfolgt über HTTP und JSON. |
| Ressourcenorientierung | Projekt, Klasse, Association, Invariante, Objekt und Link sind API-Ressourcen. |
| DTOs statt Domain-Objekte | API-DTOs sind explizite Vertragsobjekte und nicht identisch mit Domain-Klassen. |
| Backend validiert Semantik | Frontend darf UI-Vorvalidierung machen, aber Backend ist fachliche Wahrheit. |
| Vollständiges Projektformat möglich | `GET/PUT /api/v1/projects/{id}` unterstützt Save/Load und JSON-MVP-Format. |
| Vollständiger Editor-Text möglich | Der OCL Editor kann vollständigen USE-ähnlichen Modelltext senden; Backend wendet nur das dokumentierte MVP-Subset an. |
| Feingranulare Mutationen möglich | CRUD-Endpunkte für Klassen, Attribute, Associations, Objekte und Links unterstützen interaktive UI. |
| Deterministische Validierung | Gleicher Projektzustand liefert gleiche Validation Results. |
| Explizite Versionierung | API und Projektformat bekommen eigene Versionen. |

Empfehlung für den MVP:

- `GET /api/v1/projects/{id}` und `PUT /api/v1/projects/{id}` als grobe Save/Load-Basis.
- Zusätzlich zentrale Create/Update-Endpunkte für die wichtigsten UI-Aktionen.
- `POST /api/v1/projects/{id}/validate` als fachlicher Hauptendpunkt für `Check Constraints`.
- OCL-Hilfsendpunkte optional, aber sinnvoll für Editor-Feedback.

## Ressourcenmodell

```text
Project
├─ UmlModel
│  ├─ UmlClass
│  │  ├─ UmlAttribute
│  │  └─ UmlOperation
│  ├─ UmlAssociation
│  │  └─ UmlAssociationEnd
│  └─ UmlInvariant
├─ ObjectModel / Snapshot
│  ├─ ObjectInstance
│  │  └─ Slot
│  └─ ObjectLink
└─ LayoutInformation
```

| Ressource | API-Bezug | Zweck |
|---|---|---|
| `Project` | `/projects` | Speichereinheit für UML-Modell, Snapshot, Invarianten und Layout. |
| `UmlClass` | `/projects/{projectId}/classes` | Klassen im Klassendiagramm. |
| `UmlAttribute` | `/projects/{projectId}/classes/{classId}/attributes` | Attribute einer Klasse. |
| `UmlOperation` | `/projects/{projectId}/classes/{classId}/operations` | Operationen als Signaturen. |
| `UmlAssociation` | `/projects/{projectId}/associations` | Associations mit Rollen und Multiplizitäten. |
| `UmlInvariant` | `/projects/{projectId}/invariants` | OCL-Invarianten mit Kontextklasse. |
| `ObjectInstance` | `/projects/{projectId}/objects` | Objekte im aktuellen Snapshot. |
| `ObjectLink` | `/projects/{projectId}/links` | Konkrete Links zwischen Objekten. |
| `OCL` | `/projects/{projectId}/ocl/*` | Parse, Typecheck, Evaluation für OCL-Ausdrücke. |
| `ModelText` | `/projects/{projectId}/model-text/*` | Vollständiger USE-ähnlicher Editor-Text und Apply-Flow für unterstütztes Subset. |
| `ValidationResult` | `/projects/{projectId}/validate` | Ergebnis von `Check Constraints`. |

## Projekt-Endpunkte

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `POST` | `/projects` | Neues Projekt anlegen. | Ja |
| `GET` | `/projects/{projectId}` | Projekt laden. | Ja |
| `PUT` | `/projects/{projectId}` | Vollständigen Projektzustand speichern/ersetzen. | Ja |
| `PATCH` | `/projects/{projectId}` | Projektmetadaten ändern. | Should |
| `GET` | `/projects` | Projektliste für All-Projects-Seite abrufen. | Should |
| `DELETE` | `/projects/{projectId}` | Projekt löschen. | Optional |
| `GET` | `/projects/{projectId}/export` | Projekt als JSON exportieren. | Ja |
| `POST` | `/projects/import` | Projekt aus JSON importieren. | Ja |
| `GET` | `/projects/{projectId}/model-text` | Aktuellen USE-ähnlichen Modelltext für den Editor abrufen. | Ja |
| `POST` | `/projects/{projectId}/model-text/apply` | Vollständigen Editor-Text anwenden und unterstütztes Subset ins Domain-Modell übernehmen. | Ja |

### Projekt anlegen

```http
POST /api/v1/projects
Content-Type: application/json
```

```json
{
  "name": "Library Example",
  "description": "MVP model for UML/OCL validation"
}
```

Response:

```json
{
  "id": "project-library",
  "name": "Library Example",
  "description": "MVP model for UML/OCL validation",
  "formatVersion": "0.1",
  "umlModel": {
    "id": "uml-library",
    "classes": [],
    "associations": [],
    "invariants": []
  },
  "objectModel": {
    "id": "snapshot-current",
    "name": "Current Snapshot",
    "objects": [],
    "links": []
  },
  "layout": {}
}
```

### Projektliste abrufen

`19-projects.png` zeigt eine All-Projects-Seite mit Suche, Filter, Projektkarten, `Open` und `+ New Project`. Dafür braucht das Frontend eine schlanke Projektliste, ohne vollständiges UML-/Snapshot-Modell zu laden.

```http
GET /api/v1/projects?search=Library
Accept: application/json
```

```json
[
  {
    "id": "project-library",
    "name": "Library Catalog",
    "description": "Book borrowing and returning process",
    "updatedAt": "2026-07-08T10:30:00Z"
  }
]
```

MVP-/Should-Entscheidung:

| Thema | Entscheidung |
|---|---|
| `GET /projects` | Should, weil `View all` im Dashboard und die All-Projects-Seite sichtbar geplant sind. |
| Response | `ProjectSummaryDto[]` reicht zunächst aus. |
| Suche | Query-Parameter `search` kann vorbereitet werden; clientseitige Suche ist für kleine Listen akzeptabel. |
| Filter/Sortierung/Pagination | Post-MVP, falls viele Projekte oder Nutzerkontext hinzukommen. |

### Projekt speichern

```http
PUT /api/v1/projects/project-library
Content-Type: application/json
```

```json
{
  "id": "project-library",
  "formatVersion": "0.1",
  "name": "Library Example",
  "umlModel": {
    "classes": [],
    "associations": [],
    "invariants": []
  },
  "objectModel": {
    "objects": [],
    "links": []
  },
  "layout": {
    "classDiagram": {
      "nodes": [],
      "edges": []
    },
    "objectDiagram": {
      "nodes": [],
      "edges": []
    }
  }
}
```

## UML-Modell-Endpunkte

## Modelltext-Endpunkte

Der OCL Editor zeigt laut Screenshot `13-ocl-editor.png` vollständigen Modelltext, nicht nur einzelne Invarianten. Der Backend-MVP soll deshalb einen Apply-Flow unterstützen, der einen ganzen USE-ähnlichen Text entgegennimmt, das unterstützte Subset in das eigene Projektmodell überführt und nicht unterstützte Konstrukte als Diagnostics zurückgibt.

Wichtig: Dieser Flow ist kein Versprechen vollständiger USE-Kompatibilität. JSON bleibt das persistente MVP-Projektformat; der Modelltext ist eine Editor-Quelle beziehungsweise eine aus dem Projekt ableitbare Textrepräsentation.

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `GET` | `/projects/{projectId}/model-text` | Vollständigen Modelltext für den OCL Editor laden. | Ja |
| `POST` | `/projects/{projectId}/model-text/apply` | Editor-Text oder lokalen `.use` Dateiinhalt anwenden und unterstütztes Subset ins Projektmodell übernehmen. | Ja |

```http
POST /api/v1/projects/project-library/model-text/apply
Content-Type: application/json
```

```json
{
  "modelText": "model Library\n\nclass Book\nattributes\n  title : String\nend\n\nclass User\nattributes\n  books : Integer\nend\n\nassociation Borrows between\n  User[0..1] role borrower\n  Book[0..*] role borrowedBooks\nend\n\nconstraints\ncontext User inv maxBooks:\n  self.books <= 5\n",
  "sourceName": "Library.use",
  "sourceFormat": "use",
  "sourceOrigin": "open-existing",
  "mode": "REPLACE_SUPPORTED_MODEL",
  "includeDiagnostics": true
}
```

Response:

```json
{
  "status": "APPLIED_WITH_WARNINGS",
  "project": {
    "id": "project-library",
    "name": "Library",
    "formatVersion": "0.1",
    "umlModel": {},
    "objectModel": {},
    "layout": {}
  },
  "diagnostics": [
    {
      "code": "UNSUPPORTED_SYNTAX",
      "severity": "WARNING",
      "message": "Import statements are not supported in the MVP model text parser.",
      "sourceRange": {
        "startLine": 1,
        "startColumn": 1,
        "endLine": 1,
        "endColumn": 20
      }
    }
  ]
}
```

MVP-Subset für `model-text/apply`:

| Konstrukt | MVP |
|---|---|
| `model` | Ja |
| `class` mit `attributes` | Ja |
| `operations` als Signaturen | Should |
| binäre `association ... between` | Ja |
| Rollen und Multiplizitäten | Ja |
| `constraints`, `context`, `inv` | Ja |
| `import` | Diagnose/ignorieren, vollständige Verarbeitung Post-MVP |
| Vererbung | Diagnose oder später prüfen |
| `associationclass` | Post-MVP |

### Klassen

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `POST` | `/projects/{projectId}/classes` | Klasse erstellen. | Ja |
| `PUT` | `/projects/{projectId}/classes/{classId}` | Klasse vollständig ersetzen. | Ja |
| `PATCH` | `/projects/{projectId}/classes/{classId}` | Klasse teilweise ändern. | Should |
| `DELETE` | `/projects/{projectId}/classes/{classId}` | Klasse löschen und abhängige Elemente nach MVP-Cascade-Regeln bereinigen. | Ja |

```http
POST /api/v1/projects/project-library/classes
Content-Type: application/json
```

```json
{
  "name": "User"
}
```

Response:

```json
{
  "id": "class-user",
  "name": "User",
  "attributes": [],
  "operations": []
}
```

### Attribute

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `POST` | `/projects/{projectId}/classes/{classId}/attributes` | Attribut erstellen. | Ja |
| `PUT` | `/projects/{projectId}/classes/{classId}/attributes/{attributeId}` | Attribut ändern. | Ja |
| `DELETE` | `/projects/{projectId}/classes/{classId}/attributes/{attributeId}` | Attribut löschen und Slots dieses Attributs entfernen. | Ja |

```http
POST /api/v1/projects/project-library/classes/class-user/attributes
Content-Type: application/json
```

```json
{
  "name": "name",
  "type": "String"
}
```

Response:

```json
{
  "id": "attr-user-name",
  "name": "name",
  "type": "String"
}
```

### Operationen

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `POST` | `/projects/{projectId}/classes/{classId}/operations` | Operationensignatur erstellen. | Ja |
| `PUT` | `/projects/{projectId}/classes/{classId}/operations/{operationId}` | Operationensignatur ändern. | Ja |
| `DELETE` | `/projects/{projectId}/classes/{classId}/operations/{operationId}` | Operation löschen. | Ja |

```json
{
  "name": "canBorrow",
  "parameters": [
    {
      "name": "book",
      "type": "Book"
    }
  ],
  "returnType": "Boolean"
}
```

### Assoziationen

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `POST` | `/projects/{projectId}/associations` | Association erstellen. | Ja |
| `PUT` | `/projects/{projectId}/associations/{associationId}` | Association ändern. | Ja |
| `DELETE` | `/projects/{projectId}/associations/{associationId}` | Association löschen und zugehörige Objektlinks entfernen. | Ja |

```http
POST /api/v1/projects/project-library/associations
Content-Type: application/json
```

```json
{
  "name": "Borrows",
  "ends": [
    {
      "classId": "class-user",
      "roleName": "borrower",
      "multiplicity": {
        "lower": 0,
        "upper": 1,
        "unbounded": false
      },
      "navigable": true
    },
    {
      "classId": "class-book",
      "roleName": "borrowedBooks",
      "multiplicity": {
        "lower": 0,
        "upper": 5,
        "unbounded": false
      },
      "navigable": true
    }
  ]
}
```

### Invarianten

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `POST` | `/projects/{projectId}/invariants` | Invariante erstellen. | Ja |
| `PUT` | `/projects/{projectId}/invariants/{invariantId}` | Invariante ändern. | Ja |
| `DELETE` | `/projects/{projectId}/invariants/{invariantId}` | Invariante löschen und veraltete Validation Targets bereinigen. | Ja |

```http
POST /api/v1/projects/project-library/invariants
Content-Type: application/json
```

```json
{
  "name": "maxBooks",
  "contextClassId": "class-user",
  "expression": "self.borrowedBooks->size() <= 5",
  "enabled": true
}
```

Response:

```json
{
  "id": "inv-user-max-books",
  "name": "maxBooks",
  "contextClassId": "class-user",
  "expression": {
    "id": "expr-user-max-books",
    "text": "self.borrowedBooks->size() <= 5"
  },
  "enabled": true
}
```

## Objektmodell-Endpunkte

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `GET` | `/projects/{projectId}/objects` | Objekte im aktuellen Snapshot abrufen. | Optional |
| `POST` | `/projects/{projectId}/objects` | Objekt erstellen. | Ja |
| `PUT` | `/projects/{projectId}/objects/{objectId}` | Objekt ändern. | Ja |
| `DELETE` | `/projects/{projectId}/objects/{objectId}` | Objekt löschen und zugehörige Slots sowie Objektlinks entfernen. | Ja |
| `PUT` | `/projects/{projectId}/objects/{objectId}/slots/{slotId}` | Slot-Wert setzen. | Ja |
| `POST` | `/projects/{projectId}/links` | Objektlink erstellen. | Ja |
| `PUT` | `/projects/{projectId}/links/{linkId}` | Objektlink ändern. | Should |
| `DELETE` | `/projects/{projectId}/links/{linkId}` | Objektlink löschen. | Ja |

### Objekt erstellen

```http
POST /api/v1/projects/project-library/objects
Content-Type: application/json
```

```json
{
  "name": "alice",
  "classId": "class-user"
}
```

Response:

```json
{
  "id": "obj-alice",
  "name": "alice",
  "classId": "class-user",
  "slots": [
    {
      "id": "slot-alice-name",
      "attributeId": "attr-user-name",
      "value": null,
      "isUnset": true
    }
  ]
}
```

### Slot-Wert setzen

```http
PUT /api/v1/projects/project-library/objects/obj-alice/slots/slot-alice-name
Content-Type: application/json
```

```json
{
  "attributeId": "attr-user-name",
  "value": {
    "type": "String",
    "value": "Alice"
  }
}
```

### Objektlink erstellen

```http
POST /api/v1/projects/project-library/links
Content-Type: application/json
```

```json
{
  "associationId": "assoc-borrows",
  "sourceObjectId": "obj-alice",
  "targetObjectId": "obj-mobydick"
}
```

Response:

```json
{
  "id": "link-alice-mobydick",
  "associationId": "assoc-borrows",
  "sourceObjectId": "obj-alice",
  "targetObjectId": "obj-mobydick"
}
```

## OCL-Endpunkte

OCL-Endpunkte unterstützen Editor-Feedback und gezielte Entwicklung der OCL Engine. Sie ersetzen nicht den vollständigen Constraint Check.

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `POST` | `/projects/{projectId}/ocl/parse` | OCL-Ausdruck syntaktisch prüfen. | Should |
| `POST` | `/projects/{projectId}/ocl/typecheck` | OCL-Ausdruck mit Kontextklasse typprüfen. | Should |
| `POST` | `/projects/{projectId}/ocl/evaluate` | OCL-Ausdruck gegen Snapshot und Objekt auswerten. | Later/Debug |
| `GET` | `/ocl/profile` | Versioniertes OCL-2.4-Subset-Profil, Featurestatus und Runtime-Limits abrufen. | Post-MVP/Implemented |

Der vollständige Pfad des projektunabhängigen Profilendpunkts lautet
`GET /api/v1/ocl/profile`. Die Antwort verwendet `OclComplianceProfileDto` und
unterscheidet `SUPPORTED`, `PARTIAL`, `NOT_SUPPORTED` und `OUT_OF_SCOPE`.

### Parse

```http
POST /api/v1/projects/project-library/ocl/parse
Content-Type: application/json
```

```json
{
  "expression": "self.borrowedBooks->size() <= 5"
}
```

Response:

```json
{
  "status": "OK",
  "diagnostics": [],
  "astPreview": {
    "kind": "BinaryExpression",
    "operator": "<="
  }
}
```

### Typecheck

```json
{
  "contextClassId": "class-user",
  "expression": "self.borrowedBooks->size() <= 5"
}
```

Response:

```json
{
  "status": "OK",
  "resultType": "Boolean",
  "diagnostics": []
}
```

### Evaluate

```json
{
  "contextClassId": "class-user",
  "selfObjectId": "obj-alice",
  "expression": "self.books <= 5",
  "includeTrace": true
}
```

Response:

```json
{
  "status": "OK",
  "value": {
    "type": "Boolean",
    "value": false
  },
  "diagnostics": [],
  "trace": [
    {
      "expressionText": "self.books",
      "resultType": "Integer",
      "resultPreview": "6",
      "objectIds": ["obj-alice"]
    }
  ]
}
```

## Validierungs-Endpunkte

| Methode | Pfad | Zweck | MVP |
|---|---|---|---|
| `POST` | `/projects/{projectId}/validate` | Vollständige Constraint-Prüfung für gespeicherten Projektzustand. | Ja |
| `POST` | `/validation/check` | Vollständige Prüfung für übergebenen Projektzustand. | Optional |

```http
POST /api/v1/projects/project-library/validate
Content-Type: application/json
```

```json
{
  "scope": "FULL",
  "includeDebugInfo": false
}
```

Response ist ein `ValidationResultDto`.

## Fehlerformat

Technische API-Fehler und fachliche Validation Results müssen getrennt werden.

| Situation | Antworttyp |
|---|---|
| Projekt nicht gefunden | HTTP-Fehler mit `ApiErrorResponse`. |
| Request JSON ungültig | HTTP-Fehler mit `ApiErrorResponse`. |
| Create-Kommando verletzt harte API-Regel | HTTP-Fehler oder Domain Error. |
| Constraint ist verletzt | `ValidationResult` mit `INVARIANT_VIOLATION`. |
| OCL-Ausdruck hat Typfehler beim Validate | `ValidationResult` mit `TYPE_ERROR`. |

Empfohlenes technisches Fehlerformat:

```json
{
  "error": {
    "code": "PROJECT_NOT_FOUND",
    "message": "Project 'project-unknown' was not found.",
    "requestId": "req-123",
    "details": {
      "projectId": "project-unknown"
    }
  }
}
```

Typische HTTP-Statuscodes:

| Status | Verwendung |
|---|---|
| `200 OK` | Erfolgreiches Lesen, Speichern, Validieren. |
| `201 Created` | Neue Ressource erstellt. |
| `204 No Content` | Ressource gelöscht. |
| `400 Bad Request` | Ungültiger Request-Body oder Formatfehler. |
| `404 Not Found` | Projekt oder Ressource existiert nicht. |
| `409 Conflict` | Änderung kollidiert mit aktuellem Zustand. |
| `422 Unprocessable Entity` | Request ist syntaktisch korrekt, aber fachlich nicht akzeptierbar. |
| `500 Internal Server Error` | Unerwarteter technischer Fehler. |

## Validation Result Response

`POST /api/v1/projects/{projectId}/validate` liefert ein strukturiertes Ergebnis.

```json
{
  "id": "validation-library-001",
  "projectId": "project-library",
  "umlModelId": "uml-library",
  "objectModelId": "snapshot-current",
  "status": "INVALID",
  "checkedAt": "2026-07-11T20:30:00Z",
  "summary": {
    "errorCount": 1,
    "warningCount": 0,
    "infoCount": 0
  },
  "errors": [
    {
      "id": "error-invariant-max-books-alice",
      "code": "INVARIANT_VIOLATION",
      "severity": "ERROR",
      "phase": "OCL_EVALUATION",
      "message": "Object 'alice' violates invariant 'maxBooks'.",
      "modelElementIds": ["class-user"],
      "objectIds": ["obj-alice"],
      "linkIds": [],
      "slotIds": [],
      "invariantId": "inv-user-max-books",
      "sourceRange": {
        "expressionId": "expr-user-max-books",
        "startLine": 1,
        "startColumn": 1,
        "endLine": 1,
        "endColumn": 34
      },
      "details": {
        "contextClass": "User",
        "invariantName": "maxBooks",
        "expression": "self.books <= 5"
      }
    }
  ]
}
```

Statuswerte:

| Status | Bedeutung |
|---|---|
| `VALID` | Keine Errors. |
| `INVALID` | Mindestens ein fachlicher Fehler. |
| `NOT_EVALUABLE` | Prüfung konnte nicht vollständig durchgeführt werden. |

## DTO-Übersicht

| DTO | Zweck | Wichtigste Felder |
|---|---|---|
| `ProjectDto` | Vollständiger Projektzustand. | `id`, `name`, `formatVersion`, `umlModel`, `objectModel`, `layout` |
| `ProjectSummaryDto` | Projektliste. | `id`, `name`, `updatedAt` |
| `UmlModelDto` | Klassenmodell. | `classes`, `associations`, `invariants` |
| `UmlClassDto` | Klasse. | `id`, `name`, `attributes`, `operations` |
| `UmlAttributeDto` | Attribut. | `id`, `name`, `type` |
| `UmlOperationDto` | Operationensignatur. | `id`, `name`, `parameters`, `returnType` |
| `UmlAssociationDto` | Association. | `id`, `name`, `ends` |
| `UmlAssociationEndDto` | Association End. | `id`, `classId`, `roleName`, `multiplicity`, `navigable` |
| `MultiplicityDto` | Multiplizität. | `lower`, `upper`, `unbounded` |
| `UmlInvariantDto` | OCL-Invariante. | `id`, `name`, `contextClassId`, `expression`, `enabled` |
| `ObjectModelDto` | Snapshot. | `id`, `name`, `objects`, `links` |
| `ObjectInstanceDto` | Objekt. | `id`, `name`, `classId`, `slots` |
| `SlotDto` | Attributwert. | `id`, `attributeId`, `value`, `isUnset` |
| `ObjectLinkDto` | Objektlink. | `id`, `associationId`, `sourceObjectId`, `targetObjectId` |
| `ModelTextDto` | Textrepräsentation für OCL Editor. | `projectId`, `modelText`, `updatedAt` |
| `ApplyModelTextRequestDto` | Vollständigen Editor-Text anwenden. | `modelText`, `mode`, `includeDiagnostics` |
| `ApplyModelTextResponseDto` | Ergebnis des Apply-Flows. | `status`, `project`, `diagnostics` |
| `OclParseResponseDto` | Parserfeedback. | `status`, `diagnostics`, `astPreview` |
| `OclTypecheckResponseDto` | Typecheckerfeedback. | `status`, `resultType`, `diagnostics` |
| `OclEvaluateResponseDto` | Auswertungsergebnis. | `status`, `value`, `diagnostics`, `trace` |
| `OclComplianceProfileDto` | Maschinenlesbares OCL-2.4-Subset-Profil. | `profileId`, `oclVersion`, `complianceClaim`, `apiVersion`, `enabledOptionalCompliancePoints`, `features`, `runtimeLimits` |
| `ValidationResultDto` | Constraint-Check-Ergebnis. | `status`, `summary`, `errors` |
| `ValidationErrorDto` | einzelner Validierungsfehler. | `code`, `severity`, `message`, IDs, `details` |
| `ApiErrorResponse` | technischer API-Fehler. | `code`, `message`, `requestId`, `details` |

DTO-Prinzipien:

- IDs werden als Strings transportiert.
- Multiplicity nutzt strukturierte Felder, nicht nur UI-Text.
- Werte tragen immer einen Typ.
- Layout referenziert Domain-IDs, dupliziert aber keine Semantik.
- DTOs sollten OpenAPI-generierbar sein.

## Beispiel: Library-Workflow über API

### 1. Projekt anlegen

```http
POST /api/v1/projects
```

```json
{
  "name": "Library Example"
}
```

### 2. Klassen erstellen

```http
POST /api/v1/projects/project-library/classes
```

```json
{
  "name": "User"
}
```

```http
POST /api/v1/projects/project-library/classes
```

```json
{
  "name": "Book"
}
```

### 3. Attribute erstellen

```http
POST /api/v1/projects/project-library/classes/class-user/attributes
```

```json
{
  "name": "books",
  "type": "Integer"
}
```

```http
POST /api/v1/projects/project-library/classes/class-user/attributes
```

```json
{
  "name": "name",
  "type": "String"
}
```

### 4. Association erstellen

```http
POST /api/v1/projects/project-library/associations
```

```json
{
  "name": "Borrows",
  "ends": [
    {
      "classId": "class-user",
      "roleName": "borrower",
      "multiplicity": {
        "lower": 0,
        "upper": 1,
        "unbounded": false
      },
      "navigable": true
    },
    {
      "classId": "class-book",
      "roleName": "borrowedBooks",
      "multiplicity": {
        "lower": 0,
        "upper": 5,
        "unbounded": false
      },
      "navigable": true
    }
  ]
}
```

### 5. Invariante erstellen

```http
POST /api/v1/projects/project-library/invariants
```

```json
{
  "name": "maxBooks",
  "contextClassId": "class-user",
  "expression": "self.books <= 5",
  "enabled": true
}
```

### 6. Objekt erstellen und Slot setzen

```http
POST /api/v1/projects/project-library/objects
```

```json
{
  "name": "alice",
  "classId": "class-user"
}
```

```http
PUT /api/v1/projects/project-library/objects/obj-alice/slots/slot-alice-books
```

```json
{
  "attributeId": "attr-user-books",
  "value": {
    "type": "Integer",
    "value": 6
  }
}
```

### 7. Constraints prüfen

```http
POST /api/v1/projects/project-library/validate
```

```json
{
  "scope": "FULL"
}
```

Erwartung:

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
      "code": "INVARIANT_VIOLATION",
      "objectIds": ["obj-alice"],
      "invariantId": "inv-user-max-books"
    }
  ]
}
```

## MVP-Endpunkte

Für den MVP sind diese Endpunkte ausreichend:

| Methode | Pfad | Grund |
|---|---|---|
| `POST` | `/projects` | Projekt starten. |
| `GET` | `/projects/{projectId}` | Projekt laden. |
| `PUT` | `/projects/{projectId}` | Projekt speichern. |
| `GET` | `/projects/{projectId}/export` | JSON Export. |
| `POST` | `/projects/import` | JSON Import. |
| `GET` | `/projects/{projectId}/model-text` | Modelltext für OCL Editor laden. |
| `POST` | `/projects/{projectId}/model-text/apply` | Vollständigen Editor-Text mit MVP-Subset anwenden. |
| `POST` | `/projects/{projectId}/classes` | Klasse erstellen. |
| `PUT` | `/projects/{projectId}/classes/{classId}` | Klasse bearbeiten. |
| `POST` | `/projects/{projectId}/classes/{classId}/attributes` | Attribut erstellen. |
| `PUT` | `/projects/{projectId}/classes/{classId}/attributes/{attributeId}` | Attribut bearbeiten. |
| `POST` | `/projects/{projectId}/classes/{classId}/operations` | Operationensignatur erstellen. |
| `PUT` | `/projects/{projectId}/classes/{classId}/operations/{operationId}` | Operationensignatur bearbeiten. |
| `POST` | `/projects/{projectId}/associations` | Association erstellen. |
| `PUT` | `/projects/{projectId}/associations/{associationId}` | Association bearbeiten. |
| `POST` | `/projects/{projectId}/invariants` | Invariante erstellen. |
| `PUT` | `/projects/{projectId}/invariants/{invariantId}` | Invariante bearbeiten. |
| `POST` | `/projects/{projectId}/objects` | Objekt erstellen. |
| `PUT` | `/projects/{projectId}/objects/{objectId}` | Objekt bearbeiten. |
| `PUT` | `/projects/{projectId}/objects/{objectId}/slots/{slotId}` | Attributwert setzen. |
| `POST` | `/projects/{projectId}/links` | Objektlink erstellen. |
| `POST` | `/projects/{projectId}/validate` | Constraint Check ausführen. |

OCL-Hilfsendpunkte sind für den MVP nützlich, aber nicht zwingend, wenn `validate` alle OCL-Fehler zurückliefert:

| Methode | Pfad | MVP-Einstufung |
|---|---|---|
| `POST` | `/projects/{projectId}/ocl/parse` | Should |
| `POST` | `/projects/{projectId}/ocl/typecheck` | Should |
| `POST` | `/projects/{projectId}/ocl/evaluate` | Later/Debug |

## Post-MVP-Endpunkte

| Endpunkt | Zweck |
|---|---|
| `GET /api/v1/projects?sort=&filter=&page=` | Erweiterte Projektliste mit serverseitiger Suche, Filterung, Sortierung und Pagination. |
| `DELETE /api/v1/projects/{projectId}` | Projekt löschen. |
| `POST /api/v1/projects/{projectId}/snapshots` | Mehrere Snapshots erstellen. |
| `GET /api/v1/projects/{projectId}/snapshots/{snapshotId}` | Bestimmten Snapshot laden. |
| `POST /api/v1/projects/{projectId}/use/import` | Vollständiger `.use` Import. |
| `GET /api/v1/projects/{projectId}/use/export` | `.use` Export. |
| `POST /api/v1/projects/{projectId}/ocl/format` | OCL-Formatierung. |
| `POST /api/v1/projects/{projectId}/ocl/completions` | OCL Autocomplete. |
| `GET /api/v1/projects/{projectId}/versions` | Projektversionen anzeigen. |
| `POST /api/v1/projects/{projectId}/versions/{versionId}/restore` | Projektversion wiederherstellen. |
| `POST /api/v1/projects/{projectId}/layout/auto` | Layoutvorschlag berechnen. |

## Versionierung

Zwei Versionierungen sind zu unterscheiden:

| Version | Zweck |
|---|---|
| API-Version | Stabilität des HTTP-Vertrags. |
| Projektformat-Version | Migration gespeicherter Projekte. |

Empfehlung:

| Bereich | MVP-Entscheidung |
|---|---|
| API-Prefix | `/api/v1` für echte Implementierung vorsehen. |
| Dokumentationspfade | Zur Lesbarkeit ohne Prefix notieren, z. B. `/projects`. |
| Projektformat | `formatVersion: "0.1"` im JSON. |
| Breaking Changes | neue API-Version oder klarer Migrationsschritt. |

Beispiel:

```http
POST /api/v1/projects/project-library/validate
```

## Bezug zum Frontend

Das React/TypeScript-Frontend nutzt die API für interaktive Modellierung und Validierung.

| Frontend-Bereich | Benötigte API |
|---|---|
| Explorer Sidebar | `GET /api/v1/projects/{id}` und Modell-DTOs. |
| Class Diagram Canvas | Klassen, Associations, Invarianten, Layout. |
| Class Properties Panel | Klassen-, Attribut- und Operation-Endpunkte. |
| Association Properties Panel | Association-Endpunkte. |
| Invariant Modal/Panel | Invariant- und OCL-Endpunkte. |
| Object Diagram Canvas | Objekte, Links, Layout. |
| Object Properties Panel | Objekt- und Slot-Endpunkte. |
| Check Constraints Button | `POST /api/v1/projects/{id}/validate`. |
| Validation Results Panel | `ValidationResultDto`. |
| OCL Editor | `model-text/apply` für vollständigen Modelltext; optional `ocl/parse` und `ocl/typecheck` für Ausdrucksfeedback. |

Frontend-Vertragsregeln:

- Frontend hält UI-State und Layout, Backend hält fachliche Semantik.
- Frontend kann optimistisch editieren, muss aber Backend-Fehler darstellen.
- Validation Results ersetzen frühere Ergebnisse pro Lauf.
- Alle Diagrammelemente müssen über IDs auf DTOs abbildbar sein.
- TypeScript-Typen sollten aus OpenAPI-Spezifikation generierbar sein.

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Nutzt der MVP primär grobes `PUT /api/v1/projects/{id}` oder feingranulare Mutationsendpunkte? | Beeinflusst Frontend-State-Management. |
| Werden ungültige fachliche Zwischenzustände gespeichert oder bei CRUD abgelehnt? | Beeinflusst UX und ValidationResult-Nutzung. |
| Soll `PATCH` oder `PUT` für Bearbeitungen bevorzugt werden? | Beeinflusst DTO-Design. |
| Werden Layoutdaten mit jedem `PUT /api/v1/projects/{id}` gespeichert oder separat? | Beeinflusst Performance und Konflikte. |
| Sollen OCL-Hilfsendpunkte im MVP verpflichtend sein? | Beeinflusst OCL Editor Feedback. |
| Wie strikt soll `model-text/apply` bei nicht unterstützter `.use`-Syntax sein? | Empfehlung: unterstütztes Subset anwenden, nicht unterstützte Konstrukte als Diagnostics melden. |
| Wie werden IDs erzeugt: Backend-only oder optional vom Frontend vorgeschlagen? | Beeinflusst Offline-/Optimistic-UI. |
| Wird Optimistic Locking mit `revision` benötigt? | Für MVP vermutlich nicht, später relevant. |
| Werden API-DTOs per OpenAPI generiert? | Empfehlung: ja, damit Frontend und Backend synchron bleiben. |

## Zusammenfassung

Die REST-API des neuen Backends stellt dem React/TypeScript-Frontend einen klaren Vertrag für Projektverwaltung, UML-Modellierung, Snapshot-Bearbeitung, OCL-Prüfung und Constraint-Validierung bereit.

Für den MVP sind Projekt-Load/Save, `model-text/apply` für den vollständigen OCL-Editor-Text, CRUD für Klassen, Attribute, Operationen, Associations, Invarianten, Objekte und Links sowie `POST /api/v1/projects/{projectId}/validate` zentral. OCL-Parse- und Typecheck-Endpunkte sind als Editor-Unterstützung sinnvoll. Die API bleibt bewusst JSON-basiert, DTO-orientiert und unabhängig vom originalen USE-Projekt.

## B48-Command-Endpunkte

| Methode | Pfad | Zweck |
|---|---|---|
| `POST` | `/api/v1/projects/{projectId}/commands/associations/{associationId}/association-class` | Association Class mitsamt Features erzeugen und atomar binden |
| `POST` | `/api/v1/projects/{projectId}/commands/object-model/association-class-instances` | Object Link, Linkobjekt und Slots atomar erzeugen |
| `PUT` | `/api/v1/projects/{projectId}/commands/object-model/association-class-instances/{linkId}` | Object Link, Linkobjekt und Slots atomar ersetzen |

Alle Endpunkte verlangen `expectedRevision`, speichern bei Erfolg genau einmal
und liefern `MutationResultDto` mit autoritativer Aggregateprojektion. Die
bestehenden Einzelendpunkte bleiben zur API-v1-Kompatibilitaet bestehen; fuer
den gekoppelten F6-Workflow gelten die Aggregate-Endpunkte.
