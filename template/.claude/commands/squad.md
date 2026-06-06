---
description: Orquestra a squad para executar as tarefas da feature (paralelo via worktrees + integração), via skill orquestrar-squad.
allowed-tools: Read, Edit, Bash, Grep, Glob
---

Execute as tarefas da feature com a squad de subagents.

1. **Verifique o gate:** `specs/<feature>/plan.md` deve estar `Status: Aprovado`. Se não, PARE e
   peça a aprovação humana — não despache nada.
2. Acione a skill `orquestrar-squad`.
3. Leia `specs/<feature>/plan.md` (DAG), `STATUS.md` e `scenarios/*.feature`.
4. Despache tarefas prontas: uma instância de `impl-java`/`impl-go` por tarefa + `testador` (TDD via
   Gherkin), paralelas em worktrees; sequenciais quando há dependência.
5. Integre cada lote com o subagent `integrador` e rode `/verificar`.
6. Atualize `STATUS.md` até todas as tarefas ficarem `done`. Sugira `/revisar` ao final.

Feature (nome da pasta em specs/): $ARGUMENTS
