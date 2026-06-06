---
description: Mapeia a codebase (estrutura, convenções, reuso, integrações) via subagent explorador, antes de planejar.
allowed-tools: Read, Grep, Glob, Bash
---

Entenda o terreno antes de planejar.

1. Delegue ao subagente `explorador`.
2. Ele lê `specs/<feature>/negocio.md`/`spec.md` (se houver) e mapeia o código relevante.
3. Produz `specs/<feature>/exploracao.md` (ou um resumo, se sem feature): estrutura, convenções reais,
   código reutilizável, pontos de integração, riscos e perguntas abertas.

Normalmente roda automaticamente dentro de `/planejar`; use isoladamente para investigar a codebase.

Feature ou área a explorar: $ARGUMENTS
