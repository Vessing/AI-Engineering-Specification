# Object Diagram Typed Attribute Values

## Purpose and status

**Status:** `REFERENCE_MOCKUP_COMPLETE`

`assets/mockups/object-diagram-typed-attribute-values.html` extends the
canonical Object Diagram with the complete value-editor example for instance
attributes. It does not introduce a separate workflow or a new workspace.
The same editor set is used for initial values in
`assets/mockups/create-object-modal.html`.

## Slot semantics

- A stored, non-static effective Attribute creates one editable Object slot.
- A static Attribute is owned by the Classifier and creates no Object slot.
  The Object sidebar names this rule but redirects editing to the Class Diagram.
- A derived Attribute is calculated from the current snapshot, carries the UML
  `/` prefix, remains visible and read-only, and is not submitted as an
  authoritative stored value.
- Slots cannot be created independently from model Attributes.
- Inherited stored Attributes create ordinary editable slots on instances of
  the subclass. The editor identifies the defining Class, for example
  `Inherited from Person`, without implying that the value is read-only.

## Type-directed editors

| Attribute type | Object Properties editor |
|---|---|
| `Boolean` | finite `true` / `false` selection |
| `Integer` | integral numeric input |
| `Real` | decimal numeric input |
| `String` | text input |
| Enumeration | literal selection qualified by the Attribute type |
| DataType | structured editor derived from the DataType value properties |
| optional `[0..1]` | explicit `No value (null)` control plus the typed editor |
| stored Collection | ordered or unordered value editor preserving the declared Collection kind; shown only when the active model profile permits stored Collection values |

The backend remains authoritative for type conformance, multiplicity,
Collection kind, ordering, uniqueness, DataType structure and nullability.
The UI must not serialize `null` as an empty String.

The four stored Collection examples are separate model-defined Attribute
types, not a user-selectable mode:

- `Set` is unordered and rejects duplicate values.
- `Bag` is unordered and preserves duplicate occurrences.
- `Sequence` preserves positions and permits duplicates.
- `OrderedSet` preserves positions and rejects duplicates.

Only `Sequence` and `OrderedSet` expose move-earlier/move-later controls.
Changing the Collection kind remains a Class Properties model operation.

## States and behavior

- Invalid drafts remain visible and are reported in `Diagnostics`.
- Saving validates all changed slots atomically against the expected snapshot
  revision.
- Derived values are recalculated after a successful mutation.
- Changing a Classifier-scoped static value may affect derived values on many
  Objects, but that static value is never duplicated into their slots.
- `null` is a valid absence value only where multiplicity permits it and is
  distinct from an empty String or a default primitive value.
- `invalid` represents a failed expression or value evaluation, not absence.
  A failed derived evaluation is read-only in Object Properties and links to
  Diagnostics and the defining Attribute in the Class Diagram.
- Loading retains the selected Object and last committed snapshot while typed
  editors and derived values are refreshed.

## Acceptance criteria

| ID | Criterion |
|---|---|
| `OBJ-VALUE-01` | Static Attributes create no Object slot. |
| `OBJ-VALUE-02` | Derived Attributes are visible with `/`, calculated and read-only. |
| `OBJ-VALUE-03` | Boolean, Integer, Real and String use type-appropriate editors. |
| `OBJ-VALUE-04` | Enumeration Attributes accept only literals of their Enumeration. |
| `OBJ-VALUE-05` | DataType values use a structured editor based on their value properties. |
| `OBJ-VALUE-06` | Optional values distinguish null from an empty or default value. |
| `OBJ-VALUE-07` | Stored Collection editing appears only when allowed and preserves Collection semantics. |
| `OBJ-VALUE-08` | Slot updates are validated and committed atomically. |
| `OBJ-VALUE-09` | Inherited stored slots remain editable and expose their defining Class. |
| `OBJ-VALUE-10` | Valid null, invalid draft, loading and derived-evaluation error states are visually distinct. |
| `OBJ-VALUE-11` | The UI never presents `invalid` as an alternative spelling of `null`. |
| `OBJ-VALUE-12` | Set, Bag, Sequence and OrderedSet editors preserve their distinct order and uniqueness semantics. |
| `OBJ-VALUE-13` | Collection kind is read-only in the Object Diagram and can only be changed in Class Properties. |
