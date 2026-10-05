# Backend Scope

## Zweck dieser Datei

Diese Datei definiert den fachlichen und technischen Scope des neuen Backends für das webbasierte UML/OCL-System.

Sie beschreibt:

- welche Aufgaben das Backend übernimmt,
- welche Aufgaben bewusst nicht im Backend liegen,
- welche Backend-Funktionalität im MVP enthalten sein muss,
- welche Funktionalität später erweitert werden kann,
- wie das Backend vom originalen USE-Projekt abgegrenzt ist,
- welche Verantwortung beim Frontend und welche beim Backend liegt.

Das Backend ist kein technischer Nachbau des originalen USE-Cores. Das originale USE-Projekt dient als fachliche Referenz für UML-Konzepte, OCL-Semantik, Snapshots, Validierungsverhalten, Syntaxbeispiele und Testfälle. Die technische Umsetzung entsteht neu.

## Rolle des Backends im Zielsystem

Das Backend ist die zentrale fachliche Semantikschicht des neuen Systems. Es verwaltet Projekte, UML-Modelle, Snapshots, OCL-Invarianten und Validierungsergebnisse. Außerdem stellt es eine REST/JSON-API bereit, über die das React/TypeScript-Frontend mit der fachlichen Logik kommuniziert.

Die wichtigste Architekturentscheidung lautet:

> Das Frontend zeigt, editiert und visualisiert. Das Backend entscheidet fachlich.

Das bedeutet:

- UML-Strukturregeln werden im Backend geprüft.
- OCL wird im Backend geparst, typgeprüft und ausgewertet.
- Multiplicity Checks laufen im Backend.
- Validation Results werden im Backend strukturiert erzeugt.
- Das Frontend darf Ergebnisse visualisieren, aber keine fachliche Wahrheit ersetzen.

## Backend-Verantwortlichkeiten

| Verantwortlichkeit | Beschreibung | MVP-Relevanz |
|---|---|---|
| Projektverwaltung | Projekte anlegen, laden, speichern und als JSON-basiertes MVP-Format bereitstellen. | Hoch |
| UML-Modellverwaltung | Klassen, Attribute, Operationen, Assoziationen, Rollen und Multiplizitäten verwalten. | Hoch |
| OCL-Invariantenverwaltung | Invarianten mit Kontextklasse, Namen und OCL-Ausdruck speichern und ändern. | Hoch |
| Modelltext-Verarbeitung | Vollständigen USE-ähnlichen Editor-Text entgegennehmen, das MVP-Subset parsen und in das eigene Domain-/JSON-Modell überführen. | Hoch |
| Objektmodell-/Snapshot-Verwaltung | Objekte, Slots/Attributwerte und Objektlinks für einen validierbaren Snapshot verwalten. | Hoch |
| OCL-Verarbeitung | Lexer, Parser, AST, Type Checker und Evaluator für das MVP-OCL-Subset bereitstellen. | Hoch |
| Validierungsservice | UML-Struktur, Snapshot, Links, Multiplizitäten und OCL-Invarianten prüfen. | Hoch |
| REST/JSON-API | Stabile API für Frontend, Projektformat, Validierung und Fehlerausgabe bereitstellen. | Hoch |
| Fehler- und Result-Modell | Strukturierte Fehlercodes, Severity, Elementreferenzen und Validation Results erzeugen. | Hoch |
| ID- und Referenzmodell | Stabile IDs für Klassen, Attribute, Associations, Invarianten, Objekte und Links sicherstellen. | Hoch |
| Testbarkeit | Fachlogik ohne UI testbar machen; Unit-, Integration- und API-Tests ermöglichen. | Hoch |
| Erweiterbarkeit | OCL-, UML- und Persistenzfunktionen so schneiden, dass Post-MVP-Erweiterungen möglich bleiben. | Hoch |

## Nicht-Verantwortlichkeiten des Backends

Das Backend ist nicht für Darstellung und direkte UI-Interaktion zuständig.

| Nicht-Verantwortlichkeit | Liegt stattdessen bei | Bemerkung |
|---|---|---|
| Diagramm-Rendering | Frontend | Canvas, Nodes, Edges und visuelle Gestaltung werden im React-Frontend umgesetzt. |
| Canvas-Interaktionen | Frontend | Dragging, Selection, Zoom, Pan, Hover und Kontextmenüs sind UI-Zustand. |
| UI-Layout im engeren Sinn | Frontend | Panelgrößen, aktive Tabs und temporäre UI-Zustände gehören nicht zur Backend-Semantik. |
| Diagramm-Layout-Algorithmik im MVP | Frontend oder später separater Dienst | Das Backend kann Layoutdaten speichern, berechnet aber im MVP keine Diagrammlayouts. |
| Alte USE-GUI | Nicht übernehmen | Swing-/JavaFX-Desktop-GUI ist keine Zieltechnologie. |
| Direkte USE-Core-Nutzung | Nicht übernehmen | Keine Runtime Dependency auf `use-core`. |
| Vollständige OCL-Unterstützung im MVP | Post-MVP | MVP unterstützt nur ein bewusst begrenztes OCL-Subset. |
| Sequenzdiagramme | Nicht-Ziel | Nicht Teil des Zielumfangs. |
| State Machines | Nicht-Ziel | Nicht Teil des Zielumfangs. |
| Aktivitäts-, Deployment- oder Komponentendiagramme | Nicht-Ziel | Nicht Teil des MVP und nicht Teil des aktuellen fachlichen Fokus. |
| Kollaboration in Echtzeit | Post-MVP oder Nicht-Ziel | Keine Backend-Pflicht im MVP. |
| Codegenerierung | Nicht-Ziel im MVP | Fachlicher Fokus liegt auf Modellierung und Validierung. |

## MVP-Scope des Backends

Der Backend-MVP muss einen vollständigen vertikalen Durchstich unterstützen: Klassenmodell erstellen, Invarianten erfassen, Snapshot modellieren, Constraints prüfen und strukturierte Fehler zurückgeben.

| Bereich | MVP-Funktionalität | Ergebnis |
|---|---|---|
| Projekt | Projekt anlegen, laden, speichern, JSON importieren/exportieren. | Reproduzierbarer Projektzustand. |
| Klassenmodell | Klassen, Attribute, primitive Typen, Operationssignaturen verwalten. | Validierbares UML-Grundmodell. |
| Associations | Associations, Rollen und Multiplizitäten verwalten. | Grundlage für Navigation und Multiplicity Checks. |
| OCL-Invarianten | Kontextklasse, Name und OCL Expression speichern. | Invarianten sind Teil des UML-Modells. |
| Modelltext | Vollständigen Editor-Text speichern und bei `Apply Changes` das unterstützte USE-ähnliche MVP-Subset anwenden. | OCL Editor kann ganze Modelltexte aus `.use`-Beispielen anzeigen. |
| Snapshot | Objekte, Slot-Werte und Objektlinks verwalten. | Konkreter Objektzustand für Validierung. |
| OCL-Pipeline | MVP-OCL lexen, parsen, als AST repräsentieren, typprüfen und auswerten. | Keine Regex-/String-Auswertung. |
| OCL-MVP-Subset | `self`, Attributzugriff, einfache Navigation, Literale, Vergleiche, Boolean-Operatoren, Klammern, `size`, `isEmpty`, `notEmpty`. | Demonstrierbare Invariantenauswertung. |
| Validierung | UML-Struktur, Snapshot, Link-Kompatibilität, Multiplizitäten und Invarianten prüfen. | `ValidationResult` mit Fehlerliste. |
| Fehlerausgabe | Fehlercodes, Severity, Message, betroffene Element-IDs, OCL-Kontext liefern. | Frontend kann Fehler markieren. |
| API | REST/JSON-Endpunkte für Projekt, Modell, Snapshot, OCL und Validierung bereitstellen. | Frontend-Backend-Vertrag. |
| Tests | Backend-Fachlogik und API testbar machen. | Regressionen im MVP erkennbar. |

## Post-MVP-Erweiterungen

| Erweiterung | Backend-Auswirkung | Referenz |
|---|---|---|
| Erweiterter OCL-Sprachumfang | Neue AST-Knoten, Typechecker-Regeln und Evaluator-Regeln. | Original-USE-OCL als Verhaltenreferenz. |
| `forAll`, `exists`, `select`, `collect` | Collection-Auswertung und Iterator-Kontexte ergänzen. | USE Collection Operations. |
| `let`, `if-then-else`, `allInstances` | Ausdrucksmodell und Evaluation erweitern. | USE OCL-Beispiele und Tests. |
| Preconditions/Postconditions | Operation Contracts modellieren und validieren. | `MPrePostCondition` als fachliche Referenz. |
| Derived Attributes und Init Values | Zusätzliche OCL-Verwendungsarten im UML-Modell. | USE-Konzepte, Post-MVP-Scope. |
| Vererbung | Klassenhierarchie, Polymorphie, Typechecking und Snapshot-Regeln erweitern. | `MGeneralization`, `MClass`. |
| Enumerationen | Typmodell erweitern. | USE Typmodell. |
| Aggregation/Komposition | Association-End-Semantik erweitern. | `MAggregationKind`. |
| Assoziationsklassen | UML-Modell und Objektlinks erweitern. | `MAssociationClass`. |
| Mehrere Snapshots | Snapshot-Verwaltung und API um benannte Zustände erweitern. | USE-Systemzustandskonzept. |
| Vollständiger `.use` Import/Export | Parser/Serializer für vollständige USE-Syntax ergänzen. | `USECompiler`, Grammatikressourcen, Beispiele. |
| Datenbankpersistenz | JSON-Dateiformat um persistente Speicherung ergänzen oder ersetzen. | Architekturentscheidung offen. |
| Testfallbibliothek | Originale USE-Beispiele als fachliche Testfälle ableiten. | `use-core/src/main/resources/examples/`. |

## Abgrenzung zum Frontend

| Thema | Backend-Verantwortung | Frontend-Verantwortung |
|---|---|---|
| Klassen | Speichern, validieren, IDs und API bereitstellen. | Anzeigen, auswählen, bearbeiten, im Canvas positionieren. |
| Attribute | Typen und Konsistenz prüfen. | Eingabe-Controls, Tabellen, Properties Panel. |
| Operationen | Signaturen speichern und typbezogen validieren. | Signaturen anzeigen und editieren. |
| Associations | Enden, Rollen, Multiplizitäten und Referenzen verwalten. | Kanten darstellen, Labels positionieren, Auswahl ermöglichen. |
| Objekte | Objektidentität, Klassenzuordnung und Slots speichern. | Objektkarten anzeigen, Slot-Editor bereitstellen. |
| Objektlinks | Link-Kompatibilität und Snapshot-Konsistenz prüfen. | Link-Kanten darstellen und auswählbar machen. |
| OCL | Parser, AST, Typechecker, Evaluator und Diagnosen. | OCL-Text erfassen, Feedback anzeigen, Backend-Diagnosen visualisieren. |
| Validierung | Constraint Check ausführen und `ValidationResult` erzeugen. | `Check Constraints` auslösen, Ergebnisse anzeigen und Elemente markieren. |
| Fehler | Fehlercodes, Severity und Elementreferenzen liefern. | Fehlerlisten, Badges, rote Rahmen und Fokusverhalten umsetzen. |
| Layoutdaten | Persistierbare Layoutinformationen speichern. | Layout berechnen, Dragging und visuelle Darstellung steuern. |
| Projektformat | JSON-Struktur validieren und laden/speichern. | Import-/Export-Aktion anbieten, Dirty State anzeigen. |

## Abgrenzung zum originalen USE-Projekt

Das originale USE-Projekt bleibt Referenz, wird aber nicht technische Grundlage des neuen Backends.

| Aspekt | Original-USE | Neues Backend |
|---|---|---|
| Technologiebasis | Java-Maven-Projekt mit `use-core`, `use-gui`, `use-assembly`. | Neues Java/Spring-Boot-Backend. |
| UI | Swing-/JavaFX-Desktop-GUI. | Keine UI im Backend; React/TypeScript-Frontend separat. |
| Modellkern | `org.tzi.use.uml.mm.*`. | Eigenes Domänenmodell mit stabilen JSON-IDs. |
| Snapshot | `org.tzi.use.uml.sys.*`. | Eigenes Object Model / Snapshot Model. |
| OCL | ANTLR-3-basierte Grammatik, eigenes Expression-/Value-Modell. | Eigene OCL-Pipeline für MVP-Subset, später erweiterbar. |
| Fehlerausgabe | Für Desktop/Shell und interne APIs geprägt. | Strukturierte REST/JSON-Fehler für Frontend-Mapping. |
| Projektformat | `.use`, `.cmd`, weitere USE-Dateien. | JSON-Projektformat im MVP; zusätzlich vollständiger Editor-Text als Quelle/Ansicht für das unterstützte Subset. |
| Nutzung | Direkt ausführbares Desktopwerkzeug. | Webservice als fachliche Semantikschicht. |
| Codeübernahme | Bestehender USE-Code. | Keine Codeübernahme, kein Fork, keine Runtime Dependency. |

## Referenzpunkte aus dem originalen USE-Projekt

Die folgenden Bereiche des originalen Projekts sind für das Backend fachlich relevant:

| Originalbereich | Relevanz für neues Backend | Nutzungskategorie |
|---|---|---|
| `../use/use-core/src/main/java/org/tzi/use/uml/mm/` | UML-Klassenmodell, Assoziationen, Multiplizitäten, Invarianten. | fachliche Referenz |
| `../use/use-core/src/main/java/org/tzi/use/uml/sys/` | Objekte, Objektzustände, Links und Systemzustände. | Verhaltenreferenz |
| `../use/use-core/src/main/java/org/tzi/use/uml/ocl/expr/` | OCL-Ausdrucksmodell und Evaluationsverhalten. | Verhaltenreferenz |
| `../use/use-core/src/main/java/org/tzi/use/uml/ocl/type/` | OCL-Typsystem. | fachliche Referenz |
| `../use/use-core/src/main/java/org/tzi/use/uml/ocl/value/` | OCL-Werte und Collections. | fachliche Referenz |
| `../use/use-core/src/main/java/org/tzi/use/parser/` | Parser-, Compiler- und Fehlerkonzepte. | Syntaxreferenz |
| `../use/use-core/src/main/resources/grammars/` | USE-/OCL-Grammatikfragmente. | Syntaxreferenz |
| `../use/use-core/src/main/resources/examples/` | `.use`, `.cmd`, `.invs`, `.testsuite` Beispielmaterial. | Testfallquelle |
| `../use/use-core/src/test/` und `../use/use-core/src/it/` | Tests und Verhaltenserwartungen. | Testfallquelle / Verhaltenreferenz |
| `../use/use-gui/` | Historische Desktop-Interaktion. | nicht übernehmen / technische Orientierung |

Besonders relevante Originalklassen und Konzepte:

| Originalklasse/Konzept | Bedeutung für neues Backend |
|---|---|
| `MModel` | Referenz für ein gesamtes UML-Modell. |
| `MClass`, `MAttribute`, `MOperation` | Referenz für Klassen, Attribute und Operationssignaturen. |
| `MAssociation`, `MAssociationEnd`, `MMultiplicity` | Referenz für Associations, Rollen und Multiplicity Checks. |
| `MClassInvariant` | Referenz für Invarianten im Kontext einer Klasse. |
| `MSystemState`, `MObject`, `MObjectState`, `MLink` | Referenz für Snapshots, Objekte, Slots und Links. |
| `OCLCompiler`, `USECompiler` | Referenz für Parsing-Abläufe und spätere Import-/Export-Fragen. |
| `Evaluator`, `EvalContext` | Referenz für Auswertung gegen einen konkreten Systemzustand. |
| `ParseErrorHandler`, `SemanticException` | Referenz für strukturierte Fehlerkategorien. |

## Scope-Entscheidungen

| Entscheidung | Begründung | MVP-Auswirkung |
|---|---|---|
| Neues Backend statt USE-Core | Das Zielsystem soll eigenständig, webfähig und wartbar sein. | Keine Runtime Dependency auf USE. |
| Spring Boot als bevorzugte Backend-Basis | Passt zu Java-Zieltechnologie und REST/JSON-API. | Klare Service-, Controller- und Teststruktur möglich. |
| Backend als Semantikquelle | UI darf fachliche Validierung nicht duplizieren. | OCL, Multiplicity und Snapshot Checks laufen zentral. |
| JSON-Projektformat im MVP | Einfacher für Webfrontend, Tests und API als direkte `.use`-Kompatibilität. | `.use` Import/Export wird Post-MVP. |
| Modelltext-Apply im MVP | Der OCL Editor zeigt laut Screenshot vollständigen USE-ähnlichen Modelltext, nicht nur einzelne OCL-Ausdrücke. | Backend braucht einen begrenzten Apply-Flow für `model`, `class`, `attributes`, `operations`, `association` und `constraints`; nicht unterstützte Syntax wird diagnostiziert. |
| Eigene OCL-Pipeline | OCL darf nicht durch Regex-Regeln ersetzt werden. | Lexer, Parser, AST, Typechecker und Evaluator im MVP-Subset. |
| Strukturierte Validation Results | Frontend muss Fehler ohne Textparsing markieren können. | Fehler enthalten Codes und Element-IDs. |
| Layoutdaten speichern, aber nicht rendern | Layout ist UI-relevant, aber nicht fachliche Semantik. | Backend speichert Positionen, Frontend rendert Diagramme. |
| Begrenztes UML/OCL-MVP | Vertikaler Durchstich ist wichtiger als vollständige Feature-Parität. | Fokus auf Klassen, Objekte, Links, Invarianten und Validierung. |

## Offene Fragen

| Frage | Bedeutung |
|---|---|
| Wird das MVP-Projektformat ausschließlich dateibasiert gespeichert oder bereits über eine Datenbank persistiert? | Beeinflusst Persistence Service und Deployment. |
| Soll OCL-Syntax-/Typechecking separat per API aufrufbar sein oder nur im vollständigen Constraint Check laufen? | Beeinflusst OCL-Service und Frontend-Feedback. |
| Wie detailliert müssen Source Ranges für OCL-Fehler im MVP sein? | Beeinflusst Parser- und Error-Contract-Aufwand. |
| Werden Layoutinformationen vollständig im Backend gespeichert oder teilweise nur im Frontend-State gehalten? | Beeinflusst Projektformat und API. |
| Wie strikt soll das Backend bei Modelländerungen abhängige Elemente blockieren, automatisch entfernen oder als Fehler markieren? | Betrifft Lösch- und Änderungsregeln. |
| Welche `.use`-Beispiele aus dem Originalprojekt werden als erste reduzierte Modelltext-Testfälle abgeleitet? | Beeinflusst Teststrategie, MVP-Demo und Diagnose nicht unterstützter Syntax. |
| Wird es mehrere Snapshots schon im Datenmodell geben, auch wenn der MVP nur einen aktiven Snapshot nutzt? | Beeinflusst Erweiterbarkeit. |

## Zusammenfassung

Das neue Backend übernimmt die fachliche Kernlogik des Zielsystems: Projektverwaltung, UML-Modell, Snapshot, OCL-Invarianten, OCL-Verarbeitung, Validierung, REST/JSON-API und strukturierte Fehlerausgabe.

Nicht zum Backend gehören Diagramm-Rendering, Canvas-Interaktion, Desktop-GUI-Verhalten und direkte technische Wiederverwendung des originalen USE-Cores. USE bleibt eine wichtige fachliche Referenz für Konzepte, Syntax, Verhalten und Testfälle, aber die technische Architektur des neuen Systems entsteht eigenständig.

Der MVP-Backend-Scope ist bewusst vertikal geschnitten: Ein Projekt mit Klassenmodell, Invarianten und Snapshot muss validiert werden können, sodass das Frontend Fehler im Diagramm und im Validation Results Panel nachvollziehbar anzeigen kann.
