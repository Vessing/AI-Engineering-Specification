# Delete DataType Modal

## Zweck

`delete-datatype-modal.html` ist die verbindliche visuelle Referenz fuer das
referenzbewusste Loeschen eines UML DataType. Der Dialog wird aus den DataType
Properties geoeffnet und verwendet die gemeinsame Class-Diagram-Workspace-Shell.

## Fachlicher Vertrag

- Impact: `GET /api/v1/projects/{projectId}/commands/delete-impact/DATATYPE/{dataTypeId}`
- Delete: `DELETE /api/v1/projects/{projectId}/commands/DATATYPE/{dataTypeId}`
- Der Delete-Request enthaelt `expectedRevision`; Referenzen werden nicht vom
  Frontend berechnet.
- Direkte sowie in Tuple- und Collection-Typen verschachtelte Verwendungen
  blockieren die Loeschung.
- Attribute, Operationssignaturen, DataType-Properties, OCL-Ausdruecke und
  persistierte strukturierte Werte koennen Blocker liefern.
- Das Frontend migriert weder Typreferenzen noch DataType-Werte und bietet
  keine erfundene Cascade an.

## Verbindliche Zustaende

1. `Loading`: rekursiver Impact und aktuelle Modellrevision werden geladen.
2. `Blocked`: strukturierte Blocker zeigen fachliche Namen, Referenzart und
   ein nachvollziehbares Navigationsziel.
3. `Ready`: DataType und owned Value Properties werden atomar geloescht.
4. `Revision Conflict`: `STALE_MODEL_REVISION` laesst den Dialog offen und
   laedt den aktuellen Impact neu.
5. `Not Found`: `ELEMENT_NOT_FOUND` fuehrt zu einem autoritativen Reload.
6. `Success`: Explorer, Typkatalog, Canvas und Selection werden neu projiziert.

## Navigation und Accessibility

- `Escape`, Close und Cancel schliessen ohne Mutation.
- Der destruktive Button ist bei Loading, Blockern und Konflikten deaktiviert.
- Blockeraktionen navigieren ueber strukturierte Elementreferenzen zum
  Attribute, verschachtelten Typgebrauch oder Object Slot.
- Fehlercode und fachliche Meldung ergaenzen die Farbdarstellung.
- Der Dialog besitzt einen internen Scrollbereich; Footer und Aktionen bleiben
  erreichbar.

## Abgrenzung

Das Entfernen einer einzelnen Value Property ist Teil des DataType-Edit-Drafts
und nicht dieser Whole-DataType-Loeschung. Das Mockup fuehrt keine neue
Backendsemantik, automatische Wertmigration oder mobile Abnahme ein.

## Traceability

- Frontend-Zuordnung: `F10N`
- Primaermockup: `assets/mockups/delete-datatype-modal.html`
- DataType-Properties: `assets/mockups/datatype-properties.html`
- Backendnachweis: `09-ocl-extension-analysis/64-b50-persisted-structured-value-types.md`
- Compliance: `CM-UML-006`, `CM-UML-001`, `CM-OCL-017`
