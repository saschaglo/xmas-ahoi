# Spiel 2 – Peilung

Stand 26.10.2026 · [Übersicht](../../README.md) · [Spielübergreifend](../uebergreifend.md)

Die „Crew“ sind die Spieler, die einen Durchlauf der Station gemeinsam spielen, auch eine einzelne Person (siehe [Glossar](../../../CONTEXT.md)). „Beschreibung“ und „Variationen“ sind die Vorgabe des Veranstalters. „Aufbau (Empfehlung)“ stammt aus der Durchsicht und ist noch nicht entschieden.

## Beschreibung

Die Crew bestimmt, wo Fotos von Küsten und Meer aufgenommen wurden. Zu jedem Foto setzt die Person einen Pin auf eine Weltkarte. Je näher der Pin am echten Aufnahmeort liegt, desto mehr Punkte gibt es. Das Spiel hieß früher „Navigator“.

**Teilaufgabe:** Jede Person bekommt auf dem eigenen Handy 3 eigene Fotos, eins nach dem anderen.

**Karte:** Eine OpenStreetMap-Karte in der normalen Ansicht mit Länder- und Ortsnamen, frei zoom- und verschiebbar. Die Person setzt den Pin, kann ihn verschieben und tippt dann auf „Abgeben“. Danach ist das Foto gesperrt, ohne Zurück, und das nächste folgt.

**Zeit:** Die Crew hat 2 Minuten ab dem gemeinsamen Start ([Crewgröße und Teilaufgaben](../uebergreifend.md#crewgröße-und-teilaufgaben)). Die Station ist beendet, wenn alle ihre 3 Fotos abgegeben haben oder die Zeit abgelaufen ist.

**Punkte je Foto:** Sie hängen von der Luftlinie `d` zwischen Pin und Aufnahmeort ab. Bis 400 km gibt es 100 Punkte, darüber `100 · e^(−(d − 400) / 2000)`, gerundet. Ein Foto, das bei Zeitablauf nicht abgegeben ist, zählt 0 Punkte.

| Entfernung | Punkte |
|------------|--------|
| bis 400 km | 100 |
| 1.000 km | 74 |
| 2.000 km | 45 |
| 5.000 km | 10 |
| ab etwa 11.000 km | 0 |

**Punkte der Station:** Peilung ist ein Punktespiel. Die Punkte sind der Durchschnitt der Punkte aller Fotos der Crew. Am Ende sieht jede Person je Foto ihre Punkte und die Entfernung in km, aber nicht den echten Aufnahmeort. Wie jedes Punktespiel hat Peilung weder Hinweise noch Exit ([Hinweise, Abbruch und Exit](../../framework/hinweise-abbruch-exit.md)).

## Variationen

- Keine. Street View und „Koordinaten vorgeben, passendes Foto wählen“ sind verworfen.
- Die frühere Aufgabe A „Objekte auf dem Bild merken“ ist gestrichen und als [Reserve-Idee](../reserve/objekte-merken.md) aufbewahrt. Eine dritte Aufgabe entfällt.

## Requisiten

- Keine Dinge im Raum nötig, das Spiel läuft komplett auf dem Handy.
- Pool mit 40 Fotos von Küsten und Meer, jedes mit bekanntem Aufnahmeort (Breiten- und Längengrad) und freier Lizenz. Die Fotos zu beschaffen ist ein eigenes Arbeitspaket nach dem Konzept.
- Kartenansicht von OpenStreetMap: kein Schlüssel, keine Kosten.

## Aufbau (Empfehlung)

Ablauf auf dem Handy: Regelkarte (3 Fotos, 2 Minuten, Punkte nach Entfernung), Countdown, danach je Foto das Bild oben und die Karte darunter mit Restzeit-Anzeige, am Ende das Ergebnis. Die Zeit läuft auf dem Server; eine Abgabe nach Ablauf wird nicht mehr gewertet.

Gute Fotos zeigen etwas Erkennbares wie Klima, Vegetation, Felsen oder einen Leuchtturm, aber keine Ortsschilder.

Bei 80 Spielern mit je 3 Fotos sieht jedes Foto im Schnitt etwa 6 Spieler. Das reicht, damit sich zwei Crews selten überschneiden ([Weitergabe von Lösungen](../uebergreifend.md#weitergabe-von-lösungen)).
