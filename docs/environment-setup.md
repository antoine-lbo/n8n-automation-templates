# Environment Setup Guide

Complete guide to configuring environment variables, credentials, and external services for all n8n workflow templates.

## Table of Contents

- [Prerequisites](#prerequisites)
- [n8n Instance Setup](#n8n-instance-setup)
- [Environment Variables](#environment-variables)
- [Credential Configuration](#credential-configuration)
- [Service-Specific Setup](#service-specific-setup)
- [Docker Deployment](#docker-deployment)
- [Troubleshooting](#troubleshooting)

## Prerequisites

- **n8n** v1.20+ (self-hosted or n8n Cloud)
- **Node.js** 18+ (for self-hosted)
- **Docker** & **Docker Compose** (recommended for self-hosted)
- **PostgreSQL** 14+ (recommended) or SQLite (development only)
- **Redis** 7+ (for queue mode and caching)

## n8n Instance Setup

### Option 1: Docker Compose (Recommended)

```bash
git clone https://github.com/antoine-lbo/n8n-automation-templates.git
cd n8n-automation-templates
cp .env.example .env
# Edit .env with your configuration
docker compose up -d
```

### Option 2: npm (Development)

```bash
npm install n8n -g
export N8N_CONFIG_DIR=./config
n8n start
```

### Option 3: n8n Cloud

1. Sign up at [n8n.cloud](https://n8n.cloud)
2. Import workflows via **Settings > Import from File**
3. Configure credentials in the n8n UI

## Environment Variables

### Core n8n Configuration

```env
# ─── n8n Core ───────────────────────────────────────────
N8N_HOST=0.0.0.0
N8N_PORT=5678
N8N_PROTOCOL=https
WEBHOOK_URL=https://n8n.yourdomain.com/
N8N_ENCRYPTION_KEY=your-random-encryption-key-min-32-chars

# ─── Database ──────────────────────────────────────────
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=postgres
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=n8n
DB_POSTGRESDB_PASSWORD=your-secure-password

# ─── Queue Mode (for scaling) ─────────────────────────
EXECUTIONS_MODE=queue
QUEUE_BULL_REDIS_HOST=redis
QUEUE_BULL_REDIS_PORT=6379
QUEUE_BULL_REDIS_PASSWORD=your-redis-password

# ─── Execution Settings ───────────────────────────────
EXECUTIONS_DATA_SAVE_ON_ERROR=all
EXECUTIONS_DATA_SAVE_ON_SUCCESS=all
EXECUTIONS_DATA_SAVE_MANUAL_EXECUTIONS=true
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=336  # 14 days in hours

# ─── Timezone ─────────────────────────────────────────
GENERIC_TIMEZONE=America/Los_Angeles
TZ=America/Los_Angeles
```

### Workflow-Specific Variables

```env
# ─── Lead Qualifier Integration ────────────────────────
LEAD_QUALIFIER_API_URL=http://localhost:8000
LEAD_QUALIFIER_API_KEY=your-lead-qualifier-api-key

# ─── CRM (HubSpot) ────────────────────────────────────
HUBSPOT_API_KEY=your-hubspot-api-key
HUBSPOT_PORTAL_ID=your-portal-id

# ─── Email (SMTP) ─────────────────────────────────────
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your-app-password
SMTP_FROM_EMAIL=notifications@yourdomain.com
SMTP_FROM_NAME=Syncta Automation

# ─── Slack ────────────────────────────────────────────
SLACK_WEBHOOK_URL=https://hooks.slack.com/services/T.../B.../xxx
SLACK_BOT_TOKEN=xoxb-your-slack-bot-token

# ─── Supabase ─────────────────────────────────────────
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_KEY=your-supabase-service-role-key

# ─── Storage (for PDF/file uploads) ───────────────────
STORAGE_BUCKET=invoices

# ─── PDF Generation ───────────────────────────────────
PDF_SERVICE_URL=http://gotenberg:3000
```

## Credential Configuration

Each workflow requires specific n8n credentials. Configure these in **Settings > Credentials** in the n8n UI.

### Required Credentials by Workflow

| Workflow | Credentials Needed |
|----------|-------------------|
| `error-monitoring.json` | Slack (Webhook), Email (SMTP) |
| `lead-crm-sync.json` | HubSpot (API Key), Supabase (API Key), Slack (Webhook) |
| `invoice-generation.json` | Supabase (API Key), SMTP, Slack (Webhook) |
| `social-media-scheduler.json` | Twitter (OAuth2), LinkedIn (OAuth2), Buffer (API Key) |
| `data-backup-pipeline.json` | AWS S3 (Access Key), PostgreSQL, Slack (Webhook) |

### Setting Up HubSpot

1. Go to **HubSpot > Settings > Integrations > Private Apps**
2. Create a new private app with scopes:
   - `crm.objects.contacts.read`
   - `crm.objects.contacts.write`
   - `crm.objects.deals.read`
   - `crm.objects.deals.write`
3. Copy the access token
4. In n8n: **Credentials > New > HubSpot API** > paste token

### Setting Up Slack

**Option A: Incoming Webhook (simplest)**
1. Go to [api.slack.com/apps](https://api.slack.com/apps)
2. Create new app > **Incoming Webhooks** > Activate
3. Add webhook to desired channel
4. Copy the webhook URL

**Option B: Bot Token (for advanced features)**
1. Create app at [api.slack.com/apps](https://api.slack.com/apps)
2. Add bot scopes: `chat:write`, `channels:read`, `files:write`
3. Install to workspace and copy bot token

### Setting Up Supabase

1. Create project at [supabase.com](https://supabase.com)
2. Go to **Settings > API**
3. Copy the **Project URL** and **service_role key**
4. In n8n: use **Header Auth** credential with:
   - Name: `apikey`
   - Value: your service_role key

### Setting Up SMTP (Gmail)

1. Enable 2-Factor Authentication on your Google account
2. Go to **Security > App Passwords**
3. Generate a new app password for "Mail"
4. In n8n: **Credentials > New > SMTP**
   - Host: `smtp.gmail.com`
   - Port: `587`
   - User: your Gmail address
   - Password: the app password (not your Gmail password)

## Docker Deployment

### docker-compose.yml

```yaml
version: "3.8"

services:
  n8n:
    image: n8nio/n8n:latest
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=${N8N_HOST}
      - N8N_PORT=5678
      - N8N_PROTOCOL=${N8N_PROTOCOL}
      - WEBHOOK_URL=${WEBHOOK_URL}
      - N8N_ENCRYPTION_KEY=${N8N_ENCRYPTION_KEY}
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=${DB_POSTGRESDB_DATABASE}
      - DB_POSTGRESDB_USER=${DB_POSTGRESDB_USER}
      - DB_POSTGRESDB_PASSWORD=${DB_POSTGRESDB_PASSWORD}
      - EXECUTIONS_MODE=queue
      - QUEUE_BULL_REDIS_HOST=redis
      - QUEUE_BULL_REDIS_PORT=6379
      - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}
    volumes:
      - n8n_data:/home/node/.n8n
      - ./workflows:/home/node/workflows
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy

  n8n-worker:
    image: n8nio/n8n:latest
    restart: unless-stopped
    command: worker
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=${DB_POSTGRESDB_DATABASE}
      - DB_POSTGRESDB_USER=${DB_POSTGRESDB_USER}
      - DB_POSTGRESDB_PASSWORD=${DB_POSTGRESDB_PASSWORD}
      - EXECUTIONS_MODE=queue
      - QUEUE_BULL_REDIS_HOST=redis
      - QUEUE_BULL_REDIS_PORT=6379
      - N8N_ENCRYPTION_KEY=${N8N_ENCRYPTION_KEY}
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      - n8n

  postgres:
    image: postgres:16-alpine
    restart: unless-stopped
    environment:
      - POSTGRES_DB=${DB_POSTGRESDB_DATABASE}
      - POSTGRES_USER=${DB_POSTGRESDB_USER}
      - POSTGRES_PASSWORD=${DB_POSTGRESDB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_POSTGRESDB_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    restart: unless-stopped
    command: redis-server --requirepass ${QUEUE_BULL_REDIS_PASSWORD:-}
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

  gotenberg:
    image: gotenberg/gotenberg:8
    restart: unless-stopped
    ports:
      - "3000:3000"

volumes:
  n8n_data:
  postgres_data:
  redis_data:
```

### Importing Workflows

After starting n8n, import workflows using the CLI or UI:

```bash
# Import all workflows via CLI
for file in workflows/*.json; do
  docker exec -i n8n n8n import:workflow --input="$file"
done

# Or import a single workflow
docker exec -i n8n n8n import:workflow --input=workflows/lead-crm-sync.json
```

## Security Best Practices

1. **Never commit `.env` files** — use `.env.example` as a template
2. **Rotate encryption keys** — if `N8N_ENCRYPTION_KEY` changes, all credentials must be re-entered
3. **Use least-privilege API keys** — only grant the scopes each workflow needs
4. **Enable basic auth** in production:
   ```env
   N8N_BASIC_AUTH_ACTIVE=true
   N8N_BASIC_AUTH_USER=admin
   N8N_BASIC_AUTH_PASSWORD=secure-password
   ```
5. **Set up SSL** — use a reverse proxy (Nginx/Caddy) with Let's Encrypt
6. **Restrict webhook access** — use `N8N_ENDPOINT_WEBHOOK` to customize webhook paths

## Troubleshooting

### Common Issues

**Webhooks not receiving data**
- Verify `WEBHOOK_URL` matches your public URL
- Check firewall rules allow inbound traffic on port 5678
- Ensure SSL is configured if using HTTPS

**Credentials not working after restart**
- `N8N_ENCRYPTION_KEY` must remain the same across restarts
- Check that the `n8n_data` volume is persisted

**Queue mode workers not processing**
- Verify Redis is accessible: `redis-cli -h redis ping`
- Check worker logs: `docker compose logs n8n-worker`
- Ensure both main and worker share the same `N8N_ENCRYPTION_KEY`

**Email sending failures (SMTP)**
- Gmail: ensure you're using an App Password, not your account password
- Check port 587 (TLS) or 465 (SSL) is not blocked
- Verify SMTP credentials in n8n's credential manager

**HubSpot API rate limiting**
- Free tier: 100 requests per 10 seconds
- Add delays between batch operations using the Wait node
- Use the `lead-crm-sync.json` workflow's built-in rate limiting

### Logs & Debugging

```bash
# View n8n logs
docker compose logs -f n8n

# View worker logs
docker compose logs -f n8n-worker

# Check execution history
docker exec -i n8n n8n list:workflow

# Enable verbose logging
# Add to .env: N8N_LOG_LEVEL=debug
```

## Updating

```bash
# Pull latest images
docker compose pull

# Restart with new version
docker compose up -d

# Check n8n version
docker exec n8n n8n --version
```

---

For workflow-specific documentation, see the README in each workflow file or the [main README](../README.md).
