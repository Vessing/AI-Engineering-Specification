# Unified Object Diagram Workspace

## Purpose and status

**Status:** `REFERENCE_MOCKUP_COMPLETE`

`assets/mockups/workspace-object-explorer.html` defines the canonical Object
Diagram shell, Explorer and Object Properties frame. It combines the shared
top navigation and bottom panel with the object-specific `Objects` and
`Object Links` navigation. `assets/mockups/object-diagram-workspace.html`
remains the detailed selected-object and selected-link workflow example and
must visually follow this reference.

## Workspace structure

The Object Diagram keeps the same application header as the Class Diagram and
activates `Object Diagram`. The Explorer contains snapshot elements rather than
packages: `Objects` and `Object Links`. A floating Canvas toolbar provides the
direct actions `Object` and `Object Link`.

`Object` opens the classifier-driven dialog documented in
`27-create-object-modal.md`. The dialog creates and selects one object
atomically. Object Link creation is specified separately in
`29-create-object-link-modal.md`; it assigns existing objects and qualifier
values to the Ends of a modeled Association.

The selection and inspection workflow after creation is documented in
`30-object-link-association-sidebar.md`. Object selection lists related links;
link selection shows the modeled Association, end assignments and qualifier
values in the same Object Properties sidebar.

Field-level update validation, save states and impact-aware Object deletion are
specified in `31-object-properties-validation-and-delete.md`.
The type-directed slot editors and the distinction between static, stored and
derived Attributes are detailed in
`42-object-diagram-typed-attribute-values.md`.
Read-only effective navigation for subsetted, redefined, derived and union
Association Ends is detailed in `45-object-diagram-derived-association-ends.md`.
The combined consistency reference for stored Object Links and calculated
navigation in one Associations tab is `47-unified-object-associations.md`.

Object cards use UML object notation: the complete `objectName : Classifier`
title is underlined and slot rows show current values. Slots come from the
Classifier attributes and cannot be added independently. Derived slots carry a
leading slash and remain read-only. Object links show the underlying
Association name and role names near their ends.

## Properties and selection

`Object Properties` has the stable segments `Object`, `Associations` and
`Operations`. Object selection opens classifier details and an `Attribute
Values` editor. Every stored attribute value can be edited directly in one
form, while derived values remain visible and read-only. The Object Diagram
does not rename attributes or change their types; those model changes remain
in Class Properties. `Open <Classifier> in Class Diagram` uses the selected
Object's Classifier, activates the Class Diagram, selects that Class and opens
`Class Properties · Details`. Link
selection opens the underlying Association and end assignments inside
`Associations`. `Operations` lists only operations executable on the selected
receiver and continues into the invocation workflow defined by M7 and M8.

Explorer, Canvas, Properties and result navigation share one stable object or
link selection. Multiple instances of the same Classifier remain separate by
their domain object name.

## Bottom panel

The screen uses `Console`, `Diagnostics`, `Validation Results` and
`Invocation Results` exactly as defined in
`25-unified-bottom-panel-states.md`. Console is active during ordinary object
editing. `Check Constraints` activates Validation Results. Completing or
blocking an operation activates Invocation Results.

## Required states

- No object selected disables slot editing and invocation.
- Loading retains the previous snapshot and marks pending mutations.
- Invalid slot values remain in the draft and produce structured Diagnostics.
- Constraint violations mark the affected object or link and navigate from
  Validation Results.
- Successful mutations replace the snapshot revision atomically.
- Failed invocations do not show partial object or link changes as committed.

## Acceptance criteria

| ID | Criterion |
|---|---|
| `OBJ-01` | Object Diagram is the active main tab. |
| `OBJ-02` | Explorer separates Objects and Object Links. |
| `OBJ-03` | Object cards use underlined `object : Classifier` UML notation. |
| `OBJ-04` | Slots distinguish editable stored values from calculated derived values. |
| `OBJ-05` | Object links identify Association and role assignments. |
| `OBJ-06` | Properties use Object, Associations and Operations. |
| `OBJ-07` | Selection remains synchronized across all workspace surfaces. |
| `OBJ-08` | The shared four-tab bottom panel is used without semantic duplication. |
| `OBJ-09` | Stored attribute values are edited together in Object Properties; model attributes remain Class Diagram concerns. |
| `OBJ-10` | A selected Object can open its Classifier in Class Diagram Class Properties. |
| `OBJ-11` | Static Classifier Attributes create no Object slot; stored values use type-directed editors and derived values remain read-only. |
