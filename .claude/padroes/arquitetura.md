# Padrões de arquitetura

Valem para todos os repositórios da organização `passagens-app`. A fonte da verdade dos requisitos é o repositório `passagens-docs` (`docs/01-requisitos.md`).

## Plataforma

- **AWS, região `us-east-1`.** Os scrapers podem ir para `sa-east-1` apenas se houver bloqueio geográfico comprovado.
- **Serverless e pagamento por uso.** Lambda (Python 3.13, **arm64**), DynamoDB on-demand, SQS, EventBridge Scheduler, API Gateway **HTTP API**, S3 + CloudFront (OAC), Cognito, SES, SSM Parameter Store.
- **Proibido sem decisão registrada (ADR em `passagens-docs`):** VPC/NAT Gateway, RDS/Aurora, EC2/Fargate ligados o tempo todo, Secrets Manager, WAF, X-Ray ativo por padrão, ElastiCache, OpenSearch.
- **Orçamento:** AWS Budgets de US$ 10/mês. Todo recurso novo deve vir com a estimativa de custo mensal no PR.

## Contratos entre repositórios

- O `passagens-platform` publica nomes e ARNs em `/passagens/platform/*` no **SSM**; os outros repositórios leem via `data "aws_ssm_parameter"`.
- Um repositório **nunca** lê o state do Terraform de outro.
- Segredos ficam em **SSM SecureString** (`/passagens/<serviço>/<nome>`), nunca no código, em variáveis do GitHub ou em logs.

## Dados (DynamoDB single-table `passagens`)

- Chaves no formato `TIPO#valor` (ex.: `WATCH#<id>`, `USER#<sub>`, `SOURCE#<nome>`), conforme a tabela da seção 6.1 dos requisitos. Entidade nova → atualizar a tabela de requisitos no mesmo ciclo.
- Datas gravadas em **UTC ISO-8601**; exibidas em `America/Sao_Paulo`.
- Valores monetários em `Decimal` com a moeda ISO-4217 ao lado; pontos como inteiros.
- Dados brutos com TTL (`expiresAt`); agregados permanentes.
- Dados da carteira são **isolados por usuário** (nem o admin lê a carteira de outro).

## Coleta educada (qualquer acesso a site ou API de terceiros)

- Fila SQS por fonte com `maximum_concurrency`; semáforo com lease e token bucket no DynamoDB.
- Backoff exponencial com jitter total (base 2 s, teto 5 min, 5 tentativas); DLQ após 3 recebimentos.
- Circuit breaker por fonte em 403/429/CAPTCHA. **Nunca** contornar CAPTCHA ou bloqueio, nem usar proxy residencial para fugir de bloqueio.
- Nunca pedir nem guardar login/senha de programas de fidelidade.

## Mensageria e eventos

- Processamento assíncrono via SQS → Lambda com **relatório de falhas parciais** e **idempotência** (AWS Lambda Powertools).
- Reações a mudanças de dados via **DynamoDB Streams** com filtro de evento no event source mapping.

## Observabilidade

- Logs JSON estruturados (Powertools Logger) com `correlation_id`; retenção de 14 dias.
- Métricas de negócio via EMF; alarmes publicam no SNS `ops-alarms`.
- Nada de dados pessoais (e-mail, saldo) em logs.
