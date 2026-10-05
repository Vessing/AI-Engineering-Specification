# M1 UI-Baseline und Komponentenraster

## Zweck und Status

Dieses Dokument ist das Ergebnis von Mockup-Roadmap-Schritt M1. Es fixiert die
bestehende Oberfläche als visuelle und strukturelle Baseline für M2 bis M14.
M1 fügt keine neue UML- oder OCL-Funktion hinzu.

**Status:** `READY_FOR_REVIEW`  
**Gesamtfreigabe:** noch nicht `BACKEND_READY`, da M2 bis M14 offen sind.

**Überarbeitungsstand:** Die Baseline berücksichtigt verbindlich die drei
Redesign-Prioritäten einfache Navigation, moderne visuelle Gestaltung sowie
Accessibility und gestufte Hilfe.

Das ausführbare, responsive Baseline-Mockup liegt unter
`assets/mockups/workspace-ui-baseline.html`.

## Verwendete Referenzen

### Analyse-Dokumente

- `00-overview/03-documentation-map.md`
- `04-ui-ux-analysis/01-ui-overview.md`
- `04-ui-ux-analysis/02-class-diagram-ui.md`
- `04-ui-ux-analysis/03-object-diagram-ui.md`
- `04-ui-ux-analysis/04-ocl-and-validation-ui.md`
- `04-ui-ux-analysis/05-screenshot-traceability.md`
- `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md`
- `06-frontend-analysis/04-routing-and-layout.md`
- `06-frontend-analysis/11-properties-panel.md`
- `06-frontend-analysis/15-screenshot-implementation-mapping.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`
- `09-ocl-extension-analysis/15-full-ocl-uml-implementation-plan.md`

### Screenshot-Referenzen

| Screenshot | Verwendung in M1 |
|---|---|
| `01-class-diagram-class-properties.png` | primäre Desktop-Baseline und Class Properties |
| `02-class-diagram-association-properties.png` | selektierte Kante und Association Properties |
| `03-class-diagram-invariant-properties.png` | Invariant-Selektion und OCL-Feld |
| `06-object-diagram-object-properties.png` | Object-View mit identischem Shell-Raster |
| `07-object-diagram-validation-error.png` | Error-Markierung und geöffnetes Validation Panel |
| `12-object-diagram-association-properties.png` | Object-Link-Selektion |
| `13-ocl-editor.png` | Vollbreiteneditor ohne Explorer und Properties |
| `15-properties-association.png` | segmentierte Class Properties |
| `16-properties-invariants.png` | lange, scrollbar zu haltende Properties-Inhalte |
| `17-new-class.png` | neu erstellte und selektierte Klasse |

## Verbindliche Workspace-Struktur

```text
Workspace Shell
├── Top Bar
│   ├── Logo mit Navigation zum Dashboard
│   ├── fachlicher Projektname
│   ├── Class Diagram | Object Diagram | OCL Editor
│   ├── kontextbezogener Hilfeeinstieg
│   └── Refresh | Check Constraints
├── View Area
│   ├── Explorer Sidebar (nicht im OCL Editor)
│   ├── Diagram Canvas oder OCL Editor
│   └── Properties Panel (nicht im OCL Editor)
├── Bottom Panel
│   ├── Console
│   └── Validation Results
└── Help Layer
    ├── Tooltip und Inline-Hilfe
    ├── Quick Help der aktuellen View
    └── Links zu Documentation und Examples
```

## Anfängerfreundlicher Kernworkflow

Die Navigation stellt den Weg `Dashboard -> Start Project -> Class Diagram ->
Object Diagram -> Check Constraints` als primäre Aufgabenfolge dar. Der aktuelle
Schritt und die nächste sinnvolle Aktion müssen erkennbar sein. OCL Editor,
Compliance-Ansichten, State Machines und Operation Traces bleiben erreichbar,
werden aber progressiv und kontextbezogen offengelegt. Dadurch wird keine
Funktion entfernt; selten benötigte Funktionen konkurrieren lediglich nicht mit
den Kernaufgaben von Studierenden.

Desktop verwendet für Diagramm-Views drei Spalten: `220–240 px`, flexible
Mitte und `280–320 px`. Der OCL Editor verwendet die gesamte View-Breite. Das
Bottom Panel bleibt unter der View Area und überdeckt den Canvas nicht.

## Komponentenraster

| Region | Bestehende React-Komponente | Layoutregel | Scrollregel |
|---|---|---|---|
| Workspace | `WorkspaceLayout` | volle Viewporthöhe, Top/View/Bottom | kein globales horizontales Scrollen |
| Top Bar | `TopBar`, `NavigationTabs` | feste intrinsische Höhe | Tabs bei schmaler Breite horizontal erreichbar |
| Explorer | `ExplorerSidebar` | linke feste Spalte | eigener vertikaler Scrollbereich |
| Hauptbereich | `MainWorkspaceView` | `minmax(0, 1fr)` | Canvas/Editor verwaltet eigenen Overflow |
| Diagramm | `DiagramCanvasBase` | verfügbare Mitte vollständig nutzen | Pan/Zoom statt Seitenlayout zu verschieben |
| Properties | `PropertiesPanel` | rechte feste Spalte | `.properties-panel-scroll` scrollt intern |
| Bottom Panel | `BottomPanel` | eigene Zeile | Console und Validation Results scrollen intern |
| Modal Layer | `ModalLayer` | Overlay über Workspace | Modal Body scrollt, Header/Footer bleiben sichtbar |

## Ansichten und Zustände

| Zustand | Baseline-Darstellung | Spätere Mockup-Pflicht |
|---|---|---|
| Default | keine Auswahl; neutraler Canvas und erklärter Empty State | M2–M13 dürfen den Default nicht mit Demo-Selektion verwechseln |
| Selected | blaue Node-/Edge-Kontur; passendes Properties-Segment | Explorer, Canvas und Properties zeigen dasselbe Element |
| Editing | Formularfelder im Properties Panel oder Modal | Feldwerte dürfen keine Node-Abmessungen unkontrolliert verändern |
| Loading | lokaler Ladehinweis im betroffenen Bereich | Shell und Navigation bleiben stabil |
| Error | Feldfehler, Diagnostic oder Diagramm-Badge | fachlicher Name, Schweregrad und Ziel müssen erkennbar sein |
| Disabled | Aktion bleibt sichtbar, begründet aber nicht ausführbar | keine rein farbliche Erklärung |
| Empty | expliziter leerer Explorer, Canvas oder Ergebniszustand | keine erfundenen Inhalte |
| Confirmation | modaler oder lokaler Erfolgs-/Löschstatus | Erfolg und Abbruch sind unterscheidbar |

## Responsive Baseline

Unterhalb der dreispaltig nutzbaren Breite bleibt der Canvas beziehungsweise
Editor die Hauptfläche. Explorer und Properties werden nicht gleichzeitig
neben den Canvas gequetscht, sondern als getrennte Drawer oder Sheets geöffnet.
Explizit beschriftete Aktionen öffnen `Explorer`, `Properties` und `Hilfe`.
Beim Öffnen wechselt der Tastaturfokus in das Panel. Beim Schließen über eine
sichtbare Aktion oder `Escape` kehrt er zum auslösenden Element zurück.
Die Haupttabs bleiben erreichbar, das Bottom Panel erhält eine begrenzte Höhe
und eigene Scrollbereiche. Die exakte Breakpoint-Zahl bleibt M14 vorbehalten;
M1 legt nur das Verhalten fest.

## Interaktionsregeln

1. Ein Klick auf das Logo führt zum Dashboard.
2. Der fachliche Projektname wird angezeigt; technische IDs bleiben nur interne
   Referenzen oder Tooltips.
3. Auswahl in Explorer, Canvas, Properties und Validation Results verwendet ein
   gemeinsames Selection Model.
4. Verschieben verändert nur Layoutdaten und darf keine fachliche Selektion
   zurücksetzen.
5. `Check Constraints` öffnet beziehungsweise aktualisiert Validation Results.
6. OCL `Apply Changes` besitzt Loading-, Success- und Error-Feedback.
7. Console, Validation Results und Properties scrollen innerhalb ihrer Region.
8. Modals besitzen klar getrennte Bestätigungs- und Abbruchaktionen.
9. Jede Hauptview besitzt einen kontextbezogenen Hilfeeinstieg; Dashboard,
   Documentation und Examples bleiben die vertiefenden Hilfestufen.
10. Fortgeschrittene Optionen werden progressiv offengelegt und verdrängen
    nicht die Kernaktionen Erstellen, Bearbeiten und Prüfen.

## Redesign-Prioritäten

M1 bildet die Kriterien `RD-NAV-*`, `RD-VIS-*` und `RD-A11Y-*` aus
`10-redesign-design-principles.md` als Baseline ab. Insbesondere bleiben die
Navigation stabil, Controls und Zustände visuell konsistent, Kontrast und
Lesbarkeit verpflichtend und Quick Help aus allen Hauptviews erreichbar.

### Messbare visuelle und barrierebezogene Baseline

| Bereich | Verbindliche M1-Regel |
|---|---|
| Fließtext und Formfelder | mindestens `14 px`; keine alleinige Verdichtung über kleinere Schrift |
| Diagrammknoten und Labels | Knotentext mindestens `13 px`, kompakte Metadaten mindestens `12 px` |
| Kontrast | mindestens `4,5:1` für normalen Text und `3:1` für große Schrift sowie relevante UI-Grenzen |
| Bedienflächen | mindestens `40 px` Höhe auf Desktop und `44 px` im schmalen Viewport |
| Tastaturfokus | deutlich sichtbarer Fokusring von mindestens `3 px`; Farbe und Umriss bleiben vor dem Hintergrund unterscheidbar |
| Zustandskommunikation | Selected, Error, Warning und Disabled verwenden zusätzlich Text, Icon, Rahmen oder Erklärung |
| Scrollen | Properties, Quick Help, Console und Validation Results scrollen intern und verdrängen nicht die Hauptnavigation |
| Fachliche Identität | USE-Logo, UML-Notation und OCL-Begriffe bleiben erhalten; Layout, Typografie, Abstände und Controls werden modern vereinheitlicht |

Die logische Fokusreihenfolge lautet Top Bar, Hauptnavigation, Hauptfläche,
geöffnetes Seitenpanel und Bottom Panel. Modals binden den Fokus bis zum
Bestätigen oder Abbrechen. Unbekannte Icon-Aktionen erhalten Tooltip und
zugänglichen Namen; zentrale Aktionen werden nicht ausschließlich als Icon
dargestellt.

### Gestuftes Hilfemodell

1. Ein Tooltip benennt eine einzelne, nicht selbsterklärende Aktion.
2. Inline-Hilfe erklärt Format, Pflichtfeld oder unmittelbar behebbaren Fehler.
3. Quick Help erklärt die aktuelle View und den nächsten sinnvollen Schritt,
   ohne den Workspace zu verlassen.
4. Documentation und Examples vermitteln längere fachliche Inhalte und
   vollständige Modelle.

Diese Ebenen vermeiden dauerhafte Erklärungstexte in der Arbeitsfläche und
stellen gleichzeitig sicher, dass Anfänger Unterstützung auffinden können.

## UML-/OCL- und Compliance-Bezug

M1 verändert keine fachliche Semantik. Es stellt die Präsentationsflächen für
alle IDs aus `14-full-ocl-uml-compliance-matrix.md` bereit:

| Matrixbereich | M1-Fläche |
|---|---|
| `CM-UML-*` | Explorer, Class Diagram, Object Diagram und Properties |
| `CM-OCL-*` | OCL Editor, Diagnostics und Diagramm-Markierungen |
| `CM-LIB-*` | OCL Editor und Validation Results |
| `CM-CTX-*` | Properties, OCL Editor und spätere Invocation-Ansichten |

Es wird bewusst keine einzelne Matrix-ID als fachlich erfüllt markiert. M1 ist
eine UI-Grundlage und kein Compliance-Implementierungsschritt.

## Vorläufige Backend-, API- und DTO-Anforderungen

M1 erzeugt keine neuen Endpunkte oder DTO-Felder. Spätere Verträge müssen aber
folgende bereits sichtbare Grenzen einhalten:

- fachliche Namen und stabile IDs getrennt liefern,
- Selection Targets für Klasse, Association, Invariante, Objekt und Link bieten,
- strukturierte Diagnostics und Validation Results liefern,
- Loading, Success, Warning und Error unterscheidbar machen,
- Layoutdaten getrennt von UML-/OCL-Fachdaten persistieren.

Diese Anforderungen werden erst in den fachlichen Mockup-Schritten präzisiert.

## Vorläufige Frontend-Anforderungen

- Die bestehende React-Komponentenaufteilung bleibt Referenz, nicht zwingend
  unveränderlicher Implementierungsvertrag.
- Die Properties-Hauptnavigation bleibt auf `Class`, `Association` und
  `Invariant` begrenzt. Neue Funktionen werden innerhalb der fachlich passenden
  Seite als Abschnitte, aufklappbare Gruppen oder objektbezogene Untersegmente
  angeordnet und verwenden vorhandene Controls sowie interne Scrollbars.
- Neue Diagrammnotationen müssen mit Pan, Zoom, Selektion und gespeichertem
  Layout funktionieren.
- Neue Ansichten dürfen keine Karten in Karten oder zusätzliche dauerhafte
  Seitenleisten einführen.
- Für M2 bis M14 werden Desktop-, relevante Fehler- und responsive Zustände auf
  diesem Raster aufgebaut.

## Annahmen und offene Entscheidungen

| Typ | Eintrag |
|---|---|
| Annahme | Die aktuelle Workspace-Shell bleibt die Grundlage aller Folge-Mockups. |
| Annahme | OCL Editor bleibt ohne Explorer und Properties Panel. |
| Offen für M14 | exakte Breakpoints und Drawer-Breite |
| In M1 festgelegt, in M14 zu prüfen | Drawer über beschriftete Aktion öffnen, Fokus hineinsetzen, mit sichtbarer Aktion oder `Escape` schließen und Fokus zurückgeben |
| Offen für spätere Schritte | konkrete Felder und Notationen neuer UML-/OCL-Features |
| Offen | finaler Collapse-/Resize-Mechanismus des Bottom Panels |

## Interne Akzeptanzprüfung

- [x] Class Diagram, Object Diagram und OCL Editor sind annotiert.
- [x] Top Bar, Explorer, Canvas, Properties und Bottom Panel sind abgegrenzt.
- [x] Desktop-Raster ist mit stabilen Größenbereichen dokumentiert.
- [x] Schmale Darstellung besitzt ein definiertes Panelverhalten.
- [x] Default-, Selected-, Editing-, Loading-, Error-, Disabled-, Empty- und
  Confirmation-Zustände sind erfasst.
- [x] Scrollverhalten für Properties, Console und Validation Results ist fixiert.
- [x] Bestehende Komponenten wurden geprüft.
- [x] Navigation, Modernisierung, Accessibility und Hilfe sind als
  verbindliche Baseline aufgenommen.
- [x] Der Kernworkflow ist als primäre Aufgabenfolge sichtbar und fortgeschrittene
  Funktionen sind progressiv offengelegt.
- [x] Mindestgrößen, Kontrast, Fokusdarstellung und nicht rein farbliche
  Zustandskommunikation sind überprüfbar definiert.
- [x] Quick Help und responsive Drawer besitzen festgelegte Öffnungs-, Schließ-
  und Fokusregeln.
- [x] Es wurden keine produktiven Frontend- oder Backend-Dateien verändert.
- [ ] Fachliche Freigabe durch den Auftraggeber steht aus.
- [ ] `BACKEND_READY` bleibt bis zum Abschluss der übrigen Mockups offen.

## Abgrenzung

Generalisierung, abstrakte Klassen, neue Association-End-Metadaten,
Operationsausführung, Pre-/Postconditions und weitere fachliche Erweiterungen
gehören ausdrücklich nicht zu M1. Sie werden erst in M2 bis M13 entworfen.
