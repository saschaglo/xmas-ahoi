# Admin-Panel

Quelle: Trilium „Spielkonzept – Ablauf der mobilen Webseite“, Abschnitte „Admin-Panel“ und „Offen und später“ · Stand 23.10.2026 · [Übersicht](../README.md)

- Zugriff über einen Login (Nuxt-Standard), nur für Veranstalter. Bedienbar am Handy und am Desktop; die Erstpflege erfolgt am Rechner.
- Schalter „Schatzsuche öffnen/schließen“. Annahme: Im geschlossenen Zustand sind Check-in, Spielen und „Schatz bergen“ gesperrt, und alle sehen die [Ende-Seite](../framework/finale-und-ziehung.md#ende-seite).
- Nutzer hinzufügen, mit oder ohne Namen; das System vergibt den 4-stelligen Code. Der QR-Code soll dynamisch erzeugbar sein (später). Eine interne ID unterscheidet gleiche Namen (zum Beispiel zwei Peter).
- Nutzer löschen (selten, nur in Abstimmung).
- Rangliste für die Siegerehrung zeigen: Spieler im Lostopf nach Punkten, mit Spalten für Hinweise, Exits und Abbrüche, dazu die Ehrung „Teamplayer“; Regeln unter [Siegerehrung](../framework/finale-und-ziehung.md#siegerehrung). Bei Gleichstand lost das Panel.
- Log für Hinweise, Exits und Abbrüche ansehen.

## Offene Punkte

1. Die Handy-Ansicht des Admin-Panels wird bei der Umsetzung festgelegt.
2. Der Schalter „öffnen/schließen“ ist als Annahme formuliert und noch nicht bestätigt.
