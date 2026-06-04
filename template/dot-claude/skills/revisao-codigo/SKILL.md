---

## name: revisao-codigo
description: Use antes de finalizar uma tarefa ou abrir PR, ou quando o usuário pedir "revisar", "code review", "checar qualidade". Roteiro de autorrevisão de qualidade e segurança alinhado ao CLAUDE.md.

# Revisão de código

Aplique este checklist às mudanças (use `git diff` para ver o que mudou).

## Correção

- O código faz o que a spec pede? Casos de borda tratados?
- Há teste cobrindo a mudança? Roda verde?

## Segurança

- Nenhum segredo hardcoded; entrada externa validada (zod/pydantic).
- Sem injeção (SQL/command); erros não vazam dados sensíveis.

## Qualidade

- Aderente às convenções do CLAUDE.md (nomes, tipos, estrutura).
- Sem duplicação óbvia; funções coesas; nomes claros.
- Sem dependência nova injustificada.

## Antes de concluir

- Rodar `/verificar`. Não declarar "pronto" com lint/teste quebrado.
- Para revisão independente e mais rigorosa, delegar ao subagente `revisor`.

