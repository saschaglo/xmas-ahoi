# 01 Infrastruktur-Vorgaben erheben

Type: task (HITL)
Status: open
Assignee: –
Blocked by: –

## Question

Welche Vorgaben und Bausteine bringt der Cluster mit, auf dem die Schatzsuche laufen soll? Davon hängen der Echtzeit-Weg (WebSocket oder SSE durch den Ingress), Deployment, Secrets, TLS und das Löschkonzept für Server-Logs ab.

## Checkliste

Die Befehle laufen mit deinem Kubeconfig; Claude hat darauf keinen Zugriff. Ergebnisse als Stichpunkte unter „Answer“ eintragen, **keine Passwörter oder Tokens**.

- [x] Ingress-Controller: Envoy Gateway (siehe Zwischenstand).
- [ ] Envoy-Gateway-Version: `kubectl -n envoy-gateway-system get pods -o custom-columns=NAME:.metadata.name,IMAGE:.spec.containers[*].image`
- [ ] Vorbild: `kubectl get gateway -A` und `kubectl get httproute -A`. Bei einem bestehenden Projekt `kubectl -n <ns> get httproute <name> -o yaml` ansehen: `parentRefs` (welches Gateway) und Hostnamen notieren. Dazu `kubectl get backendtrafficpolicy,clienttrafficpolicy -A` (Timeouts).
- [ ] TLS und Domain: `kubectl get clusterissuer` (cert-manager vorhanden?). Welche Domain oder Subdomain bekommt die Seite, und wer legt den DNS-Eintrag an?
- [ ] Namespace und Rechte: `kubectl auth can-i create deployments -n <ns>`, ebenso für `ingresses` und `secrets`.
- [ ] Deployment-Weg: Helm, Kustomize, ArgoCD oder Flux? Prüfen mit `kubectl get applications -A` (Argo) und `kubectl get kustomizations -A` (Flux); sonst `kubectl apply`.
- [ ] Image-Registry und CI: Wohin werden Images gepusht, welche CI baut sie (GitHub Actions, GitLab), braucht der Cluster ein Pull-Secret?
- [ ] Secrets: Wie kommen Zugangsdaten in den Cluster? `kubectl get crd | grep -i -E 'secret|sealed'` (Sealed Secrets, External Secrets) oder normale Secrets.
- [ ] MySQL: Version (`SELECT VERSION();`), aus dem Cluster erreichbar (interner oder externer Host)? Rechte für `CREATE`/`ALTER TABLE` (Migrationen)? Wer macht Backups? `SHOW VARIABLES LIKE 'max_connections';`
- [ ] Logs: Zentrales Logging im Cluster? `kubectl get pods -A | grep -i -E 'loki|promtail|fluent|vector|elastic'`. Wie lange werden Logs und Ingress-Access-Logs (mit IP-Adressen) aufbewahrt?
- [ ] Firmenvorgaben: Ressourcenlimits, Pflicht-Labels, Security-Policies, Basis-Images?

## Zwischenstand

- Managed Kubernetes von SysEleven.
- Kein klassischer Ingress-Controller: Der Cluster nutzt **Envoy Gateway** (Gateway API) mit einem externen und einem internen Gateway im Namespace `envoy-gateway-system`. Routen heißen dort `HTTPRoute`, nicht `Ingress`.
- `kubectl get ingressclass` schlägt fehl (vermutlich fehlende Rechte auf clusterweite Ressourcen).
- Logging: Loki mit Alloy (`syseleven-loki-alloy`). Aufbewahrungsdauer offen.
- Datenbank: Percona XtraDB Cluster Operator (`syseleven-pxc-operator`, HAProxy davor). Ob die eigene MySQL-DB dort liegt, ist offen.

## Kontext

- [Betrieb, Datenschutz und Risiken](../../../concepts/framework/betrieb-datenschutz-risiken.md): Server-Logs mit IP-Adressen, Abbau nach 14 Tagen.
- [Echtzeit in Nuxt](02-echtzeit-in-nuxt.md): Die Research nennt, welche Ingress-Einstellungen WebSocket und SSE brauchen.
