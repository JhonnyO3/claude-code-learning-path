---
description: Dispara o subagent revisor para revisão independente do diff atual (qualidade, segurança, contratos).
allowed-tools: Bash(git diff:*), Bash(git status:*), Read, Grep, Glob
---

Faça a revisão de código das mudanças atuais.

1. Delegue ao subagente `revisor` (revisão independente e rigorosa).
2. O revisor roda `git diff`, confronta com `spec.md`/`contracts/` e relata por severidade (🔴/🟡/🟢).
3. Resuma os achados e os bloqueios para merge.

Contexto/escopo opcional: $ARGUMENTS
