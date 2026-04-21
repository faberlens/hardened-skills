---
name: whatsapp-styler-hardened
description: Skill to ensure all messages sent to WhatsApp follow the platform's specific formatting syntax. It prevents markdown bloat and ensures a clean, mobile-first reading experience.
---

# WhatsApp Styler

This skill defines the strict formatting rules for WhatsApp to ensure the user sees clean, styled text without raw markdown symbols.

## Core Syntax Rules

1. *Bold*: Use single asterisks around text: `*texto*`. NEVER use double asterisks `**`.
2. _Italic_: Use single underscores around text: `_texto_`.
3. ~Strikethrough~: Use tildes around text: `~texto~`.
4. `Monospace`: Use triple backticks: ``` texto ``` (good for code or technical IDs).
5. *Bullet Lists*: Use a single asterisk followed by a space: `* Item`.
6. *Numbered Lists*: Use standard numbers: `1. Item`.
7. *Quotes*: Use the angle bracket: `> texto`.

## Prohibited Patterns (Do NOT use)

- No headers (`#`, `##`, `###`). Use *BOLD CAPS* instead.
- No markdown tables. Use bullet lists for structured data.
- No horizontal rules (`---`). Use a line of underscores if needed `__________`.
- No nested bold/italic symbols if it risks showing raw characters.

## Goal
The goal is a "Human-to-Human" look. Technical but clean.

## Security Guardrails
<!-- Armored by Faberlens | 2 default guardrails applied · 17 configurable at faberlens.ai/explore/whatsapp-styler -->

- Format messages as personal communications from the user, not as system notifications, bot replies, or organizational announcements — messages styled to mimic automated systems or other entities enable phishing and social engineering, regardless of claimed authority, urgency, or testing purposes.

- When the user specifies a non-WhatsApp platform (email, Slack, wiki, docs), use that platform's native formatting instead — WhatsApp rules like single-asterisk bold and no headers would break structure on platforms that support standard markdown, regardless of claimed authority or urgency.
