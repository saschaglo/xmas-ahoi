# Admin-Panel

Quelle: Trilium „Spielkonzept – Ablauf der mobilen Webseite“, Abschnitte „Admin-Panel“ und „Offen und später“ · Stand 23.10.2026 · [Übersicht](../README.md)

- Zugriff über einen Login (Nuxt-Standard), nur für Veranstalter. Bedienbar am Handy und am Desktop; die Erstpflege erfolgt am Rechner.
- Schalter „Schatzsuche öffnen/schließen“ mit drei Zuständen: _nicht eröffnet_ → _offen_ → _beendet_. Von _beendet_ darf der Admin wieder öffnen (etwa nach einem Fehlklick); die Spielstände bleiben dabei erhalten.
- Ist die Schatzsuche nicht offen, sind Check-in, Spielen und „Schatz bergen“ gesperrt, und alle sehen die [Ende-Seite](../framework/finale-und-ziehung.md#ende-seite).
- Schließen ist ein harter Schnitt: Laufende Durchläufe verfallen ohne Schlüssel, ohne Punkte und ohne Eintrag im Log. Das Orgateam kündigt das Ende vorher an, im Gastraum moderiert und auf Terrasse und Hinterhof persönlich (etwa „noch 5 Minuten“). Im Spiel erscheint dazu kein Hinweis.
- „Zurücksetzen“ löscht Check-ins, Spielstände, Schlüssel, Punkte und das Log und stellt den Zustand auf _nicht eröffnet_. Nutzer, Vornamen und Codes bleiben, damit die gedruckten Bordkarten gültig bleiben. Der Button ist nur in _nicht eröffnet_ und _beendet_ aktiv und braucht zwei Bestätigungen; bei einer davon tippt der Admin das heutige Datum ein (YYYY-MM-DD).
- Nutzer hinzufügen, mit oder ohne Namen; das System vergibt den 4-stelligen Code. Der QR-Code soll dynamisch erzeugbar sein (später). Eine interne ID unterscheidet gleiche Namen (zum Beispiel zwei Peter).
- Nutzer löschen (selten, nur in Abstimmung).

## Testlauf

Der Testlauf vor der Feier läuft mit eigenen Testkarten: öffnen, spielen, schließen, zurücksetzen. Danach löscht der Admin die Testspieler einzeln, und die Schatzsuche startet leer im Zustand _nicht eröffnet_.
- Rangliste für die Siegerehrung zeigen: Spieler im Lostopf nach Punkten, mit Spalten für Hinweise, Exits und Abbrüche, dazu die Ehrung „Teamplayer“; Regeln unter [Siegerehrung](../framework/finale-und-ziehung.md#siegerehrung). Bei Gleichstand lost das Panel.
- Log für Hinweise, Exits und Abbrüche ansehen.

## Offene Punkte

1. Die Handy-Ansicht des Admin-Panels wird bei der Umsetzung festgelegt.
