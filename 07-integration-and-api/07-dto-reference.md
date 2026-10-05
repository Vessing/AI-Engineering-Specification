# DTO Reference

## Zweck dieser Datei

Diese Datei ist die zentrale Referenz fuer Data Transfer Objects (DTOs) zwischen dem neuen React/TypeScript-Frontend und dem neuen Java/Spring-Boot-Backend.

Sie beschreibt:

- welche Daten zwischen Frontend und Backend ausgetauscht werden,
- welche DTOs fuer den MVP benoetigt werden,
- welche DTOs stabile IDs fuer UI-Mapping und Validierung tragen,
- welche Daten fachliche Semantik enthalten,
- welche Daten nur fuer UI-Layout verwendet werden,
- wie OCL-, Validierungs- und Fehlerergebnisse strukturiert uebertragen werden.

Die DTOs sind nicht als interne Implementierungsklassen zu verstehen. Backend-Domain-Modelle, Frontend-State und API-DTOs sollen getrennt bleiben.

## DTO-Prinzipien

| Prinzip | Bedeutung |
|---|---|
| Stabile IDs | Jedes fachliche Element, das im Frontend selektiert, gespeichert oder markiert werden kann, besitzt eine stabile ID. |
| Klare Schichten | DTOs transportieren Daten ueber die API, ersetzen aber weder Backend-Domain-Modelle noch Frontend-View-State. |
| Backend als fachliche Quelle | UML-, OCL- und Validierungssemantik wird im Backend entschieden. |
| Frontend als UI-Quelle | Positionen, Selektion, offene Panels und rein visuelle Zustaende liegen im Frontend. Persistierbare Layoutdaten werden separat uebertragen. |
| Strukturierte Fehler | Fehler muessen maschinenlesbare Codes, Severity und Elementreferenzen enthalten. |
| Erweiterbarkeit | DTOs erhalten Versionsfelder oder optionale Erweiterungsfelder, damit Post-MVP-Funktionen ergaenzt werden koennen. |
| Keine direkte USE-Abhaengigkeit | DTOs koennen fachlich von USE-Konzepten inspiriert sein, bilden aber ein neues JSON/API-Modell. |

## ID-Konzept

Alle IDs werden als Strings uebertragen. Das konkrete Format kann UUID-basiert sein. Lesbare Praefixe sind fuer Debugging hilfreich, aber nicht fachlich verpflichtend.

| Elementtyp | Beispiel-ID | Verwendung |
|---|---:|---|
| Project | `project-library-demo` | Laden, Speichern, Recent Projects, API-Routen |
| Class | `class-user` | Class Node, Attribute, Invarianten, Typreferenzen |
| Attribute | `attr-user-books` | Properties Panel, Slot-Typpruefung, OCL-Fehler |
| Operation | `op-user-canBorrow` | Signaturanzeige, spaetere OCL-Aufrufe |
| Association | `assoc-borrows` | Class Edge, Object Links, Multiplizitaet |
| Association End | `assocend-borrows-user` | Rollen, Multiplicity Checks |
| Invariant | `inv-user-maxBooks` | OCL Editor, Validation Results |
| Object | `obj-alice` | Object Node, Slotwerte, Invariant-Kontext |
| Slot | `slot-alice-books` | Attributwert, Fehler-Mapping |
| Object Link | `link-alice-mobydick` | Object Edge, Linkvalidierung |
| Layout Node | `class-user` oder `obj-alice` | Referenziert das fachliche Element, enthaelt keine Semantik |

## DTO-Uebersicht

Die vorgeschlagenen DTOs sind grundsaetzlich passend. Ergaenzend werden `ProjectMetadataDto`, `ProjectSummaryDto`, `ElementTargetDto`, `ValidationSummaryDto`, `OclDiagnosticDto`, `SourceRangeDto`, `DiagramLayoutDto` und `ViewportDto` empfohlen, weil sie Dashboard, Fehler-Mapping, OCL-Feedback und Layout sauberer trennen.

| DTO | Kategorie | Zweck | MVP-Relevanz |
|---|---|---|---|
| `ProjectDto` | Projekt | Vollstaendiger Projektzustand | MVP |
| `ProjectMetadataDto` | Projekt | Name, Version, Zeitstempel, Beschreibung | MVP |
| `ProjectSummaryDto` | Projekt | Kurzinfo fuer Dashboard/Recent Projects | Should |
| `UmlModelDto` | UML | Klassenmodell mit Klassen, Assoziationen, Invarianten | MVP |
| `UmlClassDto` | UML | UML-Klasse | MVP |
| `UmlAttributeDto` | UML | Attribut einer Klasse | MVP |
| `UmlOperationDto` | UML | Operation als Signatur | MVP |
| `UmlParameterDto` | UML | Parameter einer Operation | MVP |
| `UmlAssociationDto` | UML | Association zwischen Klassen | MVP |
| `UmlAssociationEndDto` | UML | Rolle, Klasse und Multiplizitaet eines Association-Endes | MVP |
| `MultiplicityDto` | UML | Untere/obere Kardinalitaet | MVP |
| `UmlInvariantDto` | OCL | OCL-Invariante mit Kontextklasse | MVP |
| `ObjectModelDto` | Snapshot | Objektmodell/Snapshot | MVP |
| `ObjectInstanceDto` | Snapshot | Objektinstanz | MVP |
| `SlotDto` | Snapshot | Attributwert eines Objekts | MVP |
| `ObjectLinkDto` | Snapshot | Link zwischen Objekten ueber Association | MVP |
| `OclParseRequestDto` | OCL | Parse-Anfrage fuer OCL-Text | MVP |
| `OclParseResponseDto` | OCL | Parsergebnis mit AST-Zusammenfassung oder Fehlern | MVP |
| `OclTypecheckRequestDto` | OCL | Typecheck-Anfrage mit Kontextklasse | MVP |
| `OclTypecheckResponseDto` | OCL | Typecheck-Ergebnis mit Ergebnistyp oder Fehlern | MVP |
| `OclEvaluateRequestDto` | OCL | Evaluation eines Ausdrucks gegen Snapshot/Kontextobjekt | Should |
| `OclEvaluateResponseDto` | OCL | Evaluationsergebnis oder Evaluation Error | Should |
| `OclComplianceProfileDto` | OCL | Versioniertes, maschinenlesbares OCL-2.4-Subset-Profil | Post-MVP/Implemented |
| `OclDiagnosticDto` | OCL | Syntax-, Typecheck- oder Evaluation-Diagnostic | MVP |
| `ModelTextDto` | OCL/Import | Textuelle USE-/OCL-Modellrepräsentation aus der OCL Editor View | MVP/Should |
| `ApplyModelTextRequestDto` | OCL/Import | Apply-Request für geänderten Modell-/OCL-Text | MVP/Should |
| `ApplyModelTextResponseDto` | OCL/Import | Ergebnis von `Apply Changes` mit Projektzustand und Diagnosen | MVP/Should |
| `ValidationRequestDto` | Validation | Request fuer `Check Constraints` | MVP |
| `ValidationResultDto` | Validation | Gesamtergebnis der Validierung | MVP |
| `ValidationSummaryDto` | Validation | Anzahl Errors/Warnings/Infos | MVP |
| `ValidationErrorDto` | Validation | Einzelner Validierungsbefund mit UI-Mapping | MVP |
| `ElementTargetDto` | Validation | Referenz auf betroffenes Modell- oder UI-Element | MVP |
| `ApiErrorDto` | Error | Strukturierter API-Fehler ausserhalb fachlicher Validierung | MVP |
| `LayoutDto` | Layout | Persistierbare Diagrammpositionen | MVP |
| `DiagramLayoutDto` | Layout | Layout fuer eine Diagrammansicht | MVP |
| `NodeLayoutDto` | Layout | Position und Groesse eines Nodes | MVP |
| `EdgeLayoutDto` | Layout | Bendpoints und Labelposition einer Edge | Should |
| `ViewportDto` | Layout | Optionaler Zoom/Pan-Zustand | Later |

## Projekt-DTOs

| DTO | Zweck | Pflichtfelder | Optionale Felder | Beispiel | Frontend-Verwendung | Backend-Verwendung | MVP | Erweiterungen |
|---|---|---|---|---|---|---|---|---|
| `CreateProjectRequestDto` | Neues Projekt aus dem Dashboard anlegen | `name` | `description`, `templateId` | `{"name":"Library Model"}` | `CreateNewProjectModal`, Startflow | Project Service validiert und initialisiert Projekt | Ja | Templates, Projektbeschreibung |
| `ProjectDto` | Vollstaendiger Projektzustand | `formatVersion`, `project`, `umlModel`, `objectModel`, `layout` | `validationState`, `extensions` | `{"formatVersion":"1.0","project":{"id":"project-library-demo"}}` | Projektstate laden/speichern | Persistenz, Validierung | Ja | Mehrere Snapshots, Versionierung |
| `ProjectMetadataDto` | Projekt-Metadaten | `id`, `name` | `description`, `createdAt`, `updatedAt`, `sourceFormat` | `{"id":"project-library-demo","name":"Library"}` | Titel, Dashboard, Save Status | Formatpruefung, Migration | Ja | Autoren, Tags |
| `ProjectSummaryDto` | Kurzinfo fuer Listen | `id`, `name`, `updatedAt` | `description`, `thumbnail`, `sourceFormat` | `{"id":"project-library-demo","name":"Library"}` | Recent Projects, View all | Projektliste | Should | Suche, Sortierung |

Beispiel:

```json
{
  "formatVersion": "1.0",
  "project": {
    "id": "project-library-demo",
    "name": "Library",
    "description": "MVP-Beispielmodell fuer UML/OCL-Validierung",
    "sourceFormat": "json",
    "createdAt": "2026-07-15T10:00:00Z",
    "updatedAt": "2026-07-15T10:15:00Z"
  },
  "umlModel": {},
  "objectModel": {},
  "layout": {}
}
```

## UML-DTOs

| DTO | Zweck | Pflichtfelder | Optionale Felder | Beispiel | Frontend-Verwendung | Backend-Verwendung | MVP | Erweiterungen |
|---|---|---|---|---|---|---|---|---|
| `UmlModelDto` | Container fuer Klassenmodell | `classes`, `associations`, `invariants` | `enumerations`, `generalizations` | `{"classes":[],"associations":[]}` | Diagrammaufbau, Explorer | UML Model Service | Ja | Vererbung, Enums |
| `UmlClassDto` | UML-Klasse | `id`, `name`, `attributes`, `operations` | `abstractClass`, `superClassIds`, `visibility`, `packageId`, `qualifiedName` | `{"id":"class-user","name":"User","abstractClass":false,"superClassIds":[],"visibility":"PUBLIC"}` | Class Node, Properties | Typmodell, OCL-Kontext | Ja | B4-Generalisierung sowie B5-Sichtbarkeit und Namespace |
| `UmlPackageDto` | stabiles UML-Package | `id`, `qualifiedName` | keine | `{"id":"package-people","qualifiedName":"university::people"}` | Package Explorer | Namespace Resolver | Ja | B5 |
| `UmlModelImportDto` | gerichteter Package-Import | `id`, `importingPackageId`, `importedPackageId` | `alias`, `source`, `provenance` | `{"id":"import-core","importingPackageId":"package-people","importedPackageId":"package-core","alias":"shared"}` | Imports Explorer | Import Resolver | Ja | B5 |
| `UmlAttributeDto` | Attribut | `id`, `name`, `type` | `derived`, `deriveExpression`, `initExpression`, `visibility`, `redefinedAttributeIds`, `staticAttribute`, `classifierValue` | `{"id":"attr-next","name":"nextNumber","type":"Integer","staticAttribute":true,"classifierValue":{"type":"Integer","value":1043}}` | Attributliste, Redefinition, statische Werte | Typprüfung, Redefinition, classifierweiter Wert; statische Attribute erzeugen keinen Objekt-Slot | Ja | B38/B39 ergänzen Redefinitionsziele und statische Werte additiv. |
| `UmlOperationDto` | Operation als Signatur | `id`, `name`, `returnType`, `parameters` | `bodyExpression`, `visibility`, `abstractOperation`, `query`, `redefinedOperationIds` | `{"id":"op-user-display","name":"displayName","returnType":"String","redefinedOperationIds":["op-party-display"]}` | Operationsliste, Redefinition | Signatur, Dispatch und Invocation | Ja | B9/B19; B38 ergänzt stabile Redefinitionsziele. |
| `UmlParameterDto` | Operationsparameter | `id`, `name`, `type`, `direction`, `position` | keine | `{"id":"param-book","name":"book","type":"Book","direction":"IN","position":0}` | Operation Properties und Invocation | Parameterbindung und Signaturauflösung | Ja | B9 |
| `UmlAssociationDto` | Association | `id`, `name`, `ends` | `associationClassId` | `{"id":"assoc-enrollment","name":"EnrollmentLink","associationClassId":"class-enrollment"}` | Class Edge | Link-, Association-Class- und Multiplicity Checks | Ja | B8 |
| `UmlAssociationEndDto` | Association-Ende | `id`, `classId`, `roleName`, `multiplicity`, `navigable` | `ordered`, `unique`, `derived`, `union`, `subsettedEndIds`, `redefinedEndIds`, `navigationType`, `aggregationKind` | `{"classId":"class-folder","roleName":"whole","aggregationKind":"COMPOSITE"}` | Edge Labels, Properties | Navigation, Multiplizitaet, End-Metadaten und Ownership | Ja | B6/B8 |
| `MultiplicityDto` | Kardinalitaet | `lower`, `upper` | - | `{"lower":0,"upper":"*"}` | Anzeige und Eingabe | Multiplicity Check | Ja | Qualifier |
| `UmlInvariantDto` | Invariante | `id`, `name`, `contextClassId`, `expression` | `enabled`, `description`, `lastCheck` | `{"name":"maxBooks","expression":"self.books <= 5"}` | OCL Editor, Badges | Parser, Typechecker, Evaluator | Ja | pre/post, derived |

Beispiel fuer eine Association:

```json
{
  "id": "assoc-borrows",
  "name": "Borrows",
  "ends": [
    {
      "id": "assocend-borrows-user",
      "classId": "class-user",
      "roleName": "borrower",
      "multiplicity": { "lower": 0, "upper": "*" },
      "navigable": true
    },
    {
      "id": "assocend-borrows-book",
      "classId": "class-book",
      "roleName": "borrowedBooks",
      "multiplicity": { "lower": 0, "upper": "*" },
      "navigable": true
    }
  ]
}
```

## Objektmodell-DTOs

| DTO | Zweck | Pflichtfelder | Optionale Felder | Beispiel | Frontend-Verwendung | Backend-Verwendung | MVP | Erweiterungen |
|---|---|---|---|---|---|---|---|---|
| `ObjectModelDto` | Snapshot-Container | `id`, `objects`, `links` | `name` | `{"id":"snapshot-current","objects":[],"links":[]}` | Object Diagram | Snapshot-Validierung | Ja | Mehrere Snapshots |
| `ObjectInstanceDto` | Objektinstanz | `id`, `name`, `classId`, `slots` | `displayName` | `{"id":"obj-alice","name":"alice","classId":"class-user"}` | Object Node | OCL-Kontextobjekt | Ja | Object Lifecycle |
| `SlotDto` | Attributwert | `id`, `attributeId`, `value` | `valueType`, `isUnset` | `{"attributeId":"attr-user-books","value":6}` | Object Properties | Typvalidierung, Evaluation | Ja | Null/Undefined-Semantik |
| `ObjectLinkDto` | Objektlink | `id`, `associationId`, `endValues` | `associationClassObjectId` | `{"associationId":"assoc-enrollment","associationClassObjectId":"obj-enrollment-1"}` | Object Edge und Linkobjekt | Linkvalidierung, Navigation und gemeinsame Identität | Ja | B7/B8 |

Beispiel:

```json
{
  "id": "snapshot-current",
  "name": "Current Snapshot",
  "objects": [
    {
      "id": "obj-alice",
      "name": "alice",
      "classId": "class-user",
      "slots": [
        {
          "id": "slot-alice-books",
          "attributeId": "attr-user-books",
          "value": 6,
          "valueType": "Integer"
        }
      ]
    }
  ],
  "links": [
    {
      "id": "link-alice-mobydick",
      "associationId": "assoc-borrows",
      "endValues": [
        { "associationEndId": "assocend-borrows-user", "objectId": "obj-alice" },
        { "associationEndId": "assocend-borrows-book", "objectId": "obj-mobydick" }
      ]
    }
  ]
}
```

## OCL-DTOs

| DTO | Zweck | Pflichtfelder | Optionale Felder | Beispiel | Frontend-Verwendung | Backend-Verwendung | MVP | Erweiterungen |
|---|---|---|---|---|---|---|---|---|
| `OclParseRequestDto` | OCL-Text parsen | `expression` | `invariantId`, `contextClassId` | `{"expression":"self.books <= 5"}` | Live-Pruefung, Modal | Lexer/Parser | Ja | Vollstaendige OCL-Syntax |
| `OclParseResponseDto` | Parsergebnis | `valid`, `diagnostics` | `ast`, `normalizedExpression` | `{"valid":true}` | OCL Feedback | Parserantwort | Ja | AST-Visualisierung |
| `OclTypecheckRequestDto` | Ausdruck typpruefen | `expression`, `contextClassId` | `invariantId` | `{"contextClassId":"class-user"}` | OCL Editor | Typechecker | Ja | Imports, Libraries |
| `OclTypecheckResponseDto` | Typecheck-Ergebnis | `valid`, `diagnostics` | `resultType`, `resolvedReferences` | `{"valid":true,"resultType":"Boolean"}` | Inline-Feedback | Typechecker Result | Ja | Generics, Iteratorvariablen |
| `OclEvaluateRequestDto` | Ausdruck auswerten | `expression`, `contextObjectId`, `project` oder `projectId` | `invariantId`, `snapshotId` | `{"contextObjectId":"obj-alice"}` | Debug/Evaluation | Evaluator | Should | Batch Evaluation |
| `OclEvaluateResponseDto` | Evaluationsergebnis | `valid`, `diagnostics` | `value`, `valueType`, `trace` | `{"value":false,"valueType":"Boolean"}` | Debug, Details | Evaluation Result | Should | Trace UI |
| `OclComplianceProfileDto` | Unterstützten OCL-Umfang anzeigen | `profileId`, `oclVersion`, `complianceClaim`, `apiVersion`, `features`, `runtimeLimits` | `enabledOptionalCompliancePoints` | `{"profileId":"use-web-ocl-2.4-subset-v3"}` | spätere Featureanzeige im OCL Editor | `GET /api/v1/ocl/profile` | Post-MVP/Implemented | B33 versioniert die fachliche Profil-ID nach der Null-Gap-Abnahme auf v3; `apiVersion` und DTO-Struktur bleiben v1. Clients werten Featurestatus und Limits anhand stabiler Schlüssel aus. |
| `OclDiagnosticDto` | OCL-Fehler/Hinweis | `code`, `severity`, `message` | `range`, `target`, `technicalDetails` | `{"code":"TYPE_ERROR"}` | Editor-Marker | Parser/Typechecker/Evaluator | Ja | Quick Fixes |
| `ModelTextDto` | Textuellen Modell-/OCL-Stand transportieren | `projectId`, `modelText`, `format` | `version`, `source`, `sourceName`, `sourceOrigin`, `cursor`, `lineEnding` | `{"format":"USE_TEXT","sourceName":"Library.use","modelText":"model Library"}` | OCL Editor, Open Existing Modal | Model Text Parser/Import Service | Ja/Should | Syntax Highlighting, Formatter |
| `ApplyModelTextRequestDto` | Vollständigen Editor-Draft oder lokalen `.use` Dateiinhalt anwenden | `modelText`, `format` | `projectId`, `baseVersion`, `sourceName`, `sourceFormat`, `sourceOrigin` | `{"format":"USE_TEXT","sourceName":"Library.use","sourceOrigin":"open-existing","modelText":"model Library"}` | `Apply Changes`, `Open Existing Project` | Model Text Parser/Mapper | Ja/Should | Konflikterkennung, direkter `.use` Import |
| `ApplyModelTextResponseDto` | Ergebnis von `Apply Changes` | `success`, `diagnostics` | `project`, `modelText`, `changedElementIds` | `{"success":true}` | State Update, Diagnostics | Parser/Mapper | Ja/Should | Partial Apply |

Beispiel Parse Response:

```json
{
  "valid": true,
  "normalizedExpression": "self.books <= 5",
  "ast": {
    "nodeType": "BinaryExpression",
    "operator": "<=",
    "left": {
      "nodeType": "AttributeAccessExpression",
      "source": { "nodeType": "SelfExpression" },
      "attributeName": "books"
    },
    "right": {
      "nodeType": "LiteralExpression",
      "literalType": "Integer",
      "value": 5
    }
  },
  "diagnostics": []
}
```

Beispiel Type Error:

```json
{
  "valid": false,
  "resultType": null,
  "diagnostics": [
    {
      "code": "TYPE_ERROR",
      "severity": "ERROR",
      "message": "Operator <= kann nicht auf String und Integer angewendet werden.",
      "range": { "startLine": 1, "startColumn": 11, "endLine": 1, "endColumn": 13 },
      "target": {
        "elementType": "INVARIANT",
        "elementId": "inv-user-maxBooks",
        "path": "umlModel.invariants[0].expression"
      }
    }
  ]
}
```

## Validation-DTOs

| DTO | Zweck | Pflichtfelder | Optionale Felder | Beispiel | Frontend-Verwendung | Backend-Verwendung | MVP | Erweiterungen |
|---|---|---|---|---|---|---|---|---|
| `ValidationRequestDto` | Constraint Check starten | `mode` | `projectId`, `project`, `snapshotId`, `changedElementIds` | `{"mode":"FULL_PROJECT"}` | Check Constraints Button | Validation Service | Ja | Incremental Validation |
| `ValidationResultDto` | Gesamtergebnis | `status`, `summary`, `errors` | `warnings`, `infos`, `startedAt`, `finishedAt` | `{"status":"INVALID"}` | Validation Results Panel | Aggregiertes Ergebnis | Ja | Rule Coverage |
| `ValidationSummaryDto` | Zaehler | `errorCount`, `warningCount`, `infoCount` | `checkedInvariantCount`, `checkedObjectCount` | `{"errorCount":1}` | Badge, Panel Header | Reporting | Ja | Performance-Metriken |
| `ValidationErrorDto` | Einzelner Befund | `id`, `code`, `severity`, `message`, `targets` | `userMessage`, `details`, `invariantId`, `contextObjectId` | `{"code":"INVARIANT_VIOLATION"}` | Fehlerliste, Highlighting | Fehlertransport | Ja | Warnings/Infos als `ValidationErrorDto` |
| `ElementTargetDto` | UI-Mapping-Ziel | `elementType`, `elementId` | `path`, `range` | `{"elementType":"OBJECT","elementId":"obj-alice"}` | Fokus, Markierung | Referenzmodell | Ja | Multi-Target Fixes |

Hinweis: Falls Warnings und Infos langfristig identisch zu Fehlern behandelt werden, kann der allgemeinere Name `ValidationErrorDto` verwendet werden. Fuer den MVP ist `ValidationErrorDto` als expliziter Fehlerdatensatz ausreichend.

Beispiel:

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
      "code": "INVARIANT_VIOLATION",
      "severity": "ERROR",
      "message": "Invariant maxBooks ist fuer alice verletzt.",
      "userMessage": "alice verletzt die Invariante maxBooks: self.books <= 5.",
      "invariantId": "inv-user-maxBooks",
      "contextClassId": "class-user",
      "contextObjectId": "obj-alice",
      "oclExpression": "self.books <= 5",
      "actualValue": false,
      "targets": [
        {
          "elementType": "OBJECT",
          "elementId": "obj-alice",
          "path": "objectModel.objects[obj-alice]"
        },
        {
          "elementType": "INVARIANT",
          "elementId": "inv-user-maxBooks",
          "path": "umlModel.invariants[inv-user-maxBooks]"
        }
      ]
    }
  ]
}
```

## Error-DTOs

| DTO | Zweck | Pflichtfelder | Optionale Felder | Beispiel | Frontend-Verwendung | Backend-Verwendung | MVP | Erweiterungen |
|---|---|---|---|---|---|---|---|---|
| `ApiErrorDto` | Technischer oder API-bezogener Fehler | `code`, `message`, `timestamp` | `requestId`, `path`, `details`, `fieldErrors` | `{"code":"PROJECT_NOT_FOUND"}` | Toast, Error Page, Formfehler | Exception Handling | Ja | Problem Details RFC 9457 |

Beispiel:

```json
{
  "code": "INVALID_PROJECT_FORMAT",
  "message": "Das Projektformat konnte nicht geladen werden.",
  "userMessage": "Die Datei ist kein gueltiges USE-Web-Projekt.",
  "path": "/api/v1/projects/import",
  "timestamp": "2026-07-15T10:20:00Z",
  "requestId": "req-123",
  "details": {
    "schemaVersion": "0.5",
    "expectedSchemaVersion": "1.0"
  }
}
```

## Layout-DTOs

Layoutdaten duerfen keine fachliche Semantik enthalten. Sie beschreiben nur, wie ein fachliches Element in der UI dargestellt wird.

| DTO | Zweck | Pflichtfelder | Optionale Felder | Beispiel | Frontend-Verwendung | Backend-Verwendung | MVP | Erweiterungen |
|---|---|---|---|---|---|---|---|---|
| `LayoutDto` | Container fuer Layoutdaten | `classDiagram`, `objectDiagram` | `oclEditor`, `updatedAt` | `{"classDiagram":{}}` | Layout speichern/laden | Persistieren ohne Semantik | Ja | Mehrere Views |
| `DiagramLayoutDto` | Layout einer View | `nodes` | `edges`, `viewport` | `{"nodes":[]}` | Diagram Canvas | Speicherung | Ja | Auto-Layout-Metadaten |
| `NodeLayoutDto` | Node-Position | `elementId`, `x`, `y` | `width`, `height`, `collapsed` | `{"elementId":"class-user","x":120,"y":80}` | React Flow Node Position | Persistenz | Ja | Swimlanes, Gruppen |
| `EdgeLayoutDto` | Edge-Darstellung | `elementId` | `bendPoints`, `labelPosition` | `{"elementId":"assoc-borrows"}` | Edge Labels/Bendpoints | Persistenz | Should | Routing-Strategie |
| `ViewportDto` | Kamera/Zoom | `x`, `y`, `zoom` | - | `{"x":0,"y":0,"zoom":1}` | Letzte Ansicht | Optional speichern | Later | Benutzerprofile |

Beispiel:

```json
{
  "classDiagram": {
    "nodes": [
      {
        "elementId": "class-user",
        "x": 120,
        "y": 80,
        "width": 240,
        "height": 180
      }
    ],
    "edges": [
      {
        "elementId": "assoc-borrows",
        "labelPosition": { "x": 360, "y": 160 }
      }
    ],
    "viewport": { "x": 0, "y": 0, "zoom": 1 }
  },
  "objectDiagram": {
    "nodes": [
      {
        "elementId": "obj-alice",
        "x": 120,
        "y": 100
      }
    ],
    "edges": []
  }
}
```

## Vollstaendiges Library-Beispiel

```json
{
  "formatVersion": "1.0",
  "project": {
    "id": "project-library-demo",
    "name": "Library",
    "description": "MVP-Beispiel fuer Klassendiagramm, Objektdiagramm und OCL-Validierung",
    "createdAt": "2026-07-15T10:00:00Z",
    "updatedAt": "2026-07-15T10:15:00Z"
  },
  "umlModel": {
    "classes": [
      {
        "id": "class-user",
        "name": "User",
        "attributes": [
          {
            "id": "attr-user-name",
            "name": "name",
            "type": "String"
          },
          {
            "id": "attr-user-books",
            "name": "books",
            "type": "Integer"
          }
        ],
        "operations": [
          {
            "id": "op-user-canBorrow",
            "name": "canBorrow",
            "returnType": "Boolean",
            "parameters": []
          }
        ]
      },
      {
        "id": "class-book",
        "name": "Book",
        "attributes": [
          {
            "id": "attr-book-title",
            "name": "title",
            "type": "String"
          },
          {
            "id": "attr-book-available",
            "name": "available",
            "type": "Boolean"
          }
        ],
        "operations": []
      }
    ],
    "associations": [
      {
        "id": "assoc-borrows",
        "name": "Borrows",
        "ends": [
          {
            "id": "assocend-borrows-user",
            "classId": "class-user",
            "roleName": "borrower",
            "multiplicity": { "lower": 0, "upper": "*" },
            "navigable": true
          },
          {
            "id": "assocend-borrows-book",
            "classId": "class-book",
            "roleName": "borrowedBooks",
            "multiplicity": { "lower": 0, "upper": "*" },
            "navigable": true
          }
        ]
      }
    ],
    "invariants": [
      {
        "id": "inv-user-maxBooks",
        "name": "maxBooks",
        "contextClassId": "class-user",
        "expression": "self.books <= 5",
        "enabled": true
      }
    ]
  },
  "objectModel": {
    "snapshotId": "snapshot-default",
    "name": "Default Snapshot",
    "objects": [
      {
        "id": "obj-alice",
        "name": "alice",
        "classId": "class-user",
        "slots": [
          {
            "id": "slot-alice-name",
            "attributeId": "attr-user-name",
            "value": "Alice",
            "valueType": "String"
          },
          {
            "id": "slot-alice-books",
            "attributeId": "attr-user-books",
            "value": 6,
            "valueType": "Integer"
          }
        ]
      },
      {
        "id": "obj-mobydick",
        "name": "mobyDick",
        "classId": "class-book",
        "slots": [
          {
            "id": "slot-mobydick-title",
            "attributeId": "attr-book-title",
            "value": "Moby Dick",
            "valueType": "String"
          },
          {
            "id": "slot-mobydick-available",
            "attributeId": "attr-book-available",
            "value": false,
            "valueType": "Boolean"
          }
        ]
      }
    ],
    "links": [
      {
        "id": "link-alice-mobydick",
        "associationId": "assoc-borrows",
        "endValues": [
          {
            "associationEndId": "assocend-borrows-user",
            "objectId": "obj-alice"
          },
          {
            "associationEndId": "assocend-borrows-book",
            "objectId": "obj-mobydick"
          }
        ]
      }
    ]
  },
  "layout": {
    "classDiagram": {
      "nodes": [
        { "elementId": "class-user", "x": 120, "y": 80, "width": 240, "height": 170 },
        { "elementId": "class-book", "x": 460, "y": 80, "width": 240, "height": 170 }
      ],
      "edges": [
        { "elementId": "assoc-borrows", "labelPosition": { "x": 350, "y": 120 } }
      ]
    },
    "objectDiagram": {
      "nodes": [
        { "elementId": "obj-alice", "x": 120, "y": 100, "width": 240, "height": 150 },
        { "elementId": "obj-mobydick", "x": 460, "y": 100, "width": 240, "height": 150 }
      ],
      "edges": [
        { "elementId": "link-alice-mobydick", "labelPosition": { "x": 350, "y": 140 } }
      ]
    }
  }
}
```

## TypeScript-Interfaces

Die TypeScript-Interfaces spiegeln den API-Vertrag. Frontend-interne View Models koennen davon abgeleitet werden, sollten aber separat bleiben.

```ts
export type PrimitiveType = "String" | "Integer" | "Real" | "Boolean";
export type UmlType = PrimitiveType | string;
export type MultiplicityUpper = number | "*";

export interface ProjectDto {
  formatVersion: string;
  project: ProjectMetadataDto;
  umlModel: UmlModelDto;
  objectModel: ObjectModelDto;
  layout: LayoutDto;
  validationState?: ValidationStateDto;
  extensions?: Record<string, unknown>;
}

export interface ProjectMetadataDto {
  id: string;
  name: string;
  description?: string;
  createdAt?: string;
  updatedAt?: string;
}

export interface ProjectSummaryDto {
  id: string;
  name: string;
  updatedAt: string;
  description?: string;
  sourceFormat?: "json" | "use" | "example";
  thumbnail?: string;
}

export interface UmlModelDto {
  classes: UmlClassDto[];
  associations: UmlAssociationDto[];
  invariants: UmlInvariantDto[];
}

export interface UmlClassDto {
  id: string;
  name: string;
  attributes: UmlAttributeDto[];
  operations: UmlOperationDto[];
  abstract?: boolean;
  superClassIds?: string[];
}

export interface UmlAttributeDto {
  id: string;
  name: string;
  type: UmlType;
  multiplicity?: MultiplicityDto;
  defaultValue?: unknown;
  readonly?: boolean;
}

export interface UmlOperationDto {
  id: string;
  name: string;
  returnType: UmlType;
  parameters: UmlParameterDto[];
  visibility?: "PUBLIC" | "PROTECTED" | "PACKAGE" | "PRIVATE";
  abstractOperation?: boolean;
  query?: boolean;
  bodyExpression?: string | null;
}

export interface UmlParameterDto {
  id: string;
  name: string;
  type: UmlType;
  direction: "IN" | "OUT" | "INOUT";
  position: number;
}

export interface UmlAssociationDto {
  id: string;
  name: string;
  ends: UmlAssociationEndDto[];
  associationClassId?: string | null;
}

export interface UmlAssociationEndDto {
  id: string;
  classId: string;
  roleName: string;
  multiplicity: MultiplicityDto;
  navigable?: boolean;
  ordered?: boolean;
  unique?: boolean;
  derived?: boolean;
  union?: boolean;
  subsettedEndIds?: string[];
  redefinedEndIds?: string[];
  navigationType?: "SINGLE" | "SET" | "BAG" | "SEQUENCE" | "ORDERED_SET";
  aggregationKind?: "NONE" | "SHARED" | "COMPOSITE";
}

export interface MultiplicityDto {
  lower: number;
  upper: MultiplicityUpper;
}

export interface UmlInvariantDto {
  id: string;
  name: string;
  contextClassId: string;
  expression: string;
  enabled?: boolean;
  description?: string;
}

export interface ObjectModelDto {
  snapshotId?: string;
  name?: string;
  objects: ObjectInstanceDto[];
  links: ObjectLinkDto[];
}

export interface ObjectInstanceDto {
  id: string;
  name: string;
  classId: string;
  slots: SlotDto[];
  displayName?: string;
}

export interface SlotDto {
  id: string;
  attributeId: string;
  value: string | number | boolean | null;
  valueType?: UmlType;
  isUnset?: boolean;
}

export interface ObjectLinkDto {
  id: string;
  associationId: string;
  endValues: ObjectLinkEndValueDto[];
  associationClassObjectId?: string | null;
}

export interface ObjectLinkEndValueDto {
  associationEndId: string;
  objectId: string;
}

export interface OperationInvocationRequestDto {
  receiverObjectId: string;
  operationId: string;
  arguments: Array<{ parameterId: string; value: SlotValueDto }>;
  expectedRevision?: number;
}

export interface OperationInvocationResultDto {
  invocationId: string;
  status: "SUCCEEDED" | "BLOCKED" | "ROLLED_BACK";
  receiver: { id: string; name: string; typeName: string };
  requestedOperationId: string;
  resolvedOperationId: string;
  resolvedOperationName: string;
  resolvedOwnerClassId: string;
  result: SlotValueDto | null;
  outValues: Array<{ parameterId: string; parameterName: string; value: SlotValueDto }>;
  lifecycle: {
    createdObjects: Array<{ id: string; name: string; typeName: string }>;
    changedObjects: Array<{ id: string; name: string; typeName: string }>;
    deletedObjects: Array<{ id: string; name: string; typeName: string }>;
  };
  beforeSnapshotId: string;
  afterSnapshotId: string | null;
  candidateAfterSnapshotId: string | null;
  revision: number;
  diagnostics: string[];
  contractResults: OperationContractResultDto[];
}

export interface OperationContractDto {
  id: string;
  name: string;
  kind: "PRE" | "POST";
  expression: string;
  enabled: boolean;
}

export interface OperationContractResultDto {
  contractId: string;
  contractName: string;
  kind: "PRE" | "POST";
  status: "SATISFIED" | "VIOLATED" | "CONTEXT_ERROR" | "NOT_EVALUATED";
  diagnostics: OclDiagnosticDto[];
}

export interface OclParseRequestDto {
  expression: string;
  invariantId?: string;
  contextClassId?: string;
}

export interface OclParseResponseDto {
  valid: boolean;
  diagnostics: OclDiagnosticDto[];
  ast?: unknown;
  normalizedExpression?: string;
}

export interface OclTypecheckRequestDto {
  expression: string;
  contextClassId: string;
  invariantId?: string;
}

export interface OclTypecheckResponseDto {
  valid: boolean;
  diagnostics: OclDiagnosticDto[];
  resultType?: UmlType;
  resolvedReferences?: ElementTargetDto[];
}

export interface OclEvaluateRequestDto {
  expression: string;
  contextObjectId: string;
  projectId?: string;
  project?: ProjectDto;
  invariantId?: string;
  snapshotId?: string;
}

export interface OclEvaluateResponseDto {
  valid: boolean;
  diagnostics: OclDiagnosticDto[];
  value?: unknown;
  valueType?: UmlType;
  trace?: unknown;
}

export interface OclDiagnosticDto {
  code: string;
  severity: Severity;
  message: string;
  range?: SourceRangeDto;
  target?: ElementTargetDto;
  technicalDetails?: Record<string, unknown>;
}

export interface SourceRangeDto {
  startLine: number;
  startColumn: number;
  endLine: number;
  endColumn: number;
}

export interface ValidationRequestDto {
  mode: "FULL_PROJECT" | "SNAPSHOT_ONLY" | "OCL_ONLY";
  projectId?: string;
  project?: ProjectDto;
  snapshotId?: string;
  changedElementIds?: string[];
}

export interface ValidationResultDto {
  status: "VALID" | "INVALID" | "ERROR";
  summary: ValidationSummaryDto;
  errors: ValidationErrorDto[];
  warnings?: ValidationErrorDto[];
  infos?: ValidationErrorDto[];
  startedAt?: string;
  finishedAt?: string;
}

export interface ValidationSummaryDto {
  errorCount: number;
  warningCount: number;
  infoCount: number;
  checkedInvariantCount?: number;
  checkedObjectCount?: number;
}

export type Severity = "ERROR" | "WARNING" | "INFO";

export interface ValidationErrorDto {
  id: string;
  code:
    | "SYNTAX_ERROR"
    | "TYPE_ERROR"
    | "UNKNOWN_CLASS"
    | "UNKNOWN_ATTRIBUTE"
    | "INVALID_SLOT_VALUE"
    | "INVALID_LINK"
    | "MULTIPLICITY_VIOLATION"
    | "INVARIANT_VIOLATION"
    | "EVALUATION_ERROR";
  severity: Severity;
  message: string;
  userMessage?: string;
  targets: ElementTargetDto[];
  invariantId?: string;
  contextClassId?: string;
  contextObjectId?: string;
  associationId?: string;
  linkId?: string;
  oclExpression?: string;
  actualValue?: unknown;
  details?: Record<string, unknown>;
}

export interface ElementTargetDto {
  elementType:
    | "PROJECT"
    | "CLASS"
    | "ATTRIBUTE"
    | "OPERATION"
    | "ASSOCIATION"
    | "ASSOCIATION_END"
    | "INVARIANT"
    | "OBJECT"
    | "SLOT"
    | "OBJECT_LINK"
    | "OCL_EXPRESSION";
  elementId: string;
  path?: string;
  range?: SourceRangeDto;
}

export interface ApiErrorDto {
  code: string;
  message: string;
  userMessage?: string;
  path?: string;
  timestamp: string;
  requestId?: string;
  details?: Record<string, unknown>;
  fieldErrors?: Record<string, string>;
}

export interface LayoutDto {
  classDiagram: DiagramLayoutDto;
  objectDiagram: DiagramLayoutDto;
  updatedAt?: string;
}

export interface DiagramLayoutDto {
  nodes: NodeLayoutDto[];
  edges?: EdgeLayoutDto[];
  viewport?: ViewportDto;
}

export interface NodeLayoutDto {
  elementId: string;
  x: number;
  y: number;
  width?: number;
  height?: number;
  collapsed?: boolean;
}

export interface EdgeLayoutDto {
  elementId: string;
  bendPoints?: Array<{ x: number; y: number }>;
  labelPosition?: { x: number; y: number };
}

export interface ViewportDto {
  x: number;
  y: number;
  zoom: number;
}
```

## Java-Record-Beispiele

Die Java Records zeigen eine moegliche API-Repräsentation im Spring-Boot-Backend. Interne Domain-Objekte koennen anders aufgebaut sein.

```java
public record ProjectDto(
    String formatVersion,
    ProjectMetadataDto project,
    UmlModelDto umlModel,
    ObjectModelDto objectModel,
    LayoutDto layout,
    ValidationStateDto validationState,
    Map<String, Object> extensions
) {}

public record ProjectMetadataDto(
    String id,
    String name,
    String description,
    Instant createdAt,
    Instant updatedAt
) {}

public record UmlClassDto(
    String id,
    String name,
    List<UmlAttributeDto> attributes,
    List<UmlOperationDto> operations,
    Boolean isAbstract,
    List<String> superClassIds
) {}

public record UmlAttributeDto(
    String id,
    String name,
    String type,
    MultiplicityDto multiplicity,
    Object defaultValue,
    Boolean readonly
) {}

public record UmlAssociationDto(
    String id,
    String name,
    List<UmlAssociationEndDto> ends,
    String kind
) {}

public record UmlInvariantDto(
    String id,
    String name,
    String contextClassId,
    String expression,
    Boolean enabled,
    String description
) {}
```

```java
public record ObjectModelDto(
    String snapshotId,
    String name,
    List<ObjectInstanceDto> objects,
    List<ObjectLinkDto> links
) {}

public record ObjectInstanceDto(
    String id,
    String name,
    String classId,
    List<SlotDto> slots,
    String displayName
) {}

public record SlotDto(
    String id,
    String attributeId,
    Object value,
    String valueType,
    Boolean isUnset
) {}
```

```java
public record ValidationResultDto(
    String status,
    ValidationSummaryDto summary,
    List<ValidationErrorDto> errors,
    List<ValidationErrorDto> warnings,
    List<ValidationErrorDto> infos,
    Instant startedAt,
    Instant finishedAt
) {}

public record ValidationErrorDto(
    String id,
    String code,
    String severity,
    String message,
    String userMessage,
    List<ElementTargetDto> targets,
    String invariantId,
    String contextClassId,
    String contextObjectId,
    String associationId,
    String linkId,
    String oclExpression,
    Object actualValue,
    Map<String, Object> details
) {}

public record ElementTargetDto(
    String elementType,
    String elementId,
    String path,
    SourceRangeDto range
) {}
```

```java
public record LayoutDto(
    DiagramLayoutDto classDiagram,
    DiagramLayoutDto objectDiagram,
    Instant updatedAt
) {}

public record DiagramLayoutDto(
    List<NodeLayoutDto> nodes,
    List<EdgeLayoutDto> edges,
    ViewportDto viewport
) {}

public record NodeLayoutDto(
    String elementId,
    double x,
    double y,
    Double width,
    Double height,
    Boolean collapsed
) {}
```

## Versionierung

| Bereich | Empfehlung |
|---|---|
| Projektformat | `ProjectDto.formatVersion` ist Pflicht und wird beim Laden validiert. |
| API-Version | Routen verwenden eine Version, z. B. `/api/v1/projects`. |
| Rueckwaertskompatibilitaet | Neue Felder sollten optional eingefuehrt werden. |
| Unbekannte Felder | Backend sollte unbekannte Felder im MVP ablehnen oder ignorieren; die Entscheidung muss konsistent dokumentiert werden. |
| Migration | Spaetere Migrationen koennen projektformatbasiert von `1.0` auf `1.1` usw. laufen. |
| `.use` Import | Importierte Modelle werden in dieses DTO-/JSON-Format transformiert, nicht im originalen `.use`-Format gespeichert. |

## Offene Fragen

| Frage | Relevanz |
|---|---|
| Sollen IDs vom Frontend, Backend oder gemeinsam erzeugt werden? | Wichtig fuer Offline-Editing und Optimistic Updates. |
| Soll `ValidationErrorDto` langfristig in `ValidationErrorDto` umbenannt werden? | Sinnvoll, wenn Warnings und Infos gleichwertig behandelt werden. |
| Soll `OclEvaluateRequestDto` im MVP oeffentlich angeboten werden? | Fuer Debugging nuetzlich, fuer den Kernworkflow reicht `validate`. |
| Wie wird `null`, `undefined` oder nicht gesetzter Slot fachlich behandelt? | Relevant fuer OCL-Evaluation und UI-Formulare. |
| Werden Layoutdaten pro Nutzer oder pro Projekt gespeichert? | Relevant fuer spaetere Kollaboration und Benutzerprofile. |
| Wie streng soll das Backend unbekannte JSON-Felder behandeln? | Relevant fuer Import, Migration und Robustheit. |

## Zusammenfassung

Die DTOs definieren einen stabilen Vertrag zwischen Frontend und Backend. Fuer den MVP sind besonders `ProjectDto`, `UmlModelDto`, `ObjectModelDto`, `UmlInvariantDto`, `ValidationRequestDto`, `ValidationResultDto`, `ValidationErrorDto` und `LayoutDto` zentral.

Das Backend bleibt die fachliche Quelle fuer UML-, OCL- und Validierungslogik. Das Frontend nutzt dieselben IDs, um Klassen, Objekte, Links, Invarianten und Fehler in Diagrammen, Properties Panel und Validation Results Panel eindeutig zu verbinden. Layoutdaten werden getrennt gehalten und duerfen keine fachliche Bedeutung bekommen.
## B35 semantic read model (API v1 additive)

`GET /api/v1/projects/{projectId}/read-model` returns `ProjectReadModelDto`.
It is a read-only projection and does not replace the persisted `ProjectDto`.
Its nested projections provide stable semantic element IDs, separate explorer
node IDs, qualified names, defining classifiers, inheritance metadata, typed
value status (`VALUE`, `NULL`, `INVALID`), recursive collection/tuple/DataType
values, object-centered links and structured validation diagnostics. The exact
field and nullability contract is documented in
`09-ocl-extension-analysis/50-b35-read-and-result-contracts.md`.

Existing API-v1 request DTOs are unchanged.
## B37 Contract-Acceptance

Die erneute B37-Abnahme vom 28. August 2026 bestätigt die hier dokumentierten
B35-Read- und B36-Commandverträge ohne DTO-Korrektur. B39 ergänzt additiv
`UmlAttributeDto.staticAttribute`, `UmlAttributeDto.classifierValue` und
`FeatureProjectionDto.classifierValue`; B40 ergänzt persistierte Class-/Package-
Definitionen. B38 ergänzt additiv
`redefinedAttributeIds`, `redefinedOperationIds` und im Read Model
`FeatureProjectionDto.redefinedFeatures`. Der revisionsgeschützte Command
`PUT /api/v1/projects/{projectId}/commands/classes/{classId}/redefinitions`
verwendet `MutationCommandRequestDto`; sein Draft enthält `featureKind`,
`localFeatureId`, `redefinedFeatureIds` und optional `supertypeIds` für die
atomare gemeinsame Hierarchieänderung. Die übrigen Verträge dürfen vom
Frontend nicht vorweggenommen werden. `BACKEND_CONTRACT_READY` ist für
F1-F11 und F12 gesetzt.

## B40 Definitionsvertrag

`ProjectDto.definitions` persistiert eine Liste `OclDefinitionElementDto` mit
`id`, `kind` (`PROPERTY_DEF`/`OPERATION_DEF`), `ownerKind`
(`CLASS`/`PACKAGE`), `ownerId`, fachlichem `ownerName`, `name`,
`qualifiedName`, `resultType`, geordneten `parameters`, `expression` und
`sourceRange`. Alte API-v1-Projekte ohne Feld werden als leere Liste gelesen.

`GET /api/v1/projects/{projectId}/commands/definitions` und das additive Read
Model liefern dieselben stabilen Definition-/Owner-IDs. Create/Update verwenden
den vollständigen DTO-Draft und `expectedRevision`; Delete Impact und Delete
adressieren `DEFINITION/{definitionId}`. Details und Nullability stehen in
`09-ocl-extension-analysis/55-b40-persisted-class-package-definitions.md`.

## B41 Snapshot-Command-DTOs

```ts
interface CreateObjectCommandRequestDto {
  expectedRevision: string;
  draft: ObjectInstanceDto;
}

interface UpdateSlotCommandRequestDto {
  expectedRevision: string;
  draft: SlotDto;
}

interface CreateObjectLinkCommandRequestDto {
  expectedRevision: string;
  draft: ObjectLinkDto;
}
```

Alle drei Antworten verwenden `MutationResultDto`. `revisionScope` ist
`SNAPSHOT`; `revision` ist die neue Projekt-/Snapshotrevision; `result` ist
je nach Command `ObjectInstanceDto`, `SlotDto` oder `ObjectLinkDto`.
`affectedElements` enthaelt stabile IDs, fachliche Namen und Feldpfade fuer
Object, Classifier, Slot, Feature, Association, Association End und Qualifier.
`expectedRevision` und `draft` sind nicht nullable. Fachliche Draftfelder
behalten die Nullability ihrer bestehenden Snapshot-DTOs.

## B44 Model-Feature- und Association-Drafts

`MutationCommandRequestDto` bleibt unveraendert:

```ts
interface MutationCommandRequestDto {
  expectedRevision: string;
  draft: UmlAttributeDto | UmlOperationDto | UmlAssociationDto;
}
```

`UmlOperationDto` ergaenzt additiv das nullable Boolean-Feld
`staticOperation`; fehlt es in aelteren API-v1-Payloads, gilt `false`.
Attribute- und Association-DTOs bleiben strukturell kompatibel. Bei
Association Update ist die ID aus dem Pfad autoritativ; der Draft transportiert
Name, `associationClassId` und die vollstaendige Endliste inklusive stabilen
End-IDs, Multiplizitaet, Navigation, Ordered/Unique, Derived/Union,
Subset/Redefinition, Qualifier und `aggregationKind`.

Erfolg liefert `MutationResultDto`. `affectedElements` enthaelt bei Features
die stabile Attribute- oder Operation-ID, bei Associations zusaetzlich alle
End- und Qualifier-IDs. Fehler behalten den unveraenderten JSON-Draft.

## B42 Object-Link-Command-DTOs

```ts
interface UpdateObjectLinkCommandRequestDto {
  expectedRevision: string;
  draft: ObjectLinkDto;
}

interface ObjectLinkDeleteImpactDto {
  revisionScope: "SNAPSHOT";
  revision: string;
  target: CommandElementReferenceDto;
  currentLink: ObjectLinkDto;
  context: CommandElementReferenceDto[];
  blockers: CommandElementReferenceDto[];
  allowedCascades: CommandElementReferenceDto[];
  validationTargets: CommandElementReferenceDto[];
  blocked: boolean;
}
```

Delete verwendet `DeleteCommandRequestDto`. `expectedRevision` ist nicht
nullable; `cascadeReferenceIds` ist leer oder enthaelt ausschliesslich IDs aus
`allowedCascades`. Update und Delete antworten mit `MutationResultDto`.

## B43 Enumeration-DTOs

```ts
interface UmlEnumerationLiteralDto {
  id: string;
  name: string;
}

interface UmlEnumerationDto {
  id: string;
  name: string;
  literals: string[]; // kompatible API-v1-Projektion
  packageId: string | null;
  qualifiedName: string;
  visibility: 'PUBLIC' | 'PRIVATE' | 'PROTECTED' | 'PACKAGE';
  literalDefinitions: UmlEnumerationLiteralDto[];
}

interface EnumerationProjectionDto {
  id: string;
  name: string;
  qualifiedName: string;
  packageId: string | null;
  visibility: string;
  literals: Array<{ id: string; name: string; order: number }>;
}
```

`literalDefinitions` ist fuer neue Schreib- und Roundtrip-Vertraege
autoritativ. `literals` bleibt fuer bestehende API-v1-Clients erhalten.
Reorder veraendert nur die Listenposition; stabile Literal-IDs bleiben gleich.
`DeleteCommandRequestDto.enumerationId` ist nur beim Literal-Delete
verpflichtend, bei anderen Delete-Zielen nullable.

## B45 Package- und Import-Command-DTOs

`MutationCommandRequestDto` wird ohne API-v1-Bruch auch fuer Packages und
Imports verwendet:

```ts
interface MutationCommandRequestDto {
  expectedRevision: string;
  draft: UmlPackageDto | UmlModelImportDto;
}

interface UmlPackageDto {
  id: string;
  qualifiedName: string;
}

interface UmlModelImportDto {
  id: string;
  importingPackageId: string;
  importedPackageId: string;
  alias: string | null;
  source: string | null;
  provenance: string | null;
}
```

Beim Update sind Pfad-ID und bestehende stabile ID autoritativ. Ein
Parent-Wechsel wird durch den neuen qualifizierten Package-Namen ausgedrueckt;
der Backendservice aktualisiert den betroffenen Unterbaum und behaelt alle
Package- und Classifier-IDs. Erfolg liefert `MutationResultDto` mit
`revisionScope = "MODEL"`, neuer Revision und Package-/Import-Referenz.

Delete Impact verwendet den bestehenden `DeleteImpactDto`. Fuer `PACKAGE`
koennen enthaltene Packages, Classifier, Associations, Constraints, Objects,
Links, Definitionen und Imports als ausdrueckliche Cascades angeboten werden.
Typ-, Generalization- und OCL-/Importnutzungen bleiben nicht cascadefaehige
Blocker. `IMPORT` liefert seine OCL-Nutzer als Blocker. Der Read-Model-Explorer
bleibt fuer Parent-Knoten, qualifizierte Namen, `importId`, `provenance` und
`readOnly` autoritativ.

## B46 V2-DTO-Abnahme

B46 fuehrt keine neuen DTOs ein. Fuer alle B41-B45-Mutationen ist der
V2-Umschlag aus `MutationCommandRequestDto.expectedRevision`, vollstaendigem
`draft` und `MutationResultDto` verbindlich. Delete verwendet
`DeleteCommandRequestDto` und `DeleteImpactDto`; Snapshotmutationen liefern
`revisionScope = "SNAPSHOT"`, Modellmutationen `revisionScope = "MODEL"`.

Fehler behalten den vollstaendigen Draft und strukturierte Targets mit
stabilen Element-/Reference-IDs, fachlichen Namen und Feldpfaden. Nullable
Felder behalten die in B41-B45 dokumentierte Bedeutung. Legacy-DTOs bleiben
fuer API-v1-Kompatibilitaet erhalten, ersetzen aber keinen V2-Commandvertrag.

## B48: Association-Class-Aggregat-DTOs

- Model Create-and-bind: `MutationCommandRequestDto(expectedRevision, draft)`
  mit vollstaendigem `UmlClassDto`-Draft; Ergebnis
  `AssociationClassAggregateDto(association, associationClass)`.
- Snapshot Create/Update:
  `AssociationClassInstanceCommandRequestDto(expectedRevision, draft)` mit
  `AssociationClassInstanceDraftDto(link, associationClassObject)`; Ergebnis
  `AssociationClassInstanceAggregateDto(link, associationClassObject)`.
- `link`, `associationClassObject` und saemtliche schreibbaren Slots sind
  Bestandteile des Aggregatedrafts. Generierbare IDs duerfen `null` sein; die
  autoritative Antwort enthaelt stabile IDs und genau eine neue Revision.
- Fehler enthalten den vollstaendigen verschachtelten Draft und Feldpfade wie
  `draft.link.endValues[...]` und `draft.associationClassObject.slots[...]`.

## B49 Operation-Delete-Owner-Aufloesung

B49 fuehrt kein neues DTO und kein neues Pflichtfeld ein. Fuer
`DELETE .../commands/OPERATION/{operationId}` bleibt der bestehende
`DeleteCommandRequestDto` verbindlich:

```ts
interface DeleteCommandRequestDto {
  expectedRevision: string;
  cascadeReferenceIds: string[];
  enumerationId: string | null;
}
```

`enumerationId` bleibt ausschliesslich fuer Enumeration-Literal-Delete
relevant. Die Owner Class einer Operation wird serverseitig aus der stabilen
Pfad-`operationId` ermittelt. Delete Impact und Delete verwenden dieselbe
Aufloesung und liefern `DeleteImpactDto` beziehungsweise `MutationResultDto`
mit `revisionScope = "MODEL"`. Bei Erfolg enthaelt `affectedElements` die
geloeschte Operation und ihre definierende Class; bei Revision Conflict
enthaelt `details.currentImpact` die aktuelle autoritative Impact-Projektion.

## B50 Persistierte strukturierte Werttypen

B50 fuehrt keine inkompatiblen DTO-Felder ein. Die bestehenden Felder
`UmlAttributeDto.type`, `UmlDataTypePropertyDto.type` und
`SlotValueDto.type` bleiben Strings mit folgender serverseitiger Grammatik:

```text
Type := NamedType
      | Tuple(field:Type,...)
      | Set(Type) | Bag(Type) | Sequence(Type) | OrderedSet(Type)
```

`NamedType` ist ein primitiver oder namespace-/importsensitiv aufgeloester
Class-, Enumeration- oder DataType-Name. `SlotValueDto.value` und
`UmlAttributeDto.classifierValue.value` verwenden rekursiv:

- Primitive und Enumerationsliterale als JSON-Skalar,
- DataType und Tuple als JSON-Objekt mit exakt den deklarierten Feldnamen,
- Collections als JSON-Array; Reihenfolge bleibt fuer Sequence und OrderedSet
  erhalten, Duplikate bleiben nur fuer Bag und Sequence erhalten,
- `null` als fachlich undefinierter Wert; `invalid` ist kein persistierter
  JSON-Wert und bleibt ein eigener Ergebnis-/Diagnostic-Zustand.

Create/Update verwenden weiterhin die bestehenden
`MutationCommandRequestDto`, `CreateObjectCommandRequestDto` und
`UpdateSlotCommandRequestDto`. Erfolg liefert `MutationResultDto` mit
`MODEL`- oder `SNAPSHOT`-Revision. DataType Delete verwendet den bestehenden
generischen `DeleteImpactDto`-/`DeleteCommandRequestDto`-Vertrag mit Zielart
`DATATYPE`. Keine API-v1-Versionierung ist erforderlich.

## B51 DataType-Property-Delete-DTOs

B51 verwendet die bestehenden Umschlaege ohne neues Pflichtfeld:

```ts
interface DeleteCommandRequestDto {
  expectedRevision: string;
  cascadeReferenceIds: string[]; // fuer DataType Properties immer leer
  enumerationId: string | null;  // hier nicht anwendbar
}

interface CommandElementReferenceDto {
  referenceId: string;
  elementType: string;
  elementId: string;
  elementName: string;
  path: string;
  relation: string;
  cascadeAllowed: boolean;
  sourceRange: SourceRangeDto | null;
}
```

`sourceRange` ist additiv und nur fuer quelltextbezogene Referenzen belegt.
Impact verwendet `DeleteImpactDto` mit `revisionScope = "MODEL"`; Erfolg
verwendet `MutationResultDto` und liefert als `result` den aktualisierten
`UmlDataTypeDto`. `affectedElements` enthaelt die entfernte Property und ihren
Owner-DataType. Fehlerdetails enthalten den vollstaendigen Delete- oder
DataType-Draft und bei Delete-/Revisionkonflikten `currentImpact`.
