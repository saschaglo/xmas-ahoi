# 08 Spiel 6 festlegen

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: 01

## Question

Welches Spiel wird Spiel 6? Rahmen: Spiel an einem festen Ort mit Material und Ortsabfrage (aus [Raumspiele im Restaurant](07-raumspiele-restaurant.md)), Firmenwissen und Soziales sollen im Gesamtpaket vorkommen (bisher nur im Quiz), höchstens 5 Minuten, unbetreut. Ergebnis ist eine Beschreibung und Variationen auf dem Niveau der anderen Spielseiten. Die Details werden ein eigenes Ticket.

## Kontext

- [Spiel 6 – Kurs setzen](../../../concepts/spiele/spiel-6-kurs-setzen/kurs-setzen.md) (bisherige Ideen: Seemannsgarn, Signalflaggen, Synchron rudern)
- [Rahmen der Feier](../../../concepts/rahmen/rahmen-der-feier.md)
- Spieltyp (Rätsel oder Punktespiel) aus [Spieltypen, Punkte und Crew je Station](01-spieltypen-schluesselregel.md); Spiel 6 muss einen davon festlegen

## Answer

- Spiel 6 heißt **Kurs setzen**, ist ein Punktespiel und steht am Kartentisch.
- Material: eine Seekarte auf Papier mit 8×8 Feldern und 10 Logbuchkarten (0 bis 9) mit je 6 Zeilen. Zeile 1 nennt den Start; einige Zeilen hängen von der Seekarte oder von Firmenwissen ab.
- Code: Ziffer = Logbuchkarte, Position = Zeile. Der Server erzeugt jeden Code neu, auf der Karte bleibend und eindeutig endend. Länge 6, 5 oder 4 Ziffern je Person bei einer Crew aus 1, 2 oder 3.
- Die Codes sind farbig über die Handys der Crew verteilt (je Platz liegt die Ziffer jeder Farbe auf genau einem Handy); Soziales wie in „Keep Talking and Nobody Explodes“.
- Eingabe: eine Zeile A bis H, eine Zeile 1 bis 8, dann „Abgeben“. 2 Codes je Person; der nächste kommt, wenn alle abgegeben haben.
- Zeitlimit 2, 3 oder 4 Minuten nach Crewgröße, als Ausnahme von „gleich lang wie Einzelspieler“; Prüfung beim Testlauf.
- Punkte = richtige Codes / (2 × Personen) × 100; offen bei Zeitablauf zählt falsch.
- Verworfen: Ladung stapeln, Synchron rudern (Verzögerung zwischen Handys), eine Weltkarte mit Küstenpunkt (doppelt zum Navigator), ein 8×8-Raster am Handy.

Eingearbeitet in: Spielseite verschoben nach `spiel-6-kurs-setzen/kurs-setzen.md` und neu geschrieben, `uebergreifend.md` (Spieltyp, Ausnahme Zeitlimit, Pool, Material im Raum), `CONTEXT.md` (Punktespiel), `README.md`, `karte-und-stationen.md`, `rahmen-der-feier.md`. Neues Ticket [Spieldetails Kurs setzen](15-spieldetails-kurs-setzen.md).
