# Wireframes: Admin-Panel

Quelle: Trilium „Wireframes – Logbuch-Flow“, Abschnitt „Admin-Panel“ · Stand 21.10.2026, Anmerkungen 23.10.2026 · [Übersicht](../README.md)

**Design-Canvas (Claude Design, privat):** [Wireframes – Logbuch-Flow](https://claude.ai/artifact/79z52NjAYibAu2gyE1iL34)

## Screens (Desktop-Ansicht, 5 Screens)

1. Nutzer: hinzufügen mit oder ohne Namen, interne ID, System-Code, QR-Code
2. Crews: Liste mit Status, Auflösen
3. Ziehung: 3., 2., 1. Platz, zufällig, mit Log-Werten neben dem Namen und „Neu ziehen“
4. Hinweis- und Exit-Log (mit Spalte „Übersprungen“)
5. Login (Nuxt-Standard)

In der Seitenleiste aller Screens: Schalter „Schatzsuche öffnen/schließen“. Noch nicht gezeichnet: die drei Zustände und der Button „Zurücksetzen“ mit doppelter Bestätigung ([Admin-Panel](admin-panel.md)).

## Annahmen und offene Punkte

1. Die Screens 2 bis 4 zeigen das alte Modell mit festen Crews und zufälliger Ziehung. Neu zu zeichnen: Screen 2 entfällt, Screen 3 wird die Rangliste nach Punkten (siehe [Siegerehrung](../framework/finale-und-ziehung.md#siegerehrung)), Screen 4 bekommt „Abbrüche“ statt „Übersprungen“.
2. Der Admin-Bereich ist nur als Desktop-Ansicht gezeichnet; die Handy-Ansicht folgt bei der Umsetzung.
