# Minispiele: spielübergreifende Punkte

Quelle: Trilium „Minispiele – Übersicht“ · Stand 23.10.2026 · [Übersicht](../README.md)

Sechs Minispiele sind vorgesehen und beschrieben. Jede Spielseite enthält Beschreibung, Variationen, Requisiten, eine Aufbau-Empfehlung sowie offene Fragen und Widersprüche. Hier stehen nur die spielübergreifenden Punkte. Begriffe wie „Crew“, „Rätsel“ und „Punktespiel“ stehen im [Glossar](../../CONTEXT.md).

## Spiele

1. [Spiel 1 – Quiz](spiel-1-quiz/quiz.md): Fragen aus einem Pool, nach Crewgröße skaliert, nur am Handy.
2. [Spiel 2 – Peilung](spiel-2-peilung/peilung.md): Pin auf der Weltkarte für Küstenfotos, Punkte nach Entfernung; nur am Handy.
3. [Spiel 3 – Morsezeichen](spiel-3-morsezeichen/morsezeichen.md): Wörter eines Funkspruchs per Licht oder Ton entschlüsseln und zum Satz sortieren; an der Funkstation.
4. [Spiel 4 – Fischfang](spiel-4-fischfang/fischfang.md): Eine Reihe aus Fischen auf dem Handy fortsetzen und den fehlenden Fisch aus dem Fischteich angeln.
5. [Spiel 5 – Seemannsgarn](spiel-5-seemannsgarn/seemannsgarn.md): Eine Geschichte über einen Seefahrer lesen und an 4 Faktentafeln die passenden Fakten finden.
6. [Spiel 6 – Kurs setzen](spiel-6-kurs-setzen/kurs-setzen.md): Codes aus Logbuchkarten führen auf einer Seekarte am Kartentisch zu einem Feld.

## Spieltypen und Punkte

Jedes Spiel hat einen festen Spieltyp:

| Spieltyp | Spiele | Ergebnis | Punkte (0 bis 100) |
|----------|--------|----------|--------------------|
| Rätsel | Morsezeichen, Fischfang, Seemannsgarn | gelöst oder Exit | gelöst: 100 minus 25 je genommenem Hinweis; Exit: 0 |
| Punktespiel | Quiz, Peilung, Kurs setzen | Anteil an den möglichen Punkten | Quiz und Kurs setzen: Anteil der Treffer, mal 100; Peilung: Durchschnitt der Punkte je Foto |

- Jede beendete Station gibt den Schlüssel, auch mit 0 Punkten. Ein Punktespiel ist beendet, wenn alle Aufgaben beantwortet sind oder die Zeit abgelaufen ist.
- Ein Abbruch gibt weder Schlüssel noch Punkte, auch wenn bis dahin Treffer erzielt wurden. Die Station wird neu gespielt ([Abbruch](../framework/hinweise-abbruch-exit.md)).
- Alle in der Crew bekommen dieselben Punkte. Weil die Mengen mit der Crewgröße wachsen, zählt die Quote, nicht die absolute Trefferzahl; so sind Einzelspieler und Crews vergleichbar.
- Kein Zeitbonus: Wer ein Rätsel ohne Hinweis löst, bekommt 100 Punkte, egal wie schnell.
- Rätsel haben kein Zeitlimit, dafür Hinweise und Exit. Ausnahme ist Morsezeichen mit einer Höchstzeit von 5 Minuten, danach folgt ein automatischer Exit. Punktespiele haben ein Zeitlimit, aber weder Hinweise noch Exit ([Hinweise, Abbruch und Exit](../framework/hinweise-abbruch-exit.md)).
- Jedes Spiel braucht genug Varianten (Fragen, Sätze, Bilder, Orte), damit eine Crew nach einem Abbruch neue Aufgaben bekommt. Ausnahme ist Morsezeichen: Dort darf sich ein Satz wiederholen.

## Crewgröße und Teilaufgaben

Alle Spiele skalieren nach derselben Regel mit der Crew:

- Jede Person spielt auf dem eigenen Handy eine eigene [Teilaufgabe](../../CONTEXT.md) (eigene Fragen, eigene Fänge, eigener Code). Eine Crew aus n Personen hat n Teilaufgaben. Wie groß eine Teilaufgabe ist, legt die Seite des Spiels fest. Ausnahme ist Morsezeichen: Dort entschlüsselt die Crew einen gemeinsamen Funkspruch, und jedes Handy zieht seine Wörter aus einem gemeinsamen Stapel.
- Der Server zieht die Teilaufgaben ohne Wiederholung innerhalb der Crew. Nach einem Abbruch bekommt ein Spieler an dieser Station nichts, was er dort schon hatte, außer bei Morsezeichen. Die nötige Poolgröße legt die Seite des Spiels fest.
- Rätsel: Die Station ist beendet, wenn alle Teilaufgaben gelöst sind. Wer fertig ist, sieht „Warte auf deine Crew“ und darf den anderen helfen. Verlassen lässt sich die Station nur über Exit oder Abbruch.
- Punktespiel: Die Crew hat ein gemeinsames Zeitlimit ab dem gemeinsamen Start, gleich lang wie für Einzelspieler. Ausnahme ist Kurs setzen: Dort wächst das Zeitlimit mit der Crew, weil die Codes über die Handys verteilt sind. Die Station ist beendet, wenn alle ihre Teilaufgaben beantwortet haben oder die Zeit abgelaufen ist.
- Varianten, die auf Verständigung zwischen Personen setzen (zum Beispiel bei der Reserve-Idee [Objekte merken](reserve/objekte-merken.md) die Fragen bei einem anderen Mitglied), spielen Einzelspieler in der Grundform auf dem eigenen Handy.
- Hinweise bei Rätseln gelten je Crew, ihr Text je Person; bei Morsezeichen sehen alle in der Crew den Hinweis ([Hinweise, Abbruch und Exit](../framework/hinweise-abbruch-exit.md)).

## Weitergabe von Lösungen

Lösungen lassen sich in der Firma weitergeben (Chat, Zuruf, Foto). Die Schatzsuche schützt sich dagegen nur durch Zufall und nimmt den Rest als Risiko hin:

- In allen Spielen zieht der Server jede Teilaufgabe zufällig aus einem Pool (Fragen, Fotos, Wort, Geschichte); Fischfang erzeugt seine Reihen und Kurs setzen seine Codes jedes Mal neu. Ziel ist, dass sich zwei Crews selten überschneiden; eine Garantie gibt es nicht. Die Poolgröße legt die Seite des Spiels fest.
- Keine technische Erkennung (zum Beispiel Tab-Wechsel oder verdächtig schnelle Lösungen) und keine Sanktionen. Bei Preisen bis 20 EUR lohnt der Aufwand nicht, und Erkennung im Browser ist unzuverlässig.
- Beim Check-in bittet ein Satz darum, Lösungen nicht weiterzugeben ([Eingang und Bordkarte](../framework/eingang-und-bordkarte.md)).
- Was trotzdem möglich bleibt, steht unter [Mogeln](../framework/betrieb-datenschutz-risiken.md#bewusst-akzeptierte-risiken) in den akzeptierten Risiken.

## Spiele mit Material im Raum

Morsezeichen, Fischfang, Seemannsgarn und Kurs setzen laufen an einem festen Ort mit Material: der Funkstation, dem Fischteich, dem Erzähltisch und dem Kartentisch. Seemannsgarn nutzt dazu 4 Faktentafeln im Restaurant.

- Das Konzept legt nur Regeln fest. Die konkreten Plätze wählt das [Orgateam](../../CONTEXT.md) vor Ort (bei Umsetzung klären).
- Feste Orte: Die Station fragt vor der Crew-Bildung jede Person einzeln: „Bist du am Fischteich?“ Bei Ja geht es weiter zu „Allein spielen“ oder „Crew bilden“. Bei Nein erscheint „Den Fischteich findest du …, sonst frag jemanden aus dem Orgateam“, und es geht zurück zur Schatzkarte. Ein Nein ist kein Abbruch und wird nicht geloggt ([Karte und Stationen](../framework/karte-und-stationen.md)).
- Der Ortsname ist fester Text in der App, etwa „am Fischteich“. Wo das ist, erklärt die Einleitung in den Abend ([Rahmen der Feier](../rahmen/rahmen-der-feier.md)).
- Die Laufwege zählen zu den 5 Minuten je Spiel. Deshalb liegen die Bereiche eines Spiels an festem Ort nur wenige Schritte auseinander. Ausnahme sind die Faktentafeln von Seemannsgarn, die im Gastraum verteilt stehen.
- Kapazität ist kein Problem: Es spielen nur wenige Crews gleichzeitig, und wer warten müsste, spielt erst eine andere Station.
- Zurücksetzen: Die Stationen bleiben unbetreut. Das Orgateam sieht 2 bis 3 Mal am Abend nach dem Material und hat Ersatzfische, Ersatzkarten sowie Notizblock und Stift dabei. Am Fischteich zählt es die Fische und ersetzt fehlende. Die App hat keine Meldefunktion; Spieler sprechen das Orgateam direkt an.

## Reserve-Ideen

Nicht eingeplant, aufbewahrt als Ersatz, falls ein Spiel ausfällt:

- [Objekte merken](reserve/objekte-merken.md): Wimmelbild kurz zeigen, danach die gesehenen Objekte anklicken (früher Navigator, Aufgabe A).

## Spielübergreifende offene Punkte

1. Zeitbudget: höchstens 5 Minuten je Spiel, höchstens 30 Minuten insgesamt. Bei den Spielen mit Material im Raum zählt die Laufzeit mit.