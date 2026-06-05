---
description: Decompõe a spec da feature em plano técnico, contratos congelados e tarefas (DAG), via skill decompor-tarefas e subagent arquiteto.
---

Planeje tecnicamente a feature indicada.

1. Confirme que `specs/<feature>/spec.md` existe e está aprovada.
2. Acione a skill `decompor-tarefas` (delega ao subagent `arquiteto`).
3. Produza em `specs/<feature>/`: `plan.md`, `contracts/*` (congelados), `tasks/NN-*.md` e `STATUS.md`.
4. Valide o DAG e sugira `/squad <feature>` como próximo passo.

Feature (nome da pasta em specs/): $ARGUMENTS
