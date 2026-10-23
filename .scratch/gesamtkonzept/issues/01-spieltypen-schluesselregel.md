# 01 Spieltypen, Punkte und Crew je Station

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: –

## Question

Gibt es zwei Spieltypen, „Rätsel mit einer Lösung“ (Morse, Knoten, Begriffe) und „Zeit- oder Punktespiel“ (Quiz, Navigator)? Wenn ja: Wann gibt es bei einem Zeit- oder Punktespiel den Schlüssel (Beenden genügt, oder Mindestquote mit Wiederholung)? Und wofür zählt das Ergebnis („je mehr richtig, desto besser“), wenn die Ziehung zufällig bleibt: Anzeige, Rangliste oder gar nicht?

Im Grilling erweitert: Der Veranstalter hat die Zufallsziehung durch eine Rangliste nach Punkten ersetzt und das Crewmodell auf „Crew je Station“ umgestellt. Beides hängt an der Schlüsselregel und wurde hier mitentschieden.

## Kontext

- [Spielübergreifend, Spieltypen und Punkte](../../../concepts/spiele/uebergreifend.md#spieltypen-und-punkte)
- [Crews](../../../concepts/framework/crews.md)
- [Siegerehrung](../../../concepts/framework/finale-und-ziehung.md#siegerehrung)

## Answer

- Zwei Spieltypen, fest je Spiel: **Rätsel** (Morsezeichen, Knoten, Begriffe finden) und **Punktespiel** (Quiz, Navigator).
- Jede beendete Station gibt den Schlüssel, unabhängig von den Punkten. Ein Punktespiel ist beendet, wenn alle Aufgaben beantwortet sind oder die Zeit abgelaufen ist.
- Punkte: 0 bis 100 je Station, gleich für alle in der Crew. Punktespiel: Trefferquote mal 100. Rätsel: 100 minus 25 je Hinweis. Exit: Schlüssel, 0 Punkte. Kein Zeitbonus.
- Abbruch: gilt für die ganze Crew, kein Schlüssel, keine Punkte (auch nicht die bis dahin erzielten), Crew aufgelöst, Station wird mit neuen Aufgaben neu gespielt. Abbrüche werden je Spieler gezählt; ein Abzug bleibt dem Veranstalter bei der Siegerehrung von Hand vorbehalten (die Rangliste zeigt nur die Spalte).
- Crew je Station: bildet sich an der Station über einen Code, 1 bis 3 Spieler, löst sich danach auf. Keine Lobby, kein Austreten, keine Anwesenheitsregel, kein „übersprungen“. Der Begriff „Besetzung“ entfällt.
- Eine beendete Station ist gesperrt („Du warst hier schon mal“); jeder Spieler beendet jede Station genau einmal.
- Verbindungsverlust: Der Server misst die Aktivität der Crew-Mitglieder; fällt jemand aus, sehen alle eine Sperre mit Abbruch als Ausweg. Schwelle und Pausieren der Spielzeit bei der Umsetzung klären.
- Plätze 1 bis 3 nach Punkten je Spieler, Gleichstand per Los. Ehrung „Teamplayer“ (meiste verschiedene Mitspieler), Gleichstand per Los. Alle im Lostopf bekommen einen Trostpreis. Die Ziehung entfällt.
- Rangliste nur zentral bei der Siegerehrung, nicht live.

Eingearbeitet in: `CONTEXT.md`, `crews.md`, `karte-und-stationen.md`, `hinweise-abbruch-exit.md`, `finale-und-ziehung.md`, `grundprinzip.md`, `admin-panel.md`, `rahmen-der-feier.md`, `betrieb-datenschutz-risiken.md`, `uebergreifend.md`, Spielseiten, Wireframe-Seiten (als neu zu zeichnen markiert).
