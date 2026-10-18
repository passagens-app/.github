# .github (organização passagens-app)

Arquivos padrão da organização: templates de issue e de PR e workflows reutilizáveis de CI/CD, que valem para todos os repositórios do Buscador de Passagens.

@.claude/padroes/git-github.md
@.claude/padroes/codigo.md

## Conteúdo

- `.github/ISSUE_TEMPLATE/`: formulários Story, Bug, Spike e Fonte quebrada (com `type` e `projects` preenchidos).
- `.github/pull_request_template.md`: template padrão de PR.
- `.github/workflows/`: workflows reutilizáveis (`terraform.yml`, `python.yml`, `container.yml`, `web.yml`, `add-to-project.yml`), chamados pelos repositórios com `uses: passagens-app/.github/.github/workflows/<arquivo>@main`.

## Regras específicas

- Workflows usam OIDC (`permissions: id-token: write`) e nunca access keys.
- Actions de terceiros fixadas por SHA de commit.
- Mudança em workflow reutilizável é breaking change para todos os repositórios: testar num repositório antes do merge.
