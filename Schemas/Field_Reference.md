# ARD Stand-Up – Field Reference

Vollständiges Feld-Mapping für alle SharePoint-Listen im ARD Stand-Up System.

> **Legende Quelle**:
> - ✅ = Bestätigt aus Flow-JSON
> - 📄 = Aus CLAUDE_CODE_PROMPT.md
> - ⚠️ = Vermutet/Abgeleitet

---

## Liste 1: Sendungen

**Site**: `https://wdr.sharepoint.com/teams/ARDStand-UpO365`
**Listen-GUID**: `08c76ffb-e10b-4158-92b5-73a509a221cc`

### Systemfelder

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `ID` | ID | Counter | Primärschlüssel, auto-increment | ✅ |
| `Title` | Sendungskürzel | Text | Eindeutiges Kürzel, z.B. "LN24-01" | ✅ |
| `{Link}` | Link | Calculated | SharePoint Item-URL | ✅ |
| `Created` | Erstellt | DateTime | Erstellungszeitpunkt | 📄 |
| `Modified` | Geändert | DateTime | Letzte Änderung | 📄 |
| `Author` | Erstellt von | User | Ersteller | 📄 |
| `Editor` | Geändert von | User | Letzter Bearbeiter | 📄 |

### Sendungs-Stammdaten

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `field_5` | Sendereihe | Choice | Format/Reihe der Sendung | ✅ |
| `field_9` | Zusatztitel | MultiChoice | Ergänzende Titel-Infos | ✅ |
| `field_2` | Rechte von | DateTime | Ausstrahlungsdatum, Rechtebeginn | ✅ |
| `field_3` | Rechte bis | DateTime | Rechteende | 📄 |

**Choice-Werte für `field_5` (Sendereihe)**:
```
Ladies Night
Nightwash
Nuhr im Ersten
Satire Gipfel
Comedy & Satire im Ersten
[weitere LRA-spezifische Formate]
```

**Choice-Werte für `field_9` (Zusatztitel)**:
```
Best of
Spezial
Jahresrückblick
[projektspezifisch]
```

### Personen & Kontakte

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `Producer` | Producer | UserMulti | Zugewiesene Producer | ✅ |
| `field_10` | Künstler | MultiChoice | Comedians in der Sendung | ✅ |
| `field_13` | Ansprechpartner | Text | Kontaktperson (Freitext) | ✅ |

**Choice-Werte für `field_10` (Künstler)**:
```
[Dynamisch - Comedian-Namen]
```

### Status & Workflow

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `field_20` | Stand | Choice | Bearbeitungsstatus | ✅ |
| `SendeanPlanner` | Sende an Planner | Boolean | Aktiviert Teams-Card-Flow | ✅ |
| `AlteSendung` | Alte Sendung | Boolean | Excludiert von Teams-Card | ✅ |

**Choice-Werte für `field_20` (Stand)**:
| Wert | Beschreibung | Planner-Bucket | Teams-Badge |
|------|--------------|----------------|-------------|
| `Nicht begonnen` | Neu angelegt, keine Bearbeitung | Nicht begonnen | ⚪ |
| `In Bearbeitung` | Wird aktiv produziert | In Bearbeitung | 🔵 |
| `Abnahmebereit` | Zur Freigabe/Review | Abnahmebereit | 🟡 |
| `Fertig` | Produktion abgeschlossen | Fertig | 🟢 |
| `Archiviert` | Abgelegt, nicht mehr aktiv | - | 🟤 |

### Plattformen & Container

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `field_18` | Plattformfreigaben | MultiChoice | Ziel-Plattformen | ✅ |
| `Container` | Container | MultiChoice | Verpackungsformate | ✅ |

**Choice-Werte für `field_18` (Plattformfreigaben)**:
```
TikTok
Instagram
Facebook
YouTube
ARD Mediathek
```

**Choice-Werte für `Container`**:
```
Short
Reel
Clip
Story
[weitere Formate]
```

### Briefing & Hinweise

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `ProducerBriefing` | Producer Briefing | Note (HTML) | Arbeitsanweisungen für Producer | ✅ |
| `field_6` | Hinweis | Note (HTML) | Wichtige Infos, Warnungen | ✅ |

**HTML-Bereinigung erforderlich** – siehe [Architecture.md](../Docs/Architecture.md#html-bereinigung)

### URLs & Links

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `Scoopa0` | Scoopa | URL/Text | Link zum Projektordner | ✅ |
| `Scoopa` | Scoopa (alt) | URL/Text | Fallback für Projektordner | ✅ |
| `VideoTitel` | Video Titel | URL/Text | Mediathek-Link | ✅ |
| `Logo_x002d_Sendung` | Logo-Sendung | URL | Logo für Planner-Task | ✅ |
| `LogoCardUrl` | Logo Card URL | URL | Logo für Teams Adaptive Card | ✅ |

> **Hinweis**: Zwei Logo-Felder existieren für unterschiedliche Anwendungsfälle:
> - `Logo_x002d_Sendung`: Wird als Planner-Task-Cover verwendet
> - `LogoCardUrl`: Wird in der Teams Adaptive Card angezeigt

### Externe System-Referenzen

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `PlannerAufgabenID` | Planner Aufgaben ID | Text | Referenz zum Planner-Task | ✅ |
| `TeamsMessageId` | Teams Message ID | Text | Referenz zur Teams-Nachricht | ✅ |

**Wichtig**: Diese Felder werden automatisch von den Flows befüllt und sollten nicht manuell bearbeitet werden.

### Lookup-Felder

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `ContentElementeLookup` | Content-Elemente | LookupMulti | Verknüpfte Content-Elemente | 📄 |

**Lookup-Ziel**: Liste "Contentverzeichnis"

---

## Liste 2: Contentverzeichnis

**Site**: `https://wdr.sharepoint.com/teams/ARDStand-UpO365`
**Listen-GUID**: ⚠️ Nicht aus Flows bekannt

### Systemfelder

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `ID` | ID | Counter | Primärschlüssel | 📄 |
| `Title` | Titel | Text | Titel des Content-Elements | 📄 |
| `Created` | Erstellt | DateTime | Erstellungszeitpunkt | 📄 |
| `Modified` | Geändert | DateTime | Letzte Änderung | 📄 |

### Verknüpfungen

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `Folge` | Folge | Lookup | Verknüpfung zur Sendung | 📄 |
| `Folge/Id` | Folge ID | LookupValue | ID der verknüpften Sendung | 📄 |

**Lookup-Quelle**: Liste "Sendungen"

**Dependent Lookup-Felder** (abgeleitet von Folge):
| InternalName | Beschreibung | Quelle |
|--------------|--------------|--------|
| `Folge_x003a__x0020_Rechte_x0020_` | Rechte von der Sendung | 📄 |

### Personen

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `Producer` | Producer | UserMulti | Zugewiesene Producer | 📄 |

### Content-Details

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| `Content` | Content | Choice | Format des Content-Elements | 📄 |
| `Bearbeitungsstautus` | Bearbeitungsstatus | Choice | Producing-Stand | 📄 |
| `Container` | Container | MultiChoice | Fertige Verpackungen | 📄 |

**Choice-Werte für `Content` (Format)**:
```
Clip
Short
Reel
Story
Highlight
Behind the Scenes
[weitere]
```

**Choice-Werte für `Bearbeitungsstautus`**:
| Wert | Beschreibung |
|------|--------------|
| `Entwurf` | Noch nicht begonnen |
| `In Bearbeitung` | Wird produziert |
| `Abnahmebereit` | Zur Freigabe |
| `Fertig` | Abgeschlossen |

### Plattform-Status

| InternalName | DisplayName | Typ | Beschreibung | Quelle |
|--------------|-------------|-----|--------------|--------|
| ⚠️ `YT_Status` | YT-Status | Choice | YouTube-Veröffentlichungsstatus | 📄 |
| ⚠️ `FB_Status` | FB-Status | Choice | Facebook-Veröffentlichungsstatus | 📄 |
| ⚠️ `Insta_Status` | Insta-Status | Choice | Instagram-Veröffentlichungsstatus | 📄 |
| ⚠️ `TikTok_Status` | TikTok-Status | Choice | TikTok-Veröffentlichungsstatus | 📄 |

> **Hinweis**: Die exakten InternalNames der Plattform-Status-Felder sind nicht aus den Flows bekannt. Die obigen Namen sind Vermutungen.

---

## Feld-Namenskonventionen

### field_X Pattern

SharePoint generiert für manche Felder automatische InternalNames im Format `field_X`. Diese entstehen typischerweise wenn:
- Felder mit Sonderzeichen im DisplayName erstellt werden
- Felder via UI mit bestimmten Zeichen erstellt werden
- Legacy-Migration stattfand

**Mapping der bekannten field_X-Felder**:
| InternalName | Vermuteter DisplayName |
|--------------|------------------------|
| `field_2` | Rechte von |
| `field_3` | Rechte bis |
| `field_5` | Sendereihe |
| `field_6` | Hinweis |
| `field_9` | Zusatztitel |
| `field_10` | Künstler |
| `field_13` | Ansprechpartner |
| `field_18` | Plattformfreigaben |
| `field_20` | Stand |

### URL-Encoding in InternalNames

Sonderzeichen werden URL-encoded:
- `-` → `_x002d_`
- `:` → `_x003a_`
- Leerzeichen → `_x0020_`

**Beispiele**:
- `Logo_x002d_Sendung` = "Logo-Sendung"
- `Folge_x003a__x0020_Rechte_x0020_` = "Folge: Rechte "

---

## Flow-Verwendung

### Welcher Flow nutzt welches Feld?

| Feld | Flow 1 (Planner) | Flow 2 (Teams) |
|------|------------------|----------------|
| `ID` | ✅ Task-Titel | ✅ Card-Titel |
| `Title` | ✅ Notes | ✅ Briefing |
| `field_2` | ✅ DueDate | ✅ Datum-Anzeige |
| `field_5` | ✅ Task-Titel | ✅ Card-Titel |
| `field_6` | ✅ Notes | ✅ Hinweis-Section |
| `field_9` | ✅ Task-Titel | ✅ Card-Titel |
| `field_10` | ✅ Notes | ✅ Künstler-Spalte |
| `field_13` | ✅ Notes | ✅ Ansprechpartner |
| `field_18` | ✅ Notes | ✅ Plattformen-Spalte |
| `field_20` | ✅ Bucket-Mapping | ✅ Status-Badge |
| `Container` | ✅ Notes | ✅ Briefing |
| `Producer` | - | ✅ Producer-Anzeige |
| `ProducerBriefing` | ✅ Notes | ✅ Briefing-Section |
| `Logo_x002d_Sendung` | ✅ Task-Cover | - |
| `LogoCardUrl` | - | ✅ Card-Image |
| `Scoopa0` | ✅ Reference | ✅ Button |
| `VideoTitel` | ✅ Reference | ✅ Button |
| `PlannerAufgabenID` | ✅ R/W | ✅ Button-URL |
| `TeamsMessageId` | - | ✅ R/W |
| `SendeanPlanner` | - | ✅ Trigger-Condition |
| `AlteSendung` | - | ✅ Trigger-Condition |
| `{Link}` | ✅ Reference | ✅ Datensatz-Link |

---

## Datentyp-Referenz

### SharePoint-Typen → Power Automate

| SharePoint-Typ | Power Automate | Zugriff |
|----------------|----------------|---------|
| Text | String | `triggerBody()?['FieldName']` |
| Note | String (HTML) | `triggerBody()?['FieldName']` |
| Choice | Object | `triggerBody()?['FieldName/Value']` |
| MultiChoice | Array | `triggerBody()?['FieldName']` → `item()?['Value']` |
| DateTime | String (ISO) | `triggerBody()?['FieldName']` |
| User | Object | `triggerBody()?['FieldName/Email']` |
| UserMulti | Array | `triggerBody()?['FieldName']` → `item()?['DisplayName']` |
| Lookup | Object | `triggerBody()?['FieldName/Id']` |
| LookupMulti | Array | `triggerBody()?['FieldName']` → `item()?['Id']` |
| Boolean | Boolean | `triggerBody()?['FieldName']` |
| Counter | Integer | `triggerBody()?['ID']` |
| URL | String | `triggerBody()?['FieldName']` |

### Null-Handling

```javascript
// Choice (Single)
coalesce(triggerBody()?['field_20/Value'], triggerBody()?['field_20'], 'Default')

// MultiChoice
if(equals(triggerBody()?['field_X'], null), createArray(), array(triggerBody()?['field_X']))

// Text/Note
coalesce(triggerBody()?['FieldName'], '')

// URL (mit Fallback)
coalesce(triggerBody()?['Scoopa0'], triggerBody()?['Scoopa'], '')
```

---

## Offene Fragen

Folgende Informationen fehlen noch für eine vollständige Dokumentation:

1. **Contentverzeichnis Listen-GUID** – Benötigt für Flow-Referenzen
2. **Exakte InternalNames der Plattform-Status-Felder** in Contentverzeichnis
3. **Vollständige Choice-Werte** für alle Felder
4. **Weitere Felder** die in den nicht-analysierten Flows verwendet werden:
   - Sync Sendungs-Stand
   - Sync Sendungs-Stand ← Content
   - Sync ContentElementeLookup

---

*Dokumentation erstellt: 2026-01-22*
*Basierend auf Flow-Analyse und CLAUDE_CODE_PROMPT.md*
