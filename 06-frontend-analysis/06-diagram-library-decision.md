# Diagram Library Decision

## Zweck dieser Datei

Diese Datei bewertet geeignete Diagrammbibliotheken für das neue React/TypeScript-Frontend des webbasierten UML/OCL-Systems.

Ziel ist eine begründete Empfehlung für die Umsetzung von:

- UML-Klassendiagrammen,
- UML-Objektdiagrammen,
- Klassenkarten mit Attributen und Operationen,
- Objektkarten mit Slot-Werten,
- Verbindungslinien mit Labels,
- selektierbaren Nodes und Edges,
- Drag & Drop,
- Layout-Speicherung,
- Fehler-Markierungen,
- Integration mit Explorer Sidebar, Properties Panel und Validation Results.

Die Entscheidung ist für den MVP relevant, weil Class Diagram View und Object Diagram View zentrale Arbeitsflächen sind.

## Anforderungen aus den Screenshots

Die Ziel-Screenshots zeigen keine generischen Graphen, sondern editierbare fachliche Diagramme mit stark angepassten Knoten und UI-Interaktionen.

| Screenshot | Sichtbare Anforderung an Diagrammbibliothek |
|---|---|
| `01-class-diagram-class-properties.png` | Klassenkarten mit Name, Attributen, Operationen; selektierte Klasse; Properties Panel Integration. |
| `02-class-diagram-association-properties.png` | Association Edge mit Label; Edge-Selektion; Association Properties Panel. |
| `03-class-diagram-invariant-properties.png` | Invariantendarstellung im Klassendiagramm; Auswahl von Invarianten. |
| `04-class-diagram-new-class-selected.png` | Neue Klasse erscheint direkt als Node und ist selektiert. |
| `06-object-diagram-object-properties.png` | Objektkarten mit Objektname, Typ und Slot-Werten. |
| `07-object-diagram-validation-error.png` | Fehlerhafte Objektkarte mit rotem Rahmen und Badge; Validation Results koppeln auf Diagrammelement. |
| `10-modal-add-class-association.png` | Nach Modal-Erstellung muss eine neue Association Edge entstehen. |
| `11-modal-add-object-association.png` | Nach Modal-Erstellung muss ein neuer Objektlink entstehen. |
| `12-object-diagram-association-properties.png` | Objektlink Edge ist selektierbar und rechts bearbeitbar. |

Die Screenshots liegen unter `use-web-analysis/assets/screenshots/*.png` und werden in den Analyse-Dateien als `assets/screenshots/*.png` referenziert.

### Abgeleitete Muss-Anforderungen

| Anforderung | Bedeutung |
|---|---|
| Custom Nodes | Klassen- und Objektkarten müssen frei gestaltbar sein. |
| Custom Edges | Associations und Objektlinks brauchen Labels, Marker, Fehlerzustände. |
| Node Drag & Drop | Nutzer positioniert Klassen und Objekte manuell. |
| Edge-Selektion | Properties Panel muss auch Edges bearbeiten können. |
| Node-Selektion | Explorer, Canvas und Properties Panel synchronisieren. |
| Layout-Speicherung | Node-Positionen, Viewport und ggf. Edge-Routing müssen persistierbar sein. |
| Fehler-Markierungen | Nodes/Edges müssen visuell als fehlerhaft markiert werden können. |
| React-Integration | Diagramme müssen sauber in React State, Komponenten und Hooks integrierbar sein. |
| TypeScript-Unterstützung | DTOs, View Models und Diagrammelemente müssen typisiert sein. |
| Edge Labels | Association-Namen müssen mittig auf der Kante sichtbar sein; Rollen und Multiplizitäten müssen an beiden Kantenenden nahe der beteiligten Klasse bzw. des beteiligten Objekts sichtbar sein. |

## Bewertungskriterien

Bewertungsskala:

- `++` sehr gut geeignet,
- `+` geeignet,
- `0` neutral oder mit Aufwand geeignet,
- `-` problematisch,
- `--` stark problematisch.

| Kriterium | Bedeutung für das Projekt |
|---|---|
| Screenshot Umsetzung | Wie gut lassen sich die Zielbilder nachbauen? |
| React-Integration | Passt die Bibliothek natürlich zu React-Komponenten? |
| TypeScript-Unterstützung | Können Nodes, Edges und Events sicher typisiert werden? |
| Custom Nodes | Klassen-/Objektkarten als eigene UI-Komponenten. |
| Custom Edges | Association-/Object-Link-Kanten mit Labels und Markierungen. |
| Drag & Drop | Manuelles Layout im Canvas. |
| Selektion | Nodes und Edges auswählbar und mit Properties Panel koppelbar. |
| Layout-Speicherung | Positionen und Viewport serialisierbar. |
| Edge Labels | Mittel-Label fuer Association-Namen sowie Endlabels fuer Rollen und Multiplizitaeten. |
| Fehler-Markierungen | Rote Rahmen, Badges, Highlight States. |
| Performance | Ausreichend für realistische Modellgrößen. |
| Lernkurve | Umsetzungsrisiko im MVP. |
| Wartbarkeit | Verständliche Integration in Feature-Struktur. |
| Community | Dokumentation, Beispiele, Wartung. |
| Lizenz | Nutzbarkeit ohne kommerzielle Hürden. |
| Erweiterbarkeit | Post-MVP: Auto Layout, Mini Map, Undo/Redo, größere Modelle. |

## Option 1: React Flow

React Flow ist eine React-Bibliothek für node-basierte Editoren und interaktive Diagramme. Die offiziellen Docs beschreiben sie als anpassbare React-Komponente für node-basierte UIs; Custom Nodes, Edges, Edge Labels, TypeScript, Testing, Performance, State Management, Drag & Drop sowie Save/Restore werden in der Dokumentation explizit als Themen geführt. Die Startseite nennt außerdem MIT-Lizenz und eingebaute Interaktionen wie Dragging, Zooming, Panning und Selection.

### Stärken

| Stärke | Relevanz |
|---|---|
| Sehr gute React-Integration | Nodes sind React-Komponenten und passen zu `ClassNode`, `ObjectNode`, `ValidationBadge`. |
| Gute TypeScript-Unterstützung | Node-/Edge-Typen können mit DTO/View Models gekoppelt werden. |
| Custom Nodes | UML-Klassenkarten und Objektkarten können exakt als eigene Komponenten gebaut werden. |
| Custom Edges und Edge Labels | Associations und Objektlinks sind mit Mittel-Label und Endlabels gut darstellbar, wenn eigene Edge-Komponenten genutzt werden. |
| Eingebaute Interaktion | Dragging, Zooming, Panning, Selection und Delete sind bereits vorgesehen. |
| Layoutspeicherung | Nodes/Edges und Viewport können als State serialisiert werden. |
| Gute MVP-Geschwindigkeit | Wenig Grundinfrastruktur selbst zu bauen. |
| Gute UI-Kopplung | Selection State kann Explorer und Properties Panel treiben. |
| Open Source/MIT | Für MVP und spätere Nutzung gut geeignet. |

### Schwächen

| Schwäche | Bedeutung |
|---|---|
| Keine UML-Spezialbibliothek | UML-Notation muss selbst als Custom Nodes/Edges gestaltet werden. |
| Edge Routing begrenzt | Komplexe UML-Routings können Post-MVP Zusatzarbeit brauchen. |
| Große Diagramme brauchen Disziplin | Performance hängt von Rendering, Node-Komplexität und State-Updates ab. |
| Pro-Angebote existieren | Prüfen, ob benötigte Features wirklich im Open-Source-Kern liegen. |

### Bewertung für dieses Projekt

React Flow passt sehr gut zu den Screenshots, weil die Ziel-UI wie ein node-basierter Editor mit stark angepassten Karten, selektierbaren Kanten, Properties Panel und Layout-State aussieht.

Wichtige Praezisierung: React Flow erzeugt keine fertige UML-Notation. Die Bibliothek ist die Canvas- und Interaktionsbasis. Fuer das Projekt duerfen UML-Associations und Object Links nicht als einfache Default-Edges umgesetzt werden. Erforderlich sind eigene Edge-Komponenten:

- `UmlAssociationEdge` fuer Klassendiagramme,
- `ObjectLinkEdge` fuer Objektdiagramme.

Diese Custom Edges muessen mindestens drei Labelbereiche besitzen:

| Labelbereich | Inhalt | Position |
|---|---|---|
| Mittel-Label | Association-Name, z. B. `Borrows` | In der Mitte des Edge-Pfads. |
| Source-Endlabel | Source Role und Source Multiplicity | Nahe am Source-Knoten bzw. Source-Handle. |
| Target-Endlabel | Target Role und Target Multiplicity | Nahe am Target-Knoten bzw. Target-Handle. |

Damit bleiben Association-Enden fachlich lesbar, auch wenn Klassen oder Objekte per Drag & Drop bewegt werden. React Flow berechnet die Kantenkoordinaten nach Positionsaenderungen neu; die Custom Edge muss ihre Labelpositionen daraus ableiten.

Empfohlene Komponenten:

```text
src/features/class-diagram/components/
├─ ClassDiagramCanvas.tsx
├─ ClassNode.tsx
├─ AssociationEdge.tsx
└─ InvariantBadge.tsx

src/features/object-diagram/components/
├─ ObjectDiagramCanvas.tsx
├─ ObjectNode.tsx
├─ ObjectLinkEdge.tsx
├─ ValidationBadge.tsx
└─ InvalidObjectHighlight.tsx
```

## Option 2: Cytoscape.js

Cytoscape.js ist eine voll ausgestattete JavaScript-Graphbibliothek. Die offizielle Dokumentation beschreibt sie als reine JS-Graphbibliothek mit MIT-Lizenz, hoher Optimierung, keinen externen Dependencies und Unterstützung moderner Browser. Sie ist besonders stark für Graphvisualisierung, Layouts, Styling, Selektion und Analyse von Netzwerken.

### Stärken

| Stärke | Relevanz |
|---|---|
| Sehr stark für Graphen | Gute Basis für große Netzwerke und automatische Layouts. |
| Gute Performance | Für größere Graphen tendenziell stark. |
| Mächtige Selektoren und Styling | Graph-Elemente können gezielt gefunden und gestylt werden. |
| Viele Layout-Erweiterungen | Nützlich für spätere Auto-Layout-Funktionen. |
| MIT-Lizenz | Lizenzseitig gut geeignet. |

### Schwächen

| Schwäche | Bedeutung |
|---|---|
| Nicht React-nativ | React-Integration muss über Wrapper oder eigene Adapter sauber gelöst werden. |
| Custom HTML-Karten weniger natürlich | UML-Klassenkarten mit komplexem React-Inhalt sind aufwendiger. |
| Properties Panel Integration indirekter | Event- und State-Brücke muss selbst entworfen werden. |
| Look der Screenshots braucht mehr Custom Work | Cytoscape ist stärker Graph als UI-Editor. |

### Bewertung für dieses Projekt

Cytoscape.js ist technisch solide, aber weniger passend für eine UI mit React-Komponenten als Nodes. Es eignet sich eher, wenn Performance und Graphanalyse im Vordergrund stehen. Für den MVP mit UML-Karten, Modals, Properties Panel und Validation Badges ist React Flow pragmatischer.

## Option 3: JointJS

JointJS ist eine Diagramming-Bibliothek für komplexe visuelle Anwendungen. Die offizielle Website unterscheidet zwischen Community-Version und JointJS+ Professional. Sie nennt eine Community-Version mit Open-Source-Code und essenziellen Features sowie eine Professional-Version mit kommerzieller Lizenz und vielen zusätzlichen UI-Features. Außerdem wird eine React-Integration mit TypeScript-Unterstützung beworben.

### Stärken

| Stärke | Relevanz |
|---|---|
| Diagramming-Fokus | Näher an klassischen Diagrammeditoren als D3/Cytoscape. |
| Reife Bibliothek | Lange am Markt, viele Diagramm-Anwendungsfälle. |
| UI-Editor-Funktionen | Grundsätzlich gut für visuelle Editoren geeignet. |
| React-/TypeScript-Angebot | Relevant für das Ziel-Frontend. |
| Professionelle Features verfügbar | Post-MVP interessant, wenn Budget/Lizenz passt. |

### Schwächen

| Schwäche | Bedeutung |
|---|---|
| Community vs Professional | Risiko, dass benötigte Komfortfunktionen hinter kommerzieller Lizenz liegen. |
| Lizenz-/Budgetklärung nötig | Für ein akademisches oder freies MVP kann das hinderlich sein. |
| React-Integration muss geprüft werden | Neue React-Unterstützung klingt gut, muss aber im Projektkontext evaluiert werden. |
| Möglicherweise schwergewichtiger | Für MVP eventuell mehr Struktur als nötig. |

### Bewertung für dieses Projekt

JointJS ist eine ernsthafte Alternative, besonders wenn ein professioneller Diagrammeditor mit umfangreichen Features gewünscht ist. Für den MVP ist die Community/Professional-Abgrenzung aber ein Risiko. Ohne klare Lizenz- und Featureprüfung sollte JointJS nicht die erste Wahl sein.

## Option 4: D3

D3 ist eine sehr flexible JavaScript-Bibliothek für individuelle Datenvisualisierungen. Die offizielle Website beschreibt sie als Bibliothek für maßgeschneiderte Visualisierung mit hoher Flexibilität und DOM-/SVG-Datenbindung.

### Stärken

| Stärke | Relevanz |
|---|---|
| Maximale visuelle Freiheit | Screenshots könnten sehr individuell nachgebaut werden. |
| SVG-Kontrolle | Feingranulare Kontrolle über Kanten, Labels, Marker und Animationen. |
| Reife Community | D3 ist etabliert und gut dokumentiert. |
| Layoutalgorithmen verfügbar | Force Layouts, Trees und weitere Algorithmen möglich. |

### Schwächen

| Schwäche | Bedeutung |
|---|---|
| Keine Diagramm-Editor-Bibliothek | Drag/Drop, Selektion, Edge-Editing, Viewport, State selbst bauen. |
| React-Integration anspruchsvoll | D3-DOM-Manipulation und React State müssen sauber getrennt werden. |
| Hoher MVP-Aufwand | Viele Grundfunktionen müssten selbst entstehen. |
| Wartungsrisiko | Eigene Interaktionslogik kann schnell komplex werden. |

### Bewertung für dieses Projekt

D3 ist stark für Visualisierung, aber nicht ideal als primäre Diagramm-Editor-Bibliothek für diesen MVP. Es kann später punktuell für Spezialvisualisierungen oder Auto-Layout-Hilfen relevant werden, sollte aber nicht die Basis für Class/Object Diagram Editing sein.

## Option5: Eigenlösung

Eine Eigenlösung könnte auf SVG, Canvas oder HTML/CSS mit Pointer Events aufbauen.

### Stärken

| Stärke | Relevanz |
|---|---|
| Vollständige Kontrolle | Exakte Screenshot-Umsetzung möglich. |
| Keine Bibliotheksabhängigkeit | Keine externen API- oder Lizenzrisiken. |
| Fachlich zugeschnitten | Nur benötigte UML/OCL-Funktionen werden gebaut. |

### Schwächen

| Schwäche | Bedeutung |
|---|---|
| Sehr hoher Aufwand | Viewport, Dragging, Selection, Edges, Labels, Hit Testing, Touch, Zoom selbst bauen. |
| Hohes Fehlerrisiko | Diagramminteraktion ist komplex und fehleranfällig. |
| Schlechte MVP-Eignung | Zeit fließt in Infrastruktur statt Fachworkflow. |
| Wartung teuer | Jede Post-MVP-Funktion muss selbst implementiert werden. |

### Bewertung für dieses Projekt

Eine Eigenlösung ist für den MVP nicht empfehlenswert. Sie wäre nur sinnvoll, wenn sehr spezielle Anforderungen entstehen, die keine Bibliothek sinnvoll abdeckt. Die aktuellen Screenshots verlangen keine derart spezielle Rendering-Engine.

## Andere geeignete Bibliotheken

| Bibliothek | Kurzbewertung |
|---|---|
| `@projectstorm/react-diagrams` | React-orientiert und diagrammfähig, aber weniger Momentum/Ökosystem als React Flow; als Fallback prüfbar. |
| GoJS | Sehr mächtig, aber kommerzielle Lizenz; für MVP ohne Lizenzklärung nicht ideal. |
| Mermaid | Gut für statische Diagramme, nicht für interaktive Modellierung. |
| ElkJS / Dagre | Keine Diagramm-UI, aber nützlich als Layout-Engine zusammen mit React Flow. |

## Vergleichsmatrix

| Kriterium | React Flow | Cytoscape.js | JointJS | D3 | Eigenlösung |
|---|---:|---:|---:|---:|---:|
| Screenshot Umsetzung | ++ | 0 | + | + | ++ |
| React-Integration | ++ | 0 | + | 0 | + |
| TypeScript-Unterstützung | ++ | + | + | + | ++ |
| Custom Nodes | ++ | 0 | + | + | ++ |
| Custom Edges | ++ | + | ++ | + | ++ |
| Drag & Drop | ++ | + | + | 0 | - |
| Node-/Edge-Selektion | ++ | + | ++ | 0 | - |
| Properties Panel Integration | ++ | 0 | + | 0 | + |
| Layout-Speicherung | ++ | + | + | 0 | + |
| Edge Labels | ++ | + | ++ | + | + |
| Fehler-Markierungen | ++ | + | + | + | ++ |
| UML-Klassenkarten | ++ | - | + | 0 | ++ |
| Objektkarten | ++ | - | + | 0 | ++ |
| Performance | + | ++ | + | + | ? |
| Lernkurve | + | 0 | 0 | - | -- |
| Wartbarkeit | ++ | 0 | + | - | -- |
| Community | ++ | + | + | ++ | Nicht relevant |
| Lizenz | ++ | ++ | 0 | ++ | ++ |
| Erweiterbarkeit | ++ | + | + | 0 | - |
| MVP-Eignung | ++ | 0 | + | - | -- |

## Empfehlung

Empfehlung für den MVP: **React Flow**.

React Flow ist für dieses Projekt die beste Ausgangsbasis, weil die Zieloberfläche aus den Screenshots sehr stark einem React-basierten node/edge Editor entspricht:

- Klassen und Objekte sind komplexe Custom Nodes.
- Associations und Objektlinks sind Custom Edges.
- Selektion muss Explorer, Canvas und Properties Panel synchronisieren.
- Drag & Drop und Layout-Speicherung sind MVP-relevant.
- Fehler-Markierungen und Badges müssen direkt an Nodes/Edges hängen.
- React/TypeScript ist die Zieltechnologie.

## Begründung

| Projektanforderung | Warum React Flow passt |
|---|---|
| Klassenkarten | Als `ClassNode` mit React-Komponente sehr gut umsetzbar. |
| Objektkarten | Als `ObjectNode` mit Slots und Fehlerzustand gut umsetzbar. |
| Association Labels | Custom Edge oder Edge Label Renderer. |
| Rollen und Multiplizitaeten an Enden | Eigene `UmlAssociationEdge` mit Source-/Target-Endlabels; keine Default Edge. |
| Objektlink Labels | Custom Edge mit Linkdaten. |
| Properties Panel | Selection Callback setzt `SelectionState`; Panel liest aus Store. |
| Explorer Integration | Explorer und Canvas teilen stabile IDs. |
| Fehler-Markierung | Node-/Edge-Daten enthalten `validationState`; CSS/Komponenten rendern Rahmen und Badge. |
| Layout-Speicherung | Node-Positionen und Viewport können in `LayoutDto` persistiert werden. |
| MVP-Geschwindigkeit | Basisinteraktionen sind vorhanden. |
| Post-MVP | Mini Map, Controls, Auto Layout mit Dagre/ELK, Undo/Redo und Collaboration sind anschlussfähig. |

## Risiken der Empfehlung

| Risiko | Beschreibung | Gegenmaßnahme |
|---|---|---|
| UML-spezifische Notation fehlt | React Flow liefert keine fertige UML-Notation. | Eigene `ClassNode`, `AssociationEdge`, `ObjectNode`, `ObjectLinkEdge` bauen. |
| Edge Routing kann limitiert sein | Komplexe Diagramme können überlappende Kanten erzeugen. | MVP mit einfachen Kanten starten; Post-MVP ELK/Dagre oder Custom Edge Routing prüfen. |
| Default Edges reichen fachlich nicht | Rollen und Multiplizitaeten muessen an den Enden stehen, Association-Name in der Mitte. | Default Edges nur fuer Prototypen verwenden; MVP braucht `UmlAssociationEdge` und `ObjectLinkEdge` als Custom Edges. |
| Node-Komplexität beeinflusst Performance | Viele große React Nodes können teuer werden. | Memoization, kleine Node-Komponenten, Store-Selectoren und Performance-Tests. |
| Zu starke Kopplung an React Flow Types | Wechsel der Library würde schwerer. | Eigene `DiagramNodeViewModel`/`DiagramEdgeViewModel` vor React-Flow-Mapping setzen. |
| Pro Features verlockend | Einige Komfortfeatures könnten in Pro-Beispiele fallen. | MVP nur mit MIT-Kernfeatures planen und Feature-Lizenz vor Nutzung prüfen. |

## Konsequenzen für Komponentenstruktur

Empfohlene Struktur:

```text
src/features/diagram-core/
├─ types.ts
├─ diagramMapping.ts
├─ diagramSelection.ts
└─ reactFlowAdapter.ts

src/features/class-diagram/
├─ components/
│  ├─ ClassDiagramView.tsx
│  ├─ ClassDiagramCanvas.tsx
│  ├─ ClassNode.tsx
│  ├─ AssociationEdge.tsx
│  └─ InvariantBadge.tsx
└─ classDiagramFlowMapper.ts

src/features/object-diagram/
├─ components/
│  ├─ ObjectDiagramView.tsx
│  ├─ ObjectDiagramCanvas.tsx
│  ├─ ObjectNode.tsx
│  ├─ ObjectLinkEdge.tsx
│  └─ ValidationBadge.tsx
└─ objectDiagramFlowMapper.ts
```

### Adapter-Prinzip

Das Frontend sollte nicht überall direkt React-Flow-Typen verwenden.

```text
Project View Model
  -> Diagram View Model
  -> React Flow Adapter
  -> React Flow Nodes/Edges
```

Dadurch bleiben fachliche Diagrammdaten und Bibliotheksdetails getrennt.

## Konsequenzen für State Management

| State | Empfehlung |
|---|---|
| Project State | Enthält UML-Modell und Snapshot, unabhängig von React Flow. |
| Diagram State | Enthält View-spezifische Nodes/Edges als abgeleitete Darstellung. |
| Layout State | Speichert Positionen, Größen und Viewport in eigenem Store. |
| Selection State | Speichert `elementType` und `elementId`, nicht nur React-Flow-Node-ID. |
| Validation State | Speichert Fehler und liefert Highlights an Diagramm-Mapping. |
| React Flow State | Möglichst lokal/adaptiert halten; nicht als alleinige fachliche Quelle verwenden. |

Beispiel:

```ts
export interface DiagramElementRef {
  elementType: "class" | "association" | "invariant" | "object" | "objectLink";
  elementId: string;
}

export interface DiagramNodeViewModel {
  id: string;
  ref: DiagramElementRef;
  position: { x: number; y: number };
  validationState?: "none" | "warning" | "error";
}
```

Association-Edges brauchen ein eigenes View Model, damit UI-Labels nicht aus Backend-DTOs ad hoc im Edge gerendert werden:

```ts
export interface AssociationEdgeViewModel {
  id: string;
  ref: DiagramElementRef;
  sourceNodeId: string;
  targetNodeId: string;
  associationName: string;
  sourceEnd: {
    roleName: string;
    multiplicity: string;
  };
  targetEnd: {
    roleName: string;
    multiplicity: string;
  };
  validationState?: "none" | "warning" | "error";
}
```

## MVP-Entscheidung

Für den MVP wird folgende Entscheidung empfohlen:

| Entscheidung | Wert |
|---|---|
| Primäre Diagrammbibliothek | React Flow |
| Rendering-Modell | React Custom Nodes + zwingend eigene Custom Edges fuer UML-Associations und Object Links |
| Layout | Manuelles Layout mit speicherbaren Positionen |
| Auto Layout | Nicht im MVP, optional später mit Dagre/ELK |
| Edge Routing | Einfache Straight/SmoothStep/Bezier Edges im MVP, aber mit eigenem Label-Layout |
| Association-Endlabels | Rollen und Multiplizitaeten werden an Source- und Target-Ende gerendert. |
| Association-Mittellabel | Association-Name wird mittig auf der Kante gerendert. |
| Fehlerdarstellung | Node-/Edge-Klassen und Badges über Validation State |
| Selektion | React-Flow-Events schreiben in zentralen `SelectionState` |
| Persistenz | `LayoutDto` enthält Node-Positionen und Viewport |

## Post-MVP-Bewertung

| Post-MVP-Thema | Bewertung mit React Flow |
|---|---|
| Auto Layout | Gut ergänzbar mit Dagre oder ELK. |
| Mini Map | React Flow bietet passende Bausteine. |
| Node Resize | Möglich für spätere große Klassenkarten. |
| Undo/Redo | Über eigene Command-/History-Schicht möglich. |
| Copy/Paste | Über Selection State und Project Commands möglich. |
| Mehrere Snapshots | Object Diagram State kann pro Snapshot instanziiert werden. |
| Vererbung | Zusätzlicher Edge Type für Generalization. |
| Aggregation/Komposition | Edge Marker und Styles erweiterbar. |
| Assoziationsklassen | Zusätzliche Node/Edge-Kombination erforderlich, aber machbar. |
| Sehr große Modelle | Performance gezielt testen; ggf. Virtualisierung/Reduktion prüfen. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Sind alle benötigten React-Flow-Funktionen im MIT-Kern und nicht nur in Pro-Beispielen? | Muss vor Implementierung geprüft werden. |
| Welche Edge-Typen werden im MVP verwendet: straight, smooth step oder bezier? | Beeinflusst UML-Lesbarkeit. |
| Wie genau werden Endlabels bei kurzen oder stark geknickten Kanten positioniert? | Grundentscheidung ist getroffen: Rollen und Multiplizitaeten sind Endlabels; offen ist nur das konkrete Collision-/Overflow-Verhalten. |
| Wie groß werden typische Modelle im MVP? | Beeinflusst Performance-Tests. |
| Soll Auto Layout im MVP enthalten sein? | Wahrscheinlich nein, aber Layout-Engine sollte später integrierbar bleiben. |
| Wie wird ein Klick auf Validation Results in React Flow fokussiert? | Benötigt `elementId -> nodeId/edgeId` Mapping. |
| Muss der Canvas auf Touch-Geräten voll nutzbar sein? | Betrifft Interaktionsmodell. |

## Quellen

| Quelle | Relevanz |
|---|---|
| React Flow offizielle Dokumentation: https://reactflow.dev/ | React Integration, Custom Nodes/Edges, Interaktion, TypeScript, Lizenz, Beispiele. |
| Cytoscape.js offizielle Dokumentation: https://js.cytoscape.org/ | Graphbibliothek, Performance, MIT-Lizenz, Layout-/Graphfunktionen. |
| JointJS offizielle Website: https://www.jointjs.com/ | Community/Professional-Abgrenzung, React-/TypeScript-Angebot, Diagramming-Fokus. |
| D3 offizielle Website: https://d3js.org/ | Bespoke Visualization, SVG/DOM-Flexibilität. |

## Zusammenfassung

Für das neue React/TypeScript-Frontend ist **React Flow** die beste Wahl für den MVP.

Die Bibliothek passt am stärksten zu den Screenshot-Anforderungen: komplexe Klassen- und Objektkarten als Custom React Nodes, Associations und Objektlinks als Custom Edges, Drag & Drop, Selektion, Edge Labels, Fehler-Markierungen, Layout-Speicherung und gute Integration mit Properties Panel und Validation Results.

Die Entscheidung gilt jedoch nur mit der Einschraenkung, dass UML-Associations und Object Links nicht als einfache React-Flow-Default-Edges umgesetzt werden. Der MVP braucht eigene Edge-Komponenten, die den Association-Namen mittig und Rollen/Multiplizitaeten an beiden Enden rendern.

Cytoscape.js ist stark für Graphen und große Netzwerke, aber weniger natürlich für React-basierte UML-Karten. JointJS ist eine solide Diagramming-Alternative, bringt aber Lizenz- und Community/Professional-Abgrenzungsfragen mit. D3 und Eigenlösungen bieten viel Freiheit, verursachen aber für den MVP zu viel Infrastrukturaufwand.

Die Architektur sollte React Flow dennoch über eigene Diagram View Models und Adapter kapseln, damit fachliche Diagrammdaten nicht unkontrolliert an eine konkrete Bibliothek gebunden werden.
