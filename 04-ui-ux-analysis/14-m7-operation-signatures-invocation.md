# M7 Operation Signatures und Invocation

## Zweck und Status

Dieses Dokument ist das Ergebnis von Mockup-Roadmap-Schritt M7. Es legt die
Bearbeitung von UML-Operationssignaturen und die davon klar getrennte, atomare
Ausführung einer Operation auf einem Objekt fest.

**Status:** `READY_FOR_REVIEW`  
**Verbindliche Mockups:**
`assets/mockups/class-properties-operations.html` für die vollständige
Operationssignatur und `assets/mockups/object-diagram-operation-invocation.html` für
die Runtime Invocation. Die eigentliche Invocation beginnt nach expliziter
Receiver-Auswahl im Object Diagram. Dieses Dokument verbindet weiterhin die
fachlichen Anforderungen an Operationssignatur, Runtime-Vertrag und Zustände.

Das frühere kombinierte Mockup wurde entfernt. Seine relevanten
Signaturfunktionen, einschließlich Parameterreihenfolge, Entfernen von
Parametern, `in`/`out`/`inout` und aller UML-Sichtbarkeiten, sind im
verbindlichen Class-Properties-Mockup enthalten. Eine Invocation aus den Class
Properties ist nicht Bestandteil des festgelegten Workflows.

M7 ist ein Analyse- und Mockup-Schritt. Produktiver Frontend- oder Backend-Code
wurde nicht verändert.

## Verwendete Analysegrundlagen

| Bereich | Verwendete Dokumente |
|---|---|
| Überblick | `00-overview/03-documentation-map.md` |
| UI/UX | `04-ui-ux-analysis/01-ui-overview.md`, `02-class-diagram-ui.md`, `03-object-diagram-ui.md`, `04-ocl-and-validation-ui.md`, `05-screenshot-traceability.md`, `07-m1-ui-baseline.md`, `10-redesign-design-principles.md`, `13-m6-association-classes-aggregation-composition.md` |
| Domäne | `03-uml-ocl-domain/01-uml-ocl-scope.md`, `02-domain-model.md`, `04-validation-concept.md` |
| Frontend | `06-frontend-analysis/03-frontend-architecture.md`, `07-class-diagram-component.md`, `08-object-diagram-component.md`, `11-properties-panel.md`, `12-modal-dialogs.md`, `14-state-management.md` |
| Integration | `07-integration-and-api/01-frontend-backend-contract.md`, `03-validation-flow.md`, `04-project-save-load-flow.md`, `07-dto-reference.md`, `08-error-contract.md` |
| Compliance | `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`, `15-full-ocl-uml-implementation-plan.md` |

Als visuelle Referenz wurden `01-class-diagram-class-properties.png`,
`17-new-class.png`, `06-object-diagram-object-properties.png` und
`07-object-diagram-validation-error.png` geprüft.

Das aktuelle Frontend und Backend wurden nur zur Bestandsaufnahme geprüft. Die
vorhandene Operationsdarstellung kennt Name, Parameter, Rückgabetyp und
teilweise `query`. Parameter besitzen derzeit nur Name und Typ. Vollständige
Sichtbarkeit, abstrakte Operationen, Parameter-Directions und ein fachlicher
Invocation-Vertrag sind noch nicht durchgängig vorhanden. Das originale
USE-Projekt war für M7 nicht erforderlich.

## Abgrenzung

M7 umfasst UML-Operationssignaturen mit Sichtbarkeit, Name, Rückgabetyp,
`abstract`, `query` und geordneten Parametern. Hinzu kommen Receiver-Auswahl,
typisierte Argumente, atomare Invocation, Ergebnis, Rollback, Console sowie
Desktop- und responsive Zustände.

M7 umfasst keine Pre- oder Postconditions, keinen Vor-/Nachzustandsvergleich,
keine Darstellung von `@pre`, `result` oder `oclIsNew()` und keinen Editor für
Operation Bodies oder `def`. Diese Inhalte gehören zu M8 und M9.

## Trennung von Modell und Laufzeit

| Modus | Zweck | Veränderte Daten | Primäre Aktion |
|---|---|---|---|
| `MODEL · Edit signature` | UML-Modell bearbeiten | Operation und Parameter | `Save signature` |
| `RUNTIME · Invoke operation` | Operation auf Snapshot-Objekt ausführen | Objektzustand, falls zulässig | `Invoke operation` |

Die überarbeitete Runtime-Ausführung beginnt im Object Diagram am explizit
ausgewählten Receiver. Das Ändern eines Signaturfelds führt niemals implizit
eine Operation aus. Die Ausführung verändert umgekehrt niemals still die
Modellsignatur. Persistente Moduskennzeichnungen und unterschiedliche Verben
verhindern Verwechslungen.

## Operationssignatur

Die Operationsliste und der Signatur-Editor liegen innerhalb der bestehenden
Properties-Hauptseite `Class`. `Association` und `Invariant` bleiben als
gleichrangige Hauptseiten sichtbar. Die Auswahl einer Operation öffnet keinen
neuen Properties-Haupttab, sondern fokussiert `Operation details` innerhalb der
Class-Seite. Die Runtime-Ausführung bleibt davon getrennt und liegt im
`Operations`-Bereich der Object Properties.

| Feld | Fachliche Bedeutung | Darstellung |
|---|---|---|
| Visibility | `public`, `protected`, Package oder `private` | ausgeschriebene Auswahl mit UML-Symbol `+`, `#`, `~`, `-` |
| Name | unqualifizierter Operationsname | Pflichtfeld mit Signaturvorschau |
| Return Type | fachlicher Rückgabetyp oder kein Rückgabewert | gemeinsamer Type Picker |
| Abstract | keine konkrete Implementierung an diesem Classifier | Toggle; direkte Invocation deaktiviert |
| Query | darf keinen beobachtbaren Zustand verändern | Toggle mit kontextbezogener Hilfe |
| Parameters | geordnete Eingabe-/Ausgabeparameter | verschiebbare, intern scrollbare Liste |

Beispielnotation:

```text
+ transfer(target : BankAccount, amount : Real) : Boolean
+ availableBalance() : Real {query}
```

## Parametereditor

Jede Parameterzeile besitzt stabile Reihenfolge, Name, Typ und Direction. Die
Directions sind zunächst `in`, `out` und `inout`. Der Rückgabewert bleibt ein
separates Signaturfeld.

| Direction | Invocation-Eingabe | Ergebnisdarstellung |
|---|---|---|
| `in` | erforderlich | kein Out Value |
| `out` | keine Eingabe | typisierter Out Value |
| `inout` | erforderlich | aktualisierter Out Value |

Reihenfolgeänderungen besitzen Drag Handle und Tastaturalternative. Das Backend
validiert Namen, Typauflösung und Signaturkonflikte. Frontend-Prüfungen dienen
nur frühem Feedback und ersetzen keine fachliche Backendvalidierung.

## Operation Invocation

Der Operationsbereich zeigt die aufgelöste Signatur des ausgewählten Receivers
und verlangt typisierte Werte für `in` und `inout`. Objektauswahlen
verwenden `objectName : ClassName` statt interner IDs. Das Backend löst anhand
des Laufzeittyps und der Parametersignatur die konkrete Operation auf.

Während der Ausführung bleiben Eingaben und erneutes Absenden deaktiviert. Ein
Erfolg zeigt Rückgabewert, Out Values und geänderte Objekte. Ein Fehler führt zu
einem sichtbaren Rollback; partielle Änderungen dürfen weder gespeichert noch
als Erfolg dargestellt werden.

## Zustände

| Zustand | Festlegung |
|---|---|
| Default | Operation ist im Class Node sichtbar. |
| Selected | Operation und Signaturzeile sind gemeinsam hervorgehoben. |
| Editing | `MODEL`, Signaturfelder, Parameterliste und Vorschau sind sichtbar. |
| Loading | Spinner und deaktivierte Eingaben verhindern Doppelaufrufe. |
| Error | Meldung nennt Parameter und Typ oder Receiver und Operation. |
| Disabled | Abstrakte Operation, fehlender Receiver oder invalide Argumente besitzen eine Begründung. |
| Empty | Ohne ausgewählten kompatiblen Receiver erklärt der Operationsbereich den nächsten Schritt. |
| Confirmation | Ungespeicherte Änderungen werden vor dem Verwerfen bestätigt. |
| Success | Ausführung bestätigt Operation und Receiver. |
| Result | Rückgabewert, Typ, Out Values und Änderungen sind sichtbar. |
| Rollback | Die UI bestätigt ausdrücklich, dass nichts persistiert wurde. |

## Console, Responsive und Accessibility

Console und Ergebnis verwenden fachliche Namen und sind intern scrollbar.
Technische IDs und Stacktraces bleiben optionalen Details vorbehalten. Im
schmalen Viewport bleiben Operationsproperties in einem scrollbaren Drawer.
Eingabe und Ergebnis wechseln innerhalb dieses Drawers. Nur eine erforderliche
Bestätigung weitreichender Auswirkungen öffnet einen fokussierten Dialog.

Der Kernweg bleibt einfach, fortgeschrittene Parameteroptionen werden
progressiv offengelegt. UML-Sichtbarkeit wird ausgeschrieben, Controls besitzen
sichtbaren Fokus und ausreichenden Kontrast. Quick Help erklärt Signatur,
`query` und Invocation im Kontext. Berücksichtigt werden `RD-NAV-001` bis
`RD-NAV-005`, `RD-VIS-001` bis `RD-VIS-004` und `RD-A11Y-001` bis
`RD-A11Y-005`.

### Visuelle Konsistenzkorrektur

M7 verwendet dieselbe Anwendungsshell und dieselbe Properties-Hierarchie wie
die übergreifende Association-Dokumentationsansicht. Die Hauptseiten `Class`,
`Association` und `Invariant` sind echte, gleich breite Segmente; die
Operationssignatur bleibt ein Unterbereich von `Class`. Die frühere
Pseudo-Navigation über generierten CSS-Inhalt wurde entfernt.

Der Desktop-Workspace verwendet einen stabilen Explorer, eine flexible
Canvas-Spalte und ein intern scrollbar bleibendes Properties Panel. Die
Parameterzeilen besitzen begrenzte Grid-Spalten und erzeugen keinen
horizontalen Überlauf mehr. Headeraktionen, Projektdarstellung, Abstände und
der schmale Viewport folgen der gemeinsamen Mockup-Shell. Die sichtbaren
Hauptaktionen des Signatur-Editors sind auf Deutsch vereinheitlicht.

## Compliance-Zuordnung

| Matrix-ID | M7-Bezug |
|---|---|
| `CM-UML-015` | geordnete Operationssignatur und spätere Dispatch-Auflösung |
| `CM-UML-016` | explizite atomare Invocation, Ergebnis und Rollback |
| `CM-OCL-011` | gemeinsame Auflösung von Property-/Operation Calls als Abhängigkeit |
| `CM-UML-017` | Receiver und Objektänderungen; vollständiger Lebenszyklus bleibt außerhalb M7 |

M7 beansprucht insbesondere nicht `CM-OCL-025`, `CM-CTX-002` oder
`CM-CTX-003`, da diese Pre-/Postcondition-Kontexte zu M8 gehören.

## Vorläufige DTO- und API-Anforderungen

```ts
type VisibilityDto = 'public' | 'protected' | 'package' | 'private';
type ParameterDirectionDto = 'in' | 'out' | 'inout';

interface UmlOperationDto {
  id: string;
  name: string;
  visibility: VisibilityDto;
  abstract: boolean;
  query: boolean;
  parameters: UmlParameterDto[];
  returnType: TypeReferenceDto | null;
}

interface UmlParameterDto {
  id: string;
  name: string;
  type: TypeReferenceDto;
  direction: ParameterDirectionDto;
  position: number;
}

interface OperationInvocationRequestDto {
  receiverObjectId: string;
  operationId: string;
  arguments: ArgumentValueDto[];
  expectedRevision: number;
}

interface OperationInvocationResultDto {
  invocationId: string;
  status: 'succeeded' | 'rolled_back';
  receiver: NamedObjectReferenceDto;
  resolvedOperation: OperationSignatureDto;
  result: TypedValueDto | null;
  outValues: NamedTypedValueDto[];
  changedObjects: NamedObjectReferenceDto[];
  diagnostics: DiagnosticDto[];
  revision: number;
}
```

Das Backend benötigt verlustfreie Signaturen, zentrale Overload-/Dispatch-
Auflösung, Parameterbindung, typisierte Ergebnisse, Query-Schutz und eine
atomare, revisionsgebundene Ausführung. Signaturmutation und Invocation bleiben
getrennte Endpunkte. Fehler referenzieren Parameter, erwarteten und erhaltenen
Typ, Receiver und Operation strukturiert.

## Vorläufige Frontend-Anforderungen

- eigener Selection-Typ für Operationen, der die bestehende Hauptseite `Class`
  und dort den Operationsunterbereich fokussiert,
- scrollbares Operation Properties Panel mit Moduskennzeichnung,
- gemeinsamer Type Picker und zugängliche Parameterreihenfolge,
- getrennte Commands für Speichern und Ausführen,
- Receiver-Filter nach kompatiblem Laufzeittyp,
- typisierte Argumenteditoren,
- Sperre gegen doppeltes Absenden,
- keine optimistische Snapshot-Mutation vor Backend-Erfolg,
- Result und Rollback mit fachlichen Namen,
- scrollbare Console und responsive Drawer-/Dialogumschaltung.

## Annahmen und offene Entscheidungen

| Art | Punkt |
|---|---|
| Annahme | Parameterreihenfolge ist Bestandteil der Signatur. |
| Annahme | `out` besitzt keine Eingabe; `inout` besitzt Ein- und Ausgabewert. |
| Annahme | Abstrakte Operationen können nicht direkt ausgeführt werden. |
| Annahme | Jede Invocation ist atomar und revisionsgebunden. |
| Offen | Wie ausführbare Semantik bereitsteht, bevor M9 Operation Bodies festlegt. |
| Offen | Ob `out` und `inout` vollständig zum ersten Zielprofil gehören. |
| Offen | Regeln für Overloading und Signatur-Eindeutigkeit. |
| Offen | Anzeige statischer und tatsächlich aufgelöster Operation bei Dispatch. |
| Offen | Technische Durchsetzung des Query-Mutationsverbots. |
| Offen | Darstellung von fehlendem Rückgabewert, `invalid` und `null`. |
| Offen | Timeout- und Abbruchsemantik langer Aufrufe. |

## Akzeptanzkriterien

- [x] Signaturbearbeitung und Runtime Invocation sind sichtbar getrennt.
- [x] Visibility, Name, Return Type, Abstract und Query sind dargestellt.
- [x] Parameter besitzen Reihenfolge, Name, Typ und Direction.
- [x] Receiver und typisierte Argumente sind fachlich beschriftet.
- [x] Loading, Success, Result, Validation und Rollback sind entworfen.
- [x] Default, Selected, Editing, Disabled, Empty und Confirmation sind dokumentiert.
- [x] Console und Result verwenden fachliche Namen statt primär interner IDs.
- [x] Desktop und schmaler Viewport sind festgelegt.
- [x] Properties, Console und umfangreiche Ergebnisse sind intern scrollbar.
- [x] Backend-, API-, DTO-, Frontend- und Compliance-Folgen sind dokumentiert.
- [x] M8- und M9-Funktionen wurden nicht vorgezogen.

## Ergebnis

M7 definiert einen verständlichen Operationsworkflow mit zwei klaren Modi. Das
UML-Modell wird über eine vollständige, geordnete Signatur gepflegt. Die
Ausführung erfolgt bewusst auf einem benannten Receiver und endet entweder in
einem atomaren Commit mit typisiertem Ergebnis oder einem expliziten Rollback.
Damit ist die UI-Grundlage gelegt, ohne M8 und M9 vorwegzunehmen.
