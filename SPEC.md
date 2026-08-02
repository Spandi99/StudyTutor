# StudyTutor – Produktspezifikation

## 1. Produktvision

StudyTutor ist eine native iPhone-App mit selbst gehostetem Backend, die Vorlesungsfolien und andere PDF-Dokumente in interaktive Lerninhalte umwandelt.

Die Anwendung soll nicht nur Text extrahieren. Sie muss auch Tabellen, Diagramme, Flussdiagramme, medizinische Abbildungen, beschriftete Grafiken, Formeln und gescannte Seiten erfassen.

Ein Open-Source-Sprachmodell soll den Stoff erklären, Fragen stellen, Antworten bewerten, Wissenslücken erkennen und Wiederholungen planen.

## 2. Zentrale Anforderungen

Das System muss:

* ohne zwingende kostenpflichtige API funktionieren
* auf einem Raspberry Pi 5 mit 16 GB RAM laufen
* über das lokale Netzwerk vom iPhone erreichbar sein
* Open-Source-Modelle verwenden
* PDFs strukturiert verarbeiten
* Grafiken und Abbildungen analysieren
* Quellen und Seitenzahlen anzeigen
* Lernfortschritt speichern
* modular und erweiterbar sein

## 3. Systemkomponenten

Das System besteht aus:

1. nativer iOS-App
2. FastAPI-Backend
3. relationaler Datenbank
4. Vektorsuche
5. Dokumentverarbeitung
6. OCR
7. Text-Sprachmodell
8. Vision-Language-Modell
9. Embedding-Modell
10. Tutor-Engine
11. Spaced-Repetition-Engine

## 4. Technologievorgaben

### iOS

* Swift
* SwiftUI
* SwiftData
* URLSession
* async/await
* PDFKit
* Keychain

### Backend

* Python 3.12 oder eine aktuelle kompatible Version
* FastAPI
* Pydantic
* SQLAlchemy
* Alembic
* PostgreSQL
* pgvector
* pytest
* strukturierte Logs

### Deployment

* Docker Compose
* ARM64-kompatible Container
* persistente Volumes
* Healthchecks
* automatische Neustarts
* `.env`-Konfiguration

### Dokumentverarbeitung

Primär:

* Docling

Ergänzend:

* PDF-Seitenrendering
* OCR-Fallback
* Bildextraktion
* Tabellenextraktion
* strukturierte Layoutanalyse

## 5. Modellprovider

Definiere getrennte Abstraktionen:

* `LLMProvider`
* `VisionProvider`
* `EmbeddingProvider`

Implementiere mindestens:

* `OllamaProvider`
* `OpenAICompatibleProvider`
* `MockProvider`

Die konkreten Modellnamen müssen konfigurierbar sein.

Erforderliche Umgebungsvariablen:

```text
LLM_PROVIDER=
LLM_BASE_URL=
LLM_MODEL=
VISION_MODEL=
EMBEDDING_MODEL=
LLM_API_KEY=
LLM_CONTEXT_LENGTH=
LLM_TIMEOUT_SECONDS=
```

## 6. Hardwareprofile

### Raspberry-Pi-Profil

Für Raspberry Pi 5 mit 16 GB RAM:

* quantisiertes Textmodell
* ungefähr 3 bis 4 Milliarden Parameter als Ausgangspunkt
* Q4-Quantisierung
* begrenztes Kontextfenster
* maximal ein rechenintensiver Job gleichzeitig
* sequenzielle Bildverarbeitung
* kleine Embedding-Batches
* Bildskalierung vor Vision-Inferenz
* Caching aller wiederverwendbaren Resultate

### Remote-Profil

Für leistungsfähigere Server:

* grössere Textmodelle
* separates Vision-Modell
* grössere Kontextfenster
* parallele Dokumentverarbeitung
* optionales Reranking

## 7. Dokumentimport

Der Nutzer muss PDFs über die iOS-App importieren können.

Beim Upload muss das Backend:

1. Dateityp prüfen
2. Dateigrösse prüfen
3. sicheren Dateinamen erzeugen
4. SHA-256-Hash berechnen
5. Duplikate erkennen
6. Dokument persistent speichern
7. Verarbeitungsjob erzeugen
8. Status an die App melden

## 8. Dokumentverarbeitung

Für jedes PDF muss folgende Pipeline ausgeführt werden:

1. Dokumentstruktur analysieren
2. Überschriften erkennen
3. Absätze und Listen extrahieren
4. Tabellen extrahieren
5. Formeln erfassen, soweit technisch möglich
6. Bilder und Grafiken extrahieren
7. jede Seite als Bild rendern
8. OCR bei unzureichendem digitalem Text ausführen
9. visuell relevante Seiten identifizieren
10. visuelle Inhalte analysieren
11. Text und visuelle Beschreibung verknüpfen
12. Dokument hierarchisch segmentieren
13. Chunks erstellen
14. Embeddings berechnen
15. Inhalte in der Datenbank speichern
16. Lernziele erzeugen

Die Pipeline darf sich nicht ausschliesslich auf einfache PDF-Textextraktion verlassen.

## 9. Visuelle Analyse

Für jede Seite soll bestimmt werden, ob sie enthält:

* Diagramm
* Tabelle
* Flussdiagramm
* anatomische Abbildung
* histologische Abbildung
* radiologische Abbildung
* beschriftete Struktur
* Formel
* Screenshot
* sonstige lernrelevante Grafik

Das Vision-Modell soll strukturierte Ergebnisse liefern.

Beispiel:

```json
{
  "page_number": 12,
  "visual_type": "flowchart",
  "title": "Regulation der HPA-Achse",
  "description": "Die Grafik zeigt die hormonelle Regulation der HPA-Achse.",
  "labels": [
    "Hypothalamus",
    "CRH",
    "Hypophyse",
    "ACTH",
    "Nebennierenrinde",
    "Cortisol"
  ],
  "relationships": [
    {
      "source": "Hypothalamus",
      "relation": "setzt frei",
      "target": "CRH"
    }
  ],
  "educational_points": [
    "Cortisol hemmt Hypothalamus und Hypophyse über negative Rückkopplung."
  ],
  "uncertainties": []
}
```

Unklare Bildinhalte dürfen nicht als sicher dargestellt werden.

## 10. Chunk-Daten

Jeder Chunk benötigt mindestens:

* ID
* Dokument-ID
* Seitenzahl
* Kapitelpfad
* Abschnittspfad
* Inhaltstyp
* Text
* visuelle Beschreibung
* Schlüsselbegriffe
* Embedding
* Quellenreferenz
* optional Bounding Box
* Erstellungsdatum
* Verarbeitungsstatus

Mögliche Inhaltstypen:

* `paragraph`
* `heading`
* `list`
* `table`
* `formula`
* `figure`
* `diagram`
* `caption`
* `ocr_text`
* `combined`

## 11. Retrieval-Augmented Generation

Das Retrieval muss hybrid arbeiten:

1. semantische Vektorsuche
2. lexikalische Suche
3. Metadatenfilter
4. Zusammenführung der Resultate
5. optionales Reranking
6. Begrenzung des Modellkontexts

Retrieval-Resultate müssen ihre Herkunft behalten:

* Dokument
* Seite
* Abschnitt
* Grafik
* Tabelle
* Textausschnitt

Antworten dürfen nicht so formuliert werden, als stammten sie sicher aus dem Dokument, wenn die Informationsgrundlage nicht ausreicht.

## 12. Tutor-Funktionen

Der Tutor ist kein gewöhnlicher Chatbot.

Eine Lernsession umfasst:

1. Thema oder Lernziel auswählen
2. Vorwissen abschätzen
3. kurze Erklärung geben
4. aktive Abruffrage stellen
5. Nutzerantwort erfassen
6. Antwort analysieren
7. korrekte Punkte identifizieren
8. fehlende Punkte identifizieren
9. falsche Aussagen identifizieren
10. Fehlkonzepte erkennen
11. gezielte Rückmeldung geben
12. nächste sinnvolle Frage auswählen
13. Lernfortschritt aktualisieren
14. Wiederholung planen

## 13. Lernmodi

Die App soll mindestens anbieten:

* Stoff erklären
* Tutor-Chat
* offene Fragen
* Multiple Choice
* mündliche Prüfung
* Prüfungssimulation
* schwierige Inhalte
* Wiederholungsrunde
* Grafiktraining
* klinische Transferfragen

## 14. Fragetypen

Unterstützte Fragetypen:

* Wissensfrage
* Erklärfrage
* Vergleich
* Ursache und Wirkung
* Reihenfolge
* Zuordnung
* Multiple Choice
* klinischer Fall
* Transferfrage
* Grafikinterpretation
* Bildbeschriftung
* Fehlererkennung

## 15. Antwortbewertung

Antworten dürfen nicht nur durch exakte Wortübereinstimmung bewertet werden.

Die Modellantwort soll einem Schema entsprechen:

```json
{
  "score": 0.75,
  "correct_points": [],
  "missing_points": [],
  "incorrect_points": [],
  "misconceptions": [],
  "feedback": "",
  "follow_up_question": "",
  "recommended_action": "continue"
}
```

Das Backend muss prüfen:

* Score liegt zwischen 0 und 1
* Ausgabe entspricht dem Schema
* Quellen sind vorhanden
* ungültige Ausgaben werden sicher behandelt
* bei Bedarf erfolgt ein begrenzter erneuter Modellaufruf

## 16. Spaced Repetition

Die Wiederholungslogik soll gekapselt implementiert werden.

Pro Lernziel oder Review-Element werden gespeichert:

* Schwierigkeit
* Stabilität
* letzter Abruf
* nächste Wiederholung
* Anzahl Versuche
* Erfolgsquote
* durchschnittlicher Score
* erkannte Fehlkonzepte

Selbstbewertung:

* vergessen
* schwierig
* gut
* einfach

## 17. iOS-Ansichten

Mindestens erforderlich:

1. Onboarding
2. Serverkonfiguration
3. Verbindungstest
4. Dokumentenbibliothek
5. PDF-Import
6. Upload-Fortschritt
7. Analysefortschritt
8. Dokumentübersicht
9. Kapitelübersicht
10. Lernzielübersicht
11. Tutor-Chat
12. Lernsession
13. Antwortfeedback
14. Grafikansicht
15. Quellenansicht
16. Wiederholungen
17. Fortschrittsansicht
18. Einstellungen
19. Serverdiagnose

## 18. Backend-Endpunkte

Mindestens erforderlich:

```text
GET    /health
GET    /capabilities
GET    /models

POST   /documents
GET    /documents
GET    /documents/{id}
DELETE /documents/{id}
GET    /documents/{id}/status
GET    /documents/{id}/pages
GET    /documents/{id}/learning-objectives
POST   /documents/{id}/reprocess

POST   /retrieval/search

POST   /tutor/session
POST   /tutor/respond
POST   /tutor/explain
POST   /tutor/generate-question

POST   /reviews
GET    /reviews/due
GET    /progress
```

Für Fortschrittsmeldungen soll Server-Sent Events oder WebSocket verwendet werden.

## 19. Hintergrundjobs

Dokumentverarbeitung darf normale API-Anfragen nicht blockieren.

Erforderlich:

* In-Process-Worker für die erste Version
* persistenter Jobstatus
* Fortschritt von 0 bis 100 Prozent
* Fehlermeldung
* Retry
* Jobabbruch
* Wiederaufnahme fehlgeschlagener Jobs
* Abstraktion für späteres Redis/RQ/Celery-System

Auf dem Raspberry Pi läuft standardmässig nur ein rechenintensiver Job gleichzeitig.

## 20. Datenmodelle

Mindestens erforderlich:

* `LocalProfile`
* `StudyDocument`
* `DocumentPage`
* `DocumentElement`
* `DocumentImage`
* `DocumentChunk`
* `LearningObjective`
* `StudyQuestion`
* `TutorSession`
* `TutorMessage`
* `AnswerEvaluation`
* `ReviewItem`
* `ProcessingJob`
* `ModelConfiguration`

IDs sollen UUIDs sein.

## 21. Sicherheit

Implementiere:

* API-Key-Authentifizierung
* Upload-Grössenlimit
* Dateitypprüfung
* Schutz vor Path Traversal
* sichere Dateispeicherung
* keine sensiblen Inhalte in Logs
* keine Telemetrie
* keine fest codierten Zugangsdaten
* vollständige Dokumentlöschung
* Trennung von Systemprompt und Dokumentinhalt
* Schutz gegen Prompt Injection aus PDFs
* Request-Timeouts
* Rate-Limit-Abstraktion

Dokumentinhalte sind Daten und niemals Systeminstruktionen.

## 22. Löschung

Beim Löschen eines Dokuments müssen entfernt werden:

* Original-PDF
* Seitenbilder
* extrahierte Grafiken
* OCR-Ergebnisse
* Dokumentseiten
* Chunks
* Embeddings
* Lernziele
* Fragen
* Tutor-Sessions, soweit zugeordnet
* Review-Elemente
* Verarbeitungsjobs

## 23. Performance

Implementiere:

* Streaming
* Modell-Warmup
* kurze Prompts
* adaptive Kontextlänge
* Chunk-Selektion vor Modellaufruf
* Timeouts
* Retry mit Backoff
* Request-Cancellation
* Caching
* Dokument-Hashing
* Bildskalierung
* begrenzte Parallelität
* kleine Embedding-Batches
* konfigurierbare Speichergrenzen

## 24. Tests

Automatisierte Tests werden benötigt für:

* Health-Endpunkt
* Authentifizierung
* PDF-Upload
* ungültige Dateien
* Duplikaterkennung
* Text-Extraktion
* OCR-Fallback
* Tabellen
* visuelle Metadaten
* Chunking
* Embeddings
* Retrieval
* Quellenzuordnung
* Tutor-Sessions
* Antwortbewertung
* ungültige Modellantworten
* Spaced Repetition
* Provider-Ausfälle
* vollständige Dokumentlöschung
* Prompt Injection in Dokumenten

Tests müssen mit Mock-Providern ohne aktives LLM ausführbar sein.

## 25. Akzeptanzkriterien

### Dokumentablauf

* Backend startet reproduzierbar.
* App kann ein PDF hochladen.
* Backend verarbeitet das Dokument.
* Status wird angezeigt.
* Seiten, Text und Struktur werden gespeichert.

### Grafikablauf

* Ein Test-PDF enthält eine Grafik oder Tabelle.
* Die visuelle Information wird gespeichert.
* Der Nutzer kann dazu eine Frage stellen.
* Die Antwort verweist auf die korrekte Seite.

### Tutorablauf

* Tutor stellt eine Frage.
* Nutzer gibt eine Antwort.
* Antwort wird differenziert bewertet.
* Fehlende Punkte werden erklärt.
* Fortschritt wird gespeichert.
* Wiederholung wird geplant.

### Fehlerablauf

* LLM ist nicht erreichbar.
* Backend stürzt nicht ab.
* App stürzt nicht ab.
* Der Nutzer erhält eine verständliche Fehlermeldung.

### Löschablauf

* Ein Dokument wird gelöscht.
* Alle davon abhängigen Dateien und Datensätze werden entfernt.

## 26. Codequalität

Nicht erlaubt:

* Pseudocode anstelle einer Implementierung
* leere Platzhalterfunktionen
* erfundene Bibliotheksmethoden
* fest codierte Zugangsdaten
* zwingende kostenpflichtige Dienste
* ungetestete Erfolgsmeldungen

Erwartet:

* typisierte Python-Funktionen
* Swift Concurrency
* strukturierte Fehlerbehandlung
* Migrationen
* Tests
* verständliche Dokumentation
* reproduzierbares Setup

