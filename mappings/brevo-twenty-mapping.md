# Brevo ↔ Twenty CRM Feld-Mapping

## Kontaktfelder: Brevo → Twenty People

| Brevo Feld | Twenty Feld (API) | Twenty Typ | Mapping |
|-----------|-------------------|------------|---------|
| EMAIL | `emails.primaryEmail` | EMAILS | Direkt |
| VORNAME | `name.firstName` | FULL_NAME | Direkt |
| NACHNAME | `name.lastName` | FULL_NAME | Direkt |
| WHATSAPP | `phones.primaryPhoneNumber` | PHONES | E.164 normalisiert (+49...) |
| GEBURTSDATUM | `geburtsdatum` | DATE | DD-MM-YYYY → YYYY-MM-DD |
| POSTLEITZAHL | `postleitzahl` | TEXT | Direkt |
| KUNDENART | `kundenart` | SELECT | Mapping-Tabelle unten |
| BRIEFANREDE | `briefanrede` | SELECT | Mapping-Tabelle unten |
| EVBNUMMER | `evbNummer` | TEXT | Direkt |
| EVBVORGANG | `evbVorgang` | SELECT | Mapping-Tabelle unten |
| GEKUENDIGT | `gekuendigt` | SELECT | Mapping-Tabelle unten |
| SCHADENSART | `schadensart` | SELECT | Mapping-Tabelle unten |
| SERVICEART | `serviceart` | SELECT | Mapping-Tabelle unten |
| SONSTIGES | `sonstiges` | TEXT | Direkt |
| LEAD | `lead` | MULTI_SELECT | Array -> komma-separiert (Twenty -> Brevo) |
| JAHRESKONTAKT | `jahreskontakt` | DATE | DD-MM-YYYY → YYYY-MM-DD |
| TERMINDATUM | `terminDatum` | DATE | DD-MM-YYYY → YYYY-MM-DD |
| TERMINART | `terminArt` | TEXT | Direkt |
| TERMINUHRZEIT | `terminUhrzeit` | TEXT | Direkt |
| TERMINORT | `terminOrt` | TEXT | Direkt |
| TIERRASSE | `tierrasse` | TEXT | Direkt |
| TIERGESCHLECHT | `tiergeschlecht` | SELECT | Mapping-Tabelle unten |
| KATZENHALTUNG | `katzenhaltung` | SELECT | Mapping-Tabelle unten |
| TIERKRANKENKRANKHEITEN | `tierkrankenKrankheiten` | TEXT | Direkt |
| TIERKRANKENKASTRIERT | `tierkrankenKastriert` | SELECT | Mapping-Tabelle unten |
| TIERGEBURTSDATUM | `tiergeburtsdatum` | DATE | DD-MM-YYYY → YYYY-MM-DD |
| ZAHNALTER | `zahnalter` | TEXT | Direkt |
| NAMEKINDZAHN | `nameKindZahn` | TEXT | Direkt |
| IMMOBILIENTYP | `immobilientyp` | TEXT | Direkt |
| IMMOSCOUTANSCHRIFT | `immoscoutAnschrift` | TEXT | Direkt |
| IMMOBILIENWERT | `immobilienwert` | TEXT | Direkt |
| IMMOSCOUTID | `immoscoutId` | TEXT | Direkt |
| WOHNFLAECHE | `wohnflaeche` | TEXT | Direkt |
| GRUNDSTUECKFLAECHE | `grundstueckflaeche` | TEXT | Direkt |
| ZIMMERANZAHL | `zimmeranzahl` | TEXT | Direkt |
| BAUJAHR | `baujahr` | TEXT | Direkt |
| ORT | `ort` | TEXT | Direkt |
| STRASSE | `strasse` | TEXT | Direkt |
| ZAHNFEHLEN | `zahnfehlen` | TEXT | Direkt |
| ZAHNLINK | `zahnlink` | LINK | URL aus LINK-Feld extrahieren |
| WERBUNGFACEBOOK | `werbungFacebook` | TEXT | Direkt |
| UNLOGAUFTRAG | → UnLOG Auftraege Object | - | Separates Object |
| UNLOGPREIS | → UnLOG Aufträge Object | CURRENCY | * 1.000.000 (amountMicros) |
| UNLOGAUSGABE | → UnLOG Aufträge Object | CURRENCY | * 1.000.000 (amountMicros) |

## SELECT Feld Mappings

### Kundenart
| Brevo Wert | Twenty Value |
|-----------|-------------|
| Keine | *(leer lassen)* |
| Neu/BestandskundeDu | `NEU_BESTANDSKUNDE_DU` |
| Neu/BestandskundeSie | `NEU_BESTANDSKUNDE_SIE` |
| Interessent | `INTERESSENT` |
| Interessent Firma | `INTERESSENT_FIRMA` |
| Eigentümeranfrage | `EIGENTUEMERANFRAGE` |
| Immobilienanfrage | `IMMOBILIENANFRAGE` |

### Briefanrede
| Brevo Wert | Twenty Value |
|-----------|-------------|
| Du | `DU` |
| Sie | `SIE` |

### EvB Vorgang
| Brevo Wert | Twenty Value |
|-----------|-------------|
| Keine | *(leer lassen)* |
| Ausgegeben | `AUSGEGEBEN` |
| Erledigt | `ERLEDIGT` |

### Gekündigt
| Brevo Wert | Twenty Value |
|-----------|-------------|
| Keine Kündigung | `KEINE` |
| Kraft | `KFZ` |
| Privat Sach | `PRIVAT_SACH` |

### Schadensart
| Brevo Wert | Twenty Value |
|-----------|-------------|
| kein Schaden | `KEIN_SCHADEN` |
| Autoschaden | `AUTOSCHADEN` |
| Autoschaden mit Werkstattbindung | `AUTOSCHADEN_WERKSTATT` |
| Glasschaden Auto | `GLASSCHADEN` |
| Gebäudeschaden | `GEBAEUDESCHADEN` |
| Haftpflichtschaden | `HAFTPFLICHTSCHADEN` |

### Serviceart
| Brevo Wert | Twenty Value |
|-----------|-------------|
| Kein Service | `KEIN_SERVICE` |
| Antrag | `ANTRAG` |
| Rechnung Zahn | `RECHNUNG_ZAHN` |
| Rechnung Tierkranken | `RECHNUNG_TIERKRANKEN` |
| Rechnung Vollkranken | `RECHNUNG_VOLLKRANKEN` |
| Mahnung | `MAHNUNG` |

### Tiergeschlecht
| Brevo Wert | Twenty Value |
|-----------|-------------|
| Männlich | `MAENNLICH` |
| Weiblich | `WEIBLICH` |

### Katzenhaltung
| Brevo Wert | Twenty Value |
|-----------|-------------|
| Freigänger | `FREIGAENGER` |
| Wohnung | `WOHNUNG` |

### Kastriert
| Brevo Wert | Twenty Value |
|-----------|-------------|
| Ja | `JA` |
| Nein | `NEIN` |

### Lead (MULTI_SELECT)
| Brevo Wert | Twenty Value |
|-----------|-------------|
| TKVPferd | `TKVPFERD` |
| TKVHund | `TKVHUND` |
| TKVKatze | `TKVKATZE` |
| Zahnversicherung | `ZAHNVERSICHERUNG` |
| Immobilienanfrage | `IMMOBILIENANFRAGE` |
| Eigentuemeranfrage | `EIGENTUEMERANFRAGE` |
| Kinderkasko | `KINDERKASKO` |

**Hinweis:** LEAD ist ein MULTI_SELECT (Mehrfachauswahl). Bei Twenty -> Brevo Sync wird das Array komma-separiert gesendet (z.B. `TKVHUND,ZAHNVERSICHERUNG`).

## Twenty Tags → Brevo Listen Mapping

| Twenty Feld + Wert | → Brevo Liste |
|--------------------|---------------|
| kundenart = `NEU_BESTANDSKUNDE_DU` | Liste 15 |
| kundenart = `NEU_BESTANDSKUNDE_SIE` | Liste 16 |
| kundenart = `INTERESSENT` | Liste 48 (NeuerLead) |
| kundenart = `INTERESSENT_FIRMA` | Liste 21 |
| kundenart = `EIGENTUEMERANFRAGE` | Liste 19 |
| kundenart = `IMMOBILIENANFRAGE` | Liste 18 |
| evbVorgang = `AUSGEGEBEN` | Liste 46 |
| evbVorgang = `ERLEDIGT` | Liste 47 |
| gekuendigt = `KFZ` | Liste 28 |
| gekuendigt = `PRIVAT_SACH` | Liste 27 |
| schadensart = `AUTOSCHADEN` | Liste 29 |
| schadensart = `AUTOSCHADEN_WERKSTATT` | Liste 30 |
| schadensart = `GLASSCHADEN` | Liste 31 |
| schadensart = `GEBAEUDESCHADEN` | Liste 32 |
| schadensart = `HAFTPFLICHTSCHADEN` | Liste 33 |
| serviceart = `ANTRAG` | Liste 38 |
| serviceart = `RECHNUNG_ZAHN` | Liste 34 |
| serviceart = `RECHNUNG_TIERKRANKEN` | Liste 36 |
| serviceart = `RECHNUNG_VOLLKRANKEN` | Liste 35 |
| serviceart = `MAHNUNG` | Liste 37 |

## Telefonnummer-Normalisierung

Eingehende Formate → E.164 Format:

```
0176xxxxxxxx   → +49176xxxxxxxx
+49176xxxxxxxx → +49176xxxxxxxx  (bereits korrekt)
49176xxxxxxxx  → +49176xxxxxxxx
0049176xxxxxxxx → +49176xxxxxxxx
176xxxxxxxx    → +49176xxxxxxxx
```

## Datum-Konvertierung

Brevo Format: `DD-MM-YYYY` (z.B. `15-03-1990`)
Twenty Format: `YYYY-MM-DD` (z.B. `1990-03-15`)

```javascript
// Konvertierung
const [day, month, year] = brevoDate.split('-');
const twentyDate = `${year}-${month}-${day}`;
```

## Währung (UnLOG)

Twenty CURRENCY Typ verwendet `amountMicros`:

```
1 EUR = 1.000.000 amountMicros
150 EUR = 150.000.000 amountMicros
```

```javascript
const amountMicros = parseInt(euroValue) * 1000000;
const einnahmen = { amountMicros: amountMicros, currencyCode: "EUR" };
```
