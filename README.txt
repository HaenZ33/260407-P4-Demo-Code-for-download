# VW T3 Instrument Cluster – Firmware für ESP32-P4

Firmware-Paket zum Flashen des digitalen Kombiinstruments
auf das Waveshare ESP32-P4-Ultra Display-Board.

## Was ist drin?

| Datei | Zweck |
|---|---|
| `bootloader.bin` | Startet den Chip |
| `partition-table.bin` | Sagt dem Chip, wo was im Speicher liegt |
| `p4_instrument_cluster.bin` | Die eigentliche Firmware (Display, Logik, UI) |
| `flash_download_tool/` | Das offizielle Flash-Programm von Espressif |
| `README.md` | Diese Anleitung |

## Was du brauchst

- Windows-PC
- USB-Kabel (**wichtig:** ein Datenkabel, kein reines Ladekabel!)
- Waveshare ESP32-P4-Ultra Board

## Schritt-für-Schritt-Anleitung

### 1. Vorbereitung

- Lade das ZIP herunter und entpacke es an einen Ort
  **ohne Leerzeichen oder Umlaute** im Pfad.
  Empfehlung: `C:\p4flash\`
- Schließe das P4-Board per USB an den Rechner an.

### 2. Flash-Tool starten

- Öffne den Ordner `flash_download_tool/`
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
| ✓ | `p4_instrument_cluster.bin` | `0x20000` |

Einstellungen unten rechts:
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