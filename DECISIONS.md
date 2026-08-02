# StudyTutor – Architecture Decision Records

Diese Datei enthält wichtige technische Entscheidungen.

Neue Entscheidungen werden fortlaufend ergänzt.

---

## ADR-001 – RAG statt Fine-Tuning auf einzelnen PDFs

### Status

Akzeptiert

### Entscheidung

Dokumentinhalte werden über Retrieval-Augmented Generation bereitgestellt. Einzelne Vorlesungsfolien werden nicht durch Fine-Tuning in das Sprachmodell trainiert.

### Begründung

* Dokumente können jederzeit hinzugefügt oder gelöscht werden.
* Quellen und Seitenzahlen bleiben nachvollziehbar.
* Änderungen erfordern kein erneutes Modelltraining.
* RAG benötigt weniger Rechenleistung.
* Inhalte verschiedener Dokumente können getrennt werden.

---

## ADR-002 – Native SwiftUI-App

### Status

Akzeptiert

### Entscheidung

Der iPhone-Client wird nativ mit Swift und SwiftUI entwickelt.

### Begründung

* gute Integration mit iOS
* nativer PDF-Importer
* sichere Keychain-Speicherung
* gute Performance
* klare Unterstützung von async/await
* langfristig wartbare iPhone-App

---

## ADR-003 – FastAPI-Backend

### Status

Akzeptiert

### Entscheidung

Das Backend wird mit Python und FastAPI entwickelt.

### Begründung

* gute Unterstützung für KI- und Dokumentbibliotheken
* automatische OpenAPI-Dokumentation
* asynchrone Endpunkte
* typisierte Request- und Response-Schemas
* gute Testbarkeit

---

## ADR-004 – PostgreSQL und pgvector

### Status

Akzeptiert

### Entscheidung

Produktiv wird PostgreSQL mit pgvector eingesetzt.

### Begründung

* relationale Daten und Embeddings in einem System
* Filterung nach Dokument und Metadaten
* Datenintegrität
* Migrationen
* gute spätere Skalierbarkeit

---

## ADR-005 – Docling als primäre Dokumentpipeline

### Status

Akzeptiert, sofern ARM64-Kompatibilität bestätigt wird

### Entscheidung

Docling wird primär für Layout- und Dokumentanalyse verwendet.

### Fallback

Falls eine benötigte Docling-Komponente auf ARM64 nicht zuverlässig läuft, wird eine modulare Alternative eingesetzt. Die Abweichung muss dokumentiert und getestet werden.

---

## ADR-006 – Providerunabhängige Modellschicht

### Status

Akzeptiert

### Entscheidung

Textmodell, Vision-Modell und Embedding-Modell werden über getrennte Interfaces angesprochen.

### Implementierungen

* Ollama
* OpenAI-kompatibler Server
* Mock-Provider

### Begründung

Modelle und Server können ohne grundlegende Änderungen der Anwendung ausgetauscht werden.

---

## ADR-007 – Raspberry Pi 5 mit 16 GB als Zielsystem

### Status

Akzeptiert

### Entscheidung

Das primäre Zielsystem ist ein Raspberry Pi 5 mit 16 GB RAM und 64-bit Raspberry Pi OS.

### Konsequenzen

* ARM64-kompatible Builds
* quantisierte Modelle
* begrenzte Parallelität
* Caching
* sequenzielle Bildverarbeitung
* keine Annahme von GPU-Beschleunigung
* Vision-Verarbeitung kann deutlich langsamer als Textverarbeitung sein

---

## ADR-008 – Vision-Analyse als eigene Pipeline

### Status

Akzeptiert

### Entscheidung

Visuelle Inhalte werden nicht nur über OCR verarbeitet. Relevante Seiten und Bilder werden zusätzlich durch ein Vision-Language-Modell analysiert.

### Begründung

OCR allein erfasst keine:

* Pfeilbeziehungen
* Diagrammlogik
* anatomischen Strukturen
* Bildkomposition
* Kurvenverläufe
* visuellen Zusammenhänge

---

## ADR-009 – In-Process-Worker für das MVP

### Status

Akzeptiert

### Entscheidung

Die erste Version verwendet einen lokalen In-Process-Worker mit persistentem Jobstatus.

### Bedingung

Die Worker-Schnittstelle muss einen späteren Wechsel zu Redis, RQ oder Celery ermöglichen.

---

## ADR-010 – Keine zwingende Cloud-Abhängigkeit

### Status

Akzeptiert

### Entscheidung

Alle Kernfunktionen müssen mit selbst gehosteten Open-Source-Komponenten funktionieren.

Externe APIs dürfen später optional angeboten werden, aber nicht Voraussetzung für die Nutzung sein.

---

## ADR-011 – Dokumente sind nicht vertrauenswürdig

### Status

Akzeptiert

### Entscheidung

Inhalte aus PDFs werden immer als nicht vertrauenswürdige Daten behandelt.

Anweisungen innerhalb eines Dokuments dürfen:

* keine Systemprompts überschreiben
* keine Tools aufrufen
* keine Konfiguration verändern
* keine Sicherheitsregeln deaktivieren
* keine anderen Dokumente auslesen

---

## Neue Entscheidung ergänzen

Vorlage:

```text
## ADR-XXX – Titel

### Status

Vorgeschlagen | Akzeptiert | Verworfen | Ersetzt

### Kontext

Welches Problem muss entschieden werden?

### Entscheidung

Welche Lösung wurde gewählt?

### Begründung

Warum wurde diese Lösung gewählt?

### Konsequenzen

Welche Vor- und Nachteile entstehen?
```

