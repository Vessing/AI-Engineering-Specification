# Delete Operation Modal

## Purpose

`assets/mockups/delete-operation-modal.html` specifies deletion of an owned UML
Operation from `Class Properties -> Operations`.

## Workflow and Semantics

1. The dialog identifies the Operation through owner and complete signature.
2. Parameters, preconditions, postconditions and an optional OCL body belong to
   the Operation and are removed with it.
3. OCL expressions that call the Operation and Operations that redefine it are
   external references. They block deletion.
4. Calls and redefinitions are never rewritten automatically. Every blocker
   navigates to its owning Invariant, Definition, Operation or Class context.
5. Without blockers, the dialog becomes a compact confirmation listing the
   owned behavioral content that will be removed.
6. An abstract Operation has no body, but its signature, contracts and
   redefinition relationships follow the same deletion checks.
7. The backend checks stable Operation identity and model revision immediately
   before deletion.

## States

The mockup includes blocked, ready, abstract-operation, deleting, success and
revision-conflict states.

## API and DTO Requirements

- Impact identifies the Operation, its owned contracts/body and external calls
  or redefinitions using stable IDs and domain names.
- Delete uses stable Operation ID and expected model revision.
- Relevant errors include `OPERATION_NOT_FOUND`, `OPERATION_DELETE_BLOCKED` and
  `MODEL_REVISION_CONFLICT`.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `OPERATION-DELETE-01` | Delete Operation identifies the complete selected signature. |
| `OPERATION-DELETE-02` | Parameters, contracts and body are treated as owned content. |
| `OPERATION-DELETE-03` | External calls and redefinitions block deletion. |
| `OPERATION-DELETE-04` | References are never rewritten automatically and provide navigation. |
| `OPERATION-DELETE-05` | Without blockers, a compact confirmation is displayed. |
| `OPERATION-DELETE-06` | Stable identity and model revision are checked before deletion. |

No productive frontend or backend code is changed by this documentation mockup.
