# Safety-Review-Expert-Vibe-Coding
## Most vibe-coded apps work perfectly… until they go public. For an engineer with a bit of experience in coding and security paradigm it is possible to see tonnes of vulnerabilities in Vibe-coded apps that have shipped to production.

This repo represents safety checks for Vibe coding snippet to be sure the most common vulnerability are addressed and revised.

Here are 12 of the most common ones.

[1] Missing authentication

- Problem: Endpoints assumed “only the frontend will call this.”
- Fix: Require auth on every non-public route. Deny by default.

[2] Broken authorisation

- Problem: Users could access other users’ data by changing an ID.
- Fix: Enforce ownership checks server-side on every read and write.

[3] Leaky secrets

- Problem: API keys, JWT secrets, and DB creds lived in code or client bundles.
- Fix: Move to a secret manager, rotate, and never ship secrets to the browser.

[4] No rate limiting

- Problem: Unlimited requests enabled brute force, scraping, and bill shock.
- Fix: Rate limit per IP, per user, per token. Add abuse detection.

[5] Wide-open CORS

- Problem: `Access-Control-Allow-Origin: -` plus credentials meant silent data theft.
- Fix: Allowlist origins. Never use wildcard with credentials.

[6] Missing input validation

- Problem: SQLi, NoSQL injection, and weird payload crashes.
- Fix: Validate schema at the boundary. Reject unknown fields.

[7] Unsafe file uploads

- Problem: Anyone could upload anything and you served it back.
- Fix: Restrict types, scan, store outside web root, use signed URLs.

[8] Insecure password handling

- Problem: Plaintext or weak hashing, reused salts, no MFA.
- Fix: Use bcrypt/argon2, strong policies, and MFA for sensitive actions.

[9] Over-permissive tokens

- Problem: Long-lived “god tokens” that worked everywhere.
- Fix: Short-lived tokens, scoped permissions, refresh rotation.

[10] Missing CSRF protections

- Problem: Logged-in users could be tricked into triggering actions.
- Fix: SameSite cookies, CSRF tokens, and avoid cookie auth for APIs.

[11] Weak logging and no alerting

- Problem: Breaches looked like normal traffic until it was too late.
- Fix: Log auth events, admin actions, and anomaly alerts on spikes.

[12] No safe defaults in infra

- Problem: Public S3 buckets, open security groups, exposed DB ports.
- Fix: Private by default, least privilege IAM, IaC scanning in CI.

Vibe coding is fine for prototypes.

But the moment you deploy, you are no longer “building an app.”
You are operating an attack surface.

Ship fast. Secure faster.
