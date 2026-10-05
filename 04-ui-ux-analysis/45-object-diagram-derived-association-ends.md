# Object Diagram Derived Association Ends

## Purpose and status

**Status:** `REFERENCE_MOCKUP_COMPLETE`

This document defines the runtime rules for `subsets`, `redefines`, `derived`
and `union` Association Ends. Their visual state is integrated with normal
stored Object-Link selection in
`assets/mockups/object-properties-associations.html` and documented as one
workflow in `47-unified-object-associations.md`. The Class-model editor remains
`assets/mockups/association-properties.html`.

The Object Diagram intentionally omits visible `subsets` badges. The Class
Diagram owns that model declaration; this runtime view exposes only its
calculated result, editable source links and validation consequences.

## Runtime semantics and workflow

- Stored Object Links remain the only directly mutable link instances.
- A subsetting End contributes only values conforming to its referenced
  subsetted End.
- A derived-union End exposes the duplicate-free effective union of all
  compatible subsetting navigation results.
- The derived result is read-only and has no synthetic persisted Object Link
  identity.
- Effective values and their sources appear in one combined
  `Navigation Result and Sources` list. Every row shows the value,
  contributing End and stored Object Link together, so users can navigate
  directly to the actual mutation source without comparing separate panels.
- A redefining End is selected for compatible specialized receivers by stable
  End identity; equal role names are insufficient.
- Stored-link mutations, subset validation and derived recomputation form one
  atomic snapshot update.

`ordered` and `unique` continue to determine the declared navigation result
kind. A union does not silently erase ordering requirements; the backend must
construct the effective value according to the complete End definition.

## Backend and DTO requirements

- Stable Association, End, Object and Object-Link IDs
- Structured subsetted/redefined End references
- Effective navigation values with provenance to contributing stored links
- Read-only/derived/union metadata and declared Collection kind
- Structured `ASSOCIATION_SUBSET_VIOLATION` diagnostics and navigation targets
- Atomic recomputation with expected snapshot revision

## Compliance and acceptance criteria

This reference addresses the UI implications of `CM-UML-014` and
`OCL-PROFILE-007`. Their backend runtime semantics remain `PARTIAL` until the
object-model and OCL-navigation behavior is implemented and verified.

| ID | Criterion |
|---|---|
| `DERIVED-END-01` | Stored links and derived navigation results are visually distinct. |
| `DERIVED-END-02` | A derived result cannot be assigned or deleted directly. |
| `DERIVED-END-03` | Every effective value exposes its contributing End and stored link. |
| `DERIVED-END-04` | Union and subset semantics preserve declared type, order and uniqueness. |
| `DERIVED-END-05` | Redefinition uses stable End IDs and receiver conformance. |
| `DERIVED-END-06` | Mutations, validation and recomputation are atomic. |
| `DERIVED-END-07` | Result values and contributing subset sources are combined row by row. |
