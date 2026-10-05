# Mockup File Naming

## Purpose

Mockup filenames describe the stable product surface and state, not the
roadmap step that originally created them. Analysis documents may retain
M-step numbers for traceability, but all visual references use the canonical
filenames below.

## Convention

Use lowercase kebab case with one of these structures:

- `<workspace>-workspace.html` for a complete screen
- `<owner>-properties-<section>.html` for a Properties state
- `create-<element>-modal.html` or `delete-<element>-modal.html` for dialogs
- `workspace-<shared-surface>.html` for shell-wide components

Do not add roadmap numbers, `unified`, `final`, `new` or `reference` to new
filenames. Consolidation status belongs in the analysis document, not in the
filename.

## Renamed files

| Previous name | Canonical name |
|---|---|
| `m1-ui-baseline.html` | `workspace-ui-baseline.html` |
| `m5-qualified-nary-associations.html` | `association-properties-qualified-nary.html` |
| `m6-association-classes-composition.html` | `association-class-properties.html` |
| `m7-object-diagram-invocation.html` | `object-diagram-operation-invocation.html` |
| `m8-operation-contracts-unified.html` | `operation-contracts.html` |
| `m10-enum-datatype-type-picker-unified.html` | `classifier-type-picker.html` |
| `m10-5-datatype-properties.html` | `datatype-properties.html` |
| `m11-ocl-compliance-feature-display.html` | `ocl-compliance-feature-display.html` |
| `object-diagram-associations-unified.html` | `object-properties-associations.html` |
| `class-properties-generalizations-unified.html` | `class-properties-generalizations-and-redefinitions.html` |
| `bottom-panel-reference.html` | `workspace-bottom-panel.html` |
| `explorer-package-navigation.html` | `workspace-model-explorer.html` |

## Consolidated references

References to removed detail files use their surviving consolidated mockup:

| Removed detail file | Current reference |
|---|---|
| `m2-generalization-abstract-classes.html` | `class-properties-generalizations-and-redefinitions.html` |
| `class-properties-feature-redefinition.html` | `class-properties-generalizations-and-redefinitions.html` |
| `object-diagram-derived-association-ends.html` | `object-properties-associations.html` |

Every `assets/mockups/*.html` file must be referenced by at least one analysis
document, and every mockup path in Markdown must resolve to an existing file.

## Canonical mockup index

The primary document owns the visual contract. Additional references may link
the same mockup into a wider workflow, roadmap or traceability table.

| Mockup | Primary analysis document |
|---|---|
| `association-class-instance.html` | `41-association-class-instance.md` |
| `association-class-properties.html` | `13-m6-association-classes-aggregation-composition.md` |
| `association-end-derived-expression.html` | `54-derived-association-end-expression.md` |
| `association-end-multiplicity-ranges.html` | `55-association-end-multiplicity-ranges.md` |
| `association-properties-qualified-nary.html` | `12-m5-qualified-and-nary-associations.md` |
| `association-properties.html` | `21-association-properties.md` |
| `class-properties-attributes.html` | `24-m2-m9-unified-class-attributes.md` |
| `class-properties-definitions.html` | `16-m9-derived-init-body-def.md` |
| `class-properties-details.html` | `23-m2-m7-unified-class-properties.md` |
| `class-properties-generalizations-add-supertype.html` | `08-m2-generalization-and-abstract-classes.md` |
| `class-properties-generalizations-and-redefinitions.html` | `46-unified-generalizations-and-redefinitions.md` |
| `class-properties-generalizations.html` | `08-m2-generalization-and-abstract-classes.md` |
| `class-properties-operation-body.html` | `16-m9-derived-init-body-def.md` |
| `class-properties-operations.html` | `23-m2-m7-unified-class-properties.md` |
| `class-properties-overview.html` | `23-m2-m7-unified-class-properties.md` |
| `class-properties-static-attribute-values.html` | `43-class-properties-static-attribute-values.md` |
| `classifier-type-picker.html` | `17-m10-enum-datatype-type-picker.md` |
| `create-association-modal.html` | `28-create-association-modal.md` |
| `create-binary-association-modal.html` | `28-create-association-modal.md` |
| `create-class-modal.html` | `02-class-diagram-ui.md` |
| `create-invariant-modal.html` | `33-invariant-properties.md` |
| `create-object-link-modal.html` | `29-create-object-link-modal.md` |
| `create-object-modal.html` | `27-create-object-modal.md` |
| `datatype-properties.html` | `20-m10-5-datatype-properties.md` |
| `datatype-properties-operations.html` | `53-datatype-operations.md` |
| `delete-association-modal.html` | `34-delete-association-modal.md` |
| `delete-attribute-modal.html` | `36-delete-attribute-modal.md` |
| `delete-class-modal.html` | `32-delete-class-modal.md` |
| `delete-datatype-modal.html` | `50-delete-datatype-modal.md` |
| `delete-datatype-value-property-modal.html` | `52-delete-datatype-value-property-modal.md` |
| `delete-definition-modal.html` | `38-delete-definition-modal.md` |
| `delete-enumeration-modal.html` | `49-delete-enumeration-modal.md` |
| `delete-enumeration-literal-modal.html` | `51-delete-enumeration-literal-modal.md` |
| `delete-generalization-modal.html` | `39-delete-generalization-modal.md` |
| `delete-invariant-modal.html` | `35-delete-invariant-modal.md` |
| `delete-object-link-modal.html` | `40-delete-object-link-modal.md` |
| `delete-operation-modal.html` | `37-delete-operation-modal.md` |
| `invariant-properties.html` | `33-invariant-properties.md` |
| `object-diagram-operation-invocation.html` | `22-m7-object-diagram-invocation.md` |
| `object-diagram-typed-attribute-values.html` | `42-object-diagram-typed-attribute-values.md` |
| `object-diagram-workspace.html` | `26-unified-object-diagram-workspace.md` |
| `object-link-association-sidebar-ordered-unique.html` | `30-object-link-association-sidebar.md` |
| `object-link-association-sidebar.html` | `30-object-link-association-sidebar.md` |
| `object-properties-associations.html` | `47-unified-object-associations.md` |
| `object-properties-validation-and-delete.html` | `31-object-properties-validation-and-delete.md` |
| `ocl-compliance-feature-display.html` | `18-m11-ocl-compliance-feature-display.md` |
| `operation-contracts.html` | `15-m8-pre-post-before-after.md` |
| `package-properties-definitions.html` | `16-m9-derived-init-body-def.md` |
| `project-imports.html` | `09-m3-visibility-namespaces-imports.md` |
| `workspace-bottom-panel.html` | `25-unified-bottom-panel-states.md` |
| `workspace-collapsible-sidebars.html` | Interaktionsreferenz für ein- und ausklappbare Workspace-Seitenleisten |
| `workspace-model-explorer.html` | `24-unified-explorer-navigation.md` |
| `workspace-object-explorer.html` | `26-unified-object-diagram-workspace.md` |
| `workspace-ui-baseline.html` | `07-m1-ui-baseline.md` |

## Kanonische Workspace-Strukturen

- `workspace-model-explorer.html` ist die verbindliche Referenz für
  Top-Navigation, Class-Diagram-Explorer, Canvas-Erstellungsleiste und den
  Rahmen des Properties Panels.
- `workspace-bottom-panel.html` ist die verbindliche Referenz für `Console`,
  `Diagnostics`, `Validation Results` und `Invocation Results` in allen
  Workspaces.
- Object-Diagram-Mockups behalten ihre objektbezogenen Explorer-, Canvas- und
  Properties-Inhalte. Ihr Bottom Panel verwendet jedoch dieselbe Struktur und
  Darstellung wie die allgemeine Bottom-Panel-Referenz.
- `workspace-object-explorer.html` ist die verbindliche Referenz für den
  objektbezogenen Explorer und das Object-Properties-Grundlayout.
- Modals und reine Zustandskarten übernehmen keine vollständige
  Workspace-Shell; ihre eingebetteten Hintergrund-Workspaces müssen die
  Referenzen nur dort abbilden, wo sie sichtbar und für den Zustand relevant
  sind.
