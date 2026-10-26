# Minispiele: spielübergreifende Punkte

Quelle: Trilium „Minispiele – Übersicht“ · Stand 23.10.2026 · [Übersicht](../README.md)

Sechs Minispiele sind vorgesehen und beschrieben. Jede Spielseite enthält Beschreibung, Variationen, Requisiten, eine Aufbau-Empfehlung sowie offene Fragen und Widersprüche. Hier stehen nur die spielübergreifenden Punkte. Begriffe wie „Crew“, „Rätsel“ und „Punktespiel“ stehen im [Glossar](../../CONTEXT.md).

## Spiele

1. [Spiel 1 – Quiz](spiel-1-quiz/quiz.md): Fragen aus einem Pool, nach Crewgröße skaliert, nur am Handy.
2. [Spiel 2 – Navigator](spiel-2-navigator/navigator.md): Objekte merken, Position bestimmen, dritte Aufgabe offen; nur am Handy (optional ein Blatt).
3. [Spiel 3 – Morsezeichen](spiel-3-morsezeichen/morsezeichen.md): Wort per Licht oder Ton entschlüsseln; nur am Handy.
4. [Spiel 4 – Knoten machen](spiel-4-knoten/knoten.md): Seile, Karten und Anleitungen im Raum plus Startwort aus der App.
5. [Spiel 5 – Begriffe finden](spiel-5-begriffe-finden/begriffe-finden.md): Verschlüsselte Hinweise führen zu festen Orten und Begriffen im Raum.
6. [Spiel 6 – Kurs setzen](spiel-6-kurs-setzen/kurs-setzen.md): Codes aus Logbuchkarten führen auf einer Seekarte am Kartentisch zu einem Feld.

## Spieltypen und Punkte

Jedes Spiel hat einen festen Spieltyp:

| Spieltyp | Spiele | Ergebnis | Punkte (0 bis 100) |
|----------|--------|----------|--------------------|
| Rätsel | Morsezeichen, Knoten, Begriffe finden | gelöst oder Exit | gelöst: 100 minus 25 je genommenem Hinweis; Exit: 0 |
| Punktespiel | Quiz, Navigator, Kurs setzen | Trefferquote | Anteil der Treffer an allen Aufgaben, mal 100 |

- Jede beendete Station gibt den Schlüssel, auch mit 0 Punkten. Ein Punktespiel ist beendet, wenn alle Aufgaben beantwortet sind oder die Zeit abgelaufen ist.
- Ein Abbruch gibt weder Schlüssel noch Punkte, auch wenn bis dahin Treffer erzielt wurden. Die Station wird neu gespielt ([Abbruch](../framework/hinweise-abbruch-exit.md)).
- Alle in der Crew bekommen dieselben Punkte. Weil die Mengen mit der Crewgröße wachsen, zählt die Quote, nicht die absolute Trefferzahl; so sind Einzelspieler und Crews vergleichbar.
- Kein Zeitbonus: Wer ein Rätsel ohne Hinweis löst, bekommt 100 Punkte, egal wie schnell.
- Rätsel haben kein Zeitlimit, dafür Hinweise und Exit. Punktespiele haben ein Zeitlimit, aber weder Hinweise noch Exit ([Hinweise, Abbruch und Exit](../framework/hinweise-abbruch-exit.md)).
- Jedes Spiel braucht genug Varianten (Fragen, Wörter, Bilder, Orte), damit eine Crew nach einem Abbruch neue Aufgaben bekommt.

## Crewgröße und Teilaufgaben

Alle Spiele skalieren nach derselben Regel mit der Crew:

- Jede Person spielt auf dem eigenen Handy eine eigene [Teilaufgabe](../../CONTEXT.md) (eigene Fragen, eigenes Wort, eigener Knoten). Eine Crew aus n Personen hat n Teilaufgaben. Wie groß eine Teilaufgabe ist, legt die Seite des Spiels fest.
- Der Server zieht die Teilaufgaben ohne Wiederholung innerhalb der Crew. Nach einem Abbruch bekommt ein Spieler an dieser Station nichts, was er dort schon hatte. Die nötige Poolgröße legt die Seite des Spiels fest.
- Rätsel: Die Station ist beendet, wenn alle Teilaufgaben gelöst sind. Wer fertig ist, sieht „Warte auf deine Crew“ und darf den anderen helfen. Verlassen lässt sich die Station nur über Exit oder Abbruch.
- Punktespiel: Die Crew hat ein gemeinsames Zeitlimit ab dem gemeinsamen Start, gleich lang wie für Einzelspieler. Ausnahme ist Kurs setzen: Dort wächst das Zeitlimit mit der Crew, weil die Codes über die Handys verteilt sind. Die Station ist beendet, wenn alle ihre Teilaufgaben beantwortet haben oder die Zeit abgelaufen ist.
- Varianten, die auf Verständigung zwischen Personen setzen (zum Beispiel Navigator Aufgabe A mit Fragen bei einem anderen Mitglied), spielen Einzelspieler in der Grundform auf dem eigenen Handy.
- Hinweise bei Rätseln gelten je Crew, ihr Text je Person ([Hinweise, Abbruch und Exit](../framework/hinweise-abbruch-exit.md)).

## Weitergabe von Lösungen

Lösungen lassen sich in der Firma weitergeben (Chat, Zuruf, Foto). Die Schatzsuche schützt sich dagegen nur durch Zufall und nimmt den Rest als Risiko hin:

- In allen Spielen zieht der Server jede Teilaufgabe zufällig aus einem Pool (Fragen, Startwort, Wort, Ort); Kurs setzen erzeugt seine Codes jedes Mal neu. Ziel ist, dass sich zwei Crews selten überschneiden; eine Garantie gibt es nicht. Die Poolgröße legt die Seite des Spiels fest.
- Keine technische Erkennung (zum Beispiel Tab-Wechsel oder verdächtig schnelle Lösungen) und keine Sanktionen. Bei Preisen bis 20 EUR lohnt der Aufwand nicht, und Erkennung im Browser ist unzuverlässig.
- Beim Check-in bittet ein Satz darum, Lösungen nicht weiterzugeben ([Eingang und Bordkarte](../framework/eingang-und-bordkarte.md)).
- Was trotzdem möglich bleibt, steht unter [Mogeln](../framework/betrieb-datenschutz-risiken.md#bewusst-akzeptierte-risiken) in den akzeptierten Risiken.

## Spiele mit Material im Raum

Knoten und Kurs setzen laufen an einem festen Ort mit Material, dem Knotentisch und dem Kartentisch. Begriffe finden nutzt Schilder an Orten in der ganzen Location.

- Das Konzept legt nur Regeln fest. Die konkreten Plätze wählt das [Orgateam](../../CONTEXT.md) vor Ort (bei Umsetzung klären).
- Feste Orte: Die Station fragt vor der Crew-Bildung jede Person einzeln: „Bist du am Knotentisch?“ Bei Ja geht es weiter zu „Allein spielen“ oder „Crew bilden“. Bei Nein erscheint „Den Knotentisch findest du …, sonst frag jemanden aus dem Orgateam“, und es geht zurück zur Schatzkarte. Ein Nein ist kein Abbruch und wird nicht geloggt ([Karte und Stationen](../framework/karte-und-stationen.md)).
- Der Ortsname ist fester Text in der App, etwa „am Knotentisch“. Wo das ist, erklärt die Einleitung in den Abend ([Rahmen der Feier](../rahmen/rahmen-der-feier.md)).
- Begriffe finden zeigt keinen Wegweiser; den Ort zu finden ist die Aufgabe. Für die Orte gibt es vorab keine Ausschlüsse, auch Toiletten sind möglich.
- Die Laufwege zählen zu den 5 Minuten je Spiel. Deshalb liegen die Bereiche eines Spiels an festem Ort nur wenige Schritte auseinander.
- Kapazität ist kein Problem: Es spielen nur wenige Crews gleichzeitig, und wer warten müsste, spielt erst eine andere Station.
- Zurücksetzen: Die Stationen bleiben unbetreut. Das Orgateam sieht 2 bis 3 Mal am Abend nach dem Material und hat Ersatzseile und Ersatzschilder dabei. Die App hat keine Meldefunktion; Spieler sprechen das Orgateam direkt an.

## Spielübergreifende offene Punkte

1. Zeitbudget: höchstens 5 Minuten je Spiel, höchstens 30 Minuten insgesamt. Bei den Spielen mit Material im Raum zählt die Laufzeit mit.