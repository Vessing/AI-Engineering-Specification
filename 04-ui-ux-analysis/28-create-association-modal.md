# Create Association Modal

## Purpose

`assets/mockups/create-association-modal.html` defines the creation boundary for
a Class Diagram association. The modal establishes a valid association identity
and its mandatory ends. The existing `Association Properties` panel remains the
single place for complete editing.

`assets/mockups/create-binary-association-modal.html` is the focused default
view for the common binary case. It shows exactly two canvas-provided ends and a
standard UML line preview. Its `Add End` action extends the current draft rather
than starting another workflow; after a third complete end, the preview changes
to the n-ary central-node notation shown in the broader reference. That broader
reference also documents removing and ordering ends.

## Workflow

1. The user invokes `Create Association` from the Class Diagram toolbar,
   Explorer action, context menu, or by drawing a connector between Classes.
2. A drawn connector preselects its two endpoint Classes. Other entry points
   open the same dialog with two empty required ends.
3. The user enters an association name and selects at least two Classes.
4. Role names and multiplicities are editable in the dialog and receive safe
   defaults where they can be derived unambiguously.
5. `Add End` extends the same draft to an n-ary association. It does not open a
   different editor.
6. Additional ends can be reordered or removed. The two mandatory ends remain
   present, while their selected Classes can still be changed.
7. The preview changes from a binary line to the UML n-ary central-node notation
   as soon as a third complete end exists.
8. Successful creation closes the dialog, places the association, selects it in
   Explorer and Canvas, and opens `Association Properties`.

## Scope Boundary

The creation modal does not duplicate the complete Association Properties
editor. Navigation, `ordered`, `unique`, qualifiers, aggregation kind,
`derived`, `union`, `subsets`, `redefines`, and Association Class creation are
deferred until the new association has a stable identity. This keeps creation
short while retaining the complete UML feature set in the side panel.

## Domain and API Requirements

- The create request needs a stable project/model revision and an ordered list
  of ends containing Class identifier, role, and multiplicity.
- The backend remains authoritative for name resolution, multiplicity syntax,
  duplicate roles, minimum arity, model revision conflicts, and UML semantic
  validation.
- Creation is atomic. No incomplete association or partial end list is persisted.
- The response returns the stable association identifier, display name, resolved
  ends, updated model revision, and placement when applicable.
- Structured errors identify the association field or end index and use stable
  codes such as `ASSOCIATION_NAME_CONFLICT`, `ASSOCIATION_END_INVALID`, and
  `MODEL_REVISION_CONFLICT`.

## States

The mockup documents the primary editing state plus an incomplete-end empty
state, end-specific inline validation, loading, success, and discard
confirmation. Canvas-provided Classes carry a visible `From canvas` marker.
Cancel closes an unchanged draft immediately; a changed draft asks for
confirmation before discarding it. Long or n-ary end lists scroll inside the
modal while the Class Diagram remains visible as context.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `CREATE-ASSOC-01` | All Class Diagram creation entries open the same dialog. |
| `CREATE-ASSOC-02` | The draft requires a name and at least two valid Class ends. |
| `CREATE-ASSOC-03` | A drawn connector preselects both endpoint Classes. |
| `CREATE-ASSOC-04` | `Add End` supports n-ary associations in the same dialog. |
| `CREATE-ASSOC-05` | Detailed end semantics remain in Association Properties. |
| `CREATE-ASSOC-06` | Validation preserves the draft and identifies the affected field or end. |
| `CREATE-ASSOC-07` | Successful creation atomically updates revision, placement, Explorer and selection. |
| `CREATE-ASSOC-08` | The modal remains internally scrollable at narrow heights and widths. |
| `CREATE-ASSOC-09` | Added ends can be reordered and removed without losing the complete draft. |
| `CREATE-ASSOC-10` | Canvas-provided endpoints are visibly distinguished from manually selected ends. |
| `CREATE-ASSOC-11` | Empty and invalid ends are reported at the affected end and field. |
| `CREATE-ASSOC-12` | A changed draft requires confirmation before it is discarded. |
| `CREATE-ASSOC-13` | Three or more complete ends produce an n-ary central-node preview. |

## Assumptions and Open Decisions

- Role names and multiplicities are included because they make the initially
  rendered association understandable; they are still editable afterward.
- Whether role names are generated from Class names is a frontend convenience;
  backend validation remains normative.
- Automatic routing and collision-free placement are frontend concerns and do
  not change association semantics.

No productive frontend or backend code is changed by this documentation mockup.
