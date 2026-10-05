# UML Model Service

## Zweck dieser Datei

Diese Datei beschreibt den UML Model Service des neuen Backends.

Der UML Model Service verwaltet das statische UML-Klassenmodell des Projekts. Dazu gehören Klassen, Attribute, Operationen als Signaturen, Parameter, Assoziationen, Rollen, Multiplizitäten und die Zuordnung von Invarianten zu Kontextklassen.

Das originale USE-Projekt dient als fachliche Referenz für UML-Klassenmodelle, insbesondere durch Konzepte wie `MModel`, `MClass`, `MAttribute`, `MOperation`, `MAssociation`, `MAssociationEnd`, `MMultiplicity` und `MClassInvariant`. Der neue Service übernimmt diese Konzepte fachlich, aber keinen Code.

## Rolle des UML Model Service

Der UML Model Service ist die zentrale Backend-Komponente für Änderungen am Klassenmodell.

Er beantwortet Fragen wie:

- Welche Klassen existieren im Modell?
- Welche Attribute und Operationensignaturen besitzt eine Klasse?
- Welche Assoziationen verbinden welche Klassen?
- Welche Rollen und Multiplizitäten gelten an Association Ends?
- Welche Invarianten gehören zu welcher Kontextklasse?
- Ist eine Änderung am Modell strukturell zulässig?
- Welche abhängigen Snapshot- oder OCL-Elemente könnten durch eine Änderung betroffen sein?

Der Service ist nicht für Diagramm-Rendering zuständig. Er liefert fachliche Modellinformationen, die das Frontend darstellen kann.

## Verwaltete Konzepte

| Konzept | Domain-Objekt | MVP-Relevanz | Bemerkung |
|---|---|---|---|
| UML-Modell | `UmlModel` | Hoch | Container für Klassen, Associations und Invarianten. |
| Klasse | `UmlClass` | Hoch | Zentrales Element des Klassendiagramms. |
| Attribut | `UmlAttribute` | Hoch | Grundlage für Slots und OCL-Attributzugriff. |
| Operation | `UmlOperation` | Mittel | Im MVP nur Signatur, keine Ausführung. |
| Parameter | `UmlParameter` | Mittel | Teil von Operationssignaturen. |
| Assoziation | `UmlAssociation` | Hoch | Grundlage für Klassendiagramm-Kanten und Objektlinks. |
| Association End | `UmlAssociationEnd` | Hoch | Definiert Klasse, Rolle, Navigierbarkeit und Multiplizität. |
| Multiplicity | `Multiplicity` | Hoch | Grundlage für Multiplicity Checks. |
| Invariante | `UmlInvariant` | Hoch | Kontextklasse und OCL-Ausdruck werden im UML-Modell verwaltet. |
| Vererbung | später `Generalization` | Later | Post-MVP. |
| Enumeration | später `UmlEnumeration` | Later | Post-MVP. |
| Aggregation/Komposition | später `AggregationKind` | Later | Post-MVP. |

## MVP-Funktionen

| Funktion | Beschreibung | Ergebnis |
|---|---|---|
| Klasse erstellen | Neue `UmlClass` mit eindeutiger ID und Name anlegen. | Klasse erscheint in Modell, API und Frontend. |
| Klasse ändern | Namen und später weitere Eigenschaften ändern. | OCL- und Snapshot-Abhängigkeiten bleiben prüfbar. |
| Klasse löschen | Klasse entfernen oder kontrolliert ablehnen, wenn Abhängigkeiten existieren. | Keine stillen inkonsistenten Reste. |
| Attribut hinzufügen | Attribut mit Name und Typ zu einer Klasse hinzufügen. | Slots und OCL-Typechecking können darauf zugreifen. |
| Attribut ändern | Name oder Typ ändern. | Betroffene Slots und OCL-Ausdrücke werden validierbar. |
| Attribut löschen | Attribut entfernen oder Abhängigkeiten melden. | Slots und OCL-Ausdrücke werden als betroffen erkannt. |
| Operation hinzufügen | Operationssignatur mit Name, Parametern und Rückgabetyp speichern. | Operation im Klassendiagramm sichtbar. |
| Operation ändern/löschen | Signatur pflegen. | Keine Ausführungssemantik im MVP. |
| Assoziation erstellen | Association mit zwei Association Ends anlegen. | Klassendiagramm-Kante und Basis für Objektlinks. |
| Assoziation ändern | Name, Enden, Rollen und Multiplizitäten ändern. | OCL-Navigation und Multiplicity Checks nutzen neue Struktur. |
| Assoziation löschen | Association entfernen oder abhängige Links/OCL-Navigationen melden. | Snapshot- und OCL-Abhängigkeiten bleiben nachvollziehbar. |
| Rollen erfassen | Rollen an Association Ends speichern. | Rollennamen stehen für OCL-Navigation bereit. |
| Multiplizitäten erfassen | `lower`, `upper`, `unbounded` speichern. | Validation Service kann Linkanzahlen prüfen. |
| Invariante zuordnen | `UmlInvariant.contextClassId` setzen. | OCL `self` bezieht sich auf die Kontextklasse. |

## Post-MVP-Erweiterungen

| Erweiterung | Auswirkung auf UML Model Service |
|---|---|
| Vererbung | Klassenhierarchie verwalten, geerbte Attribute/Operationen auflösen. |
| Enumerationen | Enum-Typen im Modell verwalten und für Attribute/OCL bereitstellen. |
| Aggregation/Komposition | Association Ends um `aggregationKind` erweitern. |
| Assoziationsklassen | Associations mit Klassencharakter modellieren. |
| Derived Attributes | Attribute mit OCL-Ausdruck verwalten. |
| Init Values | Initialwerte für Attribute verwalten. |
| Preconditions/Postconditions | Operationen um OCL-Contracts erweitern. |
| N-äre Associations | Association Ends über zwei Enden hinaus unterstützen. |
| `.use` Import/Export | UML-Modell in USE-nahe Syntax übersetzen oder daraus erzeugen. |

## Typmodell

Der UML Model Service verwaltet die im Modell zulässigen Typen oder greift auf ein zentrales Typmodell zu.

MVP-Typen:

| Typ | Verwendung |
|---|---|
| `String` | Textattribute und String-Literale. |
| `Integer` | Ganzzahlige Attribute, Vergleiche, `size()`-Ergebnisse. |
| `Real` | Dezimalzahlen. |
| `Boolean` | Boolesche Attribute und OCL-Ausdrücke. |
| Class Type | Objektinstanzen und Association Ends. |
| einfache Collection | Navigation über mehrwertige Association Ends. |

Typregeln im MVP:

- Attributtypen müssen bekannt sein.
- Operationen dürfen nur bekannte Parameter- und Rückgabetypen verwenden.
- Association Ends referenzieren existierende Klassen.
- OCL Typechecker nutzt dieselben Typinformationen.
- Snapshot-Slots müssen zu den Attributtypen passen.

Beispiel:

```json
{
  "id": "attr-user-name",
  "name": "name",
  "type": "String"
}
```

## Klassen und Attribute

### Klassen

Klassen benötigen im MVP:

- stabile ID,
- eindeutigen Namen innerhalb des UML-Modells,
- Liste von Attributen,
- Liste von Operationensignaturen.

Beispiel:

```json
{
  "id": "class-user",
  "name": "User",
  "attributes": [
    {
      "id": "attr-user-name",
      "name": "name",
      "type": "String"
    }
  ],
  "operations": []
}
```

### Attribute

Attribute sind für drei andere Bereiche kritisch:

| Bereich | Abhängigkeit |
|---|---|
| Object Model Service | erzeugt und validiert Slots für Objekte dieser Klasse. |
| OCL Typechecker | löst `self.attribute` auf und bestimmt den Typ. |
| Validation Service | prüft Slot-Werte und OCL-Ausdrücke. |

Lösch- und Änderungsregeln müssen Abhängigkeiten berücksichtigen:

| Änderung | Mögliche Auswirkung |
|---|---|
| Attributname ändern | OCL-Ausdrücke mit altem Namen werden ungültig. |
| Attributtyp ändern | bestehende Slot-Werte können ungültig werden. |
| Attribut löschen | Slots und OCL-Ausdrücke können auf nicht mehr existierende Attribute zeigen. |

Empfehlung für den MVP:

- Änderung zulassen, aber betroffene Fehler über Validation Service melden.
- Bei Löschung optional Warnung oder Konflikt zurückgeben, wenn Slots oder OCL-Ausdrücke referenzieren.
- Keine stillen automatischen Reparaturen ohne explizite Produktentscheidung.

## Operationen

Operationen werden im MVP nur als Signaturen verwaltet.

Beispiel:

```json
{
  "id": "op-user-canBorrow",
  "name": "canBorrow",
  "parameters": [
    {
      "id": "param-book",
      "name": "book",
      "type": "Book"
    }
  ],
  "returnType": "Boolean"
}
```

MVP-Regeln:

- Operationsname darf nicht leer sein.
- Parametername darf nicht leer sein.
- Parameter- und Rückgabetypen müssen bekannt sein.
- Operationen werden gespeichert und angezeigt.
- Operationen werden im MVP nicht ausgeführt.
- Operation Bodies, Preconditions und Postconditions sind Post-MVP.

Beziehung zum originalen USE-Projekt:

- USE `MOperation` kann Parameter, Rückgabetyp, OCL-Bodies, SOIL-Bodies und Pre-/Postconditions tragen.
- Das neue System übernimmt im MVP nur die Signaturidee.

## Assoziationen, Rollen und Multiplizitäten

### Assoziationen

Eine MVP-Assoziation verbindet zwei Klassen.

Beispiel:

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
        "upper": null,
        "unbounded": true
      },
      "navigable": true
    },
    {
      "id": "end-borrows-books",
      "classId": "class-book",
      "roleName": "books",
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

### Rollen

Rollen sind nicht nur UI-Labels. Sie sind fachlich relevant für OCL-Navigation.

Beispiel:

```ocl
self.books->size() <= 5
```

Hier muss der OCL Typechecker `books` als Rolle eines Association Ends im Kontext `User` auflösen können.

### Multiplizitäten

MVP-Darstellung:

| Notation | Domain-Repräsentation |
|---|---|
| `1` | `lower = 1`, `upper = 1`, `unbounded = false` |
| `0..1` | `lower = 0`, `upper = 1`, `unbounded = false` |
| `0..*` | `lower = 0`, `upper = null`, `unbounded = true` |
| `1..*` | `lower = 1`, `upper = null`, `unbounded = true` |
| `0..5` | `lower = 0`, `upper = 5`, `unbounded = false` |

Im Gegensatz zu USE sollte `*` im neuen Modell nicht als interne Zahl wie `-1`, sondern explizit als `unbounded` dargestellt werden.

## Invarianten-Zuordnung

Invarianten gehören fachlich zum UML-Modell und referenzieren eine Kontextklasse.

Beispiel:

```json
{
  "id": "inv-max-books",
  "name": "maxBooks",
  "contextClassId": "class-user",
  "expression": {
    "id": "expr-max-books",
    "text": "self.books->size() <= 5"
  },
  "enabled": true
}
```

Der UML Model Service sollte sicherstellen:

- `contextClassId` referenziert eine existierende Klasse,
- Invariantennamen sind in einem sinnvollen Scope eindeutig,
- Invarianten können der Kontextklasse zugeordnet werden,
- Löschung einer Kontextklasse behandelt abhängige Invarianten kontrolliert.

Die fachliche OCL-Syntaxprüfung und Typprüfung liegt nicht im UML Model Service, sondern im OCL Service beziehungsweise OCL Typechecker.

## Validierungsregeln

Der UML Model Service sollte einfache strukturelle Regeln prüfen oder an den Validation Service delegieren.

| Regel | Zeitpunkt | Fehlercode |
|---|---|---|
| Klassenname ist nicht leer. | Create/Update | `TYPE_ERROR` oder Domain-Fehler |
| Klassenname ist eindeutig im Modell. | Create/Update | `TYPE_ERROR` oder `DUPLICATE_NAME` |
| Attributname ist nicht leer. | Create/Update | `TYPE_ERROR` |
| Attributname ist innerhalb der Klasse eindeutig. | Create/Update | `TYPE_ERROR` oder `DUPLICATE_NAME` |
| Attributtyp ist bekannt. | Create/Update | `TYPE_ERROR` |
| Operationname ist nicht leer. | Create/Update | `TYPE_ERROR` |
| Parametername ist nicht leer. | Create/Update | `TYPE_ERROR` |
| Parameter- und Rückgabetypen sind bekannt. | Create/Update | `TYPE_ERROR` |
| Association hat im MVP genau zwei Ends. | Create/Update | `TYPE_ERROR` |
| Association Ends referenzieren existierende Klassen. | Create/Update/Validation | `UNKNOWN_CLASS` |
| Rollenname ist nicht leer. | Create/Update | `TYPE_ERROR` |
| Rollenname ist im Navigationskontext eindeutig genug. | Create/Update/Typecheck | `TYPE_ERROR` |
| Multiplicity hat gültige Grenzen. | Create/Update | `TYPE_ERROR` |
| Invariante referenziert existierende Kontextklasse. | Create/Update | `UNKNOWN_CLASS` |

Offen ist, ob Create/Update-Operationen ungültige Modellzustände strikt ablehnen oder zulassen und später über `Check Constraints` melden. Für den MVP ist eine hybride Regel sinnvoll:

- strukturell nicht speicherbare Fehler ablehnen,
- fachliche Inkonsistenzen, die Nutzer korrigieren können, als Validation Results melden.

## API-Bezug

Mögliche Endpunkte:

| Methode | Pfad | Zweck |
|---|---|---|
| `POST` | `/projects/{projectId}/classes` | Klasse erstellen. |
| `PATCH` | `/projects/{projectId}/classes/{classId}` | Klasse ändern. |
| `DELETE` | `/projects/{projectId}/classes/{classId}` | Klasse löschen. |
| `POST` | `/projects/{projectId}/classes/{classId}/attributes` | Attribut hinzufügen. |
| `PATCH` | `/projects/{projectId}/classes/{classId}/attributes/{attributeId}` | Attribut ändern. |
| `DELETE` | `/projects/{projectId}/classes/{classId}/attributes/{attributeId}` | Attribut löschen. |
| `POST` | `/projects/{projectId}/classes/{classId}/operations` | Operationensignatur hinzufügen. |
| `PATCH` | `/projects/{projectId}/classes/{classId}/operations/{operationId}` | Operationensignatur ändern. |
| `DELETE` | `/projects/{projectId}/classes/{classId}/operations/{operationId}` | Operationensignatur löschen. |
| `POST` | `/projects/{projectId}/associations` | Association erstellen. |
| `PATCH` | `/projects/{projectId}/associations/{associationId}` | Association ändern. |
| `DELETE` | `/projects/{projectId}/associations/{associationId}` | Association löschen. |
| `POST` | `/projects/{projectId}/invariants` | Invariante mit Kontextklasse anlegen. |
| `PATCH` | `/projects/{projectId}/invariants/{invariantId}` | Invariante ändern. |

Alternativ kann der MVP auch mit einem gröberen `PUT /api/v1/projects/{id}` starten, der den gesamten Projektzustand speichert. Für langfristige API-Stabilität sind fachliche Endpunkte aber sinnvoller.

## Beziehung zu OCL Typechecker

Der OCL Typechecker liest das UML-Modell, verändert es aber nicht.

Er benötigt vom UML Model Service beziehungsweise `UmlModel`:

| Benötigte Information | Verwendung |
|---|---|
| Kontextklasse einer Invariante | Typ von `self`. |
| Attribute einer Klasse | Auflösung von `self.attribute`. |
| Association Ends und Rollen | Auflösung von Navigation wie `self.books`. |
| Multiplizitäten | Bestimmung, ob Navigation einzelwertig oder Collection-artig ist. |
| Primitive Typen | Typprüfung von Literalen, Vergleichen und Boolean-Operatoren. |
| Operationensignaturen | Post-MVP für Operation Calls oder pre/post conditions. |

Beispiel:

```ocl
self.books->size() <= 5
```

Typechecking benötigt:

1. Kontextklasse `User`.
2. Rolle `books` an einer Association von `User` zu `Book`.
3. Multiplicity `0..5`, daraus Collection-Navigation.
4. `size()` liefert `Integer`.
5. `5` ist `Integer`.
6. `<=` zwischen `Integer` und `Integer` ergibt `Boolean`.

## Beziehung zu Object Model Service

Der Object Model Service verwaltet Snapshots auf Basis des UML-Modells.

Er benötigt vom UML Model Service:

| UML-Information | Nutzung im Object Model Service |
|---|---|
| Klassen | Objekte dürfen nur Instanzen existierender Klassen sein. |
| Attribute | Slots werden aus Attributen abgeleitet und typgeprüft. |
| Associations | Objektlinks müssen auf existierende Associations verweisen. |
| Association Ends | Link-Enden müssen zu Klassen der beteiligten Objekte passen. |
| Multiplizitäten | Linkanzahlen werden später im Validation Service geprüft. |

Modelländerungen können Snapshots ungültig machen:

| Modelländerung | Auswirkung auf Snapshot |
|---|---|
| Klasse löschen | Objekte dieser Klasse werden ungültig. |
| Attribut löschen | Slots dieses Attributs werden ungültig. |
| Attributtyp ändern | Slot-Werte können ungültig werden. |
| Association löschen | Objektlinks dieser Association werden ungültig. |
| Association-End-Klasse ändern | bestehende Links können nicht mehr passen. |

Der UML Model Service sollte diese Abhängigkeiten sichtbar machen, aber nicht eigenständig Snapshots reparieren, sofern keine klare Produktregel existiert.

## Beziehung zu Validation Service

Der Validation Service nutzt das UML-Modell als Grundlage für alle strukturellen Checks.

Validierungsbezug:

| Validierung | UML Model Service liefert |
|---|---|
| UML-Strukturvalidierung | Klassen, Attribute, Operationen, Associations, Multiplicities. |
| Snapshot-Validierung | Klassen- und Attributdefinitionen für Objekte und Slots. |
| Linkvalidierung | Association-Definitionen und Association Ends. |
| Multiplicity Checks | Multiplicity-Regeln pro Association End. |
| OCL Typechecking | Kontextklassen, Attribute, Rollen und Typen. |
| Invariant Checks | Invarianten und ihre Kontextklasse. |

Validation Results müssen auf Elemente des UML-Modells referenzieren können, z. B.:

- `classId`,
- `attributeId`,
- `associationId`,
- `associationEndId`,
- `invariantId`.

## Bezug zum originalen USE-Projekt

| Original-USE-Konzept | Neues Backend-Konzept | Nutzung |
|---|---|---|
| `MModel` | `UmlModel` | Fachliche Referenz für Modellcontainer. |
| `MClass`, `MClassImpl` | `UmlClass` | Fachliche Referenz für Klassen. |
| `MAttribute` | `UmlAttribute` | Referenz für Attributname und Typ. |
| `MOperation` | `UmlOperation` | Referenz für Operationensignaturen, später pre/post. |
| `MAssociation`, `MAssociationImpl` | `UmlAssociation` | Referenz für Beziehungen zwischen Klassen. |
| `MAssociationEnd` | `UmlAssociationEnd` | Referenz für Rollen, Klasse, Navigierbarkeit, Multiplizität. |
| `MMultiplicity` | `Multiplicity` | Referenz für Multiplicity-Ranges und Checks. |
| `MClassInvariant` | `UmlInvariant` | Referenz für Invariante im Kontext einer Klasse. |
| `MGeneralization` | Post-MVP-Vererbung | Später prüfen. |
| `EnumType` | Post-MVP-Enumerationen | Später prüfen. |

Abgrenzung:

- Keine Übernahme der USE-Klassenstruktur.
- Kein `use-core` als Dependency.
- Kein Ziel, vollständige USE-Feature-Parität im MVP zu erreichen.
- Keine Desktop-GUI- oder Shell-Konzepte im UML Model Service.

## Fehlerfälle

| Fehlerfall | Beispiel | Behandlung |
|---|---|---|
| Klasse nicht gefunden | `classId` existiert nicht. | `404` oder Domain-Fehler `UNKNOWN_CLASS`. |
| Doppelter Klassenname | Zwei Klassen heißen `User`. | Create/Update ablehnen oder Validation Error. |
| Ungültiger Typ | Attributtyp `Money` ohne Typdefinition. | `TYPE_ERROR`. |
| Attribut nicht gefunden | Update auf unbekanntes `attributeId`. | `UNKNOWN_ATTRIBUTE`. |
| Association-End-Klasse unbekannt | End referenziert gelöschte Klasse. | `UNKNOWN_CLASS`. |
| Ungültige Multiplicity | `lower > upper`, negative Grenzen. | `TYPE_ERROR` oder `INVALID_MULTIPLICITY`. |
| Rolle fehlt | Association End ohne Rollenname. | `TYPE_ERROR`. |
| Rollenname kollidiert | Zwei navigierbare Rollen im gleichen Kontext heißen gleich. | `TYPE_ERROR`. |
| Klasse mit abhängigen Objekten löschen | Snapshot enthält Instanzen dieser Klasse. | MVP: Cascade Delete entfernt abhängige Objekte, Links, Slots, Associations, Invarianten und Layoutreferenzen. |
| Association mit Links löschen | Snapshot enthält Objektlinks. | MVP: Cascade Delete entfernt die Association und alle Objektlinks dieser Association. |
| Kontextklasse einer Invariante fehlt | Invariante zeigt auf gelöschte Klasse. | `UNKNOWN_CLASS`. |

## Teststrategie

| Testtyp | Ziel | Beispiele |
|---|---|---|
| Unit-Tests für Klassen | Klassenanlage und Namensregeln prüfen. | Klasse erstellen, Namen ändern, Duplikat ablehnen. |
| Unit-Tests für Attribute | Attributtypen und Eindeutigkeit prüfen. | `String` akzeptieren, unbekannten Typ ablehnen. |
| Unit-Tests für Operationen | Signaturregeln prüfen. | Parameter und Rückgabetyp validieren. |
| Unit-Tests für Associations | Association Ends, Rollen und Multiplizitäten prüfen. | `0..*`, `1`, `0..5` korrekt parsen/speichern. |
| Integration mit OCL Typechecker | Modellinformationen für OCL auflösen. | `self.books->size() <= 5` typprüfen. |
| Integration mit Object Model Service | Modelländerungen beeinflussen Snapshot-Validierung. | Attributtyp ändern und Slot-Fehler erkennen. |
| Integration mit Validation Service | vollständige Modellstruktur validieren. | Association mit unbekannter Klasse erzeugt Fehler. |
| API-Tests | Endpunkte für Klassen, Attribute und Associations prüfen. | `POST /classes`, `PATCH /associations/{id}`. |
| Regression aus USE-Beispielen | vereinfachte USE-Modelle als Testfälle nutzen. | Klassen, Associations und Invarianten aus Demo-/Library-Modellen. |

Test-Fixtures:

```text
src/test/resources/projects/
├─ minimal-uml-model.json
├─ library-uml-model.json
├─ duplicate-class-name.json
├─ invalid-association-end.json
└─ invalid-multiplicity.json
```

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Werden ungültige Modelländerungen sofort abgelehnt oder als späterer Validation Error zugelassen? | Beeinflusst API-Verhalten und UX. |
| Sollen Klassennamen oder IDs primär für OCL-Kontext genutzt werden? | Empfehlung: IDs intern, Namen für OCL-Syntax und UI. |
| Sind Rollennamen im MVP zwingend eindeutig pro Klasse? | Wichtig für OCL-Navigation. |
| Werden alle Association Ends im MVP als navigierbar behandelt? | Vereinfacht OCL, muss aber dokumentiert werden. |
| Soll eine Klasse mit abhängigen Objekten gelöscht werden können? | Beeinflusst Object Model Service und Validierung. |
| Wo liegt die Verantwortung für Invarianten-CRUD: UML Model Service oder OCL Service? | Empfehlung: Zuordnung im UML Model, OCL-Prüfung im OCL Service. |
| Wie wird `Multiplicity` aus UI-Strings wie `0..*` geparst? | Kann im DTO Mapper oder in einem Value Parser liegen. |

## Zusammenfassung

Der UML Model Service verwaltet das statische Modell des neuen Systems: Klassen, Attribute, Operationensignaturen, Associations, Rollen, Multiplizitäten und Invariantenzuordnungen. Er ist Grundlage für Class Diagram, Object Model, OCL Typechecking und Validation Service.

Die wichtigste Abgrenzung lautet: Der UML Model Service verwaltet Modellstruktur, aber wertet keine OCL-Ausdrücke aus, verwaltet keine Objektzustände und rendert keine Diagramme. Diese klare Trennung hält das Backend testbar, erweitertbar und unabhängig vom originalen USE-Core.
