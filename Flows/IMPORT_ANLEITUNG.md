# Power Automate Flow Import Anleitung

## Voraussetzungen

Bevor du die Flows importierst, musst du **zuerst** die neue Spalte im Contentverzeichnis erstellen:

### Schritt 1: Neue Spalte erstellen

1. Gehe zur SharePoint-Liste **Contentverzeichnis**
2. Klicke auf **+ Spalte hinzufügen** → **Auswahl**
3. Konfiguriere:
   - **Name:** `Plattform-Freigaben`
   - **Interner Name:** `PlattformFreigaben` (wichtig!)
   - **Mehrfachauswahl erlauben:** ✅ Ja
   - **Auswahlmöglichkeiten:**
     ```
     ▶ YouTube
     💬 Facebook
     📷 Instagram
     ❗ Siehe Hinweisfeld
     TikTok 💃
     ```
4. Speichern

---

## Flow Import Optionen

### Option A: Manuell nachbauen (empfohlen)

Da Power Automate JSON-Importe manchmal Probleme mit Connections haben, ist es oft einfacher, die Flows manuell nachzubauen. Hier die Schritte:

#### Flow 1: "Sync Plattform-Freigaben (Content ← Sendung)"

**Trigger:**
- "Wenn ein Element erstellt oder geändert wird"
- Liste: Contentverzeichnis
- Polling: alle 5 Minuten

**Aktionen:**
1. **Variable initialisieren** - `varSendungID` = `triggerBody()?['Folge/Id']`
2. **Element abrufen** - Sendung mit ID = `varSendungID`
3. **Bedingung** - Wenn Sendung.field_18 (Plattform-Freigaben) nicht leer:
   - **Element aktualisieren** - Contentverzeichnis, setze `PlattformFreigaben` = Sendung.field_18

#### Flow 2: "Sync Plattform-Freigaben (Sendung → Content)"

**Trigger:**
- "Wenn ein Element erstellt oder geändert wird"
- Liste: Sendungen
- Polling: alle 5 Minuten

**Aktionen:**
1. **Variable initialisieren** - `varPlattformFreigaben` = `triggerBody()?['field_18']`
2. **Variable initialisieren** - `varSendungID` = `triggerBody()?['ID']`
3. **Elemente abrufen** - Contentverzeichnis, Filter: `Folge/Id eq varSendungID`
4. **Auf alle anwenden** (For each):
   - **Element aktualisieren** - setze `PlattformFreigaben` = `varPlattformFreigaben`

---

### Option B: JSON Import (fortgeschritten)

1. Gehe zu **make.powerautomate.com**
2. Klicke auf **Meine Flows** → **Importieren** → **Paket importieren (Legacy)**
3. Lade die JSON-Datei hoch
4. **Wichtig:** Nach dem Import musst du:
   - Die SharePoint-Connection neu verknüpfen
   - Die Site-URL prüfen: `https://wdr.sharepoint.com/teams/ARDStand-UpO365`
   - Die Listen-GUIDs prüfen:
     - Sendungen: `08c76ffb-e10b-4158-92b5-73a509a221cc`
     - Contentverzeichnis: `6e406f83-7400-4568-88d3-25469885a7da`

---

## Wie die Flows zusammenarbeiten

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   SENDUNG                        CONTENTVERZEICHNIS         │
│   ┌──────────────┐               ┌──────────────────┐       │
│   │ field_18:    │               │ PlattformFreigaben│      │
│   │ Plattform-   │ ──────────────│ (neu)             │      │
│   │ Freigaben    │   Flow 2:     │                   │      │
│   │              │   Sendung→    │                   │      │
│   │              │   Content     │                   │      │
│   │              │               │                   │      │
│   │              │ ◄─────────────│   Folge (Lookup)  │      │
│   │              │   Flow 1:     │                   │      │
│   │              │   Content←    │                   │      │
│   │              │   Sendung     │                   │      │
│   └──────────────┘               └──────────────────┘       │
│                                                             │
└─────────────────────────────────────────────────────────────┘

Flow 1: Wenn Content erstellt → holt Plattform-Freigaben von Sendung
Flow 2: Wenn Sendung geändert → pusht an alle verknüpften Contents
```

---

## Testen

1. **Test Flow 1:** Erstelle ein neues Content-Element mit Folge-Verknüpfung
   → Plattform-Freigaben sollte automatisch gefüllt werden

2. **Test Flow 2:** Ändere Plattform-Freigaben in einer Sendung
   → Alle verknüpften Content-Elemente sollten aktualisiert werden

---

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Spalte nicht gefunden | Prüfe ob interner Name `PlattformFreigaben` ist |
| Flow läuft nicht | Prüfe ob Flow aktiviert ist |
| Keine Änderungen | Prüfe die Filter-Bedingungen im Flow |
| Connection-Fehler | SharePoint-Verbindung neu autorisieren |
