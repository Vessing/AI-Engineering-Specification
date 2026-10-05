# Object Model Service

## Zweck dieser Datei

Diese Datei beschreibt den Object Model Service beziehungsweise Snapshot Service des neuen Backends.

Der Service verwaltet den konkreten Objektzustand eines Projekts:

- Objektinstanzen,
- Objektnamen,
- Klassenzuordnung,
- Attributwerte beziehungsweise Slots,
- Objektlinks,
- aktuellen Snapshot-Zustand,
- Beziehung zum UML-Modell,
- Bereitstellung des Objektzustands für Validierung und OCL-Evaluation.

Das originale USE-Projekt dient als fachliche Referenz für Objektdiagramme, Systemzustände, Objektzustände und Links, insbesondere durch Konzepte wie `MSystemState`, `MObject`, `MObjectState`, `MLink` und `MLinkSet`. Die technische Umsetzung wird neu aufgebaut.

## Rolle des Object Model Service

Der Object Model Service ist die Backend-Komponente für den konkreten Zustand eines Modells. Während der UML Model Service die Struktur beschreibt, verwaltet der Object Model Service Instanzen dieser Struktur.

| Aufgabe | Beschreibung |
|---|---|
| Objekte verwalten | Objektinstanzen für vorhandene Klassen erstellen, ändern und löschen. |
| Slots verwalten | Attributwerte pro Objekt speichern und typbezogen prüfen. |
| Objektlinks verwalten | Links zwischen Objekten auf Basis von UML-Associations erstellen und löschen. |
| Snapshot bereitstellen | Einen prüfbaren Objektzustand für Validation Service und OCL Evaluator liefern. |
| Modellbezug prüfen | Sicherstellen oder melden, ob Objekte, Slots und Links zum UML-Modell passen. |
| Fehlerreferenzen ermöglichen | Stabile IDs für Objekt- und Link-Markierungen liefern. |

Nicht-Verantwortlichkeiten:

| Nicht-Aufgabe | Zuständig |
|---|---|
| Klassen, Attribute und Associations definieren | UML Model Service |
| OCL-Ausdrücke parsen oder typprüfen | OCL Service / OCL Typechecker |
| Invarianten auswerten | OCL Evaluator / Validation Service |
| Diagramm-Rendering | Frontend |
| Layoutberechnung | Frontend oder später separater Layoutdienst |

## Snapshot-Konzept

Ein Snapshot ist ein konkreter Zustand eines UML-Modells. Er enthält Objekte, deren Attributwerte und Links zwischen Objekten.

Im MVP gibt es pro Projekt einen aktiven Snapshot:

```text
Project
├─ UmlModel
│  ├─ Classes
│  ├─ Associations
│  └─ Invariants
└─ ObjectModel / Current Snapshot
   ├─ ObjectInstances
   ├─ Slots
   └─ ObjectLinks
```

Unterschied zwischen UML-Modell und Objektmodell:

| UML-Modell | Objektmodell / Snapshot |
|---|---|
| Definiert Klassen. | Enthält Objekte dieser Klassen. |
| Definiert Attribute. | Enthält Slot-Werte für Attribute. |
| Definiert Associations. | Enthält konkrete Objektlinks. |
| Definiert Multiplizitäten. | Wird gegen Linkanzahlen geprüft. |
| Definiert Invarianten. | Wird zur Auswertung der Invarianten verwendet. |
| Ist strukturell. | Ist ein konkreter Zustand. |

Das originale USE-Projekt bildet einen solchen Zustand über `MSystemState` ab. Das neue Backend nutzt ein eigenes `ObjectModel` oder `Snapshot`-Modell.

## Objektinstanzen

Eine Objektinstanz ist ein konkretes Objekt einer `UmlClass`.

MVP-Felder:

| Feld | Zweck |
|---|---|
| `id` | Stabile technische ID für API, Layout und Validation Results. |
| `name` | Nutzerlesbarer Objektname, z. B. `alice`. |
| `classId` | Referenz auf die Klasse des Objekts. |
| `slots` | Attributwerte dieses Objekts. |

Beispiel:

```json
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
      }
    }
  ]
}
```

Regeln:

- `classId` muss auf eine existierende `UmlClass` zeigen.
- Objektname sollte im Snapshot eindeutig sein.
- Jedes Objekt besitzt stabile Identität über `id`; der Name ist ein Label.
- Bei Objekterstellung können Slots für alle Attribute der Klasse vorbereitet werden.
- Fehlende Slots müssen im MVP klar geregelt werden: Fehler, Warning oder implizit unset.

Empfehlung für den MVP:

- Objekt-ID ist technische Identität.
- Objektname ist eindeutig im Snapshot.
- Für jedes Attribut der Klasse sollte ein Slot vorhanden sein.
- Slot-Werte dürfen zunächst unset sein, wenn die Validierungsregeln das klar melden können.

## Slots und Attributwerte

Ein Slot ist der Wert eines Attributes für ein konkretes Objekt.

| Feld | Zweck |
|---|---|
| `id` | Stabile Slot-ID. |
| `attributeId` | Referenz auf `UmlAttribute`. |
| `value.type` | Serialisierter Werttyp. |
| `value.value` | Konkreter Wert. |
| optional `isUnset` | Kennzeichnung für nicht gesetzten Wert. |

Beispiel:

```json
{
  "id": "slot-alice-age",
  "attributeId": "attr-user-age",
  "value": {
    "type": "Integer",
    "value": 31
  }
}
```

MVP-Werttypen:

| UML-Typ | JSON-Wert | Beispiel |
|---|---|---|
| `String` | String | `"Alice"` |
| `Integer` | ganze Zahl | `31` |
| `Real` | Dezimalzahl | `12.5` |
| `Boolean` | Boolean | `true` |

Typisierungsregeln:

- Der Slot referenziert ein Attribut.
- Das Attribut gehört zur Klasse des Objekts oder später zu einer Oberklasse.
- Der Werttyp muss zum Attributtyp passen.
- Ungültige Werte erzeugen `INVALID_SLOT_VALUE`.
- OCL-Attributzugriff liest Slot-Werte über `attributeId` oder über den Attributnamen nach Typechecking.

Bezug zu USE:

- Im Original hält `MObjectState` Attributwerte pro Objekt.
- Nicht gesetzte Werte können dort als `UndefinedValue` auftreten.
- Das neue MVP sollte Undefined/Unset einfacher behandeln, aber die spätere OCL-Undefined-Semantik vorbereiten.

## Objektlinks

Ein Objektlink ist eine konkrete Instanz einer UML-Association.

MVP-Felder für binäre Links:

| Feld | Zweck |
|---|---|
| `id` | Stabile Link-ID. |
| `associationId` | Referenz auf `UmlAssociation`. |
| `sourceObjectId` | Erstes beteiligtes Objekt. |
| `targetObjectId` | Zweites beteiligtes Objekt. |

Beispiel:

```json
{
  "id": "link-alice-mobydick",
  "associationId": "assoc-borrows",
  "sourceObjectId": "obj-alice",
  "targetObjectId": "obj-mobydick"
}
```

Validierungsregeln:

- `associationId` muss existieren.
- Source- und Target-Objekt müssen existieren.
- Die Objektklassen müssen zu den Association Ends passen.
- Linkanzahlen werden später im Validation Service gegen Multiplizitäten geprüft.
- Doppelte Links sollten entweder verhindert oder fachlich definiert werden.

Zukunftssichere Alternative:

```json
{
  "id": "link-alice-mobydick",
  "associationId": "assoc-borrows",
  "ends": [
    {
      "associationEndId": "end-borrows-user",
      "objectId": "obj-alice"
    },
    {
      "associationEndId": "end-borrows-books",
      "objectId": "obj-mobydick"
    }
  ]
}
```

Die End-basierte Form ist näher an UML und USE-Konzepten wie `MLinkEnd`, aber für den binären MVP aufwendiger. Die Architektur sollte eine spätere Umstellung ermöglichen.

## Beziehung zum UML-Modell

Der Object Model Service ist abhängig vom UML-Modell. Er darf Snapshots nicht losgelöst von Klassen, Attributen und Associations behandeln.

| Object Model Element | Benötigte UML-Information |
|---|---|
| `ObjectInstance.classId` | `UmlClass` muss existieren. |
| `Slot.attributeId` | `UmlAttribute` muss existieren und zur Objektklasse passen. |
| `Slot.value` | Attributtyp bestimmt zulässigen Werttyp. |
| `ObjectLink.associationId` | `UmlAssociation` muss existieren. |
| Link-Enden | Klassen der Objekte müssen zu Association Ends passen. |
| Linkanzahl | `Multiplicity` der Association Ends bestimmt gültige Kardinalität. |

Der Object Model Service sollte Modellreferenzen prüfen, aber komplexe Gesamtvalidierung an den Validation Service delegieren.

Beispiele für Folgen von UML-Änderungen:

| UML-Änderung | Auswirkung auf Snapshot |
|---|---|
| Klasse löschen | Objekte dieser Klasse werden ungültig. |
| Attribut löschen | Slots dieses Attributs werden ungültig. |
| Attributtyp ändern | bestehende Slot-Werte können ungültig werden. |
| Association löschen | Links dieser Association werden ungültig. |
| Association-End-Klasse ändern | bestehende Links können nicht mehr passen. |
| Multiplicity ändern | Snapshot kann neue Multiplicity Violations enthalten. |

## Beziehung zum OCL Evaluator

Der OCL Evaluator nutzt den Snapshot als Auswertungskontext.

Für eine Invariante:

```ocl
self.books->size() <= 5
```

braucht der Evaluator:

| Information | Quelle |
|---|---|
| Kontextklasse `User` | `UmlInvariant.contextClassId` |
| Alle Objekte der Klasse `User` | `ObjectModel.objects` |
| Konkretes `self` | aktuelles `ObjectInstance` während Evaluation |
| Rollennavigation `books` | `UmlAssociationEnd.roleName` und `ObjectModel.links` |
| Anzahl navigierter Bücher | Linkauswertung im Snapshot |
| Slot-Werte | `ObjectInstance.slots` |

Auswertungsprinzip:

1. Validation Service wählt alle Objekte der Kontextklasse.
2. Für jedes Objekt wird ein Evaluation Context erzeugt.
3. `self` zeigt auf dieses Objekt.
4. Attributzugriffe lesen Slots.
5. Navigation liest Links des Snapshots.
6. Ergebnis muss `Boolean` sein.
7. `false` erzeugt `INVARIANT_VIOLATION` mit betroffener Objekt-ID.

Der Object Model Service selbst wertet OCL nicht aus. Er stellt den benötigten Snapshot bereit.

## MVP-Funktionen

| Funktion | Beschreibung | Ergebnis |
|---|---|---|
| Objekt erstellen | Objekt mit Name, Klasse und initialen Slots anlegen. | Neues `ObjectInstance`. |
| Objekt löschen | Objekt entfernen und abhängige Links behandeln. | Snapshot ohne Objekt oder Konfliktmeldung. |
| Attributwert setzen | Slot-Wert für ein Objekt ändern. | Aktualisierter Slot. |
| Objektlink erstellen | Link auf Basis einer Association zwischen zwei Objekten anlegen. | Neuer `ObjectLink`. |
| Objektlink löschen | Link aus Snapshot entfernen. | Link ist nicht mehr Teil der Navigation. |
| Snapshot validieren | Snapshot-Struktur gegen UML-Modell prüfen oder Validation Service vorbereiten. | Strukturfehler oder Validation Result. |
| Snapshot für OCL bereitstellen | Objekte, Slots und Links lookup-fähig bereitstellen. | Evaluation Context kann erzeugt werden. |

Mögliche Service-Methoden:

```java
ObjectInstance createObject(ProjectId projectId, CreateObjectCommand command);
void deleteObject(ProjectId projectId, ObjectInstanceId objectId);
Slot setSlotValue(ProjectId projectId, ObjectInstanceId objectId, SetSlotValueCommand command);
ObjectLink createObjectLink(ProjectId projectId, CreateObjectLinkCommand command);
void deleteObjectLink(ProjectId projectId, ObjectLinkId linkId);
ObjectModel getCurrentSnapshot(ProjectId projectId);
```

## Post-MVP-Erweiterungen

| Erweiterung | Beschreibung |
|---|---|
| Mehrere Snapshots | Projekt kann mehrere benannte Objektzustände enthalten. |
| Snapshot-Historie | Änderungen am Objektzustand werden historisiert. |
| Snapshot-Vergleich | Zwei Snapshots können verglichen werden. |
| Undefined/Invalid-Semantik | Genauere OCL-konforme Behandlung fehlender oder ungültiger Werte. |
| Init Values | Objekte erhalten initiale Slot-Werte aus OCL-Ausdrücken. |
| Derived Attributes | Slot-Werte können berechnet statt gespeichert werden. |
| N-äre Links | Links mit mehr als zwei Enden. |
| Association Classes | Links mit eigenen Attributwerten. |
| Qualifizierte Links | Link-Enden mit Qualifier-Werten. |
| Import aus USE-Kommandos | `.cmd` oder SOIL-nahe Snapshot-Erzeugung später prüfen. |

## Validierungsregeln

| Regel | Fehlercode | Bemerkung |
|---|---|---|
| Objekt referenziert existierende Klasse. | `UNKNOWN_CLASS` | `classId` muss im UML-Modell existieren. |
| Objektname ist nicht leer. | `TYPE_ERROR` | Alternativ eigener Domain-Fehler. |
| Objektname ist im Snapshot eindeutig. | `TYPE_ERROR` oder `DUPLICATE_NAME` | Namen sind sichtbare Labels. |
| Slot referenziert existierendes Attribut. | `UNKNOWN_ATTRIBUTE` | Attribut muss zur Objektklasse passen. |
| Slot-Wert passt zum Attributtyp. | `INVALID_SLOT_VALUE` | Primitive Typprüfung im MVP. |
| Link referenziert existierende Association. | `INVALID_LINK` | Association muss im UML-Modell existieren. |
| Link referenziert existierende Objekte. | `INVALID_LINK` | Source/Target müssen im Snapshot existieren. |
| Link-Enden passen zu Association-End-Klassen. | `INVALID_LINK` | Klassenkompatibilität prüfen. |
| Linkanzahlen passen zu Multiplicities. | `MULTIPLICITY_VIOLATION` | Im Validation Service ausführen. |
| OCL-Auswertung kann Slot/Navigation lesen. | `EVALUATION_ERROR` | Wenn Snapshot unvollständig oder inkonsistent ist. |

Empfehlung für den MVP:

- Create/Update-Operationen sollten offensichtliche Strukturfehler ablehnen.
- Der vollständige `Check Constraints` meldet alle strukturellen und fachlichen Snapshot-Fehler gesammelt.
- Constraint-Verletzungen sind fachliche Ergebnisse, keine technischen Exceptions.

## API-Bezug

Mögliche Endpunkte:

| Methode | Pfad | Zweck |
|---|---|---|
| `GET` | `/projects/{projectId}/snapshot` | Aktuellen Snapshot abrufen. |
| `POST` | `/projects/{projectId}/snapshot/objects` | Objekt erstellen. |
| `PATCH` | `/projects/{projectId}/snapshot/objects/{objectId}` | Objektname oder Klasse ändern. |
| `DELETE` | `/projects/{projectId}/snapshot/objects/{objectId}` | Objekt löschen. |
| `PATCH` | `/projects/{projectId}/snapshot/objects/{objectId}/slots/{slotId}` | Slot-Wert setzen. |
| `POST` | `/projects/{projectId}/snapshot/links` | Objektlink erstellen. |
| `PATCH` | `/projects/{projectId}/snapshot/links/{linkId}` | Objektlink ändern. |
| `DELETE` | `/projects/{projectId}/snapshot/links/{linkId}` | Objektlink löschen. |
| `POST` | `/projects/{projectId}/validate` | Vollständige Snapshot- und Constraint-Validierung auslösen. |

Beispiel `CreateObjectRequest`:

```json
{
  "name": "alice",
  "classId": "class-user"
}
```

Beispiel `SetSlotValueRequest`:

```json
{
  "attributeId": "attr-user-name",
  "value": {
    "type": "String",
    "value": "Alice"
  }
}
```

Beispiel `CreateObjectLinkRequest`:

```json
{
  "associationId": "assoc-borrows",
  "sourceObjectId": "obj-alice",
  "targetObjectId": "obj-mobydick"
}
```

## Fehlerfälle

| Fehlerfall | Beispiel | Behandlung |
|---|---|---|
| Klasse unbekannt | Objekt soll für `class-unknown` erstellt werden. | `UNKNOWN_CLASS` oder API-Fehler. |
| Objektname leer | `name = ""`. | Request ablehnen. |
| Objektname doppelt | Zwei Objekte heißen `alice`. | Ablehnen oder Validation Error, Entscheidung offen. |
| Attribut unbekannt | Slot verweist auf gelöschtes Attribut. | `UNKNOWN_ATTRIBUTE`. |
| Werttyp falsch | `Integer`-Attribut erhält String. | `INVALID_SLOT_VALUE`. |
| Objektlink mit unbekannter Association | `associationId` existiert nicht. | `INVALID_LINK`. |
| Objektlink mit unbekanntem Objekt | `sourceObjectId` existiert nicht. | `INVALID_LINK`. |
| Link passt nicht zur Association | `Book` wird an `User`-Ende gesetzt. | `INVALID_LINK`. |
| Objekt mit Links löschen | Links würden verwaisen. | MVP: Links automatisch löschen und Layout-/Validation-Referenzen bereinigen. |
| Snapshot inkonsistent nach UML-Änderung | Klasse oder Attribut gelöscht. | Beim Check als Validation Result melden. |
| OCL-Navigation nicht auswertbar | Rolle existiert, aber Links sind defekt. | `EVALUATION_ERROR` oder vorgelagerter `INVALID_LINK`. |

## Beispiel: Library-Snapshot

Beispiel mit einem Nutzer, zwei Büchern und zwei Links:

```json
{
  "id": "snapshot-library-current",
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
          }
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
          "value": {
            "type": "String",
            "value": "Moby Dick"
          }
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
          }
        }
      ]
    }
  ],
  "links": [
    {
      "id": "link-alice-mobydick",
      "associationId": "assoc-borrows",
      "sourceObjectId": "obj-alice",
      "targetObjectId": "obj-mobydick"
    },
    {
      "id": "link-alice-ulysses",
      "associationId": "assoc-borrows",
      "sourceObjectId": "obj-alice",
      "targetObjectId": "obj-ulysses"
    }
  ]
}
```

OCL-Auswertung:

```ocl
self.books->size() <= 5
```

Für `self = obj-alice` ermittelt der Evaluator über `assoc-borrows` und Rolle `books` die verbundenen Bücher. Im Beispiel ergibt `size()` den Wert `2`; die Invariante ist erfüllt.

## Bezug zum originalen USE-Projekt

| Original-USE-Konzept | Neues Backend-Konzept | Nutzung |
|---|---|---|
| `MSystemState` | `ObjectModel` / `Snapshot` | Fachliche Referenz für konkreten Systemzustand. |
| `MObject` | `ObjectInstance` | Referenz für Objektidentität und Klassenzuordnung. |
| `MObjectState` | `ObjectInstance` + `Slot` | Referenz für Attributwerte pro Zustand. |
| `MLink` | `ObjectLink` | Referenz für konkrete Association-Instanz. |
| `MLinkEnd` | später `ObjectLinkEnd` | Referenz für end-basierte Linkmodellierung. |
| `MLinkSet` | Link-Index pro Association | Referenz für Navigation und Multiplicity Checks. |
| `UndefinedValue` | später `Unset`/`Undefined` Value | Referenz für fehlende Werte. |
| `MSystemState.check` | Validation Service | Referenz für Constraint-Prüfung gegen Zustand. |

Abgrenzung:

- Keine Übernahme von USE-Systemzustandsklassen.
- Keine SOIL-Ausführung im MVP.
- Keine Modellanimation im MVP.
- Keine direkte `.cmd`-Kompatibilität im MVP.
- Objektmodell wird über REST/JSON und eigene Domain-Klassen verwaltet.

## Teststrategie

| Testtyp | Ziel | Beispiel |
|---|---|---|
| Unit-Test Objekterstellung | Objekte mit gültiger Klasse erzeugen. | `User` -> `alice : User`. |
| Unit-Test ungültige Klasse | Objekt mit unbekannter Klasse ablehnen oder melden. | `UNKNOWN_CLASS`. |
| Unit-Test Slots | Slot-Werte typisiert setzen. | `String`-Attribut akzeptiert `"Alice"`. |
| Unit-Test ungültiger Slot | falscher Werttyp wird erkannt. | `Integer` erwartet, String erhalten. |
| Unit-Test Links | Link zwischen passenden Objekten erstellen. | `alice` borrows `mobyDick`. |
| Unit-Test ungültiger Link | Link passt nicht zu Association Ends. | `Book` an `User`-Ende. |
| Integration mit UML Model Service | Snapshot reagiert auf UML-Definitionen. | Attributlöschung macht Slot ungültig. |
| Integration mit OCL Evaluator | Snapshot liefert `self`, Slots und Navigation. | `self.books->size() <= 5`. |
| Integration mit Validation Service | vollständige Snapshot-Validierung. | Multiplicity Violation erzeugt Fehler. |
| API-Test | REST-Endpunkte für Objekte, Slots und Links. | `POST /snapshot/objects`, `POST /snapshot/links`. |

Test-Fixtures:

```text
src/test/resources/snapshots/
├─ library-valid-snapshot.json
├─ library-invalid-slot-type.json
├─ library-invalid-link-class.json
├─ library-multiplicity-violation.json
└─ library-ocl-self-context.json
```

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Soll der MVP genau einen Snapshot pro Projekt speichern oder bereits eine Liste mit aktivem Snapshot? | Beeinflusst Domain Model und API. |
| Werden fehlende Slots als Fehler, Warning oder `undefined` behandelt? | Beeinflusst OCL-Evaluation und Validation Results. |
| Werden Objektlinks binär oder von Beginn an end-basiert modelliert? | Beeinflusst Erweiterbarkeit für n-äre Associations. |
| Was passiert beim Löschen eines Objekts mit bestehenden Links? | MVP-Entscheidung: zugehörige Links werden automatisch gelöscht; Post-MVP kann ein `DeleteImpactDto` die Auswirkungen vorab anzeigen. |
| Darf ein Objekt nachträglich die Klasse wechseln? | Kann Slots und Links ungültig machen. |
| Müssen Objektnamen global im Snapshot eindeutig sein? | Empfehlung: ja, Namen bleiben aber nicht technische Identität. |
| Wie stark soll der Object Model Service beim Schreiben validieren? | Balance zwischen direkter Ablehnung und gesammelter Validierung. |

## Zusammenfassung

Der Object Model Service verwaltet den konkreten Snapshot eines Projekts: Objekte, Slots und Objektlinks. Er ist strikt vom UML Model Service getrennt, nutzt dessen Klassen-, Attribut- und Association-Definitionen aber als Referenz.

Für den MVP muss der Service Objekte erstellen und löschen, Slot-Werte setzen, Objektlinks erstellen und löschen sowie den Objektzustand für Validation Service und OCL Evaluator bereitstellen. Das originale USE-Projekt liefert mit `MSystemState`, `MObject`, `MObjectState` und `MLink` eine starke fachliche Referenz, die neue technische Umsetzung bleibt jedoch eigenständig.
