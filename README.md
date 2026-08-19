# VEDO KLARTEXT – Firmware für ESP32-P4

Digitales Kombiinstrument für den VW T3.
Firmware-Paket zum Flashen auf das ESP32-P4-Ultra Display-Board.

**Version:** 1.2.0 (Beta) · **Stand:** 19.08.2026

Was in dieser Version neu ist, steht im [CHANGELOG.md](CHANGELOG.md).

## Was ist drin?

| Datei | Zweck |
|---|---|
| `bootloader.bin` | Startet den Chip |
| `partition-table.bin` | Sagt dem Chip, wo was im Speicher liegt |
| `vedo_klartext_v1.2.0.bin` | Die eigentliche Firmware (Display, Logik, UI) |
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

Im Hauptfenster die folgenden drei Zeilen eintragen.
Pro Zeile: links auf das `...`-Symbol klicken, die jeweilige
`.bin`-Datei auswählen, daneben die Adresse eintippen,
und ganz links das Häkchen setzen.

| Häkchen | Datei | Adresse |
|---|---|---|
| ✓ | `bootloader.bin` | `0x2000` |
| ✓ | `partition-table.bin` | `0x8000` |
| ✓ | `vedo_klartext_v1.2.0.bin` | `0x20000` |

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
  dort kannst du prüfen, ob wirklich **v1.2.0** geflasht wurde

### 7. Einmalig nach dem Update auf v1.2.0

- **Reifengröße eintragen:** SETTINGS → REIFEN. Der Tacho rechnet ab dieser
  Version daraus – vorher steckte eine feste Pauschalzahl drin, die rund
  doppelt so hoch war wie richtig. Danach einmal gegen GPS gegenprüfen und bei
  Bedarf über `Offset` nachtrimmen.
- Screen 2 ist entfallen; das Cluster startet je nach vorherigem Screen einmalig
  auf einem anderen. Ein Encoder-Klick korrigiert das dauerhaft.
- Ein paar Menü-Häkchen können verrutscht sein – einmal durchklicken räumt das
  auf.

Ausführlich steht das im [CHANGELOG.md](CHANGELOG.md) unter „Bitte zuerst lesen".

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
