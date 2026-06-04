# CLAUDE.md — Regras do Projeto

> Memória persistente do projeto. Claude lê este arquivo no início de cada sessão.
> Mantenha enxuto (< ~200 linhas). Detalhes longos vão para arquivos importados ou subpastas.

## Visão geral

- **Nome:** <preencher>
- **Stacks:** Python (backend/IA) + TypeScript/React/Node (web)
- **Gerenciadores:** `uv` (Python) · `pnpm` (JS/TS)
- **Versões:** Python 3.12+ · Node 20+

## Estrutura de pastas

```
src/            # código de produção
  api/          # backend (FastAPI / Express)
  web/          # frontend (React/Next)
  core/         # regras de negócio puras, sem framework
tests/          # testes espelham src/ (unit, integration, e2e)
specs/          # especificações ANTES de codar (spec-driven)
docs/           # documentação humana
.claude/        # config do Claude Code (agents, skills, commands, settings)
```

## Fluxo de trabalho obrigatório (Explore → Plan → Code → Commit)

1. **Explore:** leia os arquivos relevantes ANTES de propor mudança. Não suponha.
2. **Plan:** para qualquer tarefa que toque 2+ arquivos, use o plan mode e apresente o plano antes de editar.
3. **Spec-first:** mudanças de feature começam por um arquivo em `specs/`. Sem spec aprovada, não implemente.
4. **TDD quando possível:** escreva/atualize o teste, veja falhar, implemente, veja passar.
5. **Code:** implemente em incrementos pequenos e verificáveis.
6. **Commit:** mensagens no padrão Conventional Commits (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`).

## Convenções de código

- **Python:** type hints sempre · `ruff` (lint+format) · funções puras no `core/` · erros via exceções tipadas.
- **TypeScript:** `strict: true` · sem `any` · componentes funcionais · validação de entrada com `zod`.
- **Nomes:** arquivos `kebab-case`; classes `PascalCase`; funções/vars `camelCase` (TS) / `snake_case` (Py); constantes `UPPER_SNAKE_CASE`.
- **Sem segredos no código.** Use variáveis de ambiente. Nunca leia nem escreva `.env*`.

## Comandos de verificação (rode antes de concluir uma tarefa)

```bash
# Python
uv run ruff check . && uv run ruff format --check . && uv run pytest -q

# Web
pnpm lint && pnpm typecheck && pnpm test
```

## Regras para o agente

- Prefira editar arquivos existentes a criar novos. Não crie README/docs sem ser pedido.
- Não introduza dependências novas sem justificar no plano.
- Ao terminar, **rode os comandos de verificação** e relate o resultado. Não declare "pronto" com teste/lint quebrado.
- Tarefas longas e independentes podem ser delegadas a subagents (ver `.claude/agents/`).
- Mudanças destrutivas (apagar, migrar schema, `git push --force`) exigem confirmação explícita.

## Importações

@docs/arquitetura.md
@specs/_template.md
