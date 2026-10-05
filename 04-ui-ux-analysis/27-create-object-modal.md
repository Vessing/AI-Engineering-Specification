# Create Object Modal

## Purpose and status

**Status:** `REFERENCE_MOCKUP_COMPLETE`

`assets/mockups/create-object-modal.html` defines object creation within the
unified Object Diagram workspace.

## Workflow

The dialog opens from `Object` in the floating Canvas toolbar or the plus
action beside `Objects` in the Explorer. It keeps the Object Diagram visible in
the background and creates exactly one object in the current snapshot.

The user enters a snapshot-unique object name and selects a concrete Classifier.
Abstract Classes are visible but disabled with an explanation. After selecting
the Classifier, the dialog generates initial value editors from its effective
stored attributes, including inherited attributes. Derived attributes are
shown as calculated read-only previews and are not submitted as authoritative
slots.

Inherited stored Attributes appear in the same initial-value list as local
Attributes and remain editable. Their defining Class is shown as secondary
metadata, for example `email · String` and `Inherited from Person`.

The creation dialog uses the same type-directed editors as Object Properties:
Boolean selection, integral and decimal numeric inputs, String input,
Enumeration literal selection, structured DataType fields, an explicit null
control for optional values and a Collection editor when the model profile
permits stored Collection values. Static Attributes create no Object slot.

Creation is atomic. On success, the new object is positioned in free Canvas
space and selected in Explorer, Canvas and Object Properties. On failure, the
dialog remains open and retains all entered values.

## Validation

- Object name is required and unique within the snapshot.
- Classifier must exist and be concrete.
- Required stored attributes need type-conforming values.
- Enumeration values use literal selection; DataTypes use their structured
  value editor.
- Imported Classifiers remain read-only as model definitions but may be
  instantiated when permitted by the snapshot profile.
- Backend diagnostics identify the attribute and retain stable model-element
  references.

## Acceptance criteria

| ID | Criterion |
|---|---|
| `CREATE-OBJ-01` | The dialog opens from both Object Diagram create entries. |
| `CREATE-OBJ-02` | Only concrete Classes can be instantiated. |
| `CREATE-OBJ-03` | Stored attributes receive initial value editors derived from the Classifier. |
| `CREATE-OBJ-04` | Derived values are visible but read-only and not stored authoritatively. |
| `CREATE-OBJ-05` | Validation errors retain the draft and identify the affected field. |
| `CREATE-OBJ-06` | Successful creation atomically updates snapshot revision and selection. |
| `CREATE-OBJ-07` | The modal and its value list remain internally scrollable. |
| `CREATE-OBJ-08` | Creation and later editing use the same type-directed value controls. |
| `CREATE-OBJ-09` | Static Attributes are excluded, while derived values remain calculated and read-only. |
| `CREATE-OBJ-10` | Inherited stored Attributes are editable and identify their defining Class. |
