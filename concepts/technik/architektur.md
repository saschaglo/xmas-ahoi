# Architektur

Stand 29.10.2026 · [Übersicht](README.md)

Womit `project/` gebaut wird, wie der Code liegt, wie getestet wird und in welchen Etappen die Spiele dazukommen. Der Vertrag zwischen Basis und Spielen steht unter [Spielmodul-Schnittstelle](spielmodul-schnittstelle.md).

## Stack

| Bereich | Wahl | Warum |
|---------|------|-------|
| Framework | Nuxt 4.6, SSR mit Hydration für alle Seiten | Schnelle erste Anzeige nach dem QR-Scan; eine Regel auch für das Admin-Panel |
| Laufzeit | Node 26 LTS, npm | LTS seit 28.10.2026, lokal schon installiert |
| Datenbank | Drizzle ORM 0.45.4 (exakt gepinnt) mit `mysql2` | Trägt Transaktionen, `.for('update')` und `onDuplicateKeyUpdate`; Kyselys Migrator ruft `GET_LOCK()` und scheitert auf dem Percona-Cluster. Version 1.0 ist noch RC |
| Migrationen | `drizzle-kit generate` erzeugt SQL, das SQL wird committet | Wie die Migrationen im Cluster laufen, legt der Betrieb fest |
| Session | nuxt-auth-utils | Siehe [Zugang und Session](zugang-und-session.md) |
| Oberfläche | Nuxt UI 4 mit Tailwind 4 | Fertige, zugängliche Bedienelemente; spart vor allem im Admin-Panel Zeit |
| Eingaben prüfen | Zod 4 | Ein Werkzeug für Endpunkte, `payload` der Spiele und Formulare von Nuxt UI |
| Zustand im Handy | `useFetch`, `useState`, ein Composable je laufendem Durchlauf | Der Server ist die Quelle; Pinia erst, wenn der Zustand im Handy komplex wird |
| Qualität | `@nuxt/eslint`, TypeScript strict | |

Bewusst nicht dabei:

- **`nuxt-security`:** setzt `Referrer-Policy: no-referrer` und bricht damit die Nutzungsregeln der OSM-Kacheln; sein Rate-Limit zählt nach IP. Nötige Header kommen über `routeRules`.
- **i18n:** Das Spiel ist nur auf Deutsch.
- **`@nuxt/content`:** Die wenigen Texte stehen in den Vue-Seiten.
- **Bibliotheken für einzelne Spiele** (Karte, Drag & Drop): werden in der Etappe des Spiels gewählt. Die Fakten dazu liegen in der [Recherche zu den Gerätefragen](../../.scratch/technik/research/geraetefragen-der-spiele.md).

## Rendering

- Alle Seiten werden auf dem Server gerendert und im Browser hydriert.
- Was im Server-Rendering nicht laufen kann (Leaflet, SortableJS, Web Audio), steht in einer `.client.vue`-Komponente oder in `<ClientOnly>`.

## Gestaltung

- Nuxt UI liefert nur Bedienelemente: Knopf, Eingabe, Dialog, Toast und im Admin die Tabelle.
- Die Seiten der Spieler (Begrüßung, Schatzkarte, Regelkarten, „Schatz bergen“, Ende-Seite) sind von Hand gebaute Vue-Seiten mit eigenem Layout aus Tailwind und eigenen Komponenten. Die fertigen Seiten-Blöcke von Nuxt UI (`UPageHero`, `UPageSection` und ähnliche) werden nicht benutzt, damit nichts nach Baukasten aussieht.
- Farben, Schrift und Rundungen stehen zentral als CSS-Variablen und in `app.config.ts`. Die Werte kommen aus der Einladung.

## Projektstruktur

```
project/
├── app/              Seiten, Rahmen am Handy, games.ts (id → Vue-Komponente)
├── server/           API, Basis, Taktgeber, games.ts (id → GameModule), stations.ts
├── shared/           Typen der Spielmodul-Schnittstelle, für Server und Handy
├── games/
│   ├── demo/         demo-puzzle und demo-score, bleiben dauerhaft
│   └── <id>/         module.ts, View.vue, module.test.ts
└── tests/e2e/        Playwright
```

- Ein Spiel liegt vollständig in seinem Ordner unter `games/`.
- `module.ts` enthält die Lösungen und wird nur vom Server importiert. Eine ESLint-Regel (`no-restricted-imports`) verbietet den Import aus `app/` und aus `View.vue`, damit keine Lösung ins Bundle des Handys gerät.

## Etappen

1. **MVP:** die ganze Basis mit den Demo-Spielen. Dazu gehören Check-in und Session, Schatzkarte, Crew-Bildung mit Crewcode, Durchlauf mit allen fünf Enden, Fristen, Hinweise, Exit, Abbruch, „Schatz bergen“, Lostopf und so viel Admin, wie ein Testlauf braucht.
2. **Je ein Spiel pro Etappe:** Das Spiel kommt mit eigenen Tests und eigener Wahl der Bibliotheken dazu und ersetzt in der Tabelle `live` eine Station.

### Stationen

Zwei feste Tabellen in `server/stations.ts` ordnen jeder Station ein Spielmodul zu (`stationId → moduleId`). Die Umgebungsvariable `NUXT_STATIONS` wählt eine:

| Tabelle | Stationen | Benutzt von |
|---------|-----------|-------------|
| `demo` | 3 × `demo-puzzle`, 3 × `demo-score` | Vitest, Playwright, Entwicklung |
| `live` | die echten Spiele, bis zu ihrer Etappe Demo-Spiele | Testlauf am 11.11., Feier |

Die Schatzsuche ist damit nach jeder Etappe von Anfang bis Ende spielbar, und die Tests der Basis hängen an keinem echten Spiel.

### Demo-Spiele

Die Demo-Spiele haben keine Spielmechanik. Knöpfe setzen die Zustände, die ein echtes Spiel über seine Mechanik erreicht. Hinweis, Exit und Abbruch liegen im Rahmen der Basis.

| Modul | Typ | Knöpfe | `finished` |
|-------|-----|--------|------------|
| `demo-puzzle` | Rätsel | „Meine Teilaufgabe lösen“; ein Hinweis zeigt „Hinweis 1 für Person X“ nur der Person, die ihn nahm | alle Teilaufgaben gelöst |
| `demo-score` | Punktespiel | „Richtig“ und „Falsch“ für 3 Fragen je Person aus einer kleinen Liste im Code | alle Fragen beantwortet, sonst das Zeitlimit |

- Beide haben einen Knopf „Ungültige Aktion senden“. Er prüft, dass der Zustand gleich bleibt und das Handy einen Fehler zeigt.
- Die Ansicht zeigt den ganzen Zustand als JSON, den Punktestand, den `score` auf dem jetzigen Stand ergäbe, und die gezogenen IDs (`used`, `seen`). Das ist erlaubt, weil Demo-Spiele keine Lösung haben.

### Debug-Leiste

Mit `NUXT_PUBLIC_DEBUG=1` zeigt der Rahmen der Basis bei jedem Spiel eine Leiste: ID und Status des Durchlaufs, Crew, `hintsTaken`, alle Fristen als Uhrzeit und den Abstand der Handy-Uhr zur Uhr des Servers.

## Tests

| Ebene | Werkzeug | Gegen |
|-------|----------|-------|
| Spielmodule | Vitest | reine Funktionen, ohne Server und Datenbank |
| Basis: Sperre, Idempotenz, Fristen, erster Wechsel gewinnt | Vitest | Percona XtraDB Cluster 8.4 mit einem Knoten in Docker (`percona/percona-xtradb-cluster:8.4`), damit der Modus `ENFORCING` gilt |
| Abläufe am Handy | Playwright, Projekte WebKit („iPhone 15“) und Chromium („Pixel 7“) | `nuxt build` mit der Tabelle `demo` und derselben Datenbank in Docker |

Playwright deckt ab:

1. Check-in über den QR-Link.
2. Eine Crew aus 2 bis 3 Spielern, jeder in einem eigenen Browser-Kontext: Crewcode, Start, Aktion, neue Ansicht auf allen Handys, Ende mit Schlüssel.
3. Hinweis, Exit und Abbruch an einem Rätsel.
4. Ein Durchgang über alle 6 Stationen bis „Schatz bergen“.

Ob die Tests auch in einer CI laufen, legt der Betrieb fest.

### Uhr im Test

Die Basis nimmt die aktuelle Zeit immer aus einer Uhr-Funktion; sie ist auch die Quelle von `ctx.now`. Mit `NUXT_TEST_CLOCK=1` gibt es den Endpunkt `/api/_test/clock`, über den ein Test die Uhr des Servers vorstellt, etwa auf das Ende der Höchstzeit. Die Fristen bleiben dabei echt (0:30, 60 Sekunden Sperre, 10 Minuten). `page.clock` von Playwright stellt nur die Uhr des Browsers und reicht allein nicht. In Produktion fehlt der Endpunkt.

## Umgebungsvariablen

| Variable | Werte | Zweck |
|----------|-------|-------|
| `NUXT_STATIONS` | `demo`, `live` | Tabelle der Stationen |
| `NUXT_PUBLIC_DEBUG` | `1` oder leer | Debug-Leiste am Handy |
| `NUXT_TEST_CLOCK` | `1` oder leer | Endpunkt zum Vorstellen der Server-Uhr |
| `NUXT_SESSION_PASSWORD` | mindestens 32 Zeichen | Versiegeln der Session ([Zugang und Session](zugang-und-session.md)) |

## Fachbegriffe im Code

Code, Bezeichner und gespeicherte Werte sind englisch; Deutsch steht nur in Texten für die Spieler. Die Begriffe kommen aus dem [Glossar](../../CONTEXT.md).

| Fachbegriff | Code |
|-------------|------|
| Spieler | `player` |
| Crew, Crew-Bildung | `crew`, `crewFormation` |
| Zugangscode, Crewcode | `accessCode`, `crewCode` |
| Station | `station` |
| Durchlauf, Teilnahme | `run`, `participation` |
| Teilaufgabe | `task` |
| Rätsel, Punktespiel | `puzzle`, `scoreGame` |
| läuft, beendet | `running`, `finished` |
| Exit, Zeitablauf, Abbruch, verfallen | `exit`, `timeout`, `abort`, `expired` |
| Hinweis | `hint` |
| Höchstzeit, Zeitlimit | `maxDuration`, `timeLimit` |
| Schlüssel, Punkte | `key`, `points` |
| Log, Ereignis | `log`, `event` |
| Inhalte eines Spiels | `pool` |
| Lostopf, Ziehung | `lottery`, `draw` |
| Rangliste, Ehrung | `ranking`, `honor` |
| Funkspruch, Geschichte, Faktentafel | `message`, `story`, `factBoard` |
| Bordkarte, Schatzkarte | `boardingPass`, `treasureMap` |

Der Lostopf heißt `lottery`, weil `pool` schon die Inhalte der Spiele meint.

## Offene Punkte

- Keine.
