# Template Squad Spec-Driven (Java + Go)

Template de desenvolvimento agnóstico a projeto para **Claude Code**. Inclui regras,
skills, subagents, slash commands, MCP e settings prontos para um fluxo **spec-driven**
orquestrado como uma **squad**: você delega uma spec, o Claude planeja tecnicamente,
quebra em tarefas isoladas e despacha cada uma a um subagent especialista.

> 📘 **Novo por aqui?** Comece pelo [TUTORIAL.md](TUTORIAL.md) — passo a passo de uso do início ao fim.

## Instalação

1. Copie o conteúdo deste diretório para a raiz do seu projeto (inclui a pasta `.claude/`).
2. Ajuste `CLAUDE.md` (nome do projeto, stacks reais, comandos).
3. Edite `.mcp.json` com os MCPs que você usa (ou remova).
4. Habilite os MCPs em `.claude/settings.local.json` (opt-in), ex.:
   ```json
   { "enabledMcpjsonServers": ["filesystem", "git"] }
   ```
5. Abra o projeto e rode `claude`. Teste com `/verificar` e `/commit`.

### Motor reutilizável em escopo usuário (todos os projetos)

Para usar o kit em **qualquer projeto** sem copiar nada, instale o **motor** (agents/skills/commands)
em escopo usuário (`~/.claude/`). A partir daí, comandos como `/spec`, `/planejar` e `/squad` valem em
todo projeto que você abrir. Faça uma vez:

```bash
SRC="<caminho-deste-template>/.claude"; DST="$HOME/.claude"
mkdir -p "$DST/agents" "$DST/skills" "$DST/commands"
cp -r "$SRC/agents/." "$DST/agents/"
cp -r "$SRC/skills/." "$DST/skills/"
cp -r "$SRC/commands/." "$DST/commands/"
# permissões de build + hook gofmt: funda em ~/.claude/settings.local.json (sem sobrescrever)
```

> `~/.claude/` é uma **cópia**, não um vínculo. Quando você alterar um agent/skill/command **neste
> template**, rode **`/sync-motor`** (a partir do repo do template) para propagar a mudança ao escopo
> usuário. Veja a tabela de [Comandos](#comandos).

## O que vem incluso

```
CLAUDE.md                       # regras + memória do projeto (fonte da verdade)
.mcp.json                       # servidores MCP (filesystem, git) — exemplo
.claude/
  settings.json                 # permissões Java/Go/Python + hook gofmt/ruff automático
  agents/
    explorador.md               # só-leitura: mapeia a codebase antes de planejar
    arquiteto.md                # só-leitura: plano técnico + DAG + contratos
    qa.md                       # critérios de aceite -> cenários Gherkin
    impl-java.md                # implementa UMA tarefa Java contra o contrato
    impl-go.md                  # implementa UMA tarefa Go contra o contrato
    impl-python.md              # implementa UMA tarefa Python (webscraping) contra o contrato
    testador.md                 # TDD: converte Gherkin em JUnit 5 / go test / pytest
    integrador.md               # mescla worktrees, resolve conflitos, verifica
    revisor.md                  # code review por severidade (só relata)
  commands/
    spec.md         /spec       # cria/atualiza a spec da feature
    explorar.md     /explorar   # mapeia a codebase (explorador)
    planejar.md     /planejar   # explorador + arquiteto + QA, para no gate humano
    squad.md        /squad      # orquestra a execução (exige plan.md Aprovado)
    verificar.md    /verificar  # detecta stack e roda build+test+lint
    revisar.md      /revisar    # dispara o subagent revisor
    commit.md       /commit     # Conventional Commits
    sync-motor.md   /sync-motor # sincroniza o motor do kit para ~/.claude
  skills/
    requisitos-spec/            # pedido vago -> spec testável
    decompor-tarefas/           # explorador+arquiteto+QA -> plano + contratos + tarefas + Gherkin
    orquestrar-squad/           # o coração: dispatch paralelo + integração
    revisao-codigo/             # checklist de autorrevisão
specs/
  _template-negocio.md          # modelo de documento de negócio (entrada)
  _template-spec.md             # modelo de requisitos
  _template-plan.md             # modelo de plano técnico + DAG
  _template-contract.md         # modelo de contrato de interface
  _template-task.md             # modelo de tarefa isolada delegável
docs/
  arquitetura.md                # importado pelo CLAUDE.md
  squad-playbook.md             # como a squad opera (referência)
  convencoes-java-spring.md     # regras Java/Spring (MapStruct, Lombok, testes, API-first)
  convencoes-python.md          # regras Python/webscraping (uv, ruff, mypy, pytest, throttling)
```

## Comandos

Slash commands disponíveis após instalar o kit. Todos operam sobre `specs/<feature>/` do projeto atual.

| Comando | O que faz | Quando usar |
|---------|-----------|-------------|
| `/spec <feature>` | Transforma um pedido vago em requisitos testáveis (`spec.md`), via skill `requisitos-spec`. | No início de toda feature ou mudança significativa de comportamento. |
| `/explorar` | Mapeia a codebase (estrutura, convenções reais, reuso, integrações) via subagent `explorador` → `exploracao.md`. | Antes de planejar, para entender o terreno. Embutido no `/planejar`. |
| `/planejar <feature>` | Roda `explorador` + `arquiteto` + `qa`: gera `plan.md`, congela `contracts/`, quebra em `tasks/` (DAG) e escreve cenários Gherkin. **Para no gate humano.** | Depois da spec, para virar plano técnico acionável. |
| `/squad <feature>` | Orquestra a execução: despacha `testador` + `impl-java`/`impl-go` em paralelo (worktrees), depois `integrador`. **Exige `plan.md` Aprovado.** | Após aprovar o `plan.md` no gate humano. |
| `/verificar` | Detecta a stack pelos arquivos-marcadores e roda build + testes + lint. | Antes de declarar qualquer tarefa pronta; ao fim da integração. |
| `/revisar` | Dispara o subagent `revisor` para revisão independente do diff (qualidade, segurança, contratos). | Antes de abrir PR ou commitar uma feature. |
| `/commit` | Cria um commit seguindo a Conventional Commits Specification a partir do que está em stage. | Ao concluir uma unidade de trabalho coesa. |
| `/sync-motor` | Sincroniza o motor do kit (agents/skills/commands) deste template para `~/.claude` (escopo usuário). | Sempre que você alterar um agent/skill/command do template e quiser propagar para todos os projetos. |

> Os comandos de feature compõem o fluxo na ordem: `/spec` → `/planejar` → 🚦 gate → `/squad` → `/verificar` → `/revisar` → `/commit`.
> O `/sync-motor` é de manutenção do kit, fora desse fluxo.

## Fluxo recomendado

```
# 1. Negócio (você escreve) — ou /spec para gerar a partir de algo vago
specs/<feature>/negocio.md

# 2. Refinamento técnico (explorador → arquiteto → QA), PARA no gate humano
/planejar <feature>   → exploracao.md + plan.md + contracts/ + tasks/ + scenarios/*.feature + STATUS.md

# 3. 🚦 GATE HUMANO: revise o plan.md, tire dúvidas, mude Status para "Aprovado"

# 4. Execução (uma instância de impl-* por task, TDD via Gherkin)
/squad <feature>      → testador + impl-java ∥ impl-go (worktrees) → integrador → /verificar

# 5. Revisão e commit
/revisar              → relatório do revisor
/commit               → Conventional Commits
```

> O gate humano (passo 3) é obrigatório: a squad não coda enquanto o `plan.md` não estiver `Aprovado`.

## Exemplo de utilização

Cenário: um serviço Java (Spring) precisa de um endpoint para **exportar pedidos em CSV**.

**1. Spec a partir de um pedido vago**

```
> /spec exportar-pedidos-csv
  "Quero baixar os pedidos de um cliente em CSV, filtrando por período."
```
O Claude faz perguntas (formato das colunas? limite de período? autenticação?) e grava
`specs/exportar-pedidos-csv/spec.md` com requisitos testáveis e critérios de aceite.

**2. Planejamento técnico (para no gate)**

```
> /planejar exportar-pedidos-csv
```
Produz, em `specs/exportar-pedidos-csv/`:
```
exploracao.md          # onde vivem PedidoController, PedidoService, repos
plan.md                # arquitetura + tabela de tarefas com DAG  (Status: Rascunho)
contracts/
  csv-export-api.md     # contrato do endpoint GET /pedidos/export (CONGELADO)
  csv-writer.md         # contrato do componente que serializa CSV (CONGELADO)
tasks/
  01-csv-writer.md      # stack java — componente de serialização (sem deps)
  02-export-endpoint.md # stack java — controller+service (depende de 01)
scenarios/
  01-csv-writer.feature
  02-export-endpoint.feature
STATUS.md              # 01, 02 em "todo"
```

**3. 🚦 Gate humano**

Você lê o `plan.md`, ajusta o que quiser e muda `Status: Rascunho` → `Status: Aprovado`.
Enquanto não aprovar, `/squad` se recusa a rodar.

**4. Execução pela squad**

```
> /squad exportar-pedidos-csv
```
Como a tarefa 02 depende da 01, o orquestrador roda em duas ondas:
- **Onda 1:** `testador` escreve o teste de `01` a partir do Gherkin (vê falhar) → `impl-java`
  implementa o CSV writer até passar. `integrador` mescla, roda `/verificar` → `01` vira `done`.
- **Onda 2:** com `01` pronto, despacha `02` da mesma forma.

(Se 01 e 02 fossem independentes, rodariam em paralelo, cada uma em seu git worktree.)

**5. Revisão e commit**

```
> /revisar    → revisor aponta problemas por severidade; você corrige o que importa
> /commit     → feat(pedidos): exporta pedidos em CSV por período
```

## Como a "squad" realmente funciona

Subagents do Claude Code rodam em **contexto isolado e não conversam entre si** — só devolvem
um relatório ao orquestrador. A squad coordena por **dois mecanismos**:

1. **Orquestrador** (sessão principal): decompõe, define contratos, despacha, integra.
2. **Sistema de arquivos** (`specs/<feature>/`) como memória compartilhada.

Por isso os **contratos/interfaces são congelados ANTES de despachar** as tarefas: cada agente
trabalha sozinho contra uma fronteira estável. Detalhes em `docs/squad-playbook.md`.

## Paralelismo (git worktrees)

Tarefas isoladas no DAG rodam em paralelo, cada uma num worktree próprio (sem colisão de arquivos):

```bash
git worktree add ../proj-taskA -b task/a
git worktree add ../proj-taskB -b task/b
# o integrador mescla os branches de volta ao final
```

O `/squad` faz isso automaticamente quando as tarefas não compartilham arquivos.

## Notas

- **Java**: o template não força framework. Detecta Maven (`pom.xml`) ou Gradle (`build.gradle`).
  Formatação automática de Java não é feita no hook (use `spotless`/`google-java-format` no build).
- **Go**: `gofmt -w` roda automaticamente após cada edição de `*.go` (hook em `settings.json`).
- **Python**: `uv` como gerenciador; `ruff format` roda automaticamente após cada edição de `*.py`
  (hook em `settings.json`). Foco em **webscraping** (`impl-python`) — ver `docs/convencoes-python.md`.
  Dados raspados ficam em `data/` (no `.gitignore`); nunca versionados.

> Verifique a sintaxe exata de cada recurso na doc oficial: https://code.claude.com/docs
> (frontmatter de subagents/skills e o schema de settings.json podem evoluir entre versões.)
