# Minispiele: spielübergreifende Punkte

Quelle: Trilium „Minispiele – Übersicht“ · Stand 23.10.2026 · [Übersicht](../README.md)

Sechs Minispiele sind vorgesehen; fünf sind beschrieben, das sechste steht aus. Jede Spielseite enthält Beschreibung, Variationen, Requisiten, eine Aufbau-Empfehlung sowie offene Fragen und Widersprüche. Hier stehen nur die spielübergreifenden Punkte. Begriffe wie „Crew“, „Rätsel“ und „Punktespiel“ stehen im [Glossar](../../CONTEXT.md).

## Spiele

1. [Spiel 1 – Quiz](spiel-1-quiz/quiz.md): Fragen aus einem Pool, nach Crewgröße skaliert, nur am Handy.
2. [Spiel 2 – Navigator](spiel-2-navigator/navigator.md): Objekte merken, Position bestimmen, dritte Aufgabe offen; nur am Handy (optional ein Blatt).
3. [Spiel 3 – Morsezeichen](spiel-3-morsezeichen/morsezeichen.md): Wort per Licht oder Ton entschlüsseln; nur am Handy.
4. [Spiel 4 – Knoten machen](spiel-4-knoten/knoten.md): Seile, Karten und Anleitungen im Raum plus Startwort aus der App.
5. [Spiel 5 – Begriffe finden](spiel-5-begriffe-finden/begriffe-finden.md): Verschlüsselte Hinweise führen zu festen Orten und Begriffen im Raum.
6. [Spiel 6 – noch offen](spiel-6-offen/spiel-6.md).

## Spieltypen und Punkte

Jedes Spiel hat einen festen Spieltyp:

| Spieltyp | Spiele | Ergebnis | Punkte (0 bis 100) |
|----------|--------|----------|--------------------|
| Rätsel | Morsezeichen, Knoten, Begriffe finden | gelöst oder Exit | gelöst: 100 minus 25 je genommenem Hinweis; Exit: 0 |
| Punktespiel | Quiz, Navigator | Trefferquote | Anteil der Treffer an allen Aufgaben, mal 100 |

- Jede beendete Station gibt den Schlüssel, auch mit 0 Punkten. Ein Punktespiel ist beendet, wenn alle Aufgaben beantwortet sind oder die Zeit abgelaufen ist.
- Ein Abbruch gibt weder Schlüssel noch Punkte, auch wenn bis dahin Treffer erzielt wurden. Die Station wird neu gespielt ([Abbruch](../framework/hinweise-abbruch-exit.md)).
- Alle in der Crew bekommen dieselben Punkte. Weil die Mengen mit der Crewgröße wachsen, zählt die Quote, nicht die absolute Trefferzahl; so sind Einzelspieler und Crews vergleichbar.
- Kein Zeitbonus: Wer ein Rätsel ohne Hinweis löst, bekommt 100 Punkte, egal wie schnell.
- Rätsel haben kein Zeitlimit, dafür Hinweise und Exit. Punktespiele haben ein Zeitlimit, aber weder Hinweise noch Exit ([Hinweise, Abbruch und Exit](../framework/hinweise-abbruch-exit.md)).
- Jedes Spiel braucht genug Varianten (Fragen, Wörter, Bilder, Orte), damit eine Crew nach einem Abbruch neue Aufgaben bekommt.
- Den Spieltyp von Spiel 6 legt [Spiel 6](spiel-6-offen/spiel-6.md) fest.

## Crewgröße und Teilaufgaben

Alle Spiele skalieren nach derselben Regel mit der Crew:

- Jede Person spielt auf dem eigenen Handy eine eigene [Teilaufgabe](../../CONTEXT.md) (eigene Fragen, eigenes Wort, eigener Knoten). Eine Crew aus n Personen hat n Teilaufgaben. Wie groß eine Teilaufgabe ist, legt die Seite des Spiels fest.
- Der Server zieht die Teilaufgaben ohne Wiederholung innerhalb der Crew. Nach einem Abbruch bekommt ein Spieler an dieser Station nichts, was er dort schon hatte. Die nötige Poolgröße legt die Seite des Spiels fest.
- Rätsel: Die Station ist beendet, wenn alle Teilaufgaben gelöst sind. Wer fertig ist, sieht „Warte auf deine Crew“ und darf den anderen helfen. Verlassen lässt sich die Station nur über Exit oder Abbruch.
- Punktespiel: Die Crew hat ein gemeinsames Zeitlimit ab dem gemeinsamen Start, gleich lang wie für Einzelspieler. Die Station ist beendet, wenn alle ihre Teilaufgaben beantwortet haben oder die Zeit abgelaufen ist.
- Varianten, die auf Verständigung zwischen Personen setzen (zum Beispiel Navigator Aufgabe A mit Fragen bei einem anderen Mitglied), spielen Einzelspieler in der Grundform auf dem eigenen Handy.
- Hinweise bei Rätseln gelten je Crew, ihr Text je Person ([Hinweise, Abbruch und Exit](../framework/hinweise-abbruch-exit.md)).

## Spielübergreifende offene Punkte

1. Knoten und Begriffe finden brauchen feste Orte, Material und gelegentliche Kontrolle. Das Konzept kennt Stationen bisher nur als Punkte auf der App-Karte. Es fehlt: Location, Platz, Hausrecht, Kapazität bei gleichzeitigem Spiel, Zurücksetzen der Requisiten ohne Betreuung.
2. Zeitbudget: höchstens 5 Minuten je Spiel, höchstens 30 Minuten insgesamt. Bei den Raumspielen zählt die Laufzeit mit.
3. Lösungen lassen sich in der Firma weitergeben (Chat, Zuruf). Zufällige Varianten je Crew (Fragen, Startwort, Ort, Wort) mindern das. Ob das Restrisiko akzeptiert wird, ist offen.