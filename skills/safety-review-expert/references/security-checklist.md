# Deep-Dive Security & Reliability Checklist

This reference covers technical implementation flaws in Input/Output, Authentication, and Token management that often bypass simple "vibe" checks.

## [1] Input & Output Safety
- **XSS (Cross-Site Scripting)**: 
    - **Check**: Use of `dangerouslySetInnerHTML`, `innerHTML`, or unescaped template literals in the frontend.
    - **Indicator**: `const html = ` + user_input`.
- **Injection (SQL/NoSQL/Command)**:
    - **Check**: String concatenation in database queries or `exec()` calls.
    - **Fix**: Use parameterized queries or ORM/Query Builders (Prisma, Drizzle, Mongoose).
- **SSRF (Server-Side Request Forgery)**:
    - **Check**: `fetch(user_url)` or `axios.get(user_url)` where the URL is controlled by the user.
    - **Risk**: Attackers can scan your internal network or access metadata services (e.g., `169.254.169.254`).
- **Path Traversal**:
    - **Check**: `fs.readFile('/path/' + user_input)`.
    - **Fix**: Sanitize paths and use an allowlist of directories.



## [2] JWT & Token Security
- **Algorithm Confusion**: 
    - **Check**: Does the verification logic explicitly check for the expected algorithm (e.g., `RS256`)?
    - **Flag**: Accepting `alg: "none"`.
- **Sensitive Payloads**:
    - **Check**: Storing PII (emails, roles, internal IDs) in the JWT without encryption.
    - **Note**: JWTs are base64-encoded, not encrypted. Anyone with the token can read the contents.
- **Validation**:
    - **Check**: Missing `iss` (issuer), `aud` (audience), or `exp` (expiration) validation.

## [3] Supply Chain & Dependencies
- **Unpinned Versions**: 
    - **Check**: Using `^` or `*` for critical security dependencies in `package.json`.
- **Untrusted Sources**:
    - **Check**: Importing scripts from third-party CDNs without Subresource Integrity (SRI) hashes.
- **Outdated Packages**:
    - **Check**: Scan for packages with known CVEs (e.g., older versions of `lodash` or `express`).

## [4] CORS & Security Headers
- **Permissive CORS**:
    - **Check**: `Access-Control-Allow-Origin: *` paired with `Access-Control-Allow-Credentials: true`.
- **Missing Headers**:
    - **Check**: Absence of `Content-Security-Policy` (CSP), `X-Frame-Options` (to prevent Clickjacking), and `X-Content-Type-Options`.

## [5] Cryptography
- **Weak Algorithms**:
    - **Check**: Use of `MD5` or `SHA1` for hashing passwords or sensitive data.
- **Hardcoded Entropy**:
    - **Check**: Hardcoded salts, Initialization Vectors (IVs), or static keys.
- **Mode of Operation**: 
    - **Flag**: Use of `AES-ECB` mode (identical blocks produce identical ciphertext).
    - **Fix**: Use `AES-GCM` or `AES-CBC` with unique IVs.



## [6] Prototype Pollution (JS Specific)
- **Check**: Unsafe merging of objects using `Object.assign` or `...spread` with user-controlled JSON keys.
- **Indicator**: `const config = { ...default, ...req.body };`
- **Fix**: Use `Map` or `Object.create(null)` for user-data storage.