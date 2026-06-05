# Squad Playbook — como a squad de subagents opera

Referência do modelo de orquestração. Importado pelo `CLAUDE.md`.

## Princípio central

Subagents do Claude Code rodam em **contexto isolado**. Eles **não conversam entre si** e só
devolvem um **relatório final** ao orquestrador (a sessão principal). Portanto, a "squad" não
coordena por chat — coordena por **dois mecanismos**:

1. **Orquestrador (tech-lead)** — a sessão principal. Decompõe a spec, define e congela os
   contratos, despacha tarefas, integra resultados e resolve conflitos.
2. **Sistema de arquivos = memória compartilhada** — tudo que precisa ser compartilhado vive
   em `specs/<feature>/` (spec, plano, contratos, tarefas, STATUS).

**Consequência que define o design:** os contratos/interfaces são **congelados ANTES** de
despachar qualquer tarefa de implementação. Assim cada agente trabalha sozinho contra uma
fronteira estável — exatamente como uma squad humana paraleliza definindo as costuras primeiro.

## Papéis

| Papel        | Subagent      | Escreve código? | Função |
|--------------|---------------|-----------------|--------|
| Arquiteto    | `arquiteto`   | Não (só-leitura)| spec → plano + DAG + contratos congelados |
| Impl. Java   | `impl-java`   | Sim             | implementa UMA tarefa Java contra o contrato |
| Impl. Go     | `impl-go`     | Sim             | implementa UMA tarefa Go contra o contrato |
| Testador     | `testador`    | Sim (testes)    | TDD: escreve teste que falha primeiro |
| Integrador   | `integrador`  | Sim (merge/fix) | mescla worktrees, resolve conflitos, verifica |
| Revisor      | `revisor`     | Não (só relata) | code review por severidade |

## Estrutura de coordenação em `specs/<feature>/`

```
spec.md          # o quê / porquê
plan.md          # como: arquitetura + DAG de tarefas
contracts/       # interfaces CONGELADAS (a fronteira entre tarefas)
tasks/NN-*.md    # tarefas isoladas, auto-contidas, com arquivos próprios
STATUS.md        # task board: todo | doing | done | blocked (por tarefa)
```

## Ciclo de execução (o que o `/squad` faz)

1. Lê `plan.md` (DAG) e `STATUS.md`.
2. Identifica **tarefas prontas** (todas as dependências `done`).
3. Para tarefas prontas que **não compartilham arquivos**:
   - cria um `git worktree` por tarefa (isolamento de arquivos);
   - despacha `impl-java`/`impl-go` (+`testador`) **em paralelo** (várias chamadas Agent numa
     única mensagem);
   - marca cada tarefa como `doing` no `STATUS.md`.
4. Para tarefas com dependência ainda não satisfeita: aguarda (sequencial).
5. Quando um lote termina: o `integrador` mescla os branches/worktrees, resolve conflitos e
   roda `/verificar`. Tarefas viram `done`.
6. Repete até o DAG completo. Depois: `/revisar` → `/commit`.

## Quando paralelizar vs sequencial

- **Paralelo (worktree):** tarefas sem dependência mútua e sem arquivos compartilhados.
- **Sequencial:** tarefas com dependência no DAG, ou que tocam os mesmos arquivos, ou quando
  o contrato ainda não está congelado.

## Git worktrees — mecânica

```bash
git worktree add ../<proj>-task01 -b task/01    # clone isolado por tarefa
# ... subagent trabalha no worktree ...
git worktree remove ../<proj>-task01            # após merge pelo integrador
```

Cada worktree é uma cópia de trabalho independente: dois subagents editando arquivos diferentes
nunca colidem. O `integrador` faz o merge de volta no branch da feature.

## Regras de ouro

- Sem contrato congelado, nenhuma implementação começa.
- Uma tarefa = um dono de arquivos. Sem sobreposição entre tarefas paralelas.
- O orquestrador nunca assume o relatório de um agente: relê `STATUS.md` e os diffs.
- `/verificar` verde é condição para `done`.
