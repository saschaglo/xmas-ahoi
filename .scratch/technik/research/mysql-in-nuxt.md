# Recherche: MySQL in Nuxt

Stand: 28.10.2026.

Frage aus [Ticket 03](../issues/03-mysql-in-nuxt.md): Womit greift eine Nuxt-4-/Nitro-App auf MySQL zu, und was brauchen die Spielregeln von der Datenbank? Rahmen aus der [Karte](../map.md): ein Pod, eine MySQL-Datenbank, Redis nur wenn unvermeidbar, rund 60 Spieler.

Diese Seite sammelt Fakten und Vergleiche. Sie trifft keine Entscheidung. Jede Aussage hat einen Link auf die Primärquelle. Wo die Quelle Quellcode ist, wurde das npm-Paket der genannten Version gelesen. Nicht prüfbare Punkte sind mit **[nicht verifiziert]** markiert. Abschnitte, die nur gelten, wenn die Datenbank auf dem Percona XtraDB Cluster (PXC) des Clusters liegt, sind mit **[falls die DB auf PXC liegt]** markiert; das ist noch nicht bestätigt.

## 1. Das Wichtigste in Kürze

- **Prisma:** `npm install prisma` liefert heute Prisma ORM 8 (Release Candidate), und Prisma 8 kann **kein MySQL** ([Supported databases](https://www.prisma.io/docs/orm/supported-databases): MySQL „Coming soon, planned next“). MySQL geht nur mit Prisma 7, das ausdrücklich gepinnt werden muss.
- **Drizzle:** Das npm-Tag `latest` ist 0.45.4, die Doku beschreibt aber schon 1.0 (RC, Installation mit `@rc`). Drizzle hat `.for('update')`, `onDuplicateKeyUpdate`, `.ignore()` und Isolationsstufen für MySQL; der Migrator setzt **keine** Sperre gegen parallele Läufe.
- **Kysely:** Query-Builder ohne Schema-DSL, mit `forUpdate()`, `onDuplicateKeyUpdate`, `ignore()`. Der Migrator sperrt unter MySQL mit `GET_LOCK()`; genau diese Funktion lehnt PXC im Standardmodus ab.
- **MySQL-Standard:** InnoDB läuft mit `REPEATABLE READ`; `SELECT … FOR UPDATE` sperrt nur in einer Transaktion, die Sperren fallen beim Commit. Deadlocks sind normal und brauchen einen Wiederholversuch.
- **nuxt-auth-utils:** Die Session steckt komplett im versiegelten Cookie, ohne Server-Speicher. Sie übersteht Pod-Neustarts, solange `NUXT_SESSION_PASSWORD` gleich bleibt. Serverseitig widerrufen lässt sie sich ohne eigene Prüfung nicht.
- **Ein Pod ist nicht immer einer:** Mit der Standard-Strategie `RollingUpdate` läuft beim Deploy kurz ein zweiter Pod neben dem alten. Das betrifft Zähler im Speicher und Migrationen beim Start.

## 2. Versionen und Status

Abgefragt am 28.10.2026 über die npm-Registry (`registry.npmjs.org/<paket>`).

| Paket | `latest` | Weitere Tags / Status | Quelle |
|-------|----------|-----------------------|--------|
| `nuxt` | 4.6.3 (27.10.2026) | bringt `nitropack` ^2.13.5 und `h3` ^1.15.11 über `@nuxt/nitro-server` 4.6.3 mit | [npm nuxt](https://registry.npmjs.org/nuxt), [npm @nuxt/nitro-server](https://registry.npmjs.org/@nuxt/nitro-server) |
| `nitropack` | 2.13.5 | Nitro 3 heißt `nitro` (3.0.260903-beta), Nuxt 4.6 nutzt noch Nitro 2 | [npm nitropack](https://registry.npmjs.org/nitropack), [npm nitro](https://registry.npmjs.org/nitro) |
| `drizzle-orm` | 0.45.4 (8.10.2026) | `rc` = 1.0.0-rc.5 (21.10.2026), `beta` = 1.0.0-beta.22 | [npm drizzle-orm](https://registry.npmjs.org/drizzle-orm) |
| `drizzle-kit` | 0.31.11 | `beta` = 1.0.0-beta.22 | [npm drizzle-kit](https://registry.npmjs.org/drizzle-kit) |
| `prisma` (CLI) | **8.0.0-rc.28** (27.10.2026) | `prev` = 7.10.1 | [npm prisma](https://registry.npmjs.org/prisma) |
| `@prisma/client` | 7.10.1 (15.10.2026) | | [npm @prisma/client](https://registry.npmjs.org/@prisma/client) |
| `@prisma/adapter-mariadb` | 7.10.1 | MySQL-Adapter für Prisma 7 | [npm](https://registry.npmjs.org/@prisma/adapter-mariadb) |
| `kysely` | 0.29.7 (17.10.2026) | `next` = 0.30.0-beta.2; `engines.node` >= 22 | [npm kysely](https://registry.npmjs.org/kysely) |
| `mysql2` | 3.24.6 | | [npm mysql2](https://registry.npmjs.org/mysql2) |
| `nuxt-auth-utils` | 0.5.33 (27.10.2026) | hängt an `h3` ^1.15.4 und `@adonisjs/hash` | [npm nuxt-auth-utils](https://registry.npmjs.org/nuxt-auth-utils) |

Status der Projekte laut eigener Doku:

- **Drizzle:** Die Doku installiert mit `drizzle-orm@rc` / `drizzle-kit@rc` ([Get started MySQL](https://orm.drizzle.team/docs/get-started-mysql)). Der Upgrade-Leitfaden auf v1 nennt neue Migrationsordner ohne `journal.json`, Relational Queries v2 und `npx drizzle-kit up` zum Umstellen ([Upgrade v1](https://orm.drizzle.team/docs/upgrade-v1)). Die Startseite zeigt „v1.0 98%“ ([Overview](https://orm.drizzle.team/docs/overview)).
- **Prisma:** „Prisma ORM 8 is a release candidate“, GA „expected in October 2026“; Prisma 7 bekommt „bug fixes and security updates for 18 months from the day Prisma ORM 8 reaches general availability“ ([Release status](https://www.prisma.io/docs/orm/release-status)). MySQL und MariaDB stehen bei Prisma 8 auf „Coming soon“; „Prisma ORM 7 supports all of them“ ([Supported databases](https://www.prisma.io/docs/orm/supported-databases)).
- **Kysely:** weiterhin 0.x ([npm kysely](https://registry.npmjs.org/kysely)).

## 3. Drizzle, Prisma 7 und Kysely im Vergleich

| | Drizzle ORM | Prisma ORM 7 | Kysely |
|---|---|---|---|
| Art | ORM mit Schema in TypeScript | ORM mit eigener Schema-Sprache (`schema.prisma`) und generiertem Client | Typsicherer Query-Builder |
| MySQL-Treiber | `mysql2` über `drizzle-orm/mysql2` ([Doku](https://orm.drizzle.team/docs/get-started-mysql)) | `mariadb` über `@prisma/adapter-mariadb`, für „MySQL (5.6+) or MariaDB (10.0+)“ ([Doku](https://www.prisma.io/docs/orm/v7/core-concepts/supported-databases/mysql)) | `mysql2` über `MysqlDialect` ([Doku](https://kysely.dev/docs/getting-started)) |
| Woher die Typen kommen | aus den Tabellendefinitionen im Code | aus `prisma generate`; seit v7 ist `output` Pflicht, kein Client mehr in `node_modules` ([Upgrade v7](https://www.prisma.io/docs/guides/upgrade-prisma-orm/v7)) | aus einem handgeschriebenen `Database`-Interface oder aus `kysely-codegen` per Introspektion ([Doku](https://kysely.dev/docs/getting-started)) |
| Migrationen erzeugen | `drizzle-kit generate` erzeugt SQL aus dem Schema ([Doku](https://orm.drizzle.team/docs/migrations)) | `prisma migrate dev` (nur Entwicklung, mit Shadow-DB) ([Doku](https://www.prisma.io/docs/orm/v7/prisma-migrate/workflows/development-and-production)) | von Hand als TS-Datei mit `up`/`down`, auf `Kysely<any>` ([Doku](https://kysely.dev/docs/migrations)) |
| Migrationen anwenden | `drizzle-kit migrate` oder zur Laufzeit `migrate(db)` aus `drizzle-orm/mysql2/migrator` ([Doku](https://orm.drizzle.team/docs/migrations)) | `prisma migrate deploy` per CLI ([Doku](https://www.prisma.io/docs/orm/v7/prisma-migrate/workflows/development-and-production)) | `new Migrator({ db, provider }).migrateToLatest()` im Code, optional CLI `kysely-ctl` ([Doku](https://kysely.dev/docs/migrations)) |
| Sperre gegen parallele Migrationen | keine (Quellcode 0.45.4 und 1.0.0-rc.5: Tabelle `__drizzle_migrations`, kein Lock) | Advisory Lock mit 10 s Timeout über MySQLs Locking-Funktionen, abschaltbar per `PRISMA_SCHEMA_DISABLE_ADVISORY_LOCK` ([Doku](https://www.prisma.io/docs/orm/v7/prisma-migrate/workflows/development-and-production)) | `GET_LOCK()` auf MySQL (Quellcode `dialect/mysql/mysql-adapter.js`, 0.29.7); „parallel calls are executed serially“ ([Doku](https://kysely.dev/docs/migrations)) |
| `SELECT … FOR UPDATE` | `.for('update' \| 'share', { noWait \| skipLocked })` (Typen `LockStrength`, `LockConfig` in `mysql-core/query-builders/select.types.d.ts`, 0.45.4) | keine API; offener Feature-Wunsch seit 2021, Umweg `tx.$queryRaw\`… FOR UPDATE\`` ([Issue #8580](https://github.com/prisma/prisma/issues/8580)) | `forUpdate()`, `forShare()`, `noWait()`, `skipLocked()` (`select-query-builder.d.ts`, 0.29.7) |
| `INSERT IGNORE` | `.ignore()` (`mysql-core/query-builders/insert.d.ts`) | keine eigene API **[nicht verifiziert]**; Raw SQL möglich | `ignore()` (`insert-query-builder.d.ts`) |
| `ON DUPLICATE KEY UPDATE` | `.onDuplicateKeyUpdate({ set })`; „no-op“ über `set: { id: sql\`id\` }` (JSDoc in `insert.d.ts`) | `upsert` vorhanden; wie es auf MySQL in SQL übersetzt wird **[nicht verifiziert]** | `onDuplicateKeyUpdate({...})` (`insert-query-builder.d.ts`) |
| Transaktion mit Isolationsstufe | `db.transaction(cb, { isolationLevel, accessMode, withConsistentSnapshot })` (`mysql-core/session.d.ts`); Savepoints über verschachtelte Transaktionen ([Doku](https://orm.drizzle.team/docs/transactions)) | `$transaction(async tx => …, { maxWait: 2000, timeout: 5000, isolationLevel })`, MySQL kann alle vier Stufen ([Doku](https://www.prisma.io/docs/orm/v7/prisma-client/queries/transactions)) | `db.transaction().setIsolationLevel(…).execute(cb)` (`kysely.d.ts`) |
| JSON-Spalte | `json()` mit `.$type<T>()`; schreibt `JSON.stringify` (`mysql-core/columns/json.js`) | `Json`-Feld **[nicht verifiziert für v7 MySQL]** | `JSONColumnType<T>` (`util/column-type.d.ts`); `mysql2` parst JSON-Spalten selbst (`lib/parsers/text_parser.js`) |
| Größe / Kaltstart | „exactly 0 dependencies“, Grafik „31kb“ ([Overview](https://orm.drizzle.team/docs/overview)) | Prisma 7: „~90% smaller bundle sizes“ gegenüber v6 durch den Rust-freien Client ([Changelog 7.0.0](https://www.prisma.io/changelog/2025-11-19)) | keine Angabe gefunden |
| Laufzeit-Voraussetzungen | keine Angabe gefunden | Node >= 20.19, TS >= 5.4, `"type": "module"` ([Upgrade v7](https://www.prisma.io/docs/guides/upgrade-prisma-orm/v7)) | Node >= 22 (`package.json` 0.29.7) |

Messwerte zu Bundle-Größe oder Kaltstart im Nitro-Preset `node-server` haben die drei Projekte nicht veröffentlicht. **[nicht verifiziert]:** Ein Vergleich bräuchte einen eigenen Probe-Build. Bei einem Pod, der einmal startet und dann läuft, spielt der Kaltstart nur beim Deploy eine Rolle.

### Was jede Option für diese App hieße

| Option | Bedeutung für Xmas Ahoi |
|--------|-------------------------|
| Drizzle 0.45 (`latest`) | Stabile Version, aber die Doku zeigt schon 1.0; Beispiele aus der Doku passen nicht immer. Upgrade auf 1.0 während der Umsetzung (2.–10.11.) wäre ein Umbau der Migrationsordner. |
| Drizzle 1.0 RC | Doku und Version passen zusammen; RC-Status heißt mögliche Änderungen bis zur Feier. |
| Prisma 7 | Version muss gepinnt werden (`prisma@7`, `@prisma/client@7`), weil `latest` auf 8 zeigt. Zusätzlicher Schritt `prisma generate`, eigener Treiber (`mariadb` statt `mysql2`). Zeilensperren nur über Raw SQL. Läuft 18 Monate nach Prisma-8-GA weiter mit Fixes. |
| Kysely | Kein Schema im Code; Typen von Hand oder per Codegen gegen eine laufende DB. Migrationen von Hand. Der eingebaute Migrator scheitert, falls die DB auf PXC im Modus `ENFORCING` liegt (siehe Abschnitt 9). |

## 4. Migrationen in Kubernetes

### Fakten, die jede Variante betreffen

- **DDL committet implizit.** `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`, `CREATE INDEX` und weitere „implicitly end any transaction active in the current session“ ([MySQL 8.4: Implicit commit](https://dev.mysql.com/doc/refman/8.4/en/implicit-commit.html)). Eine Migration aus mehreren DDL-Schritten lässt sich auf MySQL also nicht als Ganzes zurückrollen, auch wenn das Tool sie in eine Transaktion legt (Drizzle tut das, siehe `mysql-core/dialect.js` in 0.45.4).
- **Nitro wartet nicht auf Plugins.** Nitro-2-Plugins laufen „synchronously (by order of file name) on the first Nitro initialization“ ([Nitro v2: Plugins](https://v2.nitro.build/guide/plugins)). Im Quellcode ruft `nitropack` 2.13.5 jedes Plugin mit `plugin(nitroApp)` auf, ohne `await` (`dist/runtime/internal/app.mjs`, Zeile 157 ff.). Eine asynchrone Migration in einem Plugin läuft also, während der Server schon Anfragen annimmt.
- **Nitro-Tasks:** `nitro task run` geht nur bei laufendem Dev-Server; in Produktion lässt sich ein Task per `runTask()` aus Code starten ([Nitro v2: Tasks](https://v2.nitro.build/guide/tasks)).
- **Zwei Pods beim Deploy.** `RollingUpdate` ist Standard; `maxSurge` ist 25 % und wird aufgerundet, `maxUnavailable` ist 25 % und wird abgerundet ([K8s: Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)). Abgeleitet: Bei `replicas: 1` ergibt das 1 zusätzlichen Pod und 0 nicht verfügbare, der neue Pod startet also neben dem alten. Mit `Recreate` gilt dagegen: „All existing Pods are killed before new ones are created“ (dieselbe Quelle).

### Wo die Migration laufen kann

| Ort | Wie | Eigenschaften laut Quelle | Bedeutung für die App |
|-----|-----|---------------------------|------------------------|
| Beim App-Start im Code | Drizzle `migrate(db)` oder Kysely `migrateToLatest()` vor dem Start des Servers oder in einem Nitro-Plugin | Plugins werden nicht abgewartet (oben). Drizzle empfiehlt für `migrate` eine einzelne Verbindung statt Pool ([Doku](https://orm.drizzle.team/docs/get-started-mysql)) | Kein zusätzliches K8s-Objekt. Ohne Abwarten Anfragen auf alter Struktur möglich; bei `RollingUpdate` kann der alte Pod noch auf der neuen Struktur laufen. Readiness-Probe kann den Pod aus dem Verkehr halten, bis er fertig ist: „Readiness probes determine when a container is ready to accept traffic“ ([K8s: Probes](https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/)). |
| initContainer im selben Pod | eigener Container vor der App, gleiches oder eigenes Image | „run to completion before app containers start“, laufen nacheinander, werden bei Fehler neu gestartet, Code soll idempotent sein ([K8s: Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)) | App startet erst nach erfolgreicher Migration. Braucht im Image das Migrations-Werkzeug (z. B. `drizzle-kit` oder `prisma` CLI) oder ein kleines Skript. Bei `RollingUpdate` läuft der alte Pod währenddessen weiter. |
| Kubernetes-Job | eigener Job vor dem Rollout | „A Job creates one or more Pods and will continue to retry execution of the Pods until a specified number of them successfully terminate“ ([K8s: Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/)) | Trennt Migration und Deploy; braucht eine Reihenfolge im Deploy-Ablauf (Job zuerst, dann Rollout). Prisma empfiehlt `migrate deploy` als Teil der CI/CD-Pipeline ([Doku](https://www.prisma.io/docs/orm/v7/prisma-migrate/workflows/development-and-production)). |

Prisma: Ob die `prisma` CLI im Laufzeit-Image liegen muss, sagt die Doku nicht ausdrücklich **[nicht verifiziert]**; `migrate deploy` ist ein CLI-Befehl, braucht sie also dort, wo er läuft.

## 5. Nebenläufigkeit: Transaktionen und Zeilensperren

### MySQL-Grundlagen

- Standard-Isolationsstufe von InnoDB ist `REPEATABLE READ` ([MySQL 8.4: Isolation levels](https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html)); `transaction_isolation` hat den Standardwert `REPEATABLE-READ` ([Server system variables](https://dev.mysql.com/doc/refman/8.4/en/server-system-variables.html)).
- Unter `REPEATABLE READ` lesen normale `SELECT` den Schnappschuss des ersten Lesens; sperrende Lesezugriffe (`FOR UPDATE`, `FOR SHARE`, `UPDATE`, `DELETE`) sperren bei eindeutiger Suche über einen Unique-Index nur den gefundenen Indexeintrag, bei Bereichssuchen auch Lücken (Gap/Next-Key-Locks). Die Doku rät davon ab, sperrende und nicht sperrende Lesezugriffe in derselben Transaktion zu mischen ([Isolation levels](https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html)).
- `SELECT … FOR UPDATE` sperrt die gelesenen Zeilen „the same as if you issued an UPDATE statement“; die Sperren gelten bis Commit oder Rollback; „Locking reads are only possible when autocommit is disabled“, also nur in einer Transaktion. `NOWAIT` bricht sofort mit Fehler ab, `SKIP LOCKED` lässt gesperrte Zeilen weg ([MySQL 8.4: Locking reads](https://dev.mysql.com/doc/refman/8.4/en/innodb-locking-reads.html)).
- `innodb_lock_wait_timeout` ist standardmäßig 50 Sekunden ([InnoDB parameters](https://dev.mysql.com/doc/refman/8.4/en/innodb-parameters.html)).
- Deadlocks: „Always be prepared to re-issue a transaction if it fails due to deadlock. Deadlocks are not dangerous. Just try again.“ Dazu: Transaktionen kurz halten, Zeilen immer in derselben Reihenfolge anfassen, passende Indizes ([MySQL 8.4: Deadlocks](https://dev.mysql.com/doc/refman/8.4/en/innodb-deadlocks-handling.html)).

### Die Rennen aus den Spielregeln

Die Regeln stehen in [Crews](../../../concepts/framework/crews.md) und [Hinweise, Abbruch und Exit](../../../concepts/framework/hinweise-abbruch-exit.md). Die Muster unten sind Beispiele, wie die Datenbank-Bausteine darauf passen, keine Festlegung des Datenmodells.

| Regel | Rennen | Baustein in MySQL |
|-------|--------|-------------------|
| Maximal 3 Spieler; zwei treten gleichzeitig dem letzten Platz bei, „der erste gewinnt“ | Zwei Anfragen lesen beide „2 belegt“ und schreiben beide | Muster A: in einer Transaktion die Zeile des Durchlaufs mit `SELECT … FOR UPDATE` sperren, Mitglieder zählen, eintragen, Commit. Die zweite Anfrage wartet auf die Sperre und sieht danach 3 ([Locking reads](https://dev.mysql.com/doc/refman/8.4/en/innodb-locking-reads.html)). Muster B ohne explizite Sperre: bedingtes `UPDATE … SET belegt = belegt + 1 WHERE … AND belegt < 3` und die Anzahl betroffener Zeilen prüfen (0 heißt „Die Crew ist voll“); das `UPDATE` sperrt die Zeile selbst ([Isolation levels](https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html)). |
| „Wer gerade in einer anderen Station spielt, kann nicht beitreten“; „Jeder Spieler beendet jede Station genau einmal“ | Doppelter Beitritt über zwei Geräte oder Tabs | Sperre auf der Zeile des Spielers in derselben Transaktion, oder ein Unique-Index, der den Zustand abbildet (Abschnitt 6). |
| 2 Hinweise je Crew, nach jedem 60 s Sperre für alle | Zwei Personen tippen gleichzeitig „Hinweis nehmen“ | Zeile des Durchlaufs sperren, Zähler und Zeitpunkt des letzten Hinweises prüfen, schreiben; zusätzlich Unique-Index auf (Durchlauf, Hinweisnummer) als zweite Sicherung. |
| Exit braucht Zustimmung aller; Höchstzeit beendet automatisch | Zustimmung und Zeitablauf oder Abbruch treffen gleichzeitig ein | Endzustand des Durchlaufs nur über bedingtes `UPDATE … WHERE status = 'läuft'` setzen; wer 0 Zeilen trifft, kam zu spät. |

Mit einem Pod und 60 Spielern sind die Sperren sehr kurz und selten umkämpft. Abgeleitet aus der Deadlock-Empfehlung oben: Jede Transaktion mit `FOR UPDATE` braucht trotzdem einen Wiederholversuch bei Fehler 1213 (`ER_LOCK_DEADLOCK`).

## 6. Idempotenz: genau einmal zählen

Die Regel „Hinweise, Lösungen und das Bergen der Truhe werden serverseitig genau einmal gezählt (idempotent), auch bei Doppelklicks oder Wiederholungen“ steht in [Hinweise, Abbruch und Exit](../../../concepts/framework/hinweise-abbruch-exit.md).

### Bausteine

| Baustein | Verhalten laut MySQL-Doku | Fallstricke |
|----------|---------------------------|-------------|
| `UNIQUE`-Index / Primärschlüssel | Ein doppelter Wert bricht ein normales `INSERT` mit Duplicate-Key-Fehler ab ([INSERT](https://dev.mysql.com/doc/refman/8.4/en/insert.html)) | Die App muss den Fehler als „schon erledigt“ behandeln. |
| `INSERT IGNORE` | „the row is discarded and no error occurs. Ignored errors generate warnings instead.“ ([INSERT](https://dev.mysql.com/doc/refman/8.4/en/insert.html)) | `IGNORE` macht auch andere Fehler zu Warnungen: `NULL` in `NOT NULL`, Werte außerhalb des Bereichs, zu lange Strings werden abgeschnitten, ungültige Datumswerte ([INSERT](https://dev.mysql.com/doc/refman/8.4/en/insert.html)). |
| `INSERT … ON DUPLICATE KEY UPDATE` | Betroffene Zeilen: 1 = neu eingefügt, 2 = bestehende geändert, 0 = bestehende unverändert ([ON DUPLICATE KEY](https://dev.mysql.com/doc/refman/8.4/en/insert-on-duplicate.html)) | Bei mehreren Unique-Indizes auf einer Tabelle „avoid using ON DUPLICATE KEY UPDATE“; `VALUES()` ist veraltet, stattdessen Zeilen-Alias `AS new`; erhöht den Auto-Increment-Zähler auch ohne Einfügen ([ON DUPLICATE KEY](https://dev.mysql.com/doc/refman/8.4/en/insert-on-duplicate.html)). |

Die Anzahl betroffener Zeilen sagt der App, ob dieser Aufruf der erste war (1) oder eine Wiederholung (0). Damit kann sie Punkte nur beim ersten Mal abziehen oder gutschreiben.

### Zuordnung zu den Spielregeln (Beispiele)

| Ereignis | Möglicher Eindeutigkeitsschlüssel | Folge bei Doppelklick |
|----------|-----------------------------------|-----------------------|
| Hinweis nehmen | (Durchlauf, Hinweisnummer 1 oder 2) | Zweiter Insert trifft den Schlüssel, Crew zahlt nur einmal 25 Punkte. |
| Lösung einer Teilaufgabe | (Durchlauf, Spieler, Teilaufgabe) | Lösung zählt einmal. |
| Station beendet | (Spieler, Station, Ergebnis „beendet“) oder Statusfeld mit bedingtem `UPDATE` | „Jeder Spieler beendet jede Station genau einmal“ ([Crews](../../../concepts/framework/crews.md)). |
| Schatz bergen | (Spieler) in der Tabelle der Bergungen | Spieler landet einmal im Lostopf. |

## 7. JSON-Spalten für spielspezifischen Zustand

- MySQL hat einen nativen Typ `JSON`: ungültiges JSON wird beim Schreiben abgelehnt, Speicherung im Binärformat, Größe begrenzt durch `max_allowed_packet` ([MySQL 8.4: JSON](https://dev.mysql.com/doc/refman/8.4/en/json.html)).
- JSON-Spalten lassen sich nicht direkt indizieren; der Weg sind generierte Spalten mit Index oder Multi-Valued-Indizes für Arrays ([JSON](https://dev.mysql.com/doc/refman/8.4/en/json.html)).
- Teil-Updates mit `JSON_SET()`, `JSON_REPLACE()`, `JSON_REMOVE()` können in-place laufen, wenn der neue Wert nicht größer ist ([JSON](https://dev.mysql.com/doc/refman/8.4/en/json.html)).
- `mysql2` liefert JSON-Spalten als geparste Objekte, außer mit der Option `jsonStrings` (`lib/parsers/text_parser.js`, 3.24.6). Drizzle serialisiert beim Schreiben mit `JSON.stringify` (`mysql-core/columns/json.js`, 0.45.4).

Für die App heißt das: Der Zustand eines Spielmoduls (z. B. gezogene Fragen, Fänge, Wortliste) passt in eine JSON-Spalte je Durchlauf oder Teilaufgabe, solange niemand danach sucht oder sortiert. Werte, die für Sperren, Eindeutigkeit oder die Rangliste gebraucht werden (Status, Hinweiszähler, Punkte), brauchen echte Spalten, weil Unique-Indizes und `FOR UPDATE` an Spalten hängen.

## 8. Verbindungspool bei einem Pod

| Stelle | Standardwert | Quelle |
|--------|--------------|--------|
| `mysql2` `createPool` | `connectionLimit` 10, `waitForConnections` true, `queueLimit` 0 (unbegrenzt), `maxIdle` 10, `idleTimeout` 60000 ms, `enableKeepAlive` true | [mysql2 Doku](https://sidorares.github.io/node-mysql2/docs#using-connection-pools), `lib/pool_config.js` |
| `mariadb` (Prisma-7-Adapter) | `connectionLimit` 10 (`lib/config/pool-options.js`, 3.5.4); Prisma-Beispiel setzt 5 | [Prisma MySQL](https://www.prisma.io/docs/orm/v7/core-concepts/supported-databases/mysql) |
| Prisma 7 allgemein | „Driver adapters use the connection pool settings from the underlying Node.js database driver“ | [Upgrade v7](https://www.prisma.io/docs/guides/upgrade-prisma-orm/v7) |
| MySQL-Server | `max_connections` 151, `wait_timeout` 28800 s | [Server system variables](https://dev.mysql.com/doc/refman/8.4/en/server-system-variables.html) |

Eine Transaktion belegt eine Verbindung, bis sie endet. Bei 60 Spielern und kurzen Transaktionen reicht der Standard von 10 nach Einschätzung; eine Messung gibt es nicht **[nicht verifiziert]**. Wie viele Verbindungen die bereitgestellte Datenbank pro Benutzer erlaubt, ist offen **[nicht verifiziert]**.

## 9. Falls die DB auf PXC liegt

**[falls die DB auf PXC liegt]** Im Cluster läuft der Percona Operator für XtraDB Cluster (Namespace `syseleven-pxc-operator`, HAProxy-Pods `pxc-db-haproxy-0..2`, betrieben von SysEleven). Ob die App-Datenbank dort liegt, welche PXC-Version läuft und welcher `pxc_strict_mode` gesetzt ist, ist **nicht bestätigt**. Zitiert wird die Doku zu PXC 8.4 und, wo gleich, 8.0. PXC 8.0.46-38 vom 23.07.2026 ist laut Release Notes die letzte 8.0-Version, MySQL 8.0 ist EOL ([Release Notes 8.0.46-38](https://docs.percona.com/percona-xtradb-cluster/8.0/release-notes/8.0.46-38.html)).

### Was PXC einschränkt

| Thema | Aussage der Doku | Quelle |
|-------|------------------|--------|
| Standardmodus | „By default, PXC Strict Mode is set to ENFORCING“, außer Standalone oder Bootstrap | [Strict mode 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/strict-mode.html) |
| Primärschlüssel | „All tables must have a primary key.“ Bei `ENFORCING` oder `MASTER` setzt PXC `sql_require_primary_key` auf `ON`; `CREATE`/`ALTER TABLE` ohne Primärschlüssel scheitert, `DELETE` geht auf Tabellen ohne Primärschlüssel nicht | [Limitations 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/limitation.html), [Strict mode 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/strict-mode.html) |
| Explizite Sperren | `LOCK TABLES`, `GET_LOCK()`/`RELEASE_LOCK()`, `FLUSH TABLES … WITH READ LOCK` und das Setzen von `SERIALIZABLE` gelten als „tech-preview“; unter `ENFORCING` gilt: „any undesirable operation is denied and an error is logged“. `MASTER` prüft das nicht | [Strict mode 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/strict-mode.html) |
| Lock-Funktionen allgemein | „Lock functions, such as GET_LOCK() and RELEASE_LOCK(), are not supported.“ | [Limitations 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/limitation.html) |
| `CREATE TABLE … AS SELECT` | unter `ENFORCING` nicht unterstützt | [Strict mode 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/strict-mode.html) |
| Speicher-Engine | nur InnoDB wird repliziert | [Limitations 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/limitation.html) |
| XA | „XA transactions are not supported“ | [Limitations 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/limitation.html) |
| Transaktionsgröße | begrenzt durch `wsrep_max_ws_rows` und `wsrep_max_ws_size` | [Limitations 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/limitation.html) |

### Transaktionen und `FOR UPDATE` über mehrere Knoten

- PXC prüft Konflikte optimistisch auf Cluster-Ebene: Ändern zwei Transaktionen dieselben Zeilen auf verschiedenen Knoten, committet nur eine; die andere bekommt „Error: 1213 SQLSTATE: 40001 (ER_LOCK_DEADLOCK)“ ([Limitations 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/limitation.html)). Der Fehler kommt dann beim Commit, nicht beim Sperren.
- „the row locks of SELECT … FOR UPDATE are not replicated“; „Whichever node commits first will win, the other transaction will hit the deadlock error and will be rolled back.“ Empfehlung: „use only one writer node for these types of transactions“ ([Percona-Blog 2014](https://www.percona.com/blog/2014/09/11/openstack-users-shed-light-on-percona-xtradb-cluster-deadlock-issues/); Hersteller-Blog, älter, aber das Verhalten bestätigt die aktuelle Limitations-Seite).
- `wsrep_retry_autocommit` (Standard 1) wiederholt nur Autocommit-Anweisungen bei Zertifizierungsfehlern, keine expliziten Transaktionen ([wsrep-Variablen 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/wsrep-system-index.html)).
- `wsrep_sync_wait` ist standardmäßig 0, also keine Kausalitätsprüfung beim Lesen ([wsrep-Variablen 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/wsrep-system-index.html)).
- PXC 8.0.46-38 behebt PXC-5099: `SELECT … FOR UPDATE SKIP LOCKED` konnte unter Last einen fatalen InnoDB-Fehler auslösen und den Knoten abstürzen lassen ([Release Notes 8.0.46-38](https://docs.percona.com/percona-xtradb-cluster/8.0/release-notes/8.0.46-38.html)).

### Wohin HAProxy schreibt

Laut Operator-Doku (Version 1.20.0):

- Der Service `<cluster>-haproxy` auf Port 3306 „points to the first Percona XtraDB Cluster member (cluster1-pxc-0), when this member is available“, sonst absteigend die anderen; er taugt für Lesen und Schreiben oder nur für Schreiben („single writer mode“) ([HAProxy-Konfiguration](https://docs.percona.com/percona-operator-for-mysql/pxc/haproxy-conf.html)).
- Der Service `<cluster>-haproxy-replicas` verteilt Round Robin auf alle Knoten: „Don't use it for write requests“; er ist standardmäßig an ([HAProxy-Konfiguration](https://docs.percona.com/percona-operator-for-mysql/pxc/haproxy-conf.html)).

Abgeleitet: Verbindet sich die App nur mit dem Service `…-haproxy` (nicht `…-replicas`), landen alle Schreibzugriffe auf einem Knoten, und `FOR UPDATE` verhält sich dort wie auf einem einzelnen MySQL. Bei einem Failover auf einen anderen Knoten können laufende Transaktionen trotzdem mit 1213 abbrechen. Welchen Service SysEleven der App gibt, ist **nicht bestätigt**.

### DDL und Migrationen

- `wsrep_OSU_method` ist standardmäßig `TOI`: jeder Knoten führt die DDL an derselben Stelle im Replikationsstrom aus, „All other writes must wait until the DDL statement is executed“ ([Online schema upgrade 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/online-schema-upgrade.html), [wsrep-Variablen 8.4](https://docs.percona.com/percona-xtradb-cluster/8.4/wsrep-system-index.html)). Für kleine Tabellen vor der Feier ist das kurz; während des Spiels blockiert eine DDL alle Schreibzugriffe im Cluster.

### Folgen für die Werkzeuge

| Werkzeug | Berührungspunkt mit PXC | Quelle |
|----------|--------------------------|--------|
| Kysely `Migrator` | ruft auf MySQL `select get_lock(…)` auf; unter `ENFORCING` abgelehnt. Abgeleitet: Migration mit dem eingebauten Migrator scheitert dann. Eigene Migrationstabellen haben Primärschlüssel (`kysely_migration.name`, `kysely_migration_lock.id`) | `dialect/mysql/mysql-adapter.js`, `migration/migrator.js` (0.29.7), [Strict mode](https://docs.percona.com/percona-xtradb-cluster/8.4/strict-mode.html) |
| Prisma 7 `migrate deploy` | Advisory Lock über MySQLs Locking-Funktionen (Doku verlinkt die `GET_LOCK`-Seite). Abgeleitet: unter `ENFORCING` abgelehnt; abschaltbar über `PRISMA_SCHEMA_DISABLE_ADVISORY_LOCK`. Tabelle `_prisma_migrations` mit Primärschlüssel **[nicht verifiziert]** | [Prisma Migrate](https://www.prisma.io/docs/orm/v7/prisma-migrate/workflows/development-and-production) |
| Drizzle `migrate` | kein `GET_LOCK`; `__drizzle_migrations` wird mit `id serial primary key` angelegt | `mysql-core/dialect.js` (0.45.4) |
| Alle | jede App-Tabelle braucht einen Primärschlüssel; `SERIALIZABLE` als Isolationsstufe scheidet unter `ENFORCING` aus | [Strict mode](https://docs.percona.com/percona-xtradb-cluster/8.4/strict-mode.html) |

Ob der Benutzer der App DDL-Rechte hat, entscheidet auch, ob Migrationen aus der App heraus laufen können **[nicht verifiziert]**.

## 10. Sessions

### nuxt-auth-utils

| Thema | Fakt | Quelle |
|-------|------|--------|
| Speicherort | „secured & sealed cookies sessions“ | [README](https://github.com/atinux/nuxt-auth-utils) |
| Server-Speicher | keiner: h3 `useSession` liest das versiegelte Cookie (oder den Header `x-<name>-session`), entsiegelt mit dem Passwort und legt die Daten nur in `event.context` ab; `updateSession` schreibt das ganze versiegelte Objekt zurück ins Cookie | `h3` 1.15.11, `dist/index.mjs` (`getSession`, `updateSession`, `sealSession`) |
| Pod-Neustart | Abgeleitet aus der Zeile darüber: Sessions überleben Neustart und Deploy, solange `NUXT_SESSION_PASSWORD` gleich bleibt; ein neues Passwort macht alle Cookies ungültig | wie oben |
| Passwort | `NUXT_SESSION_PASSWORD` mit mindestens 32 Zeichen; im Dev erzeugt das Modul eines; fehlt es in Produktion, schreibt das Modul nur `console.error` (was h3 danach beim Versiegeln mit leerem Passwort tut: **[nicht verifiziert]**) | [README](https://github.com/atinux/nuxt-auth-utils), `runtime/server/utils/session.js` (0.5.33) |
| Größe | „constrained by the 4096-byte cookie size limit“ | [README](https://github.com/atinux/nuxt-auth-utils) |
| Standard-Cookie | Name `nuxt-session`, `sameSite: 'lax'`; h3 ergänzt `path: '/'`, `secure: true`, `httpOnly: true` | [README](https://github.com/atinux/nuxt-auth-utils), `h3` `DEFAULT_COOKIE` |
| Ablauf | Ohne `maxAge` setzt h3 kein `expires`, das Cookie ist ein Sitzungs-Cookie des Browsers. Mit `maxAge` zählt die Zeit ab `createdAt` der Session, nicht ab der letzten Anfrage | `h3` `updateSession`, `unsealSession` |
| API | `setUserSession` (mischt), `replaceUserSession`, `getUserSession`, `requireUserSession` (401 ohne `user`), `clearUserSession` | [README](https://github.com/atinux/nuxt-auth-utils) |
| Geheime Felder | `/api/_auth/session` gibt alles außer `secure` an den Browser | `runtime/server/api/session.get.js` |
| Abmelden / Widerruf | `clearUserSession` leert nur das Cookie des aufrufenden Browsers. Eine Kopie des Cookies bleibt gültig bis `maxAge`; Widerruf braucht eine eigene Prüfung gegen die DB. Der Hook `sessionHooks.hook('fetch')` läuft nur für `/api/_auth/session` und `useUserSession().fetch()`, nicht bei `requireUserSession` | [README, „Extend Session“](https://github.com/atinux/nuxt-auth-utils), `runtime/server/utils/session.js` |
| Admin-Login mit Passwort | `hashPassword` / `verifyPassword` mit scrypt, `passwordNeedsRehash`; scrypt-Optionen über `nuxt.config.ts` | [README](https://github.com/atinux/nuxt-auth-utils), `runtime/server/utils/password.js` (nutzt `@adonisjs/hash`) |
| Voraussetzung | „only works with a Nuxt server running“, nicht mit `nuxt generate` | [README](https://github.com/atinux/nuxt-auth-utils) |

### Was das für die App hieße

- **Spieler:** Das Cookie hält nur die Spieler-ID; der Spiel-State liegt in der DB, wie in [Eingang und Bordkarte](../../../concepts/framework/eingang-und-bordkarte.md) verlangt („kehrt die Person über ihren Spiel-State dorthin zurück, wo sie war“). Ein erneuter QR-Scan setzt das Cookie neu, auch auf einem anderen Handy.
- **Admin:** Login mit Passwort und `verifyPassword`, das Ergebnis als Feld in der Session. Wer einen Admin sofort aussperren will, braucht einen Abgleich mit der DB bei jeder Admin-Anfrage.
- Ohne `maxAge` hängt die Lebensdauer am Browser; mit `maxAge` endet die Session hart nach der eingestellten Zeit ab Login.

### Alternativen

| Alternative | Kern | Quelle |
|-------------|------|--------|
| h3 `useSession` direkt | dieselbe versiegelte Cookie-Session ohne Modul; nuxt-auth-utils baut darauf auf | [README, Konfiguration](https://github.com/atinux/nuxt-auth-utils), `h3` `useSession` |
| Session-Tabelle in MySQL | Cookie hält nur eine zufällige ID, Daten und Widerruf in der DB | eigenes Muster, keine Primärquelle |

## 11. Rate-Limit ohne Redis

Anforderung: „ein Rate-Limit pro Code oder Gerät, nicht pro IP-Adresse, weil im Firmen-WLAN alle dieselbe IP haben“ ([Eingang und Bordkarte](../../../concepts/framework/eingang-und-bordkarte.md)).

| Ort des Zählers | Fakten | Trade-offs für die App |
|-----------------|--------|------------------------|
| MySQL-Tabelle | Zeile je Schlüssel (Code oder Gerät) mit Zähler und Fensterbeginn; Hochzählen in einem Schritt mit `INSERT … ON DUPLICATE KEY UPDATE`, das 1 oder 2 als betroffene Zeilen meldet ([ON DUPLICATE KEY](https://dev.mysql.com/doc/refman/8.4/en/insert-on-duplicate.html)) | Überlebt Neustarts und den zweiten Pod beim Deploy; ein Schreibzugriff je Versuch. Auf PXC eine Tabelle mit Primärschlüssel, sonst nichts Besonderes. |
| Speicher im Prozess (`Map`) | verschwindet mit dem Prozess | Kein I/O. Neustart setzt Zähler zurück; während `RollingUpdate` zählen zwei Pods getrennt (Abschnitt 4). |
| Nitro-Storage `memory` | „Default storage is in memory“; andere Treiber werden über `storage` gemountet ([Nitro v2: Storage](https://v2.nitro.build/guide/storage)) | Wie `Map`, nur mit Storage-API. |
| unstorage-Treiber `db0` auf MySQL | unterstützt `mysql` über db0-Connector, legt Tabelle `unstorage` selbst an; „Database driver is experimental“; TTL nicht dokumentiert ([unstorage db0](https://unstorage.unjs.io/drivers/database)). Nitros eigene DB-Schicht braucht `experimental.database` ([Nitro v2: Database](https://v2.nitro.build/guide/database)) | Zweiter DB-Zugang neben dem ORM, experimentell. Ob die automatisch angelegte Tabelle einen Primärschlüssel hat (PXC), ist **[nicht verifiziert]**. |
| Modul `nuxt-security`, `rateLimiter` | zählt nach IP (optional `ipHeader`), Standard-Treiber `lruCache`; die Doku nennt ihn „not suitable for complex production applications“ ([nuxt-security Rate Limiter](https://nuxt-security.vercel.app/middleware/rate-limiter)) | Schlüssel nach IP widerspricht der Anforderung; ein eigener Schlüssel-Callback ist nicht dokumentiert. |

Zur Wahl des Schlüssels (Einordnung, keine Quelle): Ein Zähler „pro Code“ bremst das Durchprobieren vieler verschiedener Codes nicht, weil jeder Code nur einmal versucht wird. Ein Zähler „pro Gerät“ hängt an einer Kennung im Cookie, die ein Angreifer löschen kann. Beide zusammen oder ein globaler Zähler für Fehlversuche sind Varianten, die das spätere Ticket abwägen kann.

## 12. Offen und nicht verifiziert

- Ob die App-DB auf dem PXC liegt, welche PXC-Version und welcher `pxc_strict_mode` aktiv sind, welchen HAProxy-Service die App bekommt.
- Rechte des App-Benutzers (DDL ja/nein) und erlaubte Verbindungen.
- Bundle-Größe und Startzeit der drei Werkzeuge im Nitro-Preset `node-server` (keine veröffentlichten Messungen gefunden).
- Prisma 7: Übersetzung von `upsert` auf MySQL, `Json`-Feld unter MySQL, Primärschlüssel von `_prisma_migrations`, ob `migrate deploy` unter PXC `ENFORCING` ohne `PRISMA_SCHEMA_DISABLE_ADVISORY_LOCK` scheitert (abgeleitet, nicht getestet).
- Ob die Drizzle-Doku-Beispiele für MySQL (`onDuplicateKeyUpdate`, `.for()`) auch in 1.0 RC unverändert sind; geprüft wurden die Typen in 0.45.4 und der Migrator in 1.0.0-rc.5.
