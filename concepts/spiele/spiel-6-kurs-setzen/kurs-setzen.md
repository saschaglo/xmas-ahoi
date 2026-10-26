# Spiel 6 – Kurs setzen

Quelle: Abstimmung mit dem Veranstalter · Stand 26.10.2026 · [Übersicht](../../README.md) · [Spielübergreifend](../uebergreifend.md)

Die „Crew“ sind die Spieler, die einen Durchlauf der Station gemeinsam spielen, auch eine einzelne Person (siehe [Glossar](../../../CONTEXT.md)). „Beschreibung“ und „Variationen“ sind die Vorgabe des Veranstalters. „Aufbau (Empfehlung)“ und „Offene Fragen“ stammen aus der Durchsicht und sind noch nicht entschieden.

## Beschreibung

Jede Person der Crew bekommt einen Code und fährt damit einen Kurs auf einer Seekarte ab. Wo der Kurs endet, liegt der Schatz. Das Spiel steht am Kartentisch; Ort, Ortsabfrage („Bist du am Kartentisch?“) und Nachsehen regelt [Spiele mit Material im Raum](../uebergreifend.md#spiele-mit-material-im-raum).

**Am Kartentisch:**

- Eine Seekarte auf Papier mit 8×8 Feldern (Spalten A bis H, Zeilen 1 bis 8) und Symbolen wie Hafen, Leuchttürme, Inseln, Riffe und Wracks. Alle Crews nutzen dieselbe Karte.
- 10 Logbuchkarten, nummeriert von 0 bis 9, mit je 6 nummerierten Anweisungen. Zeile 1 nennt immer den Startpunkt, jede weitere Zeile einen Schritt („Segle 3 Felder nach Osten“). Einige Zeilen hängen von der Seekarte ab („Liegt westlich von dir ein Wrack: 1 Feld nach Norden, sonst 2 nach Westen“), einige von Firmenwissen („Wurde die Firma vor 2010 gegründet: 2 nach Osten, sonst 2 nach Westen“). Wer die Antwort nicht weiß, fragt jemanden.

**Der Code:** Jede Ziffer zeigt auf eine Logbuchkarte, ihre Position im Code auf die Zeile dieser Karte. Bei `0 4 3 7 5` gilt also Karte 0 Zeile 1, Karte 4 Zeile 2, Karte 3 Zeile 3 und so weiter. Der Server erzeugt jeden Code neu und nur so, dass der Kurs auf der Seekarte bleibt und in genau einem Feld endet.

| Crew | Ziffern je Code | Zeitlimit |
|------|-----------------|-----------|
| 1 Person | 6 | 2 Minuten |
| 2 Personen | 5 | 3 Minuten |
| 3 Personen | 4 | 4 Minuten |

**Verteilte Codes:** Jede Person der Crew hat eine Farbe. Jedes Handy zeigt die Plätze 1 bis n des Codes, auf jedem Platz eine Ziffer in der Farbe einer Person. Für jeden Platz liegt die Ziffer jeder Farbe auf genau einem Handy. Wer seinen Code zusammensetzen will, muss also bei den anderen nachfragen. Beispiel für eine Crew aus 3:

```
            Platz 1   Platz 2   Platz 3   Platz 4
Handy Rot   3 blau    7 grün    0 rot     5 blau
Handy Blau  1 grün    2 rot     4 blau    9 grün
Handy Grün  6 rot     8 blau    3 grün    1 rot
-> Code Rot = 6 2 0 1
```

Ein Einzelspieler sieht alle 6 Ziffern in seiner Farbe.

**Abgeben:** Das Handy zeigt eine Zeile mit den Buchstaben A bis H und eine mit den Zahlen 1 bis 8. Die Person wählt je einen und tippt auf „Abgeben“, richtig oder falsch. Haben alle in der Crew abgegeben, bekommt jede Person ihren zweiten Code. Die Station ist beendet, wenn alle ihre 2 Codes abgegeben haben oder die Zeit abgelaufen ist.

**Punkte:** Kurs setzen ist ein Punktespiel. Punkte = richtige Codes geteilt durch (2 × Personen in der Crew), mal 100. Ein Code, der bei Zeitablauf offen ist, zählt als falsch. Wie jedes Punktespiel hat Kurs setzen weder Hinweise noch Exit ([Hinweise, Abbruch und Exit](../../framework/hinweise-abbruch-exit.md)). Nach einem Abbruch bekommt die Crew neue Codes.

Das Zeitlimit wächst als Ausnahme mit der Crewgröße, weil das Zusammensetzen der Codes Zeit kostet ([Crewgröße und Teilaufgaben](../uebergreifend.md#crewgröße-und-teilaufgaben)). Die Zeiten werden bei einem Testlauf geprüft.

## Variationen

- Vom Veranstalter keine genannt.

## Requisiten

- Seekarte auf Papier, 8×8 Felder, laminiert, fest auf dem Tisch.
- Logbuchkarten 0 bis 9, laminiert, am Tisch befestigt.
- Ersatz für beides beim Orgateam.

## Aufbau (Empfehlung)

Die Symbole der Seekarte tragen Namen aus der Firma, zum Beispiel „Insel Web Care“ oder Leuchttürme nach Standorten oder Teams. Das bringt weiteres Firmenwissen, ohne das Spiel schwerer zu machen.

Grobe Schätzung: Ein Schritt (Karte suchen, Zeile lesen, auf der Seekarte ziehen) dauert etwa 15 Sekunden. Ein Einzelspieler mit 6 Schritten braucht dann etwa 1:30 je Code; 2 Codes in 2 Minuten wären knapp. Daran sollte sich der Testlauf orientieren.

Zurücksetzen entfällt: Seekarte und Logbuchkarten bleiben liegen, nichts wird verbraucht.

## Offene Fragen und Widersprüche

1. Zeitlimits: Reichen 2, 3 und 4 Minuten für 2 Codes je Person? Das klärt der Testlauf.
2. Inhalt der Seekarte und der Logbuchkarten: Welche Symbole, wie viele Wenn-dann-Zeilen und wie viele davon mit Firmenwissen?
3. Wie werden die Logbuchkarten befestigt, und braucht es mehrere Sätze, wenn zwei Crews gleichzeitig spielen?
