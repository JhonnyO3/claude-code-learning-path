# CLAUDE.md — Regras do Projeto

> Memória persistente do projeto. Claude lê este arquivo no início de cada sessão.
> Mantenha enxuto (< ~200 linhas). Detalhes longos vão para arquivos importados ou subpastas.

## Visão geral

- **Nome:** <preencher>
- **Stacks:** Java (backend/serviços) · Go (serviços/CLIs/infra)
- **Gerenciadores:** Maven ou Gradle (Java) · `go mod` (Go)
- **Versões:** Java 21 LTS · Go 1.22+
- **Modelo de repo:** suporta monorepo poliglota (Java + Go) ou mono-idioma. A stack de
  cada módulo é detectada por arquivos-marcadores (`pom.xml`/`build.gradle` → Java; `go.mod` → Go).

## Estrutura de pastas

```
# Mono-idioma (raiz é o projeto) — exemplos por stack:
#   Java:  src/main/java · src/test/java · pom.xml | build.gradle
#   Go:    cmd/ · internal/ · pkg/ · go.mod

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

1. **Spec** (`/spec`): transforme o pedido vago em `specs/<feature>/spec.md` — requisitos testáveis.
2. **Plan** (`/planejar`): o subagent `arquiteto` gera `plan.md`, **congela contratos** em
   `contracts/` e quebra em `tasks/NN-*.md` com DAG de dependências. **Sem contrato congelado,
   nenhuma tarefa de implementação começa.**
3. **Squad** (`/squad`): o orquestrador despacha tarefas prontas aos `impl-java`/`impl-go` +
   `testador` (em paralelo via git worktree quando isoladas; sequencial quando há dependência).
4. **Integrar**: o subagent `integrador` mescla os worktrees, resolve conflitos e roda `/verificar`.
5. **Revisar** (`/revisar`): o subagent `revisor` relata problemas por severidade.
6. **Commit** (`/commit`): Conventional Commits (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`).

Detalhes do modelo em `@docs/squad-playbook.md`.

## Convenções de código

- **Java:** sem `null` em APIs públicas (use `Optional`); `record` para DTOs imutáveis;
  exceções tipadas (nada de `throws Exception` genérico); controllers finos (lógica no domínio);
  imutabilidade por padrão; sem lógica de negócio em getters/setters.
- **Go:** erros explícitos (`if err != nil`, com `%w` no wrap); sem `panic` em bibliotecas;
  `context.Context` como primeiro parâmetro e propagado; interfaces pequenas definidas no
  consumidor; sem variáveis globais mutáveis.
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
