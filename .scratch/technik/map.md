# Karte: Technik Xmas Ahoi

Label: wayfinder:map

## Destination

Technisches Konzept unter `concepts/technik/` bis **So, 1.11.2026 abends**: Stack, Datenbank, Sessions, Echtzeit, Uhren, Spielmodul-Schnittstelle, Admin, Betrieb und Gestaltungsrahmen sind entschieden. Die Umsetzung in `project/` startet am 2.11. ohne offene Grundsatzfragen.

## Notes

- Sprache: Grilling-Fragen, Empfehlungen und Rückfragen an den Nutzer immer auf Deutsch. Code auf Englisch; die Zuordnung der Fachbegriffe (Crew → `crew`, Hinweis → `hint`) steht als Tabelle in `concepts/technik/architektur.md`.
- Skills: Jede Session ruft „grilling“ und „domain-modeling“ auf und nutzt die Begriffe aus dem Glossar [`CONTEXT.md`](../../CONTEXT.md). Das Glossar bleibt frei von Technik.
- Ergebnis: Eine Entscheidung wird beim Abschluss eines Tickets in eine Seite unter `concepts/technik/` geschrieben (Konventionen wie im Konzept: `Stand`-Zeile, Link zur Übersicht, Abschnitt „Offene Punkte“). Das Ticket hält nur den Weg fest. Die erste Session, die eine Seite schreibt, legt `concepts/technik/README.md` an, verlinkt sie aus `concepts/README.md` und ergänzt die Ordnertabelle in `CLAUDE.md`. `project/` bleibt reiner Code.
- ADRs unter `concepts/technik/adr/`, nur wenn eine Entscheidung schwer umkehrbar, ohne Kontext überraschend und Ergebnis einer echten Abwägung ist (Kandidaten: ein Pod ohne Redis, Wahl des Echtzeit-Wegs).
- Tiefe: Architektur und Verträge. Die Spielmodul-Schnittstelle bekommt Typen; das Datenmodell bleibt grob (Spieler, Durchlauf, Ereignis). Tabellen und Endpunkte entstehen bei der Umsetzung.
- Rahmen: Umsetzung 2.–10.11.2026 durch Sascha allein mit Claude, Testlauf 11.11., Feier 20.11. Im Zweifel gewinnt die einfachere Lösung.
- Plattform (gesetzt): vorhandener K8s-Cluster, betrieben von Sascha; eine MySQL-Datenbank mit Zugangsdaten; **ein Pod**; Redis nur, wenn unvermeidbar; WebSocket am Ingress ungeklärt, bis dahin gilt „nein“. Last: rund 60 Spieler.
- Anforderungen des Veranstalters: Spielmodule sind austauschbar und in Ablauf und Punkteberechnung in sich geschlossen, mit einer Schnittstelle zur Basis. Das Spiel reagiert schnell, ist schlank und nutzt Nuxts Hydration; WebSocket nur, wenn es dem Spielen dient. Optik modern, nicht überladen, im Stil der Einladung.
- Research-Ergebnisse liegen unter `research/`. Commits: siehe `CLAUDE.md`, Abschnitt Git.

## Decisions so far

<!-- eine Zeile je geschlossenem Ticket -->

- [Echtzeit in Nuxt: WebSocket, SSE oder Polling](issues/02-echtzeit-in-nuxt.md): WebSocket in Nitro nur experimentell, SSE ohne Flag; Envoy schneidet SSE ohne Routen-Timeout nach 15 s ab, Loadbalancer nach 50 s Leerlauf; Anwesenheit braucht bei jedem Weg einen Herzschlag. Ungetestet, daher neu: [Echtzeit-Probe im Cluster](issues/13-echtzeit-probe-im-cluster.md).
- [MySQL in Nuxt: ORM, Migrationen, Sperren](issues/03-mysql-in-nuxt.md): Prisma fällt praktisch aus (Prisma 8 ohne MySQL); Drizzle und Kysely tragen Sperren und Idempotenz; Percona-Cluster würde Primärschlüssel erzwingen und `GET_LOCK()` sperren; Sessions per versiegeltem Cookie, Rate-Limit in einer MySQL-Tabelle.

## Not yet specified

- Design-System (Farben, Schrift, Komponenten, Ton der Texte): wird ein Ticket, sobald [Einladung als Gestaltungsvorlage liefern](issues/04-einladung-liefern.md) erledigt ist.
- Gerätefragen je Spiel, nur soweit sie Stack oder Schnittstelle berühren: Ton und Licht bei Morsezeichen (Autoplay-Sperre auf iOS), Drag & Drop am Handy, Karte und Kacheln bei Peilung (Nutzungsregeln von OpenStreetMap), Steuerrad bei Fischfang. Wird schärfer nach der [Spielmodul-Schnittstelle](issues/06-spielmodul-schnittstelle.md).

## Out of scope

- Code in `project/` schreiben: Umsetzung ab 2.11.
- Volles Datenmodell (Tabellen) und API-Endpunkte: entstehen beim Bauen mit Tests.
- Inhalte erstellen (Fragen, Fotos, Wortlisten, Hinweistexte).
- Gestaltung und Druck der Bordkarten.
