# 07 Identität, Session und Zugang

Type: grilling
Status: open
Assignee: –
Blocked by: –

## Question

Wie wird aus dem Bordkarten-Code (QR oder 4 Zeichen) eine Spieler-Session, die Pod-Neustarts übersteht? Was passiert bei Handywechsel oder zwei Geräten gleichzeitig? Wie loggt sich der Admin ein, wo zählt das Rate-Limit gegen Raten (pro Code oder Gerät, nicht pro IP), und welcher Zeichensatz gilt für Nutzer- und Crewcodes?

## Kontext

- [Eingang und Bordkarte](../../../concepts/framework/eingang-und-bordkarte.md): Zeichensatz „bei der Umsetzung festgelegt“.
- [Admin-Panel](../../../concepts/admin/admin-panel.md): Login „Nuxt-Standard“.
- [MySQL in Nuxt](03-mysql-in-nuxt.md): Session-Bausteine und Rate-Limit ohne Redis, falls schon vorhanden.
