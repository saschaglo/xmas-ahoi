# Im Spiel: Hinweise, Abbruch und Exit

Quelle: Trilium „Spielkonzept – Ablauf der mobilen Webseite“, Abschnitt „Im Spiel: Hinweise, Abbruch und Exit“, überarbeitet auf Crew je Station · Stand 23.10.2026 · [Übersicht](../README.md)

- Je Station gibt es 2 gestufte Hinweise. Ein genommener Hinweis gilt für die ganze Crew und kostet bei einem Rätsel 25 Punkte (siehe [Spielübergreifend](../spiele/uebergreifend.md#spieltypen-und-punkte)).
- Mindestzeiten (serverseitig): Der erste Hinweis ist frühestens nach 1:30 verfügbar, der zweite 1:30 nach dem ersten, der Exit 1:00 nach dem zweiten, also frühestens nach 4:00.
- Abbruch: Jede Person kann eine Station abbrechen; das gilt für die ganze Crew. Niemand bekommt Schlüssel oder Punkte, die Crew löst sich auf, und die Station bleibt offen. Sie wird später neu gespielt, mit neuen Aufgaben und in beliebiger Crew. Abbrüche werden je Spieler gezählt; der Veranstalter behält sich vor, bei der Siegerehrung Punkte dafür abzuziehen.
- Exit: Nach den 2 Hinweisen und der Mindestzeit ist ein Exit möglich. Er braucht die Zustimmung aller in der Crew (Einzelspieler brauchen keine). Wer nicht innerhalb von 60 Sekunden antwortet, lehnt ab. Der Exit gibt den Schlüssel und 0 Punkte; die Station gilt als beendet.
- Hinweise, Lösungen und das Bergen der Truhe werden serverseitig genau einmal gezählt (idempotent), auch bei Doppelklicks oder Wiederholungen.
- Hinweise, Exits und Abbrüche werden geloggt und sind im [Admin-Panel](../admin/admin-panel.md) sichtbar.
- Das Ende eines Spiels ist nur das Ergebnis; es gibt kein Puzzlestück, das für ein Endspiel nötig wäre.

Die Mindestzeiten passen nicht zu allen Minispielen; siehe [spielübergreifende offene Punkte](../spiele/uebergreifend.md#spielübergreifende-offene-punkte).
