# Modal Dialogs

## Zweck dieser Datei

Diese Datei beschreibt die modalen Dialoge des React/TypeScript-Frontends. Modals dienen im MVP vor allem dazu, neue Modell- und Snapshot-Elemente fokussiert anzulegen, ohne den Nutzer aus dem aktuellen Diagrammkontext zu reißen.

Die Datei definiert Zweck, Auslöser, Eingabefelder, Validierungsregeln, Backend-Interaktionen, Erfolgs- und Fehlerzustände sowie Screenshot-Bezüge für die wichtigsten Dialoge.

## Rolle von Modals im UI

Modale Dialoge werden für kurze, fokussierte Erstellungsaktionen verwendet. Sie sollen keine komplexe Langzeitbearbeitung ersetzen. Detailbearbeitung erfolgt nach der Erstellung primär im Properties Panel.

MVP-Modals:

- `CreateNewProjectModal`
- `OpenExistingProjectModal`
- `AddNewClassModal`
- `AddInvariantModal`
- `AddClassAssociationModal`
- `AddObjectAssociationModal`

Perspektivische Modals:

- `AddObjectModal`
- `AddAttributeModal`
- `AddOperationModal`

Grundprinzip:

1. Modal öffnet aus aktuellem Kontext.
2. Kontext wird soweit möglich vorausgewählt.
3. Nutzer füllt wenige Pflichtfelder aus.
4. Frontend prüft einfache UI-Regeln.
5. Submit erzeugt oder speichert das Element.
6. Neues Element erscheint im Diagramm oder Explorer.
7. Neues Element wird nach Möglichkeit selektiert.
8. Properties Panel zeigt die Details für weitere Bearbeitung.

## Relevante Screenshots

| Screenshot | Vorschau | Dialog |
|---|---|---|
| `18-create-new-projects.png` | ![Create New Project](../assets/screenshots/18-create-new-projects.png) | Neues Projekt mit Namen erstellen |
| `08-modal-add-class.png` | ![Add New Class Modal](../assets/screenshots/08-modal-add-class.png) | Klasse erstellen |
| `09-modal-add-invariant.png` | ![Add Invariant Modal](../assets/screenshots/09-modal-add-invariant.png) | OCL-Invariante erstellen |
| `10-modal-add-class-association.png` | ![Add Class Association Modal](../assets/screenshots/10-modal-add-class-association.png) | UML-Association erstellen |
| `11-modal-add-object-association.png` | ![Add Object Association Modal](../assets/screenshots/11-modal-add-object-association.png) | Objektlink erstellen |
| `14-open-existing-project.png` | ![Open Existing Project Modal](../assets/screenshots/14-open-existing-project.png) | Lokale `.use`-Datei öffnen/importieren |

## Gemeinsame Modal-Prinzipien

| Prinzip | Beschreibung |
|---|---|
| Fokus | Beim Öffnen liegt der Fokus auf dem ersten sinnvollen Eingabefeld. |
| Pflichtfelder | Submit ist deaktiviert, solange Pflichtfelder offensichtlich fehlen. |
| Kontext | Wenn aus einem ausgewählten Element geöffnet, wird dieses vorausgewählt. |
| Dropdowns | Klassen, Objekte und Associations werden aus dem aktuellen Project State befüllt. |
| Abhängigkeiten | Abhängige Felder aktualisieren sich nach Auswahl, etwa passende Objekte nach Association. |
| Cancel/Close | Schließt das Modal ohne Änderung. |
| Submit | Führt lokale Aktion oder Backend-Aufruf aus und zeigt Ladezustand. |
| Fehler | Lokale Fehler erscheinen feldnah; Backend-Fehler erscheinen im Modal und ggf. Console. |
| Erfolg | Modal schließt, Element wird erzeugt, selektiert und im Diagramm sichtbar. |
| Keine fachliche Vollvalidierung | OCL, Multiplicity, Link- und Constraint-Semantik werden final im Backend validiert. |

Empfohlener Modal-State:

```ts
type ModalState =
  | { type: "createNewProject" }
  | { type: "openExistingProject" }
  | { type: "addClass"; initialPosition?: { x: number; y: number } }
  | { type: "addInvariant"; contextClassId?: string }
  | { type: "addClassAssociation"; sourceClassId?: string; targetClassId?: string }
  | { type: "addObjectAssociation"; associationId?: string; sourceObjectId?: string }
  | { type: "addObject"; classId?: string }
  | null;
```

## Create New Project Modal

Das `CreateNewProjectModal` wird vom Dashboard über `+ Start Project` geöffnet. Es ist der erste Projekterstellungsdialog vor dem Wechsel in den Projektworkspace.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Neues Projekt mit einem nutzerverständlichen Namen anlegen. |
| Auslöser | Dashboard-Karte `Create New Model`, Button `+ Start Project`. |
| Screenshot | `18-create-new-projects.png` |
| Kontext | Dashboard bleibt im Hintergrund; Projektworkspace wird erst nach erfolgreicher Backend-Antwort geöffnet. |

Eingabefelder und UI-Elemente:

| Feld/UI-Element | Typ | Pflicht | Validierungsregel |
|---|---|---|---|
| Projektname | Text Input | Ja | Darf nicht leer sein; führende und folgende Leerzeichen werden getrimmt. |
| Create/Start Project | Submit Button | Ja | deaktiviert oder zeigt Feldfehler, solange der Projektname ungültig ist. |
| Cancel / Close | Button/Icon | Nein | schließt den Dialog ohne Projektanlage. |

Backend-Interaktion:

| Aktion | Endpoint | Payload |
|---|---|---|
| Projekt erstellen | `POST /api/v1/projects` | `CreateProjectRequestDto` mit `name`, optional später `description` oder `templateId` |

Erfolgsfall:

- Backend liefert `ProjectDto` mit stabiler `project.id` und `project.name`.
- Frontend übernimmt das Projekt in den Server State.
- Dialog schließt.
- Navigation führt zu `/projects/{projectId}/class-diagram`.

Fehlerfall:

- Leerer Projektname wird lokal feldnah angezeigt.
- Backend-Fehler wie ungültiger Name oder Speicherfehler bleiben im Dialog sichtbar.
- Dashboard bleibt erhalten; es wird nicht in einen halb initialisierten Workspace navigiert.

## Open Existing Project Modal

Das `OpenExistingProjectModal` wird vom Dashboard über die Karte `Open Existing` geöffnet. Es ist der Einstieg für lokale `.use`-Dateien, beispielsweise aus dem `examples/`-Ordner des originalen USE-Projekts.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Lokale UML/OCL-Spezifikation als `.use`-Datei auswählen und in das neue Websystem importieren oder als Modelltext anwenden. |
| Auslöser | Dashboard-Karte `Open Existing`. |
| Screenshot | `14-open-existing-project.png` |
| Kontext | Dashboard bleibt im Hintergrund abgedunkelt; Import findet vor dem Projektworkspace statt. |

Eingabefelder und UI-Elemente:

| Feld/UI-Element | Typ | Pflicht | Validierungsregel |
|---|---|---|---|
| `Local File` | Tab/Segment | Ja | MVP nutzt lokale Datei als primären Importweg. |
| Upload-/Dropzone | File Input + Drag-and-drop | Ja | akzeptiert `.use`; leere Datei oder falsche Endung erzeugt lokalen Fehler. |
| Supported Format Hinweis | Readonly Text | Ja | zeigt `Supported format: .use`. |
| `Open Project` | Submit Button | Ja | deaktiviert, solange keine gültige Datei gewählt wurde oder ein Import läuft. |
| `Cancel` / Close | Button/Icon | Nein | schließt das Modal ohne Import. |

Backend-Interaktion:

| Variante | Endpoint | Einordnung |
|---|---|---|
| MVP-nah | Frontend liest Dateiinhalt, erstellt bei Bedarf ein Projekt und ruft `POST /api/v1/projects/{projectId}/model-text/apply` auf. | Passt zum OCL-/Model-Text-Editor und vermeidet vollständige Import-Kompatibilität im ersten Schnitt. |
| Alternativ | `POST /api/v1/projects/import/use` mit Dateiinhalt oder Multipart Upload. | Sinnvoll, sobald Backend einen dedizierten USE-Import-Service besitzt. |

Erfolgsfall:

- Modal schließt.
- importiertes oder neu erzeugtes Projekt wird in den Frontend Server State übernommen.
- bei vollständig angewendetem Modell öffnet die Class Diagram View.
- bei Diagnosen kann der OCL Editor geöffnet werden, damit Nutzer den Modelltext prüfen und korrigieren können.
- Console protokolliert Importstart, Erfolg, Warnungen oder Fehler.

Fehlerfall:

- falsche Dateiendung wird lokal im Modal angezeigt.
- Backend-Diagnosen wie `UNSUPPORTED_USE_FEATURE`, `MODEL_TEXT_SYNTAX_ERROR` oder `IMPORT_FAILED` bleiben sichtbar.
- Modal bleibt geöffnet oder führt in den OCL Editor mit Diagnostics, falls der Text teilweise übernommen werden kann.
- vollständige USE-Kompatibilität wird nicht impliziert.

## Add New Class Modal

Das `AddNewClassModal` erstellt eine neue UML-Klasse im Klassendiagramm.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Neue `UmlClass` im UML-Modell anlegen. |
| Auslöser | Toolbar `Add Class`, Explorer `+`, Canvas-Kontextaktion. |
| Screenshot | `08-modal-add-class.png` |
| Kontext | Optional Canvas-Position für neue Klasse. |

Eingabefelder:

| Feld | Typ | Pflicht | Validierungsregel |
|---|---|---|---|
| Class Name | Text Input | Ja | nicht leer, keine reinen Leerzeichen, eindeutig im Modell |
| Initial Attributes | optional Liste | Nein | Attribute müssen gültige Namen und Typen haben |
| Initial Operations | optional Liste | Nein | Operationen sind Signaturen, keine Implementierung |

Backend-Interaktion:

| Aktion | Möglicher Endpoint | Request |
|---|---|---|
| Klasse erstellen | `POST /api/v1/projects/{projectId}/classes` | Name, optionale Position, optionale Attribute/Operationen |
| alternativ Projekt speichern | `PUT /api/v1/projects/{projectId}` | aktualisierter Projektzustand |

Erfolgsfall:

- Modal schließt.
- Klasse erscheint als `UmlClassNode`.
- neue Klasse wird selektiert.
- `ClassPropertiesPanel` öffnet.
- Explorer Sidebar aktualisiert `Classes`.

Fehlerfall:

- lokaler Pflichtfeldfehler bleibt im Modal.
- Backend-Fehler wie doppelter Name wird feldnah angezeigt.
- Modal bleibt geöffnet.
- Submit Button wird nach Fehlermeldung wieder aktiv.

## Add Invariant Modal

Das `AddInvariantModal` erstellt eine neue OCL-Invariante.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Neue `UmlInvariant` mit Kontextklasse und OCL-Ausdruck anlegen. |
| Auslöser | Toolbar `Add Invariant`, Explorer `+`, Class Node, OCL Editor. |
| Screenshot | `09-modal-add-invariant.png` |
| Kontext | Ausgewählte Klasse kann als Kontextklasse vorausgewählt werden. |

Eingabefelder:

| Feld | Typ | Pflicht | Validierungsregel |
|---|---|---|---|
| Context Class | Dropdown | Ja | muss existierende Klasse sein |
| Invariant Name | Text Input | Ja | nicht leer, idealerweise eindeutig pro Kontextklasse |
| OCL Expression | mehrzeiliges Textfeld | Ja | nicht leer; fachliche Prüfung durch Backend |

Backend-Interaktion:

| Aktion | Möglicher Endpoint | Request |
|---|---|---|
| Invariante erstellen | `POST /api/v1/projects/{projectId}/invariants` | `contextClassId`, `name`, `expression` |
| optional OCL parse | `POST /api/v1/projects/{projectId}/ocl/parse` | Kontextklasse und Ausdruck |
| optional OCL typecheck | `POST /api/v1/projects/{projectId}/ocl/typecheck` | Kontextklasse und Ausdruck |

Erfolgsfall:

- Modal schließt.
- Invariante erscheint im Explorer unter `Invariants`.
- Invariante wird an der Kontextklasse sichtbar.
- neue Invariante wird selektiert.
- `InvariantPropertiesPanel` oder OCL Editor zeigt Details.

Fehlerfall:

- leere Pflichtfelder werden lokal markiert.
- Syntax-/Typecheck-Fehler werden als Backend-Diagnosen angezeigt, falls Prüfung beim Submit erfolgt.
- Bei reinem Speichern kann ein fachlicher OCL-Fehler auch erst bei `Check Constraints` erscheinen.

## Add Class Association Modal

Das `AddClassAssociationModal` erstellt eine UML-Association im Klassendiagramm.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Neue `UmlAssociation` zwischen zwei Klassen anlegen. |
| Auslöser | Toolbar `Add Association`, Explorer `+`, Auswahl zweier Klassen. |
| Screenshot | `10-modal-add-class-association.png` |
| Kontext | Selektierte Klasse kann Source oder Target vorauswählen. |

Eingabefelder:

| Feld | Typ | Pflicht | Validierungsregel |
|---|---|---|---|
| Association Name | Text Input | Ja/Should | nicht leer oder nach Modellregel optional |
| Source Class | Dropdown | Ja | existierende Klasse |
| Source Role | Text Input | Should | gültiger Rollenname für OCL-Navigation |
| Source Multiplicity | Text Input oder Select | Ja | gültige Syntax, z. B. `1`, `0..1`, `0..*`, `1..*` |
| Target Class | Dropdown | Ja | existierende Klasse |
| Target Role | Text Input | Should | gültiger Rollenname für OCL-Navigation |
| Target Multiplicity | Text Input oder Select | Ja | gültige Multiplicity-Syntax |

Abhängigkeiten:

- Source und Target Class beeinflussen zulässige Rollen und spätere Objektlinks.
- Rollen beeinflussen OCL-Navigation.
- Multiplizitäten beeinflussen spätere Snapshot-Validierung.

Backend-Interaktion:

| Aktion | Möglicher Endpoint | Request |
|---|---|---|
| Association erstellen | `POST /api/v1/projects/{projectId}/associations` | Name, Endklassen, Rollen, Multiplizitäten |
| alternativ Projekt speichern | `PUT /api/v1/projects/{projectId}` | aktualisierter Projektzustand |

Erfolgsfall:

- Modal schließt.
- Association erscheint als Edge im Class Diagram.
- Edge wird selektiert.
- `AssociationPropertiesPanel` öffnet.
- Explorer Sidebar aktualisiert `Associations`.

Fehlerfall:

- ungültige Multiplicity-Syntax wird lokal markiert.
- Backend kann doppelte Rollen, ungültige Klassen oder semantische Konflikte ablehnen.
- Modal bleibt geöffnet und zeigt die betroffenen Felder.

## Add Object Association Modal

Das `AddObjectAssociationModal` erstellt einen Objektlink im Objektdiagramm. Im Screenshot ist dies als Add Association im Object Diagram sichtbar; fachlich handelt es sich um `ObjectLink`.

| Aspekt | Beschreibung |
|---|---|
| Zweck | Neuen `ObjectLink` zwischen Objektinstanzen anlegen. |
| Auslöser | Object Diagram Toolbar, Explorer `+` bei Associations, Kontextaktion auf Objekt. |
| Screenshot | `11-modal-add-object-association.png` |
| Kontext | Selektiertes Objekt kann als Source Object vorausgewählt werden. |

Eingabefelder:

| Feld | Typ | Pflicht | Validierungsregel |
|---|---|---|---|
| Association | Dropdown | Ja | muss existierende UML-Association sein |
| Source Object | Dropdown | Ja | Objekt muss zur Source Class der Association passen |
| Target Object | Dropdown | Ja | Objekt muss zur Target Class der Association passen |
| Label | abgeleitet oder Text | Nein | standardmäßig Association-Name |

Abhängigkeiten:

- Auswahl der Association filtert Source-/Target-Objekte.
- Auswahl des Source Object kann Target-Optionen einschränken.
- Bei selbstreferenziellen Associations kann dieselbe Klasse auf beiden Seiten stehen.

Backend-Interaktion:

| Aktion | Möglicher Endpoint | Request |
|---|---|---|
| ObjectLink erstellen | `POST /api/v1/projects/{projectId}/links` | `associationId`, Source/Target Object IDs |
| alternativ Projekt speichern | `PUT /api/v1/projects/{projectId}` | aktualisierter Snapshot |

Erfolgsfall:

- Modal schließt.
- Link erscheint als `ObjectLinkEdge`.
- neuer Link wird selektiert.
- `ObjectAssociationPropertiesPanel` öffnet.
- Explorer Sidebar aktualisiert Objektlinks.

Fehlerfall:

- fehlende Dropdown-Auswahl wird lokal markiert.
- Backend kann Link wegen Typfehler, fehlender Association oder ungültigem Snapshot ablehnen.
- Multiplicity Violations können sofort gemeldet oder erst beim nächsten `Check Constraints` sichtbar werden.

## Perspektivische Modals

| Modal | Zweck | MVP/Post-MVP | Bemerkung |
|---|---|---|---|
| `AddObjectModal` | Objektinstanz mit Name und Klasse erstellen. | MVP fachlich nötig | Kein Screenshot vorhanden, aber für MVP-Workflow erforderlich. |
| `AddAttributeModal` | Attribut mit Name und Typ erstellen. | Should/Post-MVP | Alternative: inline im Class Properties Panel. |
| `AddOperationModal` | Operation als Signatur erstellen. | Should/Post-MVP | Alternative: inline im Class Properties Panel. |
| `ConfirmDeleteModal` | Löschaktionen absichern. | Should | Wichtig bei abhängigen Objekten, Links oder Invarianten. |
| `ImportProjectModal` | JSON oder vollständige `.use`-Kompatibilität importieren. | Post-MVP | Der sichtbare Open-Existing-Dialog ist bereits MVP-nah; vollständige Kompatibilität bleibt später. |

Empfohlenes `AddObjectModal` im MVP:

| Feld | Typ | Pflicht | Validierungsregel |
|---|---|---|---|
| Object Name | Text Input | Ja | eindeutig im Snapshot |
| Class | Dropdown | Ja | existierende Klasse |
| Initial Slot Values | optional Liste | Nein | Werte müssen zu Attributtypen passen |

## Formularvalidierung

Modals validieren nur UI-nahe Eingaben. Fachliche Semantik bleibt Aufgabe des Backends.

| Validierung | Frontend | Backend |
|---|---|---|
| Pflichtfelder gefüllt | Ja | Ja |
| leere Namen verhindern | Ja | Ja |
| einfache Namenssyntax | Should | Ja |
| eindeutige Namen | optional lokal | Ja |
| gültige Dropdown-Auswahl | Ja | Ja |
| Multiplicity-Syntax | einfache Prüfung | Ja |
| OCL-Syntax | Nein, nur optional via Backend-Endpunkt | Ja |
| OCL-Typecheck | Nein | Ja |
| ObjectLink passt zur Association | Dropdown-Filter möglich | Ja |
| Multiplicity Violation | Nein | Ja |

Disabled States:

- Submit ist deaktiviert, solange Pflichtfelder fehlen.
- Submit ist deaktiviert, während ein Request läuft.
- abhängige Dropdowns sind deaktiviert, bis ihr Vorgängerfeld gesetzt ist.
- OCL-Parse/Typecheck Buttons sind deaktiviert, wenn Ausdruck leer ist.

## Backend-Interaktionen

Modals sollten nicht direkt HTTP aufrufen. Sie nutzen Feature-Actions aus Class Diagram, Object Diagram oder OCL Editor.

| Modal | Feature-Action | Möglicher Endpoint |
|---|---|---|
| `CreateNewProjectModal` | `createProject(input)` | `POST /api/v1/projects` |
| `AddNewClassModal` | `createClass(input)` | `POST /api/v1/projects/{projectId}/classes` |
| `AddInvariantModal` | `createInvariant(input)` | `POST /api/v1/projects/{projectId}/invariants` |
| `AddClassAssociationModal` | `createAssociation(input)` | `POST /api/v1/projects/{projectId}/associations` |
| `AddObjectAssociationModal` | `createObjectLink(input)` | `POST /api/v1/projects/{projectId}/links` |
| `AddObjectModal` | `createObject(input)` | `POST /api/v1/projects/{projectId}/objects` |
| `OpenExistingProjectModal` | `openExistingUseFile(file)` | `POST /api/v1/projects/{projectId}/model-text/apply` oder später `POST /api/v1/projects/import/use` |

Bei einem projektbasierten Save-Modell können Modals stattdessen den lokalen Project State aktualisieren und die Persistenz einem späteren `Save` überlassen.

## Erfolgs- und Fehlerzustände

| Zustand | UI-Verhalten |
|---|---|
| Initial | Felder leer oder aus Kontext vorbelegt. |
| Dirty | Nutzer hat Eingaben geändert. |
| Invalid Local | Pflichtfeld oder Formatfehler; Submit deaktiviert oder blockiert. |
| Submitting | Submit Button zeigt Ladezustand, Close kann optional blockiert sein. |
| Success | Modal schließt, Element wird selektiert und sichtbar gemacht. |
| Backend Validation Error | Modal bleibt offen, feldbezogene Fehlermeldung anzeigen. |
| API Error | technische Fehlermeldung anzeigen, Retry ermöglichen. |
| Cancel | Modal schließt ohne Änderung, Fokus kehrt zum Auslöser zurück. |

Fehlermeldungen sollten kurz und feldnah sein:

| Fehler | Beispielmeldung |
|---|---|
| Projektname fehlt | `Project name is required.` |
| Klassenname fehlt | `Class name is required.` |
| Kontextklasse fehlt | `Select a context class.` |
| OCL-Ausdruck fehlt | `Enter an OCL expression.` |
| Multiplicity ungültig | `Use a multiplicity such as 1, 0..1, 0..* or 1..*.` |
| Source Object passt nicht | `Selected object does not match the association end type.` |

## MVP-Anforderungen

| ID | Anforderung | Priorität | Screenshot-Bezug |
|---|---|---|---|
| `MOD-MVP-000A` | Nutzer kann nach `Start Project` einen Projektnamen erfassen und ein Projekt erstellen. | MVP | `18-create-new-projects.png` |
| `MOD-MVP-001` | Nutzer kann eine Klasse per Modal erstellen. | MVP | `08-modal-add-class.png` |
| `MOD-MVP-000` | Nutzer kann `Open Existing` als Modal für lokale `.use`-Dateien öffnen. | MVP/Should | `14-open-existing-project.png` |
| `MOD-MVP-002` | Nutzer kann eine Invariante per Modal erstellen. | MVP | `09-modal-add-invariant.png` |
| `MOD-MVP-003` | Nutzer kann eine UML-Association per Modal erstellen. | MVP | `10-modal-add-class-association.png` |
| `MOD-MVP-004` | Nutzer kann einen Objektlink per Modal erstellen. | MVP | `11-modal-add-object-association.png` |
| `MOD-MVP-005` | Modals prüfen Pflichtfelder lokal. | MVP | alle Modal-Screenshots |
| `MOD-MVP-006` | Dropdowns werden aus aktuellem Project State befüllt. | MVP | Association- und Invariant-Modals |
| `MOD-MVP-007` | Submit zeigt Disabled/Loading States. | MVP | UI-Prinzip |
| `MOD-MVP-008` | Erfolgreich erstellte Elemente werden selektiert. | MVP | Workflow-Anforderung |
| `MOD-MVP-009` | Backend-Fehler bleiben im Modal sichtbar und verhindern stilles Schließen. | MVP | API/Error Contract |
| `MOD-MVP-010` | AddObject wird fachlich unterstützt, auch ohne vorhandenen Screenshot. | MVP | MVP-Workflow |

## Post-MVP-Erweiterungen

| Erweiterung | Beschreibung |
|---|---|
| Rich Attribute/Operation Modals | Attribute und Operationen mit Parametern, Typen und Constraints erfassen. |
| Live OCL Parse/Typecheck im Modal | OCL-Ausdruck bereits vor Erstellung prüfen. |
| Advanced Association Settings | Navigability, Aggregation, Komposition, Assoziationsklasse. |
| Templates | vordefinierte Klassen, Objekte oder Invarianten anlegen. |
| Wizard für komplexe Links | Schrittweise Objektlink-Erstellung mit Filterung und Vorschlägen. |
| Keyboard Shortcuts | Enter zum Submit, Escape zum Cancel, Command-Palette. |
| Inline Preview | Vorschau, wie Node oder Edge im Diagramm aussehen wird. |
| Batch Creation | mehrere Attribute oder Objekte in einem Dialog anlegen. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Speichern Modals direkt über Backend oder nur lokal in den Project State? | Beeinflusst Ladezustände und Fehlerbehandlung. |
| Soll `AddObjectModal` im MVP gestaltet werden, obwohl kein Screenshot vorliegt? | Für vollständigen MVP-Workflow fachlich nötig. |
| Sind Association-Namen Pflicht oder optional? | Beeinflusst Validierung und Edge Labels. |
| Werden Rollen im Add Association Modal Pflichtfelder? | Wichtig für OCL-Navigation. |
| Werden Multiplicities als Textfelder oder Selects umgesetzt? | Selects reduzieren Fehler, Textfelder sind flexibler. |
| Wird OCL im Add Invariant Modal sofort geprüft? | Beeinflusst Backend-Last und UX. |
| Darf Cancel bei Dirty-Formular ohne Rückfrage schließen? | Beeinflusst Datenverlustschutz. |

## Zusammenfassung

Modale Dialoge unterstützen im MVP fokussierte Erstellungsaktionen für Klassen, Invarianten, UML-Associations und Objektlinks. Sie verwenden lokale UI-Validierung für Pflichtfelder und einfache Formate, verlassen sich für fachliche Semantik aber auf das Backend.

Nach erfolgreichem Submit sollten Modals schließen, das neue Element im Diagramm sichtbar machen und es direkt selektieren. Dadurch entsteht ein flüssiger Ablauf vom Erstellen zum detaillierten Bearbeiten im Properties Panel.
