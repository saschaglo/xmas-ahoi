# 10 Spieldetails Navigator

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: 02, 03

## Question

Wie ist eine Navigator-Station aufgebaut (Aufgaben A, B, C nacheinander oder eine je Durchlauf), welche Variante von Aufgabe B gilt (Foto auf OpenStreetMap, Street View oder Koordinaten), wie genau muss der Tipp sein, woher kommen die Bilder, was wird Aufgabe C, und welches Zeitlimit hat jede Aufgabe?

## Kontext

- [Peilung, früher Navigator, offene Fragen](../../../concepts/spiele/spiel-2-peilung/peilung.md) (1 bis 7)

## Answer

- Aufgabe A „Objekte merken“ ist gestrichen und als Reserve-Idee aufbewahrt. Aufgabe C entfällt. Das Spiel besteht nur noch aus „Position bestimmen“ und heißt jetzt **Peilung**.
- Die Person sieht ein Küstenfoto, setzt einen Pin auf eine OpenStreetMap-Karte mit Länder- und Ortsnamen und tippt auf „Abgeben“. Street View und die Koordinaten-Variation sind verworfen, damit entfallen auch das Blatt an der Station und die Anleitung auf der Bordkarte.
- Punkte je Foto nach der Luftlinie `d`: bis 400 km 100, darüber `100 · e^(−(d − 400) / 2000)`, gerundet (1.000 km: 74, 2.000 km: 45, 5.000 km: 10). Die lineare Kurve bis zur Gegenseite der Erde ist verworfen, weil fast jeder 80 oder mehr Punkte bekäme.
- Die Punkte der Station sind der Durchschnitt aller Fotos der Crew. Die Definition von Punktespiel lautet jetzt „Anteil an den möglichen Punkten“ statt „Trefferquote“.
- 3 Fotos je Person, 2 Minuten gesamt ab dem gemeinsamen Start. Nach der Abgabe ist das Foto gesperrt. Ein Foto ohne Abgabe zählt 0.
- Rückmeldung am Ende: Punkte und Entfernung in km je Foto, ohne den echten Ort. Der Pool hat 40 Fotos mit bekanntem Aufnahmeort und freier Lizenz; sie zu beschaffen ist out of scope.

Eingearbeitet in: `concepts/spiele/spiel-2-peilung/peilung.md` (umbenannt von `spiel-2-navigator/navigator.md`, offene Fragen gelöscht), neue Seite `concepts/spiele/reserve/objekte-merken.md`, `uebergreifend.md` (Spieleliste, Punktetabelle, Pool-Beispiel, neuer Abschnitt „Reserve-Ideen“), `CONTEXT.md` (Punktespiel, Crew), `README.md`, `betrieb-datenschutz-risiken.md` (Mogeln: Bildersuche statt Wimmelbild-Screenshot).
