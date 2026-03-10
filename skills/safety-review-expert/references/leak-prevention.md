# Leak Prevention Checklist

Use this to keep secrets and PII out of source, builds, logs, and responses without bloating the Dirty Dozen list.

## App-Layer Controls
- **PII Discovery**: Scan fixtures, seeds, test data, and logs for real names/emails/SSNs; replace with synthetic data.
- **Secret Scanning (History + Working Tree)**: Run gitleaks/trufflehog across the full git history and block CI on findings.
- **Artifact Hygiene**: Ensure `.env`, secret configs, and sample data stay out of Docker images/bundles (`.dockerignore`, `npm pack --dry-run` checks).
- **Client Bundle Check**: Confirm no `NEXT_PUBLIC_`/client-exposed env vars contain keys or secrets.
- **Logging Redaction**: Allowlist log fields; mask tokens, passwords, auth headers, PANs/PII before writing to stdout/files.
- **API Response Whitelists**: Serialize only required fields (DTOs/`select` clauses); never return `password_hash`, tokens, internal notes.
- **Error Exposure**: Show generic user errors; keep stack traces server-side only and scrub sensitive values from exception logging.
