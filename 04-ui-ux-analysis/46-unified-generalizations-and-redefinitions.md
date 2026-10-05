# Unified Generalizations and Redefinitions

## Purpose and status

**Status:** `OFFICIAL_UNIFIED_MOCKUP`

`assets/mockups/class-properties-generalizations-and-redefinitions.html` is the official
consistency reference for Generalizations, multiple inheritance, inherited
features and explicit feature Redefinition. The earlier Generalization and
Redefinition files remain detailed source references.

## Unified workflow

1. Select a Class and open `Class Properties -> Generalizations`.
2. Review and select a direct Supertype relationship.
3. Inspect fixed Subclass/Superclass endpoints, the inheritance chain and
   inherited read-only Attributes and Operations.
4. Add another Generalization through a modal with a fixed Subclass and
   validated Superclass candidates.
5. If multiple inherited features conflict, start `Create Local Resolution`
   or reuse an existing compatible local feature. The backend fixes the full
   set of inherited conflict targets; the user confirms creation/reuse and
   all stable-ID Redefinition relationships without target checkboxes.
6. Save hierarchy or Redefinition changes atomically; the backend recalculates
   effective features, conflicts, type conformance and OCL dispatch.

Generalization is not an Association and creates no Object Links. Equal feature
names do not imply Redefinition. Existing Generalization endpoints are removed
and recreated rather than edited in place, according to the existing decision.

## Acceptance criteria

| ID | Criterion |
|---|---|
| `GEN-UNIFIED-01` | Multiple direct Supertypes and UML triangle direction are visible. |
| `GEN-UNIFIED-02` | Direct relationships are selectable and show a transitive inheritance chain. |
| `GEN-UNIFIED-03` | Inherited Attributes and Operations remain read-only and identify their owner. |
| `GEN-UNIFIED-04` | Add Generalization fixes the Subclass and validates candidates. |
| `GEN-UNIFIED-05` | Feature conflicts continue into explicit multi-target Redefinition in the same workflow. |
| `GEN-UNIFIED-06` | Name equality never substitutes stable Redefinition references. |
| `GEN-UNIFIED-07` | Loading, empty, error, confirmation and success states are represented. |
| `GEN-UNIFIED-08` | Conflict resolution distinguishes creating a local feature from reusing one. |
| `GEN-UNIFIED-09` | Fixed conflict targets remain owned by their Superclasses and are never deleted by resolution. |

The mockup covers the frontend implications of `CM-UML-002`, `CM-UML-003`
and `OCL-PROFILE-007` without claiming completion of remaining backend gaps.
