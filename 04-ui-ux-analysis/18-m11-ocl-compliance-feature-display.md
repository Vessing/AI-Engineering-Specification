# M11: OCL Compliance und Featureanzeige

## Status und Zweck

**Status:** `READY_FOR_REVIEW`  
**Mockup:** `assets/mockups/ocl-compliance-feature-display.html`

M11 legt fest, wie das Frontend den maschinenlesbaren OCL-Unterstützungsumfang
ehrlich und verständlich darstellt. Die Anzeige unterstützt Benutzer bei der
Auswahl geeigneter Sprachfeatures, ersetzt aber weder Backenddiagnostik noch
fachliche Dokumentation. Sie behauptet ausdrücklich keine vollständige
OCL-2.4-Compliance.

## Verwendete Grundlagen

| Bereich | Verwendete Dokumente |
|---|---|
| Einstieg | `00-overview/03-documentation-map.md`, `04-ui-ux-analysis/06-full-ocl-uml-mockup-roadmap.md` |
| UI/UX | `01-ui-overview.md`, `04-ocl-and-validation-ui.md`, `05-screenshot-traceability.md`, `07-m1-ui-baseline.md`, `10-redesign-design-principles.md` |
| OCL | `03-uml-ocl-domain/03-ocl-architecture-and-extension-strategy.md`, `04-validation-concept.md`, `09-ocl-extension-analysis/08-ocl-error-handling-and-source-locations.md` |
| Compliance | `09-ocl-extension-analysis/13-ocl-compliance-profile.md`, `14-full-ocl-uml-compliance-matrix.md`, `15-full-ocl-uml-implementation-plan.md` |
| Frontend/API | `06-frontend-analysis/03-frontend-architecture.md`, `09-ocl-editor-ui.md`, `10-validation-error-ui.md`, `07-integration-and-api/01-frontend-backend-contract.md`, `08-error-contract.md` |

Als visuelle Referenz wurden `07-object-diagram-validation-error.png`,
`13-ocl-editor.png`, `03-class-diagram-invariant-properties.png` und
`16-properties-invariants.png` geprüft. Der OCL Editor bleibt dabei wie im
aktuellen Frontend ohne Explorer und Properties Sidebar.

Im Backend wurden `OclProfileController`, `OclComplianceProfileDto`,
`OclComplianceProfile`, `OclFeatureSupport` und `OclFeatureStatus` geprüft.
`GET /api/v1/ocl/profile` liefert bereits Profilidentität, OCL-Version,
Compliance Claim, API-Version, optionale Compliance Points, Featureliste und
Runtime-Limits. Im aktuellen Frontend wurde noch keine Profile-API oder
Featureanzeige gefunden. Produktiver Code wurde nicht verändert. Das originale
USE-Projekt musste für M11 nicht geprüft werden, weil OMG OCL 2.4 und das
Backendprofil die maßgeblichen Quellen sind.

## Informationsarchitektur

M11 ergänzt den vorhandenen OCL Editor auf drei Ebenen:

1. In der Toolbar steht eine kompakte Schaltfläche `OCL 2.4 · Subset` mit
   `View support`.
2. Ein intern scrollbarer Detaildialog zeigt Profil, Claim, Statusübersicht,
   Suche und Features.
3. Bei konkret verwendeter nicht unterstützter Syntax erscheint eine lokale
   Diagnostic am Ausdruck mit direktem Zugang zum zugehörigen Feature.

Die vollständige Featureliste steht nicht permanent neben dem Editor. Dadurch
bleibt der Kernworkflow einfach und die fortgeschrittene Information wird bei
Bedarf offengelegt.

## Statusmodell

| Status | Nutzerbedeutung | UI-Verhalten |
|---|---|---|
| `SUPPORTED` | Der dokumentierte Teilumfang ist im Backend implementiert und regulär getestet. | grüne Textmarke `Supported`; normale Verwendung |
| `PARTIAL` | Ein fachlich nutzbarer Teil existiert, aber Grenzen bleiben. | gelbe Textmarke `Partial`; Details nennen Available und Limit; kein pauschales Editor-Blocking |
| `NOT_SUPPORTED` | Das Feature gehört zum Profil, ist aber nicht implementiert. | rote Textmarke `Not supported`; lokale Diagnostic bei Verwendung |
| `OUT_OF_SCOPE` | Das Feature ist bewusst kein Produktziel. | graue Textmarke `Out of scope`; Begründung statt Umsetzungsversprechen |

Farbe ist nur ein redundantes Signal. Jeder Status besitzt vollständigen Text,
eine Beschreibung und bei `PARTIAL` eine konkrete Grenze.

## Entworfene Ansichten und Zustände

### Desktop-Hauptzustand

Der bestehende OCL Editor erhält in seiner Toolbar eine kompakte
Subset-Schaltfläche. Sie enthält weder eine Prozentzahl noch ein irreführendes
„compliant“. Der Editor und `Apply Changes` bleiben der visuelle Schwerpunkt.

### Feature-Detaildialog

Der Dialog zeigt:

- `profileId`, OCL- und API-Version,
- den unveränderten Compliance Claim,
- Anzahl der Features je Status,
- Suche über Gruppe, Feature-ID und Beschreibung,
- Status, Featuregruppe, Standardbasis und Notes,
- optionale Compliance Points und Runtime-Limits,
- Zeitpunkt beziehungsweise Cachezustand der geladenen Antwort.

Die Detailansicht eines `PARTIAL`-Features trennt ausdrücklich `Available` und
`Limit`. Die UI rechnet aus dem Status keine nicht gelieferten Fähigkeiten ab.

### Nicht unterstütztes Feature im Editor

Das Mockup markiert eine `OclMessage`-Expression. Eine lokale Diagnostic nennt
`OCL_FEATURE_NOT_SUPPORTED`, Source Range, Profil-ID und einen Link zum Eintrag
`OCL-PROFILE-012`. Der Text darf im Editor bleiben; Anwenden beziehungsweise
Validieren dieses Ausdrucks kann fehlschlagen, andere unterstützte Ausdrücke
werden aber nicht pauschal blockiert.

### About/Engine Details

Ein kompakter Bereich zeigt Profil-ID, OMG-OCL-Basis, API-Version, aktivierte
optionale Compliance Points und Runtime-Limits. Reference-Test-Zahlen dürfen
optional in technischen Details erscheinen, werden aber nicht als
Compliance-Prozentwert interpretiert.

### Loading, Offline, Error und Empty

- Beim Laden ist `View support` deaktiviert und erhält einen textlichen
  Loading-Status.
- Bei Netzwerk- oder API-Fehler bleibt der Editor benutzbar. Die UI sagt, dass
  der Status aktuell nicht verifiziert werden kann, und bietet `Retry`.
- Eine leere oder schemawidrige Featureliste wird nicht als vollständige
  Unterstützung interpretiert, sondern als nicht vertrauenswürdige Antwort.
- Eine bestätigte Cacheantwort darf angezeigt werden, muss aber Profilversion
  und Alter nennen.

Ein Confirmation-Zustand ist nicht erforderlich, da M11 ausschließlich
lesende Informationen und keine destruktive Mutation enthält.

### Schmaler Viewport

Die Featureliste öffnet als intern scrollbarer Drawer. Suche, Profilidentität
und Statusbedeutung bleiben erreichbar. Nach dem Schließen kehrt der Fokus zur
Subset-Schaltfläche zurück.

## Interaktionsregeln

1. Die Subset-Schaltfläche öffnet den Detaildialog, ohne Editorinhalt oder
   Auswahl zu verändern.
2. Suche filtert nur die bereits vom Backend gelieferten Einträge.
3. Auswahl eines Features öffnet Standardbasis, verfügbaren Umfang, Grenze und
   Notes.
4. Eine Feature-Diagnostic springt zum passenden Profileintrag; von dort kann
   der Benutzer zur Source Range zurückkehren.
5. `PARTIAL` blockiert nicht automatisch das gesamte Feature. Entscheidend sind
   Backenddiagnostik und die dokumentierte Grenze.
6. `NOT_SUPPORTED` und `OUT_OF_SCOPE` werden weder sprachlich noch visuell
   gleichgesetzt.
7. Bei nicht verfügbarem Profil bleibt der Editor funktionsfähig; die UI darf
   keinen lokalen `SUPPORTED`-Fallback erfinden.
8. Profiländerungen werden anhand `profileId` beziehungsweise API-Version
   erkannt und aktualisieren Anzeige und Cache atomar.

## Compliance-Zuordnung

M11 visualisiert das Backendprofil `OCL-PROFILE-001` bis
`OCL-PROFILE-016`. Die Detailansicht kann auf die feingranularen Einträge
`CM-OCL-001` bis `CM-OCL-027`, `CM-LIB-001` bis `CM-LIB-015`, `CM-CTX-001`
bis `CM-CTX-008` und `CM-UML-001` bis `CM-UML-019` verweisen.

Besonders sichtbar sind:

| ID | M11-Abdeckung |
|---|---|
| `CM-OCL-001` | Lexer-/Syntaxumfang wird nicht pauschal als vollständig bezeichnet. |
| `CM-OCL-002` | Featurediagnostic besitzt Source Range. |
| `CM-OCL-026` | `OclMessage` wird korrekt als nicht unterstützt angezeigt. |
| `CM-OCL-027`, `CM-UML-019` | State Machines und `oclInState` werden über `OCL-PROFILE-016` als außerhalb des Zielprofils angezeigt. |
| `CM-CTX-002`, `CM-CTX-003` | Operation Contracts werden als `PARTIAL` erklärt. |
| `CM-CTX-004` bis `CM-CTX-007` | Derived/Init/Body/Def werden als `PARTIAL` erklärt. |
| `CM-OCL-013` | Unterstützte Navigation ist verifiziert; der optionale Zugriff über nicht navigierbare Ends wird über `OCL-PROFILE-013` als nicht unterstützt ausgewiesen. |
| `CM-UML-004` | UML-Sichtbarkeit ist verifiziert; ein optionaler Sichtbarkeits-Bypass wird über `OCL-PROFILE-015` als nicht unterstützt ausgewiesen. |

Die Anzeige ist eine Projektion des maschinenlesbaren Profils und keine zweite,
manuell gepflegte Compliance-Wahrheit.

## Vorläufige API- und DTO-Anforderungen

Der vorhandene Backendvertrag entspricht im Kern:

```ts
type OclFeatureStatus =
  | 'SUPPORTED'
  | 'PARTIAL'
  | 'NOT_SUPPORTED'
  | 'OUT_OF_SCOPE';

interface OclFeatureSupportDto {
  id: string;
  group: string;
  status: OclFeatureStatus;
  standardBasis: string;
  notes: string;
}

interface OclComplianceProfileDto {
  profileId: string;
  oclVersion: string;
  complianceClaim: string;
  apiVersion: string;
  enabledOptionalCompliancePoints: string[];
  features: OclFeatureSupportDto[];
  runtimeLimits: Record<string, number>;
}
```

B22 ergänzt die variablen Schlüssel `maxSourceCharacters`, `maxDiagnostics`
und `maxResultElements`. Die bestehende generische Darstellung kann diese ohne
neuen DTO-Typ anzeigen. `SOURCE_LIMIT_EXCEEDED`,
`DIAGNOSTIC_LIMIT_EXCEEDED` und `RESULT_LIMIT_EXCEEDED` werden wie andere
strukturierte OCL-Diagnostics behandelt.

Später hilfreich, aber noch abzustimmen, sind:

- ein stabiler Diagnostic-Bezug `featureId`,
- optional getrennte Felder für `availableCapabilities` und `limitations`,
- dokumentierte Cache-/HTTP-Semantik über `ETag` oder äquivalente Versionierung,
- ein Schemafehler, falls Status oder Pflichtfelder unbekannt sind,
- keine projektabhängige Vermischung des Engineprofils mit dem Modellzustand.

## Spätere Frontend-Anforderungen

- Profile-API-Client und streng typisierte DTOs,
- kompakte Subset-Schaltfläche im vorhandenen OCL-Editor-Toolbar,
- zugänglicher Dialog/Drawer mit Suche und interner Scrollbarkeit,
- gemeinsames Status-Badge mit Text, Icon und ausreichendem Kontrast,
- lokale Diagnostic-Verknüpfung zwischen Source Range und Feature-ID,
- Loading-, Offline-, Schemafehler- und Retry-Zustände,
- Cache nur versionsgebunden und ohne optimistische Hochstufung,
- keine permanente Seitenleiste und keine Blockade des gesamten Editors bei
  `PARTIAL`,
- Tastaturfokus und Screenreader-Beschreibungen für Status und Grenzen.

## Annahmen und offene Entscheidungen

**Annahmen**

- `GET /api/v1/ocl/profile` bleibt projektunabhängig und read-only.
- Das Backendprofil ist die einzige maschinenlesbare Statuswahrheit.
- Das Produkt bezeichnet sich als OCL-2.4-basiertes Subset.
- Profileinträge können in der UI verständlich umformuliert werden, ohne ihren
  fachlichen Inhalt zu verändern.

**Offen**

- Ob das Backend `Available` und `Limit` später strukturiert statt gemeinsam in
  `notes` liefert.
- Ob Featurediagnosen direkt eine `featureId` transportieren.
- Welche technische Dokumentationsseite `Open related documentation` öffnet.
- Wie lange ein Profil offline zwischengespeichert werden darf.
- Ob Reference-Test-Bestände in einem getrennten Expertenbereich sichtbar
  werden sollen.

## Akzeptanzkriterien

| ID | Kriterium | Ergebnis |
|---|---|---|
| `M11-AC-01` | Die UI nennt sich niemals vollständig OCL-2.4-konform. | erfüllt |
| `M11-AC-02` | Alle vier Profilstatus sind textlich und semantisch unterscheidbar. | erfüllt |
| `M11-AC-03` | `PARTIAL` nennt verfügbaren Umfang und Grenze und blockiert nicht pauschal. | erfüllt |
| `M11-AC-04` | Nicht unterstützte Syntax erhält eine lokale Diagnostic und einen Featurebezug. | erfüllt |
| `M11-AC-05` | Profil-ID, OCL-/API-Version und Runtime-Limits sind erreichbar. | erfüllt |
| `M11-AC-06` | Loading, Offline/API-Fehler, ungültige/Empty-Antwort und Retry sind dargestellt. | erfüllt |
| `M11-AC-07` | Der OCL Editor bleibt bei nicht verfügbarem Profil benutzbar. | erfüllt |
| `M11-AC-08` | Detaildialog und Responsive Drawer sind intern scrollbar und fokussierbar. | erfüllt |
| `M11-AC-09` | Das Backendprofil bleibt die einzige Statuswahrheit; es gibt keinen Frontend-Fallback. | erfüllt |
| `M11-AC-10` | Produktiver Code und spätere Mockup-Schritte wurden nicht vorgezogen. | erfüllt |

## Abgrenzung

M11 gestaltet ausschließlich die OCL-Compliance- und Featureanzeige. Es
implementiert keine OCL-Funktion und verändert keine bestehende
Evaluationssemantik. State Machines, `oclInState`, Operation Traces und die
`OclMessage`-Runtime sind nach B21 im aktuellen Profil ausgeschlossen; M12 und
M13 sind deshalb `NOT_REQUIRED`. Die übergreifende responsive und
barrierebezogene Abnahme bleibt M14. Der Gesamtstatus ist weiterhin `MOCKUP`;
`BACKEND_READY` wird nicht gesetzt.
