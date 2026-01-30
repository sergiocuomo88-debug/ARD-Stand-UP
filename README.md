# ARD Stand-Up – Infrastruktur & Automation

Repository für das **ARD Stand-Up** Projekt (WDR).

## Repository-Struktur

```
├── README.md
├── CLAUDE_CODE_PROMPT.md
├── Schemas/
│   ├── Sendungen_Fields.csv
│   └── Contentverzeichnis_Fields.csv
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

| Komponente | Verwendung |
|------------|------------|
| SharePoint | Datenhaltung (Listen) |
| Power Automate | Workflows & Sync |
| Planner | Aufgabenverwaltung |
| Teams | Adaptive Cards |

## SharePoint-Listen

| Liste | GUID | Schema |
|-------|------|--------|
| Sendungen | `08c76ffb-e10b-4158-92b5-73a509a221cc` | [Sendungen_Fields.csv](Schemas/Sendungen_Fields.csv) |
| Contentverzeichnis | `6e406f83-7400-4568-88d3-25469885a7da` | [Contentverzeichnis_Fields.csv](Schemas/Contentverzeichnis_Fields.csv) |

## Power Automate Flows

| Flow | Beschreibung |
|------|--------------|
| Sendungen→Planner | Synchronisiert Sendungs-Einträge mit Planner-Aufgaben |
| Sendungen→TeamsAdaptiveCard | Postet/aktualisiert Adaptive Cards in Teams-Channel |
| SyncSendungs-Stand | Aggregiert Content-Status zum Gesamt-Sendungsstatus |
| SyncSendungs-Stand←Content | Trigger-Flow bei Content-Änderungen |
| SyncContentElementeLookup | Synchronisiert Lookup-Felder zwischen Listen |
| WenneinElementerstelltwird | Erstellt automatisch Projektordner für neue Einträge |

## Dokumentation

- [Architecture.md](Docs/Architecture.md) – Systemarchitektur & Datenflüsse
- - [CLAUDE_CODE_PROMPT.md](CLAUDE_CODE_PROMPT.md) – Regeln für Claude Code
 
  - ## Claude Code Regeln
 
  - Bei Arbeit mit diesem Repository:
 
  - 1. **InternalNames** aus den CSV-Schemas verwenden (nicht DisplayNames raten)
    2. 2. **Lookup-Felder** enden auf `Id` für die ID-Referenz
       3. 3. **Multi-Choice-Felder** als Arrays behandeln, mit `join()` verbinden
          4. 4. **HTML-Felder** normalisieren (div/br/&nbsp; entfernen)
             5. 5. **Flow-Updates**: JSON-Export aus Power Automate, dann in `Flows/` ablegen
