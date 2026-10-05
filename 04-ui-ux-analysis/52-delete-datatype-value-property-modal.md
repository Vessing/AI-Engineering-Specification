# Delete DataType Value Property Modal

## Zweck

`delete-datatype-value-property-modal.html` ist die verbindliche visuelle
Referenz fuer das referenzbewusste Entfernen einer einzelnen Value Property.
Der Owner-DataType bleibt bestehen; persistierte strukturierte Werte werden
nicht automatisch migriert.

## Vertrag und Zustaende

- Geplanter Impact: `GET /api/v1/projects/{projectId}/commands/datatypes/{dataTypeId}/properties/{propertyId}/delete-impact`
- Geplanter Delete: `DELETE /api/v1/projects/{projectId}/commands/datatypes/{dataTypeId}/properties/{propertyId}`
- Der Request verwendet `DeleteCommandRequestDto` mit `expectedRevision`.
- Direkte und verschachtelte statische Werte, Object Slots sowie OCL-
  Propertyzugriffe koennen strukturierte Blocker liefern.
- Verbindliche Zustaende sind Loading, Blocked, Ready, Revision Conflict,
  Not Found und Success.
- Es gibt keine automatische Cascade, Wertmigration oder implizite Entfernung
  ueber einen ungeprueften Full-DataType-Draft.

## Freigabe und Traceability

- Frontend-Zuordnung: `F10N`
- Primaermockup: `assets/mockups/delete-datatype-value-property-modal.html`
- Owner-Workflow: `assets/mockups/datatype-properties.html`
- Backendvoraussetzung: B51 (`IMPLEMENTED` am 1. September 2026)
- Matrix: Eintrag 47 ist backendseitig `SUPPORTED`; die reale F10N-
  Frontendintegration und Desktop-Nachabnahme stehen noch aus.
- Compliance: `CM-UML-006`, `CM-UML-001`, `CM-OCL-017`
