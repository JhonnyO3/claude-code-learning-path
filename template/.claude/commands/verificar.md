---
description: Detecta a stack (Java/Go) pelos arquivos-marcadores e roda build, testes e lint, relatando o resultado.
allowed-tools: Bash, Read, Glob
---

Verifique a saúde do projeto antes de concluir uma tarefa.

1. **Detecte as stacks** pelos arquivos-marcadores (procure na raiz e em `services/*`):
   - `pom.xml` → Java/Maven
   - `build.gradle` ou `build.gradle.kts` → Java/Gradle
   - `go.mod` → Go
2. **Rode o que se aplica** (em cada módulo detectado):
   - Maven:  `mvn -q verify`
   - Gradle: `./gradlew check`
   - Go:     `go vet ./... && go test ./...` (e `golangci-lint run` se disponível)
3. **Resuma:** o que passou, o que falhou e os próximos passos para corrigir falhas.
   Não declare "pronto" com build/lint/teste quebrado.

Argumento opcional (caminho/módulo/filtro): $ARGUMENTS
