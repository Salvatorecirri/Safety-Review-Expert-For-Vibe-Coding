# Safety Review Expert

A comprehensive security and reliability skill for AI agents. Performs structured reviews with a senior security engineer lens, covering "vibe-coding" pitfalls, financial protection, and data integrity.

## Installation

```bash
npx skills add salvatorecirri/Safety-Review-Expert-Vibe-Coding --path skills/safety-review-expert
```

## Features

- **The Dirty Dozen** - Detect missing auth, IDOR, leaky secrets, and wide-open CORS
- **Hyper-Usage Guard** - Identify missing budget caps and rate limits to prevent "bill shock"
- **Race Condition Hunter** - Detect TOCTOU (Check-Then-Act) bugs and non-atomic DB operations
- **Security Deep-Dive** - Scan for XSS, Injection, SSRF, and JWT algorithm confusion
- **Data Integrity** - Audit for missing transactions, idempotency gaps, and partial write risks
- **Vibe-marker Detection** - Flag hardcoded keys and "temporary" hacks left in code

## Usage

After installation, simply run:

```
/safety-review-expert
```

The skill will automatically review your current git changes or the specified directory.

## Workflow

1. **Preflight** - Scope changes via git diff and identify "vibe-markers."

2. **Vulnerability Scan** - Run the "Dirty Dozen" check for core security gaps.

3. **Financial Protection** - Audit AI API calls for billing and hyper-usage risks.

4. **Reliability Dive** - Analyze concurrency, race conditions, and data integrity.

5. **Output** - Findings by severity (P0-P3) with "The Vibe Trap" context.

6. **Confirmation** - Ask user before implementing security fixes or guardrails.

## Severity Levels

| Level | Name | Action |
|-------|------|--------|
| P0 | Critical | Potential exploit, leaked key, or catastrophic cost risk. Must block merge |
| P1 | High | Probable security risk or data corruption. Should fix before merge |
| P2 | Medium | Best practice violation (headers, logging). Fix or follow-up |
| P3 | Low | Optional improvement or minor code smell |

## Structure

```
safety-review-expert/
|-- SKILL.md                 # Main skill definition
|-- README.md                # Technical documentation for the specific auditor tool
|-- agents/
|   \-- agent.yaml           # Agent interface config
\-- references/
    |-- vibe-check-12.md     # The 12 core vulnerabilities
    |-- catastrophic-usage.md # Billing & hyper-usage guardrails
    |-- race-conditions.md   # Concurrency & TOCTOU patterns
    |-- security-checklist.md # Deep-dive security & reliability
    |-- data-integrity.md    # Transactions & idempotency
    \-- project-brain.md     # Custom stack & org rules
```

## References

Each checklist provides detailed prompts and anti-patterns:

- **vibe-check-12.md** - Core OWASP risks and rapid-prototyping errors.
- **catastrophic-usage.md** - LLM loops, budget caps, and API credit draining.
- **race-conditions.md** - Concurrency, database locking, and atomic state.
- **security-checklist.md** - JWT confusion, XSS, SSRF, and crypto gaps.
- **data-integrity.md** - Transactional safety and idempotency logic.

## License

MIT
