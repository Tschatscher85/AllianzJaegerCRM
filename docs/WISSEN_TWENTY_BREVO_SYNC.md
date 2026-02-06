# Twenty -> Brevo Kontakt Sync - Technisches Wissen

## Workflow: `wJDcKGNAwfscBq51` - "Twenty -> Brevo Kontakt Sync"

### Architektur (v4 - REST API)
```
Twenty Webhook -> Person-ID extrahieren -> 2 Min warten -> Person aus Twenty laden (REST)
-> Parse & Filter -> Brevo Suche per TWENTY_ID -> Smart Brevo Upsert
-> IF Update: PUT /contacts/{alteEmail} mit attributes.EMAIL
-> IF Create: POST /contacts mit updateEnabled
```

### Twenty REST API
- **Endpoint**: `GET https://twenty.tschatscher.eu/rest/people/{id}`
- **Auth**: `predefinedCredentialType: httpHeaderAuth`, Credential ID: `pRZsKWNSoLeAlxo4`
- **WICHTIG**: GraphQL (`/api/graphql`) funktioniert NICHT (404)! Immer REST verwenden.
- **Response-Format**: Daten unter `data.person` oder direkt im Root

### Brevo API - Wichtige Erkenntnisse

#### Email aendern
- `PUT /v3/contacts/{identifier}` mit `{"attributes": {"EMAIL": "neue@email.de"}}`
- **NICHT** `{"email": "neue@email.de"}` - das wird ignoriert!
- Der `identifier` in der URL ist die ALTE Email

#### TWENTY_ID System
- Brevo Attribut `TWENTY_ID` (Text) speichert die Twenty Person UUID
- Suche: `GET /v3/contacts?filter=equals(TWENTY_ID,"uuid")&limit=1`
- Ermoeglicht: Kontakt finden auch wenn Email geaendert wurde

#### WHATSAPP Constraint
- Brevo erlaubt eine WHATSAPP-Nummer nur bei EINEM Kontakt
- Fehler: "WHATSAPP is already associated with another Contact"
- Loesung: Alten Kontakt per TWENTY_ID updaten statt neuen erstellen

#### Dropdown/Enum Felder
- Brevo speichert Dropdowns als numerische IDs (z.B. "2" statt "Kraft")
- Beim Schreiben muessen die Anzeigenamen verwendet werden (z.B. "Kraft")
- Beim Lesen kommen numerische IDs zurueck

### Lead MULTI_SELECT Mapping
```javascript
const leadBrevo = {
  'TKVPFERD': 'TKVPferd',
  'TKVHUND': 'TKVHund',
  'TKVKATZE': 'TKVKatze',
  'ZAHNVERSICHERUNG': 'Zahnversicherung',
  'IMMOBILIENANFRAGE': 'Immobilienanfrage',
  'EIGENTUEMERANFRAGE': 'Eigentuemeranfrage',
  'KINDERKASKO': 'Kinderkasko'
};
// Array -> komma-separiert: ["TKVHUND"] -> "TKVHund"
```

### Kundenart -> Brevo Listen Mapping
```
NEU_BESTANDSKUNDE_DU -> Liste 15
NEU_BESTANDSKUNDE_SIE -> Liste 16
INTERESSENT -> Liste 48
INTERESSENT_FIRMA -> Liste 21
EIGENTUEMERANFRAGE -> Liste 19
IMMOBILIENANFRAGE -> Liste 18
```

### Credentials
- **Twenty API**: httpHeaderAuth Credential `pRZsKWNSoLeAlxo4` (JWT Token)
- **Brevo API Key**: (in n8n Workflow hinterlegt, nicht in Git speichern)
- **Twenty Workspace**: `57085d64-6998-41f1-8d36-f349b517e3ee`

### Dokument-Sync Workflow: `hLmDJBA0uOn2xB9w`
- Referenz fuer korrekte Twenty REST API Nutzung
- Verwendet gleichen httpHeaderAuth Credential
- WebDAV Upload zu Unraid NAS

### Docker Compose (Twenty CRM)
- Pfad: `docker/twenty-compose.yml`
- OpenAI Keys gesetzt fuer KI-Features:
  - `OPENAI_COMPATIBLE_BASE_URL: https://api.openai.com/v1`
  - `OPENAI_COMPATIBLE_MODEL_NAMES: gpt-4o-mini,gpt-4o`

### n8n Tipps
- Wait-Node: Workflow-Updates koennen wartende Executions killen
- `n8n_update_partial_workflow` mit `updateNode` - nutze `updates` Objekt
- Waiting Executions erscheinen nicht in normaler Execution-Liste
- REST API Felder: Twenty nutzt camelCase (z.B. `versicherungEvbNummer`)

## Datum: 2026-02-06
