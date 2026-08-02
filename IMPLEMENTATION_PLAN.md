# StudyTutor – Implementierungsplan

## Arbeitsweise

Die Entwicklung erfolgt in vertikalen, testbaren Phasen.

Nach jedem Arbeitspaket muss Codex:

1. Code ausführen oder kompilieren
2. Tests ausführen
3. Fehler beheben
4. `TASKS.md` aktualisieren
5. Dokumentation aktualisieren
6. wichtige Entscheidungen in `DECISIONS.md` festhalten
7. einen verständlichen Git-Commit erstellen, sofern Git verfügbar ist

Eine Aufgabe darf nur als abgeschlossen markiert werden, wenn sie implementiert und geprüft wurde.

---

# Phase 1 – Backend Foundation

## Ziele

* Python-Projekt initialisieren
* FastAPI starten
* Konfiguration implementieren
* Datenbank anbinden
* Provider-Abstraktionen erstellen
* Docker Compose vorbereiten

## Aufgaben

1. Monorepo-Struktur erstellen
2. Python-Projekt mit `pyproject.toml` initialisieren
3. FastAPI-Anwendung erstellen
4. zentrale Settings mit Umgebungsvariablen implementieren
5. strukturiertes Logging einrichten
6. PostgreSQL und pgvector konfigurieren
7. SQLAlchemy integrieren
8. Alembic integrieren
9. Basismodelle erstellen
10. `LLMProvider` definieren
11. `VisionProvider` definieren
12. `EmbeddingProvider` definieren
13. Ollama-Provider implementieren
14. OpenAI-kompatiblen Provider implementieren
15. Mock-Provider implementieren
16. `/health` implementieren
17. `/capabilities` implementieren
18. `/models` implementieren
19. Dockerfile erstellen
20. Docker Compose erstellen
21. Unit- und Integrationstests schreiben

## Abschlusskriterien

* Backend startet
* Datenbankverbindung funktioniert
* Migrationen laufen
* Health-Endpunkt antwortet
* Provider-Verbindung kann geprüft werden
* Tests laufen reproduzierbar

---

# Phase 2 – Dokumentimport

## Ziele

* PDFs sicher hochladen
* Dokumente persistent speichern
* Verarbeitungsjobs erzeugen
* Status anzeigen

## Aufgaben

1. Dokumentmodelle erstellen
2. Upload-Endpunkt implementieren
3. Dateityp validieren
4. Grössenlimit implementieren
5. SHA-256-Hashing implementieren
6. Duplikaterkennung implementieren
7. sichere Dateipfade implementieren
8. ProcessingJob implementieren
9. Status-Endpunkt implementieren
10. In-Process-Worker implementieren
11. Fortschrittsmeldungen implementieren
12. Abbruch und Retry vorbereiten
13. Tests schreiben

## Abschlusskriterien

* PDF kann hochgeladen werden
* ungültige Dateien werden abgelehnt
* Duplikate werden erkannt
* Jobstatus wird persistent gespeichert
* API bleibt während der Verarbeitung ansprechbar

---

# Phase 3 – PDF- und Layoutanalyse

## Ziele

* Dokumentstruktur erfassen
* Text, Tabellen und Bilder extrahieren
* OCR-Fallback implementieren

## Aufgaben

1. Docling integrieren
2. Dokumentseiten speichern
3. Überschriften erkennen
4. Absätze und Listen extrahieren
5. Tabellen extrahieren
6. Bildunterschriften extrahieren
7. Formeln erfassen, soweit unterstützt
8. PDF-Seiten als Bilder rendern
9. Bilder und Grafiken extrahieren
10. OCR-Entscheidungslogik implementieren
11. OCR-Fallback integrieren
12. strukturierte Dokumentelemente speichern
13. synthetisches Test-PDF erzeugen
14. Tests schreiben

## Abschlusskriterien

* digitales PDF wird verarbeitet
* gescanntes PDF wird verarbeitet
* Seitenzahlen bleiben korrekt
* Tabellen werden strukturiert gespeichert
* Seitenbilder werden erzeugt

---

# Phase 4 – Chunking und Retrieval

## Ziele

* Dokumente sinnvoll segmentieren
* Embeddings erzeugen
* hybride Suche implementieren

## Aufgaben

1. Chunking-Service implementieren
2. Kapitelhierarchie erhalten
3. visuelle und textuelle Elemente verknüpfen
4. Embedding-Provider verwenden
5. pgvector-Speicherung implementieren
6. lexikalische Suche implementieren
7. semantische Suche implementieren
8. Metadatenfilter implementieren
9. Resultate zusammenführen
10. Quellenreferenzen erzeugen
11. Retrieval-Endpunkt erstellen
12. Tests schreiben

## Abschlusskriterien

* relevante Chunks werden gefunden
* Seitenzahlen stimmen
* Tabellen und Grafiken bleiben referenzierbar
* Retrieval funktioniert mit Mock-Embeddings

---

# Phase 5 – Textbasierter Tutor

## Ziele

* Lernziele erzeugen
* Fragen stellen
* Antworten bewerten
* Quellen anzeigen

## Aufgaben

1. Lernzielmodell implementieren
2. Lernziele aus Dokumenten erzeugen
3. TutorSession implementieren
4. Fragen generieren
5. Antwortbewertungsschema implementieren
6. JSON-Validierung implementieren
7. Retry bei ungültiger Ausgabe
8. korrekte Punkte erkennen
9. fehlende Punkte erkennen
10. falsche Punkte erkennen
11. Fehlkonzepte speichern
12. Folgefragen auswählen
13. Quellen an Antworten anhängen
14. Streaming implementieren
15. Tests schreiben

## Abschlusskriterien

* Tutor kann eine Lernsession beginnen
* Tutor stellt dokumentbezogene Fragen
* Antworten werden differenziert bewertet
* Quellen werden angezeigt
* fehlende Dokumentgrundlage wird transparent angegeben

---

# Phase 6 – Visuelle Analyse

## Ziele

* Diagramme und medizinische Abbildungen verstehen
* visuelle Inhalte für Retrieval und Tutor nutzbar machen

## Aufgaben

1. relevante Seiten erkennen
2. Vision-Provider anbinden
3. Bilder vor Inferenz skalieren
4. strukturiertes Vision-Schema definieren
5. Beschriftungen extrahieren
6. Beziehungen extrahieren
7. Unsicherheiten speichern
8. Bildbeschreibung mit Begleittext verknüpfen
9. Vision-Ergebnisse cachen
10. visuelle Chunks erstellen
11. Grafikfragen generieren
12. Quellen auf Bilder beziehen
13. Tests mit Mock-Vision-Provider schreiben

## Abschlusskriterien

* Grafik wird als visuelles Element gespeichert
* zentrale Beschriftungen werden erfasst
* Nutzer kann Fragen zur Grafik stellen
* Antwort referenziert die richtige Seite und Grafik

---

# Phase 7 – Spaced Repetition

## Ziele

* Lernfortschritt langfristig speichern
* Wiederholungen planen

## Aufgaben

1. ReviewItem implementieren
2. Wiederholungsalgorithmus kapseln
3. Stabilität speichern
4. Schwierigkeit speichern
5. nächste Wiederholung berechnen
6. Selbstbewertung einbeziehen
7. Modellscore einbeziehen
8. Fehlkonzepte berücksichtigen
9. Due-Reviews-Endpunkt implementieren
10. Fortschrittsendpunkt implementieren
11. Tests schreiben

## Abschlusskriterien

* beantwortete Fragen erzeugen Review-Elemente
* Wiederholungsdatum wird berechnet
* fällige Inhalte können abgefragt werden
* Algorithmus ist deterministisch testbar

---

# Phase 8 – Native iOS-App

## Ziele

* vollständiger End-to-End-Ablauf auf dem iPhone

## Aufgaben

1. SwiftUI-Projekt erstellen
2. API-Client implementieren
3. Serverkonfiguration implementieren
4. API-Key im Keychain speichern
5. Verbindungstest implementieren
6. Dokumentenbibliothek erstellen
7. PDF-Importer implementieren
8. Upload-Fortschritt anzeigen
9. Analysefortschritt anzeigen
10. Dokumentansicht erstellen
11. Lernziele anzeigen
12. Tutor-Chat erstellen
13. Lernsession erstellen
14. Antwortfeedback anzeigen
15. Quellenansicht erstellen
16. Grafikansicht mit Zoom erstellen
17. Wiederholungen anzeigen
18. Fortschritt lokal speichern
19. Offline- und Fehlerzustände behandeln
20. Swift-Tests erstellen

## Abschlusskriterien

* App verbindet sich mit dem Backend
* PDF kann ausgewählt und hochgeladen werden
* Verarbeitungsstatus wird angezeigt
* Lernsession funktioniert
* Quellen und Grafiken können geöffnet werden

---

# Phase 9 – Raspberry-Pi-Deployment

## Ziele

* reproduzierbarer Betrieb auf Raspberry Pi 5 mit 16 GB RAM

## Aufgaben

1. ARM64-Dockerfiles prüfen
2. Raspberry-Pi-Compose-Profil erstellen
3. Ollama-Konfiguration dokumentieren
4. llama.cpp-Konfiguration dokumentieren
5. Modellprofile konfigurieren
6. Parallelität begrenzen
7. persistente Volumes konfigurieren
8. Backup-Skript erstellen
9. Restore-Skript erstellen
10. Healthchecks konfigurieren
11. automatische Neustarts aktivieren
12. systemd-Integration dokumentieren
13. lokale Netzwerkverbindung dokumentieren
14. HTTPS oder Tailscale dokumentieren
15. Speicher- und Lasttests ausführen

## Abschlusskriterien

* Backend startet auf ARM64
* Daten bleiben nach Neustart erhalten
* Ollama ist erreichbar
* iPhone kann im lokalen Netzwerk zugreifen
* Backup und Restore funktionieren

---

# Phase 10 – End-to-End-Validierung

## Testablauf 1

* Backend starten
* PDF hochladen
* Verarbeitung abschliessen
* Text und Seiten prüfen
* dokumentbezogene Frage stellen

## Testablauf 2

* PDF mit Tabelle und Diagramm hochladen
* visuelle Verarbeitung prüfen
* Frage zur Grafik stellen
* Quelle öffnen

## Testablauf 3

* Lernsession starten
* Frage beantworten
* Feedback erhalten
* Wiederholung prüfen

## Testablauf 4

* Ollama stoppen
* Fehlerverhalten prüfen
* Ollama erneut starten
* Wiederherstellung prüfen

## Testablauf 5

* Dokument löschen
* Dateien und Datenbankeinträge prüfen

## Projektabschluss

Das MVP ist abgeschlossen, wenn alle fünf Abläufe reproduzierbar funktionieren und in `TASKS.md` dokumentiert sind.

