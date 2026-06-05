# Spec: <Nome da Feature>

> Status: Rascunho | Em revisão | Aprovada
> Autor: <nome> · Data: <YYYY-MM-DD>
> Pasta da feature: `specs/<feature>/`

## Problema / Objetivo
O que queremos resolver e por quê. Quem é o usuário.

## Fora de escopo
O que esta feature explicitamente NÃO faz.

## Requisitos funcionais
- [ ] RF1: ...
- [ ] RF2: ...

## Requisitos não-funcionais
- [ ] Performance: ex. p95 < 200ms
- [ ] Segurança: ...
- [ ] Observabilidade: logs/métricas relevantes

## Contrato de API (se aplicável)
> Detalhe completo em `contracts/`. Resumo aqui.
```
POST /recurso
req:  { ... }
res:  { ... }
erros: 400 (...), 409 (...)
```

## Mudanças de dados (se aplicável)
Tabelas/migrações necessárias.

## Como verificar (requisito -> teste)
- RF1 -> tests/...
- RF2 -> tests/...
