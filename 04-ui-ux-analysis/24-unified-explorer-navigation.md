# Unified Explorer Navigation

## Zweck und Status

**Status:** `REFERENCE_MOCKUP_COMPLETE`

Das Referenzmockup `assets/mockups/workspace-model-explorer.html`
vereinheitlicht die kanonische Anwendungsshell des Class Diagram. Top-
Navigation, Explorer, Canvas-Erstellungsleiste und der Rahmen des Properties
Panels sind für Class-Diagram-Mockups verbindlich und dürfen nicht je Mockup
neu gestaltet werden.

Die bestehenden Class-Diagram-Workspace-Mockups binden dafür gemeinsam
`assets/mockups/workspace-model-shell.css` ein. Diese Shell vereinheitlicht
Hauptreiter, Explorer-Rahmen, Suche, ausgewählte Explorer-Einträge,
Properties-Rahmen und segmentierte Properties-Reiter. Fachliche Inhalte und
der jeweils aktive Zustand bleiben in der einzelnen Mockup-Datei definiert.

Object-Diagram-Mockups sind von der inhaltlichen Explorer- und
Properties-Struktur ausgenommen: Sie verwenden weiterhin objektbezogene
Objekte, Object Links, Slots, Associations und Operations. Ihre
Top-Navigation folgt derselben Workspace-Shell; ihr Bottom Panel folgt ohne
Abweichung `workspace-bottom-panel.html`. Die kanonische objektbezogene
Ausprägung ist `assets/mockups/workspace-object-explorer.html`.

Die Anwendungsshell, Haupttabs und lokalen Class Nodes folgen
`class-properties-details.html`. Importierte Class Nodes verwenden die
gestrichelte read-only Darstellung und vollständigen UML-Compartments aus
`project-imports.html`. Das Class Properties Panel übernimmt die gemeinsamen
Haupttabs `Class`, `Association`, `Invariant` und die Class-Untertabs `Details`,
`Attributes`, `Operations`, `Generalizations`, `Definitions`.

Die Haupttabs verwenden den hellen aktiven Zustand aus
`class-properties-operations.html`: Der aktive Tab besitzt eine hellblaue
Fläche und blaue Schrift, während inaktive Tabs transparent bleiben. Das
Bottom Panel besitzt die stabilen Tabs `Console`, `Diagnostics`,
`Validation Results` und `Invocation Results`; seine visuelle und inhaltliche
Referenz ist `workspace-bottom-panel.html`. Im
Class-Details-Hauptzustand ist `Console` aktiv und protokolliert die aktuelle
Class-Auswahl sowie den Status geladener Imports. Strukturierte Ergebniszeilen
zeigen links die Ereignisart, mittig die fachliche Meldung und rechts
Model-/Import-Metadaten. Der Inhaltsbereich ist intern scrollbar.

## Informationsarchitektur

Der Explorer besitzt zwei dauerhaft getrennte Bereiche. `Project model` zeigt
immer zuerst `Project root`. Alle lokalen Packages und alle Classifier ohne
Package sind Kinder dieses stabilen Wurzelknotens. `Imports` zeigt jeden Import
als eigenen, ausdrücklich bezeichneten read-only `Import root` und darunter
dessen ursprüngliche Package-Hierarchie. Diese Wurzeln bleiben auch sichtbar,
wenn Packages vorhanden sind.

Innerhalb dieser Hierarchien erscheinen ausschließlich Packages, Classes,
Enumerations und DataTypes. Associations, Invariants und Definitions werden
nicht als konkurrierende Explorer-Ordner angezeigt. Nach Auswahl einer Klasse
sind sie über `Association`, `Invariant` und den Class-Untertab `Definitions`
erreichbar. Attributes, Operations und Generalizations bleiben ebenfalls
Class-Untertabs.

Ein UML Package ist nicht verpflichtend. Classifier ohne Namespace werden
direkt unter `Project root` gruppiert, während Packages ebenfalls dort
beginnen. `Project root` und `Import root` sind Navigationscontainer und
erzeugen keine zusätzlichen UML Packages im Domänenmodell.

## Auswahl und Aktionen

- Ein Package klappt seine direkten Kinder ein oder aus.
- Class, Enumeration oder DataType synchronisieren Explorer, Canvas und das
  passende Properties Panel.
- Der Importwurzelknoten öffnet allgemeine Importinformationen.
- Importierte Classifier verwenden dieselben Properties, jedoch read-only.
- `+` neben `Project model` erstellt Package, Class, Enumeration oder DataType.
- `+` neben `Imports` öffnet ausschließlich den Importdialog.
- Type-Icons und UML-Stereotype unterscheiden Class, Enumeration und DataType.

Unterhalb der Haupttabs besitzt der Diagramm-Canvas zusätzlich eine kompakte,
schwebende Modellierungsleiste mit `Class`, `Enumeration` und `DataType`.
Die drei Aktionen verwenden unterschiedliche zugängliche Akzentfarben und
ausgeschriebene Typnamen. Im Explorer markieren farbige Punkte den Typ; die
Legende und ausgeschriebenen Typnamen in Suchtreffern stellen sicher, dass
Farbe nicht die einzige Unterscheidung trägt. Diese
häufigen Aktionen sind dadurch ohne geöffnetes Menü erreichbar. Sie verwenden
den aktuell ausgewählten lokalen Package-Kontext.
Wenn kein Package ausgewählt ist, wird das Element direkt unter `Project root`
erstellt. Das Plus bei `Project model` bleibt der kompakte Einstieg für
dieselben Aktionen und zusätzlich `Create Package`.

## Suche

Die Suche filtert lokale und importierte Elemente anhand einfacher Namen,
qualifizierter Namen, Packages und Classifier. Feature-Namen dürfen als Treffer
erscheinen, führen aber zur besitzenden Klasse und öffnen dort den passenden
Properties-Untertab; sie erzeugen keinen dauerhaften Explorer-Ordner.

Treffer zeigen Typ, Name und Herkunftspfad. `Escape` oder die Löschen-Aktion
setzt die Suche zurück und stellt den vorherigen Baumzustand wieder her. Eine
leere Trefferliste verändert weder Auswahl noch Canvas.

## Zustände

| Zustand | Festlegung |
|---|---|
| Default | Package- und Importbäume zeigen ihren gespeicherten Aufklappzustand. |
| Selected | Genau ein Classifier oder Strukturknoten ist primär ausgewählt. |
| Search results | Treffer ersetzen vorübergehend den Baum, ohne dessen Zustand zu verlieren. |
| Empty search | Meldung und Suchhinweis; keine automatische Create-Aktion. |
| Loading | Bestehender Baum bleibt sichtbar und erhält einen textlichen Ladeindikator. |
| Error | Betroffener Import bleibt mit Fehlerstatus und Importdetails sichtbar. |
| Read only | Importwurzel und importierte Properties zeigen den Status ausdrücklich. |

## Anforderungen an Frontend und Verträge

Das Frontend benötigt eine rekursive Tree-Komponente mit stabilen Element-IDs,
lokalem Expand-State, Selection-Synchronisierung und projektweitem Suchindex.
Der Baum erhält strukturierte Parent- und Package-Beziehungen und leitet
Ownership nicht aus visueller Einrückung ab.

Backend beziehungsweise API müssen stabile ID, fachlichen Namen,
qualifizierten Namen, Kind, Package-Pfad, Importprovenienz und read-only Status
liefern. Feature-Suchtreffer benötigen Owner-ID und Ziel-Untertab. Dieses
Dokument fordert noch keine konkrete API-Änderung.

## Akzeptanzkriterien

| ID | Kriterium |
|---|---|
| `EXP-01` | `Project root` ist immer sichtbar und enthält lokale Packages sowie unqualifizierte Classifier. |
| `EXP-02` | Classifier ohne Package erscheinen unter `Project root`, ohne ein UML Package zu erzeugen. |
| `EXP-03` | Jeder Import besitzt einen sichtbaren read-only `Import root` und behält darunter seine Package-Hierarchie. |
| `EXP-04` | Der Baum enthält nur Packages, Classes, Enumerations und DataTypes. |
| `EXP-05` | Association, Invariant und Definition öffnen erst im Class-Kontext. |
| `EXP-06` | Suche unterscheidet Treffer über Typ und Herkunftspfad. |
| `EXP-07` | Leere Suche und Zurücksetzen verändern Auswahl und Canvas nicht. |
| `EXP-08` | Explorer, Canvas und Properties teilen dieselbe Auswahl. |
| `EXP-09` | Class, Enumeration und DataType können über die feste Modellierungsleiste erstellt werden. |
| `EXP-10` | Lokale und importierte Classes sowie Class Properties verwenden die verbindlichen Class- und Import-Mockupmuster. |
| `EXP-11` | Haupttabs und Console entsprechen der visuellen Struktur von Class Properties Operations. |

## Offene Entscheidungen

Die Frontendumsetzung muss noch festlegen, ob große Package-Bäume virtuell
gerendert werden und ob der Aufklappzustand projekt- oder benutzerbezogen
persistiert wird. Beides ändert die fachliche Navigation nicht.
