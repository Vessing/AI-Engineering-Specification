# Unified Object Associations

## Purpose and status

**Status:** `UNIFIED_REFERENCE_MOCKUP`

`assets/mockups/object-properties-associations.html` combines the
canonical Object-Link selection workflow with the runtime presentation of
subsetted, derived and union Association Ends. It is the consistency reference
for `Object Properties -> Associations`; the earlier files remain detailed
references.

## Unified workflow

1. Selecting an Object opens `Associations` and lists its stored Object Links
   together with calculated navigation entries.
2. `Create Object Link` remains the only creation action and instantiates an
   existing modeled Association.
3. Selecting a stored Object Link immediately opens editable End assignments.
4. Selecting a derived navigation opens its read-only End definition and one
   combined `Navigation Result and Sources` list.
5. Every derived result row identifies and links to the stored Object Link that
   contributes the value. Selecting that source returns to the stored-link
   editor in the same Associations tab.
6. Saving or deleting a stored link validates subset constraints and
   recalculates derived navigation atomically.
7. `Open in Class Diagram` opens the corresponding Association or Association
   End definition without making Class-model semantics editable here.
8. A failed stored-link save keeps every valid assignment and marks the exact
   End, Qualifier, ordered position or Association-Class field. Multiplicity
   and Unique conflicts also identify the effective Association End.

Stored Object Links and calculated navigation are distinct: the former have
identity and are mutable; the latter are computed results and cannot be
created, assigned or deleted directly.

The Object Diagram does not expose `subsets` badges. Subset declarations are
edited in Class Diagram Association Properties. Their runtime consequences
remain visible through calculated values, source links and structured
validation findings.

## Acceptance criteria

| ID | Criterion |
|---|---|
| `OBJ-ASSOC-01` | One stable Associations tab contains related stored links and derived navigation. |
| `OBJ-ASSOC-02` | Stored-link selection immediately exposes editable End assignments. |
| `OBJ-ASSOC-03` | Derived navigation is visibly read-only and has no synthetic Object-Link identity. |
| `OBJ-ASSOC-04` | Every calculated value exposes its contributing End and stored Object Link in the same row. |
| `OBJ-ASSOC-05` | Source selection returns to the editable stored-link state without changing tabs. |
| `OBJ-ASSOC-06` | Stored-link mutations, subset validation and derived recomputation are atomic. |
| `OBJ-ASSOC-07` | Class-model Association definitions remain read-only and navigable to the Class Diagram. |
| `OBJ-ASSOC-08` | Multiplicity, Qualifier, Ordered/Unique, n-ary completeness and Association-Class errors are attached to exact editable fields. |
| `OBJ-ASSOC-09` | A failed save is atomic and preserves the complete Object-Link draft without changing the snapshot revision. |

## Backend and DTO requirements

- Stable Object, Object-Link, Association and Association-End identifiers
- One related-association projection distinguishing `STORED_LINK` from
  `DERIVED_NAVIGATION`
- End assignments, role names, Classifiers, qualifier/order metadata and
  snapshot revision for stored links
- Effective values with contributing End and stored-link provenance for
  derived navigation
- Structured subset/redefinition references and validation findings
- Structured field diagnostics with `associationEndId`, optional `qualifierId`,
  Association-Class feature reference, error code and rejected draft value

The concrete error-code names shown in the mockup are provisional API contract
names until B34 has reconciled them with the backend error catalog.

No productive frontend or backend code is changed by this mockup.
