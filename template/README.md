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

## O que vem incluso

```
CLAUDE.md                       # regras + memória do projeto (fonte da verdade)
.mcp.json                       # servidores MCP (filesystem, git) — exemplo
.claude/
  settings.json                 # permissões Java/Go + hook gofmt automático
  agents/
    explorador.md               # só-leitura: mapeia a codebase antes de planejar
    arquiteto.md                # só-leitura: plano técnico + DAG + contratos
    qa.md                       # critérios de aceite -> cenários Gherkin
    impl-java.md                # implementa UMA tarefa Java contra o contrato
    impl-go.md                  # implementa UMA tarefa Go contra o contrato
    testador.md                 # TDD: converte Gherkin em JUnit 5 / go test
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
```

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

> Verifique a sintaxe exata de cada recurso na doc oficial: https://code.claude.com/docs
> (frontmatter de subagents/skills e o schema de settings.json podem evoluir entre versões.)
