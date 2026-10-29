# Technik – Übersicht

Stand 29.10.2026 · [Übersicht](../README.md)

Technisches Konzept für die Umsetzung in [project/](../../project/README.md). Es legt Architektur und Verträge fest; Tabellen und Endpunkte entstehen bei der Umsetzung. Fachbegriffe stehen im [Glossar](../../CONTEXT.md).

## Plattform

- Kubernetes-Cluster bei SysEleven, betrieben vom Veranstalter. Deployment über ArgoCD und Crossplane; Routen über Envoy Gateway (Gateway API), nicht über einen Ingress.
- Die App läuft in **einem Pod**. Rund 60 Spieler brauchen keinen zweiten; damit entfällt ein Kanal zwischen Pods.
- Datenbank: MySQL 8.4 auf dem Percona XtraDB Cluster von SysEleven, Modus `ENFORCING`. Jede Tabelle braucht einen Primärschlüssel; `GET_LOCK()` und `SERIALIZABLE` stehen nicht zur Verfügung.
- Der gesamte Spielstand liegt in der Datenbank. Ein Neustart des Pods verliert nichts.
- Redis nur, wenn es nicht anders geht.

## Seiten

- [Durchlauf und Uhren](durchlauf-und-uhren.md): Zustände eines Durchlaufs, Log, Fristen auf dem Server
- [Spielmodul-Schnittstelle](spielmodul-schnittstelle.md): Vertrag zwischen Basis und Spielmodul mit Typen
- [Zugang und Session](zugang-und-session.md): Check-in per Zugangscode, Session, Codes, Rate-Limit, Admin-Login
