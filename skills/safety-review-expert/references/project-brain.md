# Project Brain: [Project Name]

This document aligns the Safety Review Expert with your specific technical stack and organizational policies. **Fill this out before running your first review.**

## Technical Stack
- **Backend Framework**: (e.g., Node/Fastify, Python/FastAPI, Go)
- **Primary Database**: (e.g., PostgreSQL via Prisma, MongoDB via Mongoose)
- **Authentication**: (e.g., NextAuth, Clerk, Firebase, Custom JWT)
- **Infrastructure**: (e.g., AWS, Vercel, Railway, Docker)
- **AI/LLM Provider**: (e.g., Gemini 1.5 Pro, OpenAI GPT-4o)

## Security Boundaries
- **Forbidden Libraries**: (e.g., Do not use `crypto-js`, use native Web Crypto API)
- **Secrets Management**: (e.g., All secrets must come from AWS Secrets Manager, never `.env.local`)
- **CORS Policy**: (e.g., Only allow `*.myapp.com`)
- **Rate Limit Defaults**: (e.g., 100 requests / 15 mins per IP)

## Hyper-Usage / Billing Limits
- **Per-User Credit Limit**: (e.g., Max 50 LLM calls per 24h)
- **Hard Kill-Switch**: (e.g., If `process.env.MAINTENANCE_MODE` is true, return 503 for all AI routes)
- **Budget Alerts**: (e.g., Notify Slack if monthly AI spend exceeds $50)

## Concurrency & Data Integrity
- **Locking Strategy**: (e.g., Use Optimistic Locking on the `users` table via the `version` column)
- **Idempotency**: (e.g., All `/payments` or `/ai-generate` routes must include an `x-idempotency-key` header)

## Error Handling Patterns
- **Base Error Class**: (e.g., All errors must extend `AppError`)
- **Logging**: (e.g., Use `pino` for structured logs; never log `req.headers.authorization`)
- **User-Facing Errors**: (e.g., Never return stack traces; use generic error codes like `ERR_INSUFFICIENT_FUNDS`)

## Test Strategy
- **Coverage Target**: (e.g., 100% coverage for auth and billing logic)
- **Security Tests**: (e.g., Every new endpoint must have a corresponding "Unauthorized Access" test case)