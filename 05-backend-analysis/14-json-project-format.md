# JSON Project Format

## Zweck dieser Datei

Diese Datei definiert das JSON-basierte Projektformat für den MVP des neuen UML/OCL-Websystems. Das Format ist die Grundlage für:

- Projekt speichern,
- Projekt laden,
- JSON Import/Export,
- Backend-Persistenz im MVP,
- Frontend-State-Hydration,
- Test-Fixtures,
- spätere Migrationen und `.use` Import/Export.

Das Format ist nicht identisch zur originalen USE- oder `.use`-Syntax. Es ist ein neues, web- und API-taugliches JSON-Format mit stabilen IDs, klar getrenntem UML-Modell, Objektmodell, Layoutdaten und optionalen temporären Validierungsdaten.

## Anforderungen an das Projektformat

| Anforderung | Beschreibung |
|---|---|
| Frontend-tauglich | React/TypeScript kann den Projektzustand direkt laden, anzeigen und bearbeiten. |
| Backend-tauglich | Java/Spring Boot kann das Format in Domain-Objekte überführen und validieren. |
| Stabile IDs | Alle fachlichen und UI-relevanten Elemente besitzen IDs. |
| Klare Modelltrennung | UML-Modell und Objektmodell/Snapshot sind getrennte Bereiche. |
| OCL als Text speichern | AST, Typed AST und Evaluation Results werden im MVP nicht als dauerhafte Semantik gespeichert. |
| Layout getrennt speichern | Diagrammpositionen sind UI-Daten und keine fachliche Semantik. |
| Erweiterbar bleiben | Neue Felder und Bereiche müssen versionierbar ergänzt werden können. |
| Import-/Export-fähig | Das Format soll später `.use` Import/Export erleichtern. Im MVP kann zusätzlich ein vollständiger USE-ähnlicher Editor-Text gespeichert werden, ohne vollständige USE-Kompatibilität zu versprechen. |
| Validierbar beim Laden | Formatfehler müssen klar von fachlichen Constraint-Fehlern getrennt werden. |
| Testbar | Das Format eignet sich für Backend- und E2E-Test-Fixtures. |

## Grundstruktur

Empfohlene Top-Level-Struktur:

```json
{
  "formatVersion": "0.1",
  "project": {},
  "modelText": {},
  "umlModel": {},
  "objectModel": {},
  "layout": {},
  "validationState": null,
  "extensions": {}
}
```

| Feld | Pflicht | Zweck |
|---|---|---|
| `formatVersion` | Ja | Version des JSON-Projektformats. |
| `project` | Ja | Projektmetadaten wie ID, Name, Beschreibung und Zeitstempel. |
| `modelText` | Nein, empfohlen | Vollständiger USE-ähnlicher Editor-Text als Quelle/Ansicht für den OCL Editor. |
| `umlModel` | Ja | Klassen, Attribute, Operationen, Associations und Invarianten. |
| `objectModel` | Ja | Aktueller Snapshot mit Objekten, Slots und Objektlinks. |
| `layout` | Nein, empfohlen | UI- und Diagrammpositionen für Frontend. |
| `validationState` | Nein | Optionaler letzter Validierungsstatus; nicht fachlich verbindlich. |
| `extensions` | Nein | Erweiterungspunkt für spätere oder experimentelle Daten. |

Minimal gültiges Projekt:

```json
{
  "formatVersion": "0.1",
  "project": {
    "id": "project-empty",
    "name": "Empty Project"
  },
  "modelText": {
    "text": "model EmptyProject\n",
    "language": "USE_MODEL_TEXT",
    "languageVersion": "mvp-subset"
  },
  "umlModel": {
    "id": "uml-empty",
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

## Metadaten

Der Bereich `project` enthält Metadaten zum Projektcontainer.

```json
{
  "project": {
    "id": "project-library",
    "name": "Library Example",
    "description": "MVP example for UML/OCL validation",
    "createdAt": "2026-07-11T20:00:00Z",
    "updatedAt": "2026-07-11T20:30:00Z"
  }
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Projekt-ID. |
| `name` | Ja | Nutzerlesbarer Projektname. |
| `description` | Nein | Optionale Beschreibung. |
| `createdAt` | Nein | Erzeugungszeitpunkt, bevorzugt ISO-8601. |
| `updatedAt` | Nein | Letzter Änderungszeitpunkt, bevorzugt ISO-8601. |
| `revision` | Post-MVP | Optimistic Locking oder Versionierung. |

Regeln:

- `project.id` muss innerhalb des Persistenzkontexts eindeutig sein.
- `project.name` darf nicht leer sein.
- Zeitstempel sind technische Metadaten und nicht Teil der UML/OCL-Semantik.

## UML-Modell

## Modelltext

Der Bereich `modelText` speichert den vollständigen Text, der im OCL Editor angezeigt wird. Der Screenshot `13-ocl-editor.png` zeigt dabei nicht nur einzelne OCL-Ausdrücke, sondern einen ganzen USE-ähnlichen Modelltext mit `model`, `class`, `attributes`, `association` und `constraints`.

```json
{
  "modelText": {
    "text": "model Library\n\nclass Book\nattributes\n  title : String\nend\n\nclass User\nattributes\n  books : Integer\nend\n\nconstraints\ncontext User inv maxBooks:\n  self.books <= 5\n",
    "language": "USE_MODEL_TEXT",
    "languageVersion": "mvp-subset",
    "updatedAt": "2026-07-22T20:00:00Z"
  }
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `text` | Ja, wenn `modelText` vorhanden ist | Vollständiger Editorinhalt. |
| `language` | Nein, empfohlen | Kennzeichnung als USE-ähnlicher Modelltext, nicht als reines OCL. |
| `languageVersion` | Nein, empfohlen | Unterstützter Sprachumfang, z. B. `mvp-subset`. |
| `updatedAt` | Nein | Letzte Textänderung. |

Wichtige Regeln:

- `modelText.text` ist keine fachliche Wahrheit, solange er nicht über `model-text/apply` erfolgreich verarbeitet wurde.
- Das Backend erzeugt oder aktualisiert daraus `umlModel` und `umlModel.invariants` für das unterstützte MVP-Subset.
- Nicht unterstützte Konstrukte wie `import`, Vererbung oder `associationclass` können im Text sichtbar bleiben, müssen aber als Diagnostics gemeldet werden, wenn sie nicht angewendet werden.
- JSON bleibt das persistente kanonische Projektformat; der Modelltext ist eine editierbare Quelle/Ansicht.

Der Bereich `umlModel` enthält das statische UML-Klassenmodell.

```json
{
  "umlModel": {
    "id": "uml-library",
    "name": "Library",
    "primitiveTypes": ["String", "Integer", "Real", "Boolean"],
    "classes": [],
    "associations": [],
    "invariants": []
  }
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile ID des UML-Modells. |
| `name` | Nein | Nutzerlesbarer Modellname. |
| `primitiveTypes` | Nein | Dokumentiert verfügbare primitive Typen; im MVP ableitbar. |
| `classes` | Ja | Liste der UML-Klassen. |
| `associations` | Ja | Liste der UML-Associations. |
| `invariants` | Ja | Liste der OCL-Invarianten. |
| `enumerations` | Post-MVP | Enumerationen. |
| `generalizations` | Post-MVP | Vererbungsbeziehungen. |

### Klassen

```json
{
  "id": "class-user",
  "name": "User",
  "attributes": [],
  "operations": []
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Klassen-ID. |
| `name` | Ja | Klassenname, relevant für UI und spätere `.use`-Syntax. |
| `attributes` | Ja | Attribute der Klasse. |
| `operations` | Ja | Operationen als Signaturen. |
| `isAbstract` | Post-MVP | Abstrakte Klassen. |
| `superClassIds` | Post-MVP | Vererbung. |

### Attribute

```json
{
  "id": "attr-user-name",
  "name": "name",
  "type": "String"
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Attribut-ID. |
| `name` | Ja | Attributname. |
| `type` | Ja | Typname, im MVP `String`, `Integer`, `Real` oder `Boolean`. |
| `visibility` | Post-MVP | Sichtbarkeit. |
| `isDerived` | Post-MVP | Kennzeichnung derived attribute. |
| `deriveExpression` | Post-MVP | OCL-Ausdruck für derived attribute. |
| `initExpression` | Post-MVP | OCL-Ausdruck für Initialwert. |

### Operationen

Operationen werden im MVP als Signaturen gespeichert, aber nicht ausgeführt.

```json
{
  "id": "op-user-can-borrow",
  "name": "canBorrow",
  "parameters": [
    {
      "id": "param-user-can-borrow-book",
      "name": "book",
      "type": "Book"
    }
  ],
  "returnType": "Boolean"
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Operations-ID. |
| `name` | Ja | Operationsname. |
| `parameters` | Ja | Parameterliste, im MVP auch leer möglich. |
| `returnType` | Nein | Rückgabetyp; bei fehlendem Rückgabewert optional `Void` oder `null`. |
| `preconditions` | Post-MVP | OCL-Preconditions. |
| `postconditions` | Post-MVP | OCL-Postconditions. |

Parameter:

```json
{
  "id": "param-book",
  "name": "book",
  "type": "Book"
}
```

### Assoziationen

```json
{
  "id": "assoc-borrows",
  "name": "Borrows",
  "ends": [
    {
      "id": "end-borrows-user",
      "classId": "class-user",
      "roleName": "borrower",
      "multiplicity": {
        "lower": 0,
        "upper": 1,
        "unbounded": false,
        "raw": "0..1"
      },
      "navigable": true
    },
    {
      "id": "end-borrows-books",
      "classId": "class-book",
      "roleName": "borrowedBooks",
      "multiplicity": {
        "lower": 0,
        "upper": 5,
        "unbounded": false,
        "raw": "0..5"
      },
      "navigable": true
    }
  ]
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Association-ID. |
| `name` | Ja | Association-Name. |
| `ends` | Ja | Im MVP genau zwei Association Ends. |
| `kind` | Post-MVP | z. B. normale Association, Aggregation, Komposition. |
| `associationClassId` | Post-MVP | Referenz auf Association Class. |

Association End:

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Association-End-ID. |
| `classId` | Ja | Zielklasse dieses Ends. |
| `roleName` | Ja | Rollenname, relevant für OCL-Navigation. |
| `multiplicity` | Ja | Strukturierte Multiplizität. |
| `navigable` | Nein, empfohlen | Im MVP kann `true` als Standard gelten. |
| `aggregationKind` | Post-MVP | `none`, `shared`, `composite`. |

Multiplicity:

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `lower` | Ja | Untere Grenze, Ganzzahl >= 0. |
| `upper` | Nein | Obere Grenze, `null` bei unbeschränkt. |
| `unbounded` | Ja | `true`, wenn obere Grenze `*` ist. |
| `raw` | Nein | Ursprüngliche UI-Notation, z. B. `0..*`. |

Regeln:

- Wenn `unbounded = true`, sollte `upper = null` sein.
- Wenn `unbounded = false`, muss `upper` eine Ganzzahl sein.
- `lower` darf nicht größer als `upper` sein, außer `unbounded = true`.

## Invarianten

Invarianten liegen im `umlModel`, weil sie fachlich Constraints des UML-Modells sind. Sie werden aber gegen das `objectModel` ausgewertet.

```json
{
  "id": "inv-user-max-books",
  "name": "maxBooks",
  "contextClassId": "class-user",
  "enabled": true,
  "expression": {
    "id": "expr-user-max-books",
    "text": "self.borrowedBooks->size() <= 5",
    "language": "OCL",
    "languageVersion": "mvp"
  }
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Invarianten-ID. |
| `name` | Ja | Nutzerlesbarer Name. |
| `contextClassId` | Ja | Klasse, auf deren Instanzen `self` zeigt. |
| `enabled` | Nein, empfohlen | Steuert, ob die Invariante beim Check berücksichtigt wird. |
| `expression` | Ja | OCL-Ausdruck. |

Expression:

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Expression-ID für Source Ranges und UI-Mapping. |
| `text` | Ja | OCL-Text. |
| `language` | Nein | `OCL`, falls mehrere Ausdruckssprachen später denkbar sind. |
| `languageVersion` | Nein | MVP-Subset oder spätere OCL-Version. |

Nicht im MVP persistieren:

| Daten | Grund |
|---|---|
| Lexer Tokens | Können aus `text` neu erzeugt werden. |
| AST | Kann aus `text` neu geparst werden. |
| Typed AST | Hängt vom aktuellen UML-Modell ab. |
| Evaluation Result | Hängt vom aktuellen Snapshot ab. |

## Objektmodell / Snapshot

Der Bereich `objectModel` enthält den aktuellen prüfbaren Zustand.

```json
{
  "objectModel": {
    "id": "snapshot-current",
    "name": "Current Snapshot",
    "objects": [],
    "links": []
  }
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Snapshot-ID. |
| `name` | Ja | Nutzerlesbarer Snapshot-Name. |
| `objects` | Ja | Objektinstanzen. |
| `links` | Ja | Objektlinks. |
| `createdAt` | Post-MVP | Snapshot-Zeitpunkt. |

### Objekte

```json
{
  "id": "obj-alice",
  "name": "alice",
  "classId": "class-user",
  "slots": []
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Objekt-ID. |
| `name` | Ja | Objektname, z. B. `alice`. |
| `classId` | Ja | Klasse des Objekts. |
| `slots` | Ja | Attributwerte des Objekts. |

### Slots

Empfohlenes MVP-Format:

```json
{
  "id": "slot-alice-books",
  "attributeId": "attr-user-books",
  "value": {
    "type": "Integer",
    "value": 6
  },
  "isUnset": false
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Slot-ID. |
| `attributeId` | Ja | Referenz auf `UmlAttribute`. |
| `value` | Nein | Typisierter Wert. |
| `isUnset` | Nein | Kennzeichnet fehlenden oder noch nicht gesetzten Wert. |

Typisierte Werte:

```json
{ "type": "String", "value": "Alice" }
```

```json
{ "type": "Integer", "value": 6 }
```

```json
{ "type": "Real", "value": 4.5 }
```

```json
{ "type": "Boolean", "value": false }
```

Regeln:

- `value.type` muss zum Attributtyp passen.
- Wenn `isUnset = true`, darf `value` `null` sein.
- Fehlende Slots sind beim Laden zulässig, aber bei Validierung zu melden oder zu ergänzen. Die genaue MVP-Regel ist offen.

### Objektlinks

End-basiertes Format, empfohlen für Erweiterbarkeit:

```json
{
  "id": "link-alice-moby",
  "associationId": "assoc-borrows",
  "endValues": [
    {
      "associationEndId": "end-borrows-user",
      "objectId": "obj-alice"
    },
    {
      "associationEndId": "end-borrows-books",
      "objectId": "obj-moby-dick"
    }
  ]
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `id` | Ja | Stabile Link-ID. |
| `associationId` | Ja | Referenz auf `UmlAssociation`. |
| `endValues` | Ja | Zuordnung Association End zu Object. |

Warum end-basiert?

| Vorteil | Beschreibung |
|---|---|
| näher an UML | Links referenzieren Association Ends statt impliziter Source/Target-Richtung. |
| n-är erweiterbar | Mehr als zwei Ends können später ergänzt werden. |
| bessere Validierung | Jeder End-Wert kann konkret gegen seine Klasse geprüft werden. |
| klareres Mapping | Fehler können Association Ends und Objekte direkt referenzieren. |

Für API-Kommandos kann im MVP zusätzlich ein vereinfachtes `sourceObjectId`/`targetObjectId`-DTO verwendet werden. Das Persistenzformat sollte jedoch möglichst end-basiert bleiben.

## Layoutinformationen

Der Bereich `layout` speichert UI-Zustand und Diagrammpositionen. Layoutdaten sind optional und nicht semantisch.

```json
{
  "layout": {
    "classDiagram": {
      "nodes": [
        {
          "elementId": "class-user",
          "x": 420,
          "y": 120,
          "width": 220,
          "height": 160
        }
      ],
      "edges": [
        {
          "elementId": "assoc-borrows",
          "labelPosition": {
            "x": 320,
            "y": 180
          }
        }
      ],
      "viewport": {
        "x": 0,
        "y": 0,
        "zoom": 1
      }
    },
    "objectDiagram": {
      "nodes": [
        {
          "elementId": "obj-alice",
          "x": 420,
          "y": 140,
          "width": 200,
          "height": 120
        }
      ],
      "edges": [
        {
          "elementId": "link-alice-moby",
          "labelPosition": {
            "x": 310,
            "y": 170
          }
        }
      ],
      "viewport": {
        "x": 0,
        "y": 0,
        "zoom": 1
      }
    }
  }
}
```

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `classDiagram.nodes` | Nein | Positionen von Klassen und ggf. Invarianten. |
| `classDiagram.edges` | Nein | Layoutdaten für Association-Kanten. |
| `objectDiagram.nodes` | Nein | Positionen von Objekten. |
| `objectDiagram.edges` | Nein | Layoutdaten für Objektlinks. |
| `viewport` | Nein | Scroll-/Zoomzustand. |

Regeln:

- `elementId` referenziert fachliche IDs.
- Layoutreferenzen auf gelöschte Elemente dürfen beim Laden ignoriert, bereinigt oder als Warning gemeldet werden.
- Layout darf fehlen; Frontend kann Default-Layout erzeugen.
- Backend interpretiert Layout nicht als UML/OCL-Semantik.

## Validation Results

Validation Results sind Ergebnisse eines Prüflaufs. Sie sollten im MVP grundsätzlich nicht als verbindlicher Projektbestandteil persistiert werden, weil sie aus `umlModel`, `objectModel` und OCL-Ausdrücken neu berechnet werden können.

Optional kann ein transienter Bereich gespeichert werden:

```json
{
  "validationState": {
    "lastCheckedAt": "2026-07-11T20:35:00Z",
    "status": "INVALID",
    "summary": {
      "errorCount": 1,
      "warningCount": 0,
      "infoCount": 0
    }
  }
}
```

Nicht empfohlen für dauerhafte Persistenz im MVP:

```json
{
  "validationState": {
    "lastResult": {
      "errors": [
        {
          "code": "INVARIANT_VIOLATION",
          "objectIds": ["obj-alice"]
        }
      ]
    }
  }
}
```

Empfehlung:

| Daten | Persistieren? | Begründung |
|---|---|---|
| `lastCheckedAt` | optional | UI kann letzten Check anzeigen. |
| `status`/`summary` | optional | Komfortinformation. |
| vollständige Fehlerliste | eher nein | Kann veralten und wird neu berechnet. |
| Evaluation Trace | nein | Debug-/Laufzeitdaten. |

## Vollständiges Library-Beispiel

Das folgende Beispiel zeigt ein vollständiges MVP-Projekt mit Klassen, Attributen, Operationensignatur, Association, Invariante, Snapshot, Links und Layout.

```json
{
  "formatVersion": "0.1",
  "project": {
    "id": "project-library",
    "name": "Library Example",
    "description": "MVP example for UML/OCL validation",
    "createdAt": "2026-07-11T20:00:00Z",
    "updatedAt": "2026-07-11T20:30:00Z"
  },
  "umlModel": {
    "id": "uml-library",
    "name": "Library",
    "primitiveTypes": ["String", "Integer", "Real", "Boolean"],
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
            "id": "op-user-can-borrow",
            "name": "canBorrow",
            "parameters": [
              {
                "id": "param-user-can-borrow-book",
                "name": "book",
                "type": "Book"
              }
            ],
            "returnType": "Boolean"
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
            "id": "attr-book-author",
            "name": "author",
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
            "id": "end-borrows-user",
            "classId": "class-user",
            "roleName": "borrower",
            "multiplicity": {
              "lower": 0,
              "upper": 1,
              "unbounded": false,
              "raw": "0..1"
            },
            "navigable": true
          },
          {
            "id": "end-borrows-books",
            "classId": "class-book",
            "roleName": "borrowedBooks",
            "multiplicity": {
              "lower": 0,
              "upper": 5,
              "unbounded": false,
              "raw": "0..5"
            },
            "navigable": true
          }
        ]
      }
    ],
    "invariants": [
      {
        "id": "inv-user-max-books",
        "name": "maxBooks",
        "contextClassId": "class-user",
        "enabled": true,
        "expression": {
          "id": "expr-user-max-books",
          "text": "self.books <= 5",
          "language": "OCL",
          "languageVersion": "mvp"
        }
      },
      {
        "id": "inv-user-name-required",
        "name": "nameRequired",
        "contextClassId": "class-user",
        "enabled": true,
        "expression": {
          "id": "expr-user-name-required",
          "text": "self.name <> ''",
          "language": "OCL",
          "languageVersion": "mvp"
        }
      },
      {
        "id": "inv-book-not-available",
        "name": "borrowedBooksUnavailable",
        "contextClassId": "class-book",
        "enabled": true,
        "expression": {
          "id": "expr-book-not-available",
          "text": "self.available = false",
          "language": "OCL",
          "languageVersion": "mvp"
        }
      },
      {
        "id": "inv-user-borrowed-books-size",
        "name": "borrowedBooksSize",
        "contextClassId": "class-user",
        "enabled": true,
        "expression": {
          "id": "expr-user-borrowed-books-size",
          "text": "self.borrowedBooks->size() <= 5",
          "language": "OCL",
          "languageVersion": "mvp"
        }
      },
      {
        "id": "inv-user-has-borrowed-books",
        "name": "hasBorrowedBooks",
        "contextClassId": "class-user",
        "enabled": true,
        "expression": {
          "id": "expr-user-has-borrowed-books",
          "text": "self.borrowedBooks->notEmpty()",
          "language": "OCL",
          "languageVersion": "mvp"
        }
      }
    ]
  },
  "objectModel": {
    "id": "snapshot-current",
    "name": "Current Snapshot",
    "objects": [
      {
        "id": "obj-alice",
        "name": "alice",
        "classId": "class-user",
        "slots": [
          {
            "id": "slot-alice-name",
            "attributeId": "attr-user-name",
            "value": {
              "type": "String",
              "value": "Alice"
            },
            "isUnset": false
          },
          {
            "id": "slot-alice-books",
            "attributeId": "attr-user-books",
            "value": {
              "type": "Integer",
              "value": 6
            },
            "isUnset": false
          }
        ]
      },
      {
        "id": "obj-moby-dick",
        "name": "mobyDick",
        "classId": "class-book",
        "slots": [
          {
            "id": "slot-moby-title",
            "attributeId": "attr-book-title",
            "value": {
              "type": "String",
              "value": "Moby Dick"
            },
            "isUnset": false
          },
          {
            "id": "slot-moby-author",
            "attributeId": "attr-book-author",
            "value": {
              "type": "String",
              "value": "Herman Melville"
            },
            "isUnset": false
          },
          {
            "id": "slot-moby-available",
            "attributeId": "attr-book-available",
            "value": {
              "type": "Boolean",
              "value": false
            },
            "isUnset": false
          }
        ]
      },
      {
        "id": "obj-ulysses",
        "name": "ulysses",
        "classId": "class-book",
        "slots": [
          {
            "id": "slot-ulysses-title",
            "attributeId": "attr-book-title",
            "value": {
              "type": "String",
              "value": "Ulysses"
            },
            "isUnset": false
          },
          {
            "id": "slot-ulysses-author",
            "attributeId": "attr-book-author",
            "value": {
              "type": "String",
              "value": "James Joyce"
            },
            "isUnset": false
          },
          {
            "id": "slot-ulysses-available",
            "attributeId": "attr-book-available",
            "value": {
              "type": "Boolean",
              "value": false
            },
            "isUnset": false
          }
        ]
      }
    ],
    "links": [
      {
        "id": "link-alice-moby",
        "associationId": "assoc-borrows",
        "endValues": [
          {
            "associationEndId": "end-borrows-user",
            "objectId": "obj-alice"
          },
          {
            "associationEndId": "end-borrows-books",
            "objectId": "obj-moby-dick"
          }
        ]
      },
      {
        "id": "link-alice-ulysses",
        "associationId": "assoc-borrows",
        "endValues": [
          {
            "associationEndId": "end-borrows-user",
            "objectId": "obj-alice"
          },
          {
            "associationEndId": "end-borrows-books",
            "objectId": "obj-ulysses"
          }
        ]
      }
    ]
  },
  "layout": {
    "classDiagram": {
      "nodes": [
        {
          "elementId": "class-user",
          "x": 420,
          "y": 120,
          "width": 220,
          "height": 150
        },
        {
          "elementId": "class-book",
          "x": 120,
          "y": 120,
          "width": 220,
          "height": 170
        }
      ],
      "edges": [
        {
          "elementId": "assoc-borrows",
          "labelPosition": {
            "x": 320,
            "y": 180
          }
        }
      ],
      "viewport": {
        "x": 0,
        "y": 0,
        "zoom": 1
      }
    },
    "objectDiagram": {
      "nodes": [
        {
          "elementId": "obj-alice",
          "x": 420,
          "y": 140,
          "width": 220,
          "height": 130
        },
        {
          "elementId": "obj-moby-dick",
          "x": 120,
          "y": 80,
          "width": 220,
          "height": 150
        },
        {
          "elementId": "obj-ulysses",
          "x": 120,
          "y": 280,
          "width": 220,
          "height": 150
        }
      ],
      "edges": [
        {
          "elementId": "link-alice-moby",
          "labelPosition": {
            "x": 320,
            "y": 130
          }
        },
        {
          "elementId": "link-alice-ulysses",
          "labelPosition": {
            "x": 320,
            "y": 290
          }
        }
      ],
      "viewport": {
        "x": 0,
        "y": 0,
        "zoom": 1
      }
    }
  },
  "validationState": {
    "lastCheckedAt": "2026-07-11T20:35:00Z",
    "status": "INVALID",
    "summary": {
      "errorCount": 1,
      "warningCount": 0,
      "infoCount": 0
    }
  },
  "extensions": {}
}
```

## Versionierung

Das Projektformat braucht eine eigene Version, unabhängig von der REST-API-Version.

| Feld | Beispiel | Zweck |
|---|---|---|
| `formatVersion` | `"0.1"` | Erlaubt Formatvalidierung und spätere Migration. |
| API-Version | `/api/v1` | Versioniert HTTP-Vertrag, nicht gespeicherte Projektdateien. |

Versionierungsstrategie:

| Änderung | Beispiel | Strategie |
|---|---|---|
| rein additive Felder | `layout.viewport` | gleiche Minor-Version möglich, Leser ignorieren unbekannte Felder. |
| neue optionale Bereiche | `extensions`, `validationState` | additive Erweiterung. |
| Pflichtfeld ergänzt | `objectModel.snapshots` statt `objectModel` | neue Formatversion und Migration. |
| Feld umbenannt | `links` zu `objectLinks` | neue Formatversion und Migration. |
| Semantik geändert | `multiplicity.upper = -1` statt `unbounded` | vermeiden; falls nötig Migration. |

Empfehlung:

- MVP startet mit `formatVersion: "0.1"`.
- Backend akzeptiert nur bekannte Major-Versionen.
- Import älterer kompatibler Minor-Versionen kann über Migration erfolgen.
- Export schreibt immer die aktuelle Backend-Formatversion.

## Erweiterbarkeit

Das Format soll später wachsen können, ohne MVP-Dokumente zu brechen.

### Umgang mit unbekannten Feldern

| Situation | Empfehlung |
|---|---|
| unbekanntes Feld in bekanntem Objekt | Beim Laden ignorieren oder in `extensions` erhalten, sofern möglich. |
| unbekannter Top-Level-Bereich | Ignorieren, wenn `formatVersion` kompatibel ist. |
| unbekannter Typ in Pflichtfeld | Format- oder Domain-Fehler melden. |
| unbekannte `formatVersion` | Import ablehnen oder Migration verlangen. |

### Erweiterungspunkte

```json
{
  "extensions": {
    "experimentalFeature": {
      "enabled": true
    }
  }
}
```

Mögliche Post-MVP-Erweiterungen:

| Erweiterung | Möglicher JSON-Bereich |
|---|---|
| mehrere Snapshots | `objectModels` oder `snapshots` |
| Vererbung | `umlModel.generalizations` |
| Enumerationen | `umlModel.enumerations` |
| Aggregation/Komposition | `association.ends[].aggregationKind` |
| Association Classes | `association.associationClassId` |
| derived attributes | `attributes[].deriveExpression` |
| init values | `attributes[].initExpression` |
| pre/post conditions | `operations[].preconditions`, `operations[].postconditions` |
| `.use` Importmetadaten | `extensions.useImport` |
| Projektversionierung | `project.revision`, `versions` |

## Beziehung zur REST API

Das JSON-Projektformat ist eng mit `ProjectDto` verwandt, aber nicht zwangsläufig identisch mit jedem Create/Update-Request.

| API-Fall | Beziehung zum Format |
|---|---|
| `GET /api/v1/projects/{projectId}` | Kann das Projektformat oder eine sehr nahe DTO-Form zurückgeben. |
| `PUT /api/v1/projects/{projectId}` | Kann vollständiges Projektformat speichern. |
| `GET /api/v1/projects/{projectId}/export` | Sollte genau dieses Format liefern. |
| `POST /api/v1/projects/import` | Erwartet genau dieses Format. |
| CRUD-Endpunkte | Nutzen kleinere Request-/Response-DTOs, aktualisieren aber denselben Projektzustand. |
| `POST /api/v1/projects/{projectId}/validate` | Liest Projektzustand, erzeugt aber ein separates Validation Result. |

DTO-Abweichungen sind erlaubt, wenn sie bewusst dokumentiert sind. Beispiel: `POST /api/v1/projects/{id}/links` kann im Request `sourceObjectId` und `targetObjectId` akzeptieren, während das gespeicherte Format `endValues` nutzt.

## Validierungsregeln beim Laden

Beim Laden oder Import müssen Formatvalidierung und fachliche Validierung getrennt werden.

### Formatvalidierung

Formatfehler verhindern Import oder Laden.

| Regel | Fehlerbehandlung |
|---|---|
| JSON ist syntaktisch gültig. | Sonst `BAD_REQUEST` / Importfehler. |
| `formatVersion` existiert und ist unterstützt. | Sonst Import ablehnen oder Migration verlangen. |
| Top-Level-Pflichtfelder existieren. | Sonst Import ablehnen. |
| Pflichtlisten sind Arrays. | Sonst Import ablehnen. |
| IDs sind Strings und nicht leer. | Sonst Import ablehnen. |
| Objektstruktur entspricht erwartetem Schema. | Sonst Import ablehnen. |

### Fachvalidierung

Fachliche Fehler dürfen ein Projekt ladbar lassen, damit Nutzer sie sehen und korrigieren können.

| Regel | Fehlercode bei `Check Constraints` |
|---|---|
| Klassenreferenzen in Association Ends existieren. | `UNKNOWN_CLASS` |
| Attributtypen sind bekannt. | `TYPE_ERROR` |
| Invarianten-Kontextklasse existiert. | `UNKNOWN_CLASS` |
| Objekte referenzieren existierende Klassen. | `UNKNOWN_CLASS` |
| Slots referenzieren Attribute der Objektklasse. | `UNKNOWN_ATTRIBUTE` |
| Slot-Werte passen zum Attributtyp. | `INVALID_SLOT_VALUE` |
| Links referenzieren existierende Associations. | `INVALID_LINK` |
| Link-End-Werte passen zu Association Ends. | `INVALID_LINK` |
| Multiplizitäten werden eingehalten. | `MULTIPLICITY_VIOLATION` |
| OCL-Ausdrücke sind syntaktisch gültig. | `SYNTAX_ERROR` |
| OCL-Ausdrücke sind typkorrekt. | `TYPE_ERROR` |

Empfehlung:

- Import mit Formatfehlern ablehnen.
- Import mit fachlichen Modellfehlern erlauben, aber beim ersten `Check Constraints` klar melden.
- Optional nach Import automatisch eine Validierung anbieten.

## Bezug zum originalen USE-Projekt

Das JSON-Format ist eine neue Projektrepräsentation. Es soll fachliche USE-Konzepte abbilden, aber keine USE-Dateien nachahmen.

| USE-Konzept | JSON-Format | Bedeutung |
|---|---|---|
| `.use` Modelltext | `modelText.text` plus abgeleitetes `umlModel` | MVP zeigt vollständigen Editor-Text, verarbeitet aber nur ein klar begrenztes USE-ähnliches Subset. |
| `MModel` | `umlModel` | Klassen, Associations, Invarianten. |
| `MClass` | `umlModel.classes[]` | Klasse. |
| `MAttribute` | `classes[].attributes[]` | Attribut. |
| `MOperation` | `classes[].operations[]` | Operationensignatur im MVP. |
| `MAssociation` | `umlModel.associations[]` | Association. |
| `MAssociationEnd` | `associations[].ends[]` | Rolle, Klasse, Multiplizität. |
| `MMultiplicity` | `multiplicity` | Explizite JSON-Struktur statt interner USE-Repräsentation. |
| `MClassInvariant` | `umlModel.invariants[]` | OCL-Invariante. |
| `MSystemState` | `objectModel` | Snapshot. |
| `MObject` | `objectModel.objects[]` | Objektinstanz. |
| `MObjectState` | `objects[].slots[]` | Attributwerte. |
| `MLink` | `objectModel.links[]` | Objektlink. |

Späterer vollständiger `.use` Import/Export:

- `.use` Parser/Exporter kann JSON-Domainobjekte erzeugen oder daraus Text generieren.
- IDs müssen bei Import neu erzeugt oder stabil aus Namen abgeleitet werden.
- `.cmd`-artige Snapshot-Erzeugung ist Post-MVP.
- Nicht alle USE-Features müssen in das MVP-Format passen.
- Der MVP-Apply-Flow für `modelText` ist davon abzugrenzen: Er dient dem Editor-Workflow und unterstützt nur die vereinbarten Konstrukte.

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Soll `project.id` top-level liegen oder ausschließlich unter `project.id`? | Aktuelle Empfehlung: unter `project`, `formatVersion` bleibt top-level. |
| Soll `umlModel.primitiveTypes` gespeichert oder implizit vom Backend bereitgestellt werden? | Speicherung erhöht Transparenz, kann aber redundant sein. |
| Wird `returnType: "Void"` im MVP unterstützt oder besser `null` genutzt? | Muss mit Typmodell abgestimmt werden. |
| Werden fehlende Slots beim Laden ergänzt oder als Validation Error gemeldet? | Beeinflusst Import und UX. |
| Soll das Persistenzformat `endValues` nutzen, während API-Commands `sourceObjectId`/`targetObjectId` nutzen? | Empfehlung: ja, aber Mapper sauber dokumentieren. |
| Werden vollständige Validation Results jemals im Projekt gespeichert? | Für MVP nicht empfohlen. |
| Soll unbekanntes Layout automatisch bereinigt oder unverändert exportiert werden? | Beeinflusst Roundtrip-Verhalten. |
| Soll `modelText` aus `umlModel` generiert oder als letzte Nutzerquelle gespeichert werden? | Empfehlung: beides unterstützen; gespeicherter Text bleibt Editor-Draft, strukturierte Daten bleiben fachliche Semantik. |
| Wie werden Namen bei `.use` Import in stabile IDs überführt? | Relevant für Post-MVP Import/Export. |

## Zusammenfassung

Das JSON-Projektformat ist das zentrale MVP-Speicher- und Austauschformat des neuen UML/OCL-Websystems. Es speichert Projektmetadaten, optionalen vollständigen Modelltext für den OCL Editor, UML-Modell, Invarianten, aktuellen Snapshot, Objektlinks und Layoutinformationen in einer frontend- und backend-tauglichen Struktur.

Das Format verwendet stabile IDs, trennt fachliche Semantik von Layoutdaten und behandelt Validation Results als neu berechenbare, optionale Laufzeitdaten. Es ist bewusst nicht identisch zur originalen USE-Syntax, bildet aber zentrale USE-Konzepte so ab, dass der MVP-Editor vollständige USE-ähnliche Texte anzeigen kann und späterer vollständiger `.use` Import/Export möglich bleibt.
