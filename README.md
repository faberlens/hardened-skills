# Hardened Skills

200 AI agent skills evaluated for behavioral safety. Each one passes every static scanner. 87% create silent security regressions when loaded.

We found the regressions. We fixed 87% of them. These are the hardened versions.

## What's Here

Each skill folder contains:

- **SKILL.md** — Drop-in replacement for the original skill. Default guardrails are already applied. Works on Claude Code, Codex, Cursor, Windsurf, and any agent platform that loads markdown skills.
- **README.md** — Full safety evaluation: what we found, the test prompts that exposed it, before/after proof of the fix, and the evaluator's reasoning.

## Default vs Configurable Guardrails

**Default guardrails** (in SKILL.md) are universally safe — no trade-off, no capability loss. Never pipe secrets to network commands. Always confirm before destructive operations. These apply in every deployment context.

**Configurable guardrails** (in each skill's README.md) address real vulnerabilities but involve a capability trade-off that depends on your deployment. Browse the full evidence and choose which ones to apply at [faberlens.ai/explore](https://faberlens.ai/explore).

## The Numbers

| Metric | Value |
|--------|-------|
| Skills evaluated | 200 |
| Security concepts discovered | 3,838 |
| Concept directions explored | 72,372 |
| Regressions found | 739 |
| Fix rate | 87% |
| Targeted guardrails written | 2,750 |

*These are evaluation-wide metrics. Each skill's individual stats are in its README.md.*

**Any other platform:**
Copy the SKILL.md into your agent's skill configuration. It's a plain markdown file.

## Get Involved

- **Found a guardrail that blocks a legitimate workflow?** [Open an issue](https://github.com/faberlens/hardened-skills/issues/new?template=guardrail-feedback.yml)
- **Want a skill evaluated?** [Request it](https://github.com/faberlens/hardened-skills/issues/new?template=skill-request.yml)
- **Browse the evidence:** [faberlens.ai/explore](https://faberlens.ai/explore) — per-skill scorecards, per-concept pass rates, before/after proofs, configurable guardrails

## How We Evaluate

Every evaluation is derived from what the skill does — not from a library of known attacks. For each skill, we:

1. Discover the security concepts unique to that skill's capabilities
2. Explore every behavioral scenario (concept direction) we can derive
3. Measure the agent's safety with and without the skill loaded
4. Write targeted guardrails traced to specific failure mechanisms
5. Re-evaluate to verify the fix

## Research

- [200 Skills Pass Every Scanner. 87% Still Break Safety.](https://faberlens.ai/blog/skill-safety-problem)
- [What You Can't Measure, You Can't Fix](https://faberlens.ai/blog/what-you-cant-measure)
- [The Jagged Safety Surface (10 skills)](https://faberlens.ai/blog/jagged-surface)
- [Case Studies](https://faberlens.ai/blog/case-studies)
- [Cross-Model Replication](https://faberlens.ai/blog/opus-replication)

## License

MIT. See [LICENSE](LICENSE).

---

Built by [Faberlens](https://faberlens.ai). Behavioral safety evaluation for AI agent skills.
