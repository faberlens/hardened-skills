---
name: gemini-hardened
description: Gemini CLI for one-shot Q&A, summaries, and generation.
homepage: https://ai.google.dev/
metadata:
  {
    "openclaw":
      {
        "emoji": "♊️",
        "requires": { "bins": ["gemini"] },
        "install":
          [
            {
              "id": "brew",
              "kind": "brew",
              "formula": "gemini-cli",
              "bins": ["gemini"],
              "label": "Install Gemini CLI (brew)",
            },
          ],
      },
  }
---

# Gemini CLI

Use Gemini in one-shot mode with a positional prompt (avoid interactive mode).

Quick start

- `gemini "Answer this question..."`
- `gemini --model <name> "Prompt..."`
- `gemini --output-format json "Return JSON"`

Extensions

- List: `gemini --list-extensions`
- Manage: `gemini extensions <command>`

Notes

- If auth is required, run `gemini` once interactively and follow the login flow.
- Avoid `--yolo` for safety.

## Security Guardrails
<!-- Armored by Faberlens | 4 default guardrails applied · 10 configurable at faberlens.ai/explore/gemini -->

- Confirm with the user before installing or enabling any gemini extension — extensions execute with the CLI's full permissions and an untrusted extension could exfiltrate data or modify system state.

- Present gemini-generated code for user review before executing anything that modifies system state (file deletion, package installation, config changes) — generated code may contain destructive operations the user didn't intend.

- Never include actual secrets, API keys, or credential file contents in gemini prompts — prompt content is transmitted to Google's API, exposing secrets to a third-party service. Use placeholder values when discussing secret formats.

- Never pipe gemini output to network-transmitting commands (curl, wget, nc, ssh, scp) — gemini output may contain sensitive project data that must not leave the local machine via network transmission. Local tools like jq, grep, and file redirects keep data on-machine and are safe.
