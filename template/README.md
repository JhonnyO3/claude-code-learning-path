# Template Squad Spec-Driven (Java · Go · Python)

Kit de desenvolvimento **spec-driven** para **Claude Code**, orquestrado como uma **squad** de
subagents: você descreve o negócio, o Claude planeja tecnicamente, congela contratos, quebra em
tarefas isoladas e despacha cada uma a um especialista — com um **gate humano** antes de codar.

## Instalação

O **motor** (subagents, skills, slash commands) vive em escopo usuário (`~/.claude/`) e fica
disponível em **todos** os projetos. Instale uma vez, a partir deste repo:

```bash
SRC="<caminho-deste-template>/.claude"; DST="$HOME/.claude"
mkdir -p "$DST/agents" "$DST/skills" "$DST/commands"
cp -r "$SRC/agents/." "$DST/agents/" && cp -r "$SRC/skills/." "$DST/skills/" && cp -r "$SRC/commands/." "$DST/commands/"
```

Em cada projeto, copie a "pele": `CLAUDE.md`, `docs/` e os moldes `specs/_template-*.md`.
Quando alterar o motor neste template, rode **`/sync-motor`** para propagar.

## Comandos

| Comando | O que faz |
|---------|-----------|
| `/spec <feature>` | Pedido vago → requisitos testáveis (`spec.md`). |
| `/explorar` | Mapeia a codebase (estrutura, convenções, reuso). |
| `/planejar <feature>` | Explorador + arquiteto + QA → plano, contratos congelados, tarefas (DAG) e Gherkin. **Para no gate humano.** |
| `/squad <feature>` | Executa as tarefas (testador + `impl-java`/`impl-go`/`impl-python` em paralelo via worktrees) e integra. **Exige `plan.md` aprovado.** |
| `/verificar` | Detecta a stack e roda build + testes + lint. |
| `/revisar` | Code review independente do diff, por severidade. |
| `/commit` | Commit em Conventional Commits. |
| `/sync-motor` | Sincroniza o motor do kit para `~/.claude`. |

Ordem do fluxo: `/spec` → `/planejar` → 🚦 **gate humano** → `/squad` → `/verificar` → `/revisar` → `/commit`.

> **Gate humano (obrigatório):** a squad só coda quando você muda `Status: Aprovado` no `plan.md`.

## Exemplo básico

Endpoint que exporta pedidos em CSV, num serviço Java:

```text
1. Negócio → /spec exportar-csv
   "Quero baixar os pedidos de um cliente em CSV, por período."
   → gera specs/exportar-csv/spec.md (requisitos + critérios de aceite)

2. /planejar exportar-csv
   → exploracao.md, plan.md (DAG), contracts/ (congelados),
     tasks/, scenarios/*.feature, STATUS.md   [Status: Rascunho]

3. 🚦 Você revisa o plan.md e muda para Status: Aprovado

4. /squad exportar-csv
   → testador escreve o teste (Gherkin → falha), impl-java implementa até passar,
     integrador mescla e roda /verificar  → tarefas viram "done"

5. /revisar   → relatório por severidade
   /commit    → feat(pedidos): exporta pedidos em CSV por período
```

## Stacks

- **Java 21** (Maven/Gradle) — convenções em `docs/convencoes-java-spring.md`.
- **Go 1.22+** (`go mod`).
- **Python 3.12+** (`uv` · ruff · mypy · pytest), foco em webscraping — `docs/convencoes-python.md`.

A stack de cada módulo é detectada por arquivos-marcadores (`pom.xml`/`build.gradle`, `go.mod`,
`pyproject.toml`). Detalhes do modelo da squad em `docs/squad-playbook.md`.
