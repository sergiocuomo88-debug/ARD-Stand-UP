# Sendungen – SharePoint Fields

**Liste:** ARD Stand-Up Sendungen  
**GUID:** `08c76ffb-e10b-4158-92b5-73a509a221cc`

## Wichtige Felder

| InternalName | DisplayName | Type | Hinweis |
|--------------|-------------|------|---------|
| `eigeneID` | Nr.ID | Number | |
| `Title` | Sendungskürzel | Text | |
| `field_2` | Rechte von | DateTime | |
| `field_3` | Rechte bis | DateTime | |
| `field_4` | LRA | Choice | |
| `field_5` | Sendereihe | Choice | |
| `field_6` | Hinweis | Note | HTML → normalisieren |
| `field_9` | Kategorie/Zusatztitel | MultiChoice | Array → join() |
| `field_10` | Künstler | MultiChoice | Array → join() |
| `field_13` | Ansprechpartner | Text | |
| `field_18` | Plattform-Freigaben | MultiChoice | Array → join() |
| `field_20` | Stand | Choice | Status-Wert |
| `Producer` | Producer | UserMulti | Array[0]?['DisplayName'] |
| `Container` | Container | MultiChoice | Array → join() |
| `Scoopa0` | Scoopa | Text | URL Projektordner |
| `VideoTitel` | Mediathek/VoD | Text | URL |
| `PlannerAufgabenID` | PlannerAufgabenID | Text | |
| `Logo_x002d_Sendung` | Logo-Sendung | Text | URL |
| `ProducerBriefing` | Producer Briefing | Note | HTML → normalisieren |
| `SendeanPlanner` | Sende an Planner | Boolean | |
| `ContentElementeLookup` | Content-Elemente | LookupMulti | |

---
*Stand: Januar 2026*
