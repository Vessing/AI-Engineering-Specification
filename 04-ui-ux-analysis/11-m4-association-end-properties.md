# M4 Erweiterte Association-End-Properties

## Zweck und Status

Dieses Dokument ist das Ergebnis von Mockup-Roadmap-Schritt M4. Es definiert
die Benutzeroberfläche für erweiterte Metadaten binärer UML Association Ends.
Die konsolidierte visuelle Referenz liegt unter
`assets/mockups/association-properties.html`. Sie verbindet den M4-Workflow
mit den darauf aufbauenden Association-Funktionen aus M5 und M6, ohne die in
diesem Dokument festgelegte fachliche Abgrenzung von M4 zu verändern.

**Status:** `READY_FOR_REVIEW`  
**Gesamtstatus:** `MOCKUP`, noch nicht `BACKEND_READY`.

M4 implementiert keine fachliche Funktion. Das Mockup beschreibt die spätere
Darstellung, Interaktion und die dafür erforderlichen Verträge.

## Verwendete Quellen

### Analyse-Dokumente

- `00-overview/03-documentation-map.md`
- `03-uml-ocl-domain/01-uml-ocl-scope.md`
- `03-uml-ocl-domain/02-domain-model.md`
- `03-uml-ocl-domain/04-validation-concept.md`
- `04-ui-ux-analysis/01-ui-overview.md`
- `04-ui-ux-analysis/02-class-diagram-ui.md`
- `04-ui-ux-analysis/05-screenshot-traceability.md`
- `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md`
- `04-ui-ux-analysis/07-m1-ui-baseline.md`
- `04-ui-ux-analysis/08-m2-generalization-and-abstract-classes.md`
- `04-ui-ux-analysis/09-m3-visibility-namespaces-imports.md`
- `04-ui-ux-analysis/10-redesign-design-principles.md`
- `06-frontend-analysis/03-frontend-architecture.md`
- `06-frontend-analysis/06-diagram-library-decision.md`
- `06-frontend-analysis/07-class-diagram-component.md`
- `06-frontend-analysis/11-properties-panel.md`
- `06-frontend-analysis/14-state-management.md`
- `07-integration-and-api/01-frontend-backend-contract.md`
- `07-integration-and-api/07-dto-reference.md`
- `07-integration-and-api/08-error-contract.md`
- `09-ocl-extension-analysis/14-full-ocl-uml-compliance-matrix.md`
- `09-ocl-extension-analysis/15-full-ocl-uml-implementation-plan.md`

### Screenshots

| Screenshot | Übernommenes Muster |
|---|---|
| `02-class-diagram-association-properties.png` | selektierte Association, Association-Name in der Edge-Mitte und Properties-Kontext |
| `10-modal-add-class-association.png` | getrennte Klassen und Rollen beider Ends |
| `15-properties-association.png` | segmentiertes, intern scrollbar bleibendes Properties Panel |

### Aktueller Anwendungscode

Der aktuelle Frontend-Code wurde nur gelesen. Besonders relevant waren:

- `use-web-frontend/src/features/class-diagram/properties/AssociationPropertiesPanel.tsx`
- `use-web-frontend/src/features/diagram-core/components/UmlAssociationEdge.tsx`

Das aktuelle Panel unterstützt Association-Name, Rollenname und Multiplizität
beider Ends. Die Edge bindet den Association-Namen an die Mitte und Rollen sowie
Multiplizitäten an berechnete Endpositionen. Der Backend- und DTO-Stand enthält
zusätzlich bereits `navigable`; die übrigen M4-Metadaten sind noch nicht Teil
des stabilen Vertrags.

## Abgrenzung

M4 behandelt ausschließlich binäre Association Ends und folgende Felder:

- Rollenname,
- Multiplizität,
- `navigable`,
- `ordered`,
- `unique`,
- `derived`,
- `union`,
- `subsets`,
- `redefines`.

Aggregation Kind wird nur als deaktivierter Hinweis auf M6 gezeigt. Qualifier
und n-äre Associations gehören zu M5. Aggregation, Composition und ihre
Lebenszyklusregeln gehören zu M6. M4 entwirft weder Association Classes noch
Derived-OCL-Ausdrücke.

## Einfachster erfolgreicher Kernweg

1. Der Nutzer wählt eine Association im Explorer oder auf dem Canvas aus.
2. Das Properties Panel zeigt Association-Name und beide Ends mit fachlichen
   Klassennamen.
3. Rollenname und Multiplizität werden zuerst geprüft und bearbeitet.
4. Bei Bedarf werden `navigable`, `ordered` und `unique` geändert.
5. Die abgeleitete Navigationsergebnisart wird unmittelbar als Vorschau
   angezeigt.
6. Die Änderung wird atomar an das Backend gesendet.
7. Eine lokale Bestätigung nennt Association und betroffenes Ende.

`derived`, `union`, `subsets` und `redefines` liegen im aufklappbaren Bereich
`Fortgeschrittene End-Metadaten`. Sie bleiben vollständig erreichbar, belasten
den grundlegenden Association-Workflow aber nicht.

## Desktop-Hauptzustand

Das Class Diagram zeigt die Association `Borrows` zwischen `Member` und `Book`.
Der Association-Name bleibt in der Edge-Mitte. Am `Book`-Ende stehen
`borrowedBooks 0..*` und am `Member`-Ende `borrower 1`. Diese Zuordnung folgt
dem Typ des jeweiligen Association Ends und bleibt unabhängig davon, welche
Klasse links oder rechts angeordnet ist.

Die selektierte Edge öffnet `Association Properties`. Beide Ends erscheinen als
separat beschriftete, aufklappbare Abschnitte. Der Abschnittstitel kombiniert
Klassenname, Rollenname und Multiplizität. Dadurch bleiben die Enden auch bei
reflexiven Associations oder nach dem Verschieben von Klassen unterscheidbar.

## Felder eines Association Ends

| Feld | UI-Control | Darstellung und Regel |
|---|---|---|
| Class | Read-only Text | fachlicher Classifier-Name, keine sichtbare interne ID |
| Role name | Textfeld | OCL-Navigationsname; erforderliche Eindeutigkeit prüft das Backend |
| Multiplicity | Textfeld mit Vorschau | akzeptiert die später vertraglich definierten UML-Bereiche |
| Navigable | Toggle | zeigt an, ob das Ende als navigierbar modelliert ist; genaue OCL-Profilwirkung bleibt offen |
| Ordered | Toggle | beteiligt sich an der Collection-Art mehrwertiger Navigation |
| Unique | Toggle | beteiligt sich an der Collection-Art mehrwertiger Navigation |
| Derived | Toggle | kennzeichnet ein abgeleitetes Ende; der Ableitungsausdruck ist nicht Teil von M4 |
| Union | abhängiger Toggle | ist nur verfügbar, wenn `derived` aktiv ist |
| Subsets | referenzierender Picker | wählt kompatible Association Ends anhand fachlicher Namen |
| Redefines | referenzierender Picker | wählt kompatible geerbte Association Ends anhand fachlicher Namen |
| Aggregation kind | deaktivierter Slot | zeigt `None` und verweist auf M6, besitzt in M4 keine Wirkung |

## Collection-Art aus Ordered und Unique

Bei einer oberen Multiplizitätsgrenze größer als eins oder `*` zeigt das Panel
eine Vorschau der späteren OCL-Navigation:

| Ordered | Unique | Navigationsergebnis |
|---|---|---|
| false | true | `Set(T)` |
| false | false | `Bag(T)` |
| true | false | `Sequence(T)` |
| true | true | `OrderedSet(T)` |

Bei einer oberen Grenze von eins bleibt die Navigation einzelwertig. Das
Frontend zeigt die Ergebnisart nur als aus Backend-/DTO-Daten abgeleitete
Vorschau. Die normative Typableitung liegt später im Backend-Typechecker.

## Subsets und Redefines

`subsets` und `redefines` sind Beziehungen zwischen stabil identifizierten
Association Ends. Die UI zeigt Referenzen im Format
`Association · roleName`, überträgt aber IDs. Freitext ist nicht zulässig.

Der Picker filtert lediglich offensichtlich ungeeignete Kandidaten. Das Backend
prüft mindestens Existenz, Modellzugehörigkeit, Typkonformität, Multiplizität,
Zyklen und die für `redefines` erforderliche Generalisierungsbeziehung. Eine
Änderung wird nur vollständig oder gar nicht gespeichert.

## Derived und Union

`derived` kennzeichnet, dass der Wert eines Ends nicht unmittelbar aus normalen
Links stammt. M4 erfasst nur das Metadatum. Die Definition und Auswertung eines
Ableitungsausdrucks gehört zu späteren OCL-Kontextschritten.

`union` wird in der UI als abhängige Option behandelt und setzt `derived`
voraus. Wird `derived` deaktiviert, obwohl `union` aktiv ist, verlangt die UI
eine Bestätigung oder das Backend lehnt die inkonsistente Änderung strukturiert
ab. Das Frontend erfindet keine Union-Semantik.

## Edge- und Labelverhalten

- Der Association-Name bleibt in der Mitte des berechneten Edge-Pfads.
- Rollenname und Multiplizität bleiben am jeweiligen Association End.
- Zusätzliche Metadaten werden kompakt unter dem Endlabel gezeigt, sofern sie
  aktiv sind.
- Lange Metadatenlisten werden nicht vollständig auf den Canvas geschrieben.
  Das Properties Panel enthält die vollständige Information.
- Nach Drag, Zoom oder Pan werden alle Labelpositionen aus den aktuellen
  Edge-Koordinaten neu berechnet.
- Labels dürfen keine Handles blockieren und sollen bei Kollisionen eine
  definierte Offset-Strategie verwenden.

M4 verlangt noch keinen manuellen Bendpoint-Editor und kein vollständiges
automatisches Edge Routing.

## Zustände

| Zustand | Darstellung |
|---|---|
| Default | Association ist nicht selektiert; Edge zeigt Name, Rollen und Multiplizitäten |
| Selected | Edge besitzt sichtbare Kontur; Association Properties sind geöffnet |
| Editing | geänderte Felder und noch nicht geprüfter Status sind sichtbar |
| Loading | Apply zeigt Fortschritt; erneutes Apply ist deaktiviert; vorhandene Edge bleibt sichtbar |
| Error | Inline-Meldung nennt Association, Rolle, Feld und fachlichen Konflikt |
| Disabled | abhängige Option nennt den Grund, etwa `Union requires Derived` |
| Empty | `No referenced ends` statt einer unbeschrifteten leeren Liste |
| Confirmation | Erfolg nennt das gespeicherte End; Verwerfen ungespeicherter Änderungen verlangt Bestätigung |

Technische Codes und IDs sind nur in aufklappbaren Details sichtbar.

## Responsive Verhalten

Im schmalen Viewport bleibt der Canvas Primärfläche. Explorer, Association
Properties und Quick Help öffnen einzeln als Drawer. Der Properties-Drawer ist
intern scrollbar, besitzt einen sichtbaren Schließen-Button und kann mit
`Escape` geschlossen werden. Der Fokus wechselt beim Öffnen in den Drawer und
kehrt anschließend zur auslösenden Aktion oder selektierten Edge zurück.

Primäre Controls sind im schmalen Viewport mindestens `44 px` hoch. Lange
fachliche Namen umbrechen innerhalb ihrer Zeile und vergrößern nicht
unkontrolliert den Drawer.

## Accessibility, Modernisierung und Hilfe

M4 übernimmt die M1-Baseline:

- Formtexte sind mindestens `14 px` groß.
- Diagrammtext ist mindestens `13 px` groß.
- Edge Labels und kompakte Metadaten sind mindestens `12 px` groß.
- Desktop-Controls sind mindestens `40 px` hoch.
- Normaler Text erreicht mindestens `4,5:1` Kontrast.
- Relevante UI-Grenzen erreichen mindestens `3:1` Kontrast.
- Der sichtbare Tastaturfokus ist mindestens `3 px` stark.
- On/Off-Zustände besitzen Text und werden nicht nur durch Farbe vermittelt.

Quick Help erklärt Association End, Rollenname, Multiplizität sowie die Wirkung
von `ordered` und `unique`. Inline-Hilfe erklärt abhängige Felder. Ausführliche
UML- und OCL-Semantik bleibt in Documentation und Examples.

Damit adressiert M4 `RD-NAV-001` bis `RD-NAV-005`, `RD-VIS-001` bis
`RD-VIS-004` und `RD-A11Y-001` bis `RD-A11Y-005`.

## Compliance-Zuordnung

| Matrix-ID | M4-Bezug |
|---|---|
| `CM-UML-007` | bestehende binäre Association bleibt Grundlage |
| `CM-UML-008` | UI für `ordered` und `unique` sowie Collection-Vorschau |
| `CM-UML-012` | fachliche Endnamen unterstützen später eindeutige reflexive Rollen |
| `CM-UML-014` | UI für `subsets`, `redefines`, `derived` und `union` |
| `CM-OCL-013` | End-Metadaten beeinflussen spätere Association-End-Navigation |
| `CM-OCL-021` | Collection-Art beeinflusst spätere Iterator-Ergebnisarten indirekt |

Keine Matrix-ID wird durch das Mockup technisch erfüllt.

## Anforderungen an Backend, API und DTOs

`UmlAssociationEndDto` benötigt später zusätzlich zu den vorhandenen Feldern:

```ts
interface UmlAssociationEndDto {
  id: Id;
  classId: Id;
  roleName: string;
  multiplicity: MultiplicityDto;
  navigable: boolean;
  ordered: boolean;
  unique: boolean;
  derived: boolean;
  union: boolean;
  subsettedEndIds: Id[];
  redefinedEndIds: Id[];
  aggregationKind?: 'none' | 'shared' | 'composite'; // erst M6 aktiv
}
```

Die endgültigen Namen sind eine Vertragsentscheidung. Backend, JSON-Format und
TypeScript dürfen nicht stillschweigend unterschiedliche Bezeichnungen nutzen.

| Bereich | Spätere Anforderung |
|---|---|
| Persistenz | alle M4-Metadaten verlustfrei speichern und laden |
| Update | Association und beide Ends atomar aktualisieren |
| Referenzen | `subsets`/`redefines` über stabile End-IDs übertragen |
| Validation | strukturierte Fehler für inkompatible Typen, Multiplizitäten, Zyklen und unbekannte Ends |
| OCL Typechecker | Navigationsergebnis aus Typ, Multiplizität, `ordered` und `unique` ableiten |
| Evaluator | Reihenfolge und Duplikatsemantik passend zum deklarierten Navigationstyp erhalten |
| Error Contract | Association-ID und End-ID intern mappen, fachliche Namen für die UI liefern |

## Anforderungen an das Frontend

- `AssociationPropertiesPanel` benötigt wiederverwendbare End-Abschnitte.
- Das Frontend muss Source/Target nicht als fachliche Semantik behandeln,
  sondern jedes End über stabile ID und Classifier identifizieren.
- Referenzpicker benötigen fachliche Labels und ID-basierte Werte.
- Das Edge ViewModel benötigt aktive End-Metadaten für kompakte Labels.
- Labelgeometrie muss bei Node-Drag, Zoom und Pan stabil neu berechnet werden.
- Optimistische Vorschau darf Backend-Ablehnung nicht als Erfolg darstellen.
- Properties, Quick Help und technische Details benötigen eigene Scrollbereiche.
- Selection State muss Association und konkretes Association End unterscheiden
  können, falls ein Endlabel direkt auswählbar wird.

## Annahmen und offene Entscheidungen

| Typ | Eintrag |
|---|---|
| Annahme | M4 bleibt auf binäre Associations beschränkt. |
| Annahme | `union` setzt ein abgeleitetes End voraus. |
| Annahme | obere Multiplizitätsgrenze eins ergibt einzelwertige Navigation unabhängig von `ordered`/`unique`. |
| Offen | genaue Profilentscheidung für OCL-Navigation über `navigable = false` |
| Offen | ob konkrete Association Ends direkt auf dem Canvas auswählbar werden oder nur über ihren Association-Kontext |
| Offen | endgültige API-Feldnamen und eigener End-Update-Endpunkt gegenüber atomarem Association Update |
| Offen | konkrete Kollisionsstrategie für lange oder sich kreuzende Endlabels |
| Offen für M6 | Aktivierung und Semantik von `aggregationKind` |

## Überprüfbare Akzeptanzkriterien

- [x] Beide Ends sind durch fachliche Classifier-, Rollen- und
  Multiplizitätsnamen unterscheidbar.
- [x] `ordered`, `unique`, `navigable`, `derived`, `union`, `subsets` und
  `redefines` besitzen geeignete Controls.
- [x] `subsets` und `redefines` verwenden Referenzpicker statt Freitext.
- [x] Die Navigationsergebnisart aus `ordered` und `unique` ist sichtbar.
- [x] Association-Name und Endlabels besitzen getrennte, stabile Positionen.
- [x] Default-, Selected-, Editing-, Loading-, Error-, Disabled-, Empty- und
  Confirmation-Zustände sind dargestellt.
- [x] Desktop und schmaler Viewport sind entworfen.
- [x] Properties und umfangreiche Details sind intern scrollbar.
- [x] Quick Help, Fokusführung, Mindestgrößen und Kontrast sind dokumentiert.
- [x] Aggregation ist nur als M6-Platzhalter und ohne vorweggenommene Semantik
  enthalten.
- [x] Backend-, API-, DTO- und Frontendfolgen sind dokumentiert, aber nicht
  implementiert.
- [x] Produktiver Frontend- und Backend-Code blieb unverändert.
- [ ] Fachliche Freigabe durch den Auftraggeber steht aus.
- [ ] `BACKEND_READY` bleibt offen.

## Zusammenfassung

M4 erweitert das Association Properties Pattern um verständlich gruppierte
Association Ends. Der einfache Weg bleibt auf Rollen und Multiplizitäten
fokussiert; zusätzliche UML-Metadaten werden progressiv offengelegt. Das
Mockup hält Association-Name und Endlabels geometrisch getrennt, zeigt die
Collection-Auswirkung von `ordered` und `unique` und überlässt sämtliche
fachliche Validierung dem späteren Backend.
