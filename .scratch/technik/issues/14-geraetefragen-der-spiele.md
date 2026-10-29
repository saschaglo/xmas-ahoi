# 14 Gerätefragen der Spiele

Type: research
Status: resolved
Assignee: Sascha Glover
Blocked by: –

## Question

Was tragen mobile Browser (iOS Safari, Chrome auf Android) für die Oberflächen der Spielmodule, und welche Bibliothek braucht es dafür? Im Einzelnen: Ton per Web Audio nach einem Tipp bei Morsezeichen und ob der Stummschalter am iPhone ihn unterdrückt; Drag & Drop einer Wortliste per Finger (nativ oder Bibliothek für Vue); OpenStreetMap-Kacheln für Peilung bei rund 60 Spielern nach den Nutzungsregeln der OSM-Kachelserver und eine schlanke Kartenbibliothek; ein gezeichnetes Steuerrad mit markierter Speiche für Fischfang (SVG genügt?).

## Kontext

- [Spielmodul-Schnittstelle](../../../concepts/technik/spielmodul-schnittstelle.md): Alles davon liegt in der Vue-Komponente des Moduls; die Basis braucht nichts dafür.
- Spielseiten [Morsezeichen](../../../concepts/spiele/spiel-3-morsezeichen/morsezeichen.md), [Peilung](../../../concepts/spiele/spiel-2-peilung/peilung.md), [Fischfang](../../../concepts/spiele/spiel-4-fischfang/fischfang.md).
- Die Antwort fließt in [Stack und Projektstruktur](09-stack-und-projektstruktur.md) (Bibliotheken).
- Ergebnis: [research/geraetefragen-der-spiele.md](../research/geraetefragen-der-spiele.md)

## Answer

Details und Quellen in [research/geraetefragen-der-spiele.md](../research/geraetefragen-der-spiele.md). Fakten, keine Entscheidung; nichts davon ist auf einem Handy getestet:

- Morsezeichen: Web Audio gilt in Safari ohne Angabe als „ambient“ und bleibt bei eingeschaltetem Stummschalter still; WebKit sieht das als gewollt. `navigator.audioSession.type = 'playback'` (Safari ab 16.4, laut WebKit-Entwickler iOS 17) macht den Ton hörbar, pausiert aber laufende Musik. Chrome hat die API nicht; auf Android gilt vermutlich die Medienlautstärke (nicht belegt). Der `AudioContext` muss im Tipp erzeugt oder per `resume()` fortgesetzt werden; iOS setzt ihn nach dem Verlassen der Seite auf `interrupted`.
- Wortliste: Natives Drag & Drop startet per Finger nur nach langem Drücken (Chrome Android ab etwa 100, iOS ab 15, auf dem iPhone nicht belegt). SortableJS schaltet den nativen Weg auf iOS und Chrome-Android ab und zieht über eigene Pointer-Events. Schlankste Wege: SortableJS 1.15.7 direkt (15 KB gzip), `vue-draggable-plus` 0.6.1 oder VueUse `useSortable`; `vuedraggable@next` ist seit 2021 ohne Release.
- Peilung: tile.openstreetmap.org erlaubt normale interaktive Nutzung ohne Zahlengrenze, ohne SLA, Sperre ohne Vorwarnung. Pflicht sind sichtbare Attribution, gültiger `Referer`, Browser-Cache, kein Vorladen. `nuxt-security` setzt standardmäßig `Referrer-Policy: no-referrer` und bräche das. Die Standardkarte beschriftet nur in Landessprache; der deutsche Stil (tile.openstreetmap.de) beschriftet deutsch, erlaubt aber nur nichtkommerzielle und kleine Webanwendungen.
- Kartenbibliothek: Leaflet 1.9.4 hat 42 KB gzip, Pinch-Zoom und ziehbare Marker, letzter stabiler Release Mai 2023 (2.0 im Alpha). MapLibre 6.16 ist aktiv gepflegt, hat rund 294 KB gzip, braucht WebGL2 und ist für Vektorkacheln gebaut.
- Fischfang: Laut Spielseite wird das Rad nur angezeigt; dafür reicht inline SVG mit `rotate()` in 60°-Schritten. Ein drehbares Rad ginge mit Pointer Events, `touch-action: none` und `setPointerCapture` ohne Bibliothek.
