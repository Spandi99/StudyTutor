# StudyTutor – Implementierungsstatus

Codex muss diese Datei während der Entwicklung aktuell halten.

Eine Aufgabe darf nur als abgeschlossen markiert werden, wenn sie implementiert und getestet wurde.

---

# Phase 1 – Backend Foundation

## Repository

* [ ] finale Monorepo-Struktur erstellen
* [ ] `.gitignore` erstellen
* [ ] `env.example` erstellen
* [ ] Python-Projekt initialisieren
* [ ] Entwicklungsanleitung erstellen

## FastAPI

* [ ] FastAPI-Anwendung erstellen
* [ ] Settings-System implementieren
* [ ] strukturiertes Logging implementieren
* [ ] globale Fehlerbehandlung implementieren
* [ ] Request-ID implementieren
* [ ] OpenAPI-Metadaten konfigurieren

## Datenbank

* [ ] PostgreSQL konfigurieren
* [ ] pgvector aktivieren
* [ ] SQLAlchemy einrichten
* [ ] Alembic einrichten
* [ ] erste Migration erstellen
* [ ] Datenbanktests erstellen

## Provider

* [ ] `LLMProvider` definieren
* [ ] `VisionProvider` definieren
* [ ] `EmbeddingProvider` definieren
* [ ] Ollama-Provider implementieren
* [ ] OpenAI-kompatiblen Provider implementieren
* [ ] Mock-Provider implementieren
* [ ] Timeout-Behandlung implementieren
* [ ] Retry mit Backoff implementieren
* [ ] Provider-Tests erstellen

## API

* [ ] `GET /health`
* [ ] `GET /capabilities`
* [ ] `GET /models`
* [ ] API-Key-Authentifizierung

## Deployment

* [ ] Backend-Dockerfile
* [ ] Docker Compose
* [ ] PostgreSQL-Volume
* [ ] Dateispeicher-Volume
* [ ] Healthchecks
* [ ] ARM64-Kompatibilität

## Phase-1-Abnahme

* [ ] Backend startet lokal
* [ ] Backend startet mit Docker Compose
* [ ] Migrationen laufen
* [ ] `/health` antwortet
* [ ] Modellverbindung kann getestet werden
* [ ] alle Phase-1-Tests bestehen

---

# Phase 2 – Dokumentimport

* [ ] `StudyDocument` implementieren
* [ ] `ProcessingJob` implementieren
* [ ] Dokument-Repository implementieren
* [ ] PDF-Upload implementieren
* [ ] MIME-Type prüfen
* [ ] Dateiendung prüfen
* [ ] PDF-Signatur prüfen
* [ ] Upload-Grössenlimit implementieren
* [ ] SHA-256-Hash berechnen
* [ ] Duplikate erkennen
* [ ] sicheren Dateipfad erzeugen
* [ ] Original-PDF speichern
* [ ] In-Process-Worker implementieren
* [ ] Jobfortschritt speichern
* [ ] Jobfehler speichern
* [ ] Jobabbruch implementieren
* [ ] Retry implementieren
* [ ] Dokumentliste implementieren
* [ ] Dokumentdetails implementieren
* [ ] Status-Endpunkt implementieren
* [ ] Dokumentlöschung vorbereiten
* [ ] Upload-Tests schreiben

## Phase-2-Abnahme

* [ ] gültiges PDF kann hochgeladen werden
* [ ] ungültige Datei wird abgelehnt
* [ ] Duplikat wird erkannt
* [ ] Jobstatus bleibt nach Neustart erhalten
* [ ] API blockiert während Verarbeitung nicht

---

# Phase 3 – Dokumentverarbeitung

* [ ] Docling integrieren
* [ ] ARM64-Verfügbarkeit prüfen
* [ ] `DocumentPage` implementieren
* [ ] `DocumentElement` implementieren
* [ ] `DocumentImage` implementieren
* [ ] Überschriften extrahieren
* [ ] Absätze extrahieren
* [ ] Listen extrahieren
* [ ] Tabellen extrahieren
* [ ] Bildunterschriften extrahieren
* [ ] Formeln erfassen
* [ ] jede Seite als Bild rendern
* [ ] eingebettete Bilder extrahieren
* [ ] OCR-Entscheidungslogik implementieren
* [ ] OCR-Fallback implementieren
* [ ] Seitenzahlen verifizieren
* [ ] synthetisches Test-PDF erzeugen
* [ ] Verarbeitungstests schreiben

## Phase-3-Abnahme

* [ ] digitales PDF wird verarbeitet
* [ ] gescanntes PDF wird verarbeitet
* [ ] Tabellen werden erkannt
* [ ] Bilder werden gespeichert
* [ ] Seitenzahlen bleiben korrekt

---

# Phase 4 – Chunking und Retrieval

* [ ] `DocumentChunk` implementieren
* [ ] hierarchisches Chunking implementieren
* [ ] Kapitelpfade speichern
* [ ] Inhaltstypen speichern
* [ ] Text und Grafik verknüpfen
* [ ] Embeddings erzeugen
* [ ] Embeddings in pgvector speichern
* [ ] Vektorsuche implementieren
* [ ] lexikalische Suche implementieren
* [ ] Metadatenfilter implementieren
* [ ] Resultatfusion implementieren
* [ ] Kontextbudget implementieren
* [ ] Quellenobjekte implementieren
* [ ] Retrieval-Endpunkt implementieren
* [ ] Retrieval-Tests schreiben

## Phase-4-Abnahme

* [ ] relevante Chunks werden gefunden
* [ ] irrelevante Dokumente können ausgeschlossen werden
* [ ] Seitenreferenzen stimmen
* [ ] Tabellen und Bilder bleiben referenzierbar

---

# Phase 5 – Tutor

* [ ] `LearningObjective` implementieren
* [ ] `StudyQuestion` implementieren
* [ ] `TutorSession` implementieren
* [ ] `TutorMessage` implementieren
* [ ] `AnswerEvaluation` implementieren
* [ ] Lernziele generieren
* [ ] Erklärmodus implementieren
* [ ] offene Fragen implementieren
* [ ] Multiple Choice implementieren
* [ ] Vergleichsfragen implementieren
* [ ] klinische Transferfragen implementieren
* [ ] strukturiertes Bewertungsschema implementieren
* [ ] JSON-Schema validieren
* [ ] ungültige Modellausgaben behandeln
* [ ] Fehlkonzepte speichern
* [ ] Folgeaktion bestimmen
* [ ] Quellen an Antworten anhängen
* [ ] Streaming implementieren
* [ ] Tutor-Tests schreiben

## Phase-5-Abnahme

* [ ] Lernsession kann gestartet werden
* [ ] Frage basiert auf dem Dokument
* [ ] Antwort wird differenziert bewertet
* [ ] Quellen werden angezeigt
* [ ] fehlende Evidenz wird transparent angegeben

---

# Phase 6 – Visuelle Analyse

* [ ] Vision-Relevanz einer Seite bestimmen
* [ ] Bildvorverarbeitung implementieren
* [ ] Bildskalierung implementieren
* [ ] Vision-Schema definieren
* [ ] Vision-Provider anbinden
* [ ] Diagrammtyp erfassen
* [ ] Labels erfassen
* [ ] Beziehungen erfassen
* [ ] Lernpunkte erfassen
* [ ] Unsicherheiten erfassen
* [ ] Vision-Ergebnisse cachen
* [ ] visuelle Chunks erzeugen
* [ ] Grafik-Retrieval implementieren
* [ ] Grafikfragen implementieren
* [ ] Bildquellen implementieren
* [ ] Mock-Vision-Tests schreiben

## Phase-6-Abnahme

* [ ] Grafik wird erkannt
* [ ] visuelle Beschreibung wird gespeichert
* [ ] Frage zur Grafik kann beantwortet werden
* [ ] korrekte Seite wird als Quelle angegeben

---

# Phase 7 – Spaced Repetition

* [ ] `ReviewItem` implementieren
* [ ] Wiederholungsalgorithmus implementieren
* [ ] Stabilität speichern
* [ ] Schwierigkeit speichern
* [ ] nächstes Review berechnen
* [ ] Selbstbewertung speichern
* [ ] Antwortscore berücksichtigen
* [ ] Fehlkonzepte berücksichtigen
* [ ] Due-Reviews-Endpunkt implementieren
* [ ] Fortschrittsendpunkt implementieren
* [ ] deterministische Tests schreiben

## Phase-7-Abnahme

* [ ] Review wird nach Antwort erzeugt
* [ ] Wiederholungsdatum wird berechnet
* [ ] fällige Reviews können geladen werden

---

# Phase 8 – iOS-App

* [ ] Xcode-Projekt erstellen
* [ ] SwiftUI-App-Struktur erstellen
* [ ] API-Client implementieren
* [ ] Datenmodelle implementieren
* [ ] Serverkonfiguration erstellen
* [ ] Keychain-Speicherung implementieren
* [ ] Verbindungstest implementieren
* [ ] Dokumentenbibliothek implementieren
* [ ] PDF-Auswahl implementieren
* [ ] Upload implementieren
* [ ] Upload-Fortschritt implementieren
* [ ] Analysefortschritt implementieren
* [ ] Dokumentdetailansicht implementieren
* [ ] Kapitelansicht implementieren
* [ ] Lernzielansicht implementieren
* [ ] Tutor-Chat implementieren
* [ ] Lernsession implementieren
* [ ] Antwortfeedback implementieren
* [ ] Quellenansicht implementieren
* [ ] Grafikansicht mit Zoom implementieren
* [ ] Review-Ansicht implementieren
* [ ] Fortschrittsansicht implementieren
* [ ] Diagnoseansicht implementieren
* [ ] Fehlerzustände implementieren
* [ ] Swift-Tests schreiben

## Phase-8-Abnahme

* [ ] App verbindet sich mit dem Server
* [ ] PDF kann hochgeladen werden
* [ ] Verarbeitungsstatus wird angezeigt
* [ ] Tutor-Session funktioniert
* [ ] Quellen können geöffnet werden

---

# Phase 9 – Raspberry Pi 5

* [ ] Raspberry Pi OS 64-bit dokumentieren
* [ ] ARM64-Images prüfen
* [ ] Pi-Compose-Profil erstellen
* [ ] Ollama-Installation dokumentieren
* [ ] llama.cpp-Alternative dokumentieren
* [ ] Textmodellprofil konfigurieren
* [ ] Vision-Modellprofil konfigurieren
* [ ] Embedding-Modellprofil konfigurieren
* [ ] Kontextlänge begrenzen
* [ ] Parallelität begrenzen
* [ ] Modell-Warmup implementieren
* [ ] persistente Volumes prüfen
* [ ] automatischen Neustart konfigurieren
* [ ] Backup-Skript erstellen
* [ ] Restore-Skript erstellen
* [ ] lokale Netzwerkverbindung dokumentieren
* [ ] HTTPS oder VPN dokumentieren
* [ ] Lasttests durchführen
* [ ] RAM-Verbrauch dokumentieren

## Phase-9-Abnahme

* [ ] Backend läuft auf Raspberry Pi 5
* [ ] Ollama ist erreichbar
* [ ] Neustart verliert keine Daten
* [ ] iPhone erreicht den Server
* [ ] Backup und Restore funktionieren

---

# Phase 10 – End-to-End-Tests

* [ ] PDF mit Text verarbeiten
* [ ] gescanntes PDF verarbeiten
* [ ] PDF mit Tabelle verarbeiten
* [ ] PDF mit Diagramm verarbeiten
* [ ] Frage zum Text beantworten
* [ ] Frage zur Grafik beantworten
* [ ] Lernsession abschliessen
* [ ] Review planen
* [ ] LLM-Ausfall testen
* [ ] ungültige Modellausgabe testen
* [ ] Dokument vollständig löschen
* [ ] Prompt-Injection-PDF testen
* [ ] End-to-End-Ergebnisse dokumentieren

---

# Aktueller Arbeitsstand

## Zuletzt abgeschlossen

Noch nichts implementiert.

## Aktuell in Arbeit

Phase 1 – Backend Foundation

## Bekannte Blocker

Keine.

## Nächste Aufgabe

Repository-Struktur und FastAPI-Grundgerüst erstellen.

