# 05 Durchlauf-Modell und Uhren

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: –

## Question

Wie bildet die Basis einen Durchlauf einer Station ab (Zustände von Crew-Bildung über Bereit und Start bis Ende, Exit, Zeitablauf, Abbruch, Verfall), wie hängt das Log daran (Ereignisse als Quelle des Zustands oder nur als Protokoll), und wie laufen Höchstzeit, Zeitlimit und Hinweis-Takt auf dem Server ab, auch wenn gerade kein Handy verbunden ist?

## Kontext

- [Hinweise, Abbruch und Exit](../../../concepts/framework/hinweise-abbruch-exit.md), [Crews](../../../concepts/framework/crews.md), [Karte und Stationen](../../../concepts/framework/karte-und-stationen.md)
- [Spielstand und Wiedereinstieg](../../../concepts/framework/spielstand-und-wiedereinstieg.md): Zustand auf dem Server, Rückkehr bis ins laufende Spiel.
- [Admin-Panel](../../../concepts/admin/admin-panel.md): Schließen lässt laufende Durchläufe verfallen; Log je Durchlauf.
- [MySQL in Nuxt](03-mysql-in-nuxt.md): Fakten zu Transaktionen und Idempotenz, falls schon vorhanden.

## Answer

- Zustände: Ein Durchlauf beginnt mit dem gemeinsamen Start (allein: „Bin bereit“) und endet als beendet, Exit, Zeitablauf, Abbruch oder verfallen. Die Crew-Bildung davor ist kein Durchlauf und wird nicht geloggt. „Durchlauf“ steht jetzt im Glossar.
- Crew-Bildung: Vor dem Start darf jedes Mitglied über „Zurück“ gehen. Geht der Ersteller, löst sich die Crew für alle auf. Nach 10 Minuten ohne Start löst sie sich von selbst auf. Wer in einer Crew-Bildung steckt, kann nirgends sonst beitreten.
- Log: Protokoll, nicht Quelle. Der Zustand steht in Tabellen; jede Änderung schreibt in derselben Transaktion ein Ereignis.
- Uhren: Fristen als Zeitpunkte am Durchlauf; Taktgeber im Pod alle 2–5 s plus Prüfung bei jeder Anfrage; Log-Zeit ist die Frist; Handys rechnen mit der Serverzeit.
- Gleichzeitigkeit: Der erste Wechsel gewinnt; ein Abbruch während der Exit-Abstimmung beendet als Abbruch.
- Verworfen: Event Sourcing (zu teuer für 9 Tage Umsetzung).

Eingearbeitet in: `CONTEXT.md`, `concepts/framework/crews.md`, `concepts/framework/karte-und-stationen.md`, neu `concepts/technik/README.md` und `concepts/technik/durchlauf-und-uhren.md`, Verweis in `concepts/README.md` und `CLAUDE.md`.
