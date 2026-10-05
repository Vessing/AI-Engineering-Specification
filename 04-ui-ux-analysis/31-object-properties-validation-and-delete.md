# Object Properties Validation and Delete

## Purpose

`assets/mockups/object-properties-validation-and-delete.html` completes the
Object Properties editing workflow with field-level validation, save states and
impact-aware deletion. It extends the unified Object Diagram without changing
Class-model definitions.

## Validation Workflow

1. Object name and stored attribute values are edited as one draft.
2. Frontend validation reports required values and basic input-shape errors
   immediately; backend validation remains authoritative for uniqueness, value
   conformance, semantic constraints and snapshot revision.
3. Errors stay attached to the affected field and are summarized at the top of
   Object Properties and in Diagnostics.
4. Derived values remain visible and read-only. They are recalculated only from
   accepted snapshot state and are never submitted as authoritative values.
5. Save is disabled while known blocking errors remain. During saving, controls
   are disabled without clearing the draft.
6. Success synchronizes Explorer, Canvas, Properties and snapshot revision.
7. A revision conflict preserves the draft but requires current values to be
   reloaded before retrying.

## Delete Workflow

1. `Delete Object` opens a confirmation modal; deletion never occurs directly
   from a toolbar or keyboard action.
2. The backend-derived impact lists every dependent Object Link, composition
   consequence, Association-Class identity and affected validation result.
3. The explicit `Delete Object` action confirms the reviewed impact; no repeated
   object-name input is required.
4. The backend rechecks dependencies and snapshot revision immediately before
   applying the command.
5. Object and explicitly approved dependent elements are deleted atomically.
6. If composition or Association-Class semantics prohibit or expand deletion,
   the modal explains the exact consequence before enabling confirmation.

## API and DTO Requirements

- Update requests carry stable object ID, expected snapshot revision, new object
  name and typed stored attribute values.
- Delete impact is fetched before confirmation and contains display names plus
  stable IDs for affected links and objects.
- Delete requests carry object ID, expected revision and an impact token or
  equivalent concurrency proof so stale confirmations cannot be applied.
- Structured errors include `OBJECT_NAME_CONFLICT`, `ATTRIBUTE_REQUIRED`,
  `ATTRIBUTE_VALUE_TYPE_MISMATCH`, `OBJECT_DELETE_DEPENDENCY_CHANGED`, and
  `SNAPSHOT_REVISION_CONFLICT`.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `OBJECT-EDIT-01` | Name and stored values form one preserved editing draft. |
| `OBJECT-EDIT-02` | Every blocking error identifies its field and stable error code. |
| `OBJECT-EDIT-03` | Derived values remain read-only and are not submitted as stored values. |
| `OBJECT-EDIT-04` | Loading, success and revision-conflict states preserve selection context. |
| `OBJECT-DELETE-01` | Object deletion always requires confirmation. |
| `OBJECT-DELETE-02` | Confirmation lists Object Links, composition and Association-Class consequences. |
| `OBJECT-DELETE-03` | The confirmation uses an explicit destructive action without requiring repeated name input. |
| `OBJECT-DELETE-04` | Dependencies and revision are revalidated before atomic deletion. |
| `OBJECT-DELETE-05` | Long impact lists scroll inside the modal. |

## Assumptions

- Whether dependent links are deleted automatically or block object deletion is
  a domain policy returned by the backend; the UI never guesses silently.
- Undo/redo across persisted snapshot revisions is outside this mockup. The
  confirmation therefore describes deletion as irreversible from the dialog.

No productive frontend or backend code is changed by this documentation mockup.
