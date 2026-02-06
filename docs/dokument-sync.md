# Twenty Dokument-Sync Workflow

## Ziel

Wenn ein Dokument in Twenty CRM bei einer **Person** oder **Immobilie** hochgeladen wird, wird es automatisch in den richtigen Kundenordner auf dem Unraid NAS kopiert (via WebDAV).

**Routing nach Bereich:**

| Bereich | Zielpfad (NAS) |
|---------|---------------|
| VERSICHERUNG | `Z:\Agentur Jaeger\Beratung\Versicherungen\{Nachname} {Vorname}\` |
| IMMOBILIEN (Verkauf) | `Z:\Agentur Jaeger\Beratung\Immobilienmakler\Verkauf\{Nachname} {Vorname}\` |
| IMMOBILIEN (Vermietung) | `Z:\Agentur Jaeger\Beratung\Immobilienmakler\Vermietung\{Nachname} {Vorname}\` |
| UNLOG | `Z:\UnLog\Auftraege\{Jahr}\{Nachname} {Vorname}\` |
| HAUSVERWALTUNG | `Z:\Agentur Jaeger\Beratung\Hausverwaltung\{Nachname} {Vorname}\` |
| Immobilie (Objekt) | `Z:\Agentur Jaeger\Beratung\Immobilienmakler\Verkauf\{Strasse} - {PLZ} {Ort}\` |

**Wichtig:** Wenn kein Bereich gesetzt ist, wird das Dokument NICHT hochgeladen (Workflow stoppt).

## Architektur

```
Twenty CRM: Datei bei Person hochladen
  |
  v
Twenty Webhook: attachment.created
  |
  v
n8n Webhook: POST /webhook/twenty-attachment
  |
  v
Parse & Route: Event filtern (nur .created)
  |
  v
Person oder Immobilie? (IF Node)
  |               |
  v               v
Get Person    Get Immobilie (Twenty REST API)
  |               |
  v               v
Build Path    Build Path (Bereich-Routing + Ordnername)
  |               |
  v               v
      Merge (append)
          |
          v
Create Parent Folder (WebDAV MKCOL)
          |
          v
Create Folder (WebDAV MKCOL)
          |
          v
Read Source File (/data/twenty-storage/...)
          |
          v
Prepare Write (JSON + Binary merge)
          |
          v
Upload to NAS (WebDAV PUT)
          |
          v
Update Type? (IF Node)
  |               |
  v               v
Update Person  Update Immobilie (Dokumentenordner-Link setzen)
```

## n8n Workflow

**ID:** `hLmDJBA0uOn2xB9w`
**Name:** Twenty Dokument-Sync
**Webhook:** `https://make.tschatscher.eu/webhook/twenty-attachment`

### Nodes

| Node | Typ | Funktion |
|------|-----|----------|
| Webhook | webhook | Empfaengt Twenty attachment Events |
| Parse & Route | code | Filtert nur `.created` Events, extrahiert Pfade |
| Person oder Immobilie? | if | Pruefen ob personId oder immobilieId gesetzt |
| Get Person / Get Immobilie | httpRequest | Person/Immobilie Daten von Twenty REST API holen |
| Build Person/Immobilie Path | code | Bereich-Routing, Zielordner und Dateipfad generieren |
| Merge | merge | Beide Pfade zusammenfuehren |
| Create Parent Folder | httpRequest (MKCOL) | Ueberordner per WebDAV erstellen |
| Create Folder (WebDAV) | httpRequest (MKCOL) | Kundenordner per WebDAV erstellen |
| Read Source File | readWriteFile | Datei aus Twenty Storage lesen |
| Prepare Write | code | JSON-Daten und Binary zusammenfuehren |
| Upload to NAS (WebDAV) | httpRequest (PUT) | Datei per WebDAV auf NAS schreiben |
| Update Type? | if | Person oder Immobilie Update? |
| Update Person/Immobilie | httpRequest (PATCH) | Dokumentenordner-Link in Twenty setzen |

### Bereich-Routing (Build Person Path)

```javascript
// Bereich pruefen - STOPP wenn leer
const bereich = person.bereich || [];
if (!bereich.length) {
  return []; // Kein Bereich gesetzt = kein Upload
}

// Routing nach Bereich
if (bereich.includes('IMMOBILIEN')) {
  // Immobilienkategorie: VERKAUF oder VERMIETUNG
  const kategorie = person.immobilienkategorie || 'VERKAUF';
  const subFolder = kategorie === 'VERMIETUNG' ? 'Vermietung' : 'Verkauf';
  // -> /Beratung/Immobilienmakler/{Verkauf|Vermietung}/{Name}/
} else if (bereich.includes('UNLOG')) {
  // -> /UnLog/Auftraege/{Jahr}/{Name}/
} else if (bereich.includes('HAUSVERWALTUNG')) {
  // -> /Beratung/Hausverwaltung/{Name}/
} else {
  // Default: VERSICHERUNG
  // -> /Beratung/Versicherungen/{Name}/
}
```

### Event-Filter

Der Workflow reagiert nur auf `attachment.created` Events. Events wie `attachment.updated` oder `attachment.destroyed` werden im "Parse & Route" Node gefiltert und ignoriert. Geloeschte Dokumente in Twenty bleiben als Archiv auf dem NAS erhalten.

### WebDAV Upload

Der Upload erfolgt ueber WebDAV (HTTPS) statt direktem Dateisystem-Zugriff:

1. **MKCOL** erstellt den Ueberordner (z.B. `/Versicherungen/`) - `continueOnFail` fuer existierende Ordner
2. **MKCOL** erstellt den Kundenordner (z.B. `/Versicherungen/Person Test/`) - `continueOnFail`
3. **PUT** schreibt die Datei mit `Overwrite: T` Header (gleiche Dateinamen werden ueberschrieben)

### Dokumentenordner-Link

Nach erfolgreichem Upload wird in Twenty das `dokumentenordner` LINK-Feld gesetzt:
- **Label:** Windows-Netzwerkpfad (z.B. `\\Unraid\Agentur Jaeger\Beratung\Versicherungen\Person Test`)
- **URL:** WebDAV-URL zum Ordner (klickbar im Browser mit Login)

## WebDAV Konfiguration

### Credentials

| Service | Credential | n8n Typ | n8n ID |
|---------|-----------|---------|--------|
| WebDAV (NAS) | Basic Auth (tschatscher) | httpBasicAuth | `fMqVRFlX1klnaNuX` |
| Twenty REST API | Bearer Token | httpHeaderAuth | `pRZsKWNSoLeAlxo4` |

### WebDAV Pfad-Mapping

| WebDAV URL | Windows SMB | NAS Pfad |
|-----------|-------------|----------|
| `https://webdav.tschatscher.eu/Agentur%20Jaeger/Beratung/` | `\\Unraid\Agentur Jaeger\Beratung\` | `/mnt/user/Agentur Jaeger/Beratung/` |
| `https://webdav.tschatscher.eu/UnLog/` | `\\Unraid\UnLog\` | `/mnt/user/UnLog/` |

## Docker Volume Mounts

### Twenty Container
```yaml
volumes:
  - /mnt/user/Backups/Twenty:/app/packages/twenty-server/.local-storage:rw
```

### n8n Container
```yaml
volumes:
  - /mnt/user/Backups/Twenty:/data/twenty-storage:ro          # Twenty Uploads lesen
  - "/mnt/user/Agentur Jaeger/Beratung:/data/beratung:rw"    # (Legacy, nicht mehr fuer Write genutzt)
environment:
  N8N_RESTRICT_FILE_ACCESS_TO: "/data/twenty-storage,/data/beratung,/files"
```

**Hinweis:** Das Schreiben auf das NAS erfolgt ueber WebDAV, nicht ueber Docker Volume Mounts. Das Volume `/data/beratung` wird nicht mehr zum Schreiben genutzt.

### Dateipfad-Aufbau

Twenty speichert Uploads unter:
```
/data/twenty-storage/workspace-{workspaceId}/attachment/{uuid}.pdf
```

Der Workflow kopiert per WebDAV nach:
```
https://webdav.tschatscher.eu/Agentur%20Jaeger/Beratung/Versicherungen/{Nachname} {Vorname}/{originalDateiname}
https://webdav.tschatscher.eu/Agentur%20Jaeger/Beratung/Immobilienmakler/{Verkauf|Vermietung}/{Name}/{originalDateiname}
https://webdav.tschatscher.eu/UnLog/Auftr%C3%A4ge/{Jahr}/{Name}/{originalDateiname}
https://webdav.tschatscher.eu/Agentur%20Jaeger/Beratung/Hausverwaltung/{Name}/{originalDateiname}
```

## Twenty Webhook Konfiguration

| Einstellung | Wert |
|------------|------|
| Event | `attachment.*` (created, updated, destroyed) |
| Target URL | `https://make.tschatscher.eu/webhook/twenty-attachment` |
| Workspace ID | `57085d64-6998-41f1-8d36-f349b517e3ee` |

## Troubleshooting

### 405 Method Not Allowed bei MKCOL
Normal wenn der Ordner bereits existiert. Die MKCOL-Nodes haben `continueOnFail: true` gesetzt.

### 403 Forbidden bei PUT
- Pruefen ob der Zielordner existiert (MKCOL muss vorher laufen)
- WebDAV Credentials pruefen (httpBasicAuth `fMqVRFlX1klnaNuX`)
- Ordner die ueber SMB/Windows erstellt wurden koennen andere Berechtigungen haben

### Kein Upload trotz Dokument-Upload
- Pruefen ob die Person einen **Bereich** zugewiesen hat (Pflichtfeld fuer Upload)
- Pruefen ob der Twenty Webhook aktiv ist: Settings > Developers > Webhooks
- Event muss `attachment.*` sein

### Datei nicht gefunden
- Pruefen ob Twenty Storage Volume korrekt gemountet ist:
  ```bash
  docker exec -it n8n ls -la /data/twenty-storage/workspace-57085d64.../attachment/
  ```
