# 03 MySQL in Nuxt: ORM, Migrationen, Sperren

Type: research
Status: resolved
Assignee: Sascha Glover
Blocked by: –

## Question

Womit greift eine Nuxt-4-/Nitro-App am besten auf MySQL zu, und was brauchen die Spielregeln von der Datenbank? Verglichen werden Drizzle ORM, Prisma und Kysely: MySQL-Unterstützung, Migrationen und wie sie in K8s laufen (beim Start oder als Job), Transaktionen und Zeilensperren für Rennen wie „letzter freier Platz in der Crew“, Idempotenz über eindeutige Schlüssel (Hinweis, Lösung, Schatz bergen genau einmal), JSON-Spalten für spielspezifischen Zustand, Verbindungspool bei einem Pod. Dazu: Session-Bausteine für Nuxt (zum Beispiel nuxt-auth-utils mit versiegeltem Cookie) und ob sie Pod-Neustarts ohne Server-Speicher überstehen; wo ein Rate-Limit ohne Redis seinen Zähler hält.

## Kontext

- [Crews](../../../concepts/framework/crews.md): „Die Crew ist voll“ bei gleichzeitigem Beitritt.
- [Hinweise, Abbruch und Exit](../../../concepts/framework/hinweise-abbruch-exit.md): idempotente Zählung.
- [Eingang und Bordkarte](../../../concepts/framework/eingang-und-bordkarte.md): Rate-Limit pro Code oder Gerät.
- Ergebnis: [research/mysql-in-nuxt.md](../research/mysql-in-nuxt.md)

## Answer

Details und Quellen in [research/mysql-in-nuxt.md](../research/mysql-in-nuxt.md). Fakten, keine Entscheidung:

- Prisma: `npm install prisma` liefert inzwischen Prisma 8 (RC) ohne MySQL-Unterstützung; MySQL nur mit fest gepinntem Prisma 7, Zeilensperren nur über rohes SQL.
- Drizzle (stabil 0.45.x, 1.0 als RC) und Kysely decken Zeilensperren, `ON DUPLICATE KEY UPDATE`, Isolationsstufen und typisierte JSON-Spalten ab. Kyselys Migrator sperrt mit `GET_LOCK()`, Drizzles gar nicht.
- „Letzter freier Platz“: Zeile mit `FOR UPDATE` sperren oder bedingtes `UPDATE … WHERE` mit Prüfung der betroffenen Zeilen. Deadlocks (1213) brauchen Wiederholung.
- „Genau einmal“: eindeutiger Schlüssel plus Zahl der betroffenen Zeilen; `INSERT IGNORE` verschluckt auch andere Fehler.
- Migrationen: Nitro-Plugins warten nicht, DDL in MySQL committet sofort; `RollingUpdate` lässt kurz zwei Pods laufen (betrifft Migration beim Start und Zähler im Speicher).
- Falls die DB auf Percona XtraDB Cluster liegt: Primärschlüssel Pflicht, `GET_LOCK()` und `SERIALIZABLE` gesperrt (bricht Kyselys Migrator); der Haupt-HAProxy schickt alles an einen Knoten.
- `nuxt-auth-utils`: Session komplett im versiegelten Cookie, übersteht Pod-Neustarts bei gleichem `NUXT_SESSION_PASSWORD`, nicht serverseitig widerrufbar; bringt scrypt-Hashing für den Admin-Login.
- Rate-Limit ohne Redis: nur eine MySQL-Tabelle übersteht Neustarts; der Limiter von `nuxt-security` zählt pro IP und passt nicht.
