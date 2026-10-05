# Full OCL and UML Implementation Coordination

## Zweck

Diese Datei verbindet die getrennten Mockup-, Backend- und Frontend-Plaene.
Fachlogik bleibt im Backend; Darstellung, Eingabe und Interaktion bleiben im
Frontend.

## Verbindliche Plaene

| Spur | Dokument | Kennzeichnung |
|---|---|---|
| Mockups | `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md` | M1 bis M14 |
| Backend | `09-ocl-extension-analysis/15-full-ocl-uml-implementation-plan.md` | B1 bis B57 |
| Frontend | `09-ocl-extension-analysis/17-full-ocl-uml-frontend-implementation-plan.md` | F1 bis F11, F12 sowie F3N-F7N und F10N |

**Nummerierungshinweis:** Der bisherige Frontend-Schritt F14 wird ab dem
1. September 2026 als F12 gefuehrt. Die Umbenennung aendert weder seinen Scope
noch die Backenduebergaben oder den zugeordneten Milestone `M14`.

## Kanonische Layoutreferenzen

1. `assets/mockups/workspace-model-explorer.html`: Top-Navigation,
   Class-Diagram-Explorer, Canvas, Erstellungsleiste und
   Properties-Panel-Rahmen.
2. `assets/mockups/workspace-object-explorer.html`: Top-Navigation,
   Object-Diagram-Explorer, Object-/Object-Link-Canvas und
   Object-Properties-Rahmen.
3. `assets/mockups/workspace-bottom-panel.html`: gemeinsames Bottom Panel mit
   `Console`, `Diagnostics`, `Validation Results` und `Invocation Results`.

Alle Frontendschritte F1-F11, F12 sowie F3N-F7N und F10N verwenden diese Referenzen. Abweichungen sind nur
für fachliche Panelinhalte, aktive Zustände, Modals und schmale responsive
Ansichten zulässig.

## Verbindliche Referenz-Traceability

Jeder kuenftige Backend- und Frontend-Schritt protokolliert vor seinem
Abschluss alle tatsaechlich gelesenen Analysedokumente, Mockups und sonstigen
fachlichen Referenzdateien aus `use-web-analysis`.

- Backend-Schritte pflegen
  `09-ocl-extension-analysis/46-backend-step-analysis-file-traceability.md`.
- Frontend-Schritte pflegen
  `09-ocl-extension-analysis/53-frontend-step-analysis-file-traceability.md`.
- Vollstaendige und abschnittsweise Lektuere werden unterschieden; bei
  abschnittsweiser Lektuere wird der Verwendungszweck genannt.
- Nur tatsaechlich geoeffnete Dateien werden aufgenommen. Eine Nennung im
  Prompt, Plan oder in der Documentation Map allein reicht nicht aus.
- Mockups werden mit ihrem repository-relativen Dateipfad separat erfasst.
- Erst waehrend des Schritts erstellte Ergebnisdokumente werden als Ergebnisse
  gekennzeichnet und nicht rueckwirkend als Eingabereferenz ausgegeben.
- Die Abschlusszusammenfassung muss auf den jeweiligen Traceability-Eintrag
  verweisen und darf keine abweichende Referenzliste enthalten.

**Viewport-Abgrenzung ab F4:** Fuer F4 bis F11, F12 sowie F4N/F5N ist nur die Desktop-Ansicht
verbindlicher Implementierungs- und Abnahmeumfang. Ein schmaler Viewport wird
nur dann umgesetzt, wenn der jeweilige Frontend-Schritt oder ein kanonisches
Mockup ihn ausdruecklich fordert. Tastaturbedienung, interne Scrollbereiche und
allgemeine Ueberlaufsicherheit bleiben davon unberuehrt.

## Stage-Gates

1. Die verpflichtenden Mockups M1 bis M11 und M14 werden fachlich freigegeben.
2. B1 bis B3 schaffen die normative Baseline, Reference-Cases und das Compliance-Harness.
3. Ein Frontend-Fachschritt beginnt erst, wenn sein Mockup freigegeben und der benoetigte Backend-Vertrag getestet ist.
4. Backend und Frontend werden danach in vertikalen Funktionspaketen umgesetzt.
5. B25 und F12 bilden die erste gemeinsame Compliance- und Workflow-Abnahme.
6. B26 bis B32 schließen die verbleibenden fachlichen Reference-Gaps; B33 nimmt den Null-Gap-Stand ab.
7. B34 bis B37 gleichen die danach konkretisierten Mockups gegen die
   Backendvertraege ab und setzen `BACKEND_CONTRACT_READY` fuer F1-F11 und F12.
8. B35 weist zusätzlich S3 für persistierte Class-/Package-Definitionen mit
   Namespace Ownership nach. B37 darf F9 erst nach einer expliziten S3-
   Entscheidung vollständig freigeben; S1 und S2 bleiben ebenfalls sichtbar.
9. B36 stellt die additive API-v1-Command-Schicht mit Modellrevision,
   Draft-erhaltenden Fehlern, Delete-Impact und expliziten Cascades bereit.
   B40 hat danach auch Definition-Delete in diesen Vertrag aufgenommen.
10. Die erste B37-Abnahme vom 28. August 2026 ist wegen S1-S3 nicht bestanden.
    B38 (Feature-Redefinition), B39 (statische Features/Classifier-Werte) und
    B40 (persistierte Class-/Package-Definitionen) haben diese Lücken
    geschlossen. Die erneute B37-Abnahme ist bestanden;
    `BACKEND_CONTRACT_READY` ist für F1-F11 und F12 gesetzt.
11. Die F5-Integration hat strengere Mutationsanforderungen sichtbar gemacht,
    die durch die bisherige B37-Klassifikation nicht erfasst wurden. B41 bis
    B45 vereinheitlichen Snapshot-, Object-Link-, Enumeration-, Feature-,
    Association-, Package- und Import-Commands. B46 hat die strenge
    V2-Abnahme durchgefuehrt und `BACKEND_CONTRACT_READY_V2` gesetzt.

## Gemeinsame Umsetzungsreihenfolge

| Paket | Backend | Frontend | Mockup | Ergebnis |
|---|---|---|---|---|
| Grundlagen | B1-B3 | F1 | M1, M14 | Baseline, Vertraege und stabiler Workspace-Rahmen |
| Typmodell | B4-B5, B13, B50 | F2-F3, F10 | M2, M3, M10 | Vererbung, Sichtbarkeit, Typwahl und persistierte strukturierte Werttypen |
| Associations | B6-B8 | F4-F6 | M4-M6 | UML-konforme Association-Bearbeitung |
| Operationen | B9, B18-B19 | F7-F9 | M7-M9 | Signaturen, Contracts, Derived/Init/Body/Def |
| OCL-Engine | B10-B17 | F11 | M11 | vollstaendigere Semantik und nachvollziehbare Ergebnisse |
| Haertung | B20, B22-B25 | F12 | M14 | End-to-End-Workflow und erste Compliance-Abnahme |
| Gap-Abschluss | B26-B33 | nur bei Vertragsänderung neuer Frontendschritt | M11/M14, neue Mockups nur bei neuem Workflow | vollständige Gap-Schließung und Profilfolgeversion |
| Mockup-Vertragsabgleich | B34-B37 | F1-F11 und F12 nach Vertragsfreigabe | kanonischer Index aus `04-ui-ux-analysis/48-mockup-file-naming.md` | vollständige DTO-/API-Abdeckung aller verpflichtenden Mockup-Zustände |
| Mutation-Contract-Haertung | B41-B46 | F5-F11 und F12 entsprechend Abhaengigkeit | bestehende kanonische Mockups | Revisionen, Atomaritaet, Draft-Erhalt und vollstaendige CRUD-Lifecycles |
| Association-Class-Aggregathaertung | B48 | F6N | bestehende F6-Mockups | atomare Association-Class-Create-/Update-Vertraege und erneute F6-Abnahme |
| Operation-Delete-Vertrag | B49 | F7-Nachabnahme | `delete-operation-modal.html` | Owner-Aufloesung aus stabiler Operation-ID und erneute Delete-Abnahme |
| Strukturierte persistierte Werttypen | B50 | F10N | F10-Mockups fuer Typwahl, Object Create, Slots und statische Werte | DataType-, Tuple- und Collection-Typaufloesung sowie persistierte Roundtrips |
| Classifier-Delete-Nachabnahme | B43, B50 | F10N | `delete-enumeration-modal.html`, `delete-datatype-modal.html` | Whole-Classifier-Impact, Blocker, Revision Conflict und Delete mit autoritativem Reload |
| Kind-Delete-Nachabnahme | B43, B51 | F10N | `delete-enumeration-literal-modal.html`, `delete-datatype-value-property-modal.html` | Literal-/Property-Impact, rekursive Wert-/OCL-Blocker und revisionsgeschuetzter Delete |
| Imperative Operationskoerper | B57 | nachfolgender Frontendschritt | neues Operation-Body-/Invocation-Mockup erforderlich | atomare SOIL-Subset-Ausfuehrung, Lifecycle-Diff, Rollback und strukturierte Diagnostics |
| Command-Migration und Legacy-Retirement | B47 nach Frontendmigration | F3N-F7N, F10N sowie F8-F12 | bestehende kanonische Mockups | ausschliessliche Command-Nutzung und Entfernung nachweislich unbenutzter Legacy-Writes |

### Nachtraegliche Backend-Haertung B41-B46

Die B37-Abnahme bleibt als historischer Nachweis fuer die damals verwendeten
Kriterien bestehen. Sie bewertete einzelne Writes bereits dann als
`SUPPORTED`, wenn ein fachlich passender Endpoint vorhanden war. Fuer die
weitere Frontendintegration gilt zusaetzlich:

- B41 ist abgeschlossen. Neue schreibende Object-/Slot-/Object-Link-Create-
  Workflows in F5, F6, F10 und F12 verwenden die revisionsgeschuetzten
  `/commands/object-model`-Vertraege.
- B42 ist abgeschlossen. F5/F6/F12 verwenden fuer direkte Linkbearbeitung
  `PUT .../commands/object-model/links/{linkId}` und fuer Loeschung den
  zweistufigen Impact-/Delete-Vertrag. Association-Class-Identitaeten sind
  ausdrueckliche Cascades; weitere Links auf das Linkobjekt bleiben Blocker.
- B43 ist abgeschlossen. F10/F12 erhalten revisionsgeschuetztes Enumeration-
  CRUD, stabile Literal-IDs, autoritative Literalreihenfolge sowie
  referenzbewussten Enumeration-/Literal-Delete-Impact.
- B44 ist abgeschlossen. F4-F10/F12 verwenden fuer Attribute und Operations
  `POST/PUT .../commands/classes/{classId}/...` sowie fuer vollstaendige
  Association-Aenderungen `PUT .../commands/associations/{associationId}`.
  Die Commands liefern Modellrevision, Draft-erhaltende Diagnostics und
  stabile Feature-, End- und Qualifierreferenzen.
- B45 ist abgeschlossen. F3/F9/F10/F12 erhalten revisionsgeschuetzte
  Package-/Import-Commands fuer Rename, Parent-Wechsel, Update und Delete.
  Delete Impact liefert enthaltene Elemente, explizite Cascades sowie
  nicht cascadefaehige Typ-, Generalization- und OCL-/Importreferenzen.
- B46 ist abgeschlossen und setzt `BACKEND_CONTRACT_READY_V2`. Die strenge
  Abnahme weist fuer B41-B45 Revisionen, Atomaritaet, Draft-Erhalt,
  strukturierte Fehler, Persistenz und Legacy-/Command-Konsistenz nach.

Read-only-Zustaende und bereits implementierte, getestete Frontendteile bleiben
dadurch gueltig. Ein Frontendschritt darf jedoch keinen der oben genannten
fehlenden Write-Workflows durch clientseitige Ersatzsemantik simulieren.

**F1-Ergebnis vom 28. August 2026:** Der gemeinsame React-Workspace-Rahmen
verwendet die drei kanonischen Layoutreferenzen. Der Class Explorer konsumiert
den freigegebenen `GET /api/v1/projects/{projectId}/read-model`-Vertrag; der
Object Explorer bleibt objektbezogen. Das Bottom Panel ist fuer Class Diagram,
Object Diagram und OCL Editor gemeinsam. Spaetere UML-/OCL-Fachschritte wurden
nicht vorweggenommen; Enumeration und DataType bleiben bis F10 deaktiviert.

**F2-Ergebnis vom 28. August 2026:** Class Details und Generalizations nutzen
das B35-Read-Model sowie die revisionsgeschuetzten B36/B38-Commands. Direkte
Supertypen, Mehrfachvererbung, Abstract Class, geerbte schreibgeschuetzte
Features und explizite Redefinitionen werden backendgestuetzt dargestellt und
geaendert. Die fachlichen Vertraege sind integriert; die visuelle Abnahme mit
geladenen Backenddaten bleibt offen.
Der Implementierungsstand ist `IMPLEMENTED_PENDING_VISUAL_ACCEPTANCE`, bis die
Desktop- und Narrow-Viewport-Screenshots per Playwright abgenommen sind.

**F3-Ergebnis vom 28. August 2026:** Visibility, Namespaces und Imports nutzen
die bestehenden B5/B35/B36-Vertraege. Class-, Attribute- und
Operation-Visibility, Class-Package und qualifizierte Namen werden im
Class-Properties-Workflow bearbeitet. Der kanonische Explorer zeigt Package-
und Importbaeume; Package- und Import-Commands verwenden die realen
Backendendpunkte. Importwurzeln und importierte Classifier sind als read-only
gekennzeichnet und besitzen fachliche Detailansichten. Clientseitige
Namensaufloesung wurde nicht eingefuehrt. Der Implementierungsstand ist nach
der Desktop- und Narrow-Viewport-Pruefung mit laufenden Backenddaten
`IMPLEMENTED`.

### Verbindliche Mockup-Nachtraege fuer das Frontend

Die nach M1-M11 ergaenzten oder konsolidierten Mockups erzeugen keine neuen
Frontend-Schrittnummern. Sie konkretisieren die bestehenden F-Schritte:

| Frontend-Schritt | Verbindliche Mockup-Gruppe |
|---|---|
| F1 | Workspace-Baseline, Class Model Explorer, Object Explorer und Bottom Panel |
| F2 | Class Details, Generalizations, geerbte Features und Redefinitionen |
| F3 | Class Details, Package-Navigation und Project Imports |
| F4-F6 | Association Properties, Qualified/n-ary, Association Class sowie Object-Link-Properties |
| F7-F9 | Operations, Invocation, Contracts, Bodies und Class-/Package-Definitions |
| F10 | Classifier-Auswahl, Datatype Properties, typisierte Objekterstellung und Objekt-Slots |
| F11 | OCL-Compliance-Anzeige und strukturierte Bottom-Panel-Ergebnisse |
| F12 | alle Create-, Delete-, Error-, Loading-, Empty- und Confirmation-Zustaende des kanonischen Index |

Die konkreten HTML-Dateien und ihre Primaerdokumente sind verbindlich in
`04-ui-ux-analysis/48-mockup-file-naming.md` aufgefuehrt. Ein F-Schritt gilt
nur dann als abgeschlossen, wenn auch die dort zugeordnete Mockup-Gruppe
beruecksichtigt wurde.

B20 stellt F11 zusätzlich die stabilen Profil-IDs `OCL-PROFILE-013` und
`OCL-PROFILE-015` als `NOT_SUPPORTED` bereit. Die DTO-Struktur bleibt
unverändert; die Featureliste ist variabel und wird über IDs ausgewertet.

B21 dokumentiert die Exclude-Entscheidung für das optionale Paket. M12 und M13
sind im aktuellen Zielprofil `NOT_REQUIRED`; eigene Frontendschritte sind dafuer
nicht vorgesehen. F11 kann
`OCL-PROFILE-012` (`NOT_SUPPORTED`) und `OCL-PROFILE-016` (`OUT_OF_SCOPE`)
anzeigen. Eine spätere Include-Entscheidung erfordert zuerst neue Mockups und
anschließend eigene State-Machine- und Operation-Trace-Verträge.

B25 übergibt F11 und F12 die Profil-ID `use-web-ocl-2.4-subset-v2`. Der
Endpoint und `OclComplianceProfileDto` bleiben API-v1-kompatibel. Das Frontend
muss die veröffentlichten Statuswerte auswerten; insbesondere darf es
`PARTIAL` nicht wie `SUPPORTED` darstellen.

B33 ersetzt diese Profil-ID kompatibel durch `use-web-ocl-2.4-subset-v3`.
Endpoint, DTO-Struktur, Feature-IDs und `apiVersion = v1` bleiben unveraendert.
F11 und F12 muessen deshalb nur die Profil-ID dynamisch anzeigen; es entsteht
kein neuer Frontend- oder Mockup-Schritt.

### Übergabe B37 an F1-F11 und F12

B37 übergibt eine versionierte Contract-Matrix mit jedem verpflichtenden
Mockup-Zustand, dem verantwortlichen F-Schritt, dem Endpoint/DTO-Vertrag und
den strukturierten Fehlercodes. Ein Eintrag ist entweder `SUPPORTED` oder
begründet `FRONTEND_ONLY`; `UNCLEAR` ist nicht zulässig. Erst diese Übergabe
setzt `BACKEND_CONTRACT_READY`. Loading und rein visuelle Empty-/Confirmation-
Darstellungen bleiben Frontendzustände. Fachliche Werte wie `null`, `invalid`,
Blocker, Read-only-Herkunft und Modellrevision stammen aus dem Backend.

Die Übergabe ist **freigegeben**: S1/B38, S2/B39 und S3/B40 sind abgeschlossen,
und `verify-b37-contract-acceptance.ps1` bestätigt alle 46 kanonischen
Mockup-Verträge mit PASS. F1-F11 und F12 dürfen die in Matrix und API-/DTO-Referenz
dokumentierten Verträge implementieren; `OUT_OF_SCOPE`-Profilbereiche bleiben
weiterhin ausgeschlossen.

## Uebergabevertrag

Jeder sichtbare Backend-Schritt dokumentiert DTOs, Fehlercodes, fachliche
Bezeichner sowie Loading-, Empty- und Fehlerfaelle. Der zugehoerige
Frontend-Schritt verwendet diesen Vertrag und dupliziert keine UML- oder
OCL-Validierungslogik.

### Übergabe B4 an F2

B4 stellt für F2 den bestehenden Endpunkt
`PUT /api/v1/projects/{projectId}/classes/{classId}` bereit. F2 darf darüber
`name`, `abstractClass` und `superClassIds` gemeinsam speichern. Erfolgreiche
Antworten liefern das normalisierte `UmlClassDto`; Zyklen, unbekannte oder
doppelte Oberklassen, geerbte Featurekonflikte und direkte Instanzen beim
Wechsel auf abstrakt werden als strukturierte `ApiErrorDto`-Antworten
zurückgegeben. Das Frontend zeigt diese Diagnosen an, bildet die Regeln aber
nicht selbst nach.

### Übergabe B5 an F3

B5 erweitert `UmlModelDto` um `packages` und `imports`. `UmlClassDto` liefert
`visibility`, `packageId` und den read-only Wert `qualifiedName`; Attribute und
Operationen liefern ebenfalls `visibility`. F3 kann Packages und Imports über
die neuen REST-Endpunkte anlegen, Imports entfernen und Feature-Sichtbarkeit
über die jeweiligen Update-Endpunkte speichern. Der Resolver, Importzyklen,
Namensmehrdeutigkeit und Zugriffsregeln bleiben vollständig im Backend.

### Uebergabe B45 an F3, F9, F10 und F12

B45 ergaenzt die historischen Create-/Read-Vertraege um
`PUT .../commands/packages/{packageId}` und
`PUT .../commands/imports/{importId}`. `PACKAGE` und `IMPORT` verwenden den
gemeinsamen revisionsgeschuetzten Delete-Impact-/Delete-Vertrag. Erfolgreiche
Mutationen liefern die neue Modellrevision; Fehler behalten Draft, stabile
Elementreferenzen und fachliche Namen. Package-Unterbaeume und verschobene
Classifier werden aus stabilen IDs neu projiziert. Cascades sind nur nach
expliziter Auswahl aller erlaubten Reference-IDs moeglich; verbleibende Typ-,
Generalisierungs-, Definition- oder OCL-Referenzen blockieren. Der Importbaum
liefert Alias, Provenienz und `readOnly` weiterhin backendseitig. B46 hat diese
Vertraege streng abgenommen; `BACKEND_CONTRACT_READY_V2` ist gesetzt.

### Uebergabe B46 an F5 bis F11 und F12

Die V2-Abnahme hatte alle 46 kanonischen Mockup-Vertraege mit 45
`SUPPORTED` und einem `FRONTEND_ONLY` freigegeben. Die spaetere reale
F10-Abnahme hat diese Aussage fuer Matrix 14, 15, 21, 33, 34 und 45 widerlegt:
persistierte DataType-, Tuple- und Collection-Typen sind
`MISSING_SEMANTICS` und werden durch B50 bearbeitet. Neue schreibende
Frontendworkflows verwenden die
revisionsgeschuetzten Command-Endpunkte. Direkte Legacy-Endpunkte bleiben
kompatibel und verwenden dieselben Domain Services, besitzen aber nicht den
vollstaendigen V2-Umschlag aus Revision, Draft und strukturiertem
Mutationsergebnis. F10 bleibt bis B50 und seiner realen Nachabnahme blockiert;
die uebrigen Freigaben bleiben unveraendert. Das optionale B21-Paket bleibt
gemaess Zielprofil ausgeschlossen.

### Nachholreihenfolge F3N bis F10, B49-B50 und B47

1. F3N migriert Package- und Import-Create/Update/Delete auf B45-Commands.
2. F4N migriert das Association-Update auf den atomaren B44-Command.
3. F5N migriert normale und n-aere Object Links auf B41/B42 Create, Update,
   Delete Impact und Delete.
4. F6N migriert die durch B48 bereitgestellten atomaren Association-Class-
   Aggregate und wiederholt die visuelle und funktionale F6-Abnahme fuer
   Class/Features/Bindung sowie Link/Linkobjekt/Slots.
5. Jeder Nachholschritt prueft alle zugeordneten kanonischen Mockup-Felder
   visuell und funktional: vollstaendig ausfuellen, speichern, neu laden,
   erneut bearbeiten, Fehler mit erhaltenem Draft anzeigen und loeschen. Eine
   reine API-Client-Migration reicht nicht fuer den Abschluss.
6. Desktop-Playwright und Screenshots weisen nach, dass alle Controls,
   dynamischen Listen, Modals, Meldungen und Aktionen sichtbar, erreichbar und
   frei von Ueberdeckung oder Abschneiden sind.
7. F6-F10 fuehren dieselbe Feld-fuer-Feld-Abnahme durch: alle schreibbaren
   Mockupfelder mit realen Backenddaten ausfuellen, speichern, neu laden,
   erneut bearbeiten und loeschen; strukturierte Fehler behalten den Draft.
   Desktop-Playwright und Screenshots sind fuer jeden Schritt verpflichtend.
8. F6-F11 und F12 verwenden von Beginn an nur die durch B46 beziehungsweise spaetere
   reale Feldabnahmen freigegebenen Command-Vertraege fuer Mutationen.
9. F7 bleibt fuer Operation Delete bis B49 blockiert. Nach B49 werden nur
   Delete Impact, erfolgreicher Delete, Blocker/Cascade, Not Found und
   Revision Conflict erneut real abgenommen; erst danach darf F7
   `IMPLEMENTED` werden.
10. F10 bleibt fuer persistierte DataType-, Tuple- und Collection-Attribute
    sowie deren Classifier-/Slotwerte bis B50 blockiert. Nach B50 werden in F10N
    Create, Save, Reload, Edit, Validation, Revision Conflict und strukturierte
    Werte fuer alle betroffenen F10-Mockups real nachabgenommen.
11. F10N bindet zusaetzlich die nachtraeglich entworfenen Whole-Enumeration-,
    Literal-, Whole-DataType- und Value-Property-Delete-Dialoge an B43/B50/B51
    an und prueft Impact, Blocker, Navigation, Not Found, Revision Conflict,
    Delete und autoritativen Reload.
12. Erst nach Abschluss dieser Frontendmigration einschliesslich F6N, B49,
    B50, B51 und dem vollstaendigen F10N startet
    B47. B47 entfernt nur
   Legacy-Controllerpfade, DTOs und Adapter, fuer die ein repositoryweiter
   Usage-Nachweis null produktive Verbraucher zeigt. Gemeinsam genutzte Domain
   Services und Fachsemantik werden nicht geloescht.

Bis B47 abgeschlossen ist, bleiben Legacy-Routen reine Kompatibilitaetswege.
Neue Frontendimplementierungen duerfen sie nicht mehr verwenden.

**F3N-Ergebnis vom 30. August 2026:** F3N ist `IMPLEMENTED`. Package- und
Import-Create/Update/Delete verwenden ausschliesslich die B45/B46-Commands mit
Modellrevision, vollstaendigem Draft, autoritativem Reload und strukturiertem
Delete Impact. Direkte Package-/Import-Mutationsaufrufe sind aus dem
produktiven Frontend entfernt. Feld-fuer-Feld-Tests sowie acht Desktop-
Screenshots bestaetigen Create, Edit, Save, Reload, Revision Conflict,
Blocker, Cascade und Delete.

**F4N-Ergebnis vom 30. August 2026:** F4N ist `IMPLEMENTED`. Vollstaendige
Association-Drafts werden ausschliesslich ueber
`PUT .../commands/associations/{associationId}` mit Modellrevision gespeichert
und danach autoritativ neu geladen. Reale Desktop-Abnahmen bestaetigen alle
F4-Felder, Validation mit strukturierter Association-/End-/Qualifiermarkierung,
Not Found, Revision Conflict, Draft-Erhalt und Delete Impact. Direkte
Association-Update-/Delete-Legacy-Aufrufe sind aus dem produktiven Frontend
entfernt.

**F5N-Ergebnis vom 30. August 2026:** F5N ist `IMPLEMENTED`. Normale und
n-aere Object Links verwenden ausschliesslich
`POST/PUT/GET/DELETE .../commands/object-model/links`; der GET-Aufruf betrifft
den Delete Impact. Vollstaendige End- und Qualifierdrafts werden nach Erfolg
autoritativ neu geladen und bleiben bei Validation oder Revision Conflict
erhalten. Der Delete-Dialog stellt Endkontext, Blocker und explizite Cascades
strukturiert dar. Reale Desktop-Abnahmen bestaetigen Create, Save, Reload,
erneutes Editieren, Navigation zur Association, Revision Conflict und Delete.
Direkte Object-Link-Create-/Delete-Legacy-Aufrufe sind aus dem produktiven
Frontend entfernt. Damit sind F3N bis F5N abgeschlossen. B47 bleibt jedoch bis
zum Abschluss von F6N und den von ihm geforderten Verbraucherpruefungen
blockiert.

### Übergabe B13 an F10 und F11

B13 erweitert `UmlModelDto` rückwärtskompatibel um `dataTypes` und ergänzt bei
Enumerationen `packageId` und `qualifiedName`. DataTypes enthalten stabile ID,
Namespace und typisierte Value Properties. Strukturierte DataType-Slotwerte
werden als JSON-Objekte übertragen. OCL-Evaluationen von `oclType()` liefern
ein Objekt mit `classifierId`, `qualifiedName` und `representedType`. F10 kann
diese Verträge für Type Picker und Value Editor verwenden; F11 zeigt das
strukturierte Ergebnis an und rekonstruiert keine Classifieridentität aus
Anzeigenamen. Backendvalidierung bleibt für Mehrdeutigkeit und Wertstruktur
autoritativ.

### Übergabe B17 an F11

B17 erweitert die bestehende Antwort von `GET /api/v1/ocl/profile` additiv um
die `runtimeLimits`-Schlüssel `maxTokens`, `maxAstDepth`,
`maxEvaluationMillis` und `maxDefinitionRecursion`; `maxIteratorBindings`
bleibt erhalten. Parse- und Evaluate-Antworten können zusätzlich
`TOKEN_LIMIT_EXCEEDED`, `AST_DEPTH_LIMIT_EXCEEDED`,
`EVALUATION_DEPTH_LIMIT_EXCEEDED`, `EVALUATION_TIME_LIMIT_EXCEEDED`,
`ITERATION_LIMIT_EXCEEDED` und `DEFINITION_RECURSION_LIMIT` enthalten. F11 darf
diese Grenzen und Diagnostics anzeigen, berechnet oder erzwingt die OCL-Budgets
aber nicht selbst. DTO-Struktur und Endpunktversion bleiben unverändert.

### Übergabe B22 an F11

B22 ergänzt `runtimeLimits` additiv um `maxSourceCharacters = 100000`,
`maxDiagnostics = 32` und `maxResultElements = 1000000`. Parse-Antworten können
`INVALID_OCL_INPUT`, `SOURCE_LIMIT_EXCEEDED` und
`DIAGNOSTIC_LIMIT_EXCEEDED` enthalten; Evaluate-Antworten können zusätzlich
`RESULT_LIMIT_EXCEEDED` enthalten. F11 zeigt diese Grenzen und Diagnostics an,
erzwingt sie aber nicht lokal. DTO-Struktur, Profil-ID und Endpunktversion
bleiben unverändert.

**F11-Abschluss vom 1. September 2026:** F11 ist `IMPLEMENTED`. Das Frontend
verwendet fuer die Profilanzeige den realen read-only Profilvertrag und fuer
Invariant Create/Update/Delete ausschliesslich die B46-freigegebenen,
revisionsgeschuetzten Command-Vertraege. Validation Results bewahren stabile
Element- und Source-Referenzen und navigieren in den fachlichen Workspace.
Draft-Erhalt, Feldmarkierung, autoritativer Reload und echter
`STALE_MODEL_REVISION` sind funktional und per Desktop-Playwright abgenommen.
Die ersetzten Invariant-Legacy-Aufrufe wurden frontendseitig entfernt; eine
Backendbereinigung bleibt B47 vorbehalten. Produktiver Backendcode wurde in
F11 nicht veraendert.

### F12-Abschluss und Uebergabe an B47

F12 ist seit dem 1. September 2026 `IMPLEMENTED`. Die integrierte Desktop-
Abnahme bestaetigt die gemeinsame Class-Diagram-, Object-Diagram-, OCL- und
Bottom-Panel-Shell, strukturierte Validation sowie die in F1-F11 und den
Nachholschritten nachgewiesenen Command-Verbraucher. Skip-Navigation,
kontextbezogene Hilfe, Dialogfokus, roving Tab-Fokus, lange Fachnamen und die
OCL-Vollbreitenansicht wurden zentral gehaertet. Die reale Playwright-Abnahme
weist keine unbenannten sichtbaren Controls, doppelten IDs, Browserfehler oder
horizontalen Ueberlaeufe aus. Frontendtests, Lint und Produktions-Build sind
gruen; produktiver Backendcode wurde nicht veraendert. Damit ist das
Frontend-Stage-Gate fuer den separat auszufuehrenden B47-Legacy-Retirement-
Schritt erfuellt.

### Übergabe B18 an F8

`UmlOperationDto` enthält additiv `contracts`. Jeder Contract besitzt `id`,
`name`, `kind` (`PRE` oder `POST`), `expression` und `enabled`. Der bestehende
Invocation-Endpunkt kann zusätzlich `BLOCKED` liefern und ergänzt
`candidateAfterSnapshotId` sowie strukturierte `contractResults` mit Contract-ID,
Name, Kind, Status und Source Diagnostics. `BLOCKED` bedeutet, dass keine
Operation ausgeführt wurde. Bei `ROLLED_BACK` ist `afterSnapshotId` weiterhin
`null`; eine vorhandene Candidate-ID dient nur der Diagnose. Das Frontend darf
weder Contractauswertung noch Commitentscheidung lokal nachbilden.

**F8-Ergebnis vom 31. August 2026:** F8 ist `IMPLEMENTED`. Die UI verwendet
ausschliesslich die B46-freigegebenen Operation-Commands und den bestehenden
Invocation-Endpunkt. Contract-CRUD, autoritativer Reload, Draft-Erhalt,
Source Diagnostics, PRE-`BLOCKED`, POST-`ROLLED_BACK` und Before/Candidate-
After-Darstellung sind durch Tests und reale Desktop-Playwright-Abnahmen
nachgewiesen. Produktiver Backendcode wurde nicht veraendert.

**F9-Ergebnis vom 31. August 2026:** F9 ist `IMPLEMENTED`. Die UI verwendet
die B44-Attribute-/Operation-Commands sowie die B40/B45-Definition-Vertraege
aus der B46-Freigabe. Init, Derived, Query Body, Class Definitions und Package
Definitions bestehen Save, autoritativen Reload und erneutes Editieren.
Strukturierte Compile-/Revisionsfehler erhalten den vollstaendigen Draft und
markieren das betroffene Feld. Definition Delete verwendet den gemeinsamen
Impact-/Delete-Vertrag. Derived Object Values bleiben eine schreibgeschuetzte
Backendprojektion. Produktiver Backendcode wurde nicht veraendert.

**Historisches F10-Ergebnis vor B50/F10N vom 31. August 2026:** F10 war `PARTIALLY_IMPLEMENTED`
(`BLOCKED_BACKEND`). Enum-/DataType-Lifecycle, Classifier-Typwahl, statische
Classifierwerte und typgerichtete Sloteditoren sind integriert. Primitive und
Enumeration-Slots bestehen Create, Save, Reload und Edit ueber B41/B43-
Commands. Die B46-Freigabe ist fuer den gespeicherten strukturierten
Wertworkflow nicht reproduzierbar: `Money` und `Sequence(String)` werden im
Class-Command als unbekannte Typen abgelehnt. Bis der Backend-Type-Resolver
diese Typen fuer Attribute und Slots akzeptiert und die Roundtrips erneut
abgenommen sind, bleibt F10 blockiert; produktiver Backendcode wurde in F10
nicht veraendert.

**B50-Uebergabe an F10 vom 31. August 2026:** Der Backend-Type-Resolver
akzeptiert und persistiert nun DataType-, Tuple-, Set-, Bag-, Sequence- und
OrderedSet-Typen rekursiv in Attributen, statischen Classifierwerten und
Object-Slots. Model-/Snapshot-Commands liefern neue Revisionen; strukturierte
Validation referenziert Classifier, Attribute, DataType, Object und Slot und
enthaelt einen genauen Feldpfad bei vollstaendig erhaltenem Draft. DataType
Delete Impact erkennt auch verschachtelte Typnutzungen. Die normale Suite
(350 Tests) und die getrennte Reference-Suite (3 Tests) sind gruen. Diese
Uebergabe forderte die reale Nachabnahme der sechs Matrixworkflows fuer Create,
Save, Reload, Edit, Validation, Revision Conflict und Delete Impact. Bis zum
nachfolgend dokumentierten F10N-Abschluss blieben F10 und die sechs
Matrixzeilen formal offen. Produktiver Frontendcode wurde durch B50 nicht
veraendert.

**F10N-Freigabe:** B50 und B51 sind backendseitig abgeschlossen. F10N ist der
Nachholschritt fuer die reale Desktop-Abnahme von Matrix 14, 15, 21, 22, 33,
34, 45 und 47. F10N implementiert keine neue UML-/OCL-Semantik, sondern
verbindet die strukturierten Editoren und vier Delete-Mockups mit den
B43-/B50-/B51-Vertraegen.

**Historischer F10N-B50-Teilabschluss vom 31. August 2026:** Die sechs
Matrixworkflows verwenden die B50-freigegebenen Model-/Snapshot-Commands und
autoritativen Projektionen. DataType-, Tuple- und Collection-Werte bestehen
Create, Save, Reload und Edit; strukturierte Backendvalidation markiert den
betroffenen Wertpfad und ein echter Snapshot-Revision-Conflict erhaelt den
vollstaendigen Draft. DataType Delete Impact war fuer den damaligen sichtbaren
F10-Mockup-Scope `NOT_APPLICABLE`. Matrix 14, 15, 21, 33, 34 und 45 sind
`SUPPORTED`; dieser B50-Teil ist abgeschlossen. Produktiver Backendcode wurde
nicht veraendert.

**F10N-Delete-Erweiterung vom 1. September 2026:** Die nach dem B50-Teilabschluss neu erstellten
Mockups `delete-enumeration-modal.html`, `delete-enumeration-literal-modal.html`,
`delete-datatype-modal.html` und `delete-datatype-value-property-modal.html`
erfordern eine eigene reale Frontendintegration. B43 und B50 stellen Impact
und revisionsgeschuetzten Delete fuer Classifier und Literale bereit; B51
ergaenzt die Value-Property-Vertraege. F10N bleibt `PARTIALLY_IMPLEMENTED`, bis
alle vier Delete-Aktionen, strukturierte Blocker-Navigation, Not Found,
Revision Conflict, Success und autoritativer Reload im Desktop-Frontend
implementiert und abgenommen sind. Der nie gestartete F8N wurde als
Fehlzuordnung entfernt; die Pre-/Postcondition-Semantik des bestehenden F8
wird nicht veraendert. B47 bleibt bis zum vollstaendigen Abschluss von F10N
fuer diese Delete-Verbraucher nachgeordnet.

Enumeration-Literal-Delete ist bereits durch B43 verfuegbar. Der sichere
Delete einer einzelnen DataType-Value-Property bleibt bis B51 blockiert; F10N
darf den bestehenden Full-DataType-Update nicht als Umgehung verwenden.

**B51-Definition vom 1. September 2026:** B51 fuehrt den ueber stabile
DataType-/Property-Pfad-IDs adressierten Impact-/Delete-Vertrag ein und
haertet den bestehenden
Full-DataType-Update gegen das Entfernen verwendeter Property-IDs. Persistierte
Classifier-/Slotwerte sowie OCL-Verwendungen blockieren strukturiert und
navigierbar; automatische Migration oder Cascade ist ausgeschlossen.

**B51-Abschluss vom 1. September 2026:** Der verschachtelte Impact-/Delete-
Vertrag und das Gate des vollstaendigen DataType-Updates sind implementiert.
Persistierte DataType-/Tuple-/Collection-Werte und geparste OCL-Zugriffe
liefern stabile Blocker samt Source Range; Revision, Draft-Erhalt,
Seiteneffektfreiheit und JSON-Roundtrip sind getestet. F10N ist fuer den
DataType-Property-Delete backendseitig freigegeben. B47 bleibt bis zur realen
F10N-Verbrauchermigration nachgeordnet.

**F10N-Abschluss vom 1. September 2026:** Die vier Classifier-/Kind-Delete-
Ablaufe verwenden ausschliesslich die B43-/B50-/B51-Command-Vertraege.
Strukturierte Blocker und Source Ranges bleiben navigierbar; Not Found und ein
echter Model-Revision-Conflict erhalten den Dialogzustand, laden den
autoritativen Impact nach und erlauben den erneuten Delete. Erfolg laedt
Projekt, Read Model, Explorer, Canvas und Typkatalog neu. F10N und F10 sind
`IMPLEMENTED`; die F10-Verbrauchermigration fuer B47 ist abgeschlossen.

### Übergabe B6 an F4

B6 erweitert jedes `UmlAssociationEndDto` um `ordered`, `unique`, `derived`,
`union`, `subsettedEndIds`, `redefinedEndIds` und die read-only Vorschau
`navigationType`. F4 aktualisiert eine binäre Association einschließlich beider
Ends atomar über
den revisionsgeschuetzten
`PUT /api/v1/projects/{projectId}/commands/associations/{associationId}`.
Stabile End-IDs
müssen bei Updates erhalten bleiben. Das Frontend zeigt strukturierte
Metadatenfehler an, berechnet aber weder Collection-Arten noch Subset-/Redefine-
Konformität selbst.

**F4-Umsetzungsstand (30. August 2026):**
`IMPLEMENTED`. Create verwendet
`POST /api/v1/projects/{projectId}/commands/associations` mit Modellrevision;
Delete verwendet den strukturierten Delete-Impact und den revisionsgeschuetzten
Delete-Command. Der Object-Link-/Object-Properties-Anteil bleibt entsprechend
der Matrix F6/F10 und wurde in F4 nicht vorgezogen.
F4N hat das zuvor direkte Association-Update auf den B44-Command migriert.
Die Desktop-Abnahme bei 1440 x 900 ist erfolgreich; ein schmaler Viewport ist
gemaess der gemeinsamen Viewport-Abgrenzung nicht Bestandteil von F4.

### Übergabe B7 an F5

B7 hebt die binäre Begrenzung auf. `UmlAssociationDto.ends` und
`ObjectLinkDto.ends` enthalten mindestens zwei vollständig endbasierte Einträge.
Jedes `UmlAssociationEndDto` liefert zusätzlich `qualifiers` mit stabiler ID,
fachlichem Namen, Typ und Reihenfolge. Das korrespondierende
`ObjectLinkEndValueDto` liefert `qualifierValues`, deren Werte als bestehende
`SlotValueDto` übertragen werden. F5 kann damit Qualifierfelder und n-äre
Endlisten darstellen, ohne interne Positionsannahmen oder Source-/Target-Felder
einzuführen. Qualifizierte OCL-Navigation verwendet `rolle[wert]`. Leere Listen
erhalten die Kompatibilität bestehender binärer Associations und Links.

**F5-Umsetzungsstand (30. August 2026):**
`IMPLEMENTED`. Dynamische Association Ends,
Qualifierdefinitionen und n-aere Object-Link-Zuordnungen verwenden die
freigegebenen B7/B35/B36-Vertraege. N-aere Beziehungen werden im Class- und
Object-Diagramm als zentraler Knoten mit Endsegmenten dargestellt. Bestehende
Object Links waren im urspruenglichen F5-Stand ohne PUT-Vertrag
schreibgeschuetzt. F5N hat diese Beschraenkung mit B42 aufgehoben; die vollstaendige
End-/Qualifierprojektion bleibt autoritativ. Association Classes, Aggregation und
Composition wurden nicht aus F6 vorgezogen.
Typecheck, Lint ohne Fehler, 108 Frontendtests, Produktions-Build und die
F5N-Desktop-Abnahme bei 1440 x 1000 sind erfolgreich.

### Übergabe B8 an F6

B8 erweitert `UmlAssociationDto` um `associationClassId`, jedes
`UmlAssociationEndDto` um `aggregationKind` mit `NONE`, `SHARED` oder
`COMPOSITE` und `ObjectLinkDto` um `associationClassObjectId`. F6 kann damit
Association Classes, Shared-Aggregation-Diamanten, Composition-Diamanten und
gekoppelte Linkobjekte darstellen. Das Backend bleibt verantwortlich für
kompatible Eins-zu-eins-Identität, exklusive Composite Ownership, Zyklenfreiheit
und rekursives Löschen. F6 darf Löschfolgen vorab erläutern, bildet diese Regeln
aber nicht selbst nach. Strukturierte Fehler verwenden
`ASSOCIATION_CLASS_IDENTITY_VIOLATION`, `COMPOSITE_OWNERSHIP_VIOLATION` und
`COMPOSITION_CYCLE`.

**F6-Vertragskorrektur vom 30. August 2026:** Die Read-Projektionen,
Aggregation-/Composition-Diagnostics, das Binden einer vorhandenen Class,
Link-End-Updates und der gekoppelte Delete-Lifecycle sind nutzbar. F6 hat beim
realen Feld-fuer-Feld-Test jedoch zwei fehlende atomare Aggregate-Commands
nachgewiesen: Class plus Features plus Association-Class-Bindung sowie Object
Link plus Association-Class-Object plus Slots. Diese Teilablaeufe sind bis B48
blockiert. Das Frontend darf sie weder ueber Legacy-Routen noch durch mehrere
aufeinanderfolgende Commands simulieren. Nach B48 wird nur die betroffene
F6-Create-/Update-Abnahme wiederholt; B47 bleibt bis dahin nachgeordnet.

**B48-Uebergabe an F6 vom 30. August 2026:** Die beiden zuvor blockierten
Aggregate sind nun revisionsgeschuetzt und atomar verfuegbar. F6 verwendet
`POST .../commands/associations/{associationId}/association-class` fuer Class,
Features und Bindung sowie `POST/PUT .../commands/object-model/association-class-instances[/{linkId}]`
fuer Object Link, Linkobjekt und Slots. Matrix 1, 2 und 20 sind backendseitig
`SUPPORTED`. F6N muss die betroffenen Create-/Update-Felder visuell und
funktional erneut abnehmen; es darf die alten Mehrfach-Command-Sequenzen nicht
mehr verwenden. B47 bleibt bis zum Abschluss von F6N blockiert.

**F6N-Abschluss vom 31. August 2026:** Die B48-Aggregate sind im produktiven
Frontend angebunden und fuer Matrix 1, 2 und 20 feld- beziehungsweise
aktionsweise abgenommen. Association-Class-Create liefert Class, Features und
Bindung atomar; Association-Class-Instance-Create/-Update liefert Link,
Linkobjekt und typisierte Slots atomar. Reale Desktop-Playwright-Workflows
bestaetigen Backendvalidation mit erhaltenem Draft, Create, Save, autoritativen
Reload, erneutes Editieren und `STALE_SNAPSHOT_REVISION` mit erhaltenem Draft.
F6 und F6N sind `IMPLEMENTED`. Die F6N-Abhaengigkeit blockiert B47 damit nicht
mehr; dessen weitere Startvoraussetzungen bleiben unveraendert bestehen.

### Übergabe B9 an F7

B9 erweitert `UmlOperationDto` um `abstractOperation` und `query` sowie
`UmlParameterDto` um `direction` und `position`. F7 ruft Operationen getrennt
von der Signaturbearbeitung über
`POST /api/v1/projects/{projectId}/operations/{operationId}/invocations` auf.
Die Anfrage enthält Receiver, stabile Parameter-IDs, typisierte Werte und die
erwartete Revision. Das Ergebnis unterscheidet `SUCCEEDED` und `ROLLED_BACK`,
nennt angeforderte und tatsächlich aufgelöste Operation und liefert Result,
Out Values, Snapshot-IDs sowie created/changed/deleted Objects mit fachlichen
Namen. Das Frontend mutiert den Snapshot erst nach `SUCCEEDED` und berechnet
weder Dispatch, Typkompatibilität, Query-Schutz noch Rollback selbst.

**F7-Integrationsstand vom 31. August 2026:** Die Signatur-Commands und der
Invocation-Vertrag sind frontendseitig implementiert und real abgenommen.
Create, Update, autoritativer Reload, Validation, Revision Conflict,
Invocation und strukturierte Invocation Results funktionieren ohne lokale
UML-/OCL-Semantik. Die F7-Feldabnahme hat jedoch einen Widerspruch zur
historischen B46-Freigabe nachgewiesen: Der generische Operation-Delete
verlangt serviceintern `classId`, waehrend `DeleteCommandRequestDto` keine
`classId` enthaelt. Der reale Request endet mit `400 OWNER_REQUIRED`.
Matrixeintrag 30 ist deshalb wieder `MISSING_WRITE`; F7 bleibt
`PARTIAL_BACKEND_DELETE_CONTRACT_BLOCKED`. B49 muss den Commandvertrag durch
backendseitige Owner-Aufloesung aus der `operationId` korrigieren und danach Delete Impact, Delete,
Not Found und Revision Conflict erneut abnehmen. Ein Legacy-Write wird nicht
als Ersatz verwendet.

**B49-Uebergabe an F7 vom 31. August 2026:** Der generische Operation-Delete
loest die Owner Class nun projektweit aus der stabilen `operationId` auf.
`DeleteCommandRequestDto` bleibt unveraendert. Matrix 30 ist backendseitig
wieder `SUPPORTED`; 23 gezielte Controller-Tests, 345 normale Backendtests und
3 getrennte Reference-Tests sind gruen. F7 muss Delete Impact, erfolgreichen
Delete, strukturiertes Not Found, Blocker/ungueltige Cascade-Auswahl sowie
Revision Conflict mit Draft-Erhalt noch real im Desktop-Workflow nachpruefen.
Bis dahin bleibt F7 `PARTIAL_DELETE_REACCEPTANCE_REQUIRED` und B47 fuer diesen
Verbraucher blockiert.

**F7N-Abnahme vom 31. August 2026:** Der B49-Vertrag ist im realen
Desktop-Frontend bestaetigt. Impact und Delete adressieren die Operation nur
ueber ihre stabile ID; der Request enthaelt keine `classId`. Erfolg mit
autoritativer Projektion, Not Found, strukturierte navigierbare Blocker,
atomar abgewiesene ungueltige Cascade-Auswahl und Revision Conflict mit
erhaltenem Dialogzustand sind reproduzierbar. 128 Frontendtests, Build,
Lint ohne Fehler und die Playwright-Abnahme sind erfolgreich. F7/F7N sind
`IMPLEMENTED`; der Operation-Delete-Verbraucher blockiert B47 nicht mehr.

## Entkopplungsregel fuer Reference-Cases

Reference-Cases liefern fachliche Eingaben und Erwartungen, aber keinen
Produktvertrag. Backend-Schritt B23 darf deshalb weder alte USE-Shellausgaben
noch alte Runner, Kommandos oder Setupmechanismen in das neue Backend
uebernehmen. Fachlich relevante Erwartungen werden in eigene DTO-unabhaengige
Testassertions und minimale eigene Fixtures uebersetzt. Reine
Altsystemfunktionalitaet wird explizit ausgeschlossen. Bei Aenderungen an der
Reference-Strategie muessen Implementierungsplan, Teststrategie,
Migrationsanalyse, Gap-Analyse, Compliance-Matrix und B2-Zuordnung gemeinsam
auf diese Regel geprueft werden.

**B23-Ergebnis vom 22. August 2026:** Der Report enthaelt keine
`FAILING_FORMAT`- oder `FAILING_INFRASTRUCTURE`-Faelle mehr. Von 1418 Faellen
sind 799 `PASSING`, 543 `FAILING_GAP`, 59 `NON_OCL_OR_SHELL_ONLY` und 17
`UNCLEAR`. Produktive APIs und DTOs blieben unveraendert. B24 erhaelt damit
ausschliesslich fachliche Gap- und Review-Arbeit, keine Aufgabe zur Nachbildung
alter USE-Infrastruktur.

## Definition of Done

- Das Mockup besitzt freigegebene Haupt-, Bearbeitungs-, Fehler- und Bestaetigungszustaende.
- Der Backend-Schritt ist durch Unit-, Service- und gegebenenfalls API-Tests abgesichert.
- Der API-Vertrag ist stabil dokumentiert.
- Der Frontend-Schritt deckt Desktop, Tastaturbedienung und internes
  Scrollverhalten ab. Fuer F4 bis F11 und F12 ist ein schmaler Viewport nur bei einer
  ausdruecklichen schrittspezifischen oder kanonischen Mockup-Anforderung Teil
  der Definition of Done.
- Der Workflow verwendet fachliche Namen und bietet nach jeder Aktion einen sichtbaren Status.
- Normale CI und getrennte Reference-Test-Suite bleiben getrennt.

## Rueckwaertige Zuordnung

Die bisherigen Schritte 1 bis 25 entsprechen unveraendert B1 bis B25. B26 bis
B33 erweitern den Plan um die nach B25 gemessene vollständige
Reference-Gap-Schließung. Die
Praefixe verhindern Verwechslungen mit Frontend- und Mockup-Schritten.
