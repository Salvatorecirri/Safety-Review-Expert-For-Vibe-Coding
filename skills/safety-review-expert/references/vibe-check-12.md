# The Dirty Dozen: Vibe-Coding Vulnerability Checklist

Use this checklist to identify where "speed-to-market" has created an attack surface.

## [1] Missing Authentication
- **The Vibe**: "I'll just test the API; I'll add the middleware later."
- **Check**: Look for routes/endpoints without a `requireAuth` or similar wrapper.
- **Fix**: Require auth on every non-public route. Deny by default.

## [2] Broken Authorization (IDOR)
- **The Vibe**: "The frontend only sends the current user's ID anyway."
- **Check**: Find endpoints like `/api/user/:id/data`. Does the backend verify that `:id` matches the session user?
- **Fix**: Enforce server-side ownership checks on every read/write.

## [3] Leaky Secrets
- **The Vibe**: `const API_KEY = "sk-..."` hardcoded for a quick test.
- **Check**: Scan for strings starting with `sk-`, `AI...`, `ghp_`, or variables in `process.env` that are bundled into the frontend (e.g., `NEXT_PUBLIC_`).
- **Fix**: Move to a secret manager; use `.env` only on server-side.

## [4] No Rate Limiting
- **The Vibe**: "I don't have enough users for this to matter yet."
- **Check**: Missing `express-rate-limit`, `bottleneck`, or Redis-based throttling.
- **Fix**: Apply rate limits per IP/User to prevent brute force and scraping.
- **Auth Endpoints**: Apply ultra-strict limits (e.g., 5 attempts/15 mins) on `/login`, `/register`, and `/password-reset`; global limits are insufficient to stop credential stuffing.

## [5] Wide-Open CORS
- **The Vibe**: `res.setHeader('Access-Control-Allow-Origin', '*')` to fix local dev errors.
- **Check**: Search for wildcard origins in CORS config, especially if `Allow-Credentials` is true.
- **Fix**: Use an explicit allowlist of production domains.

## [6] Missing Input Validation
- **The Vibe**: "The frontend form handles the validation."
- **Check**: Lack of schema validation (Zod, Joi, Yup) at the boundary. Look for direct use of `req.body` in DB queries.
- **Fix**: Reject unknown fields; validate types and lengths server-side.

## [7] Unsafe File Uploads
- **The Vibe**: "I just need to let them upload a profile pic."
- **Check**: Direct saving of `file.originalname` or serving files from the web root without sanitization.
- **Fix**: Restrict MIME types; use signed URLs; store in isolated S3/Cloud Storage.

## [8] Insecure Password Handling
- **The Vibe**: "I'll just MD5 it for now."
- **Check**: Use of weak hashing or missing MFA for sensitive actions.
- **Fix**: Use `bcrypt` or `argon2`.

## [9] Over-Permissive Tokens
- **The Vibe**: "The JWT never expires so the user never has to log in again."
- **Check**: JWTs with no `exp` claim or very long lifetimes (e.g., 1 year).
- **Fix**: Short-lived access tokens + refresh rotation.

## [10] Missing CSRF Protection
- **The Vibe**: "APIs don't need CSRF."
- **Check**: Using cookie-based auth without `SameSite: Strict/Lax` or anti-CSRF tokens.
- **Fix**: Use header-based tokens (Bearers) or strict cookie policies.

## [11] Weak Logging & No Alerting
- **The Vibe**: `console.log("Error occurred")`.
- **Check**: Absence of structured logging (Pino, Winston) or failure to log auth events.
- **Fix**: Log critical actions (login, delete, update) with context; set up anomaly alerts.

## [12] No Safe Defaults in Infra
- **The Vibe**: "I'll just open port 5432 so I can debug the DB from my house."
- **Check**: Publicly accessible DBs, open S3 buckets, or wide-open Security Groups.
- **Fix**: Least privilege IAM; everything private by default.
