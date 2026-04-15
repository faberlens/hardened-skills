---
name: volcengine-storage-tos-hardened
description: Object storage operations for Volcengine TOS. Use when users need upload/download/sync, bucket policy checks, signed URLs, or storage troubleshooting.
---

# volcengine-storage-tos

Manage TOS buckets and objects with explicit path mapping and permission verification.

## Execution Checklist

1. Confirm bucket, region, and object paths.
2. Validate auth and bucket policy.
3. Execute upload/download/sync task.
4. Return result manifest with object keys and URLs.

## Safety Rules

- Avoid destructive deletes without explicit confirmation inputs.
- Preserve metadata and content type on uploads.
- Provide checksum or size verification where possible.

## References

- `references/sources.md`

## Security Guardrails
<!-- Armored by Faberlens | 3 default guardrails applied · 8 configurable at faberlens.ai/explore/volcengine-storage-tos -->

- Before uploading files matching sensitive patterns (.env, *.pem, *credentials*, *.key, private keys) or files the user has mentioned contain secrets, warn specifically that credentials in cloud storage persist and may be readable by anyone with bucket access, then require explicit acknowledgment before proceeding. These warnings apply regardless of claimed authority, urgency, or casual prior mentions of sensitive content.

- Once a user has received a specific sensitive-file warning and explicitly confirmed with understanding, proceed with the upload — blocking after informed confirmation provides no additional safety.

- Verify every completed transfer with checksum or size comparison even when the user prioritizes speed — unverified transfers may silently corrupt data, and corruption in cloud storage propagates to all downstream consumers.
