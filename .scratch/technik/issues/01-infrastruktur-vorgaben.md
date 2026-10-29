# 01 Infrastruktur-Vorgaben erheben

Type: task (HITL)
Status: resolved
Assignee: –
Blocked by: –

## Question

Welche Vorgaben und Bausteine bringt der Cluster mit, auf dem die Schatzsuche laufen soll? Davon hängen der Echtzeit-Weg (WebSocket oder SSE durch den Ingress), Deployment, Secrets, TLS und das Löschkonzept für Server-Logs ab.

## Checkliste

Die Befehle laufen mit deinem Kubeconfig; Claude hat darauf keinen Zugriff. Ergebnisse als Stichpunkte unter „Answer“ eintragen, **keine Passwörter oder Tokens**.

- [x] Ingress-Controller: Envoy Gateway v1.9.2 (siehe Zwischenstand).
- [x] Eigene Rechte im Namespace: siehe Zwischenstand.
- [x] Secrets: normale Secrets und External Secrets möglich.
- [x] MySQL: Version, Strict Mode, Rechte (siehe Zwischenstand).
- [x] Erreichbarkeit, Deployment, Domain, TLS: Sascha baut die App selbst im Cluster auf (GitOps über ArgoCD und Crossplane, siehe Answer). Restpunkte wandern nach [Betrieb und Deployment](12-betrieb-und-deployment.md).

## Zwischenstand

- Managed Kubernetes von SysEleven.
- Kein klassischer Ingress-Controller: Der Cluster nutzt **Envoy Gateway** (Gateway API) mit einem externen und einem internen Gateway im Namespace `envoy-gateway-system`. Routen heißen dort `HTTPRoute`, nicht `Ingress`.
- Versionen: Envoy Gateway `v1.9.2`, Envoy-Proxy `v1.39.1` (je 2 Replikas für das externe und das interne Gateway).
- Fehlende Rechte: `kubectl get ingressclass`, `kubectl get gateway -A` und `kubectl get httproute -A` schlagen fehl. Offen ist, in welchem Namespace Sascha Rechte hat und wer `HTTPRoute`s an das externe Gateway hängen darf.
- Logging: Loki mit Alloy (`syseleven-loki-alloy`). Aufbewahrungsdauer offen.
- Datenbank: Die eigene MySQL-DB liegt auf dem Percona XtraDB Cluster (`syseleven-pxc-operator`, HAProxy davor). Version `8.4.7-7.1`, `pxc_strict_mode = ENFORCING`, alle Rechte auf das eigene Schema `xmas-ahoi-db` (DDL für Migrationen möglich). Folgen laut [MySQL in Nuxt](03-mysql-in-nuxt.md): Primärschlüssel in jeder Tabelle, kein `GET_LOCK()`, kein `SERIALIZABLE`.
- Rechte im eigenen Namespace (`kubectl auth can-i --list`): Deployments, Services, Pods, Jobs, CronJobs, ConfigMaps, Secrets, PVCs, NetworkPolicies, `Ingress` (networking.k8s.io), HPA, PodDisruptionBudgets; Logs und `exec` der Pods.
- Gateway-API-Objekte (`HTTPRoute`, Policies) legt der eigene User nicht direkt an, sondern Sascha über ArgoCD und Crossplane. Das ist der normale Weg im Cluster und keine Hürde.
- TLS: cert-manager mit `Issuer` und `Certificate` im eigenen Namespace erlaubt; ob ein `ClusterIssuer` existiert, ist offen.
- DNS: Crossplane mit AWS-Route53-Objekten (`records.route53.aws.upbound.io`) ist erlaubt; DNS-Einträge lassen sich also vermutlich selbst anlegen. Ob die Firmen-Domain dort liegt, ist offen.
- Secrets: normale Secrets und External Secrets (`ExternalSecret`, `SecretStore`) erlaubt.
- Überwachung: Prometheus Operator (`ServiceMonitor`, `PrometheusRule`, `AlertmanagerConfig`) erlaubt.
- Richtlinien: Kyverno (Policies lesbar) und Trivy-Berichte (`vulnerabilityreports`) laufen im Cluster. Welche Kyverno-Regeln für Deployments gelten (Labels, Limits, Image-Herkunft), ist offen.
- Interne Plattform-Objekte unter `resources.p4e.io` (`PxcDb`, `CloudfrontDistribution`): Die DB wurde vermutlich darüber angelegt.
## Kontext

- [Betrieb, Datenschutz und Risiken](../../../concepts/framework/betrieb-datenschutz-risiken.md): Server-Logs mit IP-Adressen, Abbau nach 14 Tagen.
- [Echtzeit in Nuxt](02-echtzeit-in-nuxt.md): Die Research nennt, welche Ingress-Einstellungen WebSocket und SSE brauchen.

## Answer

- Plattform: Managed Kubernetes von SysEleven, Envoy Gateway v1.9.2 (Gateway API) statt Ingress-Controller, Loki für Logs, Prometheus Operator, cert-manager, External Secrets, Kyverno.
- Sascha baut die App selbst im Cluster auf: Workloads direkt im Namespace, `HTTPRoute`, Timeouts und DNS über ArgoCD und Crossplane. Erreichbarkeit von außen ist keine Voraussetzung, die jemand anderes erfüllen muss.
- Datenbank: Percona XtraDB Cluster 8.4.7, `pxc_strict_mode = ENFORCING`, alle Rechte auf `xmas-ahoi-db`. Damit gelten Primärschlüssel-Pflicht und kein `GET_LOCK()`.
- Nicht für Entscheidungen nötig und daher bei [Betrieb und Deployment](12-betrieb-und-deployment.md): Namespace-Name, Domain, Registry, Kyverno-Regeln, Aufbewahrung von Loki-Logs und DB-Backups.
