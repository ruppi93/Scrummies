# 🚶 Besucherzählung mit Arduino Uno R4 WiFi & REST-API

---

## 📌 Einleitung

Dieses Projekt wurde im Rahmen des Moduls **Softwareentwicklung – Semester 2** durchgeführt.  
Ziel des Projekts ist die Entwicklung einer **einfachen, modular erweiterbaren Besucherzählung** für einen Eingang.

Ein Arduino Uno R4 WiFi erfasst Besucherbewegungen mithilfe von zwei IR-Sensoren.  
Die erfassten Daten werden per WLAN im JSON-Format an eine lokale REST-API übertragen.  
Die API verarbeitet die Daten, ergänzt sie um einen serverseitigen Zeitstempel und speichert sie für eine spätere Auswertung (z. B. in Excel).

Der Fokus des Projekts liegt auf:
- der sauberen Trennung zwischen Embedded-System und Serverlogik  
- der zuverlässigen Datenübertragung  
- einer nachvollziehbaren, iterativen Projektentwicklung  

---

## 🎯 Projektziele

- Zählen von Besuchern (Eintritt / Austritt)
- Zuverlässige WLAN-Kommunikation
- Übertragung der Daten per HTTP/JSON
- Zentrale Verarbeitung der Daten in einer REST-API
- Speicherung der Daten zur späteren Auswertung
- Strukturierte und dokumentierte Projektarbeit

---

## 👥 Team & Rollen

**Teamname:** Scrummies  

| Rolle | Name |
|------|------|
| Product Owner | David Graber |
| Scrum Master | Stefan Ruppert |
| Tech Lead | Robin à Porta |

---

## 🧱 Systemübersicht

### Arduino Uno R4 WiFi
- Erfassung der Sensordaten
- Verarbeitung der Besucherlogik
- Erstellung von JSON-Daten
- Übertragung der Daten an die API

### REST-API (C++ / Crow)
- Empfang und Validierung der Daten
- Serverseitige Zeitstempel-Erzeugung
- Speicherung der Daten im Speicher
- Vorbereitung der Daten für CSV-Export
- Senden der Daten an die CSV mit Zeitstempel und "Kommen" "Gehen"

---

## 📌 Übersicht der Projektmeilensteine

| Nr. | Meilenstein | Beschreibung |
|----|------------|--------------|
| 1 | Arduino-Verbindung | Arduino Uno R4 WiFi in der Entwicklungsumgebung lauffähig machen |
| 2 | WLAN-Konfiguration | Stabile WLAN-Verbindung im Schulnetz herstellen |
| 3 | API im gleichen Subnetz | REST-API lokal erreichbar machen |
| 4 | Testdaten senden | Testweise JSON-Daten an die API übertragen |
| 5 | Sensorik auslesen | IR-Sensoren zur Besucherzählung einsetzen |
| 6 | Datenexport API | Speicherung der Daten als CSV-Datei |

---

# 📅 Projektverlauf

---

## 🗓️ 28.11. – Meilenstein 1: Arduino-Verbindung

### 🎯 Ziel des Tages
- Arduino Uno R4 WiFi in der Entwicklungsumgebung lauffähig machen  
- Sicherstellen, dass Build- und Toolchain-Prozess korrekt funktionieren  

### Erklärung
Ein minimaler Test diente zur Überprüfung von:
- korrekter Board-Erkennung  
- funktionierender Toolchain  
- erfolgreichem Build-Prozess  

### ⚠ Probleme
- Arduino wurde von CLion zunächst nicht erkannt  
- PlatformIO war nicht korrekt installiert  
- Fehlende oder falsch eingebundene Toolchains führten zu Build-Fehlern  

### ✅ Endergebnis
- PlatformIO korrekt installiert und konfiguriert  
- Arduino Uno R4 WiFi wird zuverlässig erkannt  
- Erstes Testprogramm erfolgreich kompiliert  

---

## 🗓️ 06.12. – Meilenstein 2 & 4: WLAN-Konfiguration & Testdaten senden

### 🎯 Ziel des Tages
- WLAN-Verbindung im Schulnetz herstellen  
- Kommunikation zwischen Arduino und REST-API testen  

### Erklärung – WLAN
Der Arduino verbindet sich mit dem Schul-WLAN.  
Das Programm wartet blockierend, bis eine stabile Verbindung besteht, um Verbindungsabbrüche während der Datenübertragung zu vermeiden.

### Erklärung – Testdatenübertragung
Zur Überprüfung der Kommunikation werden statische Testdaten an die API gesendet.  
Diese dienen ausschließlich dazu, die HTTP-POST-Verbindung und die Erreichbarkeit der API zu testen.

### ⚠ Probleme
- Unterschiedliche `HttpClient`-Implementierungen führten zu Problemen  
- Falsche Nutzung von API-Signaturen verursachte Compilerfehler  
- Konstruktoren und Methoden mussten an die verwendete Library angepasst werden  

### ✅ Endergebnis
- Stabile WLAN-Verbindung im Schulnetz  
- Erfolgreiche Testübertragung der Daten  
- API antwortet korrekt mit **HTTP 201 (Created)**  

---

## 🗓️ 12.12. – Meilenstein 5: Sensorik auslesen

### 🎯 Ziel des Tages
- Reale Sensordaten auslesen  
- Besucherzählung implementieren  

### Erklärung
Zwei IR-Sensoren erfassen die Bewegungsrichtung von Personen.  
Die Reihenfolge der Sensorauslösung bestimmt, ob es sich um einen Eintritt oder einen Austritt handelt.  
Ein Schutzmechanismus verhindert negative Besucherzahlen.

### ⚠ Probleme
- Sensoren reagieren nicht immer zuverlässig  
- Timing ist bei schnellen Bewegungen kritisch  
- Mehrfachauslösungen können auftreten  

### ✅ Endergebnis
- Besucherzählung grundsätzlich funktionsfähig  
- Sensorlogik implementiert  
- Weitere Optimierung der Sensorik erforderlich
- Auslesen der Sensordaten noch ungenau

---

## 🗓️ 12.12. – Meilenstein 6: Datenexport API (CSV)

### 🎯 Ziel des Tages
- Speicherung der empfangenen Sensordaten in einer CSV-Datei  

### Erklärung
Die REST-API empfängt JSON-Daten vom Arduino, validiert diese und speichert sie zeilenweise in einer CSV-Datei.  
Diese CSV-Datei dient als Grundlage für eine spätere Auswertung der Besucherzahlen (z. B. in Excel).

### ⚠ Probleme
- CSV-Datei wird noch nicht zuverlässig geschrieben  
- Dateipfad und Datenzugriffe müssen korrigiert werden  
- Fehlerbehandlung im Dateizugriff ist noch unvollständig  

### ✅ Endergebnis
- API verarbeitet eingehende Daten korrekt  
- CSV-Export konzeptionell umgesetzt  
- Funktionale Fertigstellung noch ausstehend  

---

## 🗓️ 20.12. – Meilenstein 5: Sensorik auslesen

### 🎯 Ziel des Tages
- Auslesen der Sensordaten anhand 2 Tatser. 

### Erklärung
2 Tatser erkennen bei Tatsendurck nun die Besucher. Ob diese kommen oder gehen. Diesen schritt mussten wir so ändern, da die vorhanden Sensoren zu ungenau waren, und einen zu grossen Sensorebreich hatten 
Ändern des Codes und Datensendung mit Strings

### ⚠ Probleme
- Korrektes anschliessen der Tatser
- Code zum entsprellen, dass die Tatser nur bei Tatsendruck sicken  
 

### ✅ Endergebnis
- Komme und Gehen wird bei Tatsendruck gesendet


## 🗓️ 20.12. – Meilenstein 6: Datenexport API (CSV)

### 🎯 Ziel des Tages
- Speicherung der empfangenen Sensordaten in einer CSV-Datei, mit Kommen und Gehen und Zeitstempel

### Erklärung
Die REST-API empfängt JSON-Daten vom Arduino, validiert diese und speichert sie zeilenweise in einer CSV-Datei.  
Diese CSV-Datei dient als Grundlage für eine spätere Auswertung der Besucherzahlen (z. B. in Excel).

### ⚠ Probleme
- Korrektes Schreiben der emofangen JSONS in die CSV gab Probeleme, da "/" falsch geschrieben war
- Ändern des Const_Char auf String, um die Daten sauber auswerten zu können

### ✅ Endergebnis
- API verarbeitet eingehende Daten korrekt  
- CSV-Export konzeptionell umgesetzt  
- Funktionale Fertigstellung noch ausstehend  

---
   

## 📊 Aktueller Projektstand

### Arduino
- WLAN-Verbindung stabil  
- JSON-Übertragung funktioniert  
- Sensorlogik implementiert  
-Sensorerkennung auf Tatsendruck abgeändert und getestet.  

### API
- POST- und GET-Endpunkte funktionsfähig  
- Serverseitige Zeitstempel implementiert  
- CSV-Export noch fehlerhaft  

---

## 📘 Lessons Learned

- PlatformIO erfordert eine saubere Toolchain- und Pfadkonfiguration  
- Embedded-Systeme sind stark von Library-Versionen abhängig  
- API-Signaturen müssen exakt eingehalten werden  
- Serverseitige Zeitstempel sind zuverlässiger als clientseitige Zeitangaben  
- Sensorik benötigt Entprellung und sauberes Timing  

---

## 🚧 Offene Punkte & Ausblick

- Fehlerfreier CSV-Export  
- Optimierung der Datenstruktur  
- Erweiterung um statistische Auswertungen  
- Optionale Visualisierung der Besucherzahlen  

---

## 🚧 Änderungen

- Änderung von IR Sensoren auf Taster 
- Änderung von "LeftActive" und "RightActive" auf "Kommen" und "Gehen"
- Änderung des Bildschirms auf ein kleiens OLED Dispay

---


 ## 🗓️ 20.12. – Tests und Fehlersuche

  

| Testart        | Testinhalt            | Fehler / Risiken                               | Beschreibung                                                                 |
|---------------|-----------------------|-----------------------------------------------|------------------------------------------------------------------------------|
| Funktionstest | Datenanzeige          | Keine Anzeige auf dem Bildschirm              | Keine Anzeige, Daten werden jedoch korrekt versendet.                        |
| Funktionstest | Datenanzeige          | CSV-Datei wird nicht erstellt                 | Der im Code angegebene Pfad existiert nicht.                                 |
| Funktionstest | JSON-Inhalt           | Falscher Inhalt                               | API gibt HTTP-Status 400 bei fehlendem oder falschem JSON-Format zurück.     |
| Funktionstest | Schalter              | Ein- bzw. Ausgang wird nicht erkannt          | Der Drücker wird zur Funktionsprüfung mittels LED überprüft.                |
| Funktionstest | Stromversorgung       | Keine Datensammlung im Sperrbildschirm        | Im Sperrbildschirm verhindert der Laptop das korrekte Sammeln und Senden der Arduino-Daten. |
| Funktionstest | Verbindung            | Keine Verbindung zwischen API und Arduino     | IP-Adresse und Netzwerkeinstellungen auf Korrektheit überprüfen.            |

