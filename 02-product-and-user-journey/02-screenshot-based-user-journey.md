# Screenshot-Based User Journey

## Zweck dieser Datei

Diese Datei leitet aus den vorhandenen UI-Screenshots eine konkrete User Journey für das neue UML/OCL-Websystem ab.

Sie beschreibt:

- welche Schritte ein Nutzer im System ausführt,
- welche Screenshots welchen Schritt belegen,
- welche Nutzeraktion je Schritt stattfindet,
- welche Systemreaktion erwartet wird,
- welche fachlichen Anforderungen daraus entstehen,
- welche UI-Anforderungen daraus entstehen,
- wie der Ablauf vom Dashboard über Projektstart ins Klassendiagramm, Objektdiagramm und zur Validierung führt.

Hinweis zur Ablage: Alle berücksichtigten Screenshots liegen unter `assets/screenshots/`. Die Links in dieser Datei verwenden diese normalisierten Pfade.

## Überblick über den Workflow

Die Screenshots zeigen einen vollständigen vertikalen Arbeitsablauf. Der Workflow startet nicht direkt im Klassendiagramm, sondern auf dem Dashboard:

```text
Dashboard öffnen
-> neues Modell starten, Open Existing Project Modal öffnen oder Recent Project auswählen
-> bei neuem Modell: Projektname eingeben
-> optional über View all die vollständige Projektliste öffnen
-> lokale .use-Datei optional auswählen/importieren
-> Klassendiagramm betrachten
-> Klassen, Attribute, Operationen bearbeiten
-> Assoziationen modellieren
-> OCL-Invarianten definieren
-> Objektdiagramm öffnen
-> Objekte und Objektlinks bearbeiten
-> Check Constraints ausführen
-> Fehler im Diagramm und Validation Panel verstehen
-> Modell oder Snapshot korrigieren
-> erneut validieren
```

Der sichtbare Produktkern besteht aus vier Arbeitsbereichen:

| Bereich | Rolle in der Journey |
|---|---|
| Explorer Sidebar | Navigation durch Klassen, Assoziationen, Invarianten, Objekte und Objektlinks. |
| Diagram Canvas | Visuelle Modellierung von Klassen- und Objektdiagrammen. |
| Properties Panel | Bearbeitung des aktuell ausgewählten Elements. |
| Console / Validation Results | Protokoll und Validierungsergebnisse. |

Zusätzlich gibt es zentrale globale Aktionen:

- Wechsel zwischen `Class Diagram`, `Object Diagram` und `OCL Editor`,
- `Check Constraints`,
- Refresh,
- Save,
- Search/Command Palette,
- kontextbezogene Quick Help.

## Screenshot-Index

| Screenshot | Inhalt | Primäre Journey-Relevanz |
|---|---|---|
| `00-dashboard-start-page.png` | Dashboard / Start Page mit `Create New Model`, `Open Existing`, Recent Projects und Learn & Support. | Anwendung öffnen, Projektstart, Import, Recent Projects, Dokumentation und Beispiele. |
| `18-create-new-projects.png` | Create New Project Dialog/Formular mit Projektname. | Neues Projekt mit explizitem Namen anlegen, bevor ins Klassendiagramm gewechselt wird. |
| `14-open-existing-project.png` | Open Existing Project Modal mit lokaler `.use`-Dateiauswahl. | Bestehende USE-Spezifikation aus `examples/` oder lokaler Datei öffnen/importieren. |
| `19-projects.png` | All Projects mit Suche, Filter, `Open`, `New Project` und Projektkarten. | Vollständige Projektliste öffnen, Projekte suchen, öffnen oder neues Projekt starten. |
| `01-class-diagram-class-properties.png` | Klassendiagramm mit selektierter Klasse und Class Properties. | Klassendiagramm betrachten, Klasse bearbeiten. |
| `02-class-diagram-association-properties.png` | Klassendiagramm mit Association Properties. | Assoziation betrachten und bearbeiten. |
| `03-class-diagram-invariant-properties.png` | Klassendiagramm mit Invariant Properties und OCL-Ausdruck. | Invariante betrachten und bearbeiten. |
| `04-class-diagram-new-class-selected.png` | Neue Klasse im Canvas selektiert. | Ergebnis nach Klasse erstellen. |
| `06-object-diagram-object-properties.png` | Objektdiagramm mit selektiertem Objekt und Slots. | Objekt betrachten und bearbeiten. |
| `07-object-diagram-validation-error.png` | Objektdiagramm mit Validierungsfehler. | Constraints prüfen, Fehler verstehen. |
| `08-modal-add-class.png` | Modal zum Erstellen einer Klasse. | Klasse erstellen. |
| `09-modal-add-invariant.png` | Modal zum Erstellen einer Invariante. | Invariante erstellen. |
| `10-modal-add-class-association.png` | Modal zum Erstellen einer Klassenassoziation. | Association erstellen. |
| `11-modal-add-object-association.png` | Modal zum Erstellen eines Objektlinks. | Objektassoziation erstellen. |
| `12-object-diagram-association-properties.png` | Objektdiagramm mit Association Properties. | Objektlink betrachten und bearbeiten. |
| `13-ocl-editor.png` | OCL Editor mit textuellem USE-/OCL-Modell und Console. | Modelltext bearbeiten, Änderungen anwenden, Constraints prüfen. |

## Journey-Schritt 0: Anwendung öffnen und Dashboard sehen

![Dashboard Start Page](../assets/screenshots/00-dashboard-start-page.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `00-dashboard-start-page.png` |
| Nutzeraktion | Der Nutzer öffnet die Webanwendung. |
| Systemreaktion | Das Dashboard wird angezeigt. Es zeigt Logo, Produkttitel `USE`, Untertitel `UML-based Specification Environment`, Benutzeravatar, Projektstartkarten, Recent Projects und Support-Links. |
| Sichtbare Hinweise | `Create New Model`, Button `+ Start Project`, `Open Existing`, `.use`-Importhinweis, `Recent Projects`, `View all`, `Documentation`, `Examples`. |
| Fachliche Anforderung | Die Anwendung braucht einen expliziten Projektstart vor den Diagramm-Views. |
| UI-Anforderung | Dashboard muss den Nutzer schnell zu einem neuen oder bestehenden Projekt führen. |

## Journey-Schritt 1: Projekt starten, öffnen oder importieren

![Dashboard Start Page](../assets/screenshots/00-dashboard-start-page.png)

| Option | Nutzeraktion | Erwartete Systemreaktion | MVP-Einschätzung |
|---|---|---|---|
| Neues Modell erstellen | Klick auf `+ Start Project`. | Create-New-Project-Dialog/Formular öffnet sich; Nutzer gibt einen Projektnamen ein. | MVP |
| Bestehendes Projekt öffnen | Klick auf `Open Existing`. | Import-/Dateiauswahl- oder Projektöffnen-Flow startet. | MVP für JSON, Should/Post-MVP für `.use` |
| Recent Project öffnen | Klick auf `University System`, `Hotel Management` oder `Bank ATM`. | Projekt wird geladen und ins Klassendiagramm geöffnet. | Should |
| Alle Projekte anzeigen | Klick auf `View all`. | Projektliste aus `19-projects.png` wird geöffnet. | Should |
| Documentation öffnen | Klick auf `Documentation`. | Dokumentation oder Hilfe wird geöffnet. | Should/Later |
| Examples öffnen | Klick auf `Examples`. | Beispielmodelle werden angezeigt oder geladen. | Should |

Nach Auswahl eines Projekts führt die Journey in die Modellierungsoberfläche:

```text
Dashboard
-> Start Project / Open Existing / Recent Project
-> bei Start Project: Projektname erfassen und Projekt erstellen
-> Class Diagram
```

### Journey-Schritt 1a: Neues Projekt mit Namen erstellen

![Create New Project](../assets/screenshots/18-create-new-projects.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `18-create-new-projects.png` |
| Nutzeraktion | Der Nutzer klickt `+ Start Project`, gibt einen Projektnamen ein und bestätigt die Anlage. |
| Systemreaktion | Das Frontend validiert, dass der Name nicht leer ist, sendet `POST /api/v1/projects` mit `CreateProjectRequestDto.name` und navigiert bei Erfolg ins Klassendiagramm. |
| Sichtbare Hinweise | Create-New-Project-Dialog/Formular, Eingabefeld für den Projektnamen, primäre Erstellen-Aktion und Abbrechen/Schließen. |
| Fachliche Anforderung | Jedes neue Projekt besitzt ab Anlage einen nutzerverständlichen Namen. |
| UI-Anforderung | Leere Projektnamen werden direkt am Eingabefeld oder durch deaktivierten Submit verhindert; API-Fehler bleiben im Dialog sichtbar. |
| MVP-Einschätzung | Hoch. Ohne diesen Schritt passt der Dashboard-Startflow nicht zum Screenshot. |

### Journey-Schritt 1b: `.use`-Datei über Open Existing importieren

![Open Existing Project](../assets/screenshots/14-open-existing-project.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `14-open-existing-project.png` |
| Nutzeraktion | Der Nutzer klickt auf dem Dashboard `Open Existing`, wählt im Modal eine lokale `.use`-Datei aus oder zieht sie in die Upload-Fläche und klickt `Open Project`. |
| Systemreaktion | Das Frontend liest die Datei als Text, validiert Dateiendung und leeren Inhalt lokal und sendet den Modelltext an den Import-/Model-Text-Flow des Backends. |
| Sichtbare Hinweise | Modal-Titel `Open Existing Project`, `Local File`, Dropzone, `Supported format: .use`, `Cancel`, `Open Project`. |
| Fachliche Anforderung | `.use`-Dateien aus dem originalen USE-Kontext und aus `examples/` müssen als fachliche Eingabequelle berücksichtigt werden. |
| UI-Anforderung | Importfehler und nicht unterstützte Syntax dürfen nicht stillschweigend scheitern; sie müssen als Diagnose im Modal, OCL Editor, Console oder Validation/Diagnostics Panel erscheinen. |
| MVP-Einschätzung | Der sichtbare Modal-Flow ist MVP-nah. Vollständige USE-Kompatibilität bleibt Post-MVP; der MVP verarbeitet nur den definierten Modelltext-/UML-/OCL-Subset. |

Empfohlener technischer Ablauf:

```text
Dashboard
-> Open Existing
-> Open Existing Project Modal
-> .use-Datei auswählen oder droppen
-> Dateiinhalt als modelText lesen
-> Projekt erstellen oder Import-Endpunkt aufrufen
-> modelText anwenden
-> Diagnosen anzeigen
-> bei Erfolg Class Diagram öffnen
```

### Journey-Schritt 1c: Alle Projekte anzeigen

![All Projects](../assets/screenshots/19-projects.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `19-projects.png` |
| Nutzeraktion | Der Nutzer klickt auf dem Dashboard `View all` oder öffnet direkt die Projektlisten-Route. |
| Systemreaktion | Die Anwendung zeigt `All Projects` mit Back-Navigation, Suche, Filter, `Open`, `+ New Project` und Projektkarten. |
| Sichtbare Hinweise | Projektkarten für `University System`, `Hotel Management`, `Bank ATM`, `E-Commerce System`, `Library Catalog` und `Car Rental` mit Beschreibung und Änderungszeit. |
| Fachliche Anforderung | Nutzer müssen mehr als nur die drei Recent Projects sehen können, sobald mehrere Projekte existieren. |
| UI-Anforderung | Projektkarten zeigen nutzerverständliche Namen statt IDs; Suche und Filter helfen bei wachsender Projektzahl. |
| MVP-Einschätzung | Should. Für einen frühen MVP dürfen Demo-/Mockdaten genutzt werden; bei echter Persistenz sollte `GET /api/v1/projects` genutzt werden. |

Empfohlener Ablauf:

```text
Dashboard
-> View all
-> All Projects
-> Projekt suchen oder filtern
-> Projektkarte klicken
-> GET /api/v1/projects/{projectId}
-> Class Diagram
```

## Journey-Schritt 2: Klassendiagramm nach Projektstart öffnen

![Class Diagram Class Properties](../assets/screenshots/01-class-diagram-class-properties.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `01-class-diagram-class-properties.png` |
| Nutzeraktion | Der Nutzer startet ein neues Projekt, importiert/öffnet ein bestehendes Projekt oder wählt ein Recent Project. |
| Systemreaktion | Das System lädt das Modell, zeigt das Klassendiagramm und protokolliert den Ladevorgang in der Console. |
| Sichtbare Hinweise | Console enthält Einträge wie `Project initialized`, `Loading model 'Library.use'...`, `Model loaded successfully`. |
| Fachliche Anforderung | Ein Projekt enthält Klassenmodell, Invarianten und mindestens einen Snapshot oder die Möglichkeit, einen Snapshot aufzubauen. |
| UI-Anforderung | Nach dem Laden muss ein sinnvoller Startzustand sichtbar sein: Explorer, Canvas, Properties Panel und Console. |

Der erste sichtbare Arbeitszustand zeigt bereits:

- Klassen `Book` und `User`,
- Assoziation `Borrows`,
- Invariante `maxBooks`,
- Klassendiagramm mit zwei Klassen und einer Verbindung,
- globalen Button `Check Constraints`.

## Journey-Schritt 3: Klassendiagramm betrachten

![Class Diagram Class Properties](../assets/screenshots/01-class-diagram-class-properties.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `01-class-diagram-class-properties.png` |
| Nutzeraktion | Der Nutzer betrachtet das Klassendiagramm und wählt eine Klasse aus. |
| Systemreaktion | Die ausgewählte Klasse wird im Canvas blau hervorgehoben. Rechts öffnet sich der `Class`-Tab im Properties Panel. |
| Sichtbare Hinweise | `User` ist selektiert; Attribute und Operationen sind im Klassenelement und im Properties Panel sichtbar. |
| Fachliche Anforderung | Klassen müssen Attribute, Operationen und Invariantenreferenzen anzeigen können. |
| UI-Anforderung | Selektion im Canvas und Properties Panel müssen synchronisiert sein. |

Die Class Diagram View muss mindestens zeigen:

- Klassenname,
- Attribute mit Typ,
- Operationen mit Rückgabetyp,
- Invariantenreferenzen,
- Assoziationslinien mit Namen.

## Journey-Schritt 4: Klasse erstellen

![Add Class Modal](../assets/screenshots/08-modal-add-class.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `08-modal-add-class.png` |
| Nutzeraktion | Der Nutzer klickt im Explorer bei `Classes` auf `+` oder eine vergleichbare Add-Class-Aktion. |
| Systemreaktion | Ein Modal `Add New Class` öffnet sich. Der Nutzer gibt einen Klassennamen ein und bestätigt mit `Create Class`. |
| Sichtbare Felder | `Class Name`, Button `Create Class`, Close-Icon. |
| Fachliche Anforderung | Das System muss neue Klassen mit eindeutigem Namen erzeugen. |
| UI-Anforderung | Der Dialog fokussiert nur die notwendigen Eingaben und blockiert den Hintergrund. |

Erwartete Validierungen:

- Klassenname darf nicht leer sein.
- Klassenname muss im Modell eindeutig sein.
- Klassenname muss eine gültige Modellkennung sein.

## Journey-Schritt 5: Klasseneigenschaften bearbeiten

![New Class Selected](../assets/screenshots/04-class-diagram-new-class-selected.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `04-class-diagram-new-class-selected.png` |
| Nutzeraktion | Nach dem Erstellen bearbeitet der Nutzer die neue Klasse im Properties Panel. |
| Systemreaktion | Die neue Klasse erscheint im Canvas, ist selektiert und wird rechts im Class Properties Panel angezeigt. |
| Sichtbare Hinweise | Neue Klasse `Libary` ist selektiert; rechts sind Class Name, Attributes und Operations sichtbar. |
| Fachliche Anforderung | Eine neu erzeugte Klasse muss sofort Teil des Modells sein und weiterbearbeitet werden können. |
| UI-Anforderung | Nach `Create Class` sollte kein zusätzlicher Such- oder Auswahlaufwand entstehen. |

Der Screenshot legt nahe:

- neue Klassen werden zentral im Canvas platziert,
- der Klassenname kann im Properties Panel geändert werden,
- Attribute und Operationen können nachträglich hinzugefügt werden,
- leere Attribute-/Operationsplätze oder Add Buttons führen weitere Eingaben.

### Attribute und Operationen anzeigen oder bearbeiten

![Class Properties](../assets/screenshots/01-class-diagram-class-properties.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `01-class-diagram-class-properties.png` |
| Nutzeraktion | Der Nutzer prüft oder bearbeitet Attribute und Operationen der Klasse. |
| Systemreaktion | Änderungen erscheinen im Properties Panel, im Klassenelement und später im Projektmodell. |
| Sichtbare Felder | Attribute `name : String`, `books : Integer`; Operation `borrow(): Void`; Buttons `Add Attribute`, `Add Operation`. |
| Fachliche Anforderung | Attribute brauchen Name und Typ; Operationen brauchen Name, Parameterliste und Rückgabetyp. |
| UI-Anforderung | Properties Panel muss wiederholbare Eingabegruppen für Attribute und Operationen anbieten. |

## Journey-Schritt 6: Association erstellen

![Add Class Association Modal](../assets/screenshots/10-modal-add-class-association.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `10-modal-add-class-association.png` |
| Nutzeraktion | Der Nutzer klickt im Explorer bei `Associations` auf `+` oder eine vergleichbare Add-Association-Aktion. |
| Systemreaktion | Ein Modal `Add Association` öffnet sich. Der Nutzer gibt Name, Source Class, Target Class und Rollen an. |
| Sichtbare Felder | `Association Name`, `Source Class`, `Target Class`, `Source Role`, `Target Role`, Button `Create Association`. |
| Fachliche Anforderung | Assoziationen verbinden Klassen und besitzen Rollen. Multiplizitäten müssen fachlich ebenfalls modellierbar sein. |
| UI-Anforderung | Source/Target Class sollten als Dropdowns mit existierenden Klassen angeboten werden. |

Annahme: Der Screenshot zeigt keine Multiplizitätsfelder. Da Multiplizitäten fachlich Teil des MVP sind, müssen sie entweder:

- im Add-Association-Dialog ergänzt werden,
- im Properties Panel bearbeitet werden,
- oder initial mit Defaultwerten erstellt und anschließend editierbar gemacht werden.

## Journey-Schritt 7: Invariante erstellen

![Add Invariant Modal](../assets/screenshots/09-modal-add-invariant.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `09-modal-add-invariant.png` |
| Nutzeraktion | Der Nutzer klickt im Explorer bei `Invariants` auf `+` oder im Properties Panel auf `Add Invariant`. |
| Systemreaktion | Ein Modal `Add Invariant` öffnet sich. Der Nutzer wählt eine Kontextklasse, vergibt einen Namen und gibt einen OCL-Ausdruck ein. |
| Sichtbare Felder | `Context Class`, `Invariant Name`, `OCL Expression`, Button `Add Invariant`. |
| Fachliche Anforderung | Eine Invariante ist an eine Kontextklasse gebunden und enthält einen OCL-Ausdruck. |
| UI-Anforderung | Kontextklasse als Dropdown; OCL-Ausdruck als ausreichend breites Eingabefeld. |

Erwartete Validierungen:

- Context Class muss existieren.
- Invariant Name darf nicht leer sein.
- OCL Expression darf nicht leer sein.
- OCL Expression muss syntaktisch und semantisch zum Kontext passen.

## Journey-Schritt 8: Invariante im Klassendiagramm anzeigen

![Invariant Properties](../assets/screenshots/03-class-diagram-invariant-properties.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `03-class-diagram-invariant-properties.png` |
| Nutzeraktion | Der Nutzer wählt eine Invariante aus dem Explorer oder aus einer Klassenreferenz. |
| Systemreaktion | Rechts wird der `Invariant`-Tab aktiv und zeigt Name sowie OCL Expression. |
| Sichtbare Hinweise | Explorer enthält `maxBooks`; Klassenelement `User` zeigt `inv: maxBooks`; Properties Panel zeigt `self.books < 6`. |
| Fachliche Anforderung | Invarianten müssen sowohl im Modellbaum als auch am Kontext im Diagramm nachvollziehbar sein. |
| UI-Anforderung | Properties Panel muss zwischen Class, Association und Invariant wechseln können. |

Die Journey zeigt hier eine wichtige Traceability im UI:

```text
Explorer Invariant
-> Kontextklasse im Canvas
-> Invariant Properties
-> OCL Expression
```

## Journey-Schritt 8a: OCL Editor öffnen und Invarianten zentral bearbeiten

![OCL Editor](../assets/screenshots/13-ocl-editor.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `13-ocl-editor.png` |
| Nutzeraktion | Der Nutzer wechselt über die Top-Navigation in den Tab `OCL Editor`. |
| Systemreaktion | Die Anwendung zeigt eine textbasierte Editorfläche mit Zeilennummern, USE-ähnlichem Modelltext, `Apply Changes`, `Check Constraints`, Save/Refresh und Bottom Panel. |
| Sichtbare Hinweise | Modell `Library`, Klassen `Book` und `User`, Attribute, Operationen, Association `Borrows`, Abschnitt `constraints`, Console-Ausgaben zu Projektinitialisierung und Modell-Laden. |
| Fachliche Anforderung | Nutzer müssen Modell- und OCL-Inhalte auch textuell prüfen und ändern können; Änderungen werden erst über `Apply Changes` in den Projektzustand übernommen. |
| UI-Anforderung | OCL Editor, Projektzustand, Console, Validation Results und Backend-OCL/API-Flows müssen synchronisiert werden. |

Der OCL Editor ergänzt die Modellierungs-Journey:

```text
Class Diagram
-> Add Invariant Modal
-> OCL Editor
-> Parse/Typecheck Feedback
-> Object Diagram
-> Check Constraints
```

## Journey-Schritt 9: Objektdiagramm öffnen

![Object Diagram Object Properties](../assets/screenshots/06-object-diagram-object-properties.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `06-object-diagram-object-properties.png` |
| Nutzeraktion | Der Nutzer wechselt über die Top-Navigation von `Class Diagram` zu `Object Diagram`. |
| Systemreaktion | Die Anwendung zeigt Objektinstanzen und Objektlinks des aktuellen Snapshots. |
| Sichtbare Hinweise | Explorer wechselt von `Classes` zu `Objects`; Canvas zeigt `mobyDick : Book` und `alice : User`. |
| Fachliche Anforderung | Das Objektdiagramm ist ein Snapshot auf Basis des Klassenmodells. |
| UI-Anforderung | Diagrammwechsel muss sichtbar sein und den Kontext im Explorer und Properties Panel ändern. |

Der Übergang vom Klassendiagramm zum Objektdiagramm ist fachlich entscheidend:

- Klassen werden zu Objekttypen,
- Attribute werden zu Slots,
- Assoziationen werden zu Objektlinks,
- Invarianten werden gegen konkrete Objektwerte geprüft.

## Journey-Schritt 10: Objekt bearbeiten

![Object Properties](../assets/screenshots/06-object-diagram-object-properties.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `06-object-diagram-object-properties.png` |
| Nutzeraktion | Der Nutzer wählt ein Objekt im Canvas oder Explorer aus. |
| Systemreaktion | Das Objekt wird hervorgehoben; rechts erscheinen Object Properties. |
| Sichtbare Felder | `Object Name`, `Type`, `Attribute Slots` mit `title`, `author`, `available`. |
| Fachliche Anforderung | Objektinstanzen besitzen Namen, Typen und Slots passend zu ihrer Klasse. |
| UI-Anforderung | Slot-Werte müssen direkt bearbeitbar und typabhängig dargestellt werden. |

Erwartete Validierungen:

- Object Name eindeutig im Snapshot.
- Type muss eine existierende Klasse sein.
- Slot-Namen müssen zu Attributen des Typs passen.
- Slot-Werte müssen zum Attributtyp passen.

## Journey-Schritt 11: Objektassoziation erstellen

![Add Object Association Modal](../assets/screenshots/11-modal-add-object-association.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `11-modal-add-object-association.png` |
| Nutzeraktion | Der Nutzer klickt im Object Diagram bei `Associations` auf `+`. |
| Systemreaktion | Ein Modal `Add Association` für Objektlinks öffnet sich. |
| Sichtbare Felder | `Source Object`, `Target Object`, `Association Name`, Button `Create Association`. |
| Fachliche Anforderung | Objektlinks müssen zu einer im Klassendiagramm definierten Assoziation passen. |
| UI-Anforderung | Dropdowns sollten nur semantisch zulässige Objekt- und Assoziationskombinationen anbieten. |

Nach dem Erstellen muss:

- eine Kante im Objektdiagramm erscheinen,
- der Link im Explorer unter `Associations` sichtbar sein,
- der Link selektierbar sein,
- der Link im Properties Panel überprüfbar sein.

### Objektassoziation betrachten oder bearbeiten

![Object Association Properties](../assets/screenshots/12-object-diagram-association-properties.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `12-object-diagram-association-properties.png` |
| Nutzeraktion | Der Nutzer wählt eine Objektassoziation bzw. einen Link aus. |
| Systemreaktion | Rechts wird der `Association`-Tab aktiv und zeigt Source Object, Target Object und Association Name. |
| Sichtbare Felder | `alice : User`, `mobyDick : Book`, `Borrows`. |
| Fachliche Anforderung | Ein Objektlink kennt beteiligte Objekte und die zugrundeliegende Association. |
| UI-Anforderung | Linkauswahl im Canvas und Association Properties müssen synchronisiert sein. |

## Journey-Schritt 12: Constraints prüfen

![Object Diagram Validation Error](../assets/screenshots/07-object-diagram-validation-error.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `07-object-diagram-validation-error.png` |
| Nutzeraktion | Der Nutzer klickt auf `Check Constraints` oder nutzt eine Tastenkombination wie `F5`. |
| Systemreaktion | Das System prüft Snapshot-Struktur, Multiplizitäten und OCL-Invarianten. |
| Sichtbare Hinweise | Validation Results Tab wird aktiv; Error Count erscheint; betroffene Objektkarte wird markiert. |
| Fachliche Anforderung | Constraint-Prüfung muss strukturierte Ergebnisse mit Objekt-/Link-/Invariant-Bezug liefern. |
| UI-Anforderung | Fehler müssen gleichzeitig visuell im Diagramm und textuell im Panel erscheinen. |

Der Screenshot zeigt eine Verletzung an `alice : User`. Die Invariante bezieht sich auf maximale Anzahl ausgeliehener Bücher:

```ocl
self.borrowedBooks->size() <= 5
```

Der sichtbare Objektwert `books : 6` bzw. die fachliche Anzahl überschreitet die erlaubte Grenze.

## Journey-Schritt 13: Fehler verstehen und korrigieren

![Validation Error](../assets/screenshots/07-object-diagram-validation-error.png)

| Aspekt | Beschreibung |
|---|---|
| Screenshot | `07-object-diagram-validation-error.png` |
| Nutzeraktion | Der Nutzer liest den Fehler im Validation Results Panel und identifiziert die betroffene Objektinstanz. |
| Systemreaktion | Das Panel zeigt Error Count, betroffene Entität, Erklärung, Constraint-Kontext und OCL-Ausdruck. |
| Sichtbare Hinweise | `1 Error`, `Object 'alice'`, `Violation of the system constraint`, `context: User`, `inv maxBooks`. |
| Fachliche Anforderung | Fehler müssen auf fachliche Modellbestandteile zurückführbar sein. |
| UI-Anforderung | Nutzer muss vom Fehler direkt zum betroffenen Objekt oder Constraint navigieren können. |

Korrekturpfade:

| Fehlerursache | Mögliche Nutzerkorrektur |
|---|---|
| Attributwert verletzt Invariante | Slot-Wert im Object Properties Panel ändern. |
| Zu viele Objektlinks | Einen Objektlink löschen oder ändern. |
| Falscher OCL-Ausdruck | Invariante im Class Diagram oder OCL Editor korrigieren. |
| Falsche Multiplizität | Association Properties im Class Diagram korrigieren. |
| Falscher Objekttyp oder Link | Objektlink neu erstellen oder entfernen. |

Nach der Korrektur führt der Nutzer `Check Constraints` erneut aus. Erwartet wird:

- Fehler-Markierung verschwindet,
- Validation Results zeigt keine Fehler oder aktualisierte Fehler,
- Console protokolliert die erneute Prüfung.

## Abgeleitete funktionale Anforderungen

| ID | Anforderung | Abgeleitet aus Screenshot |
|---|---|---|
| F-01 | Projekt oder Beispielmodell laden und initial anzeigen. | `01` |
| F-02 | Klassendiagramm mit Klassen, Attributen, Operationen und Assoziationen anzeigen. | `01`, `02`, `03`, `04` |
| F-03 | Klassen über Modal erstellen. | `08`, `04` |
| F-04 | Klassenname, Attribute und Operationen bearbeiten. | `01`, `04` |
| F-05 | Assoziationen zwischen Klassen erstellen. | `10` |
| F-06 | Association Properties anzeigen und bearbeiten. | `02` |
| F-07 | Invarianten mit Kontextklasse, Name und OCL-Ausdruck erstellen. | `09` |
| F-08 | Invarianten im Explorer, am Klassenelement, im Properties Panel und im OCL Editor anzeigen. | `03`, `13` |
| F-08A | OCL Editor als eigene Hauptansicht mit textbasierter Modell-/OCL-Bearbeitung bereitstellen. | `13` |
| F-08B | OCL-/Modelländerungen über `Apply Changes` übernehmen und diagnostizierbar machen. | `13`, fachliche Ableitung |
| F-08C | Lokale `.use`-Datei über Open Existing Modal als Modelltext importieren und Diagnosen anzeigen. | `14`, `13`, USE-Beispiele |
| F-08D | Vollständige Projektliste anzeigen, durchsuchen und Projektkarten öffnen. | `19` |
| F-09 | Zwischen Class Diagram und Object Diagram wechseln. | `01`, `06` |
| F-10 | Objektinstanzen anzeigen und bearbeiten. | `06` |
| F-11 | Attributwerte als Slots verwalten. | `06` |
| F-12 | Objektlinks erstellen. | `11`, `12` |
| F-13 | Objektlink-Properties anzeigen. | `12` |
| F-14 | Constraints per Button prüfen. | `01`, `06`, `07` |
| F-15 | Validierungsfehler strukturiert anzeigen. | `07` |
| F-16 | Betroffene Diagrammelemente visuell markieren. | `07` |
| F-17 | Console-Ereignisse protokollieren. | `01`, `06`, `07` |
| F-18 | Quick Help kontextbezogen anzeigen. | `01`, `02`, `03`, `06` |

## Abgeleitete UI-Anforderungen

| Bereich | UI-Anforderung | Begründung |
|---|---|---|
| Top Navigation | Tabs für `Class Diagram`, `Object Diagram`, `OCL Editor`. | Screenshots zeigen klar getrennte Hauptansichten; `13` zeigt den OCL Editor als eigene Arbeitsansicht. |
| Global Actions | `Check Constraints`, Refresh, Save, Search. | Wiederkehrend in allen Screenshots sichtbar. |
| Explorer Sidebar | Kontextabhängige Listen für Klassen/Invarianten bzw. Objekte/Assoziationen. | Explorer wechselt je nach Diagrammansicht. |
| Diagram Canvas | Selektierbare Knoten und Kanten mit visueller Hervorhebung. | Ausgewählte Klasse/Objekt blau markiert. |
| Properties Panel | Segmentierte Tabs je Elementtyp. | Class/Association/Invariant bzw. Object/Association. |
| OCL Editor | Textbasierte Bearbeitung eines USE-/OCL-Modelltexts mit Zeilennummern, `Apply Changes`, Console und Validierungsaktionen. | Screenshot `13` konkretisiert den bisher nur über Navigation abgeleiteten Tab. |
| Modals | Fokussierte Dialoge für Create-Flows. | Add Class, Add Invariant, Add Association. |
| Bottom Panel | Umschaltbar zwischen Console und Validation Results. | Console und Validation Results sind sichtbare Tabs. |
| Fehlerzustand | Rote Markierung am betroffenen Objekt plus Badge. | Validierungsfehler muss im Canvas sofort erkennbar sein. |
| Validation Panel | Error Count, betroffene Entität, Beschreibung, Constraint-Details. | Screenshot `07` zeigt diese Struktur. |
| Quick Help | Kontextbezogene Hilfe unten rechts. | Sichtbar in Properties-Kontexten. |

## Abgeleitete Backend-Anforderungen

| Bereich | Backend-Anforderung | Begründung |
|---|---|---|
| Projektmodell | Backend muss Klassen, Attribute, Operationen, Assoziationen, Invarianten, Objekte, Slots und Links speichern können. | Alle Screenshots zeigen zusammenhängende Modell- und Snapshotdaten. |
| Validierung | Backend muss `Check Constraints` fachlich ausführen. | Globaler Button ist zentraler Workflow-Auslöser. |
| OCL | Backend muss Invarianten syntaktisch, typisiert und gegen Snapshot evaluieren können. | Invariant Properties und Validation Error. |
| Multiplicity | Backend muss Objektlinks gegen Association-Multiplizitäten prüfen. | MVP-Scope und Association-Workflow. |
| DTOs | API muss Element-IDs und Referenzen liefern. | Frontend muss Fehler zu Objekten/Links/Invarianten markieren. |
| Validation Results | Backend muss strukturierte Fehler mit Severity, Code, Message, Constraint, betroffenen Objekten/Links liefern. | Validation Results Panel braucht strukturierte Daten. |
| Save/Load | Projektzustand muss persistent oder exportierbar sein. | Save-Icon und Console-Ladeereignisse. |
| Event/Log | Backend oder Frontend muss protokollierbare Ereignisse erzeugen. | Console zeigt Modell- und Erstellungsereignisse. |

## Offene Fragen

- Die Screenshots liegen aktuell unter `assets/screenshots/*.png`, während die Zielstruktur `assets/screenshots/*.png` vorsieht. Soll die Asset-Struktur später bereinigt werden?
- Wie genau werden Multiplizitäten erstellt oder bearbeitet, da der Add-Association-Dialog sie nicht sichtbar zeigt? Die Anzeige am Canvas ist entschieden: Rollen und Multiplizitäten müssen als Association-Endlabels sichtbar sein.
- Der OCL Editor soll den gesamten USE-ähnlichen Modelltext anzeigen und bearbeiten. Grundlage sind die `.use`-Beispiele unter `examples/`, die vollständige Modelltexte mit `model`, `class`, `attributes`, `operations`, `association`, `constraints` und perspektivisch `import` enthalten.
- Der neue Screenshot `14-open-existing-project.png` macht lokale `.use`-Dateien als Open-Existing-Eingabe sichtbar. Unklar bleibt, ob der erste MVP jede `.use`-Datei vollständig importieren muss oder nur den unterstützten Modelltext-Subset mit Diagnosen verarbeitet.
- Der neue Screenshot `19-projects.png` macht eine vollständige Projektübersicht sichtbar. Offen ist, ob Suche/Filter im MVP clientseitig über bereits geladene `ProjectSummaryDto[]` laufen oder serverseitige Query-Parameter bekommen.
- Soll `Check Constraints` automatisch den Validation Results Tab öffnen?
- Wie gelangt der Nutzer vom Fehler im Validation Panel direkt zum betroffenen Objekt oder zur betroffenen Invariante?
- Soll die Console rein informativ sein oder auch ausführbare Eingaben unterstützen?
- Wie werden Objektinstanzen erstellt? Ein expliziter Add-Object-Dialog ist in den vorhandenen Screenshots nicht enthalten.
- Wie werden Attribute und Operationen genau hinzugefügt? Die Screenshots zeigen Add Buttons, aber keine entsprechenden Modals.
- Soll die UI `books : 6` als manuelles Attribut oder als aus Links berechneten Wert behandeln?
- Soll das Validation Panel auch erfolgreiche Checks anzeigen oder nur Fehler?

## Zusammenfassung

Die Screenshots zeigen einen klaren MVP-Workflow: Ein Nutzer lädt ein Modell, arbeitet im Klassendiagramm an Klassen, Assoziationen und Invarianten, wechselt ins Objektdiagramm, bearbeitet Objekte und Objektlinks und prüft anschließend Constraints.

Der wichtigste fachliche Durchstich ist:

```text
Class Diagram
-> Invariant
-> Object Diagram / Snapshot
-> Check Constraints
-> Visual Error + Validation Results
-> Correction
-> Revalidation
```

Für den MVP sind daraus besonders wichtig:

- synchronisierte Explorer-, Canvas- und Properties-Zustände,
- fokussierte Modals für Erstellungsaktionen,
- strukturierte Modell- und Snapshotdaten,
- Backend-basierte UML/OCL-Validierung,
- visuelle Fehler-Markierung im Diagramm,
- verständliche Validation Results mit Bezug auf Objekt, Kontextklasse und Invariante.
