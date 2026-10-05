# Full OCL and UML Frontend Implementation Plan

## Ziel und Abgrenzung

Dieser Plan beschreibt ausschliesslich die React/TypeScript-Umsetzung der
freigegebenen Mockups. Das Frontend stellt Modelle dar, erfasst Interaktionen
und kommuniziert ueber REST/JSON. UML- und OCL-Semantik, Typpruefung und
fachliche Validierung bleiben im Backend.

Die verbindlichen Dateinamen und die jeweils fachlich verantwortlichen
Analyse-Dokumente stehen in `04-ui-ux-analysis/48-mockup-file-naming.md`.
Diese Zuordnung ist bei jeder Frontend-Umsetzung zusammen mit dem jeweiligen
F-Schritt zu lesen. Die M-Nummer allein reicht nicht als visuelle Referenz.
Die fachlichen F-Schritte beginnen erst nach B37 mit dem Status
`BACKEND_CONTRACT_READY`; rein visuelle F1-Arbeiten dürfen vorher vorbereitet
werden, dürfen aber keine Backendannahmen festschreiben.

## Einheitlicher Ablauf

1. Das freigegebene Mockup, seine Annotationen und Compliance-IDs werden gelesen.
2. Backend-Endpunkte, DTOs und strukturierte Fehler werden gegen den UI-Bedarf geprueft.
3. Komponenten, Feature-State und API-Adapter werden in der bestehenden Architektur erweitert.
4. Relevante Default-, Editing-, Loading-, Error-, Disabled-, Empty- und Confirmation-Zustaende werden umgesetzt.
5. Desktop, Tastaturbedienung und internes Scrollverhalten werden geprueft.
   Fuer F4 bis F11 sowie F12 ist die Umsetzung und Abnahme eines schmalen Viewports
   nicht verpflichtend. Sie gehoert nur dann zum Schrittumfang, wenn der
   jeweilige Schritt oder ein verbindliches Mockup sie ausdruecklich fordert.
6. Fachliche Interaktionen erhalten gezielte Komponenten- oder Integrationstests.
7. Vor dem Abschluss wird der Schritt mit allen tatsaechlich gelesenen
   Analyse- und Mockup-Dateien in
   `09-ocl-extension-analysis/53-frontend-step-analysis-file-traceability.md`
   dokumentiert. Nur erwartete oder indirekt zitierte Dateien gelten nicht als
   gelesen.

## Viewport-Abgrenzung ab F4

F1 bis F3 enthalten bereits responsive Grundlagen und entsprechende
Viewport-Pruefungen. Fuer die zukuenftigen Schritte F4 bis F11 sowie F12 ist der
Desktop-Viewport die verbindliche Implementierungs- und Abnahmebasis.

- Ein eigener schmaler oder mobiler Viewport muss nicht umgesetzt, getestet
  oder per Screenshot dokumentiert werden.
- Allgemeine Ueberlaufsicherheit, interne Scrollbereiche und lesbare Inhalte
  bleiben auch auf dem Desktop verpflichtend.
- Eine schmale Ansicht wird nur umgesetzt, wenn der betreffende F-Schritt
  oder ein kanonisches Fachmockup sie ausdruecklich als Akzeptanzkriterium
  nennt.
- Diese Abgrenzung ersetzt fuer F4 bis F11 sowie F12 allgemein formulierte Hinweise auf
  responsive oder schmale Ansichten, sofern dort keine ausdrueckliche
  fachliche Anforderung beschrieben ist.

## Schrittuebersicht

| Schritt | Inhalt | Backend-Abhaengigkeit | Mockup |
|---|---|---|---|
| F1 | Workspace-Rahmen und gemeinsame Interaktionsmuster | keine neue Fachlogik | M1, M14 |
| F2 | Generalisierung und abstrakte Klassen | B4 | M2 |
| F3 | Sichtbarkeit, Namespaces und Imports | B5 | M3 |
| F4 | Association-End-Metadaten | B6 | M4 |
| F5 | Qualifizierte und n-aere Associations | B7 | M5 |
| F6 | Association Classes, Aggregation und Composition | B8 | M6 |
| F7 | Operationssignaturen und Operation Invocation | B9 | M7 |
| F8 | Pre- und Postconditions | B9, B18 | M8 |
| F9 | Derived, Init, Body und Def | B19 | M9 |
| F10 | Enum-, Datatype- und strukturierte Werteditoren | B13, B50 | M10 |
| F11 | OCL-Profil und Compliance-Ergebnisse | B10-B17, B20, B25 | M11 |
| F12 | Integrierter Workflow und Accessibility-Abnahme | B22-B25 | M14 |
| F3N | Package-/Import-Command-Migration | B45-B46 | bestehende F3-Mockups |
| F4N | Association-Update-Command-Migration | B44-B46 | bestehende F4-Mockups |
| F5N | Object-Link-Command-Lifecycle | B41-B42, B44, B46 | bestehende F5-Mockups |
| F6N | Association-Class-Aggregat-Command-Migration und Nachabnahme | B41-B42, B44, B46, B48 | bestehende F6-Mockups |
| F7N | Operation-Delete-Nachabnahme | B49 | `delete-operation-modal.html` und bestehender F7-Workspace |
| F10N | Persistierte strukturierte Werttypen sowie Enumeration-, DataType-, Literal- und Value-Property-Delete-Nachabnahme | B43, B50, B51 | bestehende F10-Mockups, vier Delete-Mockups und Matrix 14, 15, 21, 22, 33, 34, 45, 47 |

**Nummerierungshinweis:** F12 ist die umbenannte Fortfuehrung des zuvor als
F14 bezeichneten Schritts. Scope, Backendabhaengigkeiten und Milestone `M14`
bleiben unveraendert; eine separate F14-ID existiert nicht mehr.

## Frontend-Schritt F1: Workspace-Rahmen und gemeinsame Muster

**Status:** `IMPLEMENTED_PENDING_VISUAL_ACCEPTANCE` am 28. August 2026.

**Ziel:** Die Baseline aus M1 und M14 wird als wiederverwendbarer Rahmen fuer
Navigation, Properties, Dialoge, Meldungen und responsive Sidebars umgesetzt.

- Enthalten sind Tabs, Formularraster, interne Scrollbereiche, Statusmeldungen und Dashboard-Ruecknavigation.
- Neue UML- oder OCL-Fachfunktionen sind nicht enthalten.
- Geprueft werden responsive Layouts, Fokusreihenfolge, Scrollverhalten und Tastaturzugriff.
- Der Schritt ist abgeschlossen, wenn alle nachfolgenden Fachseiten dieselben Muster verwenden koennen.
- Verbindliche Mockups: `workspace-ui-baseline.html`,
  `workspace-model-explorer.html`, `workspace-object-explorer.html` und
  `workspace-bottom-panel.html`.

Die drei spezialisierten Referenzen sind in allen Schritten F1-F11 und F12
verbindlich: `workspace-model-explorer.html` definiert Class-Diagram-Shell,
Explorer, Canvas-Erstellungsleiste und Properties-Rahmen;
`workspace-object-explorer.html` definiert die entsprechende objektbezogene
Ausprägung; `workspace-bottom-panel.html` definiert das gemeinsame Bottom
Panel für beide Diagramme und den OCL Editor.

**Tatsaechliches Ergebnis:** Das React-Frontend verwendet einen gemeinsamen
Workspace-Rahmen fuer Class Diagram und Object Diagram. Der Class Explorer
liest die additive `ProjectReadModelDto.explorer`-Projektion und bildet
Project Root, Packages, Import Roots, Classifiers, Herkunft und Read-only ab;
der Object Explorer bleibt objektbezogen. Suche und Auswahl sind mit dem
zentralen Selection-State synchronisiert. Die Canvas-Erstellungsleiste zeigt
die F1-verfuegbaren Aktionen; Enumeration und DataType bleiben bis F10
sichtbar deaktiviert. Das gemeinsame Bottom Panel besitzt in verbindlicher
Reihenfolge `Console`, `Diagnostics`, `Validation Results` und
`Invocation Results`, interne Scrollbereiche sowie leere und datengetriebene
Ergebniszustaende. Desktop- und schmale Layoutregeln sind gemeinsam definiert.
Fachsemantik und spaetere CRUD-Workflows wurden nicht vorweggenommen.
Typecheck, Lint ohne Fehler, 73 Frontendtests und Produktions-Build sind
erfolgreich. Die Playwright-/Screenshot-Abnahme fuer Desktop und schmalen
Viewport bleibt als letzter F1-Abnahmepunkt offen.

## Frontend-Schritt F2: Generalisierung und abstrakte Klassen

**Ziel:** Klassenhierarchien werden im Class Diagram erstellt, bearbeitet und UML-konform dargestellt.

- Enthalten sind Generalization-Werkzeug, Super-/Subklassenwahl, Mehrfachvererbung und abstrakte Klassen.
- Das Frontend fuehrt keine Vererbungsvalidierung durch, sondern zeigt strukturierte Backend-Fehler.
- Geprueft werden Erstellen, Bearbeiten, Loeschen, Cancel und Fehlerbestaetigung.
- Geerbte Features werden mit definierender Superclass schreibgeschuetzt
  angezeigt; explizite Redefinitionen und Namenskonflikte erhalten einen
  eigenen Aufloesungszustand.
- Das Umschalten auf `abstract` zeigt bestehende direkte Instanzen als
  blockierenden Backend-Fehler.
- Verbindliche Mockups: `class-properties-details.html`,
  `class-properties-generalizations.html`,
  `class-properties-generalizations-add-supertype.html`,
  `class-properties-generalizations-and-redefinitions.html` und
  `delete-generalization-modal.html`.

**Umsetzungsstand vom 28. August 2026:**
`IMPLEMENTED_PENDING_VISUAL_ACCEPTANCE`. Das Frontend rendert Abstract Classes
und UML-Generalization-Kanten, liest direkte Supertypen, Hierarchiereihenfolge,
geerbte Features und Redefinitionsziele aus dem Backend-Read-Model und nutzt
die revisionsgeschuetzten Command-Endpunkte fuer Abstract Class,
Generalizations und explizite Redefinitionen. Backendfehler erhalten Code und
Draft; die lokale Auswahl bleibt bestehen. TypeScript, Lint ohne Fehler, 75
Tests und Produktions-Build sind erfolgreich. Die Workspace-Shell wurde im
Browser geprueft; die abschliessende F2-Datenansicht fuer Desktop und schmalen
Viewport bleibt ohne laufendes Backend offen.

## Frontend-Schritt F3: Sichtbarkeit, Namespaces und Imports

**Ziel:** Visibility, Packages und Imports werden in die Class-, Association- und Invariant-Seiten integriert.

- Enthalten sind Visibility-Auswahl, Package-Auswahl, Importliste und qualifizierte fachliche Namen.
- Clientseitige Namensaufloesung ist nicht enthalten.
- Geprueft werden Namenskonflikte, unbekannte Referenzen, Empty State und schmaler Viewport.
- Verbindliche Mockups: `class-properties-details.html`,
  `workspace-model-explorer.html` und `project-imports.html`.

**Implementierungsstand vom 30. August 2026:**
`IMPLEMENTED`. Class-, Attribute- und
Operation-Visibility sowie Class-Namespace und qualifizierter Name werden aus
den Backendvertraegen dargestellt und gespeichert. Packages und Package
Imports werden ausschliesslich ueber die B45/B46-Command-Vertraege angelegt,
bearbeitet und nach Delete Impact entfernt. Der
Class-Diagram-Explorer zeigt Project Root, Package-/Importstruktur, Herkunft
und qualifizierte Namen. TypeScript, Tests und Produktions-Build sind
erfolgreich. Die F3N-Desktop-Abnahme belegt Create, Update, Save/Reload/Edit,
Validation, Not Found, Revision Conflict, Blocker, explizite Cascades und
Draft-Erhalt mit realen Backenddaten. Der schmale Viewport gehoert gemaess
aktueller Abgrenzung nicht zur Abnahme.

## Frontend-Schritt F4: Association-End-Metadaten

**Status:** `IMPLEMENTED` am 30. August 2026.

Der atomare Editor, der revisionsgeschuetzte Create-Command und der
strukturierte Delete-Impact-/Cascade-Workflow sind implementiert und durch
gezielte Komponenten- und API-Tests verifiziert. Die Desktop-Abnahme bei
1440 x 900 bestaetigt die gemeinsame Workspace-Shell, interne Scrollbereiche
und ueberdeckungsfreie Create-, Edit- und Delete-Zustaende.

Create, Update und Delete verwenden revisionsgeschuetzte Command-Vertraege.
Die noch offene Association-Update-Migration wurde durch F4N abgeschlossen.

**Ziel:** Die Association-Seite bearbeitet Rollenname, Multiplizitaet,
Navigierbarkeit, Ordered, Unique, ReadOnly, Subsets, Redefines und Union.

- Beide Ends bleiben eindeutig unterscheidbar und intern scrollbar.
- UML-Konsistenzregeln werden ausschliesslich vom Backend bewertet.
- Geprueft werden Edit, Save, Cancel, Delete und feldbezogene Serverdiagnosen.
- Verbindliche Mockups: `association-properties.html`,
  `create-binary-association-modal.html`, `delete-association-modal.html` und
  `object-properties-associations.html` fuer die resultierende Navigation im
  Objektdiagramm.

## Frontend-Schritt F5: Qualifizierte und n-aere Associations

**Status:** `IMPLEMENTED_LEGACY_CONTRACT_MIGRATION_REQUIRED` am 30. August 2026.

**Ziel:** Dynamische Association Ends, Qualifier und Links mit mehr als zwei Ends werden bedienbar.

- Enthalten sind End-Liste, Qualifier-Editor und strukturierte Link-Erfassung.
- Semantische Linkpruefung im Browser ist nicht enthalten.
- Geprueft werden End-Aenderungen, Qualifier-Werte, unvollstaendige Links und Diagrammlesbarkeit.
- Verbindliche Mockups: `association-properties-qualified-nary.html`,
  `create-association-modal.html`, `create-object-link-modal.html`,
  `object-link-association-sidebar.html` und
  `object-link-association-sidebar-ordered-unique.html`.

**Tatsaechliches Ergebnis:** Association Properties und der Create-Dialog
unterstuetzen dynamische Endlisten ab zwei Ends sowie Qualifier mit stabiler ID,
Typ und Reihenfolge. Der Object-Link-Dialog erzeugt strukturierte endbasierte
Links mit typisierten Qualifierwerten ueber den vorhandenen POST-Vertrag.
Bestehende n-aere Links wurden im urspruenglichen F5-Stand mangels
Update-Endpunkt bewusst schreibgeschuetzt dargestellt. B42 stellt diesen
Vertrag inzwischen bereit; F5N ersetzt den alten Create-/Delete-Pfad und
ergaenzt Update und strukturierten Delete. Class- und Object-Diagramm verwenden fuer mehr
als zwei Ends einen zentralen Beziehungsknoten mit einem Segment je End. Die
Semantikpruefung verbleibt im Backend; Association Classes und Aggregation
bleiben F6. Typecheck, Lint ohne Fehler, 83 Frontendtests und Produktions-Build
sind erfolgreich. Die Desktop-Abnahme bei 1440 x 900 bestaetigt Workspace-Shell,
interne Dialog-Scrollbereiche und fehlenden horizontalen Seitenueberlauf.

## Verbindliche visuelle und funktionale Vollstaendigkeitspruefung fuer F3N-F7N und F10N

Jeder Nachholschritt fuehrt zusaetzlich zur API-Migration eine
Feld-fuer-Feld-Abnahme gegen seine kanonischen Mockups durch.

- Jedes sichtbare, fachlich schreibbare Feld besitzt einen realen DTO-Pfad,
  einen initialen Backendwert und einen Command-Vertrag fuer die Speicherung.
- Ein vollstaendiger gueltiger Draft kann ausschliesslich ueber die UI
  ausgefuellt, gespeichert, erneut geladen und ohne Informationsverlust wieder
  bearbeitet werden.
- Pflichtfelder, optionale Felder, Listen, Auswahlwerte und dynamische
  Wiederholungen sind vollstaendig bedienbar; es verbleiben keine Platzhalter,
  unbegruendet deaktivierten Controls oder durch alte Backendluecken
  schreibgeschuetzten Zustaende.
- `OUT_OF_SCOPE`- oder fachlich read-only-Felder werden sichtbar als solche
  gekennzeichnet und nicht als funktionsfaehige Eingabe dargestellt.
- Loading, Empty, Validation Error, Revision Conflict, Disabled, Cancel,
  Success und Delete Confirmation werden mit realen beziehungsweise
  vertragstreuen Antworten geprueft. Ein Fehler behaelt alle eingegebenen
  Werte und markiert die strukturiert referenzierten Felder.
- Die Desktopansicht wird mit Playwright bei einem verbindlichen
  Referenzviewport geprueft und per Screenshot mit dem kanonischen Mockup
  verglichen. Labels, Eingaben, Listen, Modals, interne Scrollbereiche und
  Aktionen duerfen nicht ueberdecken, abgeschnitten oder unerreichbar sein.
- Der Test weist fuer jedes Feld nach: Anzeigen, Ausfuellen, Speichern,
  Reload/Persistenz, erneutes Bearbeiten und gegebenenfalls Loeschen.

Ein Nachholschritt ist nicht abgeschlossen, wenn nur der API-Client migriert
wurde, aber ein fachliches Mockup-Feld visuell fehlt oder funktional nicht
ausgefuellt und gespeichert werden kann.

## Frontend-Nachholschritt F3N: Package- und Import-Command-Migration

**Status:** `IMPLEMENTED` am 30. August 2026.

**Abhaengigkeiten:** B45, B46 und der implementierte sichtbare Umfang von F3.

**Ziel:** Alle Package- und Import-Mutationen verwenden ausschliesslich die
revisionsgeschuetzten Command-Vertraege.

- `POST/PUT .../commands/packages` fuer Create, Rename und Parent-Wechsel.
- `POST/PUT .../commands/imports` fuer Create und Update.
- Delete Impact und `DELETE .../commands/PACKAGE|IMPORT/{id}` fuer Loeschung.
- `expectedRevision`, neue Modellrevision, Draft-Erhalt, strukturierte Targets,
  Blocker und explizite Cascades werden vollstaendig dargestellt.
- Der Package-/Importbaum wird nach Erfolg autoritativ neu gelesen; das
  Frontend berechnet keine Namespace-, Alias-, Zyklus- oder Read-only-Semantik.

**Akzeptanz:** Keine produktive Frontendreferenz auf direkte Package-/Import-
Create-, Update- oder Delete-Routen; Tests fuer Erfolg, Validation, Not Found,
Revision Conflict, Delete Blocker, Cascade und unveraenderten Draft sind gruen.
Package-Name, Parent, Importquelle, Zielpackage, Alias und alle weiteren im
kanonischen F3-Mockup schreibbaren Felder bestehen die gemeinsame visuelle und
funktionale Feld-fuer-Feld-Abnahme einschliesslich Save und Reload.

**Tatsaechliches Ergebnis:** `modelCommandApi` stellt revisionsgeschuetzte
Create-/Update-Vertraege fuer Packages und Imports bereit. Package- und
Import-Properties senden vollstaendige Drafts, behalten sie bei Validation-,
Not-Found- und Revisionfehlern und laden nach Erfolg Projekt und Read Model
autoritativ neu. Package-Knoten sind im kanonischen Explorer auswaehlbar;
Package Name, Parent und qualifizierter Name sowie Import Target, importiertes
Package, Alias, Source, Provenienz, Status und importierte Projektion sind
vollstaendig dargestellt. Der gemeinsame Delete-Impact-Dialog trennt
Blocker von explizit auswaehlbaren Cascades. Direkte Package-/Import-Legacy-
Aufrufe und die dazugehoerigen unbenutzten Frontend-Methoden wurden entfernt.
96 Frontendtests, Typecheck, Lint ohne Fehler und
der Produktions-Build sind erfolgreich. Acht Desktop-Screenshots und ein
Playwright-Lauf mit realem Backend belegen die visuelle und funktionale
Vollstaendigkeit; Details stehen in der Frontend-Traceability.

## Frontend-Nachholschritt F4N: Association-Update-Command-Migration

**Status:** `IMPLEMENTED` am 30. August 2026.

**Abhaengigkeiten:** B44, B46 und F4.

**Ziel:** Der atomare Association-Editor speichert Name, Ends, Qualifier,
Multiplizitaet, Navigierbarkeit und Ordered/Unique ueber
`PUT .../commands/associations/{associationId}`.

- Das Request enthaelt `expectedRevision` und den vollstaendigen
  `UmlAssociationDto`-Draft.
- Erfolg aktualisiert die Modellrevision und liest die autoritative Projektion.
- Validation- und Revisionfehler behalten den gesamten lokalen Draft und
  markieren die strukturiert referenzierte Association beziehungsweise die
  betroffenen End-/Qualifierfelder.
- Create und Delete bleiben auf den bereits verwendeten Command-Vertraegen.

**Akzeptanz:** Keine produktive Frontendreferenz auf den direkten Legacy-
Association-Update; Tests fuer atomaren Erfolg, Field Diagnostics, Not Found,
Revision Conflict und Draft-Erhalt sind gruen. Association-Name sowie alle
End-, Multiplizitaets-, Navigierbarkeits-, Ordered-/Unique-, Qualifier-,
Subset-, Redefinition- und Union-Felder des F4-Scopes bestehen die gemeinsame
visuelle und funktionale Feld-fuer-Feld-Abnahme einschliesslich Save und
Reload. Aggregation und Association Class bleiben F6 und muessen dort dieselbe
Vollstaendigkeitspruefung bestehen.

**Ergebnis:** Der Editor sendet den vollstaendigen `UmlAssociationDto`-Draft
mit `expectedRevision` an den B44/B46-Command, laedt nach Erfolg die
autoritative Projektion und behaelt Drafts bei Validation, Not Found und
Revision Conflict. Association-, End- und Qualifierziele werden entsprechend
der strukturierten Backendreferenz markiert. Direkte Association-Update- und
Delete-Legacy-Aufrufe sowie der lokale Inline-Editor in Class Properties sind
entfernt. 100 Frontendtests, Lint ohne Fehler, Produktions-Build und zwei reale
Desktop-Playwright-Laeufe mit acht Screenshots bestaetigen alle F4-Felder,
Save/Reload/Edit, Delete Impact, Fokus und Ueberlaufsicherheit. Aggregation und
Association Class bleiben unveraendert F6.

## Frontend-Nachholschritt F5N: Object-Link-Command-Lifecycle

**Status:** `IMPLEMENTED` am 30. August 2026.

**Abhaengigkeiten:** B41, B42, B44, B46, F4N und F5.

**Ziel:** Normale und n-aere Object Links verwenden den vollstaendigen
revisionsgeschuetzten Create-, Update-, Impact- und Delete-Lifecycle.

- Create verwendet `POST .../commands/object-model/links`.
- Direkte Bearbeitung verwendet `PUT .../commands/object-model/links/{linkId}`;
  die bisherige pauschale Read-only-Beschraenkung entfaellt.
- Delete laedt zuerst den Impact und sendet nur explizit ausgewaehlte,
  erlaubte Cascades an `DELETE .../commands/object-model/links/{linkId}`.
- Multiplizitaets-, Qualifier-, Ordered-/Unique- und Revisionkonflikte werden
  aus Backenddiagnosen dargestellt; Endzuordnungen und Qualifierdraft bleiben
  bei Fehlern erhalten.
- Association-Class-spezifische Erzeugung und Bearbeitung bleibt F6, nutzt dort
  jedoch denselben B41/B42-Lifecycle.

**Akzeptanz:** Keine produktive Frontendreferenz auf direkte Object-Link-
Create-/Delete-Routen; normale und n-aere Links sind commandbasiert editierbar;
Tests fuer Erfolg, Validation, Not Found, Revision Conflict, Blocker, Cascade
und Draft-Erhalt sind gruen. Association-Auswahl, alle dynamischen
Endzuordnungen, Object-Auswahlen, Qualifierwerte und erlaubte Cascades bestehen
die gemeinsame visuelle und funktionale
Feld-fuer-Feld-Abnahme einschliesslich Save, Reload und erneutem Edit.
Association-Class-Instanzdaten bleiben F6 und muessen dort dieselbe
Vollstaendigkeitspruefung bestehen.

**Tatsaechliches Ergebnis:** Normale binaere und n-aere Object Links verwenden
ausschliesslich die B41/B42/B46-Commands fuer Create, Update, Delete Impact und
Delete. Association-Auswahl, alle dynamischen Endzuordnungen und typisierten
Qualifierwerte werden als vollstaendiger Draft gesendet; nach Erfolg wird die
autoritative Projektion geladen. Validation und Revision Conflict erhalten den
Draft und markieren strukturierte End-/Qualifierziele. Ordered/Unique wird
backendautorisiert read-only dargestellt. Delete zeigt Endkontext, Blocker und
nur explizit waehlbare Cascades. Direkte Object-Link-Create-/Delete-Legacy-
Aufrufe wurden aus dem Frontend entfernt. 108 Frontendtests, Lint ohne Fehler,
Produktions-Build und eine reale Desktop-Playwright-Abnahme mit neun
Screenshots bestaetigen Create, Save, Reload, Edit, Navigation, Revision
Conflict, Delete Impact, Delete und Ueberlaufsicherheit. Association-Class-
Instanzdaten bleiben unveraendert F6.

## Verbindliche visuelle und funktionale Vollstaendigkeitspruefung fuer F6-F10

F6 bis F10 unterliegen derselben Feld-fuer-Feld-Abnahme wie F3N bis F6N.
Jeder Schritt muss fuer alle zugeordneten kanonischen Mockups nachweisen:

- alle fachlich schreibbaren Felder sind sichtbar, erreichbar und mit realen
  Backendwerten initialisiert;
- ein vollstaendiger gueltiger Draft kann in der UI ausgefuellt, ueber einen
  durch B46 freigegebenen Command gespeichert, neu geladen und erneut
  bearbeitet werden;
- Create, Update und alle im Scope vorgesehenen Delete-/Impact-Workflows sind
  funktional, atomar und erhalten bei Fehlern den vollstaendigen Draft;
- strukturierte Validation-, Not-Found-, Conflict- und Revision-Conflict-
  Ergebnisse markieren die betroffenen Felder und bieten ihre vorgesehenen
  Navigationsziele;
- Loading, Empty, Disabled, Cancel, Confirmation, Error und Success sind fuer
  die tatsaechlichen Workflows geprueft;
- fachlich read-only oder `OUT_OF_SCOPE` bleibende Werte sind eindeutig
  gekennzeichnet und werden nicht als funktionsfaehige Eingaben dargestellt;
- Desktop-Playwright prueft Anzeigen, Ausfuellen, Speichern, Reload, erneutes
  Editieren und Loeschen. Screenshots belegen, dass Panels, Tabs, Editoren,
  Listen, Modals und Bottom-Panel-Ergebnisse nicht ueberdecken, abgeschnitten
  oder unerreichbar sind;
- es verbleiben keine Platzhalter, unbegruendet deaktivierten Controls oder
  Legacy-Mutationsaufrufe.

Eine rein visuelle Umsetzung oder eine reine API-Client-Anbindung reicht nicht
fuer den Abschluss eines Schritts.

## Frontend-Schritt F6: Association Classes, Aggregation und Composition

**Status:** `IMPLEMENTED` (31. August 2026, durch F6N nachabgenommen)

**Ziel:** Relationship Kind, Association Class und Linkobjekt-Properties werden in die Association-Seite integriert.

- No-Cycle- und No-Share-Regeln bleiben im Backend.
- Properties und Diagramm muessen denselben Beziehungstyp zeigen.
- Geprueft werden Typwechsel, Linkobjekt-Erstellung, Loeschen und Konfliktmeldungen.
- Verbindliche Mockups: `association-class-properties.html`,
  `association-class-instance.html` und `create-object-link-modal.html`.
- Vollstaendigkeitsabnahme: Relationship Kind, Aggregation Kind je End,
  Association-Class-Auswahl und -Erzeugung, Attribute und Operations im
  Create-Dialog, Linkobjekt-Identitaet, Object-/Endzuordnungen und erlaubte
  Delete-Cascades muessen visuell und funktional Save, Reload, Edit und Delete
  bestehen. Composition-Ownership und Cycle-Konflikte kommen strukturiert aus
  dem Backend und duerfen den Draft nicht verwerfen.

**Historisches F6-Teilergebnis:** Relationship Kind, vorhandene
Association-Class-Auswahl, Aggregation Kind je End, Diagrammnotation,
gemeinsame Linkobjektidentitaet, Endzuordnungen, Owned-Value-Projektion sowie
Link-Update, Delete Impact und Delete waren bereits umgesetzt. Class-/Feature-
Erzeugung plus Bindung sowie Link plus Association-Class-Object und Slots
blieben bis B48 blockiert und wurden nicht durch mehrere Requests emuliert.
B48 stellte die atomaren Aggregate bereit; F6N hat deren Migration und die
Feld-fuer-Feld-Abnahme inzwischen erfolgreich abgeschlossen.

## Frontend-Nachholschritt F6N: Association-Class-Aggregat-Command-Migration

**Status:** `IMPLEMENTED` (31. August 2026)

**Abhaengigkeiten:** B41, B42, B44, B46, B48, F5N und der implementierte
sichtbare Umfang von F6.

**Ziel:** Die durch B48 freigeschalteten Association-Class-Aggregate werden
vollstaendig an das Frontend angebunden und visuell sowie funktional erneut
abgenommen. F6N ersetzt keine bestehende UML-/OCL-Semantik und arbeitet keine
spaeteren Frontendschritte vor.

- Das Erzeugen einer Association Class mitsamt Attributen, Operationen und
  Association-Bindung verwendet ausschliesslich
  `POST .../commands/associations/{associationId}/association-class`.
- Das Erzeugen und Aktualisieren einer Association-Class-Instanz mitsamt
  Object Link, Association-Class-Object und typisierten Slots verwendet
  ausschliesslich `POST/PUT .../commands/object-model/association-class-instances[/{linkId}]`.
- Das Frontend sendet jeweils den vollstaendigen Aggregatedraft und laedt nach
  Erfolg die autoritative Modell- beziehungsweise Snapshotprojektion neu.
- Validation, Not Found, fachlicher Conflict und Revision Conflict erhalten
  den vollstaendigen Draft und markieren die strukturiert referenzierten
  Association-, Class-, Feature-, Link-, End-, Object-, Slot- und Feldziele.
- Bestehende F6-Funktionen fuer Relationship Kind, Aggregation/Composition,
  Link-Update, Delete Impact und Cascade werden nicht neu implementiert,
  muessen aber als Regression weiterhin funktionieren.
- Ersetzte Mehrfach-Command-, Legacy- oder begruendet deaktivierte
  Association-Class-Pfade werden aus dem produktiven Frontend entfernt,
  sofern keine weiteren Verbraucher bestehen. Backend-Legacy-Code bleibt B47.

**Verbindliche Mockups:** `association-class-properties.html`,
`association-class-instance.html` und `create-object-link-modal.html`. Fuer die
betroffenen Class- und Object-Workspaces gelten zusaetzlich
`workspace-model-explorer.html`, `workspace-object-explorer.html` und bei
Diagnostics oder Ergebnissen `workspace-bottom-panel.html`.

**Feld- und Aktionsabnahme:** Mindestens Association-Class-Name, Attribute,
Operations, Association-Bindung, Object-/Endzuordnungen, Linkobjektidentitaet
und typisierte Owned Values muessen ueber die UI angezeigt, ausgefuellt,
gespeichert, neu geladen und erneut bearbeitet werden. Validation und Revision
Conflict muessen den Draft erhalten. Delete Impact und Cascade werden fuer den
bereits bestehenden B42-Lifecycle regressionsgeprueft. Nicht anwendbare Punkte
werden als `NOT_APPLICABLE` dokumentiert; unbegruendete Platzhalter,
Read-only- oder Disabled-Zustaende sind nicht zulaessig.

**Verifikation:** Typecheck, Lint, relevante Frontendtests und
Produktions-Build sind gruen. Eine reale Desktop-Playwright-Abnahme prueft
Create, Save, Reload, Edit, Validation, Revision Conflict, strukturierte
Navigation und den anwendbaren Delete-Lifecycle. Screenshots belegen Haupt-,
Modal-, Fehler- und Erfolgszustaende ohne Ueberdeckung oder Abschneiden. Mit
`rg` wird nachgewiesen, dass ersetzte Legacy- und Mehrfachaufrufe nicht mehr im
produktiven Frontend vorkommen.

**Traceability und Abschluss:** F6N erhaelt einen eigenen Eintrag in
`53-frontend-step-analysis-file-traceability.md` mit ausschliesslich
tatsaechlich gelesenen Dateien, Lesetiefe, Verwendungszweck,
Contract-Matrix-Eintraegen 1, 2 und 20 sowie Feld-, Test-, Playwright- und
Screenshotnachweisen. F6N und der fachliche F6-Umfang duerfen erst bei
vollstaendiger Erfuellung auf `IMPLEMENTED` gesetzt werden. Erst danach darf
B47 die nachweislich unbenutzten Backend-Legacy-Writes entfernen.

**F6N-Ergebnis:** Die atomare Association-Class-Erstellung bindet Class,
Attribute und Operations ueber den B48-Model-Command. Create und Update einer
Association-Class-Instanz senden Object Link, Association-Class-Object und
typisierte Slots als gemeinsamen Snapshotdraft. Nach Erfolg wird die
autoritative Projektion neu geladen; Backendvalidation und echter
`STALE_SNAPSHOT_REVISION` erhalten den vollstaendigen Draft. Zieltests,
gesamte Frontendtests, Lint, Produktions-Build und die reale
Desktop-Playwright-Abnahme sind erfolgreich. Vier Screenshots belegen Create,
Instanz-Create, persistierten Success und Revision Conflict ohne horizontalen
Ueberlauf oder leere Hauptbereiche. Matrix 1, 2 und 20 sind damit auch
frontendseitig abgenommen. Produktiver Backendcode wurde nicht veraendert.

## Frontend-Schritt F7: Operationssignaturen und Invocation

**Status:** `IMPLEMENTED` (31. August 2026, durch F7N nachabgenommen)

**Ziel:** Die Class-Seite trennt die Definition einer Operation klar von ihrem Aufruf.

- Enthalten sind Parameterreihenfolge, Direction, Rueckgabetyp, Query, Zielobjekt, Argumente und Ergebnis.
- Ausfuehrungssemantik ist nicht enthalten.
- Geprueft werden Signatur-CRUD, fehlende Argumente, Loading, Erfolg und Fehler.
- Verbindliche Mockups: `class-properties-operations.html`,
  `object-diagram-operation-invocation.html` und
  `workspace-bottom-panel.html`.
- Vollstaendigkeitsabnahme: Operationsname, Visibility, Static, Query,
  Abstract, Rueckgabetyp sowie alle Parameter mit Name, Typ, Direction und
  Reihenfolge muessen Create, Save, Reload, Edit und Delete bestehen.
  Invocation muss Receiver, alle Argumente und Out-/Result-Werte vollstaendig
  erfassen und Erfolg beziehungsweise Rollback im Bottom Panel darstellen.

**F7-Ergebnis vom 31. August 2026:** Signatur-Create/-Update, autoritativer
Reload, erneutes Editieren, Validation und Revision Conflict mit vollstaendig
erhaltenem Draft sowie die revisionsgeschuetzte Object Invocation sind
implementiert. Das Bottom Panel zeigt Result, Out Values, Lifecycle,
Diagnostics und Contracts strukturiert. Die reale Desktop-Abnahme bestaetigt
Create `201`, Update `200`, Validation `400`, Revision Conflict `409` und
Invocation `200` mit `21 : Integer`. Der generische Operation-Delete ist
jedoch nicht ausfuehrbar: `DELETE .../commands/OPERATION/{operationId}` liefert
`400 OWNER_REQUIRED`, weil der Service `classId` verlangt, das
`DeleteCommandRequestDto` dieses Feld aber nicht transportiert. Es wurde kein
Legacy-Fallback eingebaut. F7 bleibt bis B49 und zur erneuten
Delete-Abnahme blockiert und wird nicht auf `IMPLEMENTED` gesetzt.

**B49-Backendfreigabe vom 31. August 2026:** Der Owner wird nun serverseitig
aus der stabilen `operationId` aufgeloest; Matrix 30 ist backendseitig wieder
`SUPPORTED`. Der historische `OWNER_REQUIRED`-Nachweis bleibt als
Gap-Evidenz erhalten. F7 ist noch nicht abgeschlossen: Impact, erfolgreicher
Delete, Not Found, Blocker/ungueltige Cascade-Auswahl und Revision Conflict
mit Draft-Erhalt muessen im realen Desktop-Workflow erneut abgenommen werden.
Status bis dahin: `PARTIAL_DELETE_REACCEPTANCE_REQUIRED`.

## Frontend-Nachholschritt F7N: Operation-Delete-Nachabnahme

**Status:** `IMPLEMENTED` (31. August 2026).

**Abhaengigkeiten:** B49, Matrixeintrag 30 und der bereits implementierte
sichtbare Umfang von F7.

**Ziel:** Der durch B49 korrigierte generische Operation-Delete wird im realen
Frontendworkflow erneut abgenommen. F7N beschraenkt sich auf Delete Impact,
Confirmation, Delete und die zugehoerigen Fehlerzustaende. Die bereits
erfolgreich abgenommenen F7-Vertraege fuer Signatur-Create/-Update und
Invocation werden nur regressionsgeprueft und nicht neu implementiert.

**Verbindliche Referenzen:** `delete-operation-modal.html` mit
`04-ui-ux-analysis/37-delete-operation-modal.md`, der Operationsbereich aus
`class-properties-operations.html`, `workspace-model-explorer.html` und das
Bottom Panel nur fuer tatsaechlich dargestellte Diagnostics. Vertraglich
verbindlich sind Matrixeintrag 30 und
`63-b49-operation-delete-owner-resolution.md`.

**Integration:**

- Impact verwendet ausschliesslich
  `GET .../commands/delete-impact/OPERATION/{operationId}`.
- Delete verwendet ausschliesslich
  `DELETE .../commands/OPERATION/{operationId}` mit `expectedRevision` und
  `cascadeReferenceIds` im bestehenden `DeleteCommandRequestDto`.
- Das Frontend sendet keine `classId` und verwendet keinen Legacy-Fallback.
  Owner Class, Referenzen, Blocker und Cascade-Erlaubnisse stammen
  ausschliesslich aus der Backendprojektion.
- Erfolg uebernimmt die neue Modellrevision, laedt die autoritative
  Read-Projektion neu und aktualisiert Explorer, Diagramm, Selection und
  Properties Panel konsistent.
- `ELEMENT_NOT_FOUND`, `DELETE_BLOCKED`, `INVALID_CASCADE_SELECTION` und
  `STALE_MODEL_REVISION` behalten stabile Elementreferenzen, fachliche Namen,
  aktuellen Impact und lokalen Dialogzustand beziehungsweise Draft.

**Visuelle und funktionale Nachabnahme:** Im Desktop-Viewport sind Impact
Loading, loeschbarer Default, Confirmation, Blocker, Not Found, unzulaessige
Cascade, Revision Conflict und Success nachzuweisen. Eine unreferenzierte
Operation muss ohne `classId` geloescht werden, nach Reload entfernt bleiben
und erneut in allen betroffenen Workspacebereichen fehlen. Body-, Contract-,
Invariant-, Definition-, Redefinitions- und Attributausdrucksreferenzen sind
mit Backendnamen darzustellen und ueber ihre strukturierten Ziele navigierbar.
Fehler duerfen keine Modellmutation und keinen Verlust des Dialogzustands
verursachen.

**Migration und Abgrenzung:** Verbleibende `classId`-Annahmen oder
Legacy-Operation-Delete-Aufrufe werden aus dem produktiven Frontend entfernt,
sofern repositoryweit kein weiterer Verbraucher existiert. Backend-Legacycode
wird nicht geloescht; dies bleibt B47 vorbehalten. F7N fuegt keine neue
Operations-, Invocation-, Contract- oder OCL-Semantik hinzu.

**Tests und Nachweise:** Typecheck, Lint, relevante Frontendtests und
Produktions-Build muessen gruen sein. Eine reale Desktop-Playwright-Abnahme
prueft Impact, erfolgreichen Delete, autoritativen Reload, Not Found,
Blocker/ungueltige Cascade-Auswahl und echten Revision Conflict mit
Draft-Erhalt. Screenshots belegen Confirmation, Blocker, Conflict und Success.
Signatur-Create/-Update und Invocation erhalten einen gezielten
Regressionstest. `rg` weist nach, dass kein `classId`-Workaround und kein
Legacy-Operation-Delete im produktiven Frontend verbleibt.

**Traceability und Abschluss:** F7N erhaelt einen eigenen Eintrag in
`53-frontend-step-analysis-file-traceability.md` mit ausschliesslich
tatsaechlich gelesenen Dateien, Lesetiefe, Verwendungszweck, Matrixeintrag 30,
allen Delete-Zustaenden sowie Test-, Playwright- und Screenshotnachweisen.
F7N und F7 duerfen nur gemeinsam auf `IMPLEMENTED` gesetzt werden, wenn die
reale Nachabnahme vollstaendig erfolgreich ist. Erst danach blockiert der
Operation-Delete-Verbraucher B47 nicht mehr.

**F7N-Ergebnis:** Der Operation-Delete verwendet ausschliesslich Matrixvertrag
30 und sendet `expectedRevision` sowie `cascadeReferenceIds`, aber keine
`classId`. Die reale Desktop-Abnahme bestaetigt Impact, Confirmation,
persistierten Delete mit autoritativem Reload, strukturierte und navigierbare
Blocker, atomar abgewiesene ungueltige Cascade-Auswahl, Not Found sowie echten
Revision Conflict mit erhaltenem Dialogzustand und aktualisiertem Impact.
Gezielte Tests (9/9), die gesamte Frontendsuite (128/128), TypeScript/Build und
Lint ohne Fehler sind erfolgreich; die bekannte OCL-Editor-Hook-Warnung bleibt
unveraendert. Fuenf Desktop-Screenshots zeigen keine Ueberdeckung und der
horizontale Seitenueberlauf ist 0. Produktiver Backendcode wurde nicht
veraendert. F7 und F7N sind damit `IMPLEMENTED`; der Operation-Delete-
Verbraucher blockiert B47 nicht mehr.

## Frontend-Schritt F8: Pre- und Postconditions

**Status:** `IMPLEMENTED` (31. August 2026)

**Ziel:** Operation Contracts werden in Operation-Properties und Validation Results eingebunden.

- Enthalten sind Pre-/Post-Tabs, Parameterkontext, `result`, `@pre`, Before/After-Vergleich und Fehlernavigation.
- Lokale Contract-Auswertung ist nicht enthalten.
- Geprueft werden CRUD, Source Locations und verletzte Pre- beziehungsweise Postconditions.
- Verbindliche Mockups: `operation-contracts.html` und
  `workspace-bottom-panel.html`.
- Vollstaendigkeitsabnahme: Pre-/Post-Art, Name, OCL-Ausdruck,
  Parameterkontext, `result` und `@pre` muessen Create, Save, Reload, Edit und
  Delete bestehen. Source Ranges, Compilefehler, verletzte Contracts und
  Before-/After-Ergebnisse muessen sichtbar sowie zu Class Diagram, Object
  Diagram oder OCL Editor navigierbar sein.

**Ergebnis:** Der Operation-Editor besitzt typisierte Signature-, Pre- und
Post-Tabs; Body bleibt als F9-Abgrenzung deaktiviert. Contracts mit stabiler
ID, Name, Kind, Ausdruck und Enabled-Status werden atomar ueber den
Operation-Command gespeichert, autoritativ neu geladen, erneut editiert und
nach Bestaetigung geloescht. Backendfehler behalten den vollstaendigen Draft.
Invocation Results unterscheiden `BLOCKED`, `ROLLED_BACK` und `SUCCEEDED`,
zeigen Before/Candidate After, Contractstatus und Source Diagnostics und
navigieren zum definierenden Classifier. Reale Desktop-Abnahmen bestaetigen
PRE-Blockierung ohne Body-Ausfuehrung sowie POST-Rollback ohne Commit. Details
und Eingabereferenzen stehen in der F8-Traceability.

## Frontend-Schritt F9: Derived, Init, Body und Def

**Ziel:** Diese OCL-Kontexte werden in die bestehenden Class- und Invariant-Arbeitsbereiche integriert.

- Enthalten sind Context Kind, OCL-Editor, erwarteter Typ, Read-only-Darstellung und Initialwertvorschau.
- Typableitung und Evaluation bleiben im Backend.
- Geprueft werden Kontextwechsel, Apply-Bestaetigung, Fehlernavigation und schreibgeschuetzte Werte.
- Verbindliche Mockups: `class-properties-attributes.html`,
  `class-properties-definitions.html`, `class-properties-operation-body.html`
  und `package-properties-definitions.html`.
- Vollstaendigkeitsabnahme: Init-, Derive- und Body-Ausdruecke sowie Property-
  und Operation-Definitions mit Owner, Context Kind, Signatur, erwartetem Typ
  und Source muessen Create, Save, Reload, Edit und Delete bestehen.
  Read-only-Vorschauen, Zyklus-/Typdiagnostics, Source-Navigation und
  Delete-Blocker muessen vollstaendig dargestellt werden.

**Implementierungsstand vom 31. August 2026:** `IMPLEMENTED`. Attribute
unterstuetzen Stored, Init und Derived ueber die revisionsgeschuetzten
B44-Commands. Query Bodies werden im vollstaendigen Operation-Draft
gespeichert. Class- und Package-Definitions unterstuetzen Property- und
Operation-Signaturen, geordnete Parameter, Create/Update sowie strukturierten
Delete Impact und Delete. Autoritative Reloads, erneutes Editieren,
Backendvalidation und Revision Conflicts mit Draft-Erhalt sind durch Tests und
reale Desktop-Playwright-Abnahmen belegt. Derived Object Values verwenden die
Read-Projektion und bleiben schreibgeschuetzt; `null` und `invalid` werden
getrennt dargestellt. Produktiver Backendcode wurde nicht veraendert.

## Frontend-Schritt F10: Strukturierte Werteditoren

**Ziel:** Enum-, Datatype-, Tuple- und Collection-Werte erhalten typgerechte Darstellung und Eingabe.

- Freitext wird dort vermieden, wo der Backend-Vertrag eine strukturierte Auswahl liefert.
- Typkompatibilitaetsregeln werden nicht dupliziert.
- Geprueft werden unbekannte Literale, verschachtelte Werte, Empty/Invalid und responsive Formulare.
- Objekt-Slots verwenden typspezifische Editoren fuer Boolean, Integer, Real,
  String, Enumeration, DataType, optionale Werte und gespeicherte Collections.
- Statische Attribute erzeugen keinen Objekt-Slot. Derived Slots sind
  schreibgeschuetzt. Geerbte Slots zeigen ihre definierende Superclass.
- `null` ist ein zulaessiger fehlender Wert; `invalid` ist ein sichtbarer
  Auswertungsfehler. Set, Bag, Sequence und OrderedSet behalten ihre jeweilige
  Ordered-/Unique-Semantik und werden nicht clientseitig ineinander umgedeutet.
- Verbindliche Mockups: `classifier-type-picker.html`,
  `datatype-properties.html`, `create-object-modal.html`,
  `object-diagram-typed-attribute-values.html`,
  `object-diagram-workspace.html` und
  `class-properties-static-attribute-values.html`.
- Vollstaendigkeitsabnahme: Enumeration und stabile Literale inklusive
  Reihenfolge, DataType-Details und Value Properties, Tuple-Werte,
  `Set`/`Bag`/`Sequence`/`OrderedSet`, Boolean, Integer, Real, String,
  Enumeration-, DataType- und optionale Slotwerte sowie statische
  Classifierwerte muessen mit ihren typgerechten Editoren Create, Save,
  Reload, Edit und Delete bestehen. `null`, `invalid`, derived, inherited und
  static muessen visuell und funktional korrekt unterschieden werden.

**Historischer Implementierungsstand vor F10N vom 31. August 2026:** `PARTIALLY_IMPLEMENTED`
(`B50_AVAILABLE_F10N_REQUIRED`). Enumeration- und DataType-Create/Update, stabile
Literalreihenfolge, Classifier-Typwahl, statische Classifierwerte sowie
typgerichtete primitive, optionale, Enumeration-, DataType-, Tuple- und
Collection-Editoren sind im Frontend umgesetzt. Object Create und Slot Update
verwenden die revisionsgeschuetzten Snapshot-Commands; Enumeration-Slots sind
mit Save, Reload, erneutem Editieren und Revision-Conflict-Draft-Erhalt real
abgenommen. Die vollstaendige F10-Abnahme ist jedoch blockiert: Der reale
Class-Command lehnt Attribute der Typen `Money` und `Sequence(String)` jeweils
mit `400 TYPE_ERROR` (`Unknown type`) ab. Dadurch koennen DataType- und
Collection-Werte trotz vorhandener UI nicht als persistierte Object-Slots
erzeugt und end-to-end geprueft werden. B50 hat diese Backend-Luecke
inzwischen geschlossen; die erneute reale Feld-fuer-Feld-Abnahme erfolgt als
F10N. F10 darf erst nach erfolgreichem F10N auf `IMPLEMENTED` gesetzt werden.

## Frontend-Nachholschritt F10N: Strukturierte Werttypen und Typ-Lifecycle

**Status:** `IMPLEMENTED` (1. September 2026; B50-Scope und Delete-Erweiterung
vollstaendig abgenommen).

**Abhaengigkeiten:** B43, B50 und B51, Ergebnisse
`58-b43-enumeration-lifecycle.md`,
`64-b50-persisted-structured-value-types.md` und
`65-b51-datatype-property-delete-lifecycle.md`, Matrix 14, 15, 21, 22, 33,
34, 45 und 47 sowie der bereits implementierte sichtbare Umfang von F10.

**Ziel:** F10N integriert die durch B50 geschlossenen persistierten DataType-,
Tuple- und Collection-Workflows und ergaenzt den zu F10 gehoerenden Delete fuer
ganze Enumerations und DataTypes sowie einzelne Enumeration-Literale und
DataType-Value-Properties. Die Pre-/Postcondition-Semantik von F8 wird nicht
veraendert.

**Verbindliche Mockups und Layouts:**
`classifier-type-picker.html`, `datatype-properties.html`,
`class-properties-static-attribute-values.html`, `create-object-modal.html`,
`object-diagram-typed-attribute-values.html`, `object-diagram-workspace.html`,
`delete-enumeration-modal.html`, `delete-enumeration-literal-modal.html`,
`delete-datatype-modal.html` und `delete-datatype-value-property-modal.html`
und fuer den objektbezogenen Rahmen `workspace-object-explorer.html`.
Class-Diagram-Anteile folgen `workspace-model-explorer.html`, Object-Diagram-
Anteile `workspace-object-explorer.html`; ein betroffenes Bottom Panel folgt
`workspace-bottom-panel.html`. Umgesetzt und abgenommen wird nur Desktop.

**Funktionaler Scope:**

- DataType-, Tuple-, Set-, Bag-, Sequence- und OrderedSet-Typen lassen sich
  ueber die reale Class-/Attribute-UI erstellen, speichern, neu laden und
  erneut bearbeiten.
- Statische strukturierte Classifierwerte bestehen Save, Reload und Edit und
  erzeugen weiterhin keinen Object-Slot.
- Object Create und Slot Update bestehen Save, autoritativen Reload und Edit
  fuer jede strukturierte Typart, einschliesslich verschachtelter Werte.
- Set/OrderedSet-Eindeutigkeit sowie Bag-/Sequence-Duplikate und Reihenfolge
  werden nur aus Backendprojektion und Backenddiagnostics dargestellt; das
  Frontend berechnet diese Semantik nicht.
- `null`, `invalid`, derived, inherited und static bleiben fachlich getrennt.
- Enumeration Properties und DataType Properties bieten den Delete des ganzen
  Classifiers; Literal- und Value-Property-Zeilen bieten ihren eigenen Delete.
- Jeder Delete laedt zuerst den autoritativen Impact. Loading, Blocked, Ready,
  Confirmation, Not Found, Revision Conflict und Success folgen den vier
  verbindlichen Delete-Mockups.
- Whole-Classifier- und Kind-Delete verwenden getrennte B43-/B50-/B51-
  Vertraege. Der Full-DataType-Update darf das B51-Struktur-Gate nicht umgehen.

**Fehler- und Revisionsabnahme:** Fuer mindestens einen verschachtelten
DataType-/Tuple-/Collection-Draft sind Validation Error und echter Model-
beziehungsweise Snapshot-Revision Conflict auszufuehren. Der vollstaendige
lokale Draft bleibt sichtbar und erneut speicherbar. `fieldPath` und stabile
Classifier-, Attribute-, DataType-, Object- und Slotreferenzen navigieren zum
betroffenen UI-Feld. Keine fehlgeschlagene Mutation darf die autoritative
Projektion teilweise veraendern.

**Reale Delete-Vertraege:**

- `GET .../commands/delete-impact/ENUMERATION/{enumerationId}` und
  `DELETE .../commands/ENUMERATION/{enumerationId}`,
- `GET .../commands/delete-impact/DATATYPE/{dataTypeId}` und
  `DELETE .../commands/DATATYPE/{dataTypeId}`,
- `GET .../commands/delete-impact/ENUMERATION_LITERAL/{literalId}` und
  `DELETE .../commands/ENUMERATION_LITERAL/{literalId}` aus B43,
- `GET .../commands/datatypes/{dataTypeId}/properties/{propertyId}/delete-impact`
  und `DELETE .../commands/datatypes/{dataTypeId}/properties/{propertyId}` aus
  B51.

Delete sendet die aktuelle `expectedRevision` und nur vom Backend erlaubte
`cascadeReferenceIds`. Blocker behalten stabile Element-, Feature-, Object-,
Slot- und Source-Ziele. Cancel und Fehler erhalten vorhandene Properties-
Drafts; Erfolg laedt Read Model, Explorer, Canvas und Typkatalog neu.

**Migration und Abgrenzung:** F10N verwendet nur die durch B43, B50 und B51
bestaetigten revisionsgeschuetzten Command-Endpunkte und autoritativen
Read-Projektionen.
Workarounds fuer unbekannte Typen, clientseitige Typparser oder lokale
Collection-Semantik werden entfernt, sofern sie existieren und keine anderen
Verbraucher besitzen. Produktiver Backendcode und Backend-Legacycode werden
nicht veraendert; dessen Retirement bleibt B47.

**Tests und Nachweise:** Typecheck, Lint, relevante Frontendtests und
Produktions-Build muessen gruen sein. Desktop-Playwright prueft fuer alle
sechs B50-Matrixzeilen Anzeige, Eingabe, Save, Reload und erneutes Editieren
sowie Validation und Revision Conflict. Fuer alle vier Delete-Zielarten prueft
Playwright Impact Loading, Blocker-Navigation, Ready/Confirmation, Not Found,
echten Revision Conflict mit Draft-Erhalt, Success und autoritativen Reload.
Mindestens ein DataType-Fall zeigt einen verschachtelten Tuple- oder
Collection-Blocker. Screenshots belegen Classifierwahl, verschachtelten Editor,
Object Create, persistierten
Slot, statischen Wert, Fehler und Erfolg ohne Ueberdeckung oder Abschneiden.
`rg` weist nach, dass kein ersetzter F10-Legacy-Aufruf oder Typ-Workaround im
produktiven Frontend verbleibt.

**Traceability und Abschluss:** F10N erhaelt bei seiner Umsetzung einen
eigenen Eintrag in `53-frontend-step-analysis-file-traceability.md`. Erfasst
werden nur tatsaechlich gelesene Dateien mit Lesetiefe und Verwendungszweck,
Matrix 14, 15, 21, 22, 33, 34, 45 und 47, die Feld-/Aktionspruefung sowie Test-,
Playwright- und Screenshotnachweise. F10N und F10 duerfen erst nach erfolgreicher
B50- und Delete-Nachabnahme gemeinsam `IMPLEMENTED` sein. Erst danach ist der
F10-Verbraucher fuer B47 abgeschlossen.

**Historisches F10N-B50-Teilergebnis vom 31. August 2026:** Der rekursive
Typeditor deckt DataType, Tuple, Set, Bag, Sequence und OrderedSet ab und sendet die unveraenderte
API-v1-Typsyntax an die B50-Commands. Statische strukturierte
Classifierwerte, Object Create und Slot Update bestehen Save, autoritativen
Reload und erneutes Editieren. Bag-Duplikate bleiben erhalten; ein doppelter
Set-Wert wird ausschliesslich durch das Backend mit `INVALID_SLOT_VALUE` und
`fieldPath=value` abgelehnt. Ein real erzeugter
`STALE_SNAPSHOT_REVISION`-Konflikt erhaelt den verschachtelten Money-Draft.
Komplexe persistierte Werte werden im Object-Diagram-Knoten rekursiv lesbar
statt als JavaScript-Objektkoerzierung dargestellt. DataType Delete Impact
war `NOT_APPLICABLE`, weil die vier Delete-Mockups damals noch nicht dem
F10N-Scope zugeordnet waren. Die fokussierte Suite ist mit 21/21, die Gesamtsuite mit 155/155
Tests gruen; Lint hat keine Fehler, der Produktions-Build und die reale
Desktop-Playwright-Abnahme sind erfolgreich. Damit ist der B50-Teil von F10N
implementiert; Matrix 14, 15, 21, 33, 34 und 45 sind `SUPPORTED`. Die
nachtraeglich zugeordneten Delete-Ablaufe und Matrix 22/47 muessen noch real im
Frontend implementiert und abgenommen werden; bis dahin bleiben F10N und F10
`PARTIALLY_IMPLEMENTED`.

**F10N-Delete-Abschluss vom 1. September 2026:** Whole-Enumeration-,
Enumeration-Literal-, Whole-DataType- und DataType-Value-Property-Delete sind
ueber die realen B43-/B50-/B51-Impact- und Command-Vertraege integriert.
Loading, Ready, Blocker, Confirmation, `ELEMENT_NOT_FOUND`, echter
`STALE_MODEL_REVISION` mit autoritativer Impact-Nachladung, Success und Reload
sind im Desktop-Frontend abgenommen. Literal-Delete sendet den verpflichtenden
Enumeration-Owner; Value-Property-Delete verwendet ausschliesslich den
verschachtelten B51-Vertrag. Die fokussierte F10N-Delete-Suite ist mit 14/14,
die Gesamtsuite mit 162/162 Tests gruen. Lint hat keine Fehler, der
Produktions-Build und die reale Desktop-Playwright-Abnahme mit zehn Screenshots
sind erfolgreich. F10N und F10 sind damit `IMPLEMENTED`; der F10-Verbraucher
ist fuer B47 abgeschlossen.

## Frontend-Schritt F11: OCL-Profil und Compliance-Ergebnisse

**Ziel:** Nutzer erkennen den unterstuetzten Sprachumfang und die genaue Ursache eines Validation-Ergebnisses.

- Enthalten sind Profilanzeige, Feature-Status, gruppierte Diagnosen, Source Locations und Navigation zu Fachobjekten.
- `Validation Results` führt Invariantenverletzungen, statische Befunde aus
  persistierten Pre-/Postconditions und Definitionsfehler revisionsgebunden
  zusammen. Einträge navigieren über stabile Referenzen in Class Diagram,
  Object Diagram oder OCL Editor; konkrete Contract-Laufzeitergebnisse bleiben
  unter `Invocation Results`.
- Reference-Test-Administration fuer Endnutzer ist nicht enthalten.
- Geprueft werden grosse Ergebnismengen, Filter, Empty/Loading/Error und Namen statt IDs.
- Verbindliche Mockups: `ocl-compliance-feature-display.html` und
  `workspace-bottom-panel.html`.

**Ergebnis vom 1. September 2026:** `IMPLEMENTED`. F11 zeigt das reale
Compliance-Profil aus `GET /api/v1/ocl/profile` mit Feature-Status,
Profilidentitaet, Suche, Detailansicht und Runtime Limits. Validation Results
bilden Loading, Error, Empty und strukturierte Ergebnisse mit stabilen
Navigationszielen ab. Invariant Create/Update/Delete verwenden ausschliesslich
die revisionsgeschuetzten B46-Command-Vertraege; Validation- und Revision-
Fehler behalten den vollstaendigen Draft und markieren das betroffene Feld.
Die autoritative Projektprojektion wird nach erfolgreichen Mutationen neu
geladen. Die zusaetzlich betroffenen Fachmockups
`invariant-properties.html` und `delete-invariant-modal.html` sind damit
ebenfalls abgenommen. Lint, Produktions-Build und 171/171 Frontendtests sind
gruen; die reale Desktop-Playwright-Abnahme belegt Profil, Validation,
Save/Reload/Edit, Revision Conflict sowie Delete Confirmation/Success mit
sechs Screenshots. Ersetzte Invariant-Legacy-Aufrufe und lokale
Invariant-Mutationshelfer wurden aus dem Frontend entfernt. Produktiver
Backendcode wurde nicht veraendert.

## Frontend-Schritt F12: Integrierter Workflow und Accessibility-Abnahme

**Ziel:** Die in F1-F11 und den Nachholschritten implementierten Workflows
werden als gemeinsamer Desktop-Arbeitsplatz visuell, funktional und
barrierebezogen abgenommen. F12 fuegt keine UML-/OCL-Semantik und keinen neuen
Backendvertrag hinzu.

- Verbindlich sind die drei kanonischen Workspace-Referenzen, die M1-Baseline
  und das Quality Gate M14.
- Geprueft werden die gemeinsamen Default-, Selected-, Editing-, Loading-,
  Empty-, Disabled-, Error-, Confirmation- und Success-Muster.
- Die Abnahme umfasst Fokusreihenfolge, Tastatursteuerung, Dialogfokus,
  zugaengliche Namen, interne Scrollbereiche, lange Fachnamen, stabile
  Diagrammknoten und horizontale Desktop-Ueberlaufsicherheit.
- Die bereits schrittspezifisch abgenommenen Create-, Update-, Validation-,
  Conflict- und Delete-Vertraege bleiben autoritativ; F12 fuehrt keine
  parallelen Mutationspfade ein.

**Ergebnis vom 1. September 2026:** `IMPLEMENTED`. Die gemeinsame Shell besitzt
einen fokussierbaren Skip-Link, kontextbezogene Quick Help fuer alle drei
Hauptviews mit gebundenem Dialogfokus und Fokusrueckgabe sowie vollstaendige
Pfeil-/Home-/End-Steuerung im Bottom Panel. Ein einheitlicher 3-px-Fokusring,
40-px-Hauptbedienflaechen und lesbare Formularschrift erfuellen die M14-
Baseline. Lange Class-, Object-, Attribute- und Invariantnamen umbrechen in
stabilen UML-Knoten; Validation-Badges ueberdecken die Bezeichnung nicht. Die
OCL-Ansicht verwendet wieder die volle Workspace-Breite. Die reale
Desktop-Playwright-Abnahme bei 1440 x 900 bestaetigt Class Diagram, Object
Diagram, OCL Editor, Quick Help und Validation Results ohne Browserfehler,
doppelte IDs, unbenannte sichtbare Controls oder horizontalen Ueberlauf.
Lint, Produktions-Build und 173/173 Frontendtests sind gruen. Produktiver
Backendcode wurde nicht veraendert. B47 kann nun die separate
Legacy-Retirement-Abnahme beginnen.

## Zusammenfassung

F1 schafft die gemeinsame Oberflaechenbasis. F2 bis F10 setzen UML- und
Contract-Editoren um. F11 integriert OCL- und Compliance-Ergebnisse. Das durch
B21 ausgeschlossene optionale State-Machine- und Operation-Trace-Paket besitzt
im aktuellen Profil keinen eigenen Frontendschritt.
F3N bis F6N migrieren die bereits vorhandenen F3-F6-Oberflaechen auf die nach
ihnen eingefuehrten V2- beziehungsweise B48-Command-Vertraege. F7N nimmt den
durch B49 korrigierten Operation-Delete real nach und schliesst danach F7.
F10N nimmt die durch B50 geschlossenen persistierten DataType-, Tuple- und
Collection-Workflows real nach und ergaenzt die nachtraeglich entworfenen
Whole-Enumeration-, Literal-, Whole-DataType- und Value-Property-Delete-
Ablaufe auf Basis von B43, B50 und B51. Erst danach schliesst F10N den F10-
Scope vollstaendig.
F12 nimmt den vollstaendigen Workflow ab; danach darf B47 nachweislich unbenutzte
Legacy-Mutationspfade entfernen. Kein
Schritt verlagert UML- oder OCL-Fachlogik in den Browser.
