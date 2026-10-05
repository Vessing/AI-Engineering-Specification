# Class Properties Feature Redefinition

## Purpose and status

**Status:** `REFERENCE_MOCKUP_COMPLETE`

This document defines the detailed UML Redefinition rules for inherited
Attributes and Operations. Their visual state is part of
`assets/mockups/class-properties-generalizations-and-redefinitions.html`; it
extends `Class Properties -> Generalizations` and is not an Association
workflow. The complete hierarchy workflow is documented in
`46-unified-generalizations-and-redefinitions.md`.

## Workflow and semantics

1. The backend detects an ambiguous inherited Attribute or Operation and
   reports `INHERITED_FEATURE_AMBIGUOUS`.
2. The user opens `Generalizations`, selects the conflict and chooses
   `Create Local Resolution`.
3. The UI proposes a compatible local feature. If one already exists, the
   workflow instead offers `Use Local Operation to Resolve`.
4. The backend determines the complete set of compatible inherited conflict
   targets. They are shown with owner and full signature as fixed, read-only
   consequences of this conflict, not as optional checkboxes.
5. The user explicitly confirms creation or reuse of the local feature and
   all stable-ID Redefinition relationships. Equal names alone never create
   such relationships.
6. The backend validates the stable feature IDs, inheritance path, feature
   kind, type/signature, multiplicity, visibility and cycle rules.
7. Saving atomically creates or reuses the local feature, stores all target
   relationships and recalculates inherited conflicts, the effective feature
   set and OCL operation/property dispatch.

Inherited features remain owned by and editable through their Superclasses.
Resolving a conflict never deletes or changes them. Deleting the local
resolving feature is a separate destructive workflow and may restore the
ambiguity.

For multiple inheritance, one local RedefinableElement may explicitly target
multiple compatible inherited elements. Incompatible inherited features are
reported through Diagnostics when relevant, rather than mixed into the picker.

## Backend and DTO requirements

- Stable local and inherited feature IDs, not names as references
- `redefinedFeatureIds` for Attributes and Operations
- Candidate owner, inheritance path, complete signature and eligibility reason
- Structured `INHERITED_FEATURE_AMBIGUOUS` and
  `REDEFINITION_SIGNATURE_MISMATCH` diagnostics
- Expected model revision and atomic hierarchy/dispatch recalculation

## Compliance and acceptance criteria

This mockup addresses the frontend requirements of `CM-UML-003` and
`OCL-PROFILE-007`; it does not mark their backend semantics as complete.

| ID | Criterion |
|---|---|
| `REDEFINE-01` | Redefinition is an explicit stable-ID relationship. |
| `REDEFINE-02` | Candidate owner and full signature are visible. |
| `REDEFINE-03` | Every compatible target participating in the detected conflict is shown as a fixed target. |
| `REDEFINE-04` | The user confirms the complete resolution, but cannot silently omit one conflict target. |
| `REDEFINE-05` | Unresolved ambiguity and signature mismatch are distinct diagnostics. |
| `REDEFINE-06` | Save atomically refreshes hierarchy, conflicts and OCL dispatch. |
| `REDEFINE-07` | Create-new and reuse-existing local feature paths are distinct. |
| `REDEFINE-08` | Inherited Superclass features are never deleted by conflict resolution. |
