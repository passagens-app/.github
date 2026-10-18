# Instruções para agentes de IA

Este repositório faz parte do **Buscador de Passagens** (organização `passagens-app`).

Leia antes de qualquer alteração:

- `CLAUDE.md`: propósito do repositório, estrutura, comandos e regras específicas.
- `.claude/padroes/`: padrões comuns de arquitetura, código, Git/GitHub e da stack.
- `.claude/agents/`: agentes especializados (revisor de padrões, arquiteto AWS, QA e os da stack).

Regras inegociáveis:

1. Comentários **somente no início de funções** (o que faz, parâmetros e retorno).
2. Infraestrutura **somente via Terraform** e deploy **somente via pipeline**. Nunca rode `terraform apply` local depois do bootstrap.
3. Serverless e barato: sem NAT, VPC, RDS, servidores ligados o tempo todo ou Secrets Manager.
4. Acesso a terceiros sempre com semáforo, backoff exponencial, circuit breaker e idempotência; nunca contornar bloqueios.
5. Nenhum segredo no código; nenhum dado pessoal em log.
6. Toda mudança nasce de uma issue e chega à `main` por PR.
