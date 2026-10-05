# Target Vision

## Zweck dieser Datei

Diese Datei beschreibt die Produktvision des neuen webbasierten UML/OCL-Systems aus Nutzersicht. Sie ergänzt den fachlichen Projektkontext um das Zielbild der Anwendung und berücksichtigt den Dashboard-Screenshot als ersten Einstiegspunkt.

## Zielbild

Die neue Anwendung soll zentrale fachliche Konzepte von USE modern als Webanwendung bereitstellen. Nutzer sollen UML-Klassendiagramme, UML-Objektdiagramme und OCL-Invarianten erstellen, bearbeiten und validieren können.

Das System startet nicht direkt im Klassendiagramm. Der Einstieg erfolgt über ein Dashboard, von dem aus Nutzer neue Modelle starten, bestehende Projekte öffnen oder Unterstützungsangebote erreichen.

![Dashboard Start Page](../assets/screenshots/00-dashboard-start-page.png)

Der Screenshot `18-create-new-projects.png` ergänzt diesen Einstieg: Ein neues Projekt wird erst nach Eingabe eines Projektnamens erstellt.

![Create New Project](../assets/screenshots/18-create-new-projects.png)

Der Screenshot `19-projects.png` konkretisiert den `View all` Einstieg: Nutzer können eine vollständige Projektliste öffnen, vorhandene Projekte suchen, filtern, öffnen oder von dort ein neues Projekt starten.

![All Projects](../assets/screenshots/19-projects.png)

## Dashboard als Einstieg

Der Screenshot `00-dashboard-start-page.png` zeigt die Startseite der Anwendung.

| Bereich | Bedeutung |
|---|---|
| Logo und Produkttitel `USE` | Produktidentität und Wiedererkennbarkeit. |
| Untertitel `UML-based Specification Environment` | Fachlicher Kontext des Werkzeugs. |
| Benutzeravatar | Platzhalter für Nutzerkonto, Profil oder spätere Workspace-Funktionen. |
| `Create New Model` | primärer Einstieg für neue UML/OCL-Projekte. |
| `+ Start Project` | öffnet die Projektnamenerfassung für ein neues Modell. |
| `Create New Project` | erfasst den Pflichtwert Projektname und startet danach die Backend-Projektanlage. |
| `Open Existing` | Einstieg zum Öffnen oder Importieren bestehender Projekte. |
| `.use`-Hinweis | macht den Bezug zum originalen USE-Format sichtbar. |
| `Recent Projects` | schneller Zugriff auf zuletzt genutzte Projekte. |
| `View all` | Einstieg in die vollständige Projektliste aus `19-projects.png`. |
| `Learn & Support` | Einstieg in Dokumentation und Beispiele. |

## Projektübersicht / All Projects

Die Projektübersicht ist die größere Verwaltungsansicht hinter `View all`. Sie liegt vor dem Workspace und dient dazu, alle vorhandenen Projekte zu finden und zu öffnen.

| Bereich | Bedeutung |
|---|---|
| Back-Navigation | Rückkehr zum Dashboard. |
| Titel `All Projects` | klare Trennung von Dashboard und Projektworkspace. |
| Suche | Projekte nach Name oder Beschreibung finden. |
| Filter | Einstieg für spätere Filter nach Typ, Datum, Quelle oder Validierungsstatus. |
| `Open` | bestehenden Projekt-/Importflow öffnen. |
| `+ New Project` | denselben Create-New-Project-Dialog wie auf dem Dashboard starten. |
| Projektkarten | alle erstellten oder verfügbaren Projekte mit Name, Beschreibung und Änderungszeit anzeigen. |

## Startoptionen

| Startoption | Ziel | MVP-Einschätzung |
|---|---|---|
| Neues Modell erstellen | Projektnamen erfassen, leeres Projekt anlegen und ins Klassendiagramm wechseln. | MVP |
| Bestehendes Projekt öffnen | Projekt aus MVP-JSON-Format laden. | MVP |
| `.use`-Projekt importieren | Original-USE-nahe Datei importieren. | Should/Post-MVP, wenn JSON für MVP genügt |
| Recent Project öffnen | Zuletzt genutztes Projekt direkt öffnen. | Should |
| Alle Projekte anzeigen | Vollständige Projektliste mit Suche, Filter und Projektkarten öffnen. | Should |
| Documentation öffnen | Hilfe oder Projektdokumentation erreichen. | Should/Later |
| Examples öffnen | Beispielmodelle als Lern- und Demo-Einstieg öffnen. | Should |

## Zielworkflow

Der Hauptworkflow beginnt auf dem Dashboard:

```text
Dashboard
-> Start Project
-> Projektname eingeben
-> Projekt erstellen, Open Existing nutzen oder All Projects öffnen
-> Class Diagram
-> Object Diagram
-> OCL / Invariants
-> Check Constraints
-> Validation Results
```

Das Dashboard bildet damit den Einstieg in die Modellierungs- und Validierungs-Workflows. Es muss den Nutzer schnell zu einem bearbeitbaren Projekt führen, ohne die eigentliche Facharbeit im Diagramm zu verdecken.

## Produktkern

Der fachliche Kern bleibt:

- UML-Klassendiagramme,
- UML-Objektdiagramme,
- Snapshots,
- Klassen, Attribute und Operationen als Signaturen,
- Assoziationen, Rollen und Multiplizitäten,
- Objekte, Slots und Objektlinks,
- OCL-Invarianten,
- Constraint Validation,
- visuelle und textuelle Fehlerdarstellung.

## Abgrenzung

Die Anwendung ist keine technische Migration des originalen USE-Projekts. Das Dashboard darf auf `.use`-Import hinweisen, aber daraus folgt keine vollständige USE-Kompatibilität im MVP.

MVP-Entscheidung: Das MVP-Projektformat bleibt JSON-basiert. `.use` Import ist sichtbar als Produktziel, aber nur dann MVP, wenn der Projektumfang ausdrücklich darauf erweitert wird.

## Zusammenfassung

Die Zielvision ist eine moderne Webanwendung, die mit einem klaren Dashboard startet und Nutzer von dort in einen vollständigen UML/OCL-Arbeitsablauf führt. Dashboard, Create-New-Project, Open-Existing und All-Projects bilden zusammen den Projektverwaltungsrahmen vor der eigentlichen Modellierung.
