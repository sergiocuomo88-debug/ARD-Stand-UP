# ARD Stand-Up – System Architecture

## Datenfluss

```
LRA-Content → Scoopa → SharePoint "Sendungen"
                              ↓
                    Power Automate Flows
                              ↓
              ┌───────────────┼───────────────┐
              ↓               ↓               ↓
         Planner          Teams Card    "Contentverzeichnis"
                                              ↑
                                    Plattform-Freigaben sync
```

## SharePoint Listen

| Liste | GUID | Zweck |
|-------|------|-------|
| ARD Stand-Up Sendungen | `08c76ffb-e10b-4158-92b5-73a509a221cc` | Master-Daten |
| Contentverzeichnis | `6e406f83-7400-4568-88d3-25469885a7da` | Content-Elemente |

## Power Automate Flows

### 1. Sendungen → Planner (Phase 1)
- Trigger: Sendungen erstellt/geändert
- Aktion: Planner-Task erstellen/aktualisieren
- Bucket-Mapping nach Status

### 2. Sync Sendungs-Stand
- Trigger: Contentverzeichnis geändert
- Aktion: Aggregiert Status (Entwurf/Abnahmebereit/Fertig)
- Schreibt zurück auf Sendungen.field_20

### 3. Sync ContentElementeLookup
- Trigger: Contentverzeichnis geändert
- Aktion: Aktualisiert Lookup-Array in Sendungen

### 4. Sendungen → Teams Adaptive Card
- Trigger: SendeanPlanner = true
- Aktion: Postet/aktualisiert Adaptive Card
- Speichert TeamsMessageId zurück

### 5. Sync Plattform-Freigaben (Sendung → Content) ✨ NEU
- Trigger: Sendungen geändert (wenn field_18 Plattform-Freigaben vorhanden)
- Aktion: Überträgt Plattform-Freigaben auf alle verknüpften Content-Elemente
- Ermöglicht Filterung im Contentverzeichnis nach Plattform (z.B. nur YouTube)

## Bekannte Limitationen

- Power Automate: Max 60+ Actions → Komplexitätsgrenze erreicht
- Delay-Workarounds für API-Timing
- Kein ARD-weiter Standard für Content-Metadaten

---
*Stand: Februar 2026*
