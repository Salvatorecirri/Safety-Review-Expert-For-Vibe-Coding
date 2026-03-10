# Guardrails for Catastrophic Usage & Billing

Use this checklist to identify "Hyper-usage" risks where a hacker or a logic bug could cause massive API charges (e.g., Gemini, OpenAI, Anthropic).

## [1] Client-Side Keys (The "Immediate P0")
- **The Risk**: API keys found in `client-side` code, `NEXT_PUBLIC_` variables, or `.env` files that get bundled into the browser.
- **The Fix**: All AI API calls must be proxied through a secure backend. No keys in the browser.

## [2] Missing Request Quotas
- **The Risk**: An endpoint like `/api/generate-content` that calls an LLM but has no per-user daily limit.
- **The Fix**: Implement a hard quota (e.g., 50 requests/day per user) stored in Redis or a database.

## [3] Unbounded LLM Loops
- **The Risk**: Code that calls an AI API inside a `while` loop or recursive function without a "Max Iterations" safety break.
- **The Fix**: Always use a `MAX_STEPS = 5` constant and throw an error if exceeded.

## [4] Prompt Injection leading to Resource Exhaustion
- **The Risk**: A user prompt that forces the model into a "Infinite Token" generation (e.g., "Repeat the word 'hello' forever").
- **The Fix**: Set `max_output_tokens` or `max_tokens` on every single API call. Never leave it uncapped.

## [5] Missing Cost-Aware Circuit Breakers
- **The Risk**: A spike in usage that continues until the monthly billing limit is hit.
- **The Fix**: Implement a middleware that tracks global hourly spend. If spend > $X, temporarily disable the AI features and alert the admin.

## [6] Stolen Key "Hyper-Usage"
- **The Risk**: A key is leaked and used by a botnet to train their own models on your dime.
- **The Fix**: Restrict API keys to specific IP addresses or Referrers in the Google/AWS/OpenAI console. Add a "Kill Switch" environment variable to the backend.

## [7] Heavy Payload Denial of Service
- **The Risk**: Allowing users to upload massive PDFs or images for AI "summarization" without size limits, leading to high processing costs.
- **The Fix**: Limit file uploads to <5MB and check token counts before sending to the model.

## [8] Extreme Usage Freeze & Confirmation
- **The Risk**: Spend spikes >5-10x normal (e.g., >500% in one hour) draining budget before alerts are handled.
- **The Fix**: Auto-freeze AI features when the threshold is hit and require a manual `ADMIN_UNLOCK`/runbook approval to resume; optionally prompt for confirmation on large step-changes before continuing.
