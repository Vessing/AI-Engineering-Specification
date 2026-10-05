# Derived Association End Expression

## Status

`MOCKUP_READY_BACKEND_CONTRACT_MISSING`

## Primaere Referenz

- `assets/mockups/association-end-derived-expression.html`
- fachliche Grundreferenz: `assets/mockups/association-properties.html`
- kanonische Shell: `assets/mockups/workspace-model-explorer.html`

## Zweck

Das Mockup beschreibt ein abgeleitetes Association End, dessen
Ableitungsausdruck aus einer USE-Datei erhalten, angezeigt und bearbeitet wird.
Die UML-Notation verwendet den Slash vor dem Rollennamen.

## Verbindlicher UI-Vertrag

- `Class / Association / Invariant`, End-Karten, End-Optionen und erweiterte
  End-Metadaten folgen `association-properties.html`.
- `Derived` und `Derivation expression` gehoeren zur aufgeklappten End-Karte.
- Rolle, Classifier, Multiplizitaet und Ausdruck bilden einen gemeinsamen Draft.
- Das Ergebnis zeigt den vom Backend bestimmten Navigationstyp nur lesend.
- Strukturierte Diagnostics verweisen auf Association, End, Ausdrucksfeld und
  Source Range.
- Beim Deaktivieren von `Derived` bleibt der Ausdruck bis zum erfolgreichen
  Save im lokalen Draft erhalten.
- Aufloesung, Typpruefung und Auswertung des Ausdrucks erfolgen ausschliesslich
  im Backend.

## Erforderlicher Backendvertrag

Das vorhandene Derived-Flag reicht nicht aus. Benoetigt werden ein
persistierter Ableitungsausdruck, Parser- und Serialisierungsunterstuetzung,
eine Read-Projektion und revisiongeschuetzte End-Commands mit Validation- und
Conflict-Diagnostics. Das Mockup erfindet keine API-Namen.

## Abgrenzung

Das Mockup beschreibt weder Package Imports noch State Machines und ersetzt
keine allgemeine OCL-Editor-Ansicht.
