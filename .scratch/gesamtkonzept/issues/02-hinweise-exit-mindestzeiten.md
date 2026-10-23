# 02 Hinweise, Exit und Mindestzeiten je Spiel

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: 01

## Question

Welche Spiele haben Hinweise und Exit, und welche Mindestzeiten gelten jeweils? Die globalen Werte (Hinweis 1 nach 1:30, Hinweis 2 nach 3:00, Exit nach 4:00) passen nicht zu Spielen mit Zeitlimit von 2 Minuten (Quiz) oder knappem Laufweg (Begriffe finden). Werden die Zeiten je Spiel einstellbar, oder entfallen Hinweise und Exit bei einem Spieltyp ganz? Gelten sie beim Navigator je Aufgabe oder für die ganze Station? Schon entschieden: Ein Exit gibt den Schlüssel und 0 Punkte, ein Hinweis kostet bei einem Rätsel 25 Punkte.

## Kontext

- [Hinweise, Abbruch und Exit](../../../concepts/framework/hinweise-abbruch-exit.md)
- [Spielübergreifend, Punkt 1](../../../concepts/spiele/uebergreifend.md#spielübergreifende-offene-punkte)
- Quiz Frage 1, Navigator Frage 2, Knoten Frage 4, Begriffe finden Frage 5 (Nummern vor dieser Entscheidung)

## Answer

- Hinweise und Exit gibt es nur bei Rätseln. Rätsel haben kein Zeitlimit; wer festhängt, nimmt Hinweise und zuletzt den Exit.
- Punktespiele haben weder Hinweise noch Exit, auch keinen Exit mit Teilpunkten: Er brächte nur ein früheres Ende bei gleichem Ergebnis. Dafür gilt: Jede Aufgabe nimmt eine beliebige Antwort an, und jedes Punktespiel hat ein Zeitlimit. Damit entfällt die Frage, ob Hinweise beim Navigator je Aufgabe gelten.
- Begriffe finden verliert sein Zeitlimit und wird damit ein normales Rätsel.
- Damit ist auch die Quiz-Frage „egal ob richtig oder falsch“ beantwortet: Eine falsche Antwort zählt als falsch, und es geht weiter.
- Die 5 Minuten je Spiel sind ein Planungswert, kein Zeitlimit.
- ~~Mindestzeiten gelten für alle Rätsel gleich, ohne Einstellung je Spiel, und zählen fest ab Start des Durchlaufs: Hinweis 1 ab 1:30, Hinweis 2 ab 3:00 (nach Hinweis 1), Exit ab 4:00 (nach Hinweis 2).~~ Ersetzt durch den Hinweis-Takt aus [Mengen und Varianten nach Crewgröße](03-mengen-nach-besetzung.md).

Eingearbeitet in: `hinweise-abbruch-exit.md`, `uebergreifend.md`, `CONTEXT.md`, `CLAUDE.md`, Quiz, Navigator, Knoten, Begriffe finden. Offene Punkte dort gelöscht und neu nummeriert; Verweise in den Tickets angepasst. Das Ticket „Spieldetails Navigator“ fragt jetzt auch nach dem Zeitlimit je Aufgabe.
