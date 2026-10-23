# Im Spiel: Hinweise, Abbruch und Exit

Quelle: Trilium „Spielkonzept – Ablauf der mobilen Webseite“, Abschnitt „Im Spiel: Hinweise, Abbruch und Exit“ · Stand 21.10.2026 · [Übersicht](../README.md)

- Je Station gibt es 2 gestufte Hinweise. Ein genommener Hinweis gilt für die ganze Crew und zählt bei jedem anwesenden Spieler als +1.
- Mindestzeiten (serverseitig): Der erste Hinweis ist frühestens nach 1:30 verfügbar, der zweite 1:30 nach dem ersten, der Exit 1:00 nach dem zweiten, also frühestens nach 4:00.
- Abbrechen: Jede Person kann eine Station abbrechen; das gilt für alle in der Crew. Die Crew kommt zurück zur Karte. Ein Abbruch setzt den Spielstand der Station zurück.
- Exit: Nach den 2 Hinweisen und der Mindestzeit ist ein Exit möglich. Er braucht die Zustimmung aller anwesenden Mitglieder (Einzelspieler brauchen keine). Wer nicht innerhalb von 60 Sekunden antwortet, lehnt ab. Der Exit gibt den Schlüssel, der intern als „Exit“ markiert ist und für den Spieler wie ein gelöster aussieht.
- Hinweise, Lösungen und das Bergen der Truhe werden serverseitig genau einmal gezählt (idempotent), auch bei Doppelklicks oder Wiederholungen.
- Hinweise, Exits und übersprungene Schlüssel werden geloggt und sind im [Admin-Panel](../admin/admin-panel.md) sichtbar, damit später prüfbar ist, wer nur so im Lostopf gelandet ist.
- Das Ende eines Spiels ist nur das Ergebnis; es gibt kein Puzzlestück, das für ein Endspiel nötig wäre.

Die Mindestzeiten passen nicht zu allen Minispielen; siehe [spielübergreifende offene Punkte](../spiele/uebergreifend.md).
