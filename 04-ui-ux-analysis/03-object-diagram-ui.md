# Object Diagram UI

## Zweck dieser Datei

Diese Datei beschreibt Anforderungen und Verhalten der Objektdiagramm-UI des neuen UML/OCL-Websystems.

Sie fokussiert auf:

- Darstellung von Objektinstanzen,
- Darstellung von Objekttypen wie `alice : User`,
- Darstellung von Attributwerten und Slots,
- Darstellung und Bearbeitung von Objektlinks,
- Explorer-Verhalten im Object-Diagram-Kontext,
- Properties Panel für Objekte und Objektlinks,
- Modal zum Erstellen von Objektlinks,
- fehlerhafte Objekte im Diagramm,
- Fehler-Badges und rote Fehlerrahmen,
- Zusammenhang zum Klassendiagramm,
- Zusammenhang zur Validierung.

Diese Datei ist eine UI/UX-Analyse. Sie beschreibt keine Implementierung und keine konkrete Diagrammbibliothek.

Die verbindliche, konsolidierte Ausgestaltung dieser Analyse liegt in:

- `26-unified-object-diagram-workspace.md` mit
  `assets/mockups/object-diagram-workspace.html`,
- `27-create-object-modal.md` mit `assets/mockups/create-object-modal.html`,
- `29-create-object-link-modal.md` mit
  `assets/mockups/create-object-link-modal.html`,
- `30-object-link-association-sidebar.md` mit
  `assets/mockups/object-link-association-sidebar.html`,
- `31-object-properties-validation-and-delete.md` mit
  `assets/mockups/object-properties-validation-and-delete.html`,
- `40-delete-object-link-modal.md` mit
  `assets/mockups/delete-object-link-modal.html`,
- `41-association-class-instance.md` mit
  `assets/mockups/association-class-instance.html`,
- `22-m7-object-diagram-invocation.md` mit
  `assets/mockups/object-diagram-operation-invocation.html`.

Die Begriffe `AddObjectAssociationModal`, `ObjectAssociationPropertiesPanel`
und Explorer-Gruppe `Associations` in den ursprünglichen Screenshots werden in
der konsolidierten UI als `Create Object Link`, `Object Properties ->
Associations` beziehungsweise `Object Links` geführt. Die Screenshots bleiben
nur visuelle Bestandsreferenzen und definieren nicht mehr die aktuelle
Komponentenbenennung.

Hinweis zur Ablage: Die Zielstruktur nennt `assets/screenshots/*.png`. Im aktuellen Repository liegen die Screenshots real unter `assets/screenshots/*.png`. Die Links in dieser Datei nutzen die aktuell vorhandenen Pfade.

## Relevante Screenshots

| Screenshot | Relevanz |
|---|---|
| `06-object-diagram-object-properties.png` | Objektdiagramm mit Objektinstanzen, Slots, Explorer und Object Properties. |
| `07-object-diagram-validation-error.png` | Fehlerhafte Objektinstanz mit rotem Rahmen, Fehler-Badge und Validation Results. |
| `11-modal-add-object-association.png` | Modal zum Erstellen eines Objektlinks. |
| `12-object-diagram-association-properties.png` | Properties Panel für Objektlink bzw. Objektassoziation. |

![Object Diagram Object Properties](../assets/screenshots/06-object-diagram-object-properties.png)

![Object Diagram Validation Error](../assets/screenshots/07-object-diagram-validation-error.png)

![Add Object Association Modal](../assets/screenshots/11-modal-add-object-association.png)

![Object Association Properties](../assets/screenshots/12-object-diagram-association-properties.png)

## Rolle des Objektdiagramms

Das Objektdiagramm zeigt einen konkreten Snapshot des UML-Modells.

Während das Klassendiagramm die Typstruktur definiert, zeigt das Objektdiagramm konkrete Instanzen:

| Klassendiagramm | Objektdiagramm |
|---|---|
| `UmlClass` | `ObjectInstance` |
| `UmlAttribute` | `Slot` |
| `UmlAssociation` | `ObjectLink` |
| `UmlAssociationEnd` | Link-End-Zuordnung |
| `UmlInvariant` | Prüfregel gegen Objekte |
| `Multiplicity` | Linkanzahl im Snapshot |

Das Objektdiagramm ist im MVP verantwortlich für:

- Objektinstanzen anzeigen und bearbeiten,
- Objekttypen anzeigen,
- Slot-Werte anzeigen und bearbeiten,
- Objektlinks anzeigen und erstellen,
- Snapshot-Zustand speichern,
- Validierungsfehler visuell hervorheben.

## Objektdiagramm-Canvas

Der `ObjectDiagramCanvas` ist der zentrale Arbeitsbereich für Snapshot-Daten.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Konkrete Objekte und Objektlinks eines Snapshots visualisieren. |
| Angezeigte Daten | `ObjectInstance`, `Slot`, `ObjectLink`, zugehörige `UmlClass`, `UmlAttribute`, `UmlAssociation`, Layoutdaten. |
| Nutzeraktionen | Objekt auswählen, Link auswählen, Objekt verschieben, Objektlink erstellen, Fehler erkennen. |
| Backend-Daten | Snapshot-Daten plus Klassendiagramm-Referenzen für Typnamen, Slotnamen und Association-Namen. |
| Lokaler Zustand | Selektion, Hover, Drag State, Viewport, Zoom/Pan, Fehler-Markierungen. |
| Screenshot-Bezug | `06`, `07`, `12` |

Canvas-Anforderungen:

- Objekte werden als Karten dargestellt.
- Objektlinks werden als Kanten dargestellt.
- Selektierte Elemente sind eindeutig hervorgehoben.
- Fehlerhafte Objekte können zusätzlich markiert werden.
- Korrektur und erneute Validierung müssen alte Fehlerzustände aktualisieren.
- Layoutpositionen müssen speicherbar sein.

## Objektkarte

Eine Objektkarte entspricht einer `ObjectInstance`.

![Object Properties](../assets/screenshots/06-object-diagram-object-properties.png)

| Bereich der Objektkarte | Inhalt |
|---|---|
| Kopf | Objektname und Typ, z. B. `alice : User`, `mobyDick : Book`. |
| Slots | Attributwerte, z. B. `title : Moby Dick`, `available : false`. |
| Selektion | Hervorhebung bei Auswahl. |
| Fehlerzustand | Roter Rahmen, Badge oder visuelles Error-Icon. |

Anforderungen:

| Anforderung | MVP | Hinweis |
|---|---|---|
| Objektname anzeigen | Ja | Sichtbares Label, nicht technische ID. |
| Objekttyp anzeigen | Ja | Muss Klasse aus dem UML-Modell referenzieren. |
| Slot-Werte anzeigen | Ja | Werte müssen typgerecht gerendert werden. |
| Objekt selektieren | Ja | Öffnet ObjectPropertiesPanel. |
| Objekt verschieben | Ja | Aktualisiert Layoutdaten. |
| Fehlerstatus anzeigen | Ja | Wichtig für `INVARIANT_VIOLATION` und Slot-Fehler. |

## Attribut-Slots

Slots zeigen konkrete Attributwerte eines Objekts.

| Aspekt | Beschreibung |
|---|---|
| Daten | `Slot.id`, `attributeId`, `attributeName`, `value`, `valueType`, optional `isUnset`. |
| Quelle | Attribute der referenzierten `UmlClass`. |
| Nutzeraktionen | Wert ansehen, im Properties Panel bearbeiten. |
| Validierungsbezug | Slot-Werte werden gegen Attributtypen geprüft und in OCL ausgewertet. |

Beispiel aus dem Screenshot:

| Objekt | Slot |
|---|---|
| `mobyDick : Book` | `title`, `author`, `available` |
| `alice : User` | sichtbar in Fehlerbeispiel mit `books : 6` |

MVP-Anforderungen:

- Slot-Liste folgt der Attributdefinition der Klasse.
- Slot-Werte sind im Properties Panel editierbar.
- Slot-Typen werden vom Backend validiert.
- Fehlerhafte Slot-Werte müssen im Objekt oder Properties Panel markierbar sein.

Offene Frage:

Es muss fachlich entschieden werden, ob jeder Attributwert zwingend vorhanden sein muss oder ob fehlende Werte als `unset`, `null`, `undefined` oder Validation Error behandelt werden.

## Objektlinks

Objektlinks entsprechen Instanzen von Assoziationen.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Konkrete Verbindung zwischen Objektinstanzen darstellen. |
| Daten | `ObjectLink.id`, `associationId`, beteiligte `ObjectInstance`-IDs, Association-End-Zuordnung. |
| Darstellung | Kante zwischen Objektkarten, Association Name mittig als Label; Endinformationen an beiden Objektenden, wenn Rollen/Multiplizitaeten der Association vorhanden sind. |
| Nutzeraktionen | Link auswählen, erstellen, ggf. ändern oder löschen. |
| Validierungsbezug | Link-Typprüfung, Multiplicity Checks, OCL-Navigation. |
| Screenshot-Bezug | `11`, `12` |

Link Labels:

| Label | Zweck |
|---|---|
| Association Name | Zeigt mittig auf dem Objektlink, welche Association instanziiert wird, z. B. `Borrows`. |
| Endlabels | Zeigen Source-/Target-Rollen und ggf. Multiplizitaeten aus der zugrundeliegenden Association nahe den beteiligten Objekten. |
| Fehlerlabel | Optional, z. B. Error-Icon an Link bei `INVALID_LINK`. |

MVP-Anforderungen:

- Objektlink muss auf existierender `UmlAssociation` basieren.
- Beteiligte Objekte müssen zu den Association Ends passen.
- Association Name muss im Canvas mittig auf dem Objektlink sichtbar sein.
- Alle Association Ends müssen endbasiert unterstützt und bei vorhandenen
  Rollen, Multiplizitäten und Qualifiern sichtbar sein. `Source` und `Target`
  bleiben reine Darstellungsbegriffe für den binären Fall.
- Link muss im Explorer unter `Object Links` erscheinen.
- Linkauswahl öffnet `Object Properties -> Associations` und unmittelbar die
  bearbeitbaren Endbelegungen des ausgewählten Object Links.
- Linkfehler müssen im Diagramm sichtbar markierbar sein.

## Explorer-Verhalten

Im Object Diagram zeigt die Explorer Sidebar Snapshot-Elemente.

| Gruppe | Inhalt | Aktionen |
|---|---|---|
| `Objects` | Alle `ObjectInstance`-Elemente. | Objekt auswählen, neues Objekt erstellen, falls UI vorgesehen. |
| `Object Links` | Alle `ObjectLink`-Elemente des aktuellen Snapshots. | Link auswählen oder `Create Object Link` öffnen. |

Explorer-Anforderungen:

- Auswahl eines Objekts selektiert die Objektkarte im Canvas.
- Auswahl eines Links selektiert die Kante im Canvas.
- `Create Object Link` öffnet den Dialog aus
  `assets/mockups/create-object-link-modal.html`.
- Fehlerhafte Objekte oder Links können Badges im Explorer erhalten.
- Explorer nutzt IDs für Selektion, Namen für Anzeige.

Die Entscheidung zur Objektanlage ist getroffen: Die Canvas-Aktion `Object`
öffnet den Dialog aus `assets/mockups/create-object-modal.html`. Nach Erfolg
wird das neue Objekt in Explorer, Canvas und Object Properties selektiert.

## Properties Panel für Objekte

Das Object Properties Panel zeigt Details des selektierten Objekts.

![Object Properties Panel](../assets/screenshots/06-object-diagram-object-properties.png)

| Feld/Bereich | Zweck | MVP |
|---|---|---|
| Object Name | Objektname anzeigen und bearbeiten. | Ja |
| Type | Klasse des Objekts anzeigen; ggf. als Dropdown ändern. | Ja |
| Attribute Slots | Slot-Werte anzeigen und bearbeiten. | Ja |
| Slot Validation | Typ- oder Wertfehler anzeigen. | Ja |

Erwartetes Verhalten:

- Objektname muss im Snapshot eindeutig sein.
- Type muss eine existierende Klasse sein.
- Slot-Liste muss zur ausgewählten Klasse passen.
- Änderung des Typs kann bestehende Slots ungültig machen und muss kontrolliert behandelt werden.
- Änderungen müssen Canvas, Explorer und Projektzustand aktualisieren.

## Object Properties für Object Links

Das Segment `Associations` in Object Properties zeigt die Details des
selektierten Object Links. `Associations` bezeichnet hier den Properties-
Kontext; die konkrete Snapshot-Instanz heißt weiterhin `Object Link`.

![Object Association Properties](../assets/screenshots/12-object-diagram-association-properties.png)

| Feld/Bereich | Zweck | MVP |
|---|---|---|
| Modeled Association | Zugrundeliegende Class-Model-Association read-only anzeigen und ins Class Diagram öffnen. | Ja |
| End assignments | Für jedes Association End das zugeordnete kompatible Objekt anzeigen oder bearbeiten. | Ja |
| Qualifier values | Konkrete typisierte Werte für vorhandene Qualifier bearbeiten. | Ja |
| Link Validation | Ungültige Kombinationen anzeigen. | Ja |

Erwartetes Verhalten:

- Alle zugeordneten Objekte müssen existieren.
- Association muss existieren.
- Objekte müssen zu den Association Ends passen.
- Änderungen am Link können Multiplicity- oder OCL-Ergebnisse beeinflussen.

## Modale Dialoge

### Create Object Link

![Add Object Association Modal](../assets/screenshots/11-modal-add-object-association.png)

Der Screenshot zeigt die historische binäre Ausgangsansicht. Verbindlich ist
`assets/mockups/create-object-link-modal.html` mit endbasierter Belegung.

| Feld | Beschreibung |
|---|---|
| Association Name | Auswahl existierender Association. |
| End assignments | Ein kompatibles Objekt je Association End. |
| Qualifier values | Typisierte Werte je qualifiziertem End. |
| Create Object Link | Erzeugt genau eine Link-Instanz im aktuellen Snapshot. |

Erwartetes Verhalten:

- Auswahlfelder zeigen nur vorhandene und je End typkompatible Objekte sowie
  modellierte Associations.
- Falls eine unzulässige Kombination möglich ist, muss das Backend sie ablehnen oder als Validation Error melden.
- Nach Erstellung erscheint die Kante im Canvas und der Link im Explorer.
- Neuer Link ist direkt selektiert oder sichtbar hervorgehoben.

### Create Object

Die verbindliche Objektanlage ist in
`assets/mockups/create-object-modal.html` festgelegt.

Mögliche Felder:

| Feld | Zweck |
|---|---|
| Object Name | Sichtbarer Objektname, z. B. `alice`. |
| Type | Auswahl einer vorhandenen `UmlClass`. |
| Initial Slots | Optional initiale Attributwerte. |

Initialwerte werden Classifier-gesteuert im Dialog gesetzt. Stored und Init
werden entsprechend ihrem Vertrag behandelt; Derived Values erscheinen
berechnet und read-only. Abstrakte Klassen sind nicht instanziierbar.

## Nutzeraktionen

| Nutzeraktion | UI-Auslöser | Systemreaktion |
|---|---|---|
| Object Diagram öffnen | Tab `Object Diagram` | Explorer, Canvas und Properties Panel wechseln in Snapshot-Kontext. |
| Objekt auswählen | Klick auf ObjectNode oder Explorer-Eintrag | Objekt wird markiert, ObjectPropertiesPanel öffnet. |
| Objekt erstellen | Add-Object-Aktion | Neues Objekt erscheint im Explorer und Canvas. |
| Objekt bearbeiten | ObjectPropertiesPanel | Name, Typ oder Slot-Werte werden aktualisiert. |
| Objektlink erstellen | `Object Link` im Canvas oder `Create Object Link` in Object Properties | Create-Object-Link-Dialog öffnet. |
| Objektlink auswählen | Klick auf Link oder Explorer-Eintrag | Link wird markiert; `Object Properties -> Associations` öffnet. |
| Objektlink bearbeiten | `Object Properties -> Associations` | Endbelegungen und Qualifierwerte werden aktualisiert; die modellierte Association bleibt read-only. |
| Constraints prüfen | `Check Constraints` | Backend validiert; Fehler werden markiert und im Panel angezeigt. |
| Fehler auswählen | Validation Results Panel | Betroffenes Objekt oder Link wird fokussiert. |
| Fehler korrigieren | Slot oder Link ändern | Erneute Validierung aktualisiert Fehlerzustand. |

## Backend-Datenbedarf

Die Objektdiagramm-UI benötigt Daten aus `ObjectModel` und `UmlModel`.

| Datenobjekt | Benötigte Felder |
|---|---|
| `ObjectModel` | `id`, `name`, `objects`, `links`. |
| `ObjectInstance` | `id`, `name`, `classId`, `slots`. |
| `Slot` | `id`, `attributeId`, `value`, `valueType`, optional `isUnset`. |
| `ObjectLink` | `id`, `associationId`, `endValues`. |
| `UmlClass` | `id`, `name`, `attributes`. |
| `UmlAttribute` | `id`, `name`, `type`. |
| `UmlAssociation` | `id`, `name`, `ends`. |
| `UmlAssociationEnd` | `id`, `classId`, `roleName`, `multiplicity`. |
| `LayoutInformation` | Positionen von Objekten und ggf. Linklayout. |
| `ValidationResult` | `objectIds`, `linkIds`, `modelElementIds`, `code`, `message`. |

## API-Bezug

Mögliche API-Fähigkeiten für die Objektdiagramm-UI:

| API-Fähigkeit | Zweck |
|---|---|
| Snapshot laden | Objekte, Slots und Links anzeigen. |
| Objekt erstellen | Neue Objektinstanz erzeugen. |
| Objekt aktualisieren | Name, Typ oder Slots ändern. |
| Objekt löschen | Objekt entfernen und abhängige Links behandeln. |
| Slot aktualisieren | Attributwert setzen. |
| Objektlink erstellen | Neuen Link zwischen Objekten erzeugen. |
| Objektlink aktualisieren | Endbelegungen und Qualifierwerte ändern. |
| Objektlink löschen | Link entfernen. |
| Layout speichern | Objektpositionen speichern. |
| Constraints prüfen | Snapshot und Invarianten validieren. |

API-Anforderungen:

- API muss stabile IDs liefern.
- API muss Typ- und Linkfehler strukturiert melden.
- API muss Validation Results mit `objectIds` und `linkIds` liefern.
- API sollte Dropdown-Daten für Objekte, Klassen und Associations konsistent bereitstellen.

## State-Bezug

Lokaler UI-State im Object Diagram:

| State | Zweck |
|---|---|
| `activeView` | Aktuell `Object Diagram`. |
| `selectedElementId` | Ausgewähltes Objekt oder Link. |
| `selectedElementType` | `object` oder `objectLink`. |
| `openModal` | `createObject` oder `createObjectLink`. |
| `formDrafts` | Unbestätigte Properties-Eingaben. |
| `diagramViewport` | Pan/Zoom-Zustand. |
| `dragState` | Temporäre Objektposition beim Verschieben. |
| `dirtyState` | Ungespeicherte Snapshot-Änderungen. |
| `validationResult` | Letztes Validierungsergebnis. |
| `invalidObjectIds` | Aus Validation Results abgeleitete Objekt-Markierungen. |
| `invalidLinkIds` | Aus Validation Results abgeleitete Link-Markierungen. |
| `focusedFromValidation` | Objekt oder Link, das aus dem Validation Panel fokussiert wurde. |

Persistenter Zustand:

- Objekte,
- Slots,
- Objektlinks,
- Layoutpositionen,
- ggf. letzter gespeicherter Snapshot.

## Validierungsbezug

Das Objektdiagramm ist der primäre Ort für sichtbare Snapshot-Validierungsfehler.

| Fehlerart | Typischer Bezug | UI-Darstellung |
|---|---|---|
| `UNKNOWN_CLASS` | Objekt referenziert fehlende Klasse. | Objektkarte markieren, Type-Feld markieren. |
| `UNKNOWN_ATTRIBUTE` | Slot referenziert fehlendes Attribut. | Objekt und Slot markieren. |
| `INVALID_SLOT_VALUE` | Slot-Wert passt nicht zum Attributtyp. | Slot-Feld und Objektkarte markieren. |
| `INVALID_LINK` | Link passt nicht zur Association. | Link und beteiligte Objekte markieren. |
| `MULTIPLICITY_VIOLATION` | Zu viele/zu wenige Links. | Betroffene Objekte und Links markieren. |
| `INVARIANT_VIOLATION` | OCL-Ausdruck ergibt false für ein Objekt. | Betroffenes Objekt markieren. |
| `EVALUATION_ERROR` | OCL kann für Objekt nicht ausgewertet werden. | Objekt und Invariante im Panel referenzieren. |

Die Screenshot-Fehlerdarstellung zeigt:

- roter Rahmen um `alice : User`,
- Fehler-Badge an der Objektkarte,
- Validation Results Panel mit Kontext und Invariante.

## Fehlerhafte Objekte

![Invalid Object Highlight](../assets/screenshots/07-object-diagram-validation-error.png)

Fehlerhafte Objekte müssen eindeutig erkennbar sein, ohne die Objektkarte unlesbar zu machen.

MVP-Darstellung:

| Element | Verhalten |
|---|---|
| Roter Rahmen | Markiert Objekt mit Fehler. |
| Fehler-Badge | Zeigt, dass Objekt an mindestens einem Fehler beteiligt ist. |
| Validation Results Panel | Erklärt Fehlertext, Kontext und Constraint. |
| Objektselektion | Klick auf Fehler kann Objekt fokussieren. |

Mehrere Fehler:

| Fall | UI-Verhalten |
|---|---|
| Ein Objekt mit einem Fehler | Ein Badge oder Markierung genügt. |
| Ein Objekt mit mehreren Fehlern | Badge kann Anzahl zeigen. |
| Mehrere Objekte fehlerhaft | Alle relevanten Objektkarten markieren. |
| Fehler an Link und Objekt | Link und beteiligte Objektkarten markieren. |

Klick auf Validation Result:

1. Validation Result wird ausgewählt.
2. Object Diagram wird geöffnet, falls eine andere View aktiv ist.
3. Betroffenes Objekt wird selektiert.
4. Canvas fokussiert oder scrollt zum Objekt.
5. Properties Panel zeigt Object Properties.

Diese Interaktion ist für den MVP sehr hilfreich, aber die Mindestanforderung ist, dass die Markierung im Diagramm sichtbar ist.

## MVP-Anforderungen

| Bereich | MVP-Anforderung |
|---|---|
| Canvas | Objekte und Objektlinks anzeigen. |
| Object Link Labels | Association-Name mittig; Endinformationen an beiden Objektenden anzeigen, sofern vorhanden. |
| Objektkarte | Objektname, Typ und Slots darstellen. |
| Selektion | Objekte und Links auswählbar machen. |
| Explorer | Gruppen `Objects` und `Object Links`. |
| Object Properties | Name, Type und Slot-Werte anzeigen/bearbeiten. |
| Link Properties | Modellierte Association read-only sowie alle Endbelegungen und Qualifierwerte anzeigen/bearbeiten. |
| Create Object | Classifier-gesteuerte Objektanlage mit initialen Slotwerten. |
| Create Object Link | Endbasierter Dialog für binäre und n-äre Objektlinks. |
| Layout | Objektpositionen speichern. |
| Validierung | Fehlerhafte Objekte markieren. |
| Validation Results | Objektfehler textuell mit Objekt- und Constraint-Bezug anzeigen. |
| Backend | Snapshot-Daten und Validation Results strukturiert liefern. |

## Post-MVP-Erweiterungen

| Erweiterung | Nutzen |
|---|---|
| Fehlernavigation aus Validation Panel | Schneller Fokus auf betroffene Objekte/Links. |
| Inline-Slot-Editing im Objektknoten | Weniger Wechsel zum Properties Panel. |
| Automatische Linkvorschläge | Nur fachlich zulässige Associations anbieten. |
| Mehrere Snapshots | Umschalten zwischen Objektzuständen. |
| Snapshot-Vergleich | Unterschiede zwischen Snapshots anzeigen. |
| Auto-Layout | Übersicht bei größeren Objektmengen. |
| Link-Routing | Bessere Lesbarkeit bei vielen Kanten. |
| Objekt-Templates | Objekte aus Klassen mit Default-Slots erzeugen. |
| Quick Fixes | Fehler direkt aus Validation Results korrigieren. |
| Erweiterte Badges | Fehleranzahl, Severity oder Fehlerart am Objekt anzeigen. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Dürfen Objekttypen nachträglich geändert werden? | Kann Slots und Links ungültig machen. |
| Müssen alle Attribute als Slots vorhanden sein? | Einfluss auf Snapshot-Validierung und UI. |
| Wie werden fehlende Slot-Werte dargestellt? | `unset`, leer, `null`, Warning oder Error. |
| Wie wird bei mehreren Fehlern an einem Objekt die Anzahl dargestellt? | Badge-Design und Validation Mapping. |
| Wie werden Linkfehler visuell dargestellt? | Screenshots zeigen primär Objektfehler. |

## Zusammenfassung

Die Objektdiagramm-UI zeigt den konkreten Snapshot eines UML-Modells. Sie macht sichtbar, welche Objekte existieren, welche Attributwerte gesetzt sind und welche Objektlinks den Assoziationen des Klassendiagramms entsprechen.

Für den MVP muss die UI Objektkarten mit Typ und Slots, Objektlinks, Explorer-Navigation, Properties Panels und die Erstellung von Objektlinks unterstützen. Zusätzlich ist eine Objektanlage erforderlich, auch wenn dafür noch kein Screenshot vorliegt.

Der wichtigste UI-Beitrag des Objektdiagramms liegt in der Validierung: Fehlerhafte Objekte und Links müssen direkt im Diagramm markiert werden, während das Validation Results Panel die fachlichen Details liefert. Dadurch wird der vertikale Ablauf von Modellierung, Snapshot-Erstellung, Constraint Check und Fehlerkorrektur nachvollziehbar.
