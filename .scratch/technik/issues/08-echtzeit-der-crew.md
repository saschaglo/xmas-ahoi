# 08 Echtzeit der Crew und Anwesenheit

Type: grilling
Status: open
Assignee: –
Blocked by: [Infrastruktur-Vorgaben erheben](01-infrastruktur-vorgaben.md), [Echtzeit-Probe im Cluster](13-echtzeit-probe-im-cluster.md), [Echtzeit in Nuxt](02-echtzeit-in-nuxt.md), [Durchlauf-Modell und Uhren](05-durchlauf-modell-und-uhren.md), [Netz im Gastraum testen](../../gesamtkonzept/issues/16-netz-im-gastraum.md)

## Question

Über welchen Weg (WebSocket, SSE oder Polling) erfahren die Handys einer Crew von Ereignissen, und wie erkennt der Server, dass ein Mitglied ausgefallen ist? Ab wann gilt jemand als ausgefallen, und laufen Zeitlimit und Höchstzeit während der Sperre weiter? Wie verhält sich die Seite bei schlechtem Netz?

## Kontext

- [Karte und Stationen, Verbindung während der Station](../../../concepts/framework/karte-und-stationen.md#verbindung-während-der-station): beide Fragen sind dort „bei der Umsetzung klären“.
- [Netz im Gastraum testen](../../gesamtkonzept/issues/16-netz-im-gastraum.md) liegt auf der Karte Gesamtkonzept.
- Falls der Ingress unklar bleibt: kleiner Probe-Pod als Prototyp.
