# CLAUDE.md — Regras do Projeto

> Memória persistente do projeto. Claude lê este arquivo no início de cada sessão.
> Mantenha enxuto (< ~200 linhas). Detalhes longos vão para arquivos importados ou subpastas.

## Visão geral

- **Nome:** <preencher>
- **Stacks:** Java (backend/serviços) · Go (serviços/CLIs/infra) · Python (webscraping/dados/scripts)
- **Gerenciadores:** Maven ou Gradle (Java) · `go mod` (Go) · `uv` (Python)
- **Versões:** Java 21 LTS · Go 1.22+ · Python 3.12+
- **Modelo de repo:** suporta monorepo poliglota (Java + Go + Python) ou mono-idioma. A stack de
  cada módulo é detectada por arquivos-marcadores (`pom.xml`/`build.gradle` → Java; `go.mod` → Go;
  `pyproject.toml`/`requirements.txt` → Python).

## Estrutura de pastas

```
# Mono-idioma (raiz é o projeto) — exemplos por stack:
#   Java:   src/main/java · src/test/java · pom.xml | build.gradle
#   Go:     cmd/ · internal/ · pkg/ · go.mod
#   Python: src/<pkg>/{domain,app,adapters} · tests/ · pyproject.toml (uv)

# Poliglota (vários serviços):
services/
  <svc-java>/   # serviço Java (Maven/Gradle)
  <svc-go>/     # serviço Go (go.mod próprio)
core/           # regras de negócio puras, sem framework (quando compartilhável)
specs/          # especificações ANTES de codar (spec-driven) — ver fluxo abaixo
docs/           # documentação humana (arquitetura, playbook da squad)
.claude/        # config do Claude Code (agents, skills, commands, settings)
```

## Fluxo de trabalho obrigatório (Squad spec-driven)

O desenvolvimento é orquestrado como uma squad: o **orquestrador** (sessão principal)
decompõe e integra; **subagents** especialistas executam tarefas isoladas em contexto próprio.
A coordenação acontece por **arquivos** em `specs/<feature>/`, não por conversa entre agents.

1. **Negócio/Spec**: escreva o negócio em `specs/<feature>/negocio.md` (seu documento detalhado)
   ou use `/spec` para transformar um pedido vago em `specs/<feature>/spec.md` (requisitos testáveis).
2. **Plan** (`/planejar`): roda em sequência:
   - `explorador` mapeia a codebase → `exploracao.md`;
   - `arquiteto` gera `plan.md`, **congela contratos** em `contracts/` e quebra em `tasks/NN-*.md` com DAG;
   - `qa` escreve cenários **Gherkin** por tarefa em `scenarios/*.feature`.
   **Sem contrato congelado, nenhuma tarefa de implementação começa.**
3. **🚦 GATE HUMANO (obrigatório)**: o humano revisa `plan.md`, tira dúvidas e **aprova**
   (muda o Status do `plan.md` para `Aprovado`). `/squad` NÃO roda enquanto não estiver aprovado.
4. **Squad** (`/squad`): o orquestrador despacha tarefas prontas — `testador` (TDD a partir do Gherkin)
   + `impl-java`/`impl-go`/`impl-python` — em paralelo via git worktree quando isoladas; sequencial quando há dependência.
5. **Integrar**: o subagent `integrador` mescla os worktrees, resolve conflitos e roda `/verificar`.
6. **Revisar** (`/revisar`): o subagent `revisor` relata problemas por severidade.
7. **Commit** (`/commit`): Conventional Commits (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`).

Detalhes do modelo em `@docs/squad-playbook.md`.

## Convenções de código

- **Java:** exceções tipadas (nada de `throws Exception` genérico); controllers finos (lógica no
  Service/domínio). **Spring Boot:** siga `@docs/convencoes-java-spring.md` (MapStruct,
  `@RestControllerAdvice`, Lombok `@Data`/`@Builder`, DTO+Entity, rich entities, API-first com
  `api.yml`/interface Swagger, testes do Controller até bordas mocadas com `@MockBean`, sem comentários).
- **Go:** erros explícitos (`if err != nil`, com `%w` no wrap); sem `panic` em bibliotecas;
  `context.Context` como primeiro parâmetro e propagado; interfaces pequenas definidas no
  consumidor; sem variáveis globais mutáveis.
- **Python:** siga `@docs/convencoes-python.md` (uv, ruff, mypy strict, pytest; tipagem completa;
  erros tipados com `raise ... from e`; sem globais mutáveis; camadas `domain`/`app`/`adapters`).
  **Webscraping:** respeite `robots.txt`/`Retry-After`, throttling por domínio, backoff com jitter,
  timeouts explícitos; separe buscar de parsear; dados raspados em `data/` (no `.gitignore`).
- **Nomes:** Java arquivos `PascalCase.java`, pacotes minúsculos; Go arquivos `snake_case.go`,
  pacotes minúsculos e curtos; constantes `UPPER_SNAKE_CASE` (Java) / `CamelCase` exportado (Go).
- **Sem segredos no código.** Use variáveis de ambiente. Nunca leia nem escreva `.env*`.

## Comandos de verificação (rode antes de concluir uma tarefa)

Use `/verificar` — ele detecta a stack pelos arquivos-marcadores. Equivalente manual:

```bash
# Java (Maven)
mvn -q verify
# Java (Gradle)
./gradlew check

# Go
go vet ./... && go test ./... && golangci-lint run

# Python (uv)
uv run ruff check . && uv run ruff format --check . && uv run mypy src && uv run pytest -q
```

## Regras para o agente

- Prefira editar arquivos existentes a criar novos. Não crie README/docs sem ser pedido.
- **Congele contratos antes de despachar** tarefas de implementação (fronteiras estáveis).
- Cada tarefa declara os **arquivos que possui**; tarefas paralelas não compartilham arquivos.
- Não introduza dependências novas sem justificar no plano.
- Ao terminar, **rode `/verificar`** e relate o resultado. Não declare "pronto" com teste/lint quebrado.
- Tarefas longas e independentes vão para subagents (ver `.claude/agents/`).
- Mudanças destrutivas (apagar, migrar schema, `git push --force`, remover worktree com trabalho)
  exigem confirmação explícita.

## Importações

@docs/arquitetura.md
@docs/squad-playbook.md
@docs/convencoes-java-spring.md
@docs/convencoes-python.md
