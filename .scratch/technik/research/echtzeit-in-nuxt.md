# Recherche: Echtzeit in Nuxt

Stand: 28.10.2026.

Frage aus [Ticket 02](../issues/02-echtzeit-in-nuxt.md): Wie kommen Ereignisse einer Crew (Bereit, Start, Sperre bei Verbindungsverlust, Exit-Zustimmung, Hinweis-Sperre, Ende) in Sekunden auf alle Handys der Crew (1 bis 3 Spieler)? Verglichen werden WebSocket, Server-Sent Events (SSE) und Polling. Rahmen aus der [Karte](../map.md): ein Pod, rund 60 Spieler, kein Redis, MySQL. Der Cluster leitet den Verkehr über **Envoy Gateway** (Gateway API) bei SysEleven, nicht über ingress-nginx oder Traefik (Auskunft des Betreibers während der Recherche).

Diese Seite stellt Fakten zusammen und trifft keine Entscheidung; die fällt in [Ticket 08](../issues/08-echtzeit-der-crew.md). Quellen sind Primärquellen (Dokus, Quelltext, Spezifikationen, npm-Registry). Was ich aus Quelltext abgeleitet oder nicht prüfen konnte, ist als **abgeleitet** oder **nicht verifiziert** markiert.

## Kernbefunde

1. **Nuxt 4 bringt Nitro 2, und dort sind WebSocket und SSE beide als experimentell markiert.** WebSocket braucht das Flag `nitro.experimental.websocket: true`; SSE (`createEventStream`) ist ohne Flag nutzbar, trägt aber in h3 1.x den Vermerk `@experimental`. Beide laufen im Preset `node-server` und im Dev-Server.
2. **Envoy Gateway kappt Antworten nach 15 Sekunden, solange die HTTPRoute nichts anderes sagt.** Das trifft SSE direkt (der Stream endet nie). WebSocket-Upgrades sind standardmäßig erlaubt; der 15-s-Timeout startet laut Envoy erst nach dem vollständigen Request, was bei WebSocket nicht eintritt. Für beide gilt zusätzlich ein Stream-Idle-Timeout von 5 Minuten, und vor Envoy sitzt bei SysEleven ein OpenStack-Load-Balancer mit 50 Sekunden Idle-Timeout.
3. **Gesperrte oder weggewischte Handys verlieren ihre Verbindung, egal welcher Weg.** Chrome friert Hintergrund-Tabs auf Android nach 5 Minuten ein und schließt dabei offene WebSockets; Safari/WebKit schließt WebSockets beim Eintritt in den Back-Forward-Cache. Wie schnell iOS eine Seite nach dem Sperren suspendiert, ist nicht offiziell dokumentiert.
4. **Anwesenheit lässt sich nur über einen Heartbeat auf Anwendungsebene zuverlässig erkennen.** Der Node-Prozess spricht nur mit Envoy, nicht mit dem Handy; ein still verschwundenes Handy löst beim Server kein `close` aus. crossws 0.3 (die Version in Nuxt 4) hat kein `peer.ping()`.
5. **ingress-nginx ist eingestellt** (seit März 2026 keine Releases und keine Sicherheitsupdates mehr). Für diesen Cluster nur noch eine Randnotiz.

## Versionen

| Paket | Version | Quelle |
|-------|---------|--------|
| `nuxt` (latest) | 4.6.3, veröffentlicht 27.10.2026 | [npm](https://registry.npmjs.org/nuxt) |
| `@nuxt/nitro-server` 4.6.3 hängt ab von | `nitropack ^2.13.5`, `h3 ^1.15.11` | [npm](https://registry.npmjs.org/@nuxt/nitro-server/4.6.3) |
| `nitropack` (latest) | 2.13.5, hängt ab von `crossws ^0.3.5` | [npm](https://registry.npmjs.org/nitropack/2.13.5) |
| `nitro` (Nitro 3) | 3.0.260903-beta, nur Beta | [npm](https://registry.npmjs.org/nitro) |
| `crossws` (latest) | 0.4.17; Nuxt 4 nutzt aber 0.3.x (0.3.5 ist die letzte 0.3) | [npm](https://registry.npmjs.org/crossws) |
| Envoy Gateway | v1.9.3, 26.10.2026 | [GitHub Releases](https://github.com/envoyproxy/gateway/releases) |

Folge: Für Nuxt 4 gelten die **Nitro-2-Doku** ([v2.nitro.build](https://v2.nitro.build/guide/websocket)) und die **h3-1-Doku** ([v1.h3.dev](https://v1.h3.dev/guide/websocket)). Die aktuelle Seite [nitro.build/docs/websocket](https://nitro.build/docs/websocket) beschreibt Nitro 3 (Flag `features.websocket`, ohne Experimental-Hinweis) und gilt für Nuxt 4 nicht. Die aktuelle crossws-Doku auf [crossws.h3.dev](https://crossws.h3.dev/guide/peer) beschreibt 0.4 (mit `peer.ping()`), nicht die 0.3 aus Nuxt 4.

## Weg 1: WebSocket (Nitro/crossws)

**Status.** Die Nitro-2-Doku sagt: „WebSockets support is currently experimental.“ Einschalten in `nuxt.config.ts` über `nitro: { experimental: { websocket: true } }` ([Nitro v2, WebSocket](https://v2.nitro.build/guide/websocket)). Das Plattform-Tracking-Issue listete `node-server` als unterstützt, `node-cluster` nicht; es wurde am 15.10.2026 als „old tracker“ geschlossen, ohne dass Nitro 2 den Experimental-Status verlor ([nitrojs/nitro#2171](https://github.com/nitrojs/nitro/issues/2171)).

**API.** Eine Datei unter `server/routes/` exportiert `defineWebSocketHandler({ open, message, close, error })`; jede Route kann so ein Upgrade-Ziel sein ([Nitro v2, WebSocket](https://v2.nitro.build/guide/websocket)). Der `peer` (crossws 0.3.5) hat `send`, `subscribe(topic)`, `unsubscribe`, `publish(topic, data)`, `close`, `terminate`, `id`, `context` und `peers` ([crossws 0.3.5, `dist/index.d.mts`](https://registry.npmjs.org/crossws/-/crossws-0.3.5.tgz)). `publish` sendet an alle Abonnenten außer dem sendenden Peer ([crossws, Peer](https://crossws.h3.dev/guide/peer); im 0.3.5-Node-Adapter ebenso: `if (peer !== this && peer._topics.has(topic))`). Pub/Sub läuft im Prozess, also ohne Redis; ein Topic je Durchlauf reicht für eine Crew.

**Heartbeat.** crossws 0.3.5 hat weder `peer.ping()` noch einen `pong`-Hook (kein Treffer für „ping“ in Typen und Node-Adapter des Pakets). Das kam erst mit 0.4 ([crossws, Peer](https://crossws.h3.dev/guide/peer)). In Nuxt 4 läuft der Heartbeat also als normale Nachricht der Anwendung.

**Dev und Produktion.** Im Preset `node-server` hängt Nitro den crossws-Node-Adapter an das `upgrade`-Ereignis des HTTP-Servers (`nitropack/dist/presets/node/runtime/node-server.mjs`, Zeile 38 f., im [Paket 2.13.5](https://registry.npmjs.org/nitropack/-/nitropack-2.13.5.tgz)). Im Dev-Modus leitet `nuxi dev` Upgrades an Nitro weiter (`@nuxt/cli` 4.0.1, `dist/dev/index.mjs`, `server.on("upgrade", …)`), und der Nitro-Dev-Worker nimmt sie an (`presets/_nitro/runtime/nitro-dev.mjs`). **Abgeleitet** aus dem Quelltext, nicht ausprobiert.

**Client.** Der Browser hat `WebSocket` eingebaut, aber ohne Wiederverbindung. VueUse `useWebSocket` bietet `autoReconnect` (Versuche, Pause) und `heartbeat` (Nachricht, Intervall, `pongTimeout`) ([VueUse](https://vueuse.org/core/useWebSocket/)); VueUse wäre eine neue Abhängigkeit.

## Weg 2: Server-Sent Events

**Status.** `createEventStream(event)` aus h3 1.x ist in Nuxt 4 ohne Flag verfügbar. Die h3-1-Doku sagt: „SSE support is currently experimental“; die Typdefinition trägt `@experimental This function is experimental and might be unstable in some environments.` ([h3 v1, WebSocket und SSE](https://v1.h3.dev/guide/websocket); `h3/dist/index.d.ts` Zeile 1286 im [Paket 1.15.11](https://registry.npmjs.org/h3/-/h3-1.15.11.tgz)). Die Nitro-2-Doku zeigt SSE als Alternative zu WebSocket ([Nitro v2](https://v2.nitro.build/guide/websocket)).

**API und Verhalten** (h3 1.15.11, `dist/index.mjs` ab Zeile 1481):

- `push(string | {id, event, data, retry} | Array)`, `pause()`, `resume()`, `close()`, `onClosed(cb)`, `send()`.
- `send()` setzt `Content-Type: text/event-stream`, `Cache-Control: private, no-cache, no-store, no-transform, …` und **`X-Accel-Buffering: no`**, bei HTTP/1.1 zusätzlich `Connection: keep-alive`.
- Ein Abbruch der Client-Verbindung (`req` `close`) schließt den Stream automatisch, außer bei `autoclose: false`.
- **Kein eingebauter Keepalive.** Kommentarzeilen oder Heartbeat-Ereignisse muss die Anwendung selbst senden.

**Client.** `EventSource` verbindet sich nach einem Abbruch selbst neu. Die Pause setzt der Server mit `retry:`, sonst gilt ein browserdefinierter Wert ([MDN, Using SSE](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)), in Chromium und WebKit jeweils 3000 ms ([Chromium `event_source.cc`](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/third_party/blink/renderer/modules/eventsource/event_source.cc), [WebKit `EventSource.cpp`](https://github.com/WebKit/WebKit/blob/main/Source/WebCore/page/EventSource.cpp)). Beim Neuverbinden schickt der Browser die letzte `id` als Header `Last-Event-ID` ([HTML-Spezifikation, SSE](https://html.spec.whatwg.org/multipage/server-sent-events.html)). Keine Wiederverbindung gibt es bei Status ≠ 200 oder falschem `Content-Type`: dann „fail the connection“ ([HTML-Spezifikation](https://html.spec.whatwg.org/multipage/server-sent-events.html)). Die Spezifikation empfiehlt gegen Proxys „a comment line (one starting with a ':' character) every 15 seconds or so“ (ebd.). Nuxt verweist für GET-SSE auf `EventSource` oder VueUse `useEventSource` ([Nuxt 4, Data Fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)).

**Rückkanal.** SSE geht nur vom Server zum Handy. „Bereit“, „Start“, Hinweis, Exit-Zustimmung und Heartbeat gehen als normale `$fetch`-POSTs zurück.

**Verbindungslimit.** Über HTTP/1.1 erlauben Browser nur 6 SSE-Verbindungen je Domain, über HTTP/2 rund 100 Streams ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)). Ein Handy mit einem Tab braucht eine. Envoy Gateway bietet an HTTPS-Listenern standardmäßig `h2` und `http/1.1` per ALPN an ([Envoy Gateway v1.9.3, `api/v1alpha1/tls_types.go`](https://github.com/envoyproxy/gateway/blob/v1.9.3/api/v1alpha1/tls_types.go)).

## Weg 3: Polling

Das Handy fragt den Zustand des Durchlaufs in festem Takt per `$fetch` ab (zum Beispiel alle 1 bis 2 Sekunden). Weder Nitro noch Envoy brauchen dafür eine Sonderkonfiguration: Jeder Request ist kurz und liegt weit unter dem 15-s-Route-Timeout. Bei 60 Spielern und einem Abruf je 2 s sind das rund 30 Requests pro Sekunde (eigene Rechnung). Die Verzögerung eines Ereignisses ist höchstens ein Takt. Ein Abruf ist zugleich der Heartbeat. In sichtbaren Tabs drosseln Browser Timer nicht; versteckte Tabs werden gedrosselt ([MDN, Page Visibility API](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API)), Chrome prüft Timer-Ketten nach 5 Minuten im Hintergrund nur noch einmal pro Minute ([Chrome 88 Timer Throttling](https://developer.chrome.com/blog/timer-throttling-in-chrome-88)). Für die App heißt das: Ein verstecktes Handy fällt aus dem Takt und gilt als abwesend, was hier ohnehin gewollt ist.

## Cluster: Envoy Gateway und Load Balancer

### Envoy Gateway (Gateway API, HTTPRoute)

**WebSocket-Upgrade: standardmäßig an.** Der Translator setzt auf jeder Route `upgrade_configs` mit `websocket`, außer wenn eine BackendTrafficPolicy `requestBuffer` aktiviert ([Envoy Gateway v1.9.3, `internal/xds/translator/route.go`](https://github.com/envoyproxy/gateway/blob/v1.9.3/internal/xds/translator/route.go), `defaultUpgradeConfig` und `buildUpgradeConfig`). Die Doku warnt: Request Buffering „does not work with streaming or upgrade-based traffic such as gRPC streaming and WebSocket“ ([Envoy Gateway, Request Buffering](https://gateway.envoyproxy.io/docs/tasks/traffic/request-buffering/)).

**Timeouts, die lange Verbindungen treffen können:**

| Timeout | Standard | Trifft SSE? | Trifft WebSocket? | Ändern über |
|---------|----------|-------------|-------------------|-------------|
| Route-Timeout (Envoy `RouteAction.timeout`) | **15 s** („If not specified, the default is 15s“) | **Ja.** Er läuft ab dem vollständigen Request bis zur vollständigen Antwort, und ein SSE-Stream endet nie. | Laut Envoy startet er erst nach dem vollständigen Request; ein Upgrade-Request endet nicht. **Abgeleitet, im Probe-Pod prüfen.** | HTTPRoute `rules[].timeouts.request` (Gateway API), oder BackendTrafficPolicy `timeout.http.requestTimeout` |
| Stream-Idle-Timeout (HCM `stream_idle_timeout`) | **5 min** ohne Daten in beide Richtungen | Ja, ohne Keepalive | Ja, ohne Heartbeat | ClientTrafficPolicy `timeout.http.streamIdleTimeout` (Listener) oder BackendTrafficPolicy `timeout.http.streamIdleTimeout` (Route) |
| Verbindungs-Idle-Timeout | 1 h ohne aktive Requests | Nein, solange der Stream offen ist | Nein, solange offen | ClientTrafficPolicy `timeout.http.idleTimeout` |
| `maxStreamDuration`, `maxConnectionDuration` | unbegrenzt | Nein | Nein | BackendTrafficPolicy |
| `requestReceivedTimeout` | aus | Nein | Nein | ClientTrafficPolicy |

Quellen zur Tabelle: Envoy-Route-Timeout 15 s und seine Spanne ([Envoy `route_components.proto`](https://github.com/envoyproxy/envoy/blob/main/api/envoy/config/route/v3/route_components.proto)); Envoy-FAQ: „This timeout defaults to 15 seconds. This is typically a problem for streaming responses … and will need to be disabled by setting to 0“, „This timeout will not be triggered by streaming requests, as it only begins when the request is complete“, Stream-Idle 5 min, Verbindungs-Idle 1 h ([Envoy FAQ, Timeouts](https://www.envoyproxy.io/docs/envoy/latest/faq/configuration/timeouts)); Envoy Gateway: „The default request timeout is set to 15 seconds in Envoy Proxy“ ([Envoy Gateway, HTTP Timeouts](https://gateway.envoyproxy.io/docs/tasks/traffic/http-timeouts/)); Felder und Standards von `HTTPTimeout` und `HTTPClientTimeout` ([Envoy Gateway API-Referenz](https://gateway.envoyproxy.io/docs/api/extension_types/), Quelltext [`api/v1alpha1/timeout_types.go`](https://github.com/envoyproxy/gateway/blob/v1.9.3/api/v1alpha1/timeout_types.go)).

**Zusammenspiel im Quelltext** (Envoy Gateway v1.9.3, [`route.go`](https://github.com/envoyproxy/gateway/blob/v1.9.3/internal/xds/translator/route.go), `getEffectiveRequestTimeout` und `idleTimeout`):

- Ohne `timeouts.request` in der HTTPRoute setzt Envoy Gateway keinen Route-Timeout; damit greift Envoys Standard von 15 s.
- `timeouts.request` der HTTPRoute hat Vorrang vor `requestTimeout` der BackendTrafficPolicy.
- Mit `timeouts.request: "0s"` ist der Route-Timeout aus, und Envoy Gateway schaltet auch den Route-Idle-Timeout ab, solange keine Policy einen `streamIdleTimeout` setzt. Ein toter SSE-Stream bleibt dann offen, bis TCP oder der Load Balancer ihn schließen. **Abgeleitet** aus dem Quelltext.
- Die Gateway API schreibt vor: „A zero-valued timeout ('0s') MUST be interpreted as disabling the timeout“. `timeouts` ist „Extended support“ ([GEP-1742](https://gateway-api.sigs.k8s.io/geps/gep-1742/)).

**Puffern.** Envoy puffert Antworten nur, wenn ein Buffer-Filter konfiguriert ist; Envoy Gateway setzt ihn nur für `requestBuffer` (Request-Seite) ([Envoy, HTTP upgrades](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/http/upgrades): „Buffering is generally not compatible with upgrades“; [Envoy Gateway, Request Buffering](https://gateway.envoyproxy.io/docs/tasks/traffic/request-buffering/)). `X-Accel-Buffering` ist ein nginx-Header; für Envoy ist er bedeutungslos. **Nicht verifiziert:** wie sich Response Compression (BackendTrafficPolicy `compressor`, [Envoy Gateway](https://gateway.envoyproxy.io/docs/tasks/traffic/response-compression/)) auf `text/event-stream` auswirkt, falls sie im Cluster aktiv ist.

### OpenStack-Load-Balancer vor Envoy (SysEleven)

Ein Service `type: LoadBalancer` bekommt bei MetaKube einen OpenStack-LBaaS (Octavia). Laut SysEleven schließt er „idle connections after 50s“; höhere Werte setzt man per Annotation am Service, etwa `loadbalancer.openstack.org/timeout-client-data: "300000"` und `timeout-member-data` ([SysEleven, Load Balancers](https://documentation.syseleven.de/en/products/metakube-core/concepts/networking/load-balancers/)). Standard in Octavia: `timeout_client_data` und `timeout_member_data` je 50000 ms ([OpenStack Octavia API](https://docs.openstack.org/api-ref/load-balancer/v2/index.html)). Der Load Balancer gehört zum Envoy-Service in `envoy-gateway-system`, nicht zur App. **Nicht verifiziert:** ob er im TCP-Modus läuft und welche Timeouts dort gesetzt sind (Ticket 01). Ein Heartbeat oder Keepalive unter 50 s hält jede Verbindung ohnehin aktiv.

### Randnotiz: ingress-nginx und Traefik

- **ingress-nginx ist eingestellt.** „In March 2026, Ingress NGINX maintenance will be halted … no further releases, no bugfixes, and no updates to resolve any security vulnerabilities“; bestehende Installationen laufen weiter ([Kubernetes Blog, 11.11.2025](https://kubernetes.io/blog/2025/11/11/ingress-nginx-retirement/)). WebSocket geht ohne Annotation, aber `proxy-read-timeout`/`proxy-send-timeout` stehen auf 60 s ([ingress-nginx, Miscellaneous](https://kubernetes.github.io/ingress-nginx/user-guide/miscellaneous/)). `proxy-buffering` ist standardmäßig `off` ([ConfigMap](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/)). nginx beachtet `X-Accel-Buffering: no` vom Backend ([nginx proxy module](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)), und h3 sendet diesen Header.
- **Traefik:** Seit v2.11.2 steht `respondingTimeouts.readTimeout` auf 60 s („maximum duration for reading the entire request, including the body“) ([Traefik Migration v2](https://github.com/traefik/traefik/blob/v2.11/docs/content/migration/v2.md)). Ob das lange WebSocket- oder SSE-Verbindungen kappt, sagt die Traefik-Doku nicht; es gibt Berichte über Abbrüche nach genau 60 s ([traefik#13820](https://github.com/traefik/traefik/issues/13820)). **Nicht verifiziert.**

## Handys im Hintergrund oder gesperrt

| Ereignis | Chrome (Android) | Safari (iOS) |
|----------|------------------|--------------|
| Tab wechselt auf „hidden“ (App-Wechsel, Sperre) | `visibilitychange` mit `hidden`. „Transitioning to hidden is the last event that's reliably observable by the page“ ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document/visibilitychange_event)) | wie links (MDN gilt browserübergreifend) |
| Timer im Hintergrund | gedrosselt; Seiten mit WebSocket gehen laut MDN ungedrosselt ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API)); Intensivdrosselung nach 5 min ([Chrome](https://developer.chrome.com/blog/timer-throttling-in-chrome-88)) | gedrosselt (MDN); Details nicht offiziell dokumentiert |
| Einfrieren | „Chrome on Android freezes background tabs after 5 minutes“ ([blink-dev, 8.10.2024](https://groups.google.com/a/chromium.org/g/blink-dev/c/Xu1C7WhoGm4)). Beim Einfrieren schließt Blink offene WebSockets mit „Page entered Back-Forward Cache.“ (Feature `DisconnectWebSocketOnBFCache`, Status `stable`; [Chromium `dom_websocket.cc`](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/third_party/blink/renderer/modules/websockets/dom_websocket.cc)) | WebKit schließt WebSockets beim Eintritt in den Back-Forward-Cache („WebSocket is closed due to suspension.“; [WebKit `WebSocket.cpp`](https://github.com/WebKit/WebKit/blob/main/Source/WebCore/Modules/websockets/WebSocket.cpp)). EventSource wird dabei pausiert und beim Zurückkehren neu verbunden ([WebKit `EventSource.cpp`](https://github.com/WebKit/WebKit/blob/main/Source/WebCore/page/EventSource.cpp)) |
| App-Suspendierung nach Sperre | Zeitpunkt nicht offiziell dokumentiert | **Nicht verifiziert.** Apple DTS zu nativen Apps: „The key factor here is not foreground/background but running/suspended … iOS will usually suspend the app shortly after moving it to the background“ ([Apple Developer Forums](https://developer.apple.com/forums/thread/716118)). Für Safari-Webseiten fand ich keine offizielle Zeitangabe. |

Empfehlung der Chrome-Doku für „frozen“: „Stop network polling or close Web Socket connections“; Seiten sollen beim Zurückkehren (`resume`, `pageshow`) neu verbinden ([Chrome, Page Lifecycle API](https://developer.chrome.com/docs/web-platform/page-lifecycle-api); [web.dev, bfcache](https://web.dev/articles/bfcache)). Laut web.dev blockieren offene WebSockets den bfcache in Chrome (ab 149) und Safari nicht mehr ([web.dev, bfcache](https://web.dev/articles/bfcache)).

Folge für alle drei Wege: Ein gesperrtes Handy ist nach kurzer Zeit offline, und beim Entsperren muss die Seite den Zustand neu holen. `EventSource` verbindet sich selbst neu (3 s) und schickt `Last-Event-ID`; ein WebSocket braucht eigene Wiederverbindung; Polling macht einfach den nächsten Abruf.

## Anwesenheit erkennen („Peter ist gerade nicht an Bord“)

- **Der Server sieht das Handy nicht direkt.** Die TCP-Verbindung des Node-Prozesses endet an Envoy. Verschwindet ein Handy still (Funkloch, Sperre ohne sauberes Schließen), merkt Envoy das erst über eigene Timeouts, und erst dann schließt Envoy die Verbindung zum Pod. **Abgeleitet** aus der Architektur.
- **WebSocket:** `close`-Hook bei sauberem Schließen (Tab zu, Einfrieren in Chrome und WebKit). Für stille Ausfälle: der Client sendet alle N Sekunden eine Nachricht, der Server führt je Peer einen Zeitstempel und meldet nach M Sekunden ohne Nachricht „abwesend“ an das Topic des Durchlaufs. crossws 0.3 hat dafür kein Ping (siehe oben).
- **SSE:** `onClosed` feuert, wenn die Verbindung zum Pod schließt. Ein Keepalive-Kommentar vom Server hält die Strecke offen, zeigt aber nicht, dass das Handy lebt, weil Schreiben in einen TCP-Puffer gelingt. Anwesenheit braucht daher einen Heartbeat per POST vom Handy, praktisch also Polling für die Anwesenheit.
- **Polling:** Der letzte Abruf ist der Heartbeat. Abwesend ist, wer M Sekunden nicht abgefragt hat.
- Für alle drei: `visibilitychange` → `hidden` kann der Client sofort melden (per `navigator.sendBeacon` oder Nachricht), damit die Sperre schneller erscheint; das ist der letzte verlässlich beobachtbare Moment ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document/visibilitychange_event)). Ob die Meldung beim Sperren noch durchkommt: **nicht verifiziert.**
- Der Zustand „anwesend“ lebt im Prozess (ein Pod), das Log des Verbindungsverlusts geht nach MySQL.

## Vergleich

| | WebSocket (Nitro/crossws) | SSE (`createEventStream`) | Polling |
|---|---|---|---|
| Status in Nuxt 4 / Nitro 2 | experimentell, Flag `nitro.experimental.websocket` | experimentell markiert (h3 1.x), kein Flag | stabil, nur `$fetch` |
| Richtung | beide | Server → Handy; Rückweg per POST | Handy → Server |
| Verzögerung eines Ereignisses | sofort | sofort | bis zu einem Takt (z. B. 1–2 s) |
| Pub/Sub im Prozess | eingebaut (`subscribe`/`publish`) | selbst bauen (Map Durchlauf → Streams) | entfällt, jeder Abruf liest den Zustand |
| Envoy Gateway, Standard | Upgrade an; 5-min-Stream-Idle braucht Heartbeat; 15-s-Route-Timeout greift vermutlich nicht (prüfen) | **bricht nach 15 s ab**, bis HTTPRoute `timeouts.request` gesetzt ist (`"0s"` oder lang); Keepalive < 5 min | keine Anpassung |
| SysEleven-LB (50 s idle) | Heartbeat < 50 s | Keepalive < 50 s | kein Thema |
| Wiederverbindung | selbst bauen (oder VueUse) | eingebaut, 3 s, `Last-Event-ID` | entfällt |
| Hintergrund/Sperre | Chrome und WebKit schließen beim Einfrieren | WebKit pausiert und verbindet neu; Chrome: **nicht geprüft** | Abrufe stoppen oder werden gedrosselt |
| Anwesenheit | Heartbeat-Nachricht + Timeout | Heartbeat-POST + Timeout | letzter Abruf + Timeout |
| Last bei 60 Spielern | 60 offene Verbindungen | 60 offene Streams + POSTs | ca. 30 Requests/s bei 2-s-Takt (eigene Rechnung) |

## Was jede Option für Xmas Ahoi bedeuten würde

- **WebSocket:** ein Kanal für alle Crew-Ereignisse und Anwesenheit, Topic je Durchlauf. Dazu kommen ein experimentelles Nitro-Feature, eigene Wiederverbindung mit Zustandsabgleich nach dem Entsperren und ein Heartbeat auf Anwendungsebene. Laut Karte gilt WebSocket am Ingress bis zur Klärung als „nein“; mit Envoy Gateway ist das Upgrade standardmäßig an, ein Test im Cluster fehlt noch.
- **SSE:** Ereignisse kommen sofort, Wiederverbindung liefert der Browser. Nötig sind eine eigene HTTPRoute-Regel mit `timeouts.request` für den Stream-Pfad, ein Keepalive-Kommentar unter 50 s und für die Anwesenheit trotzdem ein Heartbeat per POST. Das ist im Kern Polling plus Push.
- **Polling:** keine Cluster-Anpassung und keine experimentellen Features; Anwesenheit kommt gratis. Ereignisse kommen bis zu einem Takt verspätet, und der Exit-Countdown (60 s) und die Hinweis-Sperre (60 s) zeigt das Handy aus seinem eigenen Takt an. Der Zustand je Abruf sollte aus dem Speicher des Pods kommen, nicht je Abruf aus MySQL (Bemessung offen).
- Unabhängig vom Weg: Die Uhren (Start, 0:30, 60-s-Sperren, Exit-Fenster, Höchstzeit) laufen serverseitig ([Hinweise, Abbruch und Exit](../../../concepts/framework/hinweise-abbruch-exit.md)), und das Handy muss nach jedem Wiederverbinden den vollen Zustand des Durchlaufs holen. Das betrifft [Ticket 05](../issues/05-durchlauf-modell-und-uhren.md).

## Offen, im Probe-Pod zu prüfen

- Hält eine WebSocket-Verbindung durch Envoy Gateway ohne HTTPRoute-Timeout länger als 15 s und länger als 5 min (mit Heartbeat)?
- Hält ein SSE-Stream mit `timeouts.request: "0s"` und Keepalive-Kommentar über 50 s (Octavia) und über 5 min?
- Welche Timeouts hat der Octavia-Listener vor `envoy-envoy-gateway-system-external-…` tatsächlich, und ist Response Compression aktiv?
- Wie schnell verliert ein gesperrtes iPhone (Safari) und ein Android-Handy (Chrome) die Verbindung, und kommt `visibilitychange`/`sendBeacon` beim Sperren noch an?
- Verhalten von `nuxi dev` mit `experimental.websocket` (aus dem Quelltext abgeleitet, nicht ausgeführt).
