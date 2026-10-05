# Delete Association Modal

## Purpose

`assets/mockups/delete-association-modal.html` defines the destructive workflow
entered through `Delete Association` in Association Properties. It follows the
same dependency-review pattern as Class and Object deletion without extending
UML semantics.

## Deletion Rules

1. The Association and all of its owned Association Ends, qualifiers and End
   properties form one mandatory deletion unit.
2. Participating Classes and their Objects are references, not owned content,
   and remain in the model and snapshot.
3. If the selected model element is an Association Class, its Association and
   Classifier facets are one UML model element. Its owned Classifier features
   are therefore mandatory owned content, not an optional dependency.
4. Existing Object Links, including Link Objects that instantiate an
   Association Class, may be explicitly selected for atomic cascade deletion.
5. Removing a model-level composition Association does not itself cascade-delete
   the part Objects. It removes the Association and approved links.
6. OCL navigation and external `subsets` or `redefines` references are not
   rewritten automatically. They block deletion and navigate to their owning
   editor until resolved.
7. Imported or read-only Associations cannot be deleted from the local
   Association Properties panel.
8. Impact is recalculated automatically and rechecked with current model and
   snapshot revisions immediately before the atomic delete operation.

## States

The mockup documents blocked, ready, deleting and success states. The primary
delete action stays disabled while any reference blocker remains.

## API and DTO Requirements

- Impact results identify the Association, owned Ends, its optional Association
  Class facet and features, Object Links/Link Objects, external End references and OCL references
  with stable identifiers and domain names.
- The delete request carries the expected model and snapshot revisions plus the
  explicit stable IDs approved for cascade deletion.
- The backend remains authoritative and returns a refreshed impact result when
  references or revisions changed.
- Relevant errors include `ASSOCIATION_NOT_FOUND`,
  `ASSOCIATION_DELETE_BLOCKED`, `READ_ONLY_MODEL_ELEMENT`,
  `MODEL_REVISION_CONFLICT` and `SNAPSHOT_REVISION_CONFLICT`.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `ASSOC-DELETE-01` | Delete Association opens an authoritative dependency-impact dialog. |
| `ASSOC-DELETE-02` | Association Ends and their owned properties are always included with the Association. |
| `ASSOC-DELETE-03` | Participating Classes and Objects are never silently deleted. |
| `ASSOC-DELETE-04` | An Association Class facet is mandatory owned content; Object Links and Link Objects require explicit cascade selection. |
| `ASSOC-DELETE-05` | OCL and external End references block deletion until resolved. |
| `ASSOC-DELETE-06` | Every blocker provides navigation to the owning Class Diagram or OCL Editor context. |
| `ASSOC-DELETE-07` | Impact is refreshed and revisions are checked automatically on delete. |
| `ASSOC-DELETE-08` | Ready, deleting, success and revision-conflict responses preserve stable Association identity. |

No productive frontend or backend code is changed by this documentation mockup.
