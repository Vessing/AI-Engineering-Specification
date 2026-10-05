# Project Summary

## Ausgangslage

USE, das "UML-based Specification Environment", ist ein Java-basiertes Werkzeug zur Modellierung und Analyse von UML-Modellen mit OCL-Constraints. Es unterstützt unter anderem Klassendiagramme, Objektzustände, Snapshots und die Auswertung von OCL-Ausdrücken.

Für dieses Projekt dient USE fachlich als wichtige Orientierung. Das bestehende System zeigt, welche Konzepte, Modellierungsabläufe, Syntaxregeln, Validierungen und Fehlermeldungen in einem UML/OCL-Werkzeug relevant sind.

Die neue Anwendung ist jedoch keine technische Migration des originalen USE-Projekts. Der bestehende USE-Core wird nicht übernommen, nicht geforkt und nicht als direkte Dependency verwendet. Stattdessen entsteht ein neues webbasiertes System mit eigenem Backend, eigenem Frontend, eigenem Domänenmodell, eigener OCL-Verarbeitung und eigener Validierungslogik.

## Ziel des neuen Systems

Ziel ist eine moderne Webanwendung für UML/OCL-Modellierung und Constraint-Validierung.

Der fachliche Schwerpunkt liegt auf:

- UML-Klassendiagrammen,
- UML-Objektdiagrammen,
- Snapshots von Objektzuständen,
- OCL-Invarianten,
- Validierung von UML- und OCL-Constraints,
- verständlicher Darstellung von Validierungsfehlern im Diagramm und in einem Ergebnisbereich.

Das Zielsystem soll einen vollständigen Arbeitsablauf ermöglichen: Ein Nutzer modelliert ein Klassendiagramm, ergänzt OCL-Invarianten, erzeugt dazu passende Objekte und Objektlinks, setzt Attributwerte und prüft anschließend, ob Modell und Snapshot die definierten Constraints erfüllen.

## Kernidee der Zielarchitektur

Die Zielarchitektur sieht ein neu entwickeltes Websystem vor.

| Bereich | Zielidee |
|---|---|
| Backend | Neues Java/Spring-Boot-Backend |
| Frontend | Neues React/TypeScript-Frontend |
| Kommunikation | REST/JSON-Schnittstellen zwischen Frontend und Backend |
| Domänenmodell | Eigenes Modell für Klassen, Attribute, Operationen, Assoziationen, Objekte, Links und Constraints |
| OCL-Verarbeitung | Eigene, schrittweise erweiterbare OCL-Verarbeitung |
| Validierung | Eigene Validierungslogik für UML-Strukturregeln, Snapshot-Konsistenz und OCL-Invarianten |

OCL soll nicht als Sammlung einfacher Text- oder Regex-Prüfungen behandelt werden. Die Architektur soll von Beginn an eine konzeptionelle Verarbeitung über folgende Pipeline ermöglichen:

```text
Lexer -> Parser -> AST -> Type Checker -> Evaluator -> Validation Result
```

Im MVP wird nur ein begrenztes OCL-Subset benötigt. Die Architektur soll aber so vorbereitet sein, dass spätere Erweiterungen wie `forAll`, `exists`, `select`, `collect`, `let`, `if-then-else`, `allInstances`, Pre-/Postconditions oder abgeleitete Attribute möglich bleiben.

## Rolle des originalen USE-Projekts

Das originale USE-Projekt wird als fachliche Referenz untersucht.

Es dient insbesondere als Quelle für:

- zentrale UML/OCL-Konzepte,
- Modell- und OCL-Syntax,
- erwartetes Verhalten bei Modellierung und Validierung,
- Validierungsregeln,
- typische Fehlertypen,
- Beispielmodelle,
- Testfälle und fachliche Vergleichsszenarien.

Die Referenznutzung bedeutet nicht, dass bestehender USE-Code direkt übernommen wird. Das neue System soll unabhängig implementiert werden. Erkenntnisse aus USE werden in Analyse- und Architekturentscheidungen übersetzt, nicht in eine technische Kopie des alten Systems.

## Rolle der neuen UI-Screenshots

Die Screenshots unter `assets/screenshots/` zeigen das Zielbild der späteren Weboberfläche. Sie sind eine zentrale Referenz für Produktverständnis, User Journey, UI-Anforderungen, MVP-Scope, Komponentenstruktur und Akzeptanzkriterien.

Besonders relevant sind die sichtbaren Bereiche und Interaktionsmuster:

- Class Diagram View,
- Object Diagram View,
- OCL Editor,
- Explorer Sidebar,
- Diagram Canvas,
- Properties Panel,
- Validation Results,
- Console,
- Modals,
- Check Constraints Button.

Die Screenshots helfen dabei, Anforderungen nicht nur abstrakt zu formulieren, sondern aus konkreten Nutzeraktionen abzuleiten. Sie zeigen beispielsweise, wie Klassen, Assoziationen, Invarianten, Objekte und Objektlinks angelegt oder bearbeitet werden sollen und wie Validierungsfehler visuell und textuell dargestellt werden können.

## Fachlicher Fokus

Der fachliche Kern des Zielsystems liegt auf einem begrenzten, aber zusammenhängenden UML/OCL-Ausschnitt.

| Bereich | Relevante Bestandteile |
|---|---|
| Klassendiagramm | Klassen, Attribute, Operationen als Signaturen, Assoziationen, Rollen, Multiplizitäten |
| Objektdiagramm | Objekte, Objektlinks, Attributwerte, Snapshot-Zustände |
| OCL | Invarianten, `self`, Attributzugriff, einfache Association Navigation, Vergleichsoperatoren, Boolean-Operatoren, Literale, einfache Collection-Operationen |
| Validierung | UML-Strukturregeln, Multiplizitäten, Typprüfung, OCL-Auswertung, Constraint Validation |

Operationen werden im MVP primär als Signaturen betrachtet. Der Fokus liegt nicht auf ausführbarer Operationslogik, sondern auf der Struktur des Modells und der Prüfung von Objektzuständen gegen definierte Constraints.

## MVP-Zielbild

Der MVP soll einen demonstrierbaren vertikalen Durchstich durch das Zielsystem liefern.

Ein Nutzer soll im MVP:

1. ein UML-Klassenmodell erstellen,
2. Klassen, Attribute und Operationen als Signaturen modellieren,
3. Assoziationen mit Rollen und Multiplizitäten definieren,
4. OCL-Invarianten für Klassen erfassen,
5. Objekte im Objektdiagramm erstellen,
6. Attributwerte für Objekte setzen,
7. Objektlinks zwischen Objekten anlegen,
8. UML- und OCL-Constraints prüfen,
9. Validierungsfehler im Diagramm erkennen,
10. Validierungsfehler im Validation Panel nachvollziehen.

Der MVP soll nicht die vollständige Breite von USE oder OCL abbilden. Entscheidend ist ein konsistenter Kernablauf, der das Zusammenspiel aus Klassendiagramm, Objektdiagramm, Snapshot und Constraint-Validierung sichtbar macht.

## Nicht-Ziele auf hoher Ebene

Für den initialen Zielumfang gelten folgende Nicht-Ziele:

- keine Sequenzdiagramme,
- keine State Machines,
- keine weiteren verhaltensorientierten UML-Diagramme,
- keine vollständige OCL-Feature-Parität im MVP,
- keine Nachbildung der alten USE-GUI,
- keine direkte USE-Core-Abhängigkeit,
- keine technische Migration des bestehenden USE-Projekts.

Diese Abgrenzung soll den MVP fachlich fokussieren und verhindern, dass die erste Systemversion durch zu breite UML- oder OCL-Unterstützung überladen wird.

## Zweck dieses Analyse-Repository

Dieses Analyse-Repository bereitet die spätere Entwicklung der eigentlichen Implementierungsrepositories vor.

Es dokumentiert:

- fachliche Anforderungen,
- relevante Referenzen aus dem originalen USE-Projekt,
- UI- und UX-Anforderungen aus den Screenshots,
- User Journeys,
- MVP-Scope,
- Domänenmodell und zentrale Begriffe,
- Validierungsregeln,
- OCL-Strategie,
- Backend- und Frontend-Zielarchitektur,
- API- und Integrationsüberlegungen,
- offene Fragen und spätere Erweiterungen.

Das Repository ist damit keine Implementierung, sondern die fachliche, technische und planerische Grundlage für die spätere Umsetzung.

## Erwartetes Ergebnis

Am Ende der Analyse soll klar sein:

- welche fachlichen Konzepte das neue System unterstützen muss,
- welche Teile des originalen USE-Projekts als Referenz relevant sind,
- welche UI-Abläufe aus den Screenshots abgeleitet werden,
- welcher Funktionsumfang zum MVP gehört,
- welche Architekturprinzipien Backend und Frontend leiten,
- welche OCL-Fähigkeiten initial benötigt werden,
- welche Erweiterungen bewusst später eingeplant werden,
- welche technischen Zielrepositories entstehen sollen.

Die spätere Umsetzung soll voraussichtlich in getrennten Repositories oder klar getrennten Codebereichen erfolgen:

- ein neues Backend-Repository für Java/Spring Boot,
- ein neues Frontend-Repository für React/TypeScript,
- optional ergänzende Repositories oder Module für gemeinsame API-Spezifikationen, Testmodelle oder Dokumentation.

## Kurzfazit

Dieses Projekt konzipiert ein neues webbasiertes UML/OCL-System, das fachlich von USE lernt, aber technisch unabhängig entsteht. Der Fokus liegt auf einem modernen, nachvollziehbaren MVP für Klassendiagramme, Objektdiagramme, Snapshots und OCL-basierte Constraint-Validierung.

Das Analyse-Repository schafft dafür die gemeinsame Grundlage: Es verbindet Referenzanalyse, UI-Zielbild, fachlichen Scope, Architekturidee und MVP-Planung zu einer belastbaren Vorbereitung für die spätere Implementierung.
