# Twenty CRM - People Custom Fields

## Übersicht

40 Custom Fields auf dem People-Objekt, die alle Brevo-Kontaktfelder abbilden plus Dokument-Sync Felder.

**Object Metadata ID:** `f16bc624-35b3-4299-adc6-58f9613700bf`

## Basis-Kontaktdaten

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 1 | Geburtsdatum | `geburtsdatum` | DATE | Geburtsdatum des Kontakts |
| 2 | Postleitzahl | `postleitzahl` | TEXT | PLZ |
| 3 | Ort | `ort` | TEXT | Ort / Stadt |
| 4 | Strasse | `strasse` | TEXT | Strasse + Hausnummer |
| 5 | Briefanrede | `briefanrede` | SELECT | Du / Sie |
| 6 | Sonstiges | `sonstiges` | TEXT | Freitext |
| 7 | Jahreskontakt | `jahreskontakt` | DATE | Letzter Jahreskontakt |

## Geschaeftsbereich

| # | Feld | API Name | Typ | Optionen |
|---|------|----------|-----|----------|
| 8 | Bereich | `bereich` | MULTI_SELECT | `UNLOG` (rot), `VERSICHERUNG` (blau), `IMMOBILIEN` (orange), `HAUSVERWALTUNG` (gruen) |
| 9 | Kundenart | `kundenart` | SELECT | `NEU_BESTANDSKUNDE_DU`, `NEU_BESTANDSKUNDE_SIE`, `INTERESSENT`, `INTERESSENT_FIRMA`, `EIGENTUEMERANFRAGE`, `IMMOBILIENANFRAGE` |
| 10 | Lead | `lead` | MULTI_SELECT | `TKVPFERD` (gruen), `TKVHUND` (blau), `TKVKATZE` (orange), `ZAHNVERSICHERUNG` (lila), `IMMOBILIENANFRAGE` (gelb), `EIGENTUEMERANFRAGE` (tuerkis), `KINDERKASKO` (rot) |

## Versicherungs-Vorgaenge

| # | Feld | API Name | Typ | Optionen |
|---|------|----------|-----|----------|
| 11 | Versicherung EvB Nummer | `versicherungEvbNummer` | TEXT | eVB-Nummer KFZ |
| 12 | Versicherung EvB Vorgang | `versicherungEvbVorgang` | SELECT | `AUSGEGEBEN` (orange), `ERLEDIGT` (gruen) |
| 13 | Versicherung Gekuendigt | `versicherungGekuendigt` | SELECT | `KEINE` (gruen), `KFZ` (rot), `PRIVAT_SACH` (rot) |
| 14 | Versicherung Schadensart | `versicherungSchadensart` | SELECT | `KEIN_SCHADEN`, `AUTOSCHADEN`, `AUTOSCHADEN_WERKSTATT`, `GLASSCHADEN`, `GEBAEUDESCHADEN`, `HAFTPFLICHTSCHADEN` |
| 15 | Versicherung Serviceart | `versicherungServiceart` | SELECT | `KEIN_SERVICE`, `ANTRAG`, `RECHNUNG_ZAHN`, `RECHNUNG_TIERKRANKEN`, `RECHNUNG_VOLLKRANKEN`, `MAHNUNG` |

## Termin-Felder

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 16 | Termindatum | `terminDatum` | DATE | Naechster Termin |
| 17 | Terminart | `terminArt` | TEXT | Art des Termins |
| 18 | Termin Uhrzeit | `terminUhrzeit` | TEXT | Uhrzeit |
| 19 | Terminort | `terminOrt` | TEXT | Ort des Termins |

## Tierkrankenversicherung

| # | Feld | API Name | Typ | Optionen |
|---|------|----------|-----|----------|
| 20 | Tier Rasse | `tierRasse` | TEXT | Rasse des Tieres |
| 21 | Tier Geschlecht | `tierGeschlecht` | SELECT | `MAENNLICH` (blau), `WEIBLICH` (rot) |
| 22 | Tier Katzenhaltung | `tierKatzenhaltung` | SELECT | `FREIGAENGER` (gruen), `WOHNUNG` (blau) |
| 23 | Tier Krankheiten | `tierKrankheiten` | TEXT | Vorerkrankungen |
| 24 | Tier Kastriert | `tierKastriert` | SELECT | `JA` (gruen), `NEIN` (rot) |
| 25 | Tier Geburtsdatum | `tierGeburtsdatum` | DATE | Geburtsdatum Tier |

## Zahnversicherung

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 26 | Zahn Alter | `zahnAlter` | TEXT | Alter fuer Zahnversicherung |
| 27 | Zahn Name Kind | `zahnNameKind` | TEXT | Name des Kindes |
| 28 | Zahn Fehlen | `zahnFehlen` | TEXT | Fehlende Zaehne |
| 29 | Zahn Link | `zahnLink` | LINK | Link zu Zahn-Dokumenten |

## Immobilien

| # | Feld | API Name | Typ | Beschreibung / Optionen |
|---|------|----------|-----|------------------------|
| 30 | Immobilien Typ | `immobilienTyp` | TEXT | Art der Immobilie |
| 31 | Immobilien ImmoScout Anschrift | `immobilienImmoScoutAnschrift` | TEXT | Adresse aus ImmoScout |
| 32 | Immobilien Wert | `immobilienWert` | TEXT | Geschaetzter Wert |
| 33 | Immobilien ImmoScout ID | `immobilienImmoScoutId` | TEXT | ImmoScout Objekt-ID |
| 34 | Immobilien Kategorie | `immobilienKategorie` | SELECT | `VERKAUF` (orange), `VERMIETUNG` (tuerkis) |
| 35 | Immobilien Wohnflaeche | `immobilienWohnflaeche` | TEXT | Wohnflaeche in qm |
| 36 | Immobilien Grundstueckflaeche | `immobilienGrundstueckflaeche` | TEXT | Grundstueckflaeche in qm |
| 37 | Immobilien Zimmeranzahl | `immobilienZimmeranzahl` | TEXT | Anzahl der Zimmer |
| 38 | Immobilien Baujahr | `immobilienBaujahr` | TEXT | Baujahr der Immobilie |

## Marketing

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 39 | Werbung Facebook | `werbungFacebook` | TEXT | Facebook Werbe-Tracking |

## Dokumentenordner

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 40 | Dokumentenordner | `dokumentenordner` | LINK | Automatisch gesetzt durch Dokument-Sync Workflow. Label = Windows-Pfad, URL = WebDAV-Link |

## Field IDs (fuer API-Zugriff)

```json
{
  "geburtsdatum": "7ccd3a24-d5ab-4193-b065-0832426ad4e5",
  "postleitzahl": "2e74fea8-97b7-423d-a02e-513ed6f165fd",
  "ort": "b4f12dd8-869f-415a-8688-fe8a3f8a01d1",
  "strasse": "681d24b0-a4ee-472f-8710-f53de3ed7b6a",
  "kundenart": "b6774654-1c2e-458b-b633-11c0b963685c",
  "briefanrede": "bedfc155-7d20-49ea-be87-099f5188a9f9",
  "versicherungEvbNummer": "bddd113a-2321-488f-940b-151c7ee9f315",
  "versicherungEvbVorgang": "085e9b50-e8a6-41af-954b-f0e8104110d8",
  "versicherungGekuendigt": "767405ea-bd7f-4558-9bb9-eacea7210b56",
  "versicherungSchadensart": "1705cd11-e635-48e5-84f5-91e658e57111",
  "versicherungServiceart": "1db9823c-fff4-4fed-a6ea-ee45ca7519c6",
  "bereich": "40b3d685-8c7e-41ed-ac53-fb158b82da82",
  "sonstiges": "7d8fa883-6caa-4d3b-95c3-a9c530247838",
  "jahreskontakt": "8bb9f367-7e4d-486f-83d9-6c1bfde48925",
  "lead": "ac1f5ee7-e677-4e6e-867a-cd335a116cdc",
  "terminDatum": "996b5da5-2ffb-4ab5-800b-dfb7d6892a76",
  "terminArt": "c6311192-68c7-477b-9ba8-2b88fcca1e1b",
  "terminUhrzeit": "490c31f6-bd24-4166-93fc-d9decbb492e6",
  "terminOrt": "a6971496-378f-4f35-b028-00456ca4b7b1",
  "tierRasse": "0bfb8cc2-3ed3-4afb-aa69-3727217988c2",
  "tierGeschlecht": "40814e7b-97ca-4536-9aea-03568ff9a31d",
  "tierKatzenhaltung": "e0cb6d7b-8701-4f48-a767-f07bdb7d224c",
  "tierKrankheiten": "e3cba945-eb25-4eff-9db0-e415de05f148",
  "tierKastriert": "e153993d-8933-4e9b-8b5d-2908d5e6980f",
  "tierGeburtsdatum": "53b03711-bf5c-4e9e-af01-a45f38f93261",
  "zahnAlter": "492c11e9-2fba-49f9-a434-97c7de8e7bf0",
  "zahnNameKind": "f72f204a-6dc0-460e-bc0f-7d05970b8785",
  "zahnFehlen": "02f861b3-a1bf-4d11-a66f-97ff02762d99",
  "zahnLink": "0b6c6727-2992-4649-961c-2b7be2a3dbe4",
  "immobilienTyp": "edc1985f-f90c-4992-9627-5cbb01f69074",
  "immobilienImmoScoutAnschrift": "6a41d23b-b3cf-48bf-a47b-3f79af29c8d0",
  "immobilienWert": "30b0a55a-b414-419b-9e65-7f3c7d68c90e",
  "immobilienImmoScoutId": "8ffb532f-98b8-4cff-b564-267d73499e5c",
  "immobilienKategorie": "6a04b90a-832c-4310-81bb-027153fa7883",
  "immobilienWohnflaeche": "e4fcb9d9-b2f6-4d85-a652-28eb5a8c51ad",
  "immobilienGrundstueckflaeche": "dcac9558-fd8e-43ac-a5a1-d2e8689131f0",
  "immobilienZimmeranzahl": "9bcc6dd3-e801-4cb1-b443-1ec594b28dc2",
  "immobilienBaujahr": "37cd66e3-9f85-4d93-858c-1bcf5c312925",
  "werbungFacebook": "4cdcf512-ceb5-498f-859b-0f41ef0fcedf"
}
```

## API Beispiel: Person erstellen

```bash
curl -X POST https://twenty.tschatscher.eu/rest/people \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": {"firstName": "Max", "lastName": "Mustermann"},
    "emails": {"primaryEmail": "max@example.com"},
    "phones": {"primaryPhoneNumber": "+491761234567", "primaryPhoneCountryCode": "DE"},
    "kundenart": "NEU_BESTANDSKUNDE_DU",
    "bereich": ["VERSICHERUNG"],
    "lead": ["TKVHUND", "ZAHNVERSICHERUNG"],
    "briefanrede": "DU",
    "postleitzahl": "76131",
    "ort": "Karlsruhe",
    "strasse": "Kaiserstr. 1",
    "geburtsdatum": "1990-05-15",
    "versicherungEvbNummer": "W123456789",
    "versicherungEvbVorgang": "AUSGEGEBEN"
  }'
```
