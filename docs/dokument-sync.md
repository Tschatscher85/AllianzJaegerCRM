# Twenty Dokument-Sync Workflow

## Ziel

Wenn ein Dokument in Twenty CRM bei einer **Person** oder **Immobilie** hochgeladen wird, wird es automatisch in den richtigen Kundenordner auf dem Unraid NAS kopiert.

**Zielpfade:**
- Versicherungskunden: `Z:\Agentur Jaeger\Beratung\Versicherungen\{Nachname} {Vorname}\`
- Immobilien: `Z:\Agentur Jaeger\Beratung\Immobilienmakler\Verkauf\{Strasse} - {PLZ} {Ort}\`

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
Build Path    Build Path (Ordnername generieren)
  |               |
  v               v
      Merge (append)
          |
          v
Read Source File (/data/twenty-storage/...)
          |
          v
Write to NAS (/data/beratung/...)
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
| Build Person/Immobilie Path | code | Zielordner und Dateipfad generieren |
| Merge | merge | Beide Pfade zusammenfuehren |
| Read Source File | readWriteFile | Datei aus Twenty Storage lesen |
| Write to NAS | readWriteFile | Datei in Kundenordner schreiben |
| Update Type? | if | Person oder Immobilie Update? |
| Update Person/Immobilie | httpRequest | Dokumentenordner-Link in Twenty setzen |

### Event-Filter

Der Workflow reagiert nur auf `attachment.created` Events. Events wie `attachment.updated` oder `attachment.destroyed` werden im "Parse & Route" Node gefiltert und ignoriert.

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
  - "/mnt/user/Agentur Jaeger/Beratung:/data/beratung:rw"    # Kundenordner schreiben
environment:
  N8N_RESTRICT_FILE_ACCESS_TO: "/data/twenty-storage,/data/beratung,/files"
```

### Pfad-Mapping

| Container-Pfad | Host-Pfad (Unraid) | Windows SMB |
|---------------|--------------------|----|
| `/data/twenty-storage/` | `/mnt/user/Backups/Twenty/` | `Z:\Backups\Twenty\` |
| `/data/beratung/` | `/mnt/user/Agentur Jaeger/Beratung/` | `Z:\Agentur Jaeger\Beratung\` |

### Dateipfad-Aufbau

Twenty speichert Uploads unter:
```
/data/twenty-storage/workspace-{workspaceId}/attachment/{uuid}.pdf
```

Der Workflow kopiert nach:
```
/data/beratung/Versicherungen/{Nachname} {Vorname}/{originalDateiname}
/data/beratung/Immobilienmakler/Verkauf/{Strasse} - {PLZ} {Ort}/{originalDateiname}
```

## Twenty Webhook Konfiguration

| Einstellung | Wert |
|------------|------|
| Event | `attachment.*` (created, updated, destroyed) |
| Target URL | `https://make.tschatscher.eu/webhook/twenty-attachment` |
| Workspace ID | `57085d64-6998-41f1-8d36-f349b517e3ee` |

## Credentials

| Service | Credential | n8n ID |
|---------|-----------|--------|
| Twenty REST API | Header Auth (Bearer Token) | `pRZsKWNSoLeAlxo4` |

## Troubleshooting

### File Access Error
Wenn n8n "Access to the file is not allowed" meldet:
- Pruefen ob `N8N_RESTRICT_FILE_ACCESS_TO` in der docker-compose gesetzt ist
- Container komplett entfernen und neu erstellen (nicht nur restart)

### Datei nicht gefunden
- Pruefen ob Twenty Storage Volume korrekt gemountet ist:
  ```bash
  docker exec -it n8n ls -la /data/twenty-storage/workspace-57085d64.../attachment/
  ```
- Volume muss auf `/mnt/user/Backups/Twenty` zeigen (nicht alter Pfad)

### Webhook wird nicht getriggert
- Twenty Webhook pruefen: Settings > Developers > Webhooks
- Event muss `attachment.*` sein
- URL: `https://make.tschatscher.eu/webhook/twenty-attachment`
