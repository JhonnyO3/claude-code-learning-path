# Convenções Java / Spring Boot

Importado pelo `CLAUDE.md`. Aplica-se quando o projeto (ou o serviço) usa Spring Boot.
Estas regras têm precedência sobre as convenções genéricas de Java.

## Arquitetura em camadas

```
Controller  →  Service  →  Repository
   (DTO)         (regras)     (Entity/Model)
```

- **DTOs** para entrada/saída da API; **Model/Entity** para domínio/persistência. Ambos essenciais — nunca exponha a entidade diretamente na API.
- **MapStruct** para todo mapeamento DTO ↔ Entity. Não escreva mapeadores à mão.
- **Rich entities**: a entidade carrega comportamento de negócio (métodos usados pelas demais camadas), não é um saco de getters/setters anêmico.
- Controllers finos: orquestram, não contêm regra de negócio (vai no Service/domínio).

## API-first (Swagger obrigatório)

- **Projeto novo:** defina o contrato em `api.yml` (OpenAPI) — abordagem API-first; gere/implemente a partir dele.
- **Projeto existente:** siga o que já se usa; se for por código, crie uma **interface anotada** (springdoc/Swagger) com as regras do endpoint, separada da implementação.
- Documentação Swagger é obrigatória em todos os endpoints.

## Tratamento de erros

- **`@RestControllerAdvice` global é essencial** — centralize o tratamento de exceções e a tradução para respostas HTTP. Não trate exceção espalhada nos controllers.

## Lombok

- `@Data` e `@Builder` são bem-vindos em DTOs e onde fizerem sentido.
- (`@Data` gera setters/equals/hashCode/toString — use com consciência em entidades JPA; prefira `@Getter`/`@Builder` em entidade quando `@Data` causar problemas de `equals`/lazy.)

## Testes (estilo preferido — TDD)

- Teste exercitando o fluxo **do entrypoint (Controller) até as extremidades**, com as **bordas mocadas**: chamadas externas (clients HTTP), banco de dados/repositórios.
- Use `@SpringBootTest` + `MockMvc` (ou `@WebMvcTest` para slice de controller) e **`@MockBean`** nas extremidades.
  > Spring Boot 3.4+: `@MockBean` foi substituído por `@MockitoBean`. Use o que o projeto adota.
- TDD: escreva o teste (a partir dos cenários Gherkin do QA) **antes** da implementação; veja falhar; implemente; veja passar.

## Estilo

- **Sem comentários e sem Javadoc.** Código bem escrito se explica. Nomes claros em métodos, classes e objetos.
- Boas práticas de nomenclatura: intenção explícita, sem abreviações obscuras.
