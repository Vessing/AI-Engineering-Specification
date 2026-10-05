# Delete Definition Modal

## Purpose

`assets/mockups/delete-definition-modal.html` specifies deletion of OCL Property
and Operation Definitions in Class or Package scope.

## Workflow and Semantics

1. The dialog identifies kind, scope, complete signature and owned expression.
2. A Property Definition owns its name, return type and expression. An Operation
   Definition additionally owns its ordered parameters.
3. The owned expression and parameters disappear with the Definition.
4. Other OCL expressions that use the defined Property or Operation are external
   references and block deletion.
5. Calls are never rewritten automatically. Every blocker navigates to its
   owning Invariant, Definition, Operation Body or Contract.
6. Without references, the dialog becomes a compact confirmation.
7. Deleting a Class Definition keeps the Class. Deleting a Package Definition
   keeps the Package and its other members.
8. The backend checks stable Definition identity and model revision immediately
   before deletion.

## States

The mockup includes blocked, ready, Property Definition, Package Definition,
deleting, success and revision-conflict states.

## API and DTO Requirements

- Impact identifies the Definition and referencing OCL expressions using stable
  IDs, domain names and source ranges.
- Delete uses stable Definition ID and expected model revision.
- Relevant errors include `OCL_DEFINITION_NOT_FOUND`,
  `OCL_DEFINITION_DELETE_BLOCKED` and `MODEL_REVISION_CONFLICT`.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `DEFINITION-DELETE-01` | The dialog identifies Definition kind, scope and signature. |
| `DEFINITION-DELETE-02` | Parameters and expression are treated as owned content. |
| `DEFINITION-DELETE-03` | External OCL uses block deletion and navigate to their owner. |
| `DEFINITION-DELETE-04` | Calls are never rewritten automatically. |
| `DEFINITION-DELETE-05` | Class and Package scopes use the same deletion rules without deleting the scope. |
| `DEFINITION-DELETE-06` | Stable identity and model revision are checked before deletion. |

No productive frontend or backend code is changed by this documentation mockup.
