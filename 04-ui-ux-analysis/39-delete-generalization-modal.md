# Delete Generalization Modal

## Purpose

This documentation mockup defines deletion of exactly one UML Generalization.
The primary example removes `Student -> Person` while retaining both Classes
and the separate direct Generalization `Student -> Researcher`.

**Mockup:** `assets/mockups/delete-generalization-modal.html`

## Workflow and semantics

1. The user selects a direct supertype in `Class Properties -> Generalizations`.
2. `Remove Generalization` opens the modal for that exact relationship.
3. The backend checks the current model revision and references that require the
   subtype relationship.
4. Blocking elements can be opened in their owning editor. The UI does not
   rewrite or cascade-delete them.
5. Once no blocker remains, a new Delete attempt repeats the checks and removes
   the relationship atomically.

Deleting a Generalization does not delete its Subclass, Superclass, owned
features, Objects, or Object Links. It removes one direct inheritance edge.
Inherited features, type conformance, dispatch, OCL typing, and validation are
then recalculated from the complete remaining hierarchy. With multiple
inheritance, unrelated direct supertypes remain. If another path still reaches
the same supertype, it can remain an indirect supertype.

## States

- **Blocked:** a redefinition or another type-dependent reference requires the
  relationship; Delete is disabled and navigation to the owner is available.
- **Ready:** no reference requires the direct relationship; the destructive
  action is enabled.
- **Deleting:** references and model revision are checked again.
- **Success:** the edge disappears and hierarchy-dependent views are refreshed.
- **Revision conflict:** current blockers are reloaded before another attempt.

The example blocker `Student::displayName()` redefines
`Person::displayName()`. Such a redefinition requires a valid inheritance
context and is therefore not silently converted into an unrelated Operation.

## Backend and API requirements

- Identify a Generalization by a stable ID or an unambiguous normalized
  Subclass/Superclass pair.
- Return structured blockers with stable element IDs, owner names, feature kind,
  reason, and navigation target.
- Delete atomically with optimistic model-revision checking.
- Recalculate transitive hierarchy, inherited features, conformance, dispatch,
  OCL type information, and diagnostics after deletion.
- Return `MODEL_REVISION_CONFLICT` when hierarchy or references changed.

## Frontend requirements

- Open the modal from the selected direct-supertype entry or selected edge.
- Keep both endpoint Classes visible and clearly state that they are retained.
- Disable Delete while blockers exist and navigate to the owning Class,
  Operation, Association, Object Diagram, or OCL editor as identified by the
  structured blocker.
- Refresh Canvas, Explorer, Generalization list, inherited-feature view, and
  diagnostics from the saved response.

## Compliance and assumptions

- `CM-UML-002`: Generalization, subtype hierarchy, cycle-safe updates.
- `CM-UML-003`: multiple direct supertypes and hierarchy-dependent conflicts.
- `CM-OCL-014`: type operations use the recalculated hierarchy.
- `CM-OCL-015`: `allInstances()` and subtype inclusion use the recalculated
  hierarchy.

Assumption: the first implementation removes an existing relationship and then
creates another one when endpoints must change. Direct endpoint editing and
cascade deletion are outside this mockup.

## Acceptance criteria

- [x] The exact Subclass/Superclass relationship is named.
- [x] Both Classes and other direct supertypes are explicitly retained.
- [x] Blocking redefinitions are visible and navigable.
- [x] Delete is disabled while blockers exist.
- [x] Ready, loading, success, alternative-path, and revision-conflict states
  are documented.
- [x] Effective inheritance is recalculated rather than inferred from only the
  deleted edge.
- [x] No productive frontend or backend code is changed.
