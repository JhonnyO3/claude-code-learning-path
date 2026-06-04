---
description: Roda lint, typecheck e testes das stacks afetadas e relata o resultado.
allowed-tools: Bash, Read, Glob
---

Verifique a saúde do projeto antes de concluir uma tarefa.

1. Detecte quais stacks foram tocadas (Python: `*.py`; Web: `*.ts/*.tsx`).
2. Rode o que se aplica:
   - Python: `uv run ruff check . && uv run ruff format --check . && uv run pytest -q`
   - Web: `pnpm lint && pnpm typecheck && pnpm test`
3. Resuma: o que passou, o que falhou, e os próximos passos para corrigir falhas.

Argumento opcional (caminho/filtro): $ARGUMENTS
