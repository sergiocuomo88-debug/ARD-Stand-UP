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
│   └── *.json
└── Docs/
    └── Architecture.md
```

## Tech-Stack

| Komponente | Verwendung |
|------------|------------|
| SharePoint | Datenhaltung |
| Power Automate | Workflows |
| Planner | Task-Management |
| Teams | Adaptive Cards |

## Für Claude Code

1. **IMMER** `Schemas/` für InternalNames nutzen
2. 2. **NIEMALS** DisplayNames raten
   3. 3. HTML-Felder normalisieren
      4. 4. MultiChoice = Arrays → `join()` verwenden
        
         5. ---
         6. *Stand: Januar 2026*
