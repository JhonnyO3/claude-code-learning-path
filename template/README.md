# Template avançado Claude Code (Python/IA + Web)

Template de codificação com regras, skills, subagents, slash commands e settings prontos.

## Instalação

1. Copie o conteúdo deste diretório para a raiz do seu projeto.
2. **Renomeie a pasta `dot-claude/` para `.claude/`** (o ponto inicial foi removido só para o arquivo trafegar):
   ```bash
   mv dot-claude .claude
   ```
3. Ajuste `CLAUDE.md` (nome do projeto, stacks, comandos reais).
4. Edite `.mcp.json` com os MCPs que você usa (ou remova).
5. Abra o projeto e rode `claude`. Teste com `/verificar` e `/commit`.

## O que vem incluso

```
CLAUDE.md                 # regras + memória do projeto (fonte da verdade)
.mcp.json                 # servidores MCP (filesystem, git) — exemplo
.claude/
  settings.json           # permissões allow/deny + hook de format automático
  agents/
    planejador.md         # subagent só-leitura que planeja (plan/spec)
    revisor.md            # subagent de code review (só relata)
    testador.md           # subagent de testes (TDD)
  commands/
    verificar.md          # /verificar  -> lint+typecheck+testes
    commit.md             # /commit     -> Conventional Commits
  skills/
    requisitos-spec/      # dispara desenvolvimento orientado a requisitos
    revisao-codigo/       # checklist de autorrevisão
specs/_template.md        # modelo de especificação
docs/arquitetura.md       # importado pelo CLAUDE.md
```

## Fluxo recomendado

`/  ` spec (skill requisitos-spec) → subagent `planejador` → subagent `testador`
→ implementação → skill `revisao-codigo` / subagent `revisor` → `/verificar` → `/commit`.

## Paralelismo

Para tarefas independentes, rode sessões em paralelo com git worktrees:
```bash
git worktree add ../proj-featA -b feat/a
git worktree add ../proj-featB -b feat/b
# uma instância de `claude` em cada worktree
```
Cada subagent já roda no próprio contexto isolado; use-os para exploração/aplicação de template em paralelo.

> Verifique a sintaxe exata de cada recurso na doc oficial: https://code.claude.com/docs
> (frontmatter de subagents/skills e o schema de settings.json podem evoluir entre versões.)
