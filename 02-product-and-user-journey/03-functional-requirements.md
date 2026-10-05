# Functional Requirements

## Zweck dieser Datei

Diese Datei definiert funktionale Anforderungen für das neue UML/OCL-Websystem.

Sie übersetzt den globalen Projektkontext, die ausgewerteten UI-Screenshots und die Analyse des originalen USE-Projekts in priorisierte Anforderungen. Die Anforderungen beschreiben, was das Zielsystem leisten soll, ohne eine konkrete Implementierung vorzugeben.

Die Datei dient als Grundlage für:

- MVP-Abgrenzung,
- Backend- und Frontend-Konzeption,
- API-Verträge,
- Akzeptanzkriterien,
- Testfallableitung,
- spätere Issue- und Roadmap-Planung.

Wichtig: Das neue System ist keine technische Migration des originalen USE-Projekts. USE dient als fachliche Referenz für Konzepte, Syntax, Verhalten und Testfälle. Die technische Umsetzung erfolgt neu.

## Anforderungskategorien

| Kategorie | Inhalt | Primäre Quellen |
|---|---|---|
| Dashboard | Startseite, neues Modell mit Projektname, Open Existing, Open-Existing-Modal, Recent Projects, Learn & Support. | Screenshots `00`, `18`, `14` |
| Projektverwaltung | Projekt anlegen, Projektliste anzeigen, laden, speichern, exportieren. | Screenshot `19`, fachliche Ableitung |
| Klassendiagramm | Klassen, Attribute, Operationen, Assoziationen, Rollen, Multiplizitäten. | Screenshots, USE-Konzepte |
| Objektdiagramm | Objekte, Slots, Objektlinks, Snapshot-Bearbeitung. | Screenshots, USE-Systemzustände |
| OCL und Invarianten | Invarianten erfassen, OCL-Ausdrücke speichern, MVP-OCL-Subset prüfen. | Screenshots, USE-OCL |
| Validierung | UML-Struktur, Multiplizitäten und OCL-Invarianten prüfen. | USE-Verhalten, fachliche Ableitung |
| Fehlerdarstellung | Validierungsfehler im Diagramm und Validation Panel darstellen. | Screenshot `07`, USE-Fehlerarten |
| Import/Export | JSON-MVP-Format, später `.use` Import/Export. | Scope-Entscheidung, USE-Syntax |
| UI-Interaktion | Explorer, Canvas, Properties Panel, Modals, Navigation. | Screenshots |
| Backend/API | Projekt-, Modell-, Snapshot-, OCL- und Validation-Endpunkte. | Architekturziel, USE-Referenz |
| Integration | Konsistente DTOs, IDs, Synchronisation zwischen Frontend und Backend. | fachliche Ableitung |

## Dashboard-Anforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-DASH-001 | Dashboard anzeigen | Nutzer sieht beim Öffnen der Anwendung eine Startseite mit Logo `USE`, Untertitel, Benutzeravatar und zentralen Einstiegskarten. | MVP | Screenshot `00-dashboard-start-page.png` | Frontend | Dashboard ist erreichbar über `/` oder `/dashboard` und erscheint vor den Diagramm-Views. |
| FR-DASH-002 | Neues Projekt starten | Nutzer kann über `+ Start Project` den Create-New-Project-Flow öffnen. | MVP | Screenshot `00-dashboard-start-page.png`, `18-create-new-projects.png` | Frontend, Backend, API | Nach Klick erscheint ein Dialog/Formular zur Eingabe des Projektnamens. |
| FR-DASH-002A | Projektname beim Erstellen erfassen | Nutzer muss vor der Anlage eines neuen Projekts einen nicht leeren Projektnamen eingeben. | MVP | Screenshot `18-create-new-projects.png` | Frontend, Backend, API | Submit ist bei leerem Namen deaktiviert oder zeigt einen Feldfehler; `POST /api/v1/projects` erhält `CreateProjectRequestDto.name`; Backend lehnt leere Namen ab. |
| FR-DASH-003 | Open Existing anzeigen | Nutzer sieht eine Karte `Open Existing` zum Öffnen oder Importieren vorhandener Projekte. | MVP | Screenshot `00-dashboard-start-page.png` | Frontend | Klick auf die Karte öffnet den Open-Existing-Flow. |
| FR-PROJECT-IMPORT-001 | Open Existing Modal anzeigen | Nutzer sieht nach Klick auf `Open Existing` ein Modal `Open Existing Project` mit `Local File`, Upload-/Drag-and-drop-Fläche, `.use`-Hinweis, `Cancel` und `Open Project`. | MVP/Should | Screenshot `14-open-existing-project.png` | Frontend | Modal erscheint über dem Dashboard und blockiert den Hintergrund. |
| FR-PROJECT-IMPORT-002 | `.use`-Datei auswählen oder droppen | Nutzer kann eine lokale `.use`-Datei per Dateiauswahl oder Drag-and-drop in das Open-Existing-Modal übernehmen. | Should | Screenshot `14-open-existing-project.png`, Beispiele aus `examples/` | Frontend | Frontend akzeptiert nur unterstützte Dateitypen, zeigt Dateiname/Fehler und aktiviert `Open Project` erst bei gültiger Auswahl. |
| FR-PROJECT-IMPORT-003 | `.use`-Datei als Modelltext importieren | Der Dateiinhalt wird als Modelltext gelesen und an den Backend-Import- oder Model-Text-Apply-Flow übergeben. | Should | Screenshot `14-open-existing-project.png`, OCL Editor `13-ocl-editor.png`, Original-USE-Syntax | Frontend, Backend, API, Import Service, Model Text Parser | Unterstützte Syntax erzeugt ein Projekt oder aktualisierten Projektzustand; nicht unterstützte Syntax erzeugt strukturierte Diagnosen. |
| FR-PROJECT-IMPORT-004 | Importdiagnosen anzeigen | Importfehler, nicht unterstützte USE-Konstrukte und Syntax-/Typecheck-Probleme werden strukturiert angezeigt. | Should | Screenshot `14-open-existing-project.png`, Error Contract, OCL Editor | Frontend, Backend, API, OCL Engine, Import Service | Nutzer erkennt, ob die Datei geöffnet wurde, teilweise übernommen wurde oder abgelehnt wurde. |
| FR-RECENT-001 | Recent Projects anzeigen | Dashboard zeigt zuletzt verwendete oder Demo-Projekte wie `University System`, `Hotel Management`, `Bank ATM`. | Should | Screenshot `00-dashboard-start-page.png` | Frontend, Backend/API optional | Recent Projects sind sichtbar; im MVP dürfen sie Mock- oder Demo-Daten sein. |
| FR-RECENT-002 | Recent Project öffnen | Nutzer kann ein Recent Project öffnen. | Should | Screenshot `00-dashboard-start-page.png` | Frontend, Backend, API | Klick lädt das Projekt und führt zur Class Diagram View. |
| FR-RECENT-003 | View all öffnen | Nutzer kann über `View all` die vollständige Projektübersicht öffnen. | Should | Screenshots `00-dashboard-start-page.png`, `19-projects.png` | Frontend, Backend, API | Route `/projects` oder äquivalent zeigt die All-Projects-Ansicht. |
| FR-SUPPORT-001 | Documentation öffnen | Nutzer kann über `Documentation` Hilfedokumentation öffnen. | Should | Screenshot `00-dashboard-start-page.png` | Frontend | Link führt zu Dokumentation oder zu einer klaren Platzhalterseite. |
| FR-SUPPORT-002 | Examples öffnen | Nutzer kann über `Examples` Beispielmodelle oder Beispielübersicht öffnen. | Should | Screenshot `00-dashboard-start-page.png` | Frontend, Backend/API optional | Beispiele können geöffnet oder als Demo-Projekte geladen werden. |

## Projektanforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-PROJ-001 | Projekt anlegen | Nutzer kann vom Dashboard aus ein neues UML/OCL-Projekt mit angegebenem Projektnamen, leerem Klassenmodell und leerem Snapshot anlegen. | MVP | Screenshots `00`, `18`, fachliche Ableitung | Frontend, Backend, API | Nach gültigem Projektnamen und erfolgreichem `Start Project` sind Class Diagram View, Explorer, Properties Panel und Console in einem definierten Startzustand sichtbar. |
| FR-PROJ-002 | Projekt laden | Nutzer kann ein bestehendes Projekt im MVP-Projektformat laden. | MVP | Screenshot `01`, fachliche Ableitung | Frontend, Backend, API | Nach dem Laden erscheinen Klassen, Assoziationen, Invarianten und vorhandene Snapshot-Elemente konsistent in UI und Modellzustand. |
| FR-PROJ-003 | Projekt speichern | Nutzer kann den aktuellen Projektstand speichern. | MVP | Screenshot `01`, Screenshot `06` | Frontend, Backend, API | Nach `Save` bleibt der aktuelle Modell- und Snapshotzustand reproduzierbar erhalten. |
| FR-PROJ-004 | Beispielmodell öffnen | Nutzer kann ein vorbereitetes Beispielmodell, z. B. ein Library-Modell, öffnen. | MVP | Screenshots, Original-USE-Beispiele | Frontend, Backend, API | Ein Beispielmodell demonstriert Klassen, Assoziation, Invariante, Objekte, Objektlink und Validierung. |
| FR-PROJ-005 | Konsistenter Projektzustand | Das Projekt bündelt UML-Modell, OCL-Invarianten, Snapshot, Diagrammpositionen und UI-relevante Metadaten. | MVP | USE `MModel`/`MSystemState`, fachliche Ableitung | Backend, API | Ein geladener Zustand enthält alle Daten, die für Class Diagram, Object Diagram und Constraint Check benötigt werden. |
| FR-PROJ-006 | Projektmetadaten verwalten | Projektname, optionale Beschreibung und Änderungszeitpunkt können verwaltet werden. | Should | fachliche Ableitung | Frontend, Backend, API | Metadaten sind sichtbar oder abrufbar und werden mit dem Projekt gespeichert. |
| FR-PROJ-007 | Alle Projekte anzeigen | Nutzer kann eine vollständige Projektliste mit Projektkarten sehen. | Should | Screenshot `19-projects.png` | Frontend, Backend, API | Projektliste zeigt Name, Beschreibung und Änderungszeit; IDs werden nicht prominent angezeigt. |
| FR-PROJ-008 | Projekte suchen | Nutzer kann Projekte in der All-Projects-Ansicht über ein Suchfeld filtern. | Should | Screenshot `19-projects.png` | Frontend, Backend/API optional | Suche filtert mindestens nach Projektname; serverseitige Suche kann später ergänzt werden. |
| FR-PROJ-009 | Projekte filtern | Nutzer sieht einen Filter-Einstieg für die Projektliste. | Later/Should | Screenshot `19-projects.png` | Frontend, Backend/API optional | Filter-Button ist sichtbar; konkrete Filterkriterien können später ausgearbeitet werden. |
| FR-PROJ-010 | Projekt aus Projektliste öffnen | Nutzer kann eine Projektkarte in `All Projects` öffnen. | Should | Screenshot `19-projects.png` | Frontend, Backend, API | Klick lädt `GET /api/v1/projects/{projectId}` und navigiert ins Klassendiagramm. |
| FR-PROJ-011 | Neues Projekt aus Projektliste starten | Nutzer kann über `+ New Project` in der All-Projects-Ansicht denselben Create-New-Project-Flow starten. | Should | Screenshot `19-projects.png`, `18-create-new-projects.png` | Frontend, Backend, API | Button öffnet den Create-New-Project-Dialog; erfolgreicher Submit navigiert ins Klassendiagramm. |

## Klassendiagramm-Anforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-CLASS-001 | Klasse erstellen | Nutzer kann eine neue UML-Klasse über einen Dialog erstellen. | MVP | Screenshot `08`, Screenshot `04` | Frontend, Backend, API | Nach `Create Class` erscheint die Klasse im Explorer, im Canvas und im Properties Panel. |
| FR-CLASS-002 | Klasse bearbeiten | Nutzer kann den Namen einer Klasse im Properties Panel bearbeiten. | MVP | Screenshot `01`, Screenshot `04` | Frontend, Backend, API | Eine Namensänderung wird in Explorer, Canvas, Properties Panel und Projektmodell synchron angezeigt. |
| FR-CLASS-003 | Klasse löschen | Nutzer kann eine Klasse löschen, sofern abhängige Elemente kontrolliert behandelt werden. | MVP | fachliche Ableitung | Frontend, Backend, API, Validation Service | Das System verhindert inkonsistente Reste oder meldet abhängige Attribute, Assoziationen, Invarianten und Objekte verständlich. |
| FR-CLASS-004 | Klassennamen validieren | Klassennamen müssen nicht leer, eindeutig und syntaktisch gültig sein. | MVP | USE-Syntaxreferenz, fachliche Ableitung | Backend, API, Frontend | Ungültige Namen werden vor oder beim Speichern mit strukturierter Fehlermeldung abgelehnt. |
| FR-CLASS-005 | Attribute erstellen | Nutzer kann Attribute für eine Klasse erstellen. | MVP | Screenshot `01`, Screenshot `04`, USE `MAttribute` | Frontend, Backend, API | Ein neues Attribut besitzt mindestens Name und Typ und erscheint in Class Diagram und Properties Panel. |
| FR-CLASS-006 | Attribute typisieren | Nutzer kann Attribute mit primitiven Typen typisieren. | MVP | USE-Typkonzept, MVP-OCL-Subset | Frontend, Backend, API, OCL Engine | MVP unterstützt mindestens `String`, `Integer`, `Real` und `Boolean`. |
| FR-CLASS-007 | Attribute bearbeiten | Nutzer kann Attributname und Attributtyp bearbeiten. | MVP | Screenshot `01`, fachliche Ableitung | Frontend, Backend, API, Validation Service | Änderungen werden gespeichert und wirken sich auf Objekt-Slots und OCL-Typechecking aus. |
| FR-CLASS-008 | Attribute löschen | Nutzer kann Attribute löschen. | Should | fachliche Ableitung | Frontend, Backend, API, Validation Service | Bestehende Slots oder OCL-Ausdrücke mit Bezug auf das Attribut werden als betroffen erkannt oder blockiert. |
| FR-CLASS-009 | Operationen als Signaturen erfassen | Nutzer kann Operationen als Signaturen mit Name, Parametern und Rückgabetyp erfassen. | MVP | Screenshot `01`, USE `MOperation` | Frontend, Backend, API | Operationen werden angezeigt und gespeichert, aber im MVP nicht ausgeführt. |
| FR-CLASS-010 | Operationen bearbeiten | Nutzer kann Operationssignaturen ändern. | Should | Screenshot `01`, USE-Konzept | Frontend, Backend, API | Änderung erscheint im Klassenelement und im Properties Panel. |
| FR-CLASS-011 | Operationen löschen | Nutzer kann Operationssignaturen entfernen. | Should | fachliche Ableitung | Frontend, Backend, API | Gelöschte Operationen erscheinen nicht mehr in Canvas, Properties Panel und Projektformat. |
| FR-CLASS-012 | Assoziation erstellen | Nutzer kann eine Assoziation zwischen zwei Klassen erstellen. | MVP | Screenshot `10`, USE `MAssociation` | Frontend, Backend, API | Nach Erstellung erscheint die Assoziation im Explorer und als Kante im Klassendiagramm. |
| FR-CLASS-013 | Assoziation bearbeiten | Nutzer kann Name, beteiligte Klassen, Rollen und fachliche Enden einer Assoziation bearbeiten. | MVP | Screenshot `02`, Screenshot `10`, USE `MAssociationEnd` | Frontend, Backend, API, Validation Service | Properties Panel zeigt aktuelle Association-Daten und speichert Änderungen konsistent. |
| FR-CLASS-014 | Rollen erfassen | Nutzer kann Rollen für Source- und Target-Ende einer Assoziation erfassen. | MVP | Screenshot `10`, USE-Rollenkonzept | Frontend, Backend, API, OCL Engine | Rollen stehen für OCL-Navigation und UI-Beschriftung zur Verfügung. |
| FR-CLASS-015 | Multiplizitäten erfassen | Nutzer kann Multiplizitäten für Association Ends erfassen. | MVP | USE `MMultiplicity`, MVP-Scope | Frontend, Backend, API, Validation Service | Mindestens `0..1`, `1`, `0..*`, `1..*` und freie einfache Grenzen können modelliert und validiert werden. |
| FR-CLASS-016 | Assoziation löschen | Nutzer kann eine Assoziation löschen. | MVP | fachliche Ableitung | Frontend, Backend, API, Validation Service | Abhängige Objektlinks und OCL-Navigationen werden erkannt, blockiert oder als Folgeänderung behandelt. |
| FR-CLASS-017 | Klassendiagramm anzeigen | System zeigt Klassen mit Name, Attributen, Operationen und Invariantenreferenzen. | MVP | Screenshot `01`, Screenshot `03` | Frontend, API | Diagrammzustand entspricht dem gespeicherten Backend-Modell. |
| FR-CLASS-018 | Association Labels anzeigen | System zeigt Assoziationsname und optional Rollen-/Multiplizitätsinformationen im Klassendiagramm. | Should | Screenshot `02`, fachliche Ableitung | Frontend, API | Nutzer kann die fachliche Bedeutung einer Verbindung ohne Wechsel in andere Views erkennen. |

## Objektdiagramm-Anforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-OBJ-001 | Objektdiagramm öffnen | Nutzer kann vom Klassendiagramm in das Objektdiagramm wechseln. | MVP | Screenshot `06` | Frontend, API | Navigation wechselt Canvas, Explorer und Properties Panel in den Snapshot-Kontext. |
| FR-OBJ-002 | Objektinstanz erstellen | Nutzer kann eine Objektinstanz für eine vorhandene Klasse erstellen. | MVP | fachliche Ableitung, USE `MObject` | Frontend, Backend, API | Objekt besitzt eindeutigen Namen, Typ und Slots passend zur Klasse. |
| FR-OBJ-003 | Objekt bearbeiten | Nutzer kann Name und Typ einer Objektinstanz bearbeiten, soweit der Snapshot konsistent bleibt. | MVP | Screenshot `06`, USE-Systemzustand | Frontend, Backend, API, Validation Service | Änderungen werden in Canvas, Explorer und Properties Panel synchron angezeigt. |
| FR-OBJ-004 | Objekt löschen | Nutzer kann eine Objektinstanz löschen. | MVP | fachliche Ableitung | Frontend, Backend, API, Validation Service | Abhängige Objektlinks werden entfernt, blockiert oder als betroffene Elemente gemeldet. |
| FR-OBJ-005 | Slots anzeigen | System zeigt Attributwerte eines Objekts als Slots an. | MVP | Screenshot `06`, USE `MObjectState` | Frontend, API | Jeder Slot ist dem Attribut der Objektklasse zuordenbar. |
| FR-OBJ-006 | Attributwerte setzen | Nutzer kann Slot-Werte setzen oder ändern. | MVP | Screenshot `06`, USE `MAttributeAssignmentStatement` | Frontend, Backend, API, Validation Service | Werte werden typsicher gespeichert und bei Constraint Checks berücksichtigt. |
| FR-OBJ-007 | Slot-Typen validieren | System prüft, ob Slot-Werte zum Attributtyp passen. | MVP | USE-Typkonzept, fachliche Ableitung | Backend, API, Validation Service | Ungültige Werte erzeugen strukturierte Validierungs- oder Eingabefehler. |
| FR-OBJ-008 | Objektlink erstellen | Nutzer kann einen Objektlink auf Basis einer existierenden Klassenassoziation erstellen. | MVP | Screenshot `11`, USE `MLink` | Frontend, Backend, API, Validation Service | Link erscheint im Explorer und als Kante im Objektdiagramm. |
| FR-OBJ-009 | Objektlink bearbeiten | Nutzer kann Source Object, Target Object und Association eines Objektlinks prüfen oder ändern. | MVP | Screenshot `12` | Frontend, Backend, API, Validation Service | Properties Panel zeigt den Link und speichert zulässige Änderungen. |
| FR-OBJ-010 | Objektlink löschen | Nutzer kann Objektlinks entfernen. | MVP | fachliche Ableitung | Frontend, Backend, API, Validation Service | Nach dem Löschen wird der Link aus Canvas, Explorer und Snapshot entfernt. |
| FR-OBJ-011 | Link-Kompatibilität prüfen | System prüft, ob ein Objektlink zur gewählten Association und zu den beteiligten Klassentypen passt. | MVP | USE-Linkvalidierung, fachliche Ableitung | Backend, API, Validation Service | Unzulässige Objekt-/Association-Kombinationen werden verhindert oder als Fehler gemeldet. |
| FR-OBJ-012 | Snapshot speichern | System speichert Objekte, Slots und Objektlinks als aktuellen Snapshot. | MVP | USE `MSystemState`, fachliche Ableitung | Backend, API | Nach Laden des Projekts ist der Snapshot wiederhergestellt. |
| FR-OBJ-013 | Mehrere Snapshots verwalten | System kann mehrere benannte Snapshots pro Projekt verwalten. | Later | USE-Systemzustandskonzept, fachliche Ableitung | Frontend, Backend, API | Nutzer kann zwischen Snapshots wechseln, ohne das Klassenmodell zu duplizieren. |

## OCL-Anforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-OCL-001 | Invariante erstellen | Nutzer kann eine OCL-Invariante mit Kontextklasse, Name und Ausdruck erstellen. | MVP | Screenshot `09`, USE `MClassInvariant` | Frontend, Backend, API, OCL Engine | Invariante erscheint im Explorer, an der Kontextklasse und im Properties Panel. |
| FR-OCL-002 | Invariante bearbeiten | Nutzer kann Namen, Kontextklasse und OCL-Ausdruck einer Invariante bearbeiten. | MVP | Screenshot `03`, USE-Konzept | Frontend, Backend, API, OCL Engine, Validation Service | Änderungen wirken sich auf den nächsten Constraint Check aus. |
| FR-OCL-003 | Invariante löschen | Nutzer kann eine Invariante entfernen. | MVP | fachliche Ableitung | Frontend, Backend, API | Gelöschte Invariante wird nicht mehr angezeigt und nicht mehr validiert. |
| FR-OCL-004 | OCL-Ausdruck speichern | System speichert den originalen OCL-Ausdruck einer Invariante. | MVP | Screenshot `03`, USE-Syntaxreferenz | Backend, API, OCL Engine | Ausdruck bleibt nach Speichern und Laden unverändert verfügbar. |
| FR-OCL-005 | Kontext und `self` unterstützen | OCL-Ausdrücke können `self` bezogen auf die Kontextklasse verwenden. | MVP | USE-OCL, MVP-OCL-Subset | OCL Engine, Backend, API | `self.attribute` wird gegen die Kontextklasse typgeprüft und ausgewertet. |
| FR-OCL-006 | Attributzugriff unterstützen | OCL unterstützt Zugriff auf Attribute der Kontextklasse. | MVP | USE `ExpAttrOp`, Beispiele `Cars.use` | OCL Engine, Validation Service | Ein Ausdruck wie `self.mileage >= 0` kann geprüft werden. |
| FR-OCL-007 | Einfache Association Navigation unterstützen | OCL unterstützt einfache Navigation über Rollen einer Association. | MVP | USE `ExpNavigation`, Beispiele `Demo.use` | OCL Engine, Validation Service | Ein Ausdruck wie `self.borrowedBooks->size() <= 5` kann gegen Objektlinks ausgewertet werden. |
| FR-OCL-008 | Primitive Literale unterstützen | OCL unterstützt String-, Integer-, Real- und Boolean-Literale. | MVP | MVP-OCL-Subset, USE-OCL | OCL Engine | Literale können in Invarianten verwendet und typgeprüft werden. |
| FR-OCL-009 | Vergleichsoperatoren unterstützen | OCL unterstützt einfache Vergleichsoperatoren. | MVP | MVP-OCL-Subset, USE-Beispiele | OCL Engine | Ausdrücke mit `=`, `<>`, `<`, `<=`, `>` und `>=` können ausgewertet werden. |
| FR-OCL-010 | Boolean-Operatoren unterstützen | OCL unterstützt `and`, `or` und `not`. | MVP | MVP-OCL-Subset, USE-OCL | OCL Engine | Zusammengesetzte boolesche Invarianten liefern ein boolesches Ergebnis. |
| FR-OCL-011 | Klammern unterstützen | OCL unterstützt Klammern zur Gruppierung von Ausdrücken. | MVP | MVP-OCL-Subset | OCL Engine | Operatorpräzedenz kann durch Klammern eindeutig gemacht werden. |
| FR-OCL-012 | Collection-Grundoperationen unterstützen | OCL unterstützt `size`, `isEmpty` und `notEmpty` auf einfachen Navigationsergebnissen. | MVP | Screenshot `07`, USE-OCL | OCL Engine, Validation Service | Collection-Operationen liefern korrekte Werte für Linkmengen. |
| FR-OCL-013 | OCL syntaktisch prüfen | System erkennt Syntaxfehler in OCL-Ausdrücken. | MVP | USE `ParseErrorHandler`, OCL-Compiler | OCL Engine, API, Frontend | Fehler enthalten Meldung und möglichst Position im Ausdruck. |
| FR-OCL-014 | OCL typprüfen | System erkennt semantisch falsche OCL-Ausdrücke. | MVP | USE-Typechecking | OCL Engine, API, Frontend | Ungültiger Attribut- oder Rollenname erzeugt einen Typecheck-Fehler. |
| FR-OCL-015 | Erweiterte OCL-Operationen vorbereiten | Architektur bleibt für `forAll`, `exists`, `select`, `collect`, `let`, `if-then-else` und `allInstances` erweiterbar. | Later | USE-OCL, Post-MVP-Scope | OCL Engine, Backend | Neue AST-Knoten und Evaluationsregeln können ergänzt werden, ohne MVP-Pipeline zu ersetzen. |
| FR-OCL-016 | OCL Editor anzeigen | Nutzer kann eine eigene textbasierte OCL-/Modell-Editor-Ansicht öffnen. | MVP | Screenshot `13-ocl-editor.png` | Frontend, API | Der Tab `OCL Editor` zeigt eine Editorfläche mit Zeilennummern, USE-ähnlichem Modelltext, `Apply Changes`, `Check Constraints`, Save/Refresh und Console. |
| FR-OCL-017 | Modell-/OCL-Text im Editor bearbeiten | Nutzer kann den textuellen Modell-/OCL-Inhalt im OCL Editor bearbeiten. | MVP | Screenshot `13-ocl-editor.png` | Frontend, API | Änderungen bleiben als Draft sichtbar und werden erst über `Apply Changes` in den Projektzustand übernommen. |
| FR-OCL-018 | Editoränderungen anwenden | Nutzer kann Änderungen aus dem OCL Editor explizit anwenden. | MVP | Screenshot `13-ocl-editor.png` | Frontend, API, OCL Engine | `Apply Changes` sendet den Text an das Backend oder aktualisiert den Projektzustand und zeigt Diagnose-/Fehlerzustände strukturiert an. |
| FR-OCL-019 | OCL-/Modell-Diagnosen im Editor anzeigen | Parse- und Typecheck-Ergebnisse werden im OCL Editor mit Code, Severity, Message und optionaler Source Range angezeigt. | MVP/Should | Screenshot `13-ocl-editor.png`, USE-OCL | Frontend, API, OCL Engine | Backend-Diagnosen erscheinen passend zum textuellen Editor ohne fachliche OCL-Auswertung im Frontend. |

## Validierungsanforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-VAL-001 | Constraint Check ausführen | Nutzer kann über `Check Constraints` eine Validierung ausführen. | MVP | Screenshot `07`, Screenshot `01` | Frontend, Backend, API, Validation Service, OCL Engine | Ein Klick startet eine Backend-validierte Prüfung und liefert strukturierte Ergebnisse. |
| FR-VAL-002 | UML-Struktur prüfen | System prüft strukturelle Konsistenz von Klassenmodell und Snapshot. | MVP | USE `MSystemState.checkStructure`, fachliche Ableitung | Backend, Validation Service | Fehlende Typen, ungültige Links oder unpassende Slots werden erkannt. |
| FR-VAL-003 | Multiplizitäten prüfen | System prüft Objektlinks gegen Association-Multiplizitäten. | MVP | USE-Multiplicity Checks | Backend, Validation Service, API | Verletzungen enthalten Association, Rollenende und betroffene Objekte oder Links. |
| FR-VAL-004 | Invarianten prüfen | System wertet aktive Invarianten gegen alle Objekte der Kontextklasse aus. | MVP | USE `MSystemState.check`, Screenshot `07` | Backend, Validation Service, OCL Engine | Verletzungen enthalten Invariant ID, Kontextklasse und betroffene Objekt-ID. |
| FR-VAL-005 | Validierungsstatus zurückgeben | System liefert Gesamtstatus des Checks. | MVP | fachliche Ableitung | Backend, API, Frontend | Ergebnis unterscheidet mindestens `VALID`, `INVALID` und `NOT_EVALUABLE`. |
| FR-VAL-006 | Strukturierte Ergebnisse liefern | Validation Results enthalten Fehlercode, Severity, Nachricht und Elementreferenzen. | MVP | Screenshot `07`, USE-Fehlerarten | Backend, API, Frontend | Frontend kann Fehler ohne Textparsing im Diagramm markieren. |
| FR-VAL-007 | Nicht auswertbare Invarianten behandeln | System unterscheidet verletzte Invarianten von nicht auswertbaren Invarianten. | Should | USE `undefined`/Evaluation-Fehler, fachliche Ableitung | OCL Engine, Validation Service, API | Parse-, Typecheck- oder Runtime-Probleme werden nicht als normale Constraint-Verletzung vermischt. |
| FR-VAL-008 | Validierung reproduzierbar ausführen | Gleicher Projekt- und Snapshotzustand liefert gleiche Validation Results. | MVP | fachliche Ableitung | Backend, Validation Service, OCL Engine | Ergebnisse sind stabil genug für automatisierte Tests. |
| FR-VAL-009 | Validierung protokollieren | System protokolliert Start und Ergebnis eines Constraint Checks. | Should | Screenshots Console | Frontend, Backend | Console zeigt nachvollziehbare Check-Ereignisse. |

## Fehlerdarstellungsanforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-ERR-001 | Validation Results anzeigen | System zeigt Validierungsergebnisse in einem eigenen Panel an. | MVP | Screenshot `07` | Frontend, API | Nach einem fehlerhaften Check ist das Validation Results Panel sichtbar oder aktivierbar. |
| FR-ERR-002 | Error Count anzeigen | System zeigt die Anzahl der gefundenen Fehler an. | MVP | Screenshot `07` | Frontend | Nutzer erkennt sofort, ob und wie viele Fehler vorliegen. |
| FR-ERR-003 | Fehlerhafte Objekte markieren | System markiert betroffene Objekte visuell im Objektdiagramm. | MVP | Screenshot `07` | Frontend, API | Ein Objekt mit Invariant-Verletzung erhält eine klare Fehler-Markierung. |
| FR-ERR-004 | Fehlerhafte Links markieren | System kann betroffene Objektlinks bei Link- oder Multiplizitätsfehlern markieren. | MVP | fachliche Ableitung, USE-Multiplicity Checks | Frontend, API, Validation Service | Linkbezogene Fehler enthalten Link- oder Association-End-Referenzen. |
| FR-ERR-005 | Fehlerdetails anzeigen | Fehlerdetails enthalten betroffene Entität, Beschreibung, Kontext und Constraint. | MVP | Screenshot `07` | Frontend, API | Nutzer kann aus dem Panel erkennen, welches Objekt welche Invariante verletzt. |
| FR-ERR-006 | Von Fehler zu Element navigieren | Nutzer kann von einem Fehler zum betroffenen Diagrammelement navigieren. | Should | fachliche Ableitung | Frontend, API | Klick auf Fehler selektiert Objekt, Link oder Invariante. |
| FR-ERR-007 | OCL-Fehler getrennt anzeigen | Syntax- und Typecheck-Fehler werden als OCL-Fehler mit Bezug zum Ausdruck angezeigt. | MVP | USE `ParseErrorHandler`, `SemanticException` | Frontend, API, OCL Engine | Fehler zeigen Invariante und möglichst Ausdrucksposition. |
| FR-ERR-008 | Fehler nach erneuter Validierung aktualisieren | Nach Korrektur und erneutem Check verschwinden erledigte Fehler und neue Fehler erscheinen. | MVP | Screenshot-Journey | Frontend, Backend, API, Validation Service | UI zeigt keine veralteten Fehlermarkierungen. |

## Import-/Export-Anforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-IO-001 | JSON exportieren | Nutzer kann ein Projekt im MVP-JSON-Format exportieren oder speichern. | MVP | Scope-Entscheidung | Frontend, Backend, API | Export enthält Modell, Invarianten, Snapshot und Diagrammpositionen. |
| FR-IO-002 | JSON importieren | Nutzer kann ein Projekt im MVP-JSON-Format importieren oder laden. | MVP | Scope-Entscheidung | Frontend, Backend, API | Import stellt denselben Zustand wieder her, der exportiert wurde. |
| FR-IO-003 | JSON validieren | System validiert importierte JSON-Projekte gegen das erwartete Projektformat. | MVP | fachliche Ableitung | Backend, API, Validation Service | Fehlerhafte Dateien erzeugen strukturierte Importfehler. |
| FR-IO-004 | `.use` Import über Modelltext vorbereiten | Systemarchitektur berücksichtigt lokalen `.use`-Import aus dem Open-Existing-Modal und verarbeitet im MVP-nahen Scope nur den unterstützten Modelltext-/UML-/OCL-Subset. | Should | Screenshot `14-open-existing-project.png`, Original-USE-Syntax, Beispiele | Frontend, Backend, API, Model Text Parser | Vollständige USE-Kompatibilität ist nicht MVP-verpflichtend; Importdiagnosen sind Pflicht, wenn Syntax nicht übernommen werden kann. |
| FR-IO-005 | `.use` Export vorbereiten | Systemarchitektur berücksichtigt späteren Export in eine USE-nahe Syntax. | Later | Original-USE-Syntax | Backend, API | Exportregeln werden erst nach stabilem JSON-Format spezifiziert. |
| FR-IO-006 | Beispielmodelle als Testquelle nutzen | Originale USE-Beispiele können in vereinfachte JSON-Testmodelle übersetzt werden. | Should | `Cars.use`, `Demo.use`, `Demo.cmd` | Backend, API, Validation Service | Mindestens ein reduziertes Library- oder Demo-Modell ist als Test- und Demo-Projekt verfügbar. |

## UI-Interaktionsanforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-UI-001 | Hauptansichten wechseln | Nutzer kann zwischen `Class Diagram`, `Object Diagram` und `OCL Editor` wechseln. | MVP | Screenshots `01`, `06` | Frontend | Aktive Ansicht ist eindeutig sichtbar und zeigt passende Daten. |
| FR-UI-002 | Explorer Sidebar nutzen | Explorer zeigt kontextabhängig Klassen, Assoziationen, Invarianten, Objekte und Objektlinks. | MVP | Screenshots `01`, `06`, `07` | Frontend, API | Auswahl im Explorer selektiert das entsprechende Element im Canvas oder Properties Panel. |
| FR-UI-003 | Diagram Canvas selektieren | Nutzer kann Klassen, Objekte, Assoziationen und Objektlinks im Canvas auswählen. | MVP | Screenshots `01`, `04`, `06`, `12` | Frontend | Selektiertes Element wird visuell hervorgehoben und im Properties Panel angezeigt. |
| FR-UI-004 | Properties Panel synchronisieren | Properties Panel zeigt Eigenschaften des aktuell selektierten Elements. | MVP | Screenshots `01`, `02`, `03`, `06`, `12` | Frontend, API | Wechsel der Selektion aktualisiert den passenden Properties-Tab. |
| FR-UI-005 | Modals für Erstellung nutzen | Nutzer erstellt Klassen, Invarianten und Assoziationen über fokussierte Dialoge. | MVP | Screenshots `08`, `09`, `10`, `11` | Frontend, API | Dialoge validieren Pflichtfelder und erzeugen nach Bestätigung das Element. |
| FR-UI-006 | Console anzeigen | System zeigt relevante Projekt- und Validierungsereignisse in einer Console. | Should | Screenshots `01`, `06` | Frontend | Console protokolliert Laden, Speichern, Erstellen und Constraint Checks. |
| FR-UI-007 | Quick Help anzeigen | UI kann kontextbezogene Kurzhilfe anzeigen. | Later | Screenshots | Frontend | Hilfe passt zur aktiven Ansicht oder Auswahl. |
| FR-UI-008 | Änderungen sofort reflektieren | Änderungen in Modals oder Properties Panel werden ohne manuelles Neuladen in Explorer und Canvas sichtbar. | MVP | Screenshot-Journey | Frontend, API | Nach Speichern einer Änderung ist kein Refresh nötig, um den neuen Zustand zu sehen. |
| FR-UI-009 | Such- oder Command-Zugang anbieten | UI kann Modellelemente oder Aktionen über Suche/Command Palette erreichbar machen. | Later | sichtbares Search-Icon, fachliche Ableitung | Frontend | Nutzer kann Elemente oder Aktionen schneller finden. |

## Backend- und API-Anforderungen

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-API-001 | Projekt-DTO bereitstellen | API stellt ein strukturiertes Projekt-DTO bereit. | MVP | Architekturziel, USE-Modelltrennung | Backend, API, Frontend | DTO enthält UML-Modell, OCL-Invarianten, Snapshot und Diagramm-Metadaten. |
| FR-API-002 | Klassenmodell-Operationen bereitstellen | API unterstützt Create, Read, Update, Delete für Klassen, Attribute, Operationen und Assoziationen. | MVP | fachliche Ableitung | Backend, API, Frontend | Frontend kann Class Diagram ohne eigene Semantik-Persistenz bearbeiten. |
| FR-API-003 | Snapshot-Operationen bereitstellen | API unterstützt Create, Read, Update, Delete für Objekte, Slots und Objektlinks. | MVP | USE `MSystemState`, Screenshots | Backend, API, Frontend | Frontend kann Object Diagram auf Basis von Backenddaten bearbeiten. |
| FR-API-004 | Invarianten-Operationen bereitstellen | API unterstützt Create, Read, Update, Delete für Invarianten. | MVP | Screenshot `09`, USE `MClassInvariant` | Backend, API, OCL Engine, Frontend | Invarianten werden gespeichert, geprüft und in Validation Results referenziert. |
| FR-API-005 | OCL-Prüfung bereitstellen | API kann OCL-Ausdrücke syntaktisch und semantisch prüfen. | MVP | USE-OCL, MVP-OCL-Pipeline | Backend, API, OCL Engine | Frontend erhält strukturierte Parse- und Typecheck-Ergebnisse. |
| FR-API-006 | Constraint-Check-Endpunkt bereitstellen | API bietet einen Endpunkt für vollständige Constraint Validation. | MVP | Screenshot `07`, USE `MSystemState.check` | Backend, API, Validation Service, OCL Engine | Response enthält Gesamtstatus und Liste strukturierter Ergebnisse. |
| FR-API-007 | Stabile Element-IDs verwenden | API verwendet stabile IDs für Klassen, Attribute, Assoziationen, Invarianten, Objekte und Links. | MVP | fachliche Ableitung | Backend, API, Frontend | Validation Results können eindeutig auf UI-Elemente abgebildet werden. |
| FR-API-008 | Fehlervertrag definieren | API liefert Fehler in einem stabilen Error Contract. | MVP | USE-Fehlerarten, Screenshot `07` | Backend, API, Frontend | Fehler enthalten `code`, `severity`, `message` und betroffene IDs. |
| FR-API-009 | Backend als Semantikquelle nutzen | Fachliche Validierung wird im Backend durchgeführt, nicht nur im Frontend. | MVP | Scope-Entscheidung | Backend, API, Frontend, Validation Service | Frontend zeigt Ergebnisse an, aber Semantikprüfung liegt zentral im Backend. |
| FR-API-010 | Versionierbares Projektformat vorbereiten | Projektformat enthält eine Formatversion. | Should | fachliche Ableitung | Backend, API | Spätere Formatänderungen können kontrolliert migriert oder erkannt werden. |

## Integration zwischen Frontend und Backend

| ID | Titel | Beschreibung | Priorität | Quelle | Betroffene Komponenten | Akzeptanzhinweis |
|---|---|---|---|---|---|---|
| FR-INT-001 | Frontend-Zustand aus API ableiten | Frontend leitet Diagramme und Panels aus API-Daten ab. | MVP | Architekturziel | Frontend, API, Backend | Ein Projekt kann neu geladen werden und zeigt denselben fachlichen Zustand. |
| FR-INT-002 | DTOs für Diagramm-Mapping bereitstellen | API-Daten enthalten ausreichende Informationen für Canvas-Darstellung und Properties Panel. | MVP | Screenshots, fachliche Ableitung | API, Backend, Frontend | Klassen, Objekte und Links haben IDs, Namen, Typen, Beziehungen und optional Positionen. |
| FR-INT-003 | Validierungsfehler auf UI-Elemente mappen | Frontend kann Validation Results über IDs auf Diagrammelemente abbilden. | MVP | Screenshot `07`, USE-Verhalten | Frontend, API, Validation Service | Fehlerhafte Objekte oder Links werden ohne heuristische Textanalyse markiert. |
| FR-INT-004 | Änderungen konsistent speichern | Frontend-Änderungen an Modell oder Snapshot werden über API gespeichert und danach konsistent angezeigt. | MVP | Screenshot-Journey | Frontend, Backend, API | Nach Create/Update/Delete entspricht der UI-Zustand dem Backendzustand. |
| FR-INT-005 | OCL-Feedback integrieren | OCL-Parse- und Typecheck-Ergebnisse werden im OCL Editor, Invariant Modal oder Invariant Properties Panel angezeigt. | MVP/Should | USE-OCL, Screenshots `03`, `09`, `13` | Frontend, API, OCL Engine | Nutzer erkennt OCL-Probleme vor oder spätestens beim Constraint Check. |
| FR-INT-006 | E2E-Demo unterstützen | Frontend und Backend unterstützen eine durchgehende Demo vom Klassendiagramm bis zur Fehlerkorrektur. | MVP | Screenshots, MVP-Zielbild | Frontend, Backend, API, OCL Engine, Validation Service | Demo zeigt Modellieren, Snapshot, Check, Fehleranzeige, Korrektur und erneuten Check. |

## Priorisierung

| Priorität | Bedeutung | Erwartung |
|---|---|---|
| MVP | Muss im ersten vertikalen Durchstich enthalten sein. | Ohne diese Anforderung ist der Kernworkflow nicht vollständig demonstrierbar. |
| Should | Sollte früh nach dem MVP oder bei geringer Zusatzkomplexität umgesetzt werden. | Verbessert Nutzbarkeit, Robustheit oder Testbarkeit, ist aber nicht zwingend für den ersten Durchstich. |
| Later | Post-MVP-Erweiterung. | Wichtig für langfristige Produktreife, aber nicht Teil des MVP-Scope. |

## MVP-Anforderungsliste

| Bereich | MVP-Anforderungen |
|---|---|
| Dashboard | FR-DASH-001, FR-DASH-002, FR-DASH-002A, FR-DASH-003, FR-PROJECT-IMPORT-001 |
| Projektverwaltung | FR-PROJ-001, FR-PROJ-002, FR-PROJ-003, FR-PROJ-004, FR-PROJ-005 |
| Klassendiagramm | FR-CLASS-001, FR-CLASS-002, FR-CLASS-003, FR-CLASS-004, FR-CLASS-005, FR-CLASS-006, FR-CLASS-007, FR-CLASS-009, FR-CLASS-012, FR-CLASS-013, FR-CLASS-014, FR-CLASS-015, FR-CLASS-016, FR-CLASS-017 |
| Objektdiagramm | FR-OBJ-001, FR-OBJ-002, FR-OBJ-003, FR-OBJ-004, FR-OBJ-005, FR-OBJ-006, FR-OBJ-007, FR-OBJ-008, FR-OBJ-009, FR-OBJ-010, FR-OBJ-011, FR-OBJ-012 |
| OCL und Invarianten | FR-OCL-001, FR-OCL-002, FR-OCL-003, FR-OCL-004, FR-OCL-005, FR-OCL-006, FR-OCL-007, FR-OCL-008, FR-OCL-009, FR-OCL-010, FR-OCL-011, FR-OCL-012, FR-OCL-013, FR-OCL-014, FR-OCL-016, FR-OCL-017, FR-OCL-018 |
| Validierung | FR-VAL-001, FR-VAL-002, FR-VAL-003, FR-VAL-004, FR-VAL-005, FR-VAL-006, FR-VAL-008 |
| Fehlerdarstellung | FR-ERR-001, FR-ERR-002, FR-ERR-003, FR-ERR-004, FR-ERR-005, FR-ERR-007, FR-ERR-008 |
| Import/Export | FR-IO-001, FR-IO-002, FR-IO-003 |
| UI-Interaktion | FR-UI-001, FR-UI-002, FR-UI-003, FR-UI-004, FR-UI-005, FR-UI-008 |
| Backend/API | FR-API-001, FR-API-002, FR-API-003, FR-API-004, FR-API-005, FR-API-006, FR-API-007, FR-API-008, FR-API-009 |
| Integration | FR-INT-001, FR-INT-002, FR-INT-003, FR-INT-004, FR-INT-006 |

## Post-MVP-Anforderungsliste

| Priorität | Anforderungen |
|---|---|
| Should | FR-PROJECT-IMPORT-002, FR-PROJECT-IMPORT-003, FR-PROJECT-IMPORT-004, FR-RECENT-001, FR-RECENT-002, FR-RECENT-003, FR-SUPPORT-001, FR-SUPPORT-002, FR-PROJ-006, FR-PROJ-007, FR-PROJ-008, FR-PROJ-010, FR-PROJ-011, FR-CLASS-008, FR-CLASS-010, FR-CLASS-011, FR-CLASS-018, FR-VAL-007, FR-VAL-009, FR-ERR-006, FR-IO-004, FR-IO-006, FR-UI-006, FR-API-010, FR-INT-005 |
| Later | FR-PROJ-009, FR-OBJ-013, FR-OCL-015, FR-IO-005, FR-UI-007, FR-UI-009 |

Weitere spätere Erweiterungen aus dem globalen Scope, die in eigenen Dateien detailliert werden sollten:

- Vererbung,
- Enumerationen,
- Aggregation und Komposition,
- Assoziationsklassen,
- `forAll`, `exists`, `select`, `collect`,
- `let`,
- `if-then-else`,
- `allInstances`,
- Preconditions und Postconditions,
- derived attributes,
- init values,
- mehrere Snapshots,
- vollständige `.use` Import-/Export-Kompatibilität,
- OCL Syntax Highlighting,
- OCL Autocomplete,
- Undo/Redo,
- Projektversionierung,
- Datenbankpersistenz.

## Zusammenfassung

Der funktionale Kern des neuen Systems ist ein vollständiger vertikaler UML/OCL-Workflow:

```text
Projekt laden
-> Klassendiagramm modellieren
-> Invarianten definieren
-> Objektdiagramm/Snapshot erstellen
-> Attributwerte und Objektlinks setzen
-> Constraints prüfen
-> Fehler im Diagramm und Validation Panel verstehen
-> Modell oder Snapshot korrigieren
-> erneut validieren
```

Für den MVP sind besonders kritisch:

- Klassen, Attribute, Operationensignaturen, Assoziationen, Rollen und Multiplizitäten,
- Objekte, Slots und Objektlinks,
- OCL-Invarianten mit kleinem, aber echter Pipeline-basiertem OCL-Subset,
- Backend-basierte UML- und OCL-Validierung,
- strukturierte Validation Results,
- visuelle Fehler-Markierung im Objektdiagramm,
- JSON-basiertes Speichern und Laden.

Die Anforderungen grenzen das Projekt bewusst von einer vollständigen USE-Migration ab. USE bleibt Referenz für Fachlichkeit, Syntax, Verhalten und Tests; das neue Websystem erhält eine eigene Architektur, eigene APIs, eigene OCL-Verarbeitung und eine neue Weboberfläche.
