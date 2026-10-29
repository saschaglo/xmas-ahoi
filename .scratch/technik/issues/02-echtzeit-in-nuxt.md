# 02 Echtzeit in Nuxt: WebSocket, SSE oder Polling

Type: research
Status: resolved
Assignee: Sascha Glover
Blocked by: –

## Question

Welche Wege gibt es in Nuxt 4 / Nitro, um Ereignisse einer Crew (Bereit, Start, Sperre bei Verbindungsverlust, Exit-Zustimmung, Hinweis-Sperre, Ende) in Sekunden auf alle Handys zu bringen, und was braucht jeder Weg? Verglichen werden WebSocket (Nitro/crossws), Server-Sent Events und Polling: Reifegrad in Nitro, Verhalten hinter gängigen Ingress-Controllern (ingress-nginx, Traefik: Puffern, Timeouts), Verhalten auf gesperrten oder in den Hintergrund gelegten Handys (iOS Safari, Android Chrome) samt Wiederverbindung, und wie man damit Anwesenheit erkennt. Rahmen: ein Pod, rund 60 Spieler, kein Redis.

## Kontext

- [Karte und Stationen, Verbindung während der Station](../../../concepts/framework/karte-und-stationen.md#verbindung-während-der-station)
- [Crews](../../../concepts/framework/crews.md), [Hinweise, Abbruch und Exit](../../../concepts/framework/hinweise-abbruch-exit.md)
- Ergebnis: [research/echtzeit-in-nuxt.md](../research/echtzeit-in-nuxt.md)

## Answer

Details und Quellen in [research/echtzeit-in-nuxt.md](../research/echtzeit-in-nuxt.md). Fakten, keine Entscheidung; nichts davon ist im Cluster getestet:

- Nuxt 4.6 läuft auf Nitro 2.13 mit crossws 0.3: WebSocket braucht `nitro.experimental.websocket`; SSE (`createEventStream`) braucht kein Flag, ist aber als experimentell markiert. Beides läuft im Preset `node-server`. crossws 0.3 kennt kein `peer.ping()`.
- Envoy Gateway: WebSocket-Upgrade ist ab Werk an. Ohne `timeouts.request` an der `HTTPRoute` schneidet Envoys Routen-Timeout SSE nach 15 s ab; für WebSocket gilt er laut Quelltext nicht. Für beide gilt ein Leerlauf-Timeout von 5 min. Davor schließt der OpenStack-Loadbalancer von SysEleven laut Doku leere Verbindungen nach 50 s.
- ingress-nginx ist seit März 2026 eingestellt (nur Randnotiz, der Cluster nutzt Envoy Gateway).
- Handys: Android-Chrome friert Hintergrund-Tabs nach 5 min ein und schließt dabei WebSockets; WebKit schließt sie beim Back/Forward-Cache. `EventSource` verbindet sich nach 3 s selbst neu und schickt `Last-Event-ID`. Wie schnell iOS Safari nach dem Sperren pausiert, ist nicht offiziell belegt.
- Anwesenheit: Bei allen drei Wegen braucht es einen Herzschlag vom Handy und eine Frist auf dem Server, weil ein still verschwundenes Handy beim Node-Prozess kein `close` auslöst. Bei SSE ein Herzschlag-POST neben dem Strom, bei Polling sind die Abfragen selbst der Herzschlag.
