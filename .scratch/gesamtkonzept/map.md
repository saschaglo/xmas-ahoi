# Karte: Gesamtkonzept Xmas Ahoi

Label: wayfinder:map

## Destination

Umsetzungsreifes Gesamtkonzept in `concepts/` bis **Fr, 31.10.2026**: Keine Konzeptseite enthält mehr offene Punkte, die Spielregeln oder Ablauf ändern. Spiel 6 ist beschrieben. Kleinere Punkte dürfen als „bei Umsetzung klären“ markiert bleiben. Danach folgt die Nuxt-Umsetzung in `project/`.

## Notes

- Sprache: Grilling-Fragen, Empfehlungen und Rückfragen an den Nutzer immer auf Deutsch, in jeder Session und jedem Ticket.
- Domäne: Fachkonzept auf Deutsch. Jede Session ruft die Skills „grilling“ und „domain-modeling“ auf und nutzt die Begriffe aus dem Glossar [`CONTEXT.md`](../../CONTEXT.md).
- Eine Entscheidung wird beim Abschluss eines Tickets in die betroffene Konzeptseite unter `concepts/` geschrieben, und der zugehörige offene Punkt dort wird gelöscht. Das Ticket hält nur den Weg dorthin fest.
- Location: **Restaurant Rudolph's, Poggenmühle 5** (umgebucht von der Event-Location). Keine Standortkonzepte: Plätze, Hausrecht und Rückfälle löst das Orgateam vor Ort.
- Inhalte (Fragen, Wörter, Bilder, Hinweistexte) werden nicht produziert. Das Konzept legt nur fest, was gebraucht wird und wie viel.
- Commits: siehe `CLAUDE.md`, Abschnitt Git.

## Decisions so far

- [Spieltypen, Punkte und Crew je Station](issues/01-spieltypen-schluesselregel.md): Rätsel und Punktespiel; jede beendete Station gibt Schlüssel und 0–100 Punkte, Abbruch gibt nichts und wird neu gespielt; Crew bildet sich je Station; Rangliste nach Punkten statt Ziehung, Trostpreis für alle im Lostopf.
- [Hinweise, Exit und Mindestzeiten je Spiel](issues/02-hinweise-exit-mindestzeiten.md): Hinweise und Exit nur bei Rätseln; Rätsel haben kein Zeitlimit (Begriffe finden verliert seins); Punktespiele haben immer ein Zeitlimit. Mindestzeiten (1:30 / 3:00 / 4:00) später ersetzt durch den Hinweis-Takt aus [Mengen und Varianten nach Crewgröße](issues/03-mengen-nach-besetzung.md).
- [Rudolph's: Räume, Netz, Hausregeln](issues/05-rudolphs-recherche.md): Restaurant mit einem ebenerdigen Gastraum (laut Nebenquelle etwa 60 Plätze), Terrasse und Hinterhof. WLAN ist nicht genannt. Besondere Deko muss bis 13.11. abgesprochen sein.
- [Mengen und Varianten nach Crewgröße](issues/03-mengen-nach-besetzung.md): Jede Person löst auf dem eigenen Handy eine eigene Teilaufgabe; Einzelspieler spielen Verständigungs-Varianten in der Grundform. Neuer Hinweis-Takt: 2 Hinweise je Crew, ab 0:30, 60 s Sperre, Text nur für den Klickenden; Exit ab 60 s nach Hinweis 2.
- [Weitergabe von Lösungen: Restrisiko](issues/04-weitergabe-von-loesungen.md): Schutz nur durch zufällige Teilaufgaben aus Pools in allen Spielen, keine Erkennung, ein Fairness-Satz beim Check-in; Mogeln (Chat, Googeln, Screenshot, Knoten überspringen) ist akzeptiertes Risiko.
- [Raumspiele im Restaurant: Orte, Kapazität, Zurücksetzen](issues/07-raumspiele-restaurant.md): Nur Regeln, keine Plätze; Knotentisch im Gastraum, Ortsabfrage vor der Crew-Bildung bei Knoten und Spiel 6; Begriffe-Orte ohne Ausschlüsse; Orgateam sieht nach; moderierte Einleitung in den Abend.
- [Spiel 6 festlegen](issues/08-spiel-6-festlegen.md): Kurs setzen, ein Punktespiel am Kartentisch: Code aus Logbuchkarten führt auf einer Seekarte (8×8) zu einem Feld; Ziffern farbig über die Handys der Crew verteilt; Firmenwissen in Wenn-dann-Zeilen; Zeitlimit 2/3/4 Minuten je Crewgröße, 2 Codes je Person. Details in [Spieldetails Kurs setzen](issues/15-spieldetails-kurs-setzen.md).
- [Spieldetails Quiz](issues/09-spieldetails-quiz.md): 4 Kategorien (Schätzfragen, Allgemeinwissen, Sport, Firma), alle Fragen Multiple Choice mit 4 Antworten, Schätzfragen als Zahlenbereiche; 5 Fragen je Person (eine je Kategorie plus eine zufällige), 2 Minuten gesamt, keine Kategoriewahl; Pool 80 Fragen.
- [Spieldetails Navigator](issues/10-spieldetails-navigator.md): Spiel heißt jetzt **Peilung**, nur noch Pin auf OpenStreetMap zu Küstenfotos; Punkte fallen mit der Entfernung (voll bis 400 km); 3 Fotos je Person in 2 Minuten, Pool 40; Aufgabe A wird Reserve-Idee „Objekte merken“, Aufgabe C entfällt; Punktespiel heißt jetzt „Anteil an den möglichen Punkten“.
- [Spieldetails Morsezeichen](issues/11-spieldetails-morsezeichen.md): Rätsel mit Höchstzeit 5 Minuten (danach automatischer Exit); Crew entschlüsselt einen gemeinsamen **Funkspruch** aus einem Wortstapel mit Positionsnummern und sortiert ihn per Drag & Drop; Hinweise für alle in der Wortliste; Morsetabelle nur an der Funkstation; Satzlänge im Testlauf.
- [Spieldetails Knoten](issues/12-spieldetails-knoten.md): Knoten gestrichen (kein Nachweis ohne Betreuung); Spiel 4 ist jetzt **Fischfang**, ein Rätsel am Fischteich: Fischreihe mit Steuerrad und Farbe auf dem Handy fortsetzen, den fehlenden von 24 schwimmenden Fischen angeln, Code eingeben; 3 Fänge je Person.
- [Spieldetails Begriffe finden](issues/13-spieldetails-begriffe-finden.md): Begriffe finden ersetzt durch **Seemannsgarn**, ein Rätsel: Geschichte über einen Seefahrer (nur Nummer, kein Name) am Erzähltisch lesen, an 4 Faktentafeln die passenden Karten finden, 4 Buchstaben eingeben; Hinweis 1 nennt den Namen. Die Bordkarte braucht keine Rückseite mit Schlüssel.
- [Admin und Betrieb: Schalter und Löschkonzept](issues/14-admin-und-betrieb.md): Drei Zustände (nicht eröffnet, offen, beendet), Wiederöffnen erlaubt; Schließen verfällt laufende Durchläufe nach Ansage; neues „Zurücksetzen“ mit Datumseingabe behält Nutzer und Codes; Daten 14 Tage nach der Feier löschen durch kompletten Abbau, anonyme Zahlen bleiben.
- [Spieldetails Kurs setzen](issues/15-spieldetails-kurs-setzen.md): Seekarte mit 5 Symbolarten als Bezugspunkte (Firmennamen nur Deko); feste Zeilenrollen auf den Logbuchkarten (Zeile 3 Seekarte, Zeile 4 Firmenwissen, nur Nachbarfelder); ein Materialsatz, A3 plus A6, festgeklebt; Testlauf-Kriterium mit Reihenfolge Zeitlimit vor Ziffern.

## Not yet specified

- **Zeitbudget gesamt:** Die Summe aller Spiele inklusive Laufwege muss in 30 Minuten passen (übergreifend 2). Das lässt sich erst prüfen, wenn alle Spiele festgelegt sind.
- **Netz im Gastraum:** Ob es einen Rückfall bei schlechtem Empfang braucht, hängt an einem Test vor Ort.
- **Weitere Ehrungen:** Neben Plätzen 1 bis 3 und „Teamplayer“ sind weitere Werte denkbar, zum Beispiel „Gespielte Spiele als Crew“. Viele Spieler werden dabei gleichauf liegen, also braucht jede Ehrung ein Los bei Gleichstand.

## Out of scope

- Inhalte produzieren (Fragenpool, Wortlisten, Bilder, Hinweistexte, Fische): eigenes Arbeitspaket nach dem Konzept.
- Technische Spezifikation (Datenmodell, Seiten, API) und die Handy-Ansicht des Admin-Panels: kommen mit der Umsetzung.
- Trilium nachpflegen: `concepts/` ist die Quelle.
- Gestaltung und Druck der Bordkarten.
- [Hausrecht beim Rudolph's anfragen](issues/06-hausrecht-anfragen.md): Orga-Aufgabe, das Konzept hängt nicht an der Antwort.
