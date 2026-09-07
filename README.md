# VEDO KLARTEXT – Firmware für ESP32-P4

Digitales Kombiinstrument für den VW T3.
Firmware-Paket zum Flashen auf das ESP32-P4-Ultra Display-Board.

**Version:** 1.3.5 (Beta) · **Stand:** 07.09.2026

➡ **[Neueste Version herunterladen](https://github.com/HaenZ33/vedo-klartext/releases/latest)**

Was in dieser Version neu ist, steht im [CHANGELOG.md](CHANGELOG.md).

## Was ist drin?

Alle Dateien hängen am [Release](https://github.com/HaenZ33/vedo-klartext/releases/latest)
und liegen zusätzlich hier im Ordner.

| Datei | Zweck |
|---|---|
| `vedo_klartext_v1.3.5.bin` | Die Firmware – **das ist die Datei für die SD-Karte** |
| `vedo_klartext.bin` | Dieselbe Firmware ohne Versionsnummer im Namen – nur noch nötig, wenn auf deinem Gerät 1.3.4 oder älter läuft (siehe unten) |
| `bootloader.bin` | Startet den Chip – nur für den Weg über das Flash-Tool |
| `partition-table.bin` | Sagt dem Chip, wo was im Speicher liegt – nur fürs Flash-Tool |
| `otadata.bin` | Merkt sich, welche Firmware gestartet wird – nur fürs Flash-Tool |
| `README.md` | Diese Anleitung |
| `MENU.md` | Übersicht über alle Menüpunkte und ihre Bedeutung |
| `CHANGELOG.md` | Was sich von Version zu Version geändert hat |

Das Flash-Programm **`flash_download_tool.zip`** liegt nur noch am Release, nicht
mehr hier im Ordner – es ist 24 MB groß und ändert sich nie. Wer nur über die
SD-Karte aktualisiert, braucht es ohnehin nicht.

> **Der schnelle Weg:** Läuft auf deinem Cluster schon 1.3.0 oder neuer, brauchst
> du von alldem **nur eine einzige Datei** – `vedo_klartext_v1.3.5.bin` auf die
> SD-Karte, Karte rein, Zündung an. Der Abschnitt
> [Update über die SD-Karte](#ab-v130-update-über-die-sd-karte) erklärt es.
> Die Flash-Tool-Anleitung darunter brauchst du nur beim allerersten Mal.

## Was du brauchst

- Windows-PC
- USB-Kabel (**wichtig:** ein Datenkabel, kein reines Ladekabel!)
- ESP32-P4-Ultra Board + Display

## Schritt-für-Schritt-Anleitung

### 1. Vorbereitung

- Lade die Dateien aus dem
  [Release](https://github.com/HaenZ33/vedo-klartext/releases/latest) herunter
  und entpacke `flash_download_tool.zip` an einen Ort **ohne Leerzeichen oder
  Umlaute** im Pfad.
- Schließe das P4-Board per USB an den Rechner an.

### 2. Flash-Tool starten

- Öffne den entpackten Ordner `flash_download_tool/`
- Doppelklick auf `flash_download_tool.exe`
- Im ersten Fenster wählen:
  - **Chip Type:** `ESP32-P4`
  - **WorkMode:** `Develop`
  - **LoadMode:** `UART`
- Auf **OK** klicken

### 3. Dateien und Adressen eintragen

Im Hauptfenster die folgenden **vier** Zeilen eintragen.
Pro Zeile: links auf das `...`-Symbol klicken, die jeweilige
`.bin`-Datei auswählen, daneben die Adresse eintippen,
und ganz links das Häkchen setzen.

| Häkchen | Datei | Adresse |
|---|---|---|
| ✓ | `bootloader.bin` | `0x2000` |
| ✓ | `partition-table.bin` | `0x8000` |
| ✓ | `otadata.bin` | `0x11000` |
| ✓ | `vedo_klartext_v1.3.5.bin` | `0x20000` |

> **Die vierte Zeile (`otadata.bin`) ist neu und beim Update von einer älteren
> Version wichtig.** An dieser Stelle im Speicher lagen bisher Reste, mit denen
> die neue Firmware nichts anfangen kann. Die Zeile schreibt den Bereich sauber.
> Wer sie weglässt, riskiert, dass das Gerät nach dem Flashen nicht sauber
> startet.
>
> **Setze auf keinen Fall das „ERASE"-Häkchen** – das würde Kilometerstand,
> Kalibrierung und alle Menü-Einstellungen löschen. Mit der Tabelle oben
> bleiben sie erhalten.

Einstellungen unten:

- **SPI SPEED:** `80MHz`
- **SPI MODE:** `DIO`
- **FLASH SIZE:** `16MB`

### 4. COM-Port wählen

- Unten links bei **COM:** den Port deines Boards auswählen
- Falls mehrere Ports angezeigt werden: erst den einen probieren,
  bei Fehler den anderen
- **BAUD:** `460800` (Standard reicht)

### 5. Flashen

- Auf den grünen **START**-Knopf klicken
- Das Tool zeigt zuerst die MAC-Adressen des Chips an –
  das ist normal, kein Fehler
- Danach läuft der Fortschrittsbalken
- Nach ein paar Sekunden bis Minuten erscheint **FINISH** in Grün

### 6. Neustart

- USB-Kabel kurz abziehen und wieder einstecken
  (oder RST-Taste am Board drücken)
- Das Display sollte jetzt mit der Intro-Animation starten
- Im Boot-Screen und im Menü unten steht die Version –
  dort kannst du prüfen, ob wirklich **v1.3.5** geflasht wurde

### 7. Einmalig nach dem Update auf v1.3.x

- **Kilometerstand und Einstellungen prüfen.** Sie sollten den Umstieg
  unbeschadet überstanden haben – die Firmware lässt den Speicherbereich mit
  Kilometerstand, Reifengröße, Geber-Kalibrierung und Menü-Einstellungen
  bewusst unangetastet. Trotzdem einmal nachsehen.
- Wenn du von einer Version **vor 1.2.0** kommst, gilt zusätzlich der
  Reifengrößen-Hinweis aus dem [CHANGELOG.md](CHANGELOG.md) unter
  „v1.2.0 → Bitte zuerst lesen".

**Das war das letzte Mal mit Kabel.** Ab jetzt läuft jedes Update über die
SD-Karte – siehe nächster Abschnitt.

## Ab v1.3.0: Update über die SD-Karte

Das Kombiinstrument sitzt fest im Armaturenbrett. Es zum Rechner zu tragen,
nur um eine neue Firmware aufzuspielen, entfällt ab dieser Version.

**So geht ein Update ab jetzt:**

1. Im Release die Datei **`vedo_klartext_v1.3.5.bin`** herunterladen – die
   **mit** Versionsnummer im Namen. So siehst du der Karte später an, welcher
   Stand darauf liegt, ohne das Cluster einzuschalten.

   > **Läuft auf deinem Cluster noch 1.3.4 oder älter?** Dann nimm stattdessen
   > **`vedo_klartext.bin`** – die Datei ohne Versionsnummer. Ältere Firmware
   > sucht ausschließlich nach genau diesem Namen und würde die versionierte
   > Datei nicht finden. Deshalb liegen beide dem Release bei. Welche Version
   > gerade läuft, steht im Boot-Screen und unten im Menü.

   > **Immer nur eine der beiden Dateien auf die Karte legen.** Liegen beide
   > darauf, sind sie für das Cluster zwei gleichwertige Kandidaten mit
   > derselben Version. Kann es sie nicht anhand des Kopierdatums
   > unterscheiden, tut es lieber nichts, statt zu raten – dann passiert beim
   > Einschalten einfach kein Update.
2. Die Datei auf die SD-Karte kopieren, direkt in den Hauptordner
   (ein Unterordner `update` geht auch). **Umbenennen ist nicht nötig** – und
   es bringt auch nichts: welche Version in einer Datei steckt, liest das
   Cluster aus der Datei selbst, nicht aus dem Namen. Eine alte Firmware in
   `vedo_klartext_v9.9.9.bin` umzubenennen ändert daran nichts.
3. Karte ins Cluster stecken, Zündung an.
4. Das Cluster meldet sich von selbst: es zeigt, welche Version auf der Karte
   liegt und welche gerade läuft, und zählt zehn Sekunden herunter.
   - **Nichts tun** → es installiert. Nach 20 bis 40 Sekunden startet das
     Gerät allein neu, fertig.
   - **Encoder drücken** → es wird nicht installiert und das Cluster startet
     ganz normal.

**Was du dabei wissen solltest:**

- **Die Karte darf drin bleiben.** Das Cluster erkennt, ob die Datei die
  bereits laufende Firmware ist, und lässt sie dann in Ruhe. Es fragt also
  nicht bei jedem Einschalten wieder.
- **Ein misslungenes Update macht nichts kaputt.** Startet die neue Firmware
  nicht sauber durch, holt das Gerät beim nächsten Einschalten von allein die
  vorherige Version zurück.
- **Zündung mittendrin aus ist harmlos.** Bis zum Schluss läuft weiter die
  alte Firmware; erst ganz am Ende wird umgeschaltet.
- **Falsche Dateien werden abgewiesen.** Ein abgebrochener Download oder eine
  Firmware für ein anderes Gerät wird erkannt, und das Cluster startet normal.
- Während der Installation zeigt das Display einen Fortschrittsbalken und ist
  solange nicht bedienbar. Das ist normal.
- **Liegen mehrere gültige Firmware-Dateien auf der Karte, gewinnt die neueste
  Version.** Alte Stände dürfen also liegen bleiben.

Der Weg über das Flash-Tool oben bleibt als Notnagel bestehen – gebraucht wird
er im Normalfall nicht mehr.

### Update von Hand anstoßen

Manchmal will man nicht auf den nächsten Neustart warten – oder denselben Stand
noch einmal aufspielen, etwa nach einem abgebrochenen Versuch. Dafür gibt es
seit v1.3.5 den Menüpunkt:

**Einstellungen → System → Firmware**

Er durchsucht die Karte sofort und startet den gewohnten Ablauf. Anders als beim
Einschalten fragt er bei einem Rückschritt nicht lange nach – wer den Punkt
anwählt, hat sich entschieden. Firmware **vor 1.3.0** lässt sich auch hierüber
nicht aufspielen; der Grund steht im nächsten Abschnitt.

### Zurück auf eine ältere Version

Geht ebenfalls über die Karte, solange das Ziel **1.3.0 oder neuer** ist.
Einfach die ältere Firmware-Datei auf die Karte legen – und die neuere
herunternehmen, sonst gewinnt die höhere Version.

Das Cluster behandelt einen Rückschritt aber bewusst anders als ein Update:

- Der Titel lautet **„FIRMWARE ZURUECKSETZEN"**, hinter der Version steht
  **(AELTER)**.
- **Der Countdown installiert hier nicht.** Läuft er ab, passiert nichts und
  das Cluster startet normal. Zum Zurücksetzen musst du den Encoder **drücken**.

Das ist Absicht: eine vergessene Karte mit einem alten Stand soll das Cluster
nicht im Vorbeifahren zurückdrehen.

**Auf Versionen vor 1.3.0 geht es nicht über die Karte.** Sie werden abgewiesen,
und das Cluster startet normal weiter. Der Grund: diese Stände wissen noch
nichts vom Update-Mechanismus und können sich nach dem Aufspielen nicht selbst
bestätigen. Sie ließen sich zwar schreiben und würden auch einmal starten –
beim nächsten Einschalten holt das Gerät aber von allein die vorherige Version
zurück. Ein Update, das erfolgreich aussieht und sich später still selbst
rückgängig macht, ist schlechter als eines, das gleich nein sagt.

Wer wirklich auf 1.2.x oder älter zurück will, nimmt dafür das Flash-Tool.

## Wenn etwas nicht klappt

**„download data fail" oder „write process fail"**
→ Anderes USB-Kabel probieren. Häufigste Ursache.
→ Direkt am PC anschließen, nicht über USB-Hub.
→ Pfad ohne Leerzeichen/Umlaute prüfen.

**„overlap at address ..."**
→ Die Adressen in der Tabelle (Schritt 3) stimmen nicht.
   Bitte exakt so eintragen wie oben angegeben.

**Tool zeigt keinen COM-Port an**
→ USB-Kabel prüfen (Datenkabel?).
→ Im Windows-Geräte-Manager unter „Anschlüsse (COM & LPT)"
   nachsehen, ob das Board erkannt wird.

**Das Board reagiert nicht / kommt nicht in den Download-Modus**
→ BOOT-Taste am Board gedrückt halten,
→ kurz RST-Taste drücken und loslassen,
→ BOOT-Taste loslassen,
→ erst dann im Tool START drücken.

## Fragen?

Bei Problemen einfach melden – am besten mit einem Foto vom
Tool-Fenster, dann lässt sich das schnell klären.

## Zur Nutzung

Diese Firmware ist ein privates Projekt und befindet sich im Beta-Stadium. Sie
steuert Anzeigen in einem fahrenden Auto – **die Nutzung erfolgt auf eigene
Gefahr, eine Gewährleistung gibt es nicht.** Verlass dich für sicherheitsrelevante
Werte nie allein auf dieses Gerät.

Bitte gib die `.bin`-Dateien nicht selbst weiter, sondern verweise auf dieses
Repository. Sonst landen irgendwann alte Stände aus zweiter Hand auf Karten, und
niemand weiß mehr, was auf welchem Gerät läuft.
