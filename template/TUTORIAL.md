# Tutorial — usando o template passo a passo

Guia rápido para desenvolver uma feature do início ao fim com a squad.
Exemplo usado: uma **API de imobiliária** em Java.

---

## Passo 0 — Preparar o projeto (uma vez)

1. Copie o conteúdo deste template para a raiz do seu projeto (inclui a pasta `.claude/`).
2. Ajuste `CLAUDE.md`: nome do projeto, stack real, comandos.
3. Habilite os MCPs em `.claude/settings.local.json`:
   ```json
   { "enabledMcpjsonServers": ["filesystem", "git"] }
   ```
4. Abra o terminal na raiz e rode `claude`.

---

## Passo 1 — Escrever o negócio

Crie a pasta da feature e descreva o que você quer, em linguagem de negócio:

```
specs/imobiliaria-api/negocio.md
```

Use `specs/_template-negocio.md` como guia. Exemplo do conteúdo:

> - Corretor cadastra imóvel (endereço, preço, fotos).
> - Preço deve ser positivo; CEP válido.
> - Cliente busca imóveis por bairro e faixa de preço.

> 💡 Se seu pedido está vago, em vez de escrever à mão use:
> ```
> /spec quero uma API para corretores cadastrarem imóveis e clientes buscarem
> ```
> O Claude te ajuda a transformar isso em `spec.md`.

---

## Passo 2 — Planejar (refinamento técnico)

```
/planejar imobiliaria-api
```

Isso roda automaticamente, em sequência:

1. **explorador** → mapeia sua codebase (`exploracao.md`).
2. **arquiteto** → cria `plan.md` (arquitetura + tarefas + DAG), congela `contracts/`.
3. **qa** → escreve cenários de teste em Gherkin (`scenarios/*.feature`).

Ao final, o Claude **para** e te mostra o plano.

---

## Passo 3 — 🚦 Revisar e aprovar (gate humano)

Esta etapa é sua. Abra `specs/imobiliaria-api/plan.md` e:

- leia a arquitetura e a quebra de tarefas;
- tire dúvidas com o Claude ("por que separou X de Y?");
- peça ajustes se precisar.

Quando estiver satisfeito, mude no topo do `plan.md`:

```
> Status: Aprovado
```

**A squad não escreve código enquanto o plano não estiver `Aprovado`.**

---

## Passo 4 — Executar (a squad coda)

```
/squad imobiliaria-api
```

O orquestrador:

- despacha **uma instância de agente por tarefa** (`impl-java` / `impl-go` / `impl-python`) + `testador`;
- tarefas independentes rodam **em paralelo** (git worktrees); dependentes esperam;
- TDD: o teste (do Gherkin) falha primeiro, a implementação o faz passar;
- o **integrador** junta tudo e roda `/verificar`.

---

## Passo 5 — Revisar o código

```
/revisar
```

O subagent `revisor` analisa o diff e relata problemas por severidade (🔴 / 🟡 / 🟢).

---

## Passo 6 — Commitar

```
/commit
```

Gera uma mensagem em Conventional Commits a partir das mudanças. (Não faz push.)

---

## Fluxo completo (resumo)

```
negocio.md  →  /planejar  →  🚦 aprovar plan.md  →  /squad  →  /revisar  →  /commit
   (O QUÊ)      (O COMO)        (você decide)        (código)    (qualidade)
```

| Comando      | O que faz                                              |
|--------------|--------------------------------------------------------|
| `/spec`      | (opcional) pedido vago → requisitos testáveis          |
| `/explorar`  | (opcional) só mapear a codebase                        |
| `/planejar`  | explorador + arquiteto + QA → plano, contratos, Gherkin|
| `/squad`     | executa as tarefas (exige `plan.md` Aprovado)          |
| `/verificar` | build + testes + lint (detecta Java/Go)                |
| `/revisar`   | code review independente                               |
| `/commit`    | Conventional Commits                                   |

---

## Dicas

- **Já tem doc de negócio detalhado?** Pule o `/spec` e vá direto pro `/planejar`.
- **Errou o plano?** Ajuste antes de aprovar — é mais barato corrigir no `plan.md` que no código.
- **Detalhes do modelo:** veja `docs/squad-playbook.md`.
- **Regras Java/Spring:** veja `docs/convencoes-java-spring.md`.
