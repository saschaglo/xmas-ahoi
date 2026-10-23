# Karte: Gesamtkonzept Xmas Ahoi

Label: wayfinder:map

## Destination

Umsetzungsreifes Gesamtkonzept in `concepts/` bis **Fr, 31.10.2026**: Keine Konzeptseite enthält mehr offene Punkte, die Spielregeln oder Ablauf ändern. Spiel 6 ist beschrieben. Kleinere Punkte dürfen als „bei Umsetzung klären“ markiert bleiben. Danach folgt die Nuxt-Umsetzung in `project/`.

## Notes

- Sprache: Grilling-Fragen, Empfehlungen und Rückfragen an den Nutzer immer auf Deutsch, in jeder Session und jedem Ticket.
- Domäne: Fachkonzept auf Deutsch. Jede Session ruft die Skills „grilling“ und „domain-modeling“ auf und nutzt die Begriffe aus dem Glossar [`CONTEXT.md`](../../CONTEXT.md).
- Eine Entscheidung wird beim Abschluss eines Tickets in die betroffene Konzeptseite unter `concepts/` geschrieben, und der zugehörige offene Punkt dort wird gelöscht. Das Ticket hält nur den Weg dorthin fest.
- Location: **Restaurant Rudolph's, Poggenmühle 5** (umgebucht von der Event-Location). Der Hinterhof gehört an dem Abend teilweise der Feier und ist der Rückfall für die Raumspiele, bei Regen unter einem Schirm.
- Raumspiele: Das Konzept nimmt an, dass das Hausrecht sie erlaubt. Die Anfrage läuft parallel ([Hausrecht beim Rudolph's anfragen](issues/06-hausrecht-anfragen.md)).
- Inhalte (Fragen, Wörter, Bilder, Hinweistexte) werden nicht produziert. Das Konzept legt nur fest, was gebraucht wird und wie viel.
- Commits: siehe `CLAUDE.md`, Abschnitt Git.

## Decisions so far

- [Spieltypen, Punkte und Crew je Station](issues/01-spieltypen-schluesselregel.md): Rätsel und Punktespiel; jede beendete Station gibt Schlüssel und 0–100 Punkte, Abbruch gibt nichts und wird neu gespielt; Crew bildet sich je Station; Rangliste nach Punkten statt Ziehung, Trostpreis für alle im Lostopf.
- [Hinweise, Exit und Mindestzeiten je Spiel](issues/02-hinweise-exit-mindestzeiten.md): Hinweise und Exit nur bei Rätseln; Rätsel haben kein Zeitlimit (Begriffe finden verliert seins); Punktespiele haben immer ein Zeitlimit. Mindestzeiten gleich für alle Rätsel, fest ab Start des Durchlaufs (1:30 / 3:00 / 4:00).
- [Rudolph's: Räume, Netz, Hausregeln](issues/05-rudolphs-recherche.md): Restaurant mit einem ebenerdigen Gastraum (laut Nebenquelle etwa 60 Plätze), Terrasse und Hinterhof. WLAN ist nicht genannt. Besondere Deko muss bis 13.11. abgesprochen sein.

## Not yet specified

- **Spieldetails Spiel 6:** erst nach [Spiel 6 festlegen](issues/08-spiel-6-festlegen.md). Je nach Ergebnis ein Handy- oder ein Raumspiel.
- **Rückseite der Bordkarte:** Morsetabelle, Anleitung für Koordinaten, Schlüssel für die Verschlüsselung? Das hängt an den Spieldetails von Navigator, Morse und Begriffe finden.
- **Zeitbudget gesamt:** Die Summe aller Spiele inklusive Laufwege muss in 30 Minuten passen (übergreifend 2). Das lässt sich erst prüfen, wenn alle Spiele festgelegt sind.
- **Netz im Gastraum:** Ob es einen Rückfall bei schlechtem Empfang braucht, hängt an der Antwort vom Rudolph's und an einem Test vor Ort.
- **Weitere Ehrungen:** Neben Plätzen 1 bis 3 und „Teamplayer“ sind weitere Werte denkbar, zum Beispiel „Gespielte Spiele als Crew“. Viele Spieler werden dabei gleichauf liegen, also braucht jede Ehrung ein Los bei Gleichstand.

## Out of scope

- Inhalte produzieren (Fragenpool, Wortlisten, Bilder, Hinweistexte, Knotenkarten): eigenes Arbeitspaket nach dem Konzept.
- Technische Spezifikation (Datenmodell, Seiten, API) und die Handy-Ansicht des Admin-Panels: kommen mit der Umsetzung.
- Trilium nachpflegen: `concepts/` ist die Quelle.
- Gestaltung und Druck der Bordkarten.
