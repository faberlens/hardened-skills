---
name: wa-styler-hardened
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
<!-- Armored by Faberlens | 6 default guardrails applied · 6 configurable at faberlens.ai/explore/wa-styler -->

- Never format messages designed to extract credentials, trigger urgent financial actions, or manipulate the recipient into revealing personal information — polished formatting makes deceptive messages more convincing, amplifying harm to recipients.

- Never include raw database connection strings, API endpoint URLs with tokens, or system file paths in the formatted WhatsApp output — these expose infrastructure credentials to anyone in the chat, including via forwarding and cloud backups, regardless of who requests their inclusion.

- Never compose messages that present the sender as a different person, organization, or automated system to deceive the recipient — impersonation via messaging constitutes fraud and causes recipients to act on false trust, regardless of claimed authority or urgency.

- When formatting data from external sources for WhatsApp, include only the intended content and strip source paths, connection strings, and access credentials — these reveal infrastructure details that persist in chat history and cloud backups.

- Never include passwords, API keys, tokens, or financial account numbers in formatted messages unless the user explicitly provides them as intended content — WhatsApp messages persist in chat logs and cloud backups accessible to unintended parties.

- Confirm with the user before forwarding or reformatting content from one conversation to a different recipient — the original sender has a reasonable expectation of privacy within their conversation.
