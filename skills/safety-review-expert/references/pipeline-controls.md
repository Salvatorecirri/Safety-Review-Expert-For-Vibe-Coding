# Pipeline Controls Checklist

Keep tooling/process guardrails here to avoid bloating app or infra docs.

- **Secret Scans (History + PRs)**: Run gitleaks/trufflehog on full git history weekly and on every PR; fail builds on findings.
- **SAST Baseline**: Semgrep/Sonar/CodeQL on each merge; maintain an allowlisted rule set and block new high/critical issues.
- **DAST Smoke**: Lightweight dynamic scan in staging before release (XSS/SQLi/OWASP top 10) with timeouts to keep pipelines fast.
- **Dependency CVE Scan**: Daily or per-merge SCA (npm audit, pip-audit, osv-scanner); fail on high/critical; auto-create fix PRs.
- **Dependency Authenticity**: Verify packages exist in the official registry; enforce lockfiles and signature/hash verification where supported.
- **Artifact Hygiene**: CI check ensures `.env`, secrets, and sample PII aren’t bundled into images/archives (`npm pack --dry-run`, `docker build --no-cache` + `trivy config`).
- **Release Gates**: Block deploy if secret scan, SAST, SCA, or DAST reports outstanding high/critical issues; require waiver with owner + expiry for overrides.
- **Observability Hooks**: Ensure CI/CD injects release metadata (commit SHA, build id) into logs/traces to trace incidents back to a build.
