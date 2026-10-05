# Association End Multiplicity Ranges

## Status

`MOCKUP_READY_BACKEND_CONTRACT_MISSING`

## Primaere Referenz

- `assets/mockups/association-end-multiplicity-ranges.html`
- fachliche Grundreferenz: `assets/mockups/association-properties.html`
- kanonische Shell: `assets/mockups/workspace-model-explorer.html`

## Zweck

Das Mockup beschreibt nicht zusammenhaengende USE-Multiplizitaeten wie
`1..8, 10, 15..*`. Die UI erhaelt alle Range-Member und reduziert sie nicht
auf einen einzelnen unteren und oberen Grenzwert.

## Verbindlicher UI-Vertrag

- `Class / Association / Invariant`, End-Karten, End-Optionen und erweiterte
  End-Metadaten folgen `association-properties.html`.
- Das bekannte Multiplicity-Feld bleibt in der End-Karte sichtbar; die
  Range-Liste ist sein aufgeklappter Editor.
- Jedes Intervall oder Singleton ist eine eigene, tastaturerreichbare Zeile.
- Singleton-Werte werden als gleiche Lower- und Upper-Grenze bearbeitet.
- Add und Remove veraendern nur den Draft.
- Die kanonische Anzeige kommt aus der autoritativen Backendprojektion.
- Overlap-, Reihenfolge- und Grenzwertdiagnosen referenzieren Association End
  und betroffene Range-Indizes.
- Validation und Revision Conflict erhalten die vollstaendige Range-Liste.
- Das Frontend vereinigt, sortiert oder bewertet Intervalle nicht fachlich.

## Erforderlicher Backendvertrag

Das heutige einzelne Lower-/Upper-Paar muss durch ein persistierbares
Range-Listenmodell ergaenzt werden. Erforderlich sind USE-Parsing,
JSON-Roundtrip, Read-Projektion und revisiongeschuetzte End-Commands mit
strukturierten Diagnostics. Konkrete API-Namen werden erst nach der
Backendanalyse festgelegt.

## Abgrenzung

Dieses Mockup betrifft die Multiplizitaet eines Association Ends. Package
Imports, externe Modellverkettung und State Machines sind nicht enthalten.
