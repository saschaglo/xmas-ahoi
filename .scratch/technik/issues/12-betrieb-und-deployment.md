# 12 Betrieb und Deployment

Type: grilling
Status: open
Assignee: –
Blocked by: [Infrastruktur-Vorgaben erheben](01-infrastruktur-vorgaben.md), [Stack und Projektstruktur](09-stack-und-projektstruktur.md)

## Question

Wie kommt die App in den Cluster und wie wird sie am Abend betrieben? Image-Build und Deployment, Migrationen, Neustart-Verhalten bei einem Pod, Überwachung vom Handy aus während der Feier, Backup der Datenbank am Abend, Aufbewahrung der Server-Logs und der Abbau bis 4.12.2026.

## Kontext

- Aus [Infrastruktur-Vorgaben erheben](01-infrastruktur-vorgaben.md) offen: Namespace-Name, Domain, Registry, Kyverno-Regeln für Deployments, Aufbewahrung von Loki-Logs (Access-Logs mit IP-Adressen) und DB-Backups. Deployment über ArgoCD und Crossplane.
- [Betrieb, Datenschutz und Risiken](../../../concepts/framework/betrieb-datenschutz-risiken.md): Löschkonzept.
- [Admin-Panel, Testlauf](../../../concepts/admin/admin-panel.md#testlauf): Testlauf am 11.11.
- Aus [Stack und Projektstruktur](09-stack-und-projektstruktur.md): Migrationen liegen als committetes SQL von `drizzle-kit generate` vor; offen ist, ob Vitest und Playwright (mit PXC in Docker) in einer CI laufen und wie die Umgebungsvariablen `NUXT_STATIONS`, `NUXT_PUBLIC_DEBUG` und `NUXT_TEST_CLOCK` je Umgebung gesetzt werden (siehe [Architektur](../../../concepts/technik/architektur.md#umgebungsvariablen)).
