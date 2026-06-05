---
name: decompor-tarefas
description: Use após a spec estar aprovada, ou quando o usuário pedir "planejar", "decompor", "quebrar em tarefas". Transforma specs/<feature>/spec.md em plano técnico, contratos congelados e tarefas isoladas com DAG, prontos para a squad executar.
---

# Decomposição em tarefas (planejamento técnico)

Segundo passo do fluxo da squad: `spec.md` → plano + contratos + tarefas. O objetivo é produzir
unidades de trabalho **isoladas e delegáveis**, com fronteiras estáveis, para que subagents
trabalhem em paralelo sem conversar entre si.

## Passos

1. **Delegue ao subagent `arquiteto`** o desenho técnico a partir de `specs/<feature>/spec.md`.
   Ele explora o código, projeta a arquitetura e propõe contratos + tarefas + DAG.
2. **Grave os artefatos** em `specs/<feature>/`:
   - `plan.md` (de `_template-plan.md`): arquitetura, decisões, tabela de tarefas com DAG, riscos,
     ordem de integração, verificação da feature.
   - `contracts/<nome>.md` (de `_template-contract.md`): um por fronteira. Marque cada um como
     **Congelado** quando estável.
   - `tasks/NN-*.md` (de `_template-task.md`): uma por unidade de trabalho.
   - `STATUS.md`: task board com todas as tarefas em `todo`.
3. **Valide o DAG** antes de seguir para `/squad`.

## Regras de uma boa decomposição

- **Congele os contratos primeiro.** Nenhuma tarefa de implementação deve depender de uma fronteira
  ainda em rascunho.
- **Anti-colisão:** cada tarefa declara os arquivos que possui; tarefas que rodariam em paralelo NÃO
  compartilham arquivos. Se compartilham, vire dependência no DAG (sequencial).
- **Auto-contida:** a tarefa deve trazer tudo que o agente precisa (objetivo, contrato-ref, arquivos,
  critérios de aceite → testes, comando de verificação local).
- **Tamanho:** prefira tarefas pequenas e verificáveis isoladamente.
- **Stack explícita:** marque `java` ou `go` para o orquestrador escolher `impl-java`/`impl-go`.

## STATUS.md inicial (formato)

```markdown
# STATUS — <feature>

| ID | Tarefa | Stack | Estado | Worktree/Branch | Nota |
|----|--------|-------|--------|-----------------|------|
| 01 | ...    | go    | todo   | —               |      |
| 02 | ...    | java  | todo   | —               |      |
```

Próximo passo: `/squad <feature>`.
