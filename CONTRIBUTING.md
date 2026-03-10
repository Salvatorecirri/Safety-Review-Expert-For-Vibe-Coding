# Contributing to Safety-Review-Expert-Vibe-Coding

First off, thank you for helping make "Vibe-Coded" apps safer for everyone! To maintain the high security standards of this repository, we follow a strict contribution process.

## Our Philosophy

1. **Ship Fast, Secure Faster**: We value speed, but never at the expense of an attack surface.
2. **The Senior Lens**: Every skill must provide the context of *why* a vulnerability exists in a rapid-prototyping environment.
3. **Safety First**: We prioritize protections against "Bill Shock" and resource exhaustion, avoiding the possibility, for malicious individuals to steal API keys.

## How to Contribute

### 1. Fork and Clone

- Fork the repository to your own GitHub account.
- Clone your fork locally:
  ```bash
  git clone [https://github.com/your-username/Safety-Review-Expert-Vibe-Coding.git](https://github.com/your-username/Safety-Review-Expert-Vibe-Coding.git)
  ```

### 2. Create a New Skill or Reference

If adding a new checklist, place it in `skills/safety-review-expert/references/`.

If creating a new standalone skill, follow the directory structure:
- `skills/<name>/README.md`
- `skills/<name>/SKILL.md`
- `skills/<name>/agents/agent.yaml`

### 3. Running Local Validation

Before submitting, ensure your files are syntactically correct:

```bash
python -m compileall skills/
```

## Submission Process (The "Salvatore" Gate)

1. **Push to your fork**: Push your changes to a descriptive branch (e.g., `feat/add-graphql-security-check`).
2. **Open a Pull Request (PR)**: Target the `main` branch of `salvatorecirri/Safety-Review-Expert-Vibe-Coding`.
3. **Code Review**: @salvatorecirri will review all logic. We look for:
- Clear "Vibe Trap" explanations.
- Actionable "Fix" steps.
- Proper P0-P3 categorization.
4. **Approval**: Once approved and merged, your contribution will be available to everyone via the `npx skills add` command.

## Guidelines for Security Checklists

When writing new security checks, please use the following template for consistency:
- **The Vibe**: Describe the "lazy" or "fast" assumption a developer usually makes.
- **The Risk**: Explain the technical exploit or financial impact.
- **The Fix**: Provide a concrete, production-grade code correction.

-------

**Questions?** Open an Issue in the repository. Happy securing!