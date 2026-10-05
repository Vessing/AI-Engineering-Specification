# Association-Class Instance in the Object Diagram

## Purpose

`assets/mockups/association-class-instance.html` defines the runtime
representation of an Association Class. The example
`enrollmentAdaSe : Enrollment` is simultaneously the Object Link between
`ada` and `softwareEngineering` and the Object view carrying the owned
Attributes `semester` and `grade`. These are two projections of one identity,
not independently creatable snapshot elements.

## Workflow

1. `Create Object Link` selects an Association that owns an Association Class.
2. The same flow requests complete End Assignments, qualifier values, and
   initial owned Attribute values.
3. The backend creates the shared identity atomically.
4. Selecting link edge or object box selects the same identity everywhere.
5. `Associations` edits Ends and qualifiers; the Object view edits owned values;
   `Operations` exposes executable Association-Class Operations.
6. Deletion removes link and Object view together through
   `40-delete-object-link-modal.md`.

The instance can participate in further Object Links when its Classifier has
additional modeled Associations. Inherited features, Invariants, and Operations
follow normal Classifier semantics.

## Notation

- The Object Link connects participating Objects.
- The underlined `instanceName : AssociationClassName` box contains slots.
- A dashed connector joins that box to the Object Link.
- `same identity` is explanatory help, not UML model data.

## Backend and API requirements

- One stable runtime identity maps both projections.
- Creation, update, and deletion are atomic and revision-safe.
- DTOs provide Association, Association Class, ordered End Assignments,
  qualifiers, owned slots, validation state, and display names.
- Detached Association-Class Objects, duplicate identities, incomplete Ends,
  incompatible objects, and invalid owned values are rejected.
- Findings map to the shared identity and both visual projections.

## Acceptance criteria

- [x] Link and object notation are visible together.
- [x] Shared identity is explicit.
- [x] End Assignments and owned Attributes have separate editing contexts.
- [x] Creation and deletion cannot split the identity.
- [x] Navigation to the modeled Association Class is available.
- [x] Creation, validation, loading, and deletion states are represented.
- [x] No productive frontend or backend code is changed.
