# n8n Automation Templates

[![Validate Workflows](https://github.com/antoine-lbo/n8n-automation-templates/actions/workflows/validate.yml/badge.svg)](https://github.com/antoine-lbo/n8n-automation-templates/actions/workflows/validate.yml)
[![n8n](https://img.shields.io/badge/n8n-1.20+-EA4B71.svg)](https://n8n.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OpenAI](https://img.shields.io/badge/OpenAI-API-412991.svg?logo=openai&logoColor=white)](https://openai.com)
[![Slack](https://img.shields.io/badge/Slack-Integration-4A154B.svg?logo=slack&logoColor=white)](https://slack.com)
[![HubSpot](https://img.shields.io/badge/HubSpot-CRM-FF7A59.svg?logo=hubspot&logoColor=white)](https://hubspot.com)

Production-tested **n8n workflow templates** for business automation. Each workflow is battle-tested with real clients and documented with setup guides.

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

1. **Import** -- Open n8n > Workflows > Import from File > Select `.json`
2. **Configure** -- Update credentials (API keys, OAuth tokens) in the workflow
3. **Activate** -- Toggle the workflow on and test with sample data

## Workflow Architecture

Each workflow follows a consistent pattern:

```
Trigger -> Validate -> Process -> Transform -> Output -> Notify
  |          |          |          |           |         |
Webhook    Schema    API Call    Format      Store    Slack/
Cron       Check     AI/LLM     Filter      CRM      Email
Form       Auth      Enrich     Map         Sheet
```

## Requirements

- n8n v1.20+ (self-hosted or cloud)
- API credentials for integrated services (see each workflow docs)
- Node.js 18+ (for self-hosted)

## Contributing

PRs welcome! Please follow the workflow template structure and include:
- JSON workflow file
- Documentation in `/docs`
- Screenshot of the workflow canvas

See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines.

## Security

See [SECURITY.md](SECURITY.md) for credential management and vulnerability reporting.

## License

MIT License - see [LICENSE](LICENSE) for details.

---

Built by [Antoine Batreau](https://github.com/antoine-lbo) at [Syncta](https://syncta.ai)
