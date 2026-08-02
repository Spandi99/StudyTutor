# StudyTutor

StudyTutor ist eine selbst gehostete Lernplattform für Vorlesungsfolien und andere PDF-Dokumente.

Nutzer können PDFs über eine native iPhone-App importieren. Das Backend analysiert Texte, Tabellen, Diagramme, medizinische Abbildungen und gescannte Seiten. Anschliessend erzeugt ein Open-Source-Sprachmodell daraus interaktive Lernlektionen, Fragen, Erklärungen und Wiederholungen.

## Projektziele

StudyTutor soll:

* Vorlesungsfolien und PDFs verarbeiten
* Texte, Tabellen und visuelle Inhalte erkennen
* Fragen zum Stoff generieren
* offene Antworten differenziert bewerten
* Wissenslücken erkennen
* Quellen mit Seitenangaben anzeigen
* Spaced Repetition verwenden
* ohne zwingende kostenpflichtige APIs funktionieren
* auf einem Raspberry Pi 5 mit 16 GB RAM betrieben werden können

## Systemarchitektur

Das Projekt besteht aus:

* nativer iOS-App mit SwiftUI
* FastAPI-Backend
* PostgreSQL mit pgvector
* Dokumentverarbeitung mit Docling und OCR
* Open-Source-LLMs über Ollama oder eine OpenAI-kompatible API
* Retrieval-Augmented Generation
* Vision-Modell zur Analyse von Grafiken und Abbildungen

## Repository-Struktur

```text
StudyTutor/
├── ios/
├── backend/
├── deployment/
├── scripts/
├── sample-data/
├── README.md
├── SPEC.md
├── IMPLEMENTATION_PLAN.md
├── ARCHITECTURE.md
├── DECISIONS.md
└── TASKS.md
```

## Zielhardware

Primäres Zielsystem:

* Raspberry Pi 5
* 16 GB RAM
* Raspberry Pi OS 64-bit
* ARM64
* SSD-Speicher empfohlen

Entwicklung und alternative Server:

* macOS mit Apple Silicon
* Linux x86-64
* ARM64-Linux

## Modellbetrieb

Modelle sollen über konfigurierbare Provider angesprochen werden.

Unterstützt werden sollen:

* Ollama
* llama.cpp Server
* andere OpenAI-kompatible API-Endpunkte
* Mock-Provider für Tests

Es darf keine kostenpflichtige API als zwingende Voraussetzung geben.

## Dokumentation

Die vollständigen Anforderungen stehen in:

* `SPEC.md`
* `IMPLEMENTATION_PLAN.md`
* `ARCHITECTURE.md`
* `DECISIONS.md`
* `TASKS.md`

## Aktueller Status

Das Projekt befindet sich in der Initialisierungsphase.

Der jeweils aktuelle Entwicklungsstand wird in `TASKS.md` dokumentiert.

## Sicherheitsgrundsätze

* keine Telemetrie
* keine obligatorischen Cloud-Dienste
* Dokumente bleiben standardmässig auf dem selbst gehosteten Server
* keine Zugangsdaten im Repository speichern
* Konfiguration über Umgebungsvariablen
* Dokumentinhalte gelten niemals als Systeminstruktionen

## Lizenz

Die Lizenz wird vor der ersten öffentlichen Veröffentlichung festgelegt.

