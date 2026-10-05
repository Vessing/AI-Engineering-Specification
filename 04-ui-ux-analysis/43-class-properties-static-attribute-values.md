# Class Properties Static Attribute Values

## Purpose and status

**Status:** `REFERENCE_MOCKUP_COMPLETE`

`assets/mockups/class-properties-static-attribute-values.html` defines the
Classifier-scoped value editor for a static UML Attribute. It extends the
existing `Class Properties -> Attributes` workflow.

## Semantic boundary

- `isStatic = true` makes the Attribute and its value Classifier-scoped.
- A static Attribute creates no per-Object Slot.
- A stored static value is edited once in Class Properties.
- A derived static value is calculated and read-only.
- Instance-dependent `self` access is invalid in a static context.

The mockup assumes that the product profile supports authoritative stored
Classifier values. This requires an explicit backend domain and DTO contract;
it must not be emulated by copying the value into every Object.

## Acceptance criteria

| ID | Criterion |
|---|---|
| `STATIC-VALUE-01` | The editor appears only for a static Attribute. |
| `STATIC-VALUE-02` | The value is visibly owned by the Classifier. |
| `STATIC-VALUE-03` | Objects do not receive a Slot for the static Attribute. |
| `STATIC-VALUE-04` | Stored static values are type-checked and committed atomically. |
| `STATIC-VALUE-05` | Derived static values are read-only and expression-driven. |
| `STATIC-VALUE-06` | Errors retain the draft and identify the Attribute. |
