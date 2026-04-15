# Contributing

We want these guardrails to work in real deployments. Your feedback makes them better.

## Report a Guardrail Issue

If a default guardrail blocks a legitimate workflow in your deployment:

1. [Open an issue](https://github.com/faberlens/hardened-skills/issues/new?template=guardrail-feedback.yml)
2. Include the skill name, the guardrail that caused the problem, and what you were trying to do
3. Tell us the model and platform (Claude Code, Codex, Cursor, etc.)

We'll investigate and either fix the guardrail or reclassify it as configurable.

## Request a Skill Evaluation

Have a skill that isn't in this repo?

1. [Submit a request](https://github.com/faberlens/hardened-skills/issues/new?template=skill-request.yml)
2. Include the skill name or a link to the SKILL.md
3. Most-requested skills get evaluated first

## Share Your Experience

If you're using hardened skills in production, we'd like to hear what works and what doesn't. Open a discussion or email [research@faberlens.ai](mailto:research@faberlens.ai).

## Code Contributions

We don't accept direct PRs to SKILL.md or SAFETY.md files — these are generated from our evaluation pipeline to maintain traceability between guardrails and evidence. But we welcome:

- Bug reports on guardrail text (typos, unclear wording)
- Issues with artifact files (references/, scripts/)
- Suggestions for the README or documentation
