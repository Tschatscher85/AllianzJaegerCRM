# n8n Workflows - Allianz Jaeger

## Übersicht

| Workflow ID | Name | Trigger | Status |
|------------|------|---------|--------|
| `L0nt3WOb5Ew6CsUe` | UnLOG - Brevo - Meistertask - Google | Brevo Webhook | ✅ Aktiv |
| `cxgFhItwoQ56troU` | Twenty Webhook Parser | Twenty Webhook | ✅ Aktiv |
| `Ti7k7a5jC2l1Yd2z` | Twenty API Proxy | n8n Webhook | ✅ Aktiv |
| *(geplant)* | Versicherung Formular → Twenty | Brevo Webhook | ⏳ Offen |
| *(geplant)* | Twenty → Brevo Listen Sync | Twenty Webhook | ⏳ Offen |

## Workflow 1: UnLOG - Brevo → Twenty + Google + Chatwoot

**ID:** `L0nt3WOb5Ew6CsUe`
**Webhook:** `https://make.tschatscher.eu/webhook/unlog-brevo`

### Flow

```
Brevo Webhook (POST)
  → Daten extrahieren (Vorname, Nachname, Email, WhatsApp, Auftrag, Preis, Ausgabe)
  → Telefonnummer formatieren (E.164)
  → Parallel:
    ├── Google Contacts (erstellen/aktualisieren)
    ├── Google Sheets (2026 Sheet, neue Zeile)
    ├── Twenty CRM (UnLOG Auftrag erstellen, Stage: AUFTRAG)
    └── Chatwoot (WhatsApp Template senden)
```

### Twenty API Call

```
POST https://twenty.tschatscher.eu/rest/unlogAuftraege
{
  "name": "{{ vorname }} {{ nachname }}",
  "stage": "AUFTRAG",
  "email": { "primaryEmail": "{{ email }}" },
  "handyNr": { "primaryPhoneNumber": "{{ phone_e164 }}", "primaryPhoneCountryCode": "DE" },
  "auftrag": "{{ auftrag }}",
  "einnahmen": { "amountMicros": {{ preis * 1000000 }}, "currencyCode": "EUR" },
  "ausgaben": { "amountMicros": {{ ausgabe * 1000000 }}, "currencyCode": "EUR" }
}
```

## Workflow 2: Twenty Webhook Parser

**ID:** `cxgFhItwoQ56troU`

Parsed Twenty Webhook Events und verteilt sie an die richtigen Aktionen.

### Unterstützte Events

- `*.created` - Neues Objekt erstellt
- `*.updated` - Objekt geändert

### Datenextraktion

```javascript
// Twenty Webhook Format (Custom Objects)
const record = body.record;
const objectName = body.objectMetadata?.nameSingular;
const eventName = body.eventName; // z.B. "unlogAuftrag.created"

// Felder auslesen
const name = record.name;
const stage = record.stage;
const einnahmen = record.einnahmen?.amountMicros / 1000000; // → EUR
```

## Workflow 3: Twenty API Proxy

**ID:** `Ti7k7a5jC2l1Yd2z`
**Webhook:** `https://make.tschatscher.eu/webhook/twenty-proxy`

Proxy für Twenty Metadata API (Field Creation etc.) über n8n.

### Verwendung

```json
POST /webhook/twenty-proxy
{
  "method": "POST",
  "path": "/rest/metadata/fields",
  "body": {
    "objectMetadataId": "...",
    "name": "feldname",
    "label": "Feld Label",
    "type": "TEXT"
  }
}
```

## Geplant: Versicherung Formular → Twenty

### Brevo Formular Felder

```
EMAIL, VORNAME, NACHNAME, WHATSAPP, GEBURTSDATUM, EVBNUMMER
Dropdowns: KUNDENART, EVBVORGANG, GEKUENDIGT, SCHADENSART, SERVICEART
```

### Flow

```
Brevo Webhook (Versicherung Formular)
  → Daten extrahieren
  → Telefonnummer formatieren (E.164)
  → Datum konvertieren (DD-MM-YYYY → YYYY-MM-DD)
  → SELECT Werte mappen (Brevo → Twenty)
  → Twenty: Person erstellen/aktualisieren
  → Twenty: Bereich = ["VERSICHERUNG"] setzen
  → Google Contacts aktualisieren
```

## Geplant: Twenty → Brevo Listen Sync

### Flow

```
Twenty Webhook (Person updated)
  → Geänderte Felder erkennen
  → Brevo API: Kontakt erstellen/aktualisieren
  → Brevo API: In richtige Liste(n) verschieben
  → Brevo Automation läuft los (Mail + WhatsApp)
```

### Mapping Logic (JavaScript)

```javascript
const fieldToList = {
  kundenart: {
    NEU_BESTANDSKUNDE_DU: 15,
    NEU_BESTANDSKUNDE_SIE: 16,
    INTERESSENT: 48,
    INTERESSENT_FIRMA: 21,
    EIGENTUEMERANFRAGE: 19,
    IMMOBILIENANFRAGE: 18
  },
  evbVorgang: { AUSGEGEBEN: 46, ERLEDIGT: 47 },
  gekuendigt: { KFZ: 28, PRIVAT_SACH: 27 },
  schadensart: {
    AUTOSCHADEN: 29,
    AUTOSCHADEN_WERKSTATT: 30,
    GLASSCHADEN: 31,
    GEBAEUDESCHADEN: 32,
    HAFTPFLICHTSCHADEN: 33
  },
  serviceart: {
    ANTRAG: 38,
    RECHNUNG_ZAHN: 34,
    RECHNUNG_TIERKRANKEN: 36,
    RECHNUNG_VOLLKRANKEN: 35,
    MAHNUNG: 37
  }
};
```

## API Credentials

| Service | Auth | Hinweis |
|---------|------|---------|
| Twenty CRM | Bearer Token | API Key in n8n Credentials |
| Brevo | API Key | v3 API |
| Chatwoot | API Token | Account 1 |
| Google | OAuth2 | Sheets + Contacts |
