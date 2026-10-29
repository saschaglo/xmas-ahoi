# 06 Spielmodul-Schnittstelle

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: [Durchlauf-Modell und Uhren](05-durchlauf-modell-und-uhren.md)

## Question

Welchen Vertrag haben Basis und Spielmodul? Was stellt die Basis (Crew, Start, Uhren, Hinweis-Takt, Exit, Abbruch, Log, Ziehen aus Pools ohne Wiederholung), und was macht jedes Modul in sich geschlossen (Teilaufgaben erzeugen, Antworten prüfen, Punkte berechnen, eigene Oberfläche, eigener Zustand)? Wie wird ein Modul registriert und ausgetauscht, und wie spielt Morsezeichen mit dem gemeinsamen Funkspruch statt Teilaufgaben hinein?

## Kontext

- [Spielübergreifende Punkte](../../../concepts/spiele/uebergreifend.md): Spieltypen, Punkte, Teilaufgaben, Pools.
- Die sechs Spielseiten unter [concepts/spiele/](../../../concepts/spiele/uebergreifend.md#spiele).
- Ergebnis als Seite `concepts/technik/spielmodul-schnittstelle.md` mit Typen.

## Answer

- Zustand: ein JSON-Zustand je Durchlauf; das Modul liefert je Spieler eine Ansicht ohne Lösungen. Morsezeichen ist damit kein Sonderfall.
- Aktionen: Module sind reine Funktionen (`start`, `act`, `hint`, `score`, `view`); Transaktion, Sperre, Idempotenz, Log und Verteilen an die Crew macht die Basis.
- Hinweise: Takt und Zähler in der Basis, Inhalt im Modul; `hint` bekommt Stufe der Person und Zahl der Hinweise der Crew (Morsezeichen).
- Punkte: immer `score` im Modul, nur bei „beendet“; Rätsel über einen gemeinsamen Helfer. Exit und Zeitablauf: 0 durch die Basis.
- Ziehen: `start` meldet gezogene Inhalts-IDs, die Basis liefert sie später als `ctx.seen`.
- Fischteich: `ctx.otherRuns` liefert laufende Durchläufe der Station; die Basis sperrt bei jeder Aktion die Station.
- Flüchtiger Zustand (Sortierung, Pin vor Abgabe) bleibt am Handy, gern im `localStorage` je Durchlauf.
- Registrierung: feste Tabellen im Code, Server und Handy getrennt; Austausch per Deploy.
- Verworfen: Austausch zur Laufzeit im Admin, Nuxt-Layer je Spiel, getrennter Zustand je Teilnahme, Punkte für Rätsel in der Basis.

Eingearbeitet in: neu `concepts/technik/spielmodul-schnittstelle.md`, Verweis in `concepts/technik/README.md`.
