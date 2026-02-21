# n8n Automation Templates 🔄

A curated collection of production-tested **n8n workflow templates** for business automation. Each workflow is battle-tested with real clients and documented with setup guides.

![n8n](https://img.shields.io/badge/n8n-EA4B71?style=flat&logo=n8n&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat&logo=openai&logoColor=white)
![Slack](https://img.shields.io/badge/Slack-4A154B?style=flat&logo=slack&logoColor=white)
![HubSpot](https://img.shields.io/badge/HubSpot-FF7A59?style=flat&logo=hubspot&logoColor=white)

> **20+ hours saved per week** for enterprise clients using these workflows.

## Workflows

### Sales & CRM
| Workflow | Description | Time Saved |
|----------|-------------|------------|
| [Lead Enrichment Pipeline](workflows/lead-enrichment.json) | Auto-enriches new CRM contacts with company data | ~5 hrs/week |
| [Follow-up Sequencer](workflows/follow-up-sequencer.json) | AI-generated follow-up emails based on engagement | ~3 hrs/week |
| [Deal Alert System](workflows/deal-alerts.json) | Slack alerts for stale deals, big opportunities | ~1 hr/week |

### Marketing
| Workflow | Description | Time Saved |
|----------|-------------|------------|
| [Social Media Scheduler](workflows/social-scheduler.json) | Cross-platform posting with AI content generation | ~4 hrs/week |
| [Newsletter Builder](workflows/newsletter-builder.json) | Auto-curates industry news into formatted newsletters | ~3 hrs/week |
| [SEO Monitor](workflows/seo-monitor.json) | Tracks rankings and alerts on significant changes | ~1 hr/week |

### Operations
| Workflow | Description | Time Saved |
|----------|-------------|------------|
| [Invoice Processor](workflows/invoice-processor.json) | Extracts data from PDF invoices via OCR + AI | ~4 hrs/week |
| [Client Onboarding](workflows/client-onboarding.json) | Automated onboarding: accounts, folders, welcome emails | ~2 hrs/week |
| [Report Generator](workflows/report-generator.json) | Weekly KPI reports from multiple data sources | ~3 hrs/week |

## Quick Start

1. **Import** — Open n8n → Workflows → Import from File → Select `.json`
2. **Configure** — Update credentials (API keys, OAuth tokens) in the workflow
3. **Activate** — Toggle the workflow on and test with sample data

## Workflow Architecture

Each workflow follows a consistent pattern:

```
Trigger → Validate → Process → Transform → Output → Notify
  ↓          ↓          ↓          ↓          ↓        ↓
Webhook    Schema     API Call   Format     Store    Slack/
Cron       Check      AI/LLM    Filter     CRM      Email
Form       Auth       Enrich    Map        Sheet    
```

## Requirements

- n8n v1.20+ (self-hosted or cloud)
- API credentials for integrated services (see each workflow's docs)
- Node.js 18+ (for self-hosted)

## Contributing

PRs welcome! Please follow the workflow template structure and include:
- JSON workflow file
- Documentation in `/docs`
- Screenshot of the workflow canvas

## License

MIT — [Antoine Batreau](https://github.com/antoine-lbo) / [Syncta.ai](https://syncta.ai)
