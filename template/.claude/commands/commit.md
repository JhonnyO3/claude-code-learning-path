---
description: Cria um commit seguindo a Conventional Commits Specification a partir das mudanças em stage.
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git commit:*)
---

Crie um commit aderente à **Conventional Commits 1.0.0**.

## Passos

1. Rode `git status` e `git diff --staged` para entender as mudanças.
2. Se nada estiver em stage, mostre `git diff` e pergunte o que incluir.
3. Componha a mensagem conforme a especificação abaixo.
4. Crie o commit. Não faça push.

## Estrutura (obrigatória)

```
<tipo>[escopo opcional][!]: <descrição>

[corpo opcional]

[rodapé(s) opcional(is)]
```

## Regras (RFC 2119)

- O commit **MUST** começar com um **tipo** (substantivo: `feat`, `fix`, etc.), seguido de escopo
  OPCIONAL, `!` OPCIONAL, e **dois-pontos + espaço** obrigatórios.
- `feat` **MUST** ser usado quando o commit adiciona uma funcionalidade.
- `fix` **MUST** ser usado quando o commit corrige um bug.
- Outros tipos **MAY** ser usados: `docs`, `refactor`, `test`, `chore`, `build`, `ci`, `perf`, `style`.
- O **escopo** é OPCIONAL: um substantivo entre parênteses descrevendo a seção do código, ex.: `fix(parser):`.
- A **descrição** **MUST** vir logo após `: ` — resumo curto, no imperativo, sem ponto final.
- O **corpo** é OPCIONAL e **MUST** começar uma linha em branco após a descrição; pode ter vários parágrafos.
- **Rodapés** são OPCIONAIS, uma linha em branco após o corpo. Cada rodapé **MUST** ser
  `Token: valor` ou `Token #valor`; o token usa `-` no lugar de espaços (ex.: `Reviewed-by`,
  `Refs`), exceto `BREAKING CHANGE`.

## Breaking changes

- **MUST** ser indicados por `!` antes do `:` no prefixo, **ou** por um rodapé `BREAKING CHANGE: <descrição>`.
- Se em rodapé, `BREAKING CHANGE` **MUST** ser maiúsculo (sinônimo: `BREAKING-CHANGE`).
- Se usado `!` no prefixo, o rodapé `BREAKING CHANGE:` **MAY** ser omitido e a descrição descreve a quebra.
- Exceto `BREAKING CHANGE` (que **MUST** ser maiúsculo), os elementos não são case-sensitive.

## Exemplos

```
feat(auth): adiciona login via OTP
fix: corrige parsing de datas com fuso
docs(readme): atualiza instruções de instalação
refactor(core)!: remove campo legado de Usuario

BREAKING CHANGE: o campo `nomeCompleto` foi removido em favor de `nome` e `sobrenome`.
```

Contexto extra do usuário: $ARGUMENTS
