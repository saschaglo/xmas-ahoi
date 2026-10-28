# Spiel 3 – Morsezeichen

Stand 27.10.2026 · [Übersicht](../../README.md) · [Spielübergreifend](../uebergreifend.md)

Die „Crew“ sind die Spieler, die einen Durchlauf der Station gemeinsam spielen, auch eine einzelne Person (siehe [Glossar](../../../CONTEXT.md)). „Beschreibung“ und „Variationen“ sind die Vorgabe des Veranstalters. „Aufbau (Empfehlung)“ und „Offene Fragen“ stammen aus der Durchsicht und sind noch nicht entschieden.

## Beschreibung

Die Crew fängt einen [Funkspruch](../../../CONTEXT.md) ab: einen Satz, dessen Wörter einzeln gemorst werden. Sie entschlüsselt die Wörter und bringt sie in die richtige Reihenfolge. Je schneller der ganze Satz steht, desto besser. Das Spiel steht an der Funkstation; Ort, Ortsabfrage („Bist du an der Funkstation?“) und Nachsehen regelt [Spiele mit Material im Raum](../uebergreifend.md#spiele-mit-material-im-raum).

**An der Funkstation:** Tischkarten mit der Morsetabelle, dazu Notizblock und Stift. Die App zeigt keine Morsetabelle.

**Der Funkspruch:** Jede Crew bekommt einen Satz. Seine Länge wächst mit der Crew; als Startwert gelten 5 Wörter je Person, also 5, 10 oder 15 Wörter. Der Testlauf legt die Längen fest. Der Server zieht den Satz zufällig aus dem Pool der Crewgröße; dass ein Spieler einen Satz zweimal bekommt, auch nach einem Abbruch, ist als Ausnahme erlaubt.

**Wörter entschlüsseln:** Die Wörter des Satzes liegen gemischt auf einem gemeinsamen Stapel. Jedes Handy zeigt ein Wort aus dem Stapel: den Morsecode und die Positionsnummer des Wortes im Satz. Der Code läuft in Endlosschleife. Die Person tippt das Wort ein und schickt es ab. Ist es richtig, wandert es in die Liste und das Handy zeigt das nächste Wort vom Stapel. „Nächstes Wort“ legt das aktuelle Wort zurück auf den Stapel. Wer schneller entschlüsselt, schafft mehr Wörter. Das ist eine Ausnahme von „eine eigene Teilaufgabe je Person“ ([Crewgröße und Teilaufgaben](../uebergreifend.md#crewgröße-und-teilaufgaben)).

- Das Handy zeigt die Morsezeichen als Leuchtfeld auf dem Bildschirm; Ton lässt sich zuschalten. Taschenlampe, Vibration und andere gerätespezifische Kanäle gibt es nicht.
- Ein Regler stellt das Tempo je Person ein: Langsam, Normal (Standard) oder Schnell. Das Tempo ändert die Punkte nicht.
- Versuche sind unbegrenzt. Groß- und Kleinschreibung und Leerzeichen zählen nicht. Die Sätze enthalten keine Umlaute und kein ß; Ä, Ö und Ü stehen als AE, OE und UE, ß als SS.

**Die Liste:** Unten auf jedem Handy steht von Anfang an die Liste aller Wörter des Satzes, unsortiert. Ein ungelöstes Wort erscheint als Platzhalter mit einem Stern je Buchstabe (`*****`). Ein gelöstes Wort ersetzt seinen Platzhalter bei allen in der Crew, aber ohne Positionsnummer. Die Nummer sieht man nur, solange das Wort auf einem Handy läuft; die Crew muss sie notieren.

**Sortieren und Prüfen:** Jede Person sortiert die Liste auf dem eigenen Handy per Drag & Drop, schon während noch Wörter fehlen. „Satz prüfen“ geht erst, wenn alle Wörter gelöst sind, und prüft die Reihenfolge auf dem Handy, das den Knopf drückt. Bei einem Fehler werden die Wörter an der richtigen Stelle grün; Versuche sind unbegrenzt. Ist der Satz richtig, ist die Station beendet und alle sehen den ganzen Funkspruch.

**Hinweise:** Morsezeichen ist ein Rätsel mit 2 Hinweisen je Crew im üblichen Takt ([Hinweise, Abbruch und Exit](../../framework/hinweise-abbruch-exit.md)). Die Hinweise erscheinen als Ausnahme in der Liste und für alle in der Crew:

- Hinweis 1 zeigt in jedem Wort den ersten Buchstaben (`K****`).
- Hinweis 2 zeigt zusätzlich jeden zweiten Buchstaben (`K*R*E`).

**Höchstzeit:** Als einziges Rätsel hat Morsezeichen eine Höchstzeit von 5 Minuten mit sichtbarem Countdown. Bei 5:00 endet die Station mit einem automatischen Exit: Schlüssel, 0 Punkte, ohne Zustimmung der Crew. Vorher gilt der Exit wie bei jedem Rätsel.

**Punkte:** 100 minus 25 je genommenem Hinweis, wie bei jedem Rätsel. Es gibt keinen Zeitbonus; die Geschwindigkeit zählt nur über die Höchstzeit.

## Variationen

- Keine. „Ein Wort je Person“ und „Ein Satz je Person“ sind verworfen, ebenso die Morsetabelle in der App oder auf der Bordkarte.

## Requisiten

- 2 bis 3 laminierte Tischkarten mit der Morsetabelle an der Funkstation, damit jede Person der Crew eine vor sich hat.
- Notizblock und Stift an der Funkstation. Ersatz beim Orgateam.
- Pool mit 10 Sätzen je Crewgröße, also 30 Sätze, mit maritimen und Firmenbegriffen. Die Sätze ohne Bezug zur Handlung der Feier zu schreiben ist ein eigenes Arbeitspaket nach dem Konzept.

## Aufbau (Empfehlung)

Spielscreen von oben nach unten: Countdown und Hinweis-Zähler, Leuchtfeld mit Positionsnummer, Umschalter für Ton und Tempo-Regler, Eingabefeld mit „Abschicken“ und „Nächstes Wort“, darunter die Liste mit „Satz prüfen“.

Der Ton muss im Browser per Tipp gestartet werden. Auf dem iPhone kann der Stummschalter Web-Töne unterdrücken; das wird bei der Umsetzung geprüft. Zwischen zwei Durchläufen eines Wortes liegt eine kurze sichtbare Pause, damit man den Anfang erkennt.

Tempo: Auch „Schnell“ sollte unter 3 Lichtwechseln pro Sekunde bleiben (üblicher Richtwert für lichtempfindliche Menschen, kleine Leuchtfläche). Als Startwerte für den Testlauf etwa 4, 5 und 7 Wörter pro Minute.

Grobe Schätzung: Ein Wort dauert bei Tempo „Normal“ etwa 30 bis 40 Sekunden, mit 2 bis 3 Durchläufen und Eintippen. 5 Wörter je Person passen dann mit dem Sortieren knapp in 5 Minuten. Daran sollte sich der Testlauf orientieren.

## Offene Fragen und Widersprüche

1. Testlauf: Wie viele Wörter je Person passen in 5 Minuten, und welche Werte haben die 3 Tempostufen? Das klärt der Testlauf; bis dahin gelten die Startwerte.
