# Twenty CRM - Custom Objects (Pipelines)

## Übersicht

| Object | API Name | Kanban Stages |
|--------|----------|---------------|
| UnLOG Aufträge | `unlogAuftrag` | Auftrag → Termin → Erledigt |
| Versicherungen | `versicherung` | Maklermandat → Angebote → Anträge → Verträge |
| Immobilien | `immobilie` | Neue Anfrage → Erreicht → Termin → Objekt vorb. → kein Interesse |
| Büro-Workflows | `bueroWorkflow` | Aufgaben Büro → Schaden → offene Rechnung → Anträge → Mahnung |
| Arbeitsanweisungen | `arbeitsanweisung` | Agentur → KFZ → Privat Sach → Krankenvers. |

## 1. UnLOG Aufträge

**Object ID:** `a8c288b5-5933-4284-9d84-9bb6d6736f78`
**API Endpoint:** `POST /rest/unlogAuftraege`

### Stages

| Stage | Value | Farbe | Position |
|-------|-------|-------|----------|
| Auftrag | `AUFTRAG` | blau | 0 |
| Termin | `TERMIN` | orange | 1 |
| Erledigt | `ERLEDIGT` | grün | 2 |

### Custom Fields

| Feld | API Name | Typ |
|------|----------|-----|
| Name | `name` | TEXT |
| Email | `email` | EMAILS |
| Handy Nr | `handyNr` | PHONES |
| Auftrag | `auftrag` | TEXT |
| Einnahmen | `einnahmen` | CURRENCY |
| Ausgaben | `ausgaben` | CURRENCY |
| Stage | `stage` | SELECT (Kanban) |

### API Beispiel

```bash
curl -X POST https://twenty.tschatscher.eu/rest/unlogAuftraege \
  -H "Authorization: Bearer API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Max Mustermann",
    "stage": "AUFTRAG",
    "email": {"primaryEmail": "max@example.com"},
    "handyNr": {"primaryPhoneNumber": "+491761234567", "primaryPhoneCountryCode": "DE"},
    "auftrag": "Samsung S22 LCD",
    "einnahmen": {"amountMicros": 150000000, "currencyCode": "EUR"},
    "ausgaben": {"amountMicros": 50000000, "currencyCode": "EUR"}
  }'
```

## 2. Versicherungen

### Stages

| Stage | Value | Position |
|-------|-------|----------|
| Maklermandat | `MAKLERMANDAT` | 0 |
| Angebote | `ANGEBOTE` | 1 |
| Anträge | `ANTRAEGE` | 2 |
| Verträge | `VERTRAEGE` | 3 |

## 3. Immobilien

### Stages

| Stage | Value | Position |
|-------|-------|----------|
| Neue Anfrage | `NEUE_ANFRAGE` | 0 |
| Erreicht | `ERREICHT` | 1 |
| Termin | `TERMIN` | 2 |
| Objekt vorbereitet | `OBJEKT_VORBEREITET` | 3 |
| Kein Interesse | `KEIN_INTERESSE` | 4 |

## 4. Büro-Workflows

### Stages

| Stage | Value | Position |
|-------|-------|----------|
| Aufgaben Büro | `AUFGABEN_BUERO` | 0 |
| Schaden | `SCHADEN` | 1 |
| Offene Rechnung | `OFFENE_RECHNUNG` | 2 |
| Anträge | `ANTRAEGE` | 3 |
| Mahnung | `MAHNUNG` | 4 |

## 5. Arbeitsanweisungen

### Stages

| Stage | Value | Position |
|-------|-------|----------|
| Agentur | `AGENTUR` | 0 |
| Kraftfahrzeug | `KRAFTFAHRZEUG` | 1 |
| Privat Sach | `PRIVAT_SACH` | 2 |
| Krankenversicherung | `KRANKENVERSICHERUNG` | 3 |

## Webhooks

Twenty Webhooks für Custom Objects:

```
Event: *.created, *.updated, *.deleted
URL: https://make.tschatscher.eu/webhook/twenty-webhook
Operation: create, update, delete (Wildcard: *)
```

### Webhook Payload Format

```json
{
  "targetUrl": "https://make.tschatscher.eu/webhook/twenty-webhook",
  "eventName": "unlogAuftrag.created",
  "objectMetadata": {
    "id": "a8c288b5-...",
    "nameSingular": "unlogAuftrag"
  },
  "record": {
    "id": "16b3cb77-...",
    "name": "Max Mustermann",
    "stage": "AUFTRAG",
    "auftrag": "Samsung S22 LCD",
    "einnahmen": { "amountMicros": 150000000, "currencyCode": "EUR" }
  }
}
```
