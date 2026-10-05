# M10: Enum, DataType und Type Picker

## Status und Zweck

**Status:** `READY_FOR_REVIEW`  
**Verbindliche Mockups:**

- `assets/mockups/classifier-type-picker.html`
- `assets/mockups/datatype-properties.html`
- `assets/mockups/delete-datatype-modal.html`
- `assets/mockups/delete-enumeration-modal.html`

Das Unified-Mockup ist für Enumeration, Erstellung, Type Picker und typisierte
Objektwerte verbindlich. Das DataType-Properties-Mockup ist für Details und
Value Properties eines ausgewählten DataType verbindlich. Die frühere
Ausgangsfassung wurde nach vollständiger Übernahme ihrer fachlichen Inhalte
entfernt.

Das Delete-Enumeration-Mockup spezifiziert den getrennten
Impact-/Blocker-/Delete-Ablauf. Verwendete Typen und Literale werden nicht
clientseitig migriert; verbleibende Referenzen blockieren die Mutation.

Das Delete-DataType-Mockup ergaenzt diesen Ablauf fuer strukturierte
Werttypen. Der Backend-Impact verfolgt auch in Tuple- und Collection-Typen
verschachtelte Verwendungen; das Frontend bietet keine automatische Typ- oder
Wertmigration an.

M10 beschreibt die Oberfläche für UML-Enumerationen, UML-DataTypes und eine
gemeinsame, kontextbezogene Typauswahl. Das Ergebnis erweitert die bestehende
Class-Diagram-Arbeitsfläche, ohne einen neuen globalen Workspace-Tab oder
produktiven Anwendungscode einzuführen.

## Verwendete Grundlagen

| Bereich | Verwendete Dokumente |
|---|---|
| Einstieg | `00-overview/03-documentation-map.md`, `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md` |
| UI/UX | `01-ui-overview.md`, `02-class-diagram-ui.md`, `03-object-diagram-ui.md`, `05-screenshot-traceability.md`, `07-m1-ui-baseline.md`, `10-redesign-design-principles.md` |
| Konsistenz | M3 zu Namespaces und qualifizierten Namen, M7 zu Parametern/Rückgabetypen sowie M9 zu Attribute Details |
| UML/OCL | `03-uml-ocl-domain/01-uml-ocl-scope.md`, `02-domain-model.md`, `03-ocl-architecture-and-extension-strategy.md` |
| Frontend/API | `06-frontend-analysis/03-frontend-architecture.md`, `11-properties-panel.md`, `12-modal-dialogs.md`, `07-integration-and-api/01-frontend-backend-contract.md`, `07-dto-reference.md` |
| Compliance/Planung | `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`, `15-full-ocl-uml-implementation-plan.md` |

Als visuelle Referenz wurden insbesondere
`01-class-diagram-class-properties.png`,
`04-class-diagram-new-class-selected.png`,
`06-object-diagram-object-properties.png`, `08-modal-add-class.png`,
`13-ocl-editor.png` und `17-new-class.png` verwendet.

Das aktuelle Frontend wurde hinsichtlich Type-Feldern, Create-Class-Dialog,
Class Properties und Object Properties geprüft. Der aktuelle Typ ist dort als
offener String modelliert, die sichtbare Auswahl ist überwiegend auf `String`,
`Integer`, `Real` und `Boolean` begrenzt.

Im Backend wurden `UmlType`, `UmlEnumeration`, `UmlModelDto`, Typechecker und
Snapshot-Wertprüfung geprüft. Enumerationen und Enum-Literale sind teilweise
vorhanden. Ein eigenständiges UML-DataType-Modell und ein vollständiger
Typkatalogvertrag wurden nicht gefunden. Das originale USE-Projekt war für M10
nicht zusätzlich erforderlich; UML-/OCL-Analyse und Compliance-Matrix reichen
für Begriffe und Abgrenzung aus.

## Einordnung in die bestehende Oberfläche

Die Top-Navigation bleibt `Class Diagram | Object Diagram | OCL Editor`. Im
Explorer des Class Diagram werden Modelltypen gruppiert:

- Classes,
- Enumerations,
- DataTypes.

Enum und DataType sind Classifier, aber keine normalen Klassen. Sie werden
daher mit `«enumeration»` beziehungsweise `«dataType»` gekennzeichnet. Ihre
Properties verwenden das vorhandene rechte Panel und führen keine zusätzliche
globale Hauptseite ein. Bei ausgewählten Klassen bleibt die bekannte Struktur
`Class | Association | Invariant` unverändert.

## Fachliche Regeln

### Enumeration

Eine Enumeration besitzt stabile Identität, Namen, Namespace und eine
geordnete Liste eindeutiger Literale. Ein Objektwert ist ein typisiertes Literal,
beispielsweise `billing::InvoiceStatus::issued`, und kein freier String.

### DataType

Ein UML-DataType beschreibt Werte ohne Objektidentität. Im M10-Mockup wird
`Money` mit den Value Properties `amount : Real` und `currency : Currency`
gezeigt. Ein Money-Wert ist ein einzelner strukturierter Slotwert. Er ist kein
Objekt, besitzt keine Objekt-ID und kann nicht über Object Associations
verknüpft werden.

### Type Picker

Der Type Picker ist eine wiederverwendbare Komponente für:

- Attributtypen,
- Parametertypen,
- Operationsrückgabetypen,
- Value Properties eines DataType.

Er gruppiert Primitive, Enumerations, DataTypes und Classes. Welche Einträge
auswählbar sind, hängt vom Feldkontext ab. Beispielsweise ist `Void` nur als
Operationsrückgabetyp zulässig. Ein nicht zulässiger Typ bleibt sichtbar,
deaktiviert und erhält eine verständliche Begründung.

Kurze Namen werden bevorzugt. Nur bei Mehrdeutigkeit oder außerhalb des
aktuellen Namespace zeigt die Oberfläche qualifizierte Namen wie
`billing::Status` und `shipping::Status`.

## Entworfene Ansichten und Zustände

**Verbindliches Enum- und Type-Picker-Mockup:**
`assets/mockups/classifier-type-picker.html`

Die rechte Seitenleiste richtet sich nach der Auswahl im Explorer. Eine Klasse
öffnet `Class Properties` mit `Details`, `Attributes`, `Operations` und
`Generalizations`. Eine Enumeration ersetzt dieses Panel vollständig durch
`Enumeration Properties` mit den beiden Reitern `Details` und `Literals`.
Dadurch werden Enumerationseigenschaften nicht fälschlich als Unterbereich
einer Klasse dargestellt.

### Desktop-Hauptzustand

Der Explorer enthält getrennte Gruppen für Classes, Enumerations und DataTypes.
Das Canvas zeigt `InvoiceStatus` mit dem Stereotyp `«enumeration»` und `Money`
mit `«dataType»`. Die Enum Properties enthalten Name, Package und eine
sortierbare Literalliste mit Hinzufügen und Löschen.

### Create Enum

Der Dialog enthält Name, Package und mindestens ein Literal. Literale können
hinzugefügt, gelöscht und über eine explizite Drag-Fläche sortiert werden.
Doppelte oder leere Literale verhindern das Speichern.

Eine Enumeration wird entweder über `+ Type -> Enumeration` im Explorer oder
über `Create Enumeration` im Type Picker angelegt. Beim Einstieg aus einem
Attribut bleibt dessen ungespeicherter Entwurf erhalten. Nach erfolgreicher
Erstellung kehrt der Benutzer zum Type Picker zurück und die neue Enumeration
wird automatisch als Attributtyp ausgewählt. Name und Namespace werden unter
`Details`, die geordnete Literalliste unter `Literals` bearbeitet.

Die beiden Reiter wechseln interaktiv den Inhalt desselben
`Enumeration Properties`-Panels im vollständigen UML-Workspace. `Details`
zeigt Name, Namespace und den abgeleiteten qualifizierten Namen. `Literals`
zeigt die geordnete Literalliste. Der Wechsel öffnet weder ein Modal noch ein
neues Fenster und verändert die Auswahl im Explorer oder Canvas nicht.

### Create DataType

Der Dialog enthält Name, Package und Value Properties. Jedes Property besitzt
Name und einen Type Picker. Der Hilfetext erklärt den Unterschied zwischen
Value Type und Objektidentität.

### Gemeinsamer Type Picker

Der Picker bietet Suche und Kategorien. Das Mockup zeigt einen normalen Treffer,
den kontextbedingt deaktivierten Typ `Void` und zwei mehrdeutige Typen namens
`Status`, die qualifiziert angezeigt werden.

### Object Diagram

Enum-Werte werden als Select mit der autoritativen Literalliste dargestellt.
DataType-Werte werden als zusammenhängender strukturierter Editor angezeigt.
Der Benutzer sieht fachliche Namen; interne IDs bleiben verborgen.

### Fehler, Loading, Empty und Bestätigung

M10 zeigt:

- ein doppeltes Enum-Literal mit feldnaher Fehlermeldung,
- einen blockierten Löschvorgang für einen noch referenzierten Typ,
- Loading und Disabled während der Typkatalog geladen wird,
- einen Empty State ohne DataTypes,
- eine Erfolgsmeldung nach validierter Speicherung,
- Referenzanzahl und betroffene fachliche Elemente vor strukturellen Änderungen.

### Schmaler Viewport

Der Type Picker öffnet als intern scrollbarer Drawer mit feststehender Suche.
Kategorien, Typart, Auswahl und Nichtverfügbarkeit werden textlich sichtbar.
Nach Auswahl oder Abbruch kehrt der Fokus zum auslösenden Typfeld zurück.

## Interaktionsregeln

1. `+ Type` bietet `Class`, `Enumeration` und `DataType` als klar benannte
   Auswahl, ohne fortgeschrittene Typen ungefragt vorzuöffnen.
2. Sortieren verändert die Literalreihenfolge im Entwurf; gespeichert wird erst
   nach serverseitiger Validierung.
3. Das Entfernen oder Umbenennen eines verwendeten Typs beziehungsweise
   Literals zeigt Auswirkungen auf Attribute, Operationen, OCL und Objektwerte.
4. Destruktive Änderungen werden blockiert, bis eine explizite, fachlich
   gültige Migration festgelegt wurde. Das Frontend erzeugt keinen Fallback.
5. Der Picker erhält Feldkontext und Scope vom aufrufenden Formular, während
   Zulässigkeit und Namensauflösung vom Backend bestätigt werden.
6. Ein Enum-Slot akzeptiert nur Literale seiner Enumeration.
7. Ein DataType-Wert wird atomar gespeichert; Teiländerungen dürfen keinen
   ungültigen Zwischenstand persistieren.
8. Die Auswahl eines Modelltyps ersetzt das Properties Panel vollständig; die
   Klassenreiter bleiben bei ausgewählter Enumeration oder DataType verborgen.
9. `Create Enumeration` aus dem Type Picker erhält den aufrufenden
   Formularentwurf und stellt nach dem Erstellen den Auswahlkontext wieder her.

## Compliance-Zuordnung

| Matrix-ID | Abdeckung in M10 |
|---|---|
| `CM-UML-005` | Enum-Erstellung, geordnete Literale, Enum Properties und Enum-Objektwerte |
| `CM-UML-006` | DataType-Erstellung, Value Properties und strukturierte Objektwerte |
| `CM-OCL-017` | typisierte Enum-Literale, Qualifizierung und Typkonformität |
| `CM-CTX-008` | Package-/Namespace-Scope und mehrdeutige qualifizierte Namen |
| `CM-UML-001` | stabile Identität für Typen und Referenzen trotz fachlicher Namensanzeige |
| `CM-OCL-002` | feldnahe Typ- und Namensdiagnosen mit Source Location, sofern OCL betroffen ist |

M10 behauptet keine vollständige Umsetzung dieser Matrixeinträge. Besonders
DataTypes, Namespace-Auflösung und Serialisierung bleiben Backend-Gaps.

## Vorläufige Domänen-, API- und DTO-Anforderungen

### Domänenmodell

- `UmlEnumeration` benötigt ID, Name, Namespace und geordnete eindeutige
  Literale mit stabiler Referenzstrategie.
- `UmlDataType` benötigt ID, Name, Namespace und geordnete Value Properties.
- Ein Typreferenzmodell muss Typkind und stabile ID vom Anzeigenamen trennen.
- Slotwerte benötigen typisierte Enum- und DataType-Repräsentationen.
- Referenzprüfung muss Attribute, Parameter, Rückgabetypen, DataType-Properties,
  OCL-Ausdrücke und Snapshots umfassen.

### API und DTOs

Ein späterer Vertrag muss mindestens folgende Informationen transportieren:

```ts
type UmlClassifierKind = 'PRIMITIVE' | 'CLASS' | 'ENUMERATION' | 'DATATYPE';

interface UmlTypeRefDto {
  id: string;
  kind: UmlClassifierKind;
  name: string;
  qualifiedName: string;
}

interface UmlEnumerationDto {
  id: string;
  name: string;
  namespaceId?: string;
  literals: Array<{ id: string; name: string; position: number }>;
}

interface UmlDataTypeDto {
  id: string;
  name: string;
  namespaceId?: string;
  properties: Array<{ id: string; name: string; type: UmlTypeRefDto }>;
}

interface TypeCatalogEntryDto extends UmlTypeRefDto {
  displayName: string;
  selectable: boolean;
  unavailableReason?: string;
}
```

Für den Type Picker wird ein scope- und kontextbezogener Typkatalog benötigt.
Mutationsantworten müssen Referenzkonflikte, betroffene fachliche Elemente und
die neue Modellrevision liefern. Enum-/DataType-Slotwerte benötigen ein
eindeutiges strukturiertes JSON-Format. Die konkreten Feldnamen sind vorläufig.

## Spätere Frontend-Anforderungen

- Explorer-Gruppen für Classes, Enumerations und DataTypes,
- Create-/Edit-/Delete-Flows in vorhandenen Modal- und Properties-Mustern,
- eine gemeinsame Type-Picker-Komponente mit Suche, Gruppen, Scope und
  Nichtverfügbarkeitsgrund,
- Wiederverwendung des Pickers in Attributen, Parametern, Operationen und
  DataType-Properties,
- enum-spezifischer Select und rekursionssicherer strukturierter DataType-
  Value Editor im Object Diagram,
- interner Scroll für lange Literallisten, Picker und Value-Editoren,
- keine lokale Erfindung unbekannter Typen oder Literale,
- textliche Statusanzeige, Tastaturbedienung, ausreichend große Ziele und
  verständliche kontextuelle Hilfe.

## Annahmen und offene Entscheidungen

**Annahmen**

- Die Reihenfolge von Enum-Literalen ist fachlich stabil und wird persistiert.
- DataTypes besitzen Wertsemantik und keine Objektidentität.
- Der Picker zeigt kurze Namen, solange sie im Scope eindeutig sind.
- Backendvalidierung bleibt für Typauflösung und Objektwerte autoritativ.

**Offen**

- Ob Enum-Literale eigene persistente IDs oder stabile qualifizierte Namen als
  Referenzen verwenden.
- Welche UML-DataType-Features zusätzlich zu Value Properties unterstützt
  werden, beispielsweise Query Operations.
- Wie tief verschachtelte und rekursive DataTypes begrenzt werden.
- Welches Migrationsverfahren beim Umbenennen oder Löschen verwendeter Literale
  angeboten wird.
- Ob der Typkatalog Bestandteil des Projekt-DTO oder ein eigener Endpoint wird.

## Akzeptanzkriterien

| ID | Kriterium | Ergebnis |
|---|---|---|
| `M10-AC-01` | Enum und DataType sind fachlich von Classes unterscheidbar. | erfüllt |
| `M10-AC-02` | Enum Properties unterstützen eine geordnete eindeutige Literalliste. | erfüllt |
| `M10-AC-03` | Der gemeinsame Type Picker besitzt Suche und Typkategorien. | erfüllt |
| `M10-AC-04` | Qualifizierte Namen erscheinen nur bei Mehrdeutigkeit beziehungsweise Scope-Bedarf. | erfüllt |
| `M10-AC-05` | Nicht auswählbare Typen sind deaktiviert und verständlich begründet. | erfüllt |
| `M10-AC-06` | Enum- und DataType-Werte sind im Object Diagram typgerecht dargestellt. | erfüllt |
| `M10-AC-07` | Editing, Error, Loading, Disabled, Empty, Confirmation und Success sind berücksichtigt. | erfüllt |
| `M10-AC-08` | Properties und Picker sind im schmalen Viewport intern scrollbar. | erfüllt |
| `M10-AC-09` | Vorläufige Backend-, API-, DTO- und Frontendanforderungen sind dokumentiert. | erfüllt |
| `M10-AC-10` | Produktiver Code und spätere Mockup-Schritte wurden nicht vorgezogen. | erfüllt |
| `M10-AC-11` | Eine ausgewählte Enumeration zeigt ausschließlich Enumeration Properties mit Details und Literals. | erfüllt |
| `M10-AC-12` | Der Type Picker kann eine Enumeration anlegen, zum Attribut zurückkehren und den neuen Typ auswählen. | erfüllt |
| `M10-AC-13` | Details und Literals wechseln interaktiv im selben Enumeration-Properties-Panel. | erfüllt |

## Abgrenzung

M10 entwirft ausschließlich Enumeration, DataType, Type Picker und deren
Objektwerte. Die OCL-Feature-/Compliance-Anzeige folgt in M11. Optionale State-
Machine- und `OclMessage`-Ansichten bleiben M12/M13; M14 übernimmt die
übergreifende responsive und barrierebezogene Abnahme. Der Gesamtstatus bleibt
`MOCKUP`; `BACKEND_READY` wird nicht gesetzt.
