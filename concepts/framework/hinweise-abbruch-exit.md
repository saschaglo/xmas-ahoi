# Im Spiel: Hinweise, Abbruch und Exit

Quelle: Trilium „Spielkonzept – Ablauf der mobilen Webseite“, Abschnitt „Im Spiel: Hinweise, Abbruch und Exit“, überarbeitet auf Crew je Station · Stand 23.10.2026 · [Übersicht](../README.md)

- Hinweise und Exit gibt es nur bei Rätseln. Ein Rätsel hat kein Zeitlimit; wer festhängt, nimmt Hinweise und zuletzt den Exit.
- Je Rätsel gibt es 2 gestufte Hinweise. Ein genommener Hinweis gilt für die ganze Crew und kostet 25 Punkte (siehe [Spielübergreifend](../spiele/uebergreifend.md#spieltypen-und-punkte)).
- Mindestzeiten (serverseitig, für alle Rätsel gleich), gezählt ab Start des Durchlaufs: Hinweis 1 ab 1:30, Hinweis 2 ab 3:00 und erst nach Hinweis 1, Exit ab 4:00 und erst nach Hinweis 2. Wann die Crew die Hinweise nimmt, verschiebt den Exit nicht.
- Punktespiele enden von selbst: Jede Aufgabe nimmt eine beliebige Antwort an, und jedes Punktespiel hat ein Zeitlimit.
- Abbruch: Jede Person kann eine Station abbrechen; das gilt für die ganze Crew. Niemand bekommt Schlüssel oder Punkte, die Crew löst sich auf, und die Station bleibt offen. Sie wird später neu gespielt, mit neuen Aufgaben und in beliebiger Crew. Abbrüche werden je Spieler gezählt; der Veranstalter behält sich vor, bei der Siegerehrung Punkte dafür abzuziehen.
- Exit: Nach beiden Hinweisen und der Mindestzeit ist ein Exit möglich. Er braucht die Zustimmung aller in der Crew (Einzelspieler brauchen keine). Wer nicht innerhalb von 60 Sekunden antwortet, lehnt ab. Der Exit gibt den Schlüssel und 0 Punkte; die Station gilt als beendet.
- Hinweise, Lösungen und das Bergen der Truhe werden serverseitig genau einmal gezählt (idempotent), auch bei Doppelklicks oder Wiederholungen.
- Hinweise, Exits und Abbrüche werden geloggt und sind im [Admin-Panel](../admin/admin-panel.md) sichtbar.
- Das Ende eines Spiels ist nur das Ergebnis; es gibt kein Puzzlestück, das für ein Endspiel nötig wäre.
