# 14 Admin und Betrieb: Schalter und Löschkonzept

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: –

## Question

Gilt die Annahme zum Schalter „Schatzsuche öffnen/schließen“ (geschlossen: Check-in, Spielen und „Schatz bergen“ gesperrt, alle sehen die Ende-Seite)? Und wann und wie werden Vornamen, Spielstände und Logs nach der Feier gelöscht (Löschkonzept für den Datenschutzbeauftragten)?

## Kontext

- [Admin-Panel, offene Punkte](../../../concepts/admin/admin-panel.md#offene-punkte)
- [Datenschutz, offene Punkte](../../../concepts/framework/betrieb-datenschutz-risiken.md#offene-punkte)

## Answer

- Schalter mit drei Zuständen: _nicht eröffnet_ → _offen_ → _beendet_; Wiederöffnen von _beendet_ ist erlaubt, Spielstände bleiben.
- Nicht offen: Check-in, Spielen und „Schatz bergen“ gesperrt, alle sehen die Ende-Seite (Annahme bestätigt).
- Schließen ist ein harter Schnitt: Laufende Durchläufe verfallen ohne Schlüssel, Punkte und Log-Eintrag. Das Orgateam kündigt das Ende moderiert im Gastraum und persönlich draußen an; kein Banner im Spiel.
- Neu: „Zurücksetzen“ löscht Check-ins, Spielstände, Schlüssel, Punkte und Log, behält Nutzer und Codes (gedruckte Bordkarten), setzt auf _nicht eröffnet_. Nur in _nicht eröffnet_ und _beendet_, zwei Bestätigungen, eine davon durch Eintippen des Datums (YYYY-MM-DD). Testlauf auf eigenen Testkarten, Testspieler danach einzeln löschen.
- Löschkonzept: 14 Tage nach der Feier (bis 4.12.2026) anonyme Zahlen exportieren, dann Seite, Server, Datenbank, Backups und Server-Logs (IP-Adressen) abbauen; Bestätigung per Mail an den Datenschutzbeauftragten.
- Verworfen: laufende Durchläufe zu Ende spielen lassen (Rätsel ohne Zeitlimit, Schatzsuche nie sicher zu), Banner im Spiel, Löschen per Admin-Button mit Seite online, Zurücksetzen inklusive Nutzer.

Eingearbeitet in: `concepts/admin/admin-panel.md`, `concepts/admin/wireframes-admin.md`, `concepts/framework/betrieb-datenschutz-risiken.md`, `concepts/framework/finale-und-ziehung.md`.
