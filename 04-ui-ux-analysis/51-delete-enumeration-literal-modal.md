# Delete Enumeration Literal Modal

## Zweck

`delete-enumeration-literal-modal.html` ist die verbindliche visuelle Referenz
fuer das referenzbewusste Loeschen eines einzelnen Enumeration-Literals. Die
Enumeration selbst sowie die stabilen Identitaeten der verbleibenden Literale
bleiben erhalten.

## Vertrag und Zustaende

- Impact: `GET /api/v1/projects/{projectId}/commands/delete-impact/ENUMERATION_LITERAL/{literalId}`
- Delete: `DELETE /api/v1/projects/{projectId}/commands/ENUMERATION_LITERAL/{literalId}`
- Der Request enthaelt `expectedRevision` und die `enumerationId`.
- Persistierte Literalwerte und OCL-Quellreferenzen blockieren ohne Cascade.
- Verbindliche Zustaende sind Loading, Blocked, Ready, Revision Conflict,
  Not Found und Success.
- Strukturierte Blocker navigieren zum Object Slot oder OCL-Owner; das
  Frontend sucht oder migriert keine Referenzen selbst.

## Traceability

- Frontend-Zuordnung: `F10N`
- Primaermockup: `assets/mockups/delete-enumeration-literal-modal.html`
- Owner-Workflow: `assets/mockups/classifier-type-picker.html`
- Backendnachweis: `09-ocl-extension-analysis/58-b43-enumeration-lifecycle.md`
- Compliance: `CM-UML-005`, `CM-OCL-017`
