# n8n Workflows - Allianz Jaeger

## Uebersicht

| Workflow ID | Name | Trigger | Status |
|------------|------|---------|--------|
| `L0nt3WOb5Ew6CsUe` | UnLOG - Brevo - Meistertask - Google | Brevo Webhook | Aktiv |
| `cxgFhItwoQ56troU` | Twenty -> UnLOG Statusaenderung | Twenty Webhook | Aktiv |
| `Ti7k7a5jC2l1Yd2z` | Twenty API Test (Proxy) | n8n Webhook | Aktiv |
| `hLmDJBA0uOn2xB9w` | Twenty Dokument-Sync | Twenty Webhook (attachment) | Aktiv |
| *(geplant)* | Versicherung Formular -> Twenty | Brevo Webhook | Offen |
| *(geplant)* | SuperChat -> Twenty (TKV-Leads) | SuperChat Webhook | Offen |
| *(geplant)* | Twenty -> Brevo Listen Sync | Twenty Webhook | Offen |

## Workflow 1: UnLOG - Brevo -> Twenty + Google + Chatwoot

**ID:** `L0nt3WOb5Ew6CsUe`
**Webhook:** `https://make.tschatscher.eu/webhook/unlog-brevo`

### Flow

```
Brevo Webhook (POST)
  -> Daten extrahieren (Vorname, Nachname, Email, WhatsApp, Auftrag, Preis, Ausgabe)
  -> Telefonnummer formatieren (E.164)
  -> Parallel:
    +-- Google Contacts (erstellen/aktualisieren)
    +-- Google Sheets (2026 Sheet, neue Zeile)
    +-- Twenty CRM (UnLOG Auftrag erstellen, Stage: AUFTRAG)
    +-- Chatwoot (WhatsApp Template senden)
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

## Workflow 2: Twenty -> UnLOG Statusaenderung

**ID:** `cxgFhItwoQ56troU`

Parsed Twenty Webhook Events und verteilt sie an die richtigen Aktionen.

### Unterstuetzte Events

- `*.created` - Neues Objekt erstellt
- `*.updated` - Objekt geaendert

### Datenextraktion

```javascript
// Twenty Webhook Format (Custom Objects)
const record = body.record;
const objectName = body.objectMetadata?.nameSingular;
const eventName = body.eventName; // z.B. "unlogAuftrag.created"

// Felder auslesen
const name = record.name;
const stage = record.stage;
const einnahmen = record.einnahmen?.amountMicros / 1000000; // -> EUR
```

## Workflow 3: Twenty API Test (Proxy)

**ID:** `Ti7k7a5jC2l1Yd2z`
**Webhook:** `https://make.tschatscher.eu/webhook/twenty-proxy`

Proxy fuer Twenty Metadata API (Field Creation etc.) ueber n8n.

### Verwendung

```json
POST /webhook/twenty-proxy
{
  "method": "POST",
  "path": "/metadata",
  "body": {
    "query": "mutation { createOneField(...) { id name } }"
  }
}
```

## Workflow 4: Twenty Dokument-Sync

**ID:** `hLmDJBA0uOn2xB9w`
**Webhook:** `https://make.tschatscher.eu/webhook/twenty-attachment`

Automatischer Dokument-Sync von Twenty CRM auf den NAS Kundenordner via WebDAV.

Siehe [docs/dokument-sync.md](dokument-sync.md) fuer die komplette Dokumentation.

### Flow

```
Twenty Webhook (attachment.created)
  -> Parse & Route (nur .created Events)
  -> Person oder Immobilie? (IF)
    +-- Person -> Get Person -> Build Path (Bereich-Routing)
    |   VERSICHERUNG -> /Beratung/Versicherungen/{Name}/
    |   IMMOBILIEN   -> /Beratung/Immobilienmakler/{Verkauf|Vermietung}/{Name}/
    |   UNLOG        -> /UnLog/Auftraege/{Jahr}/{Name}/
    |   HAUSVERWALTUNG -> /Beratung/Hausverwaltung/{Name}/
    |   (kein Bereich -> STOP, kein Upload)
    +-- Immobilie -> Get Immobilie -> Build Path -> /Beratung/Immobilienmakler/Verkauf/{Adresse}/
  -> Create Parent Folder (WebDAV MKCOL)
  -> Create Folder (WebDAV MKCOL)
  -> Read Source File (aus Twenty Storage)
  -> Upload to NAS (WebDAV PUT)
  -> Update Twenty (Dokumentenordner-Link setzen)
```

## Geplant: Versicherung Formular -> Twenty

### Brevo Formular Felder

```
EMAIL, VORNAME, NACHNAME, WHATSAPP, GEBURTSDATUM, EVBNUMMER
Dropdowns: KUNDENART, EVBVORGANG, GEKUENDIGT, SCHADENSART, SERVICEART
```

### Flow

```
Brevo Webhook (Versicherung Formular)
  -> Daten extrahieren
  -> Telefonnummer formatieren (E.164)
  -> Datum konvertieren (DD-MM-YYYY -> YYYY-MM-DD)
  -> SELECT Werte mappen (Brevo -> Twenty)
  -> Twenty: Person erstellen/aktualisieren
  -> Twenty: Bereich = ["VERSICHERUNG"] setzen
  -> Google Contacts aktualisieren
```

## Geplant: Twenty -> Brevo Listen Sync

### Flow

```
Twenty Webhook (Person updated)
  -> Geaenderte Felder erkennen
  -> Brevo API: Kontakt erstellen/aktualisieren
  -> Brevo API: In richtige Liste(n) verschieben
  -> Brevo Automation laeuft los (Mail + WhatsApp)
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
| WebDAV (NAS) | Basic Auth | httpBasicAuth in n8n |
