---
description: Implementa uma issue do GitHub de ponta a ponta seguindo os padrões do projeto
argument-hint: <número-da-issue>
---

Implemente a issue #$ARGUMENTS deste repositório.

1. Leia a issue com `gh issue view $ARGUMENTS --comments` e a feature/épico pai, se necessário. Identifique os critérios de aceite e os requisitos (RF/RNF) citados; consulte `passagens-docs` quando precisar de detalhe.
2. Confirme que a story está "Ready" (critérios verificáveis, dependências resolvidas). Se não estiver, pare e explique o que falta.
3. Crie a branch `<tipo>/$ARGUMENTS-<slug>` a partir da `main` atualizada.
4. Planeje em poucos passos e implemente seguindo `CLAUDE.md` e `.claude/padroes/`. Para decisões de infraestrutura ou custo, consulte o agente `arquiteto-aws`.
5. Use o agente `qa-testes` para garantir um teste por critério de aceite; rode lint e testes localmente.
6. Rode o agente `revisor-padroes` e corrija os pontos bloqueantes e importantes.
7. Faça commits no padrão Conventional Commits.
8. Mostre o resumo das mudanças e **pergunte antes** de dar push e abrir o PR. Ao abrir, use o template e inclua `Closes #$ARGUMENTS`.
