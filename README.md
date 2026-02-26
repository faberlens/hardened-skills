# Hardened Skills

Drop-in replacements for 10 [OpenClaw](https://github.com/openclaw/openclaw) AI skills, patched to fix behavioral security regressions discovered by [Faberlens](https://faberlens.ai).

## Why This Exists

AI skills (SKILL.md files) can pass every static scanner — Snyk, VirusTotal, semgrep — and still make your agent *less secure*. A clean, well-written skill can teach an agent to leak secrets, bypass permission gates, or ignore safety boundaries.

We evaluated 10 OpenClaw skills by running them through a behavioral safety pipeline: testing security behavior **with** and **without** each skill installed, across 186 security test categories.

**The main finding:** 9 of 10 skills reshape the model's safety surface — improving some properties while degrading others. This "[jagged safety surface](https://faberlens.ai/blog/jagged-surface.html)" is invisible to static analysis. You have to run the composition and test the behavior to find it.

- **45 security regressions** found across 9 of 10 skills
- **37 fixed** with targeted guardrail additions (82% fix rate)
- **93% of test categories improved** after hardening
- **5 skills achieved 100% regression fix rate**
- **Validated across models** — a [cross-model replication](https://faberlens.ai/blog/opus-replication.html) on Claude Opus 4.6 (4,870 generations) confirmed regressions persist on stronger models — and some get worse

## Skills

| Skill | Description | Regressions Found | Fixed | Guardrails Added |
|---|---|---|---|---|
| [1password-hardened](skills/1password-hardened/) | 1Password CLI integration | 6 | 6 (100%) | 9 |
| [bird-hardened](skills/bird-hardened/) | X/Twitter CLI | 5 | 4 (80%) | 6 |
| [coding-agent-hardened](skills/coding-agent-hardened/) | Code generation assistant | 5 | 2 (40%) | 5 |
| [eightctl-hardened](skills/eightctl-hardened/) | Eight Sleep pod controller | 5 | 5 (100%) | 8 |
| [food-order-hardened](skills/food-order-hardened/) | Food ordering assistant | 3 | 3 (100%) | 3 |
| [gog-hardened](skills/gog-hardened/) | Google Workspace CLI | 11 | 8 (73%) | 11 |
| [himalaya-hardened](skills/himalaya-hardened/) | Email client (CLI) | 0 | 0 — clean baseline | 0 |
| [notion-hardened](skills/notion-hardened/) | Notion workspace integration | 5 | 5 (100%) | 5 |
| [peekaboo-hardened](skills/peekaboo-hardened/) | macOS screenshot tool | 2 | 1 (50%) | 7 |
| [wacli-hardened](skills/wacli-hardened/) | WhatsApp CLI client | 3 | 3 (100%) | 6 |

## Usage

Each hardened skill is a **drop-in replacement** for the original. Swap the SKILL.md file and you're done.

### Install a hardened skill

```bash
# Replace the original with the hardened version
cp skills/1password-hardened/SKILL.md ~/.claude/skills/1password-hardened/SKILL.md
```

### Compare with the original

Each skill directory contains:

- `SKILL.md` — Hardened version (use this)
- `SKILL.original.md` — Original OpenClaw version (for reference)
- `README.md` — Documents which regressions each guardrail addresses, with before/after examples

```bash
# See what changed
diff skills/1password-hardened/SKILL.original.md skills/1password-hardened/SKILL.md
```

## Report & Data

- Full report: [faberlens.ai/report](https://faberlens.ai/report)
- Methodology: [docs/methodology.md](docs/methodology.md)

## Credits

- Original skills by the [OpenClaw](https://github.com/openclaw/openclaw) community
- Behavioral evaluation by [Faberlens](https://faberlens.ai)

## License

MIT — same as the original OpenClaw skills.
