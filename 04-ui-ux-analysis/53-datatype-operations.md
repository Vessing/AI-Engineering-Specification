# DataType Operations

## Status

`MOCKUP_READY_BACKEND_CONTRACT_MISSING`

## Primaere Referenz

- `assets/mockups/datatype-properties-operations.html`
- kanonische Shell: `assets/mockups/workspace-model-explorer.html`

## Zweck

Das Mockup beschreibt den Import, die Anzeige und die Bearbeitung von
Operationen eines UML DataType. Die Operationen bleiben Bestandteil des
DataType und werden nicht als Class-Operationen oder freie OCL-Definitionen
umgedeutet.

## Verbindlicher UI-Vertrag

- `DataType Properties` erhaelt neben `Details` und `Value Properties` den
  Reiter `Operations`.
- Eine Operation zeigt fachlichen Namen, geordnete Parameter, Parametertypen,
  Rueckgabetyp und Body-Ausdruck.
- Das Diagramm besitzt ein eigenes Operations-Kompartiment fuer den DataType.
- Empty, Editing, Validation, Success und Revision-Conflict erhalten den
  vollstaendigen lokalen Draft.
- Parser-, Namensaufloesungs-, Typ- und Auswertungssemantik verbleiben im
  Backend.

## Erforderlicher Backendvertrag

Noch nicht vorhanden sind ein persistierbares DataType-Operationsmodell, der
USE-Parserpfad, eine Read-Projektion sowie revisiongeschuetzte Create-, Update-
und Delete-Commands mit strukturierten Feld- und Source-Referenzen. Das Mockup
benennt keine konkreten Endpunkte oder DTO-Felder.

## Abgrenzung

Der Import verarbeitet weiterhin genau einen gelieferten `.use`-Modelltext.
Package Imports und State Machines sind nicht Bestandteil dieses Mockups.

