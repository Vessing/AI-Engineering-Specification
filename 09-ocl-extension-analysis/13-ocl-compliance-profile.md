# OCL Compliance Profile

## Zweck dieser Datei

Diese Datei beschreibt den in OCL-Roadmap-Schritt 39 stabilisierten und
maschinenlesbar veröffentlichten OCL-Umfang des Backends. Normative Grundlage
ist OMG OCL 2.4. Das originale USE-Projekt bleibt ausschließlich
Kompatibilitäts- und Testreferenz.

Das Backend behauptet keine vollständige Syntax-, Evaluation- oder
XMI-Compliance. Die korrekte Produktbezeichnung lautet:

> OCL-2.4-basiertes Subset (`use-web-ocl-2.4-subset-v3`)

## Maschinenlesbarer Vertrag

```http
GET /api/v1/ocl/profile
```

Der Endpunkt ist projektunabhängig. Er liefert die Engine-Fähigkeiten, nicht den
Zustand eines konkreten UML-Modells. `apiVersion` ist `v1`; Änderungen, die
bisherige Clients semantisch brechen, benötigen eine neue Profil- oder
API-Version.

```json
{
  "profileId": "use-web-ocl-2.4-subset-v3",
  "oclVersion": "2.4",
  "complianceClaim": "OCL 2.4-based subset; no full syntax, evaluation, or XMI compliance claim",
  "apiVersion": "v1",
  "enabledOptionalCompliancePoints": ["allInstances", "pre-values", "oclIsNew"],
  "features": [],
  "runtimeLimits": {
    "maxIteratorBindings": 100000,
    "maxTokens": 10000,
    "maxSourceCharacters": 100000,
    "maxDiagnostics": 32,
    "maxAstDepth": 256,
    "maxEvaluationMillis": 2000,
    "maxDefinitionRecursion": 64,
    "maxResultElements": 1000000
  }
}
```

Das Frontend darf diese Antwort später zur Featureanzeige verwenden. Es darf
aus einem `PARTIAL`-Eintrag keine vollständige Unterstützung ableiten.

## Statusmodell

| Status | Bedeutung |
|---|---|
| `SUPPORTED` | Der dokumentierte Teilumfang läuft durch Parser, Typprüfung und Evaluator beziehungsweise den genannten Backenddienst und besitzt normale Tests. |
| `PARTIAL` | Eine fachlich nutzbare Grundlage existiert, aber der OCL-2.4-Kontext oder die Produktintegration ist nicht vollständig. |
| `NOT_SUPPORTED` | Das Feature gehört zum betrachteten OCL-Profil, ist aber nicht implementiert. |
| `OUT_OF_SCOPE` | Das Feature ist bewusst kein Ziel dieses REST-/JSON-Systems. |

## Compliance-Matrix

| ID | Featuregruppe | Status | Abgrenzung |
|---|---|---|---|
| `OCL-PROFILE-001` | Core Expressions | `PARTIAL` | Kernsyntax ist verfügbar; offene Literal-, Namens- und Spezialwertfälle bleiben in der Detailmatrix sichtbar. |
| `OCL-PROFILE-002` | Diagnostics | `SUPPORTED` | Lexer-, Parser-, Typ- und Evaluationsdiagnosen mit Source Ranges |
| `OCL-PROFILE-003` | Collection Types | `SUPPORTED` | `Set`, `Bag`, `Sequence`, `OrderedSet` und Literale |
| `OCL-PROFILE-004` | Collection Operations | `SUPPORTED` | Im Operationsregister implementierte Queries und Transformationen |
| `OCL-PROFILE-005` | Iterator Expressions | `SUPPORTED` | `forAll`, `exists`, `select`, `reject`, `collect`, `any`, `one`, `isUnique`, `sortedBy`, `closure`, `iterate` |
| `OCL-PROFILE-006` | Control and Binding | `SUPPORTED` | `if-then-else` und `let` mit lexikalischem Scope |
| `OCL-PROFILE-007` | Model Navigation | `PARTIAL` | Attribute, navigierbare Association Ends und expliziter Attribute-/Operations-Dispatch bei Redefinition sind verifiziert; optionale nicht navigierbare Zugriffe bleiben ausgeschlossen. |
| `OCL-PROFILE-008` | Extended Types | `SUPPORTED` | Tuple, Enum, `UnlimitedNatural`, Vererbung und Runtime-Typoperationen |
| `OCL-PROFILE-009` | `allInstances()` | `PARTIAL` | Optionaler Compliance Point ist für Modellklassen und aktuellen Snapshot aktiv, aber nicht vollständig beansprucht. |
| `OCL-PROFILE-010` | Operation Contracts | `SUPPORTED` | B18 persistiert Pre/Post und integriert `result`, `@pre`, `oclIsNew`, Contract-Gates sowie atomaren Commit/Rollback. |
| `OCL-PROFILE-011` | Derived/Init/Body/Def | `PARTIAL` | B19 integriert Derived, Init und Query-Body in den Lifecycle; eigenständige persistierte und Package-weite `def`-Einträge bleiben offen. |
| `OCL-PROFILE-012` | `OclMessage` | `NOT_SUPPORTED` | Message Expressions und Message Result Access fehlen |
| `OCL-PROFILE-013` | Nicht navigierbare Associations | `NOT_SUPPORTED` | Nur explizit navigierbare Rollen werden aufgelöst |
| `OCL-PROFILE-014` | XMI Interchange | `OUT_OF_SCOPE` | Austausch erfolgt über den versionierten REST-/JSON-Vertrag |
| `OCL-PROFILE-015` | Sichtbarkeits-Bypass für nicht öffentliche Features | `NOT_SUPPORTED` | UML-Sichtbarkeit wird kontextabhängig erzwungen und kann nicht umgangen werden. |
| `OCL-PROFILE-016` | State Machines und `oclInState` | `OUT_OF_SCOPE` | State-Machine-Modelle, aktive Zustände und `oclInState` gehören nicht zum aktuellen Produktzielprofil. |

Die Java-Quellen `OclComplianceProfile.current()` und
`OclOptionalCompliancePolicy` sind die maschinenlesbare
Wahrheit dieser Matrix. Der Konsistenztest erzwingt eindeutige IDs, vollständige
Beschreibungen, das Vorhandensein aller vier Statusklassen sowie die B20- und
B21-Entscheidungen. `OCL-PROFILE-012` hält Message Expressions weiterhin als
`NOT_SUPPORTED` fest; `OCL-PROFILE-016` dokumentiert die übergeordnete
State-Machine-Entscheidung als `OUT_OF_SCOPE`.

## Performance und Sicherheit

Iteratoren, `closure` und `allInstances()` verwenden gemeinsam die profilierte
Obergrenze `maxIteratorBindings = 100000`. B17 ergänzt Token-, AST-, Zeit- und
Definitionsrekursionsgrenzen. B22 ergänzt Quelltext-, Diagnostic- und
Ergebnismengengrenzen. Überschreitungen erzeugen die strukturierten Codes
`TOKEN_LIMIT_EXCEEDED`, `AST_DEPTH_LIMIT_EXCEEDED`,
`EVALUATION_DEPTH_LIMIT_EXCEEDED`, `EVALUATION_TIME_LIMIT_EXCEEDED`,
`ITERATION_LIMIT_EXCEEDED`, `DEFINITION_RECURSION_LIMIT`,
`SOURCE_LIMIT_EXCEEDED`, `DIAGNOSTIC_LIMIT_EXCEEDED` oder
`RESULT_LIMIT_EXCEEDED`, statt
unkontrolliert weiter auszuwerten.

## Fehler- und Ergebnisvertrag

Die bestehenden OCL-Endpunkte bleiben unter
`/api/v1/projects/{projectId}/ocl`. Parse-, Typecheck- und Evaluate-Antworten
transportieren strukturierte Diagnostics. Der Profilendpunkt ergänzt diesen
Vertrag nur lesend und verändert keine Evaluationssemantik.

## Reproduzierbare Test- und Reference-Reports

```powershell
mvn test
mvn -Preference-tests test
```

Der erste Befehl ist die blockierende normale Suite. Der zweite Befehl führt die
getrennte, nicht blockierende Original-USE-Reference-Suite aus und erzeugt:

- `target/reference-reports/original-use-reference-report.json`
- `target/reference-reports/original-use-reference-report.md`

Abnahme nach B25 vom 22. August 2026:

| Reference-Status | Anzahl |
|---|---:|
| `PASSING` | 823 |
| `FAILING_GAP` | 382 |
| `FAILING_FORMAT` | 0 |
| `FAILING_INFRASTRUCTURE` | 0 |
| `NON_OCL_OR_SHELL_ONLY` | 213 |
| `UNCLEAR` | 0 |
| Gesamt | 1418 |

Null-Gap-Abnahme nach B33 vom 24. August 2026:

| Reference-Status | Anzahl |
|---|---:|
| `PASSING` | 942 |
| `FAILING_GAP` | 0 |
| `FAILING_FORMAT` | 0 |
| `FAILING_INFRASTRUCTURE` | 0 |
| `NON_OCL_OR_SHELL_ONLY` | 476 |
| `UNCLEAR` | 0 |
| Gesamt | 1418 |

Profil v3 versioniert diese neue Faehigkeits- und Abnahmeaussage. API-Version,
DTO-Struktur und die weiterhin ehrlichen `PARTIAL`-, `NOT_SUPPORTED`- und
`OUT_OF_SCOPE`-Grenzen bleiben unveraendert.

Bekannte Reference-Fehlschläge blockieren die normale CI nicht. Ein in die
normale Regression promovierter Fall blockiert dagegen über `mvn test`.

## Provenienz

Die unveränderten Testressourcen liegen unter
`src/test/resources/reference/original-use/`. Inventar und SHA-256-Prüfsummen
belegen ihre Herkunft. Produktiver USE-Code, USE-Core und die alte
Testinfrastruktur sind keine Dependencies des neuen Backends.

## Verbleibende Grenzen

- Das Profil ist ein überprüftes Subset und keine vollständige OCL-2.4-Erfüllung.
- `FAILING_GAP` bleibt ein sichtbarer Arbeitsbestand und wird nicht als
  verschwiegene Unterstützung behandelt.
- Mutation Testing, Fuzzing, verteilte Lasttests und HTTP-weites Rate-Limiting
  sind nicht Bestandteil der erreichten Baseline.
- Frontend-Featureanzeige auf Basis des neuen Endpunkts ist eine getrennte
  Frontend-Aufgabe.

## Zusammenfassung

Schritt 39 schließt die Roadmap mit einem versionierten, testbaren und ehrlichen
OCL-Profil ab. Die Matrix trennt unterstützte, teilweise unterstützte, fehlende
und bewusst ausgeschlossene Bereiche. Normale Regression und Reference-Suite
bleiben getrennt und reproduzierbar ausführbar.
