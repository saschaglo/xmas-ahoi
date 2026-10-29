# 09 Stack und Projektstruktur

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: [Echtzeit in Nuxt](02-echtzeit-in-nuxt.md), [MySQL in Nuxt](03-mysql-in-nuxt.md), [Spielmodul-Schnittstelle](06-spielmodul-schnittstelle.md), [Gerätefragen der Spiele](14-geraetefragen-der-spiele.md)

## Question

Mit welchem Stack und welcher Ordnerstruktur wird `project/` gebaut? Nuxt-Version und Rendering (SSR mit Hydration, wo nötig nur Client), ORM, UI-Bibliothek oder eigenes CSS, Zustand im Client, Ort der Spielmodule im Repo, Teststrategie (Vitest für Spielregeln wie Punkte, Takt und Idempotenz) und die Zuordnungstabelle Fachbegriff → Code-Name.

## Kontext

- Anforderung: reaktiv, schlank, Hydration nutzen (siehe Karte, Notes).
- Ergebnis als Seite `concepts/technik/architektur.md`.

## Answer

Festgehalten in [Architektur](../../../concepts/technik/architektur.md). Der Weg dorthin:

- Drizzle 0.45.4 statt Kysely, weil Kyselys Migrator `GET_LOCK()` braucht; statt Drizzle 1.0 RC, weil nur stabile Grundfunktionen gebraucht werden und ein RC bis zur Feier wandern kann.
- Nuxt UI nur unter der Bedingung des Veranstalters, dass Seiten mit eigenem Look möglich sind und nichts nach Baukasten aussieht: Bedienelemente ja, Seiten-Blöcke nein, Theme über CSS-Variablen.
- Pinia vorerst nicht, bleibt aber Option, falls der Zustand im Handy komplex wird.
- Playwright auf Wunsch des Veranstalters dazugenommen (Vorschlag war: der Testlauf am 11.11. reicht). Daraus folgt die stellbare Server-Uhr, weil `page.clock` nur den Browser stellt.
- Code-Werte englisch statt der deutschen Werte in der Schnittstelle (`'raetsel'`, `'beendet'`); die Typen dort sind umgestellt.
- Bibliotheken je Spiel (Leaflet, SortableJS) nicht hier gewählt: Der Veranstalter baut in Etappen, erst ein MVP mit Demo-Spielen, dann ein Spiel je Etappe mit eigener Wahl.
- Demo-Spiele ohne Mechanik, nur Knöpfe und Debug-Anzeige; sie bleiben dauerhaft für Tests. Auswahl über zwei Stationstabellen `demo`/`live` statt versteckter Stationen auf der Schatzkarte.
- Kein ADR: keine Entscheidung ist zugleich schwer umkehrbar und ohne Kontext überraschend.
