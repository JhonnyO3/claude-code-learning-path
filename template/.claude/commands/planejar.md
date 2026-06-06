---
description: Refina tecnicamente a feature (explorador + arquiteto + QA) em plano, contratos, tarefas (DAG) e cenários Gherkin, e PARA no gate humano antes de implementar.
---

Planeje tecnicamente a feature indicada.

1. Confirme a entrada: `specs/<feature>/negocio.md` (documento de negócio) ou `spec.md`.
2. Acione a skill `decompor-tarefas`, que orquestra:
   - `explorador` → `exploracao.md` (mapa da codebase);
   - `arquiteto` → `plan.md` (Status: Rascunho), `contracts/*` (congelados), `tasks/NN-*.md`, `STATUS.md`;
   - `qa` → `scenarios/NN-*.feature` (Gherkin).
3. **PARE no gate humano:** apresente o `plan.md` para revisão/dúvidas. NÃO acione `/squad`.
   A implementação só começa quando o humano mudar `Status: Aprovado` no `plan.md`.

Feature (nome da pasta em specs/): $ARGUMENTS
