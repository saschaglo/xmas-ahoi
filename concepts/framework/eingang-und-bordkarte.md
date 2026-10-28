# Eingang und Bordkarte

Stand 23.10.2026 · [Übersicht](../README.md)

- Jede Person erhält eine individuell gefertigte Bordkarte mit QR-Code. Er führt auf die Webseite und enthält den Zugangscode, damit die Begrüßung direkt lautet: „Willkommen an Bord, Peter!“ Die Karten werden separat erstellt.
- Auf der Karte steht zusätzlich ein 4-stelliger Textcode, der keine verwechselbaren Zeichen enthält (zum Beispiel O/0, I/1/L, S/5, B/8, Z/2, G/6, U/V; der genaue Zeichensatz wird bei der Umsetzung festgelegt). Das System erzeugt den Code. Alle Codes (Nutzer und Crews) sind global eindeutig.
- Der Vorname genügt. Dann folgt „Einchecken“.
- Beim Check-in bittet ein Satz darum, Lösungen nicht weiterzugeben, zum Beispiel „Ehrensache unter Seeleuten: Lösungen bleiben an Bord.“ ([Weitergabe von Lösungen](../spiele/uebergreifend.md#weitergabe-von-lösungen)).
- Ein Nutzer kann ohne Namen angelegt werden. Beim ersten Betreten fragt ein Overlay „Wie heißt du?“, bevor der Check-in möglich ist.
- Gegen Raten der Codes gilt ein Rate-Limit pro Code oder Gerät, nicht pro IP-Adresse, weil im Firmen-WLAN alle dieselbe IP haben.
- Scannt jemand den QR-Code später erneut (Seite geschlossen, Handywechsel), kehrt die Person über ihren Spiel-State dorthin zurück, wo sie war.
