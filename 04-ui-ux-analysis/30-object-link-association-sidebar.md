# Object Link and Association Selection Sidebar

## Purpose

`assets/mockups/object-link-association-sidebar.html` defines the canonical
right-hand selection workflow using a simple binary Association.
`assets/mockups/object-link-association-sidebar-ordered-unique.html` extends
the same workflow with ordered-position editing and uniqueness validation.
The unified stored-link and derived-navigation presentation is defined by
`assets/mockups/object-properties-associations.html` and documented in
`47-unified-object-associations.md`.
It does not add another model editor. It connects object selection, related-link
navigation, selected-link details and validation results.

## Selection Workflow

1. Selecting an Object and opening `Associations` lists all Object Links in
   which that object occupies an Association End. This related-link list and
   `Create Object Link` are the first content directly below the tab control.
2. `Create Object Link` opens the creation modal from
   `29-create-object-link-modal.md`. The action is not named `Add Association`,
   because it instantiates an existing Class Association instead of modifying
   the Class model.
3. Selecting a list item, Canvas edge, Explorer entry or Validation Result
   establishes the same Object Link selection and immediately exposes editable
   object assignments and qualifier values. There is no additional `Edit`
   activation step.
4. The selected list card carries the user-facing link name. The detail area
   starts directly with the modeled Association and does not repeat a selection
   banner, snapshot revision or internal Link ID.
5. `Open in Class Diagram` navigates to the underlying Association and opens
   `Association Properties`; it does not make Class-model properties editable in
   the Object Diagram.
6. End assignments are shown by role, Classifier and object name. Concrete
   qualifier values appear under their respective end.
7. Assignment rows are controls in the same internally scrollable sidebar as
   soon as the link is selected. `Save Assignments` applies the complete end
   assignment atomically, while `Discard` restores the selected link. Roles,
   multiplicities and qualifier definitions remain read-only Class-model data.
   This mirrors the established inline editing pattern of `Association Properties`.
   The simple canonical mockup contains neither rule. In the dedicated
   Ordered-/Unique-variant, `ordered` and `unique` are read-only End definitions. For an
   ordered multi-valued End, the selected link exposes its position in the
   effective navigation collection and allows moving it earlier or later.
   `unique` has no Object-Diagram toggle: duplicate assignments are rejected
   with a structured finding.
8. `Delete Link` requires confirmation and reports affected Association-Class
   identity, composition lifecycle or validation consequences where applicable.
   The complete dialog is defined in `40-delete-object-link-modal.md` and
   `assets/mockups/delete-object-link-modal.html`.
9. If the selected link instantiates an Association with an Association Class,
   link edge and Association-Class Object box select one shared identity. The
   complete projection is defined in `41-association-class-instance.md`.

## States and Synchronization

- Object selected: the sidebar lists related Object Links grouped by modeled
  Association when the list becomes long.
- Object Link selected: details, Canvas edge, Explorer row and related-link card
  share one selection.
- Empty: an object without links offers the established `Create Object Link`
  entry instead of an empty editor.
- Invalid: the link remains selectable and the same structured finding is
  available in `Validation Results`.
- Loading preserves the current selection while end assignments are refreshed.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `LINK-SIDEBAR-01` | Object, Canvas edge, Explorer and Validation Results converge on one link selection. |
| `LINK-SIDEBAR-02` | An Object's Associations tab lists its related Object Links. |
| `LINK-SIDEBAR-03` | The selected link displays its modeled Association and every end assignment. |
| `LINK-SIDEBAR-04` | Qualifier values appear under the end whose definition owns them. |
| `LINK-SIDEBAR-05` | Class-model semantics are read-only and link to Association Properties. |
| `LINK-SIDEBAR-06` | Empty, invalid, loading and delete-confirmation states preserve selection context. |
| `LINK-SIDEBAR-07` | Long related-link and end lists scroll inside the sidebar. |
| `LINK-SIDEBAR-08` | `Create Object Link` opens the creation modal and is not confused with Class Association creation. |
| `LINK-SIDEBAR-09` | Selecting a link immediately allows its end assignments and qualifier values to be edited and saved in the same sidebar. |
| `LINK-SIDEBAR-10` | Ordered Ends expose a revision-safe link position without changing the modeled `ordered` flag. |
| `LINK-SIDEBAR-11` | Unique Ends reject duplicate effective assignments with an end- and object-specific diagnostic. |

## API and DTO Notes

The selected-link response needs stable link and Association identifiers plus
display names, ordered end assignments, role names, Classifier names, assigned
object identifiers and names, qualifier values, validation status and snapshot
revision. Internal identifiers support commands and synchronization but are not
the primary visible labels.

For an ordered End, the response additionally needs the stable ordering scope,
current position and neighboring link identities. Saving an order submits the
complete affected order or a revision-safe move command. For a unique End, the
backend evaluates uniqueness in the correct navigation and qualifier scope and
returns `OBJECT_LINK_DUPLICATE` with the conflicting links and End identity.

No productive frontend or backend code is changed by this documentation mockup.
