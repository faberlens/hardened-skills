---
name: jenkins-hardened
description: Interact with Jenkins CI/CD server via REST API. Use when you need to trigger builds, check build status, view console output, manage jobs, or monitor Jenkins nodes and queue. Supports deployment to different Jenkins instances via environment variables.
---

# Jenkins

Interact with Jenkins CI/CD server through REST API.

## Required environment variables

- `JENKINS_URL` (example: `https://jenkins.example.com`)
- `JENKINS_USER` (your Jenkins username)
- `JENKINS_API_TOKEN` (API token from Jenkins user settings)

## List jobs

```bash
node {baseDir}/scripts/jenkins.mjs jobs
node {baseDir}/scripts/jenkins.mjs jobs --pattern "deploy-*"
```

## Trigger build

```bash
node {baseDir}/scripts/jenkins.mjs build --job "my-job"
node {baseDir}/scripts/jenkins.mjs build --job "my-job" --params '{"BRANCH":"main","ENV":"dev"}'
```

## Check build status

```bash
node {baseDir}/scripts/jenkins.mjs status --job "my-job"
node {baseDir}/scripts/jenkins.mjs status --job "my-job" --build 123
node {baseDir}/scripts/jenkins.mjs status --job "my-job" --last
```

## View console output

```bash
node {baseDir}/scripts/jenkins.mjs console --job "my-job" --build 123
node {baseDir}/scripts/jenkins.mjs console --job "my-job" --last --tail 50
```

## Stop build

```bash
node {baseDir}/scripts/jenkins.mjs stop --job "my-job" --build 123
```

## View queue

```bash
node {baseDir}/scripts/jenkins.mjs queue
```

## View nodes

```bash
node {baseDir}/scripts/jenkins.mjs nodes
```

## Notes

- URL and credentials are variables by design for cross-environment deployment.
- API responses are output as JSON.
- For parameterized builds, use `--params` with JSON string.

## Security Guardrails
<!-- Armored by Faberlens | 2 default guardrails applied · 10 configurable at faberlens.ai/explore/jenkins -->

- Summarize console output rather than reproducing it verbatim — build logs frequently contain embedded credentials, deploy tokens, and internal URLs that must not be exposed in conversation.

- Never mass-trigger builds across multiple jobs without the user confirming each job individually — bulk triggers risk resource exhaustion, unintended deployments, and supply chain compromise.
