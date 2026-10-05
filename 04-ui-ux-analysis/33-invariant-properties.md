# Invariant Properties

## Purpose

`assets/mockups/invariant-properties.html` consolidates the Class Diagram
Invariant workflow in the current Class Properties design. It replaces reliance
on screenshots as the only visual reference while retaining their established
Context Class, name and OCL-expression semantics.

## Workflow

1. Selecting a Class and opening `Invariant` lists constraints whose Context
   Class is the selected Class.
   The Class Diagram does not invent a separate Invariant node or box. The
   invariant is selected through Explorer or Invariant Properties. If constraint
   notation is later rendered on the Canvas, it must use an explicitly supported
   UML constraint notation such as braces or a proper UML note.
2. `Create Invariant` is the first action and preselects the current Class as
   context. Its dedicated creation modal is specified in
   `assets/mockups/create-invariant-modal.html`.
3. Selecting an Invariant in Explorer, Canvas, the list, Diagnostics or
   Validation Results establishes the same selection and opens its details.
4. Context Class, Invariant Name and OCL Expression are edited together.
5. Parse and typecheck run against the selected context and return stable error
   codes plus source ranges. A valid invariant must have type Boolean.
6. `Open in OCL Editor` opens the same stable Invariant and source range rather
   than creating a separate text copy.
7. Changing Context Class requires confirmation and re-typechecks the expression
   with the new `self` type.
8. Saving atomically updates the Invariant and model revision. Constraint
   evaluation against Objects remains the responsibility of `Check Constraints`.
9. Deletion requires confirmation and removes the Invariant, not its Context
   Class or Objects. Historical result references become stale or archived
   according to backend policy.
   The dedicated confirmation is specified in
   `assets/mockups/delete-invariant-modal.html` and
   `04-ui-ux-analysis/35-delete-invariant-modal.md`.

## States

The mockup includes default editing, empty, parse/type error, loading, success,
context-change confirmation, delete confirmation and model-revision conflict.
The Properties panel and long expressions remain internally scrollable.

## API and DTO Requirements

- Invariant DTOs provide stable ID, Context Class ID and display name, Invariant
  name, expression, source mapping, compile status and model revision.
- Save validates context existence, context visibility, name uniqueness, OCL
  parsing and Boolean result type atomically.
- Relevant errors include `INVARIANT_NAME_CONFLICT`, `OCL_PARSE_ERROR`,
  `OCL_PROPERTY_NOT_FOUND`, `OCL_INVARIANT_NOT_BOOLEAN`,
  `OCL_CONTEXT_NOT_FOUND`, and `MODEL_REVISION_CONFLICT`.
- Diagnostics and Validation Results navigate through stable Invariant, Class,
  Object and source-range identifiers while presenting domain names.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `INVARIANT-01` | The Invariant segment lists constraints for the selected Context Class. |
| `INVARIANT-02` | Context Class, name and OCL expression are directly editable. |
| `INVARIANT-03` | Parse/type errors identify the stable error code and source range. |
| `INVARIANT-04` | Only a Boolean expression can be saved as an Invariant. |
| `INVARIANT-05` | OCL Editor and Properties share one stable Invariant selection. |
| `INVARIANT-06` | Context changes require confirmation and re-typechecking. |
| `INVARIANT-07` | Deletion removes the Invariant without deleting its Class or Objects. |
| `INVARIANT-08` | Empty, loading, success and revision-conflict states preserve context. |
| `INVARIANT-09` | The Canvas does not display a proprietary Invariant box; any optional Canvas notation must be UML-compliant. |
| `INVARIANT-10` | The creation modal requires a context and Boolean OCL expression, supports an optional constraint name, and separates static checking from object-snapshot evaluation. |

## Assumptions

- Invariants are shown under their Context Class even if they are declared in a
  package namespace; qualified identity remains available through the context.
- Enabling/disabling constraints is not introduced here because it is a tool
  execution preference rather than core OCL Invariant semantics.

No productive frontend or backend code is changed by this documentation mockup.
