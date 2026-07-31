# Changelog – VEDO KLARTEXT

Alle Änderungen an der Firmware für das ESP32-P4 Kombiinstrument.
Die aktuell installierte Version steht im Boot-Screen und unten im Menü.

---

## v1.1.4 (Beta) – 31.07.2026

Sammel-Update. Die Zwischenstände 1.1.2 und 1.1.3 sind hier mit enthalten und
wurden nie einzeln veröffentlicht. Schwerpunkte: automatische Hinterleuchtung,
Tacho-Fehler behoben, Uhrzeit nach längerer Standzeit.

### Neu

**Automatische Hinterleuchtung (Lichtsensor)**
- Mit einem VEML7700-Lichtsensor regelt das Display seine Helligkeit jetzt
  selbst – hell bei Tag, gedimmt bei Nacht.
- Eigenes Untermenü **HELLIGKEIT** (Doppelklick auf „Helligkeit" im
  Hauptmenü; ein einfacher Klick bleibt die manuelle Helligkeit):
  - `Auto-Helligkeit` an/aus
  - `Tendenz` (−4…+4) – der zentrale „insgesamt heller/dunkler"-Knopf
  - `Minimum` (3–30 %) – verhindert ein komplett schwarzes Display
  - `Reaktion` (träge / normal / flott) – wie schnell nachgeregelt wird
  - `Kurve` – fünf Stützstellen mit Live-Graph und Marker für den aktuell
    gemessenen Helligkeitswert
  - `Lichtsensor` – Live-Anzeige zum Einbau-Debugging
- **„Hier merken":** Auf der Kurven- oder Lichtsensor-Seite so lange drehen,
  bis die Helligkeit passt, dann lang drücken. Der aktuelle Wert wird auf die
  passende Stützstelle geschrieben, die Nachbarn ziehen sanft mit. So lässt
  sich die Regelung ohne Zahlenverständnis einstellen.
- Der Lichtsensor taucht im Selbsttest beim Start und im Debug-Screen auf.

### Behoben

**Tacho: Fantasiewert beim ersten Puls nach Stillstand**
- Nach einem Stillstand konnte der erste Magnetpuls sofort einen erfundenen
  km/h-Wert anzeigen (z. B. 34 km/h aus dem Nichts). Die Anzeige bleibt jetzt
  bei 0, bis wirklich zwei Pulse gemessen wurden.
- Der Drehzahlmesser war davon nicht betroffen und ist unverändert.

**Uhrzeit nach längerer Standzeit**
- Das GPS-Modul sichert seine Bahndaten jetzt vor dem Abstellen dauerhaft.
  Nach langer Standzeit findet es dadurch schneller wieder Satelliten – die
  Uhr steht früher.
- **Es wird nie mehr eine falsche Uhrzeit angezeigt.** Liefert das Modul eine
  unplausible Zeit, wird sie verworfen; die Uhr zeigt dann weiter
  „WARTE AUF GPS", bis eine echte Zeit da ist.

### Hinweise

- **Wichtig:** Steht nach dem Einschalten sehr lange „WARTE AUF GPS", obwohl
  freier Himmel da ist, ist meist die **Stützbatterie am GPS-Modul leer**.
  Das ist ein Hardware-Thema und lässt sich per Firmware nicht beheben – die
  Uhrzeit liegt im Modul und geht ohne Stützspannung verloren.
- Die automatische Hinterleuchtung braucht einen **VEML7700-Sensor**
  (3,3 V, SDA 28 / SCL 29). Ohne den Sensor ändert sich nichts, die manuelle
  Helligkeit funktioniert wie bisher.
- Firmware-Datei heißt jetzt `vedo_klartext_v1.1.4.bin`. Flash-Adressen
  unverändert – siehe [README.md](README.md).
- Nach dem Flashen im Boot-Screen prüfen, ob dort **v1.1.4** steht.
- Status weiterhin **Beta**.

---

## v1.1.1 (Beta) – 23.07.2026

Patch-Update. Schwerpunkte: zweites GPS-Modul wählbar, schnellerer GPS-Fix,
verbesserter Needle-Sweep.

### Neu

**Zweites GPS-Modul wählbar**
- Neben dem bisherigen **LC76G** wird jetzt auch das **SR1612U10**
  (u-blox M10) unterstützt.
- Auswahl im Menü unter **SETTINGS → „GNSS Modul"**; die Wahl wird
  gespeichert und beim nächsten Start automatisch verwendet.

**Geschwindigkeits-Ausgang (VSS)**
- Neuer VSS-Ausgang zur Weitergabe des Tachosignals.

### Verbessert

**GPS / Uhrzeit**
- **Uhrzeit ist sofort da:** Die Zeit vom GPS wird direkt beim Empfang
  in die interne Uhr (RTC) übernommen – kein Warten mehr auf den vollen
  Positions-Fix.
- **Schnellerer Fix nach dem Einschalten:** GNSS-„EASY“ aktiviert
  (der Empfänger merkt sich Bahndaten und findet dadurch nach dem Aufwachen
  deutlich schneller wieder Satelliten).
- Zusätzliche Zeit-Nachricht (ZDA) für eine sauberere Uhrzeit-Auswertung.
- Aufwach- und Fix-Ablauf (Wake/TTFF) intern überarbeitet.

**Anzeige**
- **Needle-Sweep verbessert:** Der Zeiger-Sweep beim Start läuft nur noch
  bei **stehendem Motor**. Läuft der Motor schon, zeigen die Zeiger sofort
  die echten Werte (kein kurzes „Wegzappeln" mehr).

### Hinweise

- Firmware-Datei heißt jetzt `vedo_klartext_v1.1.1.bin`. Flash-Adressen
  unverändert – siehe [README.md](README.md).
- Nach dem Flashen im Boot-Screen prüfen, ob dort **v1.1.1** steht.
- Status weiterhin **Beta**.

---

## v1.1 (Beta) – 13.07.2026

Großes Funktions-Update gegenüber der ersten Version vom April.
Neu sind vor allem das Bedien-Menü, die Uhr, GPS/SD-Logging und
deutlich mehr Sensorik.

### Neu

**Bedien-Menü**
- Komplettes Menü, bedienbar über den Dreh-Encoder
- Untermenüs für Sensorik, Anzeige und Einstellungen
- Unten wird immer die laufende Version eingeblendet

**Uhr / Standby**
- Uhren-Screen mit großer 7-Segment-Anzeige
- Dient als Standby-Anzeige; Rückkehr per Joystick/Encoder
- Zeitzone über UTC-Offset im Menü einstellbar

**GPS und Datenaufzeichnung**
- GNSS-Empfang (Position, Geschwindigkeit, Uhrzeit)
- SD-Karten-Unterstützung
- Datenlogger schreibt Messwerte auf die SD-Karte
- GPS-Track-Aufzeichnung

**Mehr Sensorik**
- Öl: Öldruck-Sensor, zwei Öldruck-Schalter (0,3 bar und 0,9 bar)
  und Öltemperatur – inklusive überarbeiteter Warnschwellen
- Außentemperatur über DS18B20 (1-Wire-Fühler)
- Chip-Temperatur des ESP32-P4

**Diagnose**
- Debug-Screen mit CPU-Auslastung und Task-Übersicht
- Status-LED signalisiert den Systemzustand

### Verbessert

- **Drehzahl deutlich stabiler:** Neuer Glitch-Filter (Auswertung der
  fallenden Flanke, Plausibilitätsprüfung gegen den Vorwert). Zappelnde
  oder ausreißende Drehzahlwerte sollten damit der Vergangenheit angehören.
- Encoder-Drehrichtung korrigiert und Menüführung neu strukturiert
- Zweiter Screen überarbeitet
- Anzeige-Refresh entzerrt (Race-Condition beim Neuzeichnen behoben)

### Hinweise

- Die Firmware-Datei heißt jetzt `vedo_klartext_v1.1.bin`
  (vorher `p4_instrument_cluster.bin`). Die Flash-Adressen sind
  unverändert – siehe [README.md](README.md).
- Nach dem Flashen im Boot-Screen prüfen, ob dort **v1.1** steht.
- Status: **Beta.** Nicht alle Sensor-Kombinationen sind im Fahrbetrieb
  langzeiterprobt.

---

## v1.0 – 07.04.2026

- Erste veröffentlichte Version
- Grundlegende Instrumenten-Anzeige auf dem 720x720-Display
- Basis-Sensorik
