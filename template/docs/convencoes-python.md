# Convenções Python (com foco em Webscraping)

Importado pelo `CLAUDE.md`. Aplica-se quando o projeto (ou o serviço) usa Python.
Estas regras têm precedência sobre hábitos genéricos de Python.

## Versão e ferramentas

- **Python 3.12+.**
- **uv** como gerenciador (deps, venv, lock). `pyproject.toml` é a **fonte única**; `uv.lock` versionado.
  - `uv add <pkg>` para dependência; `uv sync` para instalar; `uv run <cmd>` para executar no venv.
- **ruff** para lint **e** formatação (substitui black/flake8/isort). Config em `[tool.ruff]`.
- **mypy** em modo **strict**: tudo tipado, sem `def` sem anotação, sem `Any` implícito.
- **pytest** para testes (com `pytest-asyncio` quando houver código async).

## Estrutura (layout `src/`)

```
src/<pacote>/
  domain/     # regras puras (modelos, value objects) — sem I/O, sem rede  (== core)
  app/        # casos de uso / orquestração — depende de domain
  adapters/   # scrapers, clients HTTP, parsers, persistência — depende de app
  __main__.py # entrypoint / CLI
tests/        # espelha src/; sem rede real
pyproject.toml
```

Regra de dependência (igual `docs/arquitetura.md`): setas apontam para dentro. `domain` não importa `adapters`.

## Estilo e tipagem

- **Type hints completos** em assinaturas públicas. Modele dados com `pydantic` (validação) ou `dataclass`.
- **Erros explícitos e tipados:** hierarquia de exceções própria do domínio; **sem `except:` nu**;
  encadeie causa com `raise MeuErro(...) from e`. Nada de engolir exceção.
- **Sem variáveis globais mutáveis.** Injete dependências por parâmetro/construtor.
- **Context managers** para recursos: `with httpx.Client() as c:` / `async with httpx.AsyncClient() as c:`.
- **Logging** via `logging` (estruturado quando possível). **Nunca `print`** em código de produção.
- **Docstrings** curtas em módulos/funções públicas; sem comentários óbvios. Nomes claros explicam o resto.
- **Sem segredos no código** (use variáveis de ambiente). Nunca leia/escreva `.env*`.

## Webscraping — boas práticas (obrigatórias)

- **Escolha da ferramenta por tarefa** (declarada na task):
  - `httpx` + `selectolax`/`lxml`/`BeautifulSoup` → HTML estático e APIs JSON (padrão leve);
  - `scrapy` → crawls grandes/recorrentes (spiders, pipelines, AutoThrottle, dedupe);
  - `playwright` → sites com JS pesado / interação (scroll infinito, login, cliques).
- **Educação e legalidade:** respeite `robots.txt` (`urllib.robotparser`) e o `crawl-delay`; honre
  `Retry-After`. Só raspe alvos **autorizados**; respeite Termos de Uso; não burle autenticação/anti-bot.
- **Throttling e resiliência:** limite de taxa **por domínio**; **backoff exponencial com jitter** em
  `429`/`5xx` (use `tenacity`); **timeouts sempre explícitos**; retries **idempotentes**.
- **User-Agent identificável** (com forma de contato). Não se passe por navegador para enganar defesas.
- **Concorrência controlada** (quando async): `asyncio.Semaphore` limitando requisições simultâneas.
- **Separe buscar de parsear:** uma camada baixa o conteúdo (adapter HTTP), outra extrai (parser puro,
  testável sem rede). **Saída estruturada e validada** (pydantic) — não devolva dicts soltos.
- **Paginação defensiva:** condição de parada explícita; proteja contra loops e páginas faltando.
- **Cache em desenvolvimento** (`hishel`/`requests-cache`) para não martelar o alvo durante o TDD.
- **Falhe alto e claro** quando o seletor/layout mudar (scraping é frágil): erro específico, não silêncio.
- **Dados raspados não vão pro git:** persista em `data/` (no `.gitignore`); nunca commite dump/PII.

## Testes (estilo preferido — TDD)

- Escreva o teste a partir dos cenários Gherkin do QA **antes** da implementação; veja falhar; implemente.
- **Sem rede real nos testes.** Mocke HTTP com `respx`/`responses` ou grave cassetes com `vcrpy`;
  use **fixtures de HTML** salvas para o parser. Testes de parser são puros e determinísticos.
- Tabular com `@pytest.mark.parametrize`. Para async, `pytest-asyncio`.

## Verificação local (antes de concluir uma tarefa)

```bash
uv run ruff check . && uv run ruff format --check .
uv run mypy src
uv run pytest -q
```
