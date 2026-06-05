---
description: Orquestra a squad para executar as tarefas da feature (paralelo via worktrees + integração), via skill orquestrar-squad.
allowed-tools: Read, Edit, Bash, Grep, Glob
---

Execute as tarefas da feature com a squad de subagents.

1. Acione a skill `orquestrar-squad`.
2. Leia `specs/<feature>/plan.md` (DAG) e `STATUS.md`.
3. Despache tarefas prontas: paralelas em worktrees (`impl-java`/`impl-go` + `testador`), sequenciais quando há dependência.
4. Integre cada lote com o subagent `integrador` e rode `/verificar`.
5. Atualize `STATUS.md` até todas as tarefas ficarem `done`. Sugira `/revisar` ao final.

Feature (nome da pasta em specs/): $ARGUMENTS
