# Spiel 4 – Fischfang

Stand 27.10.2026 · [Übersicht](../../README.md) · [Spielübergreifend](../uebergreifend.md)

Die „Crew“ sind die Spieler, die einen Durchlauf der Station gemeinsam spielen, auch eine einzelne Person (siehe [Glossar](../../../CONTEXT.md)). „Beschreibung“ und „Variationen“ sind die Vorgabe des Veranstalters. „Aufbau (Empfehlung)“ und „Offene Fragen“ stammen aus der Durchsicht und sind noch nicht entschieden.

## Beschreibung

Jede Person der Crew setzt auf dem Handy eine Reihe von Fischen fort und angelt den fehlenden Fisch aus dem Fischteich. Fischfang ist ein Rätsel und verbindet Knobeln mit Geschicklichkeit. Ort, Ortsabfrage („Bist du am Fischteich?“) und Nachsehen regelt [Spiele mit Material im Raum](../uebergreifend.md#spiele-mit-material-im-raum).

**Am Fischteich:**

- Eine Wanne mit Wasser, im Gastraum oder notfalls draußen.
- 24 schwimmende Fische, die sich von selbst aufrichten: oben eine Öse, unten ein 4-stelliger Code, der im Wasser nicht zu sehen ist.
- Jeder Fisch hat eine von 4 Farben und oben ein Steuerrad mit 6 Speichen, von denen eine markiert ist. Die Stellung der markierten Speiche liest man wie eine Uhr: 12, 2, 4, 6, 8 oder 10 Uhr. 4 Farben mal 6 Stellungen ergeben 24 Fische, jeden genau einmal.
- Angeln mit Haken. Gefangen ist ein Fisch, wenn der Haken in seiner Öse hängt. Fische werden nur mit der Angel gefangen, die Hände bleiben draußen.

**Die Reihe:** Das Handy zeigt 3 Fische nebeneinander, gezeichnet wie im Teich, dahinter ein „?“. Darüber steht: „Der Smutje sortiert den Fang nach einem Muster. Welcher Fisch kommt als Nächstes?“ Für das Steuerrad und für die Farbe gilt je eine Regel:

| Steuerrad | Farbe |
|-----------|-------|
| dreht 1 Speiche weiter | bleibt gleich |
| dreht 2 Speichen weiter | wechselt ab (rot, blau, rot → blau) |
| dreht gegen den Uhrzeigersinn | die fehlende der 4 (rot, blau, gelb → grün) |
| bleibt stehen | |

Es ändern sich höchstens zwei Dinge zugleich, und die Reihen werden mit jedem Fang etwas schwerer:

```
Fang 1  rot 12   rot 2    rot 4    ?  -> rot 6      Rad +1 Speiche, Farbe bleibt
Fang 2  blau 12  gelb 2   blau 4   ?  -> gelb 6     Rad +1, Farbe abwechselnd
Fang 3  rot 12   blau 4   gelb 8   ?  -> grün 12    Rad +2, die fehlende Farbe
```

Der Server erzeugt jede Reihe neu, und zwar so, dass genau ein Fisch passt. Er wählt dabei nur Fische, die gerade im Teich schwimmen und nach denen niemand sonst sucht.

**Fangen:** Die Person angelt den Fisch und gibt den Code von seiner Unterseite ein. Die App antwortet so:

- Richtiger Fisch: „Richtig! Leg den Fisch neben den Teich.“ Dann erscheint die nächste Reihe.
- Anderer Fisch: „Das ist nicht der gesuchte Fisch. Zurück ins Wasser.“ Die Reihe bleibt stehen. Versuche sind unbegrenzt.
- Unbekannter Code: „Diesen Code gibt es nicht.“ Das fängt Tippfehler ab.

Jede Person fängt 3 Fische nacheinander. Gefangene Fische bleiben neben dem Teich, bis die Crew fertig ist. Nach dem Ende der Station oder einem Abbruch zeigt die App allen in der Crew: „Legt eure Fische zurück in den Teich.“ Ab da zählt der Server die Fische wieder als im Teich.

**Fisch fehlt:** Unter jeder Reihe steht der Knopf „Fisch nicht im Teich“. Er zeigt „Sag bitte jemandem aus dem Orgateam Bescheid“ und stellt danach eine neue Reihe für diesen Fang. Der Knopf meldet nichts, er wird nicht geloggt und erscheint nicht im [Admin-Panel](../../admin/admin-panel.md).

**Hinweise und Punkte:** Fischfang ist ein Rätsel ohne Zeitlimit. Hinweise und Exit folgen dem gemeinsamen Takt aus [Hinweise, Abbruch und Exit](../../framework/hinweise-abbruch-exit.md) und gelten für die laufende Reihe der Person, die den Hinweis nimmt. Hinweis 1 nennt eine der beiden Regeln („Das Rad dreht sich um 2 Speichen“). Hinweis 2 zeigt den gesuchten Fisch. Die Punkte richten sich nach den genommenen Hinweisen wie bei jedem Rätsel ([Spieltypen und Punkte](../uebergreifend.md#spieltypen-und-punkte)). Nach einem Abbruch bekommt die Crew neue Reihen.

## Variationen

- Vom Veranstalter keine genannt.

## Requisiten

- Wanne für den Fischteich, Wasser, Handtücher als Unterlage.
- 24 schwimmende, selbstaufrichtende Fische mit Öse, Code, Farbe und Steuerrad, dazu Ersatzfische beim Orgateam.
- 3 Angeln mit Haken und 1 Ersatzangel.
- Schild am Teich: „Nur mit der Angel fangen. Fische nach dem Spiel zurück in den Teich.“

## Aufbau (Empfehlung)

Die 12-Uhr-Stellung zeigt zum Kopf des Fisches. So liest jede Person das Rad gleich, egal wie der Fisch im Wasser treibt, und die App zeichnet die Fische in derselben Lage.

Wasser im Gastraum sollte mit dem Rudolph's abgesprochen sein, zusammen mit der übrigen Deko bis zum 13.11.

Grobe Schätzung: Knobeln und Angeln dauern je Fang etwa 40 bis 80 Sekunden, 3 Fänge also 2 bis 4 Minuten. Daran sollte sich der Testlauf orientieren.

Zurücksetzen: Die Crew legt ihre Fische zurück. Das Orgateam zählt beim Rundgang die Fische und ersetzt fehlende.

## Offene Fragen und Widersprüche

1. Fänge je Person: Reichen 3 Fänge in die 5 Minuten je Spiel, und sind Steuerrad und markierte Speiche im Wasser gut zu erkennen? Das klärt der Testlauf.
