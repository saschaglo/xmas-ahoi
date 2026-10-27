# 12 Spieldetails Knoten

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: 02, 03, 07

## Question

Wie wird der Ablauf des Knotenspiels so robust, dass er ohne Betreuung funktioniert: Wie wird das Lösungswort sichtbar, wie wird der Knoten geprüft (Prüfbild und Ehrlichkeit?), wie viele Knoten gehören zu einem Durchlauf, wie schließt die Crew in der App ab, wozu dient die Trennwand, wie viele Seilplätze braucht es, und was nennen die Hinweise, wenn die Crew schon am Knotentisch steht?

## Kontext

- [Spiel 4, früher Knoten](../../../concepts/spiele/spiel-4-fischfang/fischfang.md)
- [Spiele mit Material im Raum](../../../concepts/spiele/uebergreifend.md#spiele-mit-material-im-raum): Ort, Ortsabfrage, Nachsehen

## Answer

- Das Knotenspiel wird gestrichen. Ohne Betreuung lässt sich nicht nachweisen, ob ein Knoten gebunden wurde, und der Veranstalter will ein Geschicklichkeitsspiel mit Nachweis. Geprüft und verworfen: Sturmfahrt am Handy, Ringwurf, Containerschiff stapeln (Ehrlichkeit), Magnetangel (Code liegt sichtbar oben).
- Spiel 4 heißt jetzt **Fischfang**, ein Rätsel am **Fischteich** (Wanne mit Wasser, Gastraum oder draußen, mit Ortsabfrage).
- 24 schwimmende, selbstaufrichtende Fische: Öse oben, 4-stelliger Code unten. 4 Farben mal 6 Stellungen eines Steuerrads (12 bis 10 Uhr) ergeben jeden Fisch genau einmal. 3 Hakenangeln plus 1 Ersatz.
- Rätsel als figurale Reihe, milder als ein Matrizentest: Das Handy zeigt 3 Fische, der 4. folgt aus einer Regel fürs Rad (+1, +2, gegen den Uhrzeigersinn, steht) und einer für die Farbe (gleich, abwechselnd, die fehlende). Höchstens 2 Änderungen zugleich, steigend je Fang. Der Server erzeugt die Reihen, mit genau einer Lösung und nur aus Fischen im Teich, die niemand sonst sucht. Verworfen: Wissensrätsel zur Farbe, reine Farbfolgen, Anzahl Punkte als Muster, Ketten über die Unterseite.
- Teilaufgabe: 3 Fänge je Person nacheinander (Testlauf bestätigt). Code eingeben: richtig → Fisch neben den Teich bis zum Ende der Crew; anderer Fisch → zurück ins Wasser; unbekannter Code → Tippfehler-Meldung.
- Hinweis 1 nennt eine Regel, Hinweis 2 zeigt den Fisch; Takt und Punkte wie bei allen Rätseln.
- Knopf „Fisch nicht im Teich“: zeigt nur „Sag bitte jemandem aus dem Orgateam Bescheid“ und stellt eine neue Reihe. Keine Meldung, kein Log. Das Orgateam zählt beim Rundgang die Fische und ersetzt fehlende.
- Mogeln (hineingreifen, Codes fotografieren) ist akzeptiertes Risiko.

Eingearbeitet in: `concepts/spiele/spiel-4-fischfang/fischfang.md` (ersetzt `spiel-4-knoten/knoten.md`), `uebergreifend.md` (Spieleliste, Rätsel, Teilaufgaben, Weitergabe, Fischteich als fester Ort, Zurücksetzen), `karte-und-stationen.md`, `betrieb-datenschutz-risiken.md`, `CONTEXT.md` (Rätsel, Teilaufgabe, Crew), `concepts/README.md`.
