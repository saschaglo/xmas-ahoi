# Wireframes: Spieler-Flow

Quelle: Trilium „Wireframes – Logbuch-Flow“ · Stand 21.10.2026, Anmerkungen 23.10.2026 · [Übersicht](../README.md)

Graue Low-Fi-Wireframes (nur Positionierung der Elemente, keine Optik) zum Ablauf der mobilen Webseite. Die Screens des Admin-Panels stehen unter [Wireframes: Admin-Panel](../admin/wireframes-admin.md).

**Design-Canvas (Claude Design, privat):** [Wireframes – Logbuch-Flow](https://claude.ai/artifact/79z52NjAYibAu2gyE1iL34)

## Screens (mobile Webseite, 26 Screens)

1. Begrüßung und Check-in („Willkommen an Bord, [Vorname]!“, Textcode als Alternative zum QR-Code)
2. 1b · Name wählen (Overlay „Wie heißt du?“, nur bei Nutzern ohne Namen)
3. Allein oder Crew (Allein spielen, Crew gründen, Crew beitreten)
4. Crew beitreten (Code eingeben, mögliche Fehlermeldungen)
5. Lobby: Crew sammeln (Code, Mitglieder, Bereit, Crew verlassen)
6. Lobby: alle bereit (Start-Button, nicht automatisch)
7. Schatzkarte, Beginn (6 offene Stationen, Schlüsselleiste)
8. Schatzkarte mit Fortschritt (gelöste Stationen mit Schlüssel)
9. Station: Bereit, allein
10. Station: Bereit in der Crew („2 von 3 Personen sind da“, Trotzdem starten, ab 2 Personen)
11. Karte mit Crew-Banner („Peter startet Station 3. Bist du dabei?“)
12. Hinweis zum 1. Spiel (Dialog: danach keine neuen Crew-Mitglieder mehr)
13. Station mit Hinweisen (Platzhalter für die Spielfläche, Hinweis 1 von 2, nächster Hinweis mit Sperre, Station abbrechen)
14. Exit vorschlagen (Dialog, braucht Zustimmung der Anwesenden)
15. Menü (Crew anzeigen, Weitere Person einladen, Crew verlassen ganz unten)
16. Crew verlassen (Rückfrage, mit Hinweis auf den Verlust von Gründen und Beitritt)
17. Schlüssel erhalten
18. Schatztruhe (6 Schlüssel)
19. Schatz bergen (Dialog, einmalig)
20. Im Lostopf (beim erneuten Öffnen: „Du hast den Schatz bereits geborgen“)
21. Ende-Seite (Schatzsuche geschlossen, Status je Person; vor der Eröffnung neutraler Text)
22. Station: zu wenige Personen (Crew mit 1 Anwesenden, „Trotzdem starten“ gesperrt)
23. Station abbrechen (Dialog, gilt für alle, setzt den Spielstand zurück)
24. Exit: Zustimmung (Dialog bei den anderen Mitgliedern, 60 Sekunden, danach abgelehnt)
25. Station: Hinweis gesperrt (Sperre-Anzeige, „Nächster Hinweis in 0:47“), Rückfrage vor dem Nehmen
26. Crew: Beitritt-Hinweis („Peter ist noch zur Crew beigetreten“)

## Annahmen und offene Punkte

1. Die Screens zeigen das alte Modell mit festen Crews. Mit der [Crew je Station](crews.md) entfallen die Screens 3 bis 6, 10 bis 12, 15, 16, 22 und 26. Neu zu zeichnen: Station mit „Allein spielen“ oder „Crew bilden“, Crewcode eingeben an der Station, „Du warst hier schon mal“, die Sperre bei Verbindungsverlust (siehe [Karte und Stationen](karte-und-stationen.md#verbindung-während-der-station)), die Station eines Punktespiels (Restzeit, ohne Hinweise und Exit). Screen 23 ändert sich: Ein Abbruch gibt keinen Schlüssel und löst die Crew auf.
2. Die Spielflächen der Minispiele sind Platzhalter; sie werden je Spiel nach der Beschreibung ergänzt. Varianten nach Crewgröße kommen dann ebenfalls dazu.
