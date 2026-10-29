# 07 Identität, Session und Zugang

Type: grilling
Status: resolved
Assignee: Sascha Glover
Blocked by: –

## Question

Wie wird aus dem Bordkarten-Code (QR oder 4 Zeichen) eine Spieler-Session, die Pod-Neustarts übersteht? Was passiert bei Handywechsel oder zwei Geräten gleichzeitig? Wie loggt sich der Admin ein, wo zählt das Rate-Limit gegen Raten (pro Code oder Gerät, nicht pro IP), und welcher Zeichensatz gilt für Nutzer- und Crewcodes?

## Kontext

- [Eingang und Bordkarte](../../../concepts/framework/eingang-und-bordkarte.md): Zeichensatz „bei der Umsetzung festgelegt“.
- [Admin-Panel](../../../concepts/admin/admin-panel.md): Login „Nuxt-Standard“.
- [MySQL in Nuxt](03-mysql-in-nuxt.md): Session-Bausteine und Rate-Limit ohne Redis, falls schon vorhanden.

## Answer

Festgehalten in [Zugang und Session](../../../concepts/technik/zugang-und-session.md). Der Weg dorthin:

- Mehrere Geräte gelten gleichzeitig; „letztes Gerät gewinnt“ verworfen, weil es eine Prüfung je Anfrage kostet und der Spielstand ohnehin am Spieler hängt.
- QR-Code als URL mit dem Zugangscode statt eines langen Tokens: ein Token sichert nur den QR-Weg, die Eingabe von Hand bliebe so leicht zu raten.
- Vorschlag „ein Zeichensatz für beide Codes“ (21 Zeichen gemischt) vom Veranstalter geändert: Zugangscode nur Buchstaben, Crewcode nur Ziffern, damit man sie unterscheidet. Damit wurde „Textcode“ im Glossar zu „Zugangscode“.
- Rate-Limit je Gerät plus globale Bremse, weil „pro Code“ das Durchprobieren vieler Codes nicht bremst und ein Gerät sein Cookie löschen kann. Zähler im Speicher statt in MySQL, weil ein Neustart hier nichts kostet.
- Admin mit gemeinsamem Passwort und Fingerabdruck in der Session, damit ein Passwortwechsel nicht alle Spieler abmeldet.
