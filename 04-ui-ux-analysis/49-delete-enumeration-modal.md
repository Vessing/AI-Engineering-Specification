# Delete Enumeration Modal

## Zweck

`delete-enumeration-modal.html` ist die verbindliche visuelle Referenz fuer
das referenzbewusste Loeschen einer UML Enumeration. Der Dialog wird aus den
Enumeration Properties geoeffnet und verwendet die gemeinsame
Class-Diagram-Workspace-Shell.

## Fachlicher Vertrag

- Impact: `GET /api/v1/projects/{projectId}/commands/delete-impact/ENUMERATION/{enumerationId}`
- Delete: `DELETE /api/v1/projects/{projectId}/commands/ENUMERATION/{enumerationId}`
- Der Delete-Request enthaelt `expectedRevision` und keine vom Frontend
  berechneten Referenzen.
- Eine Enumeration ist nur ohne verbleibende Referenzen loeschbar.
- Attribute, Operationsrueckgaben, Parameter, DataType-Properties,
  Qualifierdefinitionen, Slots, Qualifierwerte und OCL-Ausdruecke koennen
  Blocker liefern.
- Das Frontend migriert weder Typreferenzen noch Literalwerte und bietet fuer
  diesen Vertrag keine erfundene Cascade an.

## Verbindliche Zustaende

1. `Loading`: Impact und aktuelle Modellrevision werden geladen.
2. `Blocked`: strukturierte Blocker zeigen fachliche Namen, Referenzart und
   ein nachvollziehbares Navigationsziel.
3. `Ready`: keine Referenz verbleibt; Enumeration und ihre owned Literale
   werden atomar geloescht.
4. `Revision Conflict`: `STALE_MODEL_REVISION` laesst den Dialog offen und
   laedt den aktuellen Impact neu.
5. `Not Found`: `ELEMENT_NOT_FOUND` beendet keinen fremden Draft und bietet
   einen autoritativen Reload an.
6. `Success`: Explorer, Typkatalog, Canvas und Selection werden aus der neuen
   Modellprojektion aktualisiert.

## Navigation und Accessibility

- Der initiale Fokus liegt auf der Dialogueberschrift; `Escape`, Close und
  Cancel schliessen ohne Mutation.
- Der destruktive Button ist bei Loading, Blockern und Konflikten deaktiviert.
- Blockeraktionen navigieren ueber strukturierte Elementreferenzen zu
  Attribute, Object oder Invariant, nicht ueber sichtbare interne IDs.
- Fehler werden nicht allein durch Farbe vermittelt; Fehlercode und
  fachliche Meldung bleiben sichtbar.
- Der Dialog besitzt einen internen Scrollbereich und verdeckt weder Footer
  noch erreichbare Blockeraktionen.

## Abgrenzung

Das Entfernen eines einzelnen Literals verwendet den getrennten
`ENUMERATION_LITERAL`-Impact-/Delete-Vertrag. DataType Delete erhaelt ein
eigenes Mockup. Dieses Dokument definiert keine neue Backendsemantik und keine
mobile Abnahme.

## Traceability

- Frontend-Zuordnung: `F10N`
- Primaermockup: `assets/mockups/delete-enumeration-modal.html`
- Bestehender Typworkflow: `assets/mockups/classifier-type-picker.html`
- Backendnachweis: `09-ocl-extension-analysis/58-b43-enumeration-lifecycle.md`
- Compliance: `CM-UML-005`, `CM-OCL-017`
