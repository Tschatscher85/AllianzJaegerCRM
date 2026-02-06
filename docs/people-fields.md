# Twenty CRM - People Custom Fields

## Übersicht

31 Custom Fields auf dem People-Objekt, die alle Brevo-Kontaktfelder abbilden plus Dokument-Sync Felder.

**Object Metadata ID:** `f16bc624-35b3-4299-adc6-58f9613700bf`

## Basis-Kontaktdaten

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 1 | Geburtsdatum | `geburtsdatum` | DATE | Geburtsdatum des Kontakts |
| 2 | Postleitzahl | `postleitzahl` | TEXT | PLZ |
| 3 | Briefanrede | `briefanrede` | SELECT | Du / Sie |
| 4 | Sonstiges | `sonstiges` | TEXT | Freitext |
| 5 | Lead | `lead` | TEXT | Lead-Quelle |
| 6 | Jahreskontakt | `jahreskontakt` | DATE | Letzter Jahreskontakt |

## Geschäftsbereich

| # | Feld | API Name | Typ | Optionen |
|---|------|----------|-----|----------|
| 7 | Bereich | `bereich` | MULTI_SELECT | `UNLOG` (rot), `VERSICHERUNG` (blau), `IMMOBILIEN` (orange), `HAUSVERWALTUNG` (grün) |
| 8 | Kundenart | `kundenart` | SELECT | `NEU_BESTANDSKUNDE_DU`, `NEU_BESTANDSKUNDE_SIE`, `INTERESSENT`, `INTERESSENT_FIRMA`, `EIGENTUEMERANFRAGE`, `IMMOBILIENANFRAGE` |

## Versicherungs-Vorgänge

| # | Feld | API Name | Typ | Optionen |
|---|------|----------|-----|----------|
| 9 | EvB Nummer | `evbNummer` | TEXT | eVB-Nummer KFZ |
| 10 | EvB Vorgang | `evbVorgang` | SELECT | `AUSGEGEBEN` (orange), `ERLEDIGT` (grün) |
| 11 | Gekündigt | `gekuendigt` | SELECT | `KEINE` (grün), `KFZ` (rot), `PRIVAT_SACH` (rot) |
| 12 | Schadensart | `schadensart` | SELECT | `KEIN_SCHADEN`, `AUTOSCHADEN`, `AUTOSCHADEN_WERKSTATT`, `GLASSCHADEN`, `GEBAEUDESCHADEN`, `HAFTPFLICHTSCHADEN` |
| 13 | Serviceart | `serviceart` | SELECT | `KEIN_SERVICE`, `ANTRAG`, `RECHNUNG_ZAHN`, `RECHNUNG_TIERKRANKEN`, `RECHNUNG_VOLLKRANKEN`, `MAHNUNG` |

## Termin-Felder

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 14 | Termindatum | `terminDatum` | DATE | Nächster Termin |
| 15 | Terminart | `terminArt` | TEXT | Art des Termins |
| 16 | Termin Uhrzeit | `terminUhrzeit` | TEXT | Uhrzeit |
| 17 | Terminort | `terminOrt` | TEXT | Ort des Termins |

## Tierkrankenversicherung

| # | Feld | API Name | Typ | Optionen |
|---|------|----------|-----|----------|
| 18 | Tierrasse | `tierrasse` | TEXT | Rasse des Tieres |
| 19 | Tiergeschlecht | `tiergeschlecht` | SELECT | `MAENNLICH` (blau), `WEIBLICH` (rot) |
| 20 | Katzenhaltung | `katzenhaltung` | SELECT | `FREIGAENGER` (grün), `WOHNUNG` (blau) |
| 21 | Tierkranken Krankheiten | `tierkrankenKrankheiten` | TEXT | Vorerkrankungen |
| 22 | Kastriert | `tierkrankenKastriert` | SELECT | `JA` (grün), `NEIN` (rot) |
| 23 | Tier Geburtsdatum | `tiergeburtsdatum` | DATE | Geburtsdatum Tier |

## Zahnversicherung

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 24 | Zahnalter | `zahnalter` | TEXT | Alter für Zahnversicherung |
| 25 | Name Kind (Zahn) | `nameKindZahn` | TEXT | Name des Kindes |

## Immobilien

| # | Feld | API Name | Typ | Beschreibung / Optionen |
|---|------|----------|-----|------------------------|
| 26 | Immobilientyp | `immobilientyp` | TEXT | Art der Immobilie |
| 27 | ImmoScout Anschrift | `immoscoutAnschrift` | TEXT | Adresse aus ImmoScout |
| 28 | Immobilienwert | `immobilienwert` | TEXT | Geschaetzter Wert |
| 29 | ImmoScout ID | `immoscoutId` | TEXT | ImmoScout Objekt-ID |
| 30 | Immobilienkategorie | `immobilienkategorie` | SELECT | `VERKAUF` (orange), `VERMIETUNG` (tuerkis) |

## Dokumentenordner

| # | Feld | API Name | Typ | Beschreibung |
|---|------|----------|-----|-------------|
| 31 | Dokumentenordner | `dokumentenordner` | LINK | Automatisch gesetzt durch Dokument-Sync Workflow. Label = Windows-Pfad, URL = WebDAV-Link |

## Field IDs (für API-Zugriff)

```json
{
  "geburtsdatum": "7ccd3a24-d5ab-4193-b065-0832426ad4e5",
  "postleitzahl": "2e74fea8-97b7-423d-a02e-513ed6f165fd",
  "kundenart": "b6774654-1c2e-458b-b633-11c0b963685c",
  "briefanrede": "bedfc155-7d20-49ea-be87-099f5188a9f9",
  "evbNummer": "24e11af8-1a1d-48af-96af-09dce5b065eb",
  "evbVorgang": "5553a92e-1275-403c-adc3-6f8930f6ab89",
  "gekuendigt": "e3e50bb3-a247-49e4-847e-bc82686bc491",
  "schadensart": "694c284e-b37b-4fa7-b5f0-1c8ff0fcd129",
  "serviceart": "028865fb-31ea-4297-8107-8276b0e54525",
  "bereich": "40b3d685-8c7e-41ed-ac53-fb158b82da82",
  "sonstiges": "7d8fa883-6caa-4d3b-95c3-a9c530247838",
  "jahreskontakt": "8bb9f367-7e4d-486f-83d9-6c1bfde48925",
  "lead": "2e60dec7-9ce0-4161-bad3-bdc8f2b14775",
  "terminDatum": "996b5da5-2ffb-4ab5-800b-dfb7d6892a76",
  "terminArt": "c6311192-68c7-477b-9ba8-2b88fcca1e1b",
  "terminUhrzeit": "490c31f6-bd24-4166-93fc-d9decbb492e6",
  "terminOrt": "a6971496-378f-4f35-b028-00456ca4b7b1",
  "tierrasse": "8b0e1c7b-c97f-4bce-a8d8-a993072e9060",
  "tiergeschlecht": "ac51d8fc-f806-446d-8002-04029bee7537",
  "katzenhaltung": "53dcea76-7927-4ff1-8113-04403feff0f7",
  "tierkrankenKrankheiten": "4603a847-da33-4fde-9f9a-2b4ad748adfa",
  "tierkrankenKastriert": "01329217-ef8e-4f83-a8df-406b4ef818c6",
  "tiergeburtsdatum": "45d42f8b-c83f-49cd-99cc-ec17cd63f561",
  "zahnalter": "e1622dd5-df3c-4752-8c3e-5498c06e3276",
  "nameKindZahn": "a0d73865-2a28-4b69-ada9-42b7dd14bec7",
  "immobilientyp": "89986619-ff2d-42ea-9e11-9726979a4f2c",
  "immoscoutAnschrift": "1af02bd5-883e-43b9-8df1-16e32a4424d3",
  "immobilienwert": "afe92e3f-bf91-417e-85e6-85451dd2d3a3",
  "immoscoutId": "95ea79e2-4684-42a6-b095-f3a107269ff6",
  "immobilienkategorie": "b86ca957-d830-44e2-8966-9a7f60fb1c8f"
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
    "briefanrede": "DU",
    "postleitzahl": "76131",
    "geburtsdatum": "1990-05-15"
  }'
```
