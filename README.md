<div align="center">

# 🚀 Roadmap Claude Code para Devs

**Do uso diário ao domínio: skills · subagents · paralelismo · MCP · desenvolvimento orientado a requisitos.**
Otimizado para **Python/IA + Web (TS/React/Node)**.

![Python](https://img.shields.io/badge/Python-3.12+-3776AB?logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white)
![Claude Code](https://img.shields.io/badge/Claude%20Code-advanced-D97757)
![License](https://img.shields.io/badge/license-MIT-green)

🌐 **[Ver o roadmap online »](https://jhonnyo3.github.io/claude-code-learning-path/)**

</div>

---

## 🗺️ O roadmap visual

<div align="center">

<img width="669" height="1271" alt="image" src="https://github.com/user-attachments/assets/9c4696bb-1e8e-4194-a374-501cacd9f0fe" />


</div>

> 💡 Versão interativa: abra **[`roadmap-claude-code.html`](roadmap-claude-code.html)** no navegador (ou publicada via GitHub Pages) para os links de vídeo clicáveis.

---

## 📦 O que tem neste repositório

| Parte | Arquivo | O que é |
|-------|---------|---------|
| 🗺️ **Roadmap de estudos** | `roadmap-claude-code.html` | Caminho em 6 fases com conteúdos, vídeos (PT-BR/EN) e marcos |
| 🧰 **Template avançado** | `template/` | Projeto base pronto: `CLAUDE.md`, skills, subagents, commands, hooks |

---

## 📚 As 6 fases

### 🟣 Fase 1 · ~1 sem — Fundamentos & o loop agêntico
Solidifique o fluxo **Explore → Plan → Code → Commit** e o plan mode.

> 🎯 **Marco:** rodar uma feature pequena ponta a ponta usando plan mode antes de editar.

### 🟢 Fase 2 · ~3 dias — CLAUDE.md & organização de pastas
Memória de projeto, hierarquia de contexto, `@imports` e estrutura de repositório.

> 🎯 **Marco:** escrever o `CLAUDE.md` do seu projeto real (use o do template como base).

### 🟠 Fase 3 · ~1 sem — Skills & slash commands
`SKILL.md`, descoberta automática por descrição, comandos `/` com `$ARGUMENTS`.

> 🎯 **Marco:** criar 1 skill (ex: `revisao-codigo`) + 1 slash command (`/verificar`) próprios.

### 🟡 Fase 4 · ~1 sem — MCP, hooks, settings & plugins
`.mcp.json`, `PreToolUse`/`PostToolUse`, permissões (menor privilégio), marketplaces.

> 🎯 **Marco:** conectar 1 MCP útil + 1 hook que formata/testa ao salvar.

### 🔴 Fase 5 · ~1,5 sem — Subagents, multi-agent & paralelismo
`.claude/agents/*.md`, delegação, git worktrees, agent teams vs. subagents.

> 🎯 **Marco:** rodar planejador + testador + revisor em conjunto e 2 worktrees em paralelo.

### 🔵 Fase 6 · ~1 sem — Desenvolvimento orientado a requisitos + template final
Spec-driven, TDD e consolidação de tudo num template avançado.

> 🎯 **Marco:** seu template avançado funcionando num projeto real (esta entrega é o ponto de partida).

---

## 🎓 Recursos oficiais gratuitos

- **Anthropic Academy** (`anthropic.skilljar.com`): Claude Code 101 · Claude Code in Action · Introduction to Agent Skills · Introduction to Subagents.
- **Documentação oficial**: <https://docs.claude.com>.

---

## 🧰 Template avançado (`template/`)

Projeto base para começar a codar já no fluxo correto. Stacks de exemplo: **Python (uv · ruff · pytest)** e **TypeScript/React/Node (pnpm · zod · vitest)**.

### Estrutura

```
template/
├─ CLAUDE.md            # regras do projeto (memória persistente do Claude)
├─ .mcp.json            # servidores MCP (filesystem, git)
├─ .gitignore
├─ README.md            # instruções do template
├─ specs/
│  └─ _template.md      # modelo de especificação (spec-driven)
├─ docs/
│  └─ arquitetura.md    # doc de arquitetura (importado pelo CLAUDE.md)
└─ dot-claude/          # ⚠️ renomeie para .claude após baixar
   ├─ settings.json     # permissões (allow/deny) + hook de format
   ├─ agents/
   │  ├─ planejador.md  # planeja, não edita (model: opus)
   │  ├─ revisor.md     # revisa qualidade/segurança (read-only)
   │  └─ testador.md    # escreve testes em TDD
   ├─ commands/
   │  ├─ verificar.md   # /verificar — lint + typecheck + testes
   │  └─ commit.md      # /commit — Conventional Commits
   └─ skills/
      ├─ requisitos-spec/SKILL.md  # pedido vago → spec testável
      └─ revisao-codigo/SKILL.md   # roteiro de autorrevisão
```

### ⚠️ Instalação (passo importante)

A pasta de configuração foi entregue como `dot-claude/` porque não foi possível gravar diretamente em `.claude`. Após baixar, renomeie:

```bash
cd template
mv dot-claude .claude        # Windows (cmd): ren dot-claude .claude
```

Feito isso, o Claude Code reconhece agents, skills, commands e settings automaticamente.

### 🔄 Fluxo de trabalho recomendado

1. **Explore** — leia os arquivos relevantes antes de propor mudança.
2. **Plan** — tarefas que toquem 2+ arquivos → plan mode (ou subagent `planejador`).
3. **Spec-first** — features começam por um arquivo em `specs/` (skill `requisitos-spec`).
4. **TDD** — escreva o teste (subagent `testador`), veja falhar, implemente, veja passar.
5. **Code** — incrementos pequenos e verificáveis.
6. **Verificar** — `/verificar` (lint + typecheck + testes).
7. **Revisar** — subagent `revisor` ou skill `revisao-codigo`.
8. **Commit** — `/commit` (Conventional Commits).

### ⚡ Paralelismo

Para tarefas independentes em paralelo, use **git worktrees** (um diretório de trabalho por branch) e delegue partes a subagents diferentes. Detalhes na Fase 5.

---

## ▶️ Como usar este repositório

1. Comece pelo **roadmap** e siga as 6 fases.
2. Na Fase 6, **adote o template** num projeto real.
3. Itere: ajuste `CLAUDE.md`, crie suas próprias skills e agents conforme a necessidade.

---

<div align="center">

_Bons estudos — e bom código._ ⚡

</div>
