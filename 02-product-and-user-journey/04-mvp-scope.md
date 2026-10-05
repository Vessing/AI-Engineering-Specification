# MVP Scope

## Zweck dieser Datei

Diese Datei definiert den MVP-Scope für das neue UML/OCL-Websystem.

Sie beschreibt, welche Funktionen im ersten vollständigen vertikalen Durchstich enthalten sein müssen, welche Funktionen optional sind, welche Funktionen bewusst ausgeschlossen werden und welche Themen für spätere Ausbaustufen vorgesehen sind.

Der MVP ist nicht als vollständige Nachbildung des originalen USE-Systems zu verstehen. Er soll den fachlichen Kern nachweisen:

- UML-Klassenmodell erstellen,
- OCL-Invarianten definieren,
- Snapshot im Objektdiagramm aufbauen,
- Attributwerte und Objektlinks setzen,
- Constraints prüfen,
- Fehler visuell und textuell nachvollziehbar anzeigen.

Durch den neuen Dashboard-Screenshot beginnt der MVP-Workflow nicht mehr direkt im Klassendiagramm. Der MVP umfasst auch einen klaren Projektstart über eine Dashboard-/Startseite.

## Ziel des MVP

Der MVP soll zeigen, dass das neue Websystem den zentralen UML/OCL-Arbeitsablauf end-to-end unterstützt.

Das bedeutet:

| Ziel | Bedeutung für den MVP |
|---|---|
| Projektstart | Nutzer starten auf einem Dashboard und können ein neues Modell erstellen. |
| Modellierung | Nutzer können ein kleines Klassenmodell mit Klassen, Attributen, Operationen, Assoziationen, Rollen und Multiplizitäten erstellen. |
| OCL | Nutzer können einfache Invarianten mit Kontextklasse und OCL-Ausdruck erfassen. |
| Snapshot | Nutzer können Objekte, Attributwerte und Objektlinks auf Basis des Klassenmodells erstellen. |
| Validierung | Das Backend prüft UML-Struktur, Multiplizitäten und OCL-Invarianten. |
| Fehlerverständnis | Das Frontend zeigt Fehler im Diagramm und im Validation Results Panel. |
| Speicherung | Das Projekt kann im MVP-JSON-Format gespeichert und geladen oder importiert/exportiert werden. |

Der MVP ist erfolgreich, wenn ein Nutzer ein kleines Library-ähnliches Modell modellieren, einen fehlerhaften Snapshot erzeugen, `Check Constraints` ausführen, den Fehler verstehen, korrigieren und erneut validieren kann.

## Vertikaler MVP-Workflow

Der MVP-Workflow orientiert sich an den Screenshots und an den Kernkonzepten des originalen USE-Projekts:

```text
Dashboard öffnen
-> Start Project wählen
-> Projektname eingeben
-> neues Projekt anlegen oder bestehendes MVP-JSON-Projekt öffnen
-> optional: über View all die vollständige Projektliste öffnen
-> bei Open Existing lokale .use-Datei über Importmodal auswählen
-> Klassendiagramm öffnen
-> Klassen erstellen
-> Attribute und Operationensignaturen erfassen
-> Assoziationen mit Rollen und Multiplizitäten erstellen
-> OCL-Invarianten definieren
-> Objektdiagramm öffnen
-> Objekte erzeugen
-> Attributwerte setzen
-> Objektlinks erzeugen
-> Check Constraints ausführen
-> Validation Results lesen
-> fehlerhafte Objekte oder Links im Diagramm erkennen
-> Snapshot oder Modell korrigieren
-> erneut validieren
```

Der MVP muss diesen Ablauf nicht für große Modelle, vollständige OCL-Sprache oder alle UML-Diagrammarten unterstützen. Er muss aber den Ablauf fachlich vollständig und testbar demonstrieren.

## MVP-Pflichtumfang

### Dashboard und Projektstart

- [x] Startseite / Dashboard anzeigen.
- [x] Logo und Produkttitel `USE` sowie Untertitel `UML-based Specification Environment` anzeigen.
- [x] Primäre Karte `Create New Model` anzeigen.
- [x] Button `+ Start Project` bereitstellen.
- [x] Nach `Start Project` einen Create-New-Project-Dialog oder ein entsprechendes Formular anzeigen.
- [x] Projektname als Pflichtfeld erfassen.
- [x] Neues Projekt erst nach gültigem Projektnamen über `POST /api/v1/projects` anlegen.
- [x] Nach Projektstart automatisch in die Class Diagram View wechseln.
- [x] Karte `Open Existing` sichtbar machen.
- [x] Open-Existing-Modal für lokale `.use`-Dateien als sichtbaren Import-Einstieg berücksichtigen.
- [x] Einstieg für bestehende MVP-JSON-Projekte bereitstellen.
- [x] `View all` als Einstieg in eine vollständige Projektliste berücksichtigen.
- [x] `Learn & Support` mit mindestens sichtbaren Einträgen für `Documentation` und `Examples` anzeigen.

Entscheidung zu `.use` Import im MVP:

| Thema | Entscheidung |
|---|---|
| MVP-Projektformat | JSON-basiertes Projektformat ist verbindlich für den MVP. |
| `.use` Import UI | Durch `14-open-existing-project.png` MVP-nah sichtbar: `Open Existing` öffnet ein Modal für lokale `.use`-Dateien. |
| `.use` Import Verarbeitung | Should im MVP: Dateiinhalt als Modelltext lesen und über den unterstützten Model-Text-/UML-/OCL-Subset anwenden. |
| vollständige `.use` Kompatibilität | Nicht Pflicht im MVP. Vollständiges Mapping aller USE-Konstrukte bleibt Post-MVP. |
| Begründung | Der Screenshot macht den Importflow produktrelevant, aber vollständiger `.use` Import würde Parser-, Mapping- und Kompatibilitätsfragen öffnen und den MVP über den vertikalen Kernworkflow hinaus vergrößern. |

### Projekt und Persistenz

- [x] Projekt erstellen oder ein vorhandenes Projekt laden.
- [x] Projekt im einfachen MVP-JSON-Format speichern.
- [x] Projekt im einfachen MVP-JSON-Format laden oder importieren.
- [x] Projektzustand enthält Klassenmodell, Invarianten, Snapshot und UI-relevante Diagrammdaten.
- [x] Ein vorbereitetes Beispielmodell kann für Demo und Tests genutzt werden.

### Klassendiagramm

- [x] Klassen erstellen.
- [x] Klassenname bearbeiten.
- [x] Klassen löschen oder kontrolliert ablehnen, wenn Abhängigkeiten bestehen.
- [x] Attribute erstellen.
- [x] Attribute bearbeiten.
- [x] Attribute mit primitiven Typen typisieren.
- [x] Primitive Typen im MVP: `String`, `Integer`, `Real`, `Boolean`.
- [x] Operationen als einfache Signaturen erfassen.
- [x] Assoziationen zwischen Klassen erstellen.
- [x] Assoziationsnamen erfassen.
- [x] Source- und Target-Klasse erfassen.
- [x] Rollen an Association Ends erfassen.
- [x] Multiplizitäten an Association Ends erfassen.
- [x] Klassendiagramm mit Klassen, Attributen, Operationen, Assoziationen und Invariantenreferenzen anzeigen.

### Objektdiagramm und Snapshot

- [x] Zwischen Class Diagram und Object Diagram wechseln.
- [x] Objektinstanzen für vorhandene Klassen erzeugen.
- [x] Objektname und Objekttyp anzeigen.
- [x] Attributwerte als Slots anzeigen.
- [x] Attributwerte setzen und bearbeiten.
- [x] Slot-Werte gegen primitive Attributtypen prüfen.
- [x] Objektlinks auf Basis existierender Assoziationen erzeugen.
- [x] Objektlinks anzeigen.
- [x] Objektlinks löschen oder ändern.
- [x] Snapshot mit Objekten, Slots und Links speichern.

### OCL und Invarianten

- [x] Invarianten mit Kontextklasse erstellen.
- [x] Invariantennamen erfassen.
- [x] OCL-Ausdruck speichern.
- [x] Invarianten im Explorer anzeigen.
- [x] Invarianten an der Kontextklasse im Klassendiagramm referenzieren.
- [x] OCL Editor als eigene Hauptansicht bereitstellen.
- [x] Textuellen Modell-/OCL-Editor mit Zeilennummern, `Apply Changes` und Console bereitstellen.
- [x] OCL-Diagnosen im OCL Editor anzeigen, wenn Backend-Parse-/Typecheck-Ergebnisse vorliegen.
- [x] OCL-Ausdruck syntaktisch prüfen.
- [x] OCL-Ausdruck gegen das Klassenmodell typprüfen.
- [x] OCL-Ausdruck gegen einen Snapshot auswerten.
- [x] OCL-Verarbeitung über eine erweiterbare Pipeline konzipieren.

### Validierung

- [x] `Check Constraints` als zentrale Nutzeraktion bereitstellen.
- [x] UML-Struktur des Snapshots prüfen.
- [x] Objektlinks gegen Association-End-Typen prüfen.
- [x] Objektlinks gegen Multiplizitäten prüfen.
- [x] OCL-Invarianten gegen alle Objekte der Kontextklasse prüfen.
- [x] Gesamtstatus der Validierung liefern.
- [x] Strukturierte Validation Results erzeugen.
- [x] Betroffene Objekte, Links, Invarianten oder Modellbestandteile referenzieren.

### Fehlerdarstellung

- [x] Validation Results Panel anzeigen.
- [x] Anzahl der Fehler anzeigen.
- [x] Fehlertyp oder Fehlercode anzeigen.
- [x] Fachliche Fehlermeldung anzeigen.
- [x] Kontextklasse und Invariante anzeigen, wenn relevant.
- [x] OCL-Ausdruck oder Constraint-Referenz anzeigen, wenn relevant.
- [x] Fehlerhafte Objekte im Objektdiagramm visuell markieren.
- [x] Link- oder Multiplizitätsfehler auf Links oder beteiligte Objekte zurückführen.
- [x] Fehler nach erneuter Validierung aktualisieren.

### UI-Grundstruktur

- [x] Top Navigation mit mindestens `Class Diagram`, `Object Diagram` und optional sichtbarem `OCL Editor`.
- [x] Explorer Sidebar für Klassen, Assoziationen, Invarianten, Objekte und Objektlinks.
- [x] Diagram Canvas für Klassen- und Objektdiagramm.
- [x] Properties Panel für selektierte Klassen, Assoziationen, Invarianten, Objekte und Objektlinks.
- [x] Modals für mindestens Klasse, Invariante, Klassenassoziation und Objektassoziation.
- [x] Open-Existing-Modal für lokale `.use`-Dateien aus dem Dashboard.
- [x] Globaler Button `Check Constraints`.
- [x] Bottom Panel oder vergleichbarer Bereich für Validation Results.

## MVP-Optionalumfang

Diese Punkte sind hilfreich, aber nicht zwingend für den ersten MVP. Sie können umgesetzt werden, falls sie ohne deutliche Scope-Ausweitung möglich sind.

| Optionaler Umfang | Nutzen | Grenze |
|---|---|---|
| Recent Projects | Verbessert den Dashboard-Einstieg. | Im MVP dürfen Recent Projects Mock- oder Demo-Daten sein, wenn echte Persistenz noch nicht vollständig ist. |
| All Projects / Projektliste | Nutzer können über `View all` alle Projekte sehen, suchen und öffnen. | Should: Projektliste kann zunächst einfache Backend-Daten oder Demo-Daten nutzen; serverseitige Suche, Filter und Pagination sind Post-MVP. |
| `.use` Import als lokaler Datei-Flow | Macht den USE-Bezug im Dashboard sichtbar und erlaubt Importversuche mit Beispieldateien aus `examples/`. | MVP verarbeitet nur unterstützten Modelltext-Subset; vollständige `.use` Kompatibilität bleibt Post-MVP. |
| Console Panel | Macht Laden, Speichern und Validierung nachvollziehbar. | Keine interaktive Shell im MVP erforderlich. |
| Projektmetadaten | Projektname und Beschreibung verbessern Orientierung. | Keine komplexe Projektverwaltung. |
| Attribute löschen | Wichtig für Modellpflege. | Abhängigkeiten dürfen zunächst mit einfacher Warnung behandelt werden. |
| Operationen bearbeiten/löschen | Macht Klassenelemente konsistenter editierbar. | Keine Operation Bodies. |
| Association Labels mit Rollen und Multiplizitäten | Verbessert Lesbarkeit im Diagramm. | Muss nicht pixelgenau wie spätere UI sein. |
| Navigation von Fehler zu Element | Beschleunigt Fehlerbehebung. | Fehler-Markierung im Diagramm bleibt Pflicht. |
| OCL-Feedback beim Bearbeiten | Frühes Feedback für Syntax und Typechecking. | Screenshot `13` macht den textbasierten OCL-/Modell-Editor mit `Apply Changes` und Diagnosefähigkeit MVP-relevant; Live-Debounce bleibt optional. |
| Versioniertes JSON-Format | Erleichtert spätere Änderungen. | Keine komplexe Migration im MVP. |
| Reduziertes USE-Beispielmodell als Demo | Gute Test- und Präsentationsgrundlage. | Keine vollständige `.use`-Kompatibilität. |

## Nicht im MVP

Folgende Punkte sind ausdrücklich nicht Teil des MVP:

- [ ] vollständiges OCL,
- [ ] vollständige USE-Feature-Parität,
- [ ] direkte Nutzung des USE-Cores,
- [ ] Fork oder technische Migration des originalen USE-Projekts,
- [ ] alte USE-Desktop-GUI,
- [ ] Sequenzdiagramme,
- [ ] State Machines,
- [ ] Aktivitätsdiagramme,
- [ ] Komponentendiagramme,
- [ ] Deploymentdiagramme,
- [ ] vollständige Modellanimation,
- [ ] ausführbare Operationen,
- [ ] Preconditions und Postconditions,
- [ ] derived attributes,
- [ ] init values,
- [ ] Vererbung,
- [ ] Enumerationen,
- [ ] Aggregation und Komposition,
- [ ] Assoziationsklassen,
- [ ] mehrere Snapshots,
- [ ] Kollaboration,
- [ ] komplexe Projektversionierung,
- [ ] Datenbankpersistenz als Pflicht,
- [ ] `.use` Import als Pflicht,
- [ ] `.use` Export als Pflicht,
- [ ] OCL Autocomplete als Pflicht,
- [ ] Undo/Redo als Pflicht,
- [ ] Codegenerierung.

Diese Abgrenzung ist wichtig, damit der MVP ein prüfbarer vertikaler Durchstich bleibt und nicht zu einer vollständigen USE-Neuimplementierung anwächst.

## Post-MVP-Erweiterungen

| Erweiterung | Beschreibung | Bezug zum MVP |
|---|---|---|
| Erweiterte OCL-Collections | `forAll`, `exists`, `select`, `collect`, `includes`, `excludes`. | Baut auf Parser, AST, Typechecker und Evaluator auf. |
| Bedingte OCL-Ausdrücke | `if-then-else`. | Erweitert AST und Evaluator. |
| Lokale Bindungen | `let`. | Erfordert erweiterte Scope- und Typregeln. |
| `allInstances` | Zugriff auf alle Objekte einer Klasse. | Benötigt Snapshot-weite Evaluation. |
| Preconditions/Postconditions | Constraints für Operationen. | Operationen müssen über Signaturen hinaus fachlich erweitert werden. |
| Derived Attributes | Berechnete Attribute. | Nutzt OCL-Auswertung außerhalb reiner Invarianten. |
| Init Values | Initialisierung von Slots. | Erweitert Objektanlage und Snapshot-Logik. |
| Vererbung | Generalisierung zwischen Klassen. | Erweitert Typechecking, Diagramm und Objektvalidierung. |
| Enumerationen | Domänenspezifische Aufzählungstypen. | Erweitert Typmodell und Slot-Editoren. |
| Aggregation/Komposition | Spezialisierte Association Semantik. | Baut auf Association Ends auf. |
| Assoziationsklassen | Beziehungen mit eigenen Attributen. | Erweitert Klassen- und Objektmodell deutlich. |
| Mehrere Snapshots | Verwaltung mehrerer Objektzustände. | Baut auf einem stabilen Snapshot-Modell auf. |
| `.use` Import | Einlesen ausgewählter USE-Modelle. | Nutzt Syntaxanalyse aus Originalreferenz. |
| `.use` Export | Ausgabe USE-naher Modelle. | Benötigt stabiles internes Projektformat. |
| OCL Syntax Highlighting | Bessere OCL-Erfassung im Frontend. | Verbessert UI, ändert nicht den fachlichen Kern. |
| OCL Autocomplete | Unterstützung beim Schreiben von Ausdrücken. | Nutzt Modell- und Typechecker-Informationen. |
| Undo/Redo | Modelländerungen rückgängig machen. | Erfordert Command- oder State-History-Konzept. |
| Projektversionierung | Nachvollziehbare Modellhistorie. | Baut auf Persistenz und Projektformat auf. |
| Datenbankpersistenz | Serverseitige Projektspeicherung. | Ersetzt oder ergänzt JSON-Dateiablage. |
| Erweiterte Projektliste | Suche, Filter, Sortierung, Pagination, Thumbnails und nutzerspezifische Projektverwaltung. | Baut auf `GET /api/v1/projects` und Project Summaries auf. |
| Testfallbibliothek | Sammlung von Beispiel- und Regressionstests. | Kann aus USE-Beispielen abgeleitet werden. |

## MVP-OCL-Subset

Das MVP unterstützt ein bewusst kleines OCL-Subset. Es soll echte Parsing-, Typechecking- und Evaluation-Fähigkeit demonstrieren, aber keine vollständige OCL-Implementierung liefern.

| OCL-Bestandteil | MVP-Status | Beispiel | Hinweis |
|---|---|---|---|
| Kontextklasse | Muss | `context User inv maxBooks` | Kontext wird in der UI separat gespeichert oder aus Invariantendaten abgeleitet. |
| `self` | Muss | `self` | Bezieht sich auf das aktuell geprüfte Objekt der Kontextklasse. |
| Attributzugriff | Muss | `self.books` | Attribut muss in der Kontextklasse existieren. |
| Einfache Navigation | Muss | `self.borrowedBooks` | Navigation über eine Rolle einer binären Association. |
| String-Literale | Muss | `'Alice'` | Schreibweise ist im Detail noch festzulegen. |
| Integer-Literale | Muss | `6` | Für Zählwerte und Vergleiche. |
| Real-Literale | Muss | `12.5` | Für numerische Attribute. |
| Boolean-Literale | Muss | `true`, `false` | Für boolesche Attribute und Ausdrücke. |
| Vergleichsoperatoren | Muss | `self.books <= 5` | `=`, `<>`, `<`, `<=`, `>`, `>=`. |
| Boolean-Operatoren | Muss | `self.active and self.books <= 5` | Mindestens `and`, `or`, `not`. |
| Klammern | Muss | `(self.books <= 5) and self.active` | Für eindeutige Gruppierung. |
| `size` | Muss | `self.borrowedBooks->size() <= 5` | Wichtig für Association Navigation. |
| `isEmpty` | Muss | `self.borrowedBooks->isEmpty()` | Collection-Grundoperation. |
| `notEmpty` | Muss | `self.borrowedBooks->notEmpty()` | Collection-Grundoperation. |

Nicht Teil des MVP-OCL-Subsets:

- `forAll`,
- `exists`,
- `select`,
- `collect`,
- `includes`,
- `excludes`,
- `let`,
- `if-then-else`,
- `allInstances`,
- `@pre`,
- Operation Calls mit ausführbarer Semantik,
- Tuple,
- komplexe Collection-Typen,
- vollständige Undefined-/Invalid-Semantik.

Die MVP-OCL-Architektur soll dennoch auf folgender Pipeline beruhen:

```text
OCL Text
-> Lexer
-> Parser
-> AST
-> Type Checker
-> Evaluator
-> Validation Result
```

Regex-basierte Einzelregeln reichen für den MVP nicht aus, weil spätere Erweiterungen sonst kaum kontrolliert möglich wären.

## MVP-Validierung

Die MVP-Validierung besteht aus mehreren logisch getrennten Schritten.

| Schritt | Inhalt | MVP-Erwartung |
|---|---|---|
| Projektstruktur prüfen | Existieren referenzierte Klassen, Attribute, Assoziationen und Invarianten? | Muss |
| Snapshot-Struktur prüfen | Passen Objekte, Slots und Links zum Klassenmodell? | Muss |
| Slot-Typen prüfen | Passen Attributwerte zu `String`, `Integer`, `Real`, `Boolean`? | Muss |
| Link-Typen prüfen | Verbinden Objektlinks zulässige Objekte für die gewählte Association? | Muss |
| Multiplizitäten prüfen | Erfüllen Objektlinks die Association-End-Multiplizitäten? | Muss |
| OCL parsen | Sind Invarianten syntaktisch gültig? | Muss |
| OCL typprüfen | Passen Attribute, Rollen und Operatoren zum Modell? | Muss |
| OCL evaluieren | Wird jede Invariante für jede relevante Objektinstanz erfüllt? | Muss |
| Ergebnis strukturieren | Fehler enthalten Typ, Nachricht, betroffene Elemente und Constraint-Bezug. | Muss |

MVP-Fehlertypen:

| Fehlercode | Bedeutung | Beispiel |
|---|---|---|
| `MODEL_STRUCTURE_ERROR` | Modellreferenz oder Modellstruktur ist ungültig. | Assoziation referenziert fehlende Klasse. |
| `SNAPSHOT_STRUCTURE_ERROR` | Snapshot passt nicht zum Klassenmodell. | Objekt besitzt Slot ohne passendes Attribut. |
| `TYPE_ERROR` | Wert oder OCL-Ausdruck ist typinkonsistent. | `self.books = 'six'`. |
| `MULTIPLICITY_VIOLATION` | Linkanzahl verletzt Multiplizität. | User hat zu viele Borrow-Links. |
| `OCL_SYNTAX_ERROR` | Invariante kann nicht geparst werden. | Fehlende Klammer. |
| `OCL_TYPE_ERROR` | OCL-Ausdruck ist semantisch falsch. | Unbekannte Rolle in Navigation. |
| `INVARIANT_VIOLATION` | OCL-Invariante liefert `false`. | `self.borrowedBooks->size() <= 5` ist verletzt. |
| `EVALUATION_ERROR` | Ausdruck kann zur Laufzeit nicht ausgewertet werden. | Nicht unterstützter Ausdruck oder fehlender Wert. |

## MVP-UI

Die MVP-UI orientiert sich strukturell an den vorhandenen Screenshots. Sie muss nicht pixelgenau identisch sein, soll aber dieselben zentralen Arbeitsbereiche unterstützen.

| UI-Bereich | MVP-Pflicht | Screenshot-Bezug |
|---|---|---|
| Top Navigation | Wechsel zwischen `Class Diagram`, `Object Diagram`, `OCL Editor`. | `01`, `06`, `13` |
| Explorer Sidebar | Listen für Klassen, Assoziationen, Invarianten, Objekte und Objektlinks. | `01`, `03`, `06`, `07` |
| Diagram Canvas | Darstellung und Selektion von Klassen, Objekten, Associations und Links. | `01`, `04`, `06`, `07`, `12` |
| Properties Panel | Bearbeitung des selektierten Elements. | `01`, `02`, `03`, `06`, `12` |
| Add Class Modal | Klasse erstellen. | `08` |
| Add Invariant Modal | Invariante mit Kontextklasse und OCL-Ausdruck erstellen. | `09` |
| OCL Editor View | Textbasierten USE-/OCL-Modelltext anzeigen, bearbeiten, über `Apply Changes` übernehmen und Diagnosen anzeigen. | `13` |
| Add Class Association Modal | Klassenassoziation erstellen. | `10` |
| Add Object Association Modal | Objektlink erstellen. | `11` |
| All Projects Page | Vollständige Projektliste mit Suche, Filter, Projektkarten, `Open` und `+ New Project`. | `19` |
| Check Constraints Button | Validierung auslösen. | `01`, `06`, `07` |
| Validation Results Panel | Fehler textuell anzeigen. | `07` |
| Diagram Error Highlighting | Fehlerhafte Objekte oder Links markieren. | `07` |

MVP-UI-Checkliste:

- [x] Selektion im Canvas aktualisiert Properties Panel.
- [x] Selektion im Explorer aktualisiert Canvas und Properties Panel.
- [x] Erstellte Elemente erscheinen ohne manuelles Neuladen in Explorer und Canvas.
- [x] `Check Constraints` ist in relevanten Ansichten erreichbar.
- [x] Fehler sind gleichzeitig im Validation Results Panel und im Diagramm nachvollziehbar.
- [x] Korrektur und erneute Validierung entfernen veraltete Fehlerzustände.

## MVP-Backend

Das MVP-Backend ist fachlich autoritativ für Modell, Snapshot, OCL und Validierung.

| Backend-Bereich | MVP-Pflicht |
|---|---|
| Projektmodell | Eigenes Domänenmodell für Projekt, Klassenmodell, Invarianten und Snapshot. |
| Klassenmodell-Service | Verwaltung von Klassen, Attributen, Operationensignaturen, Assoziationen, Rollen und Multiplizitäten. |
| Objektmodell-Service | Verwaltung von Objekten, Slots und Objektlinks. |
| OCL Engine | Lexer, Parser, AST, Type Checker und Evaluator für MVP-OCL-Subset. |
| Validation Service | Strukturprüfung, Multiplizitätsprüfung und Invariantenauswertung. |
| Error Model | Strukturierte Fehler mit Codes, Meldungen, Severity und betroffenen Element-IDs. |
| Projektformat | JSON Save/Load oder Import/Export für MVP-Projekte. |

Backend-Pflichtentscheidungen:

- [x] Fachliche Semantik liegt im Backend, nicht nur im Frontend.
- [x] USE-Core wird nicht als Dependency verwendet.
- [x] Original-USE-Konzepte werden fachlich nachgebildet, nicht technisch kopiert.
- [x] Validierungsergebnisse werden als strukturierte Daten geliefert.
- [x] Frontend muss Fehler nicht aus Textausgaben parsen.

## MVP-API

Die API muss den vertikalen Workflow vollständig tragen.

| API-Bereich | MVP-Pflicht |
|---|---|
| Projekt laden/speichern | Projektzustand lesen und schreiben. |
| Klassenmodell bearbeiten | CRUD-Operationen für Klassen, Attribute, Operationensignaturen und Assoziationen. |
| Invarianten bearbeiten | CRUD-Operationen für Invarianten. |
| Snapshot bearbeiten | CRUD-Operationen für Objekte, Slots und Objektlinks. |
| OCL prüfen | Syntax- und Typecheck-Ergebnisse für OCL-Ausdrücke bereitstellen. |
| Constraints prüfen | Vollständigen Validation Check ausführen. |
| Validation Results liefern | Gesamtstatus und Fehlerliste mit Elementbezug zurückgeben. |

MVP-DTOs müssen mindestens folgende Konzepte ausdrücken:

- Projekt,
- Klasse,
- Attribut,
- Operationensignatur,
- Assoziation,
- Association End,
- Multiplizität,
- Invariante,
- Snapshot,
- Objekt,
- Slot,
- Objektlink,
- Validation Result,
- Validation Error.

Stabile IDs sind Pflicht, damit Frontend und Backend dieselben Elemente referenzieren können.

## MVP-Akzeptanzlogik

Der MVP gilt als fachlich akzeptiert, wenn die folgenden End-to-End-Szenarien funktionieren.

### Szenario 1: Gültiges Modell und gültiger Snapshot

| Schritt | Erwartung |
|---|---|
| Projekt erstellen oder Beispielmodell laden | Klassendiagramm ist sichtbar. |
| Klasse `User` und Klasse `Book` modellieren | Beide Klassen erscheinen im Canvas und Explorer. |
| Attribute erfassen | Attribute werden mit Typ angezeigt. |
| Association `Borrows` mit Rollen und Multiplizitäten erfassen | Association erscheint im Klassendiagramm. |
| Invariante erfassen | Invariante ist der Kontextklasse zugeordnet. |
| Objekte und Link erstellen | Objektdiagramm zeigt Instanzen und Link. |
| Attributwerte setzen | Slots enthalten typgültige Werte. |
| `Check Constraints` ausführen | Validation Result ist `VALID`. |

### Szenario 2: Verletzte OCL-Invariante

| Schritt | Erwartung |
|---|---|
| Snapshot so verändern, dass Invariante verletzt wird | Daten bleiben speicherbar, aber invalidierbar. |
| `Check Constraints` ausführen | Validation Result ist `INVALID`. |
| Validation Results öffnen | Fehler nennt Invariante, Kontextklasse und betroffenes Objekt. |
| Objektdiagramm betrachten | Betroffenes Objekt ist visuell markiert. |
| Fehler korrigieren | Slot oder Objektlinks werden angepasst. |
| Erneut validieren | Fehler verschwindet oder Ergebnis wird aktualisiert. |

### Szenario 3: Multiplizitätsverletzung

| Schritt | Erwartung |
|---|---|
| Association mit Multiplizität definieren | Multiplizität ist im Modell gespeichert. |
| Snapshot mit zu wenigen oder zu vielen Links erzeugen | Snapshot kann validiert werden. |
| `Check Constraints` ausführen | Fehler `MULTIPLICITY_VIOLATION` wird erzeugt. |
| Fehler anzeigen | Ergebnis nennt Association, Rollenende und betroffene Objekte oder Links. |
| Diagramm markieren | Betroffene Links oder Objekte werden hervorgehoben. |

### Szenario 4: OCL-Syntax- oder Typecheck-Fehler

| Schritt | Erwartung |
|---|---|
| Invariante mit fehlerhaftem OCL-Ausdruck erfassen | System speichert oder blockiert entsprechend der festgelegten UX-Regel. |
| OCL prüfen oder `Check Constraints` ausführen | Fehler wird als OCL-Fehler klassifiziert. |
| Validation Results anzeigen | Fehler enthält Invariante, Meldung und möglichst Ausdrucksposition. |
| Ausdruck korrigieren | Danach kann die Invariante ausgewertet werden. |

## Abgrenzung zum originalen USE-Projekt

Das originale USE-Projekt ist für den MVP eine fachliche Referenz, aber keine technische Grundlage.

| Bereich | Original-USE | MVP des neuen Systems |
|---|---|---|
| Technische Basis | Java-Core mit Desktop-GUI und Shell. | Neues Backend und neues Webfrontend. |
| Modellkonzepte | Klassen, Attribute, Operationen, Assoziationen, Multiplizitäten, Invarianten, Objekte, Links. | Fachlich vergleichbare Kernkonzepte, technisch neu modelliert. |
| OCL | Umfangreiche OCL-Unterstützung. | Kleines OCL-Subset mit erweiterbarer Architektur. |
| Persistenz/Syntax | `.use` und `.cmd` Dateien. | MVP-JSON-Format; `.use` Import/Export später. |
| Validierung | Textorientierte Ausgaben und interne USE-Strukturen. | Strukturierte Validation Results für Web-UI. |
| GUI | Swing/JavaFX Desktop-UI. | React/TypeScript-Weboberfläche nach neuen Screenshots. |
| Testquellen | Beispiele und Tests im Originalprojekt. | Als fachliche Testfallquelle nutzbar, keine Codeübernahme. |

Für den MVP gilt:

- USE-Konzepte dürfen fachlich analysiert und nachgebildet werden.
- USE-Beispiele dürfen als Vorlage für eigene Demo- und Testmodelle dienen.
- USE-Verhalten darf als Referenz für Validierung und OCL-Auswertung dienen.
- USE-Code, USE-Core, USE-GUI und USE-Buildsystem werden nicht übernommen.

## Zusammenfassung

Der MVP ist ein kontrollierter vertikaler Durchstich durch das Zielsystem:

- neues UML-Klassenmodell,
- neuer Snapshot im Objektdiagramm,
- einfache OCL-Invarianten,
- Backend-basierte Constraint Validation,
- strukturierte Validation Results,
- visuelle Fehlerdarstellung,
- JSON-basiertes Speichern und Laden.

Er ist bewusst kleiner als USE und bewusst kleiner als das langfristige Zielsystem. Entscheidend ist nicht vollständige Feature-Parität, sondern ein funktionierender, erweiterbarer Kern für UML/OCL-Modellierung und Validierung im Web.
