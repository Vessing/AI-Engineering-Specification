# Acceptance Criteria

## Zweck dieser Datei

Diese Datei definiert Akzeptanzkriterien für den MVP und angrenzende Should-/Post-MVP-Funktionen. Sie verbindet Screenshots, User Journey und funktionale Anforderungen mit prüfbaren Ergebnissen.

## Dashboard und Projektstart

| ID | Kriterium | Priorität | Akzeptanz |
|---|---|---|---|
| `AC-DASH-001` | Nutzer sieht die Startseite. | MVP | Beim Öffnen der Anwendung erscheint das Dashboard mit Logo `USE`, Untertitel `UML-based Specification Environment`, Benutzeravatar und den Bereichen `Create New Model`, `Open Existing`, `Recent Projects` und `Learn & Support`. |
| `AC-DASH-002` | Nutzer kann über `Start Project` den Projektstart öffnen. | MVP | Klick auf `+ Start Project` öffnet den Create-New-Project-Dialog oder ein äquivalentes Formular aus Screenshot `18-create-new-projects.png`. |
| `AC-DASH-002A` | Projektname ist beim Erstellen Pflicht. | MVP | Nutzer kann ein neues Projekt erst erstellen, wenn ein nicht leerer Projektname eingegeben wurde; leere Namen zeigen einen Feldfehler oder deaktivieren Submit. |
| `AC-DASH-002B` | Projekt wird mit eingegebenem Namen angelegt. | MVP | Submit sendet `POST /api/v1/projects` mit `CreateProjectRequestDto.name`; die Antwort enthält ein `ProjectDto` mit ID und Name. |
| `AC-DASH-003` | Nutzer gelangt nach Projektstart ins Klassendiagramm. | MVP | Nach erfolgreichem Projektstart wird `/projects/{projectId}/class-diagram` oder eine äquivalente Class Diagram View geöffnet. |
| `AC-DASH-004` | Nutzer sieht die Option `Open Existing`. | MVP/Should | Dashboard zeigt `Open Existing`; Klick öffnet das Open-Existing-Modal oder einen klaren Import-Einstieg. |
| `AC-DASH-005` | Nutzer sieht Recent Projects. | Should | Dashboard zeigt zuletzt verwendete Projekte oder Mock-/Demoeinträge wie `University System`, `Hotel Management`, `Bank ATM`. |
| `AC-DASH-006` | Nutzer sieht Documentation und Examples Einstiegspunkte. | Should/Later | Dashboard zeigt `Documentation` und `Examples`; Links führen zu vorhandenen Seiten oder klaren Platzhalterzuständen. |
| `AC-DASH-007` | Nutzer kann über `View all` die Projektliste öffnen. | Should | Klick auf `View all` führt zu `/projects` oder einer äquivalenten All-Projects-Ansicht aus Screenshot `19-projects.png`. |

## Projektliste / All Projects

| ID | Kriterium | Priorität | Akzeptanz |
|---|---|---|---|
| `AC-PROJECTS-001` | Nutzer sieht die All-Projects-Seite. | Should | Die Ansicht zeigt `All Projects`, Untertitel, Suchfeld, `Filter`, `Open`, `+ New Project` und eine Projektkartenliste wie in Screenshot `19-projects.png`. |
| `AC-PROJECTS-002` | Projektkarten zeigen lesbare Metadaten. | Should | Jede Karte zeigt Projektname, Kurzbeschreibung oder Typinformation und letzten Änderungszeitpunkt; technische IDs werden nicht als primäre UI-Information angezeigt. |
| `AC-PROJECTS-003` | Nutzer kann ein Projekt aus der Liste öffnen. | Should | Klick auf eine Projektkarte oder `Open` lädt das Projekt per ID und navigiert ins Klassendiagramm oder zur zuletzt genutzten Projektansicht. |
| `AC-PROJECTS-004` | Nutzer kann aus der Projektliste ein neues Projekt starten. | Should | Klick auf `+ New Project` öffnet denselben Create-New-Project-Flow wie `Start Project` auf dem Dashboard. |
| `AC-PROJECTS-005` | Nutzer kann Projekte suchen. | Should | Eingabe im Suchfeld filtert Projektkarten mindestens clientseitig; serverseitige Suche ist Post-MVP, falls Persistenz/Pagination erweitert wird. |
| `AC-PROJECTS-006` | Nutzer sieht einen Filter-Einstieg. | Later | `Filter` ist sichtbar; im MVP darf der Button deaktiviert sein oder einen einfachen Platzhalterzustand zeigen, solange dies erkennbar ist. |

## Modellierungs- und Validierungsworkflow

| ID | Kriterium | Priorität | Akzeptanz |
|---|---|---|---|
| `AC-FLOW-001` | Hauptworkflow startet am Dashboard. | MVP | Dokumentierter und getesteter Workflow lautet: Dashboard -> Start Project -> Projektname eingeben oder Open Project -> Class Diagram -> Object Diagram -> OCL/Invariants -> Check Constraints -> Validation Results. |
| `AC-FLOW-002` | Klassendiagramm ist nach Projektstart erreichbar. | MVP | Nutzer kann Klassen, Attribute, Operationen, Associations und Invarianten sehen oder anlegen. |
| `AC-FLOW-003` | Objektdiagramm ist aus einem Projekt erreichbar. | MVP | Nutzer kann Objekte, Slot-Werte und Objektlinks bearbeiten. |
| `AC-FLOW-004` | Constraint Check zeigt strukturierte Ergebnisse. | MVP | Backend liefert Validation Results; Frontend zeigt Fehler im Panel und markiert betroffene Diagrammelemente. |

## OCL Editor

| ID | Kriterium | Priorität | Akzeptanz |
|---|---|---|---|
| `AC-OCL-001` | Nutzer kann die OCL-Editor-Ansicht öffnen. | MVP | Der Tab `OCL Editor` öffnet eine eigene Hauptansicht und keinen Platzhalter. |
| `AC-OCL-002` | Nutzer sieht einen textuellen Modell-/OCL-Editor. | MVP | Die Ansicht zeigt USE-ähnlichen Modelltext mit Zeilennummern, Klassen, Attributen, Operationen, Associations und Constraints. |
| `AC-OCL-003` | Nutzer kann Modell-/OCL-Text im OCL Editor bearbeiten. | MVP | Änderungen erzeugen einen Draft und können über `Apply Changes` angewendet werden; Validation Results werden als veraltet markiert. |
| `AC-OCL-004` | Backend-Diagnosen können im OCL Editor erscheinen. | MVP/Should | Parse-/Typecheck-Antworten werden mit Severity, Code, Message und optional Zeile/Spalte angezeigt; das Frontend entscheidet OCL-Semantik nicht selbst. |
| `AC-OCL-005` | OCL Editor bleibt mit Projektzustand, Diagrammen und Properties Panel synchron. | MVP | Nach `Apply Changes` werden Klassen, Associations und Invarianten konsistent in Class Diagram, Explorer, Properties Panel und Validation State übernommen oder Fehler angezeigt. |

## Import-Entscheidung

| ID | Kriterium | Priorität | Akzeptanz |
|---|---|---|---|
| `AC-IMPORT-001` | JSON-Projekt kann im MVP geöffnet werden. | MVP | Ein im MVP-Format gespeichertes Projekt kann geladen und weiterbearbeitet werden. |
| `AC-IMPORT-002` | Open-Existing-Modal wird angezeigt. | MVP/Should | Nach Klick auf `Open Existing` erscheint `Open Existing Project` mit `Local File`, Upload-/Drag-and-drop-Fläche, Hinweis `Supported format: .use`, `Cancel`, `Open Project` und Close-Icon. |
| `AC-IMPORT-003` | Nutzer kann eine lokale `.use`-Datei auswählen. | Should | Die UI akzeptiert eine `.use`-Datei per Dateiauswahl oder Drag-and-drop, zeigt einen gültigen Auswahlzustand und verhindert `Open Project` ohne Datei. |
| `AC-IMPORT-004` | `.use` Import ist funktional begrenzt und klar abgegrenzt. | Should/Post-MVP | Wenn vollständige `.use` Kompatibilität noch nicht umgesetzt ist, wird die Datei höchstens als Modelltext mit MVP-Subset verarbeitet; nicht unterstützte Syntax erzeugt strukturierte Diagnosen statt stillen Fehlern. |
| `AC-IMPORT-005` | Importdiagnosen sind sichtbar. | Should | Syntaxfehler, nicht unterstützte USE-Konstrukte oder Backend-Importfehler werden im Modal, OCL Editor, Console oder Diagnostics/Validation-Bereich angezeigt. |

## Zusammenfassung

Die Dashboard- und Projektlisten-Screenshots ergänzen die Akzeptanzkriterien um den tatsächlichen Startpunkt der Anwendung. Ein MVP gilt nicht mehr als vollständig, wenn er ausschließlich im Klassendiagramm startet und keinen Einstieg über Dashboard oder Projektstart bietet. Die All-Projects-Seite ist als Should-Umfang einzuplanen, damit `View all` aus dem Dashboard nicht ins Leere führt.
