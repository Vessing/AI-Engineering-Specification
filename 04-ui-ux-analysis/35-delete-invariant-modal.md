# Delete Invariant Modal

## Purpose

`assets/mockups/delete-invariant-modal.html` specifies the confirmation opened
from `Delete Invariant` in Invariant Properties. The dialog is intentionally
smaller than Class and Association deletion because an Invariant is removed as
one constraint and does not own a cascade of UML model elements.

## Workflow

1. The dialog identifies the Invariant by Context Class, name and expression.
2. It asks for direct confirmation and explains that future `Check Constraints`
   runs no longer evaluate it.
3. No result summary, dependency inventory or cascade selection is displayed.
4. The backend checks stable identity and model revision immediately before
   deletion.
5. On success, the Context Class remains selected and Invariant Properties shows
   the remaining constraints or its empty state.

## States

The mockup includes confirmation, deleting, success, revision-conflict and
already-deleted states.

## API and DTO Requirements

- Delete uses stable Invariant ID and expected model revision.
- Success returns the new model revision and sufficient selection information
  to return to the Context Class.
- Relevant errors are `INVARIANT_NOT_FOUND` and `MODEL_REVISION_CONFLICT`.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `INVARIANT-DELETE-01` | The dialog identifies the exact Context Class, Invariant and expression. |
| `INVARIANT-DELETE-02` | The dialog states that future constraint checks exclude the deleted Invariant. |
| `INVARIANT-DELETE-03` | No result summary or dependency inventory is displayed. |
| `INVARIANT-DELETE-04` | No unnecessary cascade controls are displayed. |
| `INVARIANT-DELETE-05` | Stable identity and model revision are checked before deletion. |
| `INVARIANT-DELETE-06` | Success returns selection to the Context Class and refreshes active validation results. |

No productive frontend or backend code is changed by this documentation mockup.
