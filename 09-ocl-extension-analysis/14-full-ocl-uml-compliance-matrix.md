# Full OCL and UML Compliance Matrix

## Zweck

Diese Matrix beschreibt den Weg vom aktuellen Profil
`use-web-ocl-2.4-subset-v1` zu einem belastbar nachgewiesenen OCL-2.4-Profil.
Normative Quelle ist `OCL-specification.pdf` im Workspace-Root. Das originale
USE-Projekt und seine Tests sind ergänzende Kompatibilitätsreferenzen, aber keine
normative Definition von OCL.

Vollständige OCL-Unterstützung bedeutet nicht nur, dass ein Ausdruck geparst
wird. Für jedes Feature müssen Concrete Syntax, AST, Well-formedness,
Typkonformität, Evaluation einschließlich `null` und `invalid`, UML-Kontext,
Diagnostics und Tests nachgewiesen sein.

## Compliance-Arten nach OCL 2.4

| Bereich | Ziel | Aktueller Stand | Entscheidung |
|---|---|---|---|
| Syntax Compliance | OCL Concrete Syntax und Well-formedness lesen und prüfen | Teilmenge | vollständig zu inventarisieren und schrittweise zu schließen |
| Evaluation Compliance | unterstützte Ausdrücke normgerecht auswerten | Teilmenge | vollständige Kernsemantik plus explizite optionale Punkte |
| XMI Compliance | OCL-Metamodell über XMI austauschen | nicht vorhanden | separates Vorhaben; aktuell `OUT_OF_SCOPE` |
| Optional: `allInstances()` | Instanzen eines Classifiers auswerten | vorhanden | Subtypen und Snapshotgrenzen vollständig verifizieren |
| Optional: Pre-Values und `oclIsNew()` | Vorzustand und neue Objekte in Postconditions | teilweise vorhanden | Operationsausführung und Snapshotpaar vervollständigen |
| Optional: `OclMessage` | Nachrichten- und Call-Trace-Ausdrücke | nicht vorhanden | nach vollständigem Operation Runtime Model implementieren |
| Optional: nicht navigierbare Associations | Navigation unabhängig vom Navigierbarkeitsflag | bewusst nicht aktiviert | B20 veröffentlicht `OCL-PROFILE-013 = NOT_SUPPORTED` und erzwingt das Flag in Typechecker und Evaluator. |
| Optional: private/protected Features | Umgehung der UML-Sichtbarkeit | bewusst nicht aktiviert | B20 veröffentlicht `OCL-PROFILE-015 = NOT_SUPPORTED`; kontextabhängige UML-Sichtbarkeit bleibt aktiv. |

## Statusmodell dieser Matrix

| Status | Bedeutung |
|---|---|
| `VERIFIED` | Parser, Typprüfung, Evaluation und relevante Integration sind durch normale Tests belegt. |
| `PARTIAL` | Wesentliche Implementierung existiert, aber Standardfälle oder Integration fehlen. |
| `REVIEW_REQUIRED` | Code existiert, vollständige OCL-2.4-Konformität ist jedoch nicht nachgewiesen. |
| `NOT_IMPLEMENTED` | Sprach- oder Bibliotheksfeature fehlt. |
| `UML_BLOCKED` | Korrekte OCL-Semantik benötigt zuerst eine fehlende UML-Fähigkeit. |
| `OUT_OF_SCOPE` | Bewusst getrenntes Compliance-Vorhaben. |

`SUPPORTED` aus dem öffentlichen Profil darf erst dann zu einer vollständigen
Compliance-Aussage werden, wenn die zugehörigen Detailzeilen hier `VERIFIED`
sind.

## Normatives Detailinventar

Backend-Schritt B1 hat die hier verdichteten Bereiche in
`18-ocl-24-normative-inventory.md` und der maschinenlesbaren Backend-Ressource
`src/test/resources/compliance/ocl-2.4-normative-inventory.csv` stabil
adressiert. `CM-OCL-*`, `CM-LIB-*` und `CM-CTX-*` bleiben Parent-IDs fuer
Planung und Reporting. Die `OCL24-*`-IDs adressieren einzelne Grammatik-,
Well-formedness-, Semantik-, Kontext- und Bibliothekssignatur-Anforderungen.
Der Status `INVENTORIED` ist dabei kein Implementierungs- oder
Compliance-Nachweis.

Backend-Schritt B3 hat mit
`src/test/resources/compliance/ocl-2.4-normative-cases.tsv` die erste
ausfuehrbare normative Nachweisebene ergaenzt. Jeder Fall verweist auf genau
eine `OCL24-*`-Inventar-ID und eine Parent-Matrix-ID. Der Harness ist nach
Matrix-ID filterbar. Die ersten gruenen Faelle aendern die Statuswerte dieser
Matrix noch nicht, weil sie nur einzelne Anforderungen und noch keine
vollstaendige Matrixzeile verifizieren.

## OCL-Sprach- und Semantikmatrix

| ID | Featuregruppe | Aktueller Status | Was noch nachzuweisen oder umzusetzen ist | UML-Abhängigkeit | Priorität |
|---|---|---|---|---|---|
| `CM-OCL-001` | Namen, qualifizierte Namen, Kommentare und Whitespace | `PARTIAL` | qualifizierte Classifier und Namespace-Auflösung sind umgesetzt; vollständige Escapes, Unicode- und Keyword-Regeln bleiben offen | Namespaces | P1 |
| `CM-OCL-002` | Source Locations und Fehlererholung | `VERIFIED` | B22 belegt begrenzte Mehrfachdiagnostik, strukturierte Null-/Größenfehler, Parser-Wiederverwendung, parallele Isolation sowie grammar-basierte und deterministische Fuzz-Eingaben ohne unkontrollierte Exceptions. B28 ergänzt vollständige Source Ranges für Exponentialliterale und mehrere `iterate`-Deklarationen; normative Reference-Fälle verbleiben nicht in der Parser-Gap-Kategorie. | keine | P0 |
| `CM-OCL-003` | `self`, Variablen und lexikalischer Scope | `VERIFIED` | Regression bei verschachteltem Shadowing fortführen | Kontextclassifier | P1 |
| `CM-OCL-004` | `let` | `VERIFIED` | vollständige Common-Type- und Invalid-Regeln prüfen | Typsystem | P1 |
| `CM-OCL-005` | `if-then-else` | `VERIFIED` | Hierarchiebewusster Branch-LUB und Lazy Evaluation sind getestet; Regression über weitere Wertarten fortführen | Generalisierung | P1 |
| `CM-OCL-006` | Boolean-Operatoren und Präzedenz | `VERIFIED` | B10 zentralisiert und testet die vollständigen Vierwerttabellen für `and`, `or`, `xor`, `implies` und `not`; Präzedenztests bleiben grün. | keine | P0 |
| `CM-OCL-007` | Gleichheit und Ungleichheit | `VERIFIED` | B10 deckt Werte, stabile Objektidentität, Tuple, konkrete Collections, `null` und `invalid` ab; subtype-spezifische Collection-Details werden zusätzlich in B15 regressiert. | Objektidentität | P0 |
| `CM-OCL-008` | Numerische Operatoren | `VERIFIED` | B11 verifiziert Promotion, Division, Modulo, Rundung, Division durch null und deterministisches `invalid` bei Überschreitung des begrenzten Integer-Laufzeitbereichs. | Primitive Types | P1 |
| `CM-OCL-009` | String-Literale und String-Operationen | `VERIFIED` | B11 verifiziert Escapes, benachbarte Literalfragmente, Unicode-Codepoints, einsbasierte Indexregeln und die registrierten OCL-2.4-String-Signaturen. | keine | P1 |
| `CM-OCL-010` | `null`/`OclVoid` und `invalid`/`OclInvalid` | `PARTIAL` | B10 trennt beide Spezialwerte und zentralisiert Boolean- und Gleichheitspropagation; B31 beseitigt Laufzeit-Auflösungsdiagnosen bei normativer Invalid-Propagation für typisierte undefinierte Empfänger. | alle Wertarten | P0 |
| `CM-OCL-011` | Property- und Operation-Calls | `VERIFIED` | B12 verwendet einen gemeinsamen Resolver für Standard Library, UML-Features und `def`; Overload-Spezifität, Sichtbarkeit, stabile IDs, Mehrdeutigkeit und implizites `self` sind getestet. | Operationen, Vererbung | P0 |
| `CM-OCL-012` | Attributnavigation und Chains | `VERIFIED` | B12 zentralisiert die Feature-Auflösung; B30 belegt implizites Collect für Properties, parameterlose Operationen und Type-Argument-Operationen mit getrennter Punkt-/Pfeilsemantik; B31 ergänzt objektwertige Slots mit stabiler Objektidentität und Classifierprüfung. | Vererbung | P0 |
| `CM-OCL-013` | Association-End-Navigation | `VERIFIED` | B7/B8 implementieren binäre, n-äre, qualifizierte und Association-Class-Navigation; B20 zentralisiert und testet die bewusste Ablehnung nicht navigierbarer Ends. | Associations | P0 |
| `CM-OCL-014` | Typoperationen | `VERIFIED` | B12 vereinheitlicht die Aufrufauflösung; B13 ergänzt `oclType()` als strukturierten Classifier-Wert mit stabiler ID und qualifiziertem Namen. | Generalisierung | P1 |
| `CM-OCL-015` | `allInstances()` | `PARTIAL` | abstrakte Klassen, konkrete Subtypen, Snapshotauswertung und profilierte Binding-/Ergebnisbudgets sind getestet; ein eigener Snapshotindex bleibt offen | Generalisierung, Snapshots | P1 |
| `CM-OCL-016` | Tuple-Literale und Tuple-Zugriff | `VERIFIED` | B13 belegt strukturelle Typgleichheit, Breitenkonformität, Common Type, Wertgleichheit und Part-Zugriff. | Typsystem | P2 |
| `CM-OCL-017` | Enum-Literale | `VERIFIED` | B13 löst kurze, qualifizierte und importierte Namen kontextbezogen auf, erkennt Mehrdeutigkeit und erhält Enum-Identität in Typ und Wert. | Enumerations | P1 |
| `CM-OCL-018` | Collection-Literale und Ranges | `VERIFIED` | B14 prüft leere Literale, Integer-Ranges, Common Types, Duplikate sowie `null`/`invalid`; B22 weist Ranges oberhalb des Ergebnisbudgets vor der Materialisierung strukturiert ab. | Typsystem | P0 |
| `CM-OCL-019` | Iterator-Scope und mehrere Iteratorvariablen | `VERIFIED` | B16: Kind-Scope, Shadowing, Duplikatdiagnose und kartesisches Produkt getestet | keine | P0 |
| `CM-OCL-020` | `forAll`, `exists`, `one`, `any` | `VERIFIED` | B16 belegt Empty- und Vierwertfälle; B30 ergänzt kartesische Mehrfachbindungen für `one`. | Collection-Semantik | P0 |
| `CM-OCL-021` | `select`, `reject`, `collect`, `collectNested` | `VERIFIED` | B16: konkrete Ergebnisart, rekursives Flattening und Invalid-Verhalten getestet | ordered/unique | P0 |
| `CM-OCL-022` | `isUnique`, `sortedBy`, `closure` | `VERIFIED` | B17 belegt Laufzeitsemantik und Budgets; B30 ergänzt die implizite Iterator-Kurzform und Tuple-Part-Scope für `isUnique`; B31 verifiziert die implizite `sortedBy`-Variable bei erhaltener Collection-Art. | Generalisierung, Ordnung | P1 |
| `CM-OCL-023` | `iterate` | `VERIFIED` | B17 belegt Akkumulator-/Scope-Semantik, Empty-/Invalid-Fälle, große Collections und profilierte Budgets; B31 ergänzt mehrere Iteratorvariablen als budgetbegrenztes kartesisches Produkt. | Collection-Semantik | P1 |
| `CM-OCL-024` | Collection-Kurzformen und implizite Iteratoren | `VERIFIED` | B16 legt den Dispatch an; B30 bewahrt den Navigationsoperator auch für Type-Argument-Calls und verifiziert implizites Collect sowie implizite Iterator-Bindungen. | Navigation | P1 |
| `CM-OCL-025` | `@pre`, `result`, `oclIsNew()` | `VERIFIED` | B18 bindet die Sprachelemente ausschließlich im typisierten Postkontext an Before State, Result-Slot und Lifecycle-Diff. | Operation Runtime | P0 |
| `CM-OCL-026` | Message Expressions `^`, `^^`, `OclMessage` | `OUT_OF_SCOPE` | B21 schließt Operation Traces und Message Expressions aus dem aktuellen Zielprofil aus; Parserdiagnosen verhindern eine versehentliche Compliance-Behauptung. | Operation Runtime | P3 |
| `CM-OCL-027` | `oclInState` und zustandsbezogene Ausdrücke | `OUT_OF_SCOPE` | B21 schließt State Machines und aktive Zustände aus dem aktuellen Zielprofil aus; der Typechecker meldet `UNKNOWN_FEATURE`. | State Machines | P3 |

## OCL-Typ- und Standardbibliotheksmatrix

| ID | Typ oder Bibliotheksbereich | Status | Fehlender Nachweis oder Umfang | Priorität |
|---|---|---|---|---|
| `CM-LIB-001` | `OclAny` | `PARTIAL` | B10 verifiziert allgemeine Gleichheit; B29 verifiziert die gemeinsame Resolver-Nutzung und grenzt die nicht normativen USE-Aliase `isUndefined` und `isDefined` aus. | P0 |
| `CM-LIB-002` | `OclInvalid` | `PARTIAL` | B10 verifiziert `oclIsInvalid()` sowie Boolean- und Gleichheitspropagation; operationenspezifische Nachweise folgen mit den jeweiligen Bibliotheksschritten. | P0 |
| `CM-LIB-003` | `OclVoid` | `VERIFIED` | Bottom-Type-Konformität, expliziter Laufzeitwert und `oclIsUndefined()` sind vorhanden; B29 ergänzt `OclVoid` und `OclInvalid` in der gemeinsamen Type-Argument-Auflösung. | P0 |
| `CM-LIB-004` | `Boolean` | `VERIFIED` | B10 deckt jede Zelle der Vierwerttabellen ab; B29 ergänzt und testet die gemeinsame Signatur und Auswertung von `Boolean::toString()`. | P0 |
| `CM-LIB-005` | `Integer`, `Real`, `UnlimitedNatural` | `PARTIAL` | B11 registriert und testet die vorgesehenen numerischen Signaturen, Promotion und Spezialfälle. Für vollständige normative Compliance fehlt weiterhin eine unbeschränkte Integer-Repräsentation anstelle des dokumentierten 32-Bit-Laufzeitprofils. | P1 |
| `CM-LIB-006` | `String` | `VERIFIED` | B11 deckt `size`, `concat`, `substring`, Case-Konvertierung, `indexOf`, `at`, `characters`, Konvertierungen, Unicode- und Indexregeln ab. | P1 |
| `CM-LIB-007` | `Tuple` | `VERIFIED` | B13 implementiert strukturelle Typregeln, Common Type, Zugriff und semantische Gleichheit; allgemeine OclAny-Operationen laufen über die gemeinsame Library. | P2 |
| `CM-LIB-008` | `Collection(T)` | `VERIFIED` | B14 implementiert die abstrakten Queries; B22 begrenzt Ergebnismengen und weist übergroße kartesische Produkte vor ihrer Materialisierung strukturiert ab. | P0 |
| `CM-LIB-009` | `Set(T)` | `VERIFIED` | B15 prüft Set-Differenz, `symmetricDifference`, `union`, `intersection`, `including` und `excluding` mit eindeutigen Ergebnissen. | P0 |
| `CM-LIB-010` | `Bag(T)` | `VERIFIED` | B15 prüft Häufigkeiten, Bag-/Set-Overloads, Gleichheit sowie erhaltende und reduzierende Operationen. | P0 |
| `CM-LIB-011` | `Sequence(T)` | `VERIFIED` | B15 implementiert und prüft Konkatenation, Insert, Slice, einbasierte Indexzugriffe, Reihenfolge und Umkehrung. | P1 |
| `CM-LIB-012` | `OrderedSet(T)` | `VERIFIED` | B15 implementiert und prüft geordnete Zugriffe und Updates unter gleichzeitiger Wahrung der Eindeutigkeit. | P1 |
| `CM-LIB-013` | Collection-Konvertierungen | `VERIFIED` | B15 prüft `asSet`, `asBag`, `asSequence` und `asOrderedSet` einschließlich Häufigkeits- und Ordnungsfolgen. | P1 |
| `CM-LIB-014` | Collection-Flattening und Kombination | `VERIFIED` | B15 prüft rekursives `flatten`, alle definierten `union`-/`intersection`-Overloads und `symmetricDifference` mit konkreten Ergebnisarten. | P0 |
| `CM-LIB-015` | `OclType`/Classifier-Werte | `VERIFIED` | B13 liefert Runtime-Repräsentation und strukturiertes API-Mapping mit stabiler Classifier-ID, qualifiziertem Namen und repräsentiertem Typ. | P2 |
| `CM-LIB-016` | `OclMessage` | `OUT_OF_SCOPE` | B21: kein Operation-Trace-Modell im aktuellen Zielprofil | P3 |
| `CM-LIB-017` | `OclState` | `OUT_OF_SCOPE` | B21: keine State-Machine-Semantik im aktuellen Zielprofil | P3 |

## XMI-Compliance-Matrix

| ID | Bereich | Status | Entscheidung | Priorität |
|---|---|---|---|---|
| `CM-XMI-001` | OCL-Metamodell-Interchange über XMI | `OUT_OF_SCOPE` | Profil v3 verwendet den versionierten REST-/JSON-Projektvertrag und beansprucht keine XMI-Compliance. | P3 |

## OCL-Kontextmatrix

| ID | Kontext | Status | Fehlender Umfang | UML-Abhängigkeit |
|---|---|---|---|---|
| `CM-CTX-001` | Classifier Invariant | `VERIFIED` | Last-, Reihenfolge- und große Ergebnismengentests ausbauen | Classifier, Snapshot |
| `CM-CTX-002` | Operation Precondition | `VERIFIED` | B18 prüft aktive Boolean-Contracts mit Receiver und Parametern vor jeder Ausführung und blockiert atomar. | Operation Runtime |
| `CM-CTX-003` | Operation Postcondition | `VERIFIED` | B18 prüft aktive Boolean-Contracts auf dem Candidate State vor Commit und rollt Verletzungen atomar zurück. | Operation Runtime |
| `CM-CTX-004` | Operation Body | `VERIFIED` | B19 führt typgeprüfte, parametrisierte OCL-Query-Bodies als Operationsfallback aus. | Operations |
| `CM-CTX-005` | Derived Property | `VERIFIED` | B19 ergänzt Rekursionsdiagnosen, dynamischen Abhängigkeitsgraph und snapshotgebundene Memoisierung. | Properties, Associations |
| `CM-CTX-006` | Initial Value | `VERIFIED` | B19 testet Init-Auswertung vor dem atomaren Objekt-Commit einschließlich Fehlerrollback. | Object Lifecycle |
| `CM-CTX-007` | Property/Operation `def` | `VERIFIED` | B40 ergänzt stabile persistierte Class-/Package-Owner, Source Ranges, Class-Dispatch, Package-`self`-Regel, Signatur-/Zyklusprüfung und CRUD-/Blockerverträge. | Namespaces, Vererbung |
| `CM-CTX-008` | Package-/Namespace-Kontext | `VERIFIED` | Packagekontext, Imports, Aliasauflösung und Sichtbarkeit sind durch Typechecker- und Persistenztests belegt | Packages/Imports |

## Erforderliche UML-Compliance-Matrix

| ID | UML-Fähigkeit | Aktueller Status | Warum OCL sie benötigt | Backendarbeiten | Frontend-/DTO-Auswirkung | Priorität |
|---|---|---|---|---|---|---|
| `CM-UML-001` | Stabile Classifier-Identität | `VERIFIED` | Typauflösung und Objektidentität | Regression erhalten | keine | P0 |
| `CM-UML-002` | Generalisierung und abstrakte Klassen | `VERIFIED` | Subtyping, hierarchischer LUB und `allInstances` | Zyklen, Mehrfachvererbung, abstrakte Instanziierung und atomare Modellupdates sind getestet | Generalization Edges | P0 |
| `CM-UML-003` | Mehrfachvererbung und Redefinition | `VERIFIED` | eindeutige Property-/Operationsauflösung | B38 modelliert stabile explizite Redefinitionsziele, prüft Owner und Signaturkonformität, löst Mehrfachvererbungskonflikte auf und verwendet das lokale Feature im OCL-Dispatch. | Domain-, Mapper-, Command-, Read-Model- und Resolver-Tests | P0 |
| `CM-UML-004` | Sichtbarkeit | `VERIFIED` | Zugriff auf Classifier, Attribute und Operationen | `public`, `private`, `protected` und `package` werden persistiert und im Typechecker kontextabhängig geprüft | Properties-Felder | P1 |
| `CM-UML-005` | Enumerations | `VERIFIED` | Enum-Literale und Typen | B13 persistiert Namespaces und unterstützt eindeutige kurze, importierte und qualifizierte Namen. | Enum-Editor | P1 |
| `CM-UML-006` | DataTypes und Value Types | `IMPLEMENTED` | benutzerdefinierte Werttypen | B13 modelliert Value Properties und OCL-Zugriff; B50 vereinheitlicht persistierte rekursive DataType-/Tuple-/Collection-Werte; B51 ergaenzt den revisionsgeschuetzten Property-Impact/-Delete, rekursive Wertblocker, OCL Source Ranges und das Gate fuer vollstaendige DataType-Updates. | Type Picker | P2 |
| `CM-UML-007` | Binäre Associations | `VERIFIED` | Basisnavigation | Regression erhalten | vorhanden | P0 |
| `CM-UML-008` | `ordered` und `unique` Association Ends | `VERIFIED` | Ziel-Collection-Art | Persistenz, DTO-Vorschau, Typechecker und Evaluator sind für alle vier Collection-Arten getestet | End Properties | P0 |
| `CM-UML-009` | Qualifier | `IMPLEMENTED` | typisierte Qualifierdefinitionen, qualifizierte Linkwerte und Navigation `rolle[wert]` | B7-Domänenmodell, Service-, Validation- und OCL-Tests | Association Editor | P1 |
| `CM-UML-010` | n-äre Associations | `IMPLEMENTED` | end-basierte Linkinstanzen, Navigation und bindungsbezogene Multiplizität für mindestens zwei Ends | B7-Domänenmodell und Validation | n-ärer Editor | P1 |
| `CM-UML-011` | Association Classes | `IMPLEMENTED` | Linkobjekte mit Properties | B8 erzwingt gemeinsame Link-/Objektidentität und ermöglicht OCL-Navigation zum Linkobjekt | Class-/Object-Diagramm | P1 |
| `CM-UML-012` | Reflexive Associations | `VERIFIED` | eindeutige Rollen und Navigation | gleiche Rollen werden strukturiert abgelehnt; Navigation verwendet stabile Source-/Target-End-IDs | Edge-End-Auswahl | P0 |
| `CM-UML-013` | Aggregation und Composition | `IMPLEMENTED` | Ownership- und Lebenszyklusregeln | B8 persistiert Aggregation Kind und erzwingt No-share, No-cycle sowie rekursiven Composition-Cascade | Aggregation Kind | P2 |
| `CM-UML-014` | `subsets`, `redefines`, derived und union Ends | `PARTIAL` | B6 persistiert und validiert End-Referenzen, Zyklen, Konformität und `union => derived`; die OCL-Ableitungsauswertung folgt in B18 | Association Properties | P1 |
| `CM-UML-015` | Operation Signatures und Dispatch | `IMPLEMENTED` | B9 persistiert vollständige Signaturen, erlaubt Overloads und löst Overrides anhand des Laufzeittyps deterministisch auf | Overloads, Override und Invocation | Operation Editor | P0 |
| `CM-UML-016` | Atomare Operation Invocation | `VERIFIED` | B9 und B18 implementieren Candidate State, Query-Schutz, Pre-/Post-Gates, Commit und vollständigen Rollback. | Invocation Service, Transaktion und Snapshotpaar | Invocation UI später | P0 |
| `CM-UML-017` | Objektlebenszyklus | `VERIFIED` | B9 ermittelt created/changed/deleted; B18/B19 sichern `oclIsNew()`, Init und atomare Commit-Grenzen. | create/delete im Operationskontext | Object UI | P0 |
| `CM-UML-018` | Packages, Namespaces und Imports | `VERIFIED` | qualifizierte Namen und modulare Modelle | B5 weist stabile Package-/Import-IDs, Aliasresolver, Zyklusdiagnosen und Provenienz nach; B45 ergaenzt revisionsgeschuetztes Rename/Move/Update/Delete, Read-only-Importprojektion, explizite Cascades und referenzbewussten Delete Impact | Import UI | P1 |
| `CM-UML-019` | State Machines | `OUT_OF_SCOPE` | `oclInState` und State-Kontexte sind durch die B21-Profilentscheidung ausgeschlossen. | bei späterer Aufnahme eigenes Domänenmodell | bei späterer Aufnahme neue UI erforderlich | P3 |

## Nachweisregeln pro Matrixzeile

Eine Zeile darf nur auf `VERIFIED` wechseln, wenn alle anwendbaren Nachweise
vorliegen:

1. normative OCL-2.4-Regel und Abgrenzung sind dokumentiert,
2. positive und negative Syntaxfälle existieren,
3. AST und Source Ranges sind geprüft,
4. Typechecker prüft Signatur, Scope, Konformität und Ergebnisart,
5. Evaluator prüft definierte Werte, `null`, `invalid` und Grenzfälle,
6. UML-Fixtures bilden die nötige Modellsemantik ab,
7. Validation und API liefern strukturierte Ergebnisse statt Exceptions,
8. normale Regression ist grün,
9. betroffene Original-USE-Referenzfälle wurden neu klassifiziert,
10. die öffentliche Profilmatrix wurde aktualisiert.

## Aktuelle Messbasis

Seit B2 enthaelt jeder der 824 nicht erfolgreichen Reference-Cases genau eine
primaere Matrix-ID, einen Backend-Zielschritt und eine Blockerklasse. Die
vollstaendige Aggregation und die Zuordnungsregeln stehen in
`19-b2-reference-case-feature-mapping.md`. Diese primaere Zuordnung ersetzt
nicht die zusaetzlichen Feature-Tags und Gap-IDs eines Falls.

| Status | Anzahl |
|---|---:|
| `PASSING` | 823 |
| `FAILING_GAP` | 382 |
| `FAILING_FORMAT` | 0 |
| `FAILING_INFRASTRUCTURE` | 0 |
| `NON_OCL_OR_SHELL_ONLY` | 213 |
| `UNCLEAR` | 0 |

Diese Zahlen sind ein Fortschrittsindikator, aber kein OCL-Compliance-Beweis.
Mehrere Fälle können dasselbe Feature abdecken, und USE-spezifische Fälle dürfen
nicht als normative OCL-Anforderungen behandelt werden.

Beim Abbau von `FAILING_FORMAT` und `FAILING_INFRASTRUCTURE` gilt die
Entkopplungsregel aus B23: Gemessen wird die fachliche OCL-/UML-Aussage in
eigenen strukturierten Assertions und eigenen minimalen Fixtures. Weder das
alte USE-Textformat noch USE-Core, USE-Shell oder die alte Testinfrastruktur
sind Teil des Compliance-Ziels. Nicht fachlich relevante Faelle werden als
`NON_OCL_OR_SHELL_ONLY` ausgewiesen.

B23 hat diese Entkopplung am 22. August 2026 umgesetzt. Die gestiegene Zahl
`FAILING_GAP` ist beabsichtigt: Sie macht 99 zuvor von Format oder Infrastruktur
verdeckte fachliche Abweichungen sichtbar. Sie ist kein Rueckschritt der
Produktsemantik und wird erst in B24 bearbeitet.

B24 hat die 543 Gap-Signale erneut gegen das eigenständige OCL-2.4-Profil
bewertet. 24 Fälle sind nun `PASSING`; 132 USE-Dialekt- oder alte
Undefined/Invalid-Kompatibilitätsfälle sind bewusst `NON_OCL_OR_SHELL_ONLY`.
Die Entscheidungen zu allen offenen P0-/P1-Zeilen und die verbleibenden 387
Gap-Signale sind in `41-b24-ocl-gap-closure-and-regression.md` dokumentiert.

B25 entscheidet die letzten 16 unklaren Fälle, veröffentlicht die vollständige
Profilabnahme als `use-web-ocl-2.4-subset-v2` und weist XMI explizit über
`CM-XMI-001` aus. Die ausführbare Zuordnung aller öffentlichen Profilzeilen
steht in `src/test/resources/compliance/ocl-2.4-profile-acceptance.csv`. Die
verbleibenden 382 Gap-Signale gehören ausschließlich zu nicht vollständig
beanspruchten Bereichen.

B31 schließt die verbleibende Evaluator-Ursache
`OCL_EVALUATION_NOT_SUPPORTED` vollständig. Der reproduzierte Reference-Stand
enthält 918 `PASSING`, 205 `FAILING_GAP`, 0 `FAILING_FORMAT`, 0
`FAILING_INFRASTRUCTURE`, 295 `NON_OCL_OR_SHELL_ONLY` und 0 `UNCLEAR`.
Verbleibende Ergebnis- und Diagnostic-Abweichungen werden erst in B32
bearbeitet; konkrete UML-Fixture-Gaps werden dadurch nicht umetikettiert.

B32 schliesst die Ursachen `STRUCTURED_RESULT_MISMATCH`,
`STRUCTURED_DIAGNOSTIC_MISMATCH` und
`EXPECTED_STRUCTURED_DIAGNOSTIC_NOT_OBSERVED` vollstaendig. Dabei wurden die
bereits verifizierten IDs `CM-OCL-004`, `CM-OCL-011`, `CM-OCL-014`,
`CM-OCL-022`, `CM-LIB-009`, `CM-LIB-010`, `CM-LIB-011`, `CM-LIB-012` und
`CM-LIB-014` erneut strukturell geprueft. Der Abschlussstand enthaelt 937
`PASSING`, 146 `FAILING_GAP`, 0 `FAILING_FORMAT`, 0
`FAILING_INFRASTRUCTURE`, 335 `NON_OCL_OR_SHELL_ONLY` und 0 `UNCLEAR`.

B33 schliesst fuenf normative Gaps fuer ausgelassene Iteratorparameter und
grenzt 140 alte USE-Modell-/Shell-Fixture-Faelle sowie eine abweichende
USE-Invalid-Regel als nicht zum Produktvertrag gehoerend ab. Die erneut
verifizierten Iteratorzeilen sind `CM-OCL-019`, `CM-OCL-020`, `CM-OCL-021`,
`CM-OCL-022` und `CM-OCL-024`. Profil `use-web-ocl-2.4-subset-v3` enthaelt 942 `PASSING`, 0
`FAILING_GAP`, 0 `FAILING_FORMAT`, 0 `FAILING_INFRASTRUCTURE`, 476
`NON_OCL_OR_SHELL_ONLY` und 0 `UNCLEAR`.

## Verbleibender Standardumfang nach B33

Die folgende Restumfang-Matrix beschreibt, was fuer einen ueber Profil v3
hinausgehenden OCL-/UML-Compliance-Anspruch backendseitig noch fehlt. Ein
Eintrag mit `OUT_OF_SCOPE` ist kein Fehler des aktuellen Profils, sondern
benoetigt zuerst eine neue Produktentscheidung.

| Matrix- und Profil-IDs | Status nach B33 | Fehlender Standardumfang | Erforderliche Backendarbeit | Abhaengigkeit oder Entscheidung |
|---|---|---|---|---|
| `CM-OCL-001`, `OCL-PROFILE-001` | `PARTIAL` | Die vollstaendigen OCL-Regeln fuer Namen, reservierte Woerter, Unicode und Escapes sind noch nicht nachgewiesen. | Lexer, Parser und Namensaufloesung muessen gegen ein vollstaendiges normatives Fallinventar erweitert und getestet werden. | Keine neue UI ist erforderlich, solange nur bestehende Texteingaben erweitert werden. |
| `CM-OCL-010`, `CM-LIB-001`, `CM-LIB-002` | `PARTIAL` | `OclAny`, `OclVoid` und `OclInvalid` sind fuer die beanspruchten Operationen umgesetzt, aber noch nicht fuer jede normative Bibliotheksoperation und jede Propagationskombination nachgewiesen. | Fuer jede Standardsignatur muessen Typregel, Laufzeitsemantik und Vierwert- beziehungsweise Invalid-Propagation inventarisiert und durch normative Tests belegt werden. | Die Umsetzung darf keine USE-spezifische `undefined`-Semantik einfuehren. |
| `CM-LIB-005` | `PARTIAL` | OCL `Integer` ist mathematisch unbeschraenkt, waehrend das aktuelle Laufzeitprofil einen begrenzten 32-Bit-Wert verwendet. | Die Wertrepraesentation muss auf beliebig grosse Ganzzahlen umgestellt werden; Parser, Arithmetik, Serialisierung, Limits und API-Werte muessen angepasst werden. | Eine DTO-Vertragspruefung ist erforderlich, falls Zahlen ausserhalb sicherer JSON-Bereiche uebertragen werden. |
| `CM-OCL-015`, `OCL-PROFILE-009` | `PARTIAL` | `allInstances()` ist fuer Modellklassen und den aktuellen Snapshot vorhanden, aber der optionale OCL-Compliance-Point ist nicht vollstaendig nachgewiesen. | Snapshotindex, Typinklusion, abstrakte und konkrete Subtypen, Lifecycle-Grenzen sowie Ressourcenlimits muessen vollstaendig spezifiziert und getestet werden. | Der optionale Compliance-Point muss anschliessend ausdruecklich als vollstaendig beansprucht werden. |
| `CM-CTX-007`, `OCL-PROFILE-011` | `VERIFIED` | B40 persistiert Property-/Operation-Definitionen mit Class-/Package-Owner und Source Range; Class-Definitionen nehmen am typisierten Dispatch teil, Package-Definitionen sind namespacegebunden und `self`-frei. | Read/CRUD, Revision, Draft-Erhalt, Blocker, Signatur-/Zyklusdiagnostics und JSON-Roundtrip sind getestet. | F9/F12 konsumieren die durch B37 freigegebenen Backendprojektionen und Commands. |
| `CM-UML-003`, `OCL-PROFILE-007` | `VERIFIED` / `PARTIAL` | B38 schließt explizite Feature-Redefinition bei Mehrfachvererbung einschließlich Konformität, Konfliktauflösung, Persistenz und OCL-Dispatch. `OCL-PROFILE-007` bleibt wegen separat ausgeschlossener optionaler Navigationsbereiche insgesamt `PARTIAL`. | Keine B38-Semantiklücke; F2 konsumiert Read Model und revisionsgeschützten Command. | Editierbare Redefinitionsbeziehungen sind backendseitig freigegeben; B39/B40 bleiben getrennt. |

## B37-Vertragsabnahme

Die ursprüngliche Abnahme vom 28. August 2026 identifizierte
`CM-UML-003` als Blocker für B38; B38 hat diesen Status anschließend auf
`VERIFIED` gesetzt. B40 setzt auch `CM-CTX-007` und `OCL-PROFILE-011` nach
Implementierung und Regression auf `VERIFIED`.
Statische Features und classifierweite Werte sind als S2/B39 umgesetzt und
getestet, weiterhin ohne eine nicht belegte Compliance-ID zu erfinden.
Die erneute B37-Abnahme ist erfolgreich; `BACKEND_CONTRACT_READY` ist für
F1-F11 und F12 gesetzt. Optionale oder ausdrücklich ausgeschlossene Profilbereiche
werden dadurch nicht als zusätzlich unterstützt klassifiziert.
| `CM-UML-014`, `OCL-PROFILE-007` | `PARTIAL` | `subsets`, `redefines`, derived und union Association Ends sind gespeichert und teilweise validiert, aber ihre vollstaendige abgeleitete Laufzeitsemantik fehlt. | Der Objektmodell- und Navigationsdienst muss Union- und Subset-Werte konsistent ableiten, Mutationen validieren und OCL-Navigation darauf ausfuehren. | Hierfuer sind aktualisierte Association-Mockups und strukturierte Validation-Ergebnisse erforderlich. |
| `OCL-PROFILE-013` | `NOT_SUPPORTED` | Der optionale Zugriff auf nicht navigierbare Association Ends ist nicht implementiert. | Typechecker und Evaluator braeuchten einen gesonderten, profilierten Zugriffspfad, ohne die normale UML-Navigierbarkeit zu umgehen. | Zuerst muss entschieden werden, ob dieser optionale Compliance-Point ueberhaupt Produktziel wird. |
| `OCL-PROFILE-015` | `NOT_SUPPORTED` | Ein optionaler Sichtbarkeits-Bypass fuer nicht oeffentliche UML-Features wird nicht angeboten. | Es waeren ein expliziter Auswertungsmodus, Berechtigungsregeln und getrennte Resolverpfade erforderlich. | Die aktuelle sichere Entscheidung, UML-Sichtbarkeit zu erzwingen, sollte nur bei klarem Anwendungsfall geaendert werden. |
| `CM-OCL-026`, `CM-LIB-016`, `OCL-PROFILE-012` | `OUT_OF_SCOPE` | `OclMessage`, `OclMessageType`, `^` und `^^` sowie Message-Result-Zugriffe fehlen. | Ein typisiertes Operation-Trace-Modell, Message-Werte, Parserknoten, Typregeln, Evaluatorsemantik, Persistenz und APIs waeren neu zu entwickeln. | Vorher sind ein eigener fachlicher Plan und neue Mockups fuer Operation Traces erforderlich. |
| `CM-OCL-027`, `CM-LIB-017`, `CM-UML-019`, `OCL-PROFILE-016` | `OUT_OF_SCOPE` | UML State Machines, aktive Zustandskonfigurationen und `oclInState()` fehlen. | Das Backend benoetigt ein State-Machine-Metamodell, Laufzeitkonfigurationen, Transitionen, Persistenz, Validierung sowie OCL-Typ- und Auswertungsregeln. | Dies ist ein separates UML-/OCL-Vorhaben mit neuen Mockups und Frontendschritten. |
| `CM-XMI-001`, `OCL-PROFILE-014` | `OUT_OF_SCOPE` | Standardkonformer UML-/OCL-Austausch ueber XMI ist nicht vorhanden. | Es waeren ein XMI-Metamodell-Mapping, Import, Export, ID-Aufloesung, Versionsregeln und Interoperabilitaetstests erforderlich. | Das aktuelle REST-/JSON-Format bleibt davon unabhaengig und darf nicht stillschweigend als XMI bezeichnet werden. |

Damit bestehen nach B33 keine offenen Gaps innerhalb des veroeffentlichten
Profils v3. Fuer vollstaendige OCL-/UML-Standardkonformitaet bleiben jedoch die
oben aufgefuehrten `PARTIAL`-, `NOT_SUPPORTED`- und `OUT_OF_SCOPE`-Bereiche
umzusetzen beziehungsweise fachlich in den Produktscope aufzunehmen.

## B46 V2-Vertragsabnahme

B46 aendert keine UML-/OCL-Semantik und damit keinen fachlichen
Compliance-Status. Die bereits als umgesetzt ausgewiesenen Object-,
Object-Link-, Enumeration-, Feature-, Association-, Package- und
Importfaehigkeiten sind nun zusaetzlich mit Revision, Atomaritaet,
Draft-Erhalt, strukturierten Fehlern und Persistenz-Roundtrips abgenommen.
`BACKEND_CONTRACT_READY_V2` ist gesetzt. Die aufgefuehrten `PARTIAL`-,
`NOT_SUPPORTED`- und `OUT_OF_SCOPE`-Standardbereiche bleiben unveraendert.

## Zusammenfassung

Der größte verbleibende Block ist nicht ein einzelner OCL-Operator. Vollständige
Semantik benötigt zuerst ein ausreichend vollständiges UML-Metamodell,
anschließend vollständige Wert-, Typ- und Bibliothekssemantik und zuletzt einen
featureweisen Compliance-Nachweis. XMI bleibt ein separat zu entscheidendes
Compliance-Vorhaben.
