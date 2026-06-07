---
description: Sincroniza o motor do kit (agents/skills/commands) deste template para o escopo usuário (~/.claude), tornando-o disponível em todos os projetos.
allowed-tools: Read, Bash, Grep, Glob
---

Sincronize o **motor** do kit (subagents, skills e slash commands) da fonte do template para o
escopo usuário `~/.claude/`, para que valha em **todos os projetos** da máquina.

> Por que existe: o `~/.claude/` é uma **cópia**, não um vínculo. Quando você altera um agent/skill/
> command neste template, o escopo usuário continua na versão antiga até rodar este comando.

## Passos

1. **Descubra a fonte do template** (a pasta que contém `.claude/skills/orquestrar-squad/SKILL.md`):
   - Se `$ARGUMENTS` for um caminho, use-o.
   - Senão, se o diretório atual (ou a raiz do repo git atual) for o próprio template, use-o.
   - Senão, PARE e peça o caminho do template (ou rode a partir do repo do template).

2. **Sincronize estes três diretórios** de `<fonte>/.claude/` para `~/.claude/` (sobrescrevendo):
   - `agents/` · `skills/` · `commands/`
   ```bash
   SRC="<fonte>/.claude"; DST="$HOME/.claude"
   mkdir -p "$DST/agents" "$DST/skills" "$DST/commands"
   cp -r "$SRC/agents/." "$DST/agents/"
   cp -r "$SRC/skills/." "$DST/skills/"
   cp -r "$SRC/commands/." "$DST/commands/"
   ```

3. **Remoções (destrutivo — só com confirmação):** se algum agent/skill/command foi **apagado** no
   template, aponte os órfãos em `~/.claude/` e **pergunte** antes de remover. Nunca apague sem confirmar.

4. **settings.local.json (NÃO sobrescreva):** as permissões de build (mvn/gradle/go/...) e o hook gofmt
   devem existir em `~/.claude/settings.local.json`. Se faltarem, **funda** (sem duplicar e sem tocar
   nas permissões pessoais já presentes); nunca substitua o arquivo inteiro.

5. **Relate** o que mudou: liste agents/skills/commands sincronizados e quaisquer órfãos detectados.

## Regras

- Idempotente: rodar de novo não deve quebrar nada nem duplicar permissões.
- Só o **motor** é sincronizado (agents/skills/commands). A "pele" de projeto (CLAUDE.md, docs/, specs/)
  não entra aqui — isso é por projeto.
- Remoções e substituição de `settings.local.json` exigem confirmação explícita.

Caminho da fonte do template (opcional; vazio = usar o repo atual): $ARGUMENTS
