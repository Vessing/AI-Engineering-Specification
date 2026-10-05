# Delete Attribute Modal

## Purpose

`assets/mockups/delete-attribute-modal.html` specifies deletion of an owned UML
Attribute from `Class Properties -> Attributes`.

## Workflow and Semantics

1. The dialog identifies the Attribute by owner, name, type, multiplicity and
   value source.
2. The Attribute's own default, init or derive expression is owned content and
   disappears with the Attribute.
3. OCL expressions in Operations, Invariants, Definitions or other derived
   features that navigate to the Attribute are external references. They block
   deletion and are never rewritten automatically.
4. Every blocker navigates to its owning editor. A new delete attempt loads the
   current impact automatically.
5. Without references, the dialog becomes a compact `Are you sure?`
   confirmation.
6. Deleting a stored Attribute removes corresponding Slots from instances of
   the owning Class. A derived Attribute has no authoritative stored Slots.
7. The backend checks stable Attribute identity and model revision immediately
   before deletion.

## States

The mockup includes blocked, ready, stored-slot impact, deleting, success and
revision-conflict states.

## API and DTO Requirements

- Impact identifies the Attribute and referencing model expressions with stable
  IDs, domain names and source ranges.
- Delete uses stable Attribute ID and expected model revision.
- Relevant errors include `ATTRIBUTE_NOT_FOUND`, `ATTRIBUTE_DELETE_BLOCKED` and
  `MODEL_REVISION_CONFLICT`.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `ATTRIBUTE-DELETE-01` | Delete Attribute opens a confirmation for the selected owned Attribute. |
| `ATTRIBUTE-DELETE-02` | External OCL references block deletion and navigate to their owning editor. |
| `ATTRIBUTE-DELETE-03` | Expressions are never rewritten automatically. |
| `ATTRIBUTE-DELETE-04` | Without blockers, the dialog presents a compact confirmation. |
| `ATTRIBUTE-DELETE-05` | Stored Slot impact is explicit; derived Attributes do not claim stored Slots. |
| `ATTRIBUTE-DELETE-06` | Stable identity and model revision are checked before deletion. |

No productive frontend or backend code is changed by this documentation mockup.
