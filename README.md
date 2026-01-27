# ARD Stand-Up – Infrastruktur & Automation

Repository für das **ARD Stand-Up** Projekt (WDR).

## Repository-Struktur

```
├── README.md
├── CLAUDE_CODE_PROMPT.md
├── Schemas/
│   ├── Sendungen_Fields.md
│   └── Contentverzeichnis_Fields.md
├── Flows/
│   ├── Sendungen→Planner(Phase1).json
│   ├── Sendungen→TeamsAdaptiveCard.json
│   ├── SyncSendungs-Stand.json
│   ├── SyncSendungs-Stand←Content.json
│   ├── SyncContentElementeLookup.json
│   └── WenneinElementerstelltwird-NeuenOrdnererstellen.json
└── Docs/
    └── Architecture.md
```

## Tech-Stack

| Komponente     | Verwendung         |
|----------------|-------------------|
| SharePoint     | Datenhaltung       |
| Power Automate | Workflows          |
| Planner        | Task-Management    |
| Teams          | Adaptive Cards     |

## Power Automate Flows

| Flow | Beschreibung |
|------|--------------|
| **Sendungen→Planner (Phase1)** | Synchronisiert Sendungen-Einträge mit Planner-Tasks (Titel, Bucket, Due Date, Attachments) |
| **Sendungen→TeamsAdaptiveCard** | Postet/aktualisiert Adaptive Cards in Teams bei Sendungsänderungen |
| **SyncSendungs-Stand** | Aggregiert Content-Status zu Sendungs-Gesamtstatus (In Bearbeitung → Abnahmebereit → Fertig) |
| **SyncSendungs-Stand←Content** | Trigger-Flow für Content-Änderungen |
| **SyncContentElementeLookup** | Synchronisiert ContentElementeLookup-Feld zwischen Listen |
| **WenneinElementerstelltwird...** | Erstellt automatisch Projektordner bei neuen Einträgen |

## SharePoint-Listen

| Liste | GUID | Schema |
|-------|------|--------|
| ARD Stand-Up Sendungen | `08c76ffb-e10b-4158-92b5-73a509a221cc` | [Sendungen_Fields.md](Schemas/Sendungen_Fields.md) |
| Contentverzeichnis | `6e406f83-7400-4568-88d3-25469885a7da` | [Contentverzeichnis_Fields.md](Schemas/Contentverzeichnis_Fields.md) |

## Für Claude Code

> **Wichtige Regeln bei der Arbeit mit diesem Repository:**

1. **IMMER** `Schemas/` für InternalNames nutzen
2. **NIEMALS** DisplayNames raten
3. HTML-Felder normalisieren (`<div>`, `<br>`, `&nbsp;` → saubere Strings)
4. MultiChoice-Felder = Arrays → `join()` verwenden
5. Lookup-Felder: Auf `/Value` oder `/Id` Suffix achten
6. Bei Flow-Änderungen: JSON in `Flows/` aktualisieren

## Weiterführende Dokumentation

- [Architecture.md](Docs/Architecture.md) – Systemarchitektur und Datenflüsse
- [CLAUDE_CODE_PROMPT.md](CLAUDE_CODE_PROMPT.md) – System-Prompt für Claude

---

*Stand: Januar 2026*
