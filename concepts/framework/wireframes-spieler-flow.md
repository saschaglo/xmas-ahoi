# Wireframes: Spieler-Flow

Stand 29.10.2026 · [Übersicht](../README.md)

Graue Low-Fi-Wireframes (nur Positionierung der Elemente, keine Optik) für alle Zustände der mobilen Webseite. Maßgeblich für den Inhalt sind die [Crews](crews.md), [Karte und Stationen](karte-und-stationen.md), [Hinweise, Abbruch und Exit](hinweise-abbruch-exit.md), [Finale](finale-und-ziehung.md) und die Seiten der sechs [Spiele](../spiele/uebergreifend.md). Die Screens des Admin-Panels stehen unter [Wireframes: Admin-Panel](../admin/wireframes-admin.md).

**Design-Canvas (Claude Design, privat):** [Xmas Ahoi – Spieler-UI Wireframes](https://claude.ai/artifact/SXzVmKTchxDyXwUeyriB7G). Das ältere Canvas [Wireframes – Logbuch-Flow](https://claude.ai/artifact/79z52NjAYibAu2gyE1iL34) zeigt das überholte Modell mit festen Crews und dient nur noch als Layoutvorlage.

## Aufbau des Canvas

- 78 Screens in 13 Zeilen. Jede Zeile ist eine Phase, der Hauptweg läuft von links nach rechts, Abzweige liegen als eigene Screens in derselben Zeile.
- Jeder Screen hat eine Nummer (Zeile.Screen). Der Zettel unter dem Screen nennt das Ziel und die Bedingung. Die Pfeile zwischen den Screens werden im Canvas von Hand gezogen.
- Es gibt kein Menü. Das Konzept sieht keins vor.
- Crewgröße: Im Quiz (Zeile 4) sind Allein, zu zweit und zu dritt einmal gezeichnet. Danach zeigen alle Screens eine Crew aus 3 Personen (Peter, Anna, Tom). Allein-Varianten gibt es nur dort, wo sich der Ablauf ändert: Regelkarte (4.2) und Exit ohne Zustimmung (9.8).
- Dieselbe Meldung wird nur einmal gezeichnet, wenn sie in mehreren Spielen gleich ist: Countdown (5.1), Zeit abgelaufen (5.4), Warte auf deine Crew (5.5, 9.11), Schlüssel erhalten (5.7), Ortsabfrage (3.1, 3.2), Abbruch (6.1 bis 6.3) und die Rätsel-Screens von Zeile 9.

## Screens

### 1. Einstieg

- 1.1 Check-in per QR-Code (Begrüßung „Willkommen an Bord, Peter!“, Fairness-Satz). Weiter: Einchecken → Schatzkarte (2.1). Hat der Nutzer keinen Namen: zuerst 1.3.
- 1.2 Check-in per Zugangscode (4 Buchstaben von der Bordkarte). Weiter: Einchecken → Schatzkarte (2.1). Falscher Code → 1.4. Zu viele Versuche → 1.5.
- 1.3 Name wählen (Overlay „Wie heißt du?“). Weiter: Schatzkarte (2.1).
- 1.4 Code nicht erkannt. Weiter: Neuer Versuch → 1.2.
- 1.5 Zu viele Versuche (Rate-Limit). Weiter: Nach der Wartezeit → 1.2.

### 2. Schatzkarte

- 2.1 Schatzkarte, Beginn (6 offene Stationen, Schlüsselleiste 0 von 6). Weiter: Station antippen → Ortsabfrage (3.1) bei Raumspielen, sonst Allein oder Crew (4.1).
- 2.2 Schatzkarte mit Fortschritt (beendete Stationen, Schlüsselleiste). Weiter: Offene Station → wie 2.1. Beendete Station → „Du warst hier schon mal“ (4.9). Sechs Schlüssel → Truhe (13.1).

### 3. Ortsabfrage bei Raumspielen (Beispiel Fischfang)

- 3.1 „Bist du am Fischteich?“ (Ja, Nein). Weiter: Ja → Allein oder Crew (4.1). Nein → 3.2.
- 3.2 Nein: Weg zum Ort („Den Fischteich findest du …, sonst frag jemanden aus dem Orgateam“). Weiter: Zur Schatzkarte → 2.1.

### 4. Station beginnen (Beispiel Quiz, einmal mit allen Crewgrößen)

- 4.1 Allein oder Crew (Allein spielen, Crew bilden, Crewcode eingeben). Weiter: Allein spielen → 4.2. Crew bilden → 4.3. Crewcode eingeben → 4.5.
- 4.2 Allein, Regelkarte („Bin bereit“). Weiter: Bin bereit → Countdown (5.1).
- 4.3 Crew bilden, zu zweit (Crewcode, Mitglieder mit Bereit, freier Platz, Start gesperrt). Weiter: Alle bereit, Gründer tippt Start → Countdown (5.1). Dritte Person tritt bei → 4.4.
- 4.4 Crew bilden, zu dritt (alle bereit, Start möglich). Weiter: Start → Countdown (5.1).
- 4.5 Crewcode eingeben. Weiter: Beitreten → 4.6. Falscher Code → 4.7. Crew voll → 4.8. Station schon beendet → 4.9. Läuft anderswo → 4.10.
- 4.6 Im Crew-Raum warten (Sicht des Mitglieds, kein Start-Knopf). Weiter: Gründer startet → Countdown (5.1).
- 4.7 Code unbekannt. Weiter: Neuer Versuch → 4.5.
- 4.8 „Die Crew ist voll“. Weiter: Crew bilden oder Allein spielen → 4.1. Zur Schatzkarte → 2.1.
- 4.9 „Du warst hier schon mal“. Weiter: Zur Schatzkarte → 2.1.
- 4.10 „Du spielst gerade eine andere Station“. Weiter: Zurück zur laufenden Station. Zur Schatzkarte → 2.1.

### 5. Quiz spielen (3er-Crew)

- 5.1 Countdown (gemeinsamer Start für alle Spiele). Weiter: Nach „Los!“ → Frage (5.2).
- 5.2 Frage (Kategorie, 4 Antworten, Restzeit, „Station abbrechen“). Weiter: Antwort tippen → nächste Frage. Fünfte Frage beantwortet → 5.5. Zeit um → 5.4. Abbrechen → 6.1.
- 5.3 Schätzfrage (4 Zahlenbereiche). Weiter: Wie 5.2.
- 5.4 Zeit abgelaufen (Meldung für alle Punktespiele). Weiter: Ergebnis (5.6).
- 5.5 Warte auf deine Crew (Punktespiel). Weiter: Alle fertig oder Zeit um → Ergebnis (5.6).
- 5.6 Ergebnis („x von y richtig“, ohne richtige Antworten). Weiter: Schlüssel erhalten (5.7).
- 5.7 Schlüssel erhalten (gleich nach jedem Spiel, auch bei Exit und Zeitablauf, ohne Punkteanzeige). Weiter: Zur Schatzkarte → 2.2.

### 6. Abbruch und Verbindungsverlust (alle Spiele)

- 6.1 Station abbrechen, Rückfrage (gilt für die ganze Crew). Weiter: Abbrechen → bei allen 6.2. Weiterspielen → zurück zum Spiel.
- 6.2 Abbruch durch ein Crew-Mitglied (kein Schlüssel, keine Punkte, Station bleibt offen). Weiter: Zur Schatzkarte → 2.1.
- 6.3 Sperre „Tom ist gerade nicht an Bord. Wir warten auf ihn …“ (mit „Station abbrechen“). Weiter: Mitglied kehrt zurück → weiter im Spiel. Abbrechen → 6.1.

### 7. Peilung (Punktespiel, 3er-Crew)

- 7.1 Regelkarte der Crew (alle bereit, Start). Weiter: Start → Countdown (5.1), dann Foto (7.2).
- 7.2 Foto mit Karte, ohne Pin (Abgeben gesperrt). Weiter: Pin setzen → 7.3. Zeit um → wie 5.4. Abbrechen → 6.1.
- 7.3 Foto mit Karte, Pin gesetzt. Weiter: Abgeben → nächstes Foto (7.2). Dritte Abgabe → Warte-Screen (wie 5.5) oder Ergebnis (7.4).
- 7.4 Ergebnis je Foto (Entfernung in km, nicht der echte Ort). Weiter: Schlüssel erhalten (5.7).

### 8. Morsezeichen (Rätsel, 3er-Crew, Höchstzeit 5 Minuten)

- 8.1 Regelkarte der Crew (15 Wörter, Höchstzeit, 2 Hinweise). Weiter: Start → Countdown (5.1), dann Spielscreen (8.2).
- 8.2 Spielscreen (Leuchtfeld mit Positionsnummer, Ton und Tempo, Eingabe, Liste, Satz prüfen). Weiter: Falsches Wort → 8.3. Hinweis → Rückfrage (9.1). Alle Wörter gelöst → 8.5.
- 8.3 Falsches Wort. Weiter: Neuer Versuch (unbegrenzt) → 8.2.
- 8.4 Hinweis 1 genommen (erster Buchstabe in jedem Wort, Sperre bis Hinweis 2). Weiter: Hinweis 2 zeigt zusätzlich jeden zweiten Buchstaben. Exit → 9.3.
- 8.5 Alle Wörter gelöst („Satz prüfen“ aktiv). Weiter: Richtig → 8.7. Falsch → 8.6.
- 8.6 Satz falsch (richtige Wörter an der richtigen Stelle markiert). Weiter: Wörter tauschen, wieder prüfen → 8.5 oder 8.7.
- 8.7 Satz richtig (ganzer Satz bei allen sichtbar). Weiter: Schlüssel erhalten (5.7).

### 9. Rätsel: Hinweise, Exit, Höchstzeit (gilt für Morsezeichen, Fischfang, Seemannsgarn)

- 9.1 Hinweis nehmen, Rückfrage („Kostet die Crew 25 Punkte“). Weiter: Hinweis nehmen → Spielscreen mit Hinweis (zum Beispiel 8.4). Abbrechen → zurück.
- 9.2 Hinweis gesperrt (60 Sekunden für alle, Exit gesperrt). Weiter: Nach der Sperre → Hinweis 2 → 9.1.
- 9.3 Exit möglich (frühestens 60 Sekunden nach Hinweis 2). Weiter: Aufgeben → 9.4 (Crew) oder 9.8 (allein).
- 9.4 Exit vorschlagen, Crew (braucht alle). Weiter: Exit vorschlagen → 9.5 (Antragsteller) und 9.6 (die anderen).
- 9.5 Exit: Warten auf Zustimmung (60 Sekunden). Weiter: Alle stimmen zu → 9.10. Eine Ablehnung oder Zeit um → 9.7.
- 9.6 Exit: Zustimmung bei den anderen. Weiter: Alle stimmen zu → 9.10. Eine Ablehnung oder Zeit um → 9.7.
- 9.7 Exit abgelehnt. Weiter: Weiterspielen.
- 9.8 Exit allein, ohne Zustimmung. Weiter: Exit → 9.10. Weiterspielen.
- 9.9 Restzeit-Hinweis in den letzten 2 Minuten (Morsezeichen ab 3:00). Weiter: Zeit um → 9.10.
- 9.10 Zeitablauf oder Exit angenommen (Schlüssel, keine Punkte). Weiter: Schlüssel erhalten (5.7).
- 9.11 Warte auf deine Crew (Rätsel, Verlassen nur über Exit, Abbruch oder Zeitablauf). Weiter: Alle gelöst → Schlüssel erhalten (5.7). Sonst wie 9.10.

### 10. Fischfang (Rätsel, 3er-Crew, Höchstzeit 10 Minuten)

- 10.1 Regelkarte der Crew (3 Fänge je Person). Weiter: Start → Countdown (5.1), dann Reihe (10.2).
- 10.2 Reihe und Code (3 Fische und „?“, Fischcode, „Fisch nicht im Teich“). Weiter: Code eingeben → 10.3, 10.4 oder 10.5. „Fisch nicht im Teich“ → 10.6. Hinweis → 9.1.
- 10.3 Richtiger Fisch („Leg den Fisch neben den Teich“). Weiter: Nächste Reihe (10.2, Fang 2 von 3). Dritter Fang → 9.11 oder 5.7.
- 10.4 Anderer Fisch („Zurück ins Wasser“). Weiter: Gleiche Reihe (10.2).
- 10.5 Unbekannter Code. Weiter: 10.2.
- 10.6 Fisch nicht im Teich („Sag bitte jemandem aus dem Orgateam Bescheid“). Weiter: Neue Reihe → 10.2.
- 10.7 Hinweis 1 genommen (nennt eine der beiden Regeln). Weiter: 10.2.
- 10.8 „Legt eure Fische zurück in den Teich“ (nach Ende oder Abbruch). Weiter: Erledigt → Schlüssel erhalten (5.7), nach Abbruch Schatzkarte (2.1).

### 11. Seemannsgarn (Rätsel, 3er-Crew, Höchstzeit 10 Minuten)

- 11.1 Regelkarte der Crew (Erzähltisch, 4 Tafeln). Weiter: Start → Countdown (5.1), dann 11.2.
- 11.2 Dein Seefahrer (Geschichte 5, 4 Buchstabenfelder). Weiter: Abgeben → 11.3 oder 11.6. Hinweis → 9.1.
- 11.3 Teils richtig („2 von 4 richtig“, ohne Angabe welche). Weiter: Neu eingeben → 11.2. Hinweis → 9.1.
- 11.4 Hinweis 1: Name des Seefahrers. Weiter: 11.2.
- 11.5 Hinweis 2: zwei der vier Buchstaben. Weiter: 11.2.
- 11.6 Richtig gelöst („Das war James Cook!“). Weiter: Alle gelöst → Schlüssel erhalten (5.7). Sonst wie 9.11.

### 12. Kurs setzen (Punktespiel, 3er-Crew, 4 Minuten)

- 12.1 Regelkarte der Crew (2 Codes je Person). Weiter: Start → Countdown (5.1), dann Code 1 (12.2).
- 12.2 Code 1 von 2 (Plätze 1 bis 4 mit Ziffern in den Farben der Crew). Weiter: Zum Abgeben → 12.3. Zeit um → wie 5.4.
- 12.3 Abgeben (Spalte A bis H, Zeile 1 bis 8). Weiter: Abgeben → 12.4, wenn andere noch fehlen.
- 12.4 Warte auf deine Crew (zweiter Code kommt, wenn alle abgegeben haben). Weiter: Alle abgegeben → 12.5.
- 12.5 Code 2 von 2. Weiter: Zum Abgeben → wie 12.3, danach 12.4 und Ergebnis (12.6).
- 12.6 Ergebnis („x von y richtig“). Weiter: Schlüssel erhalten (5.7).

### 13. Finale und Ende

- 13.1 Schatztruhe (6 Schlüssel). Weiter: Schatz bergen → 13.2.
- 13.2 Schatz bergen, Dialog (einmalig). Weiter: Schatz bergen → 13.3. Noch nicht → 13.1.
- 13.3 „Du bist im Lostopf, Peter!“. Weiter: Seite erneut öffnen → 13.4. Schatzsuche geschlossen → 13.5.
- 13.4 „Du hast den Schatz bereits geborgen“. Weiter: Schatzsuche geschlossen → 13.5.
- 13.5 Ende-Seite: im Lostopf. Weiter: Endzustand.
- 13.6 Ende-Seite: Ende nicht erreicht. Weiter: Endzustand.
- 13.7 Vor der Eröffnung (derselbe Screen mit neutralem Text). Weiter: Nach der Eröffnung → 1.1 oder 1.2.

## Annahmen und offene Punkte

1. Annahme: Station n ist Spiel n (Quiz, Peilung, Morsezeichen, Fischfang, Seemannsgarn, Kurs setzen), und die Schatzkarte zeigt den Spielnamen unter dem Kreis. Das Konzept legt nur 6 Stationen fest. Beendete Stationen zeigen im Wireframe einen Haken als Platzhalter für den Schlüssel.
2. Meldungstexte ohne Vorgabe im Konzept sind Vorschläge: 1.4, 1.5, 4.7, 4.8, 4.10, 6.2, 8.3, 9.7, 9.10 (ein Text für Exit und Zeitablauf) und 10.5.
3. Die Spielflächen sind Platzhalter (Foto, Karte, Leuchtfeld, Schlüssel-Grafik, Truhe). Ihre Gestaltung folgt später.
4. Die Screens zeigen keine Punkte, weil die Spieler sie während des Abends nicht sehen. Quiz und Kurs setzen zeigen „x von y richtig“, Peilung die Entfernung je Foto.
