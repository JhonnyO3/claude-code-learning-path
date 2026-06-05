# Plano técnico: <Nome da Feature>

> Gerado pelo subagent `arquiteto` a partir de `spec.md`. Fonte da verdade técnica.
> Status: Rascunho | Aprovado · Data: <YYYY-MM-DD>

## Contexto arquitetural
Como esta feature se encaixa nas camadas (core → app → adapters). O que será tocado.
Decisões-chave e alternativas descartadas (resumo; ADRs longos vão para `docs/adr/`).

## Contratos congelados
> Cada contrato vive em `contracts/<nome>.md`. Liste-os aqui. **Nenhuma tarefa de
> implementação começa antes do contrato correspondente estar congelado.**
- [ ] `contracts/<nome>.md` — <o que define> — status: rascunho | congelado

## Tarefas (DAG)
> Cada tarefa vira `tasks/NN-*.md`. Liste id, stack, donos de arquivos e dependências.

| ID | Tarefa | Stack | Depende de | Arquivos que possui (anti-colisão) |
|----|--------|-------|------------|-------------------------------------|
| 01 | ...    | go    | —          | `internal/...`                      |
| 02 | ...    | java  | —          | `src/main/java/...`                 |
| 03 | ...    | go    | 01         | `cmd/...`                           |

```
# DAG (ordem de execução; tarefas sem dependência mútua rodam em paralelo)
01 ──┐
     ├─> 03
02 ──┘
```

## Riscos e mitigações
- Risco: ... → Mitigação: ...

## Ordem de integração
Como o `integrador` junta os worktrees (ordem de merge, pontos de conflito previstos).

## Como verificar (feature inteira)
Comando(s) de `/verificar` que devem passar ao final + critérios de aceite globais.
