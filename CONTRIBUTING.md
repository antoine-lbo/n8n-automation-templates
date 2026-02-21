# Contributing to n8n Automation Templates

Thanks for your interest in contributing! This guide will help you add new
workflow templates or improve existing ones.

## Getting Started

1. **Fork** this repository
2. **Clone** your fork locally
3. **Create a branch** for your changes: `git checkout -b add/my-workflow`
4. **Make your changes** (see guidelines below)
5. **Test** your workflow in a local n8n instance
6. **Submit a Pull Request**

## Workflow Template Guidelines

### File Structure

All workflow templates live in the `workflows/` directory as `.json` files.

```
workflows/
  my-workflow-name.json     # Kebab-case naming
docs/
  my-workflow-name.md       # Optional: detailed documentation
```

### Naming Conventions

- Use **kebab-case** for file names: `lead-enrichment-pipeline.json`
- Names should be descriptive and reflect the workflow purpose
- Avoid abbreviations unless widely understood (API, CRM, etc.)

### Template Requirements

Every workflow template **must**:

- Be a valid n8n workflow JSON export
- Include a clear workflow name in the `"name"` field
- Use **Sticky Notes** within the workflow to explain the logic
- Replace all credentials with placeholder values
- Remove any hardcoded API keys, tokens, or secrets
- Be tested on n8n v1.0+ (self-hosted or cloud)

### Credential Handling

**Never** commit real credentials. Before exporting:

1. Go to each node with credentials
2. Note the credential type needed
3. Export the workflow (credentials will be excluded by default)
4. Add a Sticky Note listing required credentials and their types

### Categories

When adding a new workflow, it should fall into one of these categories:

| Category | Description | Example |
|----------|-------------|---------|
| **Lead Management** | Lead capture, scoring, enrichment, CRM sync | `lead-enrichment-pipeline.json` |
| **Customer Ops** | Onboarding, feedback, support automation | `customer-onboarding.json` |
| **Invoicing & Finance** | Invoice processing, reminders, billing | `invoice-processor.json` |
| **Monitoring** | Error tracking, alerting, health checks | `error-monitoring-alerting.json` |
| **Marketing** | Social media, content pipelines, scheduling | `social-media-content-pipeline.json` |
| **Data Ops** | Backups, exports, data transformation | `data-backup-pipeline.json` |

### Workflow Quality Checklist

Before submitting, verify:

- [ ] Workflow runs successfully end-to-end
- [ ] Error handling is implemented (try/catch or error workflows)
- [ ] Sticky Notes explain the purpose and each major step
- [ ] No hardcoded credentials or personal data
- [ ] Node names are descriptive (not "HTTP Request 1", "Function 2")
- [ ] Webhook URLs use relative paths where possible
- [ ] Rate limiting is considered for API-heavy workflows

## Adding Documentation

For complex workflows, add a markdown file in `docs/`:

```markdown
# Workflow Name

## Overview
Brief description of what this workflow does.

## Prerequisites
- Required accounts/services
- Required n8n credentials
- Environment variables

## Setup
Step-by-step instructions.

## Configuration
What to customize before running.
```

## Pull Request Process

1. Ensure your workflow meets all requirements above
2. Update the README if adding a new category
3. Use a clear PR title: `Add: [workflow-name] - [brief description]`
4. Include in the PR description:
   - What the workflow does
   - Which services/integrations it uses
   - Any special setup requirements

## Questions?

Open an issue if you have questions about contributing or need help
with your workflow template.
