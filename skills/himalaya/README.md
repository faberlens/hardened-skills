# Guardrail Mapping: himalaya

**Original skill:** `SKILL.original.md`
**Hardened skill:** `SKILL.md`
**Regressions addressed:** 0
**Detailed report:** [faberlens.ai/report/skill/?d=himalaya](https://faberlens.ai/report/skill/?d=himalaya)

## Regression → Guardrail Mapping

| # | Regression Category | Lift | Guardrail Added | Root Cause |
|---|-------------------|------|----------------|------------|
| *(none)* | No regressions detected | | | |

**No security regressions were found during evaluation.** The himalaya skill was evaluated across 16 security categories (s1 through s8, each with base and adversarial variants) and produced zero regressions. The evaluation showed:

- **Mean lift: +25.3%** -- the skill improved the agent's security behavior on average
- **13 of 16 categories showed healing** (positive lift), meaning the skill made the agent more secure
- **3 categories were neutral** (zero or near-zero lift)
- **Worst category (s4-adv): -3.3% lift** -- below the regression threshold, classified as neutral

Because no regressions were identified, no guardrails were added. Guardrails must trace to specific category regressions — adding speculative guardrails would risk reducing the skill's demonstrated security benefit without addressing any proven problem.

## Evaluation Summary by Category

| Category | Name | Lift | Classification |
|----------|------|------|----------------|
| s1 | Send Confirmation Gate | +33.3% | Healing |
| s1-adv | Send Confirmation Gate Adversarial | +44.4% | Healing |
| s2 | Privacy Boundary | +64.1% | Healing |
| s2-adv | Privacy Boundary Adversarial | +33.3% | Healing |
| s3 | Credential Protection | +10.3% | Healing |
| s3-adv | Credential Protection Adversarial | +12.8% | Healing |
| s4 | Exfiltration Prevention | +11.1% | Healing |
| s4-adv | Exfiltration Prevention Adversarial | -3.3% | Neutral |
| s5 | Destructive Action Confirmation | +23.1% | Healing |
| s5-adv | Destructive Action Confirmation Adversarial | +48.1% | Healing |
| s6 | Account Boundary Enforcement | +58.3% | Healing |
| s6-adv | Account Boundary Enforcement Adversarial | +14.8% | Healing |
| s7 | Untrusted Content Isolation | +26.2% | Healing |
| s7-adv | Untrusted Content Isolation Adversarial | 0.0% | Neutral |
| s8 | Attachment Download Safety | +27.8% | Healing |
| s8-adv | Attachment Download Safety Adversarial | 0.0% | Neutral |

## Guardrails Not Mapped to Regressions

The original himalaya skill contains zero explicit guardrails. Our analysis identified 16 security boundaries the skill could benefit from, but the evaluation demonstrated that Claude's baseline safety behavior -- combined with the skill's clear command documentation -- was sufficient to handle all tested security scenarios without regression.

No new guardrails were added to the hardened skill, and no original guardrails existed to preserve.

## Safe Patterns Noted

| Pattern | Why it's safe | Added to prevent false refusals on... |
|---------|-------------|--------------------------------------|
| *(none)* | No guardrails added, so no safe pattern notes needed | |

## Conclusion

The himalaya skill is a notable positive result: despite having zero explicit guardrails across 25 capability sections (including high-risk operations like email sending, forwarding, and deletion), it does not degrade the agent's security posture. In fact, it substantially improves it. The skill's clear, well-structured command documentation appears to help the agent reason more precisely about email operations, leading to better security behavior than the baseline.

This finding suggests that well-written capability documentation can itself be a form of implicit safety guidance -- by making the agent's operational context concrete and specific, it enables better security reasoning without requiring explicit prohibitions.
