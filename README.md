# Twenty CRM Setup - Allianz Jaeger Versicherung & Immobilien

Komplette CRM-Konfiguration für **Allianz Jaeger** (Versicherung, Immobilien, UnLOG Reparaturen) mit Twenty CRM, Brevo (E-Mail/WhatsApp Marketing), n8n Automations und Chatwoot.

## 🏗️ Architektur

```
┌─────────────┐     ┌──────────┐     ┌──────────────┐     ┌─────────────┐
│ Brevo Forms │────▶│   n8n    │────▶│  Twenty CRM  │     │  Chatwoot   │
│ (Webhooks)  │     │ (Router) │     │  (People +   │     │ (WhatsApp)  │
└─────────────┘     │          │────▶│  Pipelines)  │     └──────┬──────┘
                    │          │     └──────────────┘            │
                    │          │────▶┌──────────────┐            │
                    │          │     │Google Sheets  │            │
                    │          │     │Google Contacts│            │
                    │          │     └──────────────┘            │
                    │          │◀────┌──────────────┐            │
                    │          │     │Twenty Webhook │            │
                    │          │────▶│ Brevo API     │───▶ Mail + WhatsApp
                    └──────────┘     └──────────────┘
```

## 📦 Custom Objects (Pipelines)

| Object | Kanban Stages | Beschreibung |
|--------|--------------|--------------|
| **UnLOG Aufträge** | Auftrag → Termin → Erledigt | Handy-Reparaturen |
| **Versicherungen** | Maklermandat → Angebote → Anträge → Verträge | Versicherungsverträge |
| **Immobilien** | Neue Anfrage → Erreicht → Termin → Objekt vorb. → kein Interesse | Immobilienvermittlung |
| **Büro-Workflows** | Aufgaben → Schaden → offene Rechnung → Anträge → Mahnung | Bürovorgänge |
| **Arbeitsanweisungen** | Agentur → KFZ → Privat Sach → Krankenvers. | Interne Anleitungen |

## 👤 People Custom Fields

Siehe [docs/people-fields.md](docs/people-fields.md) für alle 29 Custom Fields.

## 🔄 Brevo ↔ Twenty Sync

Siehe [mappings/brevo-twenty-mapping.md](mappings/brevo-twenty-mapping.md) für das komplette Feld-Mapping.

## 🔗 Workflows

Siehe [docs/workflows.md](docs/workflows.md) für alle n8n Workflow-Beschreibungen.

## 🛠️ Tech Stack

| Service | URL | Beschreibung |
|---------|-----|--------------|
| Twenty CRM | `twenty.tschatscher.eu` | Self-hosted CRM |
| n8n | `make.tschatscher.eu` | Workflow Automation |
| Brevo | `brevo.com` | E-Mail & WhatsApp Marketing |
| Chatwoot | `chatwoot.tschatscher.eu` | WhatsApp Business |
| Google Sheets | - | Buchhaltung/Tracking |
| Google Contacts | - | Kontaktsync |

Alles self-hosted auf **Ugreen NAS** via Docker + Portainer + Nginx Proxy Manager.

## 📝 Changelog

- **2026-02-04**: People Custom Fields erstellt (29 Felder)
- **2026-02-04**: UnLOG Aufträge Pipeline + Brevo Webhook Integration
- **2026-02-04**: Twenty Custom Objects erstellt (5 Pipelines)
- **2026-02-03**: Initiales Setup Twenty CRM
