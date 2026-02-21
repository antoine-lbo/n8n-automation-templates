# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in any workflow template, please report it responsibly.

**Email:** security@syncta.ai

**Response Timeline:**
- Acknowledgment within 48 hours
- Assessment within 5 business days
- Fix or mitigation within 30 days for confirmed issues

## Security Considerations for n8n Workflows

### Credential Management
- **Never** hardcode API keys, tokens, or passwords in workflow JSON files
- Use n8n credential management for all sensitive values
- All templates use placeholder credentials that must be replaced
- Our CI automatically scans for credential leaks before merge

### Workflow Security
- All webhook triggers should be secured with authentication
- HTTP Request nodes should use HTTPS exclusively
- Input data should be validated before processing
- Error handling should not expose sensitive information

### Data Privacy
- Workflows that process PII should implement data minimization
- Logging should be configured to exclude sensitive fields
- Data retention policies should be documented per workflow

## Automated Security Checks

Our CI pipeline includes:
- JSON structure validation for all workflow files
- Credential leak detection (API keys, tokens, secrets)
- Pattern matching for common secret formats (AWS, Stripe, Slack, GitHub)

## Best Practices for Contributors

1. Use environment variables or n8n credentials for all secrets
2. Test workflows with sanitized/mock data before submitting
3. Document any required permissions or scopes
4. Include error handling in all workflow templates
5. Review webhook configurations for proper authentication

## Disclosure Policy

We follow responsible disclosure. Please do not open public issues for security vulnerabilities.
