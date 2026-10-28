# Wireframes: Admin-Panel

Stand 28.10.2026 · [Übersicht](../README.md)

Graue Low-Fi-Wireframes (nur Positionierung der Elemente, keine Optik) für alle Zustände des [Admin-Panels](admin-panel.md). Die Screens der mobilen Webseite stehen unter [Wireframes: Spieler-Flow](../framework/wireframes-spieler-flow.md).

**Design-Canvas (Claude Design, privat):** [Xmas Ahoi – Admin-UI Wireframes](https://claude.ai/artifact/HqQGs9JAw7bTgbU13wTDWM). Das ältere Canvas [Wireframes – Logbuch-Flow](https://claude.ai/artifact/79z52NjAYibAu2gyE1iL34) enthält noch fünf Admin-Screens des überholten Modells und dient nur als Layoutvorlage.

## Aufbau des Canvas

- 31 Screens in 5 Zeilen, nur als Desktop-Ansicht. Jede Zeile ist ein Bereich, der Hauptweg läuft von links nach rechts.
- Jeder Screen hat eine Nummer (Zeile.Screen). Der Zettel unter dem Screen nennt das Ziel und die Bedingung. Die Pfeile zwischen den Screens werden im Canvas von Hand gezogen.
- Alle Screens außer dem Login haben dieselbe Seitenleiste: Navigation (Schatzsuche, Nutzer, Siegerehrung, Log) und darunter Zustand und Schalter der Schatzsuche.

## Screens

### 1. Zugang

- 1.1 Login (Nuxt-Standard). Weiter: Anmelden → Schatzsuche (2.1, 2.2 oder 2.3, je nach Zustand). Fehler → 1.2.
- 1.2 Login fehlgeschlagen. Weiter: Neuer Versuch → 1.1.

### 2. Schatzsuche und Zurücksetzen

- 2.1 Zustand „nicht eröffnet“ (Schalter „Schatzsuche öffnen“, Zurücksetzen aktiv). Weiter: Schatzsuche öffnen → 2.9. Zurücksetzen → 2.4.
- 2.2 Zustand „offen“ (Schalter „Schatzsuche schließen“, Zurücksetzen gesperrt, Hinweis auf den harten Schnitt). Weiter: Schatzsuche schließen → 2.10.
- 2.3 Zustand „beendet“ (Schalter „Wieder öffnen“, Zurücksetzen aktiv). Weiter: Wieder öffnen → 2.11. Zurücksetzen → 2.4.
- 2.4 Zurücksetzen, 1. Bestätigung. Weiter: Ja, weiter → 2.5. Abbrechen → 2.3.
- 2.5 Zurücksetzen, 2. Bestätigung mit Datumsfeld (leer, Knopf gesperrt). Weiter: Datum eintippen → 2.6 oder 2.7. Abbrechen → 2.3.
- 2.6 Zurücksetzen, falsches Datum. Weiter: Korrigieren → 2.7.
- 2.7 Zurücksetzen, Datum stimmt (Knopf aktiv). Weiter: Endgültig zurücksetzen → 2.8.
- 2.8 Zurückgesetzt (Zustand „nicht eröffnet“, Nutzer und Codes bleiben). Weiter: weiter mit 2.1.
- 2.9 Rückfrage „Schatzsuche öffnen?“. Weiter: Öffnen → 2.2. Abbrechen → 2.1.
- 2.10 Rückfrage „Schatzsuche schließen?“ (nennt den harten Schnitt und fragt, ob das Orgateam das Ende angekündigt hat). Weiter: Schließen → 2.3. Abbrechen → 2.2.
- 2.11 Rückfrage „Schatzsuche wieder öffnen?“. Weiter: Wieder öffnen → 2.2. Abbrechen → 2.3.

### 3. Nutzer

- 3.1 Nutzerliste (interne ID, Name, Code, QR-Code, Löschen). Weiter: Nutzer hinzufügen → 3.2. Löschen → 3.4.
- 3.2 Nutzer hinzufügen (Vorname optional). Weiter: Anlegen → 3.3. Abbrechen → 3.1.
- 3.3 Nutzer angelegt (Code, interne ID, QR-Code als Platzhalter). Weiter: weiter mit 3.1.
- 3.4 Nutzer löschen, Rückfrage. Weiter: Löschen oder Abbrechen → 3.1.

### 4. Siegerehrung

- 4.1 Rangliste (Punkte, Im Lostopf, Hinweise, Exits, Zeitabläufe, Abbrüche). Weiter: Ziehung → 4.2. Ehrungen → 4.5.
- 4.2 Ziehung, noch nichts gezogen. Weiter: Platz 3 ziehen → 4.3.
- 4.3 Ziehung, Platz 3 gezogen (mit Werten und „Neu ziehen“). Weiter: Neu ziehen → neue Person. Platz 2 ziehen → wie 4.3 mit Platz 2.
- 4.4 Ziehung, alle drei Plätze gezogen. Weiter: Zu den Ehrungen → 4.5.
- 4.5 Ehrungen (6 Ehrungen mit Genannten, Pechvogel entfällt als Beispiel). Weiter: Gleichstand → 4.6.
- 4.6 Ehrung bei Gleichstand (Preis wird unter allen Genannten gelost). Weiter: weiter mit 4.5.

### 5. Log

- 5.1 Log, alle Durchläufe (Start, Spiel, Crew, Status, Dauer, Hinweise; Dropdown „Person“). Weiter: Startzeit anklicken → Details (5.4 bis 5.8). Dropdown → 5.2.
- 5.2 Log, Person wählen (Dropdown geöffnet, gleiche Namen mit interner ID). Weiter: Person wählen → 5.3.
- 5.3 Log, nach Person gefiltert (jeder Durchlauf, an dem die Person allein oder in einer Crew teilgenommen hat). Weiter: Startzeit anklicken → Details. „Alle Personen“ → 5.1.
- 5.4 Details eines Rätsels mit Exit (Zeitleiste: Start, Hinweise, Exit-Vorschlag, Zustimmungen, Ende). Weiter: zurück zum Log.
- 5.5 Details eines Abbruchs. Weiter: zurück zum Log.
- 5.6 Details eines Zeitablaufs. Weiter: zurück zum Log.
- 5.7 Details eines normal beendeten Durchlaufs. Weiter: zurück zum Log.
- 5.8 Details eines verfallenen Durchlaufs (mit Verbindungsverlust und Verfall beim Schließen, Status „verfallen“). Weiter: zurück zum Log.

## Festlegungen

- Der Schalter „Schatzsuche öffnen/schließen“ fragt bei jedem Wechsel nach (2.9 bis 2.11).
- Die Ziehung läuft in der Reihenfolge Platz 3, 2, 1. Die Rangliste hat eine Spalte „Im Lostopf“. Das Log ist eine Tabelle der Durchläufe mit Details je Durchlauf und einem Filter nach Person ([Admin-Panel](admin-panel.md)). Ein beim Schließen verfallener Durchlauf bleibt mit dem Status „verfallen“ im Log; ein Verbindungsverlust (Sperre) steht mit Zeit und Person in den Details. Der Umfang der Ereignisse in den Details je Spiel ist ein Vorschlag. Der Text der Löschen-Rückfrage und die Meldung bei falschem Login stehen so im Wireframe.
- Es gibt kein Dashboard mit Zahlen, weil das Konzept keins vorsieht.

## Offene Punkte

1. Der Admin-Bereich ist nur als Desktop-Ansicht gezeichnet; die Handy-Ansicht legt die Umsetzung fest ([Admin-Panel](admin-panel.md#offene-punkte)).
