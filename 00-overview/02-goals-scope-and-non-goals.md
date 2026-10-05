# Goals, Scope and Non-Goals

## Zweck dieser Datei

Diese Datei dokumentiert Ziele, fachlichen und technischen Scope, MVP-Abgrenzung, Post-MVP-Erweiterungen und Nicht-Ziele des neuen UML/OCL-Websystems.

Sie dient als verbindliche Orientierung für Analyse, Architektur und spätere Implementierung. Sie soll verhindern, dass der Projektumfang unklar wird oder das Vorhaben fälschlich als vollständige technische Migration des originalen USE-Projekts verstanden wird.

Die zentrale Leitentscheidung lautet:

> Das neue System ist keine Migration, kein Fork und keine technische Weiterentwicklung des originalen USE-Codes. Es ist eine neue Webanwendung, die fachliche Konzepte aus USE als Referenz nutzt.

## Projektziele

Das Projekt verfolgt folgende übergeordnete Ziele:

- Entwicklung eines modernen webbasierten UML/OCL-Werkzeugs.
- Modellierung von UML-Klassendiagrammen.
- Modellierung von UML-Objektdiagrammen und Snapshots.
- Definition von OCL-Invarianten.
- Validierung von UML-Strukturregeln und OCL-Constraints.
- Verständliche Darstellung von Validierungsergebnissen.
- Visuelle Hervorhebung fehlerhafter Objekte, Links oder Modellbestandteile.
- Klare Trennung zwischen Backend und Frontend.
- Aufbau einer langfristig erweiterbaren OCL-Verarbeitung.
- Vorbereitung einer Architektur, die spätere Erweiterungen ohne grundlegenden Neuaufbau ermöglicht.

Das Zielsystem soll nicht nur Diagramme zeichnen, sondern fachliche Modellkonsistenz prüfen. Der Kernnutzen liegt im Zusammenspiel aus Klassenmodell, Snapshot, OCL-Invarianten und Constraint Validation.

## Fachlicher Zielumfang

Der langfristige fachliche Zielumfang umfasst die zentralen Konzepte eines UML/OCL-Modellierungs- und Validierungswerkzeugs.

| Scope-Kategorie | Zielumfang |
|---|---|
| Klassenmodell | UML-Klassen, Attribute, Operationen als Signaturen |
| Beziehungen | Assoziationen, Rollen, Multiplizitäten |
| Objektmodell | Objekte, Slots/Attributwerte, Objektlinks |
| Zustände | Snapshots als prüfbare Objektzustände |
| Constraints | Invarianten und OCL-Ausdrücke |
| Validierung | Multiplicity Checks, Invariant Checks, Typprüfungen |
| Fehlerausgabe | Strukturierte Validation Results mit Bezug auf Modell- und Diagrammelemente |

Perspektivisch können weitere UML/OCL-Konzepte ergänzt werden:

- Vererbung,
- Enumerationen,
- Aggregation und Komposition,
- Assoziationsklassen,
- Pre- und Postconditions,
- Derived Attributes,
- Initialwerte,
- erweiterter OCL-Sprachumfang.

Der initiale Fokus bleibt dennoch auf Klassendiagrammen, Objektdiagrammen, Snapshots und Invarianten. Diese Bereiche bilden den fachlichen Kern des MVP.

## Technischer Zielumfang

Das Zielsystem soll technisch neu aufgebaut werden.

| Technischer Bereich | Zielumfang |
|---|---|
| Backend | Neues Java/Spring-Boot-Backend |
| Frontend | Neues React/TypeScript-Frontend |
| Kommunikation | REST/JSON API |
| Projektformat | Einfaches JSON-Projektformat im MVP |
| OCL | Eigene OCL Engine |
| Validierung | Eigene Validation Engine |
| Persistenz | Im MVP einfaches Speichern/Laden, später mögliche Datenbankpersistenz |
| Interoperabilität | Später möglicher `.use` Import/Export |

Die technische Architektur soll bewusst unabhängig vom originalen USE-Core entstehen. Das Backend verantwortet insbesondere Domänenmodell, semantische Prüfungen, OCL-Verarbeitung und Validierung. Das Frontend verantwortet Interaktion, Darstellung, Diagrammbearbeitung und nutzerfreundliche Visualisierung von Ergebnissen.

## MVP-Scope

Der MVP ist als vollständiger vertikaler Durchstich definiert. Er soll nicht alle später denkbaren Features enthalten, aber den zentralen Arbeitsablauf von Modellierung bis Validierung demonstrierbar machen.

Der MVP muss enthalten:

- Projekt anlegen, laden und speichern in einem einfachen Format.
- Klassen erstellen und bearbeiten.
- Attribute mit primitiven Typen erfassen.
- Operationen als einfache Signaturen erfassen.
- Assoziationen erstellen.
- Rollen erfassen.
- Multiplizitäten erfassen.
- Invarianten erfassen.
- Objekte im Objektdiagramm erzeugen.
- Attributwerte für Objekte setzen.
- Objektlinks erzeugen.
- OCL-Grundpipeline bereitstellen.
- OCL-MVP-Subset auswerten.
- Check Constraints ausführen.
- Strukturierte Validation Results erzeugen.
- Fehler im Objektdiagramm markieren.
- Validation Results Panel anzeigen.
- JSON Import/Export oder Save/Load im MVP-Format bereitstellen.

Der MVP soll dadurch folgenden Ablauf unterstützen:

1. Ein Nutzer erstellt ein Klassendiagramm.
2. Der Nutzer ergänzt Attribute, Operationen, Assoziationen, Rollen und Multiplizitäten.
3. Der Nutzer definiert OCL-Invarianten.
4. Der Nutzer erstellt einen Snapshot im Objektdiagramm.
5. Der Nutzer setzt Attributwerte und Objektlinks.
6. Der Nutzer startet `Check Constraints`.
7. Das System prüft UML- und OCL-Constraints.
8. Das System zeigt Fehler strukturiert und visuell nachvollziehbar an.

## MVP-OCL-Subset

Das MVP-OCL-Subset ist bewusst begrenzt. Es soll ausreichen, um typische Invarianten über Objektattribute und einfache Beziehungen auszudrücken.

| OCL-Element | MVP-Status | Beispiel |
|---|---|---|
| `self` | Enthalten | `self` |
| Attributzugriff | Enthalten | `self.age` |
| Einfache Association Navigation | Enthalten | `self.orders` |
| String-Literale | Enthalten | `'active'` |
| Integer-Literale | Enthalten | `18` |
| Real-Literale | Enthalten | `12.5` |
| Boolean-Literale | Enthalten | `true`, `false` |
| Vergleichsoperatoren | Enthalten | `=`, `<>`, `<`, `<=`, `>`, `>=` |
| Boolean-Operatoren | Enthalten | `and`, `or`, `not`, `implies` |
| Klammern | Enthalten | `(self.age >= 18)` |
| `size` | Enthalten | `self.orders->size() > 0` |
| `isEmpty` | Enthalten | `self.orders->isEmpty()` |
| `notEmpty` | Enthalten | `self.orders->notEmpty()` |

Die Verarbeitung soll konzeptionell über eine erweiterbare Pipeline erfolgen:

```text
Lexer -> Parser -> AST -> Type Checker -> Evaluator -> Validation Result
```

Auch wenn das MVP nur ein kleines Subset unterstützt, soll die Architektur spätere OCL-Erweiterungen ermöglichen.

## Post-MVP-Scope

Nach dem MVP können zusätzliche fachliche, technische und nutzerbezogene Funktionen ergänzt werden.

| Erweiterung | Beschreibung |
|---|---|
| `forAll` | Quantifizierung über Collections |
| `exists` | Existenzprüfung über Collections |
| `select` | Filtern von Collections |
| `collect` | Projektion über Collections |
| `includes` / `excludes` | Collection-Membership-Prüfungen |
| `if-then-else` | Bedingte OCL-Ausdrücke |
| `let` | Lokale OCL-Ausdrucksbindungen |
| `allInstances` | Zugriff auf alle Instanzen einer Klasse |
| Preconditions | Vorbedingungen für Operationen |
| Postconditions | Nachbedingungen für Operationen |
| Derived Attributes | Abgeleitete Attribute |
| Init Values | Initialwerte |
| Vererbung | Generalisierung und Spezialisierung von Klassen |
| Enumerationen | Eigene Enumeration-Typen |
| Aggregation/Komposition | Spezialisierte Assoziationsformen |
| Assoziationsklassen | Klassenartige Eigenschaften von Assoziationen |
| Mehrere Snapshots | Verwaltung mehrerer Objektzustände pro Projekt |
| `.use` Import | Einlesen ausgewählter USE-Modelle |
| `.use` Export | Export in ein USE-nahes Format |
| OCL Syntax Highlighting | Bessere Lesbarkeit im OCL Editor |
| OCL Autocomplete | Unterstützung beim Schreiben von OCL-Ausdrücken |
| Undo/Redo | Rücknahme und Wiederherstellung von Modelländerungen |
| Projektversionierung | Nachvollziehbare Entwicklung von Modellen |
| Datenbankpersistenz | Serverseitige Speicherung von Projekten |
| Testfallbibliothek | Sammlung von Testmodellen auf Basis originaler USE-Beispiele |

Diese Erweiterungen sind wichtig für die langfristige Produktfähigkeit, gehören aber nicht zwingend zum MVP.

## Nicht-Ziele

Folgende Punkte sind ausdrücklich nicht Teil des initialen Scopes:

- keine direkte Migration des USE-Codes,
- keine Wiederverwendung des USE-Cores als Dependency,
- kein Fork des alten USE-Projekts,
- keine Übernahme der alten Desktop-GUI,
- keine vollständige USE-Feature-Parität im MVP,
- keine vollständige OCL-Implementierung im MVP,
- keine Sequenzdiagramme,
- keine State Machines,
- keine Aktivitätsdiagramme,
- keine Deploymentdiagramme,
- keine Komponentendiagramme,
- keine vollständige Modellanimation,
- keine Kollaboration im MVP,
- keine Codegenerierung im MVP.

Diese Nicht-Ziele begrenzen den Aufwand und schützen den MVP vor fachlicher Überladung.

## MVP vs Post-MVP vs Nicht-Ziel

| Thema | MVP | Post-MVP | Nicht-Ziel |
|---|---|---|---|
| Klassendiagramme | Ja | Erweiterung um Vererbung, Enumerationen, Aggregation/Komposition | Nein |
| Objektdiagramme | Ja | Mehrere Snapshots, erweiterte Snapshot-Verwaltung | Nein |
| OCL | Begrenztes Invarianten-Subset | Erweiterte OCL-Ausdrücke, Pre-/Postconditions | Vollständige OCL-Parität im MVP |
| USE-Kompatibilität | Fachliche Referenz | Optionaler `.use` Import/Export | Direkte USE-Core-Nutzung |
| UI | Neue Weboberfläche nach Screenshot-Referenz | Komfortfunktionen wie Undo/Redo, Autocomplete | Alte Desktop-GUI |
| Persistenz | JSON Import/Export oder Save/Load | Datenbankpersistenz, Versionierung | Vollständige Kollaborationsplattform im MVP |
| Diagrammtypen | Klassen- und Objektdiagramme | Fachlich begründete Erweiterungen möglich | Sequenz-, State-, Aktivitäts-, Deployment- und Komponentendiagramme |
| Validierung | Backend-validierte UML/OCL-Constraints | Erweiterte Validierungsregeln und Testfallbibliothek | Reine Frontend-only-Semantik |

## Rolle des originalen USE-Projekts

Das originale USE-Projekt ist Referenz, aber nicht Implementierungsgrundlage.

Es darf genutzt werden für:

- fachliche Orientierung,
- Syntaxanalyse,
- Beispielmodelle,
- Testfälle,
- Verhaltenserwartungen,
- Fehlermeldungsarten,
- Validierungskonzepte.

Es darf nicht genutzt werden als:

- direkte Codebasis,
- Runtime Dependency,
- GUI-Vorlage zur direkten Migration,
- vollständige Feature-Verpflichtung für den MVP.

Die Analyse des originalen USE-Projekts soll helfen, fachlich sinnvolle Entscheidungen zu treffen. Sie ersetzt aber nicht die eigenständige Architektur des neuen Systems.

## Rolle der neuen UI-Screenshots

Die neuen Bilder unter `assets/screenshots/` sind visuelle Referenz für das Zielsystem.

Sie sollen in die Scope-Analyse einfließen für:

- Class Diagram UI,
- Object Diagram UI,
- OCL/Invariants UI,
- Properties Panel,
- Explorer Sidebar,
- Modals,
- Validation Results,
- Fehlerdarstellung,
- MVP-Akzeptanzkriterien.

Die Screenshots sind keine zwingend pixelgenaue UI-Spezifikation. Sie definieren aber funktionale und strukturelle Erwartungen an die spätere Weboberfläche. Besonders wichtig ist, welche Arbeitsbereiche sichtbar sind, welche Aktionen der Nutzer ausführen kann und wie Validierungsfehler nachvollziehbar dargestellt werden.

## Scope-Entscheidungen

| Entscheidung | Begründung | Konsequenz |
|---|---|---|
| Neues Backend statt USE-Core | Das Zielsystem soll technisch unabhängig und webfähig sein. | USE dient als Referenz, nicht als Runtime-Basis. |
| Neues Frontend statt Desktop-GUI | Die Zielanwendung soll browserbasiert und modern bedienbar sein. | Die alte Swing-GUI wird nicht migriert. |
| OCL erweiterbar statt vollständig im MVP | Vollständige OCL-Unterstützung wäre für den MVP zu groß. | Das MVP erhält ein begrenztes, aber architektonisch erweiterbares Subset. |
| JSON-Projektformat statt `.use`-Kompatibilität im MVP | Ein eigenes Format reduziert initiale Komplexität. | `.use` Import/Export wird als Post-MVP-Thema behandelt. |
| Klassendiagramm/Objektdiagramm statt Sequenzdiagramm/State Machine | Der Kernnutzen liegt in Strukturmodellierung und Snapshot-Validierung. | Verhaltensorientierte Diagramme bleiben außerhalb des Scopes. |
| Backend-validierte Semantik statt Frontend-only-Validierung | Semantik, Typprüfung und OCL-Auswertung sollen konsistent und testbar sein. | Das Frontend visualisiert Ergebnisse, das Backend bleibt fachlich autoritativ. |
| Operationen im MVP als Signaturen | Ausführbare Operationssemantik würde den Scope stark erweitern. | Operationen werden strukturell modelliert, aber nicht vollständig ausgeführt. |
| Screenshots als strukturelle Referenz | Die Bilder zeigen Zielabläufe und UI-Bereiche. | Die UI muss funktional vergleichbar sein, aber nicht pixelgenau identisch. |

## Abgrenzung zwischen Backend und Frontend

| Verantwortlichkeit | Backend | Frontend |
|---|---|---|
| Projektmodell | Definiert und validiert die fachliche Struktur | Bearbeitet und visualisiert das Projektmodell |
| Klassendiagramm | Prüft Klassen, Attribute, Operationen, Assoziationen, Rollen und Multiplizitäten | Stellt Diagrammelemente dar und ermöglicht Interaktion |
| Objektdiagramm | Prüft Objekte, Slots, Objektlinks und Snapshot-Konsistenz | Stellt Objekte und Links visuell dar |
| OCL | Lexing, Parsing, AST, Type Checking, Evaluation | OCL-Eingabe, Anzeige von Syntax- oder Validierungsfeedback |
| Constraint Validation | Führt `Check Constraints` fachlich aus | Startet Prüfung und zeigt Ergebnisse an |
| Validation Results | Erzeugt strukturierte Ergebnisse mit Elementbezug | Rendert Validation Results Panel und Fehler-Markierungen |
| Persistenz im MVP | Nimmt JSON-Projekte entgegen und liefert sie aus, falls serverseitig umgesetzt | Bietet Import/Export oder Save/Load-Bedienung an |
| UI-Zustand | Kein Fokus | Verantwortet Selektion, Panels, Modals, Canvas-Zustand |

Die klare Trennung soll verhindern, dass fachliche Semantik ausschließlich im Frontend entsteht. Das Frontend kann lokale UI-Prüfungen unterstützen, die verbindliche Modell- und Constraint-Validierung soll jedoch im Backend liegen.

## Offene Scope-Fragen

Folgende Fragen müssen im weiteren Analyseverlauf konkretisiert werden:

- Soll der MVP ein serverseitiges Speichern/Laden enthalten oder reicht zunächst JSON Import/Export im Browser?
- Welche primitiven Datentypen sind im MVP verbindlich: `String`, `Integer`, `Real`, `Boolean` und weitere?
- Wie genau werden Multiplizitäten intern repräsentiert und validiert?
- Wie stark soll das Frontend bereits im MVP OCL-Syntaxfehler beim Tippen anzeigen?
- Welche Beispielmodelle aus dem originalen USE-Projekt eignen sich als MVP-Testfälle?
- Welche Fehlertypen müssen im Validation Results Panel mindestens unterschieden werden?
- Soll es im MVP genau einen Snapshot pro Projekt geben oder bereits eine einfache Snapshot-Liste?
- Welche Teile des Projektformats müssen stabil sein, damit spätere Implementierungsrepositories darauf aufbauen können?

## Zusammenfassung

Das neue UML/OCL-Websystem soll einen klar fokussierten Kern liefern: Klassendiagramme modellieren, Objektdiagramme und Snapshots erstellen, OCL-Invarianten definieren und Constraints nachvollziehbar validieren.

Der MVP ist als vertikaler Durchstich geplant. Er enthält ein begrenztes, aber vollständiges Zusammenspiel aus Modellierung, OCL-Grundverarbeitung, Validierung und Ergebnisdarstellung.

Das originale USE-Projekt bleibt fachliche Referenz, wird aber nicht technisch übernommen. Die neuen Screenshots dienen als visuelle und funktionale Orientierung, ohne eine pixelgenaue UI-Kopie zu erzwingen.

Durch die klare Abgrenzung von MVP, Post-MVP und Nicht-Zielen bleibt der Projektumfang kontrollierbar und die spätere Entwicklung gezielt vorbereitbar.
