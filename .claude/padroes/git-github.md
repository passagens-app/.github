# Padrões de Git e GitHub

## Organização do trabalho

- Hierarquia: **Epic → Feature** (no `passagens-docs`) → **Story/Spike/Bug** (no repositório do código), ligados como sub-issues.
- Tudo fica no Project **"Buscador de Passagens"** (`passagens-app`, project #1), com os campos Fase, Prioridade, Tamanho, Iteração e Requisitos.
- Task de outro repositório marcada como `[web]`, `[api]`… vira sub-issue naquele repositório quando for iniciada.
- Ler uma issue: `gh issue view <n> -R passagens-app/<repo>`.

## Branches e commits

- Trunk-based: `main` sempre implantável; branches curtas `<tipo>/<nº-issue>-<slug>` (ex.: `feat/42-coletor-travelpayouts`).
- Tipos: `feat`, `fix`, `infra`, `docs`, `test`, `refactor`, `chore`.
- **Conventional Commits**: `feat(coletor): normaliza ofertas do Travelpayouts`.
- Commits pequenos e coerentes; nunca commitar segredos, `.env`, state do Terraform ou artefatos de build.

## Pull requests

- Um PR por story (ou por task grande); descrição com `Closes passagens-app/<repo>#<n>`.
- Checks obrigatórios verdes (lint, testes, `terraform plan`). Revisar o plan comentado antes do merge.
- Rodar o agente `revisor-padroes` antes de abrir o PR.
- **Squash merge**; a branch é apagada depois do merge.
- O merge na `main` faz o deploy via environment `prod` (com aprovação manual nos repositórios de infra).

## Definition of Ready

Story com critérios de aceite verificáveis, tamanho ≤ L, dependências resolvidas e requisitos referenciados.

## Definition of Done

- Critérios de aceite atendidos e cobertos por testes.
- CI verde; Terraform aplicado em prod pelo pipeline.
- Comentários apenas no início das funções.
- Logs/alarmes cobrindo o componente novo.
- Documentação em `passagens-docs` atualizada se o comportamento mudou.
