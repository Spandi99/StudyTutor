# StudyTutor – Architektur

## 1. Übersicht

StudyTutor verwendet eine Client-Server-Architektur.

```text
iPhone-App
    │
    │ HTTPS oder lokales HTTP während Entwicklung
    ▼
FastAPI-Backend
    ├── Authentifizierung
    ├── Dokumentverwaltung
    ├── Jobverwaltung
    ├── Retrieval
    ├── Tutor-Engine
    └── Fortschrittsverwaltung
          │
          ├── PostgreSQL + pgvector
          ├── Dateispeicher
          ├── Docling und OCR
          ├── Ollama oder llama.cpp
          └── Embedding- und Vision-Modelle
```

## 2. iOS-Client

Die iOS-App ist für Darstellung und Benutzerinteraktion verantwortlich.

Sie übernimmt:

* Serverkonfiguration
* sichere Speicherung des API-Keys
* PDF-Auswahl
* Upload
* Statusanzeige
* Tutor-Interaktion
* Anzeige von Quellen
* Anzeige von Grafiken
* lokale Zwischenspeicherung
* Fehlerzustände

Die App führt nicht die vollständige PDF- oder Modellverarbeitung selbst aus.

## 3. Backend

Das FastAPI-Backend ist die zentrale Orchestrierungsschicht.

Es übernimmt:

* Authentifizierung
* Dateiverwaltung
* Dokumentverarbeitung
* Datenpersistenz
* Modellaufrufe
* Retrieval
* Tutor-Logik
* Antwortbewertung
* Wiederholungsplanung
* Streaming
* Diagnose

## 4. Datenbank

Primäre Datenbank:

* PostgreSQL
* pgvector

Die relationale Datenbank speichert:

* Dokumentmetadaten
* Seiten
* Elemente
* Chunks
* Bildmetadaten
* Lernziele
* Fragen
* Sessions
* Bewertungen
* Reviews
* Jobs
* Modellkonfigurationen

Embeddings werden über pgvector gespeichert.

## 5. Dateispeicher

Der Server verwendet einen persistenten lokalen Dateispeicher.

Beispiel:

```text
data/
├── documents/
│   └── <document-uuid>/
│       ├── original.pdf
│       ├── pages/
│       ├── figures/
│       ├── ocr/
│       └── processing/
├── cache/
└── backups/
```

Dateipfade dürfen nicht direkt aus Nutzereingaben übernommen werden.

## 6. Dokumentpipeline

```text
PDF-Upload
    ↓
Validierung und Hashing
    ↓
Persistenter ProcessingJob
    ↓
Docling-Layoutanalyse
    ↓
Text-, Tabellen- und Bildextraktion
    ↓
Seitenrendering
    ↓
OCR-Fallback
    ↓
Vision-Analyse relevanter Bilder
    ↓
Hierarchisches Chunking
    ↓
Embeddings
    ↓
Datenbank und Vektorspeicher
    ↓
Lernziele und vorbereitete Lerninhalte
```

## 7. Modellarchitektur

Die Modellschicht ist providerunabhängig.

### LLMProvider

Verantwortlich für:

* Erklärungen
* Fragegenerierung
* Antwortbewertung
* Zusammenfassungen
* strukturierte JSON-Ausgaben

### VisionProvider

Verantwortlich für:

* Diagrammbeschreibung
* Bilderkennung
* Beschriftungserkennung
* Relationserkennung
* visuelle Lernpunkte
* Unsicherheiten

### EmbeddingProvider

Verantwortlich für:

* Chunk-Embeddings
* Query-Embeddings
* Batch-Verarbeitung

## 8. Unterstützte Provider

### OllamaProvider

Verwendung eines selbst gehosteten Ollama-Servers.

### OpenAICompatibleProvider

Verwendung beliebiger kompatibler Endpunkte, beispielsweise:

* llama.cpp Server
* vLLM
* andere selbst gehostete kompatible Server

### MockProvider

Deterministischer Provider für automatisierte Tests.

## 9. Retrieval

Das Retrieval kombiniert:

* Vektorsuche
* lexikalische Suche
* Metadatenfilter
* Dokumentfilter
* Seitenfilter
* Inhaltstypfilter

Der Ablauf:

```text
Nutzerfrage
    ↓
Query-Normalisierung
    ↓
lexikalische Suche + Vektorsuche
    ↓
Resultatfusion
    ↓
optionales Reranking
    ↓
Kontextbudgetierung
    ↓
LLM-Aufruf
    ↓
Antwort mit Quellen
```

## 10. Quellenmodell

Jede Antwort kann Quellen enthalten.

Eine Quelle verweist auf:

* Dokument-ID
* Dokumenttitel
* Seitenzahl
* Abschnitt
* Chunk-ID
* Dokumentelement
* optional Bild-ID
* optional Bounding Box
* verwendeten Textausschnitt

Die App muss aus einer Antwort zur entsprechenden Seite oder Grafik navigieren können.

## 11. Tutor-Engine

Die Tutor-Engine verwaltet einen expliziten Lernzustand.

```text
Lernziel auswählen
    ↓
Vorwissen oder aktuelles Niveau bestimmen
    ↓
Erklärung oder Frage erzeugen
    ↓
Antwort empfangen
    ↓
Antwort bewerten
    ↓
Fehlkonzepte und Wissenslücken speichern
    ↓
nächste Aktion bestimmen
    ↓
Review planen
```

Mögliche nächste Aktionen:

* fortfahren
* vereinfachen
* nacherklären
* Rückfrage stellen
* Transferfrage stellen
* Lernziel wiederholen
* Session abschliessen

## 12. Hintergrundverarbeitung

Das MVP verwendet einen In-Process-Worker mit persistenter Jobtabelle.

Die Architektur muss später einen externen Worker erlauben.

Jeder Job speichert:

* Status
* Fortschritt
* aktuellen Verarbeitungsschritt
* Fehlermeldung
* Anzahl Versuche
* Startzeit
* Endzeit
* Abbruchstatus

Statuswerte:

* `pending`
* `running`
* `completed`
* `failed`
* `cancelled`

## 13. Raspberry-Pi-Betrieb

Auf dem Raspberry Pi gelten standardmässig:

* ein rechenintensiver Job gleichzeitig
* kleine Batch-Grössen
* quantisierte Modelle
* sequenzielle Vision-Verarbeitung
* begrenzte Kontextlänge
* persistentes Caching
* Modellaufrufe mit Timeout
* Modell-Warmup
* Swap nicht als Ersatz für ausreichenden RAM behandeln

Das Textmodell und das Vision-Modell müssen nicht zwingend gleichzeitig im Speicher liegen.

## 14. Sicherheit

Vertrauensgrenzen:

```text
Nutzer
    ↓ nicht vertrauenswürdige Eingabe
iOS-App
    ↓ authentifizierte API-Anfrage
Backend
    ↓ nicht vertrauenswürdiger Dokumentinhalt
Dokumentpipeline
    ↓ kontrollierter Kontext
Modellprovider
```

PDF-Inhalte werden stets als Daten gekennzeichnet.

Dokumente dürfen keine Systemprompts, Providerkonfigurationen oder Sicherheitsregeln überschreiben.

## 15. Fehlerbehandlung

Fehler werden in typisierte Kategorien eingeteilt:

* Validierungsfehler
* Authentifizierungsfehler
* Datei-Verarbeitungsfehler
* OCR-Fehler
* Modell nicht erreichbar
* Modell-Timeout
* ungültige Modellausgabe
* Datenbankfehler
* Speichermangel
* Job abgebrochen

Die App erhält verständliche Meldungen und keine internen Stack-Traces.

## 16. Observability

Das System verwendet:

* strukturierte Logs
* Request-ID
* Job-ID
* Dokument-ID
* Verarbeitungsdauer
* Modell-Latenz
* Fehlerkategorie

Dokumentinhalte und vollständige Nutzerantworten dürfen standardmässig nicht in Logs geschrieben werden.

## 17. Backup

Backups umfassen:

* PostgreSQL-Dump
* Dokumentdateien
* extrahierte Bilder
* Konfiguration ohne Geheimnisse

Backups müssen wiederherstellbar und rotierbar sein.

## 18. Erweiterbarkeit

Spätere Erweiterungen sollen möglich bleiben:

* Web-App
* mehrere Nutzer
* mehrere Server
* externe Worker
* Remote-Vision-Server
* Speech-to-Text für mündliche Prüfungen
* Audioantworten
* geteilte Kurse
* Import von PowerPoint-Dateien
* Import von Webseiten
* Anki-Export

