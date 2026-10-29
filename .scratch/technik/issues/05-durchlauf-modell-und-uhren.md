# 05 Durchlauf-Modell und Uhren

Type: grilling
Status: open
Assignee: –
Blocked by: –

## Question

Wie bildet die Basis einen Durchlauf einer Station ab (Zustände von Crew-Bildung über Bereit und Start bis Ende, Exit, Zeitablauf, Abbruch, Verfall), wie hängt das Log daran (Ereignisse als Quelle des Zustands oder nur als Protokoll), und wie laufen Höchstzeit, Zeitlimit und Hinweis-Takt auf dem Server ab, auch wenn gerade kein Handy verbunden ist?

## Kontext

- [Hinweise, Abbruch und Exit](../../../concepts/framework/hinweise-abbruch-exit.md), [Crews](../../../concepts/framework/crews.md), [Karte und Stationen](../../../concepts/framework/karte-und-stationen.md)
- [Spielstand und Wiedereinstieg](../../../concepts/framework/spielstand-und-wiedereinstieg.md): Zustand auf dem Server, Rückkehr bis ins laufende Spiel.
- [Admin-Panel](../../../concepts/admin/admin-panel.md): Schließen lässt laufende Durchläufe verfallen; Log je Durchlauf.
- [MySQL in Nuxt](03-mysql-in-nuxt.md): Fakten zu Transaktionen und Idempotenz, falls schon vorhanden.
