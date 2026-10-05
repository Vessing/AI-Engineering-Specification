# M10.5: DataType Properties

## Ziel und Abgrenzung

M10.5 ergänzt M10 um den bisher nicht vollständig dargestellten
Bearbeitungszustand eines ausgewählten UML-DataTypes. Der Schritt verändert
keinen produktiven Frontend- oder Backend-Code und führt keinen neuen
Produktbereich ein.

**Mockups:**

- `assets/mockups/datatype-properties.html`
- `assets/mockups/delete-datatype-modal.html`

## Einordnung in den Workflow

1. Der Benutzer öffnet das `Class Diagram`.
2. Er wählt im Explorer unter `DataTypes` den Eintrag `Money` aus.
3. Das rechte Panel wird vollständig durch `DataType Properties` ersetzt.
4. Unter `Details` bearbeitet er Name und Namespace.
5. Unter `Value Properties` bearbeitet er die strukturbildenden Eigenschaften.
6. Jede Value Property erhält ihren Typ über den gemeinsamen Type Picker aus
   M10.
7. Nach erfolgreicher Backendvalidierung speichert das System die Änderung und
   zeigt die neue Modellrevision.

`Class`, `Association` und `Invariant` erscheinen in diesem Zustand nicht. Sie
gehören zu anderen Auswahlen und dürfen nicht gleichzeitig mit den
DataType-Eigenschaften dargestellt werden.

## Entworfene Ansichten

Der Desktop-Hauptzustand zeigt `Money` im Explorer und auf dem Canvas als
`«dataType»`. Das rechte Panel enthält `Details` und `Value Properties`. Der
aktive Reiter zeigt die Eigenschaften `amount : Real` und
`currency : Currency` sowie Aktionen zum Hinzufügen, Bearbeiten, Sortieren und
Löschen.

Der Details-Zustand befindet sich im vollständigen UML-Workspace im selben
`DataType Properties`-Panel. Ein Klick auf `Details` ersetzt dort den Inhalt
von `Value Properties`, ohne ein Modal, ein neues Fenster oder eine neue Seite
zu öffnen. Er enthält Name, Namespace und den nur lesbaren qualifizierten
Namen. Ein Klick auf `Value Properties` stellt die Property-Liste wieder her.
Der Editing-Zustand einer Value Property enthält Name und Type Picker.
Zusätzlich sind Empty, Loading, Disabled, Duplicate-Name-Fehler, Success und
eine blockierte Löschbestätigung für einen referenzierten DataType dargestellt.

Der eigenständige Delete-Dialog zeigt direkte, verschachtelte und
laufzeitbezogene Verwendungen als navigierbare Blocker. Erst ein leerer,
erneut gegen die Modellrevision geprüfter Impact aktiviert die atomare
Löschung des DataType und seiner owned Value Properties.

Im schmalen Viewport werden Canvas und Properties Panel untereinander
angeordnet. Das Properties Panel bleibt intern scrollbar.

## Fachliche Regeln

- Ein DataType besitzt Wertsemantik und keine Objektidentität.
- DataType-Werte werden nicht als eigenständige Objekte oder Teilnehmer von
  Assoziationslinks dargestellt.
- Value-Property-Namen sind innerhalb eines DataTypes eindeutig.
- Typreferenzen werden über stabile Identifikatoren gespeichert, aber mit
  fachlichen Namen angezeigt.
- Der Backend-Typkatalog entscheidet, welche Typen im jeweiligen Kontext
  auswählbar sind.
- Änderungen und Löschungen prüfen Referenzen aus Attributen, Operationen, OCL
  und Objektwerten.

## Vorläufige Anforderungen

Das Backend benötigt einen `UmlDataType` mit stabiler ID, Name, Namespace und
geordneten Value Properties. Jede Value Property benötigt eine stabile ID,
einen Namen, eine Position und eine `UmlTypeRefDto`. Mutationsantworten müssen
Validierungsdiagnosen, Referenzkonflikte und die neue Modellrevision liefern.

Das Frontend benötigt ein auswahlabhängiges `DataType Properties`-Panel, die
Reiter `Details` und `Value Properties`, den gemeinsamen Type Picker und
internes Scrollen für lange Property-Listen. Lokale Änderungen bleiben ein
Entwurf, bis das Backend sie erfolgreich validiert.

## Compliance-Zuordnung

| Matrix-ID | Abdeckung |
|---|---|
| `CM-UML-006` | DataType, Wertsemantik und typisierte Value Properties |
| `CM-UML-001` | stabile Identität und fachliche Namensanzeige |
| `CM-CTX-008` | Namespace und qualifizierte Typnamen |
| `CM-OCL-017` | Typkonformität verwendeter DataType-Werte |

## Annahmen und offene Entscheidungen

Es wird angenommen, dass Value Properties eine persistierte
Darstellungsreihenfolge besitzen. Offen bleiben die Unterstützung von
DataType-Operationen, Regeln für rekursive DataTypes und die konkrete
Migrationsoberfläche bei strukturellen Änderungen an bereits verwendeten
DataTypes.

## Akzeptanzkriterien

| ID | Kriterium | Ergebnis |
|---|---|---|
| `M10.5-AC-01` | Die Auswahl eines DataTypes öffnet ausschließlich DataType Properties. | erfüllt |
| `M10.5-AC-02` | Details und Value Properties sind getrennt bearbeitbar. | erfüllt |
| `M10.5-AC-03` | Value Properties verwenden den gemeinsamen Type Picker. | erfüllt |
| `M10.5-AC-04` | Empty, Loading, Disabled, Error, Success und Confirmation sind sichtbar. | erfüllt |
| `M10.5-AC-05` | Referenzierte DataTypes können nicht unbeabsichtigt gelöscht werden. | erfüllt |
| `M10.5-AC-06` | Desktop und schmale Anordnung sind berücksichtigt. | erfüllt |
| `M10.5-AC-07` | Produktiver Frontend- und Backend-Code bleibt unverändert. | erfüllt |
