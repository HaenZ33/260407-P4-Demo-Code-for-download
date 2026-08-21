# VEDO KLARTEXT – Firmware für ESP32-P4

Digitales Kombiinstrument für den VW T3.
Firmware-Paket zum Flashen auf das ESP32-P4-Ultra Display-Board.

**Version:** 1.3.1 (Beta) · **Stand:** 21.08.2026

Was in dieser Version neu ist, steht im [CHANGELOG.md](CHANGELOG.md).

## Was ist drin?

| Datei | Zweck |
|---|---|
| `bootloader.bin` | Startet den Chip |
| `partition-table.bin` | Sagt dem Chip, wo was im Speicher liegt |
| `vedo_klartext_v1.3.1.bin` | Die eigentliche Firmware – für das Flash-Tool |
| `vedo_klartext.bin` | Dieselbe Firmware für das Update über die SD-Karte |
| `otadata.bin` | **Neu ab 1.3.0** – merkt sich, welche Firmware gestartet wird |
| `flash_download_tool.zip` | Das offizielle Flash-Programm von Espressif |
| `README.md` | Diese Anleitung |
| `MENU.md` | Übersicht über alle Menüpunkte und ihre Bedeutung |
| `CHANGELOG.md` | Was sich von Version zu Version geändert hat |

## Was du brauchst

- Windows-PC
- USB-Kabel (**wichtig:** ein Datenkabel, kein reines Ladekabel!)
- ESP32-P4-Ultra Board + Display

## Schritt-für-Schritt-Anleitung

### 1. Vorbereitung

- Lade die Dateien herunter und entpacke `flash_download_tool.zip` an einen Ort
  **ohne Leerzeichen oder Umlaute** im Pfad.
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
| ✓ | `vedo_klartext_v1.3.1.bin` | `0x20000` |

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
  dort kannst du prüfen, ob wirklich **v1.3.1** geflasht wurde

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

1. Im Release die Datei **`vedo_klartext.bin`** herunterladen – die **ohne**
   Versionsnummer im Namen. Sie liegt dort genau für diesen Zweck neben der
   versionierten Datei.
   **Nicht umbenennen** – das Cluster sucht exakt nach diesem Namen.

   > Die Datei `vedo_klartext_v1.3.1.bin` ist inhaltlich dieselbe Firmware,
   > aber nur für den Weg über das Flash-Tool gedacht. Auf der SD-Karte wird
   > sie nicht erkannt.
2. Die Datei auf die SD-Karte kopieren, direkt in den Hauptordner
   (ein Unterordner `update` geht auch).
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

Der Weg über das Flash-Tool oben bleibt als Notnagel bestehen – gebraucht wird
er im Normalfall nicht mehr.

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
