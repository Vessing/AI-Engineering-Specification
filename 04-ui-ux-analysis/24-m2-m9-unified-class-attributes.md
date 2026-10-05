# M2 bis M9: Einheitliche Class Attributes

Attribute deletion and its reference checks are specified in
`assets/mockups/delete-attribute-modal.html` and
`04-ui-ux-analysis/36-delete-attribute-modal.md`.

## Zweck

Dieses Dokument beschreibt die konsolidierte Attributansicht, welche die
normalen UML-Attributeigenschaften aus M2 bis M7 mit den M9-Value-Sources
`Stored`, `Init` und `Derived` verbindet.

**Verbindliches Mockup:** `assets/mockups/class-properties-attributes.html`

**Ergänzendes Mockup für statische Werte:**
`assets/mockups/class-properties-static-attribute-values.html`

Der Classifier-bezogene Werteditor ist fachlich in
`43-class-properties-static-attribute-values.md` beschrieben. Ein statischer
Wert wird niemals als identischer Slot in alle Objects kopiert.

The Attribute Details form includes `Static attribute` (`isStatic`). A static
Attribute is scoped to the Classifier rather than to each Object and is shown
underlined in UML notation. It must not be exposed as a normal editable Object
Slot. Instance-dependent `self` expressions are invalid when no instance
context is available.

## Gemeinsamer Workflow

Der Benutzer wählt unter `Class > Attributes` genau ein Attribut aus. Name,
Sichtbarkeit, Typ und Multiplizität bleiben für jede Value Source gleich. Die
Auswahl `Stored | Init | Derived` bestimmt anschließend, woher der Attributwert
stammt:

- `Stored` verwendet einen normalen, persistierten und im Object Diagram
  bearbeitbaren Slot. Ein kompatibler Default Value ist optional.
- `Init` wertet einen typisierten OCL-Ausdruck einmal während erfolgreicher
  atomarer Objekterstellung aus. Der erzeugte Slot bleibt danach bearbeitbar.
- `Derived` berechnet den Wert aus dem aktuellen Snapshot. Der Wert wird nicht
  als autoritativer Slot gespeichert, trägt in UML ein führendes `/` und ist im
  Object Diagram readonly.

Ein zusätzlicher `Derived`-Toggle wird nicht verwendet, weil er dem
gegenseitigen Ausschluss der drei Value Sources widersprechen würde.

## Zustände

Der Hauptzustand zeigt ein Derived-Attribut einschließlich Ausdruck,
Ergebnistyp und readonly Objektwert. Separate Karten zeigen zusätzlich die
Value Sources `Stored` und `Init`. Loading, Empty, Error, Success und
Confirmation aus dem M2-bis-M7-Attributworkflow bleiben erhalten. Der Wechsel
von `Stored` zu `Derived` verlangt bei existierenden Slots eine
Auswirkungsbestätigung.

## Akzeptanzkriterien

- [x] Normale UML-Attributfelder und M9-Value-Sources liegen in einem Workflow.
- [x] `Stored`, `Init` und `Derived` schließen sich sichtbar gegenseitig aus.
- [x] Init erzeugt einen später bearbeitbaren Slot.
- [x] `isStatic` is edited explicitly and static Attributes are distinguished
  from per-Object Slots using UML underlining.
- [x] Derived trägt `/`, ist berechnet und im Object Diagram readonly.
- [x] Der separate Derived-Schalter wurde durch die Value-Source-Auswahl ersetzt.
- [x] Produktiver Frontend- und Backend-Code bleibt unverändert.
- [x] Statische Classifier-Werte besitzen eine eigene Class-Properties-Referenz
  und bleiben von Object Slots getrennt.
