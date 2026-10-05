# Properties Panel

## Zweck dieser Datei

Diese Datei beschreibt das `Properties Panel` des React/TypeScript-Frontends. Das Panel ist der rechte Bearbeitungsbereich der Anwendung und zeigt abhängig von der aktuellen Selektion unterschiedliche Inhalte für Klassen, Associations, Invarianten, Objekte und Objektlinks.

Das Properties Panel ist kein eigenständiger fachlicher Modellkern. Es ist eine UI-Schicht über dem Frontend-State und den Backend-DTOs. Änderungen werden lokal als Draft oder direkt im Project State geführt und anschließend über den API Client persistiert.

## Rolle des Properties Panels

Das Properties Panel verbindet Auswahl, Detailansicht und Bearbeitung. Es ist der zentrale Ort für präzise Eigenschaften, die in der Diagrammkarte selbst zu viel Platz einnehmen würden.

Kernaufgaben:

- selektiertes Element eindeutig anzeigen,
- relevante Felder typabhängig darstellen,
- bearbeitbare und readonly Felder unterscheiden,
- Eingaben lokal plausibilisieren,
- fachliche Backend-Fehler feldbezogen anzeigen,
- Änderungen mit Canvas, Explorer und OCL Editor synchron halten,
- Save-/Dirty-State unterstützen,
- Validierungsfehler an passenden Feldern sichtbar machen.

## Relevante Screenshots

| Screenshot | Vorschau | Properties-Kontext |
|---|---|---|
| `01-class-diagram-class-properties.png` | ![Class Properties](../assets/screenshots/01-class-diagram-class-properties.png) | Klasse ist selektiert; Panel zeigt Class Properties, Attribute und Operationen. |
| `02-class-diagram-association-properties.png` | ![Association Properties](../assets/screenshots/02-class-diagram-association-properties.png) | Association ist selektiert; Panel zeigt Enden, Rollen und Multiplizitäten. |
| `03-class-diagram-invariant-properties.png` | ![Invariant Properties](../assets/screenshots/03-class-diagram-invariant-properties.png) | Invariante ist selektiert; Panel zeigt Name und OCL-Ausdruck. |
| `06-object-diagram-object-properties.png` | ![Object Properties](../assets/screenshots/06-object-diagram-object-properties.png) | Objekt ist selektiert; Panel zeigt Objektname, Typ und Slot-Werte. |
| `12-object-diagram-association-properties.png` | ![Object Association Properties](../assets/screenshots/12-object-diagram-association-properties.png) | Objektlink ist selektiert; Panel zeigt Association, Source Object und Target Object. |
| `15-properties-association.png` | ![Class Related Association Properties](../assets/screenshots/15-properties-association.png) | Klasse bleibt im Diagramm selektiert; das Properties Panel bietet über ein Segment Zugriff auf zugehörige Associations. |
| `16-properties-invariants.png` | ![Class Related Invariant Properties](../assets/screenshots/16-properties-invariants.png) | Klasse bleibt selektiert; das Properties Panel bietet über ein Segment Zugriff auf Invarianten der Klasse. |
| `17-new-class.png` | ![New Class Properties](../assets/screenshots/17-new-class.png) | Neue Klasse ist selektiert; Class-Segment zeigt Name, Attribute, Operationen und Add-Aktionen. |

## Selektion als Treiber

Das Properties Panel wird ausschließlich durch den zentralen Selection State gesteuert. Es sollte nicht selbst entscheiden, welches Element fachlich aktiv ist.

```ts
type PropertiesSelection =
  | { type: "class"; id: string }
  | { type: "association"; id: string }
  | { type: "invariant"; id: string }
  | { type: "object"; id: string }
  | { type: "objectLink"; id: string }
  | null;
```

| Selection Type | Panel-Komponente | Hauptquelle |
|---|---|---|
| `class` | `ClassPropertiesPanel` | UML Model |
| `association` | `AssociationPropertiesPanel` | UML Model |
| `invariant` | `InvariantPropertiesPanel` | UML Model / OCL Model |
| `object` | `ObjectPropertiesPanel` | Object Model / Snapshot |
| `objectLink` | `ObjectAssociationPropertiesPanel` | Object Model / Snapshot |
| `null` | `EmptyPropertiesPanel` oder `ProjectPropertiesPanel` | UI State |

Die Selektion kann aus Canvas, Explorer Sidebar, Validation Results oder OCL Editor kommen. Das Panel darf diese Quellen nicht unterschiedlich behandeln; es erhält nur die normalisierte Selektion.

## Class Properties

`ClassPropertiesPanel` zeigt und bearbeitet eine UML-Klasse. Im MVP sind Klassen die zentrale Grundlage für Objekte, Slots, OCL-Kontextklassen und Associations.

Die Screenshots `15-properties-association.png`, `16-properties-invariants.png` und `17-new-class.png` präzisieren, dass die Class Properties nicht nur allgemeine Klassendaten zeigen. Bei selektierter Klasse enthält das rechte Panel ein Segment Control mit mindestens:

- `Class` für allgemeine Klassendaten, Attribute und Operationen,
- `Association` für alle Associations, an denen die Klasse beteiligt ist,
- `Invariant` für alle Invarianten, deren Kontextklasse die selektierte Klasse ist.

Das Segment Control ändert nicht die fachliche Selektion im Canvas: Die Klasse bleibt visuell selektiert. Klickt der Nutzer jedoch auf eine konkrete Association oder Invariante innerhalb des Segments, darf die UI zur entsprechenden Association- oder Invariant-Selektion wechseln und das dedizierte Properties Panel anzeigen.

| Feld | Beschreibung | Editable | Validierung | Synchronisation |
|---|---|---|---|---|
| Class ID | stabile technische ID | Nein | muss vorhanden sein | Backend/DTO |
| Name | UML-Klassenname | Ja | Pflichtfeld, eindeutig im Modell | Canvas Node, Explorer, Object Type Dropdowns |
| Attributes | Liste der Attribute | Ja | Name, Typ, Eindeutigkeit innerhalb der Klasse | Class Node, Object Slots |
| Attribute Name | Name eines Attributs | Ja | Pflichtfeld, eindeutig je Klasse | OCL Typechecker Kontext |
| Attribute Type | `String`, `Integer`, `Real`, `Boolean` im MVP | Ja | muss unterstützter Typ sein | Slot-Eingabe, OCL Typechecker |
| Operations | Operationen als Signaturen | Ja | Name, Parameter, Return Type | Class Node, OCL Post-MVP |
| Operation Parameters | Parametername und Typ | Ja | Typ muss gültig sein | Signaturanzeige |
| Related Associations | Liste der Associations mit Endklassen, Rollen und Multiplizitäten | Nein im Listenmodus, Detail per Auswahl | Association muss Klasse referenzieren | Selection State, Association Properties |
| Related Invariants | Liste der Invarianten der Kontextklasse mit OCL-Ausdruck | Nein im Listenmodus, Detail per Auswahl | Kontextklasse muss passen | Selection State, Invariant Properties, OCL Editor |
| Validation Status | strukturelle Fehler zur Klasse | Nein | aus Backend-Ergebnis | Fehleranzeige |

MVP-Verhalten:

- Umbenennung aktualisiert Canvas und Explorer.
- Attribute können hinzugefügt, bearbeitet und gelöscht werden.
- Operationen werden als Signaturen erfasst, nicht fachlich ausgeführt.
- Zugehörige Associations sind über das `Association`-Segment erreichbar.
- Zugehörige Invarianten sind über das `Invariant`-Segment erreichbar.
- `Add Association` im Class-Panel öffnet den Association-Create-Flow mit der selektierten Klasse als naheliegender Vorauswahl.
- `Add Invariant` im Class-Panel öffnet den Invariant-Create-Flow mit der selektierten Klasse als Kontextklasse.
- Änderung eines Attributtyps kann bestehende Slots ungültig machen; das Panel sollte darauf hinweisen.
- Klasse löschen ist als destruktive Aktion verfügbar und öffnet vor dem Backend-Delete einen Confirm Dialog.

### Class-Panel-Segmente

| Segment | Inhalt | Nutzeraktion | Ergebnis |
|---|---|---|---|
| `Class` | Klassenname, Attribute, Operationen, Add/Delete-Aktionen. | Namen ändern, Attribute/Operationen hinzufügen oder bearbeiten. | Class Node, Explorer und Project State aktualisieren sich. |
| `Association` | Associations, deren `UmlAssociationEnd.classId` auf die selektierte Klasse zeigt. | Association auswählen oder neue Association starten. | Association Detailpanel öffnet oder Add-Association-Modal nutzt die Klasse als Vorauswahl. |
| `Invariant` | Invarianten, deren `contextClassId` der selektierten Klasse entspricht. | Invariante auswählen oder neue Invariante starten. | Invariant Detailpanel öffnet oder Add-Invariant-Modal nutzt die Klasse als Kontext. |

Die Related-Listen sind keine zweite fachliche Datenquelle. Sie werden aus `ProjectDto.umlModel.associations` und `ProjectDto.umlModel.invariants` abgeleitet.

## Association Properties

`AssociationPropertiesPanel` zeigt und bearbeitet eine UML-Association zwischen Klassen.

| Feld | Beschreibung | Editable | Validierung | Synchronisation |
|---|---|---|---|---|
| Association ID | stabile technische ID | Nein | muss vorhanden sein | Backend/DTO |
| Name | Name der Association | Ja | Pflichtfeld oder optional nach Modellregel | Edge Label, Explorer |
| Source Class | Klasse am ersten Ende | Ja/Should | muss existieren | Edge Source, OCL Navigation |
| Source Role | Rollenname am ersten Ende | Ja | optional, eindeutig im Kontext | OCL Navigation |
| Source Multiplicity | Multiplizität am ersten Ende | Ja | Syntax wie `1`, `0..1`, `0..*`, `1..*` | Validation Service |
| Target Class | Klasse am zweiten Ende | Ja/Should | muss existieren | Edge Target |
| Target Role | Rollenname am zweiten Ende | Ja | optional, eindeutig im Kontext | OCL Navigation |
| Target Multiplicity | Multiplizität am zweiten Ende | Ja | gültige Multiplicity-Syntax | Validation Service |
| Navigability | Navigierbarkeit eines Endes | Later | konsistent mit OCL-Navigation | OCL Typechecker |
| Validation Status | Link-/Multiplicity-Probleme | Nein | aus Backend-Ergebnis | Edge Marker |

MVP-Entscheidung: Source/Target Class sollten nach Erstellung nur mit Vorsicht editierbar sein, weil bestehende Objektlinks dadurch ungültig werden können. Im MVP kann das Panel solche Änderungen entweder blockieren oder mit Bestätigung erlauben.

Association löschen ist im MVP erforderlich. Das Backend entfernt dabei auch zugehörige Objektlinks und bereinigt Layout-/Validation-Referenzen.

## Invariant Properties

`InvariantPropertiesPanel` zeigt und bearbeitet eine OCL-Invariante.

| Feld | Beschreibung | Editable | Validierung | Synchronisation |
|---|---|---|---|---|
| Invariant ID | stabile technische ID | Nein | muss vorhanden sein | Backend/DTO |
| Name | Name der Invariante | Ja | Pflichtfeld, eindeutig pro Kontextklasse | Explorer, Class Diagram Badge, OCL Editor |
| Context Class | Klasse für `self` | Ja/Should | muss existieren | OCL Typechecker, Class Diagram |
| OCL Expression | OCL-Ausdruck | Ja | nicht leer, Backend parse/typecheck | OCL Editor, Validation Results |
| Enabled | Ob die Invariante geprüft wird | Later | optional | Validation Service |
| Diagnostics | Syntax-/Typfehler | Nein | aus Backend-Diagnosen | OCL Editor |
| Last Result | letzter Validierungsstatus | Nein | aus Validation Result | Class/Object Diagram Marker |

MVP-OCL-Beispiel:

```ocl
self.books <= 5
```

Das Panel darf OCL nicht fachlich vollständig prüfen. Es kann Pflichtfelder prüfen und optional Backend-Endpunkte für `parse` oder `typecheck` anstoßen.

Invarianten können im MVP aus dem Properties Panel gelöscht werden. Nach dem Löschen werden Invariant Badge, Explorer-Eintrag, OCL-Editor-Eintrag und alte Validation Targets entfernt.

## Object Properties

`ObjectPropertiesPanel` zeigt und bearbeitet eine Objektinstanz in einem Snapshot.

| Feld | Beschreibung | Editable | Validierung | Synchronisation |
|---|---|---|---|---|
| Object ID | stabile technische ID | Nein | muss vorhanden sein | Backend/DTO |
| Name | Objektname | Ja | Pflichtfeld, eindeutig im Snapshot | Object Node, Explorer |
| Class | Typ des Objekts | Ja/Should | muss existierende `UmlClass` sein | Slot-Struktur, OCL Kontext |
| Slots | Attributwerte des Objekts | Ja | Wert muss zum Attributtyp passen | Object Node, OCL Evaluator |
| Slot Value | konkreter Wert | Ja | primitive Typprüfung im Frontend möglich, fachlich Backend | Validation Service |
| Validation Status | Objektfehler | Nein | aus Validation Result | roter Rahmen, Badge |

Slot-Felder im MVP:

| Attributtyp | Eingabekomponente | Lokale Prüfung |
|---|---|---|
| `String` | Text Input | Wert ist Text |
| `Integer` | Number Input | keine Dezimalstellen |
| `Real` | Number Input | Dezimalzahl erlaubt |
| `Boolean` | Toggle oder Select | `true`/`false` |

Änderung des Objekttyps ist fachlich kritisch, weil Slots und Links ungültig werden können. Im MVP sollte das entweder verhindert oder nur mit Bestätigung erlaubt werden.

Objekte können im MVP gelöscht werden. Das Backend entfernt dabei Slots und alle Objektlinks, die das Objekt referenzieren. Das Frontend leert danach die Selektion und entfernt zugehörige Fehler-Badges.

## Object Association Properties

`ObjectAssociationPropertiesPanel` zeigt und bearbeitet einen Objektlink. Der Screenshot benennt diesen Bereich als Association Properties im Objektdiagramm; fachlich handelt es sich um einen ObjectLink.

| Feld | Beschreibung | Editable | Validierung | Synchronisation |
|---|---|---|---|---|
| Link ID | stabile technische ID | Nein | muss vorhanden sein | Backend/DTO |
| Association | UML-Association, auf der der Link basiert | Ja/Should | muss existieren | Edge Label, Validation Service |
| Source Object | Objekt am ersten Ende | Ja | muss zur Source Class passen | ObjectLinkEdge |
| Target Object | Objekt am zweiten Ende | Ja | muss zur Target Class passen | ObjectLinkEdge |
| Source Role | aus Association-Ende abgeleitet | Nein | Backend/Model konsistent | Label/Details |
| Target Role | aus Association-Ende abgeleitet | Nein | Backend/Model konsistent | Label/Details |
| Validation Status | Link- oder Multiplicity-Fehler | Nein | aus Validation Result | Edge Marker, Badge |

Im MVP kann die Bearbeitung bestehender Objektlinks eingeschränkt werden. Eine robuste Alternative ist: Link löschen und neu erstellen, falls Association oder Endobjekte geändert werden sollen.

Objektlinks können im MVP direkt gelöscht werden. Diese Aktion ist besonders wichtig, wenn Link-Enden im Properties Panel nicht frei bearbeitet werden.

## Delete-Aktionen

Delete-Aktionen sind Teil des Properties Panels, weil die aktuelle Selektion dort fachlich sichtbar ist. Sie dürfen nicht nur lokal aus dem Diagramm entfernt werden; das Frontend ruft immer den passenden Backend-Endpunkt auf und übernimmt danach den aktualisierten Projektzustand.

| Selektion | UI-Aktion | Confirm-Hinweis | Backend-Endpunkt | State-Bereinigung |
|---|---|---|---|---|
| Klasse | `Delete Class` | Entfernt Klasse, abhängige Associations, Objekte, Invarianten und Layoutdaten. | `DELETE /api/v1/projects/{projectId}/classes/{classId}` | Selection, Canvas Node, Explorer, Validation Targets, Layout. |
| Attribut | Delete-Icon in Attributzeile | Entfernt Attribut und Slots dieses Attributs. | `DELETE /api/v1/projects/{projectId}/classes/{classId}/attributes/{attributeId}` | Class Node, Object Slots, Validation Targets. |
| Operation | Delete-Icon in Operationenzeile | Entfernt Operation-Signatur. | `DELETE /api/v1/projects/{projectId}/classes/{classId}/operations/{operationId}` | Class Node, Properties Draft. |
| Association | `Delete Association` | Entfernt Association und zugehörige Objektlinks. | `DELETE /api/v1/projects/{projectId}/associations/{associationId}` | Class Edge, Object Link Edges, Explorer, Layout. |
| Invariante | `Delete Invariant` | Entfernt Invariante und alte Invariant-Ergebnisse. | `DELETE /api/v1/projects/{projectId}/invariants/{invariantId}` | Invariant Badge, OCL Editor, Validation Targets. |
| Objekt | `Delete Object` | Entfernt Objekt, Slots und zugehörige Objektlinks. | `DELETE /api/v1/projects/{projectId}/objects/{objectId}` | Object Node, Object Link Edges, Selection, Badges. |
| Objektlink | `Delete Link` | Entfernt konkreten Objektlink. | `DELETE /api/v1/projects/{projectId}/links/{linkId}` | Object Edge, Properties Panel, Validation Targets. |

Der Confirm Dialog soll nicht versuchen, die komplette Backend-Semantik nachzubauen. Für den MVP reicht ein klarer Hinweis auf bekannte Cascade-Folgen. Post-MVP kann ein `DeleteImpactDto` die konkrete Auswirkungsübersicht vom Backend liefern.

## Eingabevalidierung

Das Frontend führt nur UI-nahe Validierung durch. Fachliche Modellvalidierung bleibt im Backend.

| Prüfung | Frontend | Backend |
|---|---|---|
| Pflichtfelder | Ja | Ja |
| primitive Eingabeformate | Ja, soweit einfach | Ja |
| eindeutige Namen | optional lokal, final Backend | Ja |
| Multiplicity-Syntax | einfache Syntaxprüfung möglich | Ja |
| OCL-Syntax | Nein, nur über Backend-Diagnose | Ja |
| OCL-Typecheck | Nein | Ja |
| Slot-Typkompatibilität | einfache Primitive möglich | Ja |
| Association-/Link-Gültigkeit | UI kann Dropdowns filtern | Ja |
| Invariantenauswertung | Nein | Ja |

Formularfehler sollten feldnah erscheinen und zusätzlich im Validation Results Panel sichtbar sein, wenn sie aus dem Backend kommen.

## Update-Strategie

Für das Properties Panel gibt es drei relevante Strategien.

| Strategie | Beschreibung | Vorteil | Risiko | Empfehlung |
|---|---|---|---|---|
| expliziter Save | Änderungen werden als Draft gehalten und per Save gespeichert. | kontrolliert, weniger API-Last | mehr UI-Aufwand | gut für MVP |
| Debounced Updates | Änderungen werden nach kurzer Pause automatisch gespeichert. | flüssig | komplexer bei Fehlern und Undo | Post-MVP prüfen |
| sofortige lokale Updates | Project State wird sofort aktualisiert, Persistenz später. | Canvas/Explorer reagieren direkt | Dirty-State nötig | im MVP mit Save kombinieren |

Empfehlung für MVP:

- Panel-Felder ändern zunächst lokalen Draft oder Project State.
- Canvas und Explorer dürfen sofort aus dem lokalen State aktualisiert werden.
- Persistenz erfolgt über expliziten Save oder projektweiten Save.
- Backend-Fehler werden nach Save oder `Check Constraints` feldbezogen angezeigt.
- Nach Änderungen werden bestehende Validation Results als `stale` markiert.

## State Management

Das Properties Panel benötigt klar getrennte State-Arten.

| State | Zweck |
|---|---|
| `selectionState` | bestimmt, welches Panel angezeigt wird. |
| `projectState` | fachlicher Projektzustand aus Backend/DTOs. |
| `formDraftState` | noch nicht gespeicherte Formularwerte. |
| `dirtyState` | zeigt ungespeicherte Änderungen. |
| `validationState` | Backend-Fehler und Marker. |
| `apiState` | Lade-, Speicher- und Fehlerstatus. |
| `panelUiState` | aktive Segmente, aufgeklappte Bereiche, Scrollposition. |

Beispiel:

```ts
type PropertiesPanelState = {
  selection: PropertiesSelection;
  activeSegment: "general" | "details" | "validation";
  dirtyFields: string[];
  fieldErrors: Record<string, string[]>;
  saving: boolean;
};
```

## API-Interaktionen

Das Properties Panel ruft API-Endpunkte nicht direkt auf. Es nutzt Feature-Actions oder Services.

| Panel | Typische Aktion | Möglicher Endpoint |
|---|---|---|
| Class Properties | Klasse aktualisieren | `PUT /api/v1/projects/{projectId}/classes/{classId}` |
| Class Properties | Attribut hinzufügen | `POST /api/v1/projects/{projectId}/classes/{classId}/attributes` |
| Class Properties | Attribut löschen | `DELETE /api/v1/projects/{projectId}/classes/{classId}/attributes/{attributeId}` |
| Class Properties | Operation hinzufügen | `POST /api/v1/projects/{projectId}/classes/{classId}/operations` |
| Class Properties | Operation löschen | `DELETE /api/v1/projects/{projectId}/classes/{classId}/operations/{operationId}` |
| Class Properties | Klasse löschen | `DELETE /api/v1/projects/{projectId}/classes/{classId}` |
| Association Properties | Association aktualisieren | `PUT /api/v1/projects/{projectId}/associations/{associationId}` |
| Association Properties | Association löschen | `DELETE /api/v1/projects/{projectId}/associations/{associationId}` |
| Invariant Properties | Invariante aktualisieren | `PUT /api/v1/projects/{projectId}/invariants/{invariantId}` |
| Invariant Properties | Invariante löschen | `DELETE /api/v1/projects/{projectId}/invariants/{invariantId}` |
| Invariant Properties | OCL parse/typecheck | `POST /api/v1/projects/{projectId}/ocl/parse`, `POST /api/v1/projects/{projectId}/ocl/typecheck` |
| Object Properties | Objekt aktualisieren | `PUT /api/v1/projects/{projectId}/objects/{objectId}` |
| Object Properties | Objekt löschen | `DELETE /api/v1/projects/{projectId}/objects/{objectId}` |
| Object Properties | Slot-Wert setzen | `PUT /api/v1/projects/{projectId}/objects/{objectId}/slots/{slotId}` |
| Object Association Properties | Objektlink aktualisieren | `PUT /api/v1/projects/{projectId}/links/{linkId}` |
| Object Association Properties | Objektlink löschen | `DELETE /api/v1/projects/{projectId}/links/{linkId}` |
| Alle Panels | Projektweit speichern | `PUT /api/v1/projects/{projectId}` |
| Alle Panels | Constraints prüfen | `POST /api/v1/projects/{projectId}/validate` |

## Fehlerdarstellung

Fehler im Properties Panel haben zwei Quellen:

1. lokale UI-Validierung,
2. Backend-Diagnosen oder Validation Results.

| Fehlerquelle | Beispiel | Darstellung |
|---|---|---|
| lokales Pflichtfeld | Klassenname leer | Feldfehler unter Input |
| lokales Format | Integer enthält Dezimalzahl | Feldfehler und Save deaktivieren |
| Backend API Error | Speichern schlägt technisch fehl | globaler Panel-Fehler oder Toast |
| Backend Validation Result | `INVALID_SLOT_VALUE` | Slot-Feld und Objektstatus markieren |
| OCL Diagnostic | `TYPE_ERROR` | OCL-Feld und Diagnostics-Bereich markieren |
| Multiplicity Violation | `MULTIPLICITY_VIOLATION` | Association-/ObjectLink-Felder markieren, Details im Panel |

Das Panel sollte technische Fehler nicht mit fachlichen Constraint-Verletzungen vermischen. Ein `500` oder Netzwerkfehler ist ein API-Fehler; eine verletzte Invariante ist ein fachlicher Validation Error.

## MVP-Anforderungen

| ID | Anforderung | Priorität | Screenshot-Bezug |
|---|---|---|---|
| `PROP-MVP-001` | Panel zeigt Class Properties bei Klassenselektion. | MVP | `01-class-diagram-class-properties.png` |
| `PROP-MVP-002` | Panel zeigt Association Properties bei Association-Selektion. | MVP | `02-class-diagram-association-properties.png` |
| `PROP-MVP-003` | Panel zeigt Invariant Properties bei Invariantenselektion. | MVP | `03-class-diagram-invariant-properties.png` |
| `PROP-MVP-004` | Panel zeigt Object Properties bei Objektselektion. | MVP | `06-object-diagram-object-properties.png` |
| `PROP-MVP-005` | Panel zeigt Object Association Properties bei Objektlink-Selektion. | MVP | `12-object-diagram-association-properties.png` |
| `PROP-MVP-006` | Panel unterscheidet editable und readonly Felder. | MVP | alle Properties-Screenshots |
| `PROP-MVP-007` | Änderungen aktualisieren Frontend-State und sichtbare Diagramm-/Explorer-Daten. | MVP | fachliche Ableitung |
| `PROP-MVP-008` | Panel zeigt lokale Feldfehler. | MVP | UX-Anforderung |
| `PROP-MVP-009` | Panel zeigt Backend-Validation-Fehler feldbezogen, wenn möglich. | MVP | Validation UI |
| `PROP-MVP-010` | Nach Änderungen werden bestehende Validation Results als veraltet markiert. | MVP | Validation UI |
| `PROP-MVP-011` | Panel bietet Delete-Aktionen für Klasse, Association, Invariante, Objekt und Objektlink. | MVP | alle Properties-Screenshots |
| `PROP-MVP-012` | Attribute und Operationen können aus Class Properties entfernt werden. | MVP | `01-class-diagram-class-properties.png` |
| `PROP-MVP-013` | Delete-Aktionen öffnen einen Confirm Dialog und nutzen Backend-Cascade-Regeln. | MVP | UX-Anforderung |
| `PROP-MVP-014` | Bei selektierter Klasse bietet das Panel Segmente für `Class`, `Association` und `Invariant`. | MVP | `15-properties-association.png`, `16-properties-invariants.png`, `17-new-class.png` |
| `PROP-MVP-015` | Das `Association`-Segment zeigt alle Associations der selektierten Klasse und ermöglicht den Sprung in Association Properties. | MVP | `15-properties-association.png` |
| `PROP-MVP-016` | Das `Invariant`-Segment zeigt alle Invarianten der selektierten Klasse und ermöglicht den Sprung in Invariant Properties. | MVP | `16-properties-invariants.png` |

## Post-MVP-Erweiterungen

| Erweiterung | Beschreibung |
|---|---|
| Erweiterte Segment Controls je Panel | Zusätzliche Segmente wie `Validation` oder `Advanced`; die Basis-Segmente `Class`, `Association`, `Invariant` für selektierte Klassen gehören bereits zum MVP. |
| Inline-Erstellung komplexer Unterelemente | Attribute, Operationen oder Slots direkt mit Tabelleneditor bearbeiten. |
| Debounced Autosave | Änderungen automatisch nach kurzer Pause speichern. |
| Undo/Redo | Panel-Änderungen rückgängig machen. |
| Rich OCL Editor im Panel | Syntax Highlighting, Autocomplete, Source-Range-Fehler. |
| Bulk Editing | mehrere Elemente gemeinsam bearbeiten. |
| Kontextabhängige Quick Fixes | Fehler direkt aus dem Panel beheben. |
| Erweiterte UML-Eigenschaften | Vererbung, Enumerationen, Aggregation/Komposition, Assoziationsklassen. |
| Mehrere Snapshots | Object Properties snapshot-spezifisch anzeigen. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Gibt es für Association-, Invariant-, Object- und Link-Panels ebenfalls Segment Controls? | Für selektierte Klassen ist das Segment Control durch `15`, `16`, `17` geklärt; offen bleibt die Ausweitung auf andere Elementtypen. |
| Werden Attribute und Operationen im Panel inline oder über eigene Modale erstellt? | Beeinflusst Class Properties. |
| Werden Association-Endklassen nach Erstellung editierbar sein? | Kann Objektlinks ungültig machen. |
| Darf der Typ eines bestehenden Objekts geändert werden? | Kann Slots und Links ungültig machen. |
| Wird OCL beim Speichern einer Invariante automatisch parse/typecheck-geprüft? | Beeinflusst API-Last und Feedback. |
| Speichert das Panel Änderungen sofort oder nur projektweit? | Beeinflusst Dirty State und Fehlerbehandlung. |
| Wie werden Backend-Fehler auf einzelne Felder gemappt, wenn nur Modellpfade geliefert werden? | Erfordert stabilen Error Contract. |

## Zusammenfassung

Das Properties Panel ist der zentrale Detail- und Bearbeitungsbereich des Frontends. Es reagiert auf die aktuelle Selektion und zeigt typabhängig Klassen, Associations, Invarianten, Objekte oder Objektlinks.

Für den MVP muss das Panel stabile Selektion, klare Feldgruppen, editable/readonly-Unterscheidung, lokale Eingabevalidierung, Backend-Fehleranzeige, State-Synchronisation und eine kontrollierte Save-Strategie unterstützen. Die fachliche Semantik bleibt im Backend; das Panel macht sie für Nutzer bearbeitbar und nachvollziehbar.
