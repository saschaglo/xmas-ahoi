# Karte und Stationen

Quelle: Trilium „Spielkonzept – Ablauf der mobilen Webseite“, Abschnitt „Karte und Stationen“, überarbeitet auf Crew je Station · Stand 23.10.2026 · [Übersicht](../README.md)

- Nach dem Check-in erscheint eine stilisierte Schatzkarte mit 6 anklickbaren Stationen. Offene Stationen sehen unfertig aus; beendete zeigen einen Schlüssel.
- Man kommt jederzeit zur Karte zurück und sieht den Fortschritt.
- Station antippen, dann „Allein spielen“ oder „Crew bilden“ bzw. Crewcode eingeben; Ablauf unter [Crews](crews.md). Allein geht es nach „Bin bereit“ sofort los.
- Bei Stationen an einem festen Ort (Morsezeichen, Fischfang, Seemannsgarn, Kurs setzen) fragt die App davor jede Person, ob sie dort ist ([Spiele mit Material im Raum](../spiele/uebergreifend.md#spiele-mit-material-im-raum)).
- Jeder Spieler hat immer nur eine aktive Station.
- Gleichzeitige Aktionen: Beim gleichzeitigen Abbruch und Exit hat der Abbruch Vorrang.

## Schlüssel und Punkte

- Jede beendete Station gibt allen in der Crew den Schlüssel, unabhängig von den Punkten. Ein Exit gibt ebenfalls den Schlüssel, aber 0 Punkte. Ein Abbruch gibt weder Schlüssel noch Punkte.
- Jede Station gibt 0 bis 100 Punkte; alle in der Crew bekommen dieselben. Die Regeln je Spieltyp stehen unter [Spielübergreifend](../spiele/uebergreifend.md#spieltypen-und-punkte).
- Eine beendete Station ist für den Spieler gesperrt („Du warst hier schon mal“). Eine abgebrochene bleibt offen und wird mit neuen Aufgaben neu gespielt.

## Verbindung während der Station

- Der Server prüft während einer Station laufend, ob jedes Crew-Mitglied die Spielseite noch offen hat.
- Fällt ein Mitglied aus (Seite geschlossen, Handy gesperrt, kein Netz), sehen alle in der Crew eine Sperre, zum Beispiel „Peter ist gerade nicht an Bord. Wir warten auf ihn …“. Kommt die Person zurück, geht es weiter.
- Während der Sperre kann jede Person „Abbruch“ wählen; das gilt für die ganze Crew.
- Bei der Umsetzung klären: ab wann ein Mitglied als ausgefallen gilt und ob die Spielzeit eines Punktespiels während der Sperre stehen bleibt.
