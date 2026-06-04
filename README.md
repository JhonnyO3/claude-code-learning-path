# Claude Code Learning Path

Um caminho de estudos completo para **dominar o Claude Code** — voltado a devs que já usam a ferramenta no dia a dia e querem nível avançado: **skills, multi-agents (subagents), desenvolvimento orientado a requisitos e paralelismo**.

O repositório tem duas partes:

1. **Roadmap de estudos** — um caminho em 6 fases com conteúdos, vídeos (PT-BR/EN) e marcos práticos.
2. **Template avançado** — um projeto base pronto para codar, com `CLAUDE.md`, skills, subagents, slash commands, hooks e configuração de permissões.

---

## 1. Roadmap de estudos

Abra **`roadmap-claude-code.html`** no navegador para a versão visual (tema escuro, fases coloridas, links de vídeo e marcos). Resumo das fases:

| # | Fase | O que você domina | Marco prático |
|---|------|-------------------|---------------|
| 1 | **Fundamentos** | CLI, sessões, contexto, modos, plan mode | Rodar uma tarefa real com Explore → Plan → Code |
| 2 | **CLAUDE.md & pastas** | Memória de projeto, hierarquia, `@imports`, organização | Escrever um `CLAUDE.md` para um projeto seu |
| 3 | **Skills & slash commands** | `SKILL.md`, discovery por descrição, comandos `/` com `$ARGUMENTS` | Criar uma skill e um comando próprios |
| 4 | **MCP, hooks, settings, plugins** | `.mcp.json`, `PreToolUse`/`PostToolUse`, permissões, marketplaces | Conectar um MCP e automatizar um hook de format |
| 5 | **Subagents & paralelismo** | `.claude/agents/*.md`, delegação, git worktrees | Rodar planejador + revisor + testador em paralelo |
| 6 | **Requisitos + template final** | Desenvolvimento orientado a specs, TDD, montagem do template | Montar e usar o template deste repositório |

> Preferência de vídeos: PT-BR quando há boa qualidade; senão, inglês de alto nível. Os links estão dentro do HTML.

### Recursos oficiais gratuitos recomendados
- **Anthropic Academy** (`anthropic.skilljar.com`): Claude Code 101, Claude Code in Action, Introduction to Agent Skills, Introduction to Subagents.
- **Documentação oficial**: `https://docs.claude.com`.

<img width="641" height="1271" alt="image" src="https://github.com/user-attachments/assets/cb9b720c-9352-4ccc-849c-dfa5f8f11cfa" />

---

## 2. Template avançado (`template/`)

Projeto base para começar a codar já no fluxo correto. Stacks de exemplo: **Python (uv · ruff · pytest)** e **TypeScript/React/Node (pnpm · zod · vitest)**.

### Estrutura

```
template/
  CLAUDE.md            # regras do projeto (memória persistente do Claude)
  .mcp.json            # servidores MCP (filesystem, git)
  .gitignore
  README.md            # instruções do template
  specs/
    _template.md       # modelo de especificação (spec-driven)
  docs/
    arquitetura.md     # doc de arquitetura (importado pelo CLAUDE.md)
  dot-claude/          # renomeie para .claude após baixar (ver abaixo)
    settings.json      # permissões (allow/deny) + hook de format
    agents/
      planejador.md    # subagent: planeja, não edita (model: opus)
      revisor.md       # subagent: revisa qualidade/segurança (read-only)
      testador.md      # subagent: escreve testes em TDD
    commands/
      verificar.md     # /verificar — lint + typecheck + testes
      commit.md        # /commit — Conventional Commits
    skills/
      requisitos-spec/SKILL.md   # transforma pedido vago em spec testável
      revisao-codigo/SKILL.md    # roteiro de autorrevisão
```

### Instalação

> **Importante:** a pasta de configuração foi entregue como `dot-claude/` porque não foi possível gravar diretamente em `.claude`. Após baixar, renomeie:

```bash
cd template
mv dot-claude .claude        # Windows (cmd): ren dot-claude .claude
```

Depois disso o Claude Code já reconhece os agents, skills, commands e settings automaticamente.

### Fluxo de trabalho recomendado

1. **Explore** — leia os arquivos relevantes antes de propor mudança.
2. **Plan** — para tarefas que toquem 2+ arquivos, use o plan mode (ou o subagent `planejador`).
3. **Spec-first** — features novas começam por um arquivo em `specs/` (use a skill `requisitos-spec`).
4. **TDD** — escreva o teste (subagent `testador`), veja falhar, implemente, veja passar.
5. **Code** — incrementos pequenos e verificáveis.
6. **Verificar** — rode `/verificar` (lint + typecheck + testes).
7. **Revisar** — use o subagent `revisor` ou a skill `revisao-codigo`.
8. **Commit** — use `/commit` (Conventional Commits).

### Paralelismo

Para rodar tarefas independentes em paralelo, use **git worktrees** (um diretório de trabalho por branch) e delegue partes a subagents diferentes. Detalhes na Fase 5 do roadmap.

---

## Como usar este repositório

1. Comece pelo **roadmap** (`roadmap-claude-code.html`) e siga as 6 fases.
2. Ao chegar na Fase 6, **adote o template** num projeto real.
3. Itere: ajuste `CLAUDE.md`, crie suas próprias skills e agents conforme a necessidade.

---

_Bons estudos — e bom código._
