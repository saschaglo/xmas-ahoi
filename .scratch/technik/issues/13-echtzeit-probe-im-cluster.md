# 13 Echtzeit-Probe im Cluster

Type: prototype
Status: open
Assignee: –
Blocked by: [Infrastruktur-Vorgaben erheben](01-infrastruktur-vorgaben.md)

## Question

Halten WebSocket und SSE durch Envoy Gateway und den Loadbalancer von SysEleven über 15 s, 50 s und 5 min, mit und ohne Herzschlag, und wie schnell fällt ein gesperrtes iPhone oder Android-Handy ab? Ein kleiner Probe-Pod mit einer Nuxt-Seite und einer `HTTPRoute` liefert die Zahlen, an denen der Echtzeit-Weg entschieden wird.

## Kontext

- [Echtzeit in Nuxt](02-echtzeit-in-nuxt.md): Abschnitt zur Probe in [research/echtzeit-in-nuxt.md](../research/echtzeit-in-nuxt.md).
- Braucht aus „Infrastruktur-Vorgaben erheben“: Namespace mit Rechten, Gateway-Name für `parentRefs`, Hostname, Registry.
- Wegwerf-Code, liegt nicht in `project/`.
