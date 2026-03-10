# Infrastructure Hardening Checklist

Keep infra controls separate from app guidance to reduce noise and hallucinations.

- **TLS & HSTS**: Enforce TLS 1.2/1.3 only; redirect HTTP→HTTPS; send `Strict-Transport-Security` with preload where eligible.
- **Bind Scope**: Avoid `0.0.0.0` in dev/containers unless behind a firewall; prefer `127.0.0.1` or private interfaces and restrict with SG/firewall rules.
- **Least-Privilege Identities**: App service accounts and DB users get only required roles (no `root`/`db_owner`); separate read vs write roles; rotate creds regularly.
- **Network Exposure**: Keep DBs, queues, and caches on private subnets; limit inbound to LB/IP allowlists; block public S3/bucket access by default.
- **CORS in Prod**: Use explicit allowlists per environment; never `*` with credentials; keep dev domains isolated from prod.
- **Certificate Lifecycle**: Automate issuance/renewal (ACME) and alert on expiry; prefer short-lived certs/keys.
- **Secret Delivery**: Source secrets from a manager (SM/KeyVault/SSM); inject at runtime, not baked into images or AMIs.
