# Tarefa NN: <Título curto>

> Unidade isolada e delegável a UM subagent. Auto-contida: tudo que o agente precisa
> para executar sem conversar com outros agents está aqui ou nos contratos referenciados.

## Stack
java | go

## Objetivo
O que esta tarefa entrega (1–3 frases). Mapeia para qual(is) RF da `spec.md`.

## Contratos de referência (congelados)
- `contracts/<nome>.md` — usado como <produtor|consumidor>

## Arquivos que esta tarefa POSSUI (anti-colisão)
> Apenas estes arquivos podem ser criados/editados por esta tarefa. Não há sobreposição
> com outras tarefas paralelas. Se precisar tocar algo fora daqui, vire dependência no DAG.
- `caminho/arquivo1`
- `caminho/arquivo2`

## Dependências (DAG)
- Depende de: <ids ou "—">
- Bloqueia: <ids ou "—">

## Cenários (Gherkin)
- `scenarios/NN-<slug>.feature` (escrito pelo QA) — base do TDD do `testador`.

## Critérios de aceite → testes
- [ ] CA1: ... → cenário `...` → `tests/...` (ou `*_test.go` / `*Test.java`)
- [ ] CA2: ...

## Verificação local (rodar nesta tarefa antes de relatar)
```bash
# Go
go vet ./... && go test ./<pacote>/...
# Java
mvn -q -pl <módulo> test    # ou ./gradlew :<módulo>:test
```

## Notas de implementação
Dicas, gotchas, decisões já tomadas no plano que se aplicam aqui.
