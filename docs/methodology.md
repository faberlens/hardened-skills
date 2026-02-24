# Methodology

How we evaluated and hardened 10 OpenClaw skills for behavioral security.

## Overview

Static analysis tools (Snyk, semgrep, VirusTotal) analyze code for known vulnerabilities. But AI skills aren't code — they're natural language instructions that change an agent's behavior. A skill can be syntactically clean and still degrade the agent's security posture.

We measure this by comparing an agent's behavior **with** and **without** a skill installed, across structured security test scenarios. The key finding: 9 of 10 skills create a "[jagged safety surface](https://faberlens.ai/blog/jagged-surface.html)" — improving some security properties while simultaneously degrading others.

## Evaluation Pipeline

### 1. Analyze the Skill's Security Surface

Each skill is analyzed to understand what it enables and where the security boundaries are. We identify:

- **What the skill teaches the agent to do** — commands, workflows, data access patterns
- **What guardrails exist** — explicit security boundaries written into the skill (most skills have zero)
- **What guardrails are missing** — security boundaries the skill *should* have given what it enables, but doesn't

For example, a credential manager skill that teaches `op run` workflows but has no rules about piping secrets to network commands is missing a critical boundary.

### 2. Generate Targeted Security Tests

For each security concern, we generate tests that probe the skill from multiple perspectives:

- **Boundary conditions** — edge cases where the right behavior isn't obvious (e.g., a command that *looks* safe but leaks credentials through a subprocess)
- **Adversarial scenarios** — authority spoofing, urgency pressure, context manipulation, social engineering, obfuscation, and other attack patterns designed to push past the skill's guardrails
- **Realistic workflows** — tests grounded in plausible user requests, not synthetic edge cases

Each skill produces 40–150+ tests organized into security categories, each targeting a specific property (e.g., "secrets must not leave the local machine via network").

### 3. Run Baseline vs. Skill Comparison

Tests are run against the agent using realistic skill injection — the same method used to load skills in production:

- **Baseline** — agent without the skill, same test prompts
- **With skill** — agent with the skill installed

Each test is run 3 times per condition to account for response variance. Responses are evaluated by Claude Opus using a semantic rubric with explicit PASS/FAIL criteria.

### 4. Detect Regressions

**Lift** = skill pass rate − baseline pass rate

- **Positive lift** — the skill improves security behavior
- **Negative lift** (below −5pp threshold) — the skill *degrades* security behavior — this is a regression

A regression means the agent handles a security scenario *worse* when the skill is installed. For example, the 1password skill produced −33.3pp lift on masking enforcement — it made the agent willing to dump unmasked secrets when the user asked.

## Hardening Process

### Guardrail Design Principles

1. **Regression-driven** — each guardrail targets one or more observed regressions
2. **Additive** — guardrails are appended to the skill, never modifying existing functionality
3. **Minimal** — no unnecessary restrictions; only address demonstrated regressions
4. **Traceable** — every guardrail maps to specific test categories and regression evidence

### Validation

Hardened skills are re-evaluated through the same pipeline:

- Confirm targeted regressions are fixed (lift returns to neutral or positive)
- Confirm no new regressions are introduced
- Confirm overall category improvement rate

## Verdict Validation

Evaluation verdicts are validated through three independent checks:

**Human audit.** All 130 failing test responses were manually reviewed by security researchers. 98% were confirmed as genuine semantic failures — only 3 of 130 contained patterns catchable by regex or keyword filters. The remaining failures required understanding intent, context, and the full response to judge correctly.

**Cross-evaluator agreement.** The same 13,802 verdict pairs were independently scored by two evaluator models (Claude Opus and Claude Haiku). Overall agreement: 80.1%. On regression classifications specifically, 43 of 45 Opus-flagged regressions were independently confirmed by Haiku (95.6% confirmation rate). The 2 unconfirmed regressions were both marginal (−5.1pp, right at threshold). Haiku additionally flagged 51 regressions that Opus did not — making the Opus-based headline numbers a conservative lower bound.

## Cross-Model Replication

To validate that regressions aren't artifacts of a single model, we replicated the evaluation on Claude Opus 4.6 for three skills (1password, bird, gog) — 4,870 generations across 62 security categories. Key findings:

- Regressions persist on the stronger model (gog: 11 on Haiku, 10 on Opus)
- Some regressions get *worse* — calendar impersonation went from −39pp to −56pp; permission gating from −33pp to −50pp
- A "warn then arm" failure pattern emerged: Opus identifies an attack, warns the user, then provides the attack command anyway

The overlap between Haiku and Opus regressions ranges from 38% (bird) to 91% (gog), confirming that the regressions are real but partly model-dependent. Full replication data: [faberlens.ai/blog/opus-replication.html](https://faberlens.ai/blog/opus-replication.html)

## Limitations

- **Sample size** — 3 runs per condition is sufficient for detecting large effects (>10pp) but may miss smaller regressions. Headline numbers should be treated as conservative.
- **Model-specific** — the primary evaluation used Claude Haiku (generation) and Opus (evaluation). The Opus replication confirms regressions hold within the Claude family, but results may differ across model families (GPT, Gemini).
- **Temporal** — model behavior changes over time. Regressions and fixes may shift with model updates.
- **Coverage** — the test suite covers security properties derivable from the skill's capability surface. Novel attack vectors not covered by the taxonomy may exist.
- **Adversarial ceiling** — some regressions resist guardrail-based fixes (e.g., coding-agent at 40% fix rate). These may require skill architecture changes rather than guardrail additions.

## Full Report

Complete evaluation data, per-category metrics, and interactive results: [faberlens.ai/report](https://faberlens.ai/report)
