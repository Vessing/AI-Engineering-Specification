# Verbindliche Redesign-Prinzipien

## Zweck und Herkunft

Diese Datei überführt die Ergebnisse des Product-Owner-Interviews und der
Studierendenbefragung in verbindliche Gestaltungsentscheidungen für alle
Mockup-, Frontend- und UI-Abnahmeschritte. Die Reihenfolge ist priorisiert und
darf nicht als unverbindliche Wunschliste behandelt werden.

## Priorität 1: Einfachheit und leichtere Navigation

Das Redesign muss das Erstellen und Aktualisieren von UML-Diagrammen und
OCL-Regeln vereinfachen. Diese Tätigkeiten bilden den fachlichen Kern der
Anwendung. Anfänger müssen den Hauptworkflow ohne Kenntnis der alten
USE-Desktop-GUI oder der USE-Shell verstehen können.

### Verbindliche Entscheidungen

- Der Hauptworkflow bleibt sichtbar und stabil:
  `Dashboard → Start Project → Class Diagram → Object Diagram → Check Constraints`.
- Class Diagram, Object Diagram und OCL Editor bleiben die drei primären
  Arbeitsmodi. Neue Features erzeugen nicht ohne zwingenden Grund weitere
  Hauptnavigationsebenen.
- Explorer, Canvas/Editor, Properties und Validation Results verwenden ein
  gemeinsames Selection- und Navigationsmodell.
- Häufige Create-, Edit- und Delete-Aktionen sind direkt im jeweiligen Kontext
  erreichbar. Nutzer müssen dafür keine Shell-Kommandos kennen.
- Erweiterte Funktionen werden schrittweise offengelegt. Die häufigsten Felder
  bleiben zuerst sichtbar; seltene Optionen erscheinen in klar benannten
  Abschnitten oder Detailansichten.
- Fehlermeldungen führen direkt zum betroffenen Diagrammelement oder
  OCL-Textbereich und verwenden fachliche Namen statt primär technischer IDs.
- Nach Create-, Apply-, Delete-, Import- und Validation-Aktionen zeigt die
  Oberfläche einen eindeutigen Ergebniszustand.
- Komplexität wird nicht entfernt, wenn sie den fachlichen Mehrwert von USE
  ausmacht. Sie wird strukturiert, erklärt und kontextbezogen zugänglich gemacht.

### Kanonische Layoutreferenzen

1. `assets/mockups/workspace-model-explorer.html` definiert Top-Navigation,
   Class-Diagram-Explorer, Canvas, Erstellungsleiste und
   Properties-Panel-Rahmen.
2. `assets/mockups/workspace-object-explorer.html` definiert dieselbe Shell in
   ihrer objektbezogenen Ausprägung mit Objects, Object Links und Object
   Properties.
3. `assets/mockups/workspace-bottom-panel.html` definiert das gemeinsame
   Bottom Panel mit `Console`, `Diagnostics`, `Validation Results` und
   `Invocation Results`.

Fachmockups variieren Inhalte und aktive Zustände, gestalten diese drei
Grundstrukturen aber nicht neu.

### Abnahmekriterien

| ID | Kriterium |
|---|---|
| `RD-NAV-001` | Jeder Kernworkflow besitzt einen eindeutigen sichtbaren Einstieg und Rückweg. |
| `RD-NAV-002` | Explorer-, Canvas-, Properties- und Fehlerauswahl bleiben synchron. |
| `RD-NAV-003` | Primäre Aktionen sind ohne Shell oder manuelle IDs erreichbar. |
| `RD-NAV-004` | Fortgeschrittene Optionen verdrängen die häufigen Grundaktionen nicht. |
| `RD-NAV-005` | Nach mutierenden Aktionen ist Erfolg, Fehler oder laufende Verarbeitung sichtbar. |

## Priorität 2: Moderne visuelle Oberfläche

Die Anwendung soll zeitgemäß wirken, ohne ihre USE-Identität und ihre
fachliche Dichte zu verlieren. Modernisierung bedeutet nicht, die Anwendung in
eine dekorative Landingpage oder ein vereinfachtes Zeichenprogramm umzubauen.

### Verbindliche Entscheidungen

- Das USE-Logo, die sachliche Modellierungsoberfläche und UML-Notation bleiben
  wiedererkennbare Identitätsträger.
- Die Oberfläche verwendet ein konsistentes System für Abstände, Typografie,
  Controls, Fokus, Selektion, Statusfarben und Panels.
- Diagramm-, Properties- und Ergebnisflächen bleiben arbeitsorientiert und
  kompakt; unnötige Kartenverschachtelung und dekorative Effekte entfallen.
- Buttons verwenden verständliche Icons und Text nur dort, wo die Aktion sonst
  nicht eindeutig wäre.
- Loading-, Empty-, Success-, Warning-, Error- und Disabled-Zustände besitzen
  ein einheitliches Erscheinungsbild.
- Neue Mockups verwenden die M1-Baseline und dürfen nicht unbemerkt in einen
  anderen visuellen Stil wechseln.

### Abnahmekriterien

| ID | Kriterium |
|---|---|
| `RD-VIS-001` | Wiederkehrende Controls und Zustände sehen in allen Hauptviews konsistent aus. |
| `RD-VIS-002` | UML-Notation bleibt normgerecht und gegenüber dekorativen Elementen dominant. |
| `RD-VIS-003` | Keine Ansicht wirkt wie eine übernommene Desktop-GUI oder Shell aus früheren Systemgenerationen. |
| `RD-VIS-004` | USE-Logo und Produktidentität bleiben sichtbar, ohne Arbeitsfläche zu verschwenden. |

## Priorität 3: Accessibility und Hilfe

Accessibility und Anfängerunterstützung sind verpflichtende Produktmerkmale.
Sie dürfen nicht vollständig auf eine spätere Optimierungsphase verschoben
werden. M14 nimmt die abschließende Prüfung vor; jeder frühere Mockup-Schritt
muss die Anforderungen bereits berücksichtigen.

### Verbindliche Entscheidungen

- Text, UML-Symbole, Kantenbeschriftungen und Controls müssen ausreichend groß
  und lesbar sein.
- Text und interaktive Elemente müssen einen ausreichenden Kontrast besitzen.
  Niedrig kontrastierte Shell-Ausgaben werden nicht als Vorbild übernommen.
- Bedeutung darf nie ausschließlich über Farbe vermittelt werden. Symbole,
  Text oder Muster ergänzen Error, Warning, Selection und Visibility.
- Tastaturfokus ist sichtbar. Dialoge, Drawer, Tabs und Formulare besitzen eine
  nachvollziehbare Fokusreihenfolge.
- Icon Buttons erhalten zugängliche Namen und bei Bedarf Tooltips.
- Kontextbezogene Hilfe ist in Class Diagram, Object Diagram, OCL Editor,
  Imports, Properties und Validation Results erreichbar.
- Hilfe erklärt das aktuelle Konzept und die nächste mögliche Aktion. Sie
  überdeckt den Arbeitsbereich nicht dauerhaft und kann geschlossen werden.
- Das Dashboard bietet weiterhin Documentation und Examples als vertiefende
  Einstiege.
- Fehlermeldungen unterscheiden eine kurze nutzerfreundliche Erklärung von
  aufklappbaren technischen Details.

### Hilfemodell

| Ebene | Zweck | Beispiel |
|---|---|---|
| Tooltip | unbekanntes Icon oder UML-Symbol kurz benennen | `# = protected` |
| Inline Help | Feld oder Fehler unmittelbar erklären | erlaubte Multiplizität |
| Quick Help | aktuelle View und häufige Aktion erklären | Klasse erstellen |
| Documentation | vollständiges Konzept erläutern | OCL Iteratoren |
| Examples | ausführbares Lernmodell bereitstellen | University System |

### Abnahmekriterien

| ID | Kriterium |
|---|---|
| `RD-A11Y-001` | Text, Controls, UML-Symbole und Edge Labels bleiben bei Desktop und schmalem Viewport lesbar. |
| `RD-A11Y-002` | Selection, Warning und Error sind nicht nur durch Farbe unterscheidbar. |
| `RD-A11Y-003` | Tastaturfokus und Fokusreihenfolge sind für alle interaktiven Zustände dokumentiert. |
| `RD-A11Y-004` | Kontextbezogene Hilfe ist aus jeder fachlichen Hauptview erreichbar. |
| `RD-A11Y-005` | Nutzerfreundliche Meldung und technische Details sind getrennt darstellbar. |

## Anwendung auf die Mockup-Roadmap

Jeder Mockup-Schritt dokumentiert künftig zusätzlich:

1. den einfachsten erfolgreichen Kernweg,
2. die Stelle für fortgeschrittene Optionen,
3. sichtbares Feedback nach jeder mutierenden Aktion,
4. Lesbarkeit und Kontrast relevanter Texte und Symbole,
5. Tastatur- und Fokusanforderungen,
6. kontextbezogene Hilfe für neue Fachbegriffe,
7. Zuordnung zu `RD-NAV-*`, `RD-VIS-*` und `RD-A11Y-*`.

M14 prüft diese Kriterien abschließend, ersetzt aber nicht ihre Berücksichtigung
in M1 bis M13.

## Abgrenzung

Beginnerfreundlichkeit bedeutet nicht, OCL, Contracts, Mehrfachvererbung oder
erweiterte Associations zu entfernen. Die Anwendung muss einfache Einstiege
und progressive Offenlegung bieten, während fortgeschrittene Funktionen
vollständig erreichbar bleiben.

## Zusammenfassung

Das Redesign priorisiert zuerst Navigation und Einfachheit, danach eine moderne
visuelle Sprache und anschließend Accessibility sowie Hilfe. Alle drei
Prioritäten sind verbindlich. Sie bewahren die fortgeschrittenen USE-Funktionen,
machen sie aber verständlicher, auffindbarer und lesbarer.
