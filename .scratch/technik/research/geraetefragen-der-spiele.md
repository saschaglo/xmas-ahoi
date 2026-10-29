# Recherche: Gerätefragen der Spiele

Stand: 29.10.2026.

Frage aus [Ticket 14](../issues/14-geraetefragen-der-spiele.md): Was tragen iOS Safari und Chrome auf Android für die Oberflächen der Spielmodule, und welche Bibliothek braucht es dafür? Vier Punkte: Ton per Web Audio bei [Morsezeichen](../../../concepts/spiele/spiel-3-morsezeichen/morsezeichen.md) und der Stummschalter am iPhone; Drag & Drop der Wortliste per Finger; OpenStreetMap-Kacheln und eine Kartenbibliothek für [Peilung](../../../concepts/spiele/spiel-2-peilung/peilung.md); das gezeichnete Steuerrad bei [Fischfang](../../../concepts/spiele/spiel-4-fischfang/fischfang.md). Rahmen: Nuxt 4 (Vue 3), rund 60 Spieler auf eigenen Handys, „schlank und schnell“ ist Vorgabe. Laut [Spielmodul-Schnittstelle](../../../concepts/technik/spielmodul-schnittstelle.md) liegt alles davon in der Vue-Komponente des Moduls.

Diese Seite stellt Fakten zusammen und trifft keine Entscheidung; Bibliotheken wählt [Ticket 09](../issues/09-stack-und-projektstruktur.md). Quellen sind Primärquellen (MDN, WebKit- und Chromium-Bugtracker, Herstellerdoku, npm-Registry, Quelltext der Pakete). Was ich abgeleitet oder nicht auf einem Gerät geprüft habe, ist als **abgeleitet** oder **nicht verifiziert** markiert. Nichts davon ist auf einem echten Handy ausprobiert.

## Kernbefunde

1. **Morsezeichen: Ohne Zutun schweigt Web Audio am iPhone im Stumm-Modus.** WebKit behandelt einen `AudioContext` ohne Angabe als „ambient“, und iOS stummt ambient-Audio, wenn der Stummschalter an ist. `navigator.audioSession.type = 'playback'` hebt das auf; das empfehlen WebKit-Entwickler in zwei Bugs. Nebenwirkung: „playback“ pausiert anderes Audio auf dem Handy (Musik). Die API gibt es nur in Safari (ab 16.4), nicht in Chrome; auf Android gilt laut Herleitung die Medienlautstärke, nicht der Klingelmodus (**nicht verifiziert**).
2. **Drag & Drop per Finger geht nativ inzwischen in beiden Browsern, aber nur nach langem Drücken.** Chrome auf Android startet einen Drag auf `draggable` seit etwa Version 100 nach Long-Press, Safari auf dem iPhone seit iOS 15 (**nicht auf Gerät geprüft**). SortableJS schaltet den nativen Weg auf iOS und Chrome-Android ausdrücklich ab und nutzt eigene Pointer-/Touch-Behandlung. Schlankste Bibliothek für Vue: SortableJS direkt oder über `vue-draggable-plus` bzw. VueUse `useSortable`, rund 15 KB gzip.
3. **Peilung: tile.openstreetmap.org ist für normale, interaktive Nutzung erlaubt, ohne Zahlengrenze, aber ohne SLA und mit Sperre ohne Vorwarnung.** Pflicht: sichtbare Attribution, gültiger `Referer` (kein restriktiver `Referrer-Policy`), Browser-Cache nicht umgehen, kein Vorladen. Fallstrick: Das Modul `nuxt-security` setzt standardmäßig `Referrer-Policy: no-referrer`, was die Regel bricht. Die Standardkarte beschriftet Orte nur in Landessprache (z. B. „日本“), nicht auf Deutsch.
4. **Kartenbibliothek: Leaflet 1.9.4 ist rund 7-mal kleiner als MapLibre GL** (42 KB gegenüber rund 294 KB gzip, eigene Messung) und kann Rasterkacheln, Pinch-Zoom und ziehbare Pins ab Werk. Leaflet 1.9.4 ist von Mai 2023; Leaflet 2.0 steckt seit Mai 2025 im Alpha. MapLibre ist aktiv gepflegt, braucht aber WebGL2 und ist für Vektorkacheln gebaut.
5. **Fischfang: Laut Spielseite zeigt das Handy die Fische nur an; gedreht oder getippt wird am Steuerrad nichts.** Für die Anzeige reicht ein SVG mit `transform="rotate(…)"` in 60°-Schritten. Wäre das Rad doch bedienbar, tragen Pointer Events mit `touch-action: none` und `setPointerCapture` das ohne Bibliothek.

## Versionen

| Paket | Version | Datum | Größe gzip (eigene Messung¹) | Quelle |
|-------|---------|-------|------------------------------|--------|
| `sortablejs` | 1.15.7 | 11.02.2026 | 15,1 KB (`Sortable.min.js`) | [npm](https://registry.npmjs.org/sortablejs) |
| `vue-draggable-plus` | 0.6.1 | 11.01.2026 | 17,5 KB (ESM, unminifiziert, Sortable eingebaut) | [npm](https://registry.npmjs.org/vue-draggable-plus) |
| `vuedraggable` (Tag `next`, Vue 3) | 4.1.0 | 22.08.2021 | 18,1 KB (UMD min) | [npm](https://registry.npmjs.org/vuedraggable) |
| `@vueuse/integrations` (`useSortable`) | 15.0.0 | 16.09.2026 | Wrapper, `sortablejs@^1` als Peer | [npm](https://registry.npmjs.org/@vueuse/integrations) |
| `mobile-drag-drop` (Polyfill) | 3.0.0-rc.0 | 04.11.2022 | – | [npm](https://registry.npmjs.org/mobile-drag-drop) |
| `leaflet` | 1.9.4 (`latest`) | 18.05.2023 | 42,4 KB JS + 3,5 KB CSS | [npm](https://registry.npmjs.org/leaflet) |
| `leaflet` | 2.0.0-alpha.1 (`alpha`) | 16.08.2025 | 40,4 KB JS | [npm](https://registry.npmjs.org/leaflet) |
| `@vue-leaflet/vue-leaflet` | 0.10.1 | 16.06.2023 | Wrapper | [npm](https://registry.npmjs.org/@vue-leaflet/vue-leaflet) |
| `maplibre-gl` | 6.16.0 | 25.10.2026 | 294 KB (`maplibre-gl.mjs`) + 10 KB CSS; daneben eine Worker-Datei mit 147 KB² | [npm](https://registry.npmjs.org/maplibre-gl) |
| `pmtiles` | 4.5.0 | 10.08.2026 | – | [npm](https://registry.npmjs.org/pmtiles) |

¹ `gzip -9` der veröffentlichten Datei aus dem npm-Tarball. Was nach Tree-Shaking und Minifizierung durch Vite im Bundle landet, kann abweichen.
² Ob `maplibre-gl.mjs` den Worker schon enthält oder die Worker-Datei zusätzlich geladen wird: **nicht verifiziert**.

## 1. Morsezeichen: Ton per Web Audio

### Stummschalter am iPhone

- **Standard ist „ambient“, und ambient ist im Stumm-Modus still.** Jean-Yves Avenard (WebKit) im Bug „webaudio api is muted when the iOS ringer is muted“: „By default the type is `ambient` and so audio will be muted if the phone is muted.“ Seine Abhilfe: „Since iOS 17, you can set the audio session type to 'playback'. Add in your code something like `navigator.audioSession.type = "playback"`“ ([WebKit Bug 237322](https://bugs.webkit.org/show_bug.cgi?id=237322), Status RESOLVED CONFIGURATION CHANGED).
- Ein zweiter Bug („REGRESSION (iOS 16.3): WebAudio muted when the silent switch is ON …“) wurde als INVALID geschlossen. Youenn Fablet (WebKit): „The silent switch is silencing notifications so it might behave as expected“, und als Abhilfe `navigator.audioSession.type = 'playback'` („This should help for silent mode at least“). Ein Melder bestätigt: „Setting navigator.audioSession.type to 'playback' or 'play-and-record' helps.“ ([WebKit Bug 251532](https://bugs.webkit.org/show_bug.cgi?id=251532)).
- Die Spezifikation legt für einen `AudioContext` den Standardtyp „ambient“ fest, wenn `type` auf „auto“ steht ([W3C Audio Session, Editor's Draft 13.11.2024](https://w3c.github.io/audio-session/)). Den Stummschalter selbst erwähnt die Spezifikation nicht.

### `navigator.audioSession`

| | Safari (iOS und macOS) | Chrome (Android) | Firefox |
|---|---|---|---|
| `navigator.audioSession.type` | ab 16.4 laut MDN-Kompatibilitätsdaten; der WebKit-Entwickler in Bug 237322 nennt iOS 17 | nein | nur Preview |
| Status | experimentell, auf Standardspur | | |

Quelle: [MDN browser-compat-data, `api/AudioSession.json`](https://github.com/mdn/browser-compat-data/blob/main/api/AudioSession.json); [MDN, AudioSession.type](https://developer.mozilla.org/en-US/docs/Web/API/AudioSession/type). Der Widerspruch 16.4 gegen 17 ist **nicht aufgelöst**; bei Geräten mit iOS 17 oder neuer spielt er keine Rolle. Chrome, Edge und Firefox auf dem iPhone nutzen WebKit und bekommen die API mit (**abgeleitet** aus der WebKit-Pflicht auf iOS).

Was die Typen tun ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/AudioSession/type)):

- `playback`: „exclusive type that will pause other playback audio on the device“. Läuft beim Spieler Musik, stoppt sie, sobald der Morseton anspringt.
- `ambient`: mischt sich mit anderem Audio und folgt dem Stummschalter (siehe oben).
- `auto`: der Browser wählt; für Web Audio heißt das ambient.

Für Xmas Ahoi heißt das: Mit `playback` hört man den Ton auch im Stumm-Modus, aber laufende Musik pausiert. Mit dem Standard bleibt das Handy im Stumm-Modus still, und der Spieler sieht nur das Leuchtfeld. Setzen sollte man den Typ vor dem Erzeugen oder Fortsetzen des `AudioContext`; die API fehlt außerhalb von WebKit, ein Aufruf braucht also eine Prüfung auf `navigator.audioSession` (**abgeleitet**).

### Freischalten nach einem Tipp

- Chrome: „If an `AudioContext` is created before the document receives a user gesture, it will be created in the 'suspended' state, and you will need to call `resume()` after the user gesture.“ Welche Gesten zählen, sei browserübergreifend nicht einheitlich: „I'd recommend you stick to `"click"`“ ([Chrome, Autoplay policy](https://developer.chrome.com/blog/autoplay)).
- MDN: Ein Start über `start()` an einem Source-Node außerhalb einer Nutzereingabe unterliegt den Autoplay-Regeln ([MDN, Autoplay guide](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay)).
- iOS Safari: Verlässt der Nutzer die Seite (Tab-Wechsel, App-Wechsel, Bildschirm aus), wechselt der Kontext in den Zustand `"interrupted"` und muss mit `resume()` fortgesetzt werden ([MDN, BaseAudioContext.state](https://developer.mozilla.org/en-US/docs/Web/API/BaseAudioContext/state)). Nach dem Entsperren braucht der Ton also erneut einen Tipp (**abgeleitet**: `resume()` außerhalb einer Geste ist auf iOS nicht belegt).
- Der Umschalter „Ton an“ aus dem [Aufbau](../../../concepts/spiele/spiel-3-morsezeichen/morsezeichen.md#aufbau-empfehlung) ist selbst der Tipp, der den Kontext erzeugt oder fortsetzt.
- Testen geht nur am echten iPhone; Simulator und Desktop-Safari kennen keinen Stummschalter (**abgeleitet**).

### Android Chrome

- Einen Stummschalter wie am iPhone gibt es nicht; Android trennt Klingel- und Medienlautstärke (`STREAM_RING`, `STREAM_MUSIC`). Dass Web Audio in Chrome der Medienlautstärke folgt und vom Lautlos- oder Vibrationsmodus unberührt bleibt, fand ich in keiner Primärquelle: **nicht verifiziert**. Eine dokumentierte Ausnahme betrifft nur das Mikrofon: Nach `getUserMedia({audio:true})` steuern die Lautstärketasten die Anrufslautstärke ([Chromium Issue 443255997](https://issues.chromium.org/issues/443255997)); Morsezeichen nutzt kein Mikrofon.
- Für das Freischalten gilt dieselbe Autoplay-Regel wie oben (`resume()` nach dem Tipp).

## 2. Drag & Drop der Wortliste per Finger

### Nativ (HTML Drag and Drop)

| | Chrome (Android) | Safari (iOS) |
|---|---|---|
| Startet ein Finger einen Drag auf `draggable`? | Ja, nach Long-Press. blink-dev, „Simultaneous touch-drag and context-menu is getting enabled on Android 100+“: „Elements with a draggable attribute would start responding to touch-drag interaction“; vorher feuerten nach `dragstart` weder `drag` noch `dragend` ([blink-dev](https://www.mail-archive.com/blink-dev@chromium.org/msg03166.html); [Chromium Issue 40952307](https://issues.chromium.org/issues/40952307), Fixed) | Ja, ab iOS 15 laut Polyfill-README: „Chrome>=96 on Android>=7 and Safari on iOS/iPadOS>=15 are reported to support drag and drop natively“ ([mobile-drag-drop](https://github.com/timruffles/mobile-drag-drop)). Apple öffnete Drag & Drop auf dem iPhone mit iOS 15 systemweit ([WWDC21, What's new in UIKit](https://developer.apple.com/videos/play/wwdc2021/10059/)). Eine WebKit-Quelle für Long-Press → `dragstart` auf dem iPhone fand ich nicht: **nicht verifiziert** |
| Kontextmenü | Long-Press kann zugleich ein Kontextmenü öffnen; Chrome lässt beides nebeneinander zu (blink-dev, ebd.) | Long-Press auf Links zeigt die Link-Vorschau (**nicht verifiziert** für `draggable`-Listeneinträge) |
| MDN-Kompatibilitätsdaten | `draggable` und `dragstart` stehen auf „mirror“ (von Desktop übernommen) und sagen über Touch nichts | wie links |

SortableJS 1.15.7 schaltet den nativen Weg auf genau diesen beiden Plattformen ab: `supportDraggable = documentExists && !ChromeForAndroid && !IOS && 'draggable' in document.createElement('div')` (`Sortable.js`, Zeile 914, im [Paket 1.15.7](https://registry.npmjs.org/sortablejs/-/sortablejs-1.15.7.tgz)). Dort zieht SortableJS über `pointermove`/`touchmove` mit einem eigenen „Ghost“ (Zeilen 1388–1393). Pointer Events sind auf iOS ausdrücklich an: `supportPointer: … (!Safari || IOS)`, Kommentar „Enabled on Safari IOS: #2244“ (Zeile 1112 f.). Daraus folgt: Die Bibliothek mit der größten Verbreitung verlässt sich auf Handys nicht auf natives Drag & Drop (**abgeleitet** aus dem Quelltext).

### Bibliotheken für Vue 3

| Option | Kern | Pflege | Größe gzip¹ | Anmerkung |
|--------|------|--------|-------------|-----------|
| SortableJS direkt | eigene Touch-/Pointer-Behandlung | Release 1.15.7 am 11.02.2026 | 15,1 KB | keine Vue-Bindung; Reihenfolge per `onEnd` selbst ins Array schreiben |
| `vue-draggable-plus` 0.6.1 | SortableJS (eingebaut, einziger Import ist `vue`) | Release 11.01.2026; GitHub rund 4.000 Sterne, 101 offene Issues ([Repo](https://github.com/Alfred-Skyblue/vue-draggable-plus)) | 17,5 KB (unminifiziert) | Komponente, Composable und Direktive; Vue 3 und 2.7 |
| VueUse `useSortable` | SortableJS als Peer (`sortablejs@^1`) ([VueUse](https://vueuse.org/integrations/useSortable/)) | `@vueuse/integrations` 15.0.0 am 16.09.2026 | Sortable + dünner Wrapper | VueUse wäre eine neue Abhängigkeit (wie schon in [Echtzeit in Nuxt](echtzeit-in-nuxt.md) vermerkt) |
| `vuedraggable@next` 4.1.0 | SortableJS | letzter Release 22.08.2021 | 18,1 KB | laut README von vue-draggable-plus „has not been updated“ |
| Eigene Pointer Events | `pointerdown`/`pointermove`/`pointerup`, `setPointerCapture`, `touch-action: none` am Griff | – | 0 KB | Auto-Scroll am Listenrand, Ghost und Einfügeposition selbst bauen |
| Natives Drag & Drop + `mobile-drag-drop` | Polyfill | letzter Release 04.11.2022 (`3.0.0-rc.0`) | – | Chrome-Android-Verhalten laut README „under investigation“ |

¹ siehe Versionstabelle.

Zur Größe der Liste: Bei 5, 10 oder 15 Wörtern ([Morsezeichen](../../../concepts/spiele/spiel-3-morsezeichen/morsezeichen.md)) passt die Liste auf kleinen Handys eventuell nicht ganz auf den Bildschirm. Dann braucht der Drag ein Auto-Scroll am Rand; SortableJS bringt eines mit (Optionen `scroll`, `scrollSensitivity`), bei eigenen Pointer Events fehlt es (**abgeleitet** aus Quelltext und Optionen von SortableJS).

## 3. Peilung: Kacheln und Kartenbibliothek

### Tile Usage Policy von tile.openstreetmap.org

Wörtlich aus der [Tile Usage Policy](https://operations.osmfoundation.org/policies/tiles/) (ohne Datum auf der Seite):

- **Erlaubt:** „Normal interactive viewing by a human where the client requests only the tiles needed for the current viewport (with modest, short-range look-ahead typical of browsers).“ Eine Zahlengrenze für Nutzer oder Requests nennt die Policy nicht.
- **Kein Anspruch:** „Availability is best-effort: there is no SLA or guarantee.“ „We may block access, without notice, if your usage degrades the service.“
- **You must:** URL genau `https://tile.openstreetmap.org/{z}/{x}/{y}.png`; sichtbare Attribution, typisch „© OpenStreetMap contributors“ unten rechts, nicht hinter Menüs oder außerhalb des Bildes; „From web pages, ensure a valid HTTP Referer header is sent“; Kacheln nach HTTP-Cache-Headern cachen (oder mindestens 7 Tage).
- **You must not:** Kacheln vorab laden („Bulk download … or offer prefetch features“, auch „Preloading entire towns/regions or multiple zoom stacks ‚just in case‘“); `Cache-Control: no-cache` senden; „Set a restrictive Referrer-Policy that prevents the HTTP Referer header being sent“; `http://` statt `https://`.
- **User-Agent:** „Browsers will use the browser's default User-Agent.“ Für Webseiten genügt also der Browser; eigene User-Agents betreffen Apps und Proxys.
- „Modern browsers, with default settings, already satisfy these technical requirements.“
- Ein eigener Caching-Proxy davor wird „generally not recommend[ed]“; wer ihn betreibt, darf den `Referer` nicht entfernen und braucht einen eigenen, kontaktierbaren User-Agent.
- **Empfohlen:** URL nicht fest einbauen, sondern umschaltbar halten; Link zu „Report a map issue“; Kontakt-E-Mail auf der Seite.
- Für Vektorkacheln von `vector.openstreetmap.org` gilt eine eigene, inhaltlich gleiche [Vector Tile Usage Policy](https://operations.osmfoundation.org/policies/vector/) (Shortbread-Schema).

**Fallstrick Referer in Nuxt.** Browser senden ohne Vorgabe `strict-origin-when-cross-origin`, also den Origin der Seite als `Referer` an tile.openstreetmap.org ([MDN, Referrer-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Referrer-Policy)); das genügt der Policy. Das Modul `nuxt-security` setzt aber standardmäßig `Referrer-Policy: no-referrer` ([nuxt-security, Referrer-Policy](https://nuxt-security.vercel.app/headers/referrerpolicy)), was gegen die Policy verstößt. Leaflet kann je Kachel `referrerPolicy` am `<img>` setzen (TileLayer-Option `referrerPolicy`, [Leaflet 1.9.4 Reference](https://leafletjs.com/reference.html)).

**Last bei 60 Spielern (eigene Rechnung, grob).** Ein Handy zeigt bei 256-px-Kacheln etwa 12 bis 20 Kacheln je Ansicht; mit Zoomen und Schieben je Foto vielleicht 100 bis 200 Requests. 60 Spieler × 3 Fotos ergeben dann 18.000 bis 36.000 Kachel-Requests über den Abend, weniger durch den Browser-Cache und durch Wiederholungen nach einem Abbruch eher mehr. Ob das jemand „heavy“ nennt, sagt die Policy nicht; sie definiert erlaubte Nutzung über das Verhalten (nur sichtbare Kacheln, Cache respektieren), nicht über Mengen.

**Beschriftung.** Die Standardkarte (OpenStreetMap Carto) beschriftet nur mit dem Tag `name`, also in Landessprache und Landesschrift; eine Sprachwahl gibt es nicht ([OSM Wiki, OpenStreetMap Carto](https://wiki.openstreetmap.org/wiki/OpenStreetMap_Carto); [openstreetmap-carto RFC #5216](https://github.com/openstreetmap-carto/openstreetmap-carto/issues/5216)). Bei Fotos von Küsten in Japan, Griechenland oder Marokko sieht der Spieler Ortsnamen in Kanji, Griechisch oder Arabisch. Die [Spielseite](../../../concepts/spiele/spiel-2-peilung/peilung.md) verlangt „normale Ansicht mit Länder- und Ortsnamen“; ob Landesschrift dafür reicht, ist eine Frage an das Konzept.

### Alternativen

| Anbieter | Format | Beschriftung | Bedingungen | Quelle |
|----------|--------|--------------|-------------|--------|
| tile.openstreetmap.org | Raster PNG | Landessprache | siehe oben; kein SLA | [Policy](https://operations.osmfoundation.org/policies/tiles/) |
| tile.openstreetmap.de (Deutscher Kartenstil) | Raster PNG | „in German and local language (transliterated where no name in Latin script is available)“ | „Das Einbinden der Kacheln in eigene Webseites können wir … nur für nichtkommerzielle Zwecke erlauben. Kleinere Webanwendungen, wie z.B. der Anfahrtsplan auf der Firmenwebseite dürfen die Kacheln selbstverständlich verwenden.“ Drosselung je IP nach einer „bestimmte[n] Menge Kacheln“. | [openstreetmap.de, Deutscher Kartenstil](https://openstreetmap.de/germanstyle/); [OSM Wiki, Raster tile providers](https://wiki.openstreetmap.org/wiki/Raster_tile_providers) |
| OpenFreeMap | Vektor (OpenMapTiles-Schema) | Stil-abhängig | „no limits on the number of map views or requests“, kein Schlüssel; „I don't offer SLA guarantees“; Attribution „OpenFreeMap © OpenMapTiles Data from OpenStreetMap“ | [openfreemap.org](https://openfreemap.org/) |
| Selbst gehostet: Protomaps PMTiles | Vektor, eine Datei | Stil-abhängig | ODbL, OSM-Attribution; Planet rund 120 GB (z0–15), „Each additional zoom level roughly doubles the size“; Ausschnitt und `--maxzoom` per `pmtiles extract`; als statische Datei von jedem Server mit Range-Requests | [Protomaps, Downloads](https://docs.protomaps.com/basemaps/downloads) |

Zum Selbsthosten: Peilung braucht eine **Weltkarte**, keinen kleinen Ausschnitt. Ein Weltextrakt bis Zoom 6 oder 7 als PMTiles wäre klein genug für den Pod oder ein statisches Verzeichnis, die genaue Größe habe ich nicht gemessen: **nicht verifiziert**. Vektorkacheln brauchen im Browser einen Renderer (MapLibre oder `protomaps-leaflet`) und einen Stil. Rasterkacheln selbst zu rendern braucht einen eigenen Render-Stack ([switch2osm](https://switch2osm.org/)); für die Welt bis Zoom 6 wären das 5.461 Kacheln (Summe 4^z für z = 0 bis 6, eigene Rechnung).

### Kartenbibliothek

| | Leaflet 1.9.4 | Leaflet 2.0.0-alpha.1 | MapLibre GL JS 6.16.0 |
|---|---|---|---|
| Größe gzip (eigene Messung) | 42 KB JS + 3,5 KB CSS | 40 KB JS | ~294 KB JS + 10 KB CSS (+ Worker, siehe oben) |
| Rendering | DOM (`<img>`-Kacheln) | DOM | WebGL2, „Interactive vector tile maps in the browser“ ([Repo](https://github.com/maplibre/maplibre-gl-js)) |
| Rasterkacheln (OSM PNG) | ja, `TileLayer` | ja | möglich als Raster-Quelle im Stil (**nicht verifiziert** in der Doku dieser Version) |
| Touch | `touchZoom` (Pinch), Doppeltipp, `tapHold` simuliert `contextmenu` auf Mobile Safari; Marker `draggable`: „Whether the marker is draggable with mouse/touch or not.“ ([Reference 1.9.4](https://leafletjs.com/reference.html)) | „Switched from `Mouse` and `Touch` events to `PointerEvents`“ ([Leaflet-Blog, 18.05.2025](https://leafletjs.com/2025/05/18/leaflet-2.0.0-alpha.html)) | eingebaut (Pinch, Drehen, Neigen) |
| Pflege | letzter stabiler Release Mai 2023 | Alpha seit Mai 2025, kein Termin für stabil | Release 25.10.2026 |
| Vue-Bindung | `@vue-leaflet/vue-leaflet` 0.10.1 (Juni 2023), oder Leaflet direkt in `onMounted` | – | keine offizielle; direkt in `onMounted` |
| SSR | Leaflet greift beim Import auf `window` zu; in Nuxt nur clientseitig laden (`<ClientOnly>` oder dynamischer Import) (**abgeleitet**) | wie links | wie links (**abgeleitet**) |

## 4. Fischfang: gezeichnetes Steuerrad

**Was die Spielseite verlangt.** Das Handy „zeigt 3 Fische nebeneinander, gezeichnet wie im Teich“; der Spieler angelt und tippt den 4-stelligen Code ein ([Fischfang](../../../concepts/spiele/spiel-4-fischfang/fischfang.md)). Das Rad wird nur angezeigt; Drehen oder Antippen kommt im Konzept nicht vor. Für die Anzeige reicht inline SVG in der Vue-Komponente: ein Rad mit 6 Speichen, die markierte Speiche per `transform="rotate(60 * k, cx, cy)"` auf 12, 2, 4, 6, 8 oder 10 Uhr, die Farbe als `fill`. Keine Bibliothek, kein Event-Handling (**abgeleitet**).

**Falls das Rad doch bedienbar würde** (Drehen per Finger), gilt:

- **Pointer Events** sind in iOS Safari ab 13 und Chrome ab 55 da ([MDN browser-compat-data, `PointerEvent`](https://github.com/mdn/browser-compat-data/blob/main/api/PointerEvent.json)).
- **`touch-action`**: Ohne Angabe übernimmt der Browser Schieben und Pinch, und die Seite bekommt `pointercancel`, „when the browser starts handling a touch gesture“. `touch-action: none` schaltet das für das Element ab ([MDN, touch-action](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/touch-action)). Safari iOS kennt `touch-action` seit 9.3 (nur `auto`/`manipulation`), `none` seit 13 ([MDN browser-compat-data](https://github.com/mdn/browser-compat-data/blob/main/css/properties/touch-action.json)). `touch-action` gilt laut Definition nicht für nicht-ersetzte Inline-Elemente; man setzt es daher am `<svg>` oder an einem umgebenden Block, nicht an inneren Formen (**abgeleitet**).
- **Passive Listener**: `touchstart` und `touchmove` an `window`, `document`, `documentElement` und `body` sind standardmäßig passiv; `preventDefault()` wirkt dort nicht ([MDN, addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)). Mit Pointer Events am Element und `touch-action: none` braucht es kein `preventDefault()` auf Touch-Events.
- **`setPointerCapture(pointerId)`** im `pointerdown` hält alle weiteren Events dieses Fingers am Rad, auch wenn er es verlässt ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/setPointerCapture)).
- **Rechnung**: Winkel = `Math.atan2(y − cy, x − cx)` relativ zum Mittelpunkt aus `getBoundingClientRect()`, Differenz zum Startwinkel aufaddieren, beim Loslassen auf 60° runden (6 Speichen). Bei `viewBox` und Skalierung rechnet man in Bildschirmkoordinaten des Mittelpunkts, nicht in SVG-Einheiten (**abgeleitet**).

## Offen, am Gerät zu prüfen

- iPhone mit Stummschalter an: Spielt der Ton mit `navigator.audioSession.type = 'playback'`, und stoppt dabei laufende Musik? Ab welcher iOS-Version (16.4 oder 17)?
- iPhone nach Sperren und Entsperren: Steht der `AudioContext` auf `interrupted`, und reicht ein Tipp auf den Ton-Umschalter zum Fortsetzen?
- Android im Lautlos- und Vibrationsmodus: Spielt Web Audio, solange die Medienlautstärke oben ist?
- Wortliste mit 15 Einträgen auf einem kleinen iPhone und einem Android-Handy: Verhalten von SortableJS (Long-Press-Verzögerung, Auto-Scroll, Scrollen der Seite neben der Liste).
- Peilung: Sendet die fertige Nuxt-App einen `Referer` an tile.openstreetmap.org (Response-Header der App prüfen)? Wie sehen Orte in Landesschrift auf der Karte für die Spieler aus?
- Größe eines PMTiles-Weltextrakts bis Zoom 6 oder 7, falls Selbsthosten in Frage kommt.
