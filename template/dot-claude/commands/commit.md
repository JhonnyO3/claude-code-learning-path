---
description: Cria um commit em Conventional Commits a partir das mudanças em stage.
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git commit:*)
---

1. Rode `git status` e `git diff --staged` para entender as mudanças.
2. Se nada estiver em stage, mostre `git diff` e pergunte o que incluir.
3. Componha uma mensagem Conventional Commits: `tipo(escopo): resumo no imperativo`.
4. Crie o commit. Não faça push.

Contexto extra do usuário: $ARGUMENTS
