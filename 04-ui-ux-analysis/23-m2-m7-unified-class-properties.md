# M2 bis M7: Einheitliche Class Properties

Operation deletion and its reference checks are specified in
`assets/mockups/delete-operation-modal.html` and
`04-ui-ux-analysis/37-delete-operation-modal.md`.

The Operation Signature form includes `Static operation` (`isStatic`). A
static Operation is owned by and invoked on the Classifier rather than an Object
instance and is underlined in UML notation. Instance receiver selection is not
offered for static Operations; their OCL context must not assume an instance
`self` unless the language contract explicitly supplies one.

## Zweck

Dieses ergänzende Dokumentationsmockup verbindet die M2-Festlegungen zu
abstrakten Klassen und Generalisierungen, die M3-Festlegungen zu Sichtbarkeit
und Namespaces sowie die M7-Bearbeitung von Operationssignaturen. Es ersetzt
die Einzeldokumente nicht, sondern legt eine gemeinsame Darstellung innerhalb
der bestehenden `Class Properties` fest.

**Offizielles Übersichts-Mockup:** `assets/mockups/class-properties-overview.html`

**Getrennte Hauptzustände:**

- `assets/mockups/class-properties-details.html`
- `assets/mockups/class-properties-attributes.html`
- `assets/mockups/class-properties-operations.html`
- `assets/mockups/class-properties-generalizations.html`
- `assets/mockups/class-properties-generalizations-add-supertype.html`
- `assets/mockups/delete-generalization-modal.html`
- `assets/mockups/class-properties-generalizations-and-redefinitions.html`
- `assets/mockups/class-properties-generalizations-and-redefinitions.html` (offizielle
  gemeinsame Konsistenzreferenz)

Die vier Einzelmockups sind die bevorzugten Dateien für die Prüfung eines
konkreten Properties-Zustands. Das kombinierte Mockup bleibt als
Gesamtübersicht und Konsistenzreferenz erhalten.

Jedes Einzelmockup dokumentiert außerdem `Loading`, `Empty`, `Error`,
`Success` und `Confirmation`. Loading verhindert doppeltes Speichern. Empty
erklärt die nächste mögliche Aktion. Error nennt das betroffene fachliche
Element und eine Korrekturmöglichkeit. Success bestätigt den gespeicherten
Modellzustand. Confirmation wird nur für folgenreiche Änderungen verwendet,
beispielsweise das Löschen eines Features, das Entfernen einer Generalisierung
oder das Ändern des abstrakten Status.

Beim Aktivieren von `Abstract class` prüft das Backend alle aktiven Snapshots
auf direkte Instanzen der Klasse. Vorhandene Instanzen blockieren das Speichern
und werden im bestehenden Details-Zustand mit fachlichem Objektnamen
aufgeführt. `Open in Object Diagram` führt zur vorhandenen Objektbearbeitung;
es gibt dafür keinen eigenen Workflow oder zusätzlichen Dialog. Nach dem
Löschen der direkten Instanzen über den bestehenden Object-Delete-Workflow
kann die Änderung erneut gespeichert werden. Subclass-Instanzen sind keine direkten Instanzen
der abstrakt zu setzenden Klasse und blockieren diese Änderung nicht allein.

Das Mockup enthält vier vollständige Desktop-Hauptzustände: einen aktiven
`Details`-Tab, einen aktiven `Operations`-Tab, einen aktiven `Attributes`-Tab
und einen aktiven `Generalizations`-Tab. Alle Zustände verwenden dieselbe Shell und
Properties-Hierarchie.

## Einheitliche Struktur

Die Hauptsegmente bleiben `Class`, `Association` und `Invariant`. Innerhalb des
aktiven Segments `Class` zeigt ein zweites, klar untergeordnetes Segment die
Bereiche `Details`, `Attributes`, `Operations` und `Generalizations`. Dadurch werden
Attribute und Operationen nicht auf getrennte Workspace-Seiten verteilt.

Beide Featurearten verwenden denselben Ablauf:

1. Der Benutzer wählt eine Klasse im Explorer oder auf dem Canvas aus.
2. Er öffnet innerhalb von `Class Properties` den gewünschten Featurebereich.
3. Er wählt ein Attribut oder eine Operation aus der Liste aus.
4. Der Detaileditor zeigt ausschließlich die Felder dieser Featureart.
5. Speichern aktualisiert Modell, Canvasnotation und Explorer synchron.

## Fachliche Trennung

Attribute beschreiben den strukturellen Zustand einer Instanz. Ihr Editor
enthält insbesondere Sichtbarkeit, Name, Typ, untere und obere Grenze sowie die
eindeutige Value Source `Stored`, `Init` oder `Derived`. `Stored` kann einen
Default Value besitzen, `Init` einen einmalig bei Objekterstellung verwendeten
OCL-Ausdruck und `Derived` einen aus dem aktuellen Snapshot berechneten
OCL-Ausdruck. Derived impliziert UML-Notation `/` und readonly Object-Werte;
separate widersprüchliche Schalter werden nicht angeboten. Operationen
beschreiben Verhalten. Ihr Editor enthält Sichtbarkeit, Name, Rückgabetyp,
`abstract`, `query` sowie geordnete Parameter mit Name, Typ und Direction.

Die Class-Ansicht bearbeitet ausschließlich die UML-Signatur. Eine Operation
wird hier nicht ausgeführt. Die Runtime Invocation beginnt weiterhin im Object
Diagram an einem ausdrücklich ausgewählten Receiver und folgt damit der
Festlegung aus `22-m7-object-diagram-invocation.md`.

Das Einzelmockup `class-properties-operations.html` übernimmt außerdem die in M8
vereinheitlichte Operationshierarchie. Unter der ausgewählten Operation stehen
`Signature`, `Preconditions`, `Postconditions` und `Body`. Im M2-bis-M7-Zustand
ist `Signature` aktiv. Pre- und Postconditions verweisen auf die zu dieser
Operation gehörenden Contracts aus M8; `Body` bleibt bis M9 deaktiviert. Der
Hauptbereich `Invariant` bleibt davon getrennt und enthält ausschließlich
Classifier-Invarianten.

Der Bereich `Generalizations` übernimmt die M2-Festlegungen. Bei Auswahl einer
Generalisierung zeigt er den fachlichen Namen `Subclass → Superclass`, beide
Enden read-only und die sichtbar geerbten Features. Das ungefüllte Dreieck
zeigt auf die Superclass. Bestehende Beziehungen werden zunächst bestätigt
entfernt und bei Bedarf neu angelegt; ein direkter Endpunktwechsel ist eine
spätere Erweiterung. Add und Remove werden jeweils atomar validiert;
insbesondere dürfen keine Vererbungszyklen entstehen.
Der Bereich `Inherited Features` im offiziellen Generalizations-Mockup trennt
effektive Attribute und Operationen der ausgewählten Superclass. Geerbte
Features bleiben in der Subclass schreibgeschützt und führen über die
Owner-Aktion zur definierenden Klasse. Eine gleichnamige Subclass-Definition
wird nicht ohne expliziten, backendvalidierten Vertrag als UML-Redefinition
angenommen.
Dieser explizite Vertrag und sein Candidate-Picker sind in
`44-class-properties-feature-redefinition.md` dokumentiert. Redefinitionsziele
werden über stabile Feature-IDs statt über gleiche Namen gespeichert.
Das offizielle Generalizations-Mockup zeigt mehrere direkte Supertypen,
klickbare bestehende Beziehungen mit geerbten Features und die ausgeschriebene
Aktion `Vererbung hinzufügen`. Diese öffnet eine modale Kandidatenauswahl mit
festgelegter Subclass. Derselbe Modalzustand ist im offiziellen Mockup auch als
dauerhaft sichtbarer Dokumentationsabschnitt enthalten. Das ergänzende
Add-Supertype-Mockup dokumentiert
begründet deaktivierte Kandidaten sowie Diagnosen für unbekannte Superklassen,
Zyklen und geerbte Featurekonflikte.
Canvas-Kanten und Direct-Supertype-Einträge teilen denselben Selection State.
Das Entfernen einer Beziehung verlangt eine explizite Bestätigung. Loading
während der Validierung sowie Empty States ohne direkte Supertypen oder ohne
passende Kandidaten sind im offiziellen Mockup sichtbar.
Der eigenständige Delete-Dialog prüft zusätzlich Redefinitionen und andere
typabhängige Referenzen, löscht niemals die beteiligten Klassen und erhält bei
Mehrfachvererbung alle anderen direkten Supertypen. Nach erfolgreichem Löschen
wird die effektive transitive Hierarchie vollständig neu berechnet.

Die zusammenhängende Hierarchie-, Inherited-Feature- und Redefinitionsabfolge
ist verbindlich in `46-unified-generalizations-and-redefinitions.md`
dokumentiert. Die älteren Einzelmockups bleiben Detailreferenzen, definieren
aber keinen abweichenden Workflow.

Das offizielle Generalizations-Mockup zeigt zusätzlich einen konkreten
zyklusbildenden und deaktivierten Kandidaten, strukturierte Diagnosen für
unbekannte Superklassen, Zyklen und geerbte Featurekonflikte sowie Quick Help
zur UML-Notation. Eine dynamische `Inheritance chain` macht neben dem direkten
Supertype auch transitive Obertypen der ausgewählten Beziehung sichtbar.

## Visuelle und responsive Regeln

- Attribute und Operationen besitzen getrennte UML-Kompartimente im Class Node.
- Sichtbarkeit wird durch Symbol und ausgeschriebenen Wert dargestellt.
- Canvas, Explorer, Featureliste und Detaileditor teilen dieselbe Auswahl.
- Das Properties Panel bleibt intern scrollbar.
- Im schmalen Viewport öffnet `Class Properties` als Drawer.
- Der Wechsel zwischen Attributes und Operations erhält Klasse und Kontext.
- Featurelisten und Detailfelder dürfen keinen horizontalen Überlauf erzeugen.

## Compliance-Zuordnung

Das gemeinsame Mockup visualisiert `CM-UML-002` für Generalisierung und
abstrakte Klassen, `CM-UML-004` für Sichtbarkeit und `CM-UML-015` für
Operationssignaturen. Namespace und qualifizierter Name
unterstützen außerdem `CM-UML-018`. Es erklärt keine dieser Anforderungen als
technisch implementiert.

## Akzeptanzkriterien

- [x] Attribute und Operationen erscheinen unter derselben Klasse.
- [x] Name, Namespace, Sichtbarkeit und abstrakter Status liegen unter `Details`.
- [x] `Class`, `Association` und `Invariant` bleiben die Hauptsegmente.
- [x] Attributes und Operations sind eindeutig untergeordnete Featurebereiche.
- [x] Beide Featurearten folgen demselben Auswahl- und Bearbeitungsmuster.
- [x] Attribut- und Operationsfelder bleiben fachlich getrennt.
- [x] Ein vollständiger Hauptzustand mit aktivem `Attributes`-Tab ist sichtbar.
- [x] Ein vollständiger Hauptzustand mit aktivem `Generalizations`-Tab ist sichtbar.
- [x] Subclass, Superclass, Dreiecksrichtung und geerbte Features sind eindeutig.
- [x] Operationssignatur und Runtime Invocation werden nicht vermischt.
- [x] Signature, Pre-/Postconditions und Body sind derselben Operation untergeordnet.
- [x] Operation Contracts und Classifier-Invarianten sind visuell getrennt.
- [x] Desktop und schmaler Viewport sind dokumentiert.
- [x] Alle vier Einzelmockups zeigen Loading, Empty, Error, Success und Confirmation.
- [x] Produktiver Frontend- und Backend-Code bleibt unverändert.
- [x] Explizite Redefinition geerbter Attributes und Operations besitzt einen
  separaten, mehrvererbungsfähigen Referenzworkflow.

## Annahmen und offene Entscheidungen

Die gemeinsame Feature-Navigation ist eine UI-Festlegung und verlangt keine
neue UML-Entität. Offen bleibt, ob die untergeordneten Featuresegmente später
als Tabs, segmentierte Auswahl oder zugängliche Listbox implementiert werden.
Die visuelle Hierarchie und der beschriebene Workflow müssen dabei erhalten
bleiben.
