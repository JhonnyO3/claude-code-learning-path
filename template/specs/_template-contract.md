# Contrato: <Nome do Contrato>

> Status: Rascunho | **Congelado** · Data: <YYYY-MM-DD>
> Uma vez **congelado**, este contrato não muda sem reabrir o plano. É a fronteira estável
> contra a qual os subagents trabalham em paralelo, sem precisar conversar entre si.

## Tipo
REST | gRPC/proto | mensagem (fila/tópico) | assinatura de função/interface | schema de dados

## Donos
- Produtor (implementa): tarefa(s) `NN`
- Consumidor (usa): tarefa(s) `NN`

## Definição

### Caso REST
```
MÉTODO /caminho
req headers: ...
req body:  { campo: tipo, ... }
res 2xx:   { campo: tipo, ... }
erros:     400 (...), 404 (...), 409 (...)
```

### Caso interface (Java / Go)
```java
// Java
public interface <Nome> {
    <Tipo> <metodo>(<args>);
}
```
```go
// Go
type <Nome> interface {
    <Metodo>(ctx context.Context, <args>) (<ret>, error)
}
```

### Caso schema/mensagem
```
{ "campo": "tipo", ... }   // inclua versão se aplicável
```

## Invariantes e erros
- Pré-condições, pós-condições, semântica de erro, idempotência, paginação, etc.

## Exemplos
Request/response (ou input/output) reais que servem de base para os testes.
