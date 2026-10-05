# Delete Object Link Modal

## Purpose

`assets/mockups/delete-object-link-modal.html` defines deletion of exactly one
Object Link from the current snapshot. It is opened through `Delete Link` in
`Object Properties -> Associations`.

## Semantic boundary

The normal operation deletes the concrete Object Link only. It does not delete
the modeled Association, participating Objects, Classes, Association Ends, or
OCL constraints. Association- and constraint-level definitions remain part of
the Class model.

Before confirmation, the dialog identifies the Object Link by display name and
modeled Association and shows all end assignments and qualifier values. The
backend also derives these special consequences:

- an attached Association-Class Object shares the link identity and is deleted
  atomically with the link;
- composition lifecycle consequences are listed explicitly and executed only
  as one backend-defined atomic mutation;
- lower multiplicities and Invariants are recalculated after deletion and can
  produce Validation Results;
- a changed link, impact, or snapshot revision produces
  `SNAPSHOT_REVISION_CONFLICT` rather than deleting stale state.

OCL Invariants normally reference the modeled Association or navigation role,
not an individual Object-Link ID. They therefore do not act as deletion
references. Their truth values are recalculated for the resulting snapshot.

## Required contract

- Request: stable Object-Link ID and expected snapshot revision.
- Impact: link display name, Association identity, ordered end assignments,
  qualifier values, Association-Class consequence, composition consequence,
  and affected validation scope.
- Success: new snapshot revision and remaining selection/navigation targets.
- Conflict: structured current impact and current revision.

## States

- normal ready-to-delete confirmation,
- Association-Class coupled identity,
- composition lifecycle consequence,
- deleting and success,
- snapshot revision conflict.

## Acceptance criteria

- [x] The dialog is reached from the selected Object Link.
- [x] Association, objects, roles, and qualifier values use domain names.
- [x] Unaffected model and snapshot elements are explicitly retained.
- [x] Association-Class and composition consequences are not silently hidden.
- [x] Constraint results are recalculated rather than treated as references to
  the concrete link.
- [x] Deletion is revision-safe and atomic.
- [x] No productive frontend or backend code is changed.
