# ARD Stand-Up – Datenmodell

## Entity-Relationship-Diagramm

```mermaid
erDiagram
    SENDUNGEN ||--o{ CONTENTVERZEICHNIS : "enthält"
    SENDUNGEN ||--o| PLANNER_TASK : "synchronisiert zu"
    SENDUNGEN ||--o| TEAMS_MESSAGE : "benachrichtigt via"
    CONTENTVERZEICHNIS }o--|| SENDUNGEN : "gehört zu"

    SENDUNGEN {
        int ID PK "Counter, Auto-Increment"
        string Title "Sendungskürzel (LN24-01)"
        datetime field_2 "Rechte von / Ausstrahlung"
        datetime field_3 "Rechte bis"
        choice field_5 "Sendereihe"
        multichoice field_9 "Zusatztitel"
        choice field_20 "Stand (Status)"
        multichoice field_10 "Künstler"
        string field_13 "Ansprechpartner"
        multichoice field_18 "Plattformfreigaben"
        multichoice Container "Verpackungsformate"
        usermulti Producer "Zugewiesene Producer"
        note field_6 "Hinweis (HTML)"
        note ProducerBriefing "Briefing (HTML)"
        url Logo_x002d_Sendung "Logo für Planner"
        url LogoCardUrl "Logo für Teams Card"
        url Scoopa0 "Projektordner-Link"
        url VideoTitel "Mediathek-Link"
        string PlannerAufgabenID FK "→ Planner Task"
        string TeamsMessageId FK "→ Teams Message"
        boolean SendeanPlanner "Flow-Gate"
        boolean AlteSendung "Exclusion-Flag"
        lookupmulti ContentElementeLookup "→ Contentverzeichnis[]"
    }

    CONTENTVERZEICHNIS {
        int ID PK "Counter, Auto-Increment"
        string Title "Content-Titel"
        lookup Folge FK "→ Sendungen.ID"
        usermulti Producer "Zugewiesene Producer"
        choice Content "Format (Clip/Short/Reel)"
        choice Bearbeitungsstautus "Producing-Stand"
        multichoice Container "Fertige Verpackungen"
        choice YT_Status "YouTube-Status"
        choice FB_Status "Facebook-Status"
        choice Insta_Status "Instagram-Status"
        choice TikTok_Status "TikTok-Status"
    }

    PLANNER_TASK {
        string id PK "Planner Task-ID"
        string planId FK "→ Plan"
        string bucketId FK "→ Bucket"
        string title "Task-Titel"
        datetime dueDateTime "Fälligkeitsdatum"
        string description "Notes/Beschreibung"
        array references "Angehängte Links"
        string previewType "Cover-Typ"
    }

    PLANNER_BUCKET {
        string id PK "Bucket-ID"
        string planId FK "→ Plan"
        string name "Bucket-Name"
        int orderHint "Sortierung"
    }

    PLANNER_PLAN {
        string id PK "BvxWXWZmdkCDv5ijzP3Lg5YABxSR"
        string groupId FK "→ M365 Group"
        string title "ARD Stand-Up Sendungen"
    }

    TEAMS_MESSAGE {
        string id PK "Message-ID"
        string channelId FK "→ Teams Channel"
        json adaptiveCard "Card-Payload"
        datetime createdDateTime "Erstellt"
        datetime lastModifiedDateTime "Geändert"
    }

    TEAMS_CHANNEL {
        string id PK "19:...@thread.tacv2"
        string groupId FK "→ M365 Group"
        string displayName "Channel-Name"
    }

    M365_GROUP {
        string id PK "d248cec5-c1d3-4ff5-9063-464b51e94769"
        string displayName "ARD Stand-Up"
    }

    PLANNER_PLAN ||--|| M365_GROUP : "gehört zu"
    PLANNER_PLAN ||--|{ PLANNER_BUCKET : "enthält"
    PLANNER_BUCKET ||--o{ PLANNER_TASK : "enthält"
    TEAMS_CHANNEL ||--|| M365_GROUP : "gehört zu"
    TEAMS_CHANNEL ||--o{ TEAMS_MESSAGE : "enthält"
```

---

## Beziehungen im Detail

### Sendungen ↔ Contentverzeichnis

```mermaid
erDiagram
    SENDUNGEN ||--o{ CONTENTVERZEICHNIS : "1:n"

    SENDUNGEN {
        int ID PK
        lookupmulti ContentElementeLookup "Reverse-Lookup"
    }

    CONTENTVERZEICHNIS {
        int ID PK
        lookup Folge FK "→ Sendungen.ID"
    }
```

**Kardinalität**: 1:n (Eine Sendung hat 0..n Content-Elemente)

**Beziehungsrichtung**:
- **Primär**: `Contentverzeichnis.Folge` → `Sendungen.ID` (Lookup)
- **Reverse**: `Sendungen.ContentElementeLookup` ← `Contentverzeichnis` (LookupMulti)

**Verwendung in Flows**:
- Flow "Sync Sendungs-Stand" aggregiert Status aller Content-Elemente einer Sendung
- Status-Hierarchie: Content → Sendung → Planner

---

### Sendungen ↔ Planner Task

```mermaid
erDiagram
    SENDUNGEN ||--o| PLANNER_TASK : "1:0..1"

    SENDUNGEN {
        int ID PK
        string PlannerAufgabenID FK
    }

    PLANNER_TASK {
        string id PK
        string bucketId FK
    }

    PLANNER_BUCKET {
        string id PK
        string name "= field_20 Value"
    }

    PLANNER_TASK }o--|| PLANNER_BUCKET : "n:1"
```

**Kardinalität**: 1:0..1 (Eine Sendung hat max. einen Planner-Task)

**Synchronisation**:
| SharePoint-Feld | Planner-Attribut |
|-----------------|------------------|
| `ID` + `field_5` + `field_9` + `field_2` | `title` |
| `field_2` | `dueDateTime` |
| `field_20` | `bucketId` (via Name-Matching) |
| `ProducerBriefing` + `field_6` + ... | `description` |
| `Scoopa0`, `VideoTitel`, `{Link}` | `references[]` |
| `Logo_x002d_Sendung` | Cover (via references) |

**ID-Rückschreibung**: `Planner.id` → `Sendungen.PlannerAufgabenID`

---

### Sendungen ↔ Teams Message

```mermaid
erDiagram
    SENDUNGEN ||--o| TEAMS_MESSAGE : "1:0..1"

    SENDUNGEN {
        int ID PK
        string TeamsMessageId FK
        boolean SendeanPlanner "Gate"
        boolean AlteSendung "Exclusion"
    }

    TEAMS_MESSAGE {
        string id PK
        json adaptiveCard
    }
```

**Kardinalität**: 1:0..1 (Eine Sendung hat max. eine Teams-Card)

**Bedingungen für Card-Erstellung**:
- `SendeanPlanner = true`
- `AlteSendung != true`

**ID-Rückschreibung**: `Teams.messageId` → `Sendungen.TeamsMessageId`

---

## Status-Aggregation

```mermaid
flowchart TB
    subgraph Contentverzeichnis
        C1[Element 1<br/>Bearbeitungsstautus]
        C2[Element 2<br/>Bearbeitungsstautus]
        C3[Element n<br/>Bearbeitungsstautus]
    end

    subgraph Sendungen
        S[field_20<br/>Stand]
    end

    subgraph Planner
        B1[Bucket: Nicht begonnen]
        B2[Bucket: In Bearbeitung]
        B3[Bucket: Abnahmebereit]
        B4[Bucket: Fertig]
    end

    C1 & C2 & C3 -->|Aggregation| S
    S -->|Bucket-Mapping| B1 & B2 & B3 & B4

    style S fill:#f9f,stroke:#333
```

### Aggregationslogik

```mermaid
flowchart TD
    A[Alle Content-Elemente<br/>einer Sendung laden] --> B{Mind. 1 Element<br/>Entwurf/In Bearbeitung?}
    B -->|Ja| C[Sendung = In Bearbeitung]
    B -->|Nein| D{Mind. 1 Element<br/>Abnahmebereit?}
    D -->|Ja| E[Sendung = Abnahmebereit]
    D -->|Nein| F{Alle Elemente<br/>Fertig?}
    F -->|Ja| G[Sendung = Fertig]
    F -->|Nein| H[Sendung = Nicht begonnen]
```

---

## Datenfluss-Diagramm

```mermaid
flowchart LR
    subgraph SharePoint
        SL[Sendungen-Liste]
        CL[Contentverzeichnis-Liste]
    end

    subgraph Power_Automate
        F1[Flow 1:<br/>Sendungen → Planner]
        F2[Flow 2:<br/>Sendungen → Teams]
        F3[Flow 3:<br/>Sync Sendungs-Stand]
    end

    subgraph Planner
        PT[Tasks]
        PB[Buckets]
    end

    subgraph Teams
        TC[Channel]
        AC[Adaptive Cards]
    end

    CL -->|Trigger| F3
    F3 -->|Update field_20| SL

    SL -->|Trigger 5min| F1
    F1 -->|Create/Update Task| PT
    F1 -->|Move to Bucket| PB
    F1 -->|Write PlannerAufgabenID| SL

    SL -->|Trigger 1min| F2
    F2 -->|Post/Update Card| AC
    F2 -->|Write TeamsMessageId| SL
    AC --> TC

    style SL fill:#0078d4,color:#fff
    style CL fill:#0078d4,color:#fff
    style PT fill:#31752f,color:#fff
    style AC fill:#6264a7,color:#fff
```

---

## Physisches Modell

### SharePoint Site

```
Site: https://wdr.sharepoint.com/teams/ARDStand-UpO365
│
├── Liste: Sendungen
│   GUID: 08c76ffb-e10b-4158-92b5-73a509a221cc
│   Primärschlüssel: ID (Counter)
│
└── Liste: Contentverzeichnis
    GUID: [unbekannt]
    Primärschlüssel: ID (Counter)
    Fremdschlüssel: Folge → Sendungen.ID
```

### Microsoft 365 Group

```
Group: ARD Stand-Up
GUID: d248cec5-c1d3-4ff5-9063-464b51e94769
│
├── Planner Plan: ARD Stand-Up Sendungen
│   ID: BvxWXWZmdkCDv5ijzP3Lg5YABxSR
│   │
│   └── Buckets:
│       ├── Nicht begonnen
│       ├── In Bearbeitung
│       ├── Abnahmebereit
│       └── Fertig
│
└── Teams Channel: [Producer-Benachrichtigungen]
    ID: 19:3f67cd94e241472aab55708084e3bce8@thread.tacv2
```

---

## Referenzielle Integrität

### Automatisch gewährleistet

| Beziehung | Mechanismus |
|-----------|-------------|
| Contentverzeichnis → Sendungen | SharePoint Lookup (verhindert ungültige FK) |

### Manuell/Flow-gesteuert

| Beziehung | Risiko | Mitigation |
|-----------|--------|------------|
| Sendungen → Planner Task | Task kann in Planner gelöscht werden | Flow prüft Task-Existenz vor Update |
| Sendungen → Teams Message | Message kann gelöscht werden | Flow postet neue Card wenn ID ungültig |

### Orphan-Szenarien

| Szenario | Auswirkung | Empfehlung |
|----------|------------|------------|
| Sendung gelöscht, Content-Elemente bleiben | Verwaiste Elemente | Cascade-Delete via Flow |
| Planner-Task manuell gelöscht | `PlannerAufgabenID` zeigt ins Leere | Flow erstellt neuen Task |
| Teams-Card manuell gelöscht | `TeamsMessageId` zeigt ins Leere | Flow postet neue Card |

---

## Index-Empfehlungen

### Sendungen-Liste

| Feld | Index-Typ | Begründung |
|------|-----------|------------|
| `field_20` | Standard | Häufige Filterung nach Status |
| `SendeanPlanner` | Standard | Trigger-Condition |
| `PlannerAufgabenID` | Standard | Lookup für Updates |
| `TeamsMessageId` | Standard | Lookup für Updates |

### Contentverzeichnis-Liste

| Feld | Index-Typ | Begründung |
|------|-----------|------------|
| `Folge` | Standard | Join mit Sendungen |
| `Bearbeitungsstautus` | Standard | Status-Aggregation |

---

## Datenvolumen-Schätzung

| Entität | Geschätztes Volumen | Wachstum |
|---------|---------------------|----------|
| Sendungen | ~500 aktiv, ~2000 archiviert | +200/Jahr |
| Contentverzeichnis | ~5 pro Sendung = ~2500 aktiv | +1000/Jahr |
| Planner Tasks | = Sendungen (1:1) | +200/Jahr |
| Teams Messages | = aktive Sendungen | +200/Jahr |

**Limitierungen**:
- SharePoint List: 30 Mio. Items (kein Problem)
- Planner Plan: 2400 Tasks (ggf. Archivierung nötig)
- SharePoint View Threshold: 5000 Items (Indizes wichtig)

---

*Dokumentation erstellt: 2026-01-22*
