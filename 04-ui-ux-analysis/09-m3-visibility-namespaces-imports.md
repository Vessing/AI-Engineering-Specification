# M3 Sichtbarkeit, Namespaces und Imports

## Zweck und Status

Dieses Dokument ist das Ergebnis von Mockup-Roadmap-Schritt M3. Es definiert
die Benutzeroberfläche für UML-Sichtbarkeit, Namespace-Gruppierung,
qualifizierte Namen und modellinterne Imports.

**Status:** `READY_FOR_REVIEW`  
**Gesamtstatus:** `MOCKUP`, noch nicht `BACKEND_READY`.

**Überarbeitungsstand:** M3 übernimmt die aktualisierte M1-Baseline und macht
den einfachen Sichtbarkeits-Workflow, progressive Modellstruktur, Hilfe und
Accessibility auch im visuellen Artefakt überprüfbar.

Die offiziellen Vollansichten liegen unter
`assets/mockups/class-properties-details.html`,
`assets/mockups/class-properties-attributes.html`,
`assets/mockups/class-properties-operations.html` und
`assets/mockups/project-imports.html`. `Class Properties -> Details` besitzt
die Klassenfelder sowie Package- und Namespace-Zuordnung. Die Attribute- und
Operationsansichten enthalten die jeweilige Feature-Sichtbarkeit. `Project
Imports` verwaltet dagegen modellinterne Abhängigkeiten, Importanalyse,
Provenienz und importierte Read-only-Elemente. Dadurch bleiben
Klassenbearbeitung und projektweite Importverwaltung getrennte, aber durch den
Explorer verbundene Workflows.

## Verwendete Quellen

### Analyse-Dokumente

- `00-overview/03-documentation-map.md`
- `03-uml-ocl-domain/01-uml-ocl-scope.md`
- `03-uml-ocl-domain/02-domain-model.md`
- `04-ui-ux-analysis/01-ui-overview.md`
- `04-ui-ux-analysis/02-class-diagram-ui.md`
- `04-ui-ux-analysis/05-screenshot-traceability.md`
- `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md`
- `04-ui-ux-analysis/07-m1-ui-baseline.md`
- `04-ui-ux-analysis/08-m2-generalization-and-abstract-classes.md`
- `04-ui-ux-analysis/10-redesign-design-principles.md`
- `06-frontend-analysis/03-frontend-architecture.md`
- `06-frontend-analysis/07-class-diagram-component.md`
- `06-frontend-analysis/09-ocl-editor-ui.md`
- `06-frontend-analysis/11-properties-panel.md`
- `06-frontend-analysis/12-modal-dialogs.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/07-dto-reference.md`
- `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`
- `09-ocl-extension-analysis/15-full-ocl-uml-implementation-plan.md`

### Screenshots

| Screenshot | Übernommenes Muster |
|---|---|
| `01-class-diagram-class-properties.png` | Class Diagram, Explorer und Properties |
| `03-class-diagram-invariant-properties.png` | OCL-Feld und kontextbezogene Fehler |
| `13-ocl-editor.png` | Modelltext, Source Locations und Apply-Zustand |
| `14-open-existing-project.png` | Dateiauswahl, Dropzone und Importdiagnosen |
| `15-properties-association.png` | segmentiertes, scrollbar bleibendes Panel |
| `17-new-class.png` | Editierfelder und bestehende Formmuster |

## Begriffs- und Workflow-Abgrenzung

`Open Existing Project` im Dashboard und `Add Import` im Workspace sind zwei
verschiedene Aktionen:

| Aktion | Ergebnis |
|---|---|
| `Open Existing Project` | Eine `.use`-Datei wird als vollständiges Projekt geöffnet oder angewendet. |
| `Add Import` | Ein bereits geöffnetes Projekt erhält eine benannte Modellabhängigkeit mit Provenienz. |

M3 ersetzt den bestehenden Dashboard-Import nicht. Der neue Importflow beginnt
über die Explorer-Gruppe `Imports` oder eine gleichwertige Projektaktion.

## UML-Sichtbarkeit

Die Oberfläche verwendet die üblichen UML-Symbole und nennt sie im Formular
zusätzlich textuell:

| Symbol | Wert im Formular | Bedeutung |
|---|---|---|
| `+` | `public` | allgemein sichtbar |
| `-` | `private` | nur im definierenden Classifier sichtbar |
| `#` | `protected` | im definierenden Classifier und zulässigen Spezialisierungen sichtbar |
| `~` | `package` | innerhalb des zulässigen Package-/Namespace-Kontexts sichtbar |

Attribute und Operationen zeigen das Symbol direkt vor dem Namen. Farbe dient
nur als zusätzliche Orientierung. Das Properties Panel verwendet ein Select,
damit Symbol und ausgeschriebener Wert gemeinsam zugänglich sind.

## Namespace-Darstellung

Der Explorer gruppiert Elemente hierarchisch nach Packages beziehungsweise
Namespaces. Klassenknoten zeigen den Namespace kompakt über dem Klassennamen.
Der vollständige qualifizierte Name wird dauerhaft angezeigt, wenn Namen
mehrdeutig sind; ansonsten ist er in Properties und Tooltip verfügbar.

Beispiel:

```text
university::people::Person
shared::core::Identifier
```

Die UI normalisiert oder entscheidet Namen nicht eigenständig. Gültigkeit,
Auflösung und Mehrdeutigkeit werden vom Backend geprüft.

## Explorer-Struktur

```text
Packages
└── university
    ├── people
    │   ├── Person
    │   └── Student
    └── courses
        └── Course

Imports
├── shared::core       resolved
└── finance            warning
```

Importeinträge zeigen Namespace beziehungsweise Alias, Quelldatei und Status.
Technische Speicherpfade werden nicht als primäre Beschriftung verwendet.

Ein aufgeklappter Import bildet eine eigene read-only Modellstruktur. Unter
dem Importknoten erscheint die ursprüngliche rekursive Package-Hierarchie des
importierten Modells. Classifier werden ihrem Package zugeordnet und durch
Name sowie gegebenenfalls UML-Stereotyp gekennzeichnet. Es gibt keine
künstliche Trennung in `Classes`, `Enumerations` und `DataTypes`.
Associations und Invariants erscheinen nicht als parallele Explorer-Hauptordner. Nach Auswahl einer importierten Klasse
wechselt der Benutzer über `Class | Association | Invariant` zwischen den
Read-only-Class-Properties und den zugehörigen Associations beziehungsweise
Constraints. Die Ordner können unabhängig geöffnet werden. Die
Auswahl des Importnamens zeigt allgemeine Importdaten und Aktionen. Die Auswahl eines konkreten Elements wie
`shared::core::Identifier` zeigt ausschließlich dessen fachliche
Read-only-Properties. Der Canvas bleibt dabei das normale Klassendiagramm.
Importierte Klassen verwenden dieselben Untertabs und Feldgruppen wie lokale
`Class Properties`: `Details`, `Attributes`, `Operations` und
`Generalizations`. Die Controls bleiben sichtbar, sind jedoch deaktiviert und
mit `IMPORTED · READ ONLY` gekennzeichnet. Dadurch müssen Benutzer kein
zweites Properties-Muster erlernen und können importierte und lokale
Modellelemente direkt vergleichen. Änderungsaktionen wie Hinzufügen, Speichern
oder Entfernen eines Features sind nicht verfügbar. Projektbezogene Aktionen
wie `Im Diagramm anzeigen`, `Aus Diagramm entfernen` und `Find References`
bleiben aktiv.
Enumerations und DataTypes verwenden ihre eigenen fachlichen
Read-only-Properties und zeigen deshalb nicht den Reiter
`Class | Association | Invariant`.
Erst die explizite Aktion `Im Diagramm anzeigen` fügt das importierte Element
als gekennzeichneten Read-only-Knoten hinzu. `Aus Diagramm entfernen` entfernt
nur diese Darstellung, nicht das importierte Element oder seine Referenzen.
Dies gilt für importierte Classes, Associations, Enumerations und DataTypes.
Eine Association blendet ihre benötigten End-Classifier gemeinsam ein. Eine
importierte Generalisierung wird sichtbar, sobald Sub- und Supertype im
Diagramm dargestellt sind. Bei einer importierten Invariant zeigt
`Kontext im Diagramm anzeigen` ihren Context-Classifier; die Invariant selbst
bleibt ein Read-only-Constraint im Properties Panel und in Validation Results.

Der Canvas stellt keine abstrakte Verbindung zwischen `Current Project` und
`Imported Model` und auch keine automatische Gesamtansicht des Imports dar.
Die Auswahl des Importnamens verändert den Canvas nicht. Allgemeine Angaben
wie Quelldatei, Alias, Provenienz und Importstatus erscheinen ausschließlich
im rechten `Import Details`-Panel.

## Properties Panel

M3 ergänzt folgende Felder, ohne die M2-Generalization-Felder zu ersetzen:

| Element | Felder |
|---|---|
| Classifier | Namespace, Class Visibility, qualifizierter Name read-only |
| Attribute | Visibility Select |
| Operation | Visibility Select |
| Import | Source, Namespace, Alias, Provenienz, Status und Remove Action |

Das Panel bleibt intern scrollbar. Visibility wird pro Feature geändert und
atomar gespeichert. Geerbte oder importierte read-only Features dürfen nicht
so wirken, als könnten sie lokal überschrieben werden.

Die offizielle Ansicht `project-imports.html` zeigt den Zustand eines im
Explorer ausgewählten Imports. `Import Details` fasst Quelle, Namespace, Alias, Status,
Provenienz, Importzeitpunkt und die Anzahl importierter Elemente zusammen.
`Im Diagramm anzeigen`, `Reanalyze`, `Replace Source` und das bestätigte
`Remove Import` wirken auf den Importvertrag; die importierten Modellelemente
selbst bleiben im aktuellen Projekt read-only.

## Add-Import-Dialog

Der Dialog enthält:

1. lokale `.use`-Datei oder später eine eindeutig definierte Modellquelle,
2. Zielnamespace,
3. optionalen Alias,
4. Analysezusammenfassung für Klassen, Associations und Diagnostics,
5. initiale Diagrammoption `Nur importieren` als Standard,
6. optionale Varianten `Ausgewählte Elemente anzeigen` und
   `Alle Elemente anzeigen`, wobei die letzte Variante bei großen Modellen
   eine Warnung erfordert,
7. `Cancel` und `Add Import`.

Bei `Ausgewählte Elemente anzeigen` öffnet sich eine nach Elementarten
verständliche Checkliste. Abhängige Elemente, beispielsweise die Enden einer
Association, werden vor dem Anwenden kenntlich gemacht und gemeinsam
eingeblendet. `Alle Elemente anzeigen` nennt die Anzahl der hinzukommenden
Elemente und warnt vor einer möglichen Neuanordnung des Diagramms. Keine der
Anzeigeoptionen verändert die Read-only-Eigenschaft des Imports.

Vor `Add Import` wird eine serverseitige Analyse erwartet. Warnungen dürfen
bestätigbar sein, blockierende Konflikte deaktivieren die Aktion. Ein Import
wird atomar angewendet; bei Fehler bleibt der bisherige Projektzustand erhalten.

## Zustände und Diagnostics

| Zustand | Darstellung |
|---|---|
| Default | Packages und Imports eingeklappt oder kompakt gruppiert |
| Selected | Klasse, Feature oder Import ist in Explorer/Canvas/Properties synchron gewählt |
| Editing | Visibility Select oder Add-Import-Formular aktiv |
| Loading | Importanalyse oder Apply läuft; Quelle bleibt sichtbar |
| Success | aufgelöster Import mit Provenienz und Confirmation |
| Warning | nicht blockierender Konflikt mit fachlichem Namen |
| Error | Importzyklus, Namenskonflikt, unbekannter Namespace oder unzulässiger Zugriff |
| Disabled | Add Import bei blockierendem Fehler; nicht bearbeitbare importierte Features |
| Empty | `No imports` beziehungsweise `No elements in this namespace` |
| Confirmation | Remove Import nennt betroffenen Namespace und mögliche Abhängigkeiten |

Strukturelle Package- und Importdiagnosen erscheinen direkt im auslösenden
Dialog. Dazu gehören ungültige Package-Hierarchien, doppelte Package-Namen,
Importzyklen und nicht auflösbare Importquellen.

OCL-bezogene Auflösungs- und Sichtbarkeitsfehler erscheinen beim Anwenden oder
Prüfen der Constraints in `Validation Results`. Die offizielle Vollansicht
zeigt `UNKNOWN_NAMESPACE`, `INACCESSIBLE_PROPERTY` und `AMBIGUOUS_NAME` mit
Constraint-Name, Zeile, Spalte, qualifizierten Namen und der Aktion
`Open Constraint`. `AMBIGUOUS_NAME` nennt alle kollidierenden qualifizierten
Namen und fordert zur eindeutigen qualifizierten Referenz auf. Die
spätere OCL-Editor-Überarbeitung definiert, wie diese Aktion den Editor öffnet
und die gemeldete Source Range markiert. Diese Editorvisualisierung ist keine
Voraussetzung für den Abschluss des M3-Details-Mockups.

## Responsive Verhalten

Der Canvas beziehungsweise OCL Editor bleibt Hauptfläche. Packages/Imports und
Properties werden als getrennte Drawer geöffnet. Der Add-Import-Dialog nutzt
die verfügbare Breite, besitzt einen intern scrollbaren Body und hält Header
sowie Aktionen sichtbar.

Beschriftete Aktionen öffnen Explorer, Properties und Hilfe. `Escape` oder eine
sichtbare Schließen-Aktion beendet Drawer und Dialoge. Danach kehrt der Fokus
zum auslösenden Element zurück. Im Importdialog verläuft die Fokusreihenfolge
von Quelle über Zielnamespace und optionale Angaben zu Abbrechen und
Bestätigen.

## Anfängerfreundlichkeit, Modernisierung und Hilfe

Der einfache M3-Kernweg lautet:

1. Klasse, Attribut oder Operation auswählen.
2. Eine ausgeschriebene Sichtbarkeit mit zugehörigem UML-Symbol wählen.
3. Änderung anwenden und die Darstellung unmittelbar im Klassenknoten prüfen.
4. Nur bei gleichnamigen Elementen oder verteilten Modellen die fortgeschrittene
   Namespace- beziehungsweise Importkonfiguration öffnen.

Namespace-Eingaben werden deshalb unter `Fortgeschritten: Namespace` gezeigt.
Imports bleiben als eigener, vollständig erreichbarer Modellstruktur-Flow
vorhanden. Importanwender wählen zuerst Datei und Zielnamespace; Alias,
Provenienzdetails und transitive Informationen erscheinen als fortgeschrittene
Optionen.

Quick Help erklärt `+` als `public`, `-` als `private`, `#` als `protected` und
`~` als `package`. Außerdem erklärt sie qualifizierte Namen und den Unterschied
zwischen `Open Existing` und `Add Import`. Die Symbole besitzen zugängliche
Namen und werden in Formularen immer mit Text kombiniert.

Importfeedback trennt eine kurze handlungsorientierte Meldung von aufklappbaren
technischen Codes, Importketten und OCL-Quelldetails. Fehler, Warnungen und
Importstatus sind nicht ausschließlich über Farbe erkennbar.

M3 übernimmt die messbaren M1-Vorgaben: Diagrammtext ist mindestens `13 px`,
Namespace- und andere kompakte Metadaten sind mindestens `12 px`, Formtexte
mindestens `14 px`, Desktop-Aktionen mindestens `40 px` und Aktionen im
schmalen Viewport mindestens `44 px` hoch. Normaler Text erreicht mindestens
`4,5:1` Kontrast; relevante UI-Grenzen mindestens `3:1`. Der sichtbare
Tastaturfokus ist mindestens `3 px` stark.

Damit adressiert M3 `RD-NAV-001` bis `RD-NAV-005`, `RD-VIS-001` bis
`RD-VIS-004` sowie `RD-A11Y-001` bis `RD-A11Y-005`.

## Compliance-Zuordnung

| Matrix-ID | M3-Bezug |
|---|---|
| `CM-UML-004` | Sichtbarkeit von Classifiern, Attributen und Operationen |
| `CM-UML-018` | Packages, Namespaces, Imports, Zyklen und Provenienz |
| `CM-OCL-001` | qualifizierte Namen und lexikalische Auflösung |
| `CM-CTX-008` | Package-/Namespace-Kontext für OCL |

M3 markiert keine Matrix-ID als technisch erfüllt.

## Anforderungen an Backend, API und DTOs

| Bereich | Vorläufige Anforderung |
|---|---|
| Visibility | enum-artiger Wert `PUBLIC`, `PRIVATE`, `PROTECTED`, `PACKAGE` für relevante UML-Elemente |
| Namespace | stabile Namespace-/Package-Identität und qualifizierter Name getrennt vom Anzeigenamen |
| Imports | Quelle, Zielnamespace, optionaler Alias, Status und Provenienz persistieren |
| Analyse | Dry-run-Endpunkt oder Apply-Modus, der Diagnostics ohne Mutation liefert |
| Apply | atomare Importanwendung und unveränderter Projektzustand bei Fehler |
| Resolver | qualifizierte und unqualifizierte Namen mit eindeutigen Konfliktregeln auflösen |
| Typechecker | Visibility und aktuellen Namespace-/Classifier-Kontext beachten |
| Diagnostics | Importzyklus, Ambiguität, unbekannter Name und Zugriffsschutz strukturiert liefern |

Ein mögliches späteres Vertragsmodell umfasst `PackageDto`, `ModelImportDto`,
`ImportAnalysisDto`, `VisibilityDto` und Source-Provenienz. Die endgültige API
wird in M3 noch nicht festgelegt oder implementiert.

## Anforderungen an das Frontend

- Explorer benötigt rekursive Package-Gruppen und einen Imports-Bereich.
- Jeder Importknoten benötigt die rekursive Package-Hierarchie des
  importierten Modells sowie selektierbare importierte Classifier. Class,
  Enumeration und DataType bleiben über UML-Notation und Properties
  unterscheidbar. Associations und Invariants werden über die zugehörige
  Context-Klasse erschlossen.
- Class-/Feature-Daten benötigen Visibility und Namespace-Metadaten.
- Klassenknoten rendern UML-Sichtbarkeitssymbole und bei Bedarf Namespaces.
- Properties verwenden zugängliche Visibility Selects statt Symbol-only Inputs.
- Add Import ist ein eigener Modaltyp und nicht der Dashboard-Open-Existing-Flow.
- Importstatus und Diagnostics müssen nach stabilen Import-/Element-IDs gemappt werden.
- OCL Source Ranges müssen im Editor fokussierbar sein.
- Importierte read-only Elemente benötigen einen eindeutigen visuellen Status.

## Annahmen und offene Entscheidungen

| Typ | Eintrag |
|---|---|
| Annahme | `::` ist die sichtbare Trennung qualifizierter Namen. |
| Annahme | Imports werden im geöffneten Projekt verwaltet und nicht mit Open Existing vermischt. |
| Offen | ob Packages eigene Canvas-Frames erhalten; M3 legt nur Explorer- und Namensdarstellung fest |
| Offen | unterstützte Importquellen neben lokaler `.use`-Datei |
| Offen | editierbare Kopie gegenüber read-only referenziertem Import |
| Offen | genaue Regeln für Alias, Re-export und transitive Imports |
| Offen | ob Classifier selbst eine Visibility besitzen oder nur Features |

## Interne Akzeptanzprüfung

- [x] Alle vier UML-Sichtbarkeiten sind als Symbol und Text dargestellt.
- [x] Namespace-Gruppierung und qualifizierte Namen sind definiert.
- [x] Add Import ist vom Dashboard-Import getrennt.
- [x] Importanalyse, Success, Warning und blockierende Fehler sind entworfen.
- [x] Zyklus- und Namenskonflikte verwenden fachliche Namen.
- [x] OCL-Namespace- und Visibility-Diagnosen sind in Validation Results mit
  Constraint, Zeile und Spalte dargestellt.
- [ ] Die visuelle Source-Range-Markierung und Fehlerkorrektur im OCL Editor
  wird im späteren OCL-Editor-Mockup festgelegt.
- [x] Desktop und schmaler Viewport sind berücksichtigt.
- [x] API-/DTO-Folgen sind dokumentiert, aber nicht implementiert.
- [x] M4 und spätere Association-Features wurden nicht vorgezogen.
- [x] Der einfache Sichtbarkeitsweg ist gegenüber Namespace- und Importoptionen
  visuell priorisiert.
- [x] Namespace-Konfiguration und technische Diagnosedetails sind progressiv
  offengelegt, bleiben aber vollständig erreichbar.
- [x] Quick Help erklärt alle vier UML-Symbole mit ausgeschriebenen Begriffen.
- [x] Schrift-, Control-, Kontrast-, Fokus- und Drawer-Anforderungen aus M1 sind
  für M3 konkretisiert.
- [x] Produktiver Frontend- und Backend-Code blieb unverändert.
- [x] Einfacher Importweg, progressive Optionen und kontextbezogene Hilfe sind festgelegt.
- [ ] Fachliche Freigabe durch den Auftraggeber steht aus.
- [ ] `BACKEND_READY` bleibt offen.

## Abgrenzung

M3 entwirft keine Association-End-Metadaten, Qualifier, n-ären Associations,
Association Classes, Operation Invocation oder vollständige Importsemantik.
