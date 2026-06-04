---
name: requisitos-spec
description: Use ao iniciar qualquer nova feature ou mudança significativa de comportamento, ou quando o usuário pedir "spec", "requisitos", "planejar feature". Conduz desenvolvimento orientado a requisitos — transforma um pedido vago em uma especificação clara e testável ANTES de codar.
---

# Desenvolvimento orientado a requisitos (spec-driven)

Antes de escrever código de produção, produza/atualize uma spec em `specs/<nome-da-feature>.md`.

## Passos

1. Faça as perguntas necessárias para remover ambiguidade (entradas, saídas, casos de erro, fora de escopo).
2. Escreva a spec usando o template em `specs/_template.md`.
3. Liste critérios de aceitação como checklist verificável.
4. Só depois da spec aprovada, prossiga para testes (skill/subagent `testador`) e implementação.

## Critérios de uma boa spec

- Cada requisito é observável e testável (evite "deve ser rápido"; prefira "p95 < 200ms").
- Define explicitamente o que está FORA de escopo.
- Inclui contratos de API (request/response) e mudanças de dados quando houver.
- Tem uma seção de "Como verificar" mapeando requisito → teste.

Mantenha a spec versionada no git. Ela é a fonte da verdade da feature.
