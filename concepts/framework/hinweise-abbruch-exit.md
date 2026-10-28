# Im Spiel: Hinweise, Abbruch und Exit

Stand 23.10.2026 · [Übersicht](../README.md)

- Hinweise und Exit gibt es nur bei Rätseln. Ein Rätsel hat kein Zeitlimit; wer festhängt, nimmt Hinweise und zuletzt den Exit. Ausnahme ist [Morsezeichen](../spiele/spiel-3-morsezeichen/morsezeichen.md) mit einer Höchstzeit von 5 Minuten.
- Je Rätsel hat die Crew höchstens 2 Hinweise, egal ob sie aus 1, 2 oder 3 Personen besteht. Jeder genommene Hinweis kostet die ganze Crew 25 Punkte (siehe [Spielübergreifend](../spiele/uebergreifend.md#spieltypen-und-punkte)).
- Takt (serverseitig, für alle Rätsel gleich): Ab 0:30 nach Start des Durchlaufs kann jede Person der Crew einen Hinweis nehmen. Nach jedem genommenen Hinweis ist der Knopf für alle 60 Sekunden gesperrt. Hinweis 2 kommt also frühestens bei 1:30.
- Den Hinweistext sieht nur die Person, die ihn genommen hat. Er gehört zu ihrer eigenen [Teilaufgabe](../../CONTEXT.md). Die Hinweise sind gestuft (erst vage, dann konkret), und die Stufe zählt je Person: Wer noch keinen Hinweis hatte, bekommt Stufe 1, auch wenn die Crew schon einen genommen hat. Ausnahme ist Morsezeichen: Dort erscheinen die Hinweise für alle in der Crew in der gemeinsamen Wortliste.
- Alle in der Crew sehen den Zähler („Hinweise: 1 von 2“) und die laufende Sperre. Vor dem Nehmen fragt die App nach: „Kostet die Crew 25 Punkte. Hinweis nehmen?“
- Punktespiele enden von selbst: Jede Aufgabe nimmt eine beliebige Antwort an, und jedes Punktespiel hat ein Zeitlimit.
- Abbruch: Jede Person kann eine Station abbrechen; das gilt für die ganze Crew. Niemand bekommt Schlüssel oder Punkte, die Crew löst sich auf, und die Station bleibt offen. Sie wird später neu gespielt, mit neuen Aufgaben und in beliebiger Crew. Abbrüche werden je Spieler gezählt; der Veranstalter behält sich vor, bei der Siegerehrung Punkte dafür abzuziehen.
- Exit: 60 Sekunden nach Hinweis 2 ist ein Exit möglich, also frühestens bei 2:30. Er braucht die Zustimmung aller in der Crew (Einzelspieler brauchen keine). Wer nicht innerhalb von 60 Sekunden antwortet, lehnt ab. Der Exit gibt den Schlüssel und 0 Punkte; die Station gilt als beendet. Bei Morsezeichen folgt bei 5:00 ein automatischer Exit ohne Zustimmung.
- Hinweise, Lösungen und das Bergen der Truhe werden serverseitig genau einmal gezählt (idempotent), auch bei Doppelklicks oder Wiederholungen.
- Hinweise, Exits und Abbrüche werden geloggt und sind im [Admin-Panel](../admin/admin-panel.md) sichtbar.
- Das Ende eines Spiels ist nur das Ergebnis; es gibt kein Puzzlestück, das für ein Endspiel nötig wäre.
