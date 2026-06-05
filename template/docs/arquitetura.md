# Arquitetura

Documento importado pelo `CLAUDE.md`. Descreva aqui os contornos do sistema.

## Camadas

```
core      # regras de negócio puras — sem framework, sem I/O, sem dependência externa
  ^
app       # casos de uso / orquestração — depende de core
  ^
adapters  # entrada (HTTP, CLI, consumers) e saída (DB, filas, APIs) — depende de app
```

Regra de dependência: setas apontam para dentro. `core` não importa `app` nem `adapters`.

### Java
- `core`: domínio (entidades, value objects, regras), sem Spring/Quarkus.
- `app`: serviços de aplicação / casos de uso.
- `adapters`: controllers REST, repositórios, clients. Frameworks vivem aqui.

### Go
- `internal/core` (ou `internal/domain`): regras puras.
- `internal/app`: casos de uso.
- `internal/adapters` + `cmd/`: handlers HTTP, repositórios, entrypoints.
- `pkg/`: apenas o que for reutilizável por outros repositórios.

## Dependências entre módulos
O que pode importar o quê. Liste exceções e por quê.

## Decisões arquiteturais (ADRs)
Decisões relevantes em resumo. Detalhes longos viram ADRs em `docs/adr/`.

## Pontos de integração externos
DB, filas, serviços de terceiros, MCPs. Contratos detalhados ficam em `specs/<feature>/contracts/`.

Mantenha curto e atualizado.
