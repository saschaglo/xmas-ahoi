# 03 Mengen und Varianten nach Crewgröße

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: 01

## Question

Nach welcher Regel skalieren alle Spiele mit der Crew (1, 2 oder 3 Spieler)? Spielt jede Person auf dem eigenen Handy parallel mit eigener Aufgabe, oder spielt die Crew gemeinsam auf einem Handy? Was gilt für Einzelspieler bei Varianten, die auf Verständigung zwischen Personen setzen (zum Beispiel Navigator Aufgabe A: Fragen bei einer anderen Person)?

## Kontext

- [Spielübergreifend, Punkt 4](../../../concepts/spiele/uebergreifend.md#spielübergreifende-offene-punkte)
- [Karte und Stationen](../../../concepts/framework/karte-und-stationen.md) (Spielstand je Spieler) und [Crews](../../../concepts/framework/crews.md) (Crew je Station)
- Quiz Fragen 1, 2, 5; Navigator Frage 3; Morse Frage 4; Knoten und Begriffe finden: Variationen

## Answer

- Jede Person spielt auf dem eigenen Handy eine eigene **Teilaufgabe** (neu im Glossar). Eine Crew aus n Personen hat n Teilaufgaben.
- Dieses Ticket legt nur die allgemeine Regel fest. Die Zahlen je Spiel (Fragen, Objekte, Wörter, Knoten, Poolgrößen) entscheiden die Spieltickets.
- Varianten, die Verständigung zwischen Personen brauchen (Navigator Aufgabe A), spielen Einzelspieler in der Grundform auf dem eigenen Handy.
- Rätsel enden, wenn alle Teilaufgaben gelöst sind; wer fertig ist, wartet und darf helfen. Punktespiele haben ein gemeinsames Zeitlimit ab Start, gleich lang wie für Einzelspieler.
- Teilaufgaben werden ohne Wiederholung in der Crew gezogen; nach einem Abbruch bekommt ein Spieler an der Station nichts, was er dort schon hatte.
- Hinweise, ersetzt die Mindestzeiten aus Ticket 02: höchstens 2 je Crew, unabhängig von der Crewgröße. Der erste ist ab 0:30 klickbar, nach jedem Klick 60 s Sperre für alle. Den Text sieht nur, wer klickt, zu seiner eigenen Teilaufgabe; die Stufe zählt je Person. Jeder Hinweis kostet die Crew 25 Punkte. Alle sehen Zähler und Sperre; vor dem Klick fragt die App nach.
- Exit 60 s nach Hinweis 2 (frühestens 2:30), Zustimmung aller bleibt.

Eingearbeitet in: `uebergreifend.md` (neuer Abschnitt „Crewgröße und Teilaufgaben“, offener Punkt 4 gelöscht), `hinweise-abbruch-exit.md`, `crews.md`, `CONTEXT.md`, `CLAUDE.md`, `README.md`, `rahmen-der-feier.md`, `wireframes-spieler-flow.md`, Quiz (Fragen 1 und 2 gelöscht), Navigator (Frage 3 gelöscht), Morsezeichen (Frage 4 gekürzt), Begriffe finden. Verweise in den Tickets 04, 09, 10, 11 angepasst; Ticket 02 als teilweise überholt markiert.
