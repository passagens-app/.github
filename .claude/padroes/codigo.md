# Padrões de código

## Comentários

- **Somente no início de funções/métodos/componentes**, dizendo o que fazem, os parâmetros de entrada e o retorno (docstring em Python, JSDoc/TSDoc em TypeScript, `description` em variáveis/outputs do Terraform).
- Proibido: comentários no meio do código, código comentado, `TODO` sem número de issue, comentários do tipo "adicionado/alterado em…". Se o código precisa de explicação no meio, extraia uma função com nome claro.

## Idioma

- Identificadores (variáveis, funções, classes, arquivos) em **inglês**.
- Docstrings, mensagens ao usuário, e-mails, documentação e descrições de PR em **português (pt-BR)**.
- Commits: tipo do Conventional Commits em inglês (`feat`, `fix`…), descrição em português.

## Design

- Funções pequenas, com uma responsabilidade e nomes que dizem o que fazem.
- Regras de negócio puras (sem I/O) em `domain/`; integrações (AWS, HTTP, sites) em `adapters/`; handlers finos que só orquestram.
- Dependências injetadas (clientes criados fora da função de negócio) para facilitar testes.
- Sem números mágicos: limites, frequências e percentuais em configuração.
- Erros nunca são engolidos: trate o que é esperado, deixe o resto subir com contexto.
- Toda operação disparada por fila, stream ou agenda deve ser **idempotente**.

## Testes

- Cada critério de aceite da issue vira pelo menos um teste automatizado.
- Testes não acessam rede nem AWS real (usar `moto`, fixtures gravadas e fakes).
- Cobertura mínima de 80% nas regras de negócio (`domain/`).
- Nome do teste descreve o comportamento: `test_alert_is_not_resent_within_cooldown`.

## Segurança

- Validar toda entrada externa (API, fila, página coletada) com schema (Pydantic/Zod).
- Menor privilégio em IAM; nenhuma credencial no repositório; `.env` nunca é commitado.
- Dependências fixadas (lockfile) e atualizadas pelo Dependabot.
