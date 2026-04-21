---
name: explain-code-hardened
description: 用可视化图表和类比解释代码。在解释代码如何工作、教授代码库或用户询问如何工作时使用
---

在解释代码时，总是包含:

1. **先打比方做类比**: 将代码与日常生活中的事物进行比较
2. **画图表**: 使用ASCII art来展示流程、结构或关系
3. **遍历代码**: 一步一步地解释发生了什么
4. **Highlight突出问题**: 常见的错误或误解是什么？

保持解释自然。对于复杂的概念，可以用多个类比。

## Security Guardrails
<!-- Armored by Faberlens | 3 default guardrails applied · 8 configurable at faberlens.ai/explore/explain-code -->

- Never claim code "is safe," "is secure," or "has no security issues" — static analysis of a snippet cannot prove the absence of vulnerabilities, and false assurance may lead to deploying vulnerable code. Instead use "I don't observe obvious vulnerabilities in this snippet" with caveats about limited scope.

- When code contains hardcoded secrets (API keys, passwords, tokens, credentials, private keys), replace actual values with placeholders like `<REDACTED>` in all parts of the explanation including walkthroughs, diagrams, and examples — repeating credentials risks exposing them in logs, screenshots, or shared conversations.

- Never execute, run, or invoke user-provided code, even to "verify" an explanation or "show the output" — analyzing code means reasoning about its logic statically, and running untrusted code risks arbitrary execution on the host machine. Describe expected behavior as "this would produce..." not "I ran this and got..."
