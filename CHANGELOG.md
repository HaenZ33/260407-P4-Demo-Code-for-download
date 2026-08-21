# Changelog – VEDO KLARTEXT

Alle Änderungen an der Firmware für das ESP32-P4 Kombiinstrument.
Die aktuell installierte Version steht im Boot-Screen und unten im Menü.

---

## v1.3.2 (Beta) – 21.08.2026

**Fehlerbehebung am neuen SD-Update-Weg: Zurückgehen auf eine ältere Version
ist jetzt sauber geregelt.** Wer 1.3.0 frisch aufgespielt hat und nicht
zurückgehen will, kann diese Version trotzdem mitnehmen – sie ändert sonst
nichts.

---

### Behoben

**Firmware vor 1.3.0 wird nicht mehr von der Karte installiert**
- Bisher wurden 1.2.0 und ältere Stände von der Karte angenommen und
  aufgespielt. Das Ergebnis war schlechter als eine Ablehnung: die
  Installation lief durch, das Gerät startete, alles wirkte normal – und beim
  nächsten Einschalten war die neue Version wieder da.
- Der Grund: diese alten Stände wissen nichts vom Update-Mechanismus und
  können sich nach dem Aufspielen nicht selbst als lauffähig melden. Genau
  darauf wartet aber die Rückfall-Sicherung, und ohne diese Meldung holt sie
  die vorherige Version zurück.
- Jetzt sagt das Cluster gleich nein und startet normal weiter. Wer wirklich
  auf 1.2.x zurück will, nimmt das Flash-Tool.

**Ein Rückschritt passiert nur noch auf ausdrücklichen Knopfdruck**
- Zurück auf 1.3.0 oder neuer bleibt möglich – ein Weg zurück auf einen
  funktionierenden Stand muss es geben.
- Das Cluster zeigt dabei **„FIRMWARE ZURUECKSETZEN"** und **(AELTER)** hinter
  der Version, und **der Countdown installiert nicht mehr von allein**. Läuft
  er ab, passiert nichts; zum Zurücksetzen drückst du den Encoder.
- Vorher hätte eine vergessene Karte mit einem alten Stand das Cluster im
  Vorbeifahren zurückgedreht.

Der genaue Ablauf steht in der [README.md](README.md) unter „Zurück auf eine
ältere Version".

### Hinweise

- Am Menü hat sich nichts geändert – die [MENU.md](MENU.md) gilt unverändert.
- Dem Release liegt ein **`sd_testkit.zip`** bei. Das ist Testmaterial für den
  SD-Update-Weg (inklusive einer Datei, die absichtlich abgelehnt werden soll)
  und wird zum normalen Aktualisieren **nicht** gebraucht. Wer einfach nur
  updaten will, nimmt `vedo_klartext.bin`.

---

## v1.3.1 (Beta) – 21.08.2026

**Testrelease – funktional identisch zu 1.3.0.** In der Firmware selbst hat
sich nichts geändert, außer der Versionsnummer.

Diese Version existiert aus einem einzigen Grund: Das Cluster installiert ein
Update von der SD-Karte nur dann, wenn sich die Datei vom gerade laufenden
Stand unterscheidet. Um den neuen SD-Weg unter echten Bedingungen einmal
durchzuspielen, braucht es also eine zweite Versionsnummer – sonst gibt es
nichts zu installieren.

**Wer 1.3.0 laufen hat, verpasst durch Überspringen nichts.** Wer den
SD-Update-Weg selbst ausprobieren möchte, nimmt sie als Testkandidat: Datei
`vedo_klartext.bin` aus diesem Release auf die Karte, Karte rein, Zündung an.
Der Ablauf steht in der [README.md](README.md) unter „Update über die
SD-Karte".

---

## v1.3.0 (Beta) – 20.08.2026

**Ab dieser Version aktualisiert sich das Cluster selbst von der SD-Karte.**
Datei drauf, Karte rein, Zündung an – das war's. Das Gerät muss dafür nicht
mehr ausgebaut und an den Rechner getragen werden.

---

### ⚠ Bitte zuerst lesen

**Dieses eine Update geht noch über das Kabel – und mit einer Zeile mehr als
sonst.**

Die Speicheraufteilung des Chips hat sich geändert, damit zwei Firmware-Stände
nebeneinander Platz haben. Deshalb reicht es diesmal nicht, nur die Firmware
aufzuspielen:

- Im Flash-Tool kommt eine **vierte Zeile** dazu: `otadata.bin` auf die
  Adresse `0x11000`. An dieser Stelle lagen bisher Reste, mit denen die neue
  Firmware nichts anfangen kann.
- **Das „ERASE"-Häkchen bitte nicht setzen.** Kilometerstand, Reifengröße,
  Geber-Kalibrierung und alle Menü-Einstellungen bleiben sonst nicht erhalten.
  Mit der normalen Tabelle überstehen sie den Umstieg.

Die genaue Tabelle steht in der [README.md](README.md) unter Schritt 3.

Ab dem nächsten Update läuft alles über die Karte.

---

### Neu

**Update über die SD-Karte**
- Im Release die Datei **ohne** Versionsnummer im Namen herunterladen
  (`vedo_klartext.bin`), unverändert auf die Karte kopieren, Karte einstecken,
  Zündung an. Die versionierte Datei `vedo_klartext_v1.3.0.bin` ist dieselbe
  Firmware, wird auf der Karte aber nicht erkannt – sie ist für das Flash-Tool.
- Das Cluster zeigt beim Start, welche Version auf der Karte liegt und welche
  gerade läuft, und zählt zehn Sekunden herunter. Wer nichts tut, bekommt das
  Update; ein Druck auf den Encoder überspringt es.
- Während der Installation läuft ein Fortschrittsbalken, danach startet das
  Gerät allein neu. Dauer: 20 bis 40 Sekunden.
- **Die Karte darf dauerhaft stecken bleiben.** Das Cluster erkennt, ob die
  Datei die ohnehin laufende Firmware ist, und fragt dann nicht wieder.
  Umbenennen oder Löschen ist nicht nötig.

**Sicherheitsnetz beim Update**
- Startet eine frisch installierte Firmware nicht sauber durch, holt das Gerät
  beim nächsten Einschalten von selbst die vorherige Version zurück.
- Geht während der Installation die Zündung aus, ist das folgenlos – bis zum
  Schluss läuft die alte Firmware, erst ganz am Ende wird umgeschaltet.
- Beschädigte Dateien, abgebrochene Downloads und Firmware für andere Geräte
  werden vor dem ersten geschriebenen Byte erkannt und abgewiesen.

### Verbessert

**Das Startbild blendet nachts nicht mehr**
- Während der Intro-Animation ist die Hintergrundbeleuchtung auf ein Viertel
  gedeckelt. Wer ohnehin dunkler eingestellt hat, sieht unverändert seinen
  eigenen Wert – heller als eingestellt wird das Startbild nie.
- Danach fährt die Helligkeit weich auf den eingestellten Wert hoch, statt zu
  springen. Der Übergang passiert noch auf dem schwarzen Hintergrund, sodass
  der erste Screen gleich richtig hell erscheint.

**Automatische Helligkeit kann dunkler**
- Die untere Grenze lässt sich jetzt bis auf 1 % stellen (vorher 3 %). Für
  sehr dunkle Nachtfahrten waren 3 % noch zu hell.

### Hinweise

- Am Menü hat sich in dieser Version nichts geändert – die
  [MENU.md](MENU.md) gilt unverändert weiter.
- Der Weg über das Flash-Tool bleibt als Notnagel bestehen, falls einmal beide
  Firmware-Stände beschädigt sein sollten.
- Für den SD-Weg wird eine eingelegte SD-Karte gebraucht. Ohne Karte läuft das
  Cluster normal weiter, kann sich dann aber nicht selbst aktualisieren.

---

## v1.2.0 (Beta) – 19.08.2026

Großes Update. Schwerpunkte: **der Tacho zeigte bisher nur die halbe
Geschwindigkeit** und ist jetzt richtig, die Reifengröße ist einstellbar, die
Kontrollleuchten im Armaturenbrett funktionieren, und die Geber lassen sich
einzeln kalibrieren.

Die Versionsnummer springt auf **1.2**, weil mit dieser Firmware das
**Adapterboard V2** dazugekommen ist – ein 1.1.5 hätte nach reiner
Fehlerbehebung ausgesehen.

---

### ⚠ Bitte zuerst lesen

**1. Der Tacho zeigt jetzt etwa doppelt so viel wie vorher – das ist der
korrigierte Wert.**
Die Firmware rechnete mit einer festen Pauschalzahl (4000 Impulse pro
Kilometer), richtig sind bei 225/55 R16 rund 1947. Der Tacho zeigte dadurch nur
**49 %** der echten Geschwindigkeit. Nach dem Update:

- **Reifengröße im Menü eintragen** (SETTINGS → REIFEN), sonst rechnet die
  Firmware mit dem Standardwert.
- Danach **einmal gegen GPS gegenprüfen** und bei Abweichung über
  `Offset` (±10 %) nachtrimmen.
- Der Tempomat-Ausgang hing an derselben Zahl und war damit ebenfalls um
  Faktor 2 daneben – das ist mit derselben Korrektur erledigt.

**2. Das Adapterboard V2 wird vorausgesetzt, wenn du den vollen Funktionsumfang
willst.**
Ohne das Board läuft die Firmware weiter, aber ohne Kontrollleuchten, ohne
abschaltbare Sensorversorgung und ohne Lichtsensor. Details unten unter
„Hinweise".

**3. Zwei Hardware-Änderungen sind Voraussetzung** (beide unten beschrieben):
der **10k/10k-Spannungsteiler am Ladedrucksensor** und der **R61-Umbau auf dem
VIEWE-Board**. Ohne den Teiler zeigt der Ladedruck nur die halbe Spanne.

---

### Neu

**Reifengröße im Menü (SETTINGS → REIFEN)**
- Breite (155–305 mm), Querschnitt (30–85 %), Felge (13–20 Zoll) und ein
  Feintrimm-**Offset** (−10 … +10 %).
- Der **Umfang** steht als eigene Zeile darunter und rechnet beim Drehen live
  mit (Kontrollwert: 225/55 R16 → 2054 mm).
- Aus diesem Umfang berechnet der Tacho seine Geschwindigkeit – deshalb der
  Hinweis oben.
- Passt eine Kombination nicht (z. B. 305/85 R20), wird die Umfang-Zeile rot
  und die Firmware rechnet ersatzweise mit dem Standardwert weiter.

**Geber-Kalibrierung (SETTINGS → SENSOREN → KALIBRIERUNG)**
- NTC-Geber streuen ab Werk um ±15 % – beim Kühlwasser sind das bei echten
  80 °C rund **9 Grad** Anzeigeunterschied von Geber zu Geber. Zu viel, wenn
  eine Übertemperaturwarnung daran hängt.
- Je Geber (Kühlwasser, Öltemperatur, Ladeluft vor/nach LLK, Öldruck) gibt es
  jetzt: `Quelle` · `R-Faktor` · `Offset` · `Ist-Wert` · `Auf Werk zurück`.
- **Ein Messpunkt genügt:** Geber in kochendes Wasser, `R-Faktor` so drehen,
  bis der `Ist-Wert` bei 100 °C steht. Der Faktor zieht die Kennlinie über den
  ganzen Temperaturbereich richtig – anders als ein reiner Offset, der nur an
  einem Punkt stimmen würde.

**Kennlinien von der SD-Karte**
- Wer einen Geber im Wasserbad komplett ausmisst, kann die ganze Tabelle als
  Datei hinterlegen: `/sdcard/cfg/sensor_cal.ini`.
- **Fehlt die Datei, legt die Firmware beim Start selbst eine kommentierte
  Vorlage an** – man bekommt also eine korrekt formatierte Datei, ohne das
  Format kennen zu müssen.
- Punkt **und** Komma sind als Dezimaltrenner erlaubt (deutsches Excel schreibt
  `82,4`), die Reihenfolge der Messpunkte ist egal.
- Neue Menüpunkte `Von SD laden` und `Vorlage auf SD`.
- Eine fehlerhafte Zeile verwirft nur ihren eigenen Geber; im Menü steht dann
  `SD-Fehler Z.42` statt der Quelle.

**Kontrollleuchten (nur mit Adapterboard V2)**
- Instrumentenbeleuchtung, Fernlicht, Blinker, Zündung, Ladekontrolle,
  Vorglühen und die beiden Öldruckschalter werden jetzt über den I/O-Baustein
  des Adapterboards eingelesen und als Warnleuchten angezeigt.

**Dynamische Öldruckkontrolle ist scharf**
- Beide Öldruckschalter (0,3 und 0,9 bar) werden ausgewertet – mit der
  gegenläufigen Logik des Originals: der obere Schalter **muss** oberhalb der
  Auswerte-Drehzahl geschlossen sein. Bleibt er offen, warnt das Display.
  Genau dieser Fall kündigt einen Lagerschaden an, während der 0,3-bar-Schalter
  noch schweigt.
- **Drehzahlschwelle einstellbar** (SENSOREN → `Oeldr.-Sch. ab`: 1500 / 1800 /
  2000 / 2500), weil VW je nach Modell 1500 oder 2000/min verwendet.
- 3 Sekunden Anlauf-Totzeit nach dem Motorstart, sonst gäbe es bei jedem Start
  kurz Alarm, bis die Pumpe Druck aufgebaut hat.
- Ohne Adapterboard V2 melden beide Schalter `N/A` statt „kein Druck" – es gibt
  also keinen Fehlalarm auf älteren Aufbauten.

**Öldruck-Anzeige (5-Bar-Geber)**
- Die Kennlinie des verbauten Widerstandsgebers ist eingebaut; bisher wurde der
  Kanal nur gelesen, aber nicht ausgewertet.
- Über 5 bar wird weitergerechnet statt abgeschnitten – der hohe Kaltstartdruck
  ist real und soll sichtbar sein.
- Kurzschluss und Kabelbruch werden als Elektrikfehler gemeldet, nicht als
  Druckwert. Und zwar **auch bei stehendem Motor**: vorher zeigte ein Geber mit
  abgezogenem Stecker brav „MOTOR AUS", der Kabelbruch blieb bis zum nächsten
  Motorlauf unsichtbar – also ausgerechnet nicht in der Werkstatt.
- Zum Nutzen muss `Oeldruck` im Sensor-Menü eingeschaltet werden.

**Screen 1: zwei Werteblocks neben den Zeigern**
- **Links:** Außentemperatur und Kühlwasser. **Rechts:** Öltemperatur und
  Öldruck.
- Je Block ein eigener Menüpunkt: `Temp links` (Aus / Luft / Wasser / Beide)
  und `Temp rechts` (Aus / Temp / Druck / Beide).
- Werte von Gebern, die im Menü nicht eingeschaltet sind, stehen auf `---`.

**Screen 1: Geschwindigkeitsanzeige mit fünf Modi**
- `Aus` · `Rad km/h` · `Rad km/h (b)` · `GPS km/h` · `Beide`.
- `(b)` blendet zusätzlich die dunklen Segmente hinter den Ziffern ein, wie bei
  einer echten Siebensegmentanzeige. `Beide` zeigt groß das Radsignal und klein
  darüber GPS.

**Screen 3: Drehzahl-Bogen wahlweise fest**
- Aus dem Schalter ist eine Auswahl geworden: `Aus` / `Zeiger` / `Voll`.
  `Voll` stellt den Bogen fest auf 100 %, er dient dann nur noch als ruhige
  Hintergrundfläche hinter dem Zeiger.

**Tempomat-Ausgang im Menü (SETTINGS → TEMPOMAT)**
- Der Geschwindigkeits-Ausgang zum Steuergerät war bisher fest an und nur über
  den Quelltext schaltbar. Jetzt: `Signalausgang` (Hauptschalter),
  `Impulse/m` (1 · 2 · 4 · 6 · 8 · 16, zusätzlich als `/km` angezeigt),
  `Im Simulator` (darf die Demo den Ausgang treiben) und `Frequenz`.
- **Die Frequenzzeile zeigt den tatsächlich am Pin anliegenden Wert**, nicht den
  gerechneten – steht dort `---`, geht wirklich nichts raus. Damit lässt sich
  der Ausgang ohne Messgerät prüfen.
- Der Ausgang liegt jetzt fest auf **GPIO 49**.

**Automatischer Tiefschlaf (SETTINGS → ENERGIE)**
- Neu: `Tiefschlaf nach` – von 1 min in feinen Stufen bis 12 h, oder `Aus`.
  Bisher war der Tiefschlaf nur von Hand über „Tiefschlaf jetzt" erreichbar.
- Neu: `Notweckung` – der Sicherheitstimer, der das Gerät aus dem Tiefschlaf
  zurückholt (Aus / 1 min / 5 min / 15 min / 1 h / 12 h).
- **Beide stehen ab Werk so, dass sich nichts ändert:** `Tiefschlaf nach` ist
  aus, die Notweckung steht wie bisher auf 60 s. Wer beides zusammen abschaltet,
  bekommt eine Warnung ins Protokoll – dann holt das Gerät nur noch die
  Weckleitung zurück.

**Demo-Modus: vier echte Fahrzyklen (SYSTEM → Run Demo)**
- Aus dem An/Aus-Schalter ist eine Auswahl geworden:
  `Aus` · `Stadt` · `Ueberland` · `Autobahn` · `Vollgas`.
- Statt der alten Endlosschleife (Vollgas bis 140, Vollbremsung, von vorn) fährt
  jetzt ein Fahrermodell: Schaltvorgänge mit Schaltloch, Kickdown,
  Überholvorgänge, Halt an der Ampel mit abgestelltem Motor, Kaltstart mit
  Vorglühen.
- **Die Temperaturen verhalten sich wie im echten Fahrzeug:** das Kühlwasser
  hing bisher an der Drehzahl und wanderte sichtbar mit dem Drehzahlmesser mit.
  Jetzt wird eine Wärmebilanz gerechnet – betriebswarm bewegt sich die Anzeige
  um höchstens 1 Grad in 10 Sekunden.
- Blinker, Fernlicht, Vorglühen und Ladekontrolle laufen in der Demo mit; auch
  eine Überladedruck-Störung kommt gelegentlich vor.
- Die Uhr und der Tageskilometerzähler bleiben im Demo-Betrieb jetzt stehen –
  vorher hat der Simulator sie überschrieben bzw. beim Beenden genullt.

**Konfiguration auf die SD-Karte sichern (LOGGER → `Config sichern`)**
- Schreibt **alle** Einstellungen des Geräts als lesbare Datei nach
  `/sdcard/cfg/vedo_config.ini`. Damit ist der Stand erstmals sicherbar; bisher
  war nach einem Speicher-Reset alles weg.
- Zeigt das Ergebnis direkt hinter dem Menüpunkt an (`OK` / `KEINE SD` /
  `FEHLER`).
- Zurückspielen ist bewusst noch nicht dabei – das kommt später.

**FPS-Anzeige statt Sys Monitor**
- Der alte Systemmonitor meldete dauerhaft 100 % CPU. Der Wert war schlicht
  falsch (die Grafikbibliothek konnte ihre Leerlaufzeit nicht sehen).
- Stattdessen jetzt eine schlichte Bildrate auf den beiden Cluster-Screens,
  farbcodiert: grün ab 20, gelb ab 10, darunter rot. Der Menüpunkt heißt jetzt
  `FPS Anzeige`.

---

### Verbessert

**Der Tacho reagiert im unteren Bereich viel schneller**
- Die Anzeige hinkte bei niedrigem Tempo spürbar hinterher, oben nicht.
  Ursache: gemittelt wurde über eine feste Anzahl **Impulse** – und die dauern
  bei 5 km/h zehnmal so lange wie bei 50. Jetzt wird über eine feste **Zeit**
  gemittelt.
- Nachlauf beim Beschleunigen (Messung gegen die alte Kette):

  | Bereich | vorher | jetzt |
  |---|---|---|
  | 2 → 12 km/h | 1,35 s | **0,42 s** |
  | 5 → 20 km/h | 0,93 s | **0,36 s** |
  | 15 → 35 km/h | 0,57 s | **0,32 s** |
  | 40 → 70 km/h | 0,38 s | **0,27 s** |

- Stop-and-Go zwischen 3 und 8 km/h: 0,50 s → **0,04 s**.
- Beim Anhalten **rollt die Anzeige weich aus**, statt stehenzubleiben und dann
  hart auf 0 zu springen.
- Ein dauerhaft schwacher Magnet am Radsensor führte bisher zu 20 % zu wenig
  Anzeige (40 statt 50 km/h) – der fehlende Impuls wird jetzt erkannt und
  eingerechnet.

**Schnelles Durchdrehen der Screens**
- Bisher löste **jede** Encoder-Raste einen kompletten Screen-Wechsel mit
  Auf- und Abblenden aus. Wer aus Versehen 10 Rasten drehte, sah eine über
  10 Sekunden lange Kette von Überblendungen durch alle Zwischenscreens und
  konnte in der Zeit nichts bedienen.
- Jetzt werden Rasten gesammelt: nach 120 ms Ruhe läuft **genau ein** Wechsel
  zum Ziel, Zwischenscreens werden übersprungen. Zurückdrehen wird verrechnet.
- Die Überblendung ist außerdem kürzer (rund 650 statt 1050 ms) und lässt sich
  durch erneutes Drehen abbrechen.

**Standby-Uhr blendet weich auf**
- Der Übergang zur Uhr lief in zehn sichtbaren Stufen. Jetzt blendet nicht nur
  das Licht, sondern auch der Uhr-Inhalt selbst weich auf: ausblenden,
  eine halbe Sekunde Schwarz, dann kommt die Uhr.
- Beim Aufwachen aus dem Schlaf geht es bewusst schneller – da wartet jemand.

**Warnbanner passen jetzt ins Display**
- Vier Meldungen waren länger als die vorgesehene Breite und wuchsen einfach
  über den Rand hinaus. Die Texte sind gekürzt (`Oeldruck NIEDRIG`,
  `Kuehlwasser HEISS`, `Ladedruck HOCH`, `Tank RESERVE`, Sensorfehler als
  `Sensor: COOLANT`), und ein künftig zu langer Text läuft innerhalb der Box
  durch, statt aus dem Bild zu wachsen.

**Kleinere Anzeigesachen**
- Die Systemtemperatur wird ohne Nachkommastelle angezeigt – die Stelle war
  erfunden, der Sensor ist mit ±2 °C spezifiziert.
- Kühlwasser- und Tankzeiger lassen sich nicht mehr abschalten. Das sind die
  klassischen Pflichtinstrumente; abschaltbar zu sein war eher eine Fußangel.
- Der Ladedrucksensor ist auf einen **Bosch TMAP 4 bar** (VW 04L 906 051 C)
  gewechselt. Der Nullpunkt wird jetzt bei stehendem Motor automatisch
  nachgeführt – die Anzeige stimmt damit bei jedem Wetter und auf jeder
  Passhöhe.

---

### Behoben

**Der Tacho zeigte nur die halbe Geschwindigkeit**
- Siehe ganz oben. Betraf auch den Tempomat-Ausgang.

**Auf der SD-Karte landete gar nichts**
- Der Datenlogger und die GPS-Track-Aufzeichnung schlugen **immer** fehl, weil
  lange Dateinamen nicht aktiviert waren – und die Dateien heißen nun mal
  `2026-08-19.csv`. Im Protokoll stand nur „fopen fehlgeschlagen", was wie ein
  Kartenproblem aussieht. Beides funktioniert jetzt.

**Abgeschaltete Anzeigen kamen nach dem Tiefschlaf zurück**
- Gemeldet als „nach dem Deep Sleep sind die Anzeigen wieder da, obwohl das Menü
  Aus sagt". Dahinter steckten zwei Fehler, und beide trafen **jeden Kaltstart**,
  nicht nur den Tiefschlaf: die gespeicherten Einstellungen wurden zwar geladen,
  aber erst beim nächsten Schließen des Menüs angewendet.

**Das Gerät wachte aus dem Tiefschlaf „spontan" auf**
- Es war der eigene Sicherheitstimer: fest auf 60 Sekunden, ohne Weg zurück in
  den Tiefschlaf. Das Gerät weckte sich also jede Minute selbst und blieb dann
  wach. Der Timer ist jetzt der Menüpunkt `Notweckung` und einstellbar.

**Das Cluster wachte im Stand von allein auf**
- Eine Phantom-Geschwindigkeit von 9 km/h holte den Cluster bei stehendem
  Fahrzeug alle paar Minuten aus dem Schlaf – jedes Mal mit komplettem
  Display-Neuaufbau. Ursache war eine Zeitmessung, die im Schlaf stehenbleibt.

**Die Außentemperatur blieb nach dem Schlafen weg**
- Der Fühler (DS18B20) war nach dem ersten Aufwachen bis zum nächsten Neustart
  tot. Der Bus wird jetzt nach dem Aufwachen neu aufgebaut.

**Der Blinker hinkte 500 ms hinterher**
- Der Interrupt des I/O-Bausteins auf dem Adapterboard kam nie an – der
  Herstellerschaltplan nennt den falschen Anschluss. Alles funktionierte
  scheinbar, nur eben träge über den Sicherheits-Poll. Sichtbar war das am
  Blinker-Telltale, das dem Relaistakt nicht folgte.

**Alle 30 Sekunden brach die Bildrate ein**
- Die Diagnose-Ausgabe lief im selben Task wie die Zeitbasis der Grafik. Sie
  läuft jetzt getrennt und mit niedriger Priorität.

**Öltemperatur: abgezogener Stecker war nicht zu erkennen**
- Ein eingeschalteter Geber mit abgezogenem Stecker war von „gar kein Geber
  verbaut" nicht zu unterscheiden (beides grau). Jetzt wird der Kabelbruch als
  roter Fehler gemeldet.

**Weitere Kleinigkeiten**
- Die Kontrollleuchten und das Warnbanner wurden 30-mal pro Sekunde neu
  gezeichnet, auch wenn sich gar nichts änderte.
- Die Demo startete direkt in die Demo, wenn die Menü-Initialisierung
  fehlschlug – und blockierte damit jeden Standby.
- Die GNSS-Meldung „Unplausible Modul-Zeit" kam sechsmal pro Minute ins
  Protokoll, wenn das Gerät ohne Empfang auf dem Basteltisch stand.

---

### Hinweise

**Was du nach dem Update einmal prüfen solltest**

1. **Reifengröße eintragen** (SETTINGS → REIFEN) und den Tacho gegen GPS
   gegenprüfen.
2. Das Cluster startet je nach vorherigem Screen einmalig auf einem anderen –
   Screen 2 ist entfallen (er war im Kern ein Screen 1 mit Tacho und Ladedruck,
   dessen Funktionen jetzt auf Screen 1 sitzen). Ein Encoder-Klick korrigiert
   das dauerhaft.
3. Ein paar Menü-Häkchen können verrutscht sein, weil sich die Reihenfolge der
   Punkte geändert hat – vor allem `Uhrzeit` auf Screen 1 kommt einmal
   eingeschaltet hoch. Einmal durchklicken räumt das auf.
4. Wenn du die Öldruckanzeige nutzen willst: `Oeldruck` unter SENSOREN
   einschalten.

**Adapterboard V2**

Ab dieser Version ist das Adapterboard V2 die vorgesehene Trägerplatine. Die
Firmware prüft die Baugruppen beim Start einzeln und läuft ohne sie weiter –
„V2 nötig" gilt für den *Funktionsumfang*, nicht für die Lauffähigkeit. Ohne
das Board fehlen:

- alle **Kontrollleuchten** aus dem I/O-Baustein (Blinker, Fernlicht, Zündung,
  Ladekontrolle, Vorglühen, Instrumentenbeleuchtung, beide Öldruckschalter),
- die **abschaltbare Sensorversorgung** (die Geber sind dann dauerhaft
  bestromt, auch im Schlaf),
- der **Lichtsensor** für die automatische Hinterleuchtung.

**Zwei Hardware-Änderungen sind Voraussetzung**

- **10k/10k-Spannungsteiler am Ladedrucksensor.** Ohne ihn zeigt die Firmware
  nur die halbe Spanne, der Ladedruck läuft also deutlich zu niedrig. Prüfen
  ohne Rechnerei: Rohspannung auf dem Debug-Screen bei Motor aus vorher
  notieren – nach dem Umbau muss dort exakt die Hälfte stehen.
- **R61 auf dem VIEWE-Board umgelötet** (10k jetzt als Pulldown nach Masse
  statt als Pull-up). Ohne den Umbau läuft der Funk-Coprozessor im Tiefschlaf
  weiter. Achtung für später: der Coprozessor kommt danach nur noch hoch, wenn
  die Firmware ihn einschaltet.

**Was noch offen ist**

- Der **Ruhestrom im Tiefschlaf** liegt bei 8,4 mA statt der angestrebten
  1,1–1,3 mA. Der Schlafpfad selbst ist geprüft und sauber – zwischen Light
  Sleep (10,2 mA) und Tiefschlaf liegen nur 1,8 mA, der Verbraucher sitzt also
  aller Wahrscheinlichkeit nach auf der Trägerplatine und nicht in der Firmware.
- Die **Steigung der Öldruck-Kennlinie** ist noch nicht gegen einen
  Referenzdruck geprüft; der Nullpunkt stimmt (gemessen 0,289 V gegen 0,300 V
  laut Datenblatt).
- Beim **Ladedruck** steckt noch ein Korrekturfaktor von 15 % drin, der gegen
  das VDO-Instrument abgeglichen wurde. Woher die Abweichung kommt, ist erst zu
  einem knappen Viertel erklärt – das gehört nach dem Teiler-Umbau erneut
  geprüft.
- Im Demo-Betrieb zeigen Kühlwasser, Öltemperatur, Öldruck und die beiden
  Ladelufttemperaturen im Live Monitor einen Sensorfehler an. Der Simulator
  liefert für diese Geber keine Rohspannung – im Fahrbetrieb ist davon nichts
  betroffen.

**Sonstiges**

- Firmware-Datei heißt jetzt `vedo_klartext_v1.2.0.bin`. Flash-Adressen
  unverändert – siehe [README.md](README.md).
- Nach dem Flashen im Boot-Screen prüfen, ob dort **v1.2.0** steht.
- Status weiterhin **Beta**.

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
