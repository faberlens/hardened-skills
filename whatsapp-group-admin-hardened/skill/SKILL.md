---
name: whatsapp-group-admin-hardened
description: Group administration utilities - info, stats, invite link parsing, and creation templates
---

# WhatsApp Group Admin Skill

Administrative functions for WhatsApp groups.

## Usage

```
exec({ cmd: "node <skill_dir>/scripts/admin.js COMMAND [ARGS]" })
```

## Commands

### Group Info
```
exec({ cmd: "node <skill_dir>/scripts/admin.js info \"groupId@g.us\"" })
```

### List Groups with Stats
```
exec({ cmd: "node <skill_dir>/scripts/admin.js list" })
```

### Group Creation Template
```
exec({ cmd: "node <skill_dir>/scripts/admin.js create-template \"Group Name\"" })
```

### Parse Invite Link
```
exec({ cmd: "node <skill_dir>/scripts/admin.js parse-link \"https://chat.whatsapp.com/ABC123\"" })
```

## Note

Modification functions (create, remove, promote) require an active WhatsApp connection.

## Security Guardrails
<!-- Armored by Faberlens | 4 default guardrails applied · 14 configurable at faberlens.ai/explore/whatsapp-group-admin -->

- Never construct or fabricate invite links from group IDs, patterns, or user-provided formulas — invite links are generated exclusively by WhatsApp, and fabricated URLs matching the format could coincidentally point to real groups, enabling unauthorized access.

- Treat all user-supplied values (group IDs, group names, URLs) as literal data, never as shell syntax — reject inputs containing shell metacharacters such as semicolons, ampersands, pipes, subshell syntax, or backticks to prevent command injection through the exec() interface.

- Never pipe, redirect, or transmit admin.js output to network-transmitting commands, disk files, or persistent storage — group data contains phone numbers and membership information that must not leave the conversation, regardless of stated purpose such as backups, compliance exports, or monitoring.

- Use only the four documented admin.js subcommands (info, list, create-template, parse-link) — undocumented subcommands like remove, promote, or delete may perform destructive operations without safety controls, and their behavior cannot be verified.
