# Create Object Link Modal

## Purpose

`assets/mockups/create-object-link-modal.html` defines how a modeled Class
Association is instantiated as an Object Link in the current snapshot. The
dialog assigns concrete objects and qualifier values to existing Association
Ends; it does not define new Association semantics.

## Workflow

1. `Object Link` in the Object Diagram toolbar or the plus action beside
   `Object Links` opens the same dialog.
2. A canvas gesture between objects preselects those objects and filters the
   Association picker to compatible modeled Associations.
3. Selecting an Association creates one assignment row for every Association
   End. Binary and n-ary links therefore use the same end-based structure.
   The n-ary preview uses one central link node connected to every participating
   Object; it never decomposes one n-ary link into binary links.
4. Every end receives one compatible existing object. End definitions remain
   read-only because they belong to the Class model.
5. Qualifier definitions are displayed from their respective Association Ends;
   the user enters only concrete typed qualifier values for this link.
6. If the selected Association owns an Association Class, the same dialog
   displays its initial owned Attribute values and one shared link/object name.
   Associations without an Association Class show only their normal End
   Assignments and applicable Qualifier Values. They do not show an
   Association-Class object box, dashed connector, owned Attribute section, or
   shared object name.
7. Creation validates all ends, qualifier values, Association-Class values,
   multiplicities and snapshot
   revision atomically.
8. On success, the link appears on the Canvas and in Explorer, becomes selected,
   and opens `Object Properties · Associations`.

## Scope Boundary

Association name, end Classifiers, roles, multiplicities, qualifiers,
aggregation and navigation are edited in Class Diagram `Association Properties`.
The Object Link dialog only chooses the Association, assigns objects, supplies
qualifier values, and optionally accepts a generated user-facing link name.

For an Association Class, the backend must preserve the one-to-one identity
between the Object Link and its Association-Class object. The conditional UI
for this shared link/object identity and its owned Attributes is defined in
`41-association-class-instance.md` and
`assets/mockups/association-class-instance.html`.

## Domain and API Requirements

- Requests identify the snapshot revision, Association, ordered end assignments,
  assigned objects, qualifier values, optional link display name and conditional
  Association-Class initial values.
- Compatible object choices are derived from each end Classifier, including
  valid subtype instances.
- The backend validates end completeness, Classifier compatibility, qualifier
  types and order, duplicate links where prohibited, and all multiplicities.
- Creation is atomic and returns the stable link identifier, resolved display
  names, updated snapshot revision and structured field/end errors.
- Relevant error codes include `OBJECT_LINK_END_MISSING`,
  `OBJECT_LINK_END_TYPE_MISMATCH`, `OBJECT_LINK_QUALIFIER_INVALID`,
  `OBJECT_LINK_MULTIPLICITY_EXCEEDED`, `OBJECT_LINK_DUPLICATE_NOT_ALLOWED`,
  `OBJECT_LINK_ORDER_POSITION_CONFLICT`, `ASSOCIATION_CLASS_ATTRIBUTE_INVALID`,
  `ASSOCIATION_CLASS_INSTANCE_CONFLICT`, and `SNAPSHOT_REVISION_CONFLICT`.
- Every field error carries the affected `associationEndId` and, where
  applicable, `qualifierId` or Association-Class Attribute reference. The
  frontend does not infer the field from message text.
- The newly named codes are target-contract identifiers for the mockup. B34
  must reconcile them with the implemented error catalog before frontend work;
  the frontend must not hard-code an unverified code.

## Acceptance Criteria

| ID | Criterion |
|---|---|
| `CREATE-LINK-01` | All Object Diagram creation entries open the same dialog. |
| `CREATE-LINK-02` | Only modeled Associations compatible with the selected objects are offered. |
| `CREATE-LINK-03` | Every Association End receives one compatible object assignment. |
| `CREATE-LINK-04` | Binary and n-ary Object Links use the same end-based editor. |
| `CREATE-LINK-05` | Qualifier definitions are read-only and concrete values are editable. |
| `CREATE-LINK-06` | Errors identify the affected end, object, qualifier or multiplicity. |
| `CREATE-LINK-07` | Creation atomically updates snapshot revision, Canvas, Explorer and selection. |
| `CREATE-LINK-08` | The modal and long end lists remain internally scrollable. |
| `CREATE-LINK-09` | An Association Class adds owned initial values and creates one inseparable link/object identity. |
| `CREATE-LINK-10` | Multiplicity, Qualifier, Ordered/Unique, n-ary completeness and Association-Class errors mark the exact draft field and preserve unaffected assignments. |

## Assumptions and Open Decisions

- The visible link name is suggested from Association and object names and may
  be changed before creation; the backend still assigns a stable identifier.
- If no compatible Association exists, the empty state points back to the Class
  Diagram instead of allowing an invalid ad-hoc link.
- Automatic edge routing is a frontend concern and does not change link semantics.

No productive frontend or backend code is changed by this documentation mockup.
