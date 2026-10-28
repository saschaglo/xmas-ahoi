# Xmas Ahoi – Schatzsuche

Unbetreute Schatzsuche auf dem Handy für die Weihnachtsfeier: Spieler lösen Stationen allein oder in einer Crew, sammeln Schlüssel für die Schatztruhe und Punkte für die Rangliste.

## Language

**Crew**:
Die 1 bis 3 Spieler, die gemeinsam einen Durchlauf einer Station spielen. Sie bildet sich vor der Station über einen Code und löst sich danach auf. Wer allein spielt, ist eine Crew aus einer Person. Alle Mengenregeln (Fragen, Fotos, Wörter, Fänge) richten sich nach der Crew.
_Avoid_: Gruppe, Team, Besetzung

**Teilaufgabe**:
Der Teil eines Durchlaufs, den eine Person der Crew auf dem eigenen Handy löst (eigene Fragen, eigene Fänge, eigener Code). Eine Crew aus n Personen hat n Teilaufgaben. Morsezeichen hat keine Teilaufgaben, dort löst die Crew einen gemeinsamen Funkspruch.
_Avoid_: Unteraufgabe, Einzelaufgabe

**Rätsel**:
Spieltyp mit genau einer richtigen Lösung, ohne Zeitdruck, aber mit einer Höchstzeit (10 Minuten, Morsezeichen 5), mit Hinweisen und Exit (Morsezeichen, Fischfang, Seemannsgarn).
_Avoid_: Puzzle

**Punktespiel**:
Spieltyp mit Zeitlimit, ohne Hinweise und Exit, dessen Ergebnis der Anteil an den möglichen Punkten ist (Quiz, Peilung, Kurs setzen).
_Avoid_: Zeitspiel, Wertungsspiel

**Funkspruch**:
Der Satz, den eine Crew bei Morsezeichen gemeinsam entschlüsselt: Jedes Wort wird einzeln gemorst, danach bringt die Crew die Wörter in die richtige Reihenfolge.
_Avoid_: Nachricht, Lösungssatz

**Geschichte**:
Der Text über einen bekannten Seefahrer, den eine Person bei Seemannsgarn am Erzähltisch liest. Er nennt den Namen nicht, sondern trägt eine Nummer.
_Avoid_: Steckbrief, Story

**Faktentafel**:
Eine der 4 nummerierten Tafeln im Restaurant, an denen bei Seemannsgarn je eine Karte zu jedem Seefahrer hängt. Eine Faktentafel ist keine Station.
_Avoid_: Station, Posten

**Schlüssel**:
Das, was eine beendete Station jedem Spieler der Crew gibt, unabhängig von den Punkten. 6 Schlüssel öffnen die Schatztruhe.

**Punkte**:
0 bis 100 je beendeter Station, gleich für alle in der Crew. Die Summe über alle Stationen bestimmt die Rangliste.
_Avoid_: Score, Wertung

**Abbruch**:
Beenden einer Station ohne Schlüssel und ohne Punkte, gültig für die ganze Crew. Die Station bleibt offen und wird neu gespielt.
_Avoid_: Aufgeben

**Log**:
Die Aufzeichnung jedes Durchlaufs einer Station mit Start, Crew und allen Ereignissen samt Zeit (Hinweis, Exit, Verbindungsverlust, Zeitablauf, Abbruch, Verfall, Ende). Nur im Admin-Panel sichtbar, filterbar nach Person.

**Höchstzeit**:
Die Zeit, die ein Rätsel ab dem gemeinsamen Start höchstens dauert: 10 Minuten, bei Morsezeichen 5 Minuten. Danach endet es automatisch (Zeitablauf): Schlüssel für alle in der Crew, 0 Punkte, auch wenn einzelne Teilaufgaben schon gelöst sind. Punktespiele haben stattdessen ein Zeitlimit.
_Avoid_: Zeitlimit (für Rätsel)

**Exit**:
Ausweg aus einem Rätsel, frühestens 60 Sekunden nach dem zweiten Hinweis: Schlüssel ja, Punkte 0.

**Lostopf**:
Alle Spieler, die die Schatztruhe geborgen haben. Jeder im Lostopf bekommt mindestens einen Trostpreis und kann gezogen werden oder eine Ehrung bekommen.

**Ziehung**:
Das Los, das bei der Siegerehrung die Plätze 1 bis 3 aus dem Lostopf bestimmt, unabhängig von den Punkten. Der Admin kann einen Platz neu ziehen.
_Avoid_: Verlosung, Gewinnspiel

**Ehrung**:
Nennung bei der Siegerehrung nach einem Wert aus den Spielständen (etwa „Teamplayer“ oder „Bester Seefahrer“), mit kleinem Preis. Bei Gleichstand werden alle genannt, der Preis wird unter ihnen gelost.
_Avoid_: Auszeichnung, Award

**Rangliste**:
Reihenfolge der Spieler im Lostopf nach Punkten. Ihre Spitze bekommt die Ehrung „Bester Seefahrer“; gezeigt wird sie erst bei der Siegerehrung.
_Avoid_: Leaderboard

**Orgateam**:
Die Leute, die die Schatzsuche vorbereiten und am Abend ansprechbar sind. Sie betreuen keine Station, sehen aber gelegentlich nach dem Material und helfen bei Fragen.
_Avoid_: Betreuer, Spielleitung
