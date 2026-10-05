# Delete Class Modal

## Purpose

`assets/mockups/delete-class-modal.html` defines dependency-aware Class
deletion from `Class Properties · Details`. The existing Class Diagram analysis
required deletion or explicit blocking but did not provide a dedicated modal.

## Dependency Policy

Deletion distinguishes dependency categories and offers `Delete Class Only` or a
controlled `Delete With Selected Dependencies` plan:

1. **Owned content** such as Attributes, Operations and contextual Definitions
   is deleted with the Class after confirmation.
2. **Cascade-selectable dependencies** such as owned-project Associations,
   Generalizations, contextual Invariants, direct Objects and their Object Links
   can be selected explicitly. They are never selected invisibly.
3. **Remaining references** always block deletion when they are neither resolved
   nor selected for cascade. Model references navigate to the Class Diagram,
   snapshot references navigate to the Object Diagram, and OCL references
   navigate to the OCL Editor. OCL expressions cannot be selected for cascade
   and are never automatically rewritten or deleted.
4. **Read-only or imported dependencies** remain blockers and must be resolved
   through their owning import or project.

Imported or otherwise read-only Classes cannot be deleted through Class
Properties. Their owning import or dependency must be changed instead.

## Workflow

1. `Delete Class` in the action row of `Class Properties · Details` requests an authoritative impact analysis for the current
   model and snapshot revisions.
2. The user chooses `Delete Class Only` or `Delete With Selected Dependencies`.
3. The modal lists owned content, selectable dependencies and blocking OCL or
   read-only references using domain names rather than internal IDs.
4. Cascade mode provides an explicit checkbox for each deletable Invariant,
   Association, Generalization, Object and dependent Object Link.
5. The blocker area contains every unselected or unresolvable reference and
   provides `Open in Class Diagram`, `Open in Object Diagram`, or
   `Open in OCL Editor` according to its owner.
6. Impact updates automatically when cascade selections change and whenever the
   user returns from editing a reference. There is no manual refresh action.
7. Pressing Delete performs a final authoritative impact and revision check. The
   button names the number of currently selected dependencies.
8. The backend applies all selected deletions atomically or none of them.
9. Success updates Explorer, both diagrams, Properties and OCL diagnostics.

## API and DTO Requirements

- Impact results contain the Class ID and display name, expected model and
  snapshot revisions, categorized dependencies, blocking status and navigation
  targets.
- The delete command carries the stable Class ID, expected revision, an impact
  token or equivalent concurrency proof and the explicit stable IDs of every
  cascade-selected dependency.
- Relevant structured errors include `CLASS_HAS_INSTANCES`,
  `CLASS_HAS_EXTERNAL_REFERENCES`, `CLASS_READ_ONLY`,
  `CLASS_DELETE_DEPENDENCY_CHANGED`, and `MODEL_REVISION_CONFLICT`.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `CLASS-DELETE-01` | Class deletion always opens an impact confirmation modal. |
| `CLASS-DELETE-02` | Owned Features are distinguished from external references and blockers. |
| `CLASS-DELETE-03` | Direct instances block class-only deletion but can be explicitly included with their Object Links in a cascade plan. |
| `CLASS-DELETE-04` | Associations, Generalizations and Invariants require explicit cascade selection; OCL references are never silently rewritten. |
| `CLASS-DELETE-05` | Imported or read-only Classes direct the user to dependency management. |
| `CLASS-DELETE-06` | Impact and revision are revalidated before atomic deletion. |
| `CLASS-DELETE-07` | Long dependency lists scroll inside the modal. |
| `CLASS-DELETE-08` | Cascade deletion requires explicit selection of every dependent model or snapshot element. |
| `CLASS-DELETE-09` | Every remaining reference blocks deletion and navigates to its Class Diagram, Object Diagram or OCL Editor owner. |
| `CLASS-DELETE-10` | Impact refreshes automatically and the cascade plan is revalidated and applied atomically when Delete is pressed. |

## Assumptions

- Reclassification of an existing object is available only if the backend
  supports it safely; otherwise the object must be deleted first or selected in
  the explicit cascade plan.
- Future bulk-resolution tooling may reduce the number of navigation steps but
  must preserve explicit semantic decisions.

No productive frontend or backend code is changed by this documentation mockup.
