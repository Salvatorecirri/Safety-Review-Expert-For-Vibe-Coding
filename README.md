# Safety Review Expert for Vibe Coding Applications

A collection of production-grade security and reliability skills for Claude Code and other AI agent terminals. Transform "vibe-coded" prototypes into hardened, production-ready applications by automating the discovery of 12 critical vulnerabilities and financial hyper-usage risks

<p align="center">
  <img src="https://img.shields.io/badge/Scripts-security-blue" alt="Scripts Security" />
  <img src="https://img.shields.io/badge/License-MIT-yellow" alt="MIT License" />
  <img src="https://img.shields.io/badge/Focus-Vibe_to_Prod-blueviolet" alt="Focus: Vibe to Prod" />
</p>

## Skills

| Skill | Description | Install |
|-------|-------------|---------|
| [**Safety Review Expert**](./skills/safety-review-expert/) | Senior security auditor hunting for the "Dirty Dozen," race conditions, leakage, infra gaps, and AI billing risks | `npx skills add salvatorecirri/Safety-Review-Expert-Vibe-Coding --path skills/safety-review-expert` |

## Quick Start

Install any skill with:

```bash
npx skills add salvatorecirri/Safety-Review-Expert-Vibe-Coding --path skills/safety-review-expert
```

Then invoke in your agent terminal:

```bash
/safety-review-expert    # Audit current changes for security & cost risks
```

## Development Setup (Python helpers)

If you are contributing new safety modules, set up the validation environment. Some helper scripts (packaging/validation) depend on PyYAML. Set up once per clone:

```bash
python -m venv venv
./venv/Scripts/pip install -r requirements.txt   # Windows
# or
source venv/bin/activate && pip install -r requirements.txt  # macOS/Linux
```

## Customizing your Audit (project-brain.md)

Each project has unique risks. Customize `skills/safety-review-expert/references/project-brain.md` before your first run:

1) **Set Billing Caps**: Define hard-limits for AI API usage to prevent "bill shock."
2) **Define Stack**: Specify your DB (Prisma/Postgres) and Auth (Clerk/NextAuth).
3) **Forbidden Patterns**: Document your specific anti-patterns (e.g., "No inline SQL").
4) **Concurrency Rules**: Set your locking strategy (Optimistic/Pessimistic).
5) **Save it**: To keep Safety reviewers aligned across runs.

## License

MIT
