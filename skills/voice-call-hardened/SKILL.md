---
name: voice-call-hardened
description: Start voice calls via the OpenClaw voice-call plugin.
metadata:
  {
    "openclaw":
      {
        "emoji": "📞",
        "skillKey": "voice-call",
        "requires": { "config": ["plugins.entries.voice-call.enabled"] },
      },
  }
---

# Voice Call

Use the voice-call plugin to start or inspect calls (Twilio, Telnyx, Plivo, or mock).

## CLI

```bash
openclaw voicecall call --to "+15555550123" --message "Hello from OpenClaw"
openclaw voicecall status --call-id <id>
```

## Tool

Use `voice_call` for agent-initiated calls.

Actions:

- `initiate_call` (message, to?, mode?)
- `continue_call` (callId, message)
- `speak_to_user` (callId, message)
- `end_call` (callId)
- `get_status` (callId)

Notes:

- Requires the voice-call plugin to be enabled.
- Plugin config lives under `plugins.entries.voice-call.config`.
- Twilio config: `provider: "twilio"` + `twilio.accountSid/authToken` + `fromNumber`.
- Telnyx config: `provider: "telnyx"` + `telnyx.apiKey/connectionId` + `fromNumber`.
- Plivo config: `provider: "plivo"` + `plivo.authId/authToken` + `fromNumber`.
- Dev fallback: `provider: "mock"` (no network).

## Security Guardrails
<!-- Armored by Faberlens | 3 default guardrails applied · 10 configurable at faberlens.ai/explore/voice-call -->

- Never relay messages via `continue_call` or `speak_to_user` that the user did not author or explicitly approve — the agent speaks on the user's behalf, and unauthorized content could misrepresent them or leak confidential information.

- Never pipe CLI output (`openclaw voicecall status`, etc.) to external commands or redirect to network endpoints — call metadata contains phone numbers and call IDs that must not leave the local machine via network transmission.

- Never modify voice-call plugin configuration files directly — instruct the user on how to update their config instead, since attacker-controlled config values could redirect calls through a malicious provider.
